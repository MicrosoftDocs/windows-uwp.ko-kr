---
title: Achievement(JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Achievement(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0e20f46a0d97cba496df5c6fb9cda14fbeccccd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628478"
---
# <a name="achievement-json"></a>Achievement(JSON)
도전 과제 개체 (버전 2)입니다.
<a id="ID4EN"></a>


## <a name="achievement"></a>도전 과제

도전 과제 개체에 다음과 같이 지정 합니다. 모든 멤버는 필수입니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| id| 문자열| 리소스 식별자입니다.|
| serviceConfigId| 문자열| 이 리소스에 대 한 서비스를 안내 합니다. 이 도전 과제는 관련이 제목을 식별 합니다. |
| name| 문자열| 지역화 된 도전 과제 이름입니다.|
| titleAssociations| 배열을 [TitleAssociation](json-titleassociation.md)| 배열 TitleAssociation입니다.|
| progressState| **ProgressState** 열거형| 진행 중 상태: <ul><li>잘못 된 (0): 도전 과제를 진행 하는 알 수 없는 상태입니다.</li><li>(1)이 지원 됩니다. 도전 과제를 잠금 해제 되었습니다.</li><li>inProgress (2): 도전 과제 잠겨 되지만 사용자가 잠금을 해제 하는 진행률입니다.</li><li>notStarted (3): 도전 과제 잠겨 및 사용자가 잠금을 해제에 대 한 모든 진행률 내용이 없는 합니다.</li></ul> | 
| 진행| [진행](json-progression.md)| 도전 과제 내에서 사용자의 진행 합니다.|
| mediaAssets| 배열을 [MediaAsset](json-mediaasset.md)| 이미지 Id와 같은 도전 과제와 연결 된 미디어 자산입니다. |
| 플랫폼| 문자열| 플랫폼 도전 과제에 획득 했습니다.|
| isSecret| 부울 값| 여부 도전 과제는 비밀입니다.|
| description| 문자열| 잠금 해제 하는 경우 성과의 설명입니다.|
| lockedDescription| 문자열| 잠긴 없는 상태가 되기 전에 성과의 설명입니다.|
| productId| 문자열| ProductId 도전 과제와 릴리스 되었습니다.|
| achievementType| **AchievementType** 열거형| 유형 (같지 레거시 성과에 이전 형식으로) 도전 과제입니다. <ul><li>잘못 된 (0): 알 수 없는 및 지원 되지 않는 도전 과제 형식입니다.</li><li>영구 (1): 성과 종료 날짜가 없습니다를 언제 든 지 잠금을 해제할 수 있습니다.</li><li>과제 (2): 특정 시간 범위 동안 해당 잠금을 해제할 수 있는 성과입니다.</li></ul> |
| participationType| **ParticipationType** 열거형| 도전 과제에 대 한 참여 형식입니다. 유효한 값은 개별 또는 그룹입니다.|
| timeWindow| TimeWindow| 이때 도전 과제 않을 잠금 기간입니다. 문제에 대해서만 지원 합니다.|
| 보상| 배열을 [보상](json-reward.md)| 획득 한 잠금 해제 하는 경우 보상의 컬렉션입니다.|
| estimatedTime| TimeSpan| 예상된 시간 도전 과제를 획득 이동 합니다.|
| 딥 링크| 문자열| 제목에 딥 링크를 합니다.|
| isRevoked| 부울 값| 여부 도전 과제 적용 하 여 해지 됩니다.|

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
                "url":"https://www.xbox.com"
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


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)
