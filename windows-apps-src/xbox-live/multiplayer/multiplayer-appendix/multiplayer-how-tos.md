---
title: 멀티 플레이 방법
author: KevinAsgari
description: Xbox Live 멀티 플레이 2015의 일반적인 작업을 구현 하는 방법을 설명 합니다.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.author: kevinasg
ms.date: 08/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 멀티 플레이 2015, xbox
ms.localizationpriority: medium
ms.openlocfilehash: f800afb70f4f32586f5e7db38665a77719792c1a
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4538336"
---
# <a name="multiplayer-how-tos"></a>멀티 플레이 방법

이 항목에서는 멀티 플레이 2015를 사용 하 여 관련 된 특정 작업을 구현 하는 방법에 대 한 정보를 포함 합니다.

* MPSD 세션 변경 알림을 등록합니다
* MPSD 세션 만들기
* MPSD 세션에 대 한에 중재자 설정
* 제목 정품 인증 관리
* 사용자가 참여할 확인
* 게임 초대 보내기
* 로비 세션에서 게임 세션에 참가
* 제목 활성화에서 MPSD 세션에 참가
* 사용자의 현재 동작을 설정 합니다.
* MPSD 세션 업데이트
* MPSD 세션 종료
* 매치 메이 킹 하는 동안 열려 세션 슬롯 채우기
* 일치 티켓 만들기
* 일치 티켓 상태 가져오기

## <a name="subscribe-for-mpsd-session-change-notifications"></a>MPSD 세션 변경 알림을 등록합니다

| 참고                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 세션에 대 한 변경 내용에 대 한 구독에 연결 된 플레이어 세션에서 활성 필요 합니다. ConnectionRequiredForActiveMembers 필드 설정 해야 세션에 대 한 /constants/system/capabilities 개체의 true입니다. 이 필드는 일반적으로 세션 템플릿에서 설정 됩니다. [MPSD 세션 템플릿](multiplayer-session-directory.md)를 참조 하세요. |



MPSD 세션 변경 알림을 받도록, 제목 아래 절차를 수행할 수 있습니다.

1.  동일한 사용자가 모든 호출에 대해 동일한 **XboxLiveContext 클래스** 개체를 사용 합니다. 구독이이 개체의 수명에 연결 됩니다. 여러 로컬 사용자 인 경우 각 사용자에 대 한 별도 **XboxLiveContext** 개체를 사용 합니다.
2.  **RealTimeActivityService.MultiplayerSessionChanged 이벤트** 와 **RealTimeActivityService.MultiplayerSubscriptionsLost 이벤트**에 대 한 이벤트 처리기를 구현 합니다.
3.  여러 사용자에 대 한 변경 내용에 가입 하는 경우 불필요 한 작업을 방지 하기 위해 **MultiplayerSessionChanged** 이벤트 처리기에 코드를 추가 합니다. **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성** 및 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성**을 사용 합니다. 이러한 속성을 사용 하 여 표시 마지막 변경 추적 및 이전 변경 무시 하 고 있습니다.
4.  구독을 허용 하도록 **MultiplayerService.EnableMultiplayerSubscriptions 메서드** 를 호출 합니다.
5.  로컬 세션 개체 및 가입 활성 해당 세션을 만듭니다.
6.  각 사용자에 대 한 호출을 **MultiplayerSession.SetSessionChangeSubscription 메서드**를 전달 받기를 세션 변경 유형을 확인 합니다.
7.  이제 쓰려고 세션 MPSD에 설명 된 대로 **하는 방법: 업데이트 멀티 플레이어 세션**합니다.

다음 순서도 Multplayer이이 절차에 설명 된 이벤트를 구독 하 여 시작 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>중복 된 세션 변경 알림을 구문 분석

여러 사용자가 동일한 세션에 대 한 알림을 구독 인 경우 해당 세션에 모든 변경 각 사용자에 대 한 어깨 탭을 트리거합니다. 다음 중 하나를 제외한 모든 중복 됩니다. 제목을; 알림을 이미 되는 모든 변경 내용을 무시 해야 알림에 세션의 모든 사용자를 구독 하는 제목 권장 여전히는 동안 분기 및 ChangeNumber 속성을 사용 하 여 수행할 수 있습니다.

