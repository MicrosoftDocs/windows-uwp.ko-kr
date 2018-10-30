---
title: 래스터화 규칙 소개
description: 흔히 꼭짓점에 지정된 점은 화면의 픽셀과 정확히 일치하지는 않습니다. 이 경우, Direct3D가 삼각형 래스터화 규칙을 적용하여 주어진 삼각형에 어떤 픽셀을 적용할지 결정합니다.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- 래스터화 규칙 소개
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 65522195b9729ddd4f2ebeb193f43c905359eda2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5757385"
---
# <a name="introduction-to-rasterization-rules"></a>래스터화 규칙 소개


흔히 꼭짓점에 지정된 점은 화면의 픽셀과 정확히 일치하지는 않습니다. 이 경우, Direct3D가 삼각형 래스터화 규칙을 적용하여 주어진 삼각형에 어떤 픽셀을 적용할지 결정합니다.

래스터화 규칙을 간단하게 소개합니다. 자세한 내용은 [래스터화 규칙](rasterization-rules.md)을 참조하세요. [래스터화(RS) 단계](rasterizer-stage--rs-.md)도 참조하세요.

## <a name="span-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>삼각형 래스터화 규칙


Direct3D는 기하 도형을 채우기 위해 왼쪽 위 채우기 규칙을 사용합니다. GDI 및 OpenGL의 사각형에 사용되는 규칙과 동일합니다. Direct3D에서 픽셀의 중앙이 기준점입니다. 중앙이 삼각형 내부에 있으면 픽셀은 삼각형의 일부입니다. 픽셀 중앙은 정수 좌표에 있습니다.

Direct3D가 사용하는 삼각형 래스터화 규칙에 대한 설명이 모든 가용 하드웨어에 적용되지는 않습니다. 테스트를 통해 이 규칙의 구현에서 약간의 차이를 발견할 수 있습니다.

다음 그림은 왼쪽 위 모서리가 (0, 0)에 있고 오른쪽 아래 모서리가 (5, 5)에 있는 직사각형을 보여줍니다. 이 사각형은 예상 대로 25픽셀을 채웁니다. 직사각형의 폭은 오른쪽 빼기 왼쪽으로 정의됩니다. 높이는 아래 빼기 위로 정의됩니다.

![6개 행과 열로 나뉜, 번호가 매겨진 정사각형](images/pixmap.png)

왼쪽 위 채우기 규칙에서 *위쪽*은 수평 범위의 수직 위치를 참조하고 *왼쪽* 범위 내에서 픽셀의 수평 위치를 참조합니다. 가장자리가 수평이 아니면 위쪽 가장자리가 될 수 없습니다. 일반적으로 대부분 삼각형에는 왼쪽 및 오른쪽 가장자리만 있습니다. 다음 그림은 위쪽 가장자리와 오른쪽 가장자리를 보여줍니다.

![두 개의 삼각형을 포함한 번호가 매겨진 정사각형](images/triedge.png)

왼쪽 위 채우기 규칙은 삼각형이 픽셀의 중앙을 통과할 때 Direct3D가 취할 조치를 결정합니다. 다음 그림은 두 개의 삼각형을 보여주는데, 하나는 (0, 0), (5, 0), (5, 5)에, 다른 하나는 (0, 5), (0, 0), (5, 5)에 있습니다. 이 경우 첫 번째 삼각형은 15픽셀(검은색으로 표시)로 구성되는 반면 두 번째는 10픽셀(회색으로 표시)로만 구성됩니다. 공유된 가장자리가 첫 번째 삼각형의 왼쪽 가장자리이기 때문입니다.

![두 개의 삼각형을 보여주는 번호가 매겨진 정사각형](images/twotris.png)

(0.5, 0.5)를 왼쪽 위 모서리로, (2.5, 4.5)를 오른쪽 아래 모서리로 직사각형을 정의하면 이 직사각형의 중앙점은 (1.5, 2.5)입니다. Direct3D 래스터라이저가 이 직사각형을 분할하면, 각 픽셀의 중앙은 확실히 네 개 삼각형의 안쪽에 놓이며 왼쪽 위 채우기 규칙이 필요하지 않습니다. 다음 그림은 이를 보여줍니다. Direct3D이 포함하는 삼각형에 따라 직사각형의 픽셀에 레이블이 지정됩니다.

![네 개 삼각형으로 나뉘는 직사각형을 포함한 번호가 매겨진 정사각형](images/noambig.png)

앞에서 본 그림처럼 왼쪽 위 모서리가 (1.0, 1.0), 오른쪽 아래 모서리가 (3.0, 5.0), 중앙 점이 (2.0, 3.0)에 놓이도록 직사각형을 옮기면 Direct3D가 왼쪽 위 채우기 규칙을 적용합니다. 다음 그림에서 보듯이 이 직사각형에서 대부분의 픽셀은 두 개 이상의 삼각형 경계를 가로지릅니다.

![네 개 삼각형으로 나뉘는 직사각형을 포함한 번호가 매겨진 정사각형](images/fillrule.png)

다음 그림과 같이 두 직사각형 모두 동일한 픽셀이 영향을 받습니다.

![번호가 매겨진 앞의 두 정사각형으로부터 영향을 받는 픽셀](images/samepix.png)

## <a name="span-idpointandlinerulesspanspan-idpointandlinerulesspanspan-idpointandlinerulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>점과 선 규칙


점은 점 스프라이트와 동일하게 렌더링됩니다. 즉, 두 가지 모두 화면 정렬 사변형으로 렌더링되기 때문에 다각형 렌더링과 같은 규칙을 준수합니다.

안티앨리어싱되지 않은 선 렌더링 규칙은 [GDI 선](https://msdn.microsoft.com/library/windows/desktop/dd145027)의 규칙과 정확히 동일합니다.

## <a name="span-idpointspriterulesspanspan-idpointspriterulesspanspan-idpointspriterulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>점 스프라이트 규칙


점 스프라이트 및 패치 원형은 원형이 먼저 삼각형으로 분할되고 그 삼각형이 래스터화되는 것처럼 래스터화됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[장치](devices.md)

[RS(래스터라이저) 단계](rasterizer-stage--rs-.md)

[래스터화 규칙](rasterization-rules.md)

 

 




