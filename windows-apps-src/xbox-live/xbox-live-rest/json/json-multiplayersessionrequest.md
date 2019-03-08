---
title: MultiplayerSessionRequest(JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
description: " MultiplayerSessionRequest(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 18929c060adeae47f0305422dd312e7410f93981
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628348"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest(JSON)
요청 JSON 개체에 대 한 작업에 전달 된 **MultiplayerSession** 개체입니다. 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionRequest JSON 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| 상수| object| 세션에 대 한 상수를 생성 하기 위해 세션 템플릿으로 병합 되는 읽기 전용으로 설정 합니다. | 
| 속성 | object | 세션 속성에 병합할 변경 내용입니다.| 
| members.me | object| 상수 및 속성 대부분을 작동 하는 같은 최상위 대응 합니다. 모든 PUT 메서드 사용자 세션의 멤버 여야 하며 필요한 경우 사용자를 추가 합니다. "Me"를 null로 지정 하면 요청 된 멤버는 세션에서 제거 됩니다. | 
| 멤버 | object| 인덱스는 0부터 시작 하 여 키가 지정 된 세션에 추가 하는 사용자를 나타내는 다른 개체입니다. 세션 이미 멤버를 포함 하는 경우에 요청에 대 한 멤버 수가 항상 0으로 시작 합니다. 멤버는 세션에서 요청에 나타나는 순서에 추가 됩니다. 만 속해 부여한 사용자가 멤버 속성을 설정할 수 있습니다. | 
| 서버 | object| 연결 된 서버 참가자의 업데이트 및 세션에 대 한 추가 나타내는 값의 집합입니다. 서버는 null로 지정 된 경우 세션에서 해당 서버 항목이 제거 됩니다. | 
  
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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>참고자료 

[MultiplayerSession (JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   