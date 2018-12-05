---
title: 스트리밍 리소스 영역의 타일링 방법
description: 스트리밍 리소스를 만들 때는 차원, 형식 요소 크기, Mipmap 및/또는 배열 슬라이스(해당하는 경우)의 개수가 전체 표면 영역을 덮는 데 필요한 타일의 수를 결정합니다.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- 스트리밍 리소스 영역의 타일링 방법
- 리소스 영역, 타일링
- 크기 표, 리소스, 타일링
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7251d4595a3e87a8629d6e717bb4f52e5b7c35fe
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8739835"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>스트리밍 리소스 영역의 타일링 방법


스트리밍 리소스를 만들 때는 차원, 형식 요소 크기, Mipmap 및/또는 배열 슬라이스(해당하는 경우)의 개수가 전체 표면 영역을 덮는 데 필요한 타일의 수를 결정합니다. 타일 내의 픽셀/바이트 레이아웃은 구현에 의해 결정됩니다. 타일에 맞는 픽셀의 수는 표준 swizzle을 사용하는지에 관계없이 형식 요소 크기에 따라 고정되고 동일합니다.

지정된 표면 크기에 사용되는 타일의 수와 형식 요소의 너비는 잘 정의되어 있고 다음 섹션의 표를 기반으로 예측 가능합니다. Mipmap이 포함된 리소스나 표면 차원이 타일을 채우지 않는 경우, 일부 제약이 있습니다. [Mipmap 압축](mipmap-packing.md)을 참조하세요.

응용 프로그램은 메모리에 한 형식으로 쓰고 다른 형식으로 읽는 결과를 인지할 수 없으므로, 다른 스트리밍 리소스가 다른 형식의 동일한 메모리를 가리킬 수 있습니다. 하지만 형식이 동일한 형식 패밀리(즉 형식이 없는 동일한 상위 형식을 가진 경우)에 속한 하나의 형식을 사용하여 메모리에 쓰고 같은 패밀리의 다른 형식으로 읽는 것에 대한 결과는 응용 프로그램이 인지할 수 있습니다. 예를 들어, DXGI\_FORMAT\_R8G8B8A8\_UNORM과 DXGI\_FORMAT\_R8G8B8A8\_UINT은 서로 호환되지만 DXGI\_FORMAT\_R16G16\_UNORM과는 호환되지 않습니다.

하지만 한 형식 별칭에서 다른 형식으로의 데이터 변환이 잘 정의되어 있는 경우는 예외입니다. 타일의 모든 비트가 0으로 완전히 채워진 경우, 메모리 콘텐츠를 0으로 해석하는 모든 형식에 메모리 레이아웃에 관계없이 이 타일을 사용할 수 있습니다. 따라서 타일이 DXGI\_FORMAT\_R8\_UNORM 형식을 통해 0x00으로 지우면 DXGI\_FORMAT\_R32G32\_FLOAT과 같은 형식으로 사용할 수 있으며 내용은 여전히 (0.0f,0.0f)로 표시됩니다.

타일 내의 데이터 레이아웃은 타일이 리소스에 전체적으로 매핑된 위치에 따라 달라지지 않습니다. 따라서 예를 들어, 타일이 모든 위치에서 일관되게 동작한다면 표면의 다른 위치에서 다시 사용할 수 있습니다.

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
<td align="left"><p>다음은 <a href="https://msdn.microsoft.com/library/windows/desktop/ff471525"><strong>Texture2D</strong></a> 및 <a href="https://msdn.microsoft.com/library/windows/desktop/ff471526"><strong>Texture2DArray</strong></a> 하위 리소스 타일 방식을 나타낸 표입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Texture3D 하위 리소스 타일링</a></p></td>
<td align="left"><p>다음은 <a href="https://msdn.microsoft.com/library/windows/desktop/ff471562"><strong>Texture3D</strong></a> 하위 리소스 타일 방식을 나타낸 표입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">버퍼 타일링</a></p></td>
<td align="left"><p><a href="introduction-to-buffers.md">버퍼</a> 리소스는 64KB의 크기로 분할되며, 크기가 64KB의 곱이 아닌 경우 마지막 타일에 약간의 빈 공간이 포함됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">Mipmap 압축</a></p></td>
<td align="left"><p>mips 중 일부(배열 슬라이스당)는 스트리밍 리소스의 크기, 형식, Mipmap의 개수 및 배열 슬라이스에 따라 일부 타일로 압축할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 만들기](creating-streaming-resources.md)

 

 




