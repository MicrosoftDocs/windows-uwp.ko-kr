---
title: RichPresenceRequest(JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
description: " RichPresenceRequest(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c49da63ecd091a886a68f508af09e33fb9c58ac
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8343813"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest(JSON)
다양 한 상태 정보를 사용 해야 하는 방법에 대 한 정보에 대 한 요청 합니다. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| id| string| 사용 하는 다양 한 상태 문자열의 <b>friendlyName</b> 합니다.| 
| 서비스 안내| string| 다양 한 상태 문자열 정의 되어 있는 알려주는 서비스 안내 합니다.| 
| 매개 변수| 문자열의 배열| 다양 한 상태 문자열을 완료 하는 데 사용할 <b>friendlyName</b> 문자열의 배열입니다. 열거형 친화적인 이름만 지정 해야 합니다 통계 되지 않습니다. 이 비워 두면 이전 값을 제거 합니다.| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   