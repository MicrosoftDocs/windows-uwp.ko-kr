---
title: XuidList(JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
description: " XuidList(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d8172063d40f8df77827ab845c4dfd0c0799811
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627918"
---
# <a name="xuidlist-json"></a>XuidList(JSON)
작업을 수행 하는 데 기반이 XUIDs의 목록입니다. 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
XuidList 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| xuids| 문자열의 배열| 작업을 수행 해야 하거나 데이터를 반환할 Xbox 사용자 ID (XUID) 값 목록입니다.| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>참고자료 

[POST (/users/ {ownerId} / 사용자/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   