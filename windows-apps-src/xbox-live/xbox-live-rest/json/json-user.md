---
title: User(JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
description: " User(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5c1b3a34ef329696d51e615dd79d57783a132d05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641358"
---
# <a name="user-json"></a>User(JSON)
사용자 순위표 데이터를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="user"></a>사용자
 
사용자 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| 게이머 태그| 문자열| 게이머의 플레이어 (최대 15 자). 클라이언트는 플레이어를 식별할 때 UI에서이 값을 사용 해야 합니다.| 
| 순위| 32 비트 부호 있는 정수| 순위표 데이터를 요청 하는 사용자를 기준으로 사용자의 순위입니다.| 
| rating| 문자열| 사용자의 등급입니다.| 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 사용자입니다.| 
  
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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   