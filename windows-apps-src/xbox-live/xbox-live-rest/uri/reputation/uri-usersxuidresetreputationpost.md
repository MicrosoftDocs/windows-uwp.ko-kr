---
title: POST (/users/xuid({xuid})/resetreputation)
assetID: 3b76857f-b043-2c76-cf0c-c8f355fe3849
permalink: en-us/docs/xboxlive/rest/uri-usersxuidresetreputationpost.html
author: KevinAsgari
description: " POST (/users/xuid({xuid})/resetreputation)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5b39060c84742f1b37087b17ebc33b021b2c5eb7
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5869858"
---
# <a name="post-usersxuidxuidresetreputation"></a>POST (/users/xuid({xuid})/resetreputation)
(예를 들어)는 계정 하이재킹 후 일부 임의의 값으로 지정 된 사용자의 평판 점수를 설정 하려면 적용 팀을 수 있습니다. 이러한 Uri에 대 한 도메인은 `reputation.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [필요한 요청 헤더](#ID4E5B)
  * [요청 본문](#ID4EYD)
  * [HTTP 상태 코드](#ID4EOE)
  * [응답 본문](#ID4EQH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
이 방법은 일반 정품을 제외 하 고 모든 샌드박스에 파트너가 및 테스트 목적으로 일반 정품을 제외 하 고 모든 샌드박스에서 사용자가 호출할 수 있습니다. Note이 요청을 "기본" 평판 점수, 사용자의 설정 및 그 양수 피드백 weightings는 모두 0으로 설정 하세요. 이러한 기본 점수와 그 특사로 보너스, 자신의 워 보너스가이 호출을 실행 한 후 사용자의 실제 평판이 됩니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)의 사용자 지정된 합니다.| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>권한 부여
 
파트너 로부터:에 대 한 정품 샌드박스를 적용 팀; **PartnerClaim** 모든 다른 샌드박스에, **PartnerClaim**합니다.
 
사용자에서: 소매, **XuidClaim** 및 **TitleClaim**를 제외한 모든 샌드박스에 합니다.
  
<a id="ID4E5B"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
모든: **콘텐츠 형식: 응용 프로그램/j**.
 
파트너 로부터: **Xbl 계약 버전 X** (현재 버전이 101), **X-Xbl-샌드박스**합니다.
 
사용자에서: **Xbl 계약 버전 X** (현재 버전이 101).
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".| 
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 101.| 
  
<a id="ID4EYD"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4E5D"></a>

 
### <a name="sample-request"></a>샘플 요청
 
요청 본문에는 간단한 [ResetReputation (JSON)](../../json/json-resetreputation.md) 문서입니다.
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EOE"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 확인.| 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 404| 찾을 수 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.| 
| 500| 내부 서버 오류| 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 합니다.| 
| 503| 사용할 수 없는 서비스| 요청을 제한, 클라이언트 재시도 값 초 (예: 5 초) 한 후 다시 시도 합니다.| 
  
<a id="ID4EQH"></a>

 
## <a name="response-body"></a>응답 본문
 
성공 시 응답 본문 비어 있습니다. [ServiceError (JSON)](../../json/json-serviceerror.md) 문서 오류가 반환 됩니다.
 
<a id="ID4E3H"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4EHAAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EJAAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

   