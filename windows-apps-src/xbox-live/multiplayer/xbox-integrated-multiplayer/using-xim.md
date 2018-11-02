---
title: XIM (c + +)를 사용 하 여
author: KevinAsgari
description: C + +를 사용 하 여 Xbox 통합 멀티 플레이어 (XIM)를 사용 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 04/24/2018
ms.topic: article
keywords: xbox live, xbox, 게임, 하나는 xbox, xbox 통합된 멀티 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: 2e4645de174bb572b75a8d5a5dcbb4091a693e75
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5934764"
---
# <a name="using-xim-c"></a>XIM (c + +)를 사용 하 여

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

XIM의 c + + API를 사용 하 여 간단한 연습입니다. C#을 통해 XIM에 액세스 하려는 게임 개발자를 [사용 하 여 XIM (C#)](using-xim-cs.md)보일 것입니다.

- [XIM (c + +)를 사용 하 여](#using-xim-c)
    - [사전 요구 사항](#prerequisites)
    - [초기화 및 시작](#initialization-and-startup)
    - [비동기 작업 및 상태 변경 내용 처리](#asynchronous-operations-and-processing-state-changes)
    - [XIM 플레이어 개체를 처리합니다.](#handling-xim-player-objects)
    - [참가 하도록 친구를 사용 하도록 설정 하 고 초대](#enabling-friends-to-join-and-inviting-them)
    - [기본 매치 메이 킹, 다른 사용자와 다른 XIM 네트워크로 이동](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [디버깅을 위해 매치 메이 킹 NAT 규칙을 사용 하지 않도록 설정](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [XIM 네트워크를 종료 하 고 정리](#leaving-a-xim-network-and-cleaning-up)
    - [메시지 보내기 및 받기](#sending-and-receiving-messages)
    - [데이터 경로 품질 평가](#assessing-data-pathway-quality)
    - [플레이어가 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.](#sharing-data-using-player-custom-properties)
    - [네트워크 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.](#sharing-data-using-network-custom-properties)
    - [싱글 플레이어 기술을 사용 하 여 매치 메이 킹](#matchmaking-using-per-player-skill)
    - [싱글 플레이어 역할을 사용 하 여 매치 메이 킹](#matchmaking-using-per-player-role)
    - [XIM은 플레이어 팀과 함께 작동 하는 방법](#how-xim-works-with-player-teams)
    - [채팅 작업](#working-with-chat)
    - [플레이어 음소거](#muting-players)
    - [팀이 플레이어를 사용 하 여 채팅 대상 구성](#configuring-chat-targets-using-player-teams)
    - [플레이어 슬롯 ("백필" 매치 메이 킹)의 자동 백그라운드 채우기](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [쿼리 있는 네트워크](#querying-joinable-networks)

## <a name="prerequisites"></a>사전 요구 사항

XIM을 사용 하 여 코딩을 시작 하기 전에 가지 두 가지 필수 조건이 있습니다.

1. 표준 멀티 플레이어 네트워킹 기능을 사용 하 여 앱의 AppXManifest을 구성 하 고 필요한 트래픽을 XIM에서 사용 되는 패턴 템플릿을 선언 하는 네트워크 매니페스트 부분을 구성 해야 합니다.

    AppXManifest 기능과 네트워크 매니페스트 플랫폼 설명서;의 각 섹션에서 자세히 설명 되어 있습니다. 일반적인 XIM 관련 XML 붙여 [XIM 프로젝트 구성](xim-manifest.md)에서 제공 됩니다.

1. 응용 프로그램 id 정보를 사용할 수 있는 두 가지 해야 합니다.

    * 할당 된 Xbox Live 제목 id.
    * Xbox Live 서비스에 대 한 액세스에 대 한 응용 프로그램을 프로 비전의 일부로 제공 되는 서비스 구성 ID입니다.

    이러한 획득 대 한 자세한 내용은 Microsoft 담당자를 참조 하세요. 초기화 하는 동안 이러한 정보의 사용 됩니다.

기본 등의 필요 XIM 컴파일 `XboxIntegratedMultiplayer.h` 헤더. 제대로 연결 하려면 프로젝트에 포함 해야 합니다 `XboxIntegratedMultiplayerImpl.h` 에서 하나 이상의 컴파일 단위 (일반적인 미리 컴파일된 헤더는 이러한 스텁 함수 구현은 컴파일러 "inline"으로 생성 하는 작은 쉽고 때문에 권장 됨).

C + 컴파일 중에서 선택 하는 프로젝트 [XIM 개요](../xbox-integrated-multiplayer.md)에서 설명 했 듯이 XIM 인터페이스 컴파일할 필요가 + CX 기존 c + +; 비교 함께 사용할 수 있습니다. 또한 구현 치명적이 지 않은 오류를 선호 하는 경우 예외 즈 프리 프로젝트에서 쉽게 사용할 수 있도록를 보고 하는 수단으로 예외를 throw 하지 않습니다.

## <a name="initialization-and-startup"></a>초기화 및 시작

Xbox Live 서비스 구성 ID 문자열 및 제목 ID 번호를 사용 하 여 XIM 개체 singleton은 초기화 하 여 라이브러리와 상호 작용을 시작 합니다. 또한 Xbox 서비스 API를 사용 하는 경우 편리할 수 있습니다 것에서이 검색 하려면 `xbox::services::xbox_live_app_config`. 다음 예제에서는 값 이미 있는 'myServiceConfigurationId' 및 'myTitleId' 변수에 각각 있다고 가정 합니다.

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

앱 참여를 전달할 현재 로컬 장치에 있는 모든 사용자에 대 한 Xbox 사용자 ID 문자열을 검색 해야 초기화 되 면는 `xim::set_intended_local_xbox_user_ids()` 를 호출 합니다. 다음 샘플 코드는 단일 사용자가 재생 의도 반드시 컨트롤러 단추를 누르고 사용자와 관련 된 Xbox 사용자 ID 문자열을 'myXuid' 변수에 이미 가져왔습니다 있다고 가정 합니다.

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

호출 `xim::set_intended_local_xbox_user_ids()` 즉시 XIM 네트워크에 추가 되어야 하는 로컬 사용자와 관련 된 Xbox 사용자 Id를 설정 합니다. Xbox 사용자 Id의이 목록에서에서 사용할 모든 향후 네트워크 작업 다른 호출을 통해 목록 변경 될 때까지 `xim::set_intended_local_xbox_user_ids()`.

XIM 네트워크 없음 전혀 아직 있는 경우 첫 번째 단계 시작 프로세스를 가져오려면 XIM 네트워크에 이동 하는 것입니다. 사용자 염두에 특정 XIM 네트워크를 아직 없는 경우 단순히 새, 빈 네트워크로 이동 하는 것이 좋습니다. 이 마치 "로비"는 수 공동으로 함께 해당 다음 멀티 플레이 활동 (예: 매치 메이 킹 입력)를 선택 하는 네트워크에 가입 하도록 사용자의 친구 수 있습니다.

다음은 이전에 추가한 로컬 사용자만을 이동 하는 방법의 예 `xim::set_intended_local_xbox_user_ids()` 최대 8 총 플레이어에 대 한 공간을 사용 하 여 빈 XIM 네트워크에:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

호출 `xim::move_to_new_network()` 비동기 이동 개발자에 대 한 정기적으로 처리 해야 하는 상태 변경 이벤트를 사용 하 여 마무리 하는 작업 시작 합니다.

## <a name="asynchronous-operations-and-processing-state-changes"></a>비동기 작업 및 상태 변경 내용 처리

XIM의 핵심은 앱의 일반, 자주 호출 하는 `xim::start_processing_state_changes()` 및 `xim::finish_processing_state_changes()` 메서드 쌍. 이러한 메서드는 어떻게 멀티 플레이 상태에 대 한 업데이트를 처리 하도록 준비 하는 앱은 XIM은 알림을 있으며 XIM 해당 업데이트를 제공 하는 방법. 이러한 UI 렌더링 루프에서 매 프레임 마다 그래픽 호출할 수 있습니다 되도록 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 또는 다중 스레드 콜백 복잡성 예측 불가능성 상관 없이 대기 중인된 모든 변경 내용을 검색 하는 편리한 위치를 제공 합니다. XIM API는 실제로이 단일 스레드 패턴에 대 한 최적화 됩니다. 보장 상태로 효율적이 고 직접 사용할 수 있도록이 두 함수 외부에서 변경 되지 것입니다.

같은 이유로, XIM API에서 반환 하는 모든 개체 해야 *하지* 스레드로부터 안전한 것으로 간주 합니다. 라이브러리는 내부 다중 스레딩 보호, 되었지만 XIM의 값에 액세스 하는 임의의 스레드 필요한 경우 고유한 잠금 구현 해야 합니다. 스레드와 워크의 경우 예를 들어 합니다 `xim::players()` 하거나 다른 스레드를 호출 될 수 있는 동안 목록 `xim::start_processing_state_changes()` 또는 `xim::finish_processing_state_changes()` 나열 하는 플레이어와 관련 된 메모리를 변경 합니다.

때 `xim::start_processing_state_changes()` 는 호출 모든 대기 중인된 업데이트의 배열에 보고 됩니다 `xim_state_change` 포인터를 구성 합니다.

앱은 다음과 같습니다.

1. 배열을 반복 합니다.
1. 더욱 구체적인 해당 형식에 대 한 기본 구조를 검사 합니다.
1. 기본 구조 캐스트할 더 자세한 유형에 해당 합니다.
1. 해당 업데이트를 적절 하 게 처리 합니다.

모든 완료 한 번의 `xim_state_change` 현재 사용할 수 있는 구조를 호출 하 여 리소스를 해제할 XIM 돌아가기 해당 배열 전달 해야 할 `xim::finish_processing_state_changes()`. 예:

```cpp
uint32_t stateChangeCount;
xim_state_change_array stateChanges;
xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
{
   const xim_state_change * stateChange = stateChanges[stateChangeIndex];
   switch (stateChange->state_change_type)
   {
       case xim_state_change_type::player_joined:
       {
           MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change*>(stateChange));
           break;
        }

       case xim_state_change_type::player_left:
       {
           MyHandlePlayerLeft(static_cast<const xim_player_left_state_change*>(stateChange));
           break;
       }

       ...
    }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

기본 처리 루프 했으므로 초기와 관련 된 상태 변경 내용을 처리할 수 있습니다 `xim::move_to_new_network()` 작업 합니다. 모든 XIM 네트워크 이동 작업이로 시작 하는 `xim_move_to_network_starting_state_change`. 어떤 이유로 든 이동 실패할 경우 앱에 제공 될 예정는 `xim_network_exited_state_change`는 처리 메커니즘 XIM 네트워크에 이동 하면 또는 현재 XIM 네트워크에서 연결을 끊을 수 있는 모든 비동기 오류에 대 한 일반적인 실패 합니다. 이동으로 완료 되는 그렇지 않은 경우는 `xim_move_to_network_succeeded_state_change` 모든 상태 완료 된 후 모든 플레이어 XIM 네트워크에 추가 되었습니다.

XIM는 만들거나 XIM 네트워크에 가입 하려고 하는 장치에 전송 사용자에 멀티 플레이 세션에서 다른 사용자와 재생을 위한 플랫폼 권한이 없는 경우는 `xim_network_exited_state_change` 이유를 사용 하 여 `xim_network_exit_reason::user_multiplayer_restricted`. 로컬 사용자에 게 사용 하 여 설정 된 경우 이런 `xim::set_intended_local_xbox_user_ids()` Xbox Live 멀티 플레이 제한 했습니다. XIM을 사용 하 여 Xbox 종말 앱 중 하나를 호출 해야 `Store::Product::CheckPrivilegeAsync` true로 attemptResolution 집합과 XIM 네트워크에 이동 또는 응답 수신으로 수행 하기 전에 로컬 플레이어에서 `xim_network_exit_reason::user_multiplayer_restricted` 으로 반환 된 사유 `xim_network_exited_state_change`합니다.

## <a name="handling-xim-player-objects"></a>XIM 플레이어 개체를 처리합니다.

성공 새로운 XIM 네트워크에 단일 로컬 사용자를 이동 하는 예제 한다고 가정 하에 앱을 제공 될 예정는 `xim_player_joined_state_change` 로컬에 대 한 `xim_player` 개체입니다. 플레이어 자체 인스턴스가 유효한으로이 개체 포인터에 대 한 유효한 유지 됩니다. 플레이어 인스턴스 해질 잘못 된 해당 `xim_player_left_state_change` 에 대 한 호출을 통해 반환 않은 인스턴스를 플레이어에 대 한 `xim::finish_processing_state_changes()`. 앱은 항상 제공는 `xim_player_left_state_change` 에 대 한 모든 `xim_player_joined_state_change`.

모든 배열도 검색할 수 있습니다 `xim_player` 는 XIM의 개체를 사용 하 여 언제 든 지 네트워크 `xim::get_players()`.

`xim_player` 와 같은 개체에 많은 도움이 메서드가, `xim_player::gamertag()` 표시 하기 위해 플레이어와 관련 된 현재 Xbox Live 게이머 문자열을 검색 하기 위한 합니다. 하는 경우는 `xim_player` 장치에 로컬 이면 null이 아닌 보고도 `xim_player::xim_local` 에서 포인터 개체 `xim_player::local()`, 로컬 플레이어에만 사용할 수 있는 추가 메서드를가지고 있는.

물론, 플레이어에 대 한 가장 중요 한 상태는 XIM 알고 있는 일반적인 정보 공유 되지만 추적 하고자 하는 특정 앱. 연결 하려는 해당 추적 정보에 대 한 고유한 구문에 될 수 있으므로 합니다 `xim_player` 언제 든 지 XIM 보고서 있도록 플레이어 구문 개체 자체에 개체는 `xim_player`, 상태를 채우고 조회 필요 없이 신속 하 게 검색할 수 있습니다는 사용자 지정 플레이어 컨텍스트 포인터가 합니다. 다음 예제에서는 개인 상태에 대 한 포인터에 가정 변수 'myPlayerStateObject' 및 새로 추가 된 `xim_player` 개체는 'newXimPlayer' 변수에:

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

로컬로 플레이어 개체를 사용 하 여 지정 된 포인터 값 저장 (되지 전송 되 면 네트워크를 통해 원격 디바이스에 메모리를 유효한 것 위치). 그런 다음 항상 돌아가기 위해 개체를 사용자 지정 컨텍스트를 검색 하 고 다음과 같이 개체를 다시 캐스팅 하 여 수 있게 됩니다.

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

에 할당 된 사용자 지정 플레이어 컨텍스트 포인터를 변경할 수는 `xim_player` 언제 든 지.

## <a name="enabling-friends-to-join-and-inviting-them"></a>참가 하도록 친구를 사용 하도록 설정 하 고 초대

개인 정보 및 보안에 대 한 모든 새 XIM 네트워크만 있는 되도록 로컬 플레이어에 기본적으로 자동으로 구성 하 고 앱이 준비가 되 면 다른 사용자가 명시적으로 허용 하는 것은 합니다. 다음 예제를 사용 하는 방법을 보여 줍니다 `xim::network_configuration()` 현재 네트워크 구성을 검색 하 고 파티를 사용 하 여 업데이트 `xim::set_network_configuration()` 초대 받은 또는 있는 "적용 되는지" 다른 사용자 뿐만 아니라 새로운 로컬 사용자 플레이어에 참여할 수 있도록 하려면 ( Xbox Live 소셜 관계) 플레이어가 XIM 네트워크에 이미:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` asynchronoulsy를 실행합니다. 이전 코드 샘플 호출이 완료 되 면는 `xim_network_configuration_changed_state_change` 의 기본값에서 파티 값이 변경 하는 앱을 알리기 위해 제공 됩니다 `xim_allowed_player_joins::none`. 확인 하 여 새 값을 쿼리할 수 있습니다는 `allowed_player_joins` 속성은 `xim_network_configuration` 반환한 `xim::network_configuration()`합니다. 

`allowed_player_joins` 디바이스가 네트워크의 파티 확인 하려면 XIM 네트워크에 있는 동안 확인할 수 있습니다.

이 XIM 네트워크에 원격 사용자에 게 초대를 보낼 원하는 로컬 플레이어 중 하나를 앱 호출할 수 있습니다 `xim_player::xim_local::show_invite_ui()` 시스템 초대 UI를 시작 하도록 합니다. 여기서 로컬 사용자 사람 초대 및 초대를 보낼 수를 선택할 수 있습니다. 다음 예제에서는이 작동 하는 방법을 보여 줍니다. 및 유효한 로컬 변수 'ximPlayer' 가리키는지 가정 `xim_player`:

```cpp
ximPlayer->local()->show_invite_ui();
```

위의 실행 한 후 시스템 초대 UI 표시 됩니다. 사용자가 초대를 보낼 (또는 그렇지 않은 경우 UI를 해제) 되 면는 `xim_show_invite_ui_completed_state_change` 다음 호출에 제공 됩니다 `xim::start_processing_state_changes()`. 앱 또는 사용 하 여 직접 초대를 보낼 수 `xim_player::xim_local::invite_users()` 시스템 초대 UI 표시 하지 않고 합니다.

어떤 방식을 선택 원격 사용자가 메시지를 받습니다 Xbox Live 초대 로그인을 승인 하거나 거부 하려면 선택할 수 있는 위치입니다. 요청을 수락 하는 경우에 프로토콜 활성화를 이미 실행 되지 및 프로토콜 활성화는 해당 XIM 네트워크로 이동 하는 데 사용할 수 있는 이벤트 인수를 사용 하 여 앱을 제공 하는 경우 해당 장치에서 앱에 시작 됩니다.

자체 프로토콜 활성화에 대 한 자세한 내용은 플랫폼 설명서를 참조 하세요.

다음 예제는 방법에 대 한 호출 `xim::extract_protocol_activation_information()` 이벤트 인수 XIM에 적용 되는지 결정 합니다. 이 경우 원시 URI 문자열을 검색 한 가정 `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` 변수에 'uriString':

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

XIM 활성화를 사용 하는 것이 경우는 채워진의 'local_xbox_user_id' 필드에 나와 있는 로컬 사용자를 확인 하려고 하면 `xim_protocol_activation_information` 구조는 로그인 하 고 지정 된 사용자가 `xim::set_intended_local_xbox_user_ids()`. 다음을 호출 하 여 지정 된 XIM 네트워크에 이동을 시작할 수 있습니다 `xim::move_to_network_using_protocol_activated_event_args()` 동일한 URI 문자열을 사용 합니다. 예:

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

또한 "뒤 에" 원격 사용자는 시스템 UI에서에서 로컬 사용자의 플레이어 카드를 이동 하 고 (위에 표시 된 대로 이러한 플레이어 조인 허용 했습니다 가정) 초대 하지 않고 가입 시도 자체를 시작할 수 있습니다. 이 작업은 프로토콜도 초대 수행 하 고 동일한 방식으로 처리 된 것 처럼 앱을 활성화 합니다.

프로토콜 활성화를 사용 하 여 XIM 네트워크로 이동 하는 것 [의 초기화 및 시작에](#initialization-and-startup)표시 된 대로 새 XIM 네트워크에 이동 하는 것과 동일 합니다. 유일한 차이점은 이동 성공 하면 이동 장치는 제공 로컬 및 원격 플레이어 `xim_player_joined_state_change` 적용 가능한 모든 플레이어를 나타내는 구조. XIM 네트워크에 이미 있는 원래 장치를 이동 하지 않습니다 하지만 새 디바이스의 사용자 추가 통해 플레이어로 추가 보게 자연스럽 게 `xim_player_joined_state_change` 구조입니다.

다른 디바이스에서 플레이어 함께 XIM 네트워크에서에 가입한 후 멀티 플레이어 시나리오를 시작 하 고, 음성 및 텍스트를 자동으로 통신할 하 고, 앱 관련 메시지 보낼 수 있습니다.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>기본 매치 메이 킹, 다른 사용자와 다른 XIM 네트워크로 이동

추가로 낯선 사람에도 있는 XIM 네트워크에는 플레이어를 이동 하 여 친구의 그룹에 대 한 네트워킹 환경을 확장할 수 있습니다. 낯선 사람 상대에서 전 세계 비슷한 관심사에 따라 Xbox Live 매치 메이 킹 서비스를 사용 하 여 함께 전환 하는 경우

호출 하 여 XIM 사용 하 여 기본 매치를 시작 하는 간단한 방법은입니다 `xim::move_to_network_using_matchmaking()` 으로 채워진된 디바이스 중 하나에 `xim_matchmaking_configuration` 구조를 함께 현재 XIM 네트워크에서 플레이어를 수행 합니다.

다음 예제에서는 시작 아니요 팀 개인 대전 일치 하는 총 8 플레이어를 찾을 수 있도록 설정 하는 매치 메이 킹 구성을 사용 하 여 이동 합니다. 총 8 플레이어를 찾을 수 없는 경우 시스템 수 여전히 일치 2-7 플레이어 함께 합니다. 이 예제에서는 유형 uint64_t, 오프 필터링 하는 게임 모드를 나타내는 MYGAMEMODE_DEATHMATCH 라는의 앱에서 정의한 상수를 사용 합니다. XIM의 매치 메이 킹만 일치시킬지이 네트워크 동일한 값과 두 번째 매개 변수를 제공 하는 경우 현재 XIM 네트워크에서 모든 사회 가입 플레이어를 따라 제공 지정 다른 플레이어와 함께 `xim_players_to_move::bring_existing_social_players` 를 `xim::move_to_network_using_matchmaking()`:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

이 연결 프로세스는 초기를 제공 하는 데 이전 이동 같은 `xim_move_to_network_starting_state_change` 모든 장치에서 및 `xim_move_to_network_succeeded_state_change` 이동 성공적으로 완료 되 면 합니다. 한 가지 차이점은는 이미 기존 하나의 XIM 네트워크에서 다른 위치로 이동 이므로 `xim_player` 개체는 로컬 및 원격 사용자에 대 한 추가 하 고 이러한 새로운 XIM 네트워크에 함께 이동 하는 모든 플레이어에 대 한 유지 됩니다. 플레이어 간에 채팅 및 데이터 통신 계속 매치 메이 킹 진행 중인 동안 계속 작동 합니다.

매치 매치 메이 킹 풀에 있는 라고도 하는 잠재적인 플레이어 수에 따라 걸릴 수 있습니다 `xim::move_to_network_using_matchmaking()`.

A `xim_matchmaking_progress_updated_state_change` 및 현재 상태에 대 한 알림을 사용자에 게 유지 하는 작업 동안 주기적으로 제공 됩니다. 일치, 추가 플레이어는 일반적인 XIM 네트워크에 추가 됩니다 `xim_player_joined_state_change` 이며 이동 작업을 완료 합니다.

한 번이 집합 "matchmade" 플레이어를 사용 하 여 멀티 플레이어 경험을 끝냈으면 매치 메이 킹의 다른 반올림 하 여 다른 XIM 네트워크로 이동 하는 프로세스를 반복할 수 있습니다. 사전 통해 가입 하는 각 플레이어를 볼 수 `xim::move_to_network_using_matchmaking()` 작업을 제공는 `xim_player_left_state_change` 나타내기 위해 자신의 `xim_player` 개체는 동일한 XIM 네트워크에 더 이상.

경우 선택 하면 매치 메이 킹 사용 하 여 다시 시작 하려면 `xim::move_to_network_using_matchmaking()`, 지정한 `xim_players_to_move::bring_existing_social_players`, 소셜 진입점을 통해 연결 되는 플레이어 (`xim::move_to_network_using_protocol_activated_event_args()` 또는 `xim::move_to_network_using_joinable_xbox_user_id()`) 새 매치 메이 킹 수행 하는 동안 유지 됩니다. 또는 지정 `xim_players_to_move::bring_only_local_players` 소셜에서 연결 된 원격 플레이어에 종료 되도록 로컬 플레이어 바로 연결이 끊어집니다. 두 경우 모두에서 두 번째 이동 작업이 완료 되 면 다른 집합 낯선 사람 추가 됩니다.

수도 다음 매치 메이 킹 구성/멀티 플레이어 활동을 결정 하는 동안이 이동 비 matchmade 플레이어 (또는 로컬 방금 플레이어)를 완전히 새로운 XIM 네트워크에 있습니다. 다음 예제에서는 장치를 호출 하는 방법을 보여 줍니다 `xim::move_to_new_network()` 는 최대 8 플레이어 하지만이 이번에는 기존 소셜에서 가입 플레이어도 수행 XIM 네트워크에 대 한:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

A `xim_move_to_network_starting_state_change` 및 `xim_move_to_network_succeeded_state_change` 모든 참가 디바이스와 함께 제공 됩니다는 `xim_player_left_state_change` 남아 있는 matchmade 플레이어에 게 합니다. 이러한 장치에서 유사 하 게 표시 됩니다는 `xim_player_left_state_change` 네트워크 밖으로 이동 되는 각 플레이어에 대 한 합니다.

원하는 만큼 위에 설명 된 방식으로 XIM 네트워크에서 XIM 네트워크에 이동 계속할 수 있습니다.

성능에 Xbox Live 서비스 피어-투-피어에 대 한 직접 연결을 설정할 수 없는 장치에서 플레이어의 그룹에 맞게 시도 하지 않습니다. 매치 메이 킹 작업을 사용 하 여 새 네트워크로 이동 해야 확실 하는 경우에 성공적으로 일치 하지 않고 무기한 계속 될 수 있습니다 표준 Xbox Live 멀티 플레이어를 지원 하도록 제대로 구성 된 네트워크 환경에서 개발 하는 경우 충분 한 플레이어 매치 메이 킹 조건을 만족 하는 동일한 로컬 환경에서 모두 이동 하 고 사용 하 여 모든 장치. 영역/Xbox 응용 프로그램 네트워크 설정에 멀티 플레이 연결 테스트를 실행 하 고 특히 "엄격한 NAT"에 대 한 문제를 보고 하는 경우 해당 권장 사항을 준수 해야 합니다.

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>디버깅을 위해 매치 메이 킹 NAT 규칙을 사용 하지 않도록 설정

XIM 없이 하나 이상의 "열기" 엄격한 NAT "장치 일치를 허용 하도록 구성 하 여 Xbox One 개발 키트에 대 한 테스트 하 여 매치 메이 킹 차단을 해제할 수 네트워크 관리자에 게 XIM의 NAT 규칙을 지원 하기 위해 필요한 환경 변경할 수 없는 경우 NAT"장치입니다. 모든 Xbox One 콘솔에서 "제목 처음" 드라이브의 루트 (콘텐츠 중요 하지 않습니다.) "xim_disable_matchmaking_nat_rule" 이라는 파일을 배치 하 여 수행 됩니다. 이렇게 하려면 하나의 예 방법은 "{console_name_or_ip_address}" 자리 표시자를 적절 하 게 각 콘솔에 대 한 대체 앱을 시작 하기 전에 XDK 명령 프롬프트에서 다음을 실행 하 여입니다.

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> 이 개발 해결 방법만 Xbox One 단독 리소스 응용 프로그램에 사용할 수 있는 현재는 하 고 유니버설 Windows 응용 프로그램에 대 한 지원 되지 않습니다. 또한이 설정을 사용 하는 콘솔은이 파일에 관계 없이 네트워크 환경에 있는 없는 장치와 일치 하지는 해야 추가 하 고 모든 곳에서 파일을 제거 합니다.

## <a name="leaving-a-xim-network-and-cleaning-up"></a>XIM 네트워크를 종료 하 고 정리

로컬 사용자가 완료 되 면 사용자를 초대 XIM 네트워크를 종종 로컬 사용자를 허용 하는 새로운 XIM 네트워크로 이동 하면 됩니다에 참여 하 고 "뒤 에" 사용자가 다음 활동을 찾는 친구와 조정 계속 수 있도록 합니다. 사용자가 모든 멀티 플레이어 경험을 통해 완전히 수행 됩니다 경우 앱은 XIM을 종료 하려면 할 수 있지만 완전히 네트워크를 마치만 상태로 반환 `xim::initialize()` 및 `xim::set_intended_local_xbox_user_ids()` 를 호출한 합니다. 이 방법은 사용 하 여는 `xim::leave_network()` 방법:

```cpp
xim::singleton_instance().leave_network();
```

이 메서드는 비동기적으로 다른 참가자 로부터 적절 하 게 분리 하는 프로세스를 시작 합니다. 원격 디바이스를 제공 하면이 `xim_player_left_state_change` 연결이 끊어진 로컬 각 플레이어에 대 한 합니다. 로컬 장치 수도 제공 됩니다는 `xim_player_left_state_change` 연결이 각 로컬 및 원격 플레이어에 대 한 합니다. 모든 연결을 끊을 때 작업이 완료 되 면 최종 `xim_network_exited_state_change` 가 제공 됩니다. 앱을 호출할 수 ' xim::cleanup() '를 모든 리소스를 해제 하 고 초기화 되지 않은 상태로 돌아갑니다.

```cpp
xim::singleton_instance().cleanup();
```

호출 `xim::leave_network()` 는 대기 상태로 유지 합니다 `xim_network_exited_state_change` 는 XIM을 종료 하려면 네트워크 정상적으로 항상 적극 권장 경우는 `xim_network_exited_state_change` 에서 이미 제공 하지 않았습니다.

하는 경우 `xim::cleanup()` 가 호출 하지 않고 직접 `xim::leave_network()` 호출 되 고 메시지를 보낼 배달할 수 없는 장치에 단순히 "사라질" `xim::cleanup()`.

## <a name="sending-and-receiving-messages"></a>메시지 보내기 및 받기

XIM와 기본 구성 요소 연결 문제 또는 전부가 아닌 일부 플레이어에 연결할 수에 대해 걱정할 필요가 인터넷을 통해 보안 통신 채널을 설정 하는 모든 번거로운 작업을 수행 합니다. 모든 기본 피어 투 피어 연결 문제가 있는 경우 XIM 네트워크에 이동 성공 하지 않습니다.

XIM 네트워크 이며 형성 승인을 `xim_move_to_network_succeeded_state_change`, 모든 가입된 장치에서 앱의 인스턴스를 모두에 대해 숙지 보장이 모든 `xim_player` XIM 네트워크에 연결 하 고 그 중 하나를 메시지를 보낼 수 있습니다.

XIM 네트워크를 통해 메시지를 전송 하는 방법을 살펴보겠습니다. 다음 예제에서는 'sendingPlayer' 변수가 유효한 로컬 플레이어 개체에 대 한 포인터를 가정 합니다. 'sendingPlayer' 호출 `local()` 호출 `send_data_to_other_players()` 메시지 구조 'msgData' 보장, 순차적 전달 (로컬 및 원격) 한 모든 플레이어에 게 보낼 수 있습니다. 특정 한 플레이어 목록이 메서드로 전달 되지 않았습니다 때문에 모든 플레이어를 이동 하는지 note 하세요.

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

메시지를 사용 하 여 모든 플레이어를 전달할 수 있습니다 `send_data_to_other_players()` 하지 특정 플레이어 배열을 메서드에 전달 하 여 합니다.

메시지의 모든 받는 사람이 제공 되는 `xim_player_to_player_data_received_state_change` 해당 xim_player에 대 한 포인터를 신뢰 개체를 로컬로 받고 것은 물론 데이터의 복사본에 대 한 포인터를 포함 합니다.

물론 보장, 순차적 배달, 편리 하지만 XIM 재전송 또는 패킷 삭제 또는 인터넷에서 disordered 메시지를 지연 해야 하므로 비효율적인 보내기 형식의 일 수도 있습니다. 앱 잃어버리거나 손실 허용할 수는 메시지에 대 한 다른 보내기 형식을 사용 하는 것이 좋습니다 해야 순서에서 도착 합니다. 원격 컴퓨터에서 메시지 데이터를 가져오는, 이후 특정 바이트 순서 ("endianness")에 따라 멀티 바이트 값을 압축 하는 등의 데이터 형식을 명확 하 게 정의 하는 것이 좋습니다. 앱이 작업을 수행 하기 전에 데이터 유효성 검사도 해야 합니다.

추가 암호화 또는 서명 스키마에 대 한 않아도 되므로 XIM 네트워크 수준의 보안을 제공 합니다. 하지만 권장 항상 다른 버전의 응용 프로그램 프로토콜 존재할 정상적으로 (중 개발, 콘텐츠 업데이트 등)를 처리 하려면 "심층" 실수로 응용 프로그램 버그를 방지 하기 위해 사용 됩니다. 사용자의 인터넷 연결 제한 및 급변 리소스 수 있습니다. 효율적인 메시지 데이터 형식 및 UI 프레임 마다 보낼 피하고 디자인의 사용을 권장 합니다.

## <a name="assessing-data-pathway-quality"></a>데이터 경로 품질 평가

두 플레이어 간에 데이터 작업의 현재 품질에 대 한 자세한를 호출 합니다 `xim_player::network_path_information()` 메서드. 다음 예제에서는 검색에 대 한 포인터를 `xim_network_path_information` 에 대 한 구조는 `xim_player` 'remotePlayer' 변수에 포함 된 포인터:

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

반환 된 구조에 예상된 왕복 대기 시간 및 얼마나 많은 메시지는 여전히 대기 로컬로 경우에서 전송 하기 위해 해당 시점에 대 한 더 많은 데이터를 전송 연결을 지원할 수 없는 포함 됩니다.

특정 'xim_player'에 대 한 큐를 백업 하 고 표시 하는 경우 데이터를 전송 하는 속도 줄여야 합니다.

`xim_network_path_information::round_trip_latency_in_milliseconds` 필드 큐 없이 기본 네트워크의 대기 시간 및 XIM의 예상된 대기 시간을 나타냅니다. 유효 대기 시간 증가 `xim_network_path_information::send_queue_size_in_messages` 증가 하 고 XIM 큐를 통해 작동 합니다.

적절 한 지점에 대 한 호출을 제한 하려면 선택 `send_data_to_other_players` 게임의 사용 및 요구 사항에 따라 합니다. 모든 메시지 보내기 큐에 효과적인 네트워크 대기 시간이 증가 나타냅니다.

XIM의 최대 제한 (현재 3500 메시지) 가까운 값이 너무 높아 대부분의 게임 및 가능성이 몇 초 정도 호출의 속도 따라 전송 대기 중인 데이터를 나타내는 `send_data_to_other_players` 각 데이터 페이로드 크기는 합니다. 게임의 어떻게 떨림 함께 게임의 대기 시간 요구를 고려 하는 숫자 대신 선택 `send_data_to_other_players` 호출 패턴입니다.

## <a name="sharing-data-using-player-custom-properties"></a>플레이어가 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.

대부분의 앱 데이터 교환 발생할 합니다 `xim_player::xim_local::send_data_to_other_players()` 가장 잘 제어할 데이터를 받을 사용자, 해당 데이터를 받을 해야 때 및 패킷 손실 시스템 처리 해야 하는 방법을 수 있으므로 메서드. 그러나 때 좋을 플레이어가 사람과 자체에 대 한 기본 상태로 거의 변화를 간단 하 게 공유할 수 있습니다. 예를 들어 각 플레이어 모든 플레이어 게임에서 표현으로 렌더링을 사용 하는 멀티 플레이어를 시작 하기 전에 선택한 문자 모델을 나타내는 고정된 문자열을 사용할 수 있습니다.

플레이어에 대 한 자주 변경 되지 않는 데이터, 사용자 지정 속성 XIM 플레이어를 제공 합니다. 이러한 속성의 로컬 플레이어에 게 적용할 수 있는 변경 될 때마다 자동으로 모든 장치에 전파 가져오기 null로 끝나는 문자열 쌍 되는 앱 정의 이름 및 값으로 구성 됩니다.

사용자 지정 플레이어 속성에 오버 헤드가 보다 더 많은 내부 동기화는 `xim_player::xim_local::send_data_to_other_players()` 메서드를 되므로 (즉, 플레이어가 위치) 데이터를 신속 하 게 변경에 대 한 여전히 직접 전송을 사용 해야 합니다.

플레이어가 사용자 지정 속성 키-값 쌍의 현재 값은 이러한 장치 XIM 네트워크에 가입 하 고 추가 플레이어를 확인할 때 참여 하는 새 장치에 자동으로 제공 했습니다. 호출 하 여 값을 설정할 수 있습니다 `xim_player::xim_local::set_player_custom_property()` 이름과 값 문자열을 포함 합니다. 다음 예제에서는 로컬에서 "무차별" 값을 "모델" 라는 속성이 설정 `xim_player` 변수 'localPlayer' 가리키는 개체.

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

플레이어 속성 변경 하면는 `xim_player_custom_properties_changed_state_change` 모든 장치에 변경 된 속성의 이름에 알려 제공 하도록 합니다. 사용 하 여 플레이어, 로컬 또는 원격 ","에 해당 이름에 대 한 값을 검색할 수 `xim_player::get_player_custom_property()`. 다음 예제에서 "모델" 라는 속성에 대 한 값을 검색 한 `xim_player` 'ximPlayer' 변수를 통해 가리키는:

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

지정 된 속성 이름에 대 한 새로운 값을 설정 하는 모든 기존 값을 바꿉니다. 처리 되는 속성 이름이 null 값 문자열 포인터의 값으로 설정 되지 아직 지정 된 것은 동일 빈 값 문자열을와 동일 합니다. 이름과 값 XIM에 의해 해석 하지 않는, 필요에 따라 문자열 콘텐츠 유효성을 검사 하는 앱에 남게 된 따라서 합니다.

사용자 지정 플레이어 속성은 항상 하나의 XIM 네트워크에서 다른 컴퓨터로 이동 하는 경우 다시 설정 합니다. 그러나 XIM 네트워크에 가입 하는 새로운 플레이어는 일부 집합을 가진 모든 플레이어에 대 한 현재 사용자 지정 플레이어 속성을 받게 됩니다.

## <a name="sharing-data-using-network-custom-properties"></a>네트워크 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.

네트워크 사용자 지정 속성의 편의 자주 변경 되지 않는 XIM 네트워크에 특정 데이터를 동기화 할 것입니다. XIM 의도 하지 않은 단일 개체에 설정 점을 제외 하 고 [플레이어가 사용자 지정 속성](#sharing-data-using-player-custom-properties)을 동일 하 게 사용자 지정 속성 작업 네트워크 `xim::set_network_custom_property()`. 다음 예제에서는 "지도" 속성 "요새" 값을 설정 합니다.

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

네트워크 속성을 변경 하면는 `xim_network_custom_properties_changed_state_change` 모든 장치에 변경 된 속성의 이름에 알려 제공 하도록 합니다. 지정 된 이름에 대 한 값을 검색할 수 있습니다 `xim::get_network_custom_property()`같이 속성 명명 된 "지도"에 대 한 값을 검색 하는 다음 예에서:

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

[플레이어가 사용자 지정 속성](#sharing-data-using-player-custom-properties)을 마찬가지로 지정 된 속성 이름에 대 한 새로운 값을 설정 기존 값을 바뀝니다. 처리 되는 속성 이름이 null 값 문자열 포인터의 값으로 설정 되지 아직 지정 된 것은 동일 빈 값 문자열을와 동일 합니다. 이름과 값 XIM에 의해 해석 하지 않는, 필요에 따라 문자열 콘텐츠 유효성을 검사 하는 앱에 남게 된 따라서 합니다.

새로 만든 XIM 네트워크는 항상 없는 네트워크 사용자 지정 속성 집합으로 시작 합니다. 그러나 기존 XIM 네트워크에 가입 하는 새로운 플레이어 해당 XIM 네트워크에 대 한 설정 네트워크 사용자 지정 속성의 현재 값을 받게 됩니다.

## <a name="matchmaking-using-per-player-skill"></a>싱글 플레이어 기술을 사용 하 여 매치 메이 킹

특정 앱에서 지정한 게임 모드에서 관심사 하 여 플레이어와 일치 하는 좋은 기본 전략입니다. 사용 가능한 플레이어의 풀 성장에 맞춰 고려해 야도 숙련 된 플레이어 최신 플레이어 수 증가 하는 동안 다른 낮 선 정상 경쟁 하는 문제를 즐길 수 있도록 자신의 개인 기술 또는 게임 경험에 따라 플레이어를 일치 다른 사용자에 대해 유사한 기능을 사용 하 여 경쟁 합니다.

이렇게 하려면 싱글 플레이어 역할 및 기술 구성 구조에 대 한 호출에 지정 된 모든 로컬 플레이어에 대 한 기술 수준을 제공 하 여 시작 `xim_player::xim_local::set_roles_and_skill_configuration()` 는 XIM 이동 하려면 시작 하기 전에 매치를 사용 하는 네트워크. 숙련도 앱 관련 개념 이며 매치 메이 킹 먼저 찾으려고 같은 기술 값을 사용 하는 플레이어가 되며 다음 기술 선언 다른 플레이어를 찾으려고 10 + /-단위로 검색을 정기적으로 확대 된다는 수 XIM에 의해 해석 되지 않습니다. 해당 기술 주위 범위의 값입니다. 다음 예제에서는 가정 하는 로컬 `xim_player` 개체를 해당 포인터가 'localPlayer', 'playerSkillValue' 라는 변수에 로컬 또는 Xbox Live 저장소에서 검색 관련된 앱 별 uint32_t 기술 값은:

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

모든 참가자에 게 제공 될 예정이 완료 되 면는 `xim_player_roles_and_skill_configuration_changed_state_change` 이 나타내는 `xim_player` 은 싱글 플레이어 역할 및 기술 구성 변경 되었습니다. 호출 하 여 새 값을 검색할 수 있습니다 `xim_player::roles_and_skill_configuration()`. 모든 플레이어가 null이 아닌 역할 및 기술 구성을 적용 하는 경우 true 값을 사용 하 여 매치를 사용 하 여 XIM 네트워크에 이동할 수 있습니다 합니다 `require_player_roles_and_skill_configuration` 필드의 `xim_matchmaking_configuration` 구조에 지정 된 `xim::move_to_network_using_matchmaking()`합니다. 다음 예제에서는 다른 플레이어와 일치만 MYGAMEMODE_DEATHMATCH 값으로 정의 하는 앱 특정 게임 모드 상수 uint64_t를 사용 하 여 아니요 팀 개인 대전에 대 한 총 2-8 플레이어를 찾을 수 있는 매치 메이 킹 구성 동일한 값을 고 싱글 플레이어 역할 및 기술 구성 해야 한다는 것을 지정 합니다.

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

이 구조는 제공 되는 경우 `xim::move_to_network_using_matchmaking()`, 이동 작업이 정상적으로 만큼 이동 하는 플레이어 불 렀 시작 `xim_player::xim_local::set_roles_and_skill_configuration()` null이 아닌를 사용 하 여 `xim_player_roles_and_skill_configuration` 포인터. 모든 플레이어 되지 않은 경우, 다음 매치 메이 킹 프로세스는 일시 중지 하 고 모든 참가자에 게 제공 될 예정 하는 경우는 `xim_matchmaking_progress_updated_state_change` 으로 `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` 값. 이후에 이전에 보낸된 초대를 통해 또는 다른 수단을 통해 XIM 네트워크에 가입 하는 플레이어 포함 됩니다 (예: 호출 `xim::move_to_network_using_joinable_xbox_user_id()`) 매치 메이 킹 완료 되기 전에 합니다. 모든 플레이어 제공 되 면 해당 `xim_player_roles_and_skill_configuration` 구조 매치 메이 킹 다시 시작 됩니다.

다음 섹션에 설명 된 대로 플레이어 당 기술을 사용 하 여 매치 매치 메이 킹 사용자 당 싱글 플레이어 역할을 사용 하 여 결합할 수도 있습니다. 하나는 원하는 하나만 있는 경우 다른 0 값을 지정할 수 있습니다. 즉 선언 하는 모든 플레이어가지고 있기 때문에 `xim_player_roles_and_skill_configuration` 기술 값 0은 서로 일치 항상 합니다.

한 번의 `xim::move_to_network_using_matchmaking()` 또는 다른 XIM 네트워크 작업이 완료 된 모든 플레이어의 이동 `xim_player_roles_and_skill_configuration` 구조를 null 포인터 자동으로 해제 됩니다 (을 함께 사용 하 여 `xim_player_roles_and_skill_configuration_changed_state_change` 알림). 호출 해야 싱글 플레이어 구성이 필요 매치를 사용 하 여 다른 XIM 네트워크로 이동 하려는 경우 `xim_player::xim_local::set_roles_and_skill_configuration()` 최신 정보를 포함 하는 새 구조 포인터를 사용 하 여 다시 합니다.

## <a name="matchmaking-using-per-player-role"></a>싱글 플레이어 역할을 사용 하 여 매치 메이 킹

또 다른 방법은 싱글 플레이어 역할 및 기술 구성을 사용 하 여 사용자의 매치 메이 킹 환경을 개선 하기 위해 필요한 플레이어 역할을 사용 하 여입니다. 이 다른 협조적 재생 스타일을 유도 하는 선택 가능한 문자 유형을 제공 하는 게임에 가장 적합 즉, 게임에서 그래픽 표현 변경 해도 하지 않지만 방어 "치료" 근접 "근거리" 불쾌할 비교 멀리 떨어진 "범위"와 같은 보완, impactful 특성을 제어 하는 형식 공격 지원 합니다. 사용자의 성격이 특정 전문으로 재생할 수도 것을 의미 합니다. 불가능 기능적 명 이상 각 역할을 수행 하지 않고 목표를 완료 하려면 되도록 게임을 디자인할 때로는 것이 일치 모든 플레이어 함께 다음 보다 먼저 협상 하도록 이러한 플레이어와 일치 하도록 하지만 한 번 수집 간에 스타일을 재생 합니다. 첫 번째 각 역할을 특정된 플레이어에 지정할 수를 나타내는 고유한 비트 플래그를 정의 하 여이 수행할 수 있는 `xim_player_roles_and_skill_configuration` 구조입니다.

다음 예제에서는 'localPlayer' 포인터가 로컬 xim_player 개체에 대 한 앱 별 MYROLEBITFLAG_HEALER uint8_t 역할 값을 설정 합니다.

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

모든 참가자에 게 제공 될 예정이 완료 되 면는 `xim_player_roles_and_skill_configuration_changed_state_change` 이 나타내는 `xim_player` 의 싱글 플레이어 역할 구성이 변경 되었습니다. 호출 하 여 새 값을 검색할 수 있습니다 `xim_player::roles_and_skill_configuration()`.

전역 `xim_matchmaking_configuration` 에 지정 된 구조 `xim::move_to_network_using_matchmaking()` 비트 OR를 사용 하 고 값이 true 이면 require_player_matchmaking_configuration 필드에 대 한 결합 된 모든 필요한 역할이 플래그 해야 합니다.

다음 예제에서는 아니요 팀 개인 대전에 대 한 총 3 플레이어를 찾을 수 있는 매치 메이 킹 구성을 채웁니다. 또한이 예제는 유형 uint64_t의 이며 오프 필터링 하는 게임 모드를 나타내는 MYGAMEMODE_COOPERATIVE 라는 앱 정의 상수를 사용 합니다. 싱글 플레이어 역할 및 기술 구성 이상의 플레이어 비트 OR 했던 각 앱 별 uint8_t 역할을 수행 하는 위치 (MYROLEBITFLAG_HEALER, MYROLEBITFLAG_ 구성에 배치 되어 함께 요구 하도록 구성 설정 또한 근거리, 및 MYROLEBITFLAG_RANGE):

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

이 구조는 제공 되는 경우 `xim::move_to_network_using_matchmaking()`, 위에서 설명한 대로 이동 작업이 시작 됩니다.

싱글 플레이어 역할을 사용 하 여 매치 매치 메이 킹 사용자 당 플레이어 기술을 사용 하 여 결합할 수도 있습니다. 하나는 원하는 하나만 있는 경우 다른 값을 0으로 지정 합니다. 즉 선언 하는 모든 플레이어가지고 있기 때문에 `xim_player_roles_and_skill_configuration` 기술 값 0은 항상 일치; 서로 모든 비트가 0에는 경우는 `xim_matchmaking_configuration` required_roles 필드에 없는 역할 비트 일치 하기 위해 필요 합니다.

한 번의 `xim::move_to_network_using_matchmaking()` 또는 다른 XIM 네트워크 작업이 완료 된 모든 플레이어의 이동 `xim_player_roles_and_skill_configuration` 구조를 null 포인터 자동으로 해제 됩니다 (을 함께 사용 하 여 `xim_player_roles_and_skill_configuration_changed_state_change` 알림). 호출 해야 싱글 플레이어 구성이 필요 매치를 사용 하 여 다른 XIM 네트워크로 이동 하려는 경우 `xim_player::xim_local::set_roles_and_skill_configuration()` 최신 정보를 포함 하는 새 구조 포인터를 사용 하 여 다시 합니다.

## <a name="how-xim-works-with-player-teams"></a>XIM은 플레이어 팀과 함께 작동 하는 방법

멀티 플레이어 반대 팀에 구성 된 플레이어는 종종 게임 합니다. XIM 사용 하면 때 팀 할당을 쉽게 설정 하 여 매치 메이 킹 `xim_team_configuration`. 다음 예제에서는 시작 (단, 4를 찾을 수 없는 경우 1 ~ 3 플레이어 허용 요소도) 4의 두 같은 팀에 배치 하는 총 8 플레이어를 찾으려고 구성 매치 메이 킹을 사용 하 여 이동, 값에 의해 정의 된 앱 별 게임 모드 상수 uint64_t를 사용 하 여 동일 하 게 값을 지정 하 고 현재 XIM 네트워크에서 사회에 가입 된 모든 플레이어를 가져와 다른 플레이어와 일치만 MYGAMEMODE_CAPTURETHEFLAG:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

이러한는 XIM 네트워크 이동 작업이 완료 되 면 플레이어 팀 인덱스 1 {n}를 통해 해당 하는 값을 요청 {n} 팀이 할당 됩니다. 특정 팀 인덱스 값의 의미를 true은 앱 달려 있습니다. 플레이어의 팀 인덱스 값을 통해 검색 `xim_player::team_index()`. 사용 하는 경우는 `xim_team_configuration` 두 개 이상의 팀과 함께 플레이어 되지 할당할 팀 인덱스 값이 0에 대 한 호출 하 여 `xim::move_to_network_using_matchmaking()`. 반면에 다른 구성 요소나 유형의 이동 작업을 사용 하 여 XIM 네트워크에 추가 하는 플레이어에 게 이것이 (같은 초대를 수락에서 발생 하는 프로토콜 활성화를 통해), 사용자는 항상 인덱스가 0 팀 합니다. 특수 "할당 되지 않은" 팀으로 팀 인덱스 0을 처리할 수 있습니다.

다음 예제에서는 포인터가 'ximPlayer' 변수에 해당 하는 xim_player 개체에 대 한 팀 인덱스를 검색 합니다.

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

(필요가, 음수 플레이어의 동작에 대 한 언급 된 감소 기회) 기본 사용자 환경을 위해 매치 메이 킹 Xbox Live 서비스는 다른 팀에 함께 XIM 네트워크로 이동 하는 분할 플레이어 하지 않습니다.

매치 메이 킹이 처음 할당 팀 인덱스 값은 권장만 및 앱을 사용 하 여 원하는 시간에 로컬 플레이어에 게 변경할 수 있습니다 `xim_player::xim_local::set_team_index()`. 이 매치 메이 킹을 전혀 사용 하지 않는 XIM 네트워크에서 호출할 수도 있습니다. 다음 예제에서는 하나의 새 팀 인덱스 값을 가질 ' localPlayer' 플레이어가 포인터를 구성 합니다.

```cpp
localPlayer->local()->set_team_index(1);
```

플레이어는 새 팀 인덱스 값을 적용 제공 하며 때 모든 디바이스는 정보는 `xim_player_team_index_changed_state_change` 는 플레이어에 대 한 합니다. 호출할 수 있습니다 `xim_player::xim_local::set_team_index()` 언제 든 지.

매치 메이 킹 팀에서 독립적으로 필요한 플레이어 역할을 평가합니다. 따라서 접수 플레이어 역할을 받지 팀 플레이어 수에 따라 조정 하기 때문에 동시 매치 메이 킹 구성 기준으로 팀와 필요한 역할을 사용 하 여 권장 되지 했습니다.

## <a name="working-with-chat"></a>채팅 작업

음성 및 문자 채팅 통신 XIM 네트워크에서 플레이어 간에 자동으로 활성화 됩니다. XIM 수에 대 한 모든 음성 헤드셋 및 마이크 하드웨어와 상호 작용을 처리 합니다. 앱 필요한 경우가 아니면 채팅, 많은 작업을 수행 하지만 텍스트 채팅에 대 한 요구 사항이 하나 없는: 입력 및 디스플레이 지원 합니다. 텍스트 입력이 플랫폼을 사용 하 여 광범위 한 실제 키보드 지금까지 적는 게임 장르에 대해서도 플레이어 텍스트 음성 변환 보조 기술을 사용 하 여 시스템을 구성할 수 있기 때문에 필요 합니다. 마찬가지로, 텍스트 표시는 플레이어가 음성-텍스트를 사용 하 여 시스템을 구성할 수 있으므로 필요 합니다.

이러한 기본 설정 호출 하 여 로컬 플레이어에서 감지할 수 `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` 및 `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()`, 및 조건부로 텍스트 메커니즘을 사용 하고자 할 수 있습니다. 그러나 텍스트 입력 및 디스플레이 항상 사용할 수 있는 옵션을 고려 하는 것이 좋습니다.

 `Windows::Xbox::UI::Accessability` Xbox One 클래스를 하도록 설계 된 음성-텍스트 보조 기술에 초점을 사용 하 여 게임에서 텍스트 채팅의 간단한 렌더링을 제공 합니다.

실제 또는 가상 키보드에서 제공 하는 텍스트 입력을 구성한 후에 문자열을 전달 합니다 `xim_player::xim_local::send_chat_text()` 메서드. 다음 코드는 로컬 컴퓨터에서 하드 코드 된 문자열의 예를 보내기 `xim_player` 변수 'localPlayer' 가리키는 개체.

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

이 채팅 텍스트에 원래 로컬 플레이어에서 채팅 통신을 수신할 수 있는 XIM 네트워크에서 모든 플레이어에 게 제공 됩니다. 음성 오디오를 합성 될 수 및로 제공 될 수는 `xim_chat_text_received_state_change`.

앱 받은 모든 텍스트 문자열의 복사본을 만들고 하 고 적절 한 금액이 시간 (또는 스크롤 가능한 창)에 대 한 원래 플레이어의 일부 식별 함께 표시 해야 합니다.

채팅에 대 한 몇 가지 모범 사례 있습니다. 아무 곳 이나 플레이어에에서 표시 되어 있는지, 특히 게이머 점수 판, 같은 태그 목록을 사용자에 대 한 피드백으로 아이콘 음소거/말하기를 표시할 수도 것이 좋습니다. 이 호출 하 여 수행 됩니다 `xim_player::chat_indicator()` 를 검색 하는 `xim_player_chat_indicator` 해당 플레이어에 대 한 채팅의 현재, 일시적인 상태를 나타내는 합니다. 다음 예제에 대 한 표시기 값을 검색 하는 `xim_player` 'ximPlayer'를 'iconToShow' 변수에 할당 하는 특정 아이콘 상수 값을 결정 하는 변수 가리키는 개체:

```cpp
switch (ximPlayer->chat_indicator())
{
   case xim_player_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

값에서 보고 `xim_player::chat_indicator()` 예를 들어 플레이어 시작 및 중지 말하기, 자주 변경 해야 합니다. 폴링 것 UI 프레임 마다 결과적으로 앱을 지원 하도록 설계 되었습니다.

> [!NOTE]
> 로컬 사용자가 디바이스 설정 인해 통신 권한이 없는 경우 `xim_player::chat_indicator()` 돌아올 `xim_player_chat_indicator::platform_restricted`. 플랫폼에 대 한 요구 사항을 충족 하도록 기대 앱 플랫폼 제한 음성 채팅 또는 메시징, 및 문제를 나타내는 사용자에 게 메시지를 나타내는 있는 표시할 수 있다는 점입니다. 예를 들어 메시지는 것이 좋습니다. "죄송 하지만, 지금 바로 채팅 수 없습니다."

## <a name="muting-players"></a>플레이어 음소거

또 다른 최선의 음소거 플레이어를 지원 하기 위해서입니다. XIM 자동으로 시스템 음소거: 플레이어 카드를 통해 사용자가 시작을 처리 하지만 게임 관련 임시 음소거를 통해 게임 UI 내에서 수행할 수 있는 앱을 지원 해야 합니다 `xim_player::set_chat_muted()` 메서드. 다음 예제에서는 원격 음소거 시작 `xim_player` 가리키는 개체 변수 'remotePlayer' 없이 음성 채팅 들립니다 없는 텍스트 채팅은 여기에서 수신 되도록 합니다.

```cpp
remotePlayer->set_chat_muted(true);
```

음소거 즉시 적용 하 고 있으면 없음 `xim_state_change` 연결 된 합니다. 호출 하 여 실행 취소할 수 `xim_player::set_chat_muted()` false 값을 사용 합니다. 다음 예제에서는 원격 unmutes `xim_player` 변수 'remotePlayer' 가리키는 개체.

```cpp
remotePlayer->set_chat_muted(false);
```

음을 소거 조건이 적용에 대 한으로 `xim_player` 플레이어를 사용 하 여 새로운 XIM 네트워크에 이동할 때 포함 하 여 존재 합니다. 플레이어를 벗어날 경우 유지 되지 않습니다 동일한 사용자가 다시 가입 하 고 (으로 새 `xim_player` 인스턴스).

플레이어는 일반적으로 unmuted 상태에서 시작합니다. 호출할 수 있는 앱에서 게임 플레이 이유로 음소거 상태는 플레이어가 시작 하려는 경우 `xim_player::set_chat_muted()` 에 `xim_player` 개체는 연결 된 처리를 완료 하기 전에 `xim_player_joined_state_change`, XIM 트는 기간 없음 음성 오디오 플레이어에서 될 수 있는 보장 됩니다 들립니다.

플레이어 평판 기반 자동 음소거 검사를 원격 플레이어를 XIM 네트워크에 연결 하는 경우 발생 합니다. 플레이어가 나쁜 평판 플래그 있으면 플레이어 자동으로 음소거 합니다. 음소거 로컬 상태에만 적용 되 고 따라서 네트워크를 통해 플레이어가 이동 하는 경우를 유지 합니다. 자동 평판 기반 음소거 검사는 한 번 수행 하 고 다시 평가 되지 다시에 대 한으로 `xim_player` 유효 합니다.

## <a name="configuring-chat-targets-using-player-teams"></a>팀이 플레이어를 사용 하 여 채팅 대상 구성

이 문서의 [플레이어 팀과 함께 작동 하는 방법을 XIM](#how-xim-works-with-player-teams) 섹션에서 설명 했 듯이 특정 팀 인덱스 값의 의미를 true은 앱 달려 있습니다. XIM 채팅 대상 구성에 대해 일치 비교를 제외 하 고 해석 하지 않습니다. 채팅 대상 구성에서 보고 하는 경우 `xim::chat_targets()` 현재 `xim_chat_targets::same_team_index_only`를 지정 된 모든 플레이어는 다른 채팅 통신 교환할만 두가 동일한 값에서 보고 하는 경우 다음 `xim_player::team_index()` (및 개인 정보 보호 정책 에서도 허용 /).

기본적으로 구성 된 보수적이 고 XIM 네트워크를 새로 만든 경쟁 시나리오는 자동으로 지원 `xim_chat_targets::same_team_index_only`. 그러나 다른 팀에서 점령한 상대 채팅 "로비" 후 게임의 예를 들어 좋을 수 있습니다. 호출 하 여 모든 (여기서 개인 정보 및 정책 허용)에 게 모든 XIM 하도록 지시할 수 있습니다 `xim::set_chat_targets()`. 다음 샘플 시작 구성 XIM 네트워크에서 사용 하 여 모든 참가자는 `xim_chat_targets::all_players` 값:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

제공 하며 때 새 대상 설정을 적용 되는 모든 참가자는 정보는 `xim_chat_targets_changed_state_change`.

앞에서 설명한 대로 대부분의 XIM 네트워크 이동 유형에 처음 할당 모든 플레이어 팀 인덱스 값으로 0입니다. 즉, 구성 `xim_chat_targets::same_team_index_only` 가능성이 구별할 `xim_chat_targets::all_players` 기본적으로 합니다. 그러나 다른 경우 인덱스 값 팀 매치를 사용 하 여 XIM 네트워크로 이동 하는 플레이어를 갖게 됩니다 매치 메이 킹 구성의 `xim_team_matchmaking_mode` 값 두 개 이상의 팀 선언 됩니다. 또한 호출할 수 있습니다 `xim_player::xim_local::set_team_index()` 언제 든 지 위와 같이 합니다. 앱에서 이러한 메서드 중 하나를 통해 0이 아닌 팀 인덱스 값을 사용 중인 경우 적절 하 게 설정 현재 채팅 대상 관리할 것을 잊지 마세요.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>플레이어 슬롯 ("백필" 매치 메이 킹)의 자동 백그라운드 채우기

호출 플레이어의 서로 다른 그룹 `xim::move_to_network_using_matchmaking()` 이와 동시에 유연성을 제공 매치 메이 킹 Xbox Live 서비스의 가장 큰 신속 하 게 새로운, 최적의 XIM 네트워크를 구성 합니다. 그러나 일부 게임 플레이 시나리오 그대로 특정 XIM 네트워크 및 빈 플레이어 슬롯 채우기 에게만 추가 플레이어 matchmake만을 유지 하고자 합니다. XIM 지원 모드 또는 "백필"를 호출 하 여 작성 된 자동 백그라운드에서 작동 하는 데 매치 메이 킹 구성 `xim::set_network_configuration()` 사용 하 여는 `xim_network_configuration` 가 `xim_allowed_player_joins::matchmade` 플래그가 설정에 해당 `xim_network_configuration::allowed_player_joins` 속성입니다.

다음 예제에서는 구성 (하지만 8를 찾을 수 없는 경우 2-7 플레이어 또한 허용) 아니요 팀 개인 대전에 대 한 총 8 플레이어를 찾을 하려고 백필 매치 메이 킹, MYGAMEMODE_ 값에 의해 정의 된 앱 별 게임 모드 상수 uint64_t를 사용 하 여 매치만 동일 하 게 값을 지정 하는 다른 플레이어와 일치 합니다.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

이렇게 하면 네트워크 기존 XIM 호출 하는 장치에 사용할 수 있는 `xim::move_to_network_using_matchmaking()` 표준 방식에서입니다. 이러한 장치 변경 동작이 없습니다. 역으로 채우는 XIM 네트워크에 참여 움직이지 것입니다 하지만 제공 될 예정는 `xim_network_configuration_changed_state_change` 켜기, 백필 뿐만 아니라 여러 나타내는 `xim_matchmaking_progress_updated_state_change` 알림 해당 하는 경우. 모든 matchmade 플레이어 법선을 사용 하 여 XIM 네트워크에 추가 됩니다 `xim_player_joined_state_change`.

기본적으로 백필 매치 메이 킹 남아 진행 무기한 XIM 네트워크 xim_team_configuration 설정에 지정 된 플레이어의 최대 수를 이미 있으면 플레이어를 추가 하려고 시도 하지 않지만 있습니다. 역으로 채우는 matchmade을 허용 하지 않도록 설정 xim_allowed_player_joins 비활성화할 수 있습니다. 다음 예제에서는 다른 모든 기존 플래그와 네트워크 구성 설정을 유지 하면서 xim_allowed_player_joins::matchmade 플래그를 취소 하 여 역으로 채우는 비활성화 합니다.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

해당 `xim_network_configuration_changed_state_change` 이 비동기 프로세스가 완료 되 면 최종 되 면 및 모든 장치에 제공 될 예정 `xim_matchmaking_progress_updated_state_change` 으로 제공 될 예정 `xim_matchmaking_status::none` 을 나타내는 없는 추가 matchmade 플레이어를 XIM 네트워크에 추가 됩니다.

사용 하 여 백필 매치를 사용 하도록 설정할 때는 `xim_team_configuration` 두 개 이상의 팀을 선언 하는 설정, 모든 기존 플레이어 1과 팀 수가 사이 있는 유효한 팀 인덱스 있어야 합니다. 불 렀 플레이어 포함 됩니다 `xim_player::xim_local::set_team_index()` 사용자 지정 값 또는 사용자가 초대를 사용 하 여 가입한 나 다른 소셜 통해 의미를 지정할 수 (예: 호출 `xim::move_to_network_using_joinable_xbox_user_id`)는 기본 팀 인덱스 값 0으로 추가 되었습니다. 모든 플레이어는 유효한 팀 인덱스 없는 면 매치 메이 킹 프로세스는 일시 중지 하 고 모든 참가자가 제공 됩니다는 `xim_matchmaking_progress_updated_state_change` 으로 `xim_matchmaking_status::waiting_for_player_team_index` 값입니다. 모든 플레이어 제공 있거나 해당 팀 인덱스 값을 수정 되 면 `xim_player::xim_local::set_team_index()`, 백필 매치 메이 킹 계정이 다시 시작 됩니다. 자세한 내용은이 문서의 [플레이어 팀과 함께 작동 하는 방법을 XIM](#how-xim-works-with-player-teams) 섹션에서 찾을 수 있습니다.

마찬가지로, 사용 하 여 백필 매치를 사용 하도록 설정할 때는 `xim_network_configuration` 와 구조는 `require_player_roles_and_skill_configuration` 역할 또는 기술에 대 한 true로 설정 모든 플레이어 null이 아닌 당 싱글 플레이어 매치 메이 킹 구성을 지정 된 다음 필드. 모든 플레이어 되지 않은 경우, 다음 매치 메이 킹 프로세스는 일시 중지 하 고 모든 참가자에 게 제공 될 예정 하는 경우는 `xim_matchmaking_progress_updated_state_change` 으로 `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` 값. 모든 플레이어 제공 되 면 해당 `xim_player_roles_and_skill_configuration` 구조 백필 매치 메이 킹 다시 시작 됩니다. 자세한 내용은이 문서의 [매치 메이 킹 플레이어 당 기술을 사용 하 여](#matchmaking-using-per-player-skill) 및 [싱글 플레이어 역할을 사용 하 여 매치 메이 킹](#matchmaking-using-per-player-role) 섹션에서 찾을 수 있습니다.

## <a name="querying-joinable-networks"></a>쿼리 있는 네트워크

매치 메이 킹 신속 하 게 플레이어를 함께 연결 하는 훌륭한 방법 이지만, 경우에 따라 것이 가장 좋습니다 사용자 지정 검색 조건을 사용 하 여 있는 네트워크 검색을 허용 하 고 가입 하려는 네트워크를 선택 합니다. 이 게임 세션 다양 한 구성 가능한 게임 규칙 및 플레이어 설정 있을 때 특히 유용할 수 있습니다. 이렇게 하려면 기존 네트워크 먼저 만들어져야 쿼리할 수를 사용 하 여 `xim_allowed_player_joins::queried` 파티 및 호출을 통해 네트워크 외부 다른 사람에 게 제공 되는 네트워크 정보 구성 `xim::set_network_configuration()`.

다음 예제에서는 `xim_allowed_player_joins::queried` 파티, 설정 네트워크 1 팀 함께 1-8 플레이어의 총 수 있도록 팀 구성 사용 하 여 구성, "고양이 GAME_MODE_BRAWL에 대 한 설명 값으로는 앱 특정 게임 모드 상수 uint64_t 정의 앱 별 지도 인덱스 상수 uint32_t MAP_KITCHEN 값으로 정의 양의 박싱 일치"태그"chatrequired","쉽게","spectatorallowed"포함 됩니다.

```cpp
PCWSTR tags[] = { L"chatrequired", L"easy", L"spectatorallowed" };
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::queried;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 1;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = GAME_MODE_BRAWL;
networkConfiguration.description = L"cat and sheep's boxing match";
networkConfiguration.map_index = MAP_KITCHEN;
networkConfiguration.tag_count = _countof(tags);
networkConfiguration.tags = tags;

xim::set_network_configuration(&networkConfiguration);
```

네트워크 외부에서 다른 플레이어를 호출 하 여 네트워크를 찾을 수 `xim::start_joinable_network_query()` 이전 네트워크 정보를 일치 하는 필터의 집합을 사용 하 여 `xim::set_network_configuration()` 를 호출 합니다. 다음 예제에서는 게임 모드 필터 옵션은 GAME_MODE_BRAWL 값으로 정의 하는 앱 별 게임 모드를 사용 하 여 네트워크에 대 한 쿼리만 사용 하 여 있는 네트워크 쿼리를 시작 합니다.

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

"쉽게" 태그를 가진 네트워크와 그 공개 쿼리 가능 구성에서 "spectatorallowed"에 대 한 쿼리 하는 태그 필터 옵션을 사용 하는 또 다른 예는 다음과 같습니다.

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

다른 필터 옵션을 결합할 수도 있습니다. 게임 모드 필터 옵션와 태그를 사용 하는 다음 예제에서는 둘 다 있는 태그와 앱 별 게임 모드 상수 GAME_MODE_BRAWL "쉽게" 네트워크에 대 한 쿼리를 시작 하는 옵션을 필터링:

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

앱에서 받을 쿼리 작업이 성공 하면는 `xim_start_joinable_network_query_completed_state_change` 를 검색할 수 있는 네트워크의 목록. 앱도 계속 해 서 받을 `xim_joinable_network_query_updated_state_change` 추가 있는 네트워크 또는 수동 또는 자동으로 중지 될 때까지 반환 된 목록에 있는 네트워크 발생 하는 모든 변경 합니다. 진행 중인 쿼리를 호출 하 여 수동으로 중지할 수 있습니다 `xim::stop_joinable_network_query()`. 호출할 때 자동으로 중지 될 것 `xim::start_joinable_network_query()` 새 쿼리를 시작 합니다.

앱 수 있는 네트워크의 목록에서 호출 하 여 네트워크에 가입 하려고 `xim::move_to_network_using_joinable_network_information()`. 다음 예제에서는 가입 하려는 가정는 `xim_joinable_network_information` (nullptr 두 번째 매개 변수에 전달) 있도록 암호에서 안전 하지 않은 포인터 'selectedNetwork' 가리키는:

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

플레이어를 호출 하 여 가입 두 개 이상의 팀에서 선언한 xim_team_configuration 사용 하 여 네트워크 쿼리를 사용 하는 경우 `xim::move_to_network_using_joinable_network_information()` 기본 팀 인덱스 값이 0 갖게 됩니다.