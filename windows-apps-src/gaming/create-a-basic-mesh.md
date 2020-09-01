---
title: 기본 메시 만들기 및 표시
description: UWP (3 차원 유니버설 Windows 플랫폼) 게임은 일반적으로 게임의 개체와 표면을 나타내는 다각형을 사용 합니다.
ms.assetid: bfe0ed5b-63d8-935b-a25b-378b36982b7d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 메시, directx
ms.localizationpriority: medium
ms.openlocfilehash: 360a916033a18805094336d868a09800f09c091c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173167"
---
# <a name="create-and-display-a-basic-mesh"></a>기본 메시 만들기 및 표시



UWP (3 차원 유니버설 Windows 플랫폼) 게임은 일반적으로 게임의 개체와 표면을 나타내는 다각형을 사용 합니다. 이러한 다각형 개체와 표면의 구조를 구성 하는 꼭 짓 점 목록을 메시 라고 합니다. 여기서는 큐브 개체에 대 한 기본 메시를 만들어 렌더링 및 표시를 위한 셰이더 파이프라인에 제공 합니다.

> **중요**    여기에 포함 된 예제 코드는 형식 (예: DirectX:: XMFLOAT3 및 DirectX:: XMFLOAT4X4)과 DirectXMath. h에 선언 된 인라인 메서드를 사용 합니다. 이 코드를 잘라내어 붙여 넣는 경우 \# &lt; 프로젝트에 directxmath .h를 포함 &gt; 합니다.

 

## <a name="what-you-need-to-know"></a>기억해야 하는 사항


### <a name="technologies"></a>기술

-   [Direct3D](/windows/desktop/getting-started-with-direct3d)

### <a name="prerequisites"></a>필수 구성 요소

-   선형 대 수 및 3 차원 좌표계에 대 한 기본 지식
-   Visual Studio 2015 이상 Direct3D 템플릿

## <a name="instructions"></a>Instructions

이러한 단계에서는 기본 메시 큐브를 만드는 방법을 보여 줍니다. 


이러한 개념에 대 한 자세한 설명을 선호 하는 경우이 비디오를 확인 하세요.
</br>
</br>
<iframe src="https://channel9.msdn.com/Series/Introduction-to-C-and-DirectX-Game-Development/03/player#time=7m39s:paused" width="600" height="338" allowFullScreen frameBorder="0"></iframe>


### <a name="step-1-construct-the-mesh-for-the-model"></a>1 단계: 모델에 대 한 메시 구성

대부분의 게임에서 게임 개체의 메시는 특정 꼭 짓 점 데이터를 포함 하는 파일에서 로드 됩니다. 이러한 꼭 짓 점의 순서는 앱에 따라 다르며 일반적으로 스트립 또는 팬으로 직렬화 됩니다. 꼭 짓 점 데이터는 모든 소프트웨어 원본에서 제공 하거나 수동으로 만들 수 있습니다. 꼭 짓 점 셰이더가 효과적으로 처리할 수 있는 방식으로 데이터를 해석 하는 것은 게임에 있습니다.

이 예에서는 큐브에 대해 간단한 메시를 사용 합니다. 파이프라인의이 단계에서 개체 메시와 같이 큐브는 자체 좌표계를 사용 하 여 표현 됩니다. 꼭 짓 점 셰이더는 좌표를 사용 하 고, 사용자가 제공 하는 변환 매트릭스를 적용 하 여 동일한 좌표계에서 최종 2 차원 뷰 프로젝션을 반환 합니다.

큐브에 대 한 메시를 정의 합니다. 또는 파일에서 로드 합니다. 전화 합니다.

```cpp
SimpleCubeVertex cubeVertices[] =
{
    { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
    { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 0.0f) },
    { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 1.0f) },
    { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 1.0f) },

    { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
    { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 1.0f) },
    { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f) },
    { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f) },
};
```

