---
title: ServiceError(JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
description: " ServiceError(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da3d682a1b66d25a12f21a93e9596d13afae7f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631708"
---
# <a name="serviceerror-json"></a>ServiceError(JSON)
서비스에 대 한 호출에 실패 한 경우 반환 된 오류에 대 한 정보를 포함 합니다. 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
ServiceError 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| code| 32 비트 부호 있는 정수 | 형식 오류입니다. 가능한 값에 대 한 아래 표를 참조 하세요. | 
| 소스| 문자열 | 오류가 발생 하는 서비스의 이름입니다. 예를 들어 값 <code>ReputationFD</code> 평판 서비스에서 오류 했음을 나타냅니다. | 
| description| 문자열| 오류 설명입니다. | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>오류 코드
 
| 값| 설명| 
| --- | --- | --- | --- | --- | 
| 0| 성공 오류 없음| 
| 4000| POST 요청이 실패 했습니다 유효성 검사를 사용 하 여 제출 된 잘못 된 요청 본문 JSON 문서입니다. 자세한 내용은 설명 필드를 참조 하세요. | 
| 4100| 사용자는 하지 존재 The XUID 요청 URI에에서 포함 된 XBOX Live의 유효한 사용자를 나타내지 않습니다.| 
| 4500| 권한 부여 오류 호출자는 요청한 작업을 수행할 권한이 없습니다.| 
| 5000| 서비스에서 내부 오류가 있습니다 오류 서비스| 
| 5300| 서비스 사용할 수 없는 서비스 제공 되지 않습니다.| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   