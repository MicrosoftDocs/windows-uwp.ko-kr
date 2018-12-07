---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
description: " GET (/users/{ownerId}/people/mute)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 94e2bf4d04619ffa3348ae08fc37964cdc58e7b5
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8783622"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
사용자에 대 한 음소거 목록을 가져옵니다.

  * [설명](#ID4EQ)
  * [URI 매개 변수](#ID4EZ)
  * [리소스의 개인 정보 설정의 효과](#ID4EEB)
  * [권한 부여](#ID4ENB)
  * [필요한 요청 헤더](#ID4ESC)
  * [요청 본문](#ID4EPE)
  * [HTTP 상태 코드](#ID4E1E)
  * [필요한 응답 헤더](#ID4E3G)
  * [응답 본문](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>설명

대상 지정이 URI 사용자가 음소거 목록, 그렇지 않으면 비어 있는 경우 사용자가 없는 경우 해당 사용자만 반환 합니다.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 필수. 해당 리소스를 액세스 하는 사용자의 식별자입니다. 가능한 값은 "me" <code>xuid({xuid})</code>, 또는 gt({gamertag}) 합니다. 인증된 된 사용자 여야 합니다. 예제 값: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. 최대 크기: 없음. |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스의 개인 정보 설정의 효과

없음.

<a id="ID4ENB"></a>


## <a name="authorization"></a>권한 부여

사용 권한 부여 클레임 | 클레임| 유형| 필수 여부| 예제 값|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| 64 비트의 부호 있는 정수| 예| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여 | 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <code>Xauth=&lt;authtoken></code>. 최대 크기: 없음.|
| X RequestedServiceVersion| string| 이 요청은 전송 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 예제 값: <code>1</code>, <code>vnext</code>. 최대 크기: 없음.|
| 수락| string| 허용 되는 콘텐츠-형식입니다. 예제 값: <code>application/json</code>. 최대 크기: 없음.|

<a id="ID4EPE"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 음소거 목록에 대 한 성공적으로 요청 합니다.|
| 400| 잘못 된 요청| URI에 지정 된 대상 ID 올바르지 않습니다.|
| 403| 금지| URI에 지정 된 소유자 인증 된 사용자가 아닙니다.|
| 404| 찾을 수 없음| URI에 지정 된 소유자 존재 하지 않습니다.|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 요청 본문의 MIME 형식입니다. 예제 값: <code>application/json</code>|
| Content-Length| string| 응답에 전송 되는 바이트 수입니다. 예제 값: 34|
| 캐시 제어| string| 캐시 동작을 지정 하려면 서버에서 정중 요청 합니다. 예제: <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>예제 응답

[UserList](../../json/json-userlist.md)를 참조 하세요.


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EJBAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ELBAC"></a>


##### <a name="parent"></a>부모

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)
