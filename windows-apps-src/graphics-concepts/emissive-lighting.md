---
title: 발광 조명
description: '발광 조명은 개체(예: 불빛)에서 발산하는 빛입니다.'
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- 발광 조명
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ce2082479261cd96fb1c5bafd5f2df06bf6f239
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5970220"
---
# <a name="emissive-lighting"></a>발광 조명


*발광 조명*은 개체(예: 불빛)에서 발산하는 빛입니다. 발광은 렌더링된 개체가 자체 발광하는 것처럼 보이게 합니다. 발광은 개체의 색에 영향을 미칩니다. 예를 들어 어두운 재료를 밝게 만들고 발산되는 색을 부분적으로 띠게 만들 수 있습니다.

발광 조명은 한 용어로 설명됩니다.

발광 조명 = Cₑ

각 항목은 다음을 의미합니다.

| 매개 변수 | 기본값 | 유형                                                                 | 설명     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | 빨간색, 녹색, 파란색 및 알파 투명도(모두 부동 소수점 값) | 발광 색입니다. |

 

Cₑ에 대한 값은 색 1 또는 색 2입니다. 꼭짓점 색을 입력하지 않으면 물체 발광 색이 사용됩니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


이 예에서는 장면 주변 조명과 물체 주변 색이 개체에 지정됩니다.

수식에 따라 개체 꼭짓점의 결과 색은 물체의 색입니다.

다음 그림은 물체 색인 녹색을 보여 줍니다. 발광 조명은 모든 개체 꼭짓점을 같은 색으로 비춥니다. 꼭짓점 법선이나 광원 방향의 영향을 받지 않습니다. 그 결과로, 개체 표면 주변에 음영 차이가 없기 때문에 구가 2D 원처럼 보입니다.

![녹색 구 그림](images/lighte.jpg)

다음 그림은 발광 조명이 세 가지 다른 종류의 조명과 혼합되는 방법을 보여 줍니다. 구 오른쪽에서 녹색 발광과 빨간색 주변 조명이 혼합됩니다. 구의 왼쪽에서 녹색 발광 조명과 빨간색 주변 조명 및 빨간색 그라데이션을 생성하는 확산 조명이 혼합됩니다. 반사 하이라이트는 중앙의 흰색이며 반사 조명 값이 주변 조명에서 급격히 멀어지며 떨어져 노란색 링을 만들고, 확산 및 발광 조명 값이 함께 혼합되어 노란색을 만듭니다.

![발광 조명이 있는 녹색 구 그림](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




