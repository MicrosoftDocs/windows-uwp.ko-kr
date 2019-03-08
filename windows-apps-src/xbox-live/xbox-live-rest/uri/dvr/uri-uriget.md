---
title: GET (/{uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
description: " GET (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 757d84c9ad5a005e042b42d699ada08504dc57ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650618"
---
# <a name="get-uri"></a>GET (/{uri})
게임 클립을 다운로드 합니다. 이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EDB)
  * [필요한 요청 헤더](#ID4EEC)
  * [선택적 요청 헤더](#ID4EQE)
  * [요청 본문](#ID4EZF)
  * [필요한 응답 헤더](#ID4EEG)
  * [HTTP 상태 코드](#ID4EYAAC)
  * [선택적 응답 헤더](#ID4EOFAC)
  * [응답 본문](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
클립 또는 미리 보기에 지정 된 다운로드 가능한 형식의 이며 게시 됨 상태에 도달한 클라이언트 다운로드 수를 **GameClipUri** 개체입니다. 파일을 요청 하는 것에 대 한 URI는 사용자 또는 공용 클립 목록 검색 하는 경우 응답 본문에 포함 됩니다.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| <b>uri</b>| 문자열| 합니다 <b>uri</b> 내에서 필드를 <b>GameClipUri</b> 개체입니다.| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 문자열| 허용 가능한 압축 인코딩입니다. 예제 값: gzip, deflate, identity입니다.| 
| ETag| 문자열| 캐시 최적화를 위해 사용 합니다. 예를 들어 값: "686897696a7c876b7e".| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Retry-after| 문자열| 서버를 사용할 수 없는 경우 나중에 다시 시도 하도록 클라이언트를 지시 합니다.| 
| 다| 문자열| 다운스트림 프록시 응답을 캐시 하는 방법을 지시 합니다.| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 되었습니다.| 
| 307| 임시 리디렉션| 서비스는 다른 URI로 이동 되었습니다.| 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과| 요청이 너무 길어서 완료 합니다.| 
| 410| 완료| 요청된 된 리소스를 더 이상 사용할 수 없습니다.| 
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 문자열| 캐시 최적화를 위해 사용 합니다. 예제: "686897696a7c876b7e".| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EUGAC"></a>

  
 
성공 하면 범위 요청 헤더에 따라 잘릴 수 있습니다 하 고 비디오 클립을 서버에 반환 됩니다. 잘린된 클립을 응답 부분 콘텐츠 (206) 됩니다. 전체 파일을 반환 하는 서버 확인 (200) 응답 합니다. 오류 시를 **GameClipsServiceErrorResponse** 개체는 적절 한 HTTP 상태 코드 (예를 들어, 416, 요청한 범위가 충분 하지 않음)와 함께 반환 될 수 있습니다.
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>Parent 

[/{uri}](uri-uri.md)

   