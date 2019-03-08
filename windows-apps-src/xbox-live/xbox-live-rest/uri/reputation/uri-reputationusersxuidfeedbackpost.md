---
title: POST (/users/xuid({xuid})/feedback)
assetID: 48a7a510-a658-f2a3-c6bc-28a3610006e7
permalink: en-us/docs/xboxlive/rest/uri-reputationusersxuidfeedbackpost.html
description: " POST (/users/xuid({xuid})/feedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8a6e118811203fd34c310840e8688c2255c6beb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660048"
---
# <a name="post-usersxuidxuidfeedback"></a>POST (/users/xuid({xuid})/feedback)
셸을 사용 하는 대신 게임, 사용자 의견 옵션을 추가 하려는 경우을 제목에서 사용 합니다. 이러한 Uri에 대 한 도메인은 `reputation.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EZ)
  * [필요한 요청 헤더](#ID4EEB)
  * [요청 본문](#ID4ENC)
  * [필수 헤더](#ID4EDE)
  * [권한 부여](#ID4EXF)
  * [HTTP 상태 코드](#ID4EEG)
  * [응답 본문](#ID4EZH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| ulong| Xbox 사용자 ID (XUID) 보고 되는 사용자입니다.| 
  
<a id="ID4EEB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| X-RequestedServiceVersion|  | 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 101.| 
  
<a id="ID4ENC"></a>

 
## <a name="request-body"></a>요청 본문 
 
<a id="ID4EVC"></a>

 
### <a name="required-members"></a>필요한 멤버 
 
요청의 [피드백](../../json/json-feedback.md) 개체입니다. 
  
<a id="ID4EED"></a>

 
### <a name="prohibited-members"></a>허용 되지 않는 멤버 
 
다른 모든 멤버를 요청에 사용할 수 없습니다.
  
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>샘플 요청 
 

```cpp
{
    "sessionRef":
    {
        "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
        "templateName": "CaptureFlag5",
        "name": "Halo556932",
    },
    "feedbackType": "CommsAbusiveVoice",
    "textReason": "He called me a doodoo-head!",
    "voiceReasonId": null,
    "evidenceId": null
}

      
```

   
<a id="ID4EDE"></a>

 
## <a name="required-headers"></a>필수 헤더
 
다음 헤더는 Xbox Live 서비스 요청을 수행 하는 경우에 필요 합니다.
 
| <b>헤더</b>| <b>값</b>| <b>Deacription</b>| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x=[hash];[token]| STS 인증 토큰입니다. STSTokenString 인증 요청에 의해 반환 된 토큰으로 대체 됩니다.| 
Content-Type| 
application/json| 
전송 되는 데이터의 형식입니다.| 
  
<a id="ID4EXF"></a>

 
## <a name="authorization"></a>권한 부여
 
요청을 올바른 Xbox Live 권한 부여 헤더를 포함 해야 합니다. 호출자가이 리소스에 액세스할 수 없습니다, 하는 경우 서비스는 403 사용 권한 없음 코드를 반환 합니다. 헤더가 잘못 되었거나 누락 된 경우 서비스는 401 허가 되지 않은 코드를 반환 합니다.
  
<a id="ID4EEG"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| 내용 없음| 요청 완료 되었지만 반환 하는 내용을 포함 하지 않습니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과| 요청이 너무 길어서 완료 합니다.| 
| 409| 충돌| 연속 토큰을 더 이상 유효 합니다.| 
  
<a id="ID4EZH"></a>

 
## <a name="response-body"></a>응답 본문 
 
호출에 성공한 경우이 응답에서 개체가 반환 됩니다. 그렇지 않으면 서비스를 반환 된 [ServiceError](../../json/json-serviceerror.md) 개체입니다.
  
<a id="ID4EOAAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

  
<a id="ID4E3AAC"></a>

 
##### <a name="reference"></a>참고자료 

[피드백 (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   