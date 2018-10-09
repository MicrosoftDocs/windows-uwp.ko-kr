---
title: GET (/serviceconfigs/{scid}/sessions)
assetID: adc65d0b-58dd-bfb9-54c8-9bc9d02e68ec
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessionsget.html
author: KevinAsgari
description: " GET (/serviceconfigs/{scid}/sessions)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7ada5040c97dcb283146cb528cf2107294b9b88b
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4461757"
---
# <a name="get-serviceconfigsscidsessions"></a>GET (/serviceconfigs/{scid}/sessions)
검색 세션 정보를 지정 합니다.

> [!IMPORTANT]
> 이 메서드는 이제 X Xbl-계약 버전의 헤더 요소 필요: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EKB)
  * [HTTP 상태 코드](#ID4EXB)
  * [요청 본문](#ID4EAC)
  * [응답 본문](#ID4ELC)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드는 제공 된 필터에 대 한 세션 정보를 검색합니다. 이 메서드는 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**하 여 줄 바꿈할 수 있습니다.


> [!NOTE] 
> 2015 멀티 플레이어 <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>하 여이 메서드가 호출 됩니다.  



> [!NOTE] 
> 이 메서드를 호출할 때마다 키워드, Xbox 사용자 ID 필터 또는 둘 다 포함 해야 합니다. 호출자에 <i>개인</i> 및 <i>예약</i> 매개 변수에 대 한 올바른 권한이 없는 경우 그러한 세션은 실제로 존재 여부 메서드 403, 오류 코드를 반환 합니다.  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션의 id.|
| 키워드| string| 키워드 해당 문자열을 사용 하 여 식별 하는 단지 세션에 결과 필터링 하는 데 사용 합니다.|
| xuid| GUID| 세션을 검색 하 고 사용자에 대 한 Xbox 사용자 Id입니다. 사용자 세션에서 활성 이어야 합니다.|
| 예약| string| 세션 목록에 사용자가 수락 하지 않는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다. 이 설정을 사용 하려면 세션에 서버 수준 액세스 호출자 또는 호출자의 XUID Xbox 사용자 ID 필터와 일치 하도록 요청 합니다. |
| 비활성| string| 사용자가 수락 하지만 적극적으로 재생 되지 세션의 목록을 포함 하는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다.|
| 개인| string| 세션의 목록을 개인 세션을 포함 하는 경우를 나타내는 값입니다. 이 매개 변수를 설정할 수만 true로 합니다. 서버를 쿼리 하는 경우 또는 고유한 세션을 쿼리 하는 경우에 유효 합니다. 호출자가 세션에 대 한 서버 수준 액세스 하려면이 매개 변수를 true로 설정 하거나 호출자의 XUID Xbox 사용자 ID 필터와 일치 하도록 요청 합니다. |
| visibility| 문자열| 결과 필터링에 사용 되는 표시 상태를 나타내는 열거형 값. 현재이 매개 변수만 설정할 수 열기 열려 있는 세션을 포함 하도록 합니다. <b>MultiplayerSessionVisibility</b>를 참조 하세요.|
| 버전| 문자열| 양의 정수 주요 세션 버전 또는 하위 세션을 나타내는 포함 하도록 합니다. 값은 100 나머지 요청의 계약 버전 보다 작거나 이어야 합니다.|
| 시험| string| 양의 정수 세션의 최대 수를 나타내는를 검색 합니다.|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EAC"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4ELC"></a>


## <a name="response-body"></a>응답 본문

이 메서드에서 반환에 일부 세션 포함 데이터 인라인 세션 참조의 JSON 배열입니다.


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


##### <a name="parent"></a>부모

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)
