---
title: ServiceErrorResponse(JSON)
assetID: a2077df8-f76c-0233-8e41-68267b681862
permalink: en-us/docs/xboxlive/rest/json-serviceerrorresponse.html
author: KevinAsgari
description: " ServiceErrorResponse(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f0eed745b9350bd1bc2f4860cb3db5e5a6b9ad7c
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4750209"
---
# <a name="serviceerrorresponse-json"></a>ServiceErrorResponse(JSON)
서비스 오류가 발생 하는 경우 적절 한 HTTP 오류 코드 반환 됩니다. 필요에 따라 서비스 아래에 정의 된 대로 ServiceErrorResponse 개체를 포함할 수도 있습니다. 프로덕션 환경에서 더 적은 데이터가 포함 될 수 있습니다. 
<a id="ID4EN"></a>

 
## <a name="serviceerrorresponse"></a>ServiceErrorResponse
 
ServiceErrorResponse 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>오류 코드</b>| 32 비트 부호 있는 정수| (Null 일 수) 오류와 관련 된 코드입니다.| 
| <b>errorMessage</b>| string| 오류에 대 한 추가 세부 정보입니다.| 
  
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

   