---
title: /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
assetID: fc096def-ac64-76c6-09f8-8f33a6bb47a0
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediagroupsmediaitemtypes.html
author: KevinAsgari
description: " /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9a5f7e0808f6d9392ea738440779e477b7c999b1
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4340463"
---
# <a name="mediamarketplaceidmetadatamediagroupsmediagroupmediaitemtypes"></a>/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
EDS의 지정 된 버전에 대 한 미디어 그룹당 사용할 수 있는 mediaItemTypes에 액세스합니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
| mediagroup| string| 필수. [GET (/media/ {marketplaceId} / 메타 데이터/mediaGroups)](uri-medialocalemetadatamediagroupsget.md)의 값 중 하나입니다.| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)

&nbsp;&nbsp;EDS의 지정 된 버전에 대 한 미디어 그룹당 사용할 수 있는 mediaItemTypes 나열 되어 있습니다.
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>부모 

[마켓플레이스 URI](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   