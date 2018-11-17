---
title: 투영 변환
description: 투영 변환은 카메라의 렌즈를 선택하는 등 카메라 내부를 제어합니다. 해당 기능은 세 개의 변환 유형 중에서 가장 복잡합니다.
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- 투영 변환
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 01a410e0e2759dcdfd6adff9c25238447fe4138b
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7150379"
---
# <a name="projection-transform"></a>투영 변환


*프로젝션 변환*은 카메라의 렌즈를 선택하는 등 카메라 내부를 제어합니다. 해당 기능은 세 개의 변환 유형 중에서 가장 복잡합니다.

일반적으로 투영 행렬은 배율 및 원근 행렬입니다. 투영 변환은 절두체 보기를 직육면체 모양으로 변환합니다. 절두체 보기의 가까운 쪽 끝이 먼 쪽 끝보다 작기 때문에 카메라에 가까운 개체를 확대하는 효과가 있습니다. 이것이 장면에 원근이 적용되는 방법입니다.

[절두체 보기](viewports-and-clipping.md)에서 카메라와 보기 변환 공간 원점 사이의 거리는 임의로 D로 정의되므로 투영 행렬은 다음 그림처럼 보입니다.

![투영 행렬 그림](images/projmat1.png)

보기 행렬은 z 방향으로 -D만큼 이동하여 카메라를 원점으로 이동합니다. 이동 행렬은 다음 그림과 같습니다.

![이동 행렬 그림](images/projmat2.png)

이동 행렬을 투영 행렬(T\*P)로 곱하면 다음 그림에서 보듯 복합 투영 행렬이 나옵니다.

![복합 투영 행렬 그림](images/projmat3.png)

원근 변형은 절두체 보기를 새로운 좌표 공간으로 변환합니다. 절두체가 직육면체가 되고 아래 다이어그램에서 보듯 원점이 장면의 위 오른쪽 모서리에서 중앙으로 이동하는 것에 유의하세요.

![원근 변형이 절두체 보기를 새로운 좌표 공간으로 변환하는 방법을 보여 주는 다이어그램](images/cuboid.png)

원근 변형에서 x방향과 y방향의 한도는 -1과 1입니다. z 방향 한도는 앞 평면의 경우는 0이고 뒤 평면의 경우 1입니다.

이 행렬은 카메라부터 가까운 클리핑 평면까지의 지정된 거리에 따라 개체를 이동하고 확대하지만 보기 필드(fov)를 고려하지 않으며, 멀리 있는 개체에 대해 생성하는 z 값이 거의 동일할 수 있어 깊이 비교가 어려워집니다. 다음 행렬은 이런 문제를 해결하고 뷰포트의 가로 세로 비율을 고려하여 꼭짓점을 조정하므로 원근 투영을 위한 좋은 선택입니다.

![원근 투영 행렬 그림](images/prjmatx1.png)

이 행렬에서 Zₙ는 가까운 클리핑 평면의 z 값입니다. 변수 w, h 및 Q의 뜻은 다음과 같습니다 fov<sub>w</sub>와 fovₖ는 뷰포트의 수평 및 수직 보기 필드를 라디언으로 나타냅니다.

![변수 수식의 의미](images/prjmatx2.png)

응용 프로그램에서 x 및 y 축척 계수 정의에 fov 각도를 사용하는 것은 뷰포트의 가로 세로 치수를 사용하는 것(카메라 공간에서)보다 불편할 수 있습니다. 수학적으로 w와 h의 다음 2개 수식은 뷰포트의 차원을 사용하며 앞의 수식과 동등합니다.

![w 및 h 변수 수식의 의미](images/prjmatx3.png)

이러한 수식에서 Zₙ는 가까운 클리핑 평면의 위치를 나타내며, V<sub>w</sub> 및 Vₕ 변수는 카메라 공간에서의 뷰포트의 너비와 높이를 나타냅니다.

어떤 수식을 사용하건 Zₙ를 최대한 큰 값으로 설정해야 합니다. 카메라에 극도로 가까운 z 값은 크게 달라지지 않기 때문입니다. 이렇게 하면 16비트 z 버퍼를 사용하는 깊이 비교가 다소 복잡해집니다.

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>w-friendly 투영 행렬


Direct3D는 월드, 보기, 투영 행렬에 의해 변환된 꼭짓점의 w 구성 요소를 사용하여 깊이 버퍼 또는 안개 효과에서 깊이 기반 계산을 수행할 수 있습니다. 이와 같은 계산에서는 투영 행렬이 w를 월드-공간 z와 동등해지도록 정규화해야 합니다. 요컨대 투영 행렬에 1이 아닌 (3,4) 계수가 포함된 경우, 적절한 행렬을 만들기 위해서는 (3.4) 계수의 역수만큼 모든 계수를 확대해야 합니다. 정책 준수 행렬을 제공하지 않으면 안개 효과 및 깊이 버퍼링이 올바로 적용되지 않습니다.

다음 그림은 정책 위반 투영 행렬 및 eye-relative 안개를 사용할 수 있도록 확대된 동일한 행렬을 보여 줍니다.

![정책 위반 투영 행렬 및 eye-relative 안개 그림](images/eyerlmx.png)

이전 행렬에서 모든 변수는 0이 아닌 것으로 간주됩니다. w 기반 깊이 버퍼링에 대한 내용은 [깊이 버퍼](depth-buffers.md)를 참조하세요.

Direct3D는 w 기반 깊이 계산에 현재 설정된 투영 행렬을 사용합니다. 결과적으로 응용 프로그램은 변형에 Direct3D를 사용하지 않는 경우에도 원하는 w 기반 기능을 받도록 정책 준수 투영 행렬을 설정해야 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[변환](transforms.md)

 

 




