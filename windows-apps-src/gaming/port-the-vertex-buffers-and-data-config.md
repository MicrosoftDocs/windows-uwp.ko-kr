---
title: 꼭 짓 점 버퍼 및 데이터를 이식 합니다.
description: 이 단계에서는 셰이더를 포함 하는 꼭 짓 점 버퍼와 셰이더가 지정 된 순서로 꼭 짓 점을 트래버스할 수 있도록 하는 인덱스 버퍼를 정의 합니다.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 포트, 꼭 짓 점 버퍼, 데이터, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 18e785fb5016e6e06225da05199df99d175afa77
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173097"
---
# <a name="port-the-vertex-buffers-and-data"></a>꼭 짓 점 버퍼 및 데이터를 이식 합니다.




**중요 API**

-   [**ID3DDevice:: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)
-   [**ID3DDeviceContext::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers)
-   [**ID3D11DeviceContext:: IASetIndexBuffer**](/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer)

이 단계에서는 셰이더를 포함 하는 꼭 짓 점 버퍼와 셰이더가 지정 된 순서로 꼭 짓 점을 트래버스할 수 있도록 하는 인덱스 버퍼를 정의 합니다.

이 시점에서 사용 중인 큐브 메시에 대해 하드 코딩 된 모델을 살펴보겠습니다. 두 표현 모두에는 삼각형 목록으로 구성 된 꼭지점이 있습니다 (줄무늬 또는 보다 효율적인 다른 삼각형 레이아웃은 아님). 두 표현의 모든 꼭 짓 점에는 연결 된 인덱스 및 색상 값이 있습니다. 이 항목의 대부분 Direct3D 코드는 Direct3D 프로젝트에 정의 된 변수 및 개체를 참조 합니다.

다음은 OpenGL ES 2.0에서 처리 하기 위한 큐브입니다. 샘플 구현에서 각 꼭 짓 점은 3 개의 위치 좌표와 4 개의 RGBA 색 값이 뒤에 오는 7 개의 float 값입니다.

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

Direct3D 11에서 처리 하는 것과 동일한 큐브입니다.

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

이 코드를 검토 하면 OpenGL ES 2.0 코드의 큐브가 오른쪽 좌표계로 표시 되 고 Direct3D 특정 코드의 큐브는 왼쪽 좌표계로 표시 됩니다. 사용자 고유의 메시 데이터를 가져올 때 모델에 대 한 z 축 좌표를 반전 하 고 각 망상의 인덱스를 적절 하 게 변경 하 여 좌표계의 변경 내용에 따라 삼각형을 트래버스 해야 합니다.

큐브 메시를 오른쪽에 있는 OpenGL ES 2.0 좌표계에서 왼손 Direct3D one으로 성공적으로 이동한 경우 두 모델에서 처리 하기 위해 큐브 데이터를 로드 하는 방법을 알아보겠습니다.

## <a name="instructions"></a>Instructions

### <a name="step-1-create-an-input-layout"></a>1단계: 입력 레이아웃 만들기

OpenGL ES 2.0에서 꼭 짓 점 데이터는 셰이더 개체가 제공 하 고 읽을 수 있는 특성으로 제공 됩니다. 일반적으로 셰이더에 사용 되는 특성 이름이 포함 된 문자열을 셰이더 프로그램 개체에 제공 하 고, 셰이더에 제공할 수 있는 메모리 위치를 가져옵니다. 이 예제에서 꼭 짓 점 버퍼 개체는 다음과 같이 정의 되 고 형식이 지정 된 사용자 지정 꼭 짓 점 구조 목록을 포함 합니다.

OpenGL ES 2.0: 꼭 짓 점 정보를 포함 하는 특성을 구성 합니다.

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

