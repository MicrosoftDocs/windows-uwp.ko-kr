---
title: /users/xuid({xuid})/achievements/{scid}/{achievementid}
assetID: 656a6d63-1a11-b0a5-63d2-2b010abd62e7
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementid.html
description: " /users/xuid({xuid})/achievements/{scid}/{achievementid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 00c577f60b67f15f75c47b5e737ca12819695110
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918764"
---
# <a name="usersxuidxuidachievementsscidachievementid"></a>/users/xuid({xuid})/achievements/{scid}/{achievementid}
도전 과제, 구성 된 메타 데이터 및 사용자 관련 데이터에 대 한 세부 정보를 반환 합니다. 

> [!NOTE] 
> 플랫폼에 대해서만 지원 합니다. 

 
이러한 Uri에 대 한 도메인은 `achievements.xboxlive.com`.
 
  * [URI 매개 변수](#ID4E2)
 
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID)의 리소스에 액세스 하 고 사용자의 합니다. 인증된 된 사용자의 XUID 일치 해야 합니다.| 
| 서비스 안내| GUID| 해당 도전 과제를 액세스 하는 서비스 구성의 고유 식별자입니다.| 
| achievementid| 32 비트 부호 없는 정수| 액세스 하는 도전 과제의 (내에서 지정 된 서비스 안내) 고유 식별자입니다.| 
  
<a id="ID4EMC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})](uri-usersxuidachievementsscidachievementidget.md)

&nbsp;&nbsp;도전 과제의 세부 정보를 가져옵니다.
 
<a id="ID4EWC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EYC"></a>

 
##### <a name="parent"></a>부모 

[도전 과제 URI](atoc-reference-achievementsv2.md)

   