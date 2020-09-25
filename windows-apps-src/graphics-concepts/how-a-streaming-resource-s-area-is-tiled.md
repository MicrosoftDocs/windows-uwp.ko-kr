---
title: 스트리밍 리소스 영역의 타일링 방법
description: 스트리밍 리소스를 만들 때 차원, 서식 요소 크기 및 mip 맵을 및/또는 배열 조각 수 (해당 하는 경우)는 전체 노출 영역을 백업 하는 데 필요한 타일 수를 결정 합니다.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- 스트리밍 리소스 영역의 타일링 방법
- 리소스 영역, 바둑판식 배열
- 크기 표, 리소스, 바둑판식 배열
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fae56f673aa3d952b7e85490ec79676c2ac6a48a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220346"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>스트리밍 리소스 영역의 타일링 방법


스트리밍 리소스를 만들 때 차원, 서식 요소 크기 및 mip 맵을 및/또는 배열 조각 수 (해당 하는 경우)는 전체 노출 영역을 백업 하는 데 필요한 타일 수를 결정 합니다. 타일 내의 픽셀/바이트 레이아웃은 구현에 의해 결정 됩니다. 서식 요소 크기에 따라 타일에 들어가는 픽셀 수는 고정 되어 있으며 표준 swizzle 사용 여부에 관계 없이 동일 합니다.

지정 된 표면 크기 및 서식 요소 너비에서 사용할 타일의 수는 다음 섹션의 테이블을 기반으로 잘 정의 되 고 예측할 수 있습니다. Mip 맵을 또는 표면 차원이 타일을 채우지 않는 사례를 포함 하는 리소스의 경우 일부 제약 조건이 있습니다. [밉 맵 압축](mipmap-packing.md)을 참조 하세요.

응용 프로그램이 한 형식의 메모리에 쓰고 다른 형식으로 읽는 결과에 의존 하지 않는 한 다른 스트리밍 리소스는 서로 다른 형식의 동일한 메모리를 가리킬 수 있습니다. 그러나 응용 프로그램은 한 형식으로 메모리에 쓴 결과를 사용 하 고 형식이 동일한 형식 패밀리에 있는 경우 다른 형식으로 읽을 수 있습니다 (즉, 형식이 같은 부모 형식이 동일). 예를 들어 DXGI \_ format \_ R8G8B8A8 \_ UNORM 및 dxgi \_ format \_ R8G8B8A8 \_ UINT는 서로 호환 되지만 dxgi \_ 형식 \_ R16G16 unorm에서는 호환 되지 않습니다 \_ .

단, 한 형식에서 다른 형식으로의 bleeding 데이터가 잘 정의 된 경우는 예외입니다. 타일에 모든 비트에 대해 0이 완전히 포함 되 면 메모리 레이아웃에 관계 없이 해당 타일을 0으로 해석 하는 모든 형식으로 사용할 수 있습니다. 따라서 타일은 DXGI 형식 R8이 아닌 형식으로 0x00으로 지운 \_ \_ 다음, \_ dxgi 형식 R32G32 FLOAT와 같은 형식으로 사용할 수 \_ \_ 있으며, \_ 콘텐츠가 여전히 (0.0 f, 0.0 f)로 표시 됩니다.

타일 내 데이터의 레이아웃은 전체 리소스에서 타일이 매핑되는 위치에 따라 달라 지지 않습니다. 따라서 예를 들어 모든 위치에서 일관 된 동작을 사용 하 여 한 번에 한 표면의 여러 위치에서 타일을 재사용할 수 있습니다.

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
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Texture2D 및 Texture2DArray 하위 리소스 타일링</a></p></td>
<td align="left"><p>다음 표에서는 <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a> 및 <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a> 하위를 바둑판식으로 배열 하는 방법을 보여 줍니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Texture3D 하위 리소스 타일링</a></p></td>
<td align="left"><p>다음 표에서는 <a href="/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a> 하위를 바둑판식으로 배열 하는 방법을 보여 줍니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">버퍼 타일링</a></p></td>
<td align="left"><p><a href="introduction-to-buffers.md">버퍼</a> 리소스는 64kb 타일로 나뉘어 크기가 64kb의 배수가 아닌 경우 마지막 타일에 몇 개의 빈 공간을 포함 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">Mipmap 압축</a></p></td>
<td align="left"><p>일부 mips (배열 조각 당) 수는 스트리밍 리소스의 차원, 형식, mip 맵을 수 및 배열 조각에 따라 몇 개의 타일로 압축 될 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 만들기](creating-streaming-resources.md)

 

 