OpenGL ES 2.0에서는 입력 레이아웃이 암시적입니다. 범용 GL \_ 요소 배열 버퍼를 사용 \_ 하 \_ 고, 데이터를 업로드 한 후 꼭지점 셰이더가 데이터를 해석할 수 있도록 stride와 오프셋을 제공 합니다. **GlVertexAttribPointer**를 사용 하 여 각 꼭 짓 점 데이터 블록의 각 부분에 매핑되는 특성을 렌더링 하기 전에 셰이더에 알립니다.

Direct3D에서는 기 하 도형을 그리기 전에 버퍼를 만들 때 꼭 짓 점 버퍼에서 꼭 짓 점 데이터의 구조를 설명 하는 입력 레이아웃을 제공 해야 합니다. 이렇게 하려면 메모리의 개별 꼭 짓 점에 대 한 데이터의 레이아웃에 해당 하는 입력 레이아웃을 사용 합니다. 이를 정확 하 게 지정 하는 것이 매우 중요 합니다.

여기서 입력 설명을 [**D3D11 \_ input \_ 요소 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) 구조의 배열로 만듭니다.

Direct3D: 입력 레이아웃 설명을 정의 합니다.

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

이 입력 설명은 꼭지점을 2 3 좌표 벡터의 쌍으로 정의 합니다. 하나는 모델 좌표에서 꼭 짓 점 위치를 저장 하는 3D 벡터이 고, 다른 하나는 꼭 짓 점과 연결 된 RGB 색 값을 저장 하는 3 차원 벡터입니다. 이 경우 3x32 비트 부동 소수점 형식을 사용 합니다 .이 요소는 코드에서로 표시 `XMFLOAT3(X.Xf, X.Xf, X.Xf)` 됩니다. 데이터의 적절 한 압축 및 맞춤을 보장 하므로 셰이더에 사용할 데이터를 처리할 때마다 [Directxmath](/windows/desktop/dxmath/ovw-xnamath-reference) 라이브러리의 형식을 사용 해야 합니다. 예를 들어 벡터 데이터의 경우 [**XMFLOAT3**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat3) 또는 [**XMFLOAT4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) 를 사용 하 고 행렬에는 [**XMFLOAT4X4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) 를 사용 합니다.

가능한 모든 서식 유형 목록을 보려면 [**DXGI \_ 형식**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)을 참조 하세요.

꼭 짓 점 별 입력 레이아웃이 정의 된 상태에서 layout 개체를 만듭니다. 다음 코드에서는 [**ID3D11InputLayout**](/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout)형식의 개체를 가리키는 **ComPtr** 형식의 변수인 **m \_ inputlayout**에 코드를 작성 합니다. **Filedata** 는 이전 단계에서 [셰이더를 이식](port-the-shader-config.md)하는 컴파일된 꼭 짓 점 셰이더 개체를 포함 합니다.

Direct3D: 꼭 짓 점 버퍼에 사용 되는 입력 레이아웃을 만듭니다.

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

입력 레이아웃을 정의 했습니다. 이제이 레이아웃을 사용 하는 버퍼를 만들어 큐브 메시 데이터로 로드 해 보겠습니다.

### <a name="step-2-create-and-load-the-vertex-buffers"></a>2 단계: 꼭 짓 점 버퍼 만들기 및 로드

OpenGL ES 2.0에서는 위치 데이터와 색 데이터에 대해 하나씩, 버퍼 쌍을 만듭니다. (및 단일 버퍼를 모두 포함 하는 구조체를 만들 수도 있습니다.) 각 버퍼를 바인딩하고 위치 및 색 데이터를 씁니다. 나중에 렌더링 함수 중에 버퍼를 다시 바인딩하고이를 올바르게 해석할 수 있도록 버퍼의 데이터 형식으로 셰이더를 제공 합니다.

OpenGL ES 2.0: 꼭 짓 점 버퍼 바인딩

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

