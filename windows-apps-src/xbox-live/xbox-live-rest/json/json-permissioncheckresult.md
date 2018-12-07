---
title: PermissionCheckResult(JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
description: " PermissionCheckResult(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dc13826be1b6f81201d069f5ade7ea5ba6668cd0
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8797932"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult(JSON)
단일 대상 사용자에 대해 하나의 권한 설정에 대 한 명의 사용자가 검사의 결과입니다. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
PermissionCheckResult 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| 이유| string| 선택 사항입니다. 권한을 거부 된 이유를 나타내는 <b>PermissionResultCode</b> 값 <b>IsAllowed</b> false 되었으면 합니다.| 
| restrictedSetting| string| 선택 사항입니다. 요청자 권한 검사에 실패 한 <b>이유</b> 멤버 <b>PermissionResultCode</b> 값 나타내면 어떤 권한 실패 나타냅니다.| 
  
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

   