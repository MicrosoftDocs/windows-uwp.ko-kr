---
title: LastSeenRecord(JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
author: KevinAsgari
description: " LastSeenRecord(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: de0ffd7c9c6c42f2a0ebf633ebcbba8a89a1b8b8
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5440501"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord(JSON)
마지막으로 본 사용자가 없는 유효한 DeviceRecord 때 사용할 수 있는 사용자를 때 시스템에 대 한 정보입니다. 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| deviceType| string| 형식에는 사용자가 마지막 현재 장치입니다.| 
| titleId| 32 비트 부호 없는 정수| 식별자는 사용자가 마지막 현재 제목입니다.| 
| titleName| string| 이름에는 사용자가 마지막 현재 제목입니다.| 
| 타임 스탬프| DateTime| 사용자의 마지막 표시 된 시기를 나타내는 UTC 타임 스탬프입니다.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
  deviceType:W8,    
  titleId:"23452345",
  titleName:"My Awesome Game",
  timestamp:"2012-09-17T07:15:23.4930000"
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>참조   