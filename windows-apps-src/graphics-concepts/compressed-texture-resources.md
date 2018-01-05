---
title: "압축된 텍스처 리소스"
description: "텍스처 맵은 더 선명하게 표시되도록 3차원 셰이프에 그려지는 디지털화된 이미지입니다."
ms.assetid: 2DD5FF94-A029-4694-B103-26946C8DFBC1
keywords: "압축된 텍스처 리소스"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 79bb9d7ea52dbb34645db3284836b73b26a416a3
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="compressed-texture-resources"></a>압축된 텍스처 리소스


텍스처 맵은 더 선명하게 표시되도록 3차원 셰이프에 그려지는 디지털화된 이미지입니다. 래스터화 중 이러한 셰이프에 텍스처 맵이 매핑되고 프로세스에서 시스템 버스와 메모리를 많이 사용할 수 있습니다. 텍스처에서 사용하는 메모리 양을 줄이기 위해 Direct3D에서 텍스처 표면의 압축을 지원합니다. 일부 Some Direct3D 장치는 기본적으로 압축된 텍스처 표면을 지원합니다. 이러한 장치에서 압축된 표면을 만들고 여기에 데이터를 로드하면 표면을 Direct3D에서 다른 텍스처 표면처럼 사용할 수 있습니다. Direct3D는 3D 개체에 텍스처 매핑 시 압축 풀기를 처리합니다.

## <a name="span-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanstorage-efficiency-and-texture-compression"></a><span id="Storage-Efficiency-and-Texture-Compression"></span><span id="storage-efficiency-and-texture-compression"></span><span id="STORAGE-EFFICIENCY-AND-TEXTURE-COMPRESSION"></span>저장소 효율성 및 텍스처 압축


모든 텍스처 압축 형식은 2의 제곱입니다. 그렇다고 텍스처가 반드시 사각형이어야 하는 것은 아니지만 x와 y 둘 다 2의 제곱입니다. 예를 들어, 텍스처가 512 x 128바이트인 경우 다음 MIP 매핑은 256 x 64입니다(각 단계가 2의 제곱씩 감소). 텍스처가 16 x 2와 8 x 1로 필터링되는 하위 수준에서는 압축 블록이 항상 4 x 4의 텍셀 블록이기 때문에 불필요하게 사용되는 비트가 있습니다. 블록의 미사용 부분은 패딩됩니다.

최저 수준에서 불필요하게 사용되는 비트가 있어도 전체적으로는 확실히 이익입니다. 이론상 최악의 경우는 2000 x 1 텍스처(2⁰ 제곱)입니다. 여기에서 블록당 하나의 픽셀 행만 인코딩되고 나머지 블록은 사용되지 않습니다.

## <a name="span-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanmixing-formats-within-a-single-texture"></a><span id="Mixing-Formats-Within-a-Single-Texture"></span><span id="mixing-formats-within-a-single-texture"></span><span id="MIXING-FORMATS-WITHIN-A-SINGLE-TEXTURE"></span>단일 텍스처 내 형식 혼합


단일 텍스처는 해당 데이터가 16개 텍셀 그룹당 64비트나 128비트로 저장되도록 지정해야 한다는 점에 유의해야 합니다. 64비트 블록(블록 압축 형식 BC1)이 텍스처에 사용되는 경우 같은 텍스처 내에 블록별로 불투명 및 1비트 알파 형식을 혼합하는 것이 가능합니다. 즉, 각각의 16개 텍셀 블록에 대해 color\_0 및 color\_1의 부호 없는 정수 크기 비교가 수행됩니다.

128비트 블록이 사용되면 전체 텍스처에 대해 명시적(형식 BC2) 또는 보간 모드(형식 BC3)에 알파 채널을 지정해야 합니다. 색과 마찬가지로 보간된 모드(형식 BC3)를 선택하면 8개의 보간된 알파나 6개의 보간된 알파 모드를 블록 단위로 사용할 수 있습니다. 다시 alpha\_0과 alpha\_1의 크기 비교가 블록 단위로 고유하게 수행됩니다.

Direct3D는 3D 모델 텍스처링에 사용되는 표면을 압축하는 서비스를 제공합니다. 이 섹션에서는 압축된 텍스처 표면에서 데이터를 만들고 조작하는 방법에 대한 정보를 제공합니다.

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
<td align="left"><p>[불투명 및 1비트 알파 텍스처](opaque-and-1-bit-alpha-textures.md)</p></td>
<td align="left"><p>텍스처 형식 BC1은 불투명 텍스처나 단일 투명 색의 텍스처용입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[알파 채널을 사용하는 텍스처](textures-with-alpha-channels.md)</p></td>
<td align="left"><p>두 가지 방법으로 더 복잡한 투명성을 표시하는 텍스처 맵을 인코딩할 수 있습니다. 각 경우에서 투명성을 설명하는 블록이 이미 설명된 64비트 블록 앞에 옵니다. 투명도는 픽셀당 4비트(명시적 인코딩) 이하의 4x4 비트맵과 색 인코딩에 사용되는 것과 비슷한 선형 보간으로 표현됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[블록 압축](block-compression.md)</p></td>
<td align="left"><p>블록 압축은 성능 향상을 위해 텍스처 크기와 메모리 공간을 줄이는 손실 허용 텍스처 압축 기술입니다. 블록이 압축된 텍스처는 색당 32비트인 텍스처보다 작을 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[압축된 텍스처 형식](compressed-texture-formats.md)</p></td>
<td align="left"><p>이 섹션에는 압축된 텍스처 형식의 내부 구성에 대한 정보가 있습니다. 압축된 형식으로 또는 압축된 형식에서 변환에 Direct3D 기능을 사용할 수 있으므로 압축된 텍스처 사용을 위해 이러한 세부 사항이 필요하지는 않습니다. 그러나 이 정보는 압축된 표면 데이터에서 직접 작업하려는 경우 유용합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처](textures.md)

 

 




