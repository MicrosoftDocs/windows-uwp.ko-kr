---
title: ServiceError(JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
author: KevinAsgari
description: " ServiceError(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9f9f5cb3f4dc0565cafc073cea35e3e6e00d273f
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4340221"
---
# <a name="serviceerror-json"></a>ServiceError(JSON)
서비스에 대 한 호출에 실패 한 경우 반환 된 오류에 대 한 정보를 포함 합니다. 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
ServiceError 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| code| 32 비트 부호 있는 정수 | 오류 유형입니다. 가능한 값 아래 표를 참조 하세요. | 
| 원본| string | 오류가 발생 하는 서비스의 이름입니다. 예를 들어, 값 <code>ReputationFD</code> 신뢰도 서비스에 오류가 발생 했음을 나타냅니다. | 
| description| string| 오류에 대 한 설명입니다. | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>오류 코드
 
| 값| 설명| 
| --- | --- | --- | --- | --- | 
| 0| 성공 오류 없음| 
| 4000| 잘못 된 요청 본문 JSON 문서 POST 요청이 실패 했습니다 유효성 검사를 사용 하 여 제출 합니다. 자세한 내용은 설명 필드를 참조 하세요. | 
| 4100| 사용자는 하지 존재 The XUID 요청 URI에에서 포함 된 XBOX Live에 유효한 사용자를 표시 하지 않습니다.| 
| 4500| 권한 부여 오류 호출자는 요청 된 작업을 수행 하도록 권한이 없습니다.| 
| 5000| 서비스 오류는 내부 오류가 서비스| 
| 5300| 서비스 사용할 수 없는 서비스 제공 되지 않습니다.| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
    
```

  
<a id="ID4EZE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E2E"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   