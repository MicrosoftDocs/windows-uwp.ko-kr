---
title: PermissionCheckResult(JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
author: KevinAsgari
description: " PermissionCheckResult(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308301b41b407291ffad74337172c5be8f4d2c59
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4540321"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult(JSON)
단일 대상 사용자에 대해 하나의 권한 설정에 대 한 명의 사용자가 확인의 결과입니다. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
PermissionCheckResult 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 이유| string| 선택 사항입니다. 권한을 거부 된 이유를 나타내는 <b>PermissionResultCode</b> 값 <b>IsAllowed</b> false 되었으면 합니다.| 
| restrictedSetting| string| 선택 사항입니다. <b>이유</b> 멤버의 <b>PermissionResultCode</b> 값 요청자에 대 한 권한 확인 실패 했음을 나타냅니다, 어떤 권한 못했습니다 나타냅니다.| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
    "reason": "MissingPrivilege",
    "restrictedSetting": "VideoCommunications"
}
    
```

  
<a id="ID4EIC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EKC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   