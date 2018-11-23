---
title: 매치 메이킹 URI
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
author: KevinAsgari
description: " 매치 메이킹 URI"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dc3aa9844a00d3a2055f263f3f7c17f33fb7d370
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7567312"
---
# <a name="matchmaking-uris"></a>매치 메이킹 URI
 
이 섹션에서는 매치 메이 킹 서비스에 대 한 Xbox Live 서비스에서 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 합니다. 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>도메인
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>서비스 버전
 
이러한 HTTP/REST Uri의 호출자에 게 전달 해야 값 103 이상 X-Xbl-계약-버전, 서비스 버전 엔터테인먼트 검색 서비스 (EDS)을 지정 하는 HTTP 헤더에 대 한 합니다. 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>시스템 개체 및 속성
 
현재 매치 메이 킹 서비스의 모든 구성이 발생 수동으로 [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com) 또는 [파트너 센터](https://partner.microsoft.com/dashboard)의 서비스 구성 부분을 사용 하 여 합니다. 일부 매치 메이 킹 정보는 MPSD에 정의 된 개체에도 반영 됩니다. 
 
매치 메이 킹 구성에 사용 되는 주 JSON 개체 [MatchTicket (JSON)](../../json/json-matchticket.md) 및 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)에서 정의 됩니다. 참고 티켓 일치 하는 모든 플레이어 또는 다른 사용자와 일치 하는 플레이어를 포함 하는 멀티 플레이 세션에 대 한 참조를 제공 하는 **ticketSessionRef** 개체를 정의 해야 합니다. 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;일치 티켓 만들기를 게시 작업을 지원 합니다. 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;hopper에 대 한 통계를 검색 하기 위한 가져오기 작업을 지원 합니다.

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;일치 티켓 삭제 작업을 지원합니다.
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EPC"></a>

   [MatchTicket(JSON)](../../json/json-matchticket.md)

 [HopperStatsResults(JSON)](../../json/json-hopperstatsresults.md)

 [세션 디렉터리 URI](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   