큐브의 좌표계는 왼쪽 좌표계를 사용 하 여 위쪽에서 아래쪽으로 실행 되는 y 축이 있는 원본에 큐브의 중심을 배치 합니다. 좌표 값은-1과 1 사이의 32 비트 부동 소수점 값으로 표현 됩니다.

각 대괄호로 묶은 쌍에서 두 번째 DirectX:: XMFLOAT3 value 그룹은 꼭 짓 점에 연결 된 색을 RGB 값으로 지정 합니다. 예를 들어 (-0.5, 0.5,-0.5)의 첫 번째 꼭 짓 점에는 전체 녹색 색 (G 값이 1.0으로 설정 되 고 "R" 및 "B" 값은 0으로 설정 됨)이 있습니다.

따라서 각각 특정 색이 있는 8 개의 꼭지점이 있습니다. 이 예에서는 각 꼭 짓 점/색 페어링이 꼭 짓 점에 대 한 전체 데이터입니다. 꼭 짓 점 버퍼를 지정 하는 경우이 특정 레이아웃을 염두에 두어야 합니다. 꼭 짓 점 데이터를 이해할 수 있도록이 입력 레이아웃을 꼭 짓 점 셰이더에 제공 합니다.

### <a name="step-2-set-up-the-input-layout"></a>2 단계: 입력 레이아웃 설정

이제 메모리에 꼭 짓 점이 있습니다. 그러나 그래픽 장치에는 자체 메모리가 있으며 Direct3D를 사용 하 여 액세스 합니다. 처리를 위해 꼭 짓 점 데이터를 그래픽 장치로 가져오려면 그래픽 장치가 게임에서 가져올 때이를 해석할 수 있도록 꼭 짓 점 데이터의 레이아웃 방법을 선언 해야 합니다. 이렇게 하려면 [**ID3D11InputLayout**](/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout)를 사용 합니다.

꼭 짓 점 버퍼의 입력 레이아웃을 선언 하 고 설정 합니다.

```cpp
const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

ComPtr<ID3D11InputLayout> inputLayout;
m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                vertexShaderBytecode->Data,
                vertexShaderBytecode->Length,
                &inputLayout)
);
```

이 코드에서 꼭 짓 점에 대 한 레이아웃을 지정 합니다. 특히 꼭 짓 점 목록의 각 요소에 포함 된 데이터를 지정 합니다. 여기서 **basicVertexLayoutDesc**는 두 가지 데이터 구성 요소를 지정 합니다.

-   **Position**: 셰이더에 제공 된 위치 데이터에 대 한 HLSL 의미 체계입니다. 이 코드에서이 코드는 DirectX:: XMFLOAT3입니다. 특히 3D 좌표 (x, y, z)에 해당 하는 3 32 비트 부동 소수점 값을 포함 하는 구조체입니다. 유형이 같은 "w" 좌표를 제공 하는 경우 float4를 사용할 수도 있습니다 .이 경우에는 DXGI \_ 형식 \_ R32G32B32A32 FLOAT를 지정 \_ 합니다. DirectX:: XMFLOAT3 또는 float4를 사용 하는 것은 게임의 특정 요구에 따라 결정 됩니다. 메시에 대 한 꼭 짓 점 데이터가 사용 하는 형식과 정확 하 게 일치 하는지 확인 하세요.

    각 좌표 값은 개체의 좌표 공간에서-1과 1 사이의 부동 소수점 값으로 표현 됩니다. 꼭 짓 점 셰이더가 완료 되 면 변환 된 꼭 짓 점이 유형이 같은 (원근 수정 됨) 뷰 프로젝션 공간에 있습니다.

    "하지만 열거형 값은 XYZ가 아니라 RGB를 나타냅니다." 현명. 좋은 시각 색 데이터 및 좌표 데이터의 경우에는 일반적으로 3 또는 4 개의 구성 요소 값을 사용 하므로 둘 다에 동일한 형식을 사용 하지 않는 이유는 무엇 인가요? 형식 이름이 아니라 HLSL 의미 체계는 셰이더가 데이터를 처리 하는 방법을 나타냅니다.

