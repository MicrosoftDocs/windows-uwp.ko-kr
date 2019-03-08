---
title: Reward(JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
description: " Reward(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d58d34263e7e0e90091c41c1df4fd5e078f5055
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607498"
---
# <a name="reward-json"></a>Reward(JSON)
보상 도전 과제를 사용 하 여 연결 합니다.
<a id="ID4EN"></a>


## <a name="reward"></a>보상

보상 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| name| 문자열| 보상의 사용자 이름입니다.|
| description| 문자열| 사용자 측 설명은 보상을 제공 합니다.|
| value| 문자열| 보상의 값입니다.|
| 유형| RewardType 열거형| Reward 유형: <ul><li>잘못 된 (0): 알 수 없는 및 지원 되지 않는 reward 형식이 구성 되었습니다.</li><li>게임 (1): 플레이어의 게임에 지점을 추가 하는 보상을 제공 합니다.</li><li>앱 (2): 보상 정의 되 고 타이틀에 전달 합니다.</li><li>아트 (3): 보상은 디지털 자산입니다.</li></ul> | 
| valueType| ProgressValueDataType 열거형| 값의 형식입니다. 참조 [요구 사항 (JSON)](json-requirement.md) 자세한 내용은 합니다.|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


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


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)
