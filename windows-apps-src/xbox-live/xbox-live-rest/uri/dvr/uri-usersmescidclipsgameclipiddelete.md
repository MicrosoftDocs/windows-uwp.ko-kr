---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 391e4d79a389c358dea83509b52782d086201ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622838"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
이러한 Uri에 대 한 도메인은 삭제 게임 클립 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4ECB)
  * [권한 부여](#ID4ENB)
  * [필요한 요청 헤더](#ID4EYB)
  * [선택적 요청 헤더](#ID4EEE)
  * [요청 본문](#ID4ENF)
  * [HTTP 상태 코드](#ID4EYF)
  * [필요한 응답 헤더](#ID4EIAAC)
  * [선택적 응답 헤더](#ID4E2CAC)
  * [응답 본문](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
GameClips 서비스에서 사용자의 비디오를 삭제 하는 메커니즘을 제공 합니다. 삭제 시 모든 메타 데이터 및 실제 비디오 자산 (생성 및 원본) 시스템에서 제거 됩니다. 영구 작업입니다. 

> [!NOTE] 
> 지정 된 소유자 ID delete 요청이 성공할 수에 대 한 권한 부여 토큰에 호출자에 게를 일치 해야 합니다. 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | 
| scid| 문자열| 서비스 액세스 되는 리소스의 ID를 구성 합니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| 문자열| 액세스 하는 리소스의 ID를 클립 합니다.| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>권한 부여
 
Xuid 클레임만이 메서드에 대 한 필요합니다.
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 문자열| 허용 가능한 압축 인코딩입니다. 예제 값: gzip, deflate, identity입니다.| 
| ETag| 문자열| 캐시 최적화를 위해 사용 합니다. 예를 들어 값: "686897696a7c876b7e".| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| 확인| 클립의 성공적으로 삭제 합니다.| 
| 401| 권한 없음| 요청에 인증 토큰 형식 사용 하 여 문제가 있습니다.| 
| 403| 사용할 수 없음| 일부 필수 클레임이 누락 됨.| 
| 404| 없음| URL에 지정 된 클립 제공 되지 않았습니다. (또는 두 번 삭제).| 
| 503| 허용 되지 않음| 서비스 또는 일부 다운스트림 종속성 중지 되었습니다. 표준 백오프 동작을 사용 하 여 다시 시도 하세요.| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Retry-after| 문자열| 서버를 사용할 수 없는 경우 나중에 다시 시도 하도록 클라이언트를 지시 합니다.| 
| 다| 문자열| 다운스트림 프록시 응답을 캐시 하는 방법을 지시 합니다.| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 문자열| 캐시 최적화를 위해 사용 합니다. 예제: "686897696a7c876b7e".| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>응답 본문
 
서비스는 성공 하면 204 (내용 없음) HTTP 상태 코드로 응답 합니다. 하는 동안 동일한 개체를 삭제 또는 존재 하지 않는 개체를 404를 반환 합니다.
 
오류의 경우는 **ServiceErrorResponse** 개체가 반환 됩니다.
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   