---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
description: " /media/{marketplaceId}/browse"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f692fb66580e20ffeefb3595b8cf9d795f504311
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589688"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
단일 미디어 그룹 내의 항목에 대 한 검색을 허용 합니다. 찾아보기 API를 통해 클라이언트에서 단일 미디어 그룹 내에서 항목을 찾습니다. 무작위로 skipItems 매개 변수를 사용 하 여 continuation 토큰을 사용 하는 대신 데이터 페이지에 액세스할 수 있습니다.
 
이 API는 지정된 된 항목의 자식 내에서 검색할 수도 있습니다. 예를 들어, 전달 하 여 ID 및 MediaItemType 매개 변수는 Xbox 360 게임에 대 한, 따라서 찾아보고 diltering 아바타 항목 등 게임에 대 한 DLC 해당 항목의 자식에 대해 합니다.
 
이 API는 쿼리 구체화를 허용합니다.
 
자식을 검색 하는 것에 대 한 일부 시나리오에는 다음이 포함 됩니다.
 
   * 앨범 트랙을
   * 계절 시리즈
   * 계절 에피소드를
   * 음악 비디오에 대 한 추적
   * 앨범을 artist
   * 게임 게임 추가 기능 (DLC, 아바타, 테마 등)
  
이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| marketplaceId| 문자열| 필수. 문자열에서 가져온 값을 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>합니다.| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET (media/{marketplaceId}/browse)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;단일 미디어 그룹 내의 항목에 대 한 검색을 허용 합니다. 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>추가 정보 

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   