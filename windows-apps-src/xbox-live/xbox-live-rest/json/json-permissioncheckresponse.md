---
title: PermissionCheckResponse(JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
author: KevinAsgari
description: " PermissionCheckResponse(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 79ac86b1cd99b8d1a6074b6aaadc8b6a62eec6db
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7581916"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse(JSON)
단일 대상 사용자에 대해 하나의 권한 설정에 대 한 명의 사용자가 확인의 결과입니다. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
PermissionCheckResponse 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| IsAllowed| 부울 값| 필수. 이 멤버는 요청 하는 사용자가 대상 사용자와 요청 된 작업을 수행할 수 있으면 <b>true</b> 입니다.| 
| 결과| [PermissionCheckResult (JSON)](json-permissioncheckresult.md) 의 배열| 선택 사항입니다. <b>IsAllowed</b> false 되었으며 요청자에 게 관련 된 것으로 거부 되었음을 확인 하는 경우 권한을 거부 된 이유를 나타냅니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   