---
title: 월드 변환
description: 월드 변환은 모델 공간에서 좌표를 변경합니다. 여기서 꼭짓점은 모델의 로컬 원점인 월드 공간을 기준으로 정의됩니다.
ms.assetid: 767B032C-69D0-4583-8FEB-247F4C41E31D
keywords:
- 월드 변환
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9c6ada4bd964a9430d6ef47bd46f954ca76c404a
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6266328"
---
# <a name="world-transform"></a>월드 변환


월드 변환은 모델 공간에서 좌표를 변경합니다. 여기서 꼭짓점은 모델의 로컬 원점인 월드 공간을 기준으로 정의됩니다. 월드 공간에서 꼭짓점은 장면에 있는 모든 개체에 대해 일반적인 원점을 기준으로 정의됩니다. 월드 변환은 모델을 월드에 배치합니다.

## <span id="What_Is_a_World_Transform"></span><span id="what_is_a_world_transform"></span><span id="WHAT_IS_A_WORLD_TRANSFORM"></span>


다음 다이어그램은 월드 좌표계와 모델의 로컬 좌표계 간의 관계를 보여 줍니다.

![월드 좌표와 로컬 좌표의 관계 다이어그램](images/worldcrd.png)

월드 변환에는 좌표 이동, 회전 및 크기 조정이 포함될 수 있습니다.

## <a name="span-idsettingupaworldmatrixxmlspansetting-up-a-world-matrix"></a><span id="SETTING_UP_A_WORLD_MATRIX.XML"></span>월드 매트릭스 설정


다른 변환과 마찬가지로 일련의 매트릭스를 연결하여 각 매트릭스의 모든 효과가 포함된 단일 매트릭스를 만들 수 있습니다. 가장 간단한 사례에서, 모델이 월드 원점에 있고 해당 로컬 좌표축이 월드 공간과 동일한 방향이면 월드 매트릭스는 ID 매트릭스입니다. 보다 일반적으로 월드 매트릭스는 공간으로의 좌표 이동과 모델을 필요한 만큼 회전하는 하나 이상의 회전의 조합입니다.

Direct3D는 여러 내부 데이터 구조를 구성하기 위해 설정한 월드 매트릭스와 보기 매트릭스를 사용합니다. 새로운 월드 매트릭스 또는 보기 매트릭스를 설정할 때마다 시스템에서 관련 내부 구조를 다시 계산합니다. 이러한 매트릭스를 자주 설정하면(예: 프레임당 수천 번) 계산에 많은 시간이 소요됩니다. 월드 매트릭스 및 보기 매트릭스를 월드 매트릭스로 설정한 월드 보기 매트릭스에 연결한 다음 보기 매트릭스를 ID로 설정하면 필요한 계산의 수를 최소화할 수 있습니다. 필요에 따라 월드 매트릭스를 수정하고 연결하고 초기화할 수 있도록 개별 월드 및 보기 매트릭스의 캐시된 복사본을 보관해 두세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[변환](transforms.md)

 

 




