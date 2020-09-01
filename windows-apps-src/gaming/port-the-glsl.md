---
title: 인 포트
description: 버퍼 및 셰이더 개체를 만들고 구성 하는 코드를 이동한 후에는 OpenGL ES 2.0의 HLSL (High level Shader language)에서 해당 셰이더 안에 있는 코드를 해당 셰이더로 이식 해야 합니다.
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 글 sl, 포트
ms.localizationpriority: medium
ms.openlocfilehash: b9f3d3a8bc6bfe9205ccba9d743409519bf8a572
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173127"
---
# <a name="port-the-glsl"></a>인 포트




**중요 API**

-   [HLSL 의미 체계](/windows/desktop/direct3dhlsl/dcl-usage---ps)
-   [셰이더 상수 (HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)

버퍼 및 셰이더 개체를 만들고 구성 하는 코드를 이동한 후에는 OpenGL ES 2.0의 HLSL (High level Shader language)에서 해당 셰이더 안에 있는 코드를 해당 셰이더로 이식 해야 합니다.

OpenGL ES 2.0에서 셰이더는 **gl \_ 위치**, **gl \_ FragColor**또는 **gl \_ FragData \[ n \] ** 과 같은 내장 함수를 사용 하 여 실행 후 데이터를 반환 합니다. 여기서 n은 특정 렌더링 대상의 인덱스입니다. Direct3D에서는 특정 내장 함수를 사용할 수 없으며 셰이더는 해당 main () 함수의 반환 형식으로 데이터를 반환 합니다.

꼭 짓 점 위치 또는 일반 등 셰이더 단계 사이에 보간된 데이터는 **다양** 한 선언을 사용 하 여 처리 됩니다. 그러나 Direct3D에는이 선언이 없습니다. 대신 셰이더 단계 간에 전달 하려는 모든 데이터를 [HLSL 의미 체계](/windows/desktop/direct3dhlsl/dcl-usage---ps)로 표시 해야 합니다. 선택한 특정 의미 체계는 데이터의 용도를 나타내며은입니다. 예를 들어 조각 셰이더 사이에 보간된 꼭 짓 점 데이터를 다음과 같이 선언 합니다.

`float4 vertPos : POSITION;`

또는

`float4 vertColor : COLOR;`

여기서 POSITION은 꼭 짓 점 위치 데이터를 나타내는 데 사용 되는 의미입니다. 또한 위치는 보간 후에 픽셀 셰이더에 액세스할 수 없기 때문에 특수 한 경우입니다. 따라서 SV 위치를 사용 하 여 픽셀 셰이더에 대 한 입력을 지정 해야 \_ 하며 보간된 꼭 짓 점 데이터가 해당 변수에 배치 됩니다.

`float4 position : SV_POSITION;`

의미 체계는 셰이더의 본문 (main) 메서드에서 선언할 수 있습니다. 픽셀 셰이더의 경우 \_ \[ \] 본문 메서드에 렌더링 대상을 나타내는 SV 대상 n이 필요 합니다. (SV \_ 숫자 접미사가 없는 대상은 기본값 렌더링 대상 인덱스 0입니다.

또한, 꼭 짓 점 셰이더는 SV \_ POSITION 시스템 값 의미 체계를 출력 하는 데 필요 합니다. 이 의미 체계는 x가-1과 1 사이에 있는 값을 조정 하기 위해 꼭 짓 점 위치 데이터를 확인 하 고, y는-1과 1 사이, z는 원래의 유형이 같은 좌표 w 값 (z/w)으로 나눈 후 w는 1을 원래 w 값 (1/w)으로 나눕니다. 픽셀 셰이더는 SV \_ POSITION 시스템 값 의미 체계를 사용 하 여 화면에서 픽셀 위치를 검색 합니다. 여기서 x는 0 사이이 고 렌더링 대상 너비는 0이 고 y는 렌더링 대상 높이 (0.5)입니다. 기능 수준 9 \_ x 픽셀 셰이더는 SV 위치 값에서 읽을 수 없습니다 \_ .

상수 버퍼는 **cbuffer** 를 사용 하 여 선언 되 고 조회를 위해 특정 시작 레지스터와 연결 되어야 합니다.

Direct3D 11: HLSL 상수 버퍼 선언

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

여기서 상수 버퍼는 압축 된 버퍼를 보관 하기 위해 등록 b0을 사용 합니다. 모든 레지스터는 b 형식에서 참조 됩니다 \# . 상수 버퍼, 레지스터 및 데이터 압축의 HLSL 구현에 대 한 자세한 내용은 [셰이더 상수 (HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)를 참조 하세요.

<a name="instructions"></a>Instructions
------------
### <a name="step-1-port-the-vertex-shader"></a>1 단계: 꼭 짓 점 셰이더 이식

간단한 OpenGL ES 2.0 예제에서 꼭 짓 점 셰이더는 상수 모델-뷰-프로젝션 4x4 매트릭스와 4 좌표 벡터의 세 가지 입력을 포함 합니다. 이러한 두 벡터에는 꼭 짓 점 위치와 색이 포함 됩니다. 셰이더는 위치 벡터를 큐브 뷰 좌표로 변환 하 고 \_ 래스터화의 gl 위치 내장 함수에 할당 합니다. 꼭 짓 점 색은 래스터화 중에 보간에 대해 다양 한 변수에 복사 됩니다.

OpenGL ES 2.0: 큐브 개체에 대 한 꼭 짓 점 셰이더 (글 안)

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

이제 Direct3D에서 상수 model-view-projection 행렬이 레지스터 b0에서 압축된 상수 버퍼에 포함되어 있으며, 꼭짓점 위치 및 색상은 적절한 해당 HLSL 의미 체계 POSITION 및 COLOR로 구체적으로 표시됩니다. 입력 레이아웃은 이러한 두 꼭 짓 점 값의 특정 정렬을 나타내므로이를 포함 하는 구조체를 만들고 셰이더 본문 함수 (main)의 입력 매개 변수에 대 한 형식으로 선언 합니다. (두 개의 개별 매개 변수로 지정할 수도 있지만이 경우 복잡할 수 있습니다.) 또한 보간된 위치 및 색을 포함 하는이 단계에 대 한 출력 형식을 지정 하 고 꼭 짓 점 셰이더의 본문 함수의 반환 값으로 선언 합니다.

Direct3D 11: 큐브 개체에 대 한 꼭 짓 점 셰이더 (HLSL)

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

출력 데이터 형식 PixelShaderInput는 래스터화 중에 채워지고 조각 (픽셀) 셰이더에 제공 됩니다.

### <a name="step-2-port-the-fragment-shader"></a>2 단계: 조각 셰이더 이식

이 글의 예제 조각 셰이더는 매우 간단 \_ 합니다. 보간된 색 값으로 gl FragColor 내장 함수를 제공 합니다. OpenGL ES 2.0는이를 기본 렌더링 대상에 기록 합니다.

OpenGL ES 2.0: 큐브 개체에 대 한 조각 셰이더 (글 안)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Direct3D는 거의 단순 합니다. 유일한 차이점은 픽셀 셰이더의 본문 함수가 값을 반환 해야 한다는 것입니다. 색이 RGBA (4 좌표) 부동 소수점 값 이므로 반환 형식으로 float4을 표시 한 다음 기본 렌더링 대상을 SV \_ 대상 시스템 값 의미 체계로 지정 합니다.

Direct3D 11: cube 개체의 픽셀 셰이더 (HLSL)

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

위치의 픽셀 색은 렌더링 대상에 기록 됩니다. 이제 [화면에 그릴](draw-to-the-screen.md)때 렌더링 대상의 콘텐츠를 표시 하는 방법을 알아보겠습니다.

## <a name="previous-step"></a>이전 단계


[꼭 짓 점 버퍼 및 데이터를 이식 합니다](port-the-vertex-buffers-and-data-config.md) . 다음 단계
---------
[화면에 그리기](draw-to-the-screen.md) 설명
-------
HLSL 의미 체계를 이해 하 고 상수 버퍼를 압축 하면 약간의 디버깅 골칫거리을 제공할 수 있을 뿐 아니라 최적화 기회를 제공할 수 있습니다. [HLSL (변수 구문)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-syntax), [Direct3D 11의 버퍼 소개](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)및 [방법: 상수 버퍼 만들기](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to)를 참조 하세요. 그러나 그렇지 않은 경우에는 의미 체계 및 상수 버퍼를 염두에 두어야 하는 몇 가지 시작 팁이 있습니다.

-   항상 렌더러의 Direct3D 구성 코드를 다시 확인 하 여 상수 버퍼의 구조가 HLSL의 cbuffer 구조체 선언과 일치 하는지, 그리고 구성 요소 스칼라 형식이 두 선언에서 일치 하는지 확인 합니다.
-   렌더러의 c + + 코드에서 상수 버퍼 선언에 [Directxmath](/windows/desktop/dxmath/directxmath-portal) 형식을 사용 하 여 적절 한 데이터 압축을 보장 합니다.
-   상수 버퍼를 효율적으로 사용 하는 가장 좋은 방법은 업데이트 빈도에 따라 셰이더 변수를 상수 버퍼로 구성 하는 것입니다. 예를 들어 프레임당 한 번 업데이트 되는 균일 한 데이터와 카메라를 이동할 때만 업데이트 되는 기타 균일 한 데이터를 보유 하는 경우 해당 데이터를 별도의 두 상수 버퍼로 분리 하는 것이 좋습니다.
-   사용자가 적용 하거나 잘못 적용 한 의미 체계가 가장 오래 된 셰이더 컴파일 (FXC.EXE) 오류의 원인이 됩니다. 두 번 확인 합니다. 이전 페이지와 샘플은 Direct3D 11 이전의 HLSL 의미 체계를 서로 다른 버전으로 참조 하기 때문에 문서는 약간 혼란 스 러 울 수 있습니다.
-   각 셰이더를 대상으로 하는 Direct3D 기능 수준을 확인 해야 합니다. 기능 수준 9의 의미 체계는 \_ \* 11 1의 의미 체계와 다릅니다 \_ .
-   SV \_ 위치 의미 체계는 연결 된 보간 후 위치 데이터를 좌표 값으로 확인 합니다. 여기서 x는 0이 고 렌더링 대상 너비는 0이 고, y는 0과 렌더링 대상 높이 사이의 좌표 이며, z는 원래의 유형이 같은 좌표 w 값 (z/w)으로 나눈 후 w는 1을 원래 w 값 (1/w)으로 나눈 값입니다.

## <a name="related-topics"></a>관련 항목


[방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 이식](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[셰이더 개체 이식](port-the-shader-config.md)

[꼭 짓 점 버퍼 및 데이터를 이식 합니다.](port-the-vertex-buffers-and-data-config.md)

[화면에 그리기](draw-to-the-screen.md)

 

 