---
title: OpenGL ES 2.0 버퍼,가 수 형태, 꼭 짓 점을 Direct3D로 이식
description: OpenGL ES 2.0에서 Direct3D 11로 이식 하는 프로세스 중에 앱과 셰이더 프로그램 간에 데이터를 전달 하기 위한 구문 및 API 동작을 변경 해야 합니다.
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, opengl, direct3d, 버퍼, 모든 형식, 꼭 짓 점 특성
ms.localizationpriority: medium
ms.openlocfilehash: 0ba95f2279491d1f1ab2b27c2aaf7a9a7e2409b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159327"
---
# <a name="compare-opengl-es-20-buffers-uniforms-and-vertex-attributes-to-direct3d"></a>OpenGL ES 2.0 버퍼, 모든 형식 및 꼭 짓 점 특성을 Direct3D와 비교




**중요 API**

-   [**ID3D11Device1:: CreateBuffer**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11Device1:: CreateInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout)
-   [**ID3D11DeviceContext1::IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout)

OpenGL ES 2.0에서 Direct3D 11로 이식 하는 프로세스 중에 앱과 셰이더 프로그램 간에 데이터를 전달 하기 위한 구문 및 API 동작을 변경 해야 합니다.

OpenGL ES 2.0에서 데이터는 여러 가지 방법으로 셰이더 프로그램에 전달 됩니다. 즉, 상수 데이터의 경우에는 꼭 짓 점 데이터의 특성으로, 다른 리소스 데이터에 대 한 버퍼 개체 (예: 질감)로 데이터를 전달 합니다. Direct3D 11에서이는 상수 버퍼, 꼭 짓 점 버퍼 및 하위 리소스에 약 매핑됩니다. 피상적인 공통점에도 불구 하 고 사용에서 매우 다르게 처리 됩니다.

기본 매핑은 다음과 같습니다.

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 균일                   | 상수 버퍼 (**cbuffer**) 필드입니다.                                                                                                                                                |
| 특성                 | 입력 레이아웃으로 지정 되 고 특정 HLSL 의미 체계를 사용 하 여 표시 되는 꼭 짓 점 버퍼 요소 필드입니다.                                                                                |
| buffer 개체             | 버퍼 [**D3D11 \_ subresource \_ DATA**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 및 [**D3D11 \_ buffer \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) 및 일반 사용 버퍼 정의를 참조 하세요. |
| 프레임 버퍼 개체 (FBO) | 렌더링 대상 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) with [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)을 참조 하세요.                                       |
| 백 버퍼               | "백 버퍼" 표면의 스왑 체인 [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) With attached [**IDXGISurface1**](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1)를 참조 하세요.                       |

 

## <a name="port-buffers"></a>포트 버퍼


OpenGL ES 2.0에서 모든 종류의 버퍼를 만들고 바인딩하는 프로세스는 일반적으로이 패턴을 따릅니다.

-   하나 이상의 버퍼를 생성 하 고이에 대 한 핸들을 반환 하려면 글 Genbuffers를 호출 합니다.
-   에이를 호출 하 여 GL \_ 요소 배열 버퍼와 같은 버퍼의 레이아웃을 정의 합니다 \_ \_ .
-   지정 된 레이아웃의 특정 데이터 (예: 꼭 짓 점 구조, 인덱스 데이터 또는 색 데이터)로 버퍼를 채우려면 글 된 데이터를 호출 합니다.

가장 일반적인 유형의 버퍼는 꼭 짓 점 버퍼 이며, 일부 좌표계에서 꼭 짓 점 위치를 최소한으로 포함 합니다. 일반적인 사용에서 꼭 짓 점은 위치 좌표, 꼭 짓 점 위치의 법선 벡터, 꼭 짓 점 위치에 대 한 탄젠트 벡터 및 텍스처 조회 (uv) 좌표를 포함 하는 구조체로 표현 됩니다. 버퍼에는 이러한 꼭 짓 점의 연속 된 목록이 순서 대로 (삼각형 목록, 줄무늬 또는 팬), 장면에 표시 되는 다각형이 전체적으로 표시 되어 있습니다. (Direct3D 11 및 OpenGL ES 2.0에서는 그리기 호출 당 여러 꼭 짓 점 버퍼를 포함 하는 것이 비효율적입니다.)

