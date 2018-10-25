---
title: XuidList(JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
author: KevinAsgari
description: " XuidList(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3853140ce5e7c3f7710f489709945fc70b6703b4
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5468955"
---
# <a name="xuidlist-json"></a>XuidList(JSON)
작업을 수행 하는 XUIDs의 목록입니다. 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
XuidList 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| xuids| 문자열의 배열| Xbox 사용자 ID (XUID) 값을 작업을 수행 해야 하거나 데이터를 반환할지 목록입니다.| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
    
```

  
<a id="ID4EVB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXB"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>참조 

[POST (/users/{ownerId}/people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   