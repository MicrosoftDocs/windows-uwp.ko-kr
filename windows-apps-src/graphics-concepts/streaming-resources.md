---
title: 스트리밍 리소스
description: 스트리밍 리소스는 소량의 실제 메모리를 사용하는 대용량 논리 리소스입니다. 전체 대용량 리소스를 전달하는 대신, 리소스의 일부가 필요에 따라 스트리밍됩니다. 이전에는 스트리밍 리소스를 타일식 리소스라고 했습니다.
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- 스트리밍 리소스
- 리소스, 스트리밍
- 리소스, 타일식
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c15c8a82219109a96d0a9ca192c4dfff5d86c9aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598238"
---
# <a name="streaming-resources"></a>스트리밍 리소스


*스트리밍 리소스*는 소량의 실제 메모리를 사용하는 대용량 논리 리소스입니다. 전체 큰 리소스를 전달하는 대신 리소스의 작은 부분이 필요에 따라 스트리밍됩니다. 이전에는 스트리밍 리소스를 *타일식 리소스*라고 했습니다.

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
<td align="left"><p><a href="the-need-for-streaming-resources.md">리소스를 스트리밍에 대 한 필요성</a></p></td>
<td align="left"><p>스트리밍 리소스는 접근하지 못하는 표면 지역을 저장하는 데 GPU 메모리를 낭비하지 않고, 하드웨어에게 인접 타일에 대한 필터링 방식을 지정하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="creating-streaming-resources.md">스트리밍 리소스 만들기</a></p></td>
<td align="left"><p>스트리밍 리소스는 리소스를 만들 때 해당 리소스가 스트리밍 리소스임을 나타내는 플래그를 지정하여 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pipeline-access-to-streaming-resources.md">리소스를 스트리밍에 대 한 파이프라인 액세스</a></p></td>
<td align="left"><p>스트리밍 리소스는 셰이더 리소스 뷰(SRV), 렌더링 대상 뷰(RTV), 깊이 스텐실 뷰(DSV), 순서가 지정되지 않은 액세스 뷰(UAV)뿐 아니라 꼭짓점 버퍼 바인딩 같이 뷰가 사용되지 않는 일부 바인딩 포인트에서 사용할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resources-features-tiers.md">리소스 기능 계층을 스트리밍</a></p></td>
<td align="left"><p>Direct3D는 세 개의 기능 계층에서 스트리밍 리소스를 지원합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[Direct3D 그래픽 학습 가이드](index.md)

[리소스](resources.md)

 

 




