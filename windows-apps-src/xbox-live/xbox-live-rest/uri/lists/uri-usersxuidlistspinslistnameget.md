---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
author: KevinAsgari
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe026ec7c63a6f255cfc60c81712a3f108499a6c
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6022697"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
목록 내용을 반환합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EIB)
  * [쿼리 문자열 매개 변수](#ID4ETB)
  * [권한 부여](#ID4ESD)
  * [필요한 요청 헤더](#ID4E6D)
  * [요청 본문](#ID4EVF)
  * [HTTP 상태 코드](#ID4EAG)
  * [응답 본문](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
반환 되는 데이터의 **listCount** 필드 같이-서비스에서 유지 총 목록에 얼마나 많은 항목이, 목록의 끝 인 및이 얼마나 많은 특정 항목이 retu 된 다른 숫자는 잠재적으로 위치를 확인 하기 위해 사용할 수 있습니다. 요청으로 rned 합니다.
 
목록 아직 존재 하지 않는 경우 다음 결과 목록 항목이 포함 됩니다, **listCount** 0이 됩니다 및 **listVersion** 0이 됩니다.
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)입니다.| 
| listtype| string| 유형 (사용 하는 방법 및 작동 방식을) 목록입니다. 항상 이러한 "핀" 관련 메서드.| 
| listname| string| 목록 이름 (의 지정 된 listtype 어떤 목록). 항상 "XBLPins"에 대 한 Pin 항목입니다.| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| 32 비트 부호 있는 정수| 선택 사항입니다. 결과 반환 하기 전에 열거에서 건너뛸 항목 수입니다. 기본값: 0입니다.| 
| maxItems| 32 비트 부호 있는 정수| 선택 사항입니다. 최대 반환할 항목 수입니다. 기본 25 개 항목 이면 무제한 요청에 지정 됩니다. 서비스에이 값은 최대 두지 않습니다. 값 목록에서 항목 개수 보다 큰 경우 모든 항목 오류 없이 반환 됩니다.| 
| filterItemId| string| 선택 사항입니다. 항목 목록에서 찾을 수를 지정 합니다. 목록에서 항목의 모든 인스턴스를 반환합니다. 신속 하 게 확인 하 고 목록의 항목은 클라이언트 수 있습니다. 큰 목록 전체 목록을 반복 하지 않고 항목의 인스턴스를 모두 확인 하는 데 편리 합니다. 기본값: null입니다.| 
| filterContentType| string| 선택 사항입니다. 쉼표로 구분 된 목록을 반환 하는 콘텐츠 형식 지정 (목록에 없는 종류를 반환 합니다). 목록에서 특정 콘텐츠 형식에만 다운로드할 하는 데 사용 합니다. 이 필터에 대 한 콘텐츠 형식 쉼표로 구분 된 목록을 사용 됩니다. (한 번 호출에 여러 콘텐츠 형식의 쿼리할 수 있습니다.) 지원 되는 콘텐츠 유형은 엔터테인먼트 검색 서비스 (EDS)으로 정의 된 모든 미디어 형식이 포함 합니다. 기본값: null (모든 콘텐츠 형식).| 
| filterDeviceType| string| 선택 사항입니다. 반환할 장치 종류의 쉼표로 구분 된 목록을 지정 합니다 (목록에 없는 종류를 반환 합니다). 만 특정 장치 유형 중에서 삽입 된 항목을 반환 결과 집합을 필터링 합니다. 장치 종류의 쉼표로 구분 된 목록 (여러 디바이스 유형에 한 번 호출에서 쿼리할 수 있습니다)이이 필터에 사용 됩니다. 가능한 값: XboxOne, MCapensis, WindowsPhone, WindowsPhone7, 웹, PC, MoLive 합니다. 기본값: null (모든 콘텐츠 형식).| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>권한 부여
 
이 호출은 **권한 부여** 헤더에서 XSTS SAML 토큰이 필요합니다. Xuid 클레임 호출자를 식별 하는 SAML 토큰 내에 있어야 합니다. 이 값은 호출자에 해당 목록 데이터에 대 한 액세스 권한을 있는지 확인 하려면 사용 합니다. 목록 자체 것도으로 Xuid로 식별 하 고 목록에 대 한 URI에 포함 됩니다. 이 사용 하 여, 앞에 목록에 대 한 지원 공유 액세스 될 하지만 하는 기능이 아니며이 시간에. 현재 사용자가 액세스 하는 모든 목록 자신의 공유 액세스할 수 없습니다. 따라서 URI의 Xuid SAML 클레임 토큰에 Xuid 일치 해야 합니다. 

> [!NOTE] 
> 현재 XBL 인증 2.0 및 3.0 토큰을 모두 지원 됩니다. 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 인증 요청을 인증 하는 데 사용 STS 토큰이 포함 되어 있습니다. XSTS 서비스에서 토큰 이어야 하 고는 XUID 클레임 중 하나로 포함 해야 합니다. | 
| XBL 계약 버전 X| 요청 된 (양의 정수) 되 고 API 버전을 지정 합니다. 핀 지원 버전 2입니다. 이 헤더 없거나 서비스는 400-반환 하는 다음 값 지원 되지 않습니다 "지원 되지 않는 없거나 계약 버전 헤더" 상태 설명에 잘못 된 요청 합니다.| 
| Content-Type| 요청/응답 본문의 콘텐츠 json 또는 xml 수 있는지 여부를 지정 합니다. 지원 되는 값은 "application/json" 및 "application/xml"| 
| If-Match| 이 헤더 수정 요청을 만들 때 목록이의 현재 버전 번호를 포함 해야 합니다. 요청 사용 PUT, POST, 수정 하거나 동사를 삭제할. "일치 하면" 헤더에 있는 버전에는 현재 버전의 목록 일치 하지 않으면, HTTP 412 요청이 거부 됩니다 – 전제 조건 반환 코드에 실패 했습니다. (선택 사항) 또한 경우 사용할 수 있습니다을 가져온에 대 한 현재 목록 버전 다음 목록 데이터 없음 및 HTTP 304와 일치 있고 전달된 버전 – 수정 되지 않음 코드를 반환 됩니다. | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EAG"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 요청이 성공적으로 완료 됩니다. 응답 본문 (GET)에 대 한 요청 된 리소스를 포함 해야 합니다. POST 및 PUT 요청 (예: 목록 버전, 개수) 최신 목록을 메타 데이터를 받게 됩니다.| 
| 201| 생성| 새 목록을 만들었습니다. 이 목록에 초기 삽입에 반환 됩니다. 응답 목록에 최신 메타 데이터를 포함 하 고 위치 헤더 목록에 대 한 URI를 포함 합니다.| 
| 304| 수정 되지 않음| 반환 된 가져옵니다. 클라이언트에 최신 버전의 목록의 있음을 나타냅니다. 서비스 목록 버전 <b>일치 경우</b> 헤더에 있는 값을 비교합니다. 같으면는 304 데이터가 없는 상태로 반환 가져옵니다.| 
| 400| 잘못 된 요청| 요청이 잘못 되었습니다. 잘못 된 값 또는 URI 또는 쿼리 문자열 매개 변수의 형식 수 있습니다. 필요한 매개 변수를 누락 결과 수 또는 데이터 값 또는 요청 누락 되거나 잘못 된 버전의 API 표시 합니다. <b>XBL 계약 버전 X</b> 헤더를 참조 하세요.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다.| 호출자는 리소스에 액세스할 수 없습니다. 이 이러한 목록이 이미 만들었다고 나타냅니다.| 
| 412| 전제 조건 실패| 버전의 목록 변경 하 고 수정 요청이 실패 했습니다 나타냅니다. 요청은 수정 삽입, 업데이트 또는 제거 됩니다. 서비스 목록 버전에 대 한 <b>경우 일치</b> 헤더를 확인합니다. 현재 버전의 목록 일치 하지 않으면, 다음 작업이 실패 하 고 (현재 버전 포함)는 현재 목록 메타 데이터와 함께 반환 됩니다.| 
| 500| 내부 서버 오류| 서비스 요청 서버 쪽 오류로 인해 거부 됩니다.| 
| 501| 구현 되지 않음| 호출자 요청 URI를 서버에 구현 되지 않았습니다. (현재 때 요청을 사용 하는 전용 대상인 허용 목록에 포함 되지 않은 목록 이름.)| 
| 503| 사용할 수 없는 서비스| 서버 과도 한 로드도 인해 일반적으로 요청을 거부 됩니다. 된 지연 시간 후 (참조를 <b>다시 시도 후</b> 헤더), 요청을 다시 시도할 수 있습니다.| 
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "http://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>자세한 정보 

[마켓플레이스 URI](../marketplace/atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   