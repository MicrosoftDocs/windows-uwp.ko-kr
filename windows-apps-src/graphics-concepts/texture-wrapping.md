---
title: 텍스처 래핑
description: 텍스처 래핑은 Direct3D가 각 꼭짓점마다 고유의 텍스처 좌표를 사용하여 텍스처 폴리곤을 래스터화하는 기본적인 방식을 바꿔놓았습니다.
ms.assetid: C28FB369-9A91-4D57-A96D-4A5D36484B35
keywords:
- 텍스처 래핑
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: bd24fc0ad14657e503feeab2a89ec7e6d4eb7ee0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044442"
---
# <a name="texture-wrapping"></a>텍스처 래핑


텍스처 래핑은 Direct3D가 각 꼭짓점마다 고유의 텍스처 좌표를 사용하여 텍스처 폴리곤을 래스터화하는 기본적인 방식을 바꿔놓았습니다. 시스템이 폴리곤을 래스터화하는 동안 각 폴리곤 꼭짓점의 텍스처 좌표 사이를 보간하여 모든 폴리곤 픽셀에 사용해야 하는 텍셀을 결정합니다. 일반적으로 시스템은 텍스처의 A 지점부터 B 지점까지 최단 거리를 이용해 새로운 텍셀을 보간하여 텍스처를 2D 평면으로 처리합니다. 만약 A 지점이 u, v 위치(0.8, 0.1)를 나타내고 B 지점이 (0.1, 0.1)에 위치한다면, 보간선은 다음 다이어그램과 같습니다.

![두 지점 간 보간선 다이어그램](images/interp1.png)

그림을 보면 A 지점과 B 지점 간 최단 거리가 대략적으로 텍스처의 중간을 향합니다. 이때 u-텍스처 또는 v- 텍스처 좌표 래핑을 사용하면 Direct3D가 u-방향과 v-방향의 텍스처 좌표 간 최단 거리를 인지하는 방식이 바뀝니다. 텍스처 래핑의 정의에 따라 래스터라이저는 0.0과 1.0이 일치한다는 가정 하에 텍스처 좌표 집합 간 최단 거리를 이용합니다. 마지막 비트는 까다로운 부분입니다. 텍스처 래핑을 한 방향으로 사용하면 시스템이 마치 원통을 중심으로 래핑되는 것처럼 텍스처를 처리할 것이라고 생각할 수 있습니다. 예를 들어 다음 다이어그램을 가정해보겠습니다.

![텍스처와 두 지점이 원통을 중심으로 래핑된 다이어그램](images/interp2.png)

위 그림은 u-방향으로 래핑할 경우 시스템의 텍스처 좌표 보간 방식에 어떤 영향을 끼치는지 나타냅니다. 이 예와 동일한 지점을 정상적인, 즉 래핑되지 않은 텍스처에 사용하면 A 지점과 B 지점 사이의 최단 거리는 텍스처 중간을 지나가지 않는다는 것을 알 수 있습니다. 그림에서는 0.0과 1.0이 공존하는 경계선을 지나고 있습니다. v-방향의 래핑도 한쪽으로 누워진 원통을 중심으로 텍스처를 래핑한다는 점만 빼면 비슷합니다. u-방향과 v-방향 모두의 래핑은 더욱 복잡합니다. 이 상황에서는 텍스처를 원환체, 즉 도넛으로 비유할 수 있습니다.

텍스처 래핑이 실질적으로 가장 많이 사용되는 경우는 환경 매핑입니다. 일반적으로 환경 맵으로 텍스처 처리되는 객체는 빛을 반사하여 표시되기 때문에 객체 주변의 미러 이미지가 나타납니다. 이번 설명을 쉽게 이해할 수 있도록 4개의 벽으로 둘러싸인 공간을 그리고, 각각 R, G, B Y라는 글자와 함께 해당하는 색상인 빨간색, 녹색, 파란색 및 노란색으로 색칠합니다. 이처럼 단순한 공간을 환경 맵으로 표현하면 다음 그림과 같습니다.

![빨간색, 녹색, 파란색 및 노란색의 세로 띠 그림](images/envmap.png)

공간의 천장이 완벽하게 반사되는 4면체 기둥으로 지지되어 있다고 생각해보세요. 환경 맵 텍스처를 기둥으로 매핑하는 것은 간단합니다. 하지만 기둥이 마치 글자와 색상을 반사하는 것처럼 보이게 하는 것은 쉽지 않습니다. 다음 다이어그램은 해당하는 텍스처 좌표를 상단 꼭짓점 가까이 나열한 기둥을 선으로 표현한 것입니다. 래핑이 텍스처 가장자리를 교차하는 경계선은 점선으로 표시되어 있습니다.

![이등분 점선으로 표현된 직사각형 다이어그램](images/seam.png)

u-방향으로 래핑을 사용하면 텍스처 처리된 기둥이 환경 맵의 색상과 글자를 알맞게 표시하며, 텍스처 전면의 경계선에서 래스터라이저가 u-좌표 0.0 및 1.0이 동일한 위치를 공유한다는 가정 하에 텍스처 좌표 간 최단 거리를 올바르게 선택합니다. 텍스처 처리된 기둥은 다름 그림과 같습니다.

![빨간색, 녹색, 파란색 및 노란색 사분면으로 구성된 기둥 그림](images/tex-seam.png)

텍스처 래핑을 사용하지 않으면 래스터라이저가 올바른 반사 이미지를 생성하는 데 필요한 방향으로 보간하지 않습니다. 오히려 텍스처 중간을 지나가면서 u-좌표 0.175와 0.875 사이에 가로로 압축된 버전의 텍셀이 기둥 전면 영역에 나타납니다. 결국 랩 효과가 손상됩니다.

텍스처 래핑과 비슷한 이름의 텍스처 주소 지정 모드를 혼동하지 마세요. 텍스처 래핑은 텍스처 주소 지정 이전에 실행됩니다. 알 수 없는 결과가 나올 수 있으므로 텍스처 래핑 데이터에 \[0.0, 1.0\]의 범위를 벗어나는 텍스처 좌표가 포함되어서는 안 됩니다. 텍스처 주소 지정에 대한 자세한 내용은 [텍스처 주소 지정 모드](texture-addressing-modes.md)를 참조하세요.

## <a name="span-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspandisplacement-map-wrapping"></a><span id="Displacement_Map_Wrapping"></span><span id="displacement_map_wrapping"></span><span id="DISPLACEMENT_MAP_WRAPPING"></span>변위 맵 래핑


변위 맵은 테셀레이션 엔진을 통해 보간됩니다. 테셀레이션 엔진에서는 랩 모드를 지정할 수 없기 때문에 변위 맵으로는 텍스처 래핑을 실행하지 못합니다. 응용 프로그램은 강제 보간을 통한 꼭짓점 집합을 사용하여 어떤 방향으로든 래핑할 수 있습니다. 또한 단순한 선형 보간으로 지정하는 것도 가능합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처](textures.md)

 

 



