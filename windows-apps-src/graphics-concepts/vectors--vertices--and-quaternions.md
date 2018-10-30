---
title: 벡터, 꼭짓점 및 사원수
description: Direct3D 전체에서 꼭짓점은 위치 및 방향을 설명합니다. 기본 형식의 각 꼭짓점은 위치, 색, 텍스처 좌표 및 방향을 제공하는 일반적인 벡터로 설명됩니다.
ms.assetid: 94EC3D59-43FC-4509-A233-916E9FA8381E
keywords:
- 벡터, 꼭짓점 및 사원수
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2373d18b51015652bc1ef3035402e1da95a54abf
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5758137"
---
# <a name="vectors-vertices-and-quaternions"></a>벡터, 꼭짓점 및 사원수


Direct3D 전체에서 꼭짓점은 위치 및 방향을 설명합니다. 기본 형식의 각 꼭짓점은 위치, 색, 텍스처 좌표 및 방향을 제공하는 일반적인 벡터로 설명됩니다.

사원주는 three-component-vector를 정의하는 \[x, y, z\] 값에 네 번째 요소를 추가합니다. 사원수는 3D 회전에 일반적으로 사용되는 매트릭스 메서드의 대체 옵션입니다. 사원수는 3D 공간의 축 그리고 해당 축을 기준으로 하는 회전을 나타냅니다. 예를 들어 사원수는 (1,1,2) 축과 1 라디안의 회전을 나타낼 수 있습니다. 사원수는 중요한 정보를 제공하지만 정말로 중요한 점은 사원수에서 수행할 수 있는 구성과 보간이라는 두 가지 작업입니다.

사원수 구성 작업은 사원수 결합 작업과 비슷합니다. 두 사원수의 구성은 다음 그림과 같이 표기합니다.

![사원수 표기법 그림](images/quateq.png)

기하 도형에 적용되는 두 사원수의 구성은 "axis₂를 기준으로 기하 도형을 rotation₂만큼 회전한 후 axis₁을 기준으로 rotation₁만큼 회전"한다는 의미입니다. 이 예에서 Q는 단일 축 기준의 회전을 나타내며 기하 도형에 q₂를 적용한 다음 q₁을 적용한 결과입니다.

사원수 보간을 사용하면 응용 프로그램이 한 축과 방향에서 다른 축과 방향으로 원활하고 합리적으로 이동하는 경로를 계산할 수 있습니다. 따라서 q₁과 q₂ 사이의 보간은 한 방향에서 다른 방향으로 애니메이션하는 간단한 방법을 제공합니다.

컴퍼지션과 보간을 함께 사용하면 복잡하게 표시되는 방식으로 기하 도형을 조작할 수 있는 간단한 방법을 제공합니다. 예를 들어 특정 방향으로 회전하려는 기하 도형이 있다고 가정합시다. 여러분은 이 기하 도형을 axis₂ 기준으로 r₂도 회전한 다음 axis₁ 기준으로 r₁도 회전하려 한다는 사실은 알고 있지만 최종 사원수는 모릅니다. 컴퍼지션을 사용하여 기하 도형의 두 회전을 결합하면 그 결과물인 단일 사원수를 얻을 수 있습니다. 그런 다음 원점에서 구성된 사원수까지 보간하여 원활하게 변환할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 




