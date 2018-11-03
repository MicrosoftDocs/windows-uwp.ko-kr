---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/contentRating)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e4cfbbccea617a19d85b9f5601c33f94839dd9ae
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5973361"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
콘텐츠 등급 토큰을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4ELB)
  * [쿼리 문자열 매개 변수](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
복잡 한 작업은 자식 볼 수 있는 콘텐츠 위에 자녀 보호를 적용 합니다. 각 미디어 항목 형식에는 자체 등급 시스템 할 뿐만 아니라 이러한 등급 시스템 국가 마다 다를 수 있습니다. 이 데이터를 제대로 모든 항목을 필터링 하려면 지정 해야 하는 다른 여러 가지는 것을 의미 합니다.
 
모든 매개 변수를 모든 API 호출에 지정 하는 대신이 API의 다른 Api **combinedContentRating** 매개 변수를 전달 하 고 여전히 동일한 정보를 전달 하기 위한 값을 생성 합니다. 이이 API에 전달 되는 여러 매개 변수가 다른 Api에 대 한 단일, 재사용 가능한 값으로 축소 된 대로 Api를 사용 하 고 유지 관리 쉽게 수 있도록 설계 되었습니다.
 
이 API에 의해 반환 되는 정확한 값 결국 변경 될 수 있지만 해야 바뀌는 매우 드물게 (예: 엔터테인먼트 검색 서비스 (EDS)의 릴리스) 따라서 오랜 시간 동안 캐시 될 수 있습니다. 표시 되는 모든 API **combinedContentRating** 매개 변수에서 전달 된 값에 유효 하지 않으면 의미 있는 오류 메시지가 생깁니다 수락 호출자 단순히 다시 업데이트 된 값을 얻으려면이 API를 호출할 필요 합니다. API **combinedContentRating** 매개 변수를 수락 하지만 제공 하지 않는 한 콘텐츠 필터링 하지 걸립니다 자녀에 따라 장소. 

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
| filterUnrated| 부울 값| 선택 사항입니다. 등급이 지정 되지 않은 콘텐츠 또는 응답에 포함 되어야 하는지 여부를 결정 합니다.| 
| maxGameRating| 32 비트의 부호 있는 정수| 선택 사항입니다. 게임을 필터링합니다.| 
| maxMovieRating| 32 비트의 부호 있는 정수| 선택 사항입니다. 특정 수준 이상 영화를 필터링합니다.| 
| maxTVRating| 32 비트의 부호 있는 정수| 선택 사항입니다. TV를 필터링합니다.| 
  
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

   