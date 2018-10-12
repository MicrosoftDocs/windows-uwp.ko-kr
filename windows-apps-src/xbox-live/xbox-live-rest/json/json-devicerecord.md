---
title: DeviceRecord(JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
author: KevinAsgari
description: " DeviceRecord(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0ae61df42706ea3ff3f52678feef8510974b5534
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563766"
---
# <a name="devicerecord-json"></a>DeviceRecord(JSON)
해당 형식 및 활성화 타이틀을 포함 하 여 장치에 대 한 정보를 제공 합니다. 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
DeviceRecord 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 유형| 문자열| 장치의 장치 유형입니다. 가능성 "D", "Xbox360", "MoLIVE" (Windows), "WindowsPhone", "WindowsPhone7" 및 "PC" (G4WL)를 포함 합니다. 유형 (예를 들어 iOS, Android 또는 웹 브라우저에 포함 된 제목) 알 수 없는 경우 "웹" 반환 됩니다.| 
| 제목| [TitleRecord](json-titlerecord.md) 의 배열| 이 장치에서 활성 제목 목록입니다.| 
  
<a id="ID4EWB"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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