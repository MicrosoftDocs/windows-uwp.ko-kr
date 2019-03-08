---
title: 매치 메이킹 URI
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
description: " 매치 메이킹 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c52abca7ed49d4a5e14520095ae944938b86f093
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590468"
---
# <a name="matchmaking-uris"></a>매치 메이킹 URI
 
이 섹션에서는 Xbox Live 서비스에 서비스를 결혼 정보 회사에서 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공합니다. 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>도메인
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>서비스 버전
 
이러한 HTTP/REST Uri의 호출자에 게 전달 해야 값 103 이상 X-Xbl-계약-버전, 엔터테인먼트 검색 서비스 (EDS) 서비스 버전을 지정 하는 HTTP 헤더에 대 한 합니다. 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>시스템 개체 및 속성
 
결혼 정보 회사 연결 서비스의 모든 구성을 수동으로 발생의 서비스 구성 부분을 사용 하는 현재 합니다 [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com) 하거나 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 일부 결혼 정보 회사 연결 정보를 MPSD에 대해 정의 된 개체에도 반영 됩니다. 
 
결혼 정보 회사 연결을 구성 하는 데 사용 된 기본 JSON 개체에 정의 된 [MatchTicket (JSON)](../../json/json-matchticket.md) 하 고 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)합니다. 티켓 모두 일치 하는 참고를 정의 해야 합니다는 **ticketSessionRef** 플레이어 또는 다른 사용자와 일치 하는 플레이어를 포함 하는 멀티 플레이 세션에 대 한 참조를 제공 하는 개체입니다. 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;일치 티켓을 만들려면 POST 작업을 지원 합니다. 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;hopper에 대 한 통계를 검색 하는 것에 대 한 가져오기 작업을 지원 합니다.

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;일치 티켓에 대 한 삭제 작업을 지원합니다.
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [세션 디렉터리 Uri](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)

   