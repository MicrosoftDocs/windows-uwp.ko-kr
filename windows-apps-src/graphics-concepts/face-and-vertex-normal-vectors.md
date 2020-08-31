---
title: 면 및 꼭짓점 법선 벡터
description: 메시의 각 면에는 수직 단위인 unit 법선 벡터가 있습니다. 벡터의 방향은 꼭지점이 정의 되는 순서와 좌표계가 오른쪽 인지 또는 왼손 인지에 따라 결정 됩니다.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- 면 및 꼭짓점 법선 벡터
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ef0d3ea5a3bc0f5c4ac6b6b660dc543919d297ec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168167"
---
# <a name="face-and-vertex-normal-vectors"></a>면 및 꼭짓점 법선 벡터


메시의 각 면에는 수직 단위인 unit 법선 벡터가 있습니다. 벡터의 방향은 꼭지점이 정의 되는 순서와 좌표계가 오른쪽 인지 또는 왼손 인지에 따라 결정 됩니다.

## <a name="span-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Front face의 수직 유닛 법선 벡터


메시의 각 면에는 수직 단위인 unit 법선 벡터가 있습니다. 벡터의 방향은 꼭지점이 정의 되는 순서와 좌표계가 오른쪽 인지 또는 왼손 인지에 따라 결정 됩니다. 얼굴 법선은 면의 전면에서 떨어져 있습니다. Direct3D에서는 면의 front만 표시 됩니다. Front face는 꼭 짓 점이 시계 방향으로 정의 된 것입니다.

다음 그림은 전면 표면의 법선 벡터를 보여 줍니다.

![전면 앞면의 법선 벡터입니다.](images/nrmlvect.png)

## <a name="span-idculling_back_facesspanspan-idculling_back_facesspanspan-idculling_back_facesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>고르기 뒷면 얼굴


정면 얼굴이 아닌 얼굴은 뒷면입니다. Direct3D는 항상 뒷면을 렌더링 하지 않습니다. 뒷면 얼굴을 추출 이라고 합니다. 뒤로 얼굴 고르기은 렌더링에서 얼굴을 제거 함을 의미 합니다. 원할 경우 고르기 모드를 변경 하 여 뒷면을 렌더링할 수 있습니다. 자세한 내용은 [고르기 상태](/windows/desktop/direct3d9/culling-state) 를 참조 하세요.

## <a name="span-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>꼭 짓 점 단위 법선


Direct3D에서는 고우러드 음영, 조명 및 질감 효과를 위해 꼭 짓 점 단위 법선을 사용 합니다.

다음 그림에서는 버텍스 법선을 보여 줍니다.

![꼭 짓 점 법선](images/vertnrml.png)

고우러드 음영을 다각형에 적용 하는 경우 Direct3D는 꼭 짓 점 법선을 사용 하 여 광원과 표면 사이의 각도를 계산 합니다. 꼭 짓 점에 대 한 색 및 강도 값을 계산 하 고 모든 기본 표면의 모든 지점에 대해 보간 합니다. Direct3D는 각도를 사용 하 여 밝기 강도 값을 계산 합니다. 각도가 클수록 조명이 표면에 비추는 것입니다.

## <a name="span-idflat_surfacesspanspan-idflat_surfacesspanspan-idflat_surfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>플랫 표면


플랫 개체를 만드는 경우 꼭 짓 점 법선을 표면에 수직인 점으로 설정 합니다.

다음 그림에서는 꼭 짓 점 법선이 있는 두 개의 삼각형으로 구성 된 평면을 보여 줍니다.

![꼭 짓 점 법선이 있는 두 개의 삼각형으로 구성 된 평면](images/flatvert.png)

## <a name="span-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>플랫 개체가 아닌 개체에 부드러운 음영 적용


플랫 개체 대신 개체가 삼각형 스트립으로 구성 되 고 삼각형이 동일 하지 않을 가능성이 높습니다. 스트립의 모든 삼각형에서 부드러운 음영을 적용 하는 한 가지 간단한 방법은 먼저 꼭지점이 연결 된 각 다각형 표면의 표면 법선 벡터를 계산 하는 것입니다. 꼭 짓 점 법선을 설정 하 여 각 표면에 동일한 각도를 만들 수 있습니다. 그러나이 메서드는 복잡 한 기본 형식에 대해 효율성이 충분 하지 않을 수 있습니다.

