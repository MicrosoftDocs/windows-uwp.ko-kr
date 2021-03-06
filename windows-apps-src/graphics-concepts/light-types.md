---
title: 조명 유형
description: 조명 유형 속성은 어떤 유형의 광원을 사용할지 정의합니다. Direct3D에는 점 광원, 스포트라이트, 방향성 광원 등 세 종류의 조명이 있습니다.
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- 조명 유형
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1815f0956fbc175fec5ca892dbeeec92b2f939ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594698"
---
# <a name="light-types"></a>조명 유형


조명 유형 속성은 어떤 유형의 광원을 사용할지 정의합니다. Direct3D에는 점 광원, 스포트라이트, 방향성 광원 등 세 종류의 조명이 있습니다. 각각의 유형은 서로 다른 방식으로 장면에서 다양한 수준의 계산 오버헤드로 물체를 비춥니다.

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>점 광원


점 광원은 장면 내에서 색상과 위치가 있지만 단일 방향이 아닙니다. 다음 그림과 같이 모든 방향으로 동일하게 빛을 방출합니다.

![점 광원의 그림](images/ptlight.png)

전구가 점 광원의 좋은 예입니다. 점 광원은 감쇠와 범위의 영향을 받고 점 단위로 메시를 비춥니다. 조명을 비추는 동안 Direct3D는 월드 공간에서 점 광원의 위치, 비춰지는 꼭짓점의 좌표를 사용하여 조명의 방향에 해당하는 벡터와 조명이 이동한 거리를 도출합니다. 표면을 비추는 데 빛이 얼마나 필요한지 계산하기 위해 꼭짓점 법선을 따라 두 가지 정보가 모두 사용됩니다.

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>방향성 광원


방향성 광원에는 색상과 방향이 있지만 위치는 없습니다. 병렬 조명을 방출합니다. 즉 방향성 광원에서 나오는 모든 빛은 한 장면에서 같은 방향으로 비춰집니다. 방향성 광원은 태양처럼 거의 무한대의 거리에 있는 광원과 비슷합니다. 방향성 광원은 감쇠나 범위의 영향을 받지 않기 때문에 사용자가 지정하는 방향과 색상은 Direct3D가 꼭짓점 색상을 계산할 때 고려되는 유일한 요소입니다. 조명을 결정하는 요소의 개수가 적기 때문에 계산을 가장 적게 필요로 하는 조명입니다.

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>추천


스포트라이트는 색상, 위치, 빛을 방출하는 방향으로 구성됩니다. 스포트라이트에서 방출되는 조명은 밝은 안쪽 원뿔과 큰 바깥쪽 원뿔로 구성됩니다. 빛의 강도는 다음 그림과 같이 이 둘 사이에서 줄어듭니다.

![안쪽 원뿔과 바깥쪽 원뿔로 구성된 스포트라이트의 그림](images/spotlt.png)

스포트라이트는 밝기, 감쇠, 범위의 영향을 받습니다. 장면에서 물체에 대한 조명 효과를 계산할 때 이러한 요소, 빛이 각 꼭짓점까지 이동한 거리가 계산됩니다. 각 꼭짓점에 대해 이러한 효과를 계산해야 하기 때문에 스포트라이트는 Direct3D에서 가장 복잡한 계산이 필요한 조명입니다.

밝기, 세타, 파이 값은 스포트라이트에만 사용됩니다. 이러한 값이 스포트라이트 개체의 안쪽과 바깥쪽 원뿔의 크기, 빛이 그 사이에서 어떻게 줄어드는지 제어합니다.

세타는 스포트라이트 안쪽 원뿔의 라디안 각도이고 파이 값은 조명의 바깥쪽 원뿔이 이루는 각도입니다. 밝기는 안쪽 원뿔의 바깥쪽 가장자리와 바깥쪽 원뿔의 안쪽 가장자리 사이에서 빛의 강도가 어떻게 줄어드는지 제어합니다. 대부분의 응용 프로그램은 밝기를 1.0으로 설정하여 두 원뿔 사이에서 균일한 밝기를 형성하지만 다른 값은 필요에 따라 설정할 수 있습니다.

