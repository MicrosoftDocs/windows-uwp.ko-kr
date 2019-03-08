---
title: Person(JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
description: " Person(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 175d66ffc7744ca8203fe7681fcb0167e150f012
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640868"
---
# <a name="person-json"></a>Person(JSON)
사용자 시스템의 단일 사용자에 대 한 메타 데이터입니다. 
<a id="ID4EN"></a>

 
## <a name="person"></a>Person
 
사용자 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| xuid| 문자열| 필수. Xbox 사용자 ID (XUID) 10 진수 형식에서입니다. 예를 들어 값: 2603643534573573.| 
| isFavorite| 부울 값| 필수. 이 사용자 인지 여부에 관심이 있는 사용자는 하나입니다. 사용자를 가질 수 있으므로 사용자 수가 매우 많은 사용자 목록에 즐겨 찾는 사용자를 환경에 우선 순위를 지정 하 고 즐겨찾기 없는 다른 부분 보다 먼저 표시 해야 합니다.| 
| isFollowingCaller| 부울 값| 선택 사항. 이 사용자는 사용자를 따르는 여부를 대신해 API 호출이 수행 되었습니다.| 
| socialNetworks| 문자열의 배열| 선택 사항. 외부 네트워크 내에서 사용자 및이 사용자는 관계가 있습니다.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>참고자료 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   