-   **Color**: 색 데이터의 HLSL 의미 체계입니다. **위치**와 마찬가지로 3 32 비트 부동 소수점 값 (DirectX:: XMFLOAT3)으로 구성 됩니다. 각 값에는 색 구성 요소인 red (r), blue (b) 또는 녹색 (g)이 포함 되어 있으며 0에서 1 사이의 부동 숫자로 표현 됩니다.

    일반적으로 **색** 값은 셰이더 파이프라인의 끝에서 4 구성 요소 RGBA 값으로 반환 됩니다. 이 예에서는 모든 픽셀에 대해 셰이더 파이프라인에서 "A" 알파 값을 1.0 (최대 불투명도)로 설정 합니다.

전체 형식 목록은 [**DXGI \_ 형식**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)을 참조 하세요. HLSL 의미 체계의 전체 목록은 [의미 체계](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)를 참조 하세요.

[**ID3D11Device:: CreateInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) 을 호출 하 고 Direct3D 장치에 입력 레이아웃을 만듭니다. 이제 데이터를 저장 하는 데 사용할 수 있는 버퍼를 만들어야 합니다.

### <a name="step-3-populate-the-vertex-buffers"></a>3 단계: 꼭 짓 점 버퍼 채우기

꼭 짓 점 버퍼에는 메시의 각 삼각형에 대 한 꼭 짓 점 목록이 포함 됩니다. 모든 꼭 짓 점은이 목록에서 고유 해야 합니다. 이 예에서는 큐브에 대해 8 개의 꼭지점이 있습니다. 꼭 짓 점 셰이더는 그래픽 장치에서 실행 되 고 꼭 짓 점 버퍼에서 읽으며, 이전 단계에서 지정한 입력 레이아웃에 따라 데이터를 해석 합니다.

