---
title: Player(JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
author: KevinAsgari
description: " Player(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0454f364bf2612c88b8f212ba935872968ce2476
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700648"
---
# <a name="player-json"></a>Player(JSON)
게임 세션에는 플레이어에 대 한 데이터를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="player"></a>플레이어
 
플레이어 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| customData| 8 비트 부호 없는 정수의 배열| 1024 바이트의 Base64 인코딩된 플레이어가 게임 관련 데이터입니다. 이 값은 서버에 불투명입니다.| 
| 게이머 태그| string| 게이머 태그-15 자-플레이어의 합니다. 플레이어를 식별할 때 클라이언트 UI에서이 값을 사용 해야 합니다. | 
| isCurrentlyInSession| 부울 값| 플레이어가 현재 세션에 또는 세션 왼쪽 나타냅니다.| 
| seatIndex| 32 비트 부호 있는 정수| 세션에서 플레이어의 인덱스입니다.| 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 플레이어의 합니다.| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>참조 

[GameSession(JSON)](json-gamesession.md)

   