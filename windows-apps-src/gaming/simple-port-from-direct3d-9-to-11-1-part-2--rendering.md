---
title: 렌더링 프레임 워크 변환
description: 기 하 도형 버퍼를 이식 하는 방법, HLSL 셰이더 프로그램을 컴파일 및 로드 하는 방법, Direct3D 11에서 렌더링 체인을 구현 하는 방법을 비롯 하 여 간단한 렌더링 프레임 워크를 Direct3D 9에서 Direct3D 11로 변환 하는 방법을 보여 줍니다.
ms.assetid: f6ca1147-9bb8-719a-9a2c-b7ee3e34bd18
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 렌더링 프레임 워크, 변환, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: 780054dd7e51456c582265c27a7e415f92cba382
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159137"
---
# <a name="convert-the-rendering-framework"></a>렌더링 프레임 워크 변환



**요약**

-   [1 부: Direct3D 11 초기화](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   2 부: 렌더링 프레임 워크 변환
-   [3 부: 게임 루프를 이식 합니다.](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


기 하 도형 버퍼를 이식 하는 방법, HLSL 셰이더 프로그램을 컴파일 및 로드 하는 방법, Direct3D 11에서 렌더링 체인을 구현 하는 방법을 비롯 하 여 간단한 렌더링 프레임 워크를 Direct3D 9에서 Direct3D 11로 변환 하는 방법을 보여 줍니다. 포트의 2 부에서는 [간단한 Direct3D 9 앱에서 DirectX 11 및 유니버설 Windows 플랫폼 (UWP) 연습을](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 안내 합니다.

## <a name="convert-effects-to-hlsl-shaders"></a>효과를 HLSL 셰이더로 변환


다음 예제는 하드웨어 꼭 짓 점 변환 및 통과 색 데이터에 대 한 레거시 효과 API 용으로 작성 된 간단한 D3DX 기술입니다.

Direct3D 9 셰이더 코드

```cpp
// Global variables
matrix g_mWorld;        // world matrix for object
matrix g_View;          // view matrix
matrix g_Projection;    // projection matrix

// Shader pipeline structures
struct VS_OUTPUT
{
    float4 Position   : POSITION;   // vertex position
    float4 Color      : COLOR0;     // vertex diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : COLOR0;  // Pixel color    
};

// Vertex shader
VS_OUTPUT RenderSceneVS(float3 vPos : POSITION, 
                        float3 vColor : COLOR0)
{
    VS_OUTPUT Output;
    
    float4 pos = float4(vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, g_mWorld);
    pos = mul(pos, g_View);
    pos = mul(pos, g_Projection);

    Output.Position = pos;
    
    // Just pass through the color data
    Output.Color = float4(vColor, 1.0f);
    
    return Output;
}

// Pixel shader
PS_OUTPUT RenderScenePS(VS_OUTPUT In) 
{ 
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}

// Technique
technique RenderSceneSimple
{
    pass P0
    {          
        VertexShader = compile vs_2_0 RenderSceneVS();
        PixelShader  = compile ps_2_0 RenderScenePS(); 
    }
}
```

Direct3D 11에서는 여전히 HLSL 셰이더를 사용할 수 있습니다. 각 셰이더를 고유한 HLSL 파일에 배치 하 여 Visual Studio에서이를 별도의 파일로 컴파일하므로 나중에 별도의 Direct3D 리소스로 로드 합니다. 이러한 셰이더는 DirectX 9.1 Gpu 용으로 작성 되었으므로 대상 수준을 [셰이더 모델 4 수준 9 \_ 1 (/4 \_ 0 \_ 수준 \_ 9 \_ 1)](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro) 로 설정 합니다.

입력 레이아웃을 정의할 때 시스템 메모리와 GPU 메모리에 꼭 짓 점 데이터를 저장 하는 데 사용 하는 것과 동일한 데이터 구조를 표시 하도록 했습니다. 마찬가지로 꼭 짓 점 셰이더의 출력은 픽셀 셰이더에 대 한 입력으로 사용 되는 구조체와 일치 해야 합니다. C + +에서 한 함수에서 다른 함수로 데이터를 전달 하는 것과 규칙이 동일 하지는 않습니다. 구조체의 끝에서 사용 하지 않는 변수를 생략할 수 있습니다. 그러나 순서를 다시 정렬할 수 없으며 데이터 구조 중간의 콘텐츠를 건너뛸 수 없습니다.

> **참고**    꼭 짓 점 셰이더를 픽셀 셰이더에 바인딩하기 위한 Direct3D 9의 규칙은 Direct3D 11의 규칙 보다 더 낮은 완화 였습니다. Direct3D 9 정렬은 유연 하지만 비효율적입니다.

 

HLSL 파일이 셰이더 의미 체계에 이전 구문을 사용할 수 있습니다. 예를 들어, SV 대상이 아닌 색을 사용할 수 \_ 있습니다. 그렇다면 HLSL compatibility 모드 (/Gec 컴파일러 옵션)를 사용 하도록 설정 하거나 셰이더 [의미 체계](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) 를 현재 구문으로 업데이트 해야 합니다. 이 예제의 꼭 짓 점 셰이더는 현재 구문으로 업데이트 되었습니다.

다음은이 시간이 자체 파일에 정의 된 하드웨어 변환 꼭 짓 점 셰이더입니다.

> **참고**    꼭 짓 점 셰이더는 SV \_ POSITION 시스템 값 의미 체계를 출력 하는 데 필요 합니다. 이 의미 체계는 x가-1과 1 사이에 있는 값을 조정 하기 위해 꼭 짓 점 위치 데이터를 확인 하 고, y는-1과 1 사이, z는 원래의 유형이 같은 좌표 w 값 (z/w)으로 나눈 후 w는 1을 원래 w 값 (1/w)으로 나눕니다.

 

HLSL 꼭 짓 점 셰이더 (기능 수준 9.1)

```cpp
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
    matrix mWorld;       // world matrix for object
    matrix View;        // view matrix
    matrix Projection;  // projection matrix
};

struct VS_INPUT
{
    float3 vPos   : POSITION;
    float3 vColor : COLOR0;
};

struct VS_OUTPUT
{
    float4 Position : SV_POSITION; // Vertex shaders must output SV_POSITION
    float4 Color    : COLOR0;
};

VS_OUTPUT main(VS_INPUT input) // main is the default function name
{
    VS_OUTPUT Output;

    float4 pos = float4(input.vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, mWorld);
    pos = mul(pos, View);
    pos = mul(pos, Projection);
    Output.Position = pos;

    // Just pass through the color data
    Output.Color = float4(input.vColor, 1.0f);

    return Output;
}
```

이는 통과 픽셀 셰이더에 모두 필요 합니다. 이를 통과 하는 경우에도 실제로 각 픽셀에 대 한 원근 올바른 보간된 색 데이터를 가져오는 것입니다. SV \_ 대상 시스템 값 의미 체계는 API에 필요한 대로 픽셀 셰이더의 출력 색 값에 적용 됩니다.

> **참고**    셰이더 수준 9 \_ x 픽셀 셰이더는 SV \_ POSITION 시스템 값 의미 체계에서 읽을 수 없습니다. 모델 4.0 이상 픽셀 셰이더는 SV 위치를 사용 \_ 하 여 화면에서 픽셀 위치를 검색할 수 있습니다. 여기서 x는 0 사이이 고 렌더링 대상 너비는 0이 고 y는 렌더링 대상 높이 (각각 0.5로 오프셋)입니다.

 

대부분의 픽셀 셰이더는 통과 보다 훨씬 더 복잡 합니다. 더 높은 Direct3D 기능 수준을 사용 하면 셰이더 프로그램 마다 훨씬 더 많은 계산을 수행할 수 있습니다.

HLSL pixel shader (기능 수준 9.1)

```cpp
struct PS_INPUT
{
    float4 Position : SV_POSITION;  // interpolated vertex position (system value)
    float4 Color    : COLOR0;       // interpolated diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : SV_TARGET;  // pixel color (your PS computes this system value)
};

PS_OUTPUT main(PS_INPUT In)
{
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}
```

## <a name="compile-and-load-shaders"></a>셰이더 컴파일 및 로드


Direct3D 9 게임은 프로그래밍 가능한 파이프라인을 구현 하는 편리한 방법으로 효과 라이브러리를 사용 하는 경우가 많습니다. [**D3DXCreateEffectFromFile 함수**](/windows/desktop/direct3d9/d3dxcreateeffectfromfile) 메서드를 사용 하 여 런타임에 결과를 컴파일할 수 있습니다.

Direct3D 9에서 효과 로드

```cpp
// Turn off preshader optimization to keep calculations on the GPU
DWORD dwShaderFlags = D3DXSHADER_NO_PRESHADER;

// Only enable debug info when compiling for a debug target
#if defined (DEBUG) || defined (_DEBUG)
dwShaderFlags |= D3DXSHADER_DEBUG;
#endif

D3DXCreateEffectFromFile(
    m_pd3dDevice,
    L"CubeShaders.fx",
    NULL,
    NULL,
    dwShaderFlags,
    NULL,
    &m_pEffect,
    NULL
    );
```

Direct3D 11은 셰이더 프로그램에서 이진 리소스로 작동 합니다. 셰이더는 프로젝트가 빌드되고 리소스로 처리 될 때 컴파일됩니다. 이 예제에서는 셰이더 바이트 코드를 시스템 메모리에 로드 하 고, Direct3D 장치 인터페이스를 사용 하 여 각 셰이더에 대 한 Direct3D 리소스를 만들고, 각 프레임을 설정할 때 Direct3D 셰이더 리소스를 가리킵니다.

Direct3D 11에서 셰이더 리소스 로드

```cpp
// BasicReaderWriter is a tested file loader used in SDK samples.
BasicReaderWriter^ readerWriter = ref new BasicReaderWriter();


// Load vertex shader:
Platform::Array<byte>^ vertexShaderData =
    readerWriter->ReadData("CubeVertexShader.cso");

// This call allocates a device resource, validates the vertex shader 
// with the device feature level, and stores the vertex shader bits in 
// graphics memory.
m_d3dDevice->CreateVertexShader(
    vertexShaderData->Data,
    vertexShaderData->Length,
    nullptr,
    &m_vertexShader
    );
```

셰이더 바이트 코드를 컴파일된 앱 패키지에 포함하려면 HLSL 파일을 Visual Studio 프로젝트에 추가하기만 하면 됩니다. Visual Studio는 Fxc.exe ( [효과-컴파일러 도구](/windows/desktop/direct3dtools/fxc) )를 사용 하 여 HLSL 파일을 컴파일된 셰이더 개체로 컴파일합니다 (. 앱 패키지에 포함 되어 있습니다.

> **참고**    HLSL 컴파일러에 대 한 올바른 대상 기능 수준을 설정 해야 합니다. Visual Studio에서 HLSL 원본 파일을 마우스 오른쪽 단추로 클릭 하 고 속성을 선택한 후 **HLSL 컴파일러- &gt; 일반**에서 **셰이더 모델** 설정을 변경 합니다. Direct3D는 앱이 Direct3D 셰이더 리소스를 만들 때 하드웨어 기능에 대해이 속성을 확인 합니다.

 

![hlsl 셰이더 속성](images/hlslshaderpropertiesmenu.png)![hlsl 셰이더 형식](images/hlslshadertypeproperties.png)

이는 Direct3D 9의 꼭 짓 점 스트림 선언에 해당 하는 입력 레이아웃을 만드는 좋은 장소입니다. 꼭 짓 점 데이터 구조는 꼭 짓 점 셰이더가 사용 하는 것과 일치 해야 합니다. Direct3D 11에서 입력 레이아웃을 보다 세부적으로 제어할 수 있습니다. 부동 소수점 벡터의 배열 크기와 비트 길이를 정의 하 고 꼭 짓 점 셰이더의 의미 체계를 지정할 수 있습니다. [**D3D11 \_ INPUT \_ 요소 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) 구조를 만들고이를 사용 하 여 Direct3D에서 꼭 짓 점 별 데이터가 표시 되는 모양을 알려 줍니다. API는 꼭 짓 점 셰이더 리소스에 대해 입력 레이아웃의 유효성을 검사 하기 때문에 입력 레이아웃을 정의 하기 위해 꼭 짓 점 셰이더를 로드할 때까지 대기 합니다. 입력 레이아웃이 호환 되지 않으면 Direct3D는 예외를 throw 합니다.

꼭 짓 점 별 데이터는 시스템 메모리의 호환 가능한 형식에 저장 해야 합니다. DirectXMath 데이터 형식이 유용할 수 있습니다. 예를 들어 DXGI \_ FORMAT \_ R32G32B32 \_ FLOAT는 [**XMFLOAT3**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat3)에 해당 합니다.

> **참고**    상수 버퍼는 한 번에 4 개의 부동 소수점 숫자에 맞는 고정 입력 레이아웃을 사용 합니다. [**XMFLOAT4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) (및 해당 파생물)은 상수 버퍼 데이터에 권장 됩니다.

 

Direct3D 11에서 입력 레이아웃 설정

```cpp
// Create input layout:
const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT,
        0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },

    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 
        0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
```

## <a name="create-geometry-resources"></a>기 하 도형 리소스 만들기


Direct3D 9에서는 Direct3D 장치에서 버퍼를 만들고 메모리를 잠그고 CPU 메모리에서 GPU 메모리로 데이터를 복사 하 여 geometry 리소스를 저장 했습니다.

Direct3D 9

```cpp
// Create vertex buffer:
VOID* pVertices;

// In Direct3D 9 we create the buffer, lock it, and copy the data from 
// system memory to graphics memory.
m_pd3dDevice->CreateVertexBuffer(
    sizeof(CubeVertices),
    0,
    D3DFVF_XYZ | D3DFVF_DIFFUSE,
    D3DPOOL_MANAGED,
    &pVertexBuffer,
    NULL);

pVertexBuffer->Lock(
    0,
    sizeof(CubeVertices),
    &pVertices,
    0);

memcpy(pVertices, CubeVertices, sizeof(CubeVertices));
pVertexBuffer->Unlock();
```

DirectX 11은 보다 간단한 프로세스를 따릅니다. API는 시스템 메모리에서 GPU로 데이터를 자동으로 복사 합니다. COM 스마트 포인터를 사용 하 여 프로그래밍을 보다 쉽게 만들 수 있습니다.

DirectX 11

```cpp
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = CubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(
    sizeof(CubeVertices),
    D3D11_BIND_VERTEX_BUFFER);
  
// This call allocates a device resource for the vertex buffer and copies
// in the data.
m_d3dDevice->CreateBuffer(
    &vertexBufferDesc,
    &vertexBufferData,
    &m_vertexBuffer
    );
```

## <a name="implement-the-rendering-chain"></a>렌더링 체인 구현


Direct3D 9 게임은 종종 효과 기반 렌더링 체인을 사용 했습니다. 이 유형의 렌더링 체인은 효과 개체를 설정 하 고, 필요한 리소스를 제공 하며, 각 패스를 렌더링할 수 있도록 합니다.

Direct3D 9 렌더링 체인

```cpp
// Clear the render target and the z-buffer.
m_pd3dDevice->Clear(
    0, NULL,
    D3DCLEAR_TARGET | D3DCLEAR_ZBUFFER,
    D3DCOLOR_ARGB(0, 45, 50, 170),
    1.0f, 0
    );

// Set the effect technique
m_pEffect->SetTechnique("RenderSceneSimple");

// Rotate the cube 1 degree per frame.
D3DXMATRIX world;
D3DXMatrixRotationY(&world, D3DXToRadian(m_frameCount++));


// Set the matrices up using traditional functions.
m_pEffect->SetMatrix("g_mWorld", &world);
m_pEffect->SetMatrix("g_View", &m_view);
m_pEffect->SetMatrix("g_Projection", &m_projection);

// Render the scene using the Effects library.
if(SUCCEEDED(m_pd3dDevice->BeginScene()))
{
    // Begin rendering effect passes.
    UINT passes = 0;
    m_pEffect->Begin(&passes, 0);
    
    for (UINT i = 0; i < passes; i++)
    {
        m_pEffect->BeginPass(i);
        
        // Send vertex data to the pipeline.
        m_pd3dDevice->SetFVF(D3DFVF_XYZ | D3DFVF_DIFFUSE);
        m_pd3dDevice->SetStreamSource(
            0, pVertexBuffer,
            0, sizeof(VertexPositionColor)
            );
        m_pd3dDevice->SetIndices(pIndexBuffer);
        
        // Draw the cube.
        m_pd3dDevice->DrawIndexedPrimitive(
            D3DPT_TRIANGLELIST,
            0, 0, 8, 0, 12
            );
        m_pEffect->EndPass();
    }
    m_pEffect->End();
    
    // End drawing.
    m_pd3dDevice->EndScene();
}

// Present frame:
// Show the frame on the primary surface.
m_pd3dDevice->Present(NULL, NULL, NULL, NULL);
```

DirectX 11 렌더링 체인은 여전히 동일한 작업을 수행 하지만 렌더링 패스를 다르게 구현 해야 합니다. 추가 정보를 FX 파일에 배치 하 고 렌더링 기법을 c + + 코드에 보다 투명 하 게 하는 대신 c + +에서 모든 렌더링을 설정 합니다.

렌더링 체인은 다음과 같습니다. 꼭 짓 점 셰이더를 로드 한 후에 만든 입력 레이아웃을 제공 하 고, 각 셰이더 개체를 제공 하 고, 각 셰이더에 사용할 상수 버퍼를 지정 해야 합니다. 이 예제에는 여러 렌더링 패스가 포함 되지 않지만 각 패스에 대해 유사한 렌더링 체인을 수행 하는 경우 필요에 따라 설정을 변경 합니다.

Direct3D 11 렌더링 체인

```cpp
// Clear the back buffer.
const float midnightBlue[] = { 0.098f, 0.098f, 0.439f, 1.000f };
m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    midnightBlue
    );

// Set the render target. This starts the drawing operation.
m_d3dContext->OMSetRenderTargets(
    1,  // number of render target views for this drawing operation.
    m_renderTargetView.GetAddressOf(),
    nullptr
    );


// Rotate the cube 1 degree per frame.
XMStoreFloat4x4(
    &m_constantBufferData.model, 
    XMMatrixTranspose(XMMatrixRotationY(m_frameCount++ * XM_PI / 180.f))
    );

// Copy the updated constant buffer from system memory to video memory.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,      // update the 0th subresource
    NULL,   // use the whole destination
    &m_constantBufferData,
    0,      // default pitch
    0       // default pitch
    );


// Send vertex data to the Input Assembler stage.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;

m_d3dContext->IASetVertexBuffers(
    0,  // start with the first vertex buffer
    1,  // one vertex buffer
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset
    );

m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0   // no offset
    );

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());


// Set the vertex shader.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0
    );

// Set the vertex shader constant buffer data.
m_d3dContext->VSSetConstantBuffers(
    0,  // register 0
    1,  // one constant buffer
    m_constantBuffer.GetAddressOf()
    );


// Set the pixel shader.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0
    );


// Draw the cube.
m_d3dContext->DrawIndexed(
    m_indexCount,
    0,  // start with index 0
    0   // start with vertex 0
    );
```

스왑 체인은 그래픽 인프라의 일부 이므로 DXGI 스왑 체인을 사용 하 여 완료 된 프레임을 표시 합니다. DXGI는 다음 vsync 될 때까지 호출을 차단 합니다. 그런 다음를 반환 하 고 게임 루프에서 다음 반복까지 계속할 수 있습니다.

DirectX 11을 사용 하 여 화면에 프레임 프레젠테이션

```cpp
m_swapChain->Present(1, 0);
```

방금 만든 렌더링 체인은 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 메서드에 구현 된 게임 루프에서 호출 됩니다. 이는 [3 부: 뷰포트 및 게임 루프](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)에서 표시 됩니다.

 

 