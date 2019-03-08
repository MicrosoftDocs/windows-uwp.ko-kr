---
title: 평판
description: 어떻게 Xbox Live 양의 게임 플레이 장려 하기 위해 평판 서비스 사용에 대해 알아봅니다.
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 평판, 소셜 플랫폼
ms.localizationpriority: medium
ms.openlocfilehash: 4e7763294458f19509d5b797a6fa10e03678abee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641818"
---
# <a name="reputation"></a>평판

평판과 다른 마찬가지로 한 사용자 통계는 보고 및 추적에 사용 하 여 모든 사용자의 통계를 검색에 대 한 호출에 포함 됩니다. 자체 평판은 표현 합니다 **평판 클래스**합니다. 합니다 **ReputationService 클래스**평판 서비스를 나타냅니다. 해당 Uri에 설명 되어 있습니다 **평판 Uri**합니다.

> [!IMPORTANT]
> 평판 통계는 특정 타이틀에 연결 되지 않고 서비스에서 전역입니다. 서비스 서비스 (안내 ID)를 서비스에 대 한 구성은 평판 통계에 액세스 하는 데 사용 하는 전역 서비스 안내 합니다.


## <a name="features-of-the-reputation-service"></a>Reputation Service의 기능

평판 서비스:

-   동일한 방식으로 사용자 의견 및 불만 처리합니다. 엔터티는 사용자에 대 한 사용자 의견을 제출 하 고이 피드백에 사용자의 평판을 영향을 줍니다. 그런 다음 추가 작업에 대 한 적용 팀에 피드백을 전달할 수 있습니다.
-   다른 사용자에 게 피드백을 제공 하는 사용자 수 있습니다. 제목을 자동으로 피드백을 제출할 수 있습니다.
-   제목에서 API에 직접 액세스할 수 있습니다. 사용자는 게임 내 및 내에서 직접 피드백을 제출할 수 있는 사용자가 현재 게임 영역의 컨텍스트.
-   핸들에는 사용자가 Xbox Live 및 제목 내에서 실행할 수에 영향을 주도록는 평판이 낮은 합니다. 따라서 사용자가 해야 주시 평판 및 act에 보다 적절 하 게 온라인 게임 중입니다.
-   부정적인 피드백 뿐만 아니라 긍정적인 피드백을 허용합니다. Xbox 커뮤니티 또는 타이틀의 커뮤니티는 데 도움이 되는 사용자가 활동에 대 한 보상을 받을 수 있습니다 및는 지정할 수 있으며도 특별 한 권한이 있습니다.
-   단일 파생 전체 신뢰도 나타내는 합니다 **Reputation.OverallReputation 속성**합니다. 평판 형식에서 파생 됩니다.

    -   Fair 재생 합니다. 표시 된 **Reputation.FairplayReputation 속성**합니다.
    -   통신 합니다. 표시 된 **Reputation.CommunicationsReputation 속성**합니다.
    -   사용자 생성 콘텐츠 (UGC)로 지정 합니다. 표시 된 **Reputation.UserGeneratedContentReputation 속성**합니다.

참조 **ResetReputation (JSON)** 자세한 내용은 합니다.


## <a name="usage-flow-for-the-reputation-service"></a>평판 서비스에 대 한 사용 흐름

평판 서비스에 대 한 전반적인 흐름은 두 단계: reporting 평판 및 결혼 정보 회사 평판 필터링 합니다.


## <a name="reporting-reputation"></a>보고 평판

특정 사용자에 대 한 충분 한 부정적인 피드백 보고 되었습니다, 제목을 설정 합니다 **Reputation.OverallReputation 속성** (JSON 특성 OverallReputationIsBad) 해당 사용자에 대 한 잘못 된 평판을 나타내기 위해. 불만 사항 없이, 사용자의 평판 느린 개선, 및 다시 좋은 평판을 달성 하기 위해 한 번 잘못 된 평판을 사용 하 여 사용자에 대 한 것이 가능 합니다.


## <a name="reputation-filtered-matchmaking"></a>결혼 정보 회사 평판 필터링

평판 필터링 결혼 정보 회사 연결을 지정 하 여 Xbox 요구 사항 (XR), 제목 검색 플레이어 **Reputation.OverallReputation 속성**합니다. 이 값은 플레이어의 전반적인 평판의 상태를 나타내는 점수입니다.

