---
title: 뷰포트 및 클리핑
description: 뷰포트는 2차원(2D) 사각형으로 여기에 3D 장면이 프로젝션됩니다.
ms.assetid: D0DD646E-13AE-452A-AD22-8C35000D0BA9
keywords:
- 뷰포트 및 클리핑
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7d2a8953d202cc22729f99a096b5fb62cf1131d9
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8894596"
---
# <a name="viewports-and-clipping"></a>뷰포트 및 클리핑


*뷰포트*는 3D 화면이 프로젝션되는 2차원(2D) 사각형입니다. Direct3D에서 사각형은 시스템이 렌더링 대상으로 사용하는 Direct3D 표면 내의 좌표로 존재합니다. 프로젝션 변환은 꼭짓점을 뷰포트에 사용되는 좌표계로 변환합니다. 뷰포트는 장면이 렌더링될 렌더링 대상 표면의 깊이 값 범위를 지정하는 데에도 사용됩니다(일반적으로 0.0~1.0).

## <a name="span-idtheviewingfrustumspanspan-idtheviewingfrustumspanspan-idtheviewingfrustumspanthe-viewing-frustum"></a><span id="The_Viewing_Frustum"></span><span id="the_viewing_frustum"></span><span id="THE_VIEWING_FRUSTUM"></span>절두체 보기


절두체 보기는 뷰포트의 카메라를 기준으로 배치되는 장면의 3D 볼륨입니다. 볼륨의 셰이프는 모델을 카메라 공간에서 장면으로 프로젝션하는 방법에 영향을 미칩니다. 가장 일반적인 프로젝션 유형인 관점 프로젝션은 카메라에 가까운 개체를 멀리 있는 개체보다 더 크게 표시합니다. 관점 보기의 경우 절두체 보기를 피라미드로 시각화하고, 다음 그림과 같이 카메라를 끝에 배치할 수 있습니다. 이 피라미드에서 전면 및 후면 클리핑 평면이 교차합니다. 전면 및 후면 클리핑 평면 사이의 피라미드 내부에 있는 볼륨이 절두체 보기입니다. 개체는 이 볼륨에 있을 때에만 표시됩니다.

![전면 및 후면 클리핑 평면을 사용하는 절두체 보기 그림](images/frustum.png)

절두체 보기를 시각화한다는 것은 어두운 방에 서서 사각형 창을 통해 내다보는 행동에 비유할 수 있습니다. 이 비유에서 가까운 클리핑 평면은 창이고, 후면 클리핑 평면은 시야를 방해하는 모든 것입니다. 거리의 고층 빌딩이 될 수도 있고, 멀리 있는 산이 될 수도 있고, 전혀 없을 수도 있습니다. 창에서 시작하고 시야를 방해하는 물체에서 끝이 나는 잘린 피라미드 내의 모든 것을 볼 수 있지만, 그 외에는 아무 것도 볼 수 없습니다.

절두체 보기는 fov(보기 필드)와 전면 및 후면 클리핑 평면의 거리로 정의되며 다음 다이어그램과 같이 z축 좌표로 지정됩니다.

![절두체 보기 다이어그램](images/fovdiag.png)

이 다이어그램에서 변수 D는 카메라에서 기하 도형 파이프라인의 마지막 부분에서 정의된 공간 원점까지의 거리이며 이것이 보기 변환입니다. 이 공간을 기준으로 절두체 보기의 제한을 지정합니다. 이 D 변수를 사용하여 프로젝션 매트릭스를 만드는 방법은 [프로젝션 변환](projection-transform.md)을 참조하세요.

## <a name="span-idviewportrectanglespanspan-idviewportrectanglespanspan-idviewportrectanglespanviewport-rectangle"></a><span id="Viewport_Rectangle"></span><span id="viewport_rectangle"></span><span id="VIEWPORT_RECTANGLE"></span>뷰포트 사각형


뷰포트 구조는 장면이 렌더링되는 렌더링 대상 표면의 영역을 정의하는 네 가지 요소(X, Y, 너비, 높이)를 포함하고 있습니다. 이러한 값은 다음 다이어그램과 같이 대상 사각형 또는 뷰포트 사각형에 해당합니다.

![뷰포트 사각형 다이어그램](images/destrect.png)

X, Y, 너비, 높이 요소에 지정한 값은 렌더링 대상 표면의 왼쪽 위 모서리를 기준으로 하는 화면 좌표입니다. 구조는 장면이 렌더링되는 깊이 범위를 나타내는 두 가지 추가 요소(MinZ 및 MaxZ)를 정의합니다.

