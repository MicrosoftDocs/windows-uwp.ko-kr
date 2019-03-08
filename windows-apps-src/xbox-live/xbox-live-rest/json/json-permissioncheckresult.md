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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598448"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult(JSON)
단일 대상 사용자에 대 한 단일 사용 권한 설정에 대 한 단일 사용자 확인의 결과입니다. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
PermissionCheckResult 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| reason| 문자열| 선택 사항. A <b>PermissionResultCode</b> 사용 권한이 거부 된 이유를 나타내는 값 경우 <b>IsAllowed</b> false입니다.| 
| restrictedSetting| 문자열| 선택 사항. 경우는 <b>PermissionResultCode</b> 값을 <b>이유</b> 멤버 요청자에 대 한 권한 검사를 실패 했음을 나타냅니다는 권한 오류가 합니다.| 
  
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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   