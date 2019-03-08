---
title: /titles/{titleId}/sessions/{sessionId}/allocationStatus
assetID: 55611f4b-4ba4-fa9a-ce44-fcc4a6df1b35
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus.html
description: " /titles/{titleId}/sessions/{sessionId}/allocationStatus"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aa66b81d1e363488a6969ab31d7091b695f09ae4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603308"
---
# <a name="titlestitleidsessionssessionidallocationstatus"></a>/titles/{titleId}/sessions/{sessionId}/allocationStatus
지정 된 제목 id 및 세션 id에 대해 티켓 요청의 상태를 가져옵니다. 이러한 Uri에 대 한 도메인이 `gameserverds.xboxlive.com` 고 `gameserverms.xboxlive.com`입니다.
 
  * [URI 매개 변수](#ID4EU)
  * [호스트 이름](#ID4EPB)
  * [올바른 메서드](#ID4EWB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 설명| 
| --- | --- | 
| titleId| ID는 요청에서 작동 해야 하는 제목입니다.| 
| sessionId| 조회 세션의 ID입니다.| 
  
<a id="ID4EPB"></a>

 
## <a name="host-name"></a>호스트 이름
 
gameserverms.xboxlive.com
  
<a id="ID4EWB"></a>

 
## <a name="valid-methods"></a>올바른 메서드
  
[GET](uri-titlestitleidsessionssessionidallocationstatus-get.md)
 
&nbsp;&nbsp;해당 세션 Id로 식별 sessionhost의 할당 상태를 반환 합니다.
   