다음 예제에서는 버퍼에 대 한 설명 및 하위 리소스를 제공 합니다 .이를 통해 Direct3D는 꼭 짓 점 데이터의 물리적 매핑과 그래픽 장치의 메모리에서이를 처리 하는 방법을 설명 합니다. 모든 항목을 포함할 수 있는 제네릭 [**ID3D11Buffer**](/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer)을 사용 하기 때문에이 작업이 필요 합니다. [**D3D11 \_ buffer \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) 및 [**D3D11 \_ subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 구조는 Direct3D가 버퍼의 각 꼭 짓 점 요소 크기와 꼭 짓 점 목록의 최대 크기를 포함 하 여 버퍼의 실제 메모리 레이아웃을 인식 하도록 하기 위해 제공 됩니다. 여기에서 버퍼 메모리에 대 한 액세스를 제어 하 고이를 트래버스하는 방법을 제어할 수도 있습니다 .이는이 자습서의 범위를 벗어나는 것입니다.

버퍼를 구성한 후에는 [**ID3D11Device:: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 를 호출 하 여 실제로 만듭니다. 물론 개체가 둘 이상 있는 경우 각 고유 모델에 대해 버퍼를 만듭니다.

꼭 짓 점 버퍼를 선언 하 고 만듭니다.

```cpp
D3D11_BUFFER_DESC vertexBufferDesc = {0};
vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
vertexBufferDesc.CPUAccessFlags = 0;
vertexBufferDesc.MiscFlags = 0;
vertexBufferDesc.StructureByteStride = 0;

D3D11_SUBRESOURCE_DATA vertexBufferData;
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;

ComPtr<ID3D11Buffer> vertexBuffer;
m_d3dDevice->CreateBuffer(
                &vertexBufferDesc,
                &vertexBufferData,
                &vertexBuffer);
```

꼭 짓 점이 로드 되었습니다. 그러나 이러한 꼭 짓 점 처리 순서는 어떻습니까? 이는 꼭 짓 점에 인덱스 목록을 제공할 때 처리 됩니다. 이러한 인덱스의 순서는 꼭 짓 점 셰이더가이를 처리 하는 순서입니다.

### <a name="step-4-populate-the-index-buffers"></a>4 단계: 인덱스 버퍼 채우기

이제 각 꼭 짓 점에 대 한 인덱스 목록을 제공 합니다. 이러한 인덱스는 0부터 시작 하는 꼭 짓 점 버퍼의 꼭 짓 점 위치에 해당 합니다. 이를 시각화 하는 데 도움이 되도록 메시의 고유한 각 꼭 짓 점에 ID와 같이 할당 된 고유 숫자가 있음을 고려 합니다. 이 ID는 꼭 짓 점 버퍼에서 꼭 짓 점의 정수 위치입니다.

![숫자가 매겨진 꼭지점이 8 개인 큐브](images/cube-mesh-1.png)

예제 큐브에는 면에 대해 6 개의 quads을 만드는 8 개의 꼭지점이 있습니다. 8 꼭지점을 사용 하는 총 12 개의 삼각형에 대해 quads를 삼각형으로 분할 합니다. 삼각형 당 3 개의 꼭 짓 점의 인덱스 버퍼에는 36 개의 항목이 있습니다. 이 예제에서는이 인덱스 패턴을 삼각형 목록 이라고 하며, 기본 토폴로지를 설정 하는 경우이를 Direct3D에 **D3D11 \_ 기본 \_ 토폴로지 \_ TRIANGLELIST** 로 표시 합니다.

이는 삼각형을 공유 하는 점에서 여러 개의 중복 항목이 있으므로 인덱스를 나열 하는 가장 비효율적 방법일 수 있습니다. 예를 들어, 삼각형이 rhombus 셰이프의 면을 공유 하는 경우 다음과 같이 4 개의 꼭 짓 점에 대해 6 개의 인덱스를 나열 합니다.

![rhombus를 생성할 때의 인덱스 순서](images/rhombus-surface-1.png)

-   삼각형 1: \[ 0, 1, 2\]
-   삼각형 2: \[ 0, 2, 3\]

구획 또는 팬 토폴로지에서는 트래버스하는 동안 여러 중복 면을 제거 하는 방식으로 꼭 짓 점 (예: 인덱스 0에서 이미지에 있는 인덱스 2까지)을 제거 합니다. 큰 메시의 경우 꼭 짓 점 셰이더가 실행 되는 횟수를 크게 줄이고 성능을 크게 향상 시킵니다. 그러나이를 간단 하 게 유지 하 고 삼각형 목록을 사용 합니다.

꼭 짓 점 버퍼에 대 한 인덱스를 간단한 삼각형 목록 토폴로지로 선언 합니다.

```cpp
unsigned short cubeIndices[] =
{   0, 1, 2,
    0, 2, 3,

    4, 5, 6,
    4, 6, 7,

    3, 2, 5,
    3, 5, 4,

    2, 1, 6,
    2, 6, 5,

    1, 7, 6,
    1, 0, 7,

    0, 3, 4,
    0, 4, 7 };
```

36 꼭 짓 점이 8 개만 있는 경우 버퍼의 인덱스 요소가 매우 중복 됩니다. 일부 중복 제거를 제거 하 고 스트립 또는 팬과 같은 다른 꼭 짓 점 목록 유형을 사용 하는 경우 [**ID3D11DeviceContext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) 메서드에 특정 [**D3D11 \_ 기본 \_ 토폴로지**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) 값을 제공할 때 해당 형식을 지정 해야 합니다.

다른 인덱스 목록 기술에 대 한 자세한 내용은 [기본 토폴로지](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-primitive-topologies)를 참조 하십시오.

### <a name="step-5-create-a-constant-buffer-for-your-transformation-matrices"></a>5 단계: 변환 행렬에 대 한 상수 버퍼 만들기

꼭 짓 점 처리를 시작 하기 전에 각 꼭 짓 점에 적용 되는 변환 매트릭스 (곱하기)를 제공 해야 합니다. 대부분의 3 차원 게임에는 다음 세 가지가 있습니다.

-   개체 (모델) 좌표계에서 전체 세계 좌표계로 변환 하는 4x4 행렬입니다.
-   영역 좌표계에서 카메라 (뷰) 좌표계로 변환 하는 4x4 매트릭스입니다.
-   카메라 좌표계에서 2 차원 뷰 프로젝션 좌표계로 변환 하는 4x4 매트릭스입니다.

이러한 매트릭스는 *상수 버퍼*의 셰이더에 전달 됩니다. 상수 버퍼는 셰이더 파이프라인의 다음 패스를 실행 하는 동안 일정 하 게 유지 되 고 HLSL 코드에서 셰이더가 직접 액세스할 수 있는 메모리 영역입니다. 각 상수 버퍼를 두 번 정의 합니다. 첫 번째는 게임의 c + + 코드에서, 셰이더 코드에 대 한 C와 유사한 HLSL 구문에서 (이상) 한 번입니다. 두 선언은 형식 및 데이터 정렬 측면에서 직접 일치 해야 합니다. 셰이더가 HLSL 선언을 사용 하 여 c + +로 선언 된 데이터를 해석 하 고 형식이 일치 하지 않거나 데이터 맞춤이 해제 된 경우 오류를 찾기 어려울 수 있습니다.

상수 버퍼는 HLSL에 의해 변경 되지 않습니다. 게임에서 특정 데이터를 업데이트할 때 변경할 수 있습니다. 게임 개발자는 종종 4 가지 유형의 상수 버퍼를 만듭니다. 모델/개체별 업데이트 유형 하나 게임 상태 새로 고침 당 하나의 업데이트 유형 그리고 게임 수명 동안 변경 되지 않는 데이터에 대 한 한 가지 유형이 있습니다.

이 예제에서는 세 행렬에 대 한 DirectX:: XMFLOAT4X4 데이터를 변경 하지 않는 것이 있습니다.

> **참고**    여기에 제공 된 예제 코드는 열 중심 매트릭스를 사용 합니다. HLSL의 **row \_ major** 키워드를 사용 하 여 행 중심 행렬을 대신 사용할 수 있으며, 원본 행렬 데이터도 행이 중요 합니다. DirectXMath는 행 중심 매트릭스를 사용 하며 **row \_ major** 키워드를 사용 하 여 정의 된 HLSL 매트릭스가 직접 사용할 수 있습니다.

 

각 꼭 짓 점을 변환 하는 데 사용 하는 세 행렬의 상수 버퍼를 선언 하 고 만듭니다.

```cpp
struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};
ComPtr<ID3D11Buffer> m_constantBuffer;
ConstantBuffer m_constantBufferData;

// ...

// Create a constant buffer for passing model, view, and projection matrices
// to the vertex shader.  This allows us to rotate the cube and apply
// a perspective projection to it.

D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags = 0;
constantBufferDesc.MiscFlags = 0;
constantBufferDesc.StructureByteStride = 0;
m_d3dDevice->CreateBuffer(
                &constantBufferDesc,
                nullptr,
                &m_constantBuffer
             );

m_constantBufferData.model = DirectX::XMFLOAT4X4( // Identity matrix, since you are not animating the object
            1.0f, 0.0f, 0.0f, 0.0f,
            0.0f, 1.0f, 0.0f, 0.0f,
            0.0f, 0.0f, 1.0f, 0.0f,
            0.0f, 0.0f, 0.0f, 1.0f);

);
// Specify the view (camera) transform corresponding to a camera position of
// X = 0, Y = 1, Z = 2.  

m_constantBufferData.view = DirectX::XMFLOAT4X4(
            -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
             0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
             0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
             0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f);
```

> **참고**    일반적으로 장치 특정 리소스를 설정할 때 프로젝션 매트릭스를 선언 합니다 .이를 사용 하는 곱셈 결과는 현재 2 차원 뷰포트 크기 매개 변수와 일치 해야 합니다 (보통 표시의 픽셀 높이 및 너비와 일치). 이러한 변경 사항이 있으면 x 좌표 및 y 좌표 값을 적절 하 게 조정 해야 합니다.

 

```cpp
// Finally, update the constant buffer perspective projection parameters
// to account for the size of the application window.  In this sample,
// the parameters are fixed to a 70-degree field of view, with a depth
// range of 0.01 to 100.  

float xScale = 1.42814801f;
float yScale = 1.42814801f;
if (backBufferDesc.Width > backBufferDesc.Height)
{
    xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
}
else
{
    yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
}
m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

여기에서[ID3D11DeviceContext](/windows/desktop/direct3d11/d3d11-graphics-reference-10level9-context)의 꼭 짓 점 및 인덱스 버퍼와 사용 중인 토폴로지를 설정 합니다.

```cpp
// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(SimpleCubeVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset);

m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0);

 m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
```

좋습니다! 입력 어셈블리를 완료 했습니다. 렌더링을 위한 모든 것이 준비 되었습니다. 꼭 짓 점 셰이더를 살펴보겠습니다.

### <a name="step-6-process-the-mesh-with-the-vertex-shader"></a>6 단계: 꼭 짓 점 셰이더를 사용 하 여 메시 처리

이제 망상을 정의 하는 꼭 짓 점 버퍼와 꼭 짓 점이 처리 되는 순서를 정의 하는 인덱스 버퍼를 만들었으므로 버텍스 셰이더에 보냅니다. 컴파일된 상위 수준 셰이더 언어로 표현 되는 꼭 짓 점 셰이더 코드는 꼭 짓 점 버퍼의 각 꼭 짓 점에 대해 한 번씩 실행 되므로 꼭 짓 점 별 변환을 수행할 수 있습니다. 최종 결과는 일반적으로 2 차원 프로젝션입니다.

꼭 짓 점 셰이더를 로드 하셨습니까? 그렇지 않은 경우 [DirectX 게임에서 리소스를 로드 하는 방법](load-a-game-asset.md)을 검토 하세요.)

여기에서 꼭 짓 점 셰이더를 만듭니다.

``` syntax
// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0);
```

... 상수 버퍼를 설정 합니다.

``` syntax
m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf());
```

다음은 개체 좌표에서 세계 좌표로 변환 하 고 그 다음에 2 차원 뷰 프로젝션 좌표계를 처리 하는 꼭 짓 점 셰이더 코드입니다. 또한 몇 가지 간단한 꼭 짓 점 조명을 적용 하 여 작업을 쉽게 수행할 수 있습니다. 이는 꼭 짓 점 셰이더의 HLSL 파일 (이 예제에서는 HLSL)로 이동 합니다.

``` syntax
cbuffer simpleConstantBuffer : register( b0 )
{
    matrix model;
    matrix view;
    matrix projection;
};

