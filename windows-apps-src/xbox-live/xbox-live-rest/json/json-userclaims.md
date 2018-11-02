---
title: UserClaims(JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
author: KevinAsgari
description: " UserClaims(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 718c5847a1588e492c5d290d042dd4d5a9ca4283
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5942238"
---
# <a name="userclaims-json"></a>UserClaims(JSON)
현재 인증 된 사용자에 대 한 정보를 반환합니다. 
<a id="ID4EN"></a>

 
## <a name="userclaims"></a>UserClaims
 
UserClaims 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 게이머 태그| string| 사용자의 게이머 합니다.| 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 사용자의 합니다.| 
  
<a id="ID4EZB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
   "xuid" : 2533274790412952,
   "gamertag" : "gamer"

}
    
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   