---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
description: " POST (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: edd91560ffb5d81b30da4b1453612cc5853a456f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623428"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
사용자가 대상 사용자 집합을 사용 하 여 지정 된 작업을 수행할 수 있는지 여부에 대 한 답변 예 또는 아니요의 집합을 가져옵니다.

  * [설명](#ID4EQ)
  * [URI 매개 변수](#ID4ECB)
  * [권한 부여](#ID4ENB)
  * [필요한 요청 헤더](#ID4ESC)
  * [요청 본문](#ID4E4D)
  * [HTTP 상태 코드](#ID4ETE)
  * [필요한 응답 헤더](#ID4EIG)
  * [응답 본문](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>설명

요청 본문 설정의 목록과 사용자 목록을 걸리며 결과 각 사용자/설정 쌍에 대 한 허용/차단 된 결과입니다.

네트워크를 통한 멀티 플레이 시나리오 (여기서 개인 통신 검사 수행 해야 Xbox 사용자 ID (XUID) 있는 사용자 사이의 하지 않는 네트워크 외부 사용자)를 참조 하십시오 [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md) 사용자 유형에 합니다.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| requestorId| 문자열| 필수. 작업을 수행 하는 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code> 고 <code>me</code>입니다. 로그인 한 사용자 이어야 합니다. 예제 값: <code>xuid(0987654321)</code>합니다.|

<a id="ID4ENB"></a>


## <a name="authorization"></a>권한 부여

권한 부여 클레임 사용 | 클레임| 형식| 필수 여부| 예제 값|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 비트 부호 있는 정수| 예| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예를 들어 값: 1.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4EDE"></a>


### <a name="required-members"></a>필요한 멤버

참조 [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md)합니다.


```cpp
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ViewTargetGameHistory",
        "ViewTargetProfile"
    ]
}

```


<a id="ID4ETE"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 요청이 잘못되었습니다.| 예: 잘못 된 설정 Id, 잘못 된 Uri, 등.|
| 404| URI에 지정 된 사용자가 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 요청 본문의 MIME 형식입니다. 예제 값: <code>application/json</code>|
| Content-Length| 문자열| 응답에는 전송 중인 바이트 수입니다. 예를 들어 값: 34|
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 서버에서 처리 완료 후 요청입니다. 예: <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>응답 본문

참조 [PermissionCheckBatchResponse (JSON)](../../json/json-permissioncheckbatchresponse.md)합니다.

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>샘플 응답


```cpp
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRest", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}

```


<a id="ID4EVAAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EXAAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId 열거형](../../enums/privacy-enum-permissionid.md)
