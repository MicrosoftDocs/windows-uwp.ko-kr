---
title: PersonSummary(JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
author: KevinAsgari
description: " PersonSummary(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33b1cb2adaafba2370e27eb98a10a5143166f0ce
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5741110"
---
# <a name="personsummary-json"></a>PersonSummary(JSON)
[Person (JSON)](json-person.md) 개체의 컬렉션입니다. 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
PersonSummary 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| 부울 값| 여부 호출자는 즐겨찾기로 대상을 표시 합니다. 예제 값: true| 
| hasCallerMarkedTargetAsKnown| 부울 값| 호출자가 것으로 표시 여부 대상 알려진 합니다. 예제 값: true| 
| isCallerFollowingTarget| 부울 값| 여부 호출자에 게 대상 팔 로우 합니다. 예제 값: true| 
| isTargetFollowingCaller| 부울 값| 여부 대상 호출자 팔 로우 합니다. 예제 값: true| 
| legacyFriendStatus| string| 호출자가 표시 된 대로 대상의 레거시 친구 상태입니다. "None", "MutuallyAccepted", "OutgoingRequest" 또는 "IncomingRequest" 될 수 있습니다. 예제 값: "MutuallyAccepted"| 
| recentChangeCount| 32 비트 부호 없는 정수| 선택 사항입니다. 대상의 소셜 그래프의 최근 변경의 수입니다. 이 값은 사용자가 자신의 요약을 보고 하는 경우에 존재 합니다. 예제 값: 5| 
| targetFollowerCount| > 32 비트 부호 없는 정수| 대상을 수행 하는 사용자의 수입니다. 예제 값: 1308| 
| targetFollowingCount| 32 비트 부호 없는 정수| 대상 팔 로우 하는 사용자의 수입니다. 예제 값: 112| 
| 워터 마크| string| 선택 사항입니다. 대상에 대 한 최근 변경 워터 마크입니다. 이 값은 사용자가 자신의 요약을 보고 하는 경우에 존재 합니다. 예제 값: 5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}
    
```

  
<a id="ID4EGE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIE"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   