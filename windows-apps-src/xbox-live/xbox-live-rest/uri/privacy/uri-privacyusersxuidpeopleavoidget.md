---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
description: " GET (/users/{ownerId}/people/avoid)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 745893b4b975b5fbf64fe76591ec15d18af59d73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641618"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
사용자에 대 한 방지 목록을 가져옵니다.

  * [설명](#ID4EQ)
  * [URI 매개 변수](#ID4EZ)
  * [권한 부여](#ID4EEB)
  * [필요한 요청 헤더](#ID4EJC)
  * [HTTP 상태 코드](#ID4EYD)
  * [필요한 응답 헤더](#ID4E1F)
  * [응답 본문](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>설명

대상이 지정 된 경우 경우 블록 목록에 그렇지 않으면 빈 되지 경우 해당 사용자를 반환만 합니다.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| ownerId| 문자열| 필수. 해당 리소스에 액세스 하는 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code>합니다. 인증 된 사용자 여야 합니다. 예제 값: <code>xuid(2603643534573581)</code>합니다. 최대 크기: 없음. |

<a id="ID4EEB"></a>


## <a name="authorization"></a>권한 부여

권한 부여 클레임 사용 | 클레임| 형식| 필수 여부| 예제 값|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 비트 부호 있는 정수| 예| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여 | 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <code>Xauth=&lt;authtoken></code>합니다. 최대 크기: 없음.|
| 수락| 문자열| 허용 되는 콘텐츠-유형입니다. 예제 값: <code>application/json</code>합니다. 최대 크기: 없음.|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 잘못된 요청| URI에 지정 된 대상 ID의 형식이 올바르지 않습니다.|
| 403| 사용할 수 없음| URI에 지정 된 소유자를 인증 된 사용자가 아닙니다.|
| 404| 없음| URI에 지정 된 소유자가 없습니다.|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 요청 본문의 MIME 형식입니다. 예제 값: <code>application/json</code>합니다. 최대 크기: 없음.|
| Content-Length| 문자열| 응답에는 전송 중인 바이트 수입니다. 예를 들어 값: 34. 최대 크기: 없음.|
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 서버에서 처리 완료 후 요청입니다. 예제 값: <code>application/json</code>합니다. 최대 크기: 없음.|

<a id="ID4ESH"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EYH"></a>


### <a name="sample-response"></a>샘플 응답


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EDAAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EFAAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)
