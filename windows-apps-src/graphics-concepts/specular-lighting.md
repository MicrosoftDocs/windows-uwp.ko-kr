---
title: "반사 조명"
description: "반사 조명은 빛이 개체 표면에 닿았다가 카메라를 향해 반사하면서 발생하는 밝은 반사 하이라이트입니다."
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords: "반사 조명"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 305924b19914bf4e85366695add2476c3c81c281
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="specular-lighting"></a>반사 조명


*반사 조명*은 빛이 개체 표면에 닿았다가 카메라를 향해 반사하면서 발생하는 밝은 반사 하이라이트를 말합니다. 반사 조명은 확산 조명보다 계산이 더 많이 필요하며, 개체 표면에서 보다 빠르게 감소합니다. 반사 조명은 확산 조명보다 계산이 오래 걸리지만, 반사 조명을 사용하는 이점은 표면에 상당한 디테일을 추가할 수 있다는 것입니다.

반사 리플렉션을 모델링하려면 시스템이 조명이 이동하는 방향과 뷰어의 시선에 대한 방향을 알고 있어야 합니다. 시스템은 Phong 반사 리플렉션 모델을 단순화한 버전을 사용합니다. 이 버전은 중간 벡터를 사용하여 반사 리플렉션의 강도를 근사값으로 계산합니다.

기본 조명 상태는 반사 하이라이트를 계산하지 않습니다.

## <a name="span-idspecularlightingequationspanspan-idspecularlightingequationspanspan-idspecularlightingequationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>반사 조명 수식


반사 조명은 다음 수식에 의해 설명됩니다.

|                                                                             |
|-----------------------------------------------------------------------------|
| 반사 조명 = Cₛ \* sum\[Lₛ \* (N · H)<sup>P</sup> \* Atten \* Spot\] |

 

변수, 형식 및 해당 범위는 다음과 같습니다.

| 매개 변수    | 기본값 | 형식                                                             | 설명                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Cₛ           | (0,0,0,0)     | 빨간색, 녹색, 파란색 및 알파 투명도(부동 소수점 값) | 반사 색.                                                                                        |
| sum          | 해당 없음           | 해당 없음                                                              | 각 빛의 반사 구성 요소 합계.                                                          |
| N            | 해당 없음           | 3D 벡터(x, y, 및 z 부동 소수점 값)                    | 꼭지점 법선.                                                                                         |
| H            | 해당 없음           | 3D 벡터(x, y, 및 z 부동 소수점 값)                    | 벡터의 절반. 중간 벡터에 대한 섹션을 참조하세요.                                                |
| <sup>P</sup> | 0.0           | 부동 소수점                                                   | 반사 리플렉션 강도. 범위는 0부터 무한대입니다.                                                     |
| Lₛ           | (0,0,0,0)     | 빨간색, 녹색, 파란색 및 알파 투명도(부동 소수점 값) | 조명 반사 색.                                                                                  |
| Atten        | 해당 없음           | 부동 소수점                                                   | 조명 감쇠 값. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요. |
| Spot         | 해당 없음           | 부동 소수점                                                   | 스포트라이트 계수. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요.        |

 

Cₐ의 값은 다음 중 하나입니다.

-   반사 재질 소스가 확산 꼭짓점 색이고, 꼭지점 선언에 첫 번째 꼭짓점 색이 제공된 경우, 꼭짓점 색 1.
-   반사 재질 소스가 반사 꼭짓점 색이고, 꼭지점 선언에 두 번째 꼭짓점 색이 제공된 경우, 꼭짓점 색 2.
-   재질 반사 색

**참고**  반사 재질 소스 옵션을 사용하고 꼭짓점 색을 제공하지 않을 경우 재질 반사 색이 사용됩니다.

 

반사 구성 요소는 모든 조명이 처리되고 별도로 보간된 후 0~255로 고정됩니다.

## <a name="span-idthehalfwayvectorspanspan-idthehalfwayvectorspanspan-idthehalfwayvectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>중간 벡터


중간 벡터(H)는 다음 두 벡터 사이에 존재합니다. 개체 꼭짓점에서 광원까지의 벡터 및 개체 꼭짓점에서 카메라 위치까지의 벡터. Direct3D는 중간 벡터를 계산하는 두 가지 방법을 제공합니다. (직교 반사 하이라이드 대신) 카메라 기준 반사 하이라이트가 활성화된 경우 시스템이 카메라 위치 및 꼭지점 위치와 조명의 방향 벡터를 함께 사용하여 중간 벡터를 계산합니다. 다음 수식이 이것을 보여 줍니다.

|                                           |
|-------------------------------------------|
| H = norm(norm(Cₚ - Vₚ) + L<sub>dir</sub>) |

 

| 매개 변수       | 기본값 | 형식                                          | 설명                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Cₛ              | 해당 없음           | 3D 벡터(x, y, 및 z 부동 소수점 값) | 카메라 위치.                                             |
| Vₚ              | 해당 없음           | 3D 벡터(x, y, 및 z 부동 소수점 값) | 꼭짓점 위치.                                             |
| L<sub>dir</sub> | 해당 없음           | 3D 벡터(x, y, 및 z 부동 소수점 값) | 꼭짓점 위치에서 조명 위치까지의 방향 벡터. |

 

이러한 방식으로 중간 벡터를 결정하면 많은 계산이 필요할 수 있습니다. 하나의 대안으로, (카메라 기준 반사 하이라이트 대신) 직교 반사 하이라이트를 사용하여 시스템에게 마치 관점이 z축에서 무한대로 멀리 있는 것처럼 동작하도록 지시합니다. 이 방식은 다음 수식에 반영되어 있습니다.

|                                     |
|-------------------------------------|
| H = norm((0,0,1) + L<sub>dir</sub>) |

 

이 설정은 계산이 덜 필요하지만 훨씬 덜 정확하므로 직교 투영을 사용하는 앱에서 사용하는 것이 좋습니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


이 예제에서는 장면 반사 광원 색과 재질 반사 색이 개체에 지정됩니다.

수식에 따라 재질 색과 광원 색을 합친 색이 개체 꼭짓점의 색이 됩니다.

다음 두 그림에서는 반사 재질 색이 회색이고 반사 광원 색이 흰색입니다.

![회색 구 그림](images/amb1.jpg)![흰색 구 그림](images/lightwhite.jpg)

결과 반사 하이라이트는 다음 그림에 나와 있습니다.

![반사 하이라이트 그림](images/lights.jpg)

반사 하이라이트를 주변 및 확산 조명과 결합하면 다음 그림이 만들어집니다. 세 가지 유형의 조명이 모두 적용되어 보다 명확하게 실제 개체를 모사합니다.

![반사 하이라이트, 주변 조명 및 확산 조명 결합을 보여주는 그림](images/lightads.jpg)

반사 조명은 확산 조명보다 계산이 더 많이 필요합니다. 일반적으로 표면 재질에 대한 시각적 단서를 제공하는 데 사용합니다. 반사 하이라이트는 표면 재질에 따라 크기와 색이 다릅니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