다음은 OpenGL ES 2.0를 사용 하 여 만든 버텍스 버퍼 및 인덱스 버퍼의 예입니다.

OpenGL ES 2.0: 꼭 짓 점 버퍼 및 인덱스 버퍼를 만들고 채웁니다.

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

다른 버퍼에는 질감 같은 픽셀 버퍼와 맵이 포함 됩니다. 셰이더 파이프라인은 pixmaps (텍스처 버퍼) 또는 렌더링 버퍼 개체에 렌더링 하 고 이후 셰이더 패스에서 이러한 버퍼를 사용할 수 있습니다. 가장 간단한 경우 호출 흐름은 다음과 같습니다.

-   프레임 버퍼 개체를 생성 하려면 글 하 버퍼를 호출 합니다.
-   GlBindFramebuffer를 호출 하 여 프레임 버퍼 개체를 쓰기용으로 바인딩합니다.
-   GlFramebufferTexture2D를 호출 하 여 지정 된 질감 지도에 그립니다.

Direct3D 11에서 버퍼 데이터 요소는 "subresources"로 간주 되며 개별 꼭 짓 점 데이터 요소부터 밉 맵 질감까지 범위를 지정할 수 있습니다.

-   [**D3D11 \_ subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 구조를 버퍼 데이터 요소에 대 한 구성으로 채웁니다.
-   버퍼 형식 뿐만 아니라 버퍼의 개별 요소 크기를 사용 하 여 [**D3D11 \_ buffer \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) 구조를 채웁니다.
-   이러한 두 개의 구조를 사용 하 여 [**ID3D11Device1:: CreateBuffer**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 를 호출 합니다.

Direct3D 11: 꼭 짓 점 버퍼 및 인덱스 버퍼 만들기 및 채우기

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);

m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);
    
```

프레임 버퍼와 같은 쓰기 가능한 픽셀 버퍼 또는 맵은 [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) 개체로 만들 수 있습니다. 이는 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 또는 [**ID3D11ShaderResourceView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview)에 리소스로 바인딩할 수 있으며,이는에 그려진 후 연결 된 스왑 체인으로 표시 되거나 셰이더에 전달 될 수 있습니다.

Direct3D 11: 프레임 버퍼 개체 만들기

``` syntax
ComPtr<ID3D11RenderTargetView> m_d3dRenderTargetViewWin;
// ...
ComPtr<ID3D11Texture2D> frameBuffer;

