---
title: 좌표계 및 기하 도형
description: Direct3D 응용 프로그램을 프로그래밍하려면 3D 기하 도형 원칙 작업에 익숙해야 합니다. 이 섹션에서는 3D 장면을 만드는 데 필요한 가장 중요한 기하 도형 개념을 소개합니다.
ms.assetid: E82EB0A9-0678-496B-96B3-8993BA580099
keywords:
- 좌표계 및 기하 도형
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 962002d635c3e6edbf1f9581a4cbc57fbd5b1d96
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8730113"
---
# <a name="coordinate-systems-and-geometry"></a>좌표계 및 기하 도형


Direct3D 응용 프로그램을 프로그래밍하려면 3D 기하 도형 원칙 작업에 익숙해야 합니다. 이 섹션에서는 3D 장면을 만드는 데 필요한 가장 중요한 기하 도형 개념을 소개합니다.

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
<td align="left"><p><a href="coordinate-systems.md">좌표계</a></p></td>
<td align="left"><p>일반적으로 3D 그래픽 응용 프로그램은 두 가지 유형의 카티전 좌표계(왼손 또는 오른손) 중 하나를 사용합니다. 두 좌표계에서 양의 x-축은 오른쪽을 가리키고 양의 y-축은 위를 가리킵니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="primitives.md">기본 요소</a></p></td>
<td align="left"><p>3D <em>원형</em>은 단일 3D 엔터티를 구성하는 꼭짓점의 모음입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="face-and-vertex-normal-vectors.md">면 및 꼭짓점 법선 벡터</a></p></td>
<td align="left"><p>메시의 각 면에는 수직 단위 법선 벡터가 있습니다. 벡터의 방향은 꼭짓점이 정의된 순서와 좌표계가 오른손용인지 왼손용인지에 따라 결정됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rectangles.md">사각형</a></p></td>
<td align="left"><p>Direct3D 및 Windows 프로그래밍 전체에서 경계 사각형을 사용하여 화면의 개체를 참조합니다. 경계 사각형의 면은 항상 화면의 면에 평행이므로 왼쪽 위 모퉁이와 오른쪽 아래 모퉁이, 이렇게 두 점으로 사각형을 설명할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-interpolation.md">삼각형 보간</a></p></td>
<td align="left"><p>렌더링 중에 파이프라인은 각 삼각형에서 꼭짓점 데이터를 보간합니다. 꼭짓점 데이터는 광범위한 데이터로써 확산 색, 반사 색, 확산 알파(삼각형 불투명도), 반사 알파 및 포그 인수를 포함할 수 있습니다(이에 국한되지 않음).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vectors--vertices--and-quaternions.md">벡터, 꼭짓점 및 사원수</a></p></td>
<td align="left"><p>Direct3D 전체에서 꼭짓점은 위치 및 방향을 설명합니다. 기본 형식의 각 꼭짓점은 위치, 색, 텍스처 좌표 및 방향을 제공하는 일반적인 벡터로 설명됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="transforms.md">변환</a></p></td>
<td align="left"><p>Direct3D 파트는 고정 함수 기하 도형 파이프라인을 통해 기하 도형을 푸시하는 변환 엔진입니다. 세계 좌표의 모델 및 뷰어를 찾아내고, 표시할 꼭짓점을 화면에 투사하고, 뷰포트에 대한 꼭짓점을 잘라냅니다. 또한 변환 엔진은 조명 계산을 수행하여 각 꼭짓점의 확산 및 반사 구성 요소를 결정합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="viewports-and-clipping.md">뷰포트 및 클리핑</a></p></td>
<td align="left"><p><em>뷰포트</em>는 3D 화면이 프로젝션되는 2차원(2D) 사각형입니다. Direct3D에서 사각형은 시스템이 렌더링 대상으로 사용하는 Direct3D 표면 내의 좌표로 존재합니다. 프로젝션 변환은 꼭짓점을 뷰포트에 사용되는 좌표계로 변환합니다. 뷰포트는 장면이 렌더링될 렌더링 대상 표면의 깊이 값 범위를 지정하는 데에도 사용됩니다(일반적으로 0.0~1.0).</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

 

 




