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
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "8735301"
---
# <a name="get-uri"></a>GET (/{uri})
게임 클립을 다운로드 합니다. 이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
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
 
클라이언트 클립 또는 게시 된 상태에 도달 하 고 **GameClipUri** 개체에 지정 된 대로 다운로드 가능한 형식의 미리 보기를 다운로드할 수 있습니다. 요청 된 파일의 URI 사용자나 공용 클립의 목록을 검색 하는 경우 응답 본문에 포함 됩니다.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| <b>uri</b>| string| <b>GameClipUri</b> 개체에서 <b>uri</b> 필드입니다.| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| string| 이 요청은 전송 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| 허용 가능한 압축 인코딩 합니다. 예제 값: gzip만 줄이기, identity 합니다.| 
| ETag| string| 캐시 최적화에 사용 됩니다. 예제 값: "686897696a7c876b7e"입니다.| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| string| 이 요청은 전송 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 다시 시도 후| string| 사용할 수 없는 서버의 경우 나중에 다시 시도 하는 클라이언트에 지시 합니다.| 
| 다| string| 응답을 캐시 하는 방법을 다운스트림 프록시에 지시 합니다.| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 합니다.| 
| 307| 임시 리디렉션| 서비스는 다른 URI로 이동 합니다.| 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음| 리소스 버전은 지원 되지 않습니다.| 
| 408| 요청 시간 제한| 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 410| 최신 상태가 아닌| 요청 된 리소스는 더 이상 사용할 수 없습니다.| 
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| string| 캐시 최적화에 사용 됩니다. 예: "686897696a7c876b7e"입니다.| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EUGAC"></a>

  
 
성공 하면 서버는 범위 요청 헤더에 따라 문자열이 비디오 클립을 반환 합니다. 잘린된 클립에 대 한 응답 일부 콘텐츠 (206) 됩니다. 서버에서 반환 하는 전체 파일 확인 (200)에서 응답 합니다. 오류 발생 시 **GameClipsServiceErrorResponse** 개체를 적절 한 HTTP 상태 코드 (예: 416, 요청 된 범위가 충분 하지 않음)와 함께 반환 될 수 있습니다.
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>부모 

[/{uri}](uri-uri.md)

   