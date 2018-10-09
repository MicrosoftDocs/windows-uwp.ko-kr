---
title: MediaAsset(JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
author: KevinAsgari
description: " MediaAsset(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5a56b524dbf88d96a34f769f7a25bed7bca8a1d
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4430496"
---
# <a name="mediaasset-json"></a>MediaAsset(JSON)
도전 과제 또는 해당 보상와 관련 된 미디어 자산입니다.
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

MediaAsset 개체에는 다음 사양을 있습니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| name| 문자열| "Tile01"와 같은 MediaAsset의 이름입니다.|
| type| MediaAssetType 열거형| 미디어 자산 유형: <ul><li>아이콘 (0): 성과 아이콘입니다.</li><li>아트 (1): 디지털 아트 자산입니다.</li></ul> | 
| url| string| URL은 MediaAsset입니다.|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>샘플 JSON 구문


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"http://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EQC"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)