---
title: MultiplayerActivityDetails(JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
author: KevinAsgari
description: " MultiplayerActivityDetails(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4de72a24c34af1a5f145c44b2acfa11a7bd07f95
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4416157"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails(JSON)
**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**나타내는 JSON 개체입니다. 

> [!NOTE] 
> 이 개체 2015 멀티 플레이어에 의해 구현 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 용도가 템플릿 계약 104/105 이상와 함께 사용 합니다.  

 
<a id="ID4ES"></a>

  
 
MultiplayerActivityDetails JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| 세션에 대 한 개인 식별 정보를 나타내는 <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b> 개체입니다.| 
| HandleId| 64 비트 부호 없는 정수| 해당 활동에 핸들 ID입니다.| 
| TitleId| 32 비트 부호 없는 정수| 해당 활동에 가입 하기 위해 시작 해야 하는 제목 ID입니다.| 
| 표시 여부| MultiplayerSessionVisibility| 세션의 표시 상태를 나타내는 <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b> 값.| 
| JoinRestriction| MultiplayerSessionJoinRestriction| 조인 제한 세션을 나타내는 <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b> 값. 이 제한 사항은 표시 여부 필드 "열기"로 설정 된 경우 적용 됩니다.| 
| 닫힘| 부울 값| 닫혀 있는 경우 세션 일시적으로 false 및 결합 하 고, 그렇지 않은 경우 true입니다.| 
| OwnerXboxUserId| 64 비트 부호 없는 정수| 해당 활동을 소유 하는 멤버의 Xbox 사용자 ID입니다.| 
| MaxMembersCount| 32 비트 부호 없는 정수| 총 슬롯의 수입니다.| 
| MembersCount| 32 비트 부호 없는 정수| 취소가 슬롯의 수입니다.| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
  "results": [{
    "id": "11111111-ebe0-42da-885f-033860a818f6",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "party",
      "name": "e3a836aeac6f4cbe9bcab985494d3175"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": true,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  },
  {
    "id": "11111111-ebe0-42da-885f-033860a818f7",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "TitleStorageTestDefault",
      "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": false,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  }]
}
    
```

  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   