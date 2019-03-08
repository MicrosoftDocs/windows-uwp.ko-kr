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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654128"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest(JSON)
다양 한 상태 정보를 사용 해야 하는 방법에 대 한 정보에 대 한 요청입니다. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| id| 문자열| 합니다 <b>friendlyName</b> 다기능 현재 상태 문자열을 사용 합니다.| 
| scid| 문자열| 다기능 현재 상태 문자열은 정의 되어 있는 알려 주는 서비스 안내 합니다.| 
| 매개 변수| 문자열의 배열| 배열을 <b>friendlyName</b> 하는 문자열을 다기능 현재 상태를 완료 하는 데 사용할 문자열입니다. 열거형에 게 친숙 한 이름만 지정할 수 없습니다 통계. 이 비워 두면 이전 값을 제거 합니다.| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   