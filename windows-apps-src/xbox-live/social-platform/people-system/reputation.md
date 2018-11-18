---
title: 평판
author: KevinAsgari
description: 어떻게 Xbox Live 양수 게임 플레이 추구 하기 평판 서비스 사용에 대해 알아봅니다.
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 평판, 소셜 플랫폼
ms.localizationpriority: medium
ms.openlocfilehash: 5b3ff2d546dc142331406f198848e9d7a7077b3d
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7156346"
---
# <a name="reputation"></a>평판

평판 다른와 마찬가지로 사용자 통계 이며 모든 사용자의 통계를 검색 하 고 보고 및 추적 사용에 대 한 호출에 포함 됩니다. 자체 평판 **평판 클래스**에 의해 표시 됩니다. **ReputationService 클래스**평판 서비스를 나타냅니다. 해당 Uri **평판 Uri**에 설명 되어 있습니다.

> [!IMPORTANT]
> 평판 통계는 특정 제목에 연결 되지 않은 서비스 간에 전역입니다. 서비스에 대 한 서비스 구성 ID (서비스 안내)는 평판 통계를 액세스 하는 데 글로벌 서비스 안내 합니다.


## <a name="features-of-the-reputation-service"></a>신뢰도 서비스의 기능

신뢰도 서비스:

-   피드백 및 불평 동일한 방식으로 처리합니다. 엔터티는 사용자에 대 한 피드백을 제출 하 고 사용자의 평판이이 피드백에 영향을 줍니다. 그런 다음 추가 작업을 위해 적용 팀에 피드백을 전달할 수 있습니다.
-   다른 사용자에 게 피드백을 제공할 수 있습니다. 타이틀 자동으로 피드백을 제출할 수 있습니다.
-   타이틀 API에 직접 액세스할 수 있습니다. 사용자가 내에서 그리고 게임 내에서 직접 피드백을 제출 하는 사용자가 현재 게임 영역의 컨텍스트.
-   핸들 낮은 사용자가 Xbox Live 타이틀 내와 실행할 수에 영향을 주도록 평판 합니다. 따라서 사용자가 유지 해야 자신의 신뢰도 및 동작에서 눈 더 적절 하 게 온라인 게임 중 합니다.
-   양수 피드백 뿐만 아니라 부정적인 피드백을 허용합니다. 사용자에 게 Xbox 커뮤니티 또는 타이틀의 커뮤니티의 노력에 대 한 보상을 받을 수 및 것도 제공 될 수 있습니다 특별 한 권한을 합니다.
-   단일 파생 전반적인 신뢰도, **Reputation.OverallReputation 속성**으로 표시 합니다. 다음 평판 형식에서 파생 됩니다.

    -   박람회 재생 합니다. **Reputation.FairplayReputation 속성**에 의해 표시 됩니다.
    -   통신 합니다. **Reputation.CommunicationsReputation 속성**에 의해 표시 됩니다.
    -   사용자 생성 콘텐츠 (UGC)입니다. **Reputation.UserGeneratedContentReputation 속성**에 의해 표시 됩니다.

자세한 내용은 **ResetReputation (JSON)를** 참조 하세요.


## <a name="usage-flow-for-the-reputation-service"></a>신뢰도 서비스에 대 한 사용량 흐름

신뢰도 서비스에 대 한 전반적인 흐름에는 2 단계: 보고 신뢰도 및 매치 메이 킹 평판 필터링 합니다.


## <a name="reporting-reputation"></a>신뢰도 보고합니다.

특정 사용자에 대 한 충분 한 부정적인 피드백에 보고 하는 경우 제목 (JSON 특성 OverallReputationIsBad) 해당 사용자에 대해 나쁜 평판을 나타내기 위해 **Reputation.OverallReputation 속성** 을 설정 합니다. 불만 없이 상시, 사용자의 평판 천천히 향상 됩니다 및 다시 좋은 안전도 달성 하기 위해 한 번 나쁜 평판을 사용 하 여 사용자에 대 한 것이 가능 합니다.


## <a name="reputation-filtered-matchmaking"></a>매치 메이 킹 평판 필터링

평판 필터링 매치를 지정 하 여 Xbox 요구 사항 (XR), 제목 플레이어의 **Reputation.OverallReputation 속성**을 검색 합니다. 이 값은 플레이어의 전반적인 신뢰도의 상태를 나타내는 점수입니다.

