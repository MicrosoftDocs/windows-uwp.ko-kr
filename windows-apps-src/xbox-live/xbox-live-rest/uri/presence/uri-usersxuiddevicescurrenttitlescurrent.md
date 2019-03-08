---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0b5c17f1791d69ca8a4c902b6c37736c4da28a31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645668"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
제목 또는 타이틀의 사용자의 현재 상태에 액세스 합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 대상 사용자입니다.| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;기다리지 않고 닫는 제목의 존재를 제거 합니다 [PresenceRecord](../../json/json-presencerecord.md) 만료 합니다.

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;사용자의 현재 상태를 사용 하 여 제목을 업데이트 합니다.
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>Parent 

[현재 상태 Uri](atoc-reference-presence.md)

   