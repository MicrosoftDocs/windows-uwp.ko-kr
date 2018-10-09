---
title: XIM (C#)를 사용 하 여
author: KevinAsgari
description: C#을 사용 하 여 Xbox 통합 멀티 플레이어 (XIM)를 사용 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 04/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 통합된 멀티 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: 7cb15206469abf96d8c88490c35c86b8c5197e93
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4414770"
---
# <a name="using-xim-c"></a>XIM (C#)를 사용 하 여

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

XIM의 C# API 사용에 대 한 간략 한 연습입니다. C + +를 통해 XIM에 액세스 하려는 게임 개발자가 [사용 하 여 XIM (c + +)](using-xim.md)보일 것입니다.
- [XIM (C#)를 사용 하 여](#using-xim-c)
    - [필수 구성 요소](#prerequisites)
    - [초기화 및 시작](#initialization-and-startup)
    - [비동기 작업 및 상태 변경 내용 처리](#asynchronous-operations-and-processing-state-changes)
    - [기본 IXimPlayer 처리](#basic-iximplayer-handling)
    - [참가 하도록 친구를 사용 하도록 설정 하 고 초대](#enabling-friends-to-join-and-inviting-them)
    - [기본 매치 메이 킹 및 다른 사용자와 다른 XIM 네트워크로 이동 합니다.](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [디버깅을 위해 매치 메이 킹 NAT 규칙을 사용 하지 않도록 설정](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [XIM 네트워크를 종료 하 고 정리](#leaving-a-xim-network-and-cleaning-up)
    - [메시지 보내기 및 받기](#sending-and-receiving-messages)
    - [데이터 경로 품질 평가](#assessing-data-pathway-quality)
    - [플레이어의 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.](#sharing-data-using-player-custom-properties)
    - [네트워크 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.](#sharing-data-using-network-custom-properties)
    - [플레이어 당 기술을 사용 하 여 매치 메이 킹](#matchmaking-using-per-player-skill)
    - [싱글 플레이어 역할을 사용 하 여 매치 메이 킹](#matchmaking-using-per-player-role)
    - [XIM은 플레이어 팀과 함께 작동 하는 방식](#how-xim-works-with-player-teams)
    - [채팅 작업](#working-with-chat)
    - [플레이어 음소거](#muting-players)
    - [팀이 플레이어를 사용 하 여 채팅 대상 구성](#configuring-chat-targets-using-player-teams)
    - [플레이어 슬롯 ("백필" 매치 메이 킹)의 배경 자동 채우기](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [참여할 네트워크를 쿼리합니다.](#querying-joinable-networks)

## <a name="prerequisites"></a>필수 구성 요소

XIM을 사용 하 여 코딩을 시작 하기 전에 가지 두 가지 필수 조건이 있습니다.

1. 표준 멀티 플레이어 네트워킹 기능을 사용 하 여 앱의 AppXManifest을 구성 하 고 필요한 트래픽을 XIM 사용 패턴 템플릿을 선언 하는 네트워크 매니페스트 부분을 구성 해야 합니다.

    AppXManifest 기능과 네트워크 매니페스트 플랫폼 설명서;의 각 섹션에서 자세히 설명 되어 있습니다. 일반적인 XIM 특정 XML은 붙여 [XIM 프로젝트 구성](xim-manifest.md)에 제공 됩니다.

1. 응용 프로그램 id 정보를 사용할 수 있는 두 가지 해야 합니다.

    * 할당 된 Xbox Live 타이틀 id.
    * Xbox Live 서비스에 대 한 액세스에 대 한 응용 프로그램을 프로 비전의 일부로 제공 된 서비스 구성 ID입니다.

    이러한 획득 대 한 자세한 내용은 Microsoft 담당자를 참조 하세요. 이러한 가지 정보를 초기화 하는 동안 사용 됩니다.

## <a name="initialization-and-startup"></a>초기화 및 시작

Xbox Live 서비스 구성 ID 문자열을 사용 하 여 XIM 정적 클래스 속성을 초기화 하 여 라이브러리와 상호 작용을 시작 하 고 ID 번호를 제목입니다. 또한 Xbox 서비스 API를 사용 하는 경우 편리할 수 있습니다 것에서이 검색 하려면 `Microsoft.Xbox.Services.XboxLiveAppConfiguration`. 다음 예제에서는 값 이미에 있는 'myServiceConfigurationId' 및 'myTitleId' 변수 각각 있다고 가정 합니다.

```cs
XboxIntegratedMultiplayer.ServiceConfigurationId = myServiceConfigurationId;
XboxIntegratedMultiplayer.TitleId = myTitleId;
```

앱 참여를 전달할 현재 로컬 장치에 있는 모든 사용자에 대 한 Xbox 사용자 ID 문자열을 검색 해야 초기화 되 면는 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 를 호출 합니다. 다음 샘플 코드는 단일 사용자가 재생할 의도 표현 컨트롤러 단추를 누르고 사용자와 관련 된 Xbox 사용자 ID 문자열을 'myXuid' 변수에 이미 검색 있다고 가정 합니다.

```cs
XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds(new List<string>() { myXuid });
```

호출 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 즉시 XIM 네트워크에 추가 되어야 하는 로컬 사용자와 관련 된 Xbox 사용자 Id를 설정 합니다. Xbox 사용자 Id의이 목록에서에서 사용할 모든 향후 네트워크 작업 다른 호출을 통해 목록 변경 될 때까지 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`.

XIM 네트워크 없음 전혀 아직 존재 하는 경우에서 첫 번째 단계를 시작 하는 프로세스를 가져오려면 XIM 네트워크에 이동 하는 것입니다. 사용자 염두에 특정 XIM 네트워크를 아직 없는 경우 단순히 새, 빈 네트워크로 이동 하는 것이 좋습니다. 이 마치는 수 공동으로 함께 자신의 다음 멀티 플레이 활동 (예: 매치 메이 킹 입력)를 선택 하는 "로비" 네트워크에 가입 하는 사용자의 친구 수 있습니다.

다음은 이전에 추가한 로컬 사용자만 이동 하는 방법의 예 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` 최대 8 총 플레이어에 대 한 공간을 사용 하 여 빈 XIM 네트워크에:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringOnlyLocalPlayers);
```

호출 `XboxIntegratedMultiplayer.MovetoNewNetwork()` 비동기 이동 개발자에 대 한 정기적으로 처리 해야 하는 상태 변경 이벤트를 사용 하 여 마무리 하는 작업 시작 합니다.

## <a name="asynchronous-operations-and-processing-state-changes"></a>비동기 작업 및 상태 변경 내용 처리

XIM의 핵심은 앱의 일반, 자주 사용에 대 한 호출의 `XboxIntegratedMultiplayer.GetStateChanges()` 메서드. 이 메서드는 어떻게 XIM 알림을 멀티 플레이 상태에 대 한 업데이트를 처리 하도록 준비 하는 앱은 XIM을 반환 하 여 이러한 업데이트를 제공 하는 방법을 `XimStateChangeCollection` 대기 중인된 모든 업데이트를 포함 하는 개체입니다. `XboxIntegratedMultiplayer.GetStateChanges()` UI 렌더링 루프에서 매 프레임 마다 그래픽 호출 될 수 있도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 대기 중인된 모든 변경 내용을 검색 하는 편리한 위치를 제공 합니다. XIM API는 실제로이 단일 스레드 패턴에 대 한 최적화 됩니다. 상태 동안 그대로 보장는 `XimStateChangeCollection` 개체 되 고 프로세스와 않은 삭제 되었습니다.

`XimStateChangeCollection` 컬렉션이 `IXimStateChange` 개체입니다.
앱을 수행 해야합니다.

1. 배열 반복 합니다.
1. 검사는 `IXimStateChange` 보다 구체적인 형식을 합니다.
1. 캐스트는 `IXimStateChange` 형식 더 자세한 유형에 해당 합니다.
1. 해당 업데이트를 적절 하 게 처리 합니다.

한 번 모두 완료 합니다 `IXimStateChange` 현재 사용할 수 있는 개체를 다시 호출 하 여 리소스를 해제 하는 XIM에 해당 배열 전달 해야 `XimStateChangeCollection.Dispose()`. `using` 문이 리소스 처리 후 삭제 보장이 되도록 좋습니다. 예를 들면 다음과 같습니다.

```cs
using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
{
    foreach (var stateChange in stateChanges)
    {
        switch (stateChange.Type)
        {
            case XimStateChangeType.PlayerJoined:
                HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                break;

            case XimStateChangeType.PlayerLeft:
                HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                break;

            ...
        }
    }
}
```

기본 처리 루프 했으므로 초기와 관련 된 상태 변경 내용을 처리할 수 있습니다 `XboxIntegratedMultiplayer.MoveToNewNetwork()` 작업 합니다. 모든 XIM 네트워크 이동 작업으로 시작 됩니다는 `XimMoveToNetworkStartingStateChange`. 어떤 이유로 든 실패 하면 이동 하는 경우 앱에 제공 될 예정는 `XimNetworkExitedStateChange`, 어떤 처리 메커니즘 XIM 네트워크에 이동 하지 못하도록 하거나 현재 XIM 네트워크에서 연결을 끊을 수 있는 모든 비동기 오류에 대 한 일반적인 실패 합니다. 이동으로 완료 되는 그렇지 않은 경우는 `XimMoveToNetworkSucceededStateChange` 모든 상태 완료 된 후 모든 플레이어가 XIM 네트워크에 추가 되었습니다.

XIM는 만들거나 XIM 네트워크에 가입 하려고 하는 장치에 전송 사용자에 다른 사용자와 멀티 플레이 세션에서 재생을 위한 플랫폼 권한 없는 경우는 `XimNetworkExitedStateChange` 이유를 사용 하 여 `UserMultiplayerRestricted`. 로컬 사용자에 게 사용 하 여 설정 된 경우 이런 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` Xbox Live 멀티 플레이 제한 했습니다. 사용자의 문제를 발견 하 여 적절 하 게 처리 하세요.

## <a name="basic-iximplayer-handling"></a>기본 IXimPlayer 처리

성공 한다고 가정 하는 새로운 XIM 네트워크에 단일 로컬 사용자를 이동 하는 예제 앱 제공 됩니다는 `XimPlayerJoinedStateChange` 로컬에 대 한 `XimLocalPlayer` 개체입니다. 해당 될 때까지이 개체는 유효한 남아 `XimPlayerLeftStateChange` 제공 되 고을 통해 반환 된 `XimStateChangeCollection.Dispose()`. 앱은 항상 제공는 `XimPlayerLeftStateChange` 에 대 한 모든 `XimPlayerJoinedStateChange`.

모든 목록이 검색할 수 있습니다 `IXimPlayer` 를 사용 하 여 언제 든 지 개체에는 XIM 네트워크 `XboxIntegratedMultiplayer.GetPlayers()`.

`IXimPlayer` 와 같은 개체에 많은 도움이 속성 및 메서드를 `IXimPlayer.Gamertag()` 에 표시 하기 위해 플레이어와 관련 된 현재 Xbox Live 게이머 문자열을 검색 합니다. 하는 경우는 `IXimPlayer` 이 장치에 로컬 IXimPlayer.Local가 true를 반환 합니다. 로컬 `IXimPlayer` 캐스팅할 수는 `XimLocalPlayer`를 추가 하는 메서드가 로컬 플레이어에만 사용할 수 있는 합니다.

물론, 플레이어에 대 한 가장 중요 한 상태는 XIM 알고 있는 일반적인 정보 공유 되지만 추적 하고자 하는 특정 앱. 연결 하려는 될 가능성이 해당 추적 정보에 대 한 고유한 구문 없으므로 합니다 `IXimPlayer` 플레이어 구문 개체 자체에 개체를 언제 든 지 XIM 보고서는 `IXimPlayer`를 수행 하 여 조회 하지 않고도 신속 하 게 상태를 검색할 수 있습니다 사용자 지정 플레이어 컨텍스트 개체를 설정 합니다. 다음 예제에서는 개인 상태를 포함 하는 개체에 가정 변수 'myPlayerStateObject' 및 새로 추가 된 `IXimPlayer` 개체는 'newXimPlayer' 변수:

```cs
newXimPlayer.CustomPlayerContext = myPlayerStateObject;
```

로컬로 플레이어 개체를 사용 하 여 지정 된 개체를 저장이 (되지 전송 되 면 네트워크를 통해 원격 디바이스에 메모리를 유효한 것 위치). 그런 다음 항상 돌아가기 개체에 사용자 지정 컨텍스트를 검색 하 고 다음과 같이 개체를 캐스팅 하 여 수 있게 됩니다.

```cs
myPlayerStateObject = (MyPlayerState)(newXimPlayer.CustomPlayerContext);
```

언제 든 지가 사용자 지정 플레이어 컨텍스트를 변경할 수 있습니다.

## <a name="enabling-friends-to-join-and-inviting-them"></a>참가 하도록 친구를 사용 하도록 설정 하 고 초대

개인 정보 및 보안에 대 한 모든 새 XIM 네트워크에 참여할 수만 로컬 플레이어가 기본적으로 자동으로 구성 됩니다 및 응용 프로그램에서 준비가 되 면 다른 사용자가 명시적으로 허용 하는 것은 합니다. 다음 예제를 사용 하는 방법을 보여 줍니다 `XboxIntegratedMultiplayer.NetworkConfiguration` 현재 네트워크 구성을 검색 하 고 사용 하 여 파티 업데이트 `XboxIntegratedMultiplayer.SetNetworkConfiguration()` 로 플레이어에 참여 하는 새로운 로컬 사용자 뿐만 아니라 다른 사용자를 초대 받은 또는 하 되 고 "뒤 에" 허용 하려면 ( Xbox Live 소셜 관계) XIM 네트워크에서 이미 플레이어가:

```cs
XimNetworkConfiguration currentConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
currentConfiguration.AllowedPlayerJoins = XimAllowedPlayerJoins.Local | XimAllowedPlayerJoins.Invited | XimAllowedPlayerJoins.Followed;
XboxIntegratedMultiplayer.SetNetworkConfiguration(currentConfiguration);
```

`XboxIntegratedMultiplayer.SetNetworkConfiguration()` 비동기적으로 실행합니다. 이전 코드 샘플 호출이 완료 되 면는 `XimNetworkConfigurationChangedStateChange` 의 기본값에서 파티 값이 변경 하는 앱을 알리기 위해 제공 `XimAllowedPlayerJoins.None`. 다음 값을 확인 하 여 새 값을 쿼리할 수 있습니다 `XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins`.

`XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins` 디바이스가 네트워크의 파티 확인 하려면 XIM 네트워크에 있는 동안 확인할 수 있습니다.

원격 사용자에 게이 XIM 네트워크에 가입 하도록 초대를 보낼 원하는 로컬 플레이어 중 하나에 앱을 호출할 수 있습니다 `XimLocalPlayer.ShowInviteUI()` 시스템 초대 UI를 시작 하도록 합니다. 여기에 로컬 사용자 사람 초대 및 초대를 보낼 수를 선택할 수 있습니다.

```cs
XimLocalPlayer.ShowInviteUI();
```

위의 실행 한 후 시스템 초대 UI 표시 됩니다. 사용자가 초대를 보낼 (또는 그렇지 않은 경우 UI를 해제) 되 면는 `xim_show_invite_ui_completed_state_change` 가 제공 됩니다. 앱 또는 사용 하 여 직접 초대를 보낼 수 있습니다 `XimLocalPlayer.InviteUsers()` 시스템 초대 UI 표시 하지 않고 합니다.

어떤 방식을 선택 원격 사용자가 메시지를 받습니다는 Xbox Live 초대 때마다 로그인 되어 있으며 수락 하도록 선택할 수 있습니다. 이미 실행 되지 않습니다 "프로토콜 활성화" 하는 경우 해당 장치에서 앱을 시작 됩니다이 것이 동일한 XIM 네트워크로 이동 하 여 사용할 수 있는 이벤트 인수를 사용 합니다.

프로토콜 활성화 자체에 대 한 자세한 내용은 플랫폼 설명서를 참조 하세요.

다음 예제에서는 호출 하는 방법 `XboxIntegratedMultiplayer.ExtractProtocolActivationInformation()` 이벤트 인수 XIM에 적용할 수 있는지를 결정 합니다. 이 가정에서 원시 URI 문자열을 검색 한 `Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs` 변수에 'uriString':

```cs
XimProtocolActivationInformation protocolActivationInformation;
XboxIntegratedMultiplayer.ExtractProtocolActivationInformation(uriString, out protocolActivationInformation);
if (protocolActivationInformation != null)
{
    // It is a XIM activation.
}
```

XIM 활성화를 사용 하는 것이의 'LocalXboxUserId' 속성에서 식별 된 로컬 사용자를 확인 하려는 경우는 `XimProtocolActivationInformation` 개체는 로그인 하 고 지정 된 사용자 들이 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`. 다음을 호출 하 여 지정 된 XIM 네트워크에 이동을 시작할 수 있습니다 `XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` 동일한 URI 문자열을 사용 합니다. 예를 들면 다음과 같습니다.

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs(uriString);
```

또한 "뒤 에" 원격 사용자는 시스템 UI에서에서 로컬 사용자의 플레이어 카드로 이동 하 고 (위에 표시 된 대로 이러한 플레이어 조인 허용 했습니다 가정) 초대 없이 직접 조인 시도 시작할 수 있습니다. 이 작업은 프로토콜도 초대 수행 하 고 동일한 방식으로 처리 된 것 처럼 앱을 활성화 합니다.

프로토콜 활성화를 사용 하 여 XIM 네트워크에 이동 [초기화 및 시작에](#initialization-and-startup)표시 된 대로 새 XIM 네트워크에 이동 하는 것과 동일 합니다. 유일한 차이점은는 이동에 성공 하면 이동 장치 제공 됩니다 로컬 및 원격 플레이어 `XimPlayerJoinedStateChange` 해당 플레이어를 나타내는 개체. XIM 네트워크에 이미 있는 원래 장치를 이동 하지 않습니다 하지만 새 디바이스의 사용자 추가 통해 플레이어로 추가 나타납니다 자연스럽 게 `XimPlayerJoinedStateChange` 개체.

다른 디바이스 간에 플레이어가 XIM 네트워크에 조인 되, 되 면 멀티 플레이어 시나리오를 시작 자동으로 음성 및 텍스트를 통해 통신 및 앱 관련 메시지를 보낼 수 있습니다.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>기본 매치 메이 킹 및 다른 사용자와 다른 XIM 네트워크로 이동 합니다.

추가로 낯선 사람에도 있는 XIM 네트워크에는 플레이어를 이동 하 여 친구의 그룹에 대 한 네트워킹 환경을 확장할 수 있습니다. 낯선 사람은 상대에서 전 세계 있는 비슷한 관심사에 따라 Xbox Live 매치 메이 킹 서비스를 사용 하 여 함께 설정 됩니다.

XIM 사용 하 여 기본 매치를 시작 하는 간단한 방법 호출 하는 것 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 으로 채워진를 사용 하 여 디바이스 중 하나에서 `MatchmakingConfiguration` 개체를 함께 현재 XIM 네트워크에서 플레이어를 수행 합니다.

다음 예제에서는 시작 아니요 팀 개인 대전 일치 하는 총 8 플레이어를 찾을 수 있도록 설정 매치 메이 킹 구성을 사용 하 여 이동 합니다. 총 8 플레이어를 찾을 수 없는 경우 시스템 여전히 일치 않을 2 ~ 7 플레이어 함께. 이 예제에서는 형식이 uint, 오프 필터링 하는 게임 모드를 나타내는 MYGAMEMODE_DEATHMATCH 라는 앱 정의 된 상수를 사용 합니다. XIM의 매치 메이 킹만 일치시킬지이 네트워크 동일한 값 및 두 번째 매개 변수를 제공 하는 경우 현재 XIM 네트워크에서 모든 사회 가입 플레이어를 따라 표시를 지정 하는 다른 플레이어와 `XimPlayersToMove.BringExistingSocialPlayers` 를 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`:

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

이전 이동 같은 초기에서는이 `MoveToNetworkStartingStateChange` 모든 장치에서 및 `MoveToNetworkSucceededStateChange` 이동 성공적으로 완료 되 면 합니다. 한 가지 차이점에는 이미 기존은 다른 하나의 XIM 네트워크에서 이동 이므로 `IXimPlayer` 로컬 및 원격 사용자에 대 한 개체를 추가 하 고 이러한 새로운 XIM 네트워크에 함께 이동 하는 모든 플레이어에 대 한 유지 됩니다. 플레이어 간에 채팅 및 데이터 통신 계속 매치 메이 킹 진행 중인 동안 계속 작동 합니다.

매치 매치 메이 킹 풀에 있는 라고도 하는 잠재적인 플레이어 수에 따라 걸릴 수 있습니다 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`.

A `XimMatchmakingProgressUpdatedStateChange` 와 사용자의 현재 상태를 유지 하는 작업 하는 동안 주기적으로 제공 됩니다. 일치, 추가 플레이어와 일반적인 XIM 네트워크에 추가 됩니다 `PlayerJoinedStateChange` 이며 이동 작업을 완료 합니다.

한 번이 집합 "matchmade" 플레이어를 사용 하 여 멀티 플레이어 경험을 끝냈으면 매치 메이 킹의 다른 반올림 하 여 다른 XIM 네트워크로 이동 하는 프로세스를 반복할 수 있습니다. 사전 통해 가입 각 플레이어를 볼 수 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 작업 제공는 `PlayerLeftStateChange` 나타내기 위해 자신의 `IXimPlayer` 개체는 더 이상 같은 XIM 네트워크에 합니다.

경우 선택 하면 매치 메이 킹 사용 하 여 다시 시작 하려면 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, 지정 `XimPlayersToMove.BringExistingSocialPlayers`, 소셜 진입점을 통해 가입 하는 플레이어가 (`XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` 또는 `xXboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId`) 새 매치 메이 킹 수행 하는 동안 유지 됩니다. 또는 지정 `XimPlayersToMove.BringOnlyLocalPlayers` 소셜에서 연결 된 원격 플레이어, 종료 상태를 유지할 로컬 플레이어 바로 연결을 끊습니다. 두 경우 모두 낯선 사람 다른 집합은 두 번째 이동 작업이 완료 되 면 추가 됩니다.

수도 다음 매치 메이 킹 구성/멀티 플레이어 활동을 결정 하는 동안이 이동 비 matchmade 플레이어 (또는 로컬 방금 플레이어)를 완전히 새로운 XIM 네트워크에 있습니다.

다음 예제에서는 장치를 호출 하는 방법을 보여 줍니다. `XboxIntegratedMultiplayer.MoveToNewNetwork()` 8 명 하지만이 이번에는 기존 소셜에서 가입 플레이어도 수행 하는 최대 XIM 네트워크에 대 한:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringExistingSocialPlayers);
```

A `MoveToNetworkStartingStateChange` 및 `MoveToNetworkSucceededStateChange` 모든 참가 디바이스와 함께 제공 됩니다는 `PlayerLeftStateChange` 남아 있는 matchmade에 대 한 합니다. 이러한 장치에서 유사 하 게 표시 됩니다는 `PlayerLeftStateChange` 네트워크 밖으로 이동 되는 각 플레이어에 대 한 합니다.

원하는 만큼 위에 설명 된 방식으로 XIM 네트워크에서 XIM 네트워크에 이동 작업을 계속할 수 있습니다.

성능에 Xbox Live 서비스 그룹의 모든 직접 피어 투 피어 연결을 설정할 수 있는 장치에는 플레이어와 일치 하도록 시도 하지 않습니다. 매치 메이 킹 작업을 사용 하 여 새 네트워크에 이동 있는 확실 하는 경우에 성공적으로 일치 하지 않고 무기한 계속 될 수 있습니다 표준 Xbox Live 멀티 플레이어를 지원 하도록 제대로 구성 된 네트워크 환경에서 개발 하는 경우 충분 한 플레이어 매치 메이 킹 조건을 만족 하는 동일한 로컬 환경에서 모두 이동 하 고 사용 하 여 모든 장치. 영역/Xbox 응용 프로그램 네트워크 설정에 멀티 플레이 연결 테스트를 실행 하 고 특히 "엄격한 NAT"에 대 한 문제를 보고 하는 경우 해당 권장 사항을 준수 해야 합니다.

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>디버깅을 위해 매치 메이 킹 NAT 규칙을 사용 하지 않도록 설정

XIM 없이 하나 이상 "열기" 엄격한 NAT "장치 일치를 허용 하도록 구성 하 여 Xbox One 개발 키트에 대 한 테스트 하 여 매치 메이 킹 차단 해제 수 네트워크 관리자 XIM의 NAT 규칙을 지원 하기 위해 필요한 환경 변경할 수 없는 경우 NAT"장치입니다. 모든 Xbox One 콘솔에서 "제목 처음" 드라이브의 루트 (콘텐츠는 중요 하지 않습니다) "xim_disable_matchmaking_nat_rule" 이라는 파일을 배치 하 여 수행 됩니다. 이렇게 하려면 하나의 예제 방법은 "{console_name_or_ip_address}" 자리 표시자를 적절 하 게 각 콘솔에 대 한 대체 앱을 시작 하기 전에 XDK 명령 프롬프트에서 다음을 실행 하 여입니다.

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> 이 개발 해결 방법만 Xbox One 단독 리소스 응용 프로그램에 사용할 수 있는 현재는 하 고 유니버설 Windows 응용 프로그램에 지원 되지 않습니다. 또한이 파일에 관계 없이 네트워크 환경 존재도 없는 디바이스를 사용 하 여이 설정을 사용 하는 콘솔와 일치 하지 것입니다 참고 해야 추가 하 고 모든 곳에서 파일 제거 합니다.

## <a name="leaving-a-xim-network-and-cleaning-up"></a>XIM 네트워크를 종료 하 고 정리

로컬 사용자가 완료 되 면 초대, XIM 네트워크에 자주는 단순히 로컬 사용자를 허용 하는 새로운 XIM 네트워크에 다시 이동에 참여 하 고 "뒤 에" 친구 다음 활동을 찾을 수를 조정 하 여 계속 수 있도록 가입 하는 사용자. 하지만 사용자가 모든 멀티 플레이어 경험을 사용 하 여 완전히 완료를 앱은 XIM을 벗어나지 시작 해야 할 경우 완전히 네트워크 및 상태를 처럼 반환할만 `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds` 를 호출한 합니다. 이렇게 사용 하는 `XboxIntegratedMultiplayer.LeaveNetwork()` 방법:

```cs
 XboxIntegratedMultiplayer.LeaveNetwork();
```

이 메서드는 비동기적으로 적절 하 게 다른 참가자에서 분리 하는 프로세스를 시작 합니다. 이렇게 하면 제공 되는 원격 디바이스를 `PlayerLeftStateChange` 연결이 끊어진 로컬 각 플레이어에 대 한 합니다. 로컬 장치 수도 제공 됩니다는 `PlayerLeftStateChange` 연결이 각 로컬 및 원격 플레이어에 대 한 합니다. 모든 연결을 끊을 때 작업이 완료 되 면 최종 `NetworkExitedStateChange` 가 제공 됩니다.

호출 `XboxIntegratedMultiplayer.LeaveNetwork()` 는 대기 상태로 유지 합니다 `NetworkExitedStateChange` 는 XIM 종료 하기 위해 네트워크 정상적으로 항상 적극 권장 때는 `NetworkExitedStateChange` 제공 되지 이미 합니다.

## <a name="sending-and-receiving-messages"></a>메시지 보내기 및 받기

XIM와 기본 구성 요소 연결 문제 또는 전부가 아닌 일부 플레이어에 도달할 수에 대해 걱정할 필요가 인터넷을 통해 보안 통신 채널을 설정 하는 모든 번거로운 작업을 수행 합니다. 기본 피어 투 피어 연결 문제가 있습니까 XIM 네트워크에 이동 성공 하지 않습니다.

XIM 네트워크 구성 되 고 승인을 경우 `XimMoveToNetworkSucceededStateChange`, 모든 가입된 장치에서 앱의 인스턴스를 모두에 대해 숙지 보장이 모든 `IXimPlayer` XIM 네트워크에 연결 하 고 그 중 하나를 메시지를 보낼 수 있습니다.

XIM 네트워크를 통해 메시지를 전송 하는 방법을 살펴보겠습니다. 다음 예제에서는 'sendingPlayer' 변수는 유효한 로컬 플레이어 개체를 가정 합니다. 이 예제에서는 ' sendingPlayer ' s 메서드 `SendDataToOtherPlayers()` XIM 네트워크에서 모든 플레이어 (로컬 또는 원격) 'msgBuffer'에 복사 된 메시지 구조를 보내려면 합니다. 특정 한 플레이어 목록이 메서드로 전달 되지 않았습니다 때문에 모든 플레이어를 이동 하는지 note 하십시오.

```cs
SendingPlayer.SendDataToOtherPlayers(msgBuffer, null, XimSendType.GuaranteedAndSequential);
```

메시지의 모든 수신자가 제공는 `XimPlayertoPlayerDataReceivedStateChange` 데이터의 복사본을 포함 하는 외에 `IXimPlayer` 그 목록이 전송 하는 개체는 `IXimPlayer` 로컬로 받고 개체.

물론 보장, 순차적 배달, 편리 하지만 XIM 재전송 또는 패킷 삭제 또는 인터넷에서 disordered 메시지를 지연 해야 하므로 비효율적인 보내기 형식의 될 수도 있습니다. 손실 하거나 앱 허용할 수 있는 메시지에 대 한 다른 전송 형식을 사용 하 여 고려해 야 순서를 벗어난 도착 합니다. 원격 컴퓨터에서 메시지 데이터를 가져오는, 이후 같은 멀티 바이트 값 특정 바이트 순서 ("endianness")에서 압축 된 데이터 형식을 명확 하 게 정의 하는 것이 좋습니다. 앱이 작업을 수행 하기 전에 데이터 유효성을 검사도 해야 합니다.

추가 암호화 또는 서명 스키마에 대 한 필요 하므로 XIM 네트워크 수준의 보안을 제공 합니다. 하지만 권장 항상 다른 버전의 응용 프로그램 프로토콜 존재할 정상적으로 (중 개발, 콘텐츠 업데이트 등)를 처리 하려면 "심층" 실수로 응용 프로그램 버그를 방지 하기 위해 사용 됩니다. 사용자가 인터넷 연결 제한 하 고 변화 리소스 될 수 있습니다. 효율적인 메시지 데이터 형식 및 UI 프레임 마다 보낼 피하고 디자인의 사용을 권장 합니다.

## <a name="assessing-data-pathway-quality"></a>데이터 경로 품질 평가

데이터 경로 검사 하 여 두 플레이어의 현재 품질에 대 한 자세한 합니다 `XimPlayer.NetworkPathInformation` 속성입니다. 현재 품질을 검사 하 여 두 플레이어 간의 진행 경로에 대해 자세히 알아볼 수 있습니다 합니다 `XimPlayer.NetworkPathInformation` 속성입니다. 예상된 왕복 대기 시간 및 얼마나 많은 메시지는 여전히 대기 로컬로 전송의 경우 해당 시점에 대 한 자세한 데이터를 전송 연결을 지원할 수 없는 속성에 포함 됩니다.

표시는 큐가 특정 'IXimPlayer'에 대 한 백업, 데이터를 전송 하는 속도 줄여야 합니다.

`NetworkPathInformation.RoundTripLatency` 필드 큐 없이 기본 네트워크의 대기 시간 및 XIM의 예상된 대기 시간을 나타냅니다. 유효 대기 시간으로 증가 `XimNetworkPathInformation.SendQueueSizeInMessages` 증가 하 고 XIM 큐를 통해 작동 합니다.

제한에 대 한 호출을 시작 하려면 적절 한 지점 선택 `SendDataToOtherPlayers` 게임의 사용 및 요구 사항에 따라 합니다. 모든 메시지 보내기 큐에 효과적인 네트워크 대기 시간 증가 나타냅니다.

XIM의 최대 제한 (현재 3500 메시지) 가까운 값이 너무 높아 대부분의 게임 및 될 가능성이 몇 초 정도 호출의 속도 따라 전송 대기 중인 데이터를 나타내는 `SendDataToOtherPlayers` 각 데이터 페이로드 크기는 합니다. 어떻게 떨림 게임과 함께 게임의 대기 시간 요구를 고려 하는 숫자 대신 선택 `SendDataToOtherPlayers` 호출 패턴입니다.

## <a name="sharing-data-using-player-custom-properties"></a>플레이어의 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.

대부분의 앱 데이터 교환 발생할 합니다 `XimLocalPlayer.SendDataToOtherPlayers()` 때 패킷 손실, 처리 하 고 등 해야 하는 방법 및이 받을 사용자에 대해 가장 제어를 허용 하므로 메서드. 그러나 때 좋을 플레이어가 사람과 자체에 대 한 기본 상태로 거의 변경를 간단 하 게 공유할 수 있습니다. 예를 들어 각 플레이어 고정된 멀티 플레이 게임에서 표현으로 렌더링을 사용 하 여 모든 플레이어 입력 하기 전에 선택한 문자 모델을 나타내는 문자열을 사용할 수 있습니다.

플레이어에 대 한 자주 변경 되지 않는 데이터, 사용자 지정 속성 XIM 플레이어를 제공 합니다. 이러한 속성은 null로 끝나는 문자열 쌍은 변경 될 때마다 자동으로 모든 장치에 전파 가져오기 및 로컬 플레이어에 적용할 수 있는 앱 정의 이름 및 값을 구성 됩니다.

사용자 지정 플레이어 속성에 오버 헤드가 보다 더 많은 내부 동기화는 `XimLocalPlayer.SendDataToOtherPlayers()` 메서드를 되므로 (즉, 플레이어가 위치) 데이터를 신속 하 게 변경에 대 한 여전히 직접 전송을 사용 해야 합니다.

플레이어가 사용자 지정 속성 키-값 쌍의 현재 값은 이러한 장치 XIM 네트워크에 가입 하 고 추가 플레이어를 볼 때 참여 하는 새 장치를 자동으로 제공 했습니다. 호출 하 여 값을 설정할 수 있습니다 `XimLocalPlayer.SetPlayerCustomProperty()` 이름과 값을 사용 하 여 문자열 속성을 설정 하는 다음 예제에서와 같은 로컬에서 "무차별" 값을 "모델" 라는 `IXIMPlayer` 변수 'localPlayer' 가리키는 개체.

```cs
localPlayer.SetPlayerCustomProperty("model","brute");
```

플레이어 속성 변경 하면는 `XimPlayerCustomPropertiesChangedStateChange` 변경 된 속성 이름에 경고 하는 모든 장치에 제공 합니다. 사용 하 여 플레이어, 로컬 또는 원격에 지정 된 이름에 대 한 값을 검색할 수 있습니다 `IXIMPlayer.GetPlayerCustomProperty()`. 다음 예제에서 "모델" 라는 속성에 대 한 값을 검색 한 `IXIMPlayer` 변수가 'ximPlayer':

```cs
string propertyValue = localPlayer.GetPlayerCustomProperty("model");
```

지정 된 속성 이름에 대 한 새 값을 설정 하는 모든 기존 값을 바꿉니다. 처리 되는 null 값 문자열 포인터의 값으로 속성 이름을 설정 것 지정 되지 아직 속성으로 동일한 값을 빈 문자열로 동일 합니다. 이름과 값 XIM에 의해 해석 되지, 따라서 필요에 따라 문자열 내용을 유효성을 검사 하는 앱에 남아 있습니다.

사용자 지정 플레이어 속성은 항상 하나의 XIM 네트워크에서 다른 컴퓨터로 이동 하는 경우 다시 설정 합니다. 그러나 새 플레이어가 XIM 네트워크에 가입 하는 일부 세트를 가진 모든 플레이어에 대 한 현재 사용자 지정 플레이어 속성을 받게 됩니다.

## <a name="sharing-data-using-network-custom-properties"></a>네트워크 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.

네트워크 사용자 지정 속성은 자주 변경 되지 않는 XIM 네트워크에 특정 데이터 동기화 편의 위해 사용 됩니다. XIM 의도 하지 않은 단일 개체에 설정 점을 제외 하 고 [플레이어가 사용자 지정 속성](#sharing-data-using-player-custom-properties)을 동일 하 게 사용자 지정 속성 작업 네트워크 `XboxIntegratedMultiplayer.SetNetworkCustomProperty()`. 다음 예제에서는 "지도" 속성 "요새" 값을 설정 합니다.

```cs
XboxIntegratedMultiplayer.SetNetworkCustomProperty("map", "stronghold");
```

네트워크 속성을 변경 하면는 `NetworkCustomPropertiesChanged` 변경 된 속성 이름에 경고 하는 모든 장치에 제공 합니다. 지정 된 이름에 대 한 값을 검색할 수 있습니다 `XboxIntegratedMultiplayer.GetNetworkCustomProperty()`속성 명명 된 "지도"에 대 한 값을 검색 하는 다음 예제에서 처럼:

```cs
string property = XboxIntegratedMultiplayer.GetNetworkCustomProperty("map")
```

[사용자 지정 속성 플레이어](#sharing-data-using-player-custom-properties)처럼 지정 된 속성 이름에 대 한 새 값을 설정 기존 값을 바뀝니다. 처리 되는 null 값 문자열 포인터의 값으로 속성 이름을 설정 것 지정 되지 아직 속성으로 동일한 값을 빈 문자열로 동일 합니다. 이름과 값 XIM에 의해 해석 되지, 따라서 필요에 따라 문자열 내용을 유효성을 검사 하는 앱에 남아 있습니다.

새로 만든 XIM 네트워크는 항상 없는 네트워크 사용자 지정 속성 집합으로 시작 합니다. 그러나 기존 XIM 네트워크에 가입 하는 새로운 플레이어 설정 해당 XIM 네트워크에 대 한 네트워크 사용자 지정 속성의 현재 값을 받게 됩니다.

## <a name="matchmaking-using-per-player-skill"></a>플레이어 당 기술을 사용 하 여 매치 메이 킹

특정 앱에서 지정한 게임 모드의 관심사 하 여 플레이어를 일치 하는 것은 좋은 기본 전략입니다. 또한 플레이어 숙련 된 플레이어 최신 플레이어 수 증가 하는 동안 다른 낮 선 정상 경쟁 하는 문제를 즐길 수 있도록 자신의 개인 기술 또는 게임 경험에 따라 일치 하는 고려해 야 사용할 수 있는 플레이어의 풀 증가 다른 사용자에 대해 유사한 기능을 사용 하 여 경쟁 합니다.

이렇게 하려면 싱글 플레이어 역할 및 기술 구성 구조에 대 한 호출에 지정 된 모든 로컬 플레이어에 대 한 기술 수준을 제공 하 여 시작 `XimLocalPlayer.SetRolesAndSkillConfiguration()` 를 시작 하는 XIM 이동 하기 전에 매치 메이 킹 사용 하는 네트워크. 숙련도 앱 관련 개념 이며 숫자 매치 메이 킹은 먼저 찾으려고 동일한 기술 값을 사용 하 여 플레이어와 기술 선언 다른 플레이어를 찾으려고 10 + /-단위로 검색을 정기적으로 확대 된다는 점을 제외 되지 XIM에 의해 해석 됩니다. 해당 기술 주위 범위 내 값입니다. 다음 예제에서는 가정 하는 로컬 `XimLocalPlayer` 개체를 해당 변수는 'localPlayer', 'playerSkillValue' 라는 변수에 로컬 또는 Xbox Live 저장소에서 검색 관련된 앱 별 정수 기술 값은:

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Skill = playerSkillValue;
localPlayer.SetRolesAndSkillConfiguration(config);
```

모든 참가자에 게 제공 될 예정이 완료 되 면는 `XimPlayerRolesAndSkillConfigurationChangedStateChange` 이 나타내는 `IXIMPlayer` 은 싱글 플레이어 역할 및 기술 구성 변경 되었습니다. 호출 하 여 새 값을 검색할 수 있습니다 `IXIMPlayer.RolesAndSkillConfiguration`. 모든 플레이어 null이 아닌 매치 메이 킹 구성이 적용를 있는 경우 true에 대 한 값을 사용 하 여 매치를 사용 하 여 XIM 네트워크에 이동할 수 있습니다 합니다 `RequirePlayerRolesAndSkillConfiguration` 필드의 `XimMatchmakingConfiguration` 구조에 지정 된 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`합니다.

다음 예제에서는 아니요 팀 개인 대전에 대 한 총 2-8 플레이어를 찾으려면 매치 메이 킹 구성 합니다. 또한이 예제에서는 형식이 Uint64 및 MYGAMEMODE_DEATHMATCH 오프 필터링 하는 게임 모드를 나타내는 라는 앱 정의 된 상수를 사용 합니다. 매치 메이 킹 싱글 플레이어 매치 메이 킹 구성 요구 뿐만 아니라 이러한 동일한 값을 지정 다른 플레이어와 XIM 네트워크의 플레이어와 일치 하도록 구성 됩니다.

```cs
XimMatchmakingConfiguration matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.RequirePlayerRolesAndSkillConfiguration = true;
```

이 구조를 제공 때 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, 이동 작업 플레이어가 이동 이라고 긴 정상적으로 시작 됩니다 `XimLocalPlayer.SetRolesAndSkillConfiguration()` null이 아닌를 사용 하 여 `XimPlayerRolesAndSkillConfiguration` 개체입니다. 모든 플레이어 않은 다음 매치 메이 킹 프로세스는 일시 중지 하 고 모든 참가자가 제공 하는 경우는 `XimMatchmakingProgressUpdatedStateChange` 으로 `WaitingForPlayerRolesAndSkillConfiguration` 값입니다. 이전에 전송한 초대를 통해 또는 다른 소셜 수단을 통해 XIM 네트워크에 가입 이후에 플레이어 포함 됩니다 (예: 호출 `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) 매치 메이 킹 완료 되기 전에 합니다. 모든 플레이어 제공 되 면 해당 `XimPlayerRolesAndSkillConfiguration` 개체에서 매치 메이 킹을 다시 시작 합니다.

다음 섹션에 설명 된 대로 플레이어 당 기술을 사용 하 여 매치 매치 메이 킹 사용자 당 플레이어 역할을 사용 하 여 결합할 수도 있습니다. 필요한 하나만 다른 값을 0으로 지정할 수 있습니다. 선언 하는 모든 플레이어 갖기 때문에 이것이 `XimPlayerRolesAndSkillConfiguration` 기술 값 0은 서로 일치 항상 합니다.

한 번의 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 또는 작업이 완료 된 모든 플레이어의 이동 XIM 네트워크 `XimPlayerRolesAndSkillConfiguration` 구조를 null 포인터 자동으로 해제 됩니다 (와 함께 `XimPlayerRolesAndSkillConfigurationChangedStateChange` 알림). 싱글 플레이어 구성이 필요 매치를 사용 하 여 다른 XIM 네트워크로 이동 하려는 경우 호출 해야 합니다 `XimLocalPlayer.SetRolesAndSkillConfiguration()` 최신 정보를 포함 하는 개체를 사용 하 여 다시 합니다.

## <a name="matchmaking-using-per-player-role"></a>싱글 플레이어 역할을 사용 하 여 매치 메이 킹

사용자의 매치 메이 킹 환경을 개선 하기 위해 싱글 플레이어 역할 및 기술 구성을 사용 하는 또 다른 방법은 필요한 플레이어 역할을 사용 하 여입니다. 다른 공동 작업을 유도 하는 선택 가능한 문자 유형에 재생 스타일을 제공 하는 게임에 가장 적합 합니다. 이러한 문자 형식은 하지 간단 하 게 게임에서 그래픽으로 변경 하 고, 대신, 플레이어에 대 한 게임 플레이 스타일을 변경 하는 것입니다. 사용자의 특정 전문으로 재생할 수도 있습니다. 그러나 불가능 기능적으로 명 이상 각 역할을 수행 하지 않고 목표를 완료 하려면 되도록 게임을 디자인할 때로는 것이 이러한 플레이어에 게임을 함께 일치 보다 먼저 다음 하도록 요구에 맞게 한 번에 수집 간에 플레이 스타일을 협상 합니다. 첫 번째 각 역할에 지정 된 플레이어를 지정할 수를 나타내는 고유한 비트 플래그를 정의 하 여이 수행할 수 있는 `XimPlayerRolesAndSkillConfiguration` 구조입니다.

형식 바이트는 있으며 MYROLEBITFLAG_HEALER, 로컬 명명 하는 앱 특정 역할 값을 설정 하는 다음 예제에서는 `XimLocalPlayer` 개체에서 포인터가 'localPlayer':

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Roles = MYROLEBITFLAG_HEALER;
localPlayer.SetRolesAndSkillConfiguration(config);
```

모든 참가자에 게 제공 될 예정이 완료 되 면는 `XimPlayerRolesAndSkillConfigurationChangedStateChange` 이 나타내는 `IXimPlayer` 은 싱글 플레이어 역할 및 기술 구성 변경 되었습니다. 호출 하 여 새 값을 검색할 수 있습니다 `IXimPlayer.RolesAndSkillConfiguration`.

전역 `XimMatchmakingConfiguration` 에 지정 된 구조 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 해야 다음 필요한 역할이 플래그를 모두 결합 한 비트 OR를 사용 하 고 값이 true로 `RequirePlayerRolesAndSkillConfiguration` 필드.

싱글 플레이어 역할을 사용 하 여 매치 매치 메이 킹 사용자 당 플레이어 기술을 사용 하 여 결합할 수도 있습니다. 필요한 하나만 다른 값을 0으로 지정 합니다. 선언 하는 모든 플레이어 갖기 때문에 이것이 `XimPlayerRolesAndSkillConfiguration` 기술 값 0은 항상 일치; 서로 모든 비트가 0에는 경우는 `XimMatchmakingConfiguration.RequiredRoles` 일치 하기 위해 필요 없는 역할 비트 필드입니다.

한 번의 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 또는 작업이 완료 된 모든 플레이어의 이동 XIM 네트워크 `XimPlayerRolesAndSkillConfiguration` 개체를 null로 자동으로 해제 됩니다 (와 함께 `XimPlayerRolesAndSkillConfigurationChangedStateChange` 알림). 싱글 플레이어 구성이 필요 매치를 사용 하 여 다른 XIM 네트워크로 이동 하려는 경우 호출 해야 합니다 `XimLocalPlayer.SetRolesAndSkillConfiguration()` 최신 정보를 포함 하는 개체를 사용 하 여 다시 합니다.

## <a name="how-xim-works-with-player-teams"></a>XIM은 플레이어 팀과 함께 작동 하는 방식

멀티 플레이어 반대 팀으로 구성 하는 플레이어는 종종 게임 합니다. XIM 사용 하면 때 팀 할당을 쉽게 설정 하 여 매치 메이 킹 `XimMatchmakingConfiguration.TeamConfiguration`. 다음 예제에서는 총 8 플레이어를 찾으려고 구성 매치 메이 킹을 사용 하 여 (하지만 4를 찾을 수 없는 경우 1 ~ 3 플레이어 또한 허용) 4의 두 팀에 배치 하는 이동을 시작 합니다. 또한이 예제에서는 형식이 uint 이며 오프 필터링 하는 게임 모드를 나타내는 MYGAMEMODE_CAPTURETHEFLAG 라는 앱 정의 된 상수를 사용 합니다.  또한 구성은 현재 XIM 네트워크에서 모든 사회 가입 플레이어와 함께 설정 됩니다.

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 2;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 4;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

이러한는 XIM 네트워크 이동 작업이 완료 되 면 플레이어 팀 인덱스 1-{n} 해당 하는 값을 요청 {n} 팀이 할당 됩니다. 플레이어의 팀 인덱스 값을 통해 검색 `IXIMPlayer.TeamIndex`. 사용 하는 경우는 `XimMatchmakingConfiguration.TeamConfiguration` 두 개 이상의 팀과 플레이어 되지 할당할 팀 인덱스 값이 0에 대 한 호출 하 여 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`. 플레이어를 XIM 네트워크 구성 또는 이동 작업의 형식에 추가 하는 반면입니다 (와 같은 초대를 수락에서 비롯 된 프로토콜 활성화를 통해), 사용자는 항상 인덱스가 0 팀 합니다. 특수 "할당 되지 않은" 팀으로 팀 인덱스 0을 처리할 수 있습니다.

다음 예제에서는 'ximPlayer' 변수에 저장 하는 XIM 플레이어 개체에 대 한 팀 인덱스를 검색 합니다.

```cs
byte playerIndex = ximPlayer.TeamIndex;
```

(하지 음수 플레이어의 동작에 대 한 언급 된 감소 기회)를 사용자 기본 환경용 매치 메이 킹 Xbox Live 서비스는 다른 팀에 함께 XIM 네트워크로 이동 하는 분할 플레이어 하지 않습니다.

매치 메이 킹이 처음 할당 팀 인덱스 값은 권장만 및 앱을 사용 하 여 원하는 시간에 로컬 플레이어가 변경할 수 있습니다 `XimLocalPlayer.SetTeamIndex()`. 이 매치 메이 킹을 전혀 사용 하지 않는 XIM 네트워크에서 호출할 수도 있습니다. 다음 예제에서는 하나의 새 팀 인덱스 값을 가질 'ximLocalPlayer' 변수에 저장 XIM 로컬 플레이어 개체를 구성 합니다.

```cs
ximLocalPlayer.SetTeamIndex(1);
```

플레이어에 새 팀 인덱스 값을 적용 제공 하며 때 모든 디바이스는 정보는 `XimPlayerTeamIndexChangedStateChange` 는 플레이어에 대 한. 호출할 수 있습니다 `XimLocalPlayer.SetTeamIndex()` 언제 든 지.

매치 메이 킹 팀에서 독립적으로 필요한 플레이어 역할을 평가합니다. 따라서 접수 플레이어 역할을 받지 팀 플레이어 수에 따라 조정 하기 때문에 팀과 필요한 역할을 모두 동시 매치 메이 킹 구성 기준으로 사용 하 여 권장 되지 했습니다.

## <a name="working-with-chat"></a>채팅 작업

음성 및 문자 채팅 통신 XIM 네트워크에 플레이어 간에 자동으로 활성화 됩니다. XIM 수에 대 한 모든 음성 헤드셋 및 마이크 하드웨어와 상호 작용을 처리 합니다. 앱에 필요한 채팅, 많은 작업을 수행 하지만 없는 텍스트 채팅에 대 한 요구 사항 중 하나: 입력 및 디스플레이 지원 합니다. 텍스트 입력이 플랫폼 또는 지금까지 적이 광범위 한 실제 키보드를 사용 하는 게임 장르에도 플레이어 텍스트 음성 변환 보조 기술을 사용 하 여 시스템을 구성할 수 있으므로 필요 합니다. 마찬가지로, 텍스트 표시는 플레이어가 음성-텍스트를 사용 하 여 시스템을 구성할 수 있으므로 필요 합니다.

이러한 기본 설정에 액세스 하 여 로컬 플레이어에서 감지할 수 조건부로 텍스트 메커니즘을 사용 하려는 경우는 `XimLocalPlayer.ChatTextToSpeechConversionPreferenceEnabled` 및 `XimLocalPlayer.ChatSpeechToTextConversionPreferenceEnabled` 각각 필드 및 조건부로 텍스트 메커니즘을 사용 하고자 할 수 있습니다. 텍스트 입력 및 디스플레이 항상 사용할 수 있는 옵션을 고려 하는 것이 좋습니다.

`Windows::Xbox::UI::Accessability` Xbox One 클래스는 하도록 설계 된 음성-텍스트 보조 기술에 초점을 사용 하 여 게임에서 텍스트 채팅의 간단한 렌더링을 제공 합니다.

실제 또는 가상 키보드에서 제공 하는 텍스트 입력을 구성한 후에 문자열을 전달 합니다 `XimLocalPlayer.SendChatText()` 메서드. 다음 코드는 로컬 하드 코드 된 문자열의 예를 보내기 `IXIMPlayer` 변수 'localPlayer' 가리키는 개체.

```cs
localPlayer.SendChatText(text);
```

이 채팅 텍스트도 원래 로컬 플레이어에서 채팅 통신을 수신할 수 있는 XIM 네트워크에서 모든 플레이어에 게 전달 되는 `XimChatTextReceivedStateChange`. TTS를 사용 하도록 설정 하는 경우이 음성 오디오를 합성 수도 있습니다.

앱 받은 모든 텍스트 문자열의 복사본을 만들고 하 고 적절 한 금액이 시간 (또는 스크롤 가능한 창)에 대 한 원래 플레이어의 일부 id와 함께 표시 해야 합니다.

채팅에 관한 몇 모범 사례가 있습니다. 아무 곳 이나 플레이어에에서 표시 되어 있는지, 특히 게이머 점수 판, 같은 태그 목록을 사용자에 대 한 피드백으로 아이콘 음소거/말하기를 표시할 수도 것이 좋습니다. 이 작업에 액세스 하 여 수행 됩니다 `IXimPlayer.ChatIndicator` 검색 하는 `XboxIntegratedMultiplayer.XimPlayerChatIndicator` 해당 플레이어에 대 한 채팅 순간, 현재 상태를 나타내는 합니다.

값에서 보고 `IXimPlayer.ChatIndicator` 예를 들어에 플레이어 시작 및 중지 말하기으로 자주 변경 해야 합니다. 폴링 것 UI 프레임 마다 결과적으로 앱을 지원 하도록 설계 되었습니다.

> [!NOTE]
> 로컬 사용자가 디바이스 설정 인해 통신 권한이 없는 경우 `XimLocalPlayer.ChatIndicator` 는 반환는 `XboxIntegratedMultiplayer.XimPlayerChatIndicator` 값을 사용 하 여 `XIM_PLAYER_CHAT_INDICATOR_PLATFORM_RESTRICTED`합니다. 플랫폼에 대 한 요구 사항을 충족 하도록 기대 하는 앱 표시 있는 음성 채팅 또는 메시징, 및 문제를 나타내는 사용자에 게 메시지에 대 한 플랫폼 제한 나타내는입니다. 예를 들어 메시지는 것이 좋습니다. "죄송 하지만, 지금 바로 채팅 수는 없습니다."

## <a name="muting-players"></a>플레이어 음소거

또 다른 최선의 음소거 플레이어를 지원 하는 것입니다. XIM 자동으로 시스템 음소거 플레이어 카드를 통해 사용자가 시작을 처리 하지만 게임 관련 임시 음소거를 통해 게임 UI 내에서 수행할 수 있는 앱을 지원 해야 합니다 `IXimPlayer.ChatMuted` 속성입니다. 다음 예제에서는 원격 음소거 시작 `IXIMPlayer` 가리키는 개체 변수 'remotePlayer' 없이 음성 채팅 들립니다 없는 텍스트 채팅에서 수신 되 고 있습니다.

```cs
remotePlayer.ChatMuted = true;
```

음소거가 적용 바로 하 고 있는 연결 된 XIM 상태 변하지 않습니다. 값을 변경할 수는 `IXimPlayer.ChatMuted` 속성을 false로 설정 합니다. 다음 예제에서는 원격 unmutes `IXIMPlayer` 변수 'remotePlayer' 가리키는 개체.

```cs
remotePlayer.ChatMuted = false;
```

음을 소거 계속 적용에 대 한으로 `IXIMPlayer` 플레이어를 사용 하 여 새로운 XIM 네트워크에 이동할 때 포함 하 여 존재 합니다. 플레이어가 유지 되지 않습니다 동일한 사용자가 다시 가입 하 고 (으로 새 `IXIMPlayer` 인스턴스).

플레이어는 일반적으로 unmuted 상태에서 시작합니다. 앱에서 게임 플레이 이유로 음소거 상태는 플레이어가 시작 하려는 경우 설정할 수 있습니다 `IXIMPlayer.ChatMuted` 에 `IXIMPlayer` 개체는 연결 된 처리를 완료 하기 전에 `PlayerJoinedStateChange`, XIM 컴파일하면 기간 없음 음성 오디오 플레이어에서 될 수 있는 보장 됩니다 들립니다.

플레이어 평판 기반 자동 음소거 확인 원격 플레이어를 XIM 네트워크에 연결 하는 경우에 발생 합니다. 플레이어에 나쁜 평판 플래그, 플레이어가 자동으로 음소거 합니다. 음소거만 로컬 상태를 적용 하 고 따라서 네트워크를 통해 플레이어를 이동 하는 경우를 유지 합니다. 자동 평판 기반 음소거 검사는 한 번 수행 하 고 다시 평가 되지 다시에 대 한으로 `IXIMPlayer` 유효 합니다.

## <a name="configuring-chat-targets-using-player-teams"></a>팀이 플레이어를 사용 하 여 채팅 대상 구성

이 문서의 [플레이어 팀과 작동 방식을 XIM](#how-xim-works-with-player-teams) 섹션에서 설명 했 듯이 특정 팀 인덱스 값의 의미를 true은 앱 달려 있습니다. XIM 채팅 대상 구성에 대해 일치 비교를 제외 하 고 해석 하지 않습니다. 채팅 대상 구성에서 보고 하는 경우 `XboxIntegratedMultiplayer.ChatTargets` 현재 `XimChatTargets.SameTeamIndexOnly`를 지정 된 모든 플레이어는 다른 채팅 통신 교환할만 두 동일한 값에서 보고 하는 경우 다음 `IXIMPlayer.TeamIndex` (및 개인 정보 보호 정책 에서도 허용 /).

기본적으로 구성 된 보수적 XIM 네트워크를 새로 만든 경쟁 시나리오는 자동으로 지원 하 고 `XimChatTargets.SameTeamIndexOnly`. 그러나 다른 팀에서 점령한 상대 채팅 "로비" 후 게임의 예를 들어 좋을 수 있습니다. 호출 하 여 다른 모든 (여기서 개인 정보 및 정책 허용)에 게 모든 XIM 하도록 지시할 수 있습니다 `XboxIntegratedMultiplayer.SetChatTargets()`. 다음 샘플을 사용 하 여 XIM 네트워크에 있는 모든 참가자 구성 시작는 `XimChatTargets.AllPlayers` 값:

```cs
XboxIntegratedMultiplayer.SetChatTargets(XimChatTargets.AllPlayers)
```

제공 하며 때 새 대상 설정이 적용 되는 모든 참가자는 정보는 `XimChatTargetsChangedStateChange`.

앞에서 설명한 대로 대부분의 XIM 네트워크 이동 유형에 처음 할당할 것 모든 플레이어 팀 인덱스 값으로 0입니다. 즉,의 구성 `XimChatTargets.SameTeamIndexOnly` 가능성이 구별할 `XimChatTargets.AllPlayers` 기본적으로 합니다. 그러나 다른 경우 인덱스 값 팀 매치를 사용 하 여 XIM 네트워크로 이동 하는 플레이어를 갖게 됩니다 매치 메이 킹 구성의 `TeamMatchmakingMode` 값 두 개 이상의 팀 선언 됩니다. 호출할 수 있습니다 `XimLocalPlayer.SetTeamIndex()` 언제 든 지 위와 같이 합니다. 앱 0이 아닌 팀 인덱스 값을 통해 이러한 방법 중 하나를 사용 하는 경우를 적절 하 게 설정 현재 채팅 대상 관리할 것을 잊지 마세요.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>플레이어 슬롯 ("백필" 매치 메이 킹)의 배경 자동 채우기

호출 플레이어의 서로 다른 그룹 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 동시에 유연성을 제공 매치 메이 킹 Xbox Live 서비스의 가장 큰 신속 하 게 새로운, 최적의 XIM 네트워크를 구성 합니다. 그러나 일부 게임 플레이 시나리오를 특정 XIM 네트워크 및 추가 플레이어를 빈 플레이어 슬롯 채우기만 matchmake 유지 하려고 합니다. XIM 지원 모드 또는 "역"으로 채우는 호출 하 여 작성 된 자동 백그라운드에서 작동 하는 데 매치 메이 킹 구성 `XboxIntegratedMultiplayer.SetNetworkConfiguration()` 와 `XimNetworkConfiguration` 가 `XimAllowedPlayerJoins.Matchmade` 플래그가 설정에 해당 `XimNetworkConfiguration.AllowedPlayerJoins` 속성입니다.

다음 예제에서는 구성 (8를 찾을 수 없는 경우에 2 ~ 7 플레이어는 허용도) 이지만 아니요 팀 개인 대전에 대 한 총 8 플레이어의 찾습니다 백필 매치 메이 킹, MYGAMEMODE_ 값에 의해 정의 된 앱 특정 게임 모드 상수 uint64_t를 사용 하 여 매치만 동일 하 게 값을 지정 하는 다른 플레이어와 일치 합니다.

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Matchmade;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

이렇게 하면 네트워크 기존 XIM 호출 하는 장치에 사용할 수 있는 `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` 표준 방식에서입니다. 이러한 장치 변경 동작이 없습니다. 역으로 채우는 XIM 네트워크에 참여 움직이지 것입니다 하지만 제공 될 예정는 `XimNetworkConfigurationChangedStateChange` 켜기, 백필 뿐만 아니라 여러 나타내는 `XimMatchmakingProgressUpdatedStateChange` 해당 하는 경우 알림을 합니다. 모든 matchmade 플레이어 법선을 사용 하 여 XIM 네트워크에 추가 됩니다 `XimPlayerJoinedStateChange`.

기본적으로 백필 매치 메이 킹 계속 사용 됩니다 무기한 플레이어를 XIM 네트워크에 지정 된 플레이어의 최대 수를 이미 있는 경우 추가 하려고 시도 하지 않지만 `XimNetworkConfiguration.TeamConfiguration` 설정 합니다. 역으로 채우는 설정 하 여 사용 하지 않도록 설정할 수 있습니다 `XimNetworkConfiguration.AllowedPlayerJoins` 제외한 `XimAllowedPlayerJoins.Matchmade`:

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins &= ~XimAllowedPlayerJoins.Matchmade;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

해당 `XimNetworkConfigurationChangedStateChange` 이 비동기 프로세스가 완료 되 면 최종 되 면 및 모든 장치에 제공 됩니다 `XimMatchmakingProgressUpdatedStateChange` 제공 됩니다 `MatchmakingStatus.None` 을 나타내는 없는 추가 matchmade 플레이어가 XIM 네트워크에 추가 됩니다.

사용 하 여 백필 매치 메이 킹 사용 하도록 설정할 때는 `XimNetworkConfiguration.TeamConfiguration` 두 개 이상의 팀 선언 된 설정, 모든 기존 플레이어 1 사이의 팀의 수 있는 유효한 팀 인덱스 있어야 합니다. 호출한 플레이어 포함 `XimLocalPlayer.SetTeamIndex()` 사용자 지정 값 또는 사용자가 초대를 사용 하 여 가입한 나 다른 소셜 통해 의미를 지정할 수 (예: 호출 `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) 기본 팀 인덱스 값 0으로 기능이 추가 되었습니다. 모든 플레이어는 유효한 팀 인덱스 없는 면 매치 메이 킹 프로세스는 일시 중지 하 고 모든 참가자가 제공는 `XimMatchmakingProgressUpdatedStateChange` 으로 `MatchmakingStatus.WaitingForPlayerTeamIndex` 값입니다. 모든 플레이어 제공 있거나 해당 팀 인덱스 값을 수정 되 면 `XimLocalPlayer.SetTeamIndex()`, 백필 매치 메이 킹 계정이 다시 시작 됩니다. 자세한 내용은이 문서의 [플레이어 팀과 작동 방식을 XIM](#how-xim-works-with-player-teams) 섹션에서 찾을 수 있습니다.

마찬가지로, 사용 하 여 백필 매치 메이 킹 사용 하도록 설정할 때는 `XimNetworkConfiguration` 구조를 사용 하 여는 `RequirePlayerRolesAndSkillConfiguration` 모든 플레이어는 플레이어 당 null이 아닌 매치 메이 킹 구성을 지정 된 다음 true로 설정 필드. 모든 플레이어 않은 다음 매치 메이 킹 프로세스는 일시 중지 하 고 모든 참가자가 제공 하는 경우는 `XimMatchmakingProgressUpdatedStateChange` 으로 `XimMatchMakingStatus.WaitingForPlayerRolesAndSkillConfiguration` 값입니다. 모든 플레이어 제공 되 면 해당 `XimPlayerRolesAndSkillConfiguration` 개체에서 백필 매치 메이 킹을 다시 시작 합니다. 자세한 내용은이 문서의 [매치 메이 킹 플레이어 당 기술을 사용 하 여](#matchmaking-using-per-player-skill) 및 [싱글 플레이어 역할을 사용 하 여 매치 메이 킹](#matchmaking-using-per-player-role) 섹션에서 찾을 수 있습니다.

## <a name="querying-joinable-networks"></a>참여할 네트워크를 쿼리합니다.

매치 메이 킹 신속 하 게 플레이어를 함께 연결 하는 훌륭한 방법 이지만, 경우에 따라 것이 가장 좋습니다 가입 하려는 네트워크를 선택 하 고 사용자 지정 검색 조건을 사용 하 여 참가할 수 있는 네트워크 검색을 허용 합니다. 이 게임 세션 다양 한 구성 가능한 게임 규칙 및 플레이어 설정 있을 때 특히 유용할 수 있습니다. 이렇게 하려면 기존 네트워크 먼저 만들어져야 쿼리할 수를 사용 하 여 `XimAllowedPlayerJoins.Queried` 파티 호출을 통해 네트워크 외부 다른 사람에 게 사용 가능한 네트워크 정보를 구성 하 고 `XboxIntegratedMultiplayer.SetNetworkConfiguration()`.

다음 예제에서는 `XimAllowedPlayerJoins.Queried` 파티, 설정 네트워크 1 팀에서 함께 1-8 플레이어의 총 수 있도록 팀 구성 사용 하 여 구성, "고양이 GAME_MODE_BRAWL에 대 한 설명 값으로 정의 된 앱 특정 게임 모드 상수 uint64_t 지도 앱 특정 인덱스 상수 uint32_t MAP_KITCHEN 값으로 정의 및 boxing 일치 양의 ", 태그"chatrequired","쉽게","spectatorallowed "를 포함 합니다.

```cs
string[] tags = { "chatrequired", "easy", "spectatorallowed" };
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Queried;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = GAME_MODE_BRAWL;
networkConfiguration.Description = "Cat and sheep's boxing match";
networkConfiguration.MapIndex = MAP_KITCHEN;
networkConfiguration.SetTags(tags);
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

그런 다음 네트워크 외부에서 다른 플레이어 xim::start_joinable_network_query() 이전 xim::set_network_configuration() 호출에 네트워크 정보를 일치 하는 필터 세트와 함께 호출 하 여 네트워크를 찾을 수 있습니다. 다음 예제에서는 게임 모드 필터 옵션은 GAME_MODE_BRAWL 값으로 정의 된 앱 특정 게임 모드를 사용 하 여 네트워크에 대 한 쿼리만 사용 하 여 참여할 네트워크 쿼리를 시작 합니다.

```cs
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

"쉽게" 태그를 가진 네트워크와 "spectatorallowed" 공개 구성을 쿼리할 수에 대 한 쿼리 하는 태그 필터 옵션을 사용 하는 또 다른 예는 다음과 같습니다.

```cs
string[] tagFilters = { "easy", "spectatorallowed" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

다른 필터 옵션을 결합할 수도 있습니다. 둘 다 있는 앱 특정 게임 모드 상수 GAME_MODE_BRAWL 및 태그 "쉽게" 네트워크에 대 한 쿼리를 시작 하는 옵션을 필터링 하는 태그와 게임 모드 필터 옵션을 사용 하는 다음 예제에서는:

```cs
string[] tagFilters = { "easy" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

쿼리 작업이 성공 하면 앱에는 앱 참여할 네트워크의 목록을 검색할 수 있는 xim_start_joinable_network_query_completed_state_change를 받게 됩니다. 앱에서 받을 지속적으로 `XimJoinableNetworkUpdatedStateChange` 추가 참여할 네트워크 또는 수동 또는 자동으로 중지 될 때까지 참여할 네트워크의 반환 된 목록에 발생 하는 모든 변경 합니다. 호출 하 여 진행 중인 쿼리를 수동으로 중지 `XboxIntegratedMultiplayer.StopJoinableNetworkQuery()`. 호출할 때 자동으로 중지 될 것 `XboxIntegratedMultiplayer.StartJoinableNetworkQuery()` 새 쿼리를 시작 합니다.

앱 참여할 네트워크의 목록에서 호출 하 여 네트워크에 가입 하려고 `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation()`. 다음 예제에서는 다음과 같이 가정 합니다 (두 번째 매개 변수에 빈 문자열 전달) 있도록 암호에서 안전 하지 않은 'selectedNetwork'에서 참조 되는 네트워크에 가입 하려고 합니다.

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation(selectedNetwork, "");
```

네트워크 쿼리를 사용 하도록 설정할 때는 `XimNetworkConfiguration.TeamConfiguration` 두 개 이상의 팀을 선언 하는, 플레이어 XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation()가 호출 하 여 가입 기본값이 팀 인덱스 0입니다.