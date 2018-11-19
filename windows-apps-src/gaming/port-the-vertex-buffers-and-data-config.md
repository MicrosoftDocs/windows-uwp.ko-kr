---
author: mtoepke
title: 꼭짓점 버퍼 및 데이터 포팅
description: 이 단계에서는 메시를 포함하는 꼭짓점 버퍼 및 셰이더가 꼭짓점을 지정된 순서로 트래버스할 수 있게 하는 인덱스 버퍼를 정의합니다.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 포트, 꼭짓점 버퍼, 데이터, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: b32747a4e11d258f71d4e55e41b7f54bb5e99246
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7289340"
---
# <a name="port-the-vertex-buffers-and-data"></a>꼭짓점 버퍼 및 데이터 포팅




**중요 API**

-   [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)

이 단계에서는 메시를 포함하는 꼭짓점 버퍼 및 셰이더가 꼭짓점을 지정된 순서로 트래버스할 수 있게 하는 인덱스 버퍼를 정의합니다.

이쯤에서, 사용할 큐브 메시에 대한 하드 코드된 모델을 살펴보겠습니다. 두 가지 표현 모두에는 삼각형 목록으로 구성된 꼭짓점이 있습니다(스트립 또는 다른 더 효율적인 삼각형 레이아웃과 반대). 또한 두 가지 표현의 모든 꼭짓점에는 인덱스 및 색상 값이 연결되어 있습니다. 이 항목의 많은 Direct3D 코드는 Direct3D 프로젝트에 정의된 변수 및 개체를 참조합니다.

OpenGL ES 2.0에서 처리할 큐브는 다음과 같습니다. 샘플 구현에서 각 꼭짓점은 7개의 float 값입니다. 3개의 위치 좌표 다음에 4개의 RGBA 색상 값이 나옵니다.

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

그리고 Direct3D 11에서 처리할 동일한 큐브는 다음과 같습니다.

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

이 코드를 검토하면, OpenGL ES 2.0 코드의 큐브는 오른쪽 좌표계에서 나타나고 Direct3D 관련 코드의 큐브는 왼쪽 좌표계에 표시되는 것을 알 수 있습니다. 고유한 메시 데이터를 가져올 때, 해당 모델에 대한 z 축 좌표를 반전하고 좌표계의 변경에 따라 삼각형을 트래버스하도록 각 메시에 대한 인덱스를 적절하게 변경해야 합니다.

오른손 OpenGL ES 2.0 좌표계에서 왼손 Direct3D 좌표계로 큐브 메시를 이동한 것으로 가정하고 두 모델에서 처리할 큐브 데이터를 로드하는 방법을 알아보겠습니다.

## <a name="instructions"></a>지침

### <a name="step-1-create-an-input-layout"></a>1단계: 입력 레이아웃 만들기

OpenGL ES 2.0에서는 셰이더 개체에 제공하고 이 개체에서 읽는 특성으로 꼭짓점 데이터가 제공됩니다. 일반적으로 셰이더 프로그램 개체에 대한 셰이더의 GLSL 사용되는 특성 이름이 포함된 문자열을 제공하고 셰이더에 제공할 수 있는 메모리 위치를 다시 가져옵니다. 이 예제에서 꼭짓점 버퍼 개체는 다음과 같이 정의되고 서식이 지정된 사용자 지정 꼭짓점 구조 목록을 포함합니다.

OpenGL ES 2.0: 꼭짓점별 정보가 포함된 특성을 구성합니다.

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

OpenGL ES 2.0에서는 입력 레이아웃은 암시적입니다. 범용 GL\_ELEMENT\_ARRAY\_BUFFER를 가져오고 꼭짓점 셰이더가 데이터를 업로드 한 후 이 데이터를 해석할 수 있도록 stride 및 오프셋 제공합니다. **glVertexAttribPointer**를 사용하여 꼭짓점 데이터의 각 블록에서 어느 부분에 어떤 특성이 매핑되는지 렌더링하기 전에 셰이더에 알려줍니다.

Direct3D에서 기하 도형을 그리기 전에 대신 버퍼를 만들 때 꼭짓점 버퍼에 있는 꼭짓점 데이터의 구조를 설명하기 위해 입력 레이아웃을 제공해야 합니다. 이렇게 하려면 메모리에서 개별 꼭짓점에 대한 데이터의 레이아웃에 해당하는 입력 레이아웃을 사용합니다. 이를 정확하게 지정하는 것이 매우 중요합니다!

여기에서 입력 설명을 [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 구조의 배열로 작성합니다.

Direct3D: 입력 레이아웃 설명을 정의합니다.

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

이 입력 설명은 2개의 3 좌표 벡터 쌍으로 꼭짓점을 정의합니다. 하나는 모델 좌표에서 꼭짓점의 위치를 저장할 3D 벡터이고 다른 하나는 꼭짓점과 연결된 RGB 색상 값을 저장할 3D 벡터입니다. 이 경우 3x32비트 부동 소수점 형식을 사용합니다. 이 형식의 요소는 코드에서 `XMFLOAT3(X.Xf, X.Xf, X.Xf)`으로 표시됩니다. 셰이더에서 사용할 데이터를 처리할 때마다 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415574) 라이브러리의 형식을 사용해야 합니다. 그러면 해당 데이터가 적절하게 압축되고 정렬됩니다. 예를 들어 벡터 데이터에는 [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) 또는 [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)를 사용하고 행렬에는 [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621)를 사용합니다.

