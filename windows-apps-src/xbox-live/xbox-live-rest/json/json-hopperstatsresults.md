---
title: HopperStatsResults(JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
author: KevinAsgari
description: " HopperStatsResults(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b6f1322d2c22e65c33667ea409b10d9209628d3b
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5984154"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults(JSON)
hopper에 대 한 통계를 나타내는 하는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
HopperStatsResults JSON 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| hopperName| string| 선택한 hopper의 이름입니다.| 
| waitTime| 32 비트의 부호 있는 정수| 평균 hopper (정수로 초) 시간 일치 합니다. | 
| 채우기| 32 비트의 부호 있는 정수| 사용자는 hopper 이후의 일치 항목을 기다리는 횟수입니다.| 
  
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

   