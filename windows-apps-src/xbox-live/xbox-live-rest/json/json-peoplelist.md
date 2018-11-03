---
title: PeopleList(JSON)
assetID: ac538652-c10c-44e5-c1e3-5314ebe8ba83
permalink: en-us/docs/xboxlive/rest/json-peoplelist.html
author: KevinAsgari
description: " PeopleList(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 261ab440091ce3825c1025e2831b054807910e94
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5995963"
---
# <a name="peoplelist-json"></a>PeopleList(JSON)
[사용자](json-person.md) 개체의 컬렉션입니다. 
<a id="ID4ER"></a>

 
## <a name="peoplelist"></a>PeopleList
 
PeopleList 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 피플| [사용자](json-person.md) 의 배열| 사용자가 목록을 구성 하는 [사람](json-person.md) 개체입니다.| 
| totalCount| 32 비트 부호 없는 정수| 설정에서 사용할 수 있는 [사용자](json-person.md) 개체의 총 수입니다. 이 값의 전체 집합을 가장 최근 응답 뿐만 아니라 크기 나타내므로 페이징 클라이언트에서 사용할 수 있습니다. 예제 값: 680 합니다.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVC"></a>

 
##### <a name="reference"></a>참조 

[GET (/users/{ownerId}/people)](../uri/people/uri-usersowneridpeopleget.md)

 [POST (/users/{ownerId}/people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   