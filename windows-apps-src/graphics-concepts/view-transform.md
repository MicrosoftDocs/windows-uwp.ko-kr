---
title: 변환 보기
description: 변환 보기는 월드 공간에서 뷰어를 배치하여 꼭짓점을 카메라 공간으로 변환합니다.
ms.assetid: DA4C2051-4C28-4ABF-8C06-241C8CB87F2F
keywords:
- 변환 보기
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 40f9312b4b85e9f6e54a8bf02c6d07df35b8b626
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6671510"
---
# <a name="view-transform"></a>변환 보기


*변환 보기*는 월드 공간에서 뷰어를 배치하여 꼭짓점을 카메라 공간으로 변환합니다. 카메라 공간에서 카메라 또는 뷰어는 원점에 있으며 양의 z축 방향을 바라봅니다. 뷰 매트릭스는 월드의 개체를 카메라의 위치(카메라 공간의 원점) 및 방향 주변으로 다시 배치합니다. Direct3D는 z축이 장면에 대해 양의 수가 되도록 왼손 좌표계를 사용합니다.

여러 가지 방법으로 뷰 매트릭스를 만들 수 있습니다. 어떤 경우에도 카메라는 월드 공간에서 장면의 모델에 적용되는 보기 매트릭스를 만들기 위한 시작점으로 사용되는 논리적 위치 및 방향을 갖습니다. 보기 매트릭스는 개체를 좌표 이동하고 회전하여 카메라 공간에 배치합니다. 여기서 카메라는 원점입니다. 보기 매트릭스를 만드는 한 가지 방법은 좌표 이동 매트릭스를 각 축의 회전 매트릭스와 결합하는 것입니다. 이 방법에서는 다음과 같은 일반 매트릭스 수식이 적용됩니다.

![보기 변환 수식](images/viewtran.png)

이 수식에서 V는 생성된 보기 매트릭스이고, T는 월드에서 개체를 다시 배치하는 좌표 이동 매트릭스이고, Rₓ~R<sub>z</sub>는 x, y, z축을 기준으로 개체를 회전하는 회전 매트릭스입니다. 좌표 이동 및 회전 매트릭스는 월드 공간에서 카메라의 논리적 위치 및 방향을 기반으로 합니다. 월드에서 카메라의 논리적 위치가 &lt;10,20,100&gt;인 경우 좌표 이동 매트릭스의 목표는 개체를 x축 방향으로 -10단위, y축 방향으로 -20단위, z축 방향으로 -100단위만큼 이동하는 것입니다. 수식의 회전 매트릭스는 카메라 공간의 축이 월드 공간과 어긋나게 회전된 정도라는 면에서 카메라의 방향을 기반으로 합니다. 예를 들어 앞에서 언급한 카메라가 바로 아래를 가리키면 다음 그림과 같이 z축은 월드 공간의 z축과 90도(pi/2 라디안) 어긋나 있습니다.

![월드 공간과 비교한 카메라의 보기 공간 그림](images/camtop.png)

회전 매트릭스는 장면의 모델에 크기는 같지만 방향이 반대인 회전을 적용합니다. 이 카메라의 보기 매트릭스는 x축을 기준으로 -90도 회전을 포함합니다. 회전 매트릭스를 좌표 이동 매트릭스와 결합하여 장면에서 개체의 위치와 방향을 조정하는 보기 매트릭스를 만들 수 있습니다. 그러면 꼭대기가 카메라를 향하기 때문에 카메라가 모델 위에 있는 모양을 제공합니다.

## <a name="span-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspansetting-up-a-view-matrix"></a><span id="Setting_Up_a_View_Matrix"></span><span id="setting_up_a_view_matrix"></span><span id="SETTING_UP_A_VIEW_MATRIX"></span>보기 매트릭스 설정


Direct3D는 월드 및 보기 매트릭스를 사용하여 여러 내부 데이터 구조를 구성합니다. 새로운 월드 또는 보기 매트릭스를 설정할 때마다 시스템에서 관련 내부 구조를 다시 계산합니다. 이러한 매트릭스를 자주 설정하면 계산에 많은 시간이 소요됩니다. 월드 및 보기 매트릭스를 월드 매트릭스로 설정한 월드-보기 매트릭스에 연결한 다음 보기 매트릭스를 ID로 설정하여 필요한 계산의 수를 최소화할 수 있습니다. 필요에 따라 월드 매트릭스를 수정하고 연결하고 초기화할 수 있도록 개별 월드 및 보기 매트릭스의 캐시된 복사본을 보관해 두세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[변환](transforms.md)

 

 




