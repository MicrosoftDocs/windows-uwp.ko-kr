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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661588"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
사용자에 대 한 음소거 목록을 가져옵니다.

  * [설명](#ID4EQ)
  * [URI 매개 변수](#ID4EZ)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EEB)
  * [권한 부여](#ID4ENB)
  * [필요한 요청 헤더](#ID4ESC)
  * [요청 본문](#ID4EPE)
  * [HTTP 상태 코드](#ID4E1E)
  * [필요한 응답 헤더](#ID4E3G)
  * [응답 본문](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>설명

대상이 지정 된 경우이 URI는 사용자가 아닌 경우 음소거 목록에 그렇지 않으면 빈 사용자가 해당 사용자만를 반환 합니다.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| ownerId| 문자열| 필수. 해당 리소스에 액세스 하는 사용자의 식별자입니다. 가능한 값은 "me" <code>xuid({xuid})</code>, 또는 gt({gamertag}) 합니다. 인증 된 사용자 여야 합니다. 예제 값: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>합니다. 최대 크기: 없음. |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과

없음.

<a id="ID4ENB"></a>


## <a name="authorization"></a>권한 부여

권한 부여 클레임 사용 | 클레임| 형식| 필수 여부| 예제 값|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 비트 부호 있는 정수| 예| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여 | 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <code>Xauth=&lt;authtoken></code>합니다. 최대 크기: 없음.|
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 권한 부여 토큰에 클레임의 유효성을 확인 한 후 서비스는 합니다. 예제 값: <code>1</code>, <code>vnext</code>합니다. 최대 크기: 없음.|
| 수락| 문자열| 허용 되는 콘텐츠-유형입니다. 예제 값: <code>application/json</code>합니다. 최대 크기: 없음.|

<a id="ID4EPE"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 음소거 목록에 대 한 성공적인 요청입니다.|
| 400| 잘못된 요청| URI에 지정 된 대상 ID의 형식이 올바르지 않습니다.|
| 403| 사용할 수 없음| URI에 지정 된 소유자를 인증 된 사용자가 아닙니다.|
| 404| 없음| URI에 지정 된 소유자가 없습니다.|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 요청 본문의 MIME 형식입니다. 예제 값: <code>application/json</code>|
| Content-Length| 문자열| 응답에는 전송 중인 바이트 수입니다. 예를 들어 값: 34|
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 서버에서 처리 완료 후 요청입니다. 예: <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>샘플 응답

참조 [UserList](../../json/json-userlist.md)합니다.


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


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)
