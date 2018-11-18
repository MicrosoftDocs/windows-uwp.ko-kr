---
title: ResetReputation(JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
author: KevinAsgari
description: " ResetReputation(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cf6db0ec47e92023fb43c7599fac3dcc9cdd9f0a
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7157794"
---
# <a name="resetreputation-json"></a>ResetReputation(JSON)
사용자의 기존 점수 변경할 수는 새 기본 평판 점수를 포함 되어 있습니다. 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
ResetReputation 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| fairplayReputation| 숫자| 원하는 Fairplay 평판 점수 (유효한 범위 0 ~ 75) 사용자에 대 한 새로운 기본 합니다.| 
| commsReputation| 숫자| 원하는 의사 소통 평판 점수 (유효한 범위 0 ~ 75) 사용자에 대 한 새로운 기본 합니다.| 
| userContentReputation| 숫자| 원하는 UserContent 평판 점수 (유효한 범위 0 ~ 75) 사용자에 대 한 새로운 기본 합니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   