---
title: Player(JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
description: " Player(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5967cbfecd47c5675926bd45939442c45dda7b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622498"
---
# <a name="player-json"></a>Player(JSON)
플레이어 게임 세션에서 데이터를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="player"></a>플레이어
 
플레이어 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| customData| 8 비트 부호 없는 정수 배열| 1024 바이트 base64 인코딩된 플레이어 게임 별 데이터입니다. 이 값은 서버에 불투명 합니다.| 
| 게이머 태그| 문자열| 게이머 태그-최대 15 자-플레이어입니다. 클라이언트는 플레이어를 식별할 때 UI에서이 값을 사용 해야 합니다. | 
| isCurrentlyInSession| 부울 값| 플레이어는 현재 세션의에서 세션을 유지 하는지 여부를 나타냅니다.| 
| seatIndex| 32 비트 부호 있는 정수| 세션에서 플레이어의 인덱스입니다.| 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 플레이어입니다.| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>참고자료 

[GameSession (JSON)](json-gamesession.md)

   