Direct3D에서는 뷰포트 클리핑 볼륨 범위를 X축은 -1.0~1.0, Y축은 1.0~-1.0로 간주합니다. 이 값은 이전의 응용 프로그램에서 가장 자주 사용되던 설정입니다. [프로젝션 변환](projection-transform.md)을 사용하여 클리핑하기 전에 뷰포트 가로 세로 비율을 조정할 수 있습니다.

**참고**  MinZ 및 MaxZ 장면 렌더링 되는 깊이 범위 나타내며 클리핑에 사용 되지 않습니다. 대부분의 응용 프로그램은 시스템에서 깊이 버퍼의 깊이 값 전체 범위까지 렌더링할 수 있도록 이러한 값을 0.0~1.0으로 설정합니다. 경우에 따라 다른 깊이 범위를 사용하여 특수 효과를 얻을 수 있습니다. 예를 들어 게임에서 헤드업 디스플레이를 렌더링하려면 두 값을 0.0으로 설정하여 시스템이 포그라운드의 장면에서 개체를 렌더링하도록 강제할 수 있습니다. 또는 둘 다 1.0으로 설정하여 항상 백그라운드에 위치하는 개체를 렌더링할 수 있습니다.

 

뷰포트 구조의 X, Y, 너비, 높이 요소에 사용되는 치수는 렌더링 대상 표면에서 뷰포트의 위치 및 차원을 정의합니다. 이러한 값은 표면의 왼쪽 위 모서리를 기준으로 하는 장면 좌표입니다.

Direct3D는 뷰포트 위치 및 차원을 사용하여 렌더링된 장면이 대상 표면의 적절한 위치에 오도록 꼭짓점을 조정합니다. 내부적으로 Direct3D는 이러한 값을 각 꼭짓점에 적용되는 다음 매트릭스에 삽입합니다.

![각 꼭짓점에 적용되는 매트릭스 수식](images/vpscale.png)

이 매트릭스는 뷰포트 차원 및 원하는 깊이 범위에 따라 꼭짓점의 크기를 조정하고 렌더링 대상 표면의 적절한 위치로 좌표 이동합니다. 또한 이 매트릭스는 y축 좌표를 돌려서 왼쪽 위 모서리에서 y축이 마이너스 방향으로 증가하는 화면 원점을 반영합니다. 이 매트릭스가 적용된 후에도 꼭짓점은 여전히 유형이 같습니다. 즉, \[x,y,z,w\] 꼭짓점으로 계속 존재하며, 래스터라이저로 꼭짓점을 전송하려면 다른 유형으로 변환해야 합니다.

**참고**  응용 프로그램 일반적으로 설정 MinZ 및 MaxZ 0.0 및 1.0 각각 시스템이 전체 깊이 범위를 렌더링 하도록 합니다. 그러나 특정 효과를 얻기 위해 다른 값을 사용해도 됩니다. 예를 들어 둘 다 0.0으로 설정하여 모든 개체를 포그라운드에 렌더링할 수도 있고, 둘 다 1.0으로 설정하여 모든 개체를 백그라운드에 렌더링할 수도 있습니다.

 

## <a name="span-idclearingaviewportspanspan-idclearingaviewportspanspan-idclearingaviewportspanclearing-a-viewport"></a><span id="Clearing_a_Viewport"></span><span id="clearing_a_viewport"></span><span id="CLEARING_A_VIEWPORT"></span>뷰포트 지우기


뷰포트를 지우면 렌더링 대상 표면에서 뷰포트 사각형의 내용이 초기화됩니다. 깊이 및 스텐실 버퍼 표면의 사각형이 지워질 수도 있습니다.

## <a name="span-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanset-up-the-viewport-for-clipping"></a><span id="Set_Up_the_Viewport_for_Clipping"></span><span id="set_up_the_viewport_for_clipping"></span><span id="SET_UP_THE_VIEWPORT_FOR_CLIPPING"></span>클리핑할 뷰포트 설정


프로젝션 매트릭스의 결과는 다음과 같이 프로젝션 공간의 클리핑 볼륨을 결정합니다.

-w<sub>c</sub>&lt;= x<sub>c</sub>&lt;= w<sub>c</sub>

-w<sub>c</sub>&lt;= y<sub>c</sub>&lt;= w<sub>c</sub>

0 &lt;= z<sub>c</sub>&lt;= w<sub>c</sub>

여기서 x, y, z, w는 프로젝션 변환이 적용된 후의 꼭짓점 좌표를 나타냅니다. 클리핑이 활성화되어 있는 경우(기본 동작) 이러한 범위 밖의 x, y 또는 z축 구성 요소를 포함한 꼭짓점은 클리핑됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 




