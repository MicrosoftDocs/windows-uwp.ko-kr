---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
author: KevinAsgari
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1fe5e366c48dc5cc7586604584adb6af00207b85
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4427490"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
제목이 나 타이틀의 사용자의 존재 여부에 액세스 합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID)는 대상 사용자의 합니다.| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;[PresenceRecord](../../json/json-presencerecord.md) 만료 될 때까지 기다리지 않고 닫는 제목의 존재 여부를 제거 합니다.

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;사용자의 현재 상태를 사용 하 여 타이틀을 업데이트 합니다.
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>부모 

[상태 URI](atoc-reference-presence.md)

   