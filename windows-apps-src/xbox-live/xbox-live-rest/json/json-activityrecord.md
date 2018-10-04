---
title: ActivityRecord(JSON)
assetID: e3a7465b-3451-0266-f8ba-b7602b59f7af
permalink: en-us/docs/xboxlive/rest/json-activityrecord.html
author: KevinAsgari
description: " ActivityRecord(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0eb34d64daa9b1349c4f956a59ccf5d8efa5b565
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4340133"
---
# <a name="activityrecord-json"></a>ActivityRecord(JSON)
하나 이상의 사용자의 풍부한 존재 여부에 대 한 서식이 지정 된 및 지역화 된 문자열입니다. 
<a id="ID4EN"></a>

 
## <a name="activityrecord"></a>ActivityRecord
 
ActivityRecord 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| richPresence| string| 포맷 하 고 지역화 된 다양 한 상태 문자열입니다.| 
| 미디어| MediaRecord| 어떤 사용자가 시청 하거나를 수신 합니다.| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
        richPresence:"Team Deathmatch on Nirvana"
      }
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   