m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&frameBuffer));
m_d3dDevice->CreateRenderTargetView(
  frameBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

## <a name="change-uniforms-and-uniform-buffer-objects-to-direct3d-constant-buffers"></a>단일 형식 및 균일 버퍼 개체를 Direct3D 상수 버퍼로 변경


Open GL ES 2.0에서 일정 한 형식은 개별 셰이더 프로그램에 상수 데이터를 제공 하는 메커니즘입니다. 셰이더는이 데이터를 변경할 수 없습니다.

일반적으로 uniform을 설정 하는 것은 \* 응용 프로그램 메모리의 데이터에 대 한 포인터와 함께 GPU의 업로드 위치를 포함 하는 뛰어난 방법 중 하나를 제공 하는 것입니다. Ithe이 메서드가 실행 된 후에는 \* 균일 한 데이터가 GPU 메모리에 있고 해당 하는이를 선언한 셰이더가 액세스할 수 있습니다. 셰이더가 셰이더 (호환 되는 형식 사용)의 균일 한 선언에 따라 해석할 수 있는 방식으로 데이터가 압축 되도록 해야 합니다.

OpenGL ES 2.0 균일 하 고 데이터를 업로드 하는 파일 만들기

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

셰이더의 고 대 중 SL에서 해당 하는 균일 선언은 다음과 같습니다.

GL ES 2.0:가 중 SL를 엽니다.

``` syntax
uniform mat4 u_mvpMatrix;
```

Direct3D는 단일 데이터를 "상수 버퍼"로 지정 합니다 .이는 유니폼 형태와 마찬가지로 개별 셰이더에 제공 되는 상수 데이터를 포함 합니다. 균일 한 버퍼와 마찬가지로 메모리의 상수 버퍼 데이터를 셰이더에 해석 해야 하는 방식과 동일 하 게 압축 하는 것이 중요 합니다. 플랫폼 형식 (예: **float \* ** 또는 **Float \[ \] 4**) 대신 Directxmath 형식 (예: [**XMFLOAT4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4))을 사용 하면 적절 한 데이터 요소 맞춤이 보장 됩니다.

상수 버퍼에는 GPU에서 해당 데이터를 참조 하는 데 사용 되는 연결 된 GPU 레지스터가 있어야 합니다. 데이터는 버퍼 레이아웃으로 표시 되는 레지스터 위치에 압축 됩니다.

Direct3D 11: 상수 버퍼 만들기 및 데이터 업로드

``` syntax
struct ModelViewProjectionConstantBuffer
{
     DirectX::XMFLOAT4X4 mvp;
};

// ...

ModelViewProjectionConstantBuffer   m_constantBufferData;

// ...

XMStoreFloat4x4(&m_constantBufferData.mvp, mvpMatrix);

CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);
m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);
```

셰이더의 HLSL에서 해당 하는 상수 버퍼 선언은 다음과 같습니다.

Direct3D 11: 상수 버퍼 HLSL 선언

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

각 상수 버퍼에 대해 레지스터를 선언 해야 합니다. Direct3D 기능 수준 마다 사용 가능한 최대 레지스터가 다르므로 대상으로 하는 가장 낮은 기능 수준의 최대 개수를 초과 하지 않습니다.

## <a name="port-vertex-attributes-to-a-direct3d-input-layouts-and-hlsl-semantics"></a>Direct3D 입력 레이아웃 및 HLSL 의미 체계에 꼭 짓 점 특성을 이식 합니다.


꼭 짓 점 데이터는 셰이더 파이프라인에서 수정할 수 있기 때문에 OpenGL ES 2.0에서는 "모든 형식" 대신 "특성"으로 지정 해야 합니다. 이는 최신 버전의 OpenGL 및 글 SL에서 변경 되었습니다. 꼭 짓 점 위치, 법선, 접선 및 색 값과 같은 꼭 짓 점 특정 데이터는 셰이더에 특성 값으로 제공 됩니다. 이러한 특성 값은 꼭 짓 점 데이터의 각 요소에 대 한 특정 오프셋에 해당 합니다. 예를 들어, 첫 번째 특성은 개별 꼭 짓 점의 위치 구성 요소를 가리키는 것이 고, 두 번째 특성은 정상으로 설정 되는 식입니다.

꼭 짓 점 버퍼 데이터를 주 메모리에서 GPU로 이동 하는 기본 프로세스는 다음과 같습니다.

-   고가를 사용 하 여 꼭 짓 점 데이터를 업로드 합니다.
-   GlGetAttribLocation를 사용 하 여 GPU에서 특성의 위치를 가져옵니다. 꼭 짓 점 데이터 요소의 각 특성에 대해이 메서드를 호출 합니다.
-   GlVertexAttribPointer를 호출 하 여 개별 꼭 짓 점 데이터 요소 내에 올바른 특성 크기와 오프셋을 제공 합니다. 각 특성에 대해이 작업을 수행 합니다.
-   GlEnableVertexAttribArray를 사용 하 여 꼭 짓 점 데이터 레이아웃 정보를 사용 하도록 설정 합니다.

OpenGL ES 2.0: 꼭 짓 점 버퍼 데이터를 셰이더 특성에 업로드 합니다.

``` syntax
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->vertexBuffer);
loc = glGetAttribLocation(renderer->programObject, "a_position");

glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), 0);
loc = glGetAttribLocation(renderer->programObject, "a_color");
glEnableVertexAttribArray(loc);

glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);
```

이제 꼭 짓 점 셰이더에서 glGetAttribLocation에 대 한 호출에서 정의한 것과 동일한 이름을 사용 하 여 특성을 선언 합니다.

OpenGL ES 2.0:에 특성을 선언 합니다.

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

일부 방법에서는 동일한 프로세스가 Direct3D에 대해 유지 됩니다. 특성 대신 꼭 짓 점 데이터는 꼭 짓 점 버퍼 및 해당 인덱스 버퍼를 포함 하는 입력 버퍼에 제공 됩니다. 그러나 Direct3D에는 "attribute" 선언이 없으므로 꼭 짓 점 버퍼에 있는 데이터 요소의 개별 구성 요소를 선언 하는 입력 레이아웃을 지정 하 고 해당 구성 요소가 꼭 짓 점 셰이더로 해석 되는 위치 및 방법을 나타내는 HLSL 의미 체계를 지정 해야 합니다. HLSL 의미 체계를 사용 하려면 셰이더 엔진을 용도에 따라 알려주는 특정 문자열을 사용 하 여 각 구성 요소를 정의 해야 합니다. 예를 들어 꼭 짓 점 위치 데이터는 위치로 표시 되 고 일반 데이터는 보통으로 표시 되 고 꼭 짓 점 색 데이터는 색으로 표시 됩니다. (다른 셰이더 단계에는 특정 의미 체계가 필요 하며, 이러한 의미 체계는 셰이더 단계를 기준으로 다르게 해석 됩니다.) HLSL 의미에 대 한 자세한 내용은 [셰이더 파이프라인](change-your-shader-loading-code.md) 및 [HLSL 의미 체계](/windows/desktop/direct3dhlsl/dcl-usage---ps)를 참조 하세요.

꼭 짓 점 및 인덱스 버퍼를 설정 하 고 입력 레이아웃을 설정 하는 프로세스는 Direct3D 그래픽 파이프라인의 IA (입력 어셈블리) 단계 라고 통칭 됩니다.

Direct3D 11: 입력 어셈블리 단계 구성

``` syntax
// Set up the IA stage corresponding to the current draw operation.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset);

m_d3dContext->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT,
        0);

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

입력 레이아웃은 꼭 짓 점 데이터 요소의 형식과 각 구성 요소에 사용 되는 의미 체계를 선언 하 여 꼭 짓 점 셰이더로 선언 되 고 연결 됩니다. 만든 D3D11 INPUT 요소에서 설명 하는 꼭 짓 점 요소 데이터 레이아웃은 \_ \_ \_ 해당 구조체의 레이아웃과 일치 해야 합니다. 여기서는 두 가지 구성 요소를 포함 하는 꼭 짓 점 데이터의 레이아웃을 만듭니다.

-   (X, y, z) 좌표에 대 한 3 32 비트 부동 소수점 값의 정렬 된 배열인 XMFLOAT3으로 주 메모리에 표시 되는 꼭 짓 점 좌표입니다.
-   XMFLOAT4으로 표현 되는 꼭 짓 점 색 값입니다 .이 값은 색 (RGBA)의 4 32 비트 부동 소수점 값에 대 한 정렬 된 배열입니다.

서식 유형 뿐만 아니라 각 항목에 대 한 의미 체계를 할당 합니다. 그런 다음 설명을 [**ID3D11Device1:: CreateInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout)에 전달 합니다. 입력 레이아웃은 render 메서드를 실행 하는 동안 입력 어셈블리를 설정할 때 [**ID3D11DeviceContext1:: IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) 를 호출할 때 사용 됩니다.

Direct3D 11: 특정 의미 체계를 사용 하 여 입력 레이아웃 설명

``` syntax
ComPtr<ID3D11InputLayout> m_inputLayout;

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileData->Length,
  &m_inputLayout);

// ...
// When we start the drawing process...

m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

마지막으로 입력을 선언 하 여 셰이더가 입력 데이터를 이해할 수 있는지 확인 합니다. 레이아웃에 할당 된 의미 체계를 사용 하 여 GPU 메모리의 올바른 위치를 선택 합니다.

Direct3D 11: HLSL 의미 체계를 사용 하 여 셰이더 입력 데이터 선언

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 