여러 어깨 누르기를 검색 하려면 제목을 해야 합니다.

-   본 각 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성** 값에 대 한 최신 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성**을 저장 합니다.
-   어깨 탭에 해당 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성**에 표시 되는 마지막 보다 높은 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성** 을 처리 하 고 업데이트 합니다 최신 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성**입니다.
-   어깨 탭 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성**에 대 한 높은 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성이** 없으면 건너뜁니다. 해당 변경 이미 처리 되었습니다.

| 참고                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성** 값은 세션을 받지 **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성**의해 추적 해야 합니다. **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch 속성** 은 세션의 수명 동안 변경 (및 되돌리려면 **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber 속성** ) 값 . |

## <a name="create-an-mpsd-session"></a>MPSD 세션 만들기


| 참고                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 기본적으로 MPSD 세션에 연결 하는 첫 번째 멤버 때 만들어집니다. 제목 논리에서 없거나 가입 시 존재 하지 제목을 기대 하는 경우 세션 업데이트 하는 동안 적절 한 쓰기 모드 값 쓰기 메서드에 전달할 수 것입니다. |



제목 새 세션을 만들려면 다음을 수행 해야 합니다.

1.  새 **XboxLiveContext 클래스** 개체를 만듭니다. 타이틀 한 번에이 개체를 만듭니다., 저장 및 전체 소스 코드에서 필요에 따라 다시 사용 합니다. 세션 구독과 함께 작업 하는 경우에 특히 정확히 동일한 컨텍스트를 사용 하는 데 필요한 것입니다.
2.  MPSD는 새 세션을 만드는 데 필요한 모든 세션 데이터를 준비 하는 새 **MultiplayerSession 클래스** 개체를 만듭니다.
3.  MPSD 세션 쓰기 전에 필요한 변경 내용을 확인 합니다. 예를 들어 구성원 **MultiplayerSession.Join 메서드**를 호출 하 여 세션에 참가 하면 클라이언트 MPSD 세션 업데이트에 대 한 호출에 가입할 수를 알려주는 숨겨진된 로컬 요청 데이터를 추가 합니다.
4.  로컬 변경 작업을 마쳤으면 연락처를 작성 MPSD에 설명 된 대로 **하는 방법: 업데이트 멀티 플레이어 세션**합니다.
5.  MPSD에서 입력 필드를 사용 하 여 새 **MultiplayerSession** 개체를 받습니다.
6.  그 이후에 새 세션 개체를 사용 하 고 새 세션을 만들려면 숨겨진된 요청을 포함 하는 이전 복사본을 삭제 합니다.

### <a name="example"></a>예

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Windows Dev Center configuration
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


## <a name="set-an-arbiter-for-an-mpsd-session"></a>MPSD 세션에 대 한에 중재자 설정




제목에 중재자 이미 만든 세션에 대 한 설정 하려면 다음 절차를 사용 합니다.

| 참고                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 멤버 세션에 참가 있고가 보안 장치 주소가 포함 될 때까지 (잠재적인 호스트) 멤버에 대 한 장치 토큰은 사용할 수 없습니다. |

1.  **MultiplayerSession.Members 속성**을 사용 하 여 호스트 후보는 MPSD에서 장치 토큰을 검색 합니다.

| 참고                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 스마트 매치 메이 킹 하 여 세션이 만들어진 클라이언트 **MultiplayerSession.HostCandidates 속성**을 통해 MPSD에서 사용할 수 있는 호스트 후보를 사용할 수 있습니다. |

2.  호스트 목록에서 필요한 호스트를 선택 합니다.
3.  MPSD의 로컬 캐시에서 장치 토큰을 설정 하려면 **MultiplayerSession.SetHostDeviceToken 메서드** 를 호출 합니다. 호스트 디바이스 토큰 설정에 대 한 호출에 성공 하면 로컬 장치 토큰 호스트의 토큰을 대체 합니다.
4.  HTTP/412 상태 코드는 호스트 장치 토큰을 설정 하려고 할 때 수신 되 면 세션 데이터를 쿼리 하 고 로컬 콘솔에 대 한 호스트 장치 토큰 있는지 확인 합니다. 로컬 콘솔에 대 한 없으면 중재자도 지정 된 다른 콘솔.

