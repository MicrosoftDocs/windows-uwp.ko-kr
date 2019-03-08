---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
description: " GET (/users/{ownerId}/people)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c8672188a93b2e8d27a081ae068387e7ee7aa42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623768"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
호출자의 사용자 컬렉션을 가져옵니다.
이러한 Uri에 대 한 도메인은 `social.xboxlive.com`합니다.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [쿼리 문자열 매개 변수](#ID4EJB)
  * [권한 부여](#ID4ERD)
  * [필요한 요청 헤더](#ID4EZE)
  * [선택적 요청 헤더](#ID4EYF)
  * [요청 본문](#ID4E5G)
  * [HTTP 상태 코드](#ID4EJH)
  * [필요한 응답 헤더](#ID4EBBAC)
  * [응답 본문](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

GET 작업 하므로 한 번 또는 여러 번 실행 하는 경우 동일한 결과 생성은이 모든 리소스를 수정 하지 않습니다.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| ownerId| 문자열| 해당 리소스에 액세스 하는 사용자의 식별자입니다. 인증된 된 사용자를 일치 해야 합니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag})입니다.|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- | --- |
| 보기| 문자열| 뷰를 사용 하 여 연결 된 사용자를 반환 합니다. 기본값은 "모두"입니다. 가능한 값은: <ul><li><b>모든</b> -사용자의 사용자 목록에 모든 사용자를 반환 합니다. 기본값입니다.</li><li><b>즐겨 찾는</b> -즐겨찾기 특성을 가진 사용자의 사용자 목록에 모든 사용자를 반환 합니다.</li><li><b>LegacyXboxLiveFriends</b> -레거시 Xbox LIVE 친구 수도 있는 사용자의 사용자 목록에 모든 사용자를 반환 합니다.</li></br>**참고:**  만 **모든** 호출 하는 사용자 소유 사용자와 다른 경우 값을 지원 합니다.|
| startIndex| 32 비트 부호 없는 정수| 지정된 된 인덱스에서 시작 하는 항목을 반환 합니다.  
| maxItems| 32 비트 부호 없는 정수| 사용자는 시작 인덱스에서 시작 하는 컬렉션에서 반환할 최대 수입니다. 서비스 하는 경우 기본 값을 제공할 수 있습니다 <b>maxItems</b> 없고 미만 반환할 수 있습니다 <b>maxItems</b> (경우에 결과의 마지막 페이지 아직 반환 되지 않은).|

<a id="ID4ERD"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 응답 없는 경우|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| 예| 호출자가 사용자의 Xbox 사용자 ID (XUID).| 401 권한이 없음|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열입니다. Xbox LIVE에 대 한 권한 부여 데이터입니다. 이것이 일반적으로 암호화 된 XSTS 토큰입니다. 예를 들어 값: <b>XBL3.0 x=&lt;userhash>;&lt;token></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 기본값: 1.|
| 수락| 문자열입니다. 호출자가 응답에 허용 하는 콘텐츠-형식입니다. 모든 응답이 <b>application/json</b>합니다.|

<a id="ID4E5G"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 성공 했습니다.|
| 400| 잘못된 요청| 쿼리 매개 변수 또는 사용자 Id 형식이 잘못 되었습니다.|
| 403| 사용할 수 없음| XUID 클레임 권한 부여 헤더에서 분석할 수 없습니다.|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| 32 비트 부호 없는 정수| 길이 (바이트)를 응답 본문입니다. 예를 들어 값: 22.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 이 값은 항상 <b>application/json</b>합니다.|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>응답 본문

호출이 성공 하면 서비스 호출자의 사용자 컬렉션 및 호출자의 사용자 컬렉션을 포함 하는 배열에서 사용자의 총 수를 반환 합니다. 참조 [PeopleList (JSON)](../../json/json-peoplelist.md)합니다.

<a id="ID4EZCAC"></a>


### <a name="sample-response"></a>샘플 응답


```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFollowingCaller": false,
            "isFavorite": false
        },
    ],
    "totalCount": 3
}

```


<a id="ID4EDDAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EFDAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people](uri-usersowneridpeople.md)
