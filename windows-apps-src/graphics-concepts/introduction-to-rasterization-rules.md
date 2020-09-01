---
title: 래스터화 규칙 소개
description: 꼭 짓 점에 대해 지정 된 점이 화면의 픽셀과 정확히 일치 하지 않는 경우가 많습니다. 이 경우 Direct3D는 삼각형 래스터화 규칙을 적용 하 여 지정 된 삼각형에 적용 되는 픽셀을 결정 합니다.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- 래스터화 규칙 소개
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 38522be28280c0a08f6cb065e5dfb5c2f26642a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162797"
---
# <a name="introduction-to-rasterization-rules"></a>래스터화 규칙 소개


꼭 짓 점에 대해 지정 된 점이 화면의 픽셀과 정확히 일치 하지 않는 경우가 많습니다. 이 경우 Direct3D는 삼각형 래스터화 규칙을 적용 하 여 지정 된 삼각형에 적용 되는 픽셀을 결정 합니다.

이는 래스터화 규칙의 간소화 된 소개입니다. 자세한 내용은 [래스터화 규칙](rasterization-rules.md)을 참조 하세요. 또한 [라이저 (RS) 단계](rasterizer-stage--rs-.md)를 참조 하세요.

## <a name="span-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>삼각형 래스터화 규칙


Direct3D는 기 하 도형을 채우기 위해 왼쪽 위 채우기 규칙을 사용 합니다. 이는 GDI 및 OpenGL의 사각형에 사용 되는 것과 동일한 규칙입니다. Direct3D에서 픽셀의 중심은 결정적인 점입니다. 중심이 삼각형 안에 있는 경우 픽셀은 삼각형의 일부입니다. 픽셀 가운데가 정수 좌표에 있습니다.

Direct3D에서 사용 되는 삼각형 래스터화 규칙에 대 한 설명은 사용 가능한 모든 하드웨어에 반드시 적용 되는 것은 아닙니다. 테스트는 이러한 규칙의 구현에서 사소한 변형을 발견할 수 있습니다.

다음 그림에서는 왼쪽 위 모퉁이가 (0, 0)에 있고 오른쪽 아래 모퉁이가 (5, 5)에 있는 사각형을 보여 줍니다. 이 사각형은 원하는 것 처럼 25 픽셀을 채웁니다. 사각형의 너비는 오른쪽에서 왼쪽으로 정의 됩니다. 높이가 아래쪽에서 위쪽으로 정의 됩니다.

![6 개의 행과 열로 구분 된 숫자의 사각형입니다.](images/pixmap.png)

왼쪽 위 채우기 규칙에서 *위쪽* 은 가로 범위의 세로 위치를 참조 하 고 *왼쪽* 은 범위 내에서 픽셀의 가로 위치를 나타냅니다. 가로가 아닌 가장자리는 위쪽 가장자리 일 수 없습니다. 일반적으로 대부분의 삼각형에는 왼쪽 및 오른쪽 가장자리만 있습니다. 다음 그림은 위쪽 가장자리와 오른쪽 가장자리를 보여 줍니다.

![두 개의 삼각형을 포함 하는 번호가 매겨진 정사각형](images/triedge.png)

왼쪽 위 채우기 규칙은 삼각형이 픽셀의 중심을 통과할 때 Direct3D에서 수행 하는 동작을 결정 합니다. 다음 그림에서는 (0, 0), (5, 0) 및 (5, 5), (0, 5), (0, 0) 및 (5, 5)에 있는 두 개의 삼각형을 보여 줍니다. 이 경우 첫 번째 삼각형은 15 픽셀 (검은색으로 표시 됨)을, 두 번째 삼각형은 10 픽셀 (회색으로 표시)을 가져옵니다 .이는 공유 가장자리가 첫 번째 삼각형의 왼쪽 가장자리 이기 때문입니다.

![두 개의 삼각형을 보여 주는 숫자가 지정 된 사각형](images/twotris.png)

(0.5, 0.5)의 왼쪽 위 모퉁이와 오른쪽 아래 모퉁이 (2.5, 4.5)로 사각형을 정의 하는 경우이 사각형의 중심점은 (1.5, 2.5)입니다. Direct3D 래스터 라이저가이 사각형을 tessellates 각 픽셀의 중심은 각각 네 개의 삼각형 안에 명확 하지 않으며 왼쪽 위 채우기 규칙이 필요 하지 않습니다. 다음 그림에서는이를 보여 줍니다. 사각형의 픽셀은 Direct3D에 포함 된 삼각형에 따라 레이블이 지정 됩니다.

![네 개의 삼각형으로 나뉘는 사각형을 포함 하는 번호가 매겨진 정사각형](images/noambig.png)

위의 그림에서 왼쪽 위 모퉁이가 (1.0, 1.0)에 있고 (3.0, 5.0)의 중심점 (2.0, 3.0)에 있는 중심점을 앞으로 이동 하는 경우 Direct3D는 왼쪽 위 채우기 규칙을 적용 합니다. 다음 그림에 나와 있는 것 처럼이 사각형의 픽셀 대부분은 두 개 이상의 삼각형 사이의 테두리를 걸쳐 있을 합니다.

![네 개의 삼각형으로 나뉘는 사각형을 포함 하는 번호가 매겨진 정사각형](images/fillrule.png)

두 사각형 모두에 대해 다음 그림과 같이 동일한 픽셀이 영향을 받습니다.

![앞에 두 개의 숫자가 지정 된 사각형의 영향을 받는 픽셀](images/samepix.png)

## <a name="span-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>점 및 선 규칙


지점은 화면에 맞춰진 quadrilaterals 렌더링 되는 점 스프라이트와 동일 하 게 렌더링 되므로 polygon 렌더링과 동일한 규칙을 준수 합니다.

앤티 앨리어싱 되지 않는 선 렌더링 규칙은 [GDI 선과](/windows/desktop/gdi/lines)정확히 동일 합니다.

## <a name="span-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Point 스프라이트 규칙


Point 스프라이트 및 patch 기본 형식은 기본 형식이 먼저 삼각형으로 공간 분할 결과 삼각형이 래스터화됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[디바이스](devices.md)

[RS(래스터라이저) 단계](rasterizer-stage--rs-.md)

[래스터화 규칙](rasterization-rules.md)

 

 