---
title: AggregateSessionsResponse(JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
description: " AggregateSessionsResponse(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ef026fd5096d047b2014faaf95a667c69827e043
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608308"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse(JSON)
사용자의 적합성에 대 한 세션에 대 한 집계 된 데이터를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
AggregateSessionsResponse 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| totalDurationInSeconds| 64 비트 부호 있는 정수| 초 단위 집계 기간 동안 세션의 총 기간입니다.| 
| totalJoules| 64 비트 부호 있는 정수| 총 소비 되는 에너지를 줄 단위로-집계 기간 동안. | 
| totalSessions| 64 비트 부호 있는 정수| 집계 기간 동안 세션의 총 수입니다.| 
| weightedAverageMets| 단 정밀도 부동 소수점 숫자 | 집계 기간 동안 작업 (충족) 값의 가중치가 적용 된 평균 metabolic 해당 합니다. 충족 값 미사용 개인의 활동적인 상태로 되돌리는 metabolic 속도 기준으로 작업 하는 동안 개인의 활동적인 상태로 되돌리는 metabolic 속도 비율을입니다. 휴면 하는 것에 대 한 metabolic 속도가 개인의 가중치에 관계 없이 1.0 개인이 놓여 metabolic 속도 기준으로 충족 값 이기 때문에 다른 가중치 개인이 수행 중인 작업의 강도 따라 비교할 사용할 수 있습니다.| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   