struct VertexShaderInput
{
    DirectX::XMFLOAT3 pos : POSITION;
    DirectX::XMFLOAT3 color : COLOR;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
    float4 color : COLOR;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projection space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    vertexShaderOutput.pos = pos;

    // Pass the vertex color through to the pixel shader.
    vertexShaderOutput.color = float4(input.color, 1.0f);

    return vertexShaderOutput;
}
```

위쪽의 **cbuffer** 를 확인 하세요. 이전에 c + + 코드에서 선언한 동일한 상수 버퍼에 대 한 HLSL 아날로그입니다. **VertexShaderInputstruct**? 그 이유는 입력 레이아웃 및 꼭 짓 점 데이터 선언 처럼 보입니다. C + + 코드의 상수 버퍼 및 꼭 짓 점 데이터 선언이 HLSL 코드의 선언과 일치 하 고 기호, 형식 및 데이터 맞춤을 포함 하는 것이 중요 합니다.

**PixelShaderInput** 는 꼭 짓 점 셰이더의 main 함수에서 반환 되는 데이터의 레이아웃을 지정 합니다. 꼭 짓 점 처리를 마치면 2 차원 프로젝션 공간에서 꼭 짓 점 위치를 반환 하 고 꼭 짓 점 별 조명에 사용 되는 색을 반환 합니다. 그래픽 카드는 셰이더에 있는 데이터 출력을 사용 하 여 파이프라인의 다음 단계에서 픽셀 셰이더가 실행 될 때 색을 지정 해야 하는 "조각" (가능한 픽셀)을 계산 합니다.

### <a name="step-7-passing-the-mesh-through-the-pixel-shader"></a>7 단계: 픽셀 셰이더를 통해 메시 전달

일반적으로 그래픽 파이프라인의이 단계에서는 개체의 보이는 예상 표면에서 픽셀 단위 작업을 수행 합니다. (질감 등의 사람) 그러나 샘플의 목적을 위해이 단계를 통해 전달 하기만 하면 됩니다.

먼저 픽셀 셰이더의 인스턴스를 만들어 보겠습니다. 픽셀 셰이더는 장면의 2 차원 프로젝션에서 모든 픽셀에 대해 실행 되어 해당 픽셀에 색을 할당 합니다. 이 경우 꼭 짓 점 셰이더에 의해 반환 되는 픽셀의 색을 똑바로 전달 합니다.

픽셀 셰이더를 설정 합니다.

``` syntax
m_d3dDeviceContext->PSSetShader( pixelShader.Get(), nullptr, 0 );
```

HLSL에서 통과 픽셀 셰이더를 정의 합니다.

``` syntax
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

