---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
description: " POST (/titles/{titleId}/clusters)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91d7c49628914f887c5d2243942e10e47d47b095
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608908"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
URI를 사용 하면 Xbox Live 계산 서버 인스턴스를 만들려면 클라이언트입니다. 이러한 Uri에 대 한 도메인은 `gameserverms.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EX)
  * [필요한 요청 헤더](#ID4EGB)
  * [권한 부여](#ID4ELD)
  * [요청 본문](#ID4EWD)
  * [필요한 응답 헤더](#ID4EZE)
  * [응답 본문](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 설명| 
| --- | --- | 
| titleId| ID는 요청에서 작동 해야 하는 제목입니다.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>호스트 이름

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
에 요청을 수행할 때 다음 표에 나와 있는 헤더는 필요 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | 
| 사용자 에이전트|  | 요청을 만드는 사용자 에이전트에 대 한 정보입니다.| 
| Content-Type| application/json| 전송 되는 데이터의 형식입니다.| 
| 호스트| gameserverms.xboxlive.com|  | 
| Content-Length|  | 요청 개체의 길이입니다.| 
| x-xbl-contract-version| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x=[hash];[token]| 인증 토큰입니다.| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>권한 부여
 
요청을 올바른 Xbox Live 권한 부여 헤더를 포함 해야 합니다. 호출자에 게가이 리소스에 액세스할 수 없습니다, 하는 경우 서비스 403 사용할 수 없음 응답에서 반환 합니다. 헤더가 잘못 되었거나 누락 된 경우 서비스 401 권한 없음 응답에 반환 합니다.
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>요청 본문
 
요청에는 다음 멤버로 구성 된 JSON 개체를 포함 해야 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| MPSD에서 세션 식별자입니다.| 
| abortIfQueued| 선택적 매개 변수는 경우 true로 설정 GSMS 즉시 수행 되지 있습니다이 경우 리소스에 대 한이 세션을 큐에 대기 하지 지시 합니다. 응답 개체를 포함 요청에는이 값은 true 이므로 중단 되 면 <code>"fulfillmentState" : "Aborted"</code>합니다. | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
응답에는 항상 다음 표에 나와 있는 헤더 포함 됩니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Cache-Control|  | 요청/응답 체인을 따라 모든 캐싱 메커니즘이 따라야 하는 지시문입니다.| 
| Content-Type| application/json| 응답에는 데이터 형식입니다.| 
| Content-Length|  | 응답 본문의 길이입니다.| 
| X-Content-Type-Options|  |  | 
| X-XblCorrelationId|  | 응답 본문의 mime 형식입니다.| 
| 날짜|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>응답 본문
 
호출이 성공 하면 서비스는 다음 멤버로 구성 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| 완료를 폴링할 밀리초에서 간격을 것이 좋습니다. 이것이 예상 클러스터 수, 하는 경우는 하지만 대신 호출자에 게 구독을 요청 및 처리 속도가 현재 풀에 지정 된 상태 업데이트를 폴링하는 빈도 대 한 권장 사항입니다.| 
| fulfillmentState| 제공 된 세션 리소스를 즉시 할당 된에 지 여부를 "처리" 향후 리소스의 가용성을 위해 큐에 추가 "대기 됨", 나타냅니다 또는 중단, "중단" 요청을 처리 하지 못하는 인해 즉시 요청 "true"로 지정 된 abortIfQueued 합니다. | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>샘플 응답
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>설명
 
제목을 다음 응답 코드를 받을 때 서비스에 대 한 호출 다시 시도 해야 합니다.
 
   * 408-서버 시간 제한
   * 429-너무 많은 요청
   * 500-서버 오류
   * 502-잘못 된 게이트웨이
   * 503-서비스를 사용할 수 없음
   * 504-게이트웨이 시간 초과
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>참고 항목
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  