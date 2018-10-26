---
title: POST (/users/xuid({xuid})/feedback)
assetID: 48a7a510-a658-f2a3-c6bc-28a3610006e7
permalink: en-us/docs/xboxlive/rest/uri-reputationusersxuidfeedbackpost.html
author: KevinAsgari
description: " POST (/users/xuid({xuid})/feedback)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7df5311a50c2192404fa83d39cbb57e7f5f239f2
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5565152"
---
# <a name="post-usersxuidxuidfeedback"></a>POST (/users/xuid({xuid})/feedback)
셸을 사용 하지 않고 게임에서 피드백 옵션을 추가 하려면 원하는 경우 타이틀에서 사용 합니다. 이러한 Uri에 대 한 도메인은 `reputation.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EZ)
  * [필요한 요청 헤더](#ID4EEB)
  * [요청 본문](#ID4ENC)
  * [필수 헤더](#ID4EDE)
  * [권한 부여](#ID4EXF)
  * [HTTP 상태 코드](#ID4EEG)
  * [응답 본문](#ID4EZH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| ulong| Xbox 사용자 ID (XUID) 보고 중인 사용자입니다.| 
  
<a id="ID4EEB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".| 
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 101.| 
  
<a id="ID4ENC"></a>

 
## <a name="request-body"></a>요청 본문 
 
<a id="ID4EVC"></a>

 
### <a name="required-members"></a>필수 멤버 
 
요청은 [피드백](../../json/json-feedback.md) 개체를 포함 해야 합니다. 
  
<a id="ID4EED"></a>

 
### <a name="prohibited-members"></a>금지 된 멤버 
 
다른 모든 구성원 요청에 사용할 수 없습니다.
  
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
 
다음 헤더는 Xbox Live 서비스 요청을 만들 때 필요 합니다.
 
| <b>헤더</b>| <b>값</b>| <b>Deacription</b>| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xbl 계약 버전 x| 101| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]. [토큰]| STS 인증 토큰입니다. STSTokenString 인증 요청으로 반환 하는 토큰으로 바뀝니다.| 
콘텐츠 유형| 
application/json| 
전송 되는 데이터 형식입니다.| 
  
<a id="ID4EXF"></a>

 
## <a name="authorization"></a>권한 부여
 
요청이 유효한 Xbox Live 권한 부여 헤더를 포함 해야 합니다. 이 리소스에 액세스 하는 호출자에 게 허용 되지 않으면, 서비스 403 사용할 수 없음 코드를 반환 합니다. 헤더 잘못 되었거나 누락 된 경우 서비스 401 승인 되지 않은 코드를 반환 합니다.
  
<a id="ID4EEG"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| 콘텐츠| 요청 완료 되 면 있지만 콘텐츠를 반환할 수 없습니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 404| 찾을 수 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 제한| 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 409| 충돌| 연속 토큰을 더 이상 유효 합니다.| 
  
<a id="ID4EZH"></a>

 
## <a name="response-body"></a>응답 본문 
 
호출 되 면이 응답의 개체가 반환 됩니다. 그렇지 않은 경우 서비스가 [ServiceError](../../json/json-serviceerror.md) 개체를 반환합니다.
  
<a id="ID4EOAAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQAAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

  
<a id="ID4E3AAC"></a>

 
##### <a name="reference"></a>참조 

[Feedback(JSON)](../../json/json-feedback.md)

 [ServiceError(JSON)](../../json/json-serviceerror.md)

   