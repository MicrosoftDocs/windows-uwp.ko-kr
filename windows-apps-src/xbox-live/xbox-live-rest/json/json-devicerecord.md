---
title: DeviceRecord(JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
description: " DeviceRecord(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9746706c00a09cd8b64913b4ae8b5c3426551e48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646148"
---
# <a name="devicerecord-json"></a>DeviceRecord(JSON)
해당 유형 및 활성화 제목을 포함 하는 장치에 대 한 정보입니다. 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
DeviceRecord 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| 유형| 문자열| 장치의 장치 형식입니다. "D", "Xbox360", "MoLIVE" (Windows), "WindowsPhone", "WindowsPhone7" 및 "PC" (G4WL) 가능성을 포함 합니다. 형식 (예: 예제에서는 iOS, Android 또는 웹 브라우저에 포함 된 제목을) 알 수 없는 경우 "Web"이 반환 됩니다.| 
| 제목| 배열을 [TitleRecord](json-titlerecord.md)| 이 장치에서 활성 타이틀의 목록입니다.| 
  
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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ENC"></a>

 
##### <a name="reference"></a>참고자료   