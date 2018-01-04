---
author: mtoepke
title: "DirectX 게임에 리소스 로드"
description: "대부분의 게임은 특정 시점에 로컬 저장소 또는 몇몇 다른 데이터 스트림에서 셰이더, 텍스처, 미리 정의된 메시 또는 기타 그래픽 데이터 등, 리소스와 자산을 로드합니다."
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 게임, directx, 리소스 로드"
ms.openlocfilehash: 032cde6294093a2c0a1c582312b9353a146e94da
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: ko-KR
---
# <a name="load-resources-in-your-directx-game"></a>DirectX 게임에 리소스 로드


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

대부분의 게임은 특정 시점에 로컬 저장소 또는 몇몇 다른 데이터 스트림에서 셰이더, 텍스처, 미리 정의된 메시 또는 기타 그래픽 데이터 등, 리소스와 자산을 로드합니다. 여기서는 UWP(유니버설 Windows 플랫폼) 게임에 사용하기 위해 이러한 파일을 로드할 때 고려해야 할 부분에 대해 심도 있게 다룹니다.

예를 들어, 게임에서 다각형 개체에 대한 메시는 다른 도구를 사용하여 만들고 특정 형식으로 내보냈을 수도 있습니다. 텍스처와 다른 리소스도 마찬가지입니다. 압축되지 않은 일반 비트맵은 대부분의 도구로 작성할 수 있고 대부분의 그래픽 API에서 이해할 수 있지만 게임에 사용하기는 아주 비효율적입니다. 이 항목에서는 Direct3D와 함께 사용하기 위해 세 가지 다른 유형의 그래픽 리소스인 메시(모델), 텍스처(비트맵), 컴파일된 셰이더 개체를 로드하는 기본 단계를 안내합니다.

## <a name="what-you-need-to-know"></a>알아야 할 사항


### <a name="technologies"></a>기술

-   병렬 패턴 라이브러리(ppltasks.h)

### <a name="prerequisites"></a>필수 조건

-   기본 Windows 런타임 이해
-   비동기 작업 이해
-   3D 그래픽 프로그래밍의 기본 개념 이해

이 샘플에는 또한 리소스 로딩 및 관리를 위한 코드 파일 세 개가 포함되어 있습니다. 앞으로 이 항목 전체에서 이러한 파일에 정의된 코드 개체가 나옵니다.

-   BasicLoader.h/.cpp
-   BasicReaderWriter.h/.cpp
-   DDSTextureLoader.h/.cpp

이 샘플의 전체 코드는 다음 링크에 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[BasicLoader의 전체 코드](complete-code-for-basicloader.md)</p></td>
<td align="left"><p>그래픽 메시 개체를 변환하여 메모리에 로드하는 클래스 및 메서드의 전체 코드입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[BasicReaderWriter의 전체 코드](complete-code-for-basicreaderwriter.md)</p></td>
<td align="left"><p>일반적으로 이진 데이터 파일을 읽고 쓰기 위한 클래스 및 메서드의 전체 코드입니다. [BasicLoader](complete-code-for-basicloader.md) 클래스에서 사용됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[DDSTextureLoader의 전체 코드](complete-code-for-ddstextureloader.md)</p></td>
<td align="left"><p>메모리에서 DDS 텍스처를 로드하는 클래스 및 메서드의 전체 코드입니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="instructions"></a>지침

### <a name="asynchronous-loading"></a>비동기 로딩

비동기 로딩은 PPL(병렬 패턴 라이브러리)에서 **task** 템플릿을 사용하여 처리됩니다. **task**에는 비동기 호출 완료 후 그 결과를 처리하는 람다 앞에 오는 메서드 호출이 포함되어 있으며, 대개 다음 형식을 따릅니다.

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`.

한 작업이 완료되면 앞 작업의 결과에 따라 다른 비동기 작업이 실행되도록 **.then()** 구문을 사용하여 작업을 함께 연결할 수 있습니다. 이렇게 하면 플레이어에게 거의 노출하지 않고 복잡한 자산을 개별 스레드에서 로드, 변환 및 관리할 수 있습니다.

자세한 내용은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

이제 비동기 파일 로딩 메서드인 **ReadDataAsync**를 선언하고 만들기 위한 기본 구조를 살펴보겠습니다.

```cpp
#include <ppltasks.h>

// ...
concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename);

// ...

using concurrency;

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

