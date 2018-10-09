---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
author: KevinAsgari
description: " POST (/titles/{titleId}/clusters)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 459624ea487c158f3fc92b9c6024b086d49c204e
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4463333"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
Xbox Live 계산 서버 인스턴스를 만드는 클라이언트 수 있는 URI입니다. 이러한 Uri에 대 한 도메인은 `gameserverms.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [필요한 요청 헤더](#ID4EGB)
  * [권한 부여](#ID4ELD)
  * [요청 본문](#ID4EWD)
  * [필수 응답 헤더](#ID4EZE)
  * [응답 본문](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 설명| 
| --- | --- | 
| titleId| 요청에서 작동 해야 하는 타이틀의 ID입니다.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>호스트 이름

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
요청을 만들 때에 다음 표에 표시 된 헤더 필요 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | 
| 사용자 에이전트|  | 사용자 에이전트 요청에 대 한 정보를 제공 합니다.| 
| 콘텐츠 유형| application/json| 전송 되는 데이터의 유형입니다.| 
| 호스트| gameserverms.xboxlive.com|  | 
| Content-Length|  | 요청 개체의 길이입니다.| 
| xbl 계약 버전 x| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]; [토큰]| 인증 토큰입니다.| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>권한 부여
 
요청이 유효한 Xbox Live 권한 부여 헤더를 포함 해야 합니다. 이 리소스에 액세스 하는 호출자에 게 허용 되지 않으면, 서비스 403 응답에 반환 합니다. 헤더 잘못 되었거나 누락 된 경우 서비스 401 권한 없음 응답에 반환 합니다.
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>요청 본문
 
요청에는 다음 멤버가 포함 된 JSON 개체를 포함 해야 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| MPSD 세션 식별자입니다.| 
| abortIfQueued| 선택적 매개 변수 되는 즉시 수행 되지 수 것 경우 리소스에 대 한이 세션을 큐에 대기 하지 GSMS true로 설정에 지시 합니다. 응답 개체는이 값이 true 이기 때문에 요청이 중단 되 면 <code>"fulfillmentState" : "Aborted"</code>. | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
응답에는 다음 표에 표시 된 헤더 항상 포함 됩니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 캐시 제어|  | 요청/응답 체인에 따라 모든 캐싱 메커니즘이 따라야 하는 지시문을 선택 합니다.| 
| 콘텐츠 유형| application/json| 응답에는 데이터의 유형입니다.| 
| Content-Length|  | 응답 본문의 길이입니다.| 
| 콘텐츠 유형 옵션 X|  |  | 
| X XblCorrelationId|  | 응답 본문의 mime 형식입니다.| 
| Date|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>응답 본문
 
호출에 성공 하면 서비스는 다음 멤버가 포함 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| 완료를 폴링 하려면 밀리초에서 간격을 권장 합니다. 참고가 예상 경우 없는 클러스터 준비 됩니다. 하지만 아니라 구독 및 요청 및이 행 속도로 현재 풀 제공 상태가 업데이트에 대 한 호출자 폴링하지 빈도 대 한 권장 합니다.| 
| fulfillmentState| 제공 된 세션 리소스를 즉시 할당 된, 여부 "수행", 향후 리소스의 가용성에 대 한 큐에 추가 "대기" 나타냅니다 또는 중단, "중단"을 요청을 수행 하지 못하기 때문 즉시 요청 "true"로 지정 된 abortIfQueued 합니다. | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>설명
 
다음과 같은 응답 코드를 받는 경우 제목을 다시 호출 서비스에만 해야:
 
   * 408-서버 시간 제한
   * 429-너무 많은 요청
   * 500-서버 오류
   * 502-잘못 된 게이트웨이
   * 503-사용할 수 없는 서비스
   * 504-게이트웨이 시간 제한
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>참고 항목
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  