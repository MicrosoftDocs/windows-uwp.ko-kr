---
title: /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd7ed139fffb8bdb10ac5074d5e9725753678f1c
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4539856"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}

일치 티켓 삭제 작업을 지원합니다.

> [!IMPORTANT]
> 이 URI에 계약 103 이상을 사용 하 여 사용 하며 X Xbl-계약 버전의 헤더 요소: 103 또는 나중에 모든 요청.

<a id="ID4ER"></a>


## <a name="domain"></a>도메인
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>설명
이 URI 값 xuid, gt, 및 me 대상 사용자의 구성에서 소유자 식별자에 대 한 지원합니다.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| name| 문자열| hopper의 이름입니다.|
| ticketId| GUID| 티켓 id입니다.|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>유효한 메서드

[DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;일치 티켓을 제거합니다.

<a id="ID4ETC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EVC"></a>


##### <a name="parent"></a>부모  

[매치 메이킹 URI](atoc-reference-matchtickets.md)
