---
title: /titles/{titleId}/sessions/{sessionId}/allocationStatus
assetID: 55611f4b-4ba4-fa9a-ce44-fcc4a6df1b35
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus.html
author: KevinAsgari
description: " /titles/{titleId}/sessions/{sessionId}/allocationStatus"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f8137980bddbf494c989f4f8a39c2f6edaf3d30a
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4472868"
---
# <a name="titlestitleidsessionssessionidallocationstatus"></a>/titles/{titleId}/sessions/{sessionId}/allocationStatus
지정 된 제목 id와 세션 id가에 대 한 티켓 요청 상태를 가져옵니다. 이러한 Uri에 대 한 도메인은 `gameserverds.xboxlive.com` 및 `gameserverms.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EU)
  * [호스트 이름](#ID4EPB)
  * [유효한 메서드](#ID4EWB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 설명| 
| --- | --- | 
| titleId| 요청에서 작동 해야 하는 타이틀의 ID입니다.| 
| sessionId| 조회 세션의 ID입니다.| 
  
<a id="ID4EPB"></a>

 
## <a name="host-name"></a>호스트 이름
 
gameserverms.xboxlive.com
  
<a id="ID4EWB"></a>

 
## <a name="valid-methods"></a>유효한 메서드
  
[GET](uri-titlestitleidsessionssessionidallocationstatus-get.md)
 
&nbsp;&nbsp;세션의 Id로 식별 sessionhost 할당 상태를 반환 합니다.
   