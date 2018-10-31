---
title: 좌표계
description: 일반적으로 3D 그래픽 응용 프로그램은 두 가지 유형의 카티전 좌표계(왼손 또는 오른손) 중 하나를 사용합니다. 두 좌표계에서 양의 x-축은 오른쪽을 가리키고 양의 y-축은 위를 가리킵니다.
ms.assetid: 138D9B81-146F-4E9F-B742-1EDED8FBF2AE
keywords:
- 좌표계
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b8faed0719419d4be8ac1e5d493610ec2660598
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5813178"
---
# <a name="coordinate-systems"></a>좌표계


일반적으로 3D 그래픽 응용 프로그램은 두 가지 유형의 카티전 좌표계(왼손 또는 오른손) 중 하나를 사용합니다. 두 좌표계에서 양의 x-축은 오른쪽을 가리키고 양의 y-축은 위를 가리킵니다.

## <a name="span-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanleft-and-right-handed-coordinates"></a><span id="Left_and_right_handed_coordinates"></span><span id="left_and_right_handed_coordinates"></span><span id="LEFT_AND_RIGHT_HANDED_COORDINATES"></span>왼손 및 오른손 좌표


왼손이나 오른손 손가락을 양의 x 방향 쪽으로 하고 양의 y 방향으로 구부려서 양의 z-축이 가리키는 방향을 기억할 수 있습니다. 엄지가 가리키는 방향이 해당 좌표계에 대해 양의 z-축이 가리키는 방향입니다. 다음 그림은 이 두 좌표계를 보여 줍니다.

![왼손 및 오른손 카티전 좌표계의 그림](images/leftrght.png)

Direct3D는 왼손 좌표계를 사용합니다. 왼손 좌표와 오른손 좌표가 가장 일반적인 좌표계이지만 3D 소프트웨어에 사용되는 다른 다양한 좌표계가 있습니다. 예를 들어, 3D 모델링 응용 프로그램에서 x-축이 관찰자 쪽이나 그 반대쪽을 가리키고 z-축은 위를 가리키는 좌표계를 사용하는 것은 드물지 않습니다.

## <a name="span-idverticesandvectorsspanspan-idverticesandvectorsspanspan-idverticesandvectorsspanvertices-and-vectors"></a><span id="Vertices_and_vectors"></span><span id="vertices_and_vectors"></span><span id="VERTICES_AND_VECTORS"></span>꼭짓점 및 벡터


좌표계가 지정되면 x, y 및 z 좌표가 공간의 한 점("꼭짓점")이나 3D 방향("벡터")를 정의할 수 있습니다.

꼭짓점의 모음을 사용하여 선과 셰이프를 정의할 수 있습니다. 꼭짓점으로 정의할 수 있는 가장 단순한 개체를 [원형](primitives.md)이라고 하며 원형 집합으로 정의되는 더 복잡한 개체를 "메시"라고 합니다.

3D 좌표계에 정의된 메시에 대해 수행되는 기본 작업은 변환, 회전 및 크기 조정입니다. 이들 기본 변환을 결합하여 변환 매트릭스를 만들 수 있습니다. 자세한 내용은 [변환](transforms.md)을 참조하세요.

이러한 작업을 결합할 때 결과는 가환성이 없습니다. 매트릭스를 곱하는 순서가 중요합니다.

## <a name="span-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanporting-from-a-right-handed-coordinate-system"></a><span id="Porting_from_a_right-handed_coordinate_system"></span><span id="porting_from_a_right-handed_coordinate_system"></span><span id="PORTING_FROM_A_RIGHT-HANDED_COORDINATE_SYSTEM"></span>오른손 좌표계로 포팅


오른손 좌표계를 기반으로 하는 응용 프로그램을 포팅하는 경우 Direct3D에 전달되는 데이터에 대한 두 가지 변경이 필요합니다.

-   시스템이 앞쪽에서부터 시계 방향으로 트래버스하도록 삼각형 꼭짓점의 순서를 대칭 이동합니다. 즉, 꼭짓점이 v0, v1, v2일 경우 이를 Direct3D에 v0, v2, v1로 전달합니다.
-   세계 좌표 공간을 z 방향으로 -1씩 크기 조정하려면 뷰 매트릭스를 사용합니다. 이렇게 하려면 뷰 매트릭스에 사용하는 매트릭스 구조의 \_31, \_32, \_33 및 \_34 멤버의 부호를 대칭 이동합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 




