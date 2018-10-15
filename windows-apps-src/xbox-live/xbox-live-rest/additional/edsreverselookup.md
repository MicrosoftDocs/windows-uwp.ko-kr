---
title: 비디오에 대한 EDS 역방향 조회
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
author: KevinAsgari
description: " 비디오에 대한 EDS 역방향 조회"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b259ae20bd07c6869bc6646fc44a70f994a261b7
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4613665"
---
# <a name="eds-reverse-lookup-for-video"></a>비디오에 대한 EDS 역방향 조회
 
  * [역방향 조회 단계](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>역방향 조회 단계
 
엔터테인먼트 검색 서비스 (EDS) 역방향 조회 (**MediaItemType.Movie**, **MediaItemType.TVSeries**, **MediaItemType.TVEpisode**, **MediaItemType.TVSeason**및 **모든 비디오 미디어 형식에 대 한 지원 됩니다. MediaItemType.TVShow**), **MediaItemType.Unknown**뿐만 아니라 합니다.
 
역방향 조회에 전달 되는 4 개의 매개 변수가 필요 합니다. 
   * `idType=ScopedMediaId`
   * `ids=` 공급자 미디어 ID
   * `ScopeIdType=Title`
   * `ScopeId=` 공급자 제목 ID
 
 
일반적으로 역방향 조회 2 단계가 필요합니다. 
   * 사용할 수 없는 경우 (예: 세부 정보 호출)에서 공급자 미디어 id를 검색 합니다. 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * 이전 응답의 **ProviderMediaId** 필드를 사용 하 여 역방향 조회에 대 한 호출을 실행 합니다. 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
EDS에서 **ProviderMediaId** 필드를 검색 하지 않았기 EDS에 올바르게 전달할 URL 인코딩 필드 여야 합니다.
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>자세한 정보 

[마켓플레이스 URI](../uri/marketplace/atoc-reference-marketplace.md)

   