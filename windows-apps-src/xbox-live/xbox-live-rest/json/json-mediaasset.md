---
title: MediaAsset(JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
description: " MediaAsset(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308a65b7c049a6aba0405865bab63fb9d28b8506
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623478"
---
# <a name="mediaasset-json"></a>MediaAsset(JSON)
도전 과제 또는 보상은 있다와 연결 된 미디어 자산입니다.
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

MediaAsset 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| name| 문자열| "Tile01" 등의 MediaAsset의 이름입니다.|
| 유형| MediaAssetType 열거형| 미디어 자산 유형: <ul><li>아이콘 (0): 도전 과제는 아이콘입니다.</li><li>아트 (1): 디지털 아트 자산입니다.</li></ul> | 
| url| 문자열| URL은 MediaAsset입니다.|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"https://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)
