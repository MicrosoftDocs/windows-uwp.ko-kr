---
title: TitleRecord(JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
author: KevinAsgari
description: " TitleRecord(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4e7fb10a0f81e24215ebc24d2545f1197d4520bc
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5399152"
---
# <a name="titlerecord-json"></a>TitleRecord(JSON)
이름 및 마지막으로 수정한 타임 스탬프를 포함 하 여 타이틀에 대 한 정보를 제공 합니다. 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
TitleRecord는 DeviceRecord 또는 LastSeenRecord, 있어야 하지만 둘 다 포함 되어 있지 않을 수 있습니다.
 
TitleRecord 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| id| 32 비트 부호 없는 정수| 레코드의 TitleId 합니다.| 
| name| 문자열| 제목의 지역화 된 이름입니다.| 
| activity| [ActivityRecord](json-activityrecord.md)| 제목에 대 한 사용자 활동입니다. 깊이 "all" 인 경우에 반환 합니다.| 
| lastModified| DateTime| UTC 타임 스탬프 레코드를 마지막으로 업데이트 하는 경우입니다.| 
| 배치| string| 사용자 인터페이스 내에서 앱의 위치입니다. 예를 들어 "fill", "전체", "스냅" 또는 "background". 기본값은 앱을 배치할 수 없는 장치에 대 한 "전체".| 
| 상태| string| 상태 제목입니다. "활성" 또는 "비활성" 일 수 있습니다 (기본값). 제목 활동 및 비활성 상태에 대 한 조건에 따라 상태를 설정 합니다.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>참조 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   