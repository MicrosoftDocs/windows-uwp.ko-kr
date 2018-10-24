---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/people)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d08a8ff9e04b255944128ffc1cd1c0b101180d8f
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483516"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
호출자의 사용자 컬렉션을 가져옵니다.
이러한 Uri의 도메인은 `social.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [쿼리 문자열 매개 변수](#ID4EJB)
  * [권한 부여](#ID4ERD)
  * [필요한 요청 헤더](#ID4EZE)
  * [선택적 요청 헤더](#ID4EYF)
  * [요청 본문](#ID4E5G)
  * [HTTP 상태 코드입니다.](#ID4EJH)
  * [필수 응답 헤더](#ID4EBBAC)
  * [응답 본문](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

한 번 또는 여러 번 실행 하는 경우 동일한 결과 얻을 것이 GET 작업 리소스 수정 되지 않습니다.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 해당 리소스에 액세스 하 고 사용자 식별자입니다. 인증된 된 사용자를 일치 해야 합니다. 가능한 값에는 "나", xuid({xuid}), 또는 gt({gamertag})은.|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- |
| 보기| string| 보기와 관련 된 사용자를 반환 합니다. 기본값은 "all"입니다. 가능한 값은: <ul><li><b>모든</b> -사용자의 사용자 목록에 모든 사용자를 반환 합니다. 기본값입니다.</li><li><b>즐겨찾기</b> -즐겨찾기 특성을 가진 사용자의 사용자 목록에 모든 사용자를 반환 합니다.</li><li><b>LegacyXboxLiveFriends</b> -레거시 Xbox LIVE 친구는 또한 사용자의 사용자 목록에 모든 사용자를 반환 합니다.</li></br>**참고:**  호출 하는 사용자가 소유 하는 사용자와 다른 **모든** 값에만 사용할 수 있습니다.|
| startIndex| 32 비트 부호 없는 정수| 지정된 된 인덱스에서 시작 하는 항목을 반환 합니다.  
| maxItems| 32 비트 부호 없는 정수| 시작 인덱스에서 시작 하 여 컬렉션에서 반환 하는 사람들의 최대 수입니다. 서비스가는 <b>maxItems</b> 존재 이며 (경우에 결과의 마지막 페이지에서 아직 반환 되지 않은) <b>maxItems</b> 보다 적은 반환할 수 있습니다 기본값을 제공할 수 있습니다.|

<a id="ID4ERD"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 응답이 없는 경우|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| 예| 호출자가 사용자의 Xbox 사용자 ID (XUID).| 401 권한이 없음|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열입니다. Xbox LIVE에 대 한 권한 부여 데이터입니다. 일반적으로 암호화 된 XSTS 토큰입니다. 예제 값: <b>XBL3.0 x =&lt;userhash >;&lt; 토큰 ></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion| 빌드 이름/번호는 Xbox LIVE 서비스를이 요청 하시기 바랍니다. 요청 헤더, 등 인증 토큰에서 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅되는. 기본값: 1.|
| 수락| 문자열입니다. 콘텐츠 유형은 응답에서 호출자를 허용 하는. 모든 응답은 <b>응용 프로그램/json</b>입니다.|

<a id="ID4E5G"></a>


## <a name="request-body"></a>요청 본문

이 요청의 본문 개체가 전달 됩니다.

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드입니다.

서비스는이 리소스에는이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 하는 표준 HTTP 상태 코드의 전체 목록은, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하십시오.

| Code| 원인 문구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 성공입니다.|
| 400| 잘못 된 요청| 쿼리 매개 변수 또는 사용자 Id 형식이 잘못 되었습니다.|
| 403| 사용할 수 없음| XUID 클레임 인증 헤더를 구문 분석 될 수 있습니다.|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>필수 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| 32 비트 부호 없는 정수| 길이 바이트 단위로 응답 본문의. 예제 값: 22.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 항상 <b>응용 프로그램/json</b>됩니다.|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>응답 본문

호출이 성공 하면 서비스 사람 컬렉션에서 호출자의 호출자의 사용자 컬렉션을 포함 하는 배열에 사람들의 총 수를 반환 합니다. [(JSON) PeopleList](../../json/json-peoplelist.md)을 참조 하십시오.

<a id="ID4EZCAC"></a>


### <a name="sample-response"></a>샘플 응답입니다.


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


##### <a name="parent"></a>부모

[/users/{ownerId}/people](uri-usersowneridpeople.md)