| 참고                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 클라이언트는 HTTP/412 표준 오류를 나타내지 않는 때문 기타 HTTP 코드와 별개로 HTTP/412 상태 코드를 처리 해야 합니다. 상태 코드에 대 한 자세한 [멀티 플레이어 세션 상태 코드](multiplayer-session-status-codes.md)를 참조 하세요. |

5.  업데이트 MPSD에서 세션에 설명 된 대로 **하는 방법: 업데이트 멀티 플레이어 세션**합니다.

| 참고                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 더 나은 알고리즘이 없는 경우 클라이언트에는 각 호스트 후보를 아직 완료가 아무도 호스트로 설정 하려고 하는 최대 알고리즘을 구현할 수 있습니다. 자세한 내용은 [세션 중재자](mpsd-session-details.md)를 참조 하세요. |

## <a name="manage-title-activation"></a>제목 정품 인증 관리

Xbox One 프로토콜 활성화 하는 동안 **CoreApplicationView.Activated 이벤트** 를 발생 합니다. 멀티 플레이 API의 컨텍스트에서 사용자가 초대를 수락 하거나 다른 사용자를 가입 하는 경우이 이벤트는 발생 합니다. 이러한 작업 트리거 조인 사용자 대상 사용자를 사용 하 여 게임 플레이에 연결 하 여 제목에 반응 해야 하는 활성화 합니다.

| 참고                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| 타이틀 언제 든 지 새로운 활성화 인수가 되 고 길이 대해 코딩할 수 없습니다. |

제목 제목 활성화를 처리 하려면 다음 기본 단계를 수행 해야 합니다.

1.  **CoreApplicationView.Activated 이벤트**에 대 한 이벤트 처리기를 설정 합니다. 이 처리기 제목 이미 실행 중인 경우에 될 때마다 발생 하는 프로토콜 활성화를 트리거합니다.
2.  제목 활성화 시 세션을 시작 하 고 세션 변경 알림을 등록 합니다. 참조 **하는 방법: MPSD 세션 변경 알림을 등록**합니다.
3.  사용자가 활성 세션에 가입 합니다. 참조 **하는 방법: 제목 활성화에서 MPSD 세션에 참가할**합니다.
4.  프로필 UI 통해 노출 활동 세션 로비 세션을 설정 합니다. 참조 **하는 방법: 사용자의 현재 동작을 설정**합니다.
5.  사용자가 활성 게임 세션에 가입 합니다. 이제 사용자 피어에 연결 하 고 게임 플레이 또는 로비 입력할 수 있습니다.

다음 순서도 제목 활성화를 처리 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>사용자가 참여할 확인

사용자가 참여할을 하려면 제목 다음을 수행 해야 합니다.

1.  세션 개체를 만들고 필요에 따라 특성을 수정 합니다.
2.  사용자가 활성 세션에 가입 합니다. 참조 **하는 방법: 제목 활성화에서 MPSD 세션에 참가할**합니다.
3.  사용자가 세션 중재자 지정한 경우 확인 합니다.
4.  사용자가 중재자 없는 경우 7 단계로 이동 합니다.
5.  사용자가 중재자 **MultiplayerSession.SetHostDeviceToken 메서드**를 호출 합니다.
6.  **MultiplayerService.TryWriteSessionAsync 메서드**를 호출 하 여 세션을 작성 하려고 합니다.
7.  현재 세션으로 세션을 설정 합니다. 참조 **하는 방법: 사용자의 현재 동작을 설정**합니다.

다음 순서도 사용자가 게임을 하는 동안 다른 플레이어에 참여할 수 있도록 하기 위해 단계를 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>게임 초대 보내기

제목 같은 방법으로 게임 초대를 보내려면 플레이어를 사용할 수 있습니다.

-   로비 세션에 초대를 보냅니다.
-   일반 Xbox 플랫폼을 사용 하 여 초대 게임 세션 참조를 사용 하 여 UI를 초대를 보냅니다.

플레이어에 대 한 게임 초대를 보내려면 제목 다음을 수행 해야 합니다.

