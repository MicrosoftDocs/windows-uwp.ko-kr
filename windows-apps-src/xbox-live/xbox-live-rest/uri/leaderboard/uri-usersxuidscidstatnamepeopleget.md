---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 40230b2ffafd9f95b6e984f8cc287dd3157a0de1
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700397"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
소셜 순위표 순위 상태는 현재 사용자의 모든 알려진된 연락처 중 하나 또는 해당 사용자가 즐겨 찾는 사용자 지정 된 연락처에만 (점수) 값을 반환 합니다.
이러한 Uri에 대 한 도메인은 `leaderboards.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EAB)
  * [쿼리 문자열 매개 변수](#ID4ELB)
  * [권한 부여](#ID4EQD)
  * [필요한 요청 헤더](#ID4EGE)
  * [선택적 요청 헤더](#ID4EXF)
  * [요청 본문](#ID4ETG)
  * [HTTP 상태 코드](#ID4ECEAC)
  * [필요한 응답 헤더](#ID4E1HAC)
  * [선택적 응답 헤더](#ID4EDJAC)
  * [응답 본문](#ID4EDKAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

순위표 Api 모두 읽기 전용 이며 따라서 (HTTPS)을 통해 GET 동사만 지원 합니다. 순위 되 고 정렬 된 "페이지" 데이터 플랫폼을 통해 작성 된 개별 사용자 통계에서 파생 되는 인덱싱된 플레이어 통계을 반영 합니다. 전체 순위표 인덱스 상당히 크며 수 및 호출자 전체에서 하나 묻지 않습니다, 그리고 따라서이 URI에 어떤 종류의 보기에 대해 해당 순위표 보려는 정확 하 게 호출자를 사용할 수 있는 몇 가지 쿼리 문자열 인수를 지원 합니다.

GET 작업 않으므로 한 번 또는 여러 번 실행 하는 경우 동일한 결과 재현 됩니다이 모든 리소스를 수정 하지 않습니다.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid| string| 사용자의 식별자입니다.|
| 서비스 안내| string| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.|
| statname| string| 액세스 되는 사용자 통계 리소스의 고유 식별자입니다.|
| 모두|즐겨찾기| 열거형| 현재 사용자의 알려진된 모든 연락처 또는 해당 사용자가 즐겨 찾는 사용자 지정 된 연락처에만 (점수) 값의 상태를 순위 여부입니다.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- |
| maxItems| 32 비트 부호 없는 정수| 결과의 페이지에서 반환할 순위표 레코드의 최대 수입니다. 지정 하지 않으면 기본 수 (10)을 반환 됩니다. MaxItems에 대 한 최대 값이 여전히 정의 되지 하지만 큰 데이터 집합을 방지 하려면,이 값이 대상으로 아마도 해야 하므로 가장 큰 설정 하는 UI 호출 당 처리할 수 튜너 합니다. |
| skipToRank| 32 비트 부호 없는 정수| 지정 된 순위표 순위부터 결과 페이지를 반환 합니다. 결과의 나머지 부분 순위 순서 대로 정렬 됩니다. 이 쿼리 문자열 "다음 페이지"의 결과 얻으려면 후속 쿼리도 다시 입력 될 수 있는 연속 토큰을 반환할 수 있습니다. |
| skipToUser| string| 해당 사용자의 순위 또는 점수에 관계 없이 지정 된 게이머 xuid 주위 순위표 결과 페이지를 반환 합니다. 페이지 % 째 순위 통계 순위표 보기에 대 한 중간 또는 미리 정의 된 보기에 대 한 페이지의 마지막 위치에 지정된 된 사용자를 사용 하 여 정렬 됩니다. 이 유형의 쿼리 제공 없는 <b>continuationToken</b> 있습니다. |
| continuationToken| 문자열| 이전에 <b>continuationToken</b>반환 되 면 다음 호출자에 게 전달할 수 다시 "있는 그대로" 토큰 결과의 다음 페이지를 가져오려면 쿼리 문자열. |
| sort| string| 여부 낮은 높은 값 순서 ("오름차순")에서 한 플레이어 목록이 순위를 지정 하거나 높은-낮은 값 순서 ("내림차순"). 이 선택적 매개 변수입니다. 기본 내림차순 합니다.|

<a id="ID4EQD"></a>


## <a name="authorization"></a>권한 부여

Xuid 권한이 필요 합니다.

권한 부여 논리는 콘텐츠 격리 및 액세스 제어 시나리오의 목적을 위해 구현 되었습니다.

순위표와 사용자 통계 호출자가 요청을 사용 하 여 유효한 XSTS 토큰 제출 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 데이터 플랫폼에서 지원 되는 클라이언트 (물론) 제한 됩니다.

제목 개발자 열기 또는 XDP 또는 개발자 센터를 사용 하 여 제한 된 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자가 해당 샌드박스에 인증으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여 샌드박스에 XDP 또는 개발자 센터를 통해 관리 됩니다.

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열입니다. HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".|
| Content-Type| 문자열입니다. 요청 본문의 MIME 형식입니다. 예제 값: "application/json".|
| X RequestedServiceVersion| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1입니다.|
| 수락| 문자열입니다. 허용 되는 콘텐츠 형식 값입니다. 예제 값: "application/json".|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| 문자열입니다. 엔터티 태그-클라이언트 캐싱을 지 원하는 경우 사용 합니다. 예제 값: "686897696a7c876b7e".|

<a id="ID4ETG"></a>


## <a name="request-body"></a>요청 본문

호출자는 최적의 표시를 위해 다시 가져오는 데이터를 이해 하는 기능을 극대화 하기 각 사용자에 대 한 통계 각 값이 있는 것, 표시 및 시점 accept language에 지정 된 로캘과 일치 하도록 서식이 지정 된 형식 문자열로 반환 됩니다. 요청에 대 한 헤더입니다. 해당 순위표에 대 한 statname에 대 한 지역화 된 "표시 이름"도 반환 됩니다.

<a id="ID4E2G"></a>


### <a name="required-members"></a>필수 멤버

| 멤버| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| 섹션| 선택 사항입니다. 페이지의 마지막 항목의 순위 totalItems 보다 낮은 경우 반환 됩니다. 이 섹션도 때 반환 되지 skipToUser 요청에 지정 됩니다.|
| continuationToken| 문자열| 필수. 값을 원하는 경우 해당 URI에 대 한 결과의 다음 페이지를 가져오려면 "continuationToken" 쿼리 매개 변수를 다시 지정 합니다. 없는 pagingInfo 반환 하는 경우에 데이터를 가져올 수 없습니다 "다음 페이지"입니다.|
| totalItems| 64 비트 부호 없는 정수| 필수. 순위표에 있는 항목의 총 수입니다.|
| <b>leaderboardInfo</b>| 섹션| 필수. 항상 반환 됩니다. 요청 된 순위표에 대 한 메타 데이터를 포함 합니다.|
| displayName| string| 필수. 미리 정의 된 순위표에 대 한 지역화 된 표시 이름입니다. 예제 값: "캡처된 플래그 총".|
| totalCount| string| 필수. 순위표에 있는 항목의 총 수입니다.|
| 열| array| 필수.|
| displayName| string| 필수. 순위표 열에 해당합니다.|
| statName| string| 필수. 순위표 열에 해당합니다.|
| 유형| 문자열| 필수. 순위표 열에 해당합니다.|
| <b>userList</b>| 섹션| 필수. 항상 반환 됩니다. 요청 된 순위표에 대 한 실제 사용자 점수를 포함 되어 있습니다.|
| 게이머 태그| string| 필수. 순위표 항목에서 사용자에 해당합니다.|
| xuid| 32 비트 부호 있는 정수| 필수. 순위표 항목에서 사용자에 해당합니다.|
| % 째| string| 필수. 순위표 항목에서 사용자에 해당합니다.|
| 순위| string| 필수. 순위표 항목에서 사용자에 해당합니다.|
| 값| array| 필수. 각 쉼표로 구분 된 값은 순위표 열에 해당합니다.|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 304| 수정 되지 않음|  |
| 400| 잘못 된 요청| | 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| | 필요한 사용자 인증을 요청 합니다.|
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.|
| 404| 찾을 수 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.|
| 406| 허용할 수 없음| 리소스 버전이 지원 되지 않습니다. MVC 계층에 의해 거부 되어야 합니다.|
| 408| 요청 시간 제한| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 응답 본문의 mime 형식입니다. 예제 값: "application/json".|
| Content-Length| string| 응답에 전송 되는 바이트 수입니다. 예제 값: "232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>선택적 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| string| 캐시 최적화에 사용 됩니다. 예제 값: "686897696a7c876b7e".|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>응답 본문

소셜 순위표, 없음 페이징에 대 한 요청:

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>예제 응답


```cpp
{
    "pagingInfo": null,
    "leaderboardInfo": {
        "displayName": "Kills",
        "totalCount": 3,
        "columns": [
            {
                "displayName": "Kills",
                "statName": "enemydefeats",
                "type": "integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag":"xxxSniper39",
            "xuid":"1234567890123555",
            "percentile":1.0,
            "rank":1,
            "values": [
                "47"
            ]
        },
        {
            "gamertag":"WarriorSaint",
            "xuid":"2533274916402282",
            "percentile":0.66,
            "rank":2,
            "values": [
                "42"
            ]
        },
        {
            "gamertag":"WhockaWhocka",
            "xuid":"1234567890123666",
            "percentile":0.33,
            "rank":3,
            "values": [
                "12"
            ]
        }
    ]
}

```


<a id="ID4EXKAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EZKAC"></a>


##### <a name="parent"></a>부모

[/ 사용자/xuid ({xuid}) /scids/ {서비스 안내} /stats/{statname {) /people/{all {all\ | favorite}](uri-usersxuidscidstatnamepeople.md)
