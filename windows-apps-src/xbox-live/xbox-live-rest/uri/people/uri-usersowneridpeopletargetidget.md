---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/people/{targetid})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a0735a65afe8b5748efefce5dec9ad1989a77b4d
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5160356"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
호출자의 사용자 컬렉션에서 대상 ID로 사용자를 가져옵니다. 이러한 Uri에 대 한 도메인은 `social.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [필요한 요청 헤더](#ID4ERC)
  * [선택적 요청 헤더](#ID4EQD)
  * [요청 본문](#ID4EWE)
  * [HTTP 상태 코드](#ID4EBF)
  * [필수 응답 헤더](#ID4EDH)
  * [응답 본문](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
GET 작업 않으므로 한 번 또는 여러 번 실행 하는 경우 동일한 결과 재현 됩니다이 모든 리소스를 수정 하지 않습니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 식별자입니다. 인증된 된 사용자와 일치 해야 합니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag}) 합니다.| 
| targetid| string| Xbox 사용자 ID (XUID) 또는 게이머 태그 소유자의 사용자 목록에서 해당 데이터를 검색 하는 사용자의 식별자입니다. 예제 값: xuid(2603643534573581), gt(SomeGamertag) 합니다.| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>권한 부여
 
| 형식| 필수| 설명| 누락 된 경우 응답| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 예| 호출자가 사용자의 Xbox 사용자 ID (XUID).| 401 권한이 없음| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열입니다. Xbox LIVE에 대 한 데이터를 권한 부여 합니다. 일반적으로 암호화 된 XSTS 토큰입니다. 예제 값: XBL3.0 <b>x =&lt;userhash >;&lt; 토큰 ></b>합니다.| 
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 기본값: 1.| 
| 수락| 문자열입니다. 호출자는 응답에서 허용 하는 콘텐츠-형식입니다. 모든 응답은 <b>응용 프로그램/j</b>됩니다.| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 되는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 성공 합니다.| 
| 400| 잘못 된 요청| 사용자 Id가 잘못 된 형식의 합니다.| 
| 403| 금지| 권한 부여 헤더에서 XUID 클레임을 분석할 수 없습니다.| 
| 404| 찾을 수 없음| 대상 사용자가 소유자의 사용자 목록에서 찾을 수 없습니다.| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 비트 부호 없는 정수| 길이, 바이트, 응답 본문의입니다. 예제 값: 22.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 이 <b>응용 프로그램/j</b>항상 됩니다.| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>응답 본문
 
호출 되 면 서비스 대상 사용자를 반환 합니다. [Person (JSON)를](../../json/json-person.md)참조 하세요.
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>부모 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   