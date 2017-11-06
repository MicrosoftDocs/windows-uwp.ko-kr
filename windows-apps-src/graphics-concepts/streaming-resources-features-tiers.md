---
title: "스트리밍 리소스 기능 계층"
description: "Direct3D는 세 개의 기능 계층에서 스트리밍 리소스를 지원합니다."
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords: "스트리밍 리소스 기능 계층"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 671c679938860057a5fe8f083eebfe47dc518e05
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="streaming-resources-features-tiers"></a>스트리밍 리소스 기능 계층


Direct3D는 세 개의 기능 계층에서 스트리밍 리소스를 지원합니다.

계층 1은 스트리밍 리소스에 대한 기본 기능을 제공합니다.

계층 2는 계층 1의 기본 기능 이외에 크기가 하나 이상의 표준 타일 형태일 경우 비압축 텍스처 mipmap 보장, LOD(level-of-detail) 고정 및 셰이더 작업 상태 획득을 위한 셰이더 명령, NULL 매핑된 타일에서 읽은 값이 샘플링된 값을 0으로 처리와 같은 기능을 추가로 제공합니다.

계층 3은 계층 2 기능 이외에 Texture3D 기능을 추가로 제공합니다.

스트리밍 리소스에 대한 하드웨어 및 드라이버 지원 및 계층 수준을 확인할 수 있도록 Direct3D의 버전에서 쿼리 함수를 제공합니다.

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
<td align="left"><p>[계층 1](tier-1.md)</p></td>
<td align="left"><p>이 섹션에서는 계층 1 지원에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[계층 2](tier-2.md)</p></td>
<td align="left"><p>스트리밍 리소스에 대한 계층 2 지원은 계층 1의 기본 기능 이외에 크기가 하나 이상의 표준 타일 형태일 경우 비압축 텍스처 mipmap 보장, LOD(level-of-detail) 고정 및 셰이더 작업 상태 획득을 위한 셰이더 명령, NULL 매핑된 타일에서 읽은 값이 샘플링된 값을 0으로 처리와 같은 기능을 추가로 제공합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[계층 3](tier-3.md)</p></td>
<td align="left"><p>계층 3은 [계층 2](tier-2.md) 기능 이외에 스트리밍 리소스에 대한 Texture3D를 지원합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스](streaming-resources.md)

 

 



