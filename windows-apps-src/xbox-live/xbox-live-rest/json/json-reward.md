---
title: Reward(JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
author: KevinAsgari
description: " Reward(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 41b8aafc25c3f8ae8ca677f8049235f327c0e36e
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6038139"
---
# <a name="reward-json"></a>Reward(JSON)
도전 과제와 관련 된 보상입니다.
<a id="ID4EN"></a>


## <a name="reward"></a>보너스

보상 개체에는 다음 사양을 있습니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| name| 문자열| 사용자에 게 이름은 보상입니다.|
| description| string| 사용자에 게 설명의 보상입니다.|
| value| string| 보상의 값입니다.|
| type| RewardType 열거형| 보상 유형: <ul><li>잘못 된 (0): 알 수 없는 및 지원 되지 않는 보상 형식의 구성 되었습니다.</li><li>(1): 게이머는 보상 포인트 플레이어의 게이머 점수에 추가합니다.</li><li>(2) inApp:는 보상 정의 되 고 제목에 제공 합니다.</li><li>아트 (3):는 보상 디지털 자산입니다.</li></ul> | 
| valueType| ProgressValueDataType 열거형| 값의 형식입니다. 자세한 내용은 [요구 사항 (JSON)를](json-requirement.md) 참조 하세요.|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>샘플 JSON 구문


```json
{
  "name":null,
  "description":null,
  "value":"10",
  "type":"Gamerscore",
  "valueType":"Int"
}

```


<a id="ID4EKD"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EMD"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)
