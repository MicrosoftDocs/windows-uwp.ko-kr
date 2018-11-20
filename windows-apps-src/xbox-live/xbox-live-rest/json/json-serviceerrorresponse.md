---
title: ServiceErrorResponse(JSON)
assetID: a2077df8-f76c-0233-8e41-68267b681862
permalink: en-us/docs/xboxlive/rest/json-serviceerrorresponse.html
author: KevinAsgari
description: " ServiceErrorResponse(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4da4a36bca0cad761ef4dda89f86b23f6cf44c30
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7432351"
---
# <a name="serviceerrorresponse-json"></a>ServiceErrorResponse(JSON)
서비스 오류가 발생할 때 적절 한 HTTP 오류 코드 반환 됩니다. 필요에 따라 서비스 아래에 정의 된 대로 ServiceErrorResponse 개체를 포함할 수도 있습니다. 프로덕션 환경에서 더 적은 데이터가 포함 될 수 있습니다. 
<a id="ID4EN"></a>

 
## <a name="serviceerrorresponse"></a>ServiceErrorResponse
 
ServiceErrorResponse 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>errorCode</b>| 32 비트 부호 있는 정수| (Null 일 수) 오류와 관련 된 코드입니다.| 
| <b>errorMessage</b>| string| 오류에 대 한 추가 세부 정보| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

   