---
title: DirectX 게임에 리소스 로드
description: '대부분의 게임에서는 로컬 저장소 또는 다른 데이터 스트림에서 리소스와 자산 (예: 셰이더, 질감, 미리 정의 된 메시 또는 기타 그래픽 데이터)을 로드 합니다.'
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 리소스 로드
ms.localizationpriority: medium
ms.openlocfilehash: 56eaebfeb6d644c4c15f14f0613b1e3b1781f637
ms.sourcegitcommit: 22ed0d4edad5e6bab352e641cf86cf455cf83825
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133996"
---
# <a name="load-resources-in-your-directx-game"></a>DirectX 게임에 리소스 로드



대부분의 게임에서는 로컬 저장소 또는 다른 데이터 스트림에서 리소스와 자산 (예: 셰이더, 질감, 미리 정의 된 메시 또는 기타 그래픽 데이터)을 로드 합니다. 여기서는 DirectX C/c + + 유니버설 Windows 플랫폼 (UWP) 게임에서 사용할 파일을 로드할 때 고려해 야 할 사항에 대 한 개략적인 보기를 안내 합니다.

예를 들어 게임의 다각형 개체에 대 한 메시는 다른 도구로 생성 되어 특정 형식으로 내보낼 수 있습니다. 질감 등에도 마찬가지입니다. 일반적으로 압축 되지 않은 플랫 비트맵은 대부분의 도구에서 일반적으로 작성 되 고 대부분의 그래픽 Api에서 이해 될 수 있지만 게임에서 사용 하는 것은 매우 비효율적입니다. 여기서는 Direct3D (모델), 질감 (비트맵) 및 컴파일된 셰이더 개체와 함께 사용할 수 있는 세 가지 유형의 그래픽 리소스를 로드 하는 기본 단계를 안내 합니다.

## <a name="what-you-need-to-know"></a>알아야 하는 작업


### <a name="technologies"></a>기술

-   병렬 패턴 라이브러리 (ppltasks.h)

### <a name="prerequisites"></a>필수 구성 요소

-   기본 Windows 런타임 이해
-   비동기 작업 이해
-   3 차원 그래픽 프로그래밍의 기본 개념을 이해 합니다.

또한이 샘플에는 리소스 로드 및 관리를 위한 세 가지 코드 파일이 포함 되어 있습니다. 이 항목 전체에서 이러한 파일에 정의 된 코드 개체가 발견 됩니다.

-   BasicLoader. h/.cpp
-   BasicReaderWriter .h/.cpp
-   DDSTextureLoader/.cpp

이러한 샘플에 대 한 전체 코드는 다음 링크에서 찾을 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-basicloader.md">BasicLoader에 대 한 전체 코드</a></p></td>
<td align="left"><p>그래픽 메시 개체를 메모리로 변환 하 고 로드 하는 클래스 및 메서드에 대 한 코드를 작성 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="complete-code-for-basicreaderwriter.md">BasicReaderWriter에 대 한 전체 코드</a></p></td>
<td align="left"><p>일반적으로 이진 데이터 파일을 읽고 쓰는 클래스 및 메서드에 대 한 코드를 작성 합니다. <a href="complete-code-for-basicloader.md">Basicloader</a> 클래스에서 사용 됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-ddstextureloader.md">DDSTextureLoader에 대 한 전체 코드</a></p></td>
<td align="left"><p>메모리에서 DDS 텍스처를 로드 하는 클래스 및 메서드에 대 한 코드를 작성 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="instructions"></a>Instructions

### <a name="asynchronous-loading"></a>비동기 로드

