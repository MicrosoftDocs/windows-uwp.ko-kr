---
title: Property(JSON)
assetID: 93de547e-d936-6fcc-92cb-e4dd284dd609
permalink: en-us/docs/xboxlive/rest/json-property.html
author: KevinAsgari
description: " Property(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d37054d03f6ebe8299db78673dc631c9b4b4bc16
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5972779"
---
# <a name="property-json"></a>Property(JSON)
매치 메이 킹 요청 조건에 대 한 클라이언트에서 제공한 속성 데이터가 포함 됩니다.
<a id="ID4EN"></a>


## <a name="property"></a>속성

속성 개체에는 다음과 같이 지정 합니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| id| string| 이 속성에 대 한 id입니다.|
| type| 32 비트의 부호 있는 정수 | 속성의 유형입니다. 지원 되는 값은: <ul><li>0 = 정수</li><li>1 = string</li></ul>| 
| value| string| 이 속성의 값입니다.|

<a id="ID4EGC"></a>


## <a name="sample-json-syntax"></a>샘플 JSON 구문


```json
{
    "id":"1",
    "value":"20",
    "type":0
}

```


<a id="ID4EPC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ERC"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)
