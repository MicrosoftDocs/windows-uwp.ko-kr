---
title: "기본 요소"
description: "3D 기본 요소는 단일한 3D 엔터티를 형성하는 꼭짓점의 모음입니다."
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords:
- "기본 요소"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e6734ff8534302d3866374adba45c34d70ae440a
ms.lasthandoff: 02/07/2017

---

# <a name="primitives"></a>기본 요소


3D *기본 요소*는 단일한 3D 엔터티를 형성하는 꼭짓점의 모음입니다.

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
<td align="left"><p>[점 목록](point-lists.md)</p></td>
<td align="left"><p>점 목록은 격리된 점으로 렌더링되는 꼭짓점의 모음입니다. 응용 프로그램은 스타 필드용 3D 장면에서 점 목록을 사용하거나 다각형 표면에서 점선을 사용할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[선 목록](line-lists.md)</p></td>
<td align="left"><p>선 목록은 격리된 선 세그먼트의 목록입니다. 선 목록은 진눈깨비나 폭우를 3D 장면에 추가하는 등의 작업에 유용합니다. 응용 프로그램은 꼭짓점 배열을 채워 선 목록을 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[선 스트립](line-strips.md)</p></td>
<td align="left"><p>선 스트립은 연결된 선 세그먼트로 구성되는 기본 요소입니다. 응용 프로그램은 닫히지 않은 다각형을 만드는 데 선 스트립을 사용할 수 있습니다. 닫힌 다각형은 마지막 꼭짓점이 선 세그먼트에 의해 첫 번째 꼭짓점에 연결되는 다각형입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[삼각형 목록](triangle-lists.md)</p></td>
<td align="left"><p>삼각형 목록은 격리된 삼각형의 목록입니다. 격리된 삼각형은 서로 가까울 수도 있고 가깝지 않을 수도 있습니다. 삼각형 목록에는 3개 이상의 꼭짓점이 있어야 하고 꼭짓점의 총수는 3의 배수여야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[삼각형 스트립](triangle-strips.md)</p></td>
<td align="left"><p>삼각형 스트립은 일련의 연결된 삼각형입니다. 삼각형이 연결되었기 때문에 응용 프로그램에서 각 삼각형의 세 꼭짓점을 반복해서 지정할 필요가 없습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 