이 코드에서는 위에서 정의한 **ReadDataAsync** 메서드를 호출하면 파일 시스템으로부터 버퍼를 읽기 위해 작업이 만들어집니다. 작업이 완료되면 연결된 작업은 버퍼를 차지하고 정적 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 유형을 사용하여 해당 버퍼에서 배열로 바이트를 스트림합니다.

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

이것이 **ReadDataAsync**에 대한 호출입니다. 호출이 완료되면 제공된 파일에서 읽은 바이트 배열이 반환됩니다. **ReadDataAsync** 자체는 작업으로 정의되므로 바이트 배열이 반환되면 람다를 사용하여 특정 작업(예: 바이트 데이터를 사용할 수 있는 DirectX 함수로 이를 전달)을 수행할 수 있습니다.

아주 간단한 게임인 경우 사용자가 게임을 시작하면 이러한 메서드를 통해 리소스를 로드합니다. 이 작업은 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 구현의 호출 시퀀스에서 특정 지점으로부터 주 게임 루프를 시작하기 전에 할 수 있습니다. 게임을 더 빨리 시작할 수 있고 플레이어가 로딩이 완료될 때까지 기다리지 않아도 되도록 다시 리소스 로딩 메서드를 비동기식으로 호출합니다.

그러나 비동기 로딩을 모두 완료한 후에 게임이 시작되게 하려는 경우에는 로딩이 완료되면 신호로 알려 주는 메서드(예: 특정 필드)를 만들고, 로딩을 마쳤을 때 로딩 메서드에 람다를 사용하여 해당 신호를 설정합니다. 이렇게 로드된 리소스를 사용하는 구성 요소를 시작하기 전에 변수를 확인합니다.

다음은 게임이 시작되면 BasicLoader.cpp에 정의된 비동기 메서드를 사용하여 셰이더, 메시, 텍스처를 로드하는 예제입니다. 이 예제는 모든 로딩 메서드가 완료되면 게임 개체 **m\_loadingComplete**에 특정 필드를 설정합니다.

```cpp
void ResourceLoading::CreateDeviceResources()
{
    // DirectXBase is a common sample class that implements a basic view provider. 
    
    DirectXBase::CreateDeviceResources(); 

    // ...

    // This flag will keep track of whether or not all application
    // resources have been loaded.  Until all resources are loaded,
    // only the sample overlay will be drawn on the screen.
    m_loadingComplete = false;

    // Create a BasicLoader, and use it to asynchronously load all
    // application resources.  When an output value becomes non-null,
    // this indicates that the asynchronous operation has completed.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    auto loadVertexShaderTask = loader->LoadShaderAsync(
        "SimpleVertexShader.cso",
        nullptr,
        0,
        &m_vertexShader,
        &m_inputLayout
        );

    auto loadPixelShaderTask = loader->LoadShaderAsync(
        "SimplePixelShader.cso",
        &m_pixelShader
        );

    auto loadTextureTask = loader->LoadTextureAsync(
        "reftexture.dds",
        nullptr,
        &m_textureSRV
        );

    auto loadMeshTask = loader->LoadMeshAsync(
        "refmesh.vbo",
        &m_vertexBuffer,
        &m_indexBuffer,
        nullptr,
        &m_indexCount
        );

    // The && operator can be used to create a single task that represents
    // a group of multiple tasks. The new task's completed handler will only
    // be called once all associated tasks have completed. In this case, the
    // new task represents a task to load various assets from the package.
    (loadVertexShaderTask && loadPixelShaderTask && loadTextureTask && loadMeshTask).then([=]()
    {
        m_loadingComplete = true;
    });

    // Create constant buffers and other graphics device-specific resources here.
}
```

이 작업은 모든 작업이 완료되었을 때만 로딩 완료 플래그를 설정하는 람다가 트리거되도록 &amp;&amp; 연산자를 사용하여 집계되었습니다. 플래그가 여러 개인 경우 경합 상태의 가능성이 있다는 점에 유의하세요. 예를 들어, 람다가 동일한 값에 두 개의 플래그를 순차적으로 설정할 경우 두 번째 플래그가 설정되기 전에 플래그를 검사하는 다른 스레드는 첫 번째 플래그만 보게 될 수 있습니다.

지금까지 리소스 파일을 비동기식으로 로드하는 방법에 대해 알아보았습니다. 동기 파일 로드는 훨씬 더 간단하며, [BasicReaderWriter의 전체 코드](complete-code-for-basicreaderwriter.md) 및 [BasicLoader의 전체 코드](complete-code-for-basicloader.md)에 해당 예제가 나와 있습니다.

