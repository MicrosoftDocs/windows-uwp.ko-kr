---
title: /users/xuid({xuid})/outbox
assetID: 0b66b885-15ff-be55-f8be-e6e9d85d087e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutbox.html
description: " /users/xuid({xuid})/outbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 88f3f3753aeac99db0a8a53e0a2ddde21d034ac5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607548"
---
# <a name="usersxuidxuidoutbox"></a>/users/xuid({xuid})/outbox
사용자에 대 한 송신 전용 액세스의 메시징을 제공 Xbox LIVE 서비스에 대 한 발신 함. 이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid | 부호 없는 64 비트 정수 | Xbox 사용자 ID (XUID)은 요청을 수행 하는 플레이어입니다. | 
  
<a id="ID4EXB"></a>

 
## <a name="valid-methods"></a>올바른 메서드 

[POST (/users/xuid({xuid})/outbox)](uri-usersxuidoutboxpost.md)

&nbsp;&nbsp;받는 사람 목록에 지정 된 메시지를 보냅니다. 
 
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent  

[사용자가 Uri](atoc-reference-users.md)

   