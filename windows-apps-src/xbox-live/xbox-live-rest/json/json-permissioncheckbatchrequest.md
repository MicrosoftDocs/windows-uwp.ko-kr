---
title: PermissionCheckBatchRequest(JSON)
assetID: 3100b17c-1b60-fdf2-3af9-7e424f1903ee
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchrequest.html
author: KevinAsgari
description: " PermissionCheckBatchRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a3b7f63e7a991420ee594cf45acf2f9952bcf74
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4752565"
---
# <a name="permissioncheckbatchrequest-json"></a>PermissionCheckBatchRequest(JSON)
PermissionCheckBatchRequest 개체의 컬렉션입니다. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckbatchrequest"></a>PermissionCheckBatchRequest
 
PermissionCheckBatchRequest 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 사용자| 사용자의 배열| 필수. 대상에 대해 사용 권한을 확인의 배열입니다. 이 배열에서 각 항목은 Xbox 사용자 ID (XUID) 또는 네트워크를 통한 시나리오에 대 한 익명 네트워크 외부 사용자 ("anonymousUser": "allUsers"). | 
| 사용 권한| [PermissionId 열거형](../enums/privacy-enum-permissionid.md) 의 배열| 필수. 각 사용자에 대해 확인할 수 있는 권한을 부여 합니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   