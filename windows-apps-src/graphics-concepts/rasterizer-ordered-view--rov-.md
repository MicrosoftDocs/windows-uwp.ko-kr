---
title: ROV(정렬된 래스터라이저 뷰)
description: 정렬된 래스터라이저 뷰는 깊이 버퍼의 일부 제한 사항, 특히 투명도가 포함된 여러 텍스처가 모두 동일한 픽셀에 적용되는 제한 사항을 해결할 수 있습니다.
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- ROV(정렬된 래스터라이저 뷰)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7327304e2b42ff5ff71be136220b58e99c6228d2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821877"
---
# <a name="rasterizer-ordered-view-rov"></a>ROV(정렬된 래스터라이저 뷰)


정렬된 래스터라이저 뷰는 깊이 버퍼의 일부 제한 사항, 특히 투명도가 포함된 여러 텍스처가 모두 동일한 픽셀에 적용되는 제한 사항을 해결할 수 있습니다.

정렬된 래스터라이저 뷰는 "OIT(Order Independent Transparency)" 알고리즘을 픽셀 렌더링에 적용할 수 있습니다. 깊이 버퍼는 픽셀을 그리거나 폐색할 수만 있고 투명도를 통한 부분적 폐색 개념이 없습니다. OIT 알고리즘은 투명한 텍스처를 올바른 순서로 적용하므로 예를 들어 투명한 텍스처를 사용하는 식물 뒤에 있는 유리창 뒤에 투명한 유리 개체가 나타나야 하는 경우, 예측 가능한 방식으로 최종 결과가 그려집니다. ROV 및 OIT 알고리즘이 없으면 이 투명 개체들이 그려지는 순서를 예측할 수 없고 렌더링된 장면은 혼란스럽고 엉뚱하게 됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[보기](views.md)

 

 




