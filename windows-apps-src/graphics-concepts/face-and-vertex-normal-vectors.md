---
title: 면 및 꼭짓점 법선 벡터
description: 메시의 각 면에는 수직 단위 법선 벡터가 있습니다. 벡터의 방향은 꼭짓점이 정의된 순서와 좌표계가 오른손용인지 왼손용인지에 따라 결정됩니다.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- 면 및 꼭짓점 법선 벡터
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2081e8c09a6f6fd75f460af3f339902bcb80bac6
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6449444"
---
# <a name="face-and-vertex-normal-vectors"></a>면 및 꼭짓점 법선 벡터


메시의 각 면에는 수직 단위 법선 벡터가 있습니다. 벡터의 방향은 꼭짓점이 정의된 순서와 좌표계가 오른손용인지 왼손용인지에 따라 결정됩니다.

## <a name="span-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>앞면에 대한 수직 단위 법선 벡터


메시의 각 면에는 수직 단위 법선 벡터가 있습니다. 벡터의 방향은 꼭짓점이 정의된 순서와 좌표계가 오른손용인지 왼손용인지에 따라 결정됩니다. 면 법선 지점은 면의 앞면에서 떨어져 있습니다. Direct3D에서는 면의 앞면만 표시됩니다. 앞면은 꼭짓점이 시계 방향 순서로 정의되는 면입니다.

다음 그림에서는 앞면에 대한 법선 벡터를 보여줍니다.

![앞면에 대한 법선 벡터](images/nrmlvect.png)

## <a name="span-idcullingbackfacesspanspan-idcullingbackfacesspanspan-idcullingbackfacesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>뒷면 컬링


