---
title: SessionEntry(JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
author: KevinAsgari
description: " SessionEntry(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a8ec59202300912d4646b0e759e7bbf037853608
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5825844"
---
# <a name="sessionentry-json"></a>SessionEntry(JSON)
피트 니스 세션에 대 한 데이터를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
SessionEntry 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| durationInSeconds| 32 비트 부호 있는 정수 | 기간-초에서-세션의 합니다. | 
| 줄| 32 비트 부호 있는 정수 | 에너지-줄 단위로-세션에 기록 합니다. | 
| 충족| 단 정밀도 부동 소수점 숫자| 평균 세션 기간 동안 값을 충족 합니다. 충족 값 미사용 개인의 metabolic 속도 기준으로 작업 하는 동안 개인의 metabolic 속도의 비율을입니다. Metabolic 속도 resting에 대 한 개인의 두께 관계 없이 1.0 있고 개인의 resting metabolic 속도 기준으로 충족 값은 다른 가중치 개인이 수행 중인 작업의 강도 비교할 사용할 수 있습니다.| 
| serverTimestamp| DateTime| 시간-UTC를 기반으로-서버에서 항목을 입력 합니다. | 
| 원본| 8 비트 부호 없는 정수| 세션 소스입니다.| 
| 타임 스탬프| DateTime| 시간-utc (협정 세계시)에 따라-클라이언트에서 항목을 만들었습니다. | 
| titleId| 64 비트 부호 없는 정수| 제목-10 진수에서-항목을 만든입니다.| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
   "titleId" : "1234567",
   "timestamp" : "2011-11-18T08:08:46Z",
   "serverTimestamp" : "2011-11-20T04:04:23Z",
   "durationInSeconds" : 240,
   "joules" :  1600,
   "met" :  "124"
   "source" :  "1"
}
    
```

  
<a id="ID4EOE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQE"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   