---
title: PermissionCheckBatchResponse(JSON)
assetID: f157ed8d-7483-1b34-bc3d-e3fcf6a7d055
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchresponse.html
author: KevinAsgari
description: " PermissionCheckBatchResponse(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9bc3d574274bdef6d0033e9f5313457706dd509e
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4425503"
---
# <a name="permissioncheckbatchresponse-json"></a>PermissionCheckBatchResponse(JSON)
여러 사용자에 대 한 권한 값의 목록에 대 한 권한 일괄 처리의 결과 확인합니다. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchresponse"></a>PermissionCheckBatchResponse
 
PermissionCheckBatchResponse 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 응답| [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md) 의 배열| 필수. 요청에서와 동일한 순서로 원래 요청에 대 한 요청 된 각 권한에 대 한 [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md) 개체입니다.| 
  
<a id="ID4EQB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRestrictsTarget", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}
    
```

  
<a id="ID4EZB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E2B"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   