가능한 모든 형식 유형 목록은 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)을 참조하세요.

정의된 꼭짓점별 입력 레이아웃을 사용하여 레이아웃 개체를 만듭니다. 다음 코드에서 이 개체를 유형 **ComPtr**(유형 [**D3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575)의 개체를 가리킴)의 변수인 **m\_inputLayout**에 씁니다. **fileData**에는 이전 단계 [셰이더 포팅](port-the-shader-config.md)의 컴파일된 꼭짓점 셰이더 개체가 포함되어 있습니다.

Direct3D: 꼭짓점 버퍼에서 사용하는 입력 레이아웃을 만듭니다.

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

입력 레이아웃을 정의 했습니다. 이제 이 레이아웃을 사용하는 버퍼를 만들고 큐브 메시 데이터를 사용하여 이 버퍼를 로드합니다.

### <a name="step-2-create-and-load-the-vertex-buffers"></a>2단계: 꼭짓점 버퍼 만들기 및 로드

OpenGL ES 2.0에서는 위치 데이터의 버퍼 하나와 색상 데이터의 버퍼 하나로 버퍼 쌍을 만듭니다. 둘 다 포함하고 단일 버퍼를 포함하는 구조체를 만들 수도 있습니다. 각 버퍼를 바인딩하고 위치 및 색상 데이터를 여기에 작성합니다. 나중에 렌더 함수 중 버퍼를 다시 바인딩하고 제대로 해석할 수 있도록 버퍼에 있는 데이터의 형식으로 셰이더를 제공합니다.

OpenGL ES 2.0: 꼭짓점 버퍼 바인딩

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

Direct3D에서 셰이더 액세스 가능한 버퍼는 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 구조로 표시됩니다. 셰이더 개체에 이 버퍼의 위치를 바인딩하려면 각 버퍼에 대한 CD3D11\_BUFFER\_DESC 구조를 [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)를 사용하여 만든 다음 [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 등 버퍼 유형 관련 set 메서드를 호출하여 Direct3D 디바이스 컨텍스트의 버퍼를 설정합니다.

버퍼를 설정하는 경우 버퍼의 시작 부분의 오프셋(여기서 실제로 꼭짓점 데이터 배열이 시작함)뿐만 아니라 stride(개별 꼭짓점에 대한 데이터 요소 크기)를 설정해야 합니다.

[**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 구조의 **pSysMem** 필드에 대한 **vertexIndices** 배열에 포인터를 할당합니다. 올바르지 않으면 메시가 손상되거나 비게 됩니다!

Direct3D: 꼭짓점 버퍼 만들기 및 설정

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

### <a name="step-3-create-and-load-the-index-buffer"></a>3단계: 인덱스 버퍼를 만들기 및 로드

인덱스 버퍼는 꼭짓점 셰이더가 개별 꼭짓점을 조회할 수 있게 하는 효율적인 방법입니다. 필수는 아니지만 이 샘플 렌더러에서는 이 버퍼를 사용합니다. OpenGL ES 2.0의 꼭짓점 버퍼에서와 같이, 인덱스 버퍼는 범용 버퍼로 만들어지고 바인딩되며 이전에 만든 꼭짓점 인덱스가 여기에 복사됩니다.

그릴 준비가 되면, 꼭짓점 및 인덱스 버퍼를 모두 다시 바인딩하고 **glDrawElements**를 호출합니다.

OpenGL ES 2.0: 인덱스 순서를 그리기 호출로 보냅니다.

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

약간 더 훈시적이긴 하지만 Direct3D에서 매우 비슷한 프로세스입니다. Direct3D 하위 리소스로 인덱스 버퍼를 Direct3D를 구성할 때 만든 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)에 제공합니다. 다음과 같이 인덱스 배열에 대한 구성된 하위 리소스를 사용하여 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)를 호출하여 이 작업을 수행합니다. 다시, [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 구조의 **pSysMem** 필드에 대한 **cubeIndices** 배열에 포인터를 할당합니다.

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

나중에, 다음과 같이 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409)(또는 인덱싱되지 않은 꼭짓점에 대한 [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407))에 대한 호출을 사용하여 삼각형을 그립니다. (자세한 내용은 [화면에 그리기](draw-to-the-screen.md)를 참조하세요.)

Direct3D: 인덱싱된 꼭짓점을 그립니다.

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


[셰이더 개체 포팅](port-the-shader-config.md)

## <a name="next-step"></a>다음 단계

[GLSL 포팅](port-the-glsl.md)

## <a name="remarks"></a>설명

Direct3D를 구성할 때 디바이스 리소스를 다시 만들어야 할 때마다 호출하는 메서드로 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)에 대한 메서드를 호출하는 코드를 구분합니다. (Direct3D 프로젝트 템플릿에서 이 코드는 렌더러 개체의 **CreateDeviceResource** 메서드에 있습니다.) 반면에 이는 실제로 셰이더 단계를 구성하고 데이터를 바인딩하는 위치이기 때문에 디바이스 컨텍스트([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385))를 업데이트하는 코드가 **Render** 메서드에 배치됩니다. .

## <a name="related-topics"></a>관련 항목


* [방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 포팅](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [셰이더 개체 포팅](port-the-shader-config.md)
* [꼭짓점 버퍼 및 데이터 포팅](port-the-vertex-buffers-and-data-config.md)
* [GLSL 포팅](port-the-glsl.md)

 

 




