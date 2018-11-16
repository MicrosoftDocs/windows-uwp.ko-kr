---
title: 조명의 수학
description: Direct3D 조명 모델은 주변, 확산, 반사 및 발광 조명을 포함합니다. 따라서 광범위한 조명 상황을 해결하기에 충분한 유연성이 있습니다. 한 장면에 비치는 빛의 총량을 전역 조명이라고 합니다.
ms.assetid: D0521F56-050D-4EDF-9BD1-34748E94B873
keywords:
- 조명의 수학
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 19734964c9b4ab087f7d5fd6ea749b57cccce26c
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6858958"
---
# <a name="mathematics-of-lighting"></a>조명의 수학


Direct3D 조명 모델은 주변, 확산, 반사 및 발광 조명을 포함합니다. 따라서 광범위한 조명 상황을 해결하기에 충분한 유연성이 있습니다. 한 장면에 비치는 빛의 총량을 *전역 조명*이라고 합니다.

전역 조명은 다음과 같이 계산합니다.

```
Global Illumination = Ambient Light + Diffuse Light + Specular Light + Emissive Light 
```

[주변 조명](ambient-lighting.md)은 일정한 조명입니다. 주변 조명은 모든 방향에서 일정하며 개체의 모든 픽셀에 동일한 색을 지정합니다. 따라서 계산이 빠르지만 개체가 편평해 보이고 사실성이 떨어집니다.

[확산 조명](diffuse-lighting.md)은 조명 방향과 개체 표면 일반에 따라 달라집니다. 확산 조명은 조명 방향 및 표면 법선 벡터의 변화로 인해 개체 표면 전반에 걸쳐 달라집니다. 각 개체 꼭짓점마다 달라지기 때문에 확산 조명을 계산하는 데 시간이 더 오래 걸리지만, 개체에 그늘을 드리워 3차원(3D) 깊이를 만들어낸다는 것이 확산 조명 사용의 이점이기도 합니다.

[반사 조명](specular-lighting.md)은 빛이 개체 표면에 닿았다가 카메라를 향해 반사하면서 발생하는 밝은 반사 하이라이트를 말합니다. 반사 조명은 확산 조명보다 계산이 더 많이 필요하며, 개체 표면에서 보다 빠르게 감소합니다. 반사 조명은 확산 조명보다 계산하는 데 시간이 더 걸리지만, 반사 조명을 사용하는 이점은 표면에 상당한 디테일을 추가할 수 있다는 것입니다.

[발광 조명](emissive-lighting.md)은 개체(예: 불빛)에서 발산하는 빛입니다. 발광은 렌더링된 개체가 자체 발광하는 것처럼 보이게 합니다. 발광은 개체의 색에 영향을 미칩니다. 예를 들어 어두운 재료를 밝게 만들고 발산되는 색을 부분적으로 띠게 만들 수 있습니다.

이러한 유형의 각 조명을 3D 장면에 적용하여 사실적인 조명을 얻을 수 있습니다. 주변, 발광, 확산 구성 요소에 대해 계산한 값은 확산 꼭짓점 색으로 출력되고, 반사 조명 구성 요소에 대한 값은 반사 꼭짓점 색으로 출력됩니다. 주변, 확산, 반사 광원 값은 특정 광원의 감쇠 및 스포트라이트 계수에 영향을 받을 수 있습니다. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요.

더 사실적인 조명 효과를 얻기 위해 광원을 추가하지만, 장면은 렌더링하는 데 시간이 더 많이 걸립니다. 디자이너가 원하는 모든 효과를 얻기 위해 일부 게임은 일반적으로 사용하는 것보다 더 강력한 CPU를 사용합니다. 이러한 경우에는 텍스처 맵을 사용함과 동시에 조명 맵과 환경 맵을 사용하여 장면에 조명을 추가하는 방법으로 조명 계산의 수를 줄이는 것이 일반적입니다.

조명은 카메라 공간에서 계산됩니다. [카메라 공간 변형](camera-space-transformations.md)을 참조하세요. 일반 벡터가 이미 정규화되어 있고, 꼭짓점 혼합이 필요하지 않으며, 변환 매트릭스가 직교하는 등 특수 조건이 존재하는 경우에는 모델 공간에서 최적화된 조명을 계산할 수 있습니다.

월드 매트릭스의 반전을 사용하여 광원의 위치 및 방향을 카메라 위치와 함께 모델 공간으로 변환하는 방법을 통해 모든 조명 계산은 모델 공간에서 이루어집니다. 결과적으로 세계 또는 보기 매트릭스가 비균등 크기 조정을 도입하는 경우, 그 결과로 얻는 조명은 부정확할 수 있습니다.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>이 섹션의 내용


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="ambient-lighting.md">주변 조명</a></p></td>
<td align="left"><p>주변 조명은 장면에 일정한 조명을 비춥니다. 또한 꼭짓점 법선, 조명 방향, 조명 위치 또는 감쇠를 비롯한 다른 조명 요소의 영향을 받지 않기 때문에 모든 개체 꼭짓점을 동일하게 비춥니다. 주변 조명은 모든 방향에서 일정하며 개체의 모든 픽셀에 동일한 색을 지정합니다. 따라서 계산이 빠르지만 개체가 편평해 보이고 사실성이 떨어집니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-lighting.md">확산 조명</a></p></td>
<td align="left"><p><em>확산 조명</em>은 조명 방향과 개체 표면 법선 모두에 따라 달라집니다. 확산 조명은 조명 방향 및 표면 법선 벡터의 변화로 인해 개체 표면 전반에 걸쳐 달라집니다. 각 개체 꼭짓점마다 달라지기 때문에 확산 조명을 계산하는 데 시간이 더 오래 걸리지만, 개체에 그늘을 드리워 3차원(3D) 깊이를 만들어낸다는 것이 확산 조명 사용의 이점이기도 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-lighting.md">반사 조명</a></p></td>
<td align="left"><p><em>반사 조명</em>은 빛이 개체 표면에 닿았다가 카메라를 향해 반사하면서 발생하는 밝은 반사 하이라이트를 말합니다. 반사 조명은 확산 조명보다 계산이 더 많이 필요하며, 개체 표면에서 보다 빠르게 감소합니다. 반사 조명은 확산 조명보다 계산하는 데 시간이 더 걸리지만, 반사 조명을 사용하는 이점은 표면에 상당한 디테일을 추가할 수 있다는 것입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="emissive-lighting.md">발광 조명</a></p></td>
<td align="left"><p><em>발광 조명</em>은 개체(예: 불빛)에서 발산하는 빛입니다. 발광은 렌더링된 개체가 자체 발광하는 것처럼 보이게 합니다. 발광은 개체의 색에 영향을 미칩니다. 예를 들어 어두운 물체를 밝게 만들고 발산되는 색을 부분적으로 띠게 만들 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="camera-space-transformations.md">카메라 공간 변형</a></p></td>
<td align="left"><p>카메라 공간의 꼭짓점은 세계 보기 매트릭스로 개체 꼭지점을 변환하여 계산합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="attenuation-and-spotlight-factor.md">감쇠 및 스포트라이트 계수</a></p></td>
<td align="left"><p>전역 조명 수식의 확산 및 반사 조명 구성 요소에는 광원 감쇠와 스포트라이트 원뿔을 설명하는 용어가 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명과 재료](lights-and-materials.md)

 

 




