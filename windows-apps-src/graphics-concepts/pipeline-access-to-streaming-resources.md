---
title: 스트리밍 리소스에 대한 파이프라인 액세스
description: 스트리밍 리소스는 셰이더 (셰이더 리소스 뷰), RTV (렌더링 대상 뷰), DSV (깊이 스텐실 뷰) 및 UAV (정렬 되지 않은 액세스 보기)에서 사용할 수 있으며,이는 꼭 짓 점 버퍼 바인딩과 같이 뷰가 사용 되지 않는 일부 바인딩 지점도 사용할 수 있습니다.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- 스트리밍 리소스에 대한 파이프라인 액세스
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 15b37e471e45a1c2ca604c1a5bf28ace69e35ad3
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220326"
---
# <a name="pipeline-access-to-streaming-resources"></a>스트리밍 리소스에 대한 파이프라인 액세스


스트리밍 리소스는 셰이더 (셰이더 리소스 뷰), RTV (렌더링 대상 뷰), DSV (깊이 스텐실 뷰) 및 UAV (정렬 되지 않은 액세스 보기)에서 사용할 수 있으며,이는 꼭 짓 점 버퍼 바인딩과 같이 뷰가 사용 되지 않는 일부 바인딩 지점도 사용할 수 있습니다. 지원 되는 바인딩 목록은 [스트리밍 리소스 생성 매개 변수](streaming-resource-creation-parameters.md)를 참조 하세요. 다양 한 D3D 복사 작업은 스트리밍 리소스 에서도 작동 합니다.

하나 이상의 뷰에서 여러 타일 좌표가 동일한 메모리 위치에 바인딩된 경우 다른 경로에서 동일한 메모리로의 읽기 및 쓰기가 명확 하지 않으며 반복 불가능 한 메모리 액세스 순서로 수행 됩니다.

셰이더의 메모리 액세스 공간 뒤에 있는 모든 타일이 고유 타일로 매핑되는 경우에는 동일한 메모리 내용이 바둑판식으로 표시 되지 않는 표면에 대 한 모든 구현에서 동작이 동일 합니다.

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
<td align="left"><p>매핑되지 않은 타일과 관련 된 SRV (셰이더 리소스 뷰) 읽기 동작은 하드웨어 지원 수준에 따라 달라 집니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">비매핑된 타일을 사용하는 UAV 동작</a></p></td>
<td align="left"><p>UAV (순서가 지정 되지 않은 액세스 뷰) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 달라 집니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">비매핑된 타일을 사용하는 래스터라이저 동작</a></p></td>
<td align="left"><p>이 섹션에서는 매핑되지 않은 타일의 래스터 동작에 대해 설명 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">중복 매핑을 사용하는 타일 액세스 제한</a></p></td>
<td align="left"><p>겹치는 원본 및 대상을 사용 하 여 스트리밍 리소스를 복사 하거나 렌더링 영역 내에서 공유 하는 타일로 렌더링 하는 경우와 같이 중복 매핑이 있는 타일 액세스에는 제한 사항이 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">스트리밍 리소스 텍스처 샘플링 기능</a></p></td>
<td align="left"><p>스트리밍 리소스 질감 샘플링 기능에는 매핑된 영역에 대 한 셰이더 상태 피드백 가져오기, 액세스 하는 모든 데이터가 리소스에 매핑되는지 여부 확인, 셰이더가 매핑되지 않은 mipmapped 스트리밍 리소스의 영역을 사용 하지 않도록 하는 데 도움이 되는 고정, 전체 질감 필터 공간에 대해 완전히 매핑되는 최소 LOD를 검색 하는 작업이 포함 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">HLSL 스트리밍 리소스 노출</a></p></td>
<td align="left"><p><a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5">셰이더 모델 5</a>에서 스트리밍 리소스를 지원 하려면 특정 Microsoft HLSL (High Level Shader Language) 구문이 필요 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스](streaming-resources.md)

 

 