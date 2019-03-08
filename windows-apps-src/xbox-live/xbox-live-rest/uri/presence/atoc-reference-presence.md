---
title: 상태 URI
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
description: " 상태 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a46ecd48c2b0bf523ab234a5f20cf9ed6669e75
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632608"
---
# <a name="presence-uris"></a>상태 URI
 
이 섹션에서 Xbox Live 서비스에 대 한 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 *있는지*합니다.
 
Xbox 360, Windows Phone 장치 또는 Windows를 실행 하는 게임만이 서비스를 사용할 수 있습니다.
 
이러한 Uri는 도메인은 userpresence.xboxlive.com입니다.
 
실시간 활동 (RTA) 서비스를 사용 하 여 사용자의 현재 상태 변경에 가입할 수 있습니다.
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;사용자의 일괄 처리에 대 한 현재 상태를 액세스 합니다.

[/users/me](uri-usersme.md)

&nbsp;&nbsp;현재 사용자의 현재 상태에 액세스 합니다.

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;내 그룹에 대 한 PresenceRecord에 액세스합니다.

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;다른 사용자 또는 클라이언트의 현재 상태에 액세스 합니다.

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;제목 또는 타이틀의 사용자의 현재 상태에 액세스 합니다.

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;그룹에 대 한 PresenceRecord에 액세스합니다.

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;액세스 그룹 모니커가 지정 된 브로드캐스트 사용자의 현재 상태 레코드를 URI에 표시 되는 XUID 관련이 있습니다.

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;액세스 그룹 모니커가 지정 된 브로드캐스트 사용자 수가 URI에 표시 되는 XUID 관련이 있습니다.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)

   