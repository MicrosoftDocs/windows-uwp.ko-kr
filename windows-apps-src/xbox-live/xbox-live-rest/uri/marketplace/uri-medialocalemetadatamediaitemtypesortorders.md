---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
assetID: 221c440d-c448-11e9-f455-6d0f95fc16ef
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypesortorders.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e807f1a29712f55fc2e404e3905c7c69eb50d166
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640928"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypesortorders"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
액세스를 사용할 수 있는 정렬 순서 지정된 mediaitem 형식과 EDS의 지정된 된 버전에 대 한 합니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| marketplaceId| 문자열| 필수. 문자열에서 가져온 값을 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>합니다.| 
| mediaitemtype| 문자열| 필수. 값 중 하나 [GET (/media/ {marketplaceId} / 메타 데이터/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)합니다.| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[가져오기 (/media/ {marketplaceId} / 메타 데이터/mediaItemTypes / {mediaitemtype} / sortorders)](uri-medialocalemetadatamediaitemtypesortordersget.md)

&nbsp;&nbsp;지정 된 mediaitem 형식과 EDS의 지정된 된 버전에 대 한 사용 가능한 정렬 순서를 나열합니다.
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>추가 정보 

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   