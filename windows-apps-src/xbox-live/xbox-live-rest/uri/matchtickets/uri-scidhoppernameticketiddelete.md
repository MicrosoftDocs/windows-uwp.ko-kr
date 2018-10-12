---
title: DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
author: KevinAsgari
description: " DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3780fb9f69a97d4e2522aa17a806b1fb4917a9f7
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4566824"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})

일치 티켓을 제거합니다.

> [!IMPORTANT]
> 이 메서드를 계약 103 이상을 사용 하 여 사용 하기 위한 있으며 X Xbl-계약 버전의 헤더 요소: 103 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4E2)
  * [권한 부여](#ID4EGB)
  * [HTTP 상태 코드](#ID4EOC)
  * [요청 본문](#ID4EXC)
  * [응답 본문](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드에 서비스 구성 ID (서비스 안내) 수준에서 명명 된 hopper에서 지정 된 티켓 ID를 삭제합니다. 이 메서드는 **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**하 여 줄 바꿈할 수 있습니다.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| name| 문자열| hopper의 이름입니다.|
| ticketId| GUID| 티켓 id입니다.|

<a id="ID4EGB"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 누락 된 경우 응답|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (사용자 ID)| 예| 요청을 만드는 사용자 티켓에서 참조 하는 티켓 세션의 구성원 이어야 합니다.| 403|
| 남용 및 장치 유형| 예| 사용자의 deviceType 콘솔에 설정 된 경우 해당 클레임의 멀티 플레이 권한 있는 사용자만는 매치 메이 킹 서비스 호출을 할 수 있습니다.| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EXC"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4ECD"></a>


## <a name="response-body"></a>응답 본문

개체가 응답 본문에 전송 됩니다.

<a id="ID4EPD"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ERD"></a>


##### <a name="parent"></a>부모  

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)
