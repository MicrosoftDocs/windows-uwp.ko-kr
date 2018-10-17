---
title: POST (/users/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
author: KevinAsgari
description: " POST (/users/batchfeedback)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d62e4f7106f7f0f2c324ca2c68ea8fe476bc7bfb
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4750228"
---
# <a name="post-usersbatchfeedback"></a>POST (/users/batchfeedback)
타이틀의 인터페이스 외부에서 일괄 처리 형태로 피드백을 보내려면 타이틀의 서비스가 사용 합니다. 이러한 Uri에 대 한 도메인은 `reputation.xboxlive.com`.
 
  * [요청 본문](#ID4EX)
  * [필수 헤더](#ID4E3E)
  * [HTTP 상태 코드](#ID4EWG)
  * [응답 본문](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>요청 본문 
 
호출자가 웹 요청 개체의 ClientCertificates 섹션에서 해당 클레임 인증서를 포함 해야 합니다.
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>필수 구성원 
 
요청은 **BatchFeedback** 개체의 배열에 포함 되어야 합니다. 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>금지 된 멤버 
 
다른 모든 멤버는 요청에 사용할 수 없습니다.
  
<a id="ID4E3B"></a>

 
### <a name="sample-request"></a>샘플 요청 
 

```cpp
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Killed 19 team members in a single session",
            "evidenceId": null
        },
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayQuitter",
            "textReason": "Quit 6 times from 9 sessions",
            "evidenceId": null
        }
    ]
}

      
```

 
| <b>필드</b>| <b>JSON 형식</b>| <b>설명</b>| 
| --- | --- | --- | 
| 항목| 배열| 컬렉션 피드백 JSON 문서입니다.| 
| targetXuid| string| 대상 사용자의 XUID| 
| titleId| string| 이 피드백에서 보낸 제목 또는 NULL입니다.| 
| sessionRef| 개체| MPSD 세션을 설명 하는 개체 또는 NULL이 피드백와 관련이 있습니다.| 
| feedbackType| string| 문자열 버전 FeedbackType 열거형의 값입니다.| 
| textReason| string| 발신자가 제출한 피드백에 대 한 자세한 내용을 제공에 추가할 수 있는 파트너 제공한 텍스트입니다.| 
| evidenceId| string| 제출한 피드백의 증거로 사용할 수 있는 리소스의 ID입니다. 예: 비디오 파일의 ID입니다.| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>필수 헤더
 
다음 헤더는 Xbox Live 서비스 요청을 만들 때 필요 합니다. 

> [!NOTE] 
> 일괄 처리 피드백을 제출 하려면 요청을 사용 하 여 파트너 클레임 인증서 보내야 합니다. 


 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| x xbl-계약 버전| 101| API 계약 버전입니다.| 
| 콘텐츠 유형| application/json| 전송 되는 데이터의 형식입니다.| 
| 권한 부여| "XBL3.0 x =&lt;userhash >; &lt;토큰 > "| HTTP 인증에 대 한 자격 증명을 인증 합니다.| 
| X RequestedServiceVersion| 101| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 서비스의 인증 토큰을 클레임 헤더의 유효성을 확인 한 후에 있습니다.| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 되는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청은 사용자 인증이 필요합니다.| 
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 500| 내부 서버 오류| 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 했습니다.| 
| 503| 사용할 수 없는 서비스| 요청을 제한, 초 (예: 5 초) 클라이언트를 다시 시도 값 후 다시 시도 합니다.| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
호출 되 면이 응답의 개체가 반환 됩니다. 그렇지 않은 경우 서비스가 [ServiceError](../../json/json-serviceerror.md) 개체를 반환합니다.
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>부모 

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>참조 

[Feedback(JSON)](../../json/json-feedback.md)

 [ServiceError(JSON)](../../json/json-serviceerror.md)

   