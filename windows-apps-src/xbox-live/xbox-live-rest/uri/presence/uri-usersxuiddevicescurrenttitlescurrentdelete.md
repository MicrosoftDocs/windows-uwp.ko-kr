---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
author: KevinAsgari
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 512cb5d65279a461937d91929284b2eb1921ec00
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5399523"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
[PresenceRecord](../../json/json-presencerecord.md) 만료 될 때까지 기다리지 않고 닫는 타이틀의 존재 여부를 제거 합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EZ)
  * [권한 부여](#ID4EEB)
  * [필요한 요청 헤더](#ID4ERD)
  * [선택적 요청 헤더](#ID4EVF)
  * [요청 본문](#ID4EVG)
  * [응답 본문](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID)는 대상 사용자의 합니다.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>권한 부여
 
| 형식| 필수| 설명| 누락 된 경우 응답| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 예| 호출자의 Xbox 사용자 ID (XUID)| 403 사용할 수 없음| 
| titleId| 예| titleId 제목| 403 사용할 수 없음| 
| deviceId| Windows 및 웹을 제외 하 고 모든 예| 호출자의 deviceId| 403 사용할 수 없음| 
| deviceType| 웹 제외한 모든 컴퓨터에 대 한 예입니다.| 호출자의 deviceType| 403 사용할 수 없음| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".| 
| xbl 계약 버전 x| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 3, vnext 합니다.| 
| Content-Type| 문자열| 예제 값 요청 본문의 mime 형식: 응용 프로그램/j 합니다.| 
| Content-Length| string| 요청 본문의 길이입니다. 예제 값: 312 합니다.| 
| 호스트| 문자열| 도메인 이름 서버입니다. 예제 값: presencebeta.xboxlive.com 합니다.| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1입니다.| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>응답 본문
 
성공 시 HTTP 상태 코드 응답 본문이 반환 됩니다.
 
(HTTP 4xx 또는 5xx) 오류를 발생 하는 경우 응답 본문에 적절 한 오류 정보를 반환 합니다.
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   