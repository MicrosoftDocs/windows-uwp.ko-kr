---
title: MultiplayerActivityDetails(JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
description: " MultiplayerActivityDetails(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 188bcebb8d6bff879f30dcc83d7039fbcbfae0b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658108"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails(JSON)
나타내는 JSON 개체를 **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**합니다. 

> [!NOTE] 
> 이 개체는 2015 다중 접속 하 여 구현 되 고 이상 멀티 플레이 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용에 대 한 것입니다.  

 
<a id="ID4ES"></a>

  
 
MultiplayerActivityDetails JSON 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| A <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b> 세션에 대 한 식별 정보를 나타내는 개체입니다.| 
| HandleId| 64 비트 부호 없는 정수| 작업에 해당 하는 핸들 ID입니다.| 
| TitleId| 32 비트 부호 없는 정수| 활동을 조인 하기 위해 시작 되어야 하는 제목 ID입니다.| 
| 표시 여부| MultiplayerSessionVisibility| A <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b> 세션의 표시 여부 상태를 나타내는 값입니다.| 
| JoinRestriction| MultiplayerSessionJoinRestriction| A <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b> 세션에 대 한 조인 제한을 나타내는 값입니다. 이 제한은 표시 유형을 필드 "열린"로 설정 된 경우 적용 됩니다.| 
| 닫힘| 부울 값| 세션 일시적으로 닫혀에 합류 한 false이 고, 그렇지 면 true입니다.| 
| OwnerXboxUserId| 64 비트 부호 없는 정수| Xbox 활동을 소유 하는 멤버의 사용자 ID입니다.| 
| MaxMembersCount| 32 비트 부호 없는 정수| 총 슬롯의 수입니다.| 
| MembersCount| 32 비트 부호 없는 정수| 점유 슬롯의 수입니다.| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   