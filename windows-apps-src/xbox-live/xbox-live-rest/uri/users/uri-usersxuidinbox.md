---
title: /users/xuid({xuid})/inbox
assetID: 352740c6-42e2-0000-495d-bf384dc3e941
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinbox.html
description: " /users/xuid({xuid})/inbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8ded70b32dfd291d17a43a1741b26710f681a397
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8798031"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
사용자에 대 한 액세스 메시징 받은 편지함 Xbox LIVE 서비스를 제공 합니다. 이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid | 64 비트의 부호 없는 정수 | Xbox 사용자 ID (XUID) 요청 하 고 있는 플레이어의 합니다. | 
| messageId | string [50] | 검색 되거나 삭제 되는 메시지의 ID입니다. | 
  
<a id="ID4EDC"></a>

 
## <a name="valid-methods"></a>유효한 메서드 

[GET (/users/xuid({xuid})/inbox)](uri-usersxuidinboxget.md)

&nbsp;&nbsp;서비스에서 지정된 된 수의 사용자 메시지 요약을 검색합니다. 

[DELETE (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageiddelete.md)

&nbsp;&nbsp;사용자의 받은 편지함에서 사용자가 메시지를 삭제합니다.

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;서비스에서 읽은 상태로 표시 하는 특정 사용자 메시지에 대 한 자세한 메시지 텍스트를 검색 합니다. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>부모  

[사용자 URI](atoc-reference-users.md)

   