---
title: 멀티 플레이 방법
description: Xbox Live 멀티 플레이 게임 2015에서 일반적인 작업을 구현 하는 방법을 설명 합니다.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.date: 08/29/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 멀티 플레이 2015, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 20bf3486491e8173ce946a3a5dc2e4bc83305c83
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648638"
---
# <a name="multiplayer-how-tos"></a>멀티 플레이 방법

이 항목에서는 멀티 플레이 2015를 사용 하 여 관련 된 특정 작업을 구현 하는 방법에 대 한 정보를 포함 합니다.

* MPSD 세션 변경 알림에 대 한 구독
* MPSD 세션 만들기
* 여기서 중재자는 MPSD 세션 설정
* 제목 정품 인증 관리
* 조인 가능한 사용자를 위해
* 게임 초대 보내기
* 로비 세션에서 게임 세션에 참가
* 제목 활성화에서 MPSD 세션에 참가합니다
* 사용자의 현재 활동 설정
* MPSD 세션을 업데이트 합니다.
* MPSD 세션을 그대로 둡니다.
* 결혼 정보 회사 연결 하는 동안 열려 있는 세션 슬롯을 채웁니다
* 일치 티켓 만들기
* 일치 티켓 상태 가져오기

## <a name="subscribe-for-mpsd-session-change-notifications"></a>MPSD 세션 변경 알림에 대 한 구독

| 참고                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 세션 변경에 대 한 구독 세션에서 활성화 되도록 연결된 플레이어에 필요 합니다. ConnectionRequiredForActiveMembers 필드 설정 해야 세션에 대 한 /constants/system/capabilities 개체의 true입니다. 이 필드는 일반적으로 세션 템플릿에서 설정 됩니다. 참조 [MPSD 세션 템플릿을](multiplayer-session-directory.md)합니다. |



MPSD 세션 변경 알림을 수신 하려면 제목 아래 절차를 따르면 됩니다.

1.  사용한 것과 동일한 **XboxLiveContext 클래스** 동일한 사용자가 모든 호출에 대 한 개체입니다. 구독이이 개체의 수명에 연결 됩니다. 여러 로컬 사용자의 경우 사용 하 여 별도 **XboxLiveContext** 각 사용자에 대 한 개체입니다.
2.  에 대 한 이벤트 처리기를 구현 합니다 **RealTimeActivityService.MultiplayerSessionChanged 이벤트** 하며 **RealTimeActivityService.MultiplayerSubscriptionsLost 이벤트**합니다.
3.  둘 이상의 사용자에 대 한 변경 내용을 구독 하는 경우 코드를 추가 하 여 **MultiplayerSessionChanged** 불필요 한 작업을 방지 하려면 이벤트 처리기입니다. 사용 된 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성** 하며 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성**합니다. 이러한 속성을 사용 하 여 표시 된 마지막 변경의 추적 및 이전 변경 내용을 무시 하 고 있습니다.
4.  호출 된 **MultiplayerService.EnableMultiplayerSubscriptions 메서드** 구독 수 있도록 합니다.
5.  로컬 세션 개체 및 조인을 활성으로 해당 세션을 만듭니다.
6.  각 사용자에 대 한 호출을 수행 합니다 **MultiplayerSession.SetSessionChangeSubscription 메서드**, 알림을 받을 수 있는 유형 변경 세션을 전달 합니다.
7.  이제 쓸 세션 MPSD에 설명 된 대로 **방법: 멀티 플레이 세션 업데이트**합니다.

다음 순서도에서는 Multplayer이이 절차에서 설명 하는 이벤트를 구독 하 여 시작 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>중복 세션 변경 알림을 구문 분석

동일한 세션에 대 한 알림을 구독 하는 여러 사용자의 경우 해당 세션을 변경할 때마다 각 사용자에 대 한 자격 증명 입력 탭이 트리거됩니다. 다음 중 하나를 제외한 모든 중복 됩니다. 제목을 이미 된 알림을 받을;의 변경 내용을 무시할지 여전히 제목을 알림 세션에서 모든 사용자를 구독 하는 것이 좋습니다, 있지만 분기 및 ChangeNumber 속성을 사용 하 여 수행할 수 있습니다.

여러 자격 증명 입력 탭에서 검색할 제목을 다음을 수행 해야 합니다.

