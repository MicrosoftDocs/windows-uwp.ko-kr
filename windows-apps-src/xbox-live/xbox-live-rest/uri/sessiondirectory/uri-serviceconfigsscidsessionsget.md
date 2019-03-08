---
title: GET (/serviceconfigs/{scid}/sessions)
assetID: adc65d0b-58dd-bfb9-54c8-9bc9d02e68ec
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessionsget.html
description: " GET (/serviceconfigs/{scid}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e54c9cd68a899cfd040bc3e16a05f6deb2daa7c3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614108"
---
# <a name="get-serviceconfigsscidsessions"></a>GET (/serviceconfigs/{scid}/sessions)
지정 된 세션 정보를 검색 합니다.

> [!IMPORTANT]
> 이 메서드는 이제 X Xbl-계약 버전 헤더 요소가 필요합니다. 104/105 또는 나중에 모든 요청 합니다.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EKB)
  * [HTTP 상태 코드](#ID4EXB)
  * [요청 본문](#ID4EAC)
  * [응답 본문](#ID4ELC)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드는 제공 된 필터에 대 한 세션 정보를 검색합니다. 이 메서드는로 래핑할 수 있습니다 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**합니다.


> [!NOTE] 
> 2015 멀티 플레이어 게임에이 메서드는 <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>합니다.  



> [!NOTE] 
> 이 메서드를 호출할 때마다 키워드, Xbox 사용자 ID 필터 또는 둘 다 포함 해야 합니다. 호출자에 대 한 올바른 권한이 없는 경우는 <i>사설</i> 하 고 <i>예약</i> 매개 변수를 메서드 반환 오류 코드 403 사용할 수 없음, 이러한 세션을 실제로 존재 하는지 여부.  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| (서비스 안내) 식별자를 구성 하는 서비스입니다. 1 부 세션의 id입니다.|
| keyword| 문자열| 해당 문자열을 사용 하 여 식별만 세션에 대 한 결과 필터링 하는 데 사용 하는 키워드입니다.|
| xuid| GUID| Xbox 사용자 세션을 검색할 사용자에 대 한 사용자 Id입니다. 사용자 세션에서 활성 상태 여야 합니다.|
| 예약| 문자열| 사용자가 수락 하지 않은 세션 목록을 포함 하는 경우를 나타내는 값입니다. 이 매개 변수는 설정만 가능 true로 합니다. 호출자의 XUID Xbox 사용자 ID 필터를 일치 하도록 클레임 또는이 설정은 호출자가 세션에 대 한 서버 수준 권한이 있어야 합니다. |
| 비활성| 문자열| 세션의 목록에 포함 된 경우 사용자가 수락 하지만 오디오가 재생 하지는 것을 나타내는 값입니다. 이 매개 변수는 설정만 가능 true로 합니다.|
| 개인| 문자열| 개인 세션 세션의 목록에 포함 하는 경우를 나타내는 값입니다. 이 매개 변수는 설정만 가능 true로 합니다. 서버-투-서버를 쿼리 하는 경우 또는 사용자 자신의 세션을 쿼리 하는 경우에 유효 합니다. 호출자의 XUID Xbox 사용자 ID 필터를 일치 하도록 클레임 또는 호출자가 세션에 대 한 서버 수준 권한이 있어야이 매개 변수를 true로 설정 합니다. |
| visibility| 문자열| 결과 필터링에 사용 되는 표시 상태를 나타내는 열거형 값입니다. 현재이 매개 변수 에서만 설정할 수 있습니다 열 열려 있는 세션을 포함 합니다. 참조 <b>MultiplayerSessionVisibility</b>합니다.|
| version| 문자열| 양의 정수 버전 주요 세션 또는 세션 중 더 작은 값을 나타내는를 포함 합니다. 값 보다 작거나 100 모듈로 요청의 계약 버전 이어야 합니다.|
| Take| 문자열| 양의 정수 세션의 최대 수를 나타내는를 검색 합니다.|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EAC"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4ELC"></a>


## <a name="response-body"></a>응답 본문

이 메서드에서 반환에는 일부 세션 데이터가 포함 된 인라인을 사용 하 여 세션 참조의 JSON 배열입니다.


```cpp
{
    "results": [ {
            "xuid": "9876",    // If the session was found from a xuid, that xuid.
            "startTime": "2009-06-15T13:45:30.0900000Z",
            "sessionRef": {
                "scid": "foo",
                "templateName": "bar",
                "name": "session-seven"
            },
            "accepted": 4,    // Approximate number of non-reserved members.
            "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
            "visibility": "open",    // or "private", "visible", or "full"
            "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
            "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
            "keywords": [ "one", "two" ]
        }
    ]
}

```


<a id="ID4EWC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EYC"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)
