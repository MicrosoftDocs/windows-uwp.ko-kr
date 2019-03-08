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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649418"
---
# <a name="resetreputation-json"></a>ResetReputation(JSON)
기존 사용자의 점수를 변경 해야 할 새 기본 신뢰도 점수를 포함 합니다. 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
ResetReputation 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| fairplayReputation| 숫자| 원하는 사용자 (유효한 범위 0 ~ 75)에 대 한 Fairplay 신뢰도 점수 새 기본입니다.| 
| commsReputation| 숫자| 원하는 사용자 (유효한 범위 0 ~ 75)에 대 한 통신에 대 한 신뢰도 점수 새 기본입니다.| 
| userContentReputation| 숫자| 원하는 UserContent 신뢰도 점수 (유효한 범위 0 ~ 75) 사용자에 대 한 새 기본입니다.| 
  
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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   