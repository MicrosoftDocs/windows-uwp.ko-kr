---
title: quotaInfo(JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
description: " quotaInfo(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f3499fdba972d6e953813fc490d080910921698e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654118"
---
# <a name="quotainfo-json"></a>quotaInfo(JSON)
제목 그룹에 대 한 할당량 정보를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
QuotaInfo 개체 같은 사양을 갖습니다.
 
전역 저장소에 대 한
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| quotaBytes| 32 비트 부호 있는 정수 | 제목에 사용 가능한 바이트의 최대 수입니다.| 
| usedBytes| 32 비트 부호 있는 정수 | 제목에 사용 된 바이트 수입니다.| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 
다음 예제에서는 전역 저장소에 대 한 응답을 보여 줍니다.
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   