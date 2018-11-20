---
author: mtoepke
title: OpenGL ES 2.0 버퍼, 유니폼, 꼭짓점을 Direct3D로 포팅
description: OpenGL ES 2.0에서 Direct3D 11로 포팅하는 프로세스 중에 앱과 셰이더 프로그램 사이에 데이터를 전달하기 위한 구문 및 API 동작을 변경해야 합니다.
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, opengl, direct3d, 버퍼, 유니폼, 꼭짓점 특성
ms.localizationpriority: medium
ms.openlocfilehash: bc0192eb4b89ef91bc895a96e46cd39524f24c44
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7278871"
---
# <a name="compare-opengl-es-20-buffers-uniforms-and-vertex-attributes-to-direct3d"></a>OpenGL ES 2.0 버퍼, 유니폼 및 꼭짓점 특성과 Direct3D 비교




**중요 API**

-   [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)

OpenGL ES 2.0에서 Direct3D 11로 포팅하는 프로세스 중에 앱과 셰이더 프로그램 사이에 데이터를 전달하기 위한 구문 및 API 동작을 변경해야 합니다.

OpenGL ES 2.0 데이터는 네 가지 방법으로 셰이더 프로그램 간에 전달됩니다. 즉, 상수 데이터는 uniform, 꼭짓점 데이터는 특성, 다른 리소스 데이터(예: 질감)는 버퍼 개체로 전달됩니다. Direct3D 11에서 이러한 데이터는 상수 버퍼, 꼭짓점 버퍼 및 하위 리소스에 대략적으로 매핑됩니다. 표면상의 범용성에도 불구하고, 이러한 데이터는 사용 시 전혀 다르게 처리됩니다.

기본 매핑은 다음과 같습니다.

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniform                   | 상수 버퍼(**cbuffer**) 필드.                                                                                                                                                |
| 특성                 | 입력 레이아웃에서 지정하고 특정 HLSL 의미 체계로 표시된 꼭짓점 버퍼 요소 필드.                                                                                |
| 버퍼 개체             | 버퍼. 일반 사용 버퍼 정의는 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 및 [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)를 참조하세요. |
| 프레임 버퍼 개체(FBO) | 렌더링 대상. [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 및 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)를 참조하세요.                                       |
| 백 버퍼               | "백 버퍼" 화면이 있는 스왑 체인. 연결된 [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343)과 함께 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)을 참조하세요.                       |

 

## <a name="port-buffers"></a>포트 버퍼


OpenGL ES 2.0에서 모든 종류의 버퍼를 만들고 바인딩하기 위한 프로세스는 일반적으로 이 패턴을 따릅니다.

-   glGenBuffers를 호출하여 하나 이상의 버퍼를 생성하고 핸들을 이러한 버퍼에 반환합니다.
-   glBindBuffer를 호출하여 버퍼의 레이아웃(예: GL\_ELEMENT\_ARRAY\_BUFFER)을 정의합니다.
-   glBufferData를 호출하여 특정 레이아웃에서 버퍼를 특정 데이터(예: 꼭짓점 구조, 인덱스 데이터 또는 색상 데이터)로 채웁니다.

가장 일반적인 종류의 버퍼는 일부 좌표계에서 최소한으로 꼭짓점 위치를 포함하는 꼭짓점 버퍼입니다. 일반적인 사용에서 꼭짓점은 위치 좌표, 꼭짓점 위치에 대한 일반 벡터, 꼭짓점 위치에 대한 탄젠트 벡터 및 텍스처 조회(uv) 좌표를 포함하는 구조로 표시됩니다. 해당 버퍼에는 몇 가지 순서(예: 삼각형 목록이나 스트립 또는 팬)로 해당 꼭짓점의 연속 목록이 포함되어 있으며, 장면에서 보이는 다각형을 집합적으로 나타냅니다. (Direct3D 11뿐만 아니라 OpenGL ES 2.0에서 그리기 호출마다 여러 꼭짓점 버퍼가 있는 것은 비효율적입니다.)

OpenGL ES 2.0으로 만든 꼭짓점 버퍼 및 인덱스 버퍼의 예는 다음과 같습니다.

OpenGL ES 2.0: 꼭짓점 버퍼 및 인덱스 버퍼 만들기 및 채우기.

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

기타 버퍼는 텍스처와 같은 픽셀 버퍼 및 맵을 포함합니다. 셰이더 파이프라인은 텍스처 버퍼(pixmaps)에 렌더링하거나 버퍼 개체를 렌더링하고 향후 셰이더 전달에서 이러한 버퍼를 사용할 수 있습니다. 가장 간단한 경우, 호출 흐름은 다음과 같습니다.

