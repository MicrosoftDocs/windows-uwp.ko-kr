---
title: GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
assetID: ee69a9e3-ea91-3cf5-a10a-811762cba892
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnamegetvaluemetadata.html
author: KevinAsgari
description: " GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 63f5d2967219a909a82e0e638fb9c852670e9749
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4418634"
---
# <a name="get-scidsscidleaderboardsleaderboardnameincludevaluemetadata"></a>GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
 
미리 정의 된 전역 순위표를 함께 순위표 값와 관련 된 메타 데이터를 가져옵니다.
 
이러한 Uri에 대 한 도메인은 `leaderboards.xboxlive.com`.
 
  * [설명](#ID4EY)
  * [URI 매개 변수](#ID4EHB)
  * [쿼리 문자열 매개 변수](#ID4ESB)
  * [권한 부여](#ID4EVD)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EQE)
  * [필요한 요청 헤더](#ID4EZE)
  * [선택적 요청 헤더](#ID4EOG)
  * [HTTP 상태 코드](#ID4EOH)
  * [응답 헤더](#ID4EFDAC)
  * [응답 본문](#ID4ECFAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>설명
 
? 포함 = valuemetadata 쿼리 매개 변수는 응답 순위표 값와 관련 된 메타 데이터를 포함 하도록 합니다. 지정 된 클라이언트를 포함 하는 값 메타 데이터 모델 등 경합 트랙에 가장 좋은 시간을 달성 하는 데 사용 되는 자동차의 색 값과 관련 된 데이터.
 
메타 데이터 값 하 고 순위표 기반으로, 순위표 자체에 없는 사용자 상태에서 정의 됩니다.
 
순위표 Api는 모든 읽기 전용 하 고 따라서 GET 동사만 지원 합니다. 순위 되 고 정렬 된 "페이지" 데이터 플랫폼을 통해 작성 된 개별 사용자 통계에서 파생 되는 인덱싱된 플레이어 통계을 반영 합니다. 전체 순위표 인덱스 상당히 클 수 및 호출자 전체에서 하나 묻지 않습니다, 그리고 따라서이 URI에 대해 해당 순위표 보려는 보기의 종류에 대 한 특정 호출자를 사용할 수 있는 몇 가지 쿼리 문자열 인수를 지원 합니다.
 
한 번 또는 여러 번 실행 하는 경우 동일한 결과 생성 합니다이 GET 작업 리소스 수정 되지 않습니다.
  
<a id="ID4EHB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.| 
| leaderboardname| string| 액세스 되는 미리 정의 된 순위표 리소스의 고유 식별자입니다.| 
  
<a id="ID4ESB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 포함 = valuemetadata| string| 응답 순위표 값와 관련 된 모든 값 메타 데이터를 포함 하는 것을 나타냅니다.| 
| maxItems| 32 비트 부호 없는 정수| 결과의 페이지에서 반환할 순위표 레코드의 최대 수입니다. 지정 하지 않으면 기본 번호 (10)을 반환 됩니다. MaxItems에 대 한 최대 값 여전히 정의 되지 않습니다. 하지만 대규모 데이터 집합을 방지 하려면, 것이 값을 대상으로 해야 하므로 가장 큰 설정 하는 튜너 UI 호출 당 처리할 수 있습니다.| 
| skipToRank| 32 비트 부호 없는 정수| 지정 된 순위표 순위부터 결과 페이지를 반환 합니다. 결과의 나머지 순위 순서 대로 정렬 됩니다. 이 쿼리 문자열 "다음 페이지"의 결과 얻으려면 후속 쿼리도 다시 입력 될 수 있는 연속 토큰을 반환할 수 있습니다.| 
| skipToUser| string| 해당 사용자의 순위 또는 점수에 관계 없이 지정 된 게이머 xuid 주위 순위표 결과 페이지를 반환 합니다. 페이지 % 째 순위 지정 된 사용자 통계 순위표 보기에 대 한 중간 또는 미리 정의 된 보기에 대 한 페이지의 마지막 위치에 정렬 됩니다. 이 유형의 쿼리 제공 없는 continuationToken 있습니다.| 
| continuationToken| 문자열| 이전에는 continuationToken 반환 되 면 다음 호출자에 게 전달할 수 다시 "는" 있는 그대로 토큰 쿼리 문자열을 결과의 다음 페이지를 가져옵니다.| 
  
<a id="ID4EVD"></a>

 
## <a name="authorization"></a>권한 부여
 
권한 부여 논리 콘텐츠 격리 및 액세스 제어 시나리오에 대해 구현 있습니다.
 
   * 순위표와 사용자 통계 호출자가 요청을 사용 하 여 유효한 XSTS 토큰 제출 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 데이터 플랫폼에서 지원 되는 클라이언트 분명히 제한 됩니다.
   * 제목 개발자 열기 또는 XDP 또는 개발자 센터를 사용 하 여 제한 된 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자가 해당 샌드박스에 인증으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여 샌드박스에 XDP 또는 개발자 센터를 통해 관리 됩니다.
  
검사에 대 한 의사 코드는 다음과 같습니다.
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EQE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과
 
개인 정보 보호 검사 순위표 데이터를 읽을 때 수행 됩니다.
  
<a id="ID4EZE"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열입니다. HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: XBL3.0 <b>x =&lt;userhash >;&lt; 토큰 ></b>| 
| Xbl 계약 버전 X| 문자열입니다. 사용 하는 API의 버전을 나타냅니다. 응답에 메타 데이터 값을 포함 하기 위해이 값을 "3" 설정 해야 합니다.| 
| X RequestedServiceVersion| 문자열입니다. 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 기본값: 1입니다.| 
| 수락| 문자열입니다. 허용 되는 콘텐츠-형식입니다. 예제 값: <b>응용 프로그램/j</b>| 
  
<a id="ID4EOG"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| 문자열입니다. 엔터티 태그를 사용 하는 경우 캐싱을 지원 하 고 클라이언트 예제 값: <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4EOH"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 304| 수정 되지 않음| 리소스 되지 요청 마지막으로 수정 합니다.| 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음| 리소스 버전은 지원 되지 않습니다.| 
| 408| 요청 시간 제한| 리소스 버전이 지원 되지 않습니다. MVC 계층에 의해 거부 되어야 합니다.| 
  
<a id="ID4EFDAC"></a>

 
## <a name="response-headers"></a>응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| 문자열| 필수. 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| Content-Length| string| 필수. 응답에 전송 되는 바이트 수입니다. 예: <b>232</b>합니다.| 
| ETag| string| 선택 사항입니다. 최적화 캐시에 사용 됩니다. 예: <b>686897696a7c876b7e</b>합니다.| 
  
<a id="ID4ECFAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EIFAC"></a>

 
### <a name="response-members"></a>응답 멤버
 
pagingInfo | pagingInfo| 섹션| 선택 사항입니다. MaxItems 요청에 지정 된 경우에 표시 합니다.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| continuationToken| 64 비트 부호 없는 정수| 필수. 값을 원하는 경우 해당 URI에 대 한 결과의 다음 페이지를 가져오려면 <b>skipItems</b> 쿼리 매개 변수를 다시 지정 합니다. <b>PagingInfo</b> 없는 반환 됩니다 다음 없는 경우 얻을 수 있는 데이터의 다음 페이지 없습니다.| 
| totalItems| 64 비트 부호 없는 정수| 필수. 순위표에 있는 항목의 총 수입니다. 예제 값: 1234567890| 
 
leaderboardInfo | leaderboardInfo| 섹션| 필수. 항상 반환 됩니다. 요청 된 순위표에 대 한 메타 데이터를 포함 합니다.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| string| 사용 되지 않습니다.| 
| totalCount| 문자열 (64 비트 부호 없는 정수)| 필수. 순위표에 있는 항목의 총 수입니다. 예제 값: 1234567890| 
| columnDefinition| JSON 개체입니다.| 필수. 열은 순위표에 설명합니다.| 
| columnDefinition.displayName| string| 필수. 설명 하는 이름 열에는 순위표입니다.| 
| columnDefinition.statName| string| 필수. 순위표를 기반으로 하는 사용자 상태의 이름입니다.| 
| columnDefinition.type| string| 필수. 데이터 형식 열에 사용자 상태입니다.| 
 
userList | userList| 섹션| 필수. 항상 반환 됩니다. 요청 순위표에 대 한 실제 사용자 점수가 포함 되어 있습니다.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 게이머 태그| string| 필수. 순위표 항목에 대 한 사용자의 게이머 합니다.| 
| xuid| 64 비트 부호 없는 정수| 필수. 순위표 항목에 대 한 사용자의 Xbox 사용자 ID입니다.| 
| % 째| string| 필수. 사용자의 % 째 순위에 순위표 합니다.| 
| 순위| string| 필수. 사용자의 숫자 순위는 순위표에 있습니다.| 
| value| string| 필수. 사용자의 값은 순위표 기반으로 하는 상태입니다.| 
| valueMetadata| string| 필수. 문자열을 쉼표로 구분 "\"name\"형식의 문자열 쌍: \"value\ ",..."

이 값은 빈 문자열 메타 데이터가 있으면. | 
  
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>예제 응답
 
다음 요청 URI 전역 순위표에 순위를 건너뛰는 보여 줍니다.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured?include=valuemetadata&maxItems=3&skipToRank=15000
         
```

 
메타 데이터 값을 반환 하기 위해도 다음 헤더를 지정 해야 합니다.
 

```cpp
X-Xbl-Contract-Version : 3
          
```

 
위의 URI는 다음과 같은 JSON 응답을 반환합니다.
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "15003",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columnDefinition" : 
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            }
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": "1234567890123444",
            "percentile": 0.64,
            "rank": 15000,
            "value": "1002",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2000, \"israining\" : false}"
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": "1234567890123555",
            "percentile": 0.64,
            "rank": 15001,
            "value": "993",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2020, \"israining\" : true}"
 
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": "1234567890123666",
            "percentile": 0.64,
            "rank": 15002,
            "value": "700",
            "valuemetadata" : "{\"color\" : \"red\",\"weight\" : 300, \"israining\" : false}"
        }
    ]
}
         
```

   
<a id="ID4E6LAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBMAC"></a>

 
##### <a name="parent"></a>부모 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   