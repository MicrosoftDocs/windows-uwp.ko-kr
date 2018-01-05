---
title: "삼각형 보간"
description: "렌더링 중에 파이프라인은 각 삼각형에서 꼭짓점 데이터를 보간합니다."
ms.assetid: 1A76DD78-CED7-42BE-BA81-B9050CD3AF9B
keywords: "삼각형 보간"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 26705e9481a96d54eff70d04c004bf62fe049091
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="triangle-interpolation"></a>삼각형 보간


렌더링 중에 파이프라인은 각 삼각형에서 꼭짓점 데이터를 보간합니다. 꼭짓점 데이터는 광범위한 데이터로써 확산 색, 반사 색, 확산 알파(삼각형 불투명도), 반사 알파 및 포그 인수를 포함할 수 있습니다(이에 국한되지 않음). 프로그래밍 가능한 꼭짓점 파이프라인의 경우 포그 인수는 포그 레지스터에서 가져옵니다. 고정 함수 꼭짓점 파이프라인의 경우 포그 인수는 반사 알파에서 가져옵니다.

일부 꼭짓점 데이터의 경우 보간은 다음과 같이 현재 음영 모드에 따라 달라질 수 있습니다.

| 음영 모드 | 설명                                                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 평면         | 포그 인수만 평면 음영 모드에 보간됩니다. 그 외의 보간된 값의 경우 삼각형의 첫 번째 꼭짓점 색이 전체 표면에 적용됩니다. |
| 고우러드      | 선형 보간은 세 개의 꼭짓점 간에 모두 수행됩니다.                                                                                                               |

 

확산 색과 반사 색은 색 모델에 따라 다르게 처리됩니다. RGB 색 모델에서는 시스템이 보간에 빨강색, 녹색 및 파란색 색상 구성 요소를 사용합니다.

디바이스 드라이버가 텍스처 블렌딩이나 스티플링을 사용하는 등 두 가지 방법으로 투명도를 구현할 수 있으므로 색의 알파 구성 요소는 별도의 보간 값으로 처리됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 




