---
title: Property(JSON)
assetID: 93de547e-d936-6fcc-92cb-e4dd284dd609
permalink: en-us/docs/xboxlive/rest/json-property.html
description: " Property(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e2a721886509c49c60d663d491f8d49bc3c95e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659708"
---
# <a name="property-json"></a>Property(JSON)
매 치메이 킹 요청 조건에 대 한 클라이언트에서 제공 하는 속성 데이터를 포함 합니다.
<a id="ID4EN"></a>


## <a name="property"></a>속성

속성 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| id| 문자열| 이 속성에 대 한 id입니다.|
| 유형| 32 비트 부호 있는 정수 | 속성의 형식입니다. 지원 되는 값은 <ul><li>0 = 정수</li><li>1 = 문자열</li></ul>| 
| value| 문자열| 이 속성의 값입니다.|

<a id="ID4EGC"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


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


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)
