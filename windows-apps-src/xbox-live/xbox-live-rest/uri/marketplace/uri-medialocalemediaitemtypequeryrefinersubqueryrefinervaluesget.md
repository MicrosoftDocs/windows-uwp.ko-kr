---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
assetID: 0fcbef77-4607-765e-72e1-d2e7620e2c61
permalink: en-us/docs/xboxlive/rest/uri-medialocalemediaitemtypequeryrefinersubqueryrefinervaluesget.html
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 183d15a306d0f15fcb5f9f7fad8411772f763c13
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661818"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinersubqueryrefinervalues"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
지정 된 쿼리 구체화 값 (예를 들어 "subgenres 지정 장르의")에 대 한 하위 값의 목록을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EDB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
쿼리 구체화는 전달 된 값의 명명 된 쿼리 문자열 매개 변수로 **queryRefinerValue**,이 형식은 전달할 URI 형태소에서 사용할 수 없는 문자를 사용 하 여 쿼리 구체화 값을 허용 하도록 합니다.
 
이 API는 음악만 지원 됩니다.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| marketplaceId| 문자열| 필수. 문자열에서 가져온 값을 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>합니다.| 
  
<a id="ID4EOB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQB"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

  
<a id="ID4E1B"></a>

 
##### <a name="further-information"></a>추가 정보 

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   