---
title: ServiceErrorResponse(JSON)
assetID: a2077df8-f76c-0233-8e41-68267b681862
permalink: en-us/docs/xboxlive/rest/json-serviceerrorresponse.html
description: " ServiceErrorResponse(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86f9389f6f76c1c51955a6c784393e9b05909298
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8874616"
---
# <a name="serviceerrorresponse-json"></a>ServiceErrorResponse(JSON)
서비스 오류가 발생 하는 경우 적절 한 HTTP 오류 코드 반환 됩니다. 필요에 따라 서비스 아래에 정의 된 대로 ServiceErrorResponse 개체를 포함할 수도 있습니다. 프로덕션 환경에서 더 적은 데이터가 포함 될 수 있습니다. 
<a id="ID4EN"></a>

 
## <a name="serviceerrorresponse"></a>ServiceErrorResponse
 
ServiceErrorResponse 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>오류 코드</b>| 32 비트 부호 있는 정수| (Null 일 수) 오류와 관련 된 코드입니다.| 
| <b>errorMessage</b>| string| 오류에 대 한 추가 세부 정보입니다.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
   "errorCode": 8377,
   "errorMessage": "XUID specified in the claim does not match URI XUID."
 }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   