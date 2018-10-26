---
title: 꼭짓점 및 인덱스 버퍼
description: 꼭짓점 버퍼는 꼭짓점 데이터를 포함한 메모리 버퍼입니다. 꼭짓점 버퍼의 꼭짓점을 처리하여 변환, 조명 및 클리핑을 수행합니다.
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- 꼭짓점 및 인덱스 버퍼
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2327036eb53ac34c406aef53163be642468fbddc
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5597135"
---
# <a name="vertex-and-index-buffers"></a>꼭짓점 및 인덱스 버퍼


*꼭짓점 버퍼*는 꼭짓점 데이터를 포함한 메모리 버퍼입니다. 꼭짓점 버퍼의 꼭짓점을 처리하여 변환, 조명 및 클리핑을 수행합니다. *인덱스 버퍼*는 인덱스 데이터를 포함하는 메모리 버퍼로써 기본 형식을 렌더링하는 데 사용되는 꼭짓점 버퍼에 대한 정수 오프셋입니다.

꼭짓점 버퍼는 변환 유무, 조명 유무 등에 관계없이 렌더링이 가능한 모든 꼭짓점 형식을 포함할 수 있습니다. 변환, 조명 또는 클리핑 플래그 생성 등의 작업을 수행하는 꼭짓점 버퍼에서 꼭짓점을 처리할 수 있습니다. 변환은 항상 수행됩니다.

꼭짓점 버퍼는 유연하기 때문에 변환된 기하 도형의 재사용을 준비하기 위한 가장 이상적인 준비 지점입니다. 꼭짓점 버퍼를 하나 만들고 그 안에서 꼭짓점에 대한 변환, 조명 및 클리핑을 수행할 수 있으며, 인터리브 렌더링 상태가 변하더라도 재변환 없이 필요한 만큼 장면의 모델을 렌더링할 수 있습니다. 이 기능은 여러 텍스처를 사용하는 모델을 렌더링할 때 유용합니다. 기하 도형을 한 번만 변환하면 필요에 따라 그 일부를 렌더링하고 필요한 텍스처 변경 사항을 인터리브할 수 있습니다. 꼭짓점을 처리한 후에 발생한 렌더링 상태 변경 사항은 다음에 꼭짓점을 처리할 때 적용됩니다.

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
<td align="left"><p><a href="introduction-to-buffers.md">버퍼 소개</a></p></td>
<td align="left"><p>버퍼 리소스는 완벽하게 입력된 데이터 컬렉션이며, 요소로 그룹화됩니다. 버퍼는 <em>꼭짓점 버퍼</em>의 텍스처 좌표, <em>인덱스 버퍼</em>의 인덱스, <em>상수 버퍼</em>의 셰이더 상수 데이터, 위치 벡터, 일반 벡터, 디바이스 상태 등의 데이터를 저장합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="index-buffers.md">인덱스 버퍼</a></p></td>
<td align="left"><p><em>인덱스 버퍼</em>는 인덱스 데이터를 포함하는 메모리 버퍼로써 기본 요소를 렌더링하는 데 사용되는 꼭짓점 버퍼에 대한 정수 오프셋입니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

 

 




