---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting
assetID: cf8319f6-46a2-b263-ea4c-f1ce403b571b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcasting.html
author: KevinAsgari
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a3d2e5c0bcbb0c59eabdffdd148e4b7f013c3f40
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4428227"
---
# <a name="usersxuidxuidgroupsmonikerbroadcasting"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting
액세스 그룹 모니커 하 여 지정 된 브로드캐스트 사용자의 존재 기록 URI에 표시 되는 XUID와 관련이 있습니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)와 관련 된 그룹에 XUIDs 사용자의 합니다.| 
| 모니커| string| 사용자의 그룹을 정의 하는 문자열입니다. 현재 허용 된 유일한 모니커 대문자 'P' "사람"입니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )](uri-usersxuidgroupsmonikerbroadcastingget.md)

&nbsp;&nbsp;지정 된 URI에 표시 되는 XUID 관련 그룹 모니커 브로드캐스트 사용자의 현재 상태 레코드를 검색 합니다.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>부모 

[상태 URI](atoc-reference-presence.md)

   