---
title: 스마트 매치를 사용 하 여
description: Xbox Live 스마트를 사용 하 여 멀티 플레이 게임에서 플레이어와 일치 하는 방법을 알아봅니다.
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one 멀티 플레이어, 연결, 스마트
ms.localizationpriority: medium
ms.openlocfilehash: 89f33768efcd649987866fd0798c222aa97f7ff8
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "8754368"
---
# <a name="using-smartmatch-matchmaking"></a>스마트 매치를 사용 하 여

다음 순서도 스마트 매치 메이 킹 프로세스를 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>일치 티켓 세션 및 일치 티켓 만들기

연결을 시작 하기 전에 연결을 함께 입력 하려면 사용자의 그룹을 나타낼 매치 메이 킹 "정찰" 일치 티켓 세션을 설정 합니다. 이 그룹의 모든 사용자 **(문자열, 부울, 부울) MultiplayerSession.Join 메서드**를 사용 하 여 세션에 참가 합니다.

티켓 세션을 만들고 플레이어 입력에, 제목 **MatchmakingService.CreateMatchTicketAsync 메서드**를 사용 하 여 연결 서비스 세션을 제출 합니다. 이 메서드는 티켓 세션을 나타내는 "검색"을 티켓 세션에서 /servers/matchmaking/properties/system/status 필드를 업데이트 하는 일치 티켓을 만듭니다. 자세한 내용은 참조 [하는 방법: 일치 티켓 만들기](multiplayer-how-tos.md)합니다.

일치 티켓 생성 메서드의 응답 **CreateMatchTicketResponse 클래스** 개체입니다. 응답 일치 티켓 ID가 포함 되어 사용할 수 있는 GUID를 사용 하 여 티켓을 삭제 하 여 연결을 취소할 수 있습니다. 응답은 사용자 설정에 사용할 수 있는 hopper에 대 한 평균 대기 시간을도 포함 되어 있습니다.


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>세션 및 플레이어에 연결 특성을 설정합니다.

연결에 대 한 세션을 제출할 때 제목 매치 메이 킹이 서비스는 다른 세션을 사용 하 여 세션을 그룹화 하는 특성을 설정할 수 있습니다. 티켓 수준이 나 구성원 단위 수준에서 특성을 지정할 수 있습니다.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>매치 메이 킹 특성 일치 티켓 수준 설정

제목은 **MatchmakingService.CreateMatchTicketAsync** 메서드의 *ticketAttributesJson* 매개 변수에서 일치 티켓 수준에서 특성을 제출합니다. 티켓 수준에서 지정 된 특성 구성원 단위 수준에서 지정 된 동일한 특성을 재정의 합니다.


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>구성원 단위 수준에서 매치 메이 킹 특성을 설정합니다.

제목 일치 티켓 세션 내에서 각 구성원에 멤버 당 특성을 지정합니다. **MultiplayerSession.SetCurrentUserMemberCustomPropertyJson 메서드**를 호출 "matchAttrs"의 속성 이름을 사용 하 여 설정 됩니다. 이 호출은 특성에 배치 /members/ {인덱스} / 속성/사용자 지정/matchAttrs 티켓 세션 내에서 각 플레이어에 필드.

