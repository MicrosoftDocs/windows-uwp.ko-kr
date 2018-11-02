---
author: mtoepke
title: GLSL 포팅
description: 버퍼 및 셰이더 개체를 만들고 구성하는 코드를 이동한 후에는 해당 셰이더 내에서 코드를 OpenGL ES 2.0 GLSL(GL Shader Language)에서 Direct3D 11 HLSL(High Level Shader Language)로 포팅할 차례입니다.
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, glsl, 포트
ms.localizationpriority: medium
ms.openlocfilehash: 47fa601a7e0ff307108713a0a6fcd7a5468b0468
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5925735"
---
# <a name="port-the-glsl"></a>GLSL 포팅




**중요 API**

-   [HLSL 의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb205574)
-   [셰이더 상수(HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)

버퍼 및 셰이더 개체를 만들고 구성하는 코드를 이동한 후에는 해당 셰이더 내에서 코드를 OpenGL ES 2.0 GLSL(GL Shader Language)에서 Direct3D 11 HLSL(High Level Shader Language)로 포팅할 차례입니다.

OpenGL ES 2.0에서 셰이더는 **gl\_Position**, **gl\_FragColor** 또는 **gl\_FragData\[n\]**(여기서 n은 특정 렌더링 대상의 인덱스)와 같은 내장 함수를 사용하여 실행 후 데이터를 반환합니다. Direct3D에서는 특정 내장 함수가 없으며 셰이더는 각 main() 함수의 반환 형식으로 데이터를 반환합니다.

꼭짓점 위치 또는 일반 등 셰이더 단계 간에 보간하려는 데이터는 **varying** 선언 사용을 통해 처리됩니다. 그러나, Direct3D는 이 선언을 사용할 수 없습니다. 셰이더 단계 간에 전달하려는 모든 데이터는 [HLSL 의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb205574)로 표시해야 합니다. 선택한 특정 의미 체계는 데이터의 용도를 나타내며 다음과 같습니다. 예를 들어, 조각 셰이더 간의 보간하려는 꼭짓점 데이터를 다음과 같이 선언합니다.

`float4 vertPos : POSITION;`

또는

`float4 vertColor : COLOR;`

여기서 POSITION은 꼭짓점 위치 데이터를 나타내는 데 사용되는 의미 체계입니다. 또한 보간 후, 픽셀 셰이더로 액세스할 수 없으므로 POSITION은 특별한 경우입니다. 따라서 픽셀 셰이더에 SV\_POSITION으로 입력을 지정해야 하고 보간된 꼭짓점 데이터가 해당 변수에 배치됩니다.

`float4 position : SV_POSITION;`

의미 체계는 셰이더의 본문(기본) 메서드에서 선언할 수 있습니다. 픽셀 셰이더의 경우 렌더링 대상을 나타내는 SV\_TARGET\[n\]이 본문 메서드에 필요합니다. SV\_TARGET은 숫자 접미사 없이 렌더링 대상 인덱스 0으로 기본적으로 지정됩니다.

또한 꼭짓점 셰이더가 SV\_POSITION 시스템 값 의미 체계를 출력하는 데 필요합니다. 이 의미 체계는 꼭짓점 위치 데이터를 결정하여 값을 조정합니다. 여기서 x는 -1과 1 사이입니다. y는 -1과 1 사이입니다. z는 원래 같은 유형의 좌표 w 값으로 나눕니다(z/w). w는 원래 w 값으로 1을 나눈 값입니다(1/w). 픽셀 셰이더는 SV\_POSITION 시스템 값 의미 체계를 사용하여 화면에서 픽셀 위치를 검색합니다. 여기서 x는 0과 렌더링 대상 너비 사이이고 y는 0과 렌더링 대상 높이 사이입니다(각각 0.5 기준 오프셋). 기능 수준 9\_x 픽셀 셰이더는 SV\_POSITION 값을 읽을 수 없습니다.

상수 버퍼는 **cbuffer**로 선언해야 하고 조회를 위해 특정 시작 레지스터와 연결되어야 합니다.

Direct3D 11: HLSL 상수 버퍼 선언

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

여기서 상수 버퍼는 레지스터 b0을 사용하여 압축된 버퍼를 유지합니다. 모든 레지스터는 b\# 형식으로 참조됩니다. 상수 버퍼, 레지스터, 데이터 압축의 HLSL 구현에 대한 자세한 내용은 [셰이더 상수(HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)를 참조하세요.

<a name="instructions"></a>지침
------------

### <a name="step-1-port-the-vertex-shader"></a>1단계: 꼭짓점 셰이더 포팅

간단한 OpenGL ES 2.0 예제에서는 꼭짓점 셰이더에는 상수 model-view-projection 4x4 행렬 및 두 개의 4좌표 벡터라는 세 개의 입력이 있습니다. 이러한 두 벡터는 꼭짓점 위치 및 해당 색상을 포함합니다. 셰이더는 위치 벡터를 원근 좌표로 변환하고 래스터화를 위해 이를 gl\_Position 내장 항목에 할당합니다. 또한 꼭짓점 색상은 래스터화 중에 보간을 위해 다양한 변수에 복사됩니다.

OpenGL ES 2.0: 큐브 개체에 대한 꼭짓점 셰이더(GLSL)

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

이제 Direct3D에서 상수 model-view-projection 행렬이 레지스터 b0에서 압축된 상수 버퍼에 포함되어 있으며, 꼭짓점 위치 및 색상은 적절한 해당 HLSL 의미 체계 POSITION 및 COLOR로 구체적으로 표시됩니다. 입력 레이아웃은 이러한 두 꼭짓점 값의 특정 배열을 나타내므로, 이러한 값을 유지하고 셰이더 본문 함수(기본)에서 입력 매개 변수의 형식으로 선언하기 위한 구조체를 만듭니다. 두 개의 개별 매개 변수로 지정할 수도 있지만 번거로울 수 있습니다. 또한 보간된 위치 및 색상을 포함하는 이 단계에 대한 출력 유형을 지정하 고 꼭짓점 셰이더의 본문 함수에 대한 반환 값으로 선언합니다.

Direct3D 11: 큐브 개체에 대한 꼭짓점 셰이더(HLSL)

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

출력 데이터 형식 PixelShaderInput은 래스터화하는 동안 채워지며 조각(픽셀) 셰이더에 제공됩니다.

### <a name="step-2-port-the-fragment-shader"></a>2단계: 조각 셰이더 포팅

GLSL의 예제 조각 셰이더는 매우 간단합니다. 보간된 색상 값과 함께 gl\_FragColor 내장 항목을 제공합니다. OpenGL ES 2.0은 기본 렌더링 대상에 이를 작성합니다.

OpenGL ES 2.0: 큐브 개체에 대한 조각 셰이더(GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Direct3D도 매우 간단합니다. 유일한 중요 차이점은 픽셀 셰이더의 본문 함수는 값을 반환해야 한다는 것입니다. 색상은 4좌표(RGBA) float 값이므로 float4를 반환 형식으로 나타내고 기본 렌더링 대상을 SV\_TARGET 시스템 값 의미 체계로 지정합니다.

Direct3D 11: 큐브 개체에 대한 픽셀 셰이더(HLSL)

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

위치에서 픽셀의 색상이 렌더링 대상에 쓰여집니다. 이제, [화면에 그리기](draw-to-the-screen.md)에서 렌더링 대상의 콘텐츠를 표시하는 방법을 살펴보겠습니다!

## <a name="previous-step"></a>이전 단계


[꼭짓점 버퍼 및 데이터 포팅](port-the-vertex-buffers-and-data-config.md) 다음 단계
---------

[화면에 그리기](draw-to-the-screen.md) 설명
-------

HLSL 의미 체계 및 상수 버퍼 압축을 이해하면 약간의 디버깅 문제를 해결하고 최적화 기회를 이용할 수 있습니다. 기회가 되면 [변수 구문(HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509706), [Direct3D 11의 버퍼 소개](https://msdn.microsoft.com/library/windows/desktop/ff476898) 및 [방법: 상수 버퍼 만들기](https://msdn.microsoft.com/library/windows/desktop/ff476896)를 확인해 보세요. 그렇지 않은 경우 의미 체계 및 상수 버퍼에 대해 고려할 몇 가지 시작 팁이 있습니다.

-   항상 렌더러의 Direct3D 구성 코드를 다시 확인하여 상수 버퍼의 구조가 HLSL의 상수 구조체 선언과 일치하고, 구성 요소 스칼라가 두 선언에서 일치하는지 확인합니다.
-   렌더러의 C++ 코드에서는 상수 버퍼 선언에 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) 유형을 사용하여 올바른 데이터 압축을 보장합니다.
-   상수 버퍼를 효율적으로 사용하는 가장 좋은 방법은 해당 업데이트 빈도에 따라 상수 버퍼에 셰이더 변수를 구성하는 것입니다. 예를 들어, 프레임마다 업데이트되는 일부 유니폼 데이터가 있고 카메라가 이동할 때만 업데이트되는 다른 유니폼 데이터가 있는 경우 두 개의 별도 상수 버퍼에 해당 데이터를 구분하는 것을 고려하세요.
-   잊어버리고 적용하지 않거나 잘못 적용한 의미 체계가 셰이더 컴파일(FXC) 오류의 최초 소스입니다. 다시 확인하세요! 여러 이전 페이지 및 샘플이 Direct3D 11 이하 HLSL 의미 체계의 다른 버전을 참조하므로 문서가 약간 혼동스러울 수 있습니다.
-   각 셰이더를 대상으로 하는 Direct3D 기능 수준을 알아야 합니다. 기능 수준 9\_\*의 의미 체계는 11\_1의 의미 체계와 다릅니다.
-   The SV\_POSITION 의미 체계는 관련된 사후 보간 위치 데이터를 결정하여 값을 조정합니다. 여기서 x는 0과 렌더링 대상 너비 사이입니다. y는 0과 렌더링 대상 높이 사이입니다. z는 원래 같은 유형의 좌표 w 값으로 나눕니다(z/w). w는 원래 w 값으로 1을 나눈 값입니다(1/w).

## <a name="related-topics"></a>관련 항목


[방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 포팅](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[셰이더 개체 포팅](port-the-shader-config.md)

[꼭짓점 버퍼 및 데이터 포팅](port-the-vertex-buffers-and-data-config.md)

[화면에 그리기](draw-to-the-screen.md)

 

 




