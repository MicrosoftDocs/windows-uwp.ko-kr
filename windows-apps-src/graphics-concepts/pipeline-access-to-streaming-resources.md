---
title: 스트리밍 리소스에 대한 파이프라인 액세스
description: 스트리밍 리소스는 셰이더 리소스 뷰(SRV), 렌더링 대상 뷰(RTV), 깊이 스텐실 뷰(DSV), 순서가 지정되지 않은 액세스 뷰(UAV)뿐 아니라 꼭짓점 버퍼 바인딩 같이 뷰가 사용되지 않는 일부 바인딩 포인트에서 사용할 수 있습니다.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- 스트리밍 리소스에 대한 파이프라인 액세스
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6d95ffc14e9ae6d4ea59a4b3bdc33fd215cb61be
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8218096"
---
# <a name="pipeline-access-to-streaming-resources"></a>스트리밍 리소스에 대한 파이프라인 액세스


스트리밍 리소스는 셰이더 리소스 뷰(SRV), 렌더링 대상 뷰(RTV), 깊이 스텐실 뷰(DSV), 순서가 지정되지 않은 액세스 뷰(UAV)뿐 아니라 꼭짓점 버퍼 바인딩 같이 뷰가 사용되지 않는 일부 바인딩 포인트에서 사용할 수 있습니다. 지원 되는 바인딩의 목록은 [리소스 생성 매개 변수 스트리밍](streaming-resource-creation-parameters.md)을 참조하세요. 다양한 D3D 복사 작업은 스트리밍 리소스에서도 작동합니다.

하나 이상의 뷰의 여러 타일 좌표가 동일한 메모리 위치에 바인딩되는 경우, 다양한 경로에서 동일한 메모리로의 읽기 및 쓰기가 비결정적이고 비반복적인 메모리 액세스 순서로 이루어집니다.

셰이더의 메모리 액세스 공간 뒤의 모든 타일이 고유의 타일에 매핑되는 경우, 비타일 방식으로 동일한 메모리 콘텐츠를 가진 표면에 대한 모든 구현에서 동작은 동일합니다.

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">비매핑된 타일을 사용하는 SRV 동작</a></p></td>
<td align="left"><p>비매핑된 타일과 관련된 셰이더 리소스 뷰(SRV) 읽기 동작은 하드웨어 지원 수준에 따라 다릅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">비매핑된 타일을 사용하는 UAV 동작</a></p></td>
<td align="left"><p>순서가 지정되지 않은 액세스 뷰(UAV) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 다릅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">비매핑된 타일을 사용하는 래스터라이저 동작</a></p></td>
<td align="left"><p>이 섹션에서는 비매핑된 타일을 사용하는 래스터라이저 동작을 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">중복 매핑을 사용하는 타일 액세스 제한</a></p></td>
<td align="left"><p>중복되는 원본과 대상을 이용해 스트리밍 리소스를 복사하거나 렌더링 영역 내에서 공유된 타일로 렌더링할 때처럼 중복 매핑을 사용한 타일 액세스에는 제한이 따릅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">스트리밍 리소스 텍스처 샘플링 기능</a></p></td>
<td align="left"><p>스트리밍 리소스 텍스처 샘플링 기능에는 매핑된 영역에 대한 셰이더 상태 피드백 받기, 액세스되는 모든 데이터가 리소스에서 매핑되었는지 확인, 비매핑으로 알려진 mipmap 스트리밍 리소스의 영역을 셰이더가 피하도록 돕는 고정, 전체 텍스처 필터 공간에 대해 완전히 매핑된 최소 LOD 발견이 포함됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">HLSL 스트리밍 리소스 노출</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff471356">셰이더 모델 5</a>에서 스트리밍 리소스를 지원하려면 특정 Microsoft HLSL(High Level Shader Language) 구문이 필요합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스](streaming-resources.md)

 

 




