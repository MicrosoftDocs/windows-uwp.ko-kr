---
title: GET (/public/scids/{scid}/clips)
assetID: 15b3e873-1f96-b1da-2f79-6dac1369a4c0
permalink: en-us/docs/xboxlive/rest/uri-publicscidclipsget.html
author: KevinAsgari
description: " GET (/public/scids/{scid}/clips)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b945427118122e3b6d52210efc5e1de84a8c8d68
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4421598"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
공용 클립을 나열 합니다. 이 URI에 대 한 도메인은 `gameclipsmetadata.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4ECB)
  * [쿼리 문자열 매개 변수](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
이 API는 공용 목록 클립에 다양 한 방법으로 허용 합니다. 클립 목록 개인 정보 보호 검사 및 요청 XUID에 대 한 콘텐츠 격리 검사에 따라 반환 됩니다.
 
쿼리 (서비스 안내) 서비스 구성 id 별로 최적화 됩니다. 필터 또는 아래에 나열 된 기본값이 아닌 정렬 순서 추가로 지정 상황에 따라 시간이 오래 걸릴 수를 반환 합니다. 이 동영상의 더 큰 집합에 대 한 더 분명 하 게 합니다. 쿼리 오름차순으로 정렬 지정할 수 없습니다.
 
특정 컬렉션 ofpublic 클립을 가져오려면 한정자가 필요 합니다. 요청 사용자에 게 요청 된 서비스 안내에 액세스할 수 있어야, 그렇지 않으면 HTTP 403 반환 됩니다.
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| string| 기본 서비스 구성 공개 클립의 식별자입니다.| 
| titleid| string| 공용 클립의 titleId 합니다. 동일한 URI는 서비스 안내도 지정할 수 없습니다. 기본 서비스 안내를 조회할 수를 지정 하는 경우 사용 됩니다.| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| <b>? achievementId = {achievementId}</b>| 최근 클립 지정된 <b>achievementId</b>일치 합니다.| 추가 정렬/필터링 하는 것은 지원 되지 않습니다.| 
| <b>? greatestMomentId = {greatestMomentId}</b>| 최근 클립 지정된 <b>greatestMomentId</b>일치 합니다.| 추가 정렬/필터링 하는 것은 지원 되지 않습니다.| 
| <b>? 한정자 = 생성 </b>| 가장 최근| 필수.| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>부모 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   