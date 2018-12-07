---
title: MatchTicket(JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
description: " MatchTicket(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4bc638dfe7735856295ed92f35e244213be7bc1e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8792676"
---
# <a name="matchticket-json"></a>MatchTicket(JSON)
멀티 플레이 세션 디렉터리 (MPSD)를 통해 다른 플레이어를 찾는 데 플레이어가 일치 티켓을 나타내는 JSON 개체입니다. 
<a id="ID4EN"></a>

  
 
MatchTicket JSON 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| serviceConfig| GUID| 세션에 대 한 서비스 구성 id (서비스 안내).| 
| hopperName| string| 이 티켓을 배치 해야 hopper의 이름입니다.| 
| giveUpDuration| 32 비트 부호 있는 정수| 최대 대기 시간 (초 정수 수)입니다.| 
| preserveSession| 열거형| 일치 하는 세션 세션 다시 사용 해야 하는 경우를 나타내는 값입니다. 가능한 값은 "항상" 또는 "안 함"입니다. | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b> 개체는 플레이어 또는 그룹은 현재 재생 세션에 대 한입니다. 이 멤버는 항상 필요 합니다. | 
| ticketAttributes| 개체의 배열| 플레이어에 대 한 사용자가 제공한 특성 및 티켓에 대 한 값의 컬렉션입니다.| 
| 플레이어| 개체의 배열| 사용자가 제공한 특성의 속성 모음을 사용 하 여 각 플레이어 개체의 컬렉션입니다. | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
        "serviceConfig": "07617C5B-3423-4505-B6C6-10A16E1E5DDB",
        "hopperName": "TestHopper",
        "giveUpDuration": 10,
        "preserveSession": "never",
        "ticketSessionRef": {
        "scid": "AFFEABDF-0000-0000-0000-000000000001",
        "templateName": "TestTemplate",
        "sessionName": "5E551041-0000-0000-0000-000000000001"
    },
    "ticketAttributes": {
        "desiredMap": "Test Map",
        "desiredGameType": "Test GameType"
    },
    "players": [
        {
            "xuid": 123412345123,
            "playerAttributes": {
                "skill": 5,
                "ageRange": "teenager"
            }
        },
        {
          "xuid": 123412345124,
          "playerAttributes": {
              "skill": 15,
              "ageRange": "twenty-something"
          }
        }
      ]
    }
  
    
```

  
<a id="ID4EEB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGB"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   