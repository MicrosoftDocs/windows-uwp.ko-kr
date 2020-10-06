---
title: 반사 조명
description: 반사 조명은 개체 화면에 조명이 걸려 카메라에 반사 될 때 발생 하는 밝은 반사 하이라이트를 식별 합니다.
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- 반사 조명
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f60bd4019f330058d4396a5b0d75d00f90ecff09
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750189"
---
# <a name="specular-lighting"></a>반사 조명


*반사 조명은* 개체 화면에 조명이 걸려 카메라에 반사 될 때 발생 하는 밝은 반사 하이라이트를 식별 합니다. 반사 조명은 확산 조명 보다 더 강 하며 개체 화면에서 더 빠르게 감소 합니다. 확산 조명 보다는 반사 조명을 계산 하는 데 더 오래 걸리며,이 효과를 사용 하면 표면에 많은 세부 정보를 추가할 수 있습니다.

반사 반사를 모델링 하려면 시스템에서 광원을 이동 하는 방향 및 표시기의 눈 방향에 대해 알고 있어야 합니다. 시스템은 중간 벡터를 사용 하 여 반사 리플렉션의 강도를 대략적으로 사용 하는 퐁 반사 모델의 간소화 된 버전을 사용 합니다.

기본 조명 상태는 반사 하이라이트를 계산 하지 않습니다.

## <a name="span-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>반사 조명 수식


반사 조명은 다음 수식에 설명 되어 있습니다.

> 반사 조명 = Cs \* sum \[ Ls \* (N · H)<sup>P</sup> \* \*\]

변수, 해당 형식 및 해당 범위는 다음과 같습니다.

| 매개 변수    | 기본값 | Type                                                             | 설명                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| 양방향           | (0, 0, 0, 0)     | 빨강, 녹색, 파랑 및 알파 투명도 (부동 소수점 값) | 반사 색입니다.                                                                                        |
| sum          | N/A           | N/A                                                              | 각 조명의 반사 구성 요소에 대 한 합계입니다.                                                          |
| N            | N/A           | 3D 벡터 (x, y 및 z 부동 소수점 값)                    | 꼭 짓 점 일반.                                                                                         |
| H            | N/A           | 3D 벡터 (x, y 및 z 부동 소수점 값)                    | 1/2 방향 벡터. 중간 벡터의 섹션을 참조 하세요.                                                |
| <sup>P</sup> | 0.0           | 부동 소수점                                                   | 반사 거듭제곱입니다. 범위는 0에서 + 무한대입니다.                                                     |
| L           | (0, 0, 0, 0)     | 빨강, 녹색, 파랑 및 알파 투명도 (부동 소수점 값) | 광원 반사 색입니다.                                                                                  |
| Atten        | N/A           | 부동 소수점                                                   | 빛 감쇠 값입니다. [감쇠 및 스포트라이트 요소](attenuation-and-spotlight-factor.md)를 참조 하세요. |
| 스폿         | N/A           | 부동 소수점                                                   | 스포트라이트 요소입니다. [감쇠 및 스포트라이트 요소](attenuation-and-spotlight-factor.md)를 참조 하세요.        |

 

Cs의 값은 다음 중 하나입니다.

-   꼭 짓 점 색 1-반사 재질 소스가 확산 꼭 짓 점 색 이면 첫 번째 꼭 짓 점 색이 꼭 짓 점 선언에 제공 됩니다.
-   꼭 짓 점 색 2, 반사 재질 소스가 반사 꼭 짓 점 색 인 경우 두 번째 꼭 짓 점 색이 꼭 짓 점 선언에 제공 됩니다.
-   재질 반사 색

**참고**    반사 재질 원본 옵션을 사용 하 고 꼭 짓 점 색을 제공 하지 않으면 재질 반사 색이 사용 됩니다.

 

반사 구성 요소는 모든 표시등이 개별적으로 처리 되 고 보간된 후 0에서 255 고정 됩니다.

## <a name="span-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>중간 벡터


중간 벡터 (H)는 두 벡터의 중간에 있습니다. 즉, 개체 꼭 짓 점에서 광원으로의 벡터와 개체 정점에서 카메라 위치로의 벡터가 있습니다. Direct3D에서는 중간 벡터를 계산 하는 두 가지 방법을 제공 합니다. 카메라 상대 반사 하이라이트를 사용 하는 경우 (직교 반사 하이라이트 대신) 시스템은 카메라의 위치와 꼭 짓 점의 위치 (조명의 방향 벡터)를 사용 하 여 중간 벡터를 계산 합니다. 다음 수식에서는이를 보여 줍니다.

> H = 일반 (일반적인 (Cp-Vp) + L<sub>dir</sub>)

 

| 매개 변수       | 기본값 | Type                                          | 설명                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Cp              | N/A           | 3D 벡터 (x, y 및 z 부동 소수점 값) | 카메라 위치.                                             |
| 부사장              | N/A           | 3D 벡터 (x, y 및 z 부동 소수점 값) | 꼭 짓 점 위치입니다.                                             |
| L<sub>dir</sub> | N/A           | 3D 벡터 (x, y 및 z 부동 소수점 값) | 꼭 짓 점 위치에서 밝은 위치로의 방향 벡터입니다. |

 

이러한 방식으로 중간 벡터를 결정 하는 것은 계산 집약적 일 수 있습니다. 대 안으로, 직각 반사 하이라이트 (카메라 상대 반사 하이라이트 대신)를 사용 하 여 시스템이 z 축에서 무한히 떨어져 있는 것 처럼 작동 하도록 지시 합니다. 이는 다음 수식에 반영 됩니다.

> H = 일반 ((0, 0, 1) + L<sub>dir</sub>)

이 설정은 계산을 많이 사용 하는 것이 아니라 매우 정확 하므로 직교 프로젝션을 사용 하는 응용 프로그램에서 가장 효과적입니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예 들어


이 예제에서 개체는 장면 반사 밝은 색과 재질 반사 색을 사용 하 여 색이 지정 됩니다.

수식에 따라 개체 꼭 짓 점에 대 한 결과 색은 재질 색과 밝은 색의 조합입니다.

다음 두 그림에서는 반사 재질 색 (회색)과 반사 밝은 색 (흰색)을 보여 줍니다.

![회색 구 그림](images/amb1.jpg)![흰색 구 그림](images/lightwhite.jpg)

다음 그림에서는 결과 반사 강조 표시를 보여 줍니다.

![반사 하이라이트 그림](images/lights.jpg)

반사 하이라이트와 주변 및 확산 조명을 결합 하면 다음 그림이 생성 됩니다. 세 가지 종류의 조명을 모두 적용 하면 현실적인 개체와 더 명확 하 게 비슷합니다.

![반사 하이라이트, 주변 광원 및 확산 조명 결합 그림](images/lightads.jpg)

반사 조명은 확산 조명 보다 계산 하기 더 집약적입니다. 일반적으로 화면 재질에 대 한 시각적 단서를 제공 하는 데 사용 됩니다. 반사 강조 표시의 크기와 색은 표면의 재질에 따라 다릅니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




