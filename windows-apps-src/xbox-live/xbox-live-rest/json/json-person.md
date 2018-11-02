---
title: Person(JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
author: KevinAsgari
description: " Person(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 69ab54fce4631e8b1d0c5361863078d28d0cf3f2
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5933732"
---
# <a name="person-json"></a>Person(JSON)
사용자가 시스템에서 단일 사용자에 대 한 메타 데이터입니다. 
<a id="ID4EN"></a>

 
## <a name="person"></a>사람
 
사용자 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| 필수. Xbox 사용자 ID (XUID) 10 진수 형식. 예제 값: 2603643534573573 합니다.| 
| isFavorite| 부울 값| 필수. 이 사용자 인지 자세히에 대 한 사용자가 더 관심이 있는 합니다. 사용자가 자신의 사용자 목록에서 매우 많은 사람들이 가질 수, 때문에 즐겨찾기 사용자를 환경에 우선 순위를 지정 하 고 즐겨찾기 하지 않은 다른 하기 전에 표시 해야 합니다.| 
| isFollowingCaller| 부울 값| 선택 사항입니다. 이 사람이 사용자 팔 로우 여부를 대신 하 여 API 호출이.| 
| socialNetworks| 문자열의 배열| 선택 사항입니다. 외부 네트워크 내에서 사용자 및 해당이 사용자는 관계가 있습니다.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>참조 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   