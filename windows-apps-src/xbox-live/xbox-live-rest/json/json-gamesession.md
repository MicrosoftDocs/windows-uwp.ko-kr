---
title: GameSession(JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
author: KevinAsgari
description: " GameSession(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a62dd48857ed9c7ba8601d5ca08179c6a32b0db5
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7152204"
---
# <a name="gamesession-json"></a>GameSession(JSON)
멀티 플레이 세션에 대 한 게임 데이터를 나타내는 JSON 개체입니다. 
<a id="ID4ER"></a>

  
 
GameSession JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| creationTime| DateTime| 날짜 및 세션이 생성 된 시간을 utc 형식에서입니다. | 
| customData| 8 비트 부호 없는 정수로의 배열| 게임 관련 세션 데이터의 1024 바이트 수입니다. 이 값은 서버에 불투명입니다. | 
| displayName| string| 디스플레이 게임 이름입니다 세션 최대 길이가 128 자입니다. 이 값은 서버에 불투명입니다. | 
| hasEnded| 부울 값| 세션 종료 되 면 true이 고 false 그렇지 않은 경우. 추가 데이터를 세션에 전송 되지 않도록 방지 true 눈금 읽기 전용으로 게임 세션에이 필드를 설정 합니다. | 
| isClosed| 부울 값| True 이면 없는 더 많은 플레이어 수, 추가 되 고 false 그렇지 않으면 고 세션이 닫힙니다. 이 값이 true 이면 세션에 참가할 요청이 거부 됩니다. | 
| maxPlayers| 32 비트 부호 있는 정수| 세션을 동시에 있을 수 있는 플레이어의 최대 수입니다. 값의 범위는 2 16입니다. 세션 플레이어의 최대 수 있으면 되 면 추가 세션에 참가할 요청이 거부 됩니다. | 
| playersCanBeRemovedBy| PlayerAcl| 세션에서 다른 플레이어를 제거할 수 있는 플레이어를 나타내는 값입니다. 가능한 값은 NoOne, 자체를 및 AnyPlayer 합니다. | 
| 명단| 플레이어 개체의 배열| 세션에서 플레이어의 배열입니다. 현재 및 사용자 세션에서 이전에 있었습니다. 하지만 남긴 플레이어 명단에 포함 되어 있습니다. 플레이어 명단의 순서 변경 되지 않습니다. 새 플레이어는 배열의 끝에 추가 됩니다. | 
| seatsAvailable| 32 비트 부호 있는 정수| 플레이어의 최대 수에 도달 하기 전에 세션에 가입할 수 있는 플레이어의 수입니다. 이 값은 읽기 전용 이며 항상 maxPlayers 필드의 값 보다 작은 합니다. | 
| sessionId| string| 세션을 만들면는 MPSD에서 할당 세션 ID입니다. 세션에 저장 된 정보에 액세스할 때이 값은 일반적으로 URI에 포함 됩니다.| 
| titleId| 32 비트 부호 없는 정수| 게임 세션을 만드는 제목의 ID입니다.| 
| 변형| 32 비트 부호 있는 정수| 게임 변형 합니다. 이 값은 서버에 불투명입니다.| 
| visibility| VisibilityLevel| 세션의 표시 여부를 나타내는 값입니다. 가능한 값은: PlayersCurrentlyInSession, PlayersEverInSession, 모든 사람이 합니다.| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "maxPlayers": 4,
    "seatsAvailable": 4,
    "isClosed": false,
    "hasEnded": false,
    "roster": [],
    "visibility": "PlayersCurrentlyInSession",
    "playersCanBeRemovedBy": "Self",
 }
    
```

  
<a id="ID4ENF"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EPF"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>참조 

[GameMessage(JSON)](json-gamemessage.md)

 [GameSessionSummary(JSON)](json-gamesessionsummary.md)

 [Player(JSON)](json-player.md)

   