-   각 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성** store 최신 표시 된 값 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성**합니다.
-   자격 증명 입력 탭에는 더 높은 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성** 표시에 대 한 마지막 보다 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성** , 처리 및 최신 업데이트 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성**합니다.
-   자격 증명 입력 탭에 없으면 더 높은 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성** 에 대 한 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성**를 건너뜁니다. 해당 변경 내용은 이미 처리 되었습니다.

| 참고                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성** 값에서 추적할 필요가 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성**, 세션에서 없습니다. 에 대 한 것이 가능 합니다 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성** 값을 변경할 (및 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성**다시 설정 하려면)는 세션의 수명 내에서. |

## <a name="create-an-mpsd-session"></a>MPSD 세션 만들기


| 참고                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 기본적으로 MPSD 세션을 조인 하는 첫 번째 멤버 때 만들어집니다. 제목 논리에 이미 존재 하거나 조인 시 존재 하지 제목에서 예상 하는 경우 세션 업데이트 하는 동안 적절 한 쓰기 모드 값을 쓰기 메서드에 전달할 수 것입니다. |



제목에는 새 세션을 만들려면 다음을 수행 해야 합니다.

1.  새 **XboxLiveContext 클래스** 개체입니다. 제목이이 개체를 한 번 만듭니다, 그리고 저장 및 전체 소스 코드에서 필요에 따라이 다시 사용 합니다. 세션 구독을 사용 하는 경우에 특히 정확히 동일한 컨텍스트를 사용 하는 데 필요한 것입니다.
2.  새 **MultiplayerSession 클래스** 는 MPSD 새 세션을 만들어야 하는 모든 세션 데이터를 준비 하는 개체입니다.
3.  세션 MPSD에 쓰기 전에 필요한 변경 작업을 확인 합니다. 예를 들어, 멤버를 호출 하 여 세션에 참가 하는 경우 **MultiplayerSession.Join 메서드**, 클라이언트 세션을 업데이트 하는 호출 시 연결할 MPSD 알려 주는 숨겨진된 로컬 요청 데이터를 추가 합니다.
4.  로컬 변경 완료 되 면 쓸 MPSD에 설명 된 대로 **방법: 멀티 플레이 세션 업데이트**합니다.
5.  새 수신 **MultiplayerSession** MPSD 입력 하는 여러 필드가 포함 된 개체입니다.
6.  앞으로 새 세션 개체를 사용 하 고 새 세션을 만들려면 숨겨진된 요청이 포함 된 이전 복사본을 삭제 합니다.

### <a name="example"></a>예

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Partner Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>여기서 중재자는 MPSD 세션 설정




제목 다음 절차를 사용 하 여 여기서 중재자는 이미 만들어져 있는 세션에 대 한 설정.

| 참고                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 멤버는 세션에 참가 하 고 해당 보안 장치 주소를 포함 될 때까지 멤버 (잠재적 호스트)에 대 한 장치 토큰 제공 되지 않습니다. |

1.  장치 토큰을 사용 하 여는 MPSD에서 호스트 후보의 검색 합니다 **MultiplayerSession.Members 속성**합니다.

| 참고                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 클라이언트를 통해 MPSD에서 사용할 수 있는 호스트 후보의 SmartMatch 결혼 정보 회사 연결 하 여 만든 세션을 하는 경우 사용할 수는 **MultiplayerSession.HostCandidates 속성**합니다. |

2.  호스트 후보 목록에서 필요한 호스트를 선택 합니다.
3.  호출을 **MultiplayerSession.SetHostDeviceToken 메서드** 는 MPSD의 로컬 캐시에 장치 토큰을 설정 합니다. 호스트 장치 토큰을 설정 하는 호출이 성공 하면 로컬 장치 토큰 호스트의 토큰으로 대체 합니다.
4.  HTTP/412 상태 코드를 호스트 장치 토큰을 설정 하려고 할 때 수신 되 면 세션 데이터를 쿼리하고 로컬 콘솔에 대 한 호스트 장치 토큰 인지 확인 합니다. 로컬 콘솔에 대 한 없으면 다른 콘솔 중재자를로 지정 했습니다.

| 참고                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 클라이언트는 HTTP/412 표준 오류를 나타내지 않습니다 하므로 다른 HTTP 코드를 별도로 HTTP/412 상태 코드를 처리 해야 합니다. 한 상태 코드에 대 한 자세한 내용은 [멀티 플레이 게임 세션 상태 코드](multiplayer-session-status-codes.md)합니다. |

