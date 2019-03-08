---
title: GameSession(JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
description: " GameSession(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca7276ccdc13d896d19873811b4fa9df9a831cd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646518"
---
# <a name="gamesession-json"></a>GameSession(JSON)
멀티 플레이 세션에 대 한 게임 데이터를 나타내는 JSON 개체입니다. 
<a id="ID4ER"></a>

  
 
GameSession JSON 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| creationTime| DateTime| 날짜 및 세션을 만든 시간을 utc에서입니다. | 
| customData| 8 비트 부호 없는 정수 배열| 게임 별 세션 데이터의 1024 바이트입니다. 이 값은 서버에 불투명 합니다. | 
| displayName| 문자열| 표시 게임 이름을 세션에는 최대 길이는 128 자입니다. 이 값은 서버에 불투명 합니다. | 
| hasEnded| 부울 값| 세션이 종료 되었음을 경우 true이 고, 그렇지 합니다. 추가 데이터를 세션에 전송 되지 않도록 방지 true 표시 읽기 전용으로 게임 세션에이 필드를 설정 합니다. | 
| IsClosed| 부울 값| 세션을 닫고 없습니다 더 많은 플레이어 수에 추가 되 고 false이 고, 그렇지 면 true입니다. 이 값이 true 이면 세션에 참가 하는 요청이 거부 됩니다. | 
| maxPlayers| 32 비트 부호 있는 정수| 최대 동시 세션 수 있는 플레이어입니다. 값의 범위는 2 ~ 16입니다. 플레이어의 최대 수를 포함 하는 세션, 되 면 추가 세션에 참가 하는 요청 거부 됩니다. | 
| playersCanBeRemovedBy| PlayerAcl| 세션에서 다른 플레이어를 제거할 수 있는 플레이어를 나타내는 값입니다. 가능한 값은 NoOne, Self 및 AnyPlayer입니다. | 
| 명단| 플레이어 개체의 배열| 세션에서 플레이어의 배열입니다. 명단 현재 및 세션에서 이전에 있었지만 남아 있는 게 플레이어를 포함 합니다. 명단에 플레이어의 순서를 변경 되지 않습니다. 새 플레이어를 배열의 끝에 추가 됩니다. | 
| seatsAvailable| 32 비트 부호 있는 정수| 플레이어의 최대 수에 도달 하기 전에 세션에 가입할 수 있는 플레이어 수입니다. 이 값은 읽기 전용 이며 항상 maxPlayers 필드의 값 보다 작지 합니다. | 
| sessionId| 문자열| 세션을 만들 때의 MPSD에서 할당 하는 세션 ID입니다. 이 값은 세션에서 저장 된 정보에 액세스할 때 일반적으로 URI에 포함 됩니다.| 
| titleId| 32 비트 부호 없는 정수| 게임 세션 만들기 타이틀의 ID입니다.| 
| Variant| 32 비트 부호 있는 정수| 게임 변형입니다. 이 값은 서버에 불투명 합니다.| 
| visibility| VisibilityLevel| 세션의 표시 여부를 나타내는 값입니다. 가능한 값은 다음과 같습니다. PlayersCurrentlyInSession, PlayersEverInSession, 및 모든 사용자| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>참고자료 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [플레이어 (JSON)](json-player.md)

   