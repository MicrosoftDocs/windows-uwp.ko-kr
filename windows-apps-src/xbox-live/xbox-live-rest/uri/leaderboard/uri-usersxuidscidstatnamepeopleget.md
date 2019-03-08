---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 674e52d15d115560d4813edcd9687c2fe9694c9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650768"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
소셜 leaderboard 순위는 stat 현재 사용자의 모든 알려진된 연락처 중 하나에 해당 사용자가 즐겨 찾는 사용자로 지정 된 연락처만 (점수) 값을 반환 합니다.
이러한 Uri에 대 한 도메인은 `leaderboards.xboxlive.com`합니다.

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

순위표 Api는 모든 읽기 전용 및 따라서 (https)만 GET 동사를 지원 합니다. 등급 및 정렬 된 "페이지" 데이터 플랫폼을 통해 작성 된 개별 사용자 통계에서 파생 되는 인덱싱된 플레이어 통계 하 게 반영 합니다. 전체 순위표 인덱스 크기가 매우 커질 수 있습니다 하 고 전체에서 참조를 호출자에 게 묻지 않습니다 하므로이 URI는 순위표에 대 한 참조 하려고 하는 보기의 종류에 대 한 구체적으로 호출자를 허용 하는 몇 가지 쿼리 문자열 인수를 지원 합니다.

GET 작업 하므로 한 번 또는 여러 번 실행 하는 경우 동일한 결과 생성은이 모든 리소스를 수정 하지 않습니다.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| xuid| 문자열| 사용자의 식별자입니다.|
| scid| 문자열| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.|
| statname| 문자열| 액세스 하 고 사용자 상태 리소스의 고유 식별자입니다.|
| 모든|즐겨찾기| 열거형| 순위는 stat 것인지 (점수)이 현재 사용자의 모든 알려진된 연락처 또는 해당 사용자가 즐겨 찾는 사용자로 지정 된 연락처만 대 한 값입니다.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- | --- |
| maxItems| 32 비트 부호 없는 정수| 페이지 결과에 반환할 순위표 레코드의 최대 수입니다. 지정 하지 않으면 기본 번호 (10)을 반환 됩니다. MaxItems의 최대값을 여전히 정의 되지 하 하지만 큰 데이터 집합을 방지 하려면이 값은 대상 아마도 가장 큰 설정 하는 튜너 UI 호출 별로 처리할 수 없습니다. |
| skipToRank| 32 비트 부호 없는 정수| 지정 된 순위표 순위부터 결과 페이지를 반환 합니다. 결과의 나머지 부분 등급에 따라 정렬 됩니다. 이 쿼리 문자열 "는" 결과의 다음 페이지를 가져오기 위해 다음 쿼리를 다시 공급할 수 있는 연속 토큰을 반환할 수 있습니다. |
| skipToUser| 문자열| 해당 사용자의 순위 또는 점수에 관계 없이 지정한 게이머 xuid 순위표 결과 페이지를 반환 합니다. 페이지는 미리 정의 된 보기에 대 한 페이지의 마지막 위치 또는 stat 순위표 보기에 대 한 중간에 지정된 된 사용자를 사용 하 여 백분위 수 등급에 따라 정렬 됩니다. 방법이 없는 <b>continuationToken</b> 이 쿼리 유형에 대해 제공 합니다. |
| continuationToken| 문자열| 이전 호출에서 반환 하는 경우는 <b>continuationToken</b>, 호출자에 게 다시 전달할 수 있습니다 해당 토큰을 "있는 그대로" 결과의 다음 페이지를 가져오는 쿼리 문자열에서을 합니다. |
| sort| 문자열| 낮음-높은 값 순서 ("오름차순")에서 플레이어 목록은 순위를 사용할지 또는 높음-낮음 값 순서 ("descending"). 이 선택적 매개 변수입니다. 기본 순서는 내림차순입니다.|

<a id="ID4EQD"></a>


## <a name="authorization"></a>권한 부여

Xuid 권한이 필요 합니다.

콘텐츠 격리 및 액세스 제어 시나리오의 목적에 대 한 권한 부여 논리 구현 됩니다.

호출자에 게 유효한 XSTS 토큰 요청을 사용 하 여 제출 된 순위표 및 사용자 통계를 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 데이터 플랫폼에서 지 원하는 클라이언트 (물론) 제한 됩니다.

