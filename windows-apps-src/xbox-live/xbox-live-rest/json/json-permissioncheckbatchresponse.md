---
title: PermissionCheckBatchResponse(JSON)
assetID: f157ed8d-7483-1b34-bc3d-e3fcf6a7d055
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchresponse.html
description: " PermissionCheckBatchResponse(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6ac79d3361e83993c372b1d651e97e900788d39f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613348"
---
# <a name="permissioncheckbatchresponse-json"></a>PermissionCheckBatchResponse(JSON)
일괄 처리 권한의 결과 여러 사용자에 대 한 권한 값의 목록을 확인합니다. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchresponse"></a>PermissionCheckBatchResponse
 
PermissionCheckBatchResponse 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| 응답| 배열을 [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)| 필수. A [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md) 해당 요청에서와 동일한 순서로 원래 요청에 대 한 요청 된 각 사용 권한에 대 한 개체입니다.| 
  
<a id="ID4EQB"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   