> [!NOTE]
> 타이틀 OverallReputationIsBad 특성 JSON 파일에 대 한 검색을 찾을 수 없으면 사용자에 게 좋은 평판 있는지 가정 안전 합니다. 이 사용자에 대 한 피드백에는 처리 될 때까지 새 계정을 발생할 수 있습니다. 다른 사용자의 모든 피드백을 사용 하 여 플레이어 평판 통계 및 **Reputation.OverallReputation** 속성에 대 한 값은 항상 됩니다.


## <a name="reputation-as-an-indicator-of-behavior"></a>동작의 표시기로 평판

평판 표시기 사용자 시스템에서 동작 하는 방법을 제공 합니다. 예를 들어,이 사용자 인지를 식별 플레이어? 다른 세션 멤버의 피드백 플레이어의 신뢰도 결정합니다. 각 사용자는 Xbox One에서 모든 곳에서 해당 사용자로 이동 하는 평판 점수를 갖습니다. 눈에 띄게 친구를 더 잘 작동 하는 플레이어에 피어 압력을 적용할 수 있도록 볼 수 있는 소셜 곳에 노출 됩니다. 예를 들면 다음과 같습니다.

-   각 사용자의 프로필 카드 켜져 있습니다. 시스템의 모든 사용자가 한 번의 클릭으로 사용자의 프로필 볼 수 있습니다.
-   멀티 플레이어 게임에 표시 됩니다. 사용자가 함께 재생 하는 경우 그룹의 평판 최저 신뢰도 그룹에 플레이어의 평판을 동일 하는 온라인으로 합니다. 그룹은만 유사한 신뢰도 다른 사용자와 일치 합니다. 다른 플레이어 평판을 사용 하 여 종류의 플레이어에 해당 게임 및 게임에 참가 것인지 결정을 결정 합니다.


## <a name="key-elements-of-reputation"></a>신뢰도의 핵심 요소

타이틀 경우 사용자에 도달 하지는 않습니다 Me 박람회 재생, 통신 또는 (UGC) 사용자가 생성 한 콘텐츠 범주에 대 한 수준 확인할 수 있습니다. 사용자에 도달 Me 방지 관련된 된 항목에 대 한 다음 속성 중 하나는 1로 설정 된 경우:

-   **Reputation.OverallReputation 속성**입니다. 관련된 JSON 특성 OverallReputationIsBad입니다.
-   **Reputation.FairplayReputation 속성**입니다. 관련된 JSON 특성 FairplayReputationIsBad입니다.
-   **Reputation.CommunicationsReputation 속성**입니다. 관련된 JSON 특성 CommsReputationIsBad입니다.
-   **Reputation.UserGeneratedContentReputation 속성**입니다. 관련된 JSON 특성 UserContentReputationIsBad입니다.


## <a name="good-game-reports"></a>좋은 게임 보고서

잘못 된 작업에 대 한 사용자를 보고 하는 것 외에도 사용자에 게 보낼 수도 있습니다 서로 가장 중요 한 플레이어에 대 한 응답으로 제목에서 만들 수 있는 좋은 게임 보고서.


## <a name="feedback-history"></a>피드백 기록

사용자가 마지막으로 보고 및 사용자가 어떤 시간 보고, 예를 들어, "최근에 받은 통신 스타일에 대 한 피드백."와 같은 피드백 기록 정보를 보고


## <a name="xbox-requirement-for-reputation"></a>평판에 대 한 Xbox 요구 사항

Xbox 요구 사항 (XR) 매치 메이 킹 평판으로 필터링 068 높은 신뢰도 다 낮은 신뢰도 플레이어의 분리를 해야합니다. 플레이어와 사용자 통계에 플레이어의 OverallReputationIsBad 특성의 **Reputation.OverallReputation** 값을 살펴보는 것으로 수행 됩니다.

타이틀 "OverallReputation" **UserStatisticsService.GetSingleUserStatisticsAsync 메서드 (문자열, 문자열, 문자열)** 의 *statisticName* 매개 변수로 전달 하 여 사용자의 신뢰도 검색할 수 있습니다. WinRT 메서드 호출 **가져오기 (/users/xuid({xuid})/scids/{scid}/stats)** 다음과 같은 JSON 본문에 표시 된 대로 합니다.

```json
    {
      "requestedusers": [
        "2533274792693551"
      ],
      "requestedscids": [
        {
          "scid": "7492baca-c1b4-440d-a391-b7ef364a8d40",
          "requestedstats": [
            "OverallReputationIsBad",
            "FairplayReputationIsBad",
            "CommsReputationIsBad",
            "UserContentReputationIsBad"
          ]
        }
      ]
    }
```

