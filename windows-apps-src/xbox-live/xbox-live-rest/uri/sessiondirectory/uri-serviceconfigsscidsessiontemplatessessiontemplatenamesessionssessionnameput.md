---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d35b3f89f8b866a5236e8f5ac91eb37d9a82d306
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598558"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
업데이트를 만들거나 세션에 참가 합니다.

> [!IMPORTANT]
> 이 URI 메서드를 헤더 요소의 X Xbl-계약 버전이 필요합니다. 104/105 또는 나중에 모든 요청 합니다.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EYB)
  * [HTTP 상태 코드](#ID4EFC)
  * [요청 본문](#ID4EOC)
  * [응답 본문](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드 만듭니다, 조인 또는 동일한 JSON 요청 본문 템플릿의 하위 집합은 전송에 따라 세션을 업데이트 합니다. 성공 하면 반환 된 **MultiplayerSession** 응답을 포함 하는 서버에서 반환 된 개체입니다. 특성에 전달 기능에 대 한 특성에서 다를 수 있습니다 **MultiplayerSession** 개체입니다. 이 메서드는로 래핑할 수 있습니다 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**합니다.

세션 만들기 및 업데이트 작업 적용 된 변경 내용을 나타내는 application/json 본문을 사용 하 여 PUT을 사용 합니다. 작업은 idempotent 상태, 즉, 동일한 변경 내용 여러 응용 프로그램 추가 영향을 주지 않습니다.

JSON 요청 본문 세션 데이터 구조를 반영합니다. 모든 필드 및 하위 필드는 선택 사항입니다.

PUT 메서드의 세션 만들기 또는 가입 모드에 대 한 통신 형식으로 다음과 같습니다.

> [!NOTE]
> 이 패턴을 사용 하 여 처리 합니다. 업데이트, 세션의 현재 상태에 관계 없이 적용 됩니다.



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



PUT 메서드의 세션 업데이트 모드에 대 한 통신 형식으로 다음과 같습니다.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



세션 속성을 업데이트 하는 PUT 메서드에 대 한 통신 형식으로 다음과 같습니다. 동일 세션 URI에 PUT 작업에 아무 것도도 아래 개체 속성으로 본문을 포함 합니다. 차이점은이 작업 오류 코드 404를 반환 하는 세션 없는 경우 찾을 수 없습니다. 이 작업은 If-match 헤더를 지원 합니다.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- |
| scid| GUID| (서비스 안내) 식별자를 구성 하는 서비스입니다. 1 부 세션 식별자입니다.|
| sessionTemplateName| 문자열| 세션 템플릿의 현재 인스턴스의 이름입니다. 2 부를 선택 하면 세션 식별자입니다.|
| sessionName| GUID| 세션의 고유 ID입니다. 3 부 세션 식별자입니다.|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EOC"></a>


## <a name="request-body"></a>요청 본문

다음은 만들거나 세션에 참가 대 한 예제 요청 본문입니다. 요청 본문의 다음 멤버는 선택적입니다. 다른 모든 가능한 멤버를 요청에 사용할 수 없습니다.

| 멤버| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 상수| object| 세션에 대 한 상수를 생성 하기 위해 세션 템플릿으로 병합 되는 읽기 전용으로 설정 합니다. |
| 속성 | object | 세션 속성에 병합할 변경 내용입니다.|
| members.me | object| 상수 및 속성 대부분을 작동 하는 같은 최상위 대응 합니다. 모든 PUT 메서드 사용자 세션의 멤버 여야 하며 필요한 경우 사용자를 추가 합니다. "Me"를 null로 지정 하면 요청 된 멤버는 세션에서 제거 됩니다. |
| 멤버 | object| 인덱스는 0부터 시작 하 여 키가 지정 된 세션에 추가 하는 사용자를 나타내는 다른 개체입니다. 세션 이미 멤버를 포함 하는 경우에 요청에 대 한 멤버 수가 항상 0으로 시작 합니다. 멤버는 세션에서 요청에 나타나는 순서에 추가 됩니다. 만 속해 부여한 사용자가 멤버 속성을 설정할 수 있습니다. |
| 서버 | object| 연결 된 서버 참가자의 업데이트 및 세션에 대 한 추가 나타내는 값의 집합입니다. 서버는 null로 지정 된 경우 세션에서 해당 서버 항목이 제거 됩니다. |



```cpp
{
  "properties": {
    "custom": {"KANWE": "MGMSY"},
    "system": {}
  },
  "constants": {
    "custom": {},
    "system": {"visibility": "open"}
  },
  "members": {
    "reserve_0": {
    "constants": {
      "custom": {"type": "leader"},
      "system": {"xuid": "5500461"} }}
   }
}

```


<a id="ID4E4C"></a>


## <a name="response-body"></a>응답 본문

만들거나 세션에 참가 대 한 샘플 응답 본문:


```cpp
{
  "contractVersion": 104,
  "correlationId": "0FE81338-EE96-46E3-A3B5-2DBBD6C41C3B",
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": {
    "system": {"visibility": "open"},
    "custom": {}
  },

  "properties": {
     "system": { "turn": [] },
     "custom": { "myProperty": "myValue" }
  },

  "members": {
      "1": {
        "properties": {
        "system": { },
        "custom": { }
      },

      "constants": {
        "system": { "xuid": "5500461" },
        "custom": { }
      }

      "gamertag": "stacy",
      "deviceToken": "9f4032ba7",
      "reserved": true,
      "activeTitleId": "8397267",
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      "turn": true,
      "initializationFailure": "latency",
      "initializationEpisode": 1,
      "next": 4
    },
  },

  "membersInfo": {
      "first": 1,
      "next": 4,
      "count": 1,
      "accepted": 0
  },

  "servers": {
      "name": {
        "constants": { },
        "properties": { }
      }
  }
}

```


<a id="ID4EID"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EKD"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
