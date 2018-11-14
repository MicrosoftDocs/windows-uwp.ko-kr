---
title: 조명 개요
description: Direct3D 조명을 사용하는 경우 Direct3D가 조명의 세부 사항을 대신 처리하도록 허용합니다. 고급 사용자는 필요에 따라 직접 조명 작업을 수행할 수 있습니다.
ms.assetid: FCBF6A92-4EAC-4CCC-A76C-79985AF348AE
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6eca73beae6634d1809c0e9e779d80a43b495a65
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6164689"
---
# <a name="lighting-overview"></a>조명 개요

Direct3D 조명을 사용하는 경우 Direct3D가 조명의 세부 사항을 대신 처리하도록 허용합니다. 고급 사용자는 필요에 따라 직접 조명 작업을 수행할 수 있습니다.

조명을 사용하면 Direct3D가 다음 항목의 조합을 기반으로 각 개체 꼭짓점의 색상을 계산합니다.

-   연결된 텍스처 지도의 텍셀.
-   꼭짓점의 확산 색과 반사 색(지정된 경우).
-   장면의 광원 또는 장면의 주변 조도에서 형성되는 조명 색상과 강도.

조명과 재료를 다루는 방식에 따라 렌더링된 장면의 표현 결과가 크게 달라집니다. 재료에 따라 빛이 표면에서 반사되는 양상이 다릅니다. 직접 조명과 주변 조도에 따라 빛의 반사가 달라집니다. 조명을 사용하는 경우 장면을 렌더링할 재료를 사용해야 합니다.

장면을 렌더링하기 위해 조명이 반드시 필요하지는 않으나 빛 없이 렌더링된 장면에서는 세부가 보이지 않습니다. 조명 없이 장면을 렌더링하면 기껏해야 사물의 윤곽만 보입니다. 대부분의 경우 이 정도로는 충분하지 않습니다.

## <a name="span-iddirectlightvsambientlightspanspan-iddirectlightvsambientlightspandirect-light-vs-ambient-light"></a><span id="direct_light_vs._ambient_light"></span><span id="DIRECT_LIGHT_VS._AMBIENT_LIGHT"></span>직접 조명과 주변 광원 비교


한 장면에서 사물을 비추기 위해 직접 조명과 주변 광원이 모두 사용되지만 서로 독립적으로 작용하며 매우 다른 효과를 가져옵니다. 사용자는 전혀 다른 방식으로 이 둘을 다루어야 합니다.

*직접 조명*은 사물을 직접 비춥니다. 직접 조명은 항상 방향과 색이 있으며 고우러드 음영 등의 음영 알고리즘을 구성하는 요소입니다. 다양한 종류의 빛이 다양한 방식으로 직접 조명을 발산하여 특수한 감쇠 효과를 가져옵니다.

*주변 광원*은 장면의 모든 곳에 존재합니다. 주변 광원은 사물의 종류, 장면 내에서 사물의 위치와 상관없이 장면 전체를 채우는 일정한 강도의 빛입니다. 주변 광원은 위치나 방향이 없고 색과 강도만 있습니다. 모든 빛이 모여 장면의 전체 주변 광을 형성합니다.

주변 광원의 색상은 RGBA 값 형태로 구성되며, 각 요소는 0 ~ 255의 정수 값입니다. 이는 Direct3D의 대다수 색상 값과 다릅니다.

적색, 녹색, 청색 요소가 결합되어 최종적인 주변 광원의 색상을 결정합니다. 알파 구성 요소가 색의 투명도를 제어합니다. 하드웨어 가속이나 RGB 에뮬레이션을 사용하면 알파 구성 요소가 무시됩니다.

## <a name="span-iddirect3dlightmodelvsnaturespanspan-iddirect3dlightmodelvsnaturespandirect3d-light-model-vs-nature"></a><span id="direct3d_light_model_vs._nature"></span><span id="DIRECT3D_LIGHT_MODEL_VS._NATURE"></span>Direct3D 조명 모델과 자연광 비교


자연 상태에서 광원으로부터 빛이 방출되면 수백 또는 수천, 수백만 개의 사물에 반사된 후 사람의 눈에 도달합니다. 빛이 반사될 때 빛의 일부는 표면에 흡수되고 일부는 임의의 방향으로 흩어지며 나머지는 다른 물체의 표면이나 사람의 눈에 도달합니다. 빛이 완전히 사라지거나 사람이 빛을 인지할 때까지 이 과정이 계속됩니다.

빛의 원리를 완벽하게 시뮬레이션하는 데 필요한 계산을 실시간 Direct3D 그래픽에 사용하기에는 너무 많은 시간이 걸립니다. 그래서 Direct3D 조명 모델은 속도를 염두에 두고 자연 상태에서 빛이 작용하는 원리를 추정합니다. Direct3D는 서로 결합되어 최종 색상을 형성하는 적색, 녹색, 청색 요소로 빛을 정의합니다.

