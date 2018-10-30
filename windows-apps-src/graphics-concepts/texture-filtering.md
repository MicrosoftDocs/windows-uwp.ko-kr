---
title: 텍스처 필터링
description: 텍스처 필터링은 3D 기본 객체를 2D 화면으로 매핑하여 기본 객체를 렌더링할 때 기본 객체의 2D 렌더링 이미지를 구성하는 각 픽셀 색상을 생성합니다.
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- 텍스처 필터링
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce80bc0f64e1aba8328880203f22ea3909fdb3e3
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5757094"
---
# <a name="texture-filtering"></a>텍스처 필터링


텍스처 필터링은 3D 기본 객체를 2D 화면으로 매핑하여 기본 객체를 렌더링할 때 기본 객체의 2D 렌더링 이미지를 구성하는 각 픽셀 색상을 생성합니다.

Direct3D가 기본 객체를 렌더링할 때는 3D 기본 객체를 2D 화면에 매핑합니다. 이때 기본 객체에 텍스처가 있으면 Direct3D는 해당 텍스처를 사용하여 기본 객체의 2D 렌더링 이미지를 구성하는 각 픽셀 색상을 생성해야 합니다. 기본 객체의 화면 이미지를 구성하는 각 픽셀마다 해당 텍스처에서 색상 값을 가져와야 합니다. 이러한 프로세스를 *텍스처 필터링*이라고 합니다.

텍스처 필터 연산을 실행하면 사용할 텍스처까지 일반적으로 확대 또는 축소됩니다. 다시 말해서 자체 이미지보다 더 크거나 작은 기본 객체의 이미지로 매핑됩니다. 텍스처가 확대되면 다수의 픽셀이 단일 텍셀로 매핑될 수 있습니다. 그 결과 이미지가 뭉친 것처럼 보일 수 있습니다. 텍스처가 축소되면 종종 단일 픽셀이 다수의 텍셀로 매핑됩니다. 그 결과 이미지가 흐릿하거나 매끄럽지 못할 수 있습니다. 이러한 문제를 해결하려면 텍셀 색상을 혼합하여 픽셀에 알맞은 색상을 찾아야 합니다.

Direct3D는 복잡한 텍스처 필터링 프로세스를 간소화하였습니다. 제공되는 텍스처 필터링 유형은 선형 필터링, 이방성 필터링 및 Mipmap 필터링 등 세 가지입니다. 텍스처 필터링을 따로 선택하지 않으면 Direct3D가 근접점 샘플링이라고 하는 기법을 사용합니다.

각 텍스처 필터링 유형에는 장단점이 있습니다. 예를 들어 선형 텍스처 필터링에서는 최종 이미지에서 가장 자리가 톱니 모양처럼 뾰족하거나 뭉친 듯한 모양이 될 수 있습니다. 하지만 계산에 따른 오버헤드를 낮출 수 있는 텍스처 필터링 방법입니다. Mipmap 필터링은 특히 이방성 필터링과 함께 사용하면 대체로 최상의 결과를 이끌어냅니다. 하지만 Direct3D가 지원하는 기법 중에서 가장 많은 메모리를 소비해야 합니다.

## <a name="span-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspantypes-of-texture-filtering"></a><span id="Types-of-texture-filtering"></span><span id="types-of-texture-filtering"></span><span id="TYPES-OF-TEXTURE-FILTERING"></span>텍스처 필터링 유형


Direct3D는 다음과 같은 텍스처 필터링 방법을 지원합니다.

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
<td align="left"><p><a href="nearest-point-sampling.md">근접점 샘플링</a></p></td>
<td align="left"><p>응용 프로그램이 텍스처 필터링을 사용할 필요는 없습니다. Direct3D가 종종 정수로 평가되지 않는 텍셀 주소를 계산한 다음 가장 가까운 정수 주소로 텍셀 색상을 복사하도록 설정하는 방법도 있습니다. 이러한 프로세스를 <em>근접점 샘플링</em>이라고 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">쌍선형 텍스처 필터링</a></p></td>
<td align="left"><p><em>쌍선형 필터링</em>은 샘플링 점에 가장 가까운 4텍셀의 가중 평균을 계산합니다. 이 필터링 방식이 가장 가까운 점 필터링보다 더 정확하고 일반적입니다. 또한 최신 그래픽 하드웨어에 구현된다는 점에서 효율적이기도 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">이방성 텍스처 필터링</a></p></td>
<td align="left"><p><em>이방성</em>은 표면 방향이 화면의 평면 기준 각도인 3D 개체의 텍셀에 표시되는 왜곡입니다. 이방성 기본 객체의 픽셀을 텍셀로 매핑할 때는 그 형상이 왜곡됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">Mipmap을 사용하는 텍스처 필터링</a></p></td>
<td align="left"><p><em>Mipmap</em>이란 순차적으로 이어지는 텍스처로서 각각 동일한 이미지의 해상도가 점차 낮아지면서 표현됩니다. Mipmap에서 각 이미지 또는 수준의 높이와 너비는 이전 수준보다 2의 제곱 더 작습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처](textures.md)

 

 




