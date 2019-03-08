---
title: SmartMatch 결혼 정보 회사 연결을 사용 하 여
description: Xbox Live SmartMatch 멀티 플레이 게임에서 플레이어에 맞게 사용 하는 방법에 알아봅니다.
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one, 멀티 플레이 매 치메이 킹, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 89f33768efcd649987866fd0798c222aa97f7ff8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592098"
---
# <a name="using-smartmatch-matchmaking"></a>SmartMatch 결혼 정보 회사 연결을 사용 하 여

다음 순서도에서는 SmartMatch 결혼 정보 회사 연결 프로세스를 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>일치 티켓 세션과 일치 티켓 만들기

결혼 정보 회사 연결을 시작 하기 전에 매 치메이 킹 "scout"는 매 치메이 킹을 함께 입력 하려는 사용자 그룹을 나타내는 일치 티켓 세션을 설정 합니다. 이 그룹의 모든 사용자가 사용 하 여 세션을 조인 합니다 **MultiplayerSession.Join 메서드 (String, Boolean, Boolean)** 합니다.

제목 전송 세션을 사용 하 여 매 치메이 킹 서비스 티켓 세션 생성 되었으며 플레이어로 채워진 후 합니다 **MatchmakingService.CreateMatchTicketAsync 메서드**합니다. 이 메서드는 티켓 세션을 나타내며 "검색" 티켓 세션에서 /servers/matchmaking/properties/system/status 필드를 업데이트 하는 일치 티켓을 만듭니다. 자세한 내용은 참조 하세요. [방법: 일치 티켓을 만드는](multiplayer-how-tos.md)합니다.

Match 티켓 만들기 메서드에서 응답은는 **CreateMatchTicketResponse 클래스** 개체입니다. 일치 티켓 ID를 포함 하는 응답을 사용할 수 있는 GUID를 사용 하 여 티켓을 삭제 하 여 매 치메이 킹을 취소할 수 수 있습니다. 응답에는 또한 사용자 기대 수준을 사용할 수 있는 hopper에 대 한 평균 대기 시간을 포함 합니다.


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>세션 및 플레이어에 매 치메이 킹 특성 설정

매 치메이 킹에 대 한 세션에 제출할 때 제목 매 치메이 킹 서비스가 다른 세션을 사용 하 여 세션을 그룹화 하는 데 사용 하는 특성을 설정할 수 있습니다. 티켓 수준 또는 멤버 단위 수준에서 특성을 지정할 수 있습니다.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>일치 티켓 수준 결혼 정보 회사 연결 특성을 설정합니다.

제목 일치 티켓 수준에서 특성을 제출 합니다 *ticketAttributesJson* 의 매개 변수를 **MatchmakingService.CreateMatchTicketAsync** 메서드. 티켓 수준에서 지정 된 특성 구성원 단위 수준에서 지정 된 동일한 특성을 재정의 합니다.


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>매 치메이 킹 특성 구성원 단위 수준에서 설정

제목 일치 티켓 세션 내에서 각 구성원에서 구성원 단위 특성을 지정합니다. 이러한 호출 하 여 설정 됩니다는 **MultiplayerSession.SetCurrentUserMemberCustomPropertyJson 메서드**, "matchAttrs"의 속성 이름을 사용 하 여 합니다. 이 호출은 특성에 배치 /members/ {index} / 속성/사용자 지정/티켓 세션 내에서 각 플레이어에 matchAttrs 필드입니다.