리소스 및 자산 유형이 다르면 그래픽 파이프라인에서 사용하기 위해 준비하기 전에 추가 처리 또는 변환이 필요할 수 있습니다. 이제 세 가지 리소스 유형(메시, 텍스처, 셰이더)을 살펴보겠습니다.

### <a name="loading-meshes"></a>로딩 메시

메시는 게임 내의 코드 프로시저로 생성되거나, 3DStudio MAX, Alias WaveFront 등, 다른 앱 또는 도구에서 파일로 내보낸 꼭짓점 데이터입니다. 이러한 메시는 게임에서 큐브 및 스피어 같은 단순한 primitive부터 집이나 캐릭터에 이르기까지 다양한 모델을 나타내며, 형식에 따라 색과 애니메이션 데이터를 포함하기도 합니다. 여기서는 꼭짓점 데이터만 포함하는 메시를 중점적으로 알아보겠습니다.

메시를 올바르게 로드하려면 메시에 대한 파일에서 데이터의 형식을 알아야 합니다. 위에 나온 단순한 **BasicReaderWriter** 형식은 데이터를 바이트 스트림으로 읽습니다. 이 형식은 바이트 데이터가 메시를 나타낸다는 것을 알지 못하며, 다른 응용 프로그램에서 내보낸 특정 메시 형식은 더욱 알 수 없습니다. 따라서 메시 데이터를 메모리로 가져올 때와 같은 변환을 수행해야 합니다.

가능하면 항상 내부 표현에 최대한 가까운 형식으로 자산 데이터를 패키지합니다. 그러면 리소스 사용이 줄어들고 시간이 절약됩니다.

이제 메시의 파일에서 바이트 데이터를 가져오겠습니다. 예제에 사용된 형식은 파일이 .vbo 접미사가 붙은 샘플별 형식이라고 가정합니다. 다시 말하지만, 이 형식은 OpenGL의 VBO 형식과는 다릅니다. 각 꼭짓점은 자체적으로 **BasicVertex** 형식에 매핑합니다. 이 형식은 obj2vbo 변환기 도구의 코드에 정의된 구조체입니다. .vbo 파일에서 꼭짓점 데이터의 레이아웃은 다음과 같습니다.

-   데이터 스트림의 첫 32비트(4바이트)에는 unit32 값으로 나타낸 메시의 꼭짓점 수(numVertices)가 들어 있습니다.
-   데이터 스트림의 다음 32비트(4바이트)에는 unit32 값으로 나타낸 메시의 인덱스 수(numIndices)가 들어 있습니다.
-   그 뒤 후속 (numVertices \* sizeof(**BasicVertex**)) 비트에는 꼭짓점 데이터가 들어 있습니다.
-   데이터의 마지막 (numIndices \* 16) 비트에는 unit16 값의 시퀀스로 나타낸 인덱스 데이터가 들어 있습니다.

핵심은 로드한 메시 데이터의 비트 수준 레이아웃을 알아야 하며, endian과 일관성을 유지해야 한다는 것입니다. 모든 Windows 8 플랫폼은 little-endian입니다.

예제에서는 **LoadMeshAsync** 메서드에서 CreateMesh 메서드를 호출하여 이 비트 수준 해석을 수행합니다.

```cpp
task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

**CreateMesh**는 파일에서 로드한 바이트 데이터를 해석한 다음 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)에 꼭짓점 및 인덱스 목록을 각각 전달하고 D3D11\_BIND\_VERTEX\_BUFFER 또는 D3D11\_BIND\_INDEX\_BUFFER를 지정하여 메시에 대한 꼭짓점 버퍼와 인덱스 버퍼를 만듭니다. 다음은 **BasicLoader**에 사용된 코드입니다.

```cpp
void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);

    m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            );
    
    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    
    m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            );
  
    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}
