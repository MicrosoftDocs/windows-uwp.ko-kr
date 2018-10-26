---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
assetID: 0fcbef77-4607-765e-72e1-d2e7620e2c61
permalink: en-us/docs/xboxlive/rest/uri-medialocalemediaitemtypequeryrefinersubqueryrefinervaluesget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 55041b6a060a79964c0fd62975642d8b381bc7c0
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5547448"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinersubqueryrefinervalues"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
지정 된 쿼리 구체화 값 (예: "subgenres 주어진 장르의")에 대 한 하위 값 목록을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EDB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
쿼리 구체화 값에서 **queryRefinerValue**, URI 획에 전달할 수를 사용할 수 없음 문자로 쿼리 구체화 값을 허용 하도록 수행할 라는 쿼리 문자열 매개 변수로 전달 됩니다.
 
이 API는 음악에 대 한 에서만 지원 됩니다.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
  
<a id="ID4EOB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQB"></a>

 
##### <a name="parent"></a>부모 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

  
<a id="ID4E1B"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   