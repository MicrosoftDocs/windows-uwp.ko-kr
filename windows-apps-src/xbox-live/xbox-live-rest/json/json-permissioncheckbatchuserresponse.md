---
title: PermissionCheckBatchUserResponse(JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
author: KevinAsgari
description: " PermissionCheckBatchUserResponse(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 36726153d1364f384358471324452422f67741d2
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4351651"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse(JSON)
단일 대상 사용자에 대 한 권한 값의 목록 일괄 처리의 이유를 확인합니다. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
PermissionCheckBatchUserResponse 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 사용자| string| 필수. 이 멤버는 요청 하는 사용자가 대상 사용자와 요청 된 작업을 수행할 수 있으면 <b>true</b> 입니다.| 
| Permissions| [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md) 의 배열| 필수. [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md) 요청에서와 동일한 순서로 원래 요청에 대 한 요청 된 각 권한에 대 한 합니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "User": {"Xuid": "12345"},
    "Permissions":
    [
        {
            "isAllowed": true
        },
        {
            "isAllowed": false
        },
        {
            "isAllowed": false,
            "reasons":
            [
                {"reason": "BlockedByRequestor"},
                {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
            ]
        }
    ]
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   