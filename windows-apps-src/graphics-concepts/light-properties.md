---
title: 조명 속성
description: 조명 속성은 광원의 유형(점, 방향성, 스포트라이트), 감쇠, 색상, 방향, 위치, 범위를 설명합니다.
ms.assetid: E832C3FD-9921-41C4-87B8-056E16B61B77
keywords:
- 조명 속성
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 07a465d8fdcd1d425ed62e8d83cadd261f316da2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5831769"
---
# <a name="light-properties"></a>조명 속성


조명 속성은 광원의 유형(점, 방향성, 스포트라이트), 감쇠, 색상, 방향, 위치, 범위를 설명합니다. 사용 중인 조명의 유형에 따라 조명에 감쇠 및 범위 속성 또는 스포트라이트 효과 속성이 있을 수 있습니다. 모든 종류의 빛이 모든 속성을 사용하지는 않습니다.

위치, 범위, 감쇠 속성은 월드 공간에서 조명의 위치, 조명에서 방출된 빛이 원거리에서 어떻게 동작하는지 정의합니다.

## <a name="span-idlightattenuationspanspan-idlightattenuationspanspan-idlightattenuationspanlight-attenuation"></a><span id="Light_Attenuation"></span><span id="light_attenuation"></span><span id="LIGHT_ATTENUATION"></span>조명 감쇠


감쇠는 범위 속성에서 정의한 최대 거리를 향하는 동안 조명 강도가 어떻게 줄어드는지 제어합니다. Attenuation0, Attenuation1, Attenuation2 등 세 개의 부동 소수점 값이 빛 감쇠를 나타내는 데 사용되기도 합니다. 0.0부터 무한대까지 이 부동 소수점 값이 조명 감쇠를 제어합니다. 일부 응용 프로그램은 Attenuation1 구성원을 1.0으로 나머지를 0.0으로 설정하여 1 / D로 변화하는 빛 강도를 만들어냅니다. 여기서 D는 광원에서 꼭짓점까지 거리입니다. 최대 빛 강도는 광원에서 측정되며 조명 범위에서 1/(조명 범위)로 줄어듭니다.

일반적으로 응용 프로그램은 Attenuation0을 0.0으로 Attenuation1을 상수 값으로, Attenuation2를 0.0으로 설정하는데, 이 값을 바꿔 다양한 조명 효과를 낼 수 있습니다. 감쇠 값을 조합하면 더욱 복잡한 감쇠 효과를 낼 수 있습니다. 또는 일반 범위를 벗어난 값으로 설정하여 더욱 독특한 감쇠 효과를 만들어낼 수 있습니다. 하지만 음의 감쇠 값은 허용되지 않습니다. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요.

## <a name="span-idlightcolorspanspan-idlightcolorspanspan-idlightcolorspanlight-color"></a><span id="Light_Color"></span><span id="light_color"></span><span id="LIGHT_COLOR"></span>조명 색상


Direct3D에서 조명은 시스템의 조명 계산에서 독립적으로 사용되는 세 가지 색상, 즉 확산 색, 주변 색, 반사 색을 방출합니다. 각각의 색은 Direct3D 조명 모듈에 의해 통합되며 현재 재료의 상대와 상호 작용하여 렌더링에 사용되는 최종 색상을 만들어냅니다. 확산 색은 현재 재료의 확산 반사율 속성과 상호 작용하고 반사 색은 재료의 반사율 속성 등과 상호 작용합니다. Direct3D가 어떻게 이러한 색상에 적용되는지에 관한 자세한 내용은 [조명의 수학](mathematics-of-lighting.md)을 참조하세요.

Direct3D 응용 프로그램에는 일반적으로 방출되는 색상을 정의하는 확산 색, 주변 색, 반사 색 등 세 가지 색상 값이 있습니다.

시스템의 계산에 가장 많이 적용되는 색상 유형은 확산 색입니다. 가장 흔한 확산 색은 흰색(R:1.0 G:1.0 B:1.0)이지만 원하는 효과를 내는 데 필요한 색상을 만들어낼 수 있습니다. 예를 들어 벽난로에 적색광을 사용하거나 녹색 신호등에 녹색광을 사용할 수 있습니다.

일반적으로 빛 색상 구성 요소를 0.0 ~ 1.0(끝 값 포함) 범위의 값으로 설정할 수 있지만 의무는 아닙니다. 예를 들어 모든 구성 요소를 2.0으로 설정하여 "흰색보다 밝은" 빛을 만들 수 있습니다. 이러한 유형의 설정은 상수 이외의 감쇠 설정을 사용할 때 특히 유용할 수 있습니다.

Direct3D가 조명에 RGBA 값을 사용하더라도 알파 색 구성 요소는 사용되지 않습니다.

일반적으로 재료 색상은 조명에 사용됩니다. 그러나 재료 색상(방출 색, 주변 색, 확산 색, 반사 색)이 확산 또는 반사 꼭짓점 색상으로 재정의되도록 지정할 수 있습니다.

알파/투명도 값은 항상 확산 색의 알파 채널에서만 나옵니다.

안개 값은 항상 반사 색의 알파 채널에서만 나옵니다.

## <a name="span-idlightdirectionspanspan-idlightdirectionspanspan-idlightdirectionspanlight-direction"></a><span id="Light_Direction"></span><span id="light_direction"></span><span id="LIGHT_DIRECTION"></span>조명 방향


조명 방향 속성은 월드 공간에서 물체 이동에 따라 방출되는 빛의 방향을 결정합니다. 방향은 방향성 광원과 스포트라이트에서만 사용되고 벡터로 설명됩니다.

조명 방향을 벡터로 설정합니다. 장면에서 조명 위치와 상관없이 방향 벡터는 논리적 원점으로부터의 거리로 설명됩니다. 따라서 양의 z 축을 따라 장면에 직접 비춰지는 스포트라이트는 위치 정의와 상관없이 &lt;0,0,1&gt;의 방향 벡터를 갖습니다. 마찬가지로 방향이 &lt;0,-1,0&gt;인 방향성 광원을 사용하여 장면에 바로 비치는 햇빛을 시뮬레이션할 수 있습니다. 좌표 축을 따라 빛나는 빛을 만들어낼 필요는 없습니다. 다양한 값을 조합하여 더 다양한 각도로 빛나는 빛을 만들어낼 수 있습니다.

조명 방향 벡터를 정규화할 필요는 없지만 크기가 있다는 점에 유의해야 합니다. 다시 말해 &lt;0,0,0&gt; 방향 벡터를 사용하지 마십시오.

## <a name="span-idlightpositionspanspan-idlightpositionspanspan-idlightpositionspanlight-position"></a><span id="Light_Position"></span><span id="light_position"></span><span id="LIGHT_POSITION"></span>조명 위치


조명 위치는 벡터 구조를 이용해 설명됩니다. x, y, z 좌표는 월드 공간에 있는 것으로 간주됩니다. 방향성 광원은 위치 속성을 사용하지 않는 유일한 빛 유형입니다.

## <a name="span-idlightrangespanspan-idlightrangespanspan-idlightrangespanlight-range"></a><span id="Light_Range"></span><span id="light_range"></span><span id="LIGHT_RANGE"></span>조명 범위


월드 공간에서 조명의 범위 속성은 장면에서 메시가 더 이상 그 물체에서 방출되는 빛을 받아들이지 않는 거리를 결정합니다. 방향성 광원은 범위 속성을 사용하지 않습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명과 재료](lights-and-materials.md)

 

 




