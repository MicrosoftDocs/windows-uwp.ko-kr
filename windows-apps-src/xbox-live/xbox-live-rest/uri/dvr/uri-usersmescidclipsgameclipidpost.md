---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89f3b53631f5570ab6d0d0619f6678fc3e3c2dd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645828"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
사용자의 데이터에 대 한 게임 클립 메타 데이터를 업데이트 합니다. 이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EAB)
  * [필요한 요청 헤더](#ID4ELB)
  * [선택적 요청 헤더](#ID4EXD)
  * [요청 본문](#ID4EAF)
  * [필요한 응답 헤더](#ID4EVF)
  * [선택적 응답 헤더](#ID4EJAAC)
  * [응답 본문](#ID4EJBAC)
  * [관련된 Uri](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
게임 클립 메타 데이터를 업데이트 하기 위한 API 두 가지 범주로, 자신의 게임 클립 제목, 접근성 등의 메타 데이터를 업데이트 하 고 (예: 등급을 적용 또는 조회 수 증가) 공용 특성 업데이트의 다른 모든 게임 클립 합니다. URI에서 XUID 클레임의 XUID 일치 하지 않는 경우 공용 데이터를 편집할 수 있으며 다른 데이터를 편집 하려면 모든 요청이 거부 됩니다. 편집 하려는 시도 하는 여러 필드에서는 그 중 하나는 요청에 대해 올바르지 않습니다.을 전체 요청은 실패 합니다.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| 문자열| 서비스 액세스 되는 리소스의 ID를 구성 합니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| 문자열| 액세스 하는 리소스의 ID를 클립 합니다.| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 문자열| 허용 가능한 압축 인코딩입니다. 예제 값: gzip, deflate, identity입니다.| 
| ETag| 문자열| 캐시 최적화를 위해 사용 합니다. 예를 들어 값: "686897696a7c876b7e".| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문 이어야 합니다는 [UpdateMetadataRequest](../../json/json-updatemetadatarequest.md) 개체를 JSON 형식으로 합니다. 예제:
 
사용자 클립 이름 및 표시 유형을 변경 합니다.
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
제목 속성 (이 예 일뿐입니다, 호출자가이 필드의 스키마 이므로)를 변경 합니다.
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Retry-after| 문자열| 서버를 사용할 수 없는 경우 나중에 다시 시도 하도록 클라이언트를 지시 합니다.| 
| 다| 문자열| 다운스트림 프록시 응답을 캐시 하는 방법을 지시 합니다.| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 문자열| 캐시 최적화를 위해 사용 합니다. 예제: "686897696a7c876b7e".| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>응답 본문
 
메타 데이터는 HTTP 상태 코드 200을 성공적으로 업데이트 되 면 반환 됩니다.
 
그렇지 않은 경우 적절 한 HTTP 상태 코드를 사용 하 여 ServiceErrorResponse 개체를 JSON 형식으로 반환 됩니다.
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>관련된 Uri
 
다음 Uri는 메타 데이터에서 public 필드를 업데이트 합니다. 이러한 요청에 필요한 본문이 없습니다. 메타 데이터는 HTTP 상태 코드 200을 성공적으로 업데이트 되 면 반환 됩니다. 그렇지 않은 경우 적절 한 HTTP 상태 코드를 사용 하 여 ServiceErrorResponse 개체를 JSON 형식으로 반환 됩니다.
 
   * **POST /users/ {ownerId} {서비스 안내} /scids/ /clips/ {gameClipId} /ratings/ {등급 값}** -지정한 등급을 지정한 클립에 적용 됩니다. 등급 값 1과 5 사이의 정수 여야 합니다.
   * **{OwnerId} /users/ /scids/ {서비스 안내} /clips/ {gameClipId을 (를) 게시 / 플래그** -를 적용 하 여 검사할 신뢰할 수 없는 자들 콘텐츠를 포함 하도록 클립에 플래그를 지정 합니다.
   * **POST /users/ {ownerId} {서비스 안내} /scids/ /clips/ {gameClipId} / 뷰에** -지정 된 게임 클립의 뷰 횟수를 증가 시킵니다. 이 호출 됩니다 하지 오른쪽 재생 시작 되 면 재생의 75%에서 80% 완료 되 면 것이 좋습니다.
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   