-   glGenFramebuffers를 호출하여 프레임 버퍼 개체를 생성합니다.
-   glBindFramebuffer를 호출하여 쓰기 위한 프레임 버퍼 개체를 바인딩합니다.
-   glFramebufferTexture2D를 호출하여 지정된 텍스처 맵에 그립니다.

Direct3D 11에서 버퍼 데이터 요소는 "하위 리소스"로 간주되며 개별 꼭짓점 데이터 요소에서 MIP-map 텍스처까지 포함할 수 있습니다.

-   [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 구조를 버퍼 데이터 요소에 대한 구성으로 채웁니다.
-   [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 구조를 버퍼 유형뿐만 아니라 버퍼의 개별 요소 크기로 채웁니다.
-   이 두 구조로 [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)를 호출합니다.

Direct3D 11: 꼭짓점 버퍼 및 인덱스 버퍼 만들기 및 채우기.

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

프레임 버퍼와 같은 쓰기 가능한 픽셀 버퍼 또는 맵은 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 개체로 만들 수 있습니다. 이러한 항목은 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 또는 [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)에 리소스로 바인딩될 수 있으며, 그린 다음에는 각각 연결된 스왑 체인과 함께 표시되거나 셰이더로 전달될 수 있습니다.

Direct3D 11: 프레임 버퍼 개체 만들기.

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

## <a name="change-uniforms-and-uniform-buffer-objects-to-direct3d-constant-buffers"></a>Uniform와 uniform 버퍼 개체를 Direct3D 상수 버퍼로 변경


Open GL ES 2.0에서 uniform은 개별 셰이더 프로그램에 상수 데이터를 제공하는 메커니즘입니다. 이 데이터는 셰이더가 변경할 수 없습니다.

일반적으로 uniform 설정에서는 앱 메모리의 데이터에 대한 포인터와 함께 GPU에서 업로드 위치에서 glUniform\* 메서드 중 하나를 제공하는 작업이 포함됩니다. glUniform\* 메서드를 실행한 후 uniform 데이터는 GPU 메모리에 있으며 uniform을 선언한 셰이더가 액세스할 수 있습니다. 셰이더의 uniform 선언에 따라 셰이더가 데이터를 해석할 수 있는 방식으로 데이터가 압축되는지 확인해야 합니다(호환되는 유형 사용).

OpenGL ES 2.0: uniform 만들기 및 이 niform에 데이터 로드

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

셰이더의 GLSL에서, 해당 uniform 선언은 다음과 같습니다.

Open GL ES 2.0: GLSL uniform 선언

``` syntax
uniform mat4 u_mvpMatrix;
```

Direct3D는 uniform과 같이 개별 셰이더에 제공된 상수 데이터가 포함된 "상수 버퍼"로 uniform 데이터를 지정합니다. uniform 버퍼에서와 같이, 셰이더가 해석하는 방법과 동일하게 메모리에서 상수 버퍼 데이터를 압축하는 것이 중요합니다. 플랫폼 형식(예 **float\*** 또는 **float\[4\]**) 대신 DirectXMath 유형(예: [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608))을 사용하면 적절한 데이터 요소 맞춤이 보장됩니다.

상수 버퍼에는 GPU에서 해당 데이터를 참조하는 데 사용되는 연결된 GPU 레지스터가 있어야 합니다. 데이터는 버퍼의 레이아웃으로 표시된 대로 레지스터 위치에 압축됩니다.

Direct3D 11: 상수 버퍼 만들기 및 이 버퍼에 데이터 업로드

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

셰이더의 GLSL에서 해당 상수 버퍼 선언은 다음과 같습니다.

Direct3D 11: 상수 버퍼 HLSL 선언

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

각 상수 버퍼에 대해 레지스터를 선언해야 합니다. 여러 Direct3D 기능 수준에는 서로 다른 최대 사용 가능 레지스터가 있으므로, 대상으로 하는 가장 낮은 기능 수준에 대한 최대 수를 초과하지 마세요.

## <a name="port-vertex-attributes-to-a-direct3d-input-layouts-and-hlsl-semantics"></a>Direct3D 입력 레이아웃 및 HLSL 의미 체계에 꼭짓점 특성 포팅


셰이더 파이프라인으로 꼭짓점 데이터를 수정할 수 있으므로 OpenGL ES 2.0을 사용하려면 "uniform" 대신 "특성"으로 이러한 데이터를 지정해야 합니다. 이는 최신 버전의 OpenGL 및 GLSL에서 변경되었습니다. 꼭짓점 위치, 법선, 접선 및 색상 값 등 꼭짓점 관련 데이터는 특성 값으로 셰이더에 제공됩니다. 이러한 특성 값은 꼭짓점 데이터의 각 요소에 대한 특정 오프셋에 해당 합니다. 예를 들어, 첫 번째 특성은 개별 꼭짓점의 위치 구성 요소를 가리키고 두 번째 특성은 법선을 가리키는 등과 같을 수 있습니다.

주 메모리에서 GPU로 꼭짓점 버퍼 데이터 이동에 대한 기본 프로세스는 다음과 같습니다.

-   GlBindBuffer를 사용하여 꼭짓점 데이터를 업로드합니다.
-   glGetAttribLocation을 사용하여 GPU에서 특성 위치를 가져옵니다. 꼭짓점 데이터 요소의 각 특성에 대해 해당 항목을 호출합니다.
-   glVertexAttribPointer를 호출하여 개별 꼭짓점 데이터 요소 내에서 올바른 특성 크기 및 오프셋을 설정합니다. 각 특성에 대해 이 작업을 수행합니다.
-   glEnableVertexAttribArray를 사용하여 꼭짓점 데이터 레이아웃 정보를 사용하도록 설정합니다.

OpenGL ES 2.0: 셰이더 특성에 꼭짓점 버퍼 데이터 업로드

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

이제, 꼭짓점 셰이더에서 glGetAttribLocation에 대한 호출에 정의된 이름이 같은 특성을 선언합니다.

OpenGL ES 2.0: GLSL에서 특성 선언

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

어떤 면에서 Direct3D에 대해 같은 프로세스가 있습니다. 특성 대신 꼭짓점 데이터가 입력 버퍼에서 제공됩니다. 이러한 버퍼에는 꼭짓점 버퍼 및 해당 인덱스 버퍼가 포함됩니다. 그러나 Direct3D에는 "특성" 선언이 없으므로, 꼭짓점 셰이더로 구성 요소가 해석되는 위치와 방법을 나타내는 HLSL 의미 체계 및 꼭짓점 버퍼에서 데이터 요소의 개별 구성 요소를 선언하는 입력 레이아웃을 지정해야 합니다. HLSL 의미 체계를 사용하려면 용도에 따라 셰이더 엔진에 알려주는 특정 문자열을 사용하여 각 구성 요소의 사용법을 정의해야 합니다. 예를 들어, 꼭짓점 위치 데이터는 POSITION으로 표시되며, 법선 데이터는 NORMAL로 표시되고, 꼭짓점 색상 데이터는 COLOR로 표시됩니다. 또한 다른 셰이더 단계에는 특정 시맨틱이 필요하면 이러한 시맨틱에는 셰이더 단계에 따라 다른 해석이 있습니다. HLSL 의미 체계에 대한 자세한 내용은 [셰이더 파이프라인 포팅](change-your-shader-loading-code.md) 및 [HLSL 의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb205574)를 참조하세요.

통칭하여, 꼭짓점 및 인덱스 버퍼를 설정하고 입력 레이아웃을 설정하는 프로세스를 Direct3D 그래픽 파이프라인의 "입력 어셈블리"(IA) 단계라고 합니다.

Direct3D 11: 입력된 어셈블리 단계 구성

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

각 구성 요소에 사용된 꼭짓점 데이터 요소 및 시맨틱 형식을 선언하여 입력 레이아웃이 선언되고 꼭짓점 셰이더와 연결됩니다. 만든 D3D11\_INPUT\_ELEMENT\_DESC에 설명된 꼭짓점 요소 데이터 레이아웃은 해당 구조의 레이아웃과 일치해야 합니다. 다음과 같은 두 구성 요소가 있는 꼭짓점 데이터에 대한 레이아웃을 만듭니다.

-   (x, y, z) 좌표에 대한 3개의 32비트 부동 소수점 값의 정렬된 배열인 XMFLOAT3로 주 메모리에 표시되는 꼭짓점 위치 좌표.
-   색상(RGBA)에 대한 4개의 32비트 부동 소수점 값의 정렬된 배열인 XMFLOAT4로 표시되는 꼭짓점 색상 값.

형식 유형뿐만 아니라 각각에 대해 시맨틱을 할당합니다. 그런 다음 [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)에 설명을 전달합니다. 렌더링 메서드 중 입력 어셈블리를 설정할 때 [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)을 호출하는 경우 입력 레이아웃이 사용됩니다.

Direct3D 11: 특정 시맨틱을 사용하여 입력 레이아웃 설명

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

마지막으로 입력을 선언하여 셰이더가 입력 데이터를 이해할 수 있는지 확인합니다. 레이아웃에 할당된 시맨틱은 GPU 메모리에서 올바른 위치를 선택하는 데 사용됩니다.

Direct3D 11: HLSL 의미 체계를 사용하여 셰이더 입력 데이터 선언

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 




