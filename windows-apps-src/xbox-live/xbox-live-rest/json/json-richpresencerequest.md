---
title: RichPresenceRequest(JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
author: KevinAsgari
description: " RichPresenceRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9d1158832623b88efb0a614680f0c0fb579f79d4
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5161617"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest(JSON)
다양 한 상태 정보를 사용 해야 하는 방법에 대 한 정보를 요청 합니다. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| id| string| 사용 하는 다양 한 상태 문자열의 <b>friendlyName</b> 합니다.| 
| 서비스 안내| string| 다양 한 상태 문자열 정의 되어 있는 알려주는 서비스 안내 합니다.| 
| 매개 변수| 문자열의 배열| 다양 한 상태 문자열을 완료할 수 있는 <b>friendlyName</b> 문자열의 배열입니다. 열거형 친화적인 이름만 지정 해야 합니다 통계 되지 않습니다. 이 비워 두면 모든 이전 값을 제거 합니다.| 
  
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

   