```

일반적으로 게임에 사용하는 모든 메시에 대해 꼭짓점/인덱스 버퍼를 만듭니다. 메시를 로드할 위치나 시점은 개발자가 결정합니다. 메시 수가 많을 때는 특정 로딩 상태, 미리 정의된 로드 상태 등, 게임의 특정 시점에 디스크에서 일부를 로드해야 할 수 있습니다. 지형 데이터 같은 대규모 메시의 경우 캐시에서 꼭짓점을 스트리밍 할 수 있지만 이는 더 복잡한 프로시저이며 이 항목의 범위를 벗어납니다.

다시 한 번 강조하지만 꼭짓점 데이터 형식을 알아야 합니다. 모델을 만드는 데 사용되는 도구에서 꼭짓점 데이터를 나타내는 방식은 아주 많습니다. 또한 꼭짓점 데이터의 입력 레이아웃을 삼각형 목록 및 스트립 등, Direct3D로 나타내는 방식도 여러 가지입니다. 꼭짓점 데이터에 대한 자세한 내용은 [Direct3D 11의 버퍼 소개](https://msdn.microsoft.com/library/windows/desktop/ff476898) 및 [Primitive](https://msdn.microsoft.com/library/windows/desktop/bb147291)를 참조하세요.

다음으로 로딩 텍스처를 살펴보겠습니다.

### <a name="loading-textures"></a>로딩 텍스처

게임에서 가장 많이 사용되는 자산이자 디스크 및 메모리에 있는 대부분의 파일로 구성되는 자산은 텍스처입니다. 메시와 유사하게, 텍스처는 다양한 형식으로 제공될 수 있습니다. 개발자는 텍스처를 로드할 때 Direct3D가 사용할 수 있는 형식으로 텍스처를 변환합니다. 또한 텍스처는 광범위한 형식으로 제공되며 서로 다른 효과를 만드는 데 사용됩니다. 텍스처의 MIP 수준을 사용하여 거리 개체의 모양과 성능을 향상할 수 있습니다. 더트(dirt) 맵 및 그림자(light) 맵은 및 기본 텍스처 위에 효과 및 세부 항목을 겹쳐서 표현하는 데 사용되고 보통의 맵은 픽셀당 조명 효과 계산에 사용됩니다. 첨단 게임에서 보통의 장면에는 잠재적으로 수천 개의 개별 텍스처가 사용될 수 있으며, 코드는 이 모두를 효율적으로 관리해야 합니다.

또한 메시와 마찬가지로 메모리 사용을 효율적으로 만드는 데 사용되는 몇 가지 특정 형식이 있습니다. 텍스처는 GPU 및 시스템 메모리의 상당 부분을 쉽게 소비할 수 있기 때문에 특정 방식으로 압축되기도 합니다. 게임의 텍스처에 반드시 압축을 사용해야 하는 것은 아니며 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 비트맵같이 Direct3D 셰이더가 이해할 수 있는 형식으로 데이터를 제공하기만 하면 원하는 압축/압축 해제 알고리즘을 사용할 수 있습니다.

모든 DXT 형식이 플레이어의 그래픽 하드웨어에서 지원되지는 않을 수도 있지만 Direct3D는 DXT 텍스처 압축 알고리즘을 지원합니다. DDS 파일은 DXT 텍스트와 다른 텍스처 압축 형식을 포함하며 .dds라는 접미사가 붙습니다.

DDS 파일은 다음 정보를 포함하는 이진 파일입니다.

-   4문자 코드 값 'DDS ' (0x20534444)가 들어 있는 DWORD(매직 넘버).

-   파일의 데이터에 대한 설명.

    데이터는 [**DDS\_HEADER**](https://msdn.microsoft.com/library/windows/desktop/bb943982)를 사용하여 헤더 설명으로 설명됩니다. 픽셀 형식은 [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984)을 사용하여 정의됩니다. **DDS\_HEADER** 및 **DDS\_PIXELFORMAT** 구조는 사용되지 않는 DDSURFACEDESC2, DDSCAPS2 및 DDPIXELFORMAT DirectDraw 7 구조를 대체합니다. **DDS\_HEADER**는 DDSURFACEDESC2 및 DDSCAPS2의 이진 값입니다. **DDS\_PIXELFORMAT**은 DDPIXELFORMAT의 이진 값입니다.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984)에서 **dwFlags**의 값은 DDPF\_FOURCC로 설정되고 **dwFourCC**는 "DX10"으로 설정됩니다. 부동 소수점 형식, sRGB 형식 등, RGB 픽셀 형식으로 표현할 수 없는 DXGI 형식 또는 텍스처 배열을 수용하기 위해 추가 [**DDS\_HEADER\_DXT10**](https://msdn.microsoft.com/library/windows/desktop/bb943983) 구조가 표시됩니다. **DDS\_HEADER\_DXT10** 구조가 표시될 때 전체 데이터 설명은 다음과 같이 나타납니다.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   주 표면 데이터를 포함하는 바이트 배열에 대한 포인터.
    ```cpp
    BYTE bdata[]
    ```

-   mipmap 수준, 큐브 맵의 표면, 볼륨 텍스처의 깊이 등, 나머지 표면을 포함하는 바이트 배열에 대한 포인터. [텍스처](https://msdn.microsoft.com/library/windows/desktop/bb205578), [큐브 맵](https://msdn.microsoft.com/library/windows/desktop/bb205577) 또는 [볼륨 텍스처](https://msdn.microsoft.com/library/windows/desktop/bb205579)의 DDS 파일 레이아웃에 대한 추가 정보는 각 링크를 참조하세요.

    ```cpp
    BYTE bdata2[]
    ```

대부분의 도구는 DDS 형식으로 내보냅니다. 텍스처를 이 형식으로 내보낼 도구가 없으면 새로 만들어 보세요. DDS 형식에 대한 자세한 내용 및 코드로 작업하는 방법은 [DDS의 프로그래밍 가이드](https://msdn.microsoft.com/library/windows/desktop/bb943991)를 참조하세요. 이 예제에서는 DDS를 사용하겠습니다.

다른 리소스 종류와 마찬가지로, 파일에서 바이트 스트림으로 데이터를 읽습니다. 로딩 작업이 완료되면 람다 호출은 코드(**CreateTexture** 메서드)를 실행하여 Direct3D가 사용할 수 있는 형식으로 바이트 스트림을 처리합니다.

```cpp
task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}
```

이전 코드 조각에서 람다는 파일 이름에 "dds"라는 확장명이 있는지 확인합니다. 이 확장명이 있으면 DDS 텍스처로 간주합니다. 이 확장명이 없으면 WIC(Windows Imaging Component) API를 사용하여 형식을 검색하고 데이터를 비트맵으로 디코딩합니다. 둘 중 어느 쪽이든 결과는 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 비트맵입니다. (또는 오류가 발생합니다.)

```cpp
void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        resource.As(&texture2D);
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory));
        }

        ComPtr<IWICStream> stream;
        m_wicFactory->CreateStream(&stream);

        stream->InitializeFromMemory(
                data,
                dataSize);

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder);

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        bitmapDecoder->GetFrame(0, &bitmapFrame);

        ComPtr<IWICFormatConverter> formatConverter;
        m_wicFactory->CreateFormatConverter(&formatConverter);

        formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom);

        uint32 width;
        uint32 height;
        bitmapFrame->GetSize(&width, &height);

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get());

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D);

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView);
        }
    }


    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}
