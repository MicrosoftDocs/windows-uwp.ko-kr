---
title: Achievement(JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Achievement(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1734caec256feceefb4f54965d9d8e0198d714ca
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8887416"
---
# <a name="achievement-json"></a>Achievement(JSON)
도전 과제 개체 (버전 2)입니다.
<a id="ID4EN"></a>


## <a name="achievement"></a>도전 과제

도전 과제 개체에 다음과 같이 지정 합니다. 모든 멤버는 필요 합니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| id| string| 리소스 식별자입니다.|
| serviceConfigId| string| 이 리소스에 대 한 서비스 안내 합니다. 이 도전 과제와 관련 된 제목을 식별 합니다. |
| name| 문자열| 지역화 된 도전 과제 이름입니다.|
| titleAssociations| [TitleAssociation](json-titleassociation.md) 의 배열| TitleAssociation의 배열입니다.|
| progressState| **ProgressState** 열거형| 진행 상태. <ul><li>잘못 된 (0): 도전 과제 진행은 알 수 없는 상태가 됩니다.</li><li>(1)는 파트너가 달성: 도전 과제를 잠금 해제 되었습니다.</li><li>진행 중 (2): 도전 과제 잠겨 하지만 사용자가 진행률을 잠금을 해제 합니다.</li><li>(3) notStarted: 도전 과제 잠겨 하 고 사용자가 잠금을 해제에 대 한 모든 진행률 변경한 되지 않습니다.</li></ul> | 
| 진행| [진행](json-progression.md)| 도전 과제 내에서 사용자의 진행 합니다.|
| mediaAssets| [MediaAsset](json-mediaasset.md) 의 배열| 이미지 Id 같은 도전 과제와 관련 된 미디어 자산입니다. |
| 플랫폼| string| 도전 과제 플랫폼에서 발생 했습니다.|
| isSecret| 부울 값| 도전 과제가 비밀 되는지 여부입니다.|
| description| string| 설명 도전 과제를 잠금 해제 하는 경우입니다.|
| lockedDescription| string| 도전 과제 잠금이 해제 되기 전에 설명입니다.|
| productId| 문자열| ProductId 도전 과제와 함께 출시 되었습니다.|
| achievementType| **AchievementType** 열거형| 유형 도전 과제 (같지 레거시 도전 과제에 이전 형식)입니다. <ul><li>잘못 된 (0): 알 수 없는 및 지원 되지 않는 도전 과제 유형입니다.</li><li>영구 (1): 성과 없는 종료 날짜는 언제 든 지 잠금을 해제할 수 있습니다.</li><li>질문 (2): 특정 시간 창을 하는 동안이 잠금을 해제할 수 있는 성과 합니다.</li></ul> |
| participationType| **ParticipationType** 열거형| 도전 과제에 대 한 참여 유형입니다. 유효한 값은 개인 또는 그룹.|
| 시간| 시간| 기간을 하는 동안 도전 과제 되지 않을 잠금이 해제 합니다. 문제에 대해서만 지원 합니다.|
| 보상| [보상](json-reward.md) 의 배열| 컬렉션 잠금을 해제할 때 보상입니다.|
| estimatedTime| TimeSpan| 예상된 시간을 획득 하기 위해 도전 과제가 걸립니다.|
| deeplink| string| 제목에 deeplink 합니다.|
| isRevoked| 부울 값| 여부에 상관 없이 도전 과제 적용 하 여 세션이 해지 됩니다.|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


```json
{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
          "requirements":
          [{
            "id":"12345678-1234-1234-1234-123456789012",
            "current":null,
            "target":"100"
          }],
          "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"http://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }

```


<a id="ID4ERAAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ETAAC"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)
