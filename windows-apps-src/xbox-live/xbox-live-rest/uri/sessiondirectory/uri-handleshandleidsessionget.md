---
title: GET (/handles/{handleId}/session)
assetID: 1f22954c-e77b-69c2-63f4-741fbd965f8f
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionget.html
description: " GET (/handles/{handleId}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 41911dc53b316f4f323b9859d9101581ec88e497
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "8743185"
---
# <a name="get-handleshandleidsession"></a>GET (/handles/{handleId}/session)
지정 된 핸들 식별자에 대 한 세션 개체를 가져옵니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EDB)
  * [HTTP 상태 코드](#ID4EOB)
  * [요청 본문](#ID4EVB)
  * [응답 본문](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드 (핸들) 세션에 제공 된 서비스에 있는 포인터를 사용 하 여 서버에서 세션 개체를 검색 합니다. 세션 개체의 모든 속성을 사용 하 여이 반환이 됩니다. 이 메서드는 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**하 여 줄 바꿈할 수 있습니다.

이 메서드의 호출자는 플레이어의 **MultiplayerActivityDetails** 개체에서 핸들 ID를 가져옵니다. 또는 호출자 사용자 게임 초대를 수락한 후에 Id에서를 프로토콜 활성화를 가져옵니다.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| handleId| GUID| 세션에 대 한 핸들의 고유 ID입니다.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4E6B"></a>


## <a name="response-body"></a>응답 본문
[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)에 대 한 응답 구조를 참조 하세요.  
<a id="ID4EIC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EKC"></a>


##### <a name="parent"></a>부모

[/handles/{handleId}/session](uri-handleshandleidsession.md)
