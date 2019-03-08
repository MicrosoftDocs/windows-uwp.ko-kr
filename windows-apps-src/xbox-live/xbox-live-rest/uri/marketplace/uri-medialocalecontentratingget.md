---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
description: " GET (/media/{marketplaceId}/contentRating)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d1cb9d09de8671d4cd3d61e96a8335412237e5c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641588"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
콘텐츠 등급 토큰을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4ELB)
  * [쿼리 문자열 매개 변수](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
자식을 볼 수 있는 콘텐츠에 대해 자녀를 적용 하는 것은 복잡 한 작업입니다. 각 미디어 항목 유형 자체 등급 시스템이 있습니까 할 뿐만 아니라 이러한 등급 시스템 국가 마다 다를 수 있습니다. 이 제대로 모든 항목을 필터링 하도록 지정 해야 하는 데이터의 일부의 조각은 것을 의미 합니다.
 
모든 매개 변수를 모든 API 호출에서 지정 하는 대신에이 API에 전달할 값을 생성 **combinedContentRating** 다른 Api에 대 한 매개 변수 여전히과 동일한 정보를 전달 하 고 있습니다. 이 Api를 쉽게 수행할 수 있도록 사용 및 유지 관리 하 고,이 API에 전달 하는 여러 매개 변수는 다른 Api에 대 한 단일, 다시 사용할 수 있는 값으로 축소 된 형태로 설계 되었습니다.
 
자주 변경 해야이 API에서 반환 된 정확한 값을 최종적으로 변경 될 수 있지만 (예: 엔터테인먼트 검색 서비스 (EDS)의 릴리스 사이) 따라서 오랜 시간 동안 캐시 될 수 없습니다. 수락 하는 모든 API는 **combinedContentRating** 매개 변수에서 의미 있는 오류 메시지가 표시에 전달 된 값을 유효 하지 않은 경우 호출자는 단순히 업데이트 된 값을 가져오려고 다시이 API를 호출 해야 하는 표시 인 합니다. API를 허용 하는 경우는 **combinedContentRating** 매개 변수 이지만 하나 제공 되지 않으면 콘텐츠 필터링을 하지 않고 수행 됩니다 자녀에 기반 합니다. 

> [!NOTE] 
> "안전한"만 콘텐츠가 반환 되도록-잠재적 성인 등급 콘텐츠를 포함 하 여 모든 콘텐츠가 반환 됩니다 것은 아닙니다. 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | 
| marketplaceId| 문자열| 필수. 문자열에서 가져온 값을 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>합니다.| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| 부울 값| 선택 사항. 명시적 음악을 필터링합니다.| 
| filterFamilyOnlyApps| 부울 값| 선택 사항. 로 표시 되지 않은 제품군 친숙 한 앱을 필터링 합니다.| 
| filterUnrated| 부울 값| 선택 사항. 여부 등급이 지정 되지 않은 콘텐츠 응답에 포함시킬지 여부를 결정 합니다.| 
| maxGameRating| 32 비트 부호 있는 정수| 선택 사항. 게임을 필터링합니다.| 
| maxMovieRating| 32 비트 부호 있는 정수| 선택 사항. 특정 수준 이상 영화를 필터링합니다.| 
| maxTVRating| 32 비트 부호 있는 정수| 선택 사항. TV를 필터링합니다.| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>추가 정보 

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   