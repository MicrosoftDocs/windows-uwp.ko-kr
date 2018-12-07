---
title: MultiplayerSessionReference(JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
description: " MultiplayerSessionReference(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5986079e1cae3338d8cc24a9e85f6941cf4fbec4
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "8731817"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference(JSON)
**MultiplayerSessionReference**나타내는 JSON 개체입니다. 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionReference JSON 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 1 부 세션 식별자입니다.| 
| templateName | string | 현재 인스턴스의 세션 템플릿 이름입니다. 파트 2 세션 식별자입니다. | 
| name | 문자열 | 세션의 이름입니다. 3 부 세션 식별자입니다. | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제 
 

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

   