매 치메이 킹 프로세스 "평면화" 구성원 각 hopper Xbox Live 구성에서 해당 특성에 대해 지정 된 결합 방법에 따라 단일 티켓 수준 특성에 있습니다. 이 구성할 수 있습니다 [XDP](https://xdp.xboxlive.com) 하거나 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.


## <a name="making-the-match"></a>일치를 수행합니다.

티켓 세션과 일치 항목을 사용 하 여 티켓을 설정 하 여 매 치메이 킹 서비스 일치 항목을 다른 그룹을 나타내는 다른 티켓 세션을 사용 하 여 표현된 티켓 세션 만들거나 일치 대상 세션을 식별 합니다. 또한 대상 세션에서 일치 하는 플레이어에 대 한 예약을 만듭니다 하 한 다음 일치 하는 대로 티켓 세션을 표시 하는 서비스입니다. MPSD 티켓 세션에 대 한이 변경은 제목을 알립니다.

이제 제목 다음는 충분 한 플레이어 표시를 확인 하려면 대상 세션을 초기화 하는 단계를 수행 하며 품질의 QoS (서비스) 검사 서로 성공적으로 연결할 수 있는지를 수행 합니다. 초기화 및/또는 QoS 실패 하면 다른 그룹을 찾을 수 있도록 제목 결혼 정보 회사 연결을 다시 제출 하기 위해 티켓 세션을 표시 합니다. 프로세스에 대 한 자세한 내용은 참조 하세요. [대상 세션 초기화 및 QoS](smartmatch-matchmaking.md)합니다.

일치 작업 중 다음 변경 내용은 세션에 대 한 JSON 개체를 적용 됩니다.

-   /servers/matchmaking/properties/system/status 필드가 "찾을"로 설정
-   대상 세션으로 /servers/matchmaking/properties/system/targetSessionRef 필드 설정
-   /members/ {index} / 속성/사용자 지정 {index} /members/ matchAttrs 필드 각 티켓 세션에 대 한 복사 / / 상수/사용자 지정/matchmakingResult/playerAttrs 필드
-   각 플레이어에 대 한 티켓 /members/ {index}에 일치 티켓 ticketAttributes 필드에서 복사 하는 특성/상수/사용자 지정/matchmakingResult/ticketAttrs 필드


## <a name="maintaining-the-match-ticket"></a>일치 티켓을 유지 관리

세션에 대 한 일치 티켓을 만들면 시 티켓 세션의 스냅숏을 사용 하는 결혼 정보 회사 연결 서비스입니다. 따라서 모든 플레이어 조인 또는 티켓 세션을 유지 하는 경우 제목을 삭제 하 고 다시 일치 티켓 매 치메이 킹 서비스를 사용 해야 합니다.


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>일치 티켓 세션으로 게임 세션을 다시 사용

| 중요                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 두 개의 세션을 실현 하려면 반드시 *preserveSession* Always로 설정 됩니다 결합할 수 없습니다 때문에 서로 일치 하는 일을 할 수 없습니다. 제목에 사용 되는 매 치메이 킹 흐름이이 사실을 고려해 야 합니다. |

제목에는 기존 게임 세션이 이미 진행 중인 게임에 참가 하려면 더 많은 플레이어를 찾을 일치 티켓 세션으로 다시 사용할 수 있습니다. 이렇게 하려면 제목 만들어야 일치 티켓을 호출 하 여 **CreateMatchTicketAsync** 사용 하 여 합니다 *preserveSession* 매개 변수 설정 **PreserveSessionMode.Always** . 매 치메이 킹 서비스 티켓에 사용 되는 기존 세션 결혼 정보 회사 연결 과정에서 유지 됩니다 하 고이 결과 대상 세션 될 것을 확인 합니다.


## <a name="deleting-the-match-ticket"></a>일치 티켓 삭제

제목 호출 일치 티켓을 삭제 하려면 **MatchmakingService.DeleteMatchTicketAsync 메서드**합니다. 티켓의 삭제:

1.  티켓 세션에서 사용자에 대해 매 치메이 킹을 중지합니다.
2.  업데이트 티켓 세션에서 /servers/matchmaking/properties/system/status 필드 "canceled"입니다.


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>Xbox Live 계산을 사용 하 여 게임에 대 한 매치 메이 킹 수행

Xbox Live Compute 기반 게임을 사용자 matchmade을 수행 하는 대략적인 단계는 다음과 같습니다. 타사에서 호스팅되는 게임에 대 한 비슷한 흐름을 적용 해야 합니다.
1.  scout에 그룹을 나타내는 티켓 세션을 만듭니다. 이 세션 /constants/system/measurementServerAddresses에서 세션 구성에 있는 잠재적인 데이터 센터의 목록을 포함 합니다. 템플릿에서 두 세션 데이터 센터 목록 정적 이면 또는 첫 번째 Xbox Live Compute에서 받은 후 세션 생성 시 작성 하는 클라이언트에서 제공 됩니다. 이 세션에는 또한 gsiSetId, gameVariantId, 및 targetSessionConstants/사용자 지정/gameServerPlatform 개체의 maxAllowedPlayers 값 포함합니다.
2.  그룹의 다른 모든 클라이언트 티켓 세션에 참가 합니다.
3.  그룹의 모든 멤버 티켓 세션에 대 한 /constants/system 개체에서 measurementServerAddresses 값 다운로드 플랫폼 API를 사용 하 여 ping, /members/ {index}에 정의 된 대로 세션에 대 한 기본 데이터 센터의 정렬된 된 목록을 업로드합니다 /properties/system/serverMeasurements 합니다.

| 참고                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 제목을 설정 하 고 사용 하 여 세션에서 measurementServerAddresses 값을 검색할 수는 **MultiplayerSession.SetMeasurementServerAddresses** 메서드 및  **MultiplayerSessionConstants.MeasurementServerAddressesJson 속성**합니다. |

4.  Scout 호출 **CreateMatchTicketAsync**티켓 세션에 대 한 참조를 전달 합니다.

| 참고                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 티켓 세션 개체에 일치 하지 않는 상수 있으면 create 티켓 메서드 실패할 수 있습니다. 반드시 추가 하 여 방지할 수 있습니다이 상수에 일치 하지 않는 플레이어를 사용 하 여 일치를 방지 하기 위해 hopper 하는 규칙입니다. |

경우는 **MatchTicketDetailsResponse.PreserveSession 속성** 설정 안 함, 매 치메이 킹 서비스 복사 server 측정 각 구성원에서 티켓의 내부 표현으로 합니다. "특별" 티켓 특성으로 티켓의 내부 표현에 저장 된 티켓을 단일 서버 측정값 컬렉션에는 티켓의 멤버 서버 측정을 평면화 합니다.

하는 경우 **MatchTicketDetailsResponse.PreserveSession** 항상 측정을 사용 하지 않는 서버에 설정 됩니다. 대신, 결혼 정보 회사 서비스 대기 시간이 0 크기 1 serverMeasurements 컬렉션으로 티켓을의 내부 표현이에 세션에 대 한 /properties/system/matchmaking/serverConnectionString 값을 복사 합니다.

5.  결혼 정보 회사 연결 서비스는 티켓 세션 서버 계정에 대 한 측정 컬렉션을 수행 하는 다른 그룹을 나타내는 다른 사용자와 일치 합니다. 항상 기본 설정에 관계 없이 동일한 데이터 센터에 있는 다른 그룹을 사용 하 여 그룹을 일치 하려고 합니다.
6.  일치 하는 그룹을 찾은, 결혼 정보 회사 연결 서비스 또는 대상 세션을 식별 하 만들고 함께 일치 하는 티켓 세션에서 모든 사람들에 게 추가 합니다. 서비스에서는 일치 하는 그룹에 대 한 최종 일반된 서버 측정 /properties/system/serverConnectionStringCandidates 씁니다. 대상 세션 새로 추가 된 각 멤버에 대해 전송 된 원래 서버 측정 /members/ {index}를 작성/상수/시스템/matchmakingResult/serverMeasurements 합니다.
7.  모든 클라이언트는 위에서 설명한 것 처럼 대상 세션에서 초기화를 수행 합니다. 그러나 클라이언트가 연결할 Xbox Live Compute을 하기 때문에 수행 하지 않습니다 QoS 서로 연결을 확인 합니다.
8.  일부 또는 모든 클라이언트 호출을 **GameServerPlatformService.AllocateClusterAsync 메서드**합니다. 첫 번째 승인을 다른 수신 하는 동안 할당을 트리거합니다. 메서드를 대상 세션 MPSD에서 가져오고 대상 세션으로 로드 및 기타 Xbox Live 계산 관련 정보에는 데이터 센터 기본 설정에 따라 데이터 센터를 선택 합니다.
9.  모든 클라이언트를 재생합니다.


## <a name="see-also"></a>참고 항목

[SmartMatch 런타임 작업](smartmatch-matchmaking.md)

[SmartMatch Matchmaking](smartmatch-matchmaking.md)

**Microsoft.Xbox.Services.Matchmaking Namespace**

**Microsoft.Xbox.Services.Multiplayer Namespace**
