---
title: GET (/users/{requestorId}/permission/validate)
assetID: 8d22c668-af9a-1d24-8d65-830c2ce913d7
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidateget.html
description: " GET (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 07ccac63b3e6681ea35405314b0cd8b93aa60f9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594758"
---
# <a name="get-usersrequestoridpermissionvalidate"></a>GET (/users/{requestorId}/permission/validate)
사용자가 대상 사용자를 사용 하 여 지정 된 작업을 수행할 수 있는지 여부에 대 한 예 또는 아니요 응답을 가져옵니다.

  * [URI 매개 변수](#ID4EQ)
  * [쿼리 문자열 매개 변수](#ID4E2)
  * [권한 부여](#ID4EDC)
  * [필요한 요청 헤더](#ID4EID)
  * [요청 본문](#ID4ETE)
  * [HTTP 상태 코드](#ID4E5E)
  * [필요한 응답 헤더](#ID4ETG)
  * [응답 본문](#ID4EKAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| requestorId| 문자열| 필수. 작업을 수행 하는 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code> 고 <code>me</code>입니다. 로그인 한 사용자 이어야 합니다. 예제 값: <code>xuid(0987654321)</code>합니다.|

<a id="ID4E2"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- | --- |
| 설정| 문자열 열거형| 검사할 PermissionId 값입니다. 예를 들어 값: "CommunicateUsingText".|
| target| 문자열| 부여한에서 동작이 수행 되는 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code>합니다. 예제 값: <code>xuid(0987654321)</code>|

<a id="ID4EDC"></a>


## <a name="authorization"></a>권한 부여

권한 부여 클레임 사용 | 클레임| 형식| 필수 여부| 예제 값|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 비트 부호 있는 정수| 예| 1234567890|

<a id="ID4EID"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예를 들어 값: 1.|

<a id="ID4ETE"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 요청이 잘못되었습니다.| 예: 잘못 된 설정 Id, 잘못 된 Uri, 등.|
| 404| URI에 지정 된 사용자가 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.|

<a id="ID4ETG"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 요청 본문의 MIME 형식입니다. 예제 값: <code>application/json</code>|
| Content-Length| 문자열| 응답에는 전송 중인 바이트 수입니다. 예를 들어 값: 34|
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 서버에서 처리 완료 후 요청입니다. 예: <code>no-cache, no-store</code>|

<a id="ID4EKAAC"></a>


## <a name="response-body"></a>응답 본문

참조 [PermissionCheckResponse (JSON)](../../json/json-permissioncheckresponse.md)합니다.

<a id="ID4EWAAC"></a>


### <a name="sample-response"></a>샘플 응답


```cpp
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}

```


<a id="ID4EABAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ECBAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId 열거형](../../enums/privacy-enum-permissionid.md)
