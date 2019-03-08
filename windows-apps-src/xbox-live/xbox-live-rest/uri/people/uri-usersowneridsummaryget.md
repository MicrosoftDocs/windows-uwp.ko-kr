---
title: GET (/users/{ownerId}/summary)
assetID: 754190c9-b15d-f34b-1dca-5c92f6f67d12
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummaryget.html
description: " GET (/users/{ownerId}/summary)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3b228adab7b035ec8f4e65fc8b7458228a677987
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613998"
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

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| ownerId| 문자열| 해당 리소스에 액세스 하는 사용자의 식별자입니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag})입니다. 예제 값: <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4E2"></a>


## <a name="authorization"></a>권한 부여

| <b>이름</b>| <b>Type</b>| <b>설명</b>|
| --- | --- | --- | --- | --- | --- |
| xuid| 64 비트 부호 없는 정수| 필수. 호출자의 사용자 식별자입니다. 예를 들어 값: 2533274790395904|

<a id="ID4EBC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| 권한 부여 데이터입니다. 이것이 일반적으로 암호화 된 XSTS 토큰입니다. 예를 들어 값: <b>XBL3.0 x=[hash];[token]</b>.|

<a id="ID4EHD"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-xbl-contract-version| 문자열| 빌드는이 요청을 보내야 하는 서비스의 이름/번호입니다. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제 값: 1|
| 수락| 문자열| 허용 되는 콘텐츠-유형입니다. 모든 응답 됩니다 <code>application/json</code>합니다.|

<a id="ID4EXE"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4ECF"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 잘못된 요청| 사용자 Id 형식이 잘못 되었습니다.|
| 403| 사용할 수 없음| XUID 클레임 권한 부여 헤더에서 분석할 수 없습니다.|

<a id="ID4EZG"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| 문자열| 응답에는 전송 중인 바이트 수입니다. 예를 들어 값: 232.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 이 여야 <b>application/json</b>합니다.|

<a id="ID4EGAAC"></a>


## <a name="response-body"></a>응답 본문

참조 [PersonSummary (JSON)](../../json/json-personsummary.md)합니다.

<a id="ID4ESAAC"></a>


### <a name="sample-response"></a>샘플 응답


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


##### <a name="parent"></a>Parent

[/users/{ownerId}/summary](uri-usersowneridsummary.md)
