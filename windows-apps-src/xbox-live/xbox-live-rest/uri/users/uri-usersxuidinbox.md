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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640898"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
사용자에 대 한 액세스의 Xbox LIVE 서비스에 대 한 받은 편지함을 메시징을 제공 합니다. 이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid | 부호 없는 64 비트 정수 | Xbox 사용자 ID (XUID)은 요청을 수행 하는 플레이어입니다. | 
| messageId | string[50] | 검색 또는 삭제 되는 메시지의 ID입니다. | 
  
<a id="ID4EDC"></a>

 
## <a name="valid-methods"></a>올바른 메서드 

[GET (/users/xuid({xuid})/inbox)](uri-usersxuidinboxget.md)

&nbsp;&nbsp;서비스에서 지정된 된 수의 사용자 메시지 요약을 검색합니다. 

[DELETE (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageiddelete.md)

&nbsp;&nbsp;사용자의 받은 편지함에서 사용자 메시지를 삭제합니다.

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;특정 사용자 메시지를 서비스에서 읽은 상태로 표시에 대 한 자세한 메시지 텍스트를 검색 합니다. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent  

[사용자가 Uri](atoc-reference-users.md)

   