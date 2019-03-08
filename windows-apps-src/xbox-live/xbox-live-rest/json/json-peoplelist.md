---
title: PeopleList(JSON)
assetID: ac538652-c10c-44e5-c1e3-5314ebe8ba83
permalink: en-us/docs/xboxlive/rest/json-peoplelist.html
description: " PeopleList(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f9ab412088707752d62cc20fd54da2639f26ddc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613328"
---
# <a name="peoplelist-json"></a>PeopleList(JSON)
컬렉션인 [Person](json-person.md) 개체입니다. 
<a id="ID4ER"></a>

 
## <a name="peoplelist"></a>PeopleList
 
PeopleList 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| 피플| 배열을 [사람](json-person.md)| 합니다 [Person](json-person.md) 사람 목록 구성 하는 개체입니다.| 
| totalCount| 32 비트 부호 없는 정수| 총 수 [Person](json-person.md) 개체 집합에서 사용할 수 있습니다. 이 값은 가장 최근 응답 뿐 아니라 전체 집합의 크기를 나타내므로 페이징에 대 한 클라이언트에서 사용할 수 있습니다. 예를 들어 값: 680.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVC"></a>

 
##### <a name="reference"></a>참고자료 

[가져오기 (/users/ {ownerId} 사람 /)](../uri/people/uri-usersowneridpeopleget.md)

 [POST (/users/ {ownerId} / 사용자/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   