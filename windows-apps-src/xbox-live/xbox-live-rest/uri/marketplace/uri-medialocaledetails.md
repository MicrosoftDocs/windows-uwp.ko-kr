---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
author: KevinAsgari
description: " /media/{marketplaceId}/details"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d61d8f23936dc40648637df793d7610159498ac0
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6253785"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
반환 제품 세부 정보 및 메타 데이터에 대 한 하나 이상의 항목입니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
API와 관련 된 API 및 찾아보기 API에서 다른 세부 정보 (때 ID에 전달) 이러한 Api는 세부 정보 API를 사용 하는 추가 정보를 반환 하는 반면 명시적으로 또는 암시적으로 fiven ID와 연결 된 다른 항목에 대 한 정보를 반환 동일한 항목입니다.
 
다른 미디어 항목 종류의 여러 Id를 하나의 전달할 수 있습니다 (만큼 ProviderContentID-아래 참조 형식의 달라도), 호출 하지만 모두 동일한 미디어 그룹에 속해야 합니다. 그러나 호출자 미디어 그룹을 인식 하지 클라이언트 시나리오의 몇 가지 있습니다. API는 다음과 같은 경우에는 미디어 그룹에 대 한 "알 수 없음" sepcial 값을 허용 하 여이 지원 합니다.
 
   * idType AppType 또는 게임 항목을 얻을 수 있는 XboxHexTitle =
   * idType MovieType 또는 TVType 항목을 얻을 수 있는 ProviderContentId =
  
다음 차트는 어떤 미디어 그룹과 제공 수 유형 ID의 전체 매핑을 요약 되어 있습니다.
 
| ID 유형| AppType| 게임| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| 알 수 없음| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 정식| 예| 예| 예| 예| 예| 예| 예| N| 
| ZuneCatalog| N| N| 예| 예| 예| 예| N| N| 
| ZuneMediaInstance| N| N| 예| N| 예| 예| N| N| 
| 제안| 예| 예| 예| N| 예| 예| N| N| 
| AMG| N| N| N| N| 예| N| N| N| 
| MediaNet| N| N| N| N| 예| N| N| N| 
| XboxHexTitle| 예| 예| N| N| N| N| N| 예| 
| ProviderContentId| N| N| 예| N| N| 예| N| 예| 
 
  * [매개 변수 메모](#ID4EEH)
  * [URI 매개 변수](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>매개 변수 메모
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
이 기능은 사용 조회 공급자에 특정 id입니다. 예 Netflix Id 또는 Hulu id입니다.
 
IdType ProviderContentId 경우 단일 값만 허용 됩니다. ProviderContentIds ID 포함할 수 있는 유일한 형식의 있기 때문에 '.' 문자입니다. 이후는 '.' 문자 사이의 Id를 사용 하 여 구분 기호 이기도 하 고 모호 Id 간 delimieter 이란 사이 ID 자체 포함 됩니다. API의 나머지 부분을 ProviderContentIds 동일한 방식으로 대량 조회 기능을 제외 하 고 사용할 수 있습니다.
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/media/{marketplaceId}/details)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;반환 제품 세부 정보 및 메타 데이터에 대 한 하나 이상의 항목입니다. 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>부모 

[마켓플레이스 URI](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   