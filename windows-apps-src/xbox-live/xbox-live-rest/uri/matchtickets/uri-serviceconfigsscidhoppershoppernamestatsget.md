---
title: GET (/serviceconfigs/{scid}/hoppers/{name}/stats)
assetID: 4de5b07d-93e1-8ff0-05dd-1d3bb1802088
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestatsget.html
author: KevinAsgari
description: " GET (/serviceconfigs/{scid}/hoppers/{name}/stats)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 242a3bd3a2e6112436ec3f7aa3dad60c05619314
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4569595"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

hopper에 대 한 통계를 가져옵니다.

> [!IMPORTANT]
> 이 메서드를 계약 103 이상을 사용 하 여 사용 하기 위한 있으며 X Xbl-계약 버전의 헤더 요소: 103 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [HTTP 상태 코드](#ID4E3C)
  * [요청 본문](#ID4EFD)
  * [응답 본문](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명
이 HTTP/REST 메서드에 서비스 구성 ID (서비스 안내) 수준에서 명명 된 hopper에서 통계를 가져옵니다. **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API이 메서드를 줄 바꿈할 수 있습니다.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| name| 문자열| hopper의 이름입니다.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 누락 된 경우 응답|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (사용자 ID)| 예| 요청을 만드는 사용자 티켓에서 참조 하는 티켓 세션의 구성원 이어야 합니다. | 403|
| 남용 및 장치 유형| 예| 사용자의 deviceType 콘솔에 설정 된 경우 해당 클레임의 멀티 플레이 권한 있는 사용자만는 매치 메이 킹 서비스 호출을 할 수 있습니다. | 403|
| 제목 ID/증명 구매/장치 유형| 예| 에 일치 하는 제목 지정 된 제목 클레임, 장치 유형 조합에 대 한 연결을 허용 해야 합니다. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4EQD"></a>


## <a name="response-body"></a>응답 본문

| 멤버| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| string| 선택한 hopper의 이름입니다.|
| waitTime| 32 비트 부호 있는 정수| 평균 시간 hopper (정수로 초)에 대 한 일치 합니다. |
| 채우기| 32 비트 부호 있는 정수| 사람들은 hopper 이후의 일치 항목을 기다리는의 수입니다.|

<a id="ID4E1D"></a>


### <a name="sample-response"></a>예제 응답


```cpp
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }


```


<a id="ID4EJE"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ELE"></a>


##### <a name="parent"></a>부모  

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)
