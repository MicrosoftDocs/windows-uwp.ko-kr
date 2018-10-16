---
title: GameResult(JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
author: KevinAsgari
description: " GameResult(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24f5af1639f5348fe20c36c56c1301f723d832f7
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4688217"
---
# <a name="gameresult-json"></a>GameResult(JSON)
게임 세션의 결과 설명 하는 데이터를 표시 하는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
GameResult JSON 개체에는 다음 멤버가 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| blob| 8 비트 부호 없는 정수로의 배열| 사용자 지정 제목 관련 결과 데이터입니다.| 
| 결과| string| 게임 세션에서 플레이어의 참여의 결과입니다. 유효한 값은 "성공", "손실" 또는 "연결". | 
| 점수| 64 비트 부호 있는 정수| 플레이어가 게임 세션에서 수신 하는 점수입니다.| 
| 시간| 64 비트 부호 있는 정수| 게임 세션에 대 한 플레이어의 시간입니다.| 
| xuid| 64 비트 부호 없는 정수| 결과 적용할 고객만 플레이어의 Xbox 사용자 ID입니다.| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

   