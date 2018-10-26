---
title: HopperStatsResults(JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
author: KevinAsgari
description: " HopperStatsResults(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d428c08a28769dfd2b62c1730779c3442fed9e86
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5542041"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults(JSON)
hopper에 대 한 통계를 나타내는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
HopperStatsResults JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| hopperName| string| 선택한 hopper의 이름입니다.| 
| waitTime| 32 비트 부호 있는 정수| 평균 시간 hopper (정수로 초)에 대 한 일치 합니다. | 
| 채우기| 32 비트 부호 있는 정수| 사용자는 hopper 이후의 일치 항목을 기다리는 횟수입니다.| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문 
 

```json
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }
  
    
```

  
<a id="ID4EGB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIB"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>참조 

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   