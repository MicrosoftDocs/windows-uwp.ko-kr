---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
author: KevinAsgari
description: " /media/{marketplaceId}/browse"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 776db1cf795ae964621d751d6b4b72d22ba82c2d
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4340558"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
단일 미디어 그룹 내에서 항목을 검색할 수 있습니다. 찾아보기 API를 통해 클라이언트 단일 미디어 그룹 내에서 항목을 찾습니다. 비 순차적으로 skipItems 매개 변수를 사용 하 여 연속 토큰을 사용 하는 대신 데이터 페이지에 액세스할 수 있습니다.
 
이 API를 사용 하면 지정 된 항목의 자식 내에서 검색 합니다. 예를 들어 전달 하 여 ID 및 MediaItemType 매개 변수는 Xbox 360 게임에 대 한, 따라서 검색 및 diltering DLC 게임에 대 한 아바타 항목 등의 해당 항목의 자식에 있습니다.
 
이 API는 쿼리 구체화를 수락합니다.
 
자식 항목 검색에 대 한 일부 시나리오는 다음과 같습니다.
 
   * 앨범 트랙을
   * 계절 시리즈
   * 에피소드를 계절
   * 음악 비디오를 추적 합니다.
   * 앨범이 아티스트
   * 게임 추가 기능 (DLC, 아바타, 테마 등)에 게임
  
이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (media/{marketplaceId}/browse)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;단일 미디어 그룹 내에서 항목을 검색할 수 있습니다. 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>부모 

[마켓플레이스 URI](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   