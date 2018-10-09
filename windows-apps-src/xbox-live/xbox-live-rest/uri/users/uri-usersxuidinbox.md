---
title: /users/xuid({xuid})/inbox
assetID: 352740c6-42e2-0000-495d-bf384dc3e941
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinbox.html
author: KevinAsgari
description: " /users/xuid({xuid})/inbox"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 944b2c9f0e5758444295ef9ec189d84728a3845d
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4445515"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
Xbox LIVE 서비스에 대 한 사용자에 대 한 액세스 받은 편지함 메시징 제공 합니다. 이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`.
 
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

&nbsp;&nbsp;사용자의 받은 편지함에서 사용자 메시지를 삭제합니다.

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;서비스에 읽기 상태로 표시 하는 특정 사용자 메시지에 대 한 자세한 메시지 텍스트를 검색 합니다. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>부모  

[사용자 URI](atoc-reference-users.md)

   