1.  게임 초대 플레이어 참여할 확인 합니다. 참조 **하는 방법: 사용자가 참여할 확인**.
2.  초대 UI를 사용 하 여 또는 초대 로비 세션을 통해 보낼 수 있는지 확인 합니다.
3.  로비 세션을 사용 하 여 **MultiplayerService.SendInvitesAsync 메서드**를 호출 하 여 초대를 보냅니다. 이 메서드는 게임에서 UI 명단 **SystemUI.ShowPeoplePickerAsync 메서드** 또는 **PartyChat.GetPartyChatViewAsync 메서드**를 사용 하 여 빌드가 필요할 수 있습니다.
4.  초대 UI를 사용 하는 경우 초대 UI를 표시 하도록 **SystemUI.ShowSendGameInvitesAsync 메서드** 를 호출 합니다.
5.  로컬 플레이어에 대 한 원격 플레이어 가입 후 **RealTimeActivityService.MultiplayerSessionChanged 이벤트** 를 처리 합니다.
6.  원격 플레이어에 대 한 제목 활성화 코드를 구현 합니다. 참조 **하는 방법: 제목 정품 인증 관리**합니다.

다음 순서도 초대를 전송 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>로비 세션에서 게임 세션에 참가

Windows 10 장치에서 게임 플레이 세션 있어야 합니다 `userAuthorizationStyle` 큰 세션 하지 않은 접근 권한 값을 **true** 로 설정 합니다. 이 `joinRestriction` 속성을 "none"는 세션을 직접 공개적으로 참여할 수 없습니다 즉 수 없습니다.

일반적인 시나리오는 플레이어를 수집 하 고 다음 세션 또는 매치 메이 킹 세션에는 게임 플레이에 해당 플레이어 이동에 대 한 로비 세션을 만드는 것입니다. 게임 플레이 세션을 공개적으로 참여할 수 없는 경우 게임 클라이언트 충족 하지 않는 한 게임 플레이 세션에 참가할 수 있지만 `joinRestriction` 설정은이 시나리오에 대 한 대부분의 경우 너무 제한적입니다.

솔루션 전송 핸들을를 사용 하 여 로비 세션 및 게임 세션에 연결 됩니다.  제목 다음을 수행 하 여이 수행할 수 있습니다.

