---
title: Progression(JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
description: " Progression(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dda124e5be9a4d21a1ee5b9d6130290207e31921
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8340394"
---
# <a name="progression-json"></a>Progression(JSON)
도전 과제를 잠금 해제 하는까지 사용자의 작업 진행 합니다. 
<a id="ID4EN"></a>

 
## <a name="progression"></a>진행
 
진행은 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 요구 사항| 배열 요구 사항| 도전 과제를 획득을 위한 요구 사항 및 잠금을 해제 하는 방향으로 사용자가 얼마나 진행 합니다.| 
| timeUnlocked| DateTime| 먼저 도전 과제를 잠금 시간입니다.| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

   