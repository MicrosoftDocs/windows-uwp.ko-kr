---
title: 단색 조명 맵
description: 이전 3D 가속기 보드가 대상 픽셀의 알파 값을 사용하는 텍스처 혼합을 지원하지 않는 경우, 단색 조명 매핑을 사용하면 이전 어댑터가 멀티패스 텍스처 혼합을 수행할 수 있습니다.
ms.assetid: 60F8F8F6-9DB7-452B-8DC0-407FFAA4BFE1
keywords:
- 단색 조명 맵
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b81838393d7b2692e6fd04b7ce535f58dc773780
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7839967"
---
# <a name="monochrome-light-maps"></a>단색 조명 맵


이전 3D 가속기 보드가 대상 픽셀의 알파 값을 사용하는 텍스처 혼합을 지원하지 않는 경우, 단색 조명 매핑을 사용하면 이전 어댑터가 멀티패스 텍스처 혼합을 수행할 수 있습니다.

일부 이전 3D 가속기 보드는 대상 픽셀의 알파 값을 사용하는 텍스처 혼합을 지원하지 않습니다. 이러한 어댑터는 일반적으로 다중 텍스처 혼합 역시 지원하지 않습니다. 이 같은 어댑터에서 응용 프로그램을 실행 중인 경우, 멀티패스 텍스처 혼합을 사용하여 단색 조명 매핑을 수행할 수 있습니다.

응용 프로그램은 단색 조명 매핑을 수행하기 위해 조명 맵 텍스처의 알파 데이터에 조명 정보를 저장합니다. 응용 프로그램은 Direct3D의 텍스처 필터링 기능을 사용하여 원형의 이미지에 있는 각 픽셀에서부터 조명 맵의 해당 텍셀에 이르기까지 매핑을 수행합니다. 소스 혼합 요소는 해당 텍셀의 알파 값으로 설정합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처와 조명 매핑](light-mapping-with-textures.md)

 

 




