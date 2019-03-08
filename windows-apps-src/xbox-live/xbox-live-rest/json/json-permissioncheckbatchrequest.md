---
title: PermissionCheckBatchRequest(JSON)
assetID: 3100b17c-1b60-fdf2-3af9-7e424f1903ee
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchrequest.html
description: " PermissionCheckBatchRequest(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 538547c85648970ab3e9fe3ae413e8a03df814ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623508"
---
# <a name="permissioncheckbatchrequest-json"></a>PermissionCheckBatchRequest(JSON)
PermissionCheckBatchRequest 개체의 컬렉션입니다. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckbatchrequest"></a>PermissionCheckBatchRequest
 
PermissionCheckBatchRequest 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| 사용자| 사용자의 배열| 필수. 배열에 대 한 사용 권한을 확인 하는 대상입니다. 이 배열의 각 항목은 Xbox 사용자 ID (XUID) 또는 교차 네트워크 시나리오에 대 한 익명 네트워크 외부 사용자 ("anonymousUser": "allUsers"). | 
| 권한| 배열을 [PermissionId 열거형](../enums/privacy-enum-permissionid.md)| 필수. 각 사용자에 대해 확인할 권한입니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ShareFriendList",
        "ShareGameHistory"
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   