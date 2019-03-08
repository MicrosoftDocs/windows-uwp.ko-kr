---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
description: " GET (/media/{marketplaceId}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cc3ae8abfe04b7a0b9728d07b9488f9ed7c27816
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606678"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
필드 토큰을 가져옵니다. 이러한 Uri에 대 한 도메인은 `eds.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EGC)
  * [쿼리 문자열 매개 변수](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
엔터테인먼트 검색 서비스 (EDS) Api를 기본적으로 각 항목에 대 한 필드 집합이 매우 작은 최소를 반환합니다.
 
   * 미디어 항목 유형
   * 미디어 그룹
   * Id
   * 이름
  
자세한 정보를 얻으려면 Api는 다음과 같이 적용 됩니다.는 **필드** 반환 되어야 하는 추가 데이터 조각을 지정 하는 매개 변수입니다. 여러 가능한 필드 이기 때문에 각 API 호출에 대 한 전체에 해당 이름을 지정 하는 크게 블 로트 요청 합니다. 대신 이름은 다른 Api에 전달할 수 있는 훨씬 더 작은 값을 생성 하는이 API에 전달할 수 있습니다.
 
제공 된 값이 매개이 변수를 허용 하는 모든 API에 대 한 모든 지정 된 미디어 항목 형식에 모든 필드의 상위 집합 이어야 합니다. 다양 한 미디어 항목 형식에 대 한 필드의 다른 집합을 지정 하는 것이 불가능 합니다. 그러나 필드 하나 이상의 미디어 항목 형식을 다른을 제외한이 적용 되는 경우에 나타나며 미디어 항목 형식 데이터가 존재 하는 ("AvatarBodyType"에 대 한 호출에 포함 된 경우에 예를 들어 [GET (/media/ {marketplaceId} 필드 /)](uri-medialocalefields.md)만 AvatarItems 사용 될 필드).
 
이 API에서 반환 되는 값은 실제로-캐시가, EDS의 배포 간에 제외 하 고 변경 되지 않아야 합니다. 캐싱 필요한 경우 캐시 마지막 사용자의 세션 보다 길지 않은 것이 좋습니다.
 
실제 필드 이름이 수락 하는 것 외에도이 API가 허용 "all"을 유효한 값으로. 이 지정 하는 것이 불가능 각 필드를 포함 하는 값을 생성 됩니다. "All" 값을 사용 하 여 개발, 디버깅 및 테스트 목적에만 권장 됩니다.
 
보낼 수 있습니다 `desired={list of fields separated by a '.'}` 수락 하는 모든 API에는 **필드** 토큰입니다.
 
모두 전달할 수 없으며 **원하는** 하 고 **필드** 함께 합니다.
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| marketplaceId| 문자열| 필수. 문자열에서 가져온 값을 <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>합니다.| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 원하는| 문자열| 필수. '.' -최소 집합 외에도 반환 되어야 하는 필드 목록을 구분 합니다. 지정 된 모든 필드가 보장 됩니다 (데이터가 없을 수 있습니다 단순히 특정 항목에 대 한) 각 개체에 대해 반환할 하지만 여기에 지정 되지 않은 최소 집합 외부 없는 필드가 반환 됩니다. | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>추가 정보 

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   