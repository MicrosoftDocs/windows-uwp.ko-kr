---
title: 확산 조명
description: 확산 조명은 조명 방향과 개체 표면 법선 모두에 따라 달라집니다.
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- 확산 조명
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5846edda167823b7ae161d332fbde450ccf20d72
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7581643"
---
# <a name="diffuse-lighting"></a>확산 조명


*확산 조명*은 조명 방향과 개체 표면 법선 모두에 따라 달라집니다. 확산 조명은 조명 방향 및 표면 법선 벡터의 변화로 인해 개체 표면 전반에 걸쳐 달라집니다. 각 개체 꼭짓점마다 달라지기 때문에 확산 조명을 계산하는 데 시간이 더 오래 걸리지만, 개체에 그늘을 드리워 3차원(3D) 깊이를 만들어낸다는 것이 확산 조명 사용의 이점이기도 합니다.

모든 감쇠 효과에 대한 조명 강도를 조정한 후, 조명 엔진이 꼭짓점 법선의 각도와 입사 조명 방향을 기준으로 꼭짓점에서 반사되는 남은 조명의 양을 계산합니다. 지향 조명은 거리에 따라 약해지기 때문에 조명 엔진은 이 단계를 건너뜁니다. 시스템은 확산과 반사의 두 반사 유형을 고려하며, 각각에 반사되는 조명의 양을 판단하기 위해 다른 수식을 사용합니다.

반사된 조명의 양을 계산한 후, Direct3D는 이러한 새 값을 현재 물체의 확산 및 반사 속성에 적용합니다. 결과 색 값은 래스터라이저가 고우러드 채색 및 반사 강조를 생성하는 데 사용하는 확산 및 반사 구성 요소입니다.

확산 조명은 다음 수식에 의해 설명됩니다.

확산 조명 = sum\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup>L<sub>dir</sub>)\*Atten\*Spot\]

| 매개 변수       | 기본값 | 유형          | 설명                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| sum             | 해당 없음           | 해당 없음           | 각 조명의 확산 구성 요소 합계.                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | 확산 색상.                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | 조명 확산 색상.                                                                             |
| N               | 해당 없음           | D3DVECTOR     | 꼭짓점 법선                                                                                    |
| L<sub>dir</sub> | 해당 없음           | D3DVECTOR     | 개체 꼭짓점에서 조명으로의 방향 벡터.                                                |
| Atten           | 해당 없음           | FLOAT         | 조명 감쇠. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요. |
| Spot            | 해당 없음           | FLOAT         | 스포트라이트 계수. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요.  |

 

감쇠(Atten)나 스포트라이트 특성(Spot)을 계산하려면 [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요.

확산 구성 요소는 모든 조명이 처리되고 별도로 보간된 후 0~255로 고정됩니다. 결과 확산 조명 값은 주변, 확산 및 발광 조명 값의 조합입니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


이 예에서 개체는 조명 확산 색상과 물질 확산 색상을 사용하여 색상이 지정됩니다.

수식에 따라 물체 색과 광원 색을 조합한 결과가 개체 꼭짓점의 색이 됩니다.

다음 두 그림은 재질 색(회색)과 조명 색(밝은 빨강)을 보여 줍니다.

![회색 구 그림](images/amb1.jpg)![빨강 구 그림](images/lightred.jpg)

결과 장면은 다음 그림에 나와 있습니다. 장면의 개체는 구 뿐입니다. 확산 조명 계산은 물질과 조명 확산 색상을 고려하며, 조명 방향과 내적을 사용한 꼭짓점 법선 사이의 각도를 사용하여 수정합니다. 결과적으로, 구 뒷면의 표면은 조명에서 곡선으로 떨어지므로 어두워집니다.

![확산 광원이 적용된 구 그림](images/lightd.jpg)

이전 예에서 확산 조명과 주변 조명의 조합이 개체의 전체 표면에 음영을 입힙니다. 다음 그림과 같이 주변 조명이 전에 표면에 음영을 입히고 확산 조명이 개체의 3D 모양을 밝히는 데 도움이 됩니다.

![확산 조명과 주변 조명을 사용한 구 그림](images/lightad.jpg)

확산 조명은 주변 조명보다 계산이 더 많이 필요합니다. 이 조명은 꼭짓점 법선 및 조명 방향에 따라 달라지므로, 주변 조명보다 더 현실적인 조명을 생성하는 3D 공간에서 개체 기하 도형을 볼 수 있습니다. 더 현실적인 모습을 표현하기 반사 하이라이트를 사용할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




