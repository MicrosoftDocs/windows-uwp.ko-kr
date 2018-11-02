---
title: DELETE (/handles/{handleId})
assetID: 71cceff4-3a74-dc05-12db-cfe3f20a8aea
permalink: en-us/docs/xboxlive/rest/uri-handleshandleiddelete.html
author: KevinAsgari
description: " DELETE (/handles/{handleId})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da73760969e7a4a9e268644555d0790980b16123
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5921768"
---
# <a name="delete-handleshandleid"></a>DELETE (/handles/{handleId})
핸들 id 지정 처리를 삭제 합니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EAB)
  * [HTTP 상태 코드](#ID4ELB)
  * [요청 본문](#ID4ESB)
  * [응답 본문](#ID4E4B)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명
이 HTTP/REST 메서드에 지정 된 ID에 대 한 핸들을 삭제 하 고 세션에 대 한 사용자의 현재 활동을 지웁니다. 이 메서드는 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.ClearActivityAsync**하 여 줄 바꿈할 수 있습니다.  
<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| handleId| GUID| 세션에 대 한 핸들의 고유 ID입니다.|

<a id="ID4ELB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4ESB"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4E4B"></a>


## <a name="response-body"></a>응답 본문

개체가 응답 본문에 전송 됩니다.

<a id="ID4EIC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EKC"></a>


##### <a name="parent"></a>부모

[/handles/{handleId}](uri-handleshandleid.md)
