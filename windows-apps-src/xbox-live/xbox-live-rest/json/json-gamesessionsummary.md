---
title: GameSessionSummary(JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
author: KevinAsgari
description: " GameSessionSummary(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 610f771641352447f9d38fc4217231ba3230e6fb
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4564603"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary(JSON)
게임 세션에 대 한 요약 데이터를 나타내는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
GameSessionSummary JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| creationTime| DateTime| 날짜 및 세션이 생성 된 시간을 utc 형식에서입니다. | 
| customData| 8 비트 부호 없는 정수로의 배열| 게임 관련 세션 데이터의 1024 바이트 수입니다. 이 값은 서버에 불투명 합니다. | 
| displayName| string| 디스플레이 이상과 세션 최대 128 자 길이의 합니다. 이 값은 서버에 불투명 합니다. | 
| hasEnded| 부울 값| 세션 종료 되 면 true이 고 false 그렇지 않은 경우. 추가 데이터를 세션에 전송 되지 않도록 방지 true 눈금 읽기 전용으로 게임 세션에이 필드를 설정 합니다. | 
| sessionId| 문자열 세션 id입니다. | 
| titleId| 32 비트 부호 없는 정수| 게임 세션을 만드는 제목 ID입니다.| 
| 변형| 32 비트 부호 있는 정수| 게임 변형 합니다. 이 값은 서버에 불투명 합니다.| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "hasEnded": false,
}
    
```

  
<a id="ID4ERD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ETD"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>참조 

[GameSession(JSON)](json-gamesession.md)

   