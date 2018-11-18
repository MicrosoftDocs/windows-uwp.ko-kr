---
title: AggregateSessionsResponse(JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
author: KevinAsgari
description: " AggregateSessionsResponse(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 590acdd89d83fa21a401b5573053ba341f7f05c7
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7166254"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse(JSON)
사용자의 피트 니스 세션에 대 한 집계 데이터를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
AggregateSessionsResponse 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| totalDurationInSeconds| 64 비트 부호 있는 정수| 집계 기간 (초)에서 세션의 총 기간입니다.| 
| totalJoules| 64 비트 부호 있는 정수| 총 에너지 굽거나-줄 단위로-집계 기간 동안 합니다. | 
| totalSessions| 64 비트 부호 있는 정수| 집계 기간 동안 세션의 총 수입니다.| 
| weightedAverageMets| 단 정밀도 부동 소수점 숫자 | 집계 기간 동안 작업 (충족) 값의가 중된 평균 활동적인 상태로 되돌리는 metabolic 해당 합니다. 충족 값은 개인의 미사용 활동적인 상태로 되돌리는 metabolic 속도 기준으로 작업 하는 동안 개인의 활동적인 상태로 되돌리는 metabolic 속도의 비율입니다. Resting에 대 한 활동적인 상태로 되돌리는 metabolic 속도가 개인의 두께 관계 없이 1.0이 고 충족 값은 개인의 resting 활동적인 상태로 되돌리는 metabolic 속도 기준으로 하기 때문에 다른 가중치의 개인이 수행 중인 작업의 강도 비교할 사용할 수 있습니다.| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
   "totalSessions" : 300,
   "totalDurationInSeconds" : 1240,
   "totalJoules" :  21600,
   "weightedAvgMet" : 21,
}

    
```

  
<a id="ID4E2C"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E4C"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   