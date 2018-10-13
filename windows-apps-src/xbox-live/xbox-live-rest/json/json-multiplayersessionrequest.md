---
title: MultiplayerSessionRequest(JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
author: KevinAsgari
description: " MultiplayerSessionRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d7c2cb3ca95524b49ea6e0cbe14771036a3e6925
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4572644"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest(JSON)
**MultiplayerSession** 개체에 대 한 작업에 대 한 전달 되는 요청 JSON 개체입니다. 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionRequest JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 상수| 개체| 세션에 대 한 상수를 생성 하기 위해 세션 템플릿을 사용 하 여 병합 된 읽기 전용으로 설정 합니다. | 
| 속성 | 개체 | 세션 속성에 병합 될 변경 합니다.| 
| members.me | 개체| 상수 및 많은 작동 하는 속성 같은 최상위에 상응 합니다. PUT 메서드는 사용자 세션의 구성원에 게 하 고 필요한 경우 사용자를 추가 합니다. "Me"를 null로 지정 하는 경우 요청 멤버 세션에서 제거 됩니다. | 
| 멤버 | 개체| 인덱스 0부터 시작 하 여 키 입력 세션에 추가 하는 사용자를 표시 하는 다른 개체입니다. 세션을 이미 멤버를 포함 하는 경우에 항상 요청의 구성원 수를 0으로 시작 합니다. 멤버는 요청에 나타나는 순서 세션에 추가 됩니다. 구성원 속성 속한 받은 사용자만 설정할 수 있습니다. | 
| 서버 | 개체| 업데이트 및 추가 세션을 나타내는 값의 관련된 서버 참가자 설정 됩니다. 서버를 null로 지정 하는 경우 해당 서버 항목 세션에서 제거 됩니다. | 
  
<a id="ID4EZ"></a>

 
## <a name="request-structure"></a>요청 구조
 

```json
{
  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },
  "members": {
    // Requires a service principal. Existing members can be deleted by index.
    // Not available on large sessions.
    "5": null,

    // Reservation requests must start with zero. New users will get added in order to the end of the session's member list.
    // Large sessions don't support reservations.
    "reserve_0": {
      "constants": { /* Property Bag */ }
    },
    "reserve_1": {
      "constants": { /* Property Bag */ }
    },

    // Requires a user principal with a xuid claim. Can be 'null' to remove myself from the session.
    "me": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
    }
  },

  // Requires a server principal.
  "servers": {
    // Can be any name. The value can be 'null' to remove the server from the session.
    "name": {
      "constants": {  /* Property Bag */ },
      "properties": {  /* Property Bag */ }
    }
  }
}
```

  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>참조 

[MultiplayerSession(JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   