---
title: 스트리밍 리소스 만들기
description: 스트리밍 리소스는 리소스를 만들 때 해당 리소스가 스트리밍 리소스임을 나타내는 플래그를 지정하여 만듭니다.
ms.assetid: B3F3E43C-54D4-458C-9E16-E13CB382C83F
keywords:
- 스트리밍 리소스 만들기
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec96f6245969d32357563c44107f539fb9043aac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618248"
---
# <a name="creating-streaming-resources"></a>스트리밍 리소스 만들기


스트리밍 리소스는 리소스를 만들 때 해당 리소스가 스트리밍 리소스임을 나타내는 플래그를 지정하여 만듭니다.

스트리밍 리소스로 리소스를 만들 수 있는 경우의 제한 사항은 [스트리밍 리소스 생성 매개 변수](streaming-resource-creation-parameters.md)에 설명되어 있습니다.

2D 텍스처의 배열에 대한 할당과 같이 리소스 생성 시 그래픽 시스템에 비 스트리밍 리소스의 저장소가 할당됩니다.

스트리밍 리소스를 만들 때 그래픽 시스템에서 리소스 콘텐츠에 대한 저장소를 할당하지 않습니다. 대신 응용 프로그램에서 스트리밍 리소스를 만들 때 그래픽 시스템에서 바둑판식 표면의 영역에 대해서만 주소 공간 예약을 만든 다음 응용 프로그램에서 타일의 매핑을 제어할 수 있도록 허용합니다. 타일의 "매핑"은 단순히 리소스의 논리적 타일이 가리키는 메모리의 물리적 위치입니다(매핑되지 않은 타일의 경우 **NULL**).

이 개념을 CPU 액세스를 위해 Direct3D 리소스를 매핑하는 개념과 혼동하면 안 됩니다. 이름은 같지만 서로 완전히 다른 개념입니다. 표면에 대한 모든 타일을 한 번에 매핑할 필요는 없으므로 필요에 따라 각 타일의 매핑을 개별적으로 정의하고 변경하여 사용 가능한 메모리를 효과적으로 사용할 수 있습니다.

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
<td align="left"><p><a href="mappings-are-into-a-tile-pool.md">매핑은 타일 풀으로</a></p></td>
<td align="left"><p>리소스를 스트리밍 리소스로 만들 때 리소스를 구성하는 타일은 타일 풀의 위치를 가리키는 것에서 시작합니다. 타일 풀은 메모리 풀로서 응용 프로그램에 보이지 않는 장면 뒤에서 하나 이상의 할당으로 지원됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-creation-parameters.md">스트리밍 리소스 생성 매개 변수</a></p></td>
<td align="left"><p>스트리밍 리소스로 만들 수 있는 Direct3D 리소스의 유형에는 몇 가지 제약이 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tile-pool-creation-parameters.md">타일 풀 생성 매개 변수</a></p></td>
<td align="left"><p>버퍼를 만들 때 이 섹션의 매개 변수를 사용하여 타일 풀을 정의합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-cross-process-and-device-sharing.md">스트리밍 프로세스 간 리소스 및 공유 장치</a></p></td>
<td align="left"><p>타일 풀은 일반적인 리소스와 마찬가지로 다른 프로세스와 공유할 수 있습니다. 타일 풀을 참조하는 스트리밍 리소스는 장치 및 프로세스 사이에서 공유할 수 없습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="operations-available-on-streaming-resources.md">리소스를 스트리밍에 사용할 수 있는 작업</a></p></td>
<td align="left"><p>이 섹션에서는 스트리밍 리소스에 대해 수행할 수 있는 작업이 나와 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="operations-available-on-tile-pools.md">타일 풀에서 사용 가능한 작업</a></p></td>
<td align="left"><p>타일 풀에 대한 작업에는 타일 풀 크기 조정, 리소스 제공(시스템에 전체 타일 풀에 대한 메모리 임시 생성) 및 리소스 회수가 포함됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="how-a-streaming-resource-s-area-is-tiled.md">리소스의 영역을 바둑판식으로 스트리밍 하는 방법</a></p></td>
<td align="left"><p>스트리밍 리소스를 만들 때는 차원, 형식 요소 크기, MIP 맵 및/또는 배열 슬라이스(해당하는 경우)의 개수가 전체 표면 영역을 덮는 데 필요한 타일의 수를 결정합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[스트리밍 리소스](streaming-resources.md)

 

 




