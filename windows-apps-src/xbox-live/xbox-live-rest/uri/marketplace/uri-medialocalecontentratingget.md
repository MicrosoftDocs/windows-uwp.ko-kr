---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/contentRating)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 456ae44dcffeede64011719c02dbeb3806792405
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5169420"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
콘텐츠 등급 토큰을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4ELB)
  * [쿼리 문자열 매개 변수](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
복잡 한 작업은 자식 볼 수 있는 콘텐츠 위에 자녀 보호를 적용 합니다. 각 미디어 항목 형식에는 자체 등급 시스템 할 뿐만 아니라 이러한 등급 시스템 국가 마다 다를 수 있습니다. 즉, 제대로 모든 항목을 필터링 하려면 지정 해야 하는 데이터의 서로 다른 여러 가지 있습니다.
 
모든 매개 변수를 모든 API 호출에 지정 하는 대신이 API는 다른 Api에서 **combinedContentRating** 매개 변수로 전달 하 고 여전히 동일한 정보를 전달 하기 위한 값을 생성 합니다. 이 쉽게 Api를 사용 하 고 유지 하 고,이 API에 전달 되는 몇 가지 매개 변수가 다른 Api에 대 한 단일, 재사용 가능한 값으로 축소 된 대로 설계 되었습니다.
 
이 API에서 반환 되는 정확한 값 결국 변경 될 수 있지만 해야 바뀌는 매우 드물게 (예: 엔터테인먼트 검색 서비스 (EDS)의 릴리스) 따라서 오랜 시간 동안 캐시 될 수 있습니다. 수락 **combinedContentRating** 매개 변수에서 의미 있는 오류 메시지가 표시에 전달 된 값을 잘못 된 경우 표시 되는 모든 API 호출자 단순히 해야 다시 업데이트 된 값을 얻으려면이 API를 호출 합니다. API 수락 **combinedContentRating** 매개 변수를 입력 하지 않은 경우, 콘텐츠 필터링 없이 적용 됩니다 자녀에 따라 합니다. 

> [!NOTE] 
> 그렇다고 "안전한"만 콘텐츠는 반환 했음을-의미 명시적 잠재적으로 콘텐츠를 비롯 한 모든 콘텐츠 반환 됩니다. 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| 부울 값| 선택 사항입니다. 명시적 음악을 필터링합니다.| 
| filterFamilyOnlyApps| 부울 값| 선택 사항입니다. 표시 되지 않은 패밀리로 식별 하는 앱을 필터링 합니다.| 
| filterUnrated| 부울 값| 선택 사항입니다. 여부 등급이 지정 되지 않은 콘텐츠 응답에 포함 되어야 하는지 여부를 결정 합니다.| 
| maxGameRating| 32 비트 부호 있는 정수| 선택 사항입니다. 게임을 필터링합니다.| 
| maxMovieRating| 32 비트 부호 있는 정수| 선택 사항입니다. 특정 수준 이상 영화를 필터링합니다.| 
| maxTVRating| 32 비트 부호 있는 정수| 선택 사항입니다. TV를 필터링합니다.| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>부모 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   