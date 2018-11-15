---
title: User(JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
author: KevinAsgari
description: " User(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 255fe2918f10bb3b941cf2023ff358c58e191cbf
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6836200"
---
# <a name="user-json"></a>User(JSON)
사용자 순위표 데이터가 들어 있습니다. 
<a id="ID4EN"></a>

 
## <a name="user"></a>사용자
 
사용자 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 게이머 태그| string| 게이머 player (최대 15 자). 플레이어를 식별할 때 클라이언트 UI에서이 값을 사용 해야 합니다.| 
| 순위| 32 비트 부호 있는 정수| 순위표 데이터를 요청 하는 사용자를 기준으로 사용자의 순위를 지정 합니다.| 
| rating| string| 사용자의 평점입니다.| 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 사용자의 합니다.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

   