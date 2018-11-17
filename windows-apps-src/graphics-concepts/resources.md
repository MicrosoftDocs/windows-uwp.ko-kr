---
title: 리소스
description: 리소스는 Direct3D 파이프라인에서 액세스할 수 있는 메모리 내 영역입니다.
ms.assetid: 2E68E5A8-83DA-4DC8-B7F3-B8988CF8090C
keywords:
- 리소스
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a56b03de29e110a2768ebe71f4e61d8099ca1cf8
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7173596"
---
# <a name="resources"></a>리소스


리소스는 Direct3D 파이프라인에서 액세스할 수 있는 메모리 내 영역입니다. 파이프라인에서 메모리에 효율적으로 액세스하려면 파이프라인에 제공된 데이터(입력 기하 도형, 셰이더 리소스, 텍스처 등)를 리소스에 저장해야 합니다. 모든 Direct3D 리소스가 파생되는 리소스 종류는 버퍼와 텍스처, 두 가지입니다. 각 파이프라인 단계에 최대 128개 리소스를 활성화할 수 있습니다.

일반적으로 각 응용 프로그램은 여러 개의 리소스를 만듭니다. 리소스의 예: 버텍스 버퍼, 인덱스 버퍼, 상수 버퍼, 텍스처, 셰이더 리소스. 리소스를 사용할 수 있는 방법을 결정하는 다음과 같은 몇 가지 옵션이 있습니다. 강력한 형식이거나 형식이 없는 리소스를 만들 수 있습니다. 리소스의 읽기 및 쓰기 액세스가 모두 가능하도록 할지 여부를 제어할 수 있습니다. CPU나 GPU만 또는 CPU와 GPU 모두 리소스에 액세스할 수 있도록 할 수 있습니다. 당연히 속도와 기능의 상충 관계가 생깁니다. 리소스에 허용하는 기능이 많아질수록 성능은 떨어진다고 예상해야 합니다.

응용 프로그램은 많은 텍스처를 사용하는 경우가 많으므로 Direct3D에도 텍스처 관리를 단순화하기 위한 텍스처 배열 개념이 있습니다. 텍스처 배열에는 응용 프로그램 내에서 또는 셰이더에 의해 인덱싱될 수 있는 하나 이상의 텍스처(모두 동일한 유형 및 차원)가 포함됩니다. 텍스처 배열을 통해 여러 개의 인덱스가 있는 단일 인터페이스를 사용하여 많은 텍스처에 액세스할 수 있습니다. 필요한 만큼 많은 텍스처 배열을 만들어 다양한 텍스처 유형을 관리할 수 있습니다.

응용 프로그램이 사용할 리소스를 만든 후에 리소스를 사용할 파이프라인 단계에 각 리소스를 연결하거나 바인딩합니다. 이 작업은 포인터를 리소스로 가져가는 bind API를 호출하여 수행됩니다. 둘 이상의 파이프라인 단계가 동일한 리소스에 액세스해야 할 수 있으므로 Direct3D에는 리소스 뷰 개념이 있습니다. 뷰는 액세스할 수 있는 리소스 부분을 식별합니다. 공유 리소스의 바인딩 규칙을 따른다고 가정하면 *m* 뷰 또는 리소스를 만들어 *n* 파이프라인에 바인딩할 수 있습니다(규칙에 따르지 않으면 컴파일 시간에 런타임이 오류를 생성합니다).

리소스 뷰는 리소스(예: 텍스처 또는 버퍼)에 액세스하기 위한 일반 모델을 제공합니다. 뷰를 사용하여 액세스할 데이터와 액세스 방법을 알 수 있으므로 리소스 뷰로 형식이 없는 리소스를 만들 수 있습니다. 즉, 컴파일 시간에 주어진 크기에 대한 리소스를 만든 다음 리소스가 파이프라인에 바인딩될 때 리소스 내에서 데이터 형식을 선언할 수 있습니다. 뷰는 셰이더에서 깊이/스텐실 표면을 다시 읽는 기능, 단일 패스에서 동적 큐브 맵 생성, 볼륨의 여러 슬라이스에 동시에 렌더링 등 리소스 사용을 위한 여러 기능을 노출합니다.

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
<td align="left"><p><a href="resource-types.md">리소스 종류</a></p></td>
<td align="left"><p>다양한 리소스 종류는 고유한 레이아웃(또는 메모리 공간)을 가지고 있습니다. Direct3D 파이프라인이 사용하는 모든 리소스는 <a href="resource-types.md#buffer-resources">버퍼</a>와 <a href="resource-types.md#texture-resources">텍스처</a>라는 두 가지 기본 리소스 종류에서 파생됩니다. 버퍼는 원시 데이터(요소)의 모음이며, 텍스처는 텍셀(텍스처 요소)의 모음입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="choosing-a-resource.md">리소스 선택</a></p></td>
<td align="left"><p>리소스는 3D 파이프라인이 사용하는 데이터의 모음입니다. 리소스를 만들고 리소스의 동작을 정의하는 것이 응용 프로그램 프로그래밍의 첫 단계입니다. 이 가이드에서는 응용 프로그램에 필요한 리소스를 선택하기 위한 기본적 항목을 다룹니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="copying-and-accessing-resource-data.md">리소스 데이터 복사 및 액세스</a></p></td>
<td align="left"><p>사용 플래그는 성능이 가장 좋은 메모리 영역에 리소스를 배치할 수 있도록 응용 프로그램에서 리소스 데이터를 사용하는 방법을 나타냅니다. 성능에 영향을 주지 않고 CPU나 GPU에서 액세스할 수 있도록 리소스 데이터가 전체 리소스에 복사됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-views.md">텍스처 뷰</a></p></td>
<td align="left"><p>Direct3D에서 텍스처 리소스는 메모리 내 리소스의 하드웨어 해석 메커니즘인 뷰를 사용하여 액세스됩니다. 뷰를 사용하면 특정 파이프라인 단계가 응용 프로그램이 원하는 표현으로 필요한 <a href="resource-types.md">하위 리소스</a>에만 액세스할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계](coordinate-systems.md)

[Direct3D 그래픽 학습 가이드](index.md)

[부동 소수점 규칙](floating-point-rules.md)

[데이터 형식 변환](data-type-conversion.md)
