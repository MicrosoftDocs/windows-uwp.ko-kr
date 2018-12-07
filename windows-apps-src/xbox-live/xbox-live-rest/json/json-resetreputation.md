---
title: ResetReputation(JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
description: " ResetReputation(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d09c8bbc1130f91dfea3d4c35e391dcf9adcf127
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "8747408"
---
# <a name="resetreputation-json"></a>ResetReputation(JSON)
사용자의 기존 점수를 변경 해야 하는 새 기본 평판 점수를 포함 되어 있습니다. 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
ResetReputation 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| fairplayReputation| 숫자| 원하는 Fairplay 평판 점수 (유효한 범위 0 ~ 75) 사용자에 대 한 새로운 기본 합니다.| 
| commsReputation| 숫자| 원하는 사용자 (유효한 범위 0 ~ 75)에 대 한 의사 소통 평판 점수 새로운 기본 합니다.| 
| userContentReputation| 숫자| 원하는 UserContent 평판 점수 (유효한 범위 0 ~ 75) 사용자에 대 한 새로운 기본 합니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

   