---
title: 이방성 텍스처 필터링
description: 이방성은 표면 방향이 화면의 평면 기준 각도인 3D 개체의 텍셀에 표시되는 왜곡입니다. 이방성 기본 객체의 픽셀을 텍셀로 매핑할 때는 그 형상이 왜곡됩니다.
ms.assetid: 58923809-EF76-4C16-BCE7-922A66425F83
keywords:
- 이방성 텍스처 필터링
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: efac0a9454f750d4b9040577b613496d29a30bc3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591198"
---
# <a name="anisotropic-texture-filtering"></a>이방성 텍스처 필터링


*이방성*은 표면 방향이 화면의 평면 기준 각도인 3D 개체의 텍셀에 표시되는 왜곡입니다. 이방성 기본 객체의 픽셀을 텍셀로 매핑할 때는 그 형상이 왜곡됩니다. Direct3D는 텍스처 공간에 역으로 매핑되는 화면 픽셀의 이각으로 픽셀의 이방성을 측정합니다(즉, 길이를 너비로 나눔).

이방성 텍스처 필터링을 선형 텍스처 필터링이나 MIP 맵 텍스처 필터링과 함께 사용하여 렌더링 결과를 개선할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[질감 필터링](texture-filtering.md)

 

 




