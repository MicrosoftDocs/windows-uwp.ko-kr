---
title: LastSeenRecord(JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
author: KevinAsgari
description: " LastSeenRecord(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d4889ced5f8942c080b3336bda8c0d8d9b25af2
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5996790"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord(JSON)
시스템 사용자, 사용자가 없는 유효한 DeviceRecord 때 사용할 수 있는 마지막 본 되는 시기에 대 한 정보. 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| deviceType| string| 사용자가 마지막 표시 한 디바이스의 유형.| 
| titleId| 32 비트 부호 없는 정수| 식별자는 사용자가 마지막 현재 제목입니다.| 
| titleName| string| 이름에는 사용자가 마지막 현재 제목입니다.| 
| 타임 스탬프| DateTime| 사용자의 마지막으로 표시 된 시기를 나타내는 UTC 타임 스탬프입니다.| 
  
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