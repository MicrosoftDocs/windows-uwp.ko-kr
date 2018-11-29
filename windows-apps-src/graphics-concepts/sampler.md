---
title: 샘플러
description: 샘플링은 텍스처 또는 기타 리소스로부터 입력 값을 읽는 프로세스입니다. \ 0034;샘플러\ 0034;는 리소스로부터 읽는 객체입니다.
ms.assetid: 7ECE13BB-9FC5-44A3-B1B2-2FE163F1D627
keywords:
- 샘플러
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e80e160e1225e510ab95f52cbd9f21049c890457
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7975565"
---
# <a name="sampler"></a>샘플러


샘플링은 텍스처 또는 기타 리소스로부터 입력 값을 읽는 프로세스입니다. "샘플러"는 리소스로부터 읽는 객체입니다.

텍스처로부터 샘플링하여 화면 영역으로 렌더링할 때 많은 문제 및 아티팩트가 발생합니다. 예를 들어 렌더링할 영역은 50 x 50픽셀이고 텍스처는 16 x 16픽셀 또는 256 x 256픽셀일 경우 상당한 텍스처 확장 또는 축소를 적용해야 합니다. 이 크기 불일치는 원치 않는 아티팩트로 이어지므로 이러한 아티팩트를 완화하기 위해 필터링 기법을 사용합니다. 큰 렌더링 영역에서 작은 텍스처를 사용하기 위한 일반적인 필터링 기법은 "쌍선형" 필터링입니다.

렌더링할 영역이 뷰에 대해 큰 사각을 이루는 경우(예: 뷰 앵글 때문에 256 x 256픽셀 텍스처를 너비가 100픽셀이지만 깊이가 5픽셀에 불과한 영역으로 렌더링하는 경우) 또 다른 문제가 발생합니다. 이 경우 "이방성" 필터링이 자주 적용됩니다. 이방성 필터링은 과도한 흐림 없이 앨리어싱 효과를 제거하므로 쌍선형보다 뛰어난 이미지 품질을 제공하지만 계산 비용은 더 많이 듭니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[보기](views.md)

 

 




