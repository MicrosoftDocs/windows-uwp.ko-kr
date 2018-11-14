---
title: SRV(셰이더 리소스 뷰) 및 UAV(순서기 지정되지 않은 액세스 뷰)
description: 셰이더 리소스 뷰는 일반적으로 셰이더가 텍스처에 액세스할 수 있는 형식으로 텍스처를 래핑합니다. 순서가 지정되지 않은 액세스 뷰는 유사한 기능을 제공하지만, 순서와 상관없이 텍스처(또는 다른 리소스)에 대한 읽기 및 쓰기를 허용합니다.
ms.assetid: 4505BCD2-0EDA-40F2-887C-EC081FE32E8F
keywords:
- 셰이더 리소스 뷰(SRV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e98b9942dfc14604c061a036cd3c9803abaf3915
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6451935"
---
# <a name="shader-resource-view-srv-and-unordered-access-view-uav"></a>SRV(셰이더 리소스 뷰) 및 UAV(순서가 지정되지 않은 액세스 뷰)


셰이더 리소스 뷰는 일반적으로 셰이더가 텍스처에 액세스할 수 있는 형식으로 텍스처를 래핑합니다. 순서가 지정되지 않은 액세스 뷰는 유사한 기능을 제공하지만, 순서와 상관없이 텍스처(또는 다른 리소스)에 대한 읽기 및 쓰기를 허용합니다.

단일 텍스처 래핑이 가장 단순한 형태의 셰이더 리소스 뷰입니다. 보다 복잡한 예는 하위 리소스(mipmap 텍스처의 개별 어레이, 평면 또는 색)의 모음, 3D 텍스처, 1D 텍스처 색 그라데이션 등이 될 것입니다.

순서가 지정되지 않은 뷰는 성능 면에서 약간 더 많은 비용이 들지만, 예를 들어 텍스처를 읽는 동시에 쓸 수 있습니다. 그러면 그래픽 파이프라인이 업데이트된 텍스처를 다른 용도로 재사용할 수 있습니다. 셰이더 리소스 뷰는 읽기 전용입니다(리소스의 가장 일반적으로 용도).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[보기](views.md)

 

 