다음 그림은 이러한 값들 사이의 관계, 스포트라이트 조명의 안쪽 및 바깥쪽 원뿔에 미치는 영향을 보여줍니다.

![파이 및 세타 값과 스포트라이트 원뿔의 관계를 나타낸 그림](images/spotlt2.png)

스포트라이트는 밝은 안쪽 원뿔과 바깥쪽 원뿔 등 두 부분으로 구성된 원뿔 모양의 빛을 방출합니다. 안쪽 원뿔 내부의 빛이 가장 밝고 바깥쪽 원뿔 밖에는 빛이 없습니다. 조명 강도는 두 영역 사이에서 감쇠합니다. 이 유형의 감쇠를 일반적으로 밝기라고 합니다.

꼭짓점이 받아들이는 빛의 양은 안쪽 또는 바깥쪽 원뿔에서 꼭짓점의 위치에 따라 달라집니다. Direct3D는 조명에서 꼭짓점까지 벡터(D)와 스포트라이트의 방향 벡터(L)의 내적을 계산합니다. 이 값은 두 벡터가 이루는 각도의 코사인과 같고, 안쪽 또는 바깥쪽 원뿔 사이에서 꼭짓점 위치를 결정하기 위해 조명의 원뿔 각도와 비교할 수 있는 꼭짓점의 위치를 나타내는 지표 역할을 합니다. 다음 그림은 이 두 벡터 사이의 연관성을 그래픽으로 보여줍니다.

![스포트라이트 방향 벡터와 꼭짓점에서 스포트라이트까지 벡터 그림](images/spotalg1.png)

시스템이 이 값을 스포트라이트의 안쪽 및 바깥쪽 원뿔 각도의 코사인과 비교합니다. 조명의 세타 및 파이 값은 안쪽 및 바깥쪽 원뿔의 전체 원뿔 각도를 나타냅니다. 꼭짓점이 조명의 중심에서 멀어짐에 따라(전체 원뿔 각도를 가로지르지 않음) 감쇠가 이루어지므로 런타임이 코사인을 계산하기 전에 이 원뿔 각도를 반으로 나눕니다.

벡터 L과 D의 내적이 바깥쪽 원뿔 각도의 코사인 값 이하인 경우 꼭짓점은 바깥쪽 원뿔에서 벗어난 위치에 있으며 조명을 받지 않습니다. L과 D의 내적이 안쪽 원뿔 각도의 코사인보다 큰 경우 꼭짓점이 안쪽 원뿔 내에 있으며 가장 많은 양의 빛을 받습니다. 이 경우에도 원거리에 걸친 감쇠를 고려합니다. 꼭짓점이 두 영역 사이에 있다면 다음 식으로 밝기가 계산됩니다.

![밝기 이후 꼭짓점의 조명 강도를 구하는 식](images/falloff.png)

여기서

-   I<sub>f</sub>는 밝기 이후 조명 강도
-   Alpha는 벡터 L과 D 사이의 각도
-   Theta는 안쪽 원뿔 각도
-   Phi는 바깥쪽 원뿔 각도
-   p는 밝기

이 식은 꼭짓점에서의 밝기를 고려해 조명 강도를 나타내는 0.0 ~ 1.0 범위의 값을 도출합니다. 조명에서 꼭짓점까지 거리의 두 배에 해당하는 감쇠도 적용됩니다. 다음 그래프는 밝기 값이 밝기 곡선에 미치는 영향을 보여줍니다.

![조명 강도와 광원에서 꼭짓점까지 거리를 비교한 그래프](images/fallgraf.png)

다양한 밝기 값이 실제 조명에 미치는 영향은 미세하며, 1.0 이외의 밝기 값으로 밝기 곡선을 형성하면 약간의 성능 저하가 발생합니다. 이러한 이유로 이 값은 일반적으로 1.0으로 설정됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[광원 및 재질](lights-and-materials.md)

 

 




