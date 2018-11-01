---
title: 텍스처
description: 텍스처는 컴퓨터에서 생성되는 3D 이미지에 현실감을 불어넣을 수 있는 강력한 도구입니다. Direct3D는 광범위한 텍스처 기능 세트를 지원하여 개발자가 고급 텍스처 기법에 쉽게 접근할 수 있습니다.
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords:
- 텍스처
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 516a15c17546d9f9b5e7cb7f8c0651f1372275ae
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5878029"
---
# <a name="textures"></a>텍스처


텍스처는 컴퓨터에서 생성되는 3D 이미지에 현실감을 불어넣을 수 있는 강력한 도구입니다. Direct3D는 광범위한 텍스처 기능 세트를 지원하여 개발자가 고급 텍스처 기법에 쉽게 접근할 수 있습니다.

성능을 높이려면 동적 텍스처를 사용하는 것이 좋습니다. 동적 텍스처는 각 프레임마다 잠그거나, 작성하거나, 잠금 해제할 수 있습니다.

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
<td align="left"><p><a href="introduction-to-textures.md">텍스처 소개</a></p></td>
<td align="left"><p>텍스처 리소스는 텍셀을 저장할 수 있는 데이터 구조이며, 여기에서 텍셀이란 읽거나 쓸 수 있는 가장 작은 단위의 텍스처를 의미합니다. 셰이더에서 텍스처를 읽을 때는 텍스처 샘플러에서 필터링할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="basic-texturing-concepts.md">기본 텍스처 지정 개념</a></p></td>
<td align="left"><p>초기에 컴퓨터에서 생성된 3D 이미지는 시기적으로 진보한 기술이기는 했지만 빛나는 플라스틱처럼 보이는 경향이 있었습니다. 이러한 이미지에는 3D 개체에 사실적인 시각적 복잡성을 더하는 스친 자국, 금, 지문, 얼룩 등의 무늬가 없었습니다. 오늘날 텍스처는 컴퓨터에서 생성되는 3D 이미지의 사실감을 높여준다는 점에서 인기가 매우 높아졌습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-addressing-modes.md">텍스처 주소 지정 모드</a></p></td>
<td align="left"><p>Direct3D 응용 프로그램은 모든 기본 객체의 꼭짓점에 텍스처 좌표를 지정할 수 있습니다. 일반적으로 꼭짓점에 할당되는 u 및 v-텍스처 좌표는 0.0~1.0(0.0과 1.0 포함)의 범위 내에 있습니다. 하지만 범위 외부에 텍스처 좌표를 지정할 경우 특수한 텍스처 효과를 생성할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering.md">텍스처 필터링</a></p></td>
<td align="left"><p>텍스처 필터링은 3D 기본 객체를 2D 화면으로 매핑하여 기본 객체를 렌더링할 때 기본 객체의 2D 렌더링 이미지를 구성하는 각 픽셀 색상을 생성합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-resources.md">텍스처 리소스</a></p></td>
<td align="left"><p>텍스처는 렌더링에 사용되는 일종의 리소스입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-wrapping.md">텍스처 래핑</a></p></td>
<td align="left"><p>텍스처 래핑은 Direct3D가 각 꼭짓점마다 고유의 텍스처 좌표를 사용하여 텍스처 폴리곤을 래스터화하는 기본적인 방식을 바꿔놓았습니다. 시스템이 폴리곤을 래스터화하는 동안 각 폴리곤 꼭짓점의 텍스처 좌표 사이를 보간하여 모든 폴리곤 픽셀에 사용해야 하는 텍셀을 결정합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-blending.md">텍스처 혼합</a></p></td>
<td align="left"><p>Direct3D는 싱글패스에 최대 8개까지 텍스처를 기본 객체로 혼합할 수 있습니다. 다중 텍스처 혼합을 사용하면 Direct3D 응용 프로그램의 프레임 속도를 크게 높일 수 있습니다. 응용 프로그램은 다중 텍스처 혼합을 사용하여 싱글패스에 텍스처, 그림자, 정반사 조명, 확산 조명 및 기타 특수 효과를 적용합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-mapping-with-textures.md">텍스처와 조명 매핑</a></p></td>
<td align="left"><p>조명 맵은 3D 장면에서 조명 정보가 저장되는 텍스처 또는 텍스처 그룹으로서 조명 맵은 원형에 빛과 그림자 영역을 매핑합니다. 애플리케이션은 멀티패스 및 다중 텍스처 혼합 기능을 사용하여 셰이딩 기법보다 더욱 사실적인 장면을 렌더링할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compressed-texture-resources.md">압축된 텍스처 리소스</a></p></td>
<td align="left"><p>텍스처 맵은 3D 형상으로 그려진 디지털 이미지로서 시각적 디테일을 높여줍니다. 래스터화 중 이러한 셰이프에 텍스처 맵이 매핑되고 프로세스에서 시스템 버스와 메모리를 많이 사용할 수 있습니다. 텍스처에서 사용하는 메모리 양을 줄이기 위해 Direct3D에서 텍스처 표면의 압축을 지원합니다. 일부 Direct3D 장치는 압축된 텍스처 표면을 기본적으로 지원하기도 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

 

 




