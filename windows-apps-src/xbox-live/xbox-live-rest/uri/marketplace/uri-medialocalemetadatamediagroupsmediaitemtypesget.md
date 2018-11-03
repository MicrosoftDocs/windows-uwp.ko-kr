---
title: GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)
assetID: 1bbfdfd7-84e0-68e0-49e8-ba1c60fabaa3
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediagroupsmediaitemtypesget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a7ad60197979e24c5ed89e768aba916189cc0918
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5985625"
---
# <a name="get-mediamarketplaceidmetadatamediagroupsmediagroupmediaitemtypes"></a>GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)
EDS의 지정 된 버전에 대 한 미디어 그룹당 사용 가능한 mediaItemTypes 나열 되어 있습니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
| mediagroup| string| 필수. [GET (/media/ {marketplaceId} / 메타 데이터/mediaGroups)](uri-medialocalemetadatamediagroupsget.md)의 값 중 하나입니다.| 
  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>부모 

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

  
<a id="ID4EMB"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   