---
title: GET (/users/{ownerId}/summary)
assetID: 754190c9-b15d-f34b-1dca-5c92f6f67d12
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummaryget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/summary)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 73ba0cd060b3432de1cbb641a8991283974da192
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5407500"
---
# <a name="get-usersowneridsummary"></a>GET (/users/{ownerId}/summary)
호출자의 관점에서 소유자에 대 한 요약 데이터를 가져옵니다.

  * [URI 매개 변수](#ID4EQ)
  * [권한 부여](#ID4E2)
  * [필요한 요청 헤더](#ID4EBC)
  * [선택적 요청 헤더](#ID4EHD)
  * [요청 본문](#ID4EXE)
  * [HTTP 상태 코드](#ID4ECF)
  * [필요한 응답 헤더](#ID4EZG)
  * [응답 본문](#ID4EGAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 식별자입니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag}) 합니다. 예제 값: <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4E2"></a>


## <a name="authorization"></a>권한 부여

| <b>이름</b>| <b>유형</b>| <b>설명</b>|
| --- | --- | --- | --- | --- | --- |
| xuid| 64 비트 부호 없는 정수| 필수. 호출자의 사용자 식별자. 예제 값: 2533274790395904|

<a id="ID4EBC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| 권한 부여 데이터입니다. 일반적으로 암호화 된 XSTS 토큰입니다. 예제 값: XBL3.0 <b>x = [해시] [ 토큰]</b>합니다.|

<a id="ID4EHD"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| xbl 계약 버전 x| string| 이 요청은 전송 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트됩니다 됩니다. 예제 값: 1|
| 수락| string| 허용 되는 콘텐츠-형식입니다. 모든 응답 됩니다 <code>application/json</code>.|

<a id="ID4EXE"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4ECF"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 잘못 된 요청| 사용자 Id가 잘못 된 형식의 합니다.|
| 403| 사용할 수 없음| 권한 부여 헤더에서 XUID 클레임을 분석할 수 없습니다.|

<a id="ID4EZG"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| string| 응답에 전송 되는 바이트 수입니다. 예제 값: 232 합니다.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. <b>응용 프로그램/j</b>이어야 합니다.|

<a id="ID4EGAAC"></a>


## <a name="response-body"></a>응답 본문

[PersonSummary (JSON)를](../../json/json-personsummary.md)참조 하세요.

<a id="ID4ESAAC"></a>


### <a name="sample-response"></a>예제 응답


```cpp
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}

```


<a id="ID4E3AAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E5AAC"></a>


##### <a name="parent"></a>부모

[/users/{ownerId}/summary](uri-usersowneridsummary.md)
