---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
author: KevinAsgari
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 75c5e104add620c68f06a589be8f49f2a625de63
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5752349"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
업데이트, 만들거나 세션에 참가 합니다.

> [!IMPORTANT]
> 이 URI 메서드에 필요 Xbl 계약 버전 X의 헤더 요소: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EYB)
  * [HTTP 상태 코드](#ID4EFC)
  * [요청 본문](#ID4EOC)
  * [응답 본문](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드에 만듭니다 조인 또는 동일한 JSON 요청 본문 템플릿의 하위 집합은 전송에 따라 세션을 업데이트 합니다. 성공 시 서버에서 반환 된 응답을 포함 하는 **MultiplayerSession** 개체를 반환 합니다. 특성에 전달 된 **MultiplayerSession** 개체의 특성 으로부터 다를 수 있습니다. 이 메서드는 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**하 여 줄 바꿈할 수 있습니다.

세션 만들기 및 업데이트 작업 PUT 변경 내용이 적용을 나타내는 응용 프로그램/j 본문을 사용 합니다. 작업은 idempotent, 즉, 동일한 변경의 여러 응용 프로그램 추가 영향을 주지 않습니다.

JSON 요청 본문 세션 데이터 구조를 미러링합니다. 모든 필드와 하위 필드는 옵션입니다.

PUT 메서드에서 세션 만들기 또는 모드에 대 한 통신 형식은 다음과 같습니다.

> [!NOTE]
> 이 패턴을 사용 하 여 처리 합니다. 업데이트, 세션의 현재 상태에 관계 없이 적용 됩니다.



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



PUT 메서드에서 세션 업데이트 모드에 대 한 통신 형식은 다음과 같습니다.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



세션 속성을 업데이트할 PUT 메서드에 대 한 통신 형식은 다음과 같습니다. 것 URI 세션에 PUT 연산에 해당 하는 것은 본문 속성으로 아래 개체에만 필요 합니다. 이 작업 404 오류 코드를 반환 차이점은 세션 존재 하지 않는 경우 찾을 수 없습니다. 이 작업은 헤더를 지원합니다.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션 식별자입니다.|
| sessionTemplateName| string| 현재 인스턴스의 세션 템플릿 이름입니다. 파트 2 세션 식별자입니다.|
| 세션 이름| GUID| 세션의 고유 ID입니다. 파트 3 세션 식별자입니다.|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EOC"></a>


## <a name="request-body"></a>요청 본문

다음은 샘플 요청 본문을 만들거나 세션에 참가 하기 위한입니다. 요청 본문의 다음 멤버는 선택적입니다. 다른 모든 가능한 구성원 요청에 사용할 수 없습니다.

| 멤버| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 상수| 개체| 세션에 대 한 상수를 생성 하기 위해 세션 템플릿을 사용 하 여 병합 된 읽기 전용으로 설정 합니다. |
| 속성 | 개체 | 세션 속성에 병합할 변경 합니다.|
| members.me | 개체| 상수 및 작업 속성 같은 최상위에 상응 합니다. PUT 메서드 사용자 세션의 구성원 이어야 하며 필요한 경우 사용자를 추가 합니다. "Me"를 null로 지정 하는 경우 요청 멤버 세션에서 제거 됩니다. |
| 멤버 | 개체| 인덱스 0부터 시작 하 여 키 입력 세션에 추가 하는 사용자를 나타내는 다른 개체입니다. 세션을 이미 멤버를 포함 하는 경우에 항상 요청의 구성원 수를 0으로 시작 합니다. 멤버는 요청에 나타나는 순서 세션에 추가 됩니다. 구성원 속성 속한 고객만 사용자만 설정할 수 있습니다. |
| 서버 | 개체| 업데이트 및 세션에 대 한 추가 나타내는 값의 연결 된 서버 참가자 설정 됩니다. 서버를 null로 지정 하는 경우 해당 서버 항목 세션에서 제거 됩니다. |



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

응답 본문 만들거나 세션에 참가 하기 위한 샘플:


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


##### <a name="parent"></a>부모

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