이 메서드는 위의 두 표면이 표시 되는 다음 다이어그램에 설명 되어 있습니다. S1 및 s 2의 법선 벡터는 파란색으로 표시 됩니다. 꼭 짓 점 법선 벡터는 빨강으로 표시 됩니다. S1의 표면 법선에서 사용 하는 꼭 짓 점 법선 벡터의 각도는 s 1의 보통 꼭지점과 표면의 법선의 각도와 동일 합니다. 이러한 두 서피스가 고우러드 음영이 적용 되 고 회색으로 표시 되 면 그 결과는 그 사이에 매끄러운 음영이 있는 가장자리가 됩니다.

다음 그림에서는 두 개의 표면 (S1 및 S2)과 표준 벡터 및 꼭 짓 점 법선 벡터를 보여 줍니다.

![두 개의 표면 (s1 및 s2) 및 법선 벡터와 꼭 짓 점 법선 벡터](images/gvert.png)

꼭 짓 점 법선이 연결 된 면 중 하나를 향해 기울어집니다 면 광원을 사용 하는 각도에 따라 조명 강도가 해당 표면의 점에 대해 증가 하거나 감소 합니다. 다음 다이어그램에서는 예제를 보여 줍니다. 이러한 표면은 가장자리에 표시 됩니다. 꼭 짓 점 법선은 S1에 대해 가장 작은 각도를 사용 하 여 꼭 짓 점 법선이 표면 법선과 동일한 각도를 갖는 경우 보다 더 작은 각도를 광원으로 기울어집니다 합니다.

다음 그림에서는 하나의 면으로 기울어집니다는 꼭 짓 점 법선 벡터를 사용 하는 두 개의 표면 (S1 및 S2)을 보여 줍니다.

![한 면에 기울어집니다 하는 꼭 짓 점 법선 벡터를 사용 하는 두 개의 표면 (s1 및 s2)](images/gvert2.png)

## <a name="span-idsharp_edgesspanspan-idsharp_edgesspanspan-idsharp_edgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>날카로운 가장자리


고우러드 음영을 사용 하면 선명한 가장자리가 있는 3D 장면에 일부 개체를 표시할 수 있습니다. 이렇게 하려면 날카로운 가장자리가 필요한 면의 모든 교차점에서 꼭 짓 점 법선 벡터를 복제 합니다.

다음 그림은 선명한 가장자리에서 중복 된 꼭 짓 점 법선 벡터를 보여 줍니다.

![날카로운 가장자리의 중복 꼭 짓 점 법선 벡터](images/shade1.png)

DrawPrimitive 메서드를 사용 하 여 장면을 렌더링 하는 경우 삼각형 스트립이 아닌 삼각형 목록으로 날카로운 가장자리를 사용 하 여 개체를 정의 합니다. 개체를 삼각형 스트립으로 정의 하면 Direct3D는이를 여러 삼각형 면으로 구성 된 단일 다각형으로 처리 합니다. 고우러드 음영은 다각형의 각 면과 인접 한 얼굴 사이에서 적용 됩니다.

결과는 표면에서 표면으로 부드럽게 음영 처리 되는 개체입니다. 삼각형 목록은 여러 개의 비연속 삼각형 면으로 구성 된 다각형 이므로 Direct3D는 다각형의 각 면에서 고우러드 음영을 적용 합니다. 그러나 면에서 face로 적용 되지 않습니다. 삼각형 목록의 삼각형 두 개 이상이 인접해 있으면 두 삼각형 사이에 가장자리가 선명 하 게 표시 됩니다.

또 다른 대안은 가장자리가 선명한 개체를 렌더링할 때 플랫 음영으로 변경 하는 것입니다. 이는 가장 효율적인 방법입니다. 하지만 고우러드 된 개체 처럼 현실적으로 렌더링 되지 않는 개체를 장면의 개체에 초래할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 