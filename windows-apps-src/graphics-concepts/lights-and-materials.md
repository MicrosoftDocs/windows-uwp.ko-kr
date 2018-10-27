---
title: 조명
description: 조명은 장면에서 물체를 비추는 데 사용됩니다. 각 개체 꼭짓점의 색상은 현재 텍스처 맵, 꼭짓점 색상, 광원을 기반으로 합니다.
ms.assetid: AB16CF5B-47CE-455C-A8BD-36305E54BEA9
keywords:
- 조명
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6f690e08d211692b05f0a80722aa4a3e3a06b39f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5682617"
---
# <a name="lighting"></a>조명


조명은 장면에서 물체를 비추는 데 사용됩니다. 각 개체 꼭짓점의 색상은 현재 텍스처 맵, 꼭짓점 색상, 광원을 기반으로 합니다.

**참고**  이 섹션은 고정 함수 파이프라인에만 적용 됩니다. 프로그램 가능 셰이더는 모든 조명 작업을 명시적으로 수행합니다.

 

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
<td align="left"><p><a href="lighting-overview.md">조명 개요</a></p></td>
<td align="left"><p>Direct3D 조명을 사용하는 경우 Direct3D가 조명의 세부 사항을 대신 처리하도록 허용합니다. 고급 사용자는 필요에 따라 직접 조명 작업을 수행할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-types.md">조명 유형</a></p></td>
<td align="left"><p>조명 유형 속성은 어떤 유형의 광원을 사용할지 정의합니다. Direct3D에는 점 광원, 스포트라이트, 방향성 광원 등 세 종류의 조명이 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="light-properties.md">조명 속성</a></p></td>
<td align="left"><p>조명 속성은 광원의 유형(점, 방향성, 스포트라이트), 감쇠, 색상, 방향, 위치, 범위를 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mathematics-of-lighting.md">조명의 수학</a></p></td>
<td align="left"><p>Direct3D 조명 모델은 주변, 확산, 반사 및 발광 조명을 포함합니다. 따라서 광범위한 조명 상황을 해결하기에 충분한 유연성이 있습니다. 한 장면에 비치는 빛의 총량을 <em>전역 조명</em>이라고 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

 

 




