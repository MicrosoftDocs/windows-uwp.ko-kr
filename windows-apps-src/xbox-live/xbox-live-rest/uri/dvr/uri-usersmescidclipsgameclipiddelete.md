---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
author: KevinAsgari
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a68d765cfdec81da064b0522ea2ff9a4be12bafb
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4419055"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
이러한 Uri에 대 한 도메인은 삭제 게임 클립 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4ECB)
  * [권한 부여](#ID4ENB)
  * [필요한 요청 헤더](#ID4EYB)
  * [선택적 요청 헤더](#ID4EEE)
  * [요청 본문](#ID4ENF)
  * [HTTP 상태 코드](#ID4EYF)
  * [필수 응답 헤더](#ID4EIAAC)
  * [선택적 응답 헤더](#ID4E2CAC)
  * [응답 본문](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
GameClips 서비스에서 사용자의 비디오를 삭제 하는 메커니즘을 제공 합니다. 삭제 시 모든 메타 데이터 및 실제 비디오 자산 (생성 및 원본) 시스템에서 제거 됩니다. 영구 작업입니다. 

> [!NOTE] 
> 지정 된 소유자 ID 성공 하려면 삭제 요청에 대 한 인증 토큰에 호출자에 게를 일치 해야 합니다. 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | 
| 서비스 안내| string| 액세스 되는 리소스의 서비스 구성 ID입니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| string| GameClip ID 액세스 되는 리소스입니다.| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>권한 부여
 
Xuid 클레임에만이 메서드에 대 한 필요 합니다.
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Encodings 허용 압축 합니다. 예제 값: gzip만 줄이기 identity 합니다.| 
| ETag| string| 최적화 캐시에 사용 됩니다. 예제 값: "686897696a7c876b7e".| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| 확인| 클립의 성공적인 삭제 합니다.| 
| 401| 권한 없음| 요청에서 인증 토큰 형식으로 문제가 있습니다.| 
| 403| 금지| 일부 클레임 누락 된 필요 합니다.| 
| 404| 찾을 수 없음| URL에 지정 된 클립 있었습니다 (또는 두 번 삭제).| 
| 503| 허용할 수 없음| 서비스 또는 일부 다운스트림 종속성 다운 되었습니다. 표준 백오프 동작을 사용 하 여 다시 시도 합니다.| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 다시 시도 후| string| 클라이언트는 사용할 수 없는 서버의 경우 나중에 다시 시도 하도록 지시 합니다.| 
| 다| string| 응답을 캐시 하는 방법을 다운스트림 프록시에 지시 합니다.| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| string| 최적화 캐시에 사용 됩니다. 예: "686897696a7c876b7e"입니다.| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>응답 본문
 
성공 시 (콘텐츠 없음) 204 HTTP 상태 코드를 사용 하 여 서비스에서 응답 합니다. 하려고 동일한 개체를 삭제 하거나 404 존재 하지 않는 개체를 반환 합니다.
 
오류가 발생 하면 **ServiceErrorResponse** 개체가 반환 됩니다.
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>부모 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   