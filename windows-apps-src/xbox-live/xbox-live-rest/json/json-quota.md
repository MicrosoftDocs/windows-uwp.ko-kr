---
title: quotaInfo(JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
author: KevinAsgari
description: " quotaInfo(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6cb2283f5214d1d25e1aa0e8bcba17f4c51019fd
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5811381"
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
 
다음 예제는 글로벌 저장소에 대 한 응답을 보여줍니다.
 

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

   