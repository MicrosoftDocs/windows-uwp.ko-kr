---
title: GET (/handles/{handle-id})
assetID: c95b5ab5-d56a-f70d-20d8-afb48d122ccd
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidget.html
author: KevinAsgari
description: " GET (/handles/{handle-id})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 39d39db0fa17c85dfad3dcca7526d13618835135
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5994330"
---
# <a name="get-handleshandle-id"></a>GET (/handles/{handle-id})
핸들 id 지정 핸들을 검색 합니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EDB)
  * [HTTP 상태 코드](#ID4EOB)
  * [요청 본문](#ID4EUB)
  * [응답 본문](#ID4E5B)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드는 지정 된 핸들에 대 한 세션에 대 한 사용자의 현재 동작을 가져옵니다. 세션 개체의 모든 특성을 사용 하 여이 반환이 됩니다. 이 메서드는 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**하 여 줄 바꿈할 수 있습니다.

이 메서드의 호출자는 플레이어의 **MultiplayerActivityDetails** 개체에서 핸들의 ID를 가져옵니다. 또는 호출자에 게 사용자 게임 초대를 수락한 후에 Id에서를 프로토콜 활성화를 가져옵니다.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| handleId| GUID| 세션에 대 한 핸들의 고유 ID입니다.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4E5B"></a>


## <a name="response-body"></a>응답 본문
[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)응답 구조를 참조 하세요.  
<a id="ID4EKC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EMC"></a>


##### <a name="parent"></a>부모

[/handles/{handleId}](uri-handleshandleid.md)
