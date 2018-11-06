---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
assetID: 1e2f5fd3-3d14-9a97-ffbf-ab2a18ff4f15
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.html
author: KevinAsgari
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5105b8eb5c23aac5b71c6cadec35e62c7dced927
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6026530"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinername"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
지정 된 쿼리 구체화 이름에 대 한 및 미디어 항목 유형 지정 된 허용 되는 값에 액세스합니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
| mediaitemtype| string| 필수. 값 중 하나로 [GET (/media/ {marketplaceId} / /metadata/mediagroups/ {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)합니다.| 
| queryrefinername| string| 필수. 이름 쿼리 구체화의 어떤 값이 필요한에 대 한, 예: "장르" 또는 "10 년"입니다. QueryRefiners를 참조 하세요.| 
  
<a id="ID4EKC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername})](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinernameget.md)

&nbsp;&nbsp;지정 된 쿼리 구체화 이름에 대 한 및 미디어 항목 유형 지정 된 허용 되는 값을 나열합니다.
 
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>부모 

[마켓플레이스 URI](atoc-reference-marketplace.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   