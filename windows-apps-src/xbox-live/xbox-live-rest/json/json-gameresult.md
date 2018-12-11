---
title: GameResult(JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
description: " GameResult(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b408b1aaae5e6f54958a016575c4a2c37765f1e9
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8895446"
---
# <a name="gameresult-json"></a>GameResult(JSON)
게임 세션의 결과 설명 하는 데이터를 나타내는 하는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
GameResult JSON 개체에는 다음 멤버가 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| blob| 8 비트 부호 없는 정수 배열| 사용자 지정 제목 관련 결과 데이터입니다.| 
| 결과| string| 게임 세션에서 플레이어의 참여의 결과입니다. 유효한 값은 "성공", "손실" 또는 "연결"입니다. | 
| 점수| 64 비트의 부호 있는 정수| 플레이어가 게임 세션에서 수신 하는 점수입니다.| 
| time| 64 비트의 부호 있는 정수| 게임 세션에 대 한 플레이어의 시간입니다.| 
| xuid| 64 비트 부호 없는 정수| 결과 적용할 고객만 플레이어의 Xbox 사용자 ID입니다.| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
   "xuid": 2533274790412952,
   "outcome": "Win",
   "score": 100
}
    
```

  
<a id="ID4EYC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E1C"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   