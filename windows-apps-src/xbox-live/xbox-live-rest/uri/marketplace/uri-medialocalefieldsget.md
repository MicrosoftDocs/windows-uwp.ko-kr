---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/fields)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c46981121393ce80228d857c32a01784d58af96
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5407868"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
필드 토큰을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EGC)
  * [쿼리 문자열 매개 변수](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
엔터테인먼트 검색 서비스 (EDS) Api를 기본적으로 각 항목에 대 한 필드는 매우 작은 세트로 반환합니다.
 
   * 미디어 항목 유형
   * 미디어 그룹
   * Id
   * 이름
  
자세한 정보를 가져오려면 Api는 데이터의 추가 조각을 반환할지를 지정 하는 **필드** 매개 변수를 수락 합니다. 수 있는 많은 필드 인 각 API 호출에 대 한 전체에 자신의 이름을 지정 하는 요청 크게 볼록 합니다. 대신, 이름은 다른 Api로 전달 되는 훨씬 더 작은 값을 생성할이 API에 전달할 수 있습니다.
 
이 매개 변수를 허용 하는 모든 API를 제공 된 값에는 모든 지정 된 미디어 항목 종류의 모든 필드의 상위 집합 해야 합니다. 서로 다른 미디어 항목 형식에 대 한 필드를 지정 하는 것이 불가능 합니다. 하지만 필드 하나 이상의 미디어 항목 형식을 다른을 제외한이 적용 되는 경우에 표시 됩니다 미디어 항목 형식 데이터 존재 하는 위치 ("AvatarBodyType"에 대 한 호출에 포함 된 경우 예를 들어 [GET (/media/ {marketplaceId} /fields)]()만 AvatarItems 됩니다 필드가 포함).
 
이 API에서 반환 된 값은 매우 캐시할-사실, EDS의 배포 사이 제외 하 고 변경 되지 않아야 합니다. 필요한 캐싱의 경우 캐시 마지막 사용자의 세션 보다 더 이상 것이 좋습니다.
 
실제 필드 이름을 수락 하는 것 외에도이 API가 허용 "모든" 유효한 값으로 합니다. 이렇게 하면 각 필드를 지정할 수 있는 값을 생성 됩니다. "All" 값을 사용 하 여 디버깅 및 테스트 목적으로 개발 작업에이 좋습니다.
 
보낼 수 있습니다 `desired={list of fields separated by a '.'}` **필드** 토큰을 허용 하는 모든 api.
 
함께 **원하는** 및 **필드** 에 전달할 수 없습니다.
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| marketplaceId| string| 필수. 문자열 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>에서 가져온 값입니다.| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 원하는| string| 필수. '.' -최소 집합 외에도 반환할지 필드 목록을 구분 합니다. 지정 된 모든 필드 (데이터 간단 하 게 없거나 특정 항목에 대 한)는 각 개체에 대해 반환할 보장이 하지만 여기에 지정 되지 않은 최소한 외부 필드가 반환 됩니다. | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>부모 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   