> [!NOTE]
> 제목 OverallReputationIsBad 특성의 JSON 파일을 찾고 찾을 수 없는, 하는 경우 사용자가 좋은 평판을 가정 하에 안전 합니다. 이 값은 사용자에 대 한 피드백 처리가 될 때까지 새 계정을 사용 하 여 발생할 수 있습니다. 다른 사용자 로부터 피드백을 사용 하 여 플레이어는 항상 있고 평판 통계에 대 한 값을 **Reputation.OverallReputation** 속성입니다.


## <a name="reputation-as-an-indicator-of-behavior"></a>동작의 표시기로 평판

신뢰도를 시스템에서 사용자의 동작 방법을 나타내는 표시기를 제공 합니다. 예를 들어 인지 하는 사용자는 친숙 한 플레이어? 다른 세션 구성원 으로부터 피드백 플레이어의 평판을 확인합니다. 각 사용자는 어디에서 나 Xbox One에 해당 사용자를 사용 하 여 전송 되는 신뢰도 점수를 갖습니다. 피어가 중 더 잘 작동 하는 플레이어에 게 적용할 수 있도록 친구를 볼 수 있는 공유 위치에서 확실 하 게 노출 됩니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

-   각 사용자의 프로필 카드입니다. 시스템의 모든 사용자가 한 번의 클릭을 사용 하 여 사용자의 프로필을 확인할 수 있습니다.
-   멀티 플레이 게임에서 표시 됩니다. 사용자가 함께 재생 하는 경우 온라인으로 그룹의 신뢰도 동일 그룹의 가장 낮은 신뢰도 사용 하 여 플레이어의 평판입니다. 만 그룹은 유사한 신뢰도 사용 하 여 다른 사용자와 일치 합니다. 다른 플레이어 평판을 사용 하 여 유형의 플레이어가 게임에 및 게임에 참가 하고자 하는 경우 결정을 결정 합니다.


## <a name="key-elements-of-reputation"></a>명성의 핵심 요소

제목을 경우 사용자 도달 하지는 말고 Me fair play, 통신 또는 사용자 생성 콘텐츠 (UGC) 범주에 대 한 수준으로 확인할 수 있습니다. 사용자에 도달 했습니다 Me 방지, 연결 된 범주에 대 한 다음 속성 중 하나라도 1로 설정 된 경우:

-   **Reputation.OverallReputation 속성**합니다. 연결 된 JSON OverallReputationIsBad입니다.
-   **Reputation.FairplayReputation 속성**합니다. 연결 된 JSON FairplayReputationIsBad입니다.
-   **Reputation.CommunicationsReputation 속성**합니다. 연결 된 JSON CommsReputationIsBad입니다.
-   **Reputation.UserGeneratedContentReputation 속성**합니다. 연결 된 JSON UserContentReputationIsBad입니다.


## <a name="good-game-reports"></a>적절 한 게임 보고서

잘못 된 작업에 대 한 사용자가 보고 하는 것 외에도 사용자를 보낼 수도 서로 가장 중요 한 플레이어에 대 한 응답으로 제목에 만들 수 있는 훌륭한 게임 보고서.


## <a name="feedback-history"></a>피드백 기록

피드백 기록을 사용자 마지막으로 보고 하는 사용자를 던 시간 보고에 대 한 예를 들어, "최근에 받은 통신 스타일에 대 한 피드백."와 같은 정보를 보고


## <a name="xbox-requirement-for-reputation"></a>평판 Xbox 요구 사항

Xbox 요구 사항 (XR) 결혼 정보 회사 평판, 필터링, 068 플레이어에서 높은 평판이 낮은 신뢰도 사용 하 여 분리를 해야합니다. 확인 하 여 이렇게 합니다 **Reputation.OverallReputation** 플레이어 및 사용자 통계에서 플레이어의 OverallReputationIsBad 특성의 값입니다.

제목에 "OverallReputation"를 전달 하 여 사용자의 신뢰도 검색할 수는 *statisticName* 의 매개 변수는 **UserStatisticsService.GetSingleUserStatisticsAsync 메서드 (String, String, String)**. WinRT 메서드 호출 **GET (/users/xuid({xuid})/scids/{scid}/stats)** 다음 JSON 본문에 표시 된 대로 합니다.

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

다른 플레이어에서 사용자의 피드백 낮은 수준에 도달 하면 OverallReputationIsBad 사용자에 대해 1로 설정 됩니다. 사용자에 대 한 **Reputation.OverallReputation** 는 1을 가진 다른 사용자와만 일치 해야 **OverallReputation** 1로 설정 합니다. 기본적으로 사용자 입력을 일치 하는 경우 일반적으로 필요가 평판이 낮은 사용자를 사용 하 여 처리 합니다. 필요에 따라 제목 평판이 좋은 플레이어 낮은 신뢰도 플레이어와 일치 하도록 옵트인 할 수 있습니다.

