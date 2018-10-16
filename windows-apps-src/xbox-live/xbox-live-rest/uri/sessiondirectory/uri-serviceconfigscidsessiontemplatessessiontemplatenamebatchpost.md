---
title: POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
assetID: 1a0a62ca-e120-e705-3c93-efd4697e2ccf
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.html
author: KevinAsgari
description: " POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0ed9a461b630f1ec277190c43efa99aa74492b0e
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4679124"
---
# <a name="post-serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
여러 Xbox 사용자 Id에서 일괄 처리 쿼리를 만듭니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 되며 나중 및 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X-Xbl-계약-버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EKB)
  * [쿼리 문자열 매개 변수](#ID4EVB)
  * [HTTP 상태 코드](#ID4EGF)
  * [요청 본문](#ID4ENF)
  * [응답 본문](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드에 여러 Xbox 사용자 Id 세션 템플릿 수준에서 일괄 처리 쿼리를 만듭니다. 이 메서드는 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**하 여 줄 바꿈할 수 있습니다.

2015 멀티 플레이어를 결합할 수 있습니다 많은 쿼리 한 번만 호출 경우 모든 쿼리는 동일 하지만 *xuid* 쿼리 문자열 매개 변수에서 표시 된 대로 다른 Xbox 사용자 Id (XUIDs)를 처리 합니다. 쿼리 문자열 매개 변수 및 응답을 동일한 지 일반 쿼리와 일괄 처리에 대 한 참고 합니다.

일괄 처리 쿼리를 위해 각 XUID에 속하는 세션 *xuid* 매개 변수는 요청에 표시 된 대로 동일한 순서로 응답에 기록 됩니다. 일치 하는 각 *xuid* 한 번에 대 한 응답으로 여러 번 표시 동일한 세션에 대 한 것 같습니다.

<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 1 부 세션 식별자입니다.|
| sessionTemplateName| string| 세션 템플릿 현재 인스턴스의 이름입니다. 2 부 세션 식별자입니다.|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

다음 표에 쿼리 문자열 매개 변수를 사용 하 여 쿼리를 수정할 수 있습니다.

| <b>매개 변수</b>| <b>유형</b>| <b>설명</b>|
| --- | --- | --- | --- | --- | --- | --- |
| 키워드| string| 예를 들어 "foo"가, 키워드는는에 있어야 세션 또는 템플릿 검색 하려는 경우. |
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID, 예를 들어 "123", 세션 쿼리에서 포함 하도록 합니다. 기본적으로 사용자를 포함 하는 세션에서 활성 상태 여야 합니다. |
| 예약| 부울| 세션을 포함 하려면 사용자 예약 된 플레이어로 설정 되어 있지만 되는 활성 플레이어에 가입 되지 않은 했습니다. 이 매개 변수는 자신의 세션 쿼리할 때 또는 특정 사용자의 세션-서버를 쿼리 하는 경우에 사용 됩니다. |
| 비활성| 부울| 사용자는 수락 하지만 적극적으로 재생 되지 세션을 포함 하는 경우 사용자가 "준비" 하지만 "비활성" 세션 비활성으로 계산 됩니다. |
| 개인| 부울| 개인 세션을 포함 하는 경우 이 매개 변수는 자신의 세션 쿼리할 때 또는 특정 사용자의 세션-서버를 쿼리 하는 경우에 사용 됩니다. |
| visibility| 문자열| 세션에 대 한 가시성 상태입니다. 가능한 값은 <b>MultiplayerSessionVisibility에</b>의해 정의 됩니다. 이 매개 변수는 "열기"를 설정 되 면 쿼리에 열려 있는 세션을 포함 해야 합니다. <i>개인</i> 매개 변수를 설정 해야 "개인"으로 설정 되 면 true로 합니다. |
| 버전| 32 비트 부호 있는 정수| 포함 되어야 하는 최대 세션 버전입니다. 예를 들어 2의 값은 2의 주요 세션 버전을 사용 하 여 해당 유일한 세션 지정 또는 덜 포함 되어야 합니다. 버전 번호 보다 작거나 같아야 요청의 계약 버전 mod 100 이어야 합니다. |
| 시험| 32 비트 부호 있는 정수| 검색할 세션의 최대 수입니다. 이 숫자는 0과 100 사이 여야 합니다. |


*개인* 또는 *예약* 을 true로 설정 하면 세션에 대 한 서버 수준 액세스가 필요 합니다. 또는 이러한 설정을 호출자의 XUID URI의 XUID 필터와 일치 하도록 요청 해야 합니다. 그렇지 않은 경우 HTTP/403 상태 코드는 반환, 그러한 세션은 실제로 존재 여부.

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

이 메서드에서 반환의 세션 참조, 일부 세션 포함 데이터 인라인 JSON 배열입니다.


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


##### <a name="parent"></a>부모

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)