이 코드는 꼭 짓 점 셰이더 HLSL (예: SimplePixelShader)와 별개인 HLSL 파일에 배치 합니다. 이 코드는 뷰포트에 표시 되는 모든 픽셀에 대해 한 번 실행 됩니다 (이 경우에는 그리기 중인 화면 부분의 메모리 내 표현) .이 경우 전체 화면에 매핑됩니다. 이제 그래픽 파이프라인이 완전히 정의 되었습니다.

### <a name="step-8-rasterizing-and-displaying-the-mesh"></a>8 단계: 메시 래스터화 및 표시

파이프라인을 실행 해 보겠습니다. [**ID3D11DeviceContext::D rawindexed**](/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexed)를 호출 하는 것이 쉽습니다.

해당 큐브를 그립니다.

```cpp
// Draw the cube.
m_d3dDeviceContext->DrawIndexed( ARRAYSIZE(cubeIndices), 0, 0 );
            
```

그래픽 카드 내에서 각 꼭 짓 점은 인덱스 버퍼에 지정 된 순서 대로 처리 됩니다. 코드가 꼭짓점 셰이더를 실행하고 2-D 프래그먼트가 정의되고 나면 픽셀 셰이더가 호출되고 삼각형에 색이 입혀집니다.

이제 큐브를 화면에 배치 합니다.

표시에 프레임 버퍼를 표시 합니다.

```cpp
// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop is generally  throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the app on Present until the screen is refreshed.

m_swapChain->Present(1, 0);
```

완료되었습니다! 모델의 전체 장면에 대해 여러 꼭 짓 점 및 인덱스 버퍼를 사용 하 고 모델 유형에 따라 다른 셰이더가 있을 수 있습니다. 각 모델에는 자체 좌표계가 있으며 상수 버퍼에서 정의한 행렬을 사용 하 여 공유 세계 좌표계로 변환 해야 합니다.

## <a name="remarks"></a>설명

이 항목에서는 사용자가 직접 만든 간단한 기 하 도형을 만들고 표시 하는 방법을 설명 합니다. 파일에서 더 복잡 한 기 하 도형을 로드 하 고이를 샘플 별 꼭 짓 점 버퍼 개체 (vbo) 형식으로 변환 하는 방법에 대 한 자세한 내용은 [DirectX 게임에서 리소스를 로드 하는 방법](load-a-game-asset.md)을 참조 하세요.  

 

## <a name="related-topics"></a>관련 항목


* [DirectX 게임에서 리소스를 로드 하는 방법](load-a-game-asset.md)

 

 