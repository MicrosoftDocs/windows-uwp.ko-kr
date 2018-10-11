---
title: User(JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
author: KevinAsgari
description: " User(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7070d829000821cb48d8fcbaa4fde1d6f393b16a
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4528670"
---
# <a name="user-json"></a>User(JSON)
사용자 순위표 데이터가 들어 있습니다. 
<a id="ID4EN"></a>

 
## <a name="user"></a>사용자
 
사용자 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 게이머 태그| string| 게이머의 플레이어 (최대 15 자). 플레이어를 식별할 때 클라이언트 UI에서이 값을 사용 해야 합니다.| 
| 순위| 32 비트 부호 있는 정수| 순위표 데이터를 요청 하는 사용자를 기준으로 사용자의 순위를 지정 합니다.| 
| rating| string| 사용자의 평점입니다.| 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 사용자의 합니다.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{ 
   "gamertag":"TrueBlue402",
   "rank":2,
   "rating":"2:19:21.17",
   "xuid":1234567890123456 
}
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   