5.  에 설명 된 대로 MPSD, 세션 업데이트 **방법: 멀티 플레이 세션 업데이트**합니다.

| 참고                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 더 나은 알고리즘이 없는 경우 클라이언트는 각 호스트 후보 다른 사람이 작업을 수행 하는 경우 호스트로 설정 하려고 greedy 알고리즘을 구현할 수 있습니다. 자세한 내용은 [세션 중재자](mpsd-session-details.md)합니다. |

## <a name="manage-title-activation"></a>제목 정품 인증 관리

Xbox One에 발생 합니다 **CoreApplicationView.Activated 이벤트** 프로토콜 활성화 하는 동안. 멀티 플레이 API의 컨텍스트에서 사용자 초대를 수락 하거나 다른 사용자를 조인 하는 경우이 이벤트가 발생 합니다. 이러한 작업 제목 조인 사용자 대상 사용자를 사용 하 여 게임 플레이를 가져와서에 반응 해야 하는 활성화를 트리거합니다.

| 참고                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| 프로그램 제목 언제 든 지 새 활성화 인수를 예상 해야 및 길이 대 한 코딩 되지 있어야 합니다. |

제목에는 제목 활성화를 처리 하려면 다음 기본 단계를 수행 해야 합니다.

1.  에 대 한 이벤트 처리기를 설정 합니다 **CoreApplicationView.Activated 이벤트**합니다. 프로토콜 활성화 발생할 때마다 제목을 이미 실행 중인 경우에이 처리기를 트리거합니다.
2.  제목 정품 인증에 세션을 시작 하 고 세션 변경 알림에 대 한 구독. 참조 **방법: MPSD 세션 변경 알림에 대 한 구독**합니다.
3.  활성으로 세션에 사용자를 조인 합니다. 참조 **방법: 제목 활성화에서 MPSD 세션에 참가할**합니다.
4.  UI 프로필을 통해 노출 되는 활동 세션으로 로비 세션을 설정 합니다. 참조 **방법: 사용자의 현재 활동 설정**합니다.
5.  활성으로 게임 세션에 사용자를 조인 합니다. 이제 사용자는 동료에 게 연결 하 고 게임 플레이 또는 로비 입력 수 있습니다.

다음 순서도에서는 title 활성화를 처리 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>조인 가능한 사용자를 위해

참가할 수 있는 사용자를 위해, 제목 다음을 수행 해야 합니다.

1.  세션 개체를 만들고 필요에 따라 특성을 수정 합니다.
2.  활성으로 세션에 사용자를 조인 합니다. 참조 **방법: 제목 활성화에서 MPSD 세션에 참가할**합니다.
3.  사용자가 세션 중재자를 지정한 경우를 결정 합니다.
4.  사용자가 중재자가 아닌 경우 7 단계로 이동 합니다.
5.  사용자가 중재자 인 경우 호출 된 **MultiplayerSession.SetHostDeviceToken 메서드**합니다.
6.  호출 하 여 세션을 쓰려고 합니다 **MultiplayerService.TryWriteSessionAsync 메서드**합니다.
7.  활성 세션으로 세션을 설정 합니다. 참조 **방법: 사용자의 현재 활동 설정**합니다.

다음 순서도에서는 사용자가 게임을 하는 동안 다른 플레이어에서 조인할 수 있도록 하려면 수행 하는 단계를 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>게임 초대 보내기

제목 게임 다음과 같은 방법으로 초대를 보내려고 플레이어를 활성화할 수 있습니다.

-   로비 세션에 대 한 초대를 보냅니다.
-   게임에 대 한 참조를 사용 하 여 UI를 초대 하는 제네릭 Xbox 플랫폼을 사용 하 여 초대를 보냅니다.

게임 플레이어에 대 한 초대를 보내려면 제목 다음을 수행 해야 합니다.

