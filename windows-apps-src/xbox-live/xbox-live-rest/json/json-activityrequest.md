---
title: ActivityRequest(JSON)
assetID: 9eb03ab7-352c-4465-ec86-d544e76f63f9
permalink: en-us/docs/xboxlive/rest/json-activityrequest.html
author: KevinAsgari
description: " ActivityRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a3d1560c7bb8c6a6eb4fe9e4786f0378d74aeca2
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4500861"
---
# <a name="activityrequest-json"></a>ActivityRequest(JSON)
하나 이상의 사용자의 다양 한 상태에 대 한 정보에 대 한 요청 합니다. 
<a id="ID4EN"></a>

 
## <a name="activityrequest"></a>ActivityRequest
 
ActivityRequest 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| richPresence| [RichPresenceRequest](json-richpresencerequest.md)| 사용 해야 하는 다양 한 상태 문자열의 이름입니다.| 
| 미디어| MediaRequest| 사용자에 대 한 미디어 정보 시청을 수신 합니다.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f"
    }
  }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   