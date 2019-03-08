---
title: /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9722d06af64c5fd24f53485a1bcfe765f89b08cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627318"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}

일치 티켓에 대 한 삭제 작업을 지원합니다.

> [!IMPORTANT]
> 이 URI 103 이상 계약과 함께 사용 하기 위한 하며 헤더 요소를 X Xbl-계약 버전: 103 또는 나중에 모든 요청 합니다.

<a id="ID4ER"></a>


## <a name="domain"></a>도메인
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>설명
이 URI는 대상 사용자의 구성에서 소유자 식별자에 대 한 값 xuid, gt, 및 me를 지원합니다.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| scid| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| name| 문자열| hopper의 이름입니다.|
| ticketId| GUID| 티켓 id입니다.|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>올바른 메서드

[DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;일치 티켓을 제거합니다.

<a id="ID4ETC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent  

[결혼 정보 회사 연결 Uri](atoc-reference-matchtickets.md)
