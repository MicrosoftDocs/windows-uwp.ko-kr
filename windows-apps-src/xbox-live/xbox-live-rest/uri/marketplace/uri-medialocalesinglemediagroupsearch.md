---
title: /media/{marketplaceId}/singleMediaGroupSearch
assetID: f5599db7-4050-640e-db96-2df01a007c07
permalink: en-us/docs/xboxlive/rest/uri-medialocalesinglemediagroupsearch.html
description: " /media/{marketplaceId}/singleMediaGroupSearch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b26b4c2dc51ef5591480372aa9908a49d2f8cbe2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661758"
---
# <a name="mediamarketplaceidsinglemediagroupsearch"></a>/media/{marketplaceId}/singleMediaGroupSearch
단일 미디어 그룹 내의 항목에 대 한 검색을 허용합니다. 이 검색에서 반환 된 데이터의 페이지에 비순차적 skipItems 매개 변수를 사용 하 여 continuation 토큰을 사용 하는 대신 액세스할 수 있는지 참고 합니다. 이 API는 쿼리 구체화를 허용합니다.
 
이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| marketplaceId| 문자열| 필수. 문자열에서 가져온 값을 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>합니다.| 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[가져오기 (media / {marketplaceId} / singleMediaGroupSearch)](uri-medialocalesinglemediagroupsearchget.md)

&nbsp;&nbsp;단일 미디어 그룹 내의 항목에 대 한 검색을 허용합니다. 
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EOC"></a>

 
##### <a name="further-information"></a>추가 정보 

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   