평판에 적용 되는 XRs의 최신 버전을 참조 하세요 [Xbox 요구 사항](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx) 게임 개발자 네트워크 (GDN).


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>테스트에 대 한 잘못 된 전체 신뢰도 사용 하 여 사용자 만들기

테스트에 대 한 일치 하는 필터링 제목에 대 한 구현을 테스트 하려면 매우 낮은 신뢰도 사용 하 여 사용자 설정할 수 있습니다. 사용자, 테스트 제목 또는 도구에 대 한 특정 값을 설정 하려면 호출 **POST (/users/xuid({xuid})/resetreputation)** 사용자의 특정 평판 값을 설정 하는 JSON 파일을 사용 하 여 합니다. 참조 **ResetReputation (JSON)** 자세한 내용은 합니다.

예를 들어 다음 JSON 본문을 사용자의 fair play 평판 품질 수준이 매우 낮은 값으로 설정합니다.

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

파트너 소매 제외 하 고 모든 샌드박스에 대 한이 호출을 만들 수 있습니다. 이 요청에는 사용자의 기본 신뢰도 점수를 설정 하 고 긍정적인 피드백에 대 한 플레이어의 가중치는 모두 지워진다는 키를 누릅니다. 이 호출 후에 사용자의 실제 평판 이러한 기본 점수와 플레이어가 특사 보너스 플레이어의 팔로 워 보너스도 구성 됩니다. 이 메커니즘은 낮은 점수를 사용 하 여 사용자를 만듭니다. 및 **Reputation.OverallReputation** 일치 필터 XR 테스트 하려면 1로 설정 합니다. 이 항목의 "Xbox 요구 사항에 대 한 평판" 섹션에 설명 된 대로 제목 사용자 통계 서비스에서 사용자에 대 한 사용자 신뢰도 점수를 얻을 수 있습니다.


## <a name="resetting-a-users-reputation-to-the-defaults"></a>사용자의 평판을 기본값으로 다시 설정

제목은 사용자 좋은 평판이 있음을 나타내기 위해 OverallReputationIsBad 특성을 설정 합니다. 호출한 **POST (/ 사용자/me/resetreputation)** 75 모든 값을 가져오거나 설정 합니다. 에 대 한 단일 호출 **/users/xuid({xuid})/deleteuserdata** 여러 Xbox 사용자 Id를 다시 설정에 사용할 수 있습니다. 요청 본문은 다음과 같습니다.

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>제목에서 사용자 의견 보내기

제목 일치에서 플레이어에 대 한 fair play 피드백을 보낼 수 있습니다. 제목에서 직접 또는 일괄 처리의 제목 서비스에서 수행 됩니다. 제목을 사용할 수는 **ReputationService.SubmitReputationFeedbackAsync 메서드** 다음 직접 REST 메서드 또는:

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| 클라이언트 POST          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| 서비스 (일괄 처리) 게시물 | https://reputation.xboxlive.com:10443/users/batchfeedback   |

참조 **피드백 (JSON)** 제출을 위한 및 허용 되는 필드 정의 대 한 샘플 JSON 본문입니다.


## <a name="types-of-feedback-allowed"></a>허용 되는 피드백의 형식

게시할 수 있는 피드백의 범주에 정의 된 **피드백 (JSON)** 합니다.


## <a name="when-titles-should-send-feedback"></a>제목 피드백을 전송 해야 경우

이벤트가 게임에서 플레이어 환경을 부정적인 영향을 주는 경우 제목을 부정적인 피드백을 전송 해야 합니다. 일반적으로 제목 보내야 유형당 하나의 피드백 당 왕복 재생 합니다. 예를 들어 제목을 다음과 같은 작업을 수행 해야합니다.

1.  사용자를 팀원과 종료 될 때마다 하나의 이벤트를 전송 하는 대신 3 개 이상의 팀원이 kill 라운드 당 사람이 하나 FairPlayKillsTeammates 피드백 항목을 보내기만 합니다.
2.  종료 될 때 사용자가 의도적으로 일치 하는 초기 FairplayQuitter 피드백을 보냅니다.
3.  레이싱 게임에서 사용자는 이전 버전과에 구동 하는 경우 경합을 한 번씩 FairplayUnsporting 피드백 항목을 보냅니다.