매치 메이 킹 프로세스 "병합" 구성원 단위 각는 hopper에 대 한 Xbox Live 구성과 해당 특성에 대해 지정 된 결합 방법에 따라 단일 티켓 수준 특성으로 합니다. 이 [XDP](https://xdp.xboxlive.com) 또는 [파트너 센터](https://partner.microsoft.com/dashboard)에서 구성할 수 있습니다.


## <a name="making-the-match"></a>일치 하는 만들기

티켓 세션 및 일치를 사용 하 여 티켓 설정 매치 메이 킹 서비스 일치 하는 다른 그룹을 나타내는 다른 티켓 세션 표현된 티켓 세션 및 만들거나 일치 대상 세션을 식별 합니다. 서비스 대상 세션에서 일치 하는 플레이어에 대 한 예약 만듭니다 하며 일치 하는 것으로 다음 티켓 세션을 표시 합니다. MPSD 티켓 세션에 이러한 변경의 제목을 알립니다.

이제 제목 해야 다음, 충분 한 플레이어에 나타나는지 확인 하기 위해 대상 세션을 초기화 하는 단계를 수행 및 품질의 QoS (서비스) 검사를 성공적으로 연결할 수 있는를 수행 합니다. 초기화 및/또는 QoS 실패할 경우 제목 다른 그룹을 찾을 수 있도록 연결을 다시 제출 하기 위해 티켓 세션을 표시 합니다. 프로세스에 대 한 자세한 내용은 [대상 세션 초기화 단계 및 QoS를](smartmatch-matchmaking.md)참조 하세요.

일치 활동 동안 다음과 같은 변경 내용은 세션에 대 한 JSON 개체 적용 됩니다.

-   "찾을 수 없습니다"로 설정 하는 /servers/matchmaking/properties/system/status 필드
-   /servers/matchmaking/properties/system/targetSessionRef 필드가 대상 세션 설정
-   {인덱스} /members//속성/custom 각 티켓 세션에 대 한 matchAttrs 필드 /members/ {인덱스}로 복사 / / 상수/사용자 지정/matchmakingResult/playerAttrs 필드
-   각 플레이어에 대 한 일치 티켓에서 ticketAttributes 필드에서 /members/ {인덱스}로 복사 하는 특성 티켓/상수/사용자 지정/matchmakingResult/ticketAttrs 필드


## <a name="maintaining-the-match-ticket"></a>일치 티켓을 유지합니다.

연결 서비스는 세션에 대 한 일치 티켓 생성 될 때 한 번에 티켓 세션의 스냅숏을 사용 합니다. 따라서 모든 플레이어에 가입 하거나 티켓 세션을 제목은 삭제 하 고 일치 티켓 다시 연결 서비스를 사용 해야 합니다.


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>일치 티켓 세션 게임 세션을 다시 사용

| 중요                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *PreserveSession* 를 사용 하 여 두 개의 세션을 항상 설정 수 없음을 결합 될 수 있으므로, 서로 일치 시킬 수 있다는 것을 것입니다. 제목에서 사용 되는 연결 흐름 고려이 취해야 합니다. |

제목을 이미 진행 중인 게임에 참가 하려면 더 많은 플레이어를 찾으려면 일치 티켓 세션 기존 게임 세션 다시 사용할 수 있습니다. 이렇게 하려면 제목 **CreateMatchTicketAsync** **PreserveSessionMode.Always**설정 *preserveSession* 매개 변수를 사용 하 여 호출 하 여 일치 티켓을 만들어야 합니다. 연결 서비스 티켓에 사용 되는 기존 세션 연결 프로세스 내내 유지 및 결과 대상 세션 수를 확인 합니다.


## <a name="deleting-the-match-ticket"></a>일치 티켓 삭제

일치 티켓을 삭제 하려면 제목 **MatchmakingService.DeleteMatchTicketAsync 메서드**를 호출 합니다. 티켓을 삭제 합니다.

1.  사용자 티켓 세션에 대 한 연결을 중지 합니다.
2.  업데이트를 티켓 세션의 /servers/matchmaking/properties/system/status 필드 "취소".


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>Xbox Live 계산을 사용 하 여 게임에 대 한 연결을 수행 합니다.

사용자 matchmade Xbox Live 계산 기반 게임을 갖도록 하는 개략적인 단계는 다음과 같습니다. 제 3 자에서 호스트 되는 게임에 대 한 유사한 흐름 적용 해야 합니다.
1.  정찰 그룹 티켓 세션을 만듭니다. 이 세션 /constants/system/measurementServerAddresses에서 세션 구성에 있는 잠재적인 데이터 센터의 목록이 포함 되어 있습니다. 데이터 센터 목록은 정적 하는 경우는 세션 템플릿 또는 첫 번째 Xbox Live 계산에서 가져온 후 세션 생성 시을 작성 하는 클라이언트에서 제공 됩니다. 또한이 세션 gsiSetId, gameVariantId, 및 maxAllowedPlayers 값 targetSessionConstants/사용자 지정/gameServerPlatform 개체에 포함 되어 있습니다.
2.  그룹의 다른 모든 클라이언트 티켓 세션에 참가 합니다.
3.  그룹의 모든 구성원 티켓 세션에 대 한 /constants/system 개체에서 measurementServerAddresses 값을 다운로드, 플랫폼 API를 사용 하 여 ping 및 /members/ {인덱스}에 정의 된 기본 데이터 센터의 정렬된 된 목록 세션에 업로드 /properties/system/serverMeasurements 합니다.

| 참고                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 제목 설정 하 고 **MultiplayerSession.SetMeasurementServerAddresses** 메서드와 **MultiplayerSessionConstants.MeasurementServerAddressesJson 속성을 사용 하 여 세션의 measurementServerAddresses 값을 검색할 수 있습니다. **. |

4.  정찰 **CreateMatchTicketAsync**, 티켓 세션에 대 한 참조를 전달 하 여 호출 합니다.

| 참고                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 티켓 세션 개체 일치 하지 않는 상수 있으면 만들기 티켓 메서드를 완료할 수 없습니다. 필수인을 추가 하 여이 방지할 수 있습니다 규칙 상수를 일치 하는 플레이어와 일치 하는 것을 방지 하는 hopper입니다. |

**MatchTicketDetailsResponse.PreserveSession 속성** 을 사용 안 함 설정 연결 서비스는 티켓의 내부 표현으로 서버 측정 각 구성원에서 복사 합니다. "특수" 티켓 특성으로 티켓의 내부 표현에 저장 된 티켓에 대 한 단일 서버 측정 컬렉션에 티켓의 구성원 서버 측정 값을 병합 합니다.

**MatchTicketDetailsResponse.PreserveSession** 항상으로 설정 되 면 서버 측정 사용 되지 않습니다. 대신, 다른 서비스 티켓 내부 표현에 대기 시간이 0 크기 1 serverMeasurements 컬렉션으로에 세션에 대 한 /properties/system/matchmaking/serverConnectionString 값을 복사 합니다.

5.  연결 서비스 서버에 고려 하 여 측정 컬렉션을 수행할 다른 그룹을 나타내는 다른 사용자와 티켓 세션을 찾습니다. 이 기본 설정 같은 데이터 센터에 있는 다른 그룹을 사용 하 여 그룹 일치 하려고 합니다.
6.  일치 하는 그룹을 찾으면 매치 메이 킹 서비스 또는 대상 세션을 식별 만들고 함께 일치 하는 티켓 세션에서 모든 플레이어를 추가 합니다. 서비스 /properties/system/serverConnectionStringCandidates 일치 하는 그룹에 대 한 마지막 병합 된 서버 측정을 기록합니다. 대상 세션에서 각 새로 추가 된 멤버에 대해 제출 된 원래 서버 측정 /members/ {인덱스}를 기록/상수/시스템/matchmakingResult/serverMeasurements 합니다.
7.  모든 클라이언트는 위에서 설명한 것 처럼 대상 세션에서 초기화를 수행 합니다. 그러나 Xbox Live 계산에 클라이언트 연결, 때문에 수행 하지 않습니다 QoS 상호 연결을 확인 합니다.
8.  일부 또는 모든 클라이언트 **GameServerPlatformService.AllocateClusterAsync 메서드**를 호출 합니다. 첫 번째 승인을 다른 수신 하는 동안 할당을 트리거합니다. 메서드는 MPSD에서 대상 세션을 가져오고 대상 세션 뿐만 아니라 부하 및 기타 Xbox Live 계산 관련 기술 데이터 센터 기본 설정에 따라 데이터 센터를 선택 합니다.
9.  모든 클라이언트를 재생합니다.


## <a name="see-also"></a>참고 항목

[스마트 런타임 작업](smartmatch-matchmaking.md)

[스마트 매치 매치 메이킹](smartmatch-matchmaking.md)

**Microsoft.Xbox.Services.Matchmaking Namespace**

**Microsoft.Xbox.Services.Multiplayer Namespace**
