---
title: 비디오에 대한 EDS 역방향 조회
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
description: " 비디오에 대한 EDS 역방향 조회"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d535dec8a95eba4d486bfc6946e187e2da66ae49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598438"
---
# <a name="eds-reverse-lookup-for-video"></a>비디오에 대한 EDS 역방향 조회
 
  * [역방향 조회 단계](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>역방향 조회 단계
 
엔터테인먼트 검색 서비스 (EDS) 역방향 조회는 모든 비디오 미디어 유형에 대해 지원 됩니다 (**MediaItemType.Movie**하십시오 **MediaItemType.TVSeries**, **MediaItemType.TVEpisode**, **MediaItemType.TVSeason**, 및 **MediaItemType.TVShow**), 뿐만 **MediaItemType.Unknown**합니다.
 
역방향 조회를 전달 하도록 4 개의 매개 변수가 필요 합니다. 
   * `idType=ScopedMediaId`
   * `ids=` 공급자 미디어 ID
   * `ScopeIdType=Title`
   * `ScopeId=` 공급자 제목 ID
 
 
일반적으로 역방향 조회는 2 단계가 필요합니다. 
   * 사용할 수 없는 경우 (예를 들어, 세부 정보 호출)에서 미디어 id 공급자를 검색 합니다. 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * 역방향 조회를 사용 하 여에 대 한 호출을 실행 합니다 **ProviderMediaId** 이전 응답에서 필드: 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
경우는 **ProviderMediaId** 필드 EDS에서 검색 되지 않았습니다 다음 필드는 EDS에 올바르게 전달할 URL 인코딩 이어야 합니다.
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>추가 정보 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   