1.  조인 가능한 초대 게임 플레이어를 확인 합니다. 참조 **방법: 조인 가능한 사용자를 위해**합니다.
2.  초대 UI를 사용 하 여 또는 초대는 로비 세션을 통해 전송할 경우를 확인 합니다.
3.  로비 세션을 사용 하는 경우 호출 하 여 초대를 보낼 **MultiplayerService.SendInvitesAsync 메서드**합니다. 이 메서드는 게임 내 명단 사용 하 여 UI 작성 해야 합니다 **SystemUI.ShowPeoplePickerAsync 메서드** 또는 **PartyChat.GetPartyChatViewAsync 메서드**합니다.
4.  초대 UI를 사용 하는 경우 호출 합니다 **SystemUI.ShowSendGameInvitesAsync 메서드** 초대 UI를 표시 합니다.
5.  처리를 **RealTimeActivityService.MultiplayerSessionChanged 이벤트** 원격 플레이어 조인 후 로컬 플레이어에 대 한 합니다.
6.  원격 플레이어에 대해 제목 활성화 코드를 구현 합니다. 참조 **방법: 제목 정품 인증 관리**합니다.

다음 순서도 초대를 전송 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>로비 세션에서 게임 세션에 참가

Windows 10 장치에서 게임 플레이 세션 있어야 합니다 `userAuthorizationStyle` 기능 설정 **true** 큰 세션 하지 않은 경우. 즉는 `joinRestriction` "none", 즉 세션 없습니다 공개적으로 참가할 수 있는 직접 속성 일 수 없습니다.

일반적인 시나리오는 플레이어를 수집 하 고 그런 다음 이러한 플레이어 세션 또는 결혼 정보 회사 연결 세션에는 게임 플레이 이동할 로비 세션을 만드는 것입니다. 게임 플레이 세션 없는 공개적으로 참가할 수 있는 경우 게임 클라이언트를 충족 하지 않는 경우 게임 플레이 세션을 연결할 수 없습니다 있지만 `joinRestriction` 이 시나리오에 대 한 대부분의 경우 너무 제한적인 설정 합니다.

솔루션은 로비 세션 및 게임 세션에 연결할 전송 핸들을 사용 합니다.  제목을 이렇게 하려면 다음을 수행 하 여:

