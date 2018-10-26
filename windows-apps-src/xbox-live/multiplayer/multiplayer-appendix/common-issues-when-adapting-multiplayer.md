---
title: 일반적인 멀티 플레이 2015 마이그레이션 문제
author: KevinAsgari
description: 멀티 플레이 2014 타이틀 2015 멀티 플레이어를 조정 하는 경우에 발생할 수 있습니다 일반적인 문제에 알아봅니다.
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0704fe3919a84df718c9c1fba44a4fff85ee02ca
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5554414"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>멀티 플레이 2015 멀티 플레이 2014 타이틀을 조정 하는 경우에 일반적인 문제

이 항목에서는 2015 멀티 플레이어 2014 멀티 플레이 제목에 적응 때 고려해 야 할 문제를 설명 합니다.


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>구성 변경에 영향을 주지 2015 멀티 플레이어

이 섹션에서는 2015 멀티 플레이어 세션 및 템플릿을 구성할 때 주의 해야 할 변경 내용을 설명 합니다. 설명 특정 항목의 예제를 [MPSD 세션 템플릿](multiplayer-session-directory.md)을 참조 하세요.

### <a name="add-a-capability-for-active-member-connection"></a>활성 구성원 연결에 대 한 접근 권한 값 추가

ConnectionRequiredForActiveMembers 접근 하면 연결 끊기 감지 및 세션 2015 멀티 플레이어의 구독 기능을 변경 합니다. 모든 세션 템플릿 /constants/system/capabilities 개체에이 기능을 추가 합니다.


### <a name="add-a-system-constant-for-invite-protocol"></a>초대 프로토콜에 대 한 시스템 상수를 추가 합니다.

InviteProtocol 시스템 상수 보낸 사람 제목 호출 **MultiplayerService.SendInvitesAsync 메서드** 또는 SystemUI.ShowSendGameInvitesAsync 메서드 (≪ 사용자, **알림을 수신 하도록 초대 수신자를 수 있습니다. IMultiplayerSessionReference, String)**. 이 상수를 추가 하 고 "게임", 제목 플레이어가 초대를 세션에 대 한 모든 템플릿에 대해 /constants/system 개체를 설정 합니다.


## <a name="runtime-considerations-for-2015-multiplayer"></a>2015 멀티 플레이어에 대 한 런타임 고려 사항

2015 제목을 멀티 플레이어 해야: 항상 제목 코드의 멀티 플레이 영역을 시작 하기 전에 **MultiplayerService.EnableMultiplayerSubscriptions 메서드** 를 호출 합니다. 이 호출은 세션 변경에 모두 가입 하 고 감지를 분리 합니다.
-   동일한 사용자가 모든 호출에 대해 동일한 **XboxLiveContext 클래스** 개체를 사용 해야 합니다. 컨텍스트 사용 하는 멀티 플레이어 구독에 대 한 연결 관리와 관련 된 상태를 포함 하 고 감지를 분리 합니다.
-   여러 로컬 사용자 인 경우 각 사용자에 대 한 별도 **XboxLiveContext** 개체를 사용 합니다.


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>107를 세션 템플릿 104/105 계약 버전에서 마이그레이션

최신 세션 템플릿 계약 버전은 107 MPSD에 사용 되는 스키마에 몇 가지 변경 합니다. 세션 템플릿 계약 버전 104/105을 작성 한 템플릿 107 버전을 사용 하 여 다시 게시 않은 경우 업데이트 되어야 합니다. 이 항목의 최신 버전으로 템플릿의 마이그레이션을 수행할 변경 요약 되어 있습니다. [MPSD 세션 템플릿](multiplayer-session-directory.md)템플릿 자체 설명 되어 있습니다.

| 중요                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MPSD에 쓰기를 통해 템플릿을 통해 설정 하는 기능을 변경할 수 없습니다. 값을 변경 하려면 만들기 하 고 새 템플릿을 필요한 변경 사항으로 제출 해야 합니다. 템플릿을 통해 설정 되지 않은 모든 항목은 MPSD 쓰기를 통해 변경 될 수 있습니다. |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>개체 /constants/system/managedInitialization 변경

/Constants/system/managedInitialization 개체를 /constants/system/memberInitialization 이름이 변경 되었습니다. 다음은이 개체에 대 한 이름/값 쌍에 게 변경 내용을: autoEvaluate externalEvaluation로 변경 됩니다. 해당 false 제외 하 고 해당 극성 변경 내용을 기본 유지 됩니다.
-   MembersNeededToStart 기본값인 1 2에서 변경 됩니다.
-   기본값인 joinTimeout 5 초에서 10 초 변경 됩니다.
-   기본값인 measurementTimeout 5 초에서 30 초 변경 됩니다.


### <a name="changes-to-the-constantssystemtimeouts-object"></a>개체 /constants/system/timeouts 변경

/Constants/system/timeouts 개체가 제거 하 고 시간 제한을 바꾸고 /constants/system 아래 재배치 합니다. 다음은이 개체에 대 한 이름/값 쌍에 게 변경 내용을: 예약 된 시간 초과 reservedRemovalTimeout 됩니다.
-   비활성 시간 제한 inactiveRemovalTimeout 됩니다. 새 기본값은 0 (시간).
-   준비 시간 초과 readyRemovalTimeout 됩니다.
-   SessionEmpty 시간 제한 sessionEmptyTimeout 됩니다.

[세션 시간 제한](mpsd-session-details.md)시간 제한의 세부 정보가 표시 됩니다.


### <a name="change-to-the-game-play-capability"></a>게임 플레이 접근 권한 값을 변경 합니다.

게임 접근 권한 값 사용 하면 최근 플레이어와 신뢰도 보고를 재생 합니다. 이제 일치 하는 도우미 세션 아니라 실제 게임 플레이 나타내는, 예를 들어 세션 로비 세션에서 참으로 /constants/system/capabilities/gameplay 필드를 지정 해야 합니다.


## <a name="see-also"></a>참고 항목

[MPSD 세션 템플릿](mpsd-session-details.md)
