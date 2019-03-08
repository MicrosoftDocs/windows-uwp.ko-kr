---
title: 일반적인 다중 접속 2015 마이그레이션 문제
description: 멀티 플레이 2014 제목 2015 멀티 플레이 게임을 조정 하는 경우에 직면할 수도 있습니다는 일반적인 문제에 알아봅니다.
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a04370ee2f45534c88467700b9523c5a4ad11094
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615818"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>멀티 플레이 2015 멀티 플레이 2014 제목에 맞게 조정 하는 경우 일반적인 문제

이 항목에서는 2015 멀티 플레이어 게임 2014 멀티 플레이 제목을 조정 하는 경우 고려해 야 할 문제를 설명 합니다.


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>구성 변경에 영향을 주지 2015 멀티 플레이어 게임

이 섹션에서는 2015 멀티 플레이 게임에 대 한 템플릿을 확인 하 고 세션을 구성할 때 알아야 할 변경 내용을 설명 합니다. 설명 된 특정 항목의 예제를 참조 하세요 [MPSD 세션 템플릿을](multiplayer-session-directory.md)합니다.

### <a name="add-a-capability-for-active-member-connection"></a>활성 구성원 연결에 대 한 기능 추가

ConnectionRequiredForActiveMembers 기능 연결 끊기 검색 있으며 세션 2015 멀티 플레이어의 구독 기능을 변경 합니다. 모든 세션 템플릿에 대해 /constants/system/capabilities 개체에이 기능을 추가 합니다.


### <a name="add-a-system-constant-for-invite-protocol"></a>초대 프로토콜에 대 한 시스템 상수를 추가 합니다.

InviteProtocol 시스템 상수는 초대를 보낸 사람의 제목 호출할 때 알림의 받을 받는 사람을 사용 하도록 설정 합니다 **MultiplayerService.SendInvitesAsync 메서드** 또는 **SystemUI.ShowSendGameInvitesAsync 메서드 (IUser에서 IMultiplayerSessionReference, String)** 합니다. 이 상수를 추가, "게임"을 제목은 플레이어를 초대 하는 세션에 대 한 모든 템플릿에 대해 /constants/system 개체를 설정 합니다.


## <a name="runtime-considerations-for-2015-multiplayer"></a>멀티 플레이 게임 2015 런타임 시 고려 사항

제목 2015 용 멀티 플레이어 수행 해야합니다.   항상 호출 합니다 **MultiplayerService.EnableMultiplayerSubscriptions 메서드** 제목 코드의 멀티 플레이 영역에 진입 하기 전에 합니다. 이 호출은 세션 변경에 대 한 두 구독을 사용 하도록 설정 하 고 검색을 분리 합니다.
-   동일한 것을 사용 해야 **XboxLiveContext 클래스** 동일한 사용자가 모든 호출에 대 한 개체입니다. 컨텍스트 관리와 관련 된 멀티 플레이어 구독에 사용 되는 연결의 상태를 포함 및 연결 해제 감지 합니다.
-   여러 로컬 사용자의 경우 사용 하 여 별도 **XboxLiveContext** 각 사용자에 대 한 개체입니다.


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>계약 버전 104/105 세션 템플릿을 107로

최신 세션 템플릿을 계약 버전이 107, MPSD에 사용 된 스키마를 변경 하 여 사용 합니다. 템플릿 계약 버전 104/105 세션에 대 한 기록 템플릿은 107 버전을 사용 하도록 다시 게시 하는 경우 업데이트 되어야 합니다. 이 항목에서는 템플릿을 최신 버전으로 마이그레이션할 때에 변경 내용이 요약 되어 있습니다. 템플릿 자체에 설명 되어 있습니다 [MPSD 세션 템플릿을](multiplayer-session-directory.md)합니다.

| 중요                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 템플릿을 통해 설정 된 기능 MPSD에 쓰기를 통해 변경할 수 없습니다. 값을 변경 하려면 해야 만들고 필요에 따라 변경을 사용 하 여 새 템플릿을 제출 합니다. 템플릿을 통해 설정 되지 않은 모든 항목을 MPSD에 쓰기를 통해 변경할 수 있습니다. |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>/Constants/system/managedInitialization 개체 변경

/Constants/system/managedInitialization 개체 /constants/system/memberInitialization으로 바뀌었습니다. 이 개체에 대 한 이름/값 쌍을 위해 변경: autoEvaluate externalEvaluation로 바뀌었습니다. false 제외 하 고 해당 극성 (polarity) 변경 내용을 기본값을 유지 합니다.
-   MembersNeededToStart 기본값인 2에서 1로 변경합니다.
-   JoinTimeout 기본값인 5 초에서 10 초로 변경합니다.
-   MeasurementTimeout 기본값인 5 초에서 30 초로 변경합니다.


### <a name="changes-to-the-constantssystemtimeouts-object"></a>/Constants/system/timeouts 개체 변경

/Constants/system/timeouts 개체가 제거 되 고 시간 제한의 이름을 바꾸고 /constants/system에서 재배치 됩니다. 변경 내용을이 개체에 대 한 이름/값 쌍에는 다음과 같습니다.   예약 된 시간 제한 reservedRemovalTimeout 됩니다.
-   비활성 시간 제한 inactiveRemovalTimeout 됩니다. 새 기본값은 0 (시간).
-   준비 시간 초과 readyRemovalTimeout 됩니다.
-   SessionEmpty 제한 sessionEmptyTimeout 됩니다.

시간 제한 세부 정보에 제공 됩니다 [세션 제한 시간은](mpsd-session-details.md)합니다.


### <a name="change-to-the-game-play-capability"></a>게임 플레이 기능 변경

게임 기능 사용에 대 한 최근 플레이어 및 평판 보고를 재생 합니다. 이제 세션을 나타내는 도우미 세션 대신 실제 게임 플레이, 예를 들어, 일치 및 세션 로비에서 true로 /constants/system/capabilities/gameplay 필드를 지정 해야 합니다.


## <a name="see-also"></a>참고 항목

[MPSD 세션 템플릿](mpsd-session-details.md)
