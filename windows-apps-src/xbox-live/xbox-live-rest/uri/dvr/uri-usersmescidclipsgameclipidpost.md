---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
author: KevinAsgari
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 28c8b9e20e990c51c6b3d7e56e72f4d5d6551b39
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4389390"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
사용자의 데이터에 대 한 게임 클립 메타 데이터를 업데이트 합니다. 이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EAB)
  * [필요한 요청 헤더](#ID4ELB)
  * [선택적 요청 헤더](#ID4EXD)
  * [요청 본문](#ID4EAF)
  * [필수 응답 헤더](#ID4EVF)
  * [선택적 응답 헤더](#ID4EJAAC)
  * [응답 본문](#ID4EJBAC)
  * [관련된 Uri](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
자신의 게임 클립의 접근성, 제목, 같은 메타 데이터를 업데이트 하 고 공용 속성 (예: 등급 적용 또는 보기 횟수가)의 업데이트 API 게임 클립 메타 데이터를 업데이트 하기 위한 두 가지 범주로 나누어집니다 다른 게임 클립의 합니다. URI의 XUID 클레임에서 XUID와 일치 하지 않는 경우 공용 데이터만 편집할 수 있으며 다른 데이터를 편집 하려면 모든 요청이 거부 됩니다. 경우에서 여러 필드를 편집할 수 시도 및 그 중 하나에 유효 하지 요청, 전체 요청에 실패 합니다.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| string| 액세스 되는 리소스의 서비스 구성 ID입니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| string| GameClip ID 액세스 되는 리소스입니다.| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Encodings 허용 압축 합니다. 예제 값: gzip만 줄이기 identity 합니다.| 
| ETag| string| 최적화 캐시에 사용 됩니다. 예제 값: "686897696a7c876b7e".| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문에는 JSON 형식에서 [UpdateMetadataRequest](../../json/json-updatemetadatarequest.md) 개체 여야 합니다. 예제:
 
클립 사용자 이름 및 표시 여부를 변경 합니다.
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
(이 예는 단지, 호출자에 게이 필드의 스키마 이므로)만 제목 속성 변경:
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 다시 시도 후| string| 클라이언트는 사용할 수 없는 서버의 경우 나중에 다시 시도 하도록 지시 합니다.| 
| 다| string| 응답을 캐시 하는 방법을 다운스트림 프록시에 지시 합니다.| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| string| 최적화 캐시에 사용 됩니다. 예: "686897696a7c876b7e"입니다.| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>응답 본문
 
HTTP 상태 코드 200 메타 데이터의 성공적인 업데이트 시 반환 됩니다.
 
그렇지 않은 경우 적절 한 HTTP 상태 코드를 사용 하 여 JSON 형식의 ServiceErrorResponse 개체를 반환 됩니다.
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>관련된 Uri
 
다음 Uri는 메타 데이터에서 공용 필드를 업데이트 합니다. 이러한 요청에 필요한 본문이 없습니다. HTTP 상태 코드 200 메타 데이터의 성공적인 업데이트 시 반환 됩니다. 그렇지 않은 경우 적절 한 HTTP 상태 코드를 사용 하 여 JSON 형식의 ServiceErrorResponse 개체를 반환 됩니다.
 
   * **POST /scids/ {서비스 안내} {gameClipId} /clips/ /ratings/ /users/ {ownerId} {등급 값}** -지정 된 클립에 지정 된 등급 적용 됩니다. 등급 값 1과 5 사이의 정수 있어야 합니다.
   * **/Users/ {ownerId} {서비스 안내} /scids/ /clips/ {gameClipId} 게시 / 플래그** -플래그를 적용 하 여 체크 잠재적으로 의심 스러운 콘텐츠를 포함 하 고 클립입니다.
   * **게시물 /users/ {ownerId} {서비스 안내} /scids/ /clips/ {gameClipId} 보기 /** -지정 된 게임 클립에 대 한 보기 수를 증가 합니다. 이 호출 되도록 하지 오른쪽 재생을 시작 하지만 재생의 75%-80% 완료 되 면 것이 좋습니다.
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>부모 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   