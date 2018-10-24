---
title: 상태 URI
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
author: KevinAsgari
description: " 상태 URI"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f4c2a34d47f894e2ac9aeaf6228c8ebd41348306
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5442364"
---
# <a name="presence-uris"></a>상태 URI
 
이 섹션에서는 *현재 상태*에 대 한 Xbox Live 서비스에서 유니버설 URI (Resource Identifier) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 합니다.
 
Xbox 360, Windows Phone 디바이스 또는 Windows를 실행 하는 게임만이 서비스를 사용할 수 있습니다.
 
이러한 Uri에 대 한 도메인 userpresence.xboxlive.com입니다.
 
실시간으로 활동 (RTA) 서비스를 사용 하 여 사용자의 현재 상태 변경에 가입할 수 있습니다.
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;사용자의 일괄 처리에 대 한 액세스 존재 합니다.

[/users/me](uri-usersme.md)

&nbsp;&nbsp;현재 사용자의 현재 상태에 액세스 합니다.

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;PresenceRecord 그룹에 액세스합니다.

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;다른 사용자 또는 클라이언트의 존재 여부에 액세스 합니다.

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;제목이 나 타이틀의 사용자의 존재 여부에 액세스 합니다.

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;PresenceRecord 그룹에 액세스합니다.

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;액세스 그룹 모니커에 지정 된 브로드캐스트 사용자의 존재 기록 URI에 표시 되는 XUID와 관련이 있습니다.

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;액세스 그룹 모니커에 지정 된 브로드캐스트 사용자 수가 URI에 표시 되는 XUID와 관련이 있습니다.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   