1. 게임 세션을 만들 때 사용 하 여는 `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` API 로비 세션 및 게임 세션에 연결 하는 전송 핸들을 만들 수 있습니다.
2. 게임 세션의 세션 참조 하는 대신 로비 세션에서 전송 핸들 GUID를 저장 합니다.
3. 제목을 게임 세션에 로비 세션에서 멤버를 이동 하려는 경우 각 클라이언트 사용 로비 세션에서 전송 핸들 사용 하 여 게임 세션에 참가 하 고 `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API입니다.
4. MPSD 로비 세션 전송 핸들을 사용 하 여 게임 세션에 참가 하는 구성원은 대기실 세션에도 확인을 조회 합니다.
5. 멤버 로비 세션에 있는 게임 세션에 액세스할 수 없게 됩니다.

## <a name="join-an-mpsd-session-from-a-title-activation"></a>제목 활성화에서 MPSD 세션에 참가

사용자가 가입 친구의 활동 또는 Xbox 셸 UI를 사용 하 여 초대를 수락, 제목 어떤 세션 사용자 참여를 설정할지 여부를 나타내는 매개 변수를 사용 하 여 활성화 됩니다. 제목이이 활성화를 처리 하 고 해당 세션에 사용자를 추가 해야 합니다.

다음은 제목 따라야 하는 단계입니다.

1.  **CoreApplicationView.Activated 이벤트**에 대 한 이벤트 처리기를 구현 합니다. 이 이벤트는 제목에 대 한 정품 인증에 알립니다.
2.  처리기가 발생 하는 경우 **IActivatedEventArgs.Kind 속성**을 확인 합니다. 프로토콜에 설정 된 경우 **ProtocolActivatedEventArgs 클래스**에 대 한 이벤트 인수를 캐스팅 합니다.
3.  **ProtocolActivatedEventArgs** 개체를 검사 합니다. URI에 표시 된 경우 **ProtocolActivatedEventArgs.Uri 속성** (허용 된 초대에 해당) 각 inviteHandleAccept 일치 또는 activityHandleJoin (셸 UI 통해 가입에 해당)은 URI의 쿼리 문자열을 구문 분석 다음 필드를 추출 하는 키/값 쌍을 사용 하 여 일반 URI 쿼리 문자열 형식의:
    -   허용 된 초대에 대 한:
        1.  핸들
        2.  invitedXuid
        3.  senderXuid
    -   가입:
        1.  핸들
        2.  joinerXuid
        3.  joineeXuid

4.  **MultiplayerService.EnableMultiplayerSubscriptions 메서드**호출을 포함 해야 하는 타이틀의 멀티 플레이 코드를 시작 합니다.
5.  **MultiplayerSession (XboxLiveContext) 생성자**를 사용 하 여 로컬 **MultiplayerSession 클래스** 개체를 만듭니다.
6.  세션에 참가할 **MultiplayerSession.Join 메서드 (문자열, 부울, 부울)를** 호출 합니다. 다음 매개 변수 설정을 사용 하 여 조인을 활성으로 설정 되도록 합니다.
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  가입 후 세션 변경 될 때 어깨 탭 **MultiplayerSession.SetSessionChangeSubscription 메서드** 를 호출 합니다.
8.  호출 하 여 **MultiplayerService.WriteSessionByHandleAsync 메서드**3 단계에 설명 된 대로 획득 핸들을 사용 하 여 합니다. 이제 사용자 세션의 구성원을 게임에 연결할 세션에서 데이터를 사용할 수 있습니다.

## <a name="set-the-users-current-activity"></a>사용자의 현재 동작을 설정 합니다.

사용자의 현재 활동 제목에 대 한 Xbox 대시보드 사용자 환경에 표시 됩니다. 세션을 통해 또는 제목 활성화를 통해 사용자에 대 한 작업을 설정할 수 있습니다. 후자의 경우 사용자가 연결을 통해 또는 게임을 시작 하 여 세션을 입력 합니다.

| 참고                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **MultiplayerService.ClearActivityAsync 메서드**를 호출 하 여 세션을 통해 설정 된 활동을 삭제할 수 있습니다. |

세션을 사용자의 현재 동작을 설정 하려면 제목 **MultiplayerService.SetActivityAsync 메서드**를 호출 세션에 대 한 세션 참조를 전달 합니다.

제목 활성화를 통해 사용자의 현재 동작을 설정 하려면 다음을 참조 **하는 방법: 제목 활성화에서 MPSD 세션에 참가할**합니다.

## <a name="update-an-mpsd-session"></a>MPSD 세션 업데이트

| 참고                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 타이틀 멀티 플레이 API를 사용 하 여 기존 세션을 업데이트 하면 세션을 작성 하는 호출 될 때까지 로컬 복사본을 사용 하 여 작동 해야 합니다. |

기존 세션을 업데이트 하려면 제목 해야 합니다.

1.  변경할 필요에 따라 현재 세션 예를 들어 **MultiplayerSession.Leave 메서드**를 호출 합니다.
2.  모든 변경 사항이 있을 경우 로컬 변경 내용을 다음이 방법 중 하나를 사용 하 여 MPSD를 씁니다.

    -   **MultiplayerService.WriteSessionAsync 메서드**
    -   **MultiplayerService.WriteSessionByHandleAsync 방법**입니다.
    -   **MultiplayerService.TryWriteSessionAsync 메서드**
    -   **MultiplayerService.TryWriteSessionByHandleAsync 메서드**

    다른 타이틀을 수정할 수도 있는 공유 부분을 작성 하는 경우 쓰기 모드 **SynchronizedUpdate** 를 설정 합니다. 자세한 내용은 [세션 업데이트 동기화를](multiplayer-session-directory.md) 참조 하세요.

    쓰기 메서드 가입 서버에 쓰고 다른 세션 멤버와가 콘솔의 보안 장치 주소 (SDAs)를 검색 하는 최신 세션을 가져옵니다. 이러한 콘솔 간에 네트워크 연결을 설정 하는 방법에 대 한 자세한 내용은 **Xbox One에서 Winsock 소개**를 참조 하세요.

3.  이전 로컬 세션 개체를 무시 하 고 이후 작업은 최신 알려진된 세션 상태에 따라 새로 검색된 된 세션 개체를 사용 합니다.

## <a name="leave-an-mpsd-session"></a>MPSD 세션 종료

사용자 세션에서 나갈 수 있도록 제목을 다음을 수행 해야 합니다.

1.  게임 세션에 대 한 **MultiplayerSession.Leave 메서드** 를 호출 합니다.
2.  업데이트 MPSD에서 게임 세션에 설명 된 대로 **하는 방법: 업데이트 멀티 플레이어 세션**합니다.
3.  로비 세션에 대 한 **종료** 메서드를 호출 하 고 해당 세션을 업데이트 합니다.
4.  로비 세션에 대 한 필요한 경우 **RealTimeActivityService.MultiplayerSubscriptionsLost 이벤트** 와 **RealTimeActivityService.MultiplayerSessionChanged 이벤트**를 등록 취소 하 여 멀티 플레이 API를 종료 합니다.

다음 순서도 세션을 종료 하는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>매치 메이 킹 하는 동안 열려 세션 슬롯 채우기

매치 메이 킹 티켓 세션에서 열린 슬롯 채우기 제목 다음과 유사한 단계를 수행 해야 합니다.

1.  매치 메이 킹 만들어진 티켓 세션에 대 한 최신 세션 상태에 액세스 합니다.
2.  로비 세션에서 사용할 수 있는 플레이어 게임에 대 한 추가 합니다.
3.  티켓 세션 전체 인지 확인 합니다.
4.  세션은 전체, 게임 플레이 계속 합니다.
5.  세션 아직 전체 없는 경우에 설명 된 대로 일치 티켓을 만듭니다 **하는 방법: 일치 티켓 만들기**합니다. 항상 설정 *preserveSession* 매개 변수를 사용 하 여 티켓 만들기를 사용 해야 합니다.
6.  매치 메이 킹 계속 진행 합니다. [스마트 매치를 사용 하 여](using-smartmatch-matchmaking.md)참조 하세요.

다음 순서도을 연결 하는 동안 열려 세션 슬롯을 채우는 방법을 보여 줍니다.

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>일치 티켓 만들기

일치 티켓을 만들려면 매치 메이 킹 정찰 해야 합니다.

1.  호출 하 여 **MatchmakingService.CreateMatchTicketAsync 메서드**티켓 세션에 대 한 참조를 전달 합니다. 메서드는 티켓 세션 MPSD에서 읽고 세션에서 사용자에 대 한 연결을 시작 합니다. 내부적으로 메서드가 **POST (/serviceconfigs/ {서비스 안내} /hoppers/ {hoppername})를**호출 합니다.
2.  매치 메이 킹 서비스를 새 세션 세션이 나 다른 기존 세션의 멤버를 일치 하는 경우 Never를 *preserveSession* 매개 변수를 설정 합니다. 제목을 게임 플레이 계속 하려면 티켓 세션으로 기존 게임 세션을 다시 사용할 수 있도록 항상 *preserveSession* 매개 변수를 설정 합니다. 제출 된 세션은 유지 하 고 해당 세션에 일치 하는 모든 플레이어 추가 매치 메이 킹 서비스 확인 한 다음 수 있습니다.

3.  매치 메이 킹 시간의 사용자 설정에 **CreateMatchTicketResponse 클래스** 개체에 반환 된 **CreateMatchTicketResponse.EstimatedWaitTime 속성** 을 사용 합니다.
4.  티켓을 삭제 하 여 필요한 경우 세션에 대 한 연결을 취소 하는 응답 개체에 반환 된 **CreateMatchTicketResponse.MatchTicketId 속성** 을 사용 합니다. 티켓 삭제 **MatchmakingService.DeleteMatchTicketAsync 메서드**를 사용합니다.

## <a name="get-match-ticket-status"></a>일치 티켓 상태 가져오기

타이틀 일치 티켓 상태를 검색 하려면 다음을 수행 해야 합니다.

1.  티켓 세션에 대 한 **MultiplayerSession 클래스** 개체를 가져옵니다.
2.  **MultiplayerSession.MatchmakingServer 속성** 을 사용 하 여 연결에 사용 되는 **MatchmakingServer 클래스** 개체에 액세스 합니다.
3.  일치를 찾을 경우 세션 및 대상 세션 참조에 대 한 일반적인 대기 시간, 매치 메이 킹 프로세스의 상태를 확인 하려면 **MatchmakingServer** 개체를 선택 합니다.
