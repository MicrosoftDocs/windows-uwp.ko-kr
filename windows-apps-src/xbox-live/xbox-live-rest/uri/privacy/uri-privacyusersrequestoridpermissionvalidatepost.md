---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
author: KevinAsgari
description: " POST (/users/{requestorId}/permission/validate)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0848aaa74fcecec599c701d944c54defae1fa011
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5707268"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
대상 사용자의 집합을 사용 하 여 지정 된 작업을 수행 하는 사용자가 허용 되는지 여부에 대 한 예 또는 아니요 답변 집합을 가져옵니다.

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

요청 본문에는 사용자 목록 및 설정의 목록 및 결과 각 사용자/설정을 쌍에 대 한 허용/차단 결과입니다.

네트워크를 통한 멀티 플레이어 시나리오 (여기서 개인 정보 보호 통신 검사를 수행 해야 합니다 Xbox 사용자 ID (XUID)이 있는 사용자와 하지 않는 네트워크 외부 사용자 간의)에서는 사용자가 입력할 [PermissionCheckBatchRequest (JSON)을](../../json/json-permissioncheckbatchrequest.md) 참조 하십시오.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| requestorId| string| 필수. 작업을 수행 하는 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code> 및 <code>me</code>. 로그인 한 사용자 여야 합니다. 예제 값: <code>xuid(0987654321)</code>.|

<a id="ID4ENB"></a>


## <a name="authorization"></a>권한 부여

사용 권한 부여 클레임 | 클레임| 유형| 필수 여부| 예제 값|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| 64 비트의 부호 있는 정수| 예| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트됩니다 됩니다. 예제 값: 1입니다.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4EDE"></a>


### <a name="required-members"></a>필수 멤버

[PermissionCheckBatchRequest (JSON)를](../../json/json-permissioncheckbatchrequest.md)참조 하세요.


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

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 400| 요청이 잘못되었습니다.| 예: 잘못 된 설정 Id, 잘못 된 Uri, 등.|
| 404| URI에 지정 된 사용자가 존재 하지 않습니다.| 지정된 된 리소스를 찾을 수 없습니다.|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 문자열| 요청 본문의 MIME 형식입니다. 예제 값: <code>application/json</code>|
| Content-Length| string| 응답에 전송 되는 바이트 수입니다. 예제 값: 34|
| 캐시 제어| string| 캐시 동작을 지정 하려면 서버에서 정중 요청 합니다. 예제: <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>응답 본문

[PermissionCheckBatchResponse (JSON)를](../../json/json-permissioncheckbatchresponse.md)참조 하세요.

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>예제 응답


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


##### <a name="parent"></a>부모

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId 열거형](../../enums/privacy-enum-permissionid.md)