제목 개발자에 따라 개방 또는 제한 XDP 또는 파트너 센터를 사용 하 여 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자는 샌드박스에 허용으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여는 샌드박스에 XDP 또는 파트너 센터를 통해 관리 됩니다.

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열입니다. HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".|
| Content-Type| 문자열입니다. 요청 본문의 MIME 형식입니다. 예제 값: "application/json"입니다.|
| X-RequestedServiceVersion| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.|
| 수락| 문자열입니다. 허용 되는 콘텐츠 형식 값입니다. 예제 값: "application/json"입니다.|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| 문자열입니다. 엔터티 태그-클라이언트 캐싱을 지 원하는 경우 사용 합니다. 예를 들어 값: "686897696a7c876b7e".|

<a id="ID4ETG"></a>


## <a name="request-body"></a>요청 본문

적절 한 표시를 위해 다시 가져오는 데이터를 이해 하는 모든 호출자의 기능을 최대화 하려면 각 사용자에 대 한 각 상태 값이 있는 수를 표시 하 고 수용-언어에 지정 된 로캘에 맞게 형식이 지정 된 형식 문자열로 반환 됩니다. 요청에 헤더입니다. 해당 순위표에 대 한 statname에 대 한 지역화 된 "표시 이름"도 반환 됩니다.

<a id="ID4E2G"></a>


### <a name="required-members"></a>필요한 멤버

| 멤버| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| 섹션| 선택 사항. 페이지에서 마지막 항목의 순위는 totalItems 보다 작은 경우 반환 됩니다. 이 섹션에서는 반환 되지 않습니다 skipToUser 요청에 지정 된 경우.|
| continuationToken| 문자열| 필수. 원하는 경우 해당 URI에 대 한 다음 결과 페이지를 가져오려고 "continuationToken" 쿼리 매개 변수를 다시 제공 해야 하는 값을 지정 합니다. 없는 pagingInfo 반환 되 면 다음 방법이 없는 "다음"데이터 페이지를 가져올 수입니다.|
| totalItems| 64 비트 부호 없는 정수| 필수. 순위 표에서 항목의 총 수입니다.|
| <b>leaderboardInfo</b>| 섹션| 필수. 항상 반환 됩니다. 요청한 순위표에 대 한 메타 데이터를 포함 합니다.|
| displayName| 문자열| 필수. 미리 정의 된 순위표에 대 한 지역화 된 표시 이름입니다. 예를 들어 값: "플래그 캡처된 총"입니다.|
| totalCount| 문자열| 필수. 순위 표에서 항목의 총 수입니다.|
| 열| 배열| 필수.|
| displayName| 문자열| 필수. 순위 표에서 열에 해당합니다.|
| statName| 문자열| 필수. 순위 표에서 열에 해당합니다.|
| 유형| 문자열| 필수. 순위 표에서 열에 해당합니다.|
| <b>userList</b>| 섹션| 필수. 항상 반환 됩니다. 요청한 순위표에 대 한 실제 사용자 점수를 포함 합니다.|
| 게이머 태그| 문자열| 필수. 순위표 항목에서 사용자에 해당합니다.|
| xuid| 32 비트 부호 있는 정수| 필수. 순위표 항목에서 사용자에 해당합니다.|
| 백분위 수| 문자열| 필수. 순위표 항목에서 사용자에 해당합니다.|
| 순위| 문자열| 필수. 순위표 항목에서 사용자에 해당합니다.|
| 값| 배열| 필수. 각 쉼표로 구분 된 값 순위 표에서 열에 해당합니다.|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 304| 수정 안 됨|  |
| 400| 잘못된 요청| | 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| | 요청에 사용자 인증이 필요합니다.|
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.|
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.|
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다. MVC 계층에서 거부 해야 합니다.|
| 408| 요청 시간 초과| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 응답 본문의 mime 형식입니다. 예제 값: "application/json"입니다.|
| Content-Length| 문자열| 응답에는 전송 중인 바이트 수입니다. 예를 들어 값: "232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>선택적 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| 문자열| 캐시 최적화를 위해 사용 합니다. 예를 들어 값: "686897696a7c876b7e".|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>응답 본문

소셜 순위표를 페이징하지 않음을 대 한 요청:

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>샘플 응답


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


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite}](uri-usersxuidscidstatnamepeople.md)
