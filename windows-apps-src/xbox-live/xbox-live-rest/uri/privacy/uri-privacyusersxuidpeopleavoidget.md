---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/people/avoid)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 68237ed101870a8fed4b7b5fb298006f784a0910
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5972351"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
사용자에 대 한 문제 방지 목록을 가져옵니다.

  * [설명](#ID4EQ)
  * [URI 매개 변수](#ID4EZ)
  * [권한 부여](#ID4EEB)
  * [필요한 요청 헤더](#ID4EJC)
  * [HTTP 상태 코드](#ID4EYD)
  * [필요한 응답 헤더](#ID4E1F)
  * [응답 본문](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>설명

대상 지정 하는 경우 되기 차단 목록에 그렇지 않으면 빈 경우 되지 않을 경우 해당 사용자를 반환만 합니다.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 필수. 해당 리소스에 액세스 하 고 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code>. 인증된 된 사용자를 이어야 합니다. 예제 값: <code>xuid(2603643534573581)</code>. 최대 크기: 없음. |

<a id="ID4EEB"></a>


## <a name="authorization"></a>권한 부여

사용 권한 부여 클레임 | 클레임| 유형| 필수 여부| 예제 값|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| 64 비트의 부호 있는 정수| 예| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여 | 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <code>Xauth=&lt;authtoken></code>. 최대 크기: 없음.|
| 수락| string| 허용 되는 콘텐츠-형식입니다. 예제 값: <code>application/json</code>. 최대 크기: 없음.|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 잘못 된 요청| URI에 지정 된 대상 ID 올바르지 않습니다.|
| 403| 금지| URI에 지정 된 소유자 인증 된 사용자가 아닙니다.|
| 404| 찾을 수 없습니다.| URI에 지정 된 소유자 존재 하지 않습니다.|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 요청 본문의 MIME 형식입니다. 예제 값: <code>application/json</code>. 최대 크기: 없음.|
| Content-Length| string| 응답에 전송 되는 바이트 수입니다. 예제 값: 34 합니다. 최대 크기: 없음.|
| 캐시 제어| string| 캐시 동작을 지정 하려면 서버에서 정중 요청 합니다. 예제 값: <code>application/json</code>. 최대 크기: 없음.|

<a id="ID4ESH"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EYH"></a>


### <a name="sample-response"></a>예제 응답


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


##### <a name="parent"></a>부모

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)