다른 플레이어에서 사용자의 피드백에 낮은 수준에 도달 하면 OverallReputationIsBad 사용자에 대 한 1로 설정 됩니다. 사람들이 **Reputation.OverallReputation** 는 1 누구 **OverallReputation** 1로 설정 하지 다른 사람과 일치 해야 합니다. 기본적으로 사용자가 일치 하는 항목을 입력 하는 경우 일반적으로 필요가 낮은 평판 장애인 처리 합니다. 제목은 신뢰도 낮은 플레이어와 일치 하도록 옵트인 하는 좋은 신뢰도 플레이어를 선택할 수 있습니다.

평판에 적용 되는 XRs의 최신 버전에 대 한 게임 개발자 네트워크 (GDN) [Xbox 요구 사항](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx) 참조.


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>테스트에 대 한 전반적인 나쁜 평판 사용자 만들기

테스트를 위해 제목에 대 한 구현을 필터링 일치를 테스트 하려면 매우 저하 신뢰도 사용 하 여 사용자를 설정할 수 있습니다. 사용자, 테스트 제목 또는 도구에 대 한 특정 값을 설정 하려면 호출 **POST (/users/xuid({xuid})/resetreputation)** 사용자의 특정 신뢰도 값을 설정 하는 JSON 파일을 사용 하 여 합니다. 자세한 내용은 **ResetReputation (JSON)를** 참조 하세요.

예를 들어 다음과 같은 JSON 본문 매우 낮은 값을 사용자의 박람회 플레이 신뢰도 설정합니다.

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

파트너는 일반 정품을 제외 하 고 모든 샌드박스에 대 한이 호출을 만들 수 있습니다. 사용자의 기본 평판 점수를 설정 하는이 요청 하 고 양수 피드백에 대 한 플레이어의 weightings은 모두 0으로 설정 하세요. 사용자의 실제 평판이이 호출 후에 이러한 기본 점수와 플레이어의 특사로 보너스, 플레이어의 워 보너스 이루어져 있습니다. 이 메커니즘 낮은 점수를 가진 사용자 만들고 **Reputation.OverallReputation** 일치 필터 XR에 대 한 테스트를 1로 설정 합니다. 이 항목의 "Xbox 요구 사항에 대 한 안전도" 섹션에 설명 된 대로 제목 사용자 통계 서비스에서 사용자에 대 한 사용자 평판 점수를 가져올 수 있습니다.


## <a name="resetting-a-users-reputation-to-the-defaults"></a>기본값을 사용자의 신뢰도 다시 설정

제목은 사용자에 게 좋은 평판 있는지 나타내는 OverallReputationIsBad 특성을 설정 합니다. 호출 **POST (/ 사용자/me/resetreputation)** 75을 모든 값을 설정 합니다. 단일 **/users/xuid({xuid})/deleteuserdata** 호출 여러 Xbox 사용자 Id를 다시 사용할 수 있습니다. 요청 본문은입니다.

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>타이틀에서 피드백 보내기

타이틀 일치 항목에서 플레이어에 대 한 박람회 플레이 피드백을 보낼 수 있습니다. 제목에서 직접 또는 일괄 처리의 제목 서비스에서 수행 됩니다. 제목은 **ReputationService.SubmitReputationFeedbackAsync 메서드** 또는 다음 직접 REST 메서드를 사용할 수 있습니다.

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| 클라이언트 게시물          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| POST (배치) 서비스 | https://reputation.xboxlive.com:10443/users/batchfeedback   |

허용 되는 필드에 대 한 정의 하 고 제출에 대 한 예제 JSON 본문 **피드백 (JSON)를** 참조 하세요.


## <a name="types-of-feedback-allowed"></a>허용 되는 피드백 유형

게시할 수 있는 피드백 범주의 **피드백 (JSON)** 에서 정의 됩니다.


## <a name="when-titles-should-send-feedback"></a>타이틀 피드백에 보내면 해야

게임에서 플레이어 경험에 부정적인 영향을 줍니다 이벤트 때 제목을 부정적인 피드백 보내야 합니다. 일반적으로 제목 보내야 타입 당 하나의 피드백 당 원형 재생 합니다. 예를 들어, 제목 다음과 같은 작업을 수행 해야합니다.

1.  사용자는 팀 동료 중단 될 때마다 한 이벤트를 전송 하는 대신 3 개 이상의 동료 중단 라운드 당 사람에 대 한 하나의 FairPlayKillsTeammates 피드백 항목을 보내기만 합니다.
2.  종료 될 때 사용자가 의도적으로 일치 하는 초기 FairplayQuitter 피드백 항목을 보냅니다.
3.  사용자가 레이싱 게임에서 뒤로 운전 때 FairplayUnsporting 피드백 항목 경합, 마다 한 번씩를 전송 합니다.
