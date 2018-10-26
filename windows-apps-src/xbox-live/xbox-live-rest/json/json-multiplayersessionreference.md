---
title: MultiplayerSessionReference(JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
author: KevinAsgari
description: " MultiplayerSessionReference(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e54c7359456d5fb608fcdd82d0130298bb8605c8
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5561428"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference(JSON)
**MultiplayerSessionReference**나타내는 JSON 개체입니다. 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionReference JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션 식별자입니다.| 
| templateName | string | 현재 인스턴스의 세션 템플릿 이름입니다. 파트 2 세션 식별자입니다. | 
| name | 문자열 | 세션의 이름입니다. 파트 3 세션 식별자입니다. | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문 
 

```json
{
  "scid": "8d050174-412b-4d51-a29b-d55a34edfdb7",
  "templateName": "integration",
  "name": "19de0095d8bb41048f19edbbb6bc6b04"
}
  
    
```

  
<a id="ID4EJB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELB"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVB"></a>

 
##### <a name="reference"></a>참조 

[MultiplayerSession(JSON)](json-multiplayersession.md)

   