---
title: Progression(JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
author: KevinAsgari
description: " Progression(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5a0e534d92e4bcb77565f59de5252afcbbe3eef5
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4538768"
---
# <a name="progression-json"></a>Progression(JSON)
도전 과제를 잠금 해제까지 사용자의 작업 진행 합니다. 
<a id="ID4EN"></a>

 
## <a name="progression"></a>진행
 
진행은 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 요구 사항| 배열 요구 사항| 도전 과제를 획득을 위한 요구 사항으로 잠금을 해제는 사용자가 얼마나 진행 합니다.| 
| timeUnlocked| DateTime| 먼저 도전 과제를 잠금 시간입니다.| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
  "requirements":
  [{
    "id":"12345678-1234-1234-1234-123456789012",
    "current":null,
    "target":"100"
  }],
  "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
}
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   