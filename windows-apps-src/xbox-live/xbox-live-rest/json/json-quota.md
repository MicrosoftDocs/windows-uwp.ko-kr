---
title: quotaInfo(JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
author: KevinAsgari
description: " quotaInfo(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4308c148a530233e06d666da5ec446821ba6ee26
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4461178"
---
# <a name="quotainfo-json"></a>quotaInfo(JSON)
제목 그룹에 대 한 할당량 정보를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
QuotaInfo 개체에는 다음 사양에 있습니다.
 
글로벌 저장소에 대 한
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| quotaBytes| 32 비트 부호 있는 정수 | 바이트 제목에 사용할 수 있는 최대 수입니다.| 
| usedBytes| 32 비트 부호 있는 정수 | 제목에 사용 된 바이트 수입니다.| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 
다음 예제에서는 글로벌 저장소에 대 한 응답을 보여 줍니다.
 

```json
{
    "quotaInfo":
    {
        "usedBytes":4194304,
        "quotaBytes":536870912
    }
}
      
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   