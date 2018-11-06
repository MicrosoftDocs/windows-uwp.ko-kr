---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
author: KevinAsgari
description: " POST (/handles)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aa749dac2638dbdb1f474300e9799e3e67827079
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6037996"
---
# <a name="post-handles"></a>POST (/handles)
사용자의 현재 활동에 대 한 멀티 플레이 세션을 설정 하 고 필요한 경우 세션 멤버를 초대 합니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EHB)
  * [HTTP 상태 코드](#ID4EPB)
  * [요청 본문](#ID4EVB)
  * [응답 본문](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

현재 활동에 대 한 세션을 설정 하이 HTTP/REST 메서드를 사용할 수 있습니다. 이 경우 메서드 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**래핑한 될 수 있습니다. 요청 본문에는 "작업"을 입력 필드를 사용 하 여 JSON 파일에서 **sessionRef** 개체를 사용 하 여 세션 참조를 정의 해야 합니다. 응답 본문 없이 검색 됩니다. 세션 참조에 지정 된 항목의 정의 **Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**를 참조 하세요.

세션에 대 한 핸들에 지정 된 사용자를 초대 하 여이 POST 메서드를 사용할 수도 있습니다. 이 경우 메서드 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**래핑한 될 수 있습니다. 이러한 POST 메서드를 사용이 하는 세션 참조를 정의 하 여 요청 본문 필요 하지만 형식과 필드 "초대"로 설정 합니다. 응답 본문에 초대 핸들입니다.

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

없음

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
MPSD에 적용 되는 서비스에서 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>요청 본문 활동을 설정 하기 위한


```cpp
{
  "version": 1,
  "type": "activity",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    // The session name is optional in a POST; if not specified, MPSD fills in a GUID.//
    "name": "session-49"
  },
}

```


<a id="ID4EBC"></a>


### <a name="request-body-for-sending-invites"></a>초대 보내기에 대 한 요청 본문


```cpp
{
  // Common handle fields
  "id: "47ca0049-a5ba-4bc1-aa22-fcf834ce4c13",
  "version": 1,
  "type": "invite",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    "name": "session-49"
   },
   "inviteAttributes": {
     "titleId" : {titleId}, // The title being invited to, in decimal uint32. This value is used to find the title name and/or image.
     "context" : {context}, // The title defined context token. Must be 256 characters or less when URI-encoded.
     "contextString" : {contextstring}, // The string name of a custom invite string to display in the invite notification.
     "senderString" : {sender} // The string name of the sender when the sender is a service.
   },
   "invitedXuid": "3210",
}

```


<a id="ID4EJC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EOC"></a>


### <a name="response-body-for-setting-activity"></a>활동 설정에 대 한 응답 본문
없음.  
<a id="ID4ESC"></a>


### <a name="response-body-for-sending-invites"></a>초대 보내기에 대 한 응답 본문
초대 핸들입니다.   
<a id="ID4EXC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EZC"></a>


##### <a name="parent"></a>부모

[/handles](uri-handles.md)
