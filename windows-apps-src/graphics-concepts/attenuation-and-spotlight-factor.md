---
title: 감쇠 및 스포트라이트 계수
description: 전역 조명 수식의 확산 및 반사 조명 구성 요소에는 조명 감쇠와 스포트라이트 원뿔을 설명하는 용어가 있습니다.
ms.assetid: F61D4ACB-09AB-4087-9E2D-224E472D6196
keywords:
- 감쇠 및 스포트라이트 계수
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8126ac8fa738a2b8a9680d215179fe23f77c5d44
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7849233"
---
# <a name="attenuation-and-spotlight-factor"></a>감쇠 및 스포트라이트 계수


전역 조명 수식의 확산 및 반사 조명 구성 요소에는 조명 감쇠와 스포트라이트 원뿔을 설명하는 용어가 있습니다. 다음은 이러한 용어에 대한 설명입니다.

## <a name="span-idattenuationspanspan-idattenuationspanspan-idattenuationspanattenuation"></a><span id="Attenuation"></span><span id="attenuation"></span><span id="ATTENUATION"></span>감쇠


조명의 감쇠는 조명 유형 및 조명과 꼭짓점 위치 간의 거리에 따라 달라집니다. 감쇠를 계산하려면 다음 수식 중 하나를 사용합니다.

Atten = 1/( att0<sub>i</sub> + att1<sub>i</sub> \* d + att2<sub>i</sub> \* d²)

다음은 항목에 대한 설명입니다.

| 매개 변수        | 기본값 | 유형           | 설명                                     | 범위          |
|------------------|---------------|----------------|-------------------------------------------------|----------------|
| att0<sub>i</sub> | 0.0           | 부동 소수점 | 정적 감쇠 계수                     | 0 ~ 양의 무한대 |
| att1<sub>i</sub> | 0.0           | 부동 소수점 | 선형 감쇠 계수                       | 0 ~ 양의 무한대 |
| att2<sub>i</sub> | 0.0           | 부동 소수점 | 이차 감쇠 계수                    | 0 ~ 양의 무한대 |
| d                | 해당 없음           | 부동 소수점 | 꼭짓점 위치에서 조명 위치까지의 거리 | 해당 없음            |

 

-   Atten = 1, 방향성 광원인 경우
-   Atten = 0, 조명과 꼭짓점 간의 거리가 조명의 범위를 초과할 경우

조명과 꼭짓점 위치 간의 거리는 항상 양수입니다.

d = | L<sub>dir</sub> |

다음은 항목에 대한 설명입니다.

| 매개 변수       | 기본값 | 유형                                             | 설명                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dir</sub> | 해당 없음           | x, y 및 z 부동 소수점 값을 포함한 3D 벡터 | 꼭짓점 위치에서 조명 위치까지의 방향 벡터 |

 

d가 조명의 범위보다 클 경우 Direct3D가 더 이상 감쇠 계산을 수행하지 않고 조명의 효과를 꼭짓점에 적용하지 않습니다.

감쇠 상수는 수식에서 계수 역할을 합니다. 감쇠 상수를 간단히 조정하여 다양한 감쇠 곡선을 만들 수 있습니다. Attenuation1을 1.0으로 설정하여 감쇠되지 않지만 범위로 제한되는 조명을 만들거나 여러 값을 사용하여 다양한 감쇠 효과를 낼 수 있습니다.

최대 조명 범위의 감쇠는 0.0이 아닙니다. 조명 범위에 있을 때 조명이 갑자기 나타나지 않도록 응용 프로그램에서 조명 범위를 늘릴 수 있습니다. 또는 감쇠 계수가 조명 범위에서 0.0에 가깝도록 응용 프로그램에서 감쇠 상수를 설정할 수 있습니다. 감쇠 값에 조명 색의 빨강, 녹색 및 파랑 구성 요소를 곱하여 조명이 꼭짓점으로 이동하는 거리의 계수로 조명의 강도를 조정합니다.

## <a name="span-idspotlight-factorspanspan-idspotlight-factorspanspan-idspotlight-factorspanspotlight-factor"></a><span id="Spotlight-Factor"></span><span id="spotlight-factor"></span><span id="SPOTLIGHT-FACTOR"></span>스포트라이트 계수


다음은 스포트라이트 계수를 지정하는 수식입니다.

![스포트라이트 계수의 수식](images/dx8light9.png)

| 매개 변수         | 기본값 | 유형           | 설명                              | 범위                    |
|-------------------|---------------|----------------|------------------------------------------|--------------------------|
| rho<sub>i</sub>   | 해당 없음           | 부동 소수점 | 스포트라이트 i에 대한 코사인(각도)            | 해당 없음                      |
| phi<sub>i</sub>   | 0.0           | 부동 소수점 | 스포트라이트 i의 반영 각도(라디안 단위) | \[theta<sub>i</sub>, pi) |
| theta<sub>i</sub> | 0.0           | 부동 소수점 | 스포트라이트 i의 본영 각도(라디안 단위)    | \[0, pi)                 |
| falloff           | 0.0           | 부동 소수점 | 대칭 계수                           | (음의 무한대, 양의 무한대)   |

 

다음은 항목에 대한 설명입니다.

rho = norm(L<sub>dcs</sub>)<sup>.</sup>norm(L<sub>dir</sub>)

및

| 매개 변수       | 기본값 | 유형                                             | 설명                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dcs</sub> | 해당 없음           | x, y 및 z 부동 소수점 값을 포함한 3D 벡터 | 카메라 공간에서 조명 방향의 음수         |
| L<sub>dir</sub> | 해당 없음           | x, y 및 z 부동 소수점 값을 포함한 3D 벡터 | 꼭짓점 위치에서 조명 위치까지의 방향 벡터 |

 

조명 감쇠를 계산한 후 꼭짓점에 대한 확산 및 반사 구성 요소를 계산하기 위해 Direct3D는 스포트라이트 효과(적용 가능한 경우), 표면에서 빛이 반사되는 각도 및 현재 재질의 반사율도 고려합니다. [조명 유형](light-types.md)에서 "스포트라이트"를 참조하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




