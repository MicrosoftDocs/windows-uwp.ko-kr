---
title: 데이터 형식 개요
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
description: " 데이터 형식 개요"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 62932a921d51a988a5533d7ee08f4968bb67a29d
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8782940"
---
# <a name="data-type-overview"></a>데이터 형식 개요
 
Xbox Live 서비스는 다양 한 id 및 인증 관련 된 데이터 형식 사용 합니다. 이 항목에서는 이러한 종류의 개요를 제공 합니다.
 
| 유형| 설명| 
| --- | --- | 
| 게이머 태그| 사용자에 대 한 고유 하 고 이해 하기 쉬운 화면 이름입니다.| 
| 플레이어| 사용자의 XUID 및 플레이어 세션 및 사용자 지정 데이터의 작은 blob에 계속 참여 여부 세션 (또는 "실제 사용자")에서 플레이어의 인덱스를 함께 게이머 태그를 포함 하는 JSON 개체입니다.| 
| profile| 프로필 URI 주소 및 HTTP 메서드, 일반적으로 사용자의 사용자를 통해 액세스 하지만,도 등 gamercard, 게이머 태그, XUID, 사용자에 대 한 정보를 제공 합니다.| 
| 설정| 사용자 개체의 제목 관련 설정 중 하나입니다.| 
| UserClaims| 사용자의 XUID 및 게이머 태그를 포함 하는 간단한 JSON 개체입니다.| 
| UserSettings| 제목 관련 설정 또는 현재 인증 된 사용자에 대 한 기본 설정의 컬렉션을 포함 하는 JSON 개체입니다. UserSettings, 게임 작업에 관련 된 임의 데이터를 포함할 수 있습니다.| 
| XUID| 사용자의 Xbox 사용자 ID를 고유한 부호 없는 정수입니다. 읽을 수 없습니다.| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>참조 [Player (JSON)](../json/json-player.md)

 [UserClaims(JSON)](../json/json-userclaims.md)

 [UserSettings(JSON)](../json/json-usersettings.md)

   