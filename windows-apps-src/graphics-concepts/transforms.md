---
title: "변환"
description: "Direct3D 파트는 고정 함수 기하 도형 파이프라인을 통해 기하 도형을 푸시하는 변환 엔진입니다."
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords: "변환"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e5eb63a41ad04577196b6a622736211c1d1576f9
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="transforms"></a>변환


Direct3D 파트는 고정 함수 기하 도형 파이프라인을 통해 기하 도형을 푸시하는 변환 엔진입니다. 세계 좌표의 모델 및 뷰어를 찾아내고, 표시할 꼭짓점을 화면에 투사하고, 뷰포트에 대한 꼭짓점을 잘라냅니다. 또한 변환 엔진은 조명 계산을 수행하여 각 꼭짓점의 확산 및 반사 구성 요소를 결정합니다.

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
<td align="left"><p>[변환 개요](transform-overview.md)</p></td>
<td align="left"><p>매트릭스 변환은 수많은 하위 수준의 3D 그래픽 연산을 처리합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[월드 변환](world-transform.md)</p></td>
<td align="left"><p>월드 변환은 모델 공간에서 좌표를 변경합니다. 여기서 꼭짓점은 모델의 로컬 원점인 월드 공간을 기준으로 정의됩니다. 월드 공간에서 꼭짓점은 장면에 있는 모든 개체에 대해 일반적인 원점을 기준으로 정의됩니다. 월드 변환은 모델을 월드에 배치합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[변환 보기](view-transform.md)</p></td>
<td align="left"><p><em>변환 보기</em>는 월드 공간에서 뷰어를 배치하여 꼭짓점을 카메라 공간으로 변환합니다. 카메라 공간에서 카메라 또는 뷰어는 원점에 있으며 양의 z축 방향을 바라봅니다. 뷰 매트릭스는 카메라의 위치(카메라 공간의 원점) 및 방향 주변에 있는 월드의 개체를 다시 배치합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[프로젝션 변환](projection-transform.md)</p></td>
<td align="left"><p><em>프로젝션 변환</em>은 카메라의 렌즈를 선택하는 등 카메라 내부를 제어합니다. 해당 기능은 세 개의 변환 유형 중에서 가장 복잡합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 




