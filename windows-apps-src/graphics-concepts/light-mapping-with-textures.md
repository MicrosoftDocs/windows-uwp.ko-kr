---
title: 텍스처를 사용한 조명 매핑
description: 조명 맵은 3D 장면에서 조명에 대한 정보를 포함하는 텍스처 또는 텍스처 그룹입니다.
ms.assetid: 5C7518D2-AC92-4A97-B7AF-4469D213D7BD
keywords:
- 텍스처를 사용한 조명 매핑
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b5d245247d33f3c04839620615f2778ef7dfb59
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7713686"
---
# <a name="light-mapping-with-textures"></a>텍스처를 사용한 조명 매핑


조명 맵은 3D 장면에서 조명 정보가 저장되는 텍스처 또는 텍스처 그룹으로서 조명 맵은 원형에 빛과 그림자 영역을 매핑합니다. 멀티패스와 여러 텍스처 혼합을 사용하면 응용 프로그램에서 음영 기술보다 사실적인 모양으로 장면을 렌더링할 수 있습니다.

응용 프로그램에서 3D 장면을 사실적으로 렌더링하려면 광원이 장면 형태에 가져다 주는 효과를 고려해야 합니다. 그런 면에서 기본 및 고우러드 음영 등의 기술은 유용한 도구이지만 충분하지 않을 수 있습니다. Direct3D는 멀티패스와 여러 텍스처 혼합을 지원합니다. 이러한 기능을 통해 응용 프로그램에서 음영 기술 하나만으로 렌더링한 장면보다 사실적인 형태로 장면을 렌더링할 수 있습니다. 하나 이상의 조명 맵을 적용하면 응용 프로그램이 빛과 그림 영역을 원형에 매핑할 수 있습니다.

조명 맵은 3D 장면에서 조명에 대한 정보를 포함하는 텍스처 또는 텍스처 그룹입니다. 조명 정보를 조명 지도의 알파 값, 색상 값 또는 두 가지 형식으로 저장할 수 있습니다.

멀티패스 텍스처 혼합을 이용해 조명 매핑을 구현하면 응용 프로그램은 조명 맵을 첫 번째 패스의 원형에 렌더링해야 합니다. 기본 텍스처를 렌더링하려면 두 번째 패스를 이용해야 합니다. 유일한 예외는 반사광 매핑입니다. 그 경우 기본 텍스처를 먼저 렌더링한 후 조명 맵을 추가합니다.

여러 텍스처 혼합을 사용하면 하나의 패스로 응용 프로그램이 조명 지도와 기본 텍스처를 렌더링할 수 있습니다. 사용자의 하드웨어가 여러 텍스처 혼합을 지원하면 응용 프로그램이 조명 매핑을 수행할 때 이를 활용해야 합니다. 그러면 응용 프로그램의 성능이 크게 향상됩니다.

조명 지도를 사용하면 Direct3D 응용 프로그램이 원형을 렌더링할 때 다양한 조명 효과를 얻을 수 있습니다. 장면에서 단색 및 컬러 조명을 매핑할 수 있을 뿐만 아니라 반사 하이라이트와 확산 조명 등의 세부 사항을 추가할 수도 있습니다.

조명 매핑을 위해 Direct3D 텍스처 혼합을 사용하는 경우에 대한 정보가 다음 그림에 나와 있습니다.

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
<td align="left"><p><a href="monochrome-light-maps.md">단색 조명 맵</a></p></td>
<td align="left"><p>이전 3D 가속기 보드가 대상 픽셀의 알파 값을 사용하는 텍스처 혼합을 지원하지 않는 경우, 단색 조명 매핑을 사용하면 이전 어댑터가 멀티패스 텍스처 혼합을 수행할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="color-light-maps.md">컬러 조명 맵</a></p></td>
<td align="left"><p>컬러 조명 맵은 조명 지도에 RGB 데이터를 사용해 조명 정보를 표시합니다. 응용 프로그램은 일반적으로 컬러 조명 맵을 사용할 경우 3D 장면을 더 사실적으로 렌더링합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-light-maps.md">반사 조명 맵</a></p></td>
<td align="left"><p>광원으로 조명했을 때 고 반사성 재질을 사용하는 반짝거리는 개체는 반사 하이라이트를 받습니다. 때로는 조명 모듈에서 생성한 반사 하이라이트를 사용하는 대신 반사 조명 맵을 원형에 적용하면 보다 정확한 하이라이트를 얻을 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-light-maps.md">확산 조명 맵</a></p></td>
<td align="left"><p>매트 표면에는 확산 조명 반사가 있습니다. 확산 조명의 밝기는 광원에서의 거리와 표면 법선과 광원 방향 벡터 사이의 각도에 따라 달라집니다. 텍스처 조명 맵은 복잡한 확산 조명을 시뮬레이션할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처](textures.md)

 

 




