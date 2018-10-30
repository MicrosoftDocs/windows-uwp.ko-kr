---
title: RichPresenceRequest(JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
author: KevinAsgari
description: " RichPresenceRequest(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 92e52e4ebb58abb3a522f81a5ae4cce6486785f0
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5752859"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest(JSON)
다양 한 상태 정보를 사용 해야 하는 방법에 대 한 정보를 요청 합니다. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| id| string| 사용 하 여 다양 한 상태 문자열의 <b>friendlyName</b> 합니다.| 
| 서비스 안내| string| 다양 한 상태 문자열 정의 된 알려주는 서비스 안내 합니다.| 
| 매개 변수| 문자열의 배열| 다양 한 상태 문자열을 완료할 수 있는 <b>friendlyName</b> 문자열의 배열입니다. 열거형 친화적인 이름만 지정 해야 합니다 통계 되지 않습니다. 이 비워 두면 이전 값을 제거 합니다.| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   