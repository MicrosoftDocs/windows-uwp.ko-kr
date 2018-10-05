---
title: UserList(JSON)
assetID: 894f5a39-4eed-0c5b-fc45-cb0097dc46fd
permalink: en-us/docs/xboxlive/rest/json-userlist.html
author: KevinAsgari
description: " UserList(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51dd19ebed394bb0c3c8b5f4649dd5c83a58027c
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4386389"
---
# <a name="userlist-json"></a>UserList(JSON)
[사용자](json-user.md) 개체의 컬렉션입니다. 
<a id="ID4ER"></a>

 
## <a name="userlist"></a>UserList
 
UserList 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 사용자| [User (JSON)](json-user.md) 의 배열| 아래 JSON 예제를 참조 하세요.| 
  
<a id="ID4EPB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ] 
}
    
```

  
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   