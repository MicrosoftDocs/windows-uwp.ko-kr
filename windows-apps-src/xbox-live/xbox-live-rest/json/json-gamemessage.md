---
title: GameMessage(JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
author: KevinAsgari
description: " GameMessage(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 089d2a492c8878e79bd60de1226c948e1eee7e0f
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4531966"
---
# <a name="gamemessage-json"></a>GameMessage(JSON)
게임 세션의 메시지 큐 메시지에 대 한 데이터를 정의 하는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
GameMessage JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| data| 8 비트 부호 없는 정수로의 배열| 다른 게임 클라이언트에 전송 하는 게임 클라이언트 Base64 인코딩된 데이터입니다. 이 값은 서버에 불투명 합니다. | 
| senderXuid| 64 비트 부호 없는 정수| 메시지를 보내는 플레이어의 Xbox 사용자 ID입니다. | 
| 일련 번호| 32 비트 부호 있는 정수| 게임 메시지의 시퀀스 번호입니다. 이 값은 서버에 의해 할당 됩니다. 시퀀스 번호 단조롭게 증가 보장이 있지만 연속 수 있습니다. 시퀀스 번호는 고유한 메시지 큐 내 있지만 메시지 큐 간에 되지 않습니다. | 
| queueIndex| 32 비트 부호 있는 정수| 메시지에 대 한 세션 메시지 큐의 인덱스입니다. 가능한 값은 0 ~ 3입니다.| 
| 타임 스탬프| DateTime| 게임 메시지 서버 UTC에서 큐에서 생성 된 시간입니다. | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

   