비동기 로드는 PPL (병렬 패턴 라이브러리)의 **작업** 템플릿을 사용 하 여 처리 됩니다. **작업** 에는 비동기 호출의 결과를 처리 하는 람다가 뒤에 오는 메서드 호출이 포함 되며, 일반적으로 다음 형식을 따릅니다.

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`.

**. Then ()** 구문을 사용 하 여 작업을 함께 연결 하 여 한 작업이 완료 되 면 이전 작업의 결과에 의존 하는 다른 비동기 작업을 실행할 수 있습니다. 이러한 방식으로 플레이어에 게 거의 표시 되지 않는 방식으로 별도의 스레드에서 복잡 한 자산을 로드, 변환 및 관리할 수 있습니다.

자세한 내용은 [c + +의 비동기 프로그래밍](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)을 참조 하세요.

이제 **ReadDataAsync**비동기 파일 로딩 메서드를 선언 하 고 만드는 기본 구조를 살펴보겠습니다.

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

이 코드에서는 코드에서 위에 정의 된 **ReadDataAsync** 메서드를 호출할 때 파일 시스템에서 버퍼를 읽도록 작업을 만듭니다. 완료 되 면 연결 된 작업에서 버퍼를 가져와서 정적 [**DataReader**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) 형식을 사용 하 여 해당 버퍼의 바이트를 배열로 스트리밍합니다.

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

**ReadDataAsync**에 대해 수행 하는 호출은 다음과 같습니다. 완료 되 면 코드는 제공 된 파일에서 읽은 바이트 배열을 받습니다. **ReadDataAsync** 자체가 작업으로 정의 되므로 람다를 사용 하 여 바이트 배열이 반환 될 때 해당 바이트 데이터를 사용할 수 있는 DirectX 함수에 해당 바이트 데이터를 전달 하는 것과 같은 특정 작업을 수행할 수 있습니다.

게임이 충분히 간단 하면 사용자가 게임을 시작할 때 다음과 같은 방법으로 리소스를 로드 합니다. [**IFrameworkView:: Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) 구현의 호출 시퀀스의 특정 지점에서 주 게임 루프를 시작 하기 전에이 작업을 수행할 수 있습니다. 다시, 게임을 신속 하 게 시작할 수 있도록 리소스 로딩 메서드를 비동기적으로 호출 하므로, 로드가 완료 될 때까지 플레이어가 초기 상호 작용에 참여 하기 전에 기다릴 필요가 없습니다.

그러나 모든 비동기 로드가 완료 될 때까지 게임을 적절 하 게 시작 하지 않는 것이 좋습니다. 특정 필드와 같은 로드가 완료 될 때 신호를 보내기 위한 몇 가지 메서드를 만들고, 로드 메서드에 람다 식을 사용 하 여 완료 될 때 해당 신호를 설정 합니다. 로드 된 리소스를 사용 하는 구성 요소를 시작 하기 전에 변수를 확인 합니다.

다음은 BasicLoader .cpp에 정의 된 비동기 메서드를 사용 하 여 셰이더, 메시 및 게임 시작 시 질감을 로드 하는 예제입니다. 모든 로드 메서드가 완료 되 면 게임 개체 **m \_ loadingComplete**에서 특정 필드를 설정 합니다.

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

모든 작업이 완료 된 경우에만 로드 완료 플래그를 설정 하는 람다가 트리거되는 && 연산자를 사용 하 여 작업을 집계 합니다. 여러 플래그가 있는 경우 경합 상태가 발생할 수 있습니다. 예를 들어 람다가 두 개의 플래그를 동일한 값으로 설정 하는 경우 두 번째 플래그를 설정 하기 전에 다른 스레드에서 첫 번째 플래그를 검사 하는 경우에만 첫 번째 플래그를 볼 수 있습니다.

리소스 파일을 비동기적으로 로드 하는 방법을 살펴보았습니다. 동기 파일 로드는 훨씬 간단 하며, [BasicReaderWriter의 전체 코드](complete-code-for-basicreaderwriter.md) 와 [Basicloader의 전체 코드](complete-code-for-basicloader.md)에서 예제를 찾을 수 있습니다.

물론 다른 리소스 및 자산 유형에는 그래픽 파이프라인에서 사용할 준비가 되기 전에 추가 처리 또는 변환이 필요한 경우가 많습니다. 메시, 질감 및 셰이더의 세 가지 특정 리소스 형식을 살펴보겠습니다.

### <a name="loading-meshes"></a>메시 로드

메시는 게임 내에서 코드에 의해 procedurally 생성 되거나 다른 앱 (예: WaveFront Studio MAX 또는 Alias) 또는 도구에서 파일로 내보낸 꼭 짓 점 데이터입니다. 이러한 메시는 큐브, 구 등의 간단한 기본 형식에서 자동차 및 집 및 문자에 이르기까지 게임의 모델을 나타냅니다. 형식에 따라 색 및 애니메이션 데이터도 포함 됩니다. 꼭 짓 점 데이터만 포함 하는 메시를 집중적으로 살펴보겠습니다.

메시를 올바르게 로드 하려면 메시의 파일에 있는 데이터의 형식을 알아야 합니다. 위의 간단한 **Basicreaderwriter** 형식은 단순히 바이트 스트림으로 데이터를 읽습니다. 다른 응용 프로그램에서 내보낸 것과는 다른 응용 프로그램에서 내보낸 것과 비슷한 특정 메시 형식으로 바이트 데이터를 표시 한다는 것을 알 수 없습니다. 메시 데이터를 메모리로 가져올 때 변환을 수행 해야 합니다.

(가능한 한 내부 표현과 가까운 형식으로 자산 데이터를 항상 패키지 해야 합니다. 이렇게 하면 리소스 사용률을 줄이고 시간이 절약 됩니다.

메시 파일에서 바이트 데이터를 가져옵니다. 이 예제의 형식은 파일이 샘플 별 형식 (. vbo) 인 것으로 가정 합니다. 이 형식은 OpenGL의 VBO 형식과 동일 하지 않습니다. 각 꼭 짓 점 자체는 obj2vbo 변환기 도구의 코드에 정의 된 struct 인 **Basicvertex** 형식에 매핑됩니다. . Vbo 파일에 있는 꼭 짓 점 데이터의 레이아웃은 다음과 같습니다.

-   데이터 스트림의 첫 번째 32 비트 (4 바이트)에는 메시에 서 uint32 값으로 표시 되는 꼭 짓 점 (numVertices 짓 점)의 수가 포함 됩니다.
-   데이터 스트림의 다음 32 비트 (4 바이트)에는 uint32 값으로 표시 되는 메시의 인덱스 수 (numIndices)가 포함 됩니다.
-   그런 다음 후속 ( \* **numvertices Sizeof (basicvertex**)) 비트는 꼭 짓 점 데이터를 포함 합니다.
-   데이터의 마지막 (numIndices \* 16) 비트에는 uint16 값의 시퀀스로 표시 되는 인덱스 데이터가 포함 되어 있습니다.

지점은 로드 한 메시 데이터의 비트 수준 레이아웃을 파악 하는 것입니다. 또한 endian와 일치 해야 합니다. 모든 Windows 8 플랫폼은 작은 endian입니다.

이 예제에서는 **LoadMeshAsync** 메서드에서 CreateMesh 메서드를 호출 하 여이 비트 수준 해석을 수행 합니다.

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

**CreateMesh** 는 파일에서 로드 된 바이트 데이터를 해석 하 고 꼭 짓 점 및 인덱스 목록을 각각 [**ID3D11Device:: createbuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 에 전달 하 고 D3D11 \_ bind \_ vertex \_ buffer 또는 D3D11 \_ bind \_ 인덱스 \_ 버퍼를 지정 하 여 메시에 대 한 꼭 짓 점 버퍼 및 인덱스 버퍼를 만듭니다. **Basicloader**에서 사용 되는 코드는 다음과 같습니다.

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

일반적으로 게임에서 사용 하는 모든 메시에 대 한 꼭 짓 점/인덱스 버퍼 쌍을 만듭니다. 메시를 로드 하는 위치와 위치는 사용자에 게 있습니다. 메시가 많은 경우에는 미리 정의 된 특정 로드 상태와 같이 게임의 특정 지점에 있는 디스크에서 일부를 로드 해야 할 수 있습니다. 지형 데이터와 같은 커다란 메시의 경우 캐시에서 꼭 짓 점을 스트리밍할 수 있지만이는이 항목의 범위에서 다루지 않습니다.

다시, 꼭 짓 점 데이터 형식을 알고 있습니다. 모델을 만드는 데 사용 되는 도구에서 꼭 짓 점 데이터를 표현 하는 여러 가지 방법이 있습니다. 또한 삼각형 목록 및 스트립 같이 Direct3D에 대 한 꼭 짓 점 데이터의 입력 레이아웃을 나타내는 다양 한 방법이 있습니다. 꼭 짓 점 데이터에 대 한 자세한 내용은 [Direct3D 11의 버퍼 소개](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro) 및 [기본 형식](https://docs.microsoft.com/windows/desktop/direct3d9/primitives)을 참조 하세요.

다음으로 질감을 로드 하는 방법을 살펴보겠습니다.

### <a name="loading-textures"></a>텍스처 로드

게임에서 가장 일반적으로 사용할 수 있는 자산과 디스크 및 메모리의 파일을 대부분 구성 하는 것은 질감입니다. 메시와 마찬가지로 질감은 다양 한 형식으로 제공 될 수 있으며,이를 로드할 때 Direct3D에서 사용할 수 있는 형식으로 변환 합니다. 또한 질감은 다양 한 형식으로 제공 되며 다른 효과를 만드는 데 사용 됩니다. 질감에 대 한 밉 레벨을 사용 하 여 거리 개체의 모양과 성능을 향상 시킬 수 있습니다. 먼지 및 광원 맵은 기본 질감의 효과 및 세부 정보를 계층화 하는 데 사용 됩니다. 그리고 법선 지도는 픽셀 별 조명 계산에 사용 됩니다. 오늘날의 게임에서는 일반적인 장면에 수천 개의 개별 질감이 있을 수 있으며, 코드에서이를 모두 효과적으로 관리 해야 합니다.

메시와 마찬가지로 메모리 사용을 효율적으로 만드는 데 사용 되는 다양 한 특정 형식이 있습니다. 질감이 GPU (및 시스템) 메모리의 많은 부분을 쉽게 사용할 수 있기 때문에 종종 특정 방식으로 압축 됩니다. 게임의 질감에서 압축을 사용할 필요가 없으며, Direct3D 셰이더를 이해할 수 있는 형식으로 제공 하는 경우 (예: [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) 비트맵) 원하는 압축/압축 풀기 알고리즘을 사용할 수 있습니다.

Direct3D는 모든 txt 형식이 플레이어의 그래픽 하드웨어에서 지원 되지 않을 수 있지만, TXT 질감 압축 알고리즘에 대 한 지원을 제공 합니다. DDS 파일에는 TXT 질감 (및 기타 질감 압축 형식도 포함)이 포함 되며.

DDS 파일은 다음 정보를 포함 하는 이진 파일입니다.

-   문자 코드 값 ' DDS ' (0x20534444) 4 개를 포함 하는 DWORD (매직 number)입니다.

-   파일의 데이터에 대 한 설명입니다.

    데이터는 [**dds \_ 헤더**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-header)를 사용 하 여 헤더 설명과 함께 설명 됩니다. 픽셀 형식은 [**dds \_ PIXELFORMAT**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-pixelformat)를 사용 하 여 정의 됩니다. **Dds \_ 헤더** 및 **dds \_ PIXELFORMAT** 구조는 사용 되지 않는 DDSURFACEDESC2, DDSCAPS2 및 DDPIXELFORMAT DirectDraw 7 구조를 대체 합니다. **DDS \_ 헤더** 는 DDSURFACEDESC2 및 DDSCAPS2에 해당 하는 이진 값입니다. **DDS \_ PIXELFORMAT** 는 DDPIXELFORMAT에 해당 하는 이진 값입니다.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    [**Dds \_ PIXELFORMAT**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-pixelformat) 의 **dwFlags** 값을 ddpf FOURCC로 설정 하 \_ 고 **dwFourCC** 를 "DX10"로 설정 하면 추가 [**DDS \_ 헤더 \_ DXT10**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-header-dxt10) 구조가 부동 소수점 형식, sRGB 형식 등의 RGB 픽셀 형식으로 표현할 수 없는 질감 배열 또는 DXGI 형식을 수용 하기 위해 제공 됩니다. **DDS \_ 헤더 \_ DXT10** 구조가 있으면 전체 데이터 설명이 다음과 같이 표시 됩니다.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   주 표면 데이터를 포함 하는 바이트 배열에 대 한 포인터입니다.
    ```cpp
    BYTE bdata[]
    ```

-   와 같은 나머지 표면을 포함 하는 바이트 배열에 대 한 포인터입니다. 밉 맵 수준, 큐브 맵의 면, 볼륨 질감의 깊이 의 DDS 파일 레이아웃에 대 한 자세한 내용은 다음 링크를 참조 하십시오. [질감](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-textures), [큐브 맵](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-cubic-environment-maps)또는 [볼륨 질감](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-volume-textures).

    ```cpp
    BYTE bdata2[]
    ```

여러 도구에서 DDS 형식으로 내보냅니다. 텍스처를이 형식으로 내보낼 수 있는 도구가 없는 경우 새로 만드는 것이 좋습니다. DDS 형식에 대 한 자세한 내용 및 코드에서이를 사용 하는 방법에 대 한 자세한 내용은 [Dds 프로그래밍 가이드](https://docs.microsoft.com/windows/desktop/direct3ddds/dx-graphics-dds-pguide)를 참조 하세요. 이 예제에서는 DDS를 사용 합니다.

다른 리소스 형식과 마찬가지로 파일의 데이터를 바이트 스트림으로 읽습니다. 로드 작업이 완료 되 면 람다 호출은 코드 ( **CreateTexture** 메서드)를 실행 하 여 Direct3D에서 사용할 수 있는 형식으로 바이트 스트림을 처리 합니다.

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

위의 코드 조각에서 람다는 파일 이름에 "dds"의 확장명이 있는지 확인 합니다. 이 경우 DDS 질감이 아닌 것으로 가정 합니다. 잘 모르는 경우 WIC (Windows Imaging Component) Api를 사용 하 여 형식을 검색 하 고 데이터를 비트맵으로 디코딩합니다. 어느 쪽이 든 결과는 [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) 비트맵 (또는 오류)입니다.

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

이 코드가 완료 되 면 이미지 파일에서 로드 된 [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) 메모리에 있습니다. 메시와 마찬가지로 게임 및 지정 된 장면에 많은 것이 있을 수 있습니다. 게임 또는 수준이 시작 될 때 모든 항목을 로드 하는 대신 장면 또는 수준별로 정기적으로 액세스 하는 캐시를 만드는 것이 좋습니다.

위의 샘플에서 호출 되는 **CreateDDSTextureFromMemory** 메서드는 [DDSTextureLoader에 대 한 전체 코드](complete-code-for-ddstextureloader.md)에서 전체를 탐색할 수 있습니다.

또한 개별 질감이 나 질감 "스킨"은 특정 메시 다각형 또는 표면에 매핑될 수 있습니다. 일반적으로이 매핑 데이터는 모델 및 질감을 만드는 데 사용 되는 음악가 또는 디자이너를 도구로 내보냅니다. 내보낸 데이터를 로드 하는 경우에도이 정보를 캡처해야 합니다 .이 정보를 사용 하는 경우 조각 음영을 수행할 때 올바른 질감을 해당 표면에 매핑할 수 있습니다.

### <a name="loading-shaders"></a>셰이더 로드

셰이더는 메모리에 로드 되 고 그래픽 파이프라인의 특정 단계에서 호출 되는 컴파일된 HLSL (High Level Shader Language) 파일입니다. 가장 일반적이 고 필수적인 셰이더는 꼭 짓 점 및 픽셀 셰이더 이며, 각각 망상의 개별 꼭 짓 점 및 장면의 장면에서 픽셀을 처리 합니다. HLSL 코드는 기 하 도형을 변형 하 고, 조명 효과와 질감을 적용 하 고, 렌더링 된 장면에서 후 처리를 수행 하기 위해 실행 됩니다.

Direct3D 게임에는 서로 다른 여러 셰이더가 있을 수 있으며 각각은 별도의 CSO 컴파일된 셰이더 개체, .cs) 파일로 컴파일됩니다. 일반적으로이를 동적으로 로드 해야 하는 것은 없지만 대부분의 경우 게임이 시작 될 때 또는 특정 수준 (예: 비 효과에 대 한 셰이더)을 로드할 수 있습니다.

**Basicloader** 클래스의 코드는 꼭 짓 점, 기 하 도형, 픽셀 및 선체 셰이더를 비롯 한 다양 한 셰이더에 대 한 여러 오버 로드를 제공 합니다. 아래 코드는 픽셀 셰이더를 예로 설명 합니다. ( [BasicLoader의](complete-code-for-basicloader.md)전체 코드에서 전체 코드를 검토할 수 있습니다.)

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

이 예제에서는 **basicreaderwriter** 인스턴스 (**m \_ basicreaderwriter**)를 사용 하 여 제공 된 컴파일된 셰이더 개체 (.cof) 파일에서 바이트 스트림으로 읽습니다. 작업이 완료 되 면 람다는 파일에서 로드 된 바이트 데이터를 사용 하 여 [**ID3D11Device:: CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) 를 호출 합니다. 콜백은 로드에 성공 했음을 나타내는 일부 플래그를 설정 해야 하며, 코드는 셰이더를 실행 하기 전에이 플래그를 확인 해야 합니다.

꼭 짓 점 셰이더는 약간 더 복잡 합니다. 꼭 짓 점 셰이더의 경우 꼭 짓 점 데이터를 정의 하는 별도의 입력 레이아웃도 로드 합니다. 다음 코드를 사용 하 여 사용자 지정 꼭 짓 점 입력 레이아웃과 함께 꼭 짓 점 셰이더를 비동기적으로 로드할 수 있습니다. 메시에서 로드 하는 꼭 짓 점 정보를이 입력 레이아웃으로 올바르게 나타낼 수 있어야 합니다.

꼭 짓 점 셰이더를 로드 하기 전에 입력 레이아웃을 만들어 보겠습니다.

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

이 특정 레이아웃에서 각 꼭 짓 점에는 꼭 짓 점 셰이더에 의해 처리 되는 다음 데이터가 있습니다.

-   모델 좌표 공간의 3D 좌표 위치 (x, y, z) 이며 32 비트 부동 소수점 값의 3 개 숫자로 표시 됩니다.
-   3 32 비트 부동 소수점 값으로 표시 되는 꼭 짓 점의 법선 벡터입니다.
-   32 비트 부동 소수점 값의 쌍으로 표시 되는 변환 된 2D 질감 좌표 값 (u, v)입니다.

이러한 각 꼭 짓 점 입력 요소를 [HLSL 의미 체계](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)라고 하며, 컴파일된 셰이더 개체와 데이터를 전달 하는 데 사용 되는 정의 된 레지스터 집합입니다. 파이프라인은 사용자가 로드 한 메시의 모든 꼭 짓 점 셰이더를 한 번씩 실행 합니다. 의미 체계는 실행 되는 꼭 짓 점 셰이더의 입력 및 출력을 정의 하 고, 셰이더의 HLSL 코드에서 꼭 짓 점 당 계산에 대해이 데이터를 제공 합니다.

이제 꼭 짓 점 셰이더 개체를 로드 합니다.

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

이 코드에서 꼭 짓 점 셰이더의 CSFILE 파일의 바이트 데이터를 읽은 후에는 [**ID3D11Device:: CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)를 호출 하 여 꼭 짓 점 셰이더를 만듭니다. 그런 다음 동일한 람다에서 셰이더에 대 한 입력 레이아웃을 만듭니다.

다른 셰이더 형식 (예: 선체 및 geometry 셰이더)은 특정 구성이 필요할 수도 있습니다. 다양 한 셰이더 로드 메서드에 대 한 전체 코드는 BasicLoader 및 [Direct3D 리소스 로딩 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/Direct3D%20resource%20loading%20sample%20(Windows%208)/C%2B%2B)에 [대 한 전체 코드](complete-code-for-basicloader.md) 에서 제공 됩니다.

## <a name="remarks"></a>설명

이 시점에서 메시, 질감 및 컴파일된 셰이더와 같은 일반적인 게임 리소스와 자산을 비동기적으로 로드 하기 위한 메서드를 이해 하 고 수정할 수 있어야 합니다.

## <a name="related-topics"></a>관련 항목

* [Direct3D 리소스 로딩 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/Direct3D%20resource%20loading%20sample%20(Windows%208)/C%2B%2B)
* [BasicLoader에 대 한 전체 코드](complete-code-for-basicloader.md)
* [BasicReaderWriter에 대 한 전체 코드](complete-code-for-basicreaderwriter.md)
* [DDSTextureLoader에 대 한 전체 코드](complete-code-for-ddstextureloader.md)

 

 




