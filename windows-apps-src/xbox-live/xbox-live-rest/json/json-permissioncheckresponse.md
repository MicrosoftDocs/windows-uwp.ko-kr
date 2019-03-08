---
title: PermissionCheckResponse(JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
description: " PermissionCheckResponse(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7623a45fa21e30a015bd5c6a7c1f5add19cc189b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623368"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse(JSON)
단일 대상 사용자에 대 한 단일 사용 권한 설정에 대 한 단일 사용자 확인의 결과입니다. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
PermissionCheckResponse 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| IsAllowed| 부울 값| 필수. 이 멤버인 <b>true</b> 요청 하는 사용자는 대상 사용자를 사용 하 여 요청한 작업을 수행 하도록 허용 된 경우.| 
| 결과| 배열을 [PermissionCheckResult (JSON)](json-permissioncheckresult.md)| 선택 사항. 하는 경우 <b>IsAllowed</b> false 확인이 거부 되어 요청자에 게 관련 된 작업에 의해 사용 권한이 거부 된 이유를 나타냅니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   