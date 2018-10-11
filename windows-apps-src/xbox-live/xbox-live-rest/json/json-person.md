---
title: Person(JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
author: KevinAsgari
description: " Person(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e8e4ac4e91c4359ca20822297ccb625d09e3d59
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4506550"
---
# <a name="person-json"></a>Person(JSON)
사용자가 시스템에서 한 사람에 대 한 메타 데이터입니다. 
<a id="ID4EN"></a>

 
## <a name="person"></a>사람
 
사용자 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| 필수. Xbox 사용자 ID (XUID) 10 진수 형식의 합니다. 예제 값: 2603643534573573 합니다.| 
| isFavorite| 부울 값| 필수. 이 사용자 인지 자세히에 대 한 사용자가 더 관심이 있는 합니다. 사용자가 자신의 사용자 목록에서 매우 많은 사람을 가질 수, 있으므로 즐겨 찾는 사용자를 환경에 우선 순위를 지정 하 고 즐겨찾기 하지 않은 다른 하기 전에 표시 해야 합니다.| 
| isFollowingCaller| 부울 값| 선택 사항입니다. 이 사용자가 사용자에 따라 여부 대신 API 호출이 발생 합니다.| 
| socialNetworks| 문자열의 배열| 선택 사항입니다. 외부 네트워크 내에서 사용자와이 사용자는 관계를 맺어야 합니다.| 
  
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

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>참조 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   