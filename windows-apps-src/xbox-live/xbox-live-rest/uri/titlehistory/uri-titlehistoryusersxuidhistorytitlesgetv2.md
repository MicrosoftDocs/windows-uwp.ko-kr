---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
description: " GET (/users/xuid({xuid})/history/titles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cadbf38385bbc321ef5bf23eb93c3fbc5c1a2417
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655588"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
사용자가 잠금 해제 하거나 해당 성과에서 진전을 보였습니다 타이틀의 목록을 가져옵니다. 이 API는 사용자의 전체 기록을 재생 하거나 시작 제목의 반환 하지 않습니다. 이러한 Uri에 대 한 도메인은 `achievements.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EY)
  * [쿼리 문자열 매개 변수](#ID4EDB)
  * [권한 부여](#ID4EFD)
  * [선택적 요청 헤더](#ID4EGE)
  * [요청 본문](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 제목 기록을 액세스 하는 사용자입니다.| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 필수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| 아니오| 32 비트 부호 있는 정수| 항목 수가 지정 된 후 시작 하는 항목을 반환 합니다. 예를 들어 <b>skipItems = "3"</b> 항목을 검색 하는 네 번째 항목 부터는 검색 합니다. | 
| continuationToken| 아니오| 문자열| 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. | 
| maxItems| 아니오| 32 비트 부호 있는 정수| 과 결합 될 수 있는 컬렉션에서 반환할 항목의 최대 수 <b>skipItems</b> 하 고 <b>continuationToken</b> 항목의 범위를 반환할 수 있습니다. 서비스 하는 경우 기본 값을 제공할 수 있습니다 <b>maxItems</b> 없 및 보다 더 적은 수를 반환할 수 있습니다 <b>maxItems</b>결과의 마지막 페이지 아직 반환 되지 않은 경우에 합니다. | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>권한 부여
 
| 클레임| 필수 여부| 설명| 누락 된 경우 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 사용자| 호출자에 게 권한이 부여 된 Xbox LIVE 사용자입니다.| 호출자는 Xbox LIVE의 유효한 사용자 해야 합니다.| 403 Forbidden| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다.| 
| <b>x-xbl-contract-version</b>| 32 비트 부호 없는 정수| 있는 경우 2로이 API의 V2 버전 사용 됩니다. 그렇지 않으면 V1 합니다.| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/history/titles](uri-titlehistoryusersxuidhistorytitlesv2.md)

  
<a id="ID4EPG"></a>

 
##### <a name="reference"></a>참고자료 

[UserTitle (JSON)](../../json/json-usertitlev2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [페이징 매개 변수](../../additional/pagingparameters.md)

   