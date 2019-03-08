---
title: /users/xuid({xuid})/groups/{moniker}
assetID: 7c73236b-95ee-723b-e5e0-68252c953e14
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmoniker.html
description: " /users/xuid({xuid})/groups/{moniker}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25ddc8120f05f04d5285fbbe4efc5a41f98265ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617558"
---
# <a name="usersxuidxuidgroupsmoniker"></a>/users/xuid({xuid})/groups/{moniker}
그룹에 대 한 PresenceRecord에 액세스합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 문자열| Xbox 사용자 ID (XUID)의 그룹 XUIDs와 관련 된 사용자입니다.| 
| 모니커| 문자열| 사용자 그룹을 정의 하는 문자열입니다. 현재 허용 된 유일한 모니커 대문자 'P'를 사용 하 여 "People" 됩니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET (/users/xuid({xuid})/groups/{moniker} )](uri-usersxuidgroupsmonikerget.md)

&nbsp;&nbsp;그룹을 PresenceRecord를 가져옵니다.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[현재 상태 Uri](atoc-reference-presence.md)

   