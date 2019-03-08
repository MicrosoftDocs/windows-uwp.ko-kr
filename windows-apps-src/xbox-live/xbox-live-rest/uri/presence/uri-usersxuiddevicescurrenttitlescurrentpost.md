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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649528"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
사용자의 현재 상태를 사용 하 여 제목을 업데이트 합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [권한 부여](#ID4EPB)
  * [필요한 요청 헤더](#ID4ENE)
  * [선택적 요청 헤더](#ID4ERG)
  * [요청 본문](#ID4ERH)
  * [응답 본문](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
이 URI는 추가 하 여 현재 상태, 다양 한 현재 상태 및 제목에 대 한 현재 상태 데이터를 미디어 업데이트 콘솔 내 플랫폼의 모든 제목으로 사용할 수 있습니다.
 
**SandboxId** 는 XToken에 클레임에서 검색 되 고 적용 합니다. 경우는 **SandboxId** 없을 엔터테인먼트 검색 서비스 (EDS) 400 잘못 된 요청 오류가 throw 됩니다.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 대상 사용자입니다.| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>권한 부여
 
| 형식| 필수| 설명| 응답 없는 경우| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 예| 호출자의 Xbox 사용자 ID (XUID)| 403 Forbidden| 
| titleId| 예| 제목의 titleId| 403 Forbidden| 
| deviceId| Windows 및 웹을 제외한 모든 예| 호출자의 deviceId| 403 Forbidden| 
| deviceType| 웹을 제외한 모든 예| 호출자의 deviceType| 403 Forbidden| 
| sandboxId| 들어오는 호출에 대 한 예 | 호출자의 샌드박스| 403 Forbidden| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| x-xbl-contract-version| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 3, vnext입니다.| 
| Content-Type| 문자열| 예를 들어 값 요청 본문의 mime 형식: application/json.| 
| Content-Length| 문자열| 요청 본문의 길이입니다. 예를 들어 값: 312.| 
| 호스트| 문자열| 서버의 도메인 이름입니다. 예제 값: presencebeta.xboxlive.com 합니다.| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 개체를 [TitleRequest](../../json/json-titlerequest.md)합니다. 본문에 실제로 있는 속성만 업데이트 됩니다. 모든 속성은 본문의 일부가 아닌 하지만 서버에 있는지는 수정 되지 것입니다.
 
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
 
성공 하면 HTTP 상태 코드 200 또는 201 Created는 반환 된를 적절 하 게 합니다.
 
오류 발생 시 (HTTP 4xx 또는 5xx), 적절 한 오류 정보가 응답 본문에 반환 됩니다.
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>추가 정보 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   