Direct3D에서 셰이더 액세스 가능 버퍼는 [**D3D11 \_ subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 구조로 표시 됩니다. 이 버퍼의 위치를 셰이더 개체에 바인딩하려면 \_ \_ [**ID3DDevice:: createbuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)를 사용 하 여 각 버퍼에 대 한 CD3D11 buffer DESC 구조를 만든 다음, 해당 버퍼 유형과 관련 된 set 메서드 (예: [**ID3DDeviceContext:: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers))를 호출 하 여 Direct3D 장치 컨텍스트의 버퍼를 설정 해야 합니다.

버퍼를 설정 하는 경우에는 버퍼의 시작 부분에서 stride (개별 꼭 짓 점에 대 한 데이터 요소의 크기) 뿐만 아니라 버텍스 데이터 배열이 실제로 시작 되는 오프셋을 설정 해야 합니다.

**VertexIndices** 배열에 대 한 포인터를 [**D3D11 \_ Subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 구조의 **pSysMem** 필드에 할당 합니다. 올바르지 않으면 메쉬가 손상 되거나 비어 있게 됩니다.

Direct3D: 꼭 짓 점 버퍼 만들기 및 설정

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
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>3 단계: 인덱스 버퍼 만들기 및 로드

인덱스 버퍼는 꼭 짓 점 셰이더가 개별 꼭 짓 점을 조회할 수 있도록 하는 효율적인 방법입니다. 반드시 필요한 것은 아니지만이 샘플 렌더러에서 사용 합니다. OpenGL ES 2.0의 꼭 짓 점 버퍼와 마찬가지로 인덱스 버퍼는 생성 되어 일반적인 용도의 버퍼로 바인딩되고 이전에 만든 꼭 짓 점 인덱스가 복사 됩니다.

그릴 준비가 되 면 꼭 짓 점 및 인덱스 버퍼를 모두 바인딩하고,이 **요소**를 호출 합니다.

OpenGL ES 2.0: 그리기 호출에 인덱스 순서를 보냅니다.

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

Direct3D를 사용 하는 경우 약간 유사한 프로세스 이며 약간 더 didactic. Direct3D를 구성할 때 만든 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 에 대 한 direct3d subresource로 인덱스 버퍼를 제공 합니다. 이렇게 하려면 다음과 같이 인덱스 배열의 구성 된 하위 리소스를 사용 하 여 [**ID3D11DeviceContext:: IASetIndexBuffer**](/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer) 를 호출 합니다. (다시, **Cubeindices** 배열에 대 한 포인터를 [**D3D11 \_ Subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 구조의 **pSysMem** 필드에 할당 하는 것을 확인할 수 있습니다.)

Direct3D: 인덱스 버퍼를 만듭니다.

``` syntax
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

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

나중에 다음과 같이 [**ID3D11DeviceContext::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (또는 인덱싱되지 않은 꼭 짓 점에 대 한 [**:D raw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) ) 호출을 사용 하 여 삼각형을 그립니다. (자세한 내용은 [화면으로 그리기](draw-to-the-screen.md)로 이동 하세요.)

Direct3D: 인덱싱된 꼭 짓 점을 그립니다.

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>이전 단계


[셰이더 개체 이식](port-the-shader-config.md)

## <a name="next-step"></a>다음 단계

[인 포트](port-the-glsl.md)

## <a name="remarks"></a>설명

Direct3D를 구성할 때 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 에서 메서드를 호출 하는 코드를 장치 리소스를 다시 만들어야 할 때마다 호출 되는 메서드로 분리 합니다. (Direct3D 프로젝트 템플릿에서이 코드는 렌더러 개체의 **Createdeviceresource** 메서드에 있습니다. 반면에 장치 컨텍스트 ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext))를 업데이트 하는 코드는 실제로 셰이더 단계를 생성 하 고 데이터를 바인딩하는 위치 이므로 **Render** 메서드에 배치 됩니다.

## <a name="related-topics"></a>관련 항목


* [방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 이식](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [셰이더 개체 이식](port-the-shader-config.md)
* [꼭 짓 점 버퍼 및 데이터를 이식 합니다.](port-the-vertex-buffers-and-data-config.md)
* [인 포트](port-the-glsl.md)

 

 