```

이 코드가 완료되면 이미지 파일에서 로드된 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)가 메모리에 들어 있게 됩니다. 메시와 마찬가지로, Texture2D는 게임 및 여러 장면에 많이 사용됩니다. 게임 또는 레벨이 시작될 때 모두 로드하는 대신 규칙적으로 액세스되는 장면별 또는 레벨별 텍스처에 대해 캐시를 만들어 보세요.

위 샘플에서 호출된 **CreateDDSTextureFromMemory** 메서드는 [DDSTextureLoader의 전체 코드](complete-code-for-ddstextureloader.md)에서 전부 찾아볼 수 있습니다.

또한 개별 텍스처 또는 텍스처 "스킨"은 특정 메시 다각형이나 표면에 매핑할 수 있습니다. 이 매핑 데이터는 주로 아티스트나 디자이너가 모델과 텍스처를 만드는 데 사용한 도구에서 내보냅니다. 내보낸 데이터를 로드할 때는 이 정보도 캡처해야 합니다. 조각 음영을 수행할 경우 올바른 텍스처를 해당 표면에 매핑하는 데 이 정보를 사용할 것이기 때문입니다.

### <a name="loading-shaders"></a>로딩 셰이더

셰이더는 메모리로 로드되어 그래픽 파이프라인의 특정 단계에 호출되는 컴파일된 HLSL(High Level Shader Language) 파일입니다. 가장 일반적이고 필수적인 셰이더는 꼭짓점과 픽셀 셰이더로, 장면의 뷰포트에서 메시의 개별 꼭짓점과 픽셀을 각각 처리합니다. HLSL 코드는 기하 도형을 변환하기 위해 실행되며, 조명 효과와 텍스처를 적용하고 렌더링된 장면에 대해 사후 처리를 수행합니다.

Direct3D 게임에는 각각 별도의 CSO(컴파일된 셰이더 개체, .cso) 파일로 컴파일되는 서로 다른 여러 셰이더를 사용할 수 있습니다. 일반적으로는 동적으로 로드해야 할 대부분의 경우 게임이 시작될 때 또는 레벨별 기반(예: 비 효과용 셰이더)으로 간단히 로드할 수 있습니다.

**BasicLoader** 클래스의 코드는 꼭짓점, 기하 도형, 픽셀 및 헐(hull) 셰이더를 비롯한 여러 셰이더에 대해 오버로드를 제공합니다. 아래 코드는 하나의 예제로서 픽셀 셰이더를 다룹니다. 전체 코드는 [BasicLoader의 전체 코드](complete-code-for-basicloader.md)에서 참조할 수 있습니다.

```cpp
concurrency::task<void> LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        
       m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);
    });
}

