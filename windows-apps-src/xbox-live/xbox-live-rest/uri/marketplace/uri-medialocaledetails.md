---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
description: " /media/{marketplaceId}/details"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58e5247c3fd52e84a3a9bab28c6926f74e864e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655708"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
세부 정보 및 메타 데이터 반환 제공에 대 한 하나 이상의 항목. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`합니다.
 
API는 관련 API와 검색 API에서 다른 세부 정보 (때 ID의 passin) 세부 정보 API 추가 정보를 반환 하는 반면 해당 Api 명시적 또는 암시적으로 fiven ID와 연결 된 다른 항목에 대 한 정보를 반환 에 대 한 동일한 항목입니다.
 
단일에 다양 한 미디어 항목 유형의 여러 Id를 전달할 수 있습니다 (한 ProviderContentID-아래 참조 형식의 없는 것 이므로), 호출 되지만 모두 동일한 미디어 그룹에 속해야 합니다. 그러나 호출자에 게 미디어 그룹 알 수 없습니다 클라이언트 시나리오의 두 가지가 있습니다. API는 다음과 같은 경우의 미디어 그룹에 대 한 "알 수 없음" sepcial 값을 허용 하 여이 지원 합니다.
 
   * idType = XboxHexTitle AppType 또는 GameType 항목 생성
   * idType = ProviderContentId MovieType 또는 TVType 항목 생성
  
다음 차트에는 ID의 형식을 지정할 수 미디어 그룹을 사용 하 여 전체 매핑을 요약 되어 있습니다.
 
| ID 형식| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| 알 수 없음| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 정식| Y| Y| Y| Y| Y| Y| Y| N| 
| ZuneCatalog| N| N| Y| Y| Y| Y| N| N| 
| ZuneMediaInstance| N| N| Y| N| Y| Y| N| N| 
| 제품| Y| Y| Y| N| Y| Y| N| N| 
| AMG| N| N| N| N| Y| N| N| N| 
| MediaNet| N| N| N| N| Y| N| N| N| 
| XboxHexTitle| Y| Y| N| N| N| N| N| Y| 
| ProviderContentId| N| N| Y| N| N| Y| N| Y| 
 
  * [매개 변수 정보](#ID4EEH)
  * [URI 매개 변수](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>매개 변수 정보
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
조회 공급자에는이 특정 id입니다. 예: Netflix Id 또는 Hulu id입니다.
 
IdType ProviderContentId 경우 단일 값만 허용 됩니다. ProviderContentIds 유형만 포함할 수 있는 ID 있기 때문에 '.' 문자입니다. 이후는 '.' 문자 Id 간에 사용 하는 구분 기호 이기도, Id 간에 delimieter 이란 간에 모호성이 및 새로운 기능으로 ID의 일부입니다. Rest API의 대량 조회 기능을 제외 하 고 ProviderContentIds를 동일한 방식으로 작동합니다.
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| 문자열| 필수. 문자열에서 가져온 값을 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>합니다.| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[가져오기 (/media/ {marketplaceId} / 세부 정보)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;세부 정보 및 메타 데이터 반환 제공에 대 한 하나 이상의 항목. 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>추가 정보 

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   