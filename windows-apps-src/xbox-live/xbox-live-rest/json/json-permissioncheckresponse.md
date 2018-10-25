---
title: PermissionCheckResponse(JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
author: KevinAsgari
description: " PermissionCheckResponse(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f04da7d375b404a5d05dcf5d5e9905b00b7545d9
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5467987"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse(JSON)
단일 대상 사용자에 대해 하나의 권한 설정에 대 한 명의 사용자가 확인의 결과입니다. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
PermissionCheckResponse 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| IsAllowed| 부울 값| 필수. 이 멤버는 요청 하는 사용자가 대상 사용자를 사용 하 여 요청한 작업을 수행할 수 있으면 <b>true</b> 입니다.| 
| 결과| [PermissionCheckResult (JSON)](json-permissioncheckresult.md) 의 배열| 선택 사항입니다. <b>IsAllowed</b> false가 확인 요청 자가 관련 된 항목에 의해 거부 되었습니다 권한이 거부 되었습니다 이유 나타냅니다.| 
  
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

   