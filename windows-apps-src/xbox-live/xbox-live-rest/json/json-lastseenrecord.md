---
title: LastSeenRecord(JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
description: " LastSeenRecord(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e06de31cabaedb68ed57d3d4f2ff30614ceb6317
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933055"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord(JSON)
시스템 사용자, 사용자가 없는 유효한 DeviceRecord 때 사용할 수 있는 마지막 본 되는 시기에 대 한 정보. 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| deviceType| string| 형식에는 사용자가 마지막 현재 장치입니다.| 
| titleId| 32 비트 부호 없는 정수| 식별자는 사용자가 마지막 현재 제목입니다.| 
| titleName| string| 이름에는 사용자가 마지막 현재 제목입니다.| 
| 타임 스탬프| DateTime| 사용자의 마지막으로 표시 된 시기를 나타내는 UTC 타임 스탬프입니다.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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