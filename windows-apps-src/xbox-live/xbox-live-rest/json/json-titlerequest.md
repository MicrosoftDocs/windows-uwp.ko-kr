---
title: TitleRequest(JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
author: KevinAsgari
description: " TitleRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 98adbf3f170c679452f4a78a18097b83e93faffa
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4392944"
---
# <a name="titlerequest-json"></a>TitleRequest(JSON)
타이틀에 대 한 정보에 대 한 요청 합니다. 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
TitleRequest 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| id| 32 비트 부호 없는 정수| 제목의 식별자입니다.| 
| activity| [ActivityRequest](json-activityrequest.md)| 제목 정보를 사용할 수 있는 경우 다양 한 상태 및 미디어 정보를 포함 합니다.| 
| 상태| string| 사용자 인지 여부 활성화 합니다. 이 필드는 사용자가 비활성으로 표시 해야 합니다. 기본값은 "활성".| 
| 배치| string| 배치 모드 제목입니다. 가능한 값 "전체", "fill", "스냅" 또는 "background" 포함 됩니다. 기본값은 "전체".| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>참조 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   