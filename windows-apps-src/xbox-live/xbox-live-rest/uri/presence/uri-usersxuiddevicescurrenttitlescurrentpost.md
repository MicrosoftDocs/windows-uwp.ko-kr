---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b29a0bc796712d7b7c44a1fe8512f99bf09eb4fc
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8876482"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
사용자의 현재 상태를 사용 하 여 타이틀을 업데이트 합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [권한 부여](#ID4EPB)
  * [필요한 요청 헤더](#ID4ENE)
  * [선택적 요청 헤더](#ID4ERG)
  * [요청 본문](#ID4ERH)
  * [응답 본문](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
이 URI는 추가 하 고 상태, 다양 한 상태 및 제목에 대 한 미디어 상태 데이터를 업데이트 비 콘솔 플랫폼에서 모든 타이틀에서 사용할 수 있습니다.
 
이제 **SandboxId** 는 XToken 클레임이에서 검색 이며 적용 합니다. **SandboxId** 없으면 엔터테인먼트 검색 서비스 (EDS) 400 잘못 된 요청 오류를 throw 합니다.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID)는 대상 사용자의 합니다.| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>권한 부여
 
| 형식| 필수| 설명| 누락 된 경우 응답| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 예| 호출자의 Xbox 사용자 ID (XUID)| 403 사용할 수 없음| 
| titleId| 예| titleId 제목| 403 사용할 수 없음| 
| deviceId| Windows 및 웹을 제외 하 고 모든 예| 호출자의 deviceId| 403 사용할 수 없음| 
| deviceType| 웹을 제외 하 고 모든 예| 호출자의 deviceType| 403 사용할 수 없음| 
| sandboxId| 제공 하는 호출에 대 한 예입니다. | 호출자의 샌드박스| 403 사용할 수 없음| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > "입니다.| 
| xbl 계약 버전 x| string| 이 요청은 전송 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 서비스의 인증 토큰을 클레임 헤더의 유효성을 확인 한 후에 있습니다. 예제 값: 3, vnext 합니다.| 
| Content-Type| 문자열| 예제 값 요청 본문의 mime 형식: 응용 프로그램/j 합니다.| 
| Content-Length| string| 요청 본문의 길이입니다. 예제 값: 312 합니다.| 
| 호스트| 문자열| 도메인 이름 서버입니다. 예제 값: presencebeta.xboxlive.com 합니다.| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion|  | 이 요청은 전송 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 서비스의 인증 토큰을 클레임 헤더의 유효성을 확인 한 후에 있습니다. 기본값: 1입니다.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>요청 본문
 
[TitleRequest](../../json/json-titlerequest.md)요청입니다. 본문에 실제로 있는 속성만 업데이트 됩니다. 모든 속성은 본문의 일부가 아닌 하지만 존재 하는 서버에서 수정 되지 것입니다.
 
<a id="ID4EAAAC"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
  id:"12341234",
  placement:"snapped",
  state:"active"
}
      
```

   
<a id="ID4EKAAC"></a>

 
## <a name="response-body"></a>응답 본문
 
성공, 200 개 201 Created의 HTTP 상태 코드는 반환 된, 적절 하 게 합니다.
 
(HTTP 4xx 또는 5xx) 오류를 발생 하는 경우 응답 본문에 적절 한 오류 정보를 반환 합니다.
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>자세한 정보 

[마켓플레이스 URI](../marketplace/atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   