1. 게임 세션을 만들 때 사용 된 `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` 로비 세션 및 게임 세션을 연결 하는 전송 핸들을 만드는 API입니다.
2. 게임 세션의 세션 대신 로비 세션의 GUID를 처리 하는 저장소를 전송 합니다.
3. 제목에서 게임 세션에 로비 세션에서 멤버를 이동 하려는 경우 각 클라이언트 사용 로비 세션에서 전송 핸들을 사용 하 여 게임 세션에 참가 하기는 `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API.
4. MPSD 로비 세션 전송 핸들을 사용 하 여 게임 세션에 참가 하는 동안 멤버 로비 세션에는 확인을 조회 합니다.
5. 멤버 로비 세션에 있는 경우 게임 세션 액세스할 수 있습니다.

## <a name="join-an-mpsd-session-from-a-title-activation"></a>제목 활성화에서 MPSD 세션에 참가합니다

사용자가 친구의 활동 참여 하거나 Xbox 셸 UI를 사용 하 여 초대를 선택, 제목에 연결할 사용자가 원하는 어떤 세션을 나타내는 매개 변수를 사용 하 여 활성화 됩니다. 제목이이 활성화를 처리 하 고 해당 세션에 사용자를 추가 해야 합니다.

제목 수행 해야 하는 단계는 다음과 같습니다.

1.  에 대 한 이벤트 처리기를 구현 합니다 **CoreApplicationView.Activated 이벤트**합니다. 이 이벤트의 제목에 대 한 정품 인증에 알립니다.
2.  검사 된 처리기가 실행 되는 **IActivatedEventArgs.Kind 속성**합니다. 프로토콜 설정 된 경우 이벤트 인수를 캐스트 **ProtocolActivatedEventArgs 클래스**합니다.
3.  검토 합니다 **ProtocolActivatedEventArgs** 개체입니다. URI에 지정 된 경우는 **ProtocolActivatedEventArgs.Uri 속성** (허용 된 초대에 따름) 하거나 inviteHandleAccept 일치 activityHandleJoin (셸 UI 통해 조인에 따름), 쿼리 문자열을 구문 분석 또는 키/값 쌍을 사용 하 여 일반 URI 쿼리 문자열로 형식이 지정 되는 URI의 다음 필드를 추출 합니다.
    -   에 대 한 초대를 수락 된 경우:
        1.  핸들
        2.  invitedXuid
        3.  senderXuid
    -   조인 합니다.
        1.  핸들
        2.  joinerXuid
        3.  joineeXuid

4.  호출을 포함 해야 하는 타이틀의 멀티 플레이 code를 시작 합니다 **MultiplayerService.EnableMultiplayerSubscriptions 메서드**합니다.
5.  로컬 **MultiplayerSession 클래스** 개체를 사용 하는 **MultiplayerSession 생성자 (XboxLiveContext)** 합니다.
6.  호출 된 **MultiplayerSession.Join 메서드 (String, Boolean, Boolean)** 세션에 참가 하기. 조인 활성으로 설정 되도록 다음 매개 변수 설정을 사용 합니다.
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  호출 된 **MultiplayerSession.SetSessionChangeSubscription 메서드** 세션 연결한 후 변경 되 면 자격 증명 입력을 탭 하 합니다.
8.  호출을 **MultiplayerService.WriteSessionByHandleAsync 메서드**, 3 단계에 설명 된 대로 획득 핸들을 사용 합니다. 이제 사용자 세션의 구성원이 며 게임에 연결 하는 세션에서 데이터를 사용할 수 있습니다.

## <a name="set-the-users-current-activity"></a>사용자의 현재 활동 설정

제목의 Xbox 대시보드에서 사용자 경험에서 사용자의 현재 동작이 표시 됩니다. 세션을 통해 또는 제목 활성화를 통해 사용자에 대 한 작업을 설정할 수 있습니다. 후자의 경우, 사용자가 결혼 정보 회사 연결을 통해 또는 게임을 시작 하 여 세션을 입력 합니다.

| 참고                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 세션을 통해 설정 하는 작업을 호출 하 여 삭제할 수는 **MultiplayerService.ClearActivityAsync 메서드**합니다. |

사용자의 현재 활동을 호출 하 여 제목으로 세션을 설정 하는 **MultiplayerService.SetActivityAsync 메서드**, 세션에 대 한 세션 파일에 대 한 참조를 전달 합니다.

제목 활성화를 통해 사용자의 현재 활동을 설정 하려면 참조 **방법: 제목 활성화에서 MPSD 세션에 참가할**합니다.

## <a name="update-an-mpsd-session"></a>MPSD 세션을 업데이트 합니다.

| 참고                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 제목 멀티 플레이어 API를 사용 하 여 기존 세션을 업데이트 하는 경우, 세션 작성에 대 한 호출 될 때까지 로컬 복사본을 사용 하 여 작동 합니다. |

기존 세션을 업데이트 하려면 제목 수행 해야 합니다.

1.  호출 하 여 예를 들어, 필요에 따라 현재 세션에 변경 된 **MultiplayerSession.Leave 메서드**합니다.
2.  모든 변경 되 면 MPSD, 이러한 방법 중 하나를 사용 하 여 로컬에 변경 내용 쓰기:

    -   **MultiplayerService.WriteSessionAsync 메서드**
    -   **MultiplayerService.WriteSessionByHandleAsync 메서드**합니다.
    -   **MultiplayerService.TryWriteSessionAsync 메서드**
    -   **MultiplayerService.TryWriteSessionByHandleAsync Method**

    쓰기 모드를 설정 **SynchronizedUpdate** 도 수정할 수 있도록 공유 부분에 다른 제목을 작성 합니다. 참조 [세션 동기화 업데이트](multiplayer-session-directory.md) 자세한 내용은 합니다.

    Write 메서드 서버로 조인을 쓰고 다른 세션 멤버와 해당 콘솔의 보안 장치 주소 (SDAs)를 검색 하는 최신 세션을 가져옵니다. 이러한 콘솔 간의 네트워크 연결을 설정 하는 방법에 대 한 자세한 내용은 참조 하세요. **Xbox One에서 Winsock 소개**합니다.

3.  이전 로컬 세션 개체를 삭제 하 고 이후 작업 최신 알려진된 세션 상태에 기반한 있도록 새로 검색된 된 세션 개체를 사용 합니다.

## <a name="leave-an-mpsd-session"></a>MPSD 세션을 그대로 둡니다.

제목 세션을 종료 하는 사용자를 허용 하려면 다음을 수행 해야 합니다.

1.  호출 된 **MultiplayerSession.Leave 메서드** 게임 세션에 대 한 합니다.
2.  에 설명 된 대로 MPSD, 게임 세션 업데이트 **방법: 멀티 플레이 세션 업데이트**합니다.
3.  필요한 경우 호출 **둡니다** 로비 세션에 대 한 메서드 및 해당 세션을 업데이트 합니다.
4.  로비 세션에 대 한 필요한 경우 등록을 취소 하 여 멀티 플레이어 API를 종료 합니다 **RealTimeActivityService.MultiplayerSubscriptionsLost 이벤트** 하며  **RealTimeActivityService.MultiplayerSessionChanged 이벤트**합니다.

다음 순서도에서는 세션을 유지 하 고 종료 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>결혼 정보 회사 연결 하는 동안 열려 있는 세션 슬롯을 채웁니다

결혼 정보 회사 연결 하는 동안 티켓 세션에서 열려 슬롯 맞게 제목을 다음과 유사한 단계를 수행 해야 합니다.

1.  결혼 정보 회사 연결 중에 만들어진 티켓 세션에 대 한 최신 세션 상태에 액세스 합니다.
2.  로비 세션에서 사용할 수 있는 플레이어 게임 플레이를 추가 합니다.
3.  티켓 세션 전체 인지 확인 합니다.
4.  세션 꽉 차면 게임 플레이 계속 합니다.
5.  세션이 아직 전체 없는 경우에 설명 된 대로 일치 티켓 만들기 **방법: 일치 티켓을 만드는**합니다. 사용 하 여 티켓을 만들어야 합니다 *preserveSession* 매개 변수가 Always로 설정 합니다.
6.  결혼 정보 회사 연결을 사용 하 여 계속 합니다. 참조 [SmartMatch 결혼 정보 회사 연결을 사용 하 여](using-smartmatch-matchmaking.md)입니다.

다음 순서도 결혼 정보 회사 연결 하는 동안 열려 있는 세션 슬롯을 입력 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>일치 티켓 만들기

매 치메이 킹 scout 일치 티켓을 만들려면 다음을 해야 합니다.

1.  호출 된 **MatchmakingService.CreateMatchTicketAsync 메서드**티켓 세션에 대 한 참조를 전달 합니다. 메서드 티켓 세션 MPSD을 읽고 사용자에 대해 매 치메이 킹 세션에서 시작 합니다. 메서드 호출을 내부적으로 **POST (/serviceconfigs/ {서비스 안내} /hoppers/ {hoppername})** 합니다.
2.  설정 된 *preserveSession* Never 결혼 정보 회사 연결 서비스는 새 세션이 나 다른 기존 세션에 세션의 멤버에 일치 하는 경우 매개 변수입니다. 설정 된 *preserveSession* 제목을 게임 플레이 계속 하려면 티켓 세션으로 기존 게임 세션을 다시 사용할 수 있도록 항상 매개 변수입니다. 결혼 정보 회사 연결 서비스는 제출 된 세션이 유지 되 고 일치 하는 모든 플레이어는 해당 세션에 추가 됩니다 확인 수 있습니다.

3.  사용 하 여는 **CreateMatchTicketResponse.EstimatedWaitTime 속성** 반환 합니다 **CreateMatchTicketResponse 클래스** 매 치메이 킹 시간의 사용자 기대치를 설정할 개체입니다.
4.  사용 된 **CreateMatchTicketResponse.MatchTicketId 속성** 티켓을 삭제 하 여 필요한 경우 세션에 대해 매 치메이 킹을 취소 하려면 응답 개체에서 반환 합니다. 삭제 사용 하 여 티켓을 **MatchmakingService.DeleteMatchTicketAsync 메서드**합니다.

## <a name="get-match-ticket-status"></a>일치 티켓 상태 가져오기

제목 일치 티켓 상태를 검색 하려면 다음을 수행 해야 합니다.

1.  가져올는 **MultiplayerSession 클래스** 티켓 세션에 대 한 개체입니다.
2.  사용 하 여는 **MultiplayerSession.MatchmakingServer 속성** 액세스 하는 **MatchmakingServer 클래스** 결혼 정보 회사에서 사용 되는 개체입니다.
3.  확인 합니다 **MatchmakingServer** 일치 항목이 발견 된 경우 세션 및 대상 세션 참조에 대 한 일반적인 대기 시간, 매 치메이 킹 프로세스 상태를 확인 하는 개체입니다.
