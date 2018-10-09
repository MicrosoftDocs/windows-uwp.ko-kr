---
title: 데이터 형식 개요
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
author: KevinAsgari
description: " 데이터 형식 개요"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9340f4adb83932ef2c48aba271367e7faab645c3
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4419114"
---
# <a name="data-type-overview"></a>데이터 형식 개요
 
Xbox Live 서비스는 다양 한 id 및 인증 관련 된 데이터 형식 사용 합니다. 이 항목에서는 이러한 종류의 개요를 제공 합니다.
 
| 유형| 설명| 
| --- | --- | 
| 게이머 태그| 사용자에 대 한 고유한, 읽을 화면 이름입니다.| 
| 플레이어| 사용자의 XUID 및 플레이어 세션 및 사용자 지정 데이터의 작은 blob에 여전히 참여 여부 세션 (또는 "실제 사용자")에서 플레이어의 인덱스를 함께 게이머 태그를 포함 하는 JSON 개체입니다.| 
| profile| 프로필 URI 주소 및 일반적으로 사용자의 UserSettings HTTP 메서드를 통해 액세스 하지만,도 등 gamercard, 게이머 태그, XUID, 사용자에 대 한 정보를 제공 합니다.| 
| 설정| UserSettings 개체의 제목 관련 설정 중 하나입니다.| 
| UserClaims| 사용자의 XUID 및 게이머를 포함 하는 간단한 JSON 개체입니다.| 
| UserSettings| 제목 관련 설정 또는 현재 인증 된 사용자에 대 한 기본 설정의 컬렉션을 포함 하는 JSON 개체입니다. UserSettings 게임에서 활동과 관련 임의의 데이터를 포함할 수 있습니다.| 
| XUID| 사용자의 Xbox 사용자 ID, 고유한 부호 없는 정수입니다. 읽을 수 없습니다.| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>참조 [Player (JSON)](../json/json-player.md)

 [UserClaims(JSON)](../json/json-userclaims.md)

 [UserSettings(JSON)](../json/json-usersettings.md)

   