---
title: /users/xuid({xuid})/groups/{moniker}
assetID: 7c73236b-95ee-723b-e5e0-68252c953e14
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmoniker.html
author: KevinAsgari
description: " /users/xuid({xuid})/groups/{moniker}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 640635458f50046a8fbc6a9cf539c659950e163e
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7146996"
---
# <a name="usersxuidxuidgroupsmoniker"></a>/users/xuid({xuid})/groups/{moniker}
PresenceRecord 그룹에 대 한 액세스합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)와 관련 된 그룹에 XUIDs 사용자의 합니다.| 
| 모니커| string| 사용자의 그룹을 정의 하는 문자열입니다. 현재만 허용된 모니커 대문자 'P' "사람"입니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/users/xuid({xuid})/groups/{moniker} )](uri-usersxuidgroupsmonikerget.md)

&nbsp;&nbsp;그룹의 PresenceRecord를 가져옵니다.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>부모 

[상태 URI](atoc-reference-presence.md)

   