---
title: 스트리밍 리소스 기능 계층
description: 이전의 타일 리소스 라고 하는 Direct3D streaming resources의 세 가지 기능 계층에 대 한 문서에 액세스 합니다.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- 스트리밍 리소스 기능 계층
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ee27244c4d4c2797b71c9d5c8c2c5185a99596b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173057"
---
# <a name="streaming-resources-features-tiers"></a>스트리밍 리소스 기능 계층


Direct3D는 세 가지 계층의 기능으로 스트리밍 리소스를 지원 합니다.

계층 1은 스트리밍 리소스에 대 한 기본 기능을 제공 합니다.

계층 2는 계층 1 이상의 기능을 추가 합니다. 예를 들어 크기가 하나 이상의 표준 타일 셰이프 일 때 압축 되지 않은 질감 밉 맵을 보장 합니다. 고정 (LOD) 및 셰이더 작업에 대 한 상태 가져오기에 대 한 셰이더 지침 또한 NULL로 매핑된 타일에서 읽으면 샘플링 된 값이 0으로 처리 됩니다.

계층 3에는 계층 2를 벗어난 Texture3D 기능이 추가 됩니다.

쿼리 함수는 Direct3D 버전에서 사용할 수 있으며,이를 통해 스트리밍 리소스에 대 한 하드웨어 및 드라이버 지원의 유효성을 검사 하 고 어떤 계층 수준을 사용할 수 있습니다.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>섹션 항목


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tier-1.md">계층 1</a></p></td>
<td align="left"><p>이 섹션에서는 계층 1 지원에 대해 설명 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">계층 2</a></p></td>
<td align="left"><p>스트리밍 리소스에 대 한 계층 2 지원에는 계층 1 이상의 기능이 추가 됩니다. 예를 들어 크기가 적어도 하나 이상의 표준 타일 셰이프 일 때 압축 되지 않은 질감 밉를 보장 합니다. 고정 (LOD) 및 셰이더 작업에 대 한 상태 가져오기에 대 한 셰이더 지침 또한 NULL로 매핑된 타일에서 읽으면 샘플링 된 값이 0으로 처리 됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">계층 3</a></p></td>
<td align="left"><p>계층 3에는 <a href="tier-2.md">계층 2</a> 기능 외에도 스트리밍 리소스에 대 한 Texture3D에 대 한 지원이 추가 되었습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스](streaming-resources.md)

 

 




