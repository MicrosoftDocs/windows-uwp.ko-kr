---
title: HopperStatsResults(JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
description: " HopperStatsResults(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 38e345fc20e92cdf6446c6ae1100e347fe634eff
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883294"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults(JSON)
hopper에 대 한 통계를 나타내는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
HopperStatsResults JSON 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| hopperName| string| 선택한 hopper의 이름입니다.| 
| waitTime| 32 비트 부호 있는 정수| 평균 시간 hopper (정수로 초)에 대 한 일치 합니다. | 
| 채우기| 32 비트 부호 있는 정수| 사용자는 hopper 이후의 일치 항목을 기다리는 수입니다.| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제 
 

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

   