Direct3D에서 빛이 표면에 반사되면 조명 색이 표면 자체와 수학적으로 상호 작용하여 화면에 최종적으로 표시되는 색상을 형성합니다. Direct3D가 사용하는 알고리즘에 대한 자세한 내용은 [조명의 수학](mathematics-of-lighting.md)을 참조하세요.

Direct3D 조명 모델은 주변광과 직접 조명, 두 유형으로 일반화할 수 있습니다. 각 조명의 속성은 다르고 표면의 재료와도 다른 방식으로 상호 작용합니다. 주변 광원은 아주 폭넓게 흩어져 있는 빛이기 때문에 방향과 광원을 가늠할 수 없으며 모든 곳에서 낮은 강도로 유지됩니다. 사진 작가가 사용하는 간접 조명이 주변 광원의 좋은 예입니다.

Direct3D에서는 자연 상태에서와 마찬가지로 주변광에 실제 방향이나 광원이 없고 색상과 강도만 있습니다. 사실 주변 조도는 장면에서 빛을 형성하는 사물과는 완전히 독립적입니다. 주변 광원은 정반사에 영향을 주지 않습니다.

직접 조명은 장면 내에서 광원이 형성하는 빛입니다. 항상 색과 강도가 있고 지정된 방향으로 이동합니다. 직접 조명은 표면의 재료와 상호 작용하여 반사 하이라이트를 형성하고, 조명의 방향이 고우러드 음영을 비롯한 음영 알고리즘의 한 요소로 사용됩니다. 직접 조명이 반사되어도 장면의 주변 조도에 영향을 미치지 않습니다. 장면에서 직접 조명을 형성하는 광원은 장면에서 빛이 사용되는 방식에 영향을 주는 다양한 특징을 지닙니다.

게다가 다각형의 재료는 다각형에 도달한 빛을 반사하는 방식에 영향을 미치는 속성이 있습니다. 재료가 주변 광원을 반사하는 방식을 결정하는 단일 반사 특성을 설정하고, 재료의 정반사율과 확산 반사율을 결정하는 개별 특성을 설정합니다.

## <a name="span-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspancolor-values-for-lights-and-materials"></a><span id="Color_Values_for_Lights_and_Materials"></span><span id="color_values_for_lights_and_materials"></span><span id="COLOR_VALUES_FOR_LIGHTS_AND_MATERIALS"></span>조명과 재료의 색 값


Direct3D는 서로 결합되어 최종 색상을 형성하는 네 개 구성 요소(적색, 녹색, 청색, 알파)로 색을 정의합니다. 각 구성 요소의 범위는 0.0 ~ 1.0입니다. 빛과 재료는 모두 동일한 구조를 사용하여 색을 정의하지만, 값은 빛과 재료에서 각기 조금씩 다르게 사용됩니다.

광원의 색 값은 광원이 방출하는 특정 광원 요소의 양을 나타냅니다. 조명은 알파 요소를 사용하지 않기 때문에 색상의 적색, 녹색, 청색 요소만 의미가 있습니다. 세 개 구성 요소를 프로젝션 텔레비전에 적색, 녹색, 청색 렌즈로 시각화할 수 있습니다. 각 렌즈를 끄거나(적절한 구성원에서 값 0.0), 최대한 밝기를 높이거나(값 1.0) 그 사이의 값이 될 수 있습니다.

렌즈를 통해 들어오는 색상이 결합되어 빛의 최종 색상을 형성합니다. R(1.0), G(1.0), B(1.0)의 조합은 흰색 빛을, R(0.0), G(0.0), B(0.0)의 조합은 빛을 전혀 방출하지 않습니다. 한 구성 요소만 사용하여 순수한 적색, 녹색 또는 청색광을 방출하는 빛을 만들어낼 수 있습니다. 또는 황색 또는 보라색 등의 색상을 방출하는 조합도 사용할 수 있습니다. 음의 색상 요소 값을 설정하여 장면에서 빛을 실제로 제거하는 "어두운 빛"을 만들 수도 있습니다. 또는 구성 요소를 1.0보다 큰 값으로 설정하여 아주 밝은 빛을 만들 수 있습니다.

한편, 재료를 사용할 경우 색상 값은 그 재료로 렌더링한 표면에서 빛 요소가 어느 정도 반사되는지 나타냅니다. 색상 구성 요소가 R(1.0), G(1.0), B(1.0), A(1.0)인 재료는 표면에 닿은 모든 빛을 반사합니다. 마찬가지로 R(0.0), G(1.0), B(0.0), A(1.0)인 재료는 표면에 닿는 모든 녹색 빛을 반사합니다. 재료의 반사율 값은 여러 개라서 다양한 종류의 효과를 냅니다.

[조명 유형](light-types.md) 및 [조명 속성](light-properties.md)을 참조하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명과 재료](lights-and-materials.md)

 

 