```

이 예제에서는 **BasicReaderWriter** 인스턴스(**m\_basicReaderWriter**)를 사용하여 제공된 .cso(컴파일된 셰이더 개체) 파일에서 바이트 스트림으로 읽습니다. 해당 작업이 완료되면 람다는 파일에서 로드된 바이트 데이터를 사용하여 [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)를 호출합니다. 콜백은 로드가 성공적이었음을 나타내는 일부 플래그를 설정해야 하며, 코드는 셰이더를 실행하기 전에 이 플래그를 확인해야 합니다.

꼭짓점 셰이더는 조금 더 복잡합니다. 꼭짓점 셰이더의 경우에도 꼭짓점 데이터를 정의하는 개별 입력 레이아웃을 로드합니다. 다음 코드는 사용자 지정 꼭짓점 입력 레이아웃과 함께 꼭짓점 셰이더를 비동기식으로 로드하는 데 사용할 수 있습니다. 메시에서 로드하는 꼭짓점 정보는 이 입력 레이아웃에 의해 올바르게 나타낼 수 있습니다.

꼭짓점 셰이더를 로드하기 전에 입력 레이아웃을 만들어 보겠습니다.

```cpp
void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout);
    }
    else
    {
        m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout);
    }
}
```

이 특별한 레이아웃에서 각 꼭짓점은 꼭짓점 셰이더에서 처리되는 다음 데이터를 갖습니다.

-   모델의 좌표 공간에서의 3D 좌표 위치(x, y, z). 32비트 부동 소수점 값 3개를 한 묶음으로 나타냅니다.
-   꼭짓점에 대한 일반 벡터. 역시 32비트 부동 소수점 값 3개로 나타냅니다.
-   변환된 2D 텍스처 좌표 값(u, v). 32비트 부동 값 한 쌍으로 나타냅니다.

이러한 꼭짓점별 입력 요소는 [HLSL 시맨틱](https://msdn.microsoft.com/library/windows/desktop/bb509647)이라고 하며, 컴파일된 셰이더 개체와 데이터를 주고받는 데 사용되는 정의된 레지스터의 집합입니다. 파이프라인은 로드한 메시의 모든 꼭짓점에 대해 꼭짓점 셰이더를 한 번 실행합니다. 시맨틱은 실행 시 꼭짓점 셰이더로의 입력과 꼭짓점 셰이더에서의 출력을 정의하고, 셰이더의 HLSL 코드에서 꼭짓점별 계산에 대해 이 데이터를 제공합니다.

이제 꼭짓점 셰이더 개체를 로드합니다.

```cpp
concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
       m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout);   
        }
    });
}

```

이 코드에서 지점 셰이더의 CSO 파일에 대한 바이트 데이터를 읽고 나면 [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524)를 호출하여 꼭짓점 셰이더를 만듭니다. 그런 다음 동일한 람다로 셰이더의 입력 레이아웃을 만듭니다.

헐(hull) 및 기하 도형 셰이더 등, 다른 셰이더 형식에도 특정 구성이 필요할 수 있습니다. 다양한 셰이더 로딩 메서드에 대한 전체 코드는 [BasicLoader의 전체 코드](complete-code-for-basicloader.md) 및 [Direct3D 리소스 로딩 샘플]( http://go.microsoft.com/fwlink/p/?LinkID=265132)에 제공됩니다.

## <a name="remarks"></a>설명

이제 메시, 텍스처 및 컴파일된 셰이더 등, 많이 사용되는 게임 리소스와 자산을 비동기식으로 로드하는 데 필요한 메서드를 이해하고, 만들거나 수정할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [Direct3D 리소스 로드 샘플]( http://go.microsoft.com/fwlink/p/?LinkID=265132)
* [BasicLoader의 전체 코드](complete-code-for-basicloader.md)
* [BasicReaderWriter의 전체 코드](complete-code-for-basicreaderwriter.md)
* [DDSTextureLoader의 전체 코드](complete-code-for-ddstextureloader.md)

 

 




