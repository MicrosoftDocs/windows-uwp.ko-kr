---
title: GET (/scids/{scid}/leaderboards/{leaderboardname})
assetID: 4adea46c-e910-8709-c28e-ce9178e712ef
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnameget.html
description: " GET (/scids/{scid}/leaderboards/{leaderboardname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0d313262a642ee0db956f6d3264025931e63e8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629498"
---
# <a name="get-scidsscidleaderboardsleaderboardname"></a>GET (/scids/{scid}/leaderboards/{leaderboardname})
 
미리 정의 된 전체 순위표를 가져옵니다.
 
이러한 Uri에 대 한 도메인은 `leaderboards.xboxlive.com`합니다.
 
  * [설명](#ID4EY)
  * [URI 매개 변수](#ID4EDB)
  * [쿼리 문자열 매개 변수](#ID4EOB)
  * [권한 부여](#ID4EID)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EDE)
  * [필요한 요청 헤더](#ID4EME)
  * [선택적 요청 헤더](#ID4E1F)
  * [HTTP 상태 코드](#ID4E1G)
  * [응답 헤더](#ID4ERCAC)
  * [응답 본문](#ID4EOEAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>설명
 
순위표 Api는 모든 읽기 전용 및 따라서 GET 동사를만 지원 합니다. 등급 및 정렬 된 "페이지" 데이터 플랫폼을 통해 작성 된 개별 사용자 통계에서 파생 되는 인덱싱된 플레이어 통계 하 게 반영 합니다. 전체 순위표 인덱스 크기가 매우 커질 수 있습니다 하 고 전체에서 참조를 호출자에 게 묻지 않습니다 하므로이 URI는 순위표에 대 한 참조 하려고 하는 보기의 종류에 대 한 구체적으로 호출자를 허용 하는 몇 가지 쿼리 문자열 인수를 지원 합니다.
 
GET 작업 하므로 한 번 또는 여러 번 실행 하는 경우 동일한 결과 생성은이 모든 리소스를 수정 하지 않습니다.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.| 
| leaderboardname| 문자열| 액세스 되는 미리 정의 된 순위표 리소스의 고유 식별자입니다.| 
  
<a id="ID4EOB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| maxItems| 32 비트 부호 없는 정수| 페이지 결과에 반환할 순위표 레코드의 최대 수입니다. 지정 하지 않으면 기본 번호 (10)을 반환 됩니다. MaxItems의 최대값을 여전히 정의 되지 하 하지만 큰 데이터 집합을 방지 하려면이 값은 대상 아마도 가장 큰 설정 하는 튜너 UI 호출 별로 처리할 수 없습니다.| 
| skipToRank| 32 비트 부호 없는 정수| 지정 된 순위표 순위부터 결과 페이지를 반환 합니다. 결과의 나머지 부분 등급에 따라 정렬 됩니다. 이 쿼리 문자열 "는" 결과의 다음 페이지를 가져오기 위해 다음 쿼리를 다시 공급할 수 있는 연속 토큰을 반환할 수 있습니다.| 
| skipToUser| 문자열| 해당 사용자의 순위 또는 점수에 관계 없이 지정한 게이머 xuid 순위표 결과 페이지를 반환 합니다. 페이지는 미리 정의 된 보기에 대 한 페이지의 마지막 위치 또는 stat 순위표 보기에 대 한 중간에 지정된 된 사용자를 사용 하 여 백분위 수 등급에 따라 정렬 됩니다. 이 쿼리 유형에 대해 제공 되지 않습니다 continuationToken 있습니다.| 
| continuationToken| 문자열| 이전 호출에는 continuationToken 반환 되 면 다음 호출자에 게 전달할 수 다시 해당 토큰을 "있는 그대로" 결과의 다음 페이지를 가져오는 쿼리 문자열.| 
  
<a id="ID4EID"></a>

 
## <a name="authorization"></a>권한 부여
 
콘텐츠 격리 및 액세스 제어 시나리오에 대해 구현 하는 권한 부여 논리가 있습니다.
 
   * 순위표 및 사용자 통계는 호출자에 게 유효한 XSTS 토큰 요청과 함께 전송 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 분명히 데이터 플랫폼에서 지 원하는 클라이언트 제한 됩니다.
   * 제목 개발자에 따라 개방 또는 제한 XDP 또는 파트너 센터를 사용 하 여 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자는 샌드박스에 허용으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여는 샌드박스에 XDP 또는 파트너 센터를 통해 관리 됩니다.
  
확인에 대 한 의사 (pseudo) 코드는 다음과 같습니다.
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EDE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과
 
개인 정보 보호 검사 안 함 순위표 데이터를 읽을 때 수행 됩니다.
  
<a id="ID4EME"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열입니다. HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: <b>XBL3.0 x=&lt;userhash>;&lt;token></b>| 
| X-RequestedServiceVersion| 문자열입니다. 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 기본값: 1.| 
| 수락| 문자열입니다. 허용 되는 콘텐츠-유형입니다. 예제 값: <b>application/json</b>| 
  
<a id="ID4E1F"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| 문자열입니다. 클라이언트 캐싱을 지 원하는 경우 사용 되는 엔터티 태그입니다. 예를 들어 값: <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4E1G"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 304| 수정 안 됨| 리소스 되지 마지막 요청 이후 수정 합니다.| 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과| 리소스 버전이 지원 되지 않습니다. MVC 계층에서 거부 해야 합니다.| 
  
<a id="ID4ERCAC"></a>

 
## <a name="response-headers"></a>응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| 문자열| 필수. 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| Content-Length| 문자열| 필수. 응답에는 전송 중인 바이트 수입니다. 예제: <b>232</b>.| 
| ETag| 문자열| 선택 사항. 캐시 최적화를 위해 사용 합니다. 예제: <b>686897696a7c876b7e</b>.| 
  
<a id="ID4EOEAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EUEAC"></a>

 
### <a name="response-members"></a>응답 멤버
 
pagingInfo | pagingInfo| 섹션| 선택 사항. MaxItems 요청에 지정 된 경우에 제공 합니다.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| continuationToken| 64 비트 부호 없는 정수| 필수. 다시 제공 해야 하는 값을 지정 합니다 <b>skipItems</b> 쿼리 매개 변수를 원하는 경우 해당 URI에 대 한 결과의 다음 페이지를 가져옵니다. 없으면 <b>pagingInfo</b> 가져올 데이터의 다음 페이지인 다음 반환 됩니다.| 
| totalCount| 64 비트 부호 없는 정수| 필수. 순위 표에서 항목의 총 수입니다. 예를 들어 값: 1234567890| 
 
leaderboardInfo | leaderboardInfo| 섹션| 필수. 항상 반환 됩니다. 요청한 순위표에 대 한 메타 데이터를 포함 합니다.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| 문자열| 사용 되지 않습니다.| 
| totalCount| 문자열 (64 비트 부호 없는 정수)| 필수. 순위 표에서 항목의 총 수입니다. 예를 들어 값: 1234567890| 
| 열| 배열| 필수. 순위 표에서 열입니다.| 
| displayName| 문자열| 필수. 순위 표에서 열에 해당합니다.| 
| statName| 문자열| 필수. 순위 표에서 열에 해당합니다.| 
| 유형| 문자열| 필수. 순위 표에서 열에 해당합니다.| 
 
userList | userList| 섹션| 필수. 항상 반환 됩니다. 요청한 순위표에 대 한 실제 사용자 점수를 포함 합니다.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 게이머 태그| 문자열| 필수. 순위표 항목에서 사용자에 해당합니다.| 
| xuid| 64 비트 부호 없는 정수| 필수. 순위표 항목에서 사용자에 해당합니다.| 
| 백분위 수| 문자열| 필수. 순위표 항목에서 사용자에 해당합니다.| 
| 순위| 문자열| 필수. 순위표 항목에서 사용자에 해당합니다.| 
| 값| 배열| 필수. 각 쉼표로 구분 된 값 순위 표에서 열에 해당합니다.| 
  
<a id="ID4EGKAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 
다음 요청 URI에 전체 순위표를 건너뛰는 중 보여 줍니다.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured/columns/deaths?maxItems=3&skipToRank=15000
         
```

 
위의 URI는 다음과 같은 JSON 응답을 반환합니다.
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "152",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columns": [
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            },
            {
                "displayName": "Deaths",
                "statName": "deaths",
                "type": "Integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": 1234567890123444,
            "percentile": 0.64,
            "rank": 15000,
            "values": [
                1000,
                47
            ]
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": 1234567890123555,
            "percentile": 0.64,
            "rank": 15001,
            "values": [
                998,
                17
            ]
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": 1234567890123666,
            "percentile": 0.64,
            "rank": 15002,
            "values": [
                996,
                2
            ]
        }
    ]
}
         
```

 
다음 요청 URI에 전체 순위표를 사용자에 게 건너뜁니다 보여 줍니다.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/totalflagscaptured?maxItems=3&skipToUser=2343737892734237
         
```

 
위의 URI는 다음과 같은 JSON 응답을 반환합니다.
 

```cpp
{
    "leaderboardInfo": 
    {   
       "displayName": "Total flags captured",
       "totalCount": 23437,
       "columns": [
              {
                  "displayName": "Flags Captured",
                  "statName": "flagscaptured",
                  "type": "Integer"
              }
       ]
    },
    "userList": [
        {
            "gamertag": "AverageJoe",
            "percentile": 55.00,
            "rank": 11718,
            "value": 1219,
            "xuid": 1234567890123444
        },
        {
            "gamertag": "AreUWatchinMe",
            "percentile": 60.00,
            "rank": 14162,
            "value": 1062,
            "xuid": 2343737892734333
        },
         {
            "gamertag": "WarriorSaint",
            "percentile": 64.39,
            "rank": 15000,
            "value ": 1000,
            "xuid": 1234567890123455
        }
    ]
}
         
```

   
<a id="ID4E5KAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EALAC"></a>

 
##### <a name="parent"></a>Parent 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   