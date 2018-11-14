---
title: DeviceRecord(JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
author: KevinAsgari
description: " DeviceRecord(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ecfaf3ff6ee83c2d577614dc613939669ef60d62
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6255186"
---
# <a name="devicerecord-json"></a>DeviceRecord(JSON)
형식 및 활성화 타이틀을 포함 하는 장치에 대 한 정보를 제공 합니다. 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
DeviceRecord 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 유형| 문자열| 디바이스의 장치 유형입니다. 예를 들어 "D", "Xbox360", "MoLIVE" (Windows), "WindowsPhone", "WindowsPhone7" 및 "PC" (G4WL). 유형 (예를 들어 iOS, Android 또는 웹 브라우저에 포함 된 제목) 알 수 없는 경우 "웹" 반환 됩니다.| 
| 제목| [TitleRecord](json-titlerecord.md) 의 배열| 이 장치에서 활성 책의 목록입니다.| 
  
<a id="ID4EWB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
[{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  }]
    
```

  
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ENC"></a>

 
##### <a name="reference"></a>참조   