앞면이 아닌 모든 면은 뒷면입니다. Direct3D는 뒷면을 렌더링하지 않습니다. 뒷면은 컬링된다고 합니다. 뒷면 컬링은 렌더링에서 뒷면을 제거하는 것을 의미합니다. 원하는 경우 뒷면을 렌더링하도록 컬링 모드를 변경할 수 있습니다. 자세한 내용은 [컬링 상태](https://msdn.microsoft.com/library/windows/desktop/bb204882)를 참조하세요.

## <a name="span-idvertexunitnormalsspanspan-idvertexunitnormalsspanspan-idvertexunitnormalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>꼭짓점 단위 법선


Direct3D는 고우러드 음영, 조명, 텍스처링 효과를 위해 꼭짓점 단위 법선을 사용합니다.

다음 그림은 꼭짓점 법선을 보여줍니다.

![꼭짓점 법선](images/vertnrml.png)

다각형에 고우러드 음영을 적용할 때, Direct3D는 꼭짓점 법선을 사용하여 광원과 표면 사이의 각도를 계산합니다. 꼭짓점에 대한 색상과 강도 값을 계산한 다음, 모든 기본 요소의 표면에 걸친 모든 지점을 보간합니다. Direct3D는 각도를 사용하여 조명 강도 값을 계산합니다. 각도가 클수록 더 적은 빛이 표면에서 빛나게 됩니다.

## <a name="span-idflatsurfacesspanspan-idflatsurfacesspanspan-idflatsurfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>평평한 표면


평평한 개체를 만드는 경우 꼭짓점 법선이 표면에 수직인 지점을 가리키도록 설정합니다.

다음 그림은 두 삼각형과 꼭짓점 법선으로 구성된 평평한 표면을 보여줍니다.

![두 개의 삼각형과 꼭짓점 법선으로 구성된 평평한 표면](images/flatvert.png)

## <a name="span-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>평평하지 않은 개체에 부드러운 음영 적용


개체는 평평하지 않고 삼각형 스트립과 동일 평면에 있지 않은 삼각형으로 만들어지는 경우가 많습니다. 스트립의 모든 삼각형에 걸쳐 부드러운 음영을 적용하는 간단한 한 가지 방법은, 꼭짓점이 연결된 각 다각형 표면에 대한 표면 법선 벡터를 먼저 계산하는 것입니다. 꼭짓점 법선은 각 표면 법선과 같은 각도를 가지도록 설정할 수 있습니다. 그러나 이 방법은 복잡한 기본 요소에는 효율이 충분하지 않을 수 있습니다.

이 방법이 다음 다이어그램에 나와 있습니다. S1과 S2라는 두 면이 있고, 경계가 위에 있습니다. S1과 S2의 법선 벡터는 파란색으로 표시되어 있습니다. 꼭짓점 법선 벡터는 빨간색으로 표시되어 있습니다. 꼭짓점 법선 벡터와 S1의 표면 법선이 이루는 각도는 꼭짓점 법선과 S2의 표면 법선이 이루는 각도와 동일합니다. 이러한 두 표면을 비추고 고우러드 음영을 적용하면, 부드럽게 음영이 적용되고 모서리가 부드럽게 처리된 결과를 얻을 수 있습니다.

다음 그림은 두 표면(S1과 S2)과 그 법선 벡터 및 꼭짓점 법선 벡터를 보여줍니다.

![두 표면(s1 and s2)과 그 법선 벡터 및 꼭짓점 법선 벡터](images/gvert.png)

꼭짓점 법선이 연결된 한 면 쪽으로 기울면, 해당 표면의 지점이 광원과 이루는 각도에 따라 조명 강도가 늘어나거나 줄어들게 됩니다. 다음 다이어그램이 그 예입니다. 이러한 표면의 가장자리가 위에 있습니다. 꼭짓점 법선이 S1 쪽으로 기울어, 꼭짓점 법선이 표면 법선과 동일한 각도를 가질 때보다 광원과의 각도가 더 작아집니다.

다음 그림은 꼭짓점 법선 벡터가 한 면으로 기운 두 표면(S1과 S2)을 보여줍니다.

![꼭짓점 법선 벡터가 한 면으로 기운 두 표면(s1과 s2)](images/gvert2.png)

## <a name="span-idsharpedgesspanspan-idsharpedgesspanspan-idsharpedgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>예리한 모서리


고우러드 음영을 사용하여 3D 장면에 가장자리가 예리한 일부 개체를 표시할 수 있습니다. 이렇게 하려면 예리한 가장자리가 필요한 면의 모든 교차 지점에서 꼭짓점 법선 벡터를 복제합니다.

다음 그림은 예리한 모서리에서 복제된 꼭지점 법선 벡터를 보여줍니다.

![예리한 모서리에서 복제된 꼭지점 법선 벡터](images/shade1.png)

DrawPrimitive 메서드를 사용하여 장면을 렌더링할 경우, 예리한 모서리를 가진 개체를 삼각형 스트립이 아닌 삼각형 목록에 정의하십시오. 개체를 삼각형 스트립으로 정의하면 Direct3D는 이를 여러 삼각형 면으로 구성된 하나의 다각형으로 취급합니다. 고우러드 음영이 다각형의 교차하는 각 면과 인접한 면 사이에 모두 적용됩니다.

그 결과로 개체의 면에서 면 사이가 부드럽게 음영 처리됩니다. 삼각형 목록은 일련의 비연속 삼각형 면으로 구성된 다각형이므로, Direct3D는 고우러드 음영을 다각형의 각 면에 적용합니다. 그러나 면에서 면 사이에는 적용되지 않습니다. 삼각형 목록에서 두 개 이상의 삼각형이 인접한 경우, 그 사이의 가장자리가 예리하게 표시됩니다.

또 다른 방법은 예리한 가장자리를 가진 개체를 렌더링할 때 평평한 음영으로 변경하는 것입니다. 이 방법이 계산상 가장 효율적인 방법이지만, 개체에 고우러드 음영이 적용되지 않아 장면의 개체가 현실적으로 렌더링되지 않는 결과가 발생할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 




