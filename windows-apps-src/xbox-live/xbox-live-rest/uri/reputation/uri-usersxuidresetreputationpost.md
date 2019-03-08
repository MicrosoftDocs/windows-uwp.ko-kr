---
title: POST (/users/xuid({xuid})/resetreputation)
assetID: 3b76857f-b043-2c76-cf0c-c8f355fe3849
permalink: en-us/docs/xboxlive/rest/uri-usersxuidresetreputationpost.html
description: " POST (/users/xuid({xuid})/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2918249eaf359b383e89f24b8a37352bc3fe5132
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623528"
---
# <a name="post-usersxuidxuidresetreputation"></a>POST (/users/xuid({xuid})/resetreputation)
(예를 들어)는 계정 하이재킹 후 일부 임의 값으로 지정된 된 사용자의 신뢰도 점수를 설정 하려면 적용 팀을 수 있습니다. 이러한 Uri에 대 한 도메인은 `reputation.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [필요한 요청 헤더](#ID4E5B)
  * [요청 본문](#ID4EYD)
  * [HTTP 상태 코드](#ID4EOE)
  * [응답 본문](#ID4EQH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
이 메서드가 소매를 제외한 모든 샌드박스에 대 한 다른 파트너에서 소매를 제외한 모든 샌드박스의 테스트 목적으로 사용자가 호출할 수 있습니다. 이 요청을 사용자의 "기본" 신뢰도 점수를 설정 하 고 자신의 긍정적인 피드백 가중치는 모두 지워진다는 note 합니다. 이러한 기본 점수와 자신의 특사 보너스, 자신의 팔로 워 보너스가이 호출을 수행한 후 사용자의 실제 평판이 됩니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 문자열| Xbox 사용자 ID (XUID) 지정된 된 사용자입니다.| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>권한 부여
 
파트너: 소매 샌드박스에 **PartnerClaim** 적용 팀에서, 다른 모든 샌드박스 **PartnerClaim**합니다.
 
사용자: 소매를 제외한 모든 샌드박스에 대 한 **XuidClaim** 하 고 **TitleClaim**합니다.
  
<a id="ID4E5B"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
모든: **콘텐츠-유형: application/json**합니다.
 
파트너: **X Xbl-계약 버전** (현재 버전은 101) **샌드박스-X-Xbl**합니다.
 
사용자: **X Xbl-계약 버전** (현재 버전이 101).
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| X-RequestedServiceVersion|  | 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 101.| 
  
<a id="ID4EYD"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4E5D"></a>

 
### <a name="sample-request"></a>샘플 요청
 
요청 본문은 단순 [ResetReputation (JSON)](../../json/json-resetreputation.md) 문서.
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EOE"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 확인.| 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 500| 내부 서버 오류| 서버에는 요청을 처리 하지 못하게 하는 예기치 않은 상황이 발생 합니다.| 
| 503| 서비스 이용 불가| 요청이 제한 되었습니다, 그리고 요청 클라이언트 다시 시도 값 (초) (예: 5 초) 후에 다시 시도 하십시오.| 
  
<a id="ID4EQH"></a>

 
## <a name="response-body"></a>응답 본문
 
성공 하면 응답 본문이 비어 있습니다. 실패 한 [ServiceError (JSON)](../../json/json-serviceerror.md) 문서가 반환 됩니다.
 
<a id="ID4E3H"></a>

 
### <a name="sample-response"></a>샘플 응답
 

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

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

   