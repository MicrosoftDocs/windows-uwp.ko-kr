---
title: PermissionCheckBatchUserResponse(JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
description: " PermissionCheckBatchUserResponse(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9e20cc195ad737a7e847a8ad41b76247220adfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628358"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse(JSON)
일괄 처리 권한의 이유는 단일 대상 사용자에 대 한 사용 권한 값 목록을 확인합니다. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
PermissionCheckBatchUserResponse 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| 사용자| 문자열| 필수. 이 멤버인 <b>true</b> 요청 하는 사용자는 대상 사용자를 사용 하 여 요청한 작업을 수행 하도록 허용 된 경우.| 
| 권한| 배열을 [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)| 필수. A [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md) 요청에서와 동일한 순서로 원래 요청에 대 한 요청 된 각 사용 권한에 대해 합니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   