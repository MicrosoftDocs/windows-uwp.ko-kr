---
title: GameMessage(JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
author: KevinAsgari
description: " GameMessage(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f8d04b0487c7b42becd9a899c3532a6e5221f22e
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6260816"
---
# <a name="gamemessage-json"></a>GameMessage(JSON)
게임 세션의 메시지 큐에서 메시지에 대 한 데이터를 정의 하는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
GameMessage JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| data| 8 비트 부호 없는 정수로의 배열| 게임 클라이언트가 다른 게임 클라이언트에 게 보낼 수 있는 Base64 인코딩된 데이터입니다. 이 값은 서버에 불투명입니다. | 
| senderXuid| 64 비트 부호 없는 정수| 메시지를 보내는 플레이어의 Xbox 사용자 ID입니다. | 
| 일련 번호| 32 비트 부호 있는 정수| 게임 메시지의 시퀀스 번호입니다. 이 값은 서버에 의해 할당 됩니다. 시퀀스 번호 단조롭게 증가 보장이 있지만 연속 수 있습니다. 시퀀스 번호는 고유한 메시지 큐 내에서 있지만 간에 메시지 큐는 그렇지 않습니다. | 
| queueIndex| 32 비트 부호 있는 정수| 메시지에 대 한 세션 메시지 큐의 인덱스입니다. 가능한 값은 0-3입니다.| 
| 타임 스탬프| DateTime| 게임 메시지 서버 UTC에서 큐에서 생성 된 시간입니다. | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "queueIndex": 0,
    "sequenceNumber": 5,
    "senderXuid": 65536,
    "timestamp": "2011-06-23T18:49:50Z",
    "data": null
}
    
```

  
<a id="ID4E1C"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E3C"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>참조 

[GameSession(JSON)](json-gamesession.md)

   