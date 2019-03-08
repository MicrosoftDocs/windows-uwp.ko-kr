---
title: POST (/serviceconfigs/{scid}/batch)
assetID: b821a6eb-1add-ef91-bdf5-10e107082197
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatchpost.html
description: " POST (/serviceconfigs/{scid}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e718fda26264667a7bf08c9254a462eb268dcd74
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603448"
---
# <a name="post-serviceconfigsscidbatch"></a>POST (/serviceconfigs/{scid}/batch)
서비스 구성에 대 한 여러 Xbox 사용자 Id에 일괄 처리 쿼리를 만듭니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이 게임에서 사용 되 고 이상 멀티 플레이 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X-Xbl-계약-버전 헤더 요소를 필요 합니다. 104/105 또는 나중에 모든 요청 합니다.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4ELB)
  * [쿼리 문자열 매개 변수](#ID4EVB)
  * [HTTP 상태 코드](#ID4EGF)
  * [요청 본문](#ID4ENF)
  * [응답 본문](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드는 서비스 구성 서비스 ID (안내) 수준에서 여러 Xbox 사용자 Id에 일괄 처리 쿼리를 만듭니다. 이 메서드는로 래핑할 수 있습니다 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**합니다.

2015 멀티 플레이 게임을 결합할 수 있습니다 여러 쿼리를 단일 호출으로 모든 쿼리는 동일 하지만 경우에 표시 된 대로 다른 Xbox 사용자 Id (XUIDs)를 사용 하 여 처리 합니다 *xuid* 쿼리 문자열 매개 변수입니다. 쿼리 문자열 매개 변수 및 응답에는 일반 쿼리 및 일괄 처리 쿼리를 위해 동일한 참고 합니다.

와 동일한 순서로 응답에 기록 하는 각 XUID 속한 세션 일괄 처리 쿼리를 작성 합니다 *xuid* 매개 변수는 요청에 제공 됩니다. 각각에 대해 여러 번 응답에서 한 번 표시 동일한 세션에 대 한 있기 *xuid* 일치 하는 합니다.

<a id="ID4ELB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| scid| GUID| (서비스 안내) 식별자를 구성 하는 서비스입니다. 1 부 세션 식별자입니다.|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

다음 표에 쿼리 문자열 매개 변수를 사용 하 여 쿼리를 수정할 수 있습니다.

| <b>매개 변수</b>| <b>Type</b>| <b>설명</b>|
| --- | --- | --- | --- | --- | --- | --- |
| keyword| 문자열| 예를 들어, "foo" 키워드를 검색할 경우 세션 또는 서식 파일에서 발견 되어야 하는 합니다. |
| xuid| 64 비트 부호 없는 정수| 예를 들어 "123", 쿼리에 포함 하는 세션에 대 한 하 여 Xbox 사용자 ID입니다. 기본적으로 사용자를 포함 하는 세션에서 활성 상태 여야 합니다. |
| 예약| boolean| 세션을 포함 하려면 사용자 예약된 플레이어로 설정 되어 있지만 활성 플레이어를 조인 되지 않았습니다. 이 매개 변수는 고유한 세션을 쿼리할 때 또는 특정 사용자의 세션 서버-투-서버를 쿼리 하는 경우에 사용 됩니다. |
| 비활성| boolean| 사용자가 수락 하지만 적극적으로 재생 되지 않으면 세션을 포함 하려면 true입니다. 사용자가 "ready" 하지만 "비활성" 세션 비활성으로 계산 합니다. |
| 개인| boolean| 개인 세션을 포함 하려면 true입니다. 이 매개 변수는 고유한 세션을 쿼리할 때 또는 특정 사용자의 세션 서버-투-서버를 쿼리 하는 경우에 사용 됩니다. |
| visibility| 문자열| 세션에 대 한 표시 여부 상태입니다. 으로 정의 된 가능한 값은 <b>MultiplayerSessionVisibility</b>합니다. 이 매개 변수를 "열린"로 하는 경우 쿼리만 열려 있는 세션을 포함 해야 합니다. 에 "개인"으로 설정 되어 있으면 합니다 <i>개인</i> 매개 변수를 설정 해야 true로 합니다. |
| version| 32 비트 부호 있는 정수| 포함 되어야 하는 최대 세션 버전입니다. 예를 들어 2의 주요 세션 버전과 해당 세션을 지정 하는 값이 2 또는 작은 포함 되어야 합니다. 버전 번호를 요청 계약 버전 mod 100 보다 작거나 여야 합니다. |
| Take| 32 비트 부호 있는 정수| 검색할 세션의 최대 수입니다. 이 번호는 0과 100 사이 여야 합니다. |


설정 중 하나 *사설* 또는 *예약* true 세션에 대 한 서버 수준 액세스를 해야 합니다. 또는 이러한 설정은 URI에 XUID 필터와 일치 하는 호출자의 XUID 클레임 필요 합니다. 그렇지 않은 경우 HTTP/403 상태 코드가 반환 됩니다 이러한 세션을 실제로 존재 하는지 여부.

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4ENF"></a>


## <a name="request-body"></a>요청 본문


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


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


<a id="ID4EDG"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EFG"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)
