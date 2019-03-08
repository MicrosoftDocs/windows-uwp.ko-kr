---
title: XIM (c + +)를 사용 하 여
description: C + +를 사용 하 여 Xbox 통합 멀티 플레이 게임 (XIM)를 사용 하는 방법에 알아봅니다.
ms.date: 04/24/2018
ms.topic: article
keywords: xbox live, xbox, 게임, 하나는 xbox, xbox 통합된 멀티 플레이 게임
ms.localizationpriority: medium
ms.openlocfilehash: 798d1a1bc738cbdc7bb2b3fb34076f0897fc76ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644058"
---
# <a name="using-xim-c"></a>XIM (c + +)를 사용 하 여

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

이것이 XIM의 c + + API를 사용 하 여 간단한 연습입니다. 게임을 통해 XIM 액세스 하려는 개발자에 게 C# 나타납니다 [XIM를 사용 하 여 (C#)](using-xim-cs.md)합니다.

- [XIM (c + +)를 사용 하 여](#using-xim-c)
    - [필수 구성 요소](#prerequisites)
    - [초기화 및 시작](#initialization-and-startup)
    - [비동기 작업 및 상태 변경 내용 처리](#asynchronous-operations-and-processing-state-changes)
    - [XIM player 개체를 처리합니다.](#handling-xim-player-objects)
    - [조인할 친구를 사용 하도록 설정 하 고 초대 하](#enabling-friends-to-join-and-inviting-them)
    - [기본 결혼 정보 회사 연결 및 다른 사용자와 다른 XIM 네트워크로 이동](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [디버깅 목적으로 매 치메이 킹 NAT 규칙을 사용 하지 않도록 설정](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [XIM 네트워크를 유지 하 고 정리](#leaving-a-xim-network-and-cleaning-up)
    - [메시지 보내기 및 받기](#sending-and-receiving-messages)
    - [데이터 경로 품질 평가](#assessing-data-pathway-quality)
    - [플레이어 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.](#sharing-data-using-player-custom-properties)
    - [네트워크 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.](#sharing-data-using-network-custom-properties)
    - [플레이어 당 기술을 사용 하 여 매 치메이 킹](#matchmaking-using-per-player-skill)
    - [플레이어 당 역할을 사용 하 여 매 치메이 킹](#matchmaking-using-per-player-role)
    - [XIM은 플레이어 팀과 함께 작동 하는 방법](#how-xim-works-with-player-teams)
    - [채팅을 사용 하 여 작업](#working-with-chat)
    - [플레이어 음소거](#muting-players)
    - [플레이어 팀을 사용 하 여 채팅 대상 구성](#configuring-chat-targets-using-player-teams)
    - [플레이어 슬롯 ("백필" 매 치메이 킹)의 자동 채우기](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [조인 가능한 네트워크를 쿼리합니다.](#querying-joinable-networks)

## <a name="prerequisites"></a>필수 구성 요소

XIM를 사용 하 여 코딩을 시작 하기 전에 두 가지 필수 조건이 있습니다.

1. 표준 다중 접속 네트워킹 기능을 사용 하 여 앱의 AppXManifest 구성 했어야 합니다 하 고 필요한 트래픽을 XIM에서 사용 하는 패턴 템플릿 선언에 있는 네트워크 매니페스트 부분을 구성 해야 합니다.

    AppXManifest 기능과 네트워크 매니페스트 플랫폼 문서의 각 섹션에 자세히 설명 됩니다. 일반적인 XIM 특정 XML을 붙여 제공 됩니다 [XIM 프로젝트 구성](xim-manifest.md)합니다.

1. 두 가지 응용 프로그램 id 정보를 제공 하도록 해야 합니다.

    * 할당 된 Xbox Live 제목 id입니다.
    * Xbox Live 서비스에 대 한 액세스에 대 한 응용 프로그램을 프로 비전의 일부로 제공 되는 서비스 구성 ID입니다.

    이러한 획득 대 한 자세한 내용은 Microsoft 담당자를 참조 하세요. 이러한 정보를 초기화 하는 동안 사용 됩니다.

주를 포함 하 여 필요한 XIM 컴파일 `XboxIntegratedMultiplayer.h` 헤더입니다. 을 올바르게 연결 하려면 프로젝트가 포함 해야 `XboxIntegratedMultiplayerImpl.h` (공용 미리 컴파일된 헤더는 작고 "인라인"으로 생성 하도록 컴파일러에 쉽게 이러한 스텁 함수 구현 되므로 권장 됨)는 하나 이상의 컴파일 단위에서.

에 설명 된 대로 [XIM 개요](../xbox-integrated-multiplayer.md), XIM 인터페이스 프로젝트를 C +를 사용 하 여 컴파일 중에서 선택할 필요가 없습니다 + CX 기존 c + +와 함께 사용할 수 있습니다. 또한 구현 치명적이 지 않은 오류 예외 없는 프로젝트에서 쉽게 사용할 수 있도록 보고를 원하는 경우 수단으로 예외를 throw 하지 않습니다.

## <a name="initialization-and-startup"></a>초기화 및 시작

먼저, Xbox Live 서비스 구성 ID 문자열 및 제목 ID 번호로 XIM 개체 singleton을 초기화 하 여 라이브러리와 상호 작용 합니다. 또한 Xbox 서비스 API를 사용 하는 경우 편리할 수 있습니다 것에서 검색할 `xbox::services::xbox_live_app_config`합니다. 다음 예제에서는 값에에서 이미 있을 'myServiceConfigurationId' 및 'myTitleId' 변수 각각 가정 합니다.

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

앱 참여를 전달 하는 현재 로컬 장치에서 모든 사용자에 대 한 Xbox 사용자 ID 문자열을 검색 해야 초기화 되 면을 `xim::set_intended_local_xbox_user_ids()` 호출 합니다. 다음 샘플 코드에는 단일 사용자가 재생 하려는 의도 표현 하는 컨트롤러 단추를 눌렀습니다 및 사용자와 연결 된 Xbox 사용자 ID 문자열 'myXuid' 변수에 이미 검색 된 가정 합니다.

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

에 대 한 호출 `xim::set_intended_local_xbox_user_ids()` 즉시 XIM 네트워크에 추가 해야 하는 로컬 사용자와 연결 된 Xbox 사용자 Id를 설정 합니다. 이 Xbox 사용자 Id이 목록을 사용할 모든 향후의 네트워크 작업에 대 한 다른 호출을 통해 목록 변경 될 때까지 `xim::set_intended_local_xbox_user_ids()`입니다.

XIM 네트워크 없음 전혀 아직 있는 경우 첫 번째 단계 시작 하는 프로세스를 가져오기 위해 XIM 네트워크에 이동 하는 것입니다. 사용자가 없으면 특정 XIM 네트워크 염두에서에 비어 있는 네트워크로 이동 하는 것이 좋습니다. 이 사용자의 친구를 "로비"가 올 수 공동 함께 (예: 매 치메이 킹 입력)가 다음 멀티 플레이 작업 선택의 일종으로 네트워크를 연결할 수 있습니다.

다음은 이전에 사용 하 여 추가 된 로컬 사용자만 이동 하는 방법의 예가 `xim::set_intended_local_xbox_user_ids()` 최대 8 명의 총 플레이어를 위한 공간을 사용 하 여 빈 XIM 네트워크:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

에 대 한 호출 `xim::move_to_new_network()` 비동기 이동 개발자에 대 한 정기적으로 처리 해야 하는 상태 변경 이벤트를 사용 하 여 완료 하는 작업을 시작 합니다.

## <a name="asynchronous-operations-and-processing-state-changes"></a>비동기 작업 및 상태 변경 내용 처리

XIM의 핵심은 앱의 일반, 자주 호출 하는 `xim::start_processing_state_changes()` 및 `xim::finish_processing_state_changes()` 메서드 쌍입니다. 이 메서드는 어떻게 XIM 앱 멀티 플레이 상태에 대 한 업데이트를 처리할 준비가 되었음을 알게 되 고 XIM 해당 업데이트를 제공 하는 방법. UI 렌더링 루프에서 그래픽 프레임 마다 호출할 수 있습니다는 신속 하 게 작동 하도록 설계 되었습니다. 이 네트워크 타이밍 지출 하거나 복잡 한 다중 스레드 콜백의 예측 불가능성을 고려 하는 방법에 대 한 걱정 없이 모든 큐에 대기 중인된 변경 내용을 검색 하는 데 편리한 위치를 제공 합니다. XIM API가 실제로이 단일 스레드 패턴에 맞게 최적화 됩니다. 상태는 그대로 유지이 두 함수 외부에서 직접이 고 효율적으로 사용할 수 있도록 보장 합니다.

XIM API에서 반환 된 모든 개체는 동일한 이유로 *되지* 스레드로부터 안전한 것으로 간주 합니다. 라이브러리 내부 다중 스레딩 보호를 갖지만 XIM의 값 중 하나를 액세스 하는 임의의 스레드 필요한 경우 고유한 잠금을 구현 해야 합니다. 하나의 스레드를 설명 하는 경우에 예를 들어, 합니다 `xim::players()` 하나 다른 스레드를 호출할 수 없습니다 하는 동안 나열 `xim::start_processing_state_changes()` 또는 `xim::finish_processing_state_changes()` 및 해당 플레이어 목록과 연결 된 메모리를 변경 합니다.

때 `xim::start_processing_state_changes()` 가 배열을에 모든 큐에 대기 중인된 업데이트는 보고 호출 `xim_state_change` 구조체 포인터입니다.

앱을 수행 해야합니다.

1. 배열 반복 합니다.
1. 보다 구체적인 형식에 대 한 기본 구조를 검사 합니다.
1. 기본 구조 형식을 캐스팅 자세한 형식에 해당 합니다.
1. 업데이트를 적절 하 게 처리 합니다.

모두를 사용 하 여 한 번 완료 합니다 `xim_state_change` 구조를 현재 사용할 수 있는, 해당 배열에 전달 되어야 다시 호출 하 여 리소스를 해제 하도록 XIM `xim::finish_processing_state_changes()`합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

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

기본 처리 루프 했으므로 초기와 연결 된 상태 변경 내용을 처리할 수 있습니다 `xim::move_to_new_network()` 작업 합니다. 사용 하 여 모든 XIM 네트워크 이동 작업이 시작 하는 `xim_move_to_network_starting_state_change`합니다. 어떤 이유로 실패 하면 이동 하는 경우 앱이 제공 됩니다는 `xim_network_exited_state_change`는 처리 메커니즘 XIM 네트워크를 이동 하지 못하도록 하거나 있습니다 현재 XIM 네트워크에서 연결을 끊습니다는 비동기 오류에 대 한 일반적인 오류입니다. 이동와 함께 완료 됩니다이 고, 그렇지는 `xim_move_to_network_succeeded_state_change` 모든 상태 종료 된 후 모든 사람들에 게 XIM 네트워크에 추가 되었습니다.

사용자에 멀티 플레이 세션에서 다른 사용자와 재생을 위한 플랫폼 권한 없는 XIM에서 만들거나 XIM 네트워크에 가입 하려고 하는 장치에 보냅니다는 `xim_network_exited_state_change` 이유인 `xim_network_exit_reason::user_multiplayer_restricted`합니다. 사용 하 여 로컬 사용자 설정 하는 경우에 이런 `xim::set_intended_local_xbox_user_ids()` Xbox Live 멀티 플레이 제한이 있어야 합니다. XIM를 사용 하 여 연대 Xbox 앱 중 하나를 호출 해야 `Store::Product::CheckPrivilegeAsync` attemptResolution 집합과 XIM 네트워크에 true로 이동에 대 한 응답으로 수신 작업을 수행 하기 전에 로컬 플레이어 `xim_network_exit_reason::user_multiplayer_restricted` 반환 된 이유로 `xim_network_exited_state_change`합니다.

## <a name="handling-xim-player-objects"></a>XIM player 개체를 처리합니다.

새 XIM 네트워크에 단일 로컬 사용자를 이동 하는 예제를 가정할 때 성공, 앱 제공 됩니다는 `xim_player_joined_state_change` 로컬 `xim_player` 개체입니다. 플레이어 인스턴스 자체가 유효으로이 개체 포인터에 대 한 유효한 남아 있습니다. 플레이어 인스턴스 무효화 될 때 해당 `xim_player_left_state_change` 는 플레이어에 대 한 인스턴스 반환 되었습니다. 호출을 통해 `xim::finish_processing_state_changes()`합니다. 앱을 항상 제공 되어야 합니다는 `xim_player_left_state_change` 에 대 한 모든 `xim_player_joined_state_change`합니다.

모든 배열을 검색할 수도 있습니다 `xim_player` 는 XIM에서 개체를 사용 하 여 언제 든 지 네트워크 `xim::get_players()`합니다.

합니다 `xim_player` 개체와 같은 여러 유용한 메서드를가지고 `xim_player::gamertag()` 표시 하기 위해 플레이어를 사용 하 여 연결 된 현재 Xbox Live 게이머 태그 문자열을 검색 합니다. 경우는 `xim_player` 가 로컬 장치에도 null이 아닌 보고 `xim_player::xim_local` 개체 포인터를 `xim_player::local()`는 메서드가 추가 로컬 플레이어에만 사용할 수 있습니다.

물론 가장 중요 한 플레이어를 위해 상태가 XIM 알고 있는 공용 정보가 있지만 추적 하려고 하는 특정 앱. 연결 하려는 가능성이 추적 정보에 대 한 고유한 구문이 없으므로 합니다 `xim_player` 언제 든 지 XIM 보고서 있도록 개체를 사용자 고유의 플레이어 구문 개체에는 `xim_player`를 채울 필요가 없는 상태를 신속 하 게 검색할 수 있습니다 및 사용자 지정 플레이어 컨텍스트에 대 한 포인터를 조회 합니다. 다음 예에서는 가정 개인 상태에 대 한 포인터 변수 'myPlayerStateObject' 및 새로 추가한 `xim_player` 개체 'newXimPlayer' 변수에:

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

로컬로 player 개체를 사용 하 여 지정 된 포인터 값을 저장 하는이 (되지 전송할 네트워크를 통해 원격 장치에 메모리를 유효한 것 위치). 항상로 돌아가려면 개체 사용자 지정 컨텍스트를 검색 하 고 다음 예제와 같이 개체도 다시 캐스팅 하 여 수 있습니다.

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

에 할당 된 사용자 지정 플레이어 상황에 맞는 포인터를 변경할 수는 `xim_player` 언제 든 지 합니다.

## <a name="enabling-friends-to-join-and-inviting-them"></a>조인할 친구를 사용 하도록 설정 하 고 초대 하

개인 정보 및 보안에 대 한 새 XIM 네트워크에서 모든 로컬 플레이어가 으로만 조인 되도록 기본적으로 자동으로 구성 됩니다 하 고 준비가 되 면 다른 사용자가 명시적으로 허용 하려면 앱에 달려 있습니다. 다음 예제에서는 사용 하는 방법을 보여 줍니다 `xim::network_configuration()` 현재 네트워크 구성을 가져오고 joinability 사용 하 여 업데이트 `xim::set_network_configuration()` 다른 사용자는 초대 받은 또는 되 뿐만 아니라 새 로컬 사용자가 플레이어로 가입 하도록 허용 하려면 " XIM 네트워크에서 이미 플레이어가 "(Xbox Live 소셜 관계를)를 수행 합니다.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` asynchronoulsy를 실행합니다. 이전 코드 샘플 호출에는 다음이 완료 되 면을 `xim_network_configuration_changed_state_change` joinability 값의 기본값에서 변경 된 앱에 알리기 위해 제공 됩니다 `xim_allowed_player_joins::none`합니다. 확인 하 여 새 값에 대해 쿼리할 수 있습니다 합니다 `allowed_player_joins` 의 속성을 `xim_network_configuration` 반환한 `xim::network_configuration()`합니다. 

`allowed_player_joins` 장치는 네트워크의 joinability 결정할 XIM 네트워크에서 확인할 수 있습니다.

원격 사용자에 게이 XIM 네트워크에 가입 하도록 초대를 보낼 않으려는 로컬 플레이어 중 하나에서 앱을 호출할 수 `xim_player::xim_local::show_invite_ui()` 시스템 초대 UI를 시작 합니다. 여기에서 로컬 사용자 초대를 보냅니다 및 초대 하려는 사용자를 선택할 수 있습니다. 다음 예제에서는 작동 방식을 보여 줍니다. 및 해당 변수의 'ximPlayer'는 올바른 로컬 지점 가정 `xim_player`:

```cpp
ximPlayer->local()->show_invite_ui();
```

위의 문을 실행 한 후 시스템 초대 UI 표시 됩니다. 사용자가 초대를 전송, 그렇지 않으면 UI를 해제 한 후에 `xim_show_invite_ui_completed_state_change` 의 다음 호출에서 제공 됩니다 `xim::start_processing_state_changes()`합니다. 앱에서 직접 사용 하 여 초대를 보낼 수는 또는 `xim_player::xim_local::invite_users()` 시스템 초대 UI 표시 하지 않고 있습니다.

어느 방법이 든 원격 사용자가 메시지를 수신할 Xbox Live 초대 서명한을 수락 하거나 거부할 수도 있는 위치입니다. 요청을 수락 하는 경우에 프로토콜 활성화를 이미 실행 되지 않는 프로토콜 활성화 해당 XIM 네트워크로 이동 하는 이벤트 인수를 사용 하 여 앱에 게 배달 될 경우 해당 장치에서 앱에 시작 됩니다.

자체 프로토콜 활성화에 대 한 자세한 내용은 플랫폼 설명서를 참조 하세요.

다음 예제에서는 호출 하는 방법을 보여 줍니다. `xim::extract_protocol_activation_information()` 이벤트 인수를 XIM 적용할 여부를 결정 합니다. 이 가정에서 원시 URI 문자열을 검색 한 `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` 변수에 'uriString'.

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

XIM 활성화를 경우 채워진된 'local_xbox_user_id' 필드에 식별 된 로컬 사용자를 확인 하려고 `xim_protocol_activation_information` 구조 서명 되 고에 지정 된 사용자 중 `xim::set_intended_local_xbox_user_ids()`합니다. 지정된 된 XIM 네트워크에 대 한 호출으로 이동 한 다음 시작할 수 있습니다 `xim::move_to_network_using_protocol_activated_event_args()` 동일한 URI 문자열을 사용 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

또한 "뒤 에" 원격 사용자는 시스템 UI의에서 로컬 사용자의 플레이어 카드 이동 하 고 (위에 표시 된 대로 이러한 플레이어 조인 허용한 것으로 가정) 초대 없이 자체 조인 시도 시작할 수 있습니다. 이 작업은 프로토콜도 초대 수행 하 고 동일한 방식으로 처리 된 것 처럼 응용 프로그램을 활성화 합니다.

에 표시 된 대로 새 XIM 네트워크 이동 동일 XIM 네트워크 프로토콜 활성화를 사용 하 여 이동할 [초기화 및 시작](#initialization-and-startup)합니다. 유일한 차이점은 이동에 성공 하면 이동 장치를 제공할 것 로컬 및 원격 플레이어 `xim_player_joined_state_change` 모든 적용 가능한 플레이어를 나타내는 구조입니다. XIM 네트워크에 이미 있던 원래 장치를 이동할 수 없습니다 하지만 새 장치의 사용자 추가 통해 플레이어도 추가할 수 참조는 물론 `xim_player_joined_state_change` 구조입니다.

XIM 네트워크에 여러 장치에서 플레이어 함께 조인 되 면 멀티 플레이어 시나리오를 시작 하 고, 음성 및 텍스트를 자동으로 통신할 하 고, 앱 별 메시지를 보낼 수 있습니다 이러한.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>기본 결혼 정보 회사 연결 및 다른 사용자와 다른 XIM 네트워크로 이동

플레이어 면허증 있는 XIM 네트워크로 이동 하 여 친구 들에 대 한 네트워킹 환경을 더 확장할 수 있습니다. 면허증에서 상대 세계 관심 영역이 비슷한 기반 Xbox Live 결혼 정보 회사 연결 서비스를 사용 하 여 함께 작동 하는 경우

XIM 사용 하 여 기본 결혼 정보 회사 연결을 시작 하는 간단한 방법은 호출 하 여 것 `xim::move_to_network_using_matchmaking()` 채워진는 사용 하 여 장치 중 하나에서 `xim_matchmaking_configuration` 구조를 함께 현재 XIM 네트워크에서 플레이어를 수행 합니다.

다음 예제에서는 시작 아니요 팀 전투 자유 일치 하는 총 8 플레이어를 찾을 수 있도록 설정 하는 결혼 정보 회사 연결 구성을 사용 하 여 이동 합니다. 총 8 플레이어 없으면 시스템 수 여전히 일치 2-7 플레이어 함께 합니다. 이 예제에서는 형식 uint64_t,으로 필터링 하는 게임 모드를 나타내는 MYGAMEMODE_DEATHMATCH 라는의 앱 정의 된 상수를 사용 합니다. 매 치메이 킹 XIM의 동일한 값 및 두 번째 매개 변수를 제공 하는 경우 현재 XIM 네트워크에서 모든 사회에 가입 된 플레이어에 따라 제공을 지정 하는 다른 플레이어를 사용 하 여이 네트워크만 찾는다는 것 `xim_players_to_move::bring_existing_social_players` 에 `xim::move_to_network_using_matchmaking()`:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

이 매 치메이 킹 프로세스는 초기값을 제공 하는 데 이전 이동 등 `xim_move_to_network_starting_state_change` 모든 장치에서 및 `xim_move_to_network_succeeded_state_change` 이동이 성공적으로 완료 되 면 합니다. 다른 하나의 XIM 네트워크에서 이동 이므로, 한 가지 차이점은는 이미 기존 `xim_player` 로컬 및 원격 사용자에 대 한 개체를 추가 하 고 새 XIM 네트워크에 함께 이동 하는 모든 플레이어를 위해 유지 됩니다. 플레이어 간에 채팅 및 데이터 통신 작업을 중단 하지 매 치메이 킹 진행 중인 동안 계속 됩니다.

결혼 정보 회사 연결도 호출 하는 결혼 정보 회사 연결 풀의 잠재적인 플레이어 수에 따라 시간이 오래 걸리는 프로세스가 될 수 있습니다 `xim::move_to_network_using_matchmaking()`합니다.

`xim_matchmaking_progress_updated_state_change` 하 고 사용자가 현재 상태를 유지 하려면 작업 전체에서 주기적으로 제공 됩니다. 일치 항목을 찾을 수 추가 플레이어 일반적인 XIM 네트워크에 추가 됩니다 `xim_player_joined_state_change` 이동이 완료 하 고 있습니다.

이 집합이 "matchmade" 플레이어를 사용 하 여 멀티 플레이어 환경을 완료 했으면, 또 다른 결혼 정보 회사 연결을 사용 하 여 다른 XIM 네트워크로 이동 하는 프로세스를 반복할 수 있습니다. 이전을 통해 조인 하는 각 플레이어 볼 수 `xim::move_to_network_using_matchmaking()` 제공 하는 작업을 `xim_player_left_state_change` 나타내기 위해 해당 `xim_player` 개체는 더 이상 동일한 XIM 네트워크.

경우 하도록 선택 하면 매 치메이 킹을 사용 하 여 다시 시작 `xim::move_to_network_using_matchmaking()`를 지정할 `xim_players_to_move::bring_existing_social_players`, 소셜 진입점을 통해 조인 하는 플레이어 (`xim::move_to_network_using_protocol_activated_event_args()` 또는 `xim::move_to_network_using_joinable_xbox_user_id()`) 새 매 치메이 킹 발생 되는 동안 유지 됩니다. 또는 지정 `xim_players_to_move::bring_only_local_players` 유지 하려면 로컬 플레이어 방금 종료 사회 연결 된 원격 플레이어의 연결이 끊어집니다. 두 경우 모두, 두 번째 이동 작업이 완료 되 면 다양 한 완전히 모르는 사람과 추가 됩니다.

또한 선택할 수 있습니다 아닌 matchmade 플레이어 (또는 단순한 로컬 플레이어)를 이동 하려면 완전히 새로운 XIM 네트워크에 다음 매 치메이 킹 구성/멀티 플레이 게임 활동을 결정 하는 동안. 다음 예제에서는 장치를 호출 하는 방법을 보여 줍니다. `xim::move_to_new_network()` XIM 네트워크는 최대 8 플레이어 하지만이 이번에는 기존 사회에 가입 된 플레이어도 수행 합니다.

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

`xim_move_to_network_starting_state_change` 하 고 `xim_move_to_network_succeeded_state_change` 와 함께 모든 참여 하는 장치에 제공 됩니다는 `xim_player_left_state_change` 남아 있는 matchmade 플레이어에 대 한 합니다. 해당 장치에서 비슷하게 나타납니다는 `xim_player_left_state_change` 네트워크 외부로 이동 되는 각 플레이어에 대 한 합니다.

위에 설명 된 방식으로 원하는 횟수 만큼 XIM 네트워크 XIM 네트워크에서 이동 작업을 계속할 수 있습니다.

Xbox Live 서비스 성능에 대 한 그룹의 모든 직접 피어-투-피어 연결을 설정할 수 있으려면 가능성이 있는 장치에서 플레이어와 일치 하도록 시도 하지 않습니다. 결혼 정보 회사 연결 작업을 사용 하 여 새 네트워크로 있다고 확신 하는 경우에 성공적으로 일치 하지 않고 무기한 계속 될 수 있습니다를 표준 Xbox Live 멀티 플레이 지원 하도록 제대로 구성 된 네트워크 환경에서 개발 하는 경우 동일한 로컬 환경에서 모든 이동 및 모든 장치는 조건을 만족 하는 매 치메이 킹 충분 한 플레이어입니다. 영역/Xbox 응용 프로그램 네트워크 설정에 멀티 플레이 연결 테스트를 실행 하 고 특히 "Strict NAT"에 대 한 문제를 보고 하는 경우 해당 권장 구성을 수행 해야 합니다.

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>디버깅 목적으로 매 치메이 킹 NAT 규칙을 사용 하지 않도록 설정

네트워크 관리자를 XIM의 NAT 규칙을 지 원하는 데 필요한 환경을 변경할 수 없는 경우 차단을 해제할 수 있습니다 XIM 일치 하는 하나 이상의 "오픈 없이" Strict NAT "장치를 허용 하도록 구성 하 여 Xbox One 개발 키트에서 테스트 하 여 매 치메이 킹 NAT"장치입니다. 이렇게 "xim_disable_matchmaking_nat_rule" (내용을 중요 하지 않습니다.) 라는 파일을 배치 하 여 모든 Xbox One 콘솔에서 "제목 처음 부터" 드라이브의 루트에 있습니다. 작업을 수행 하는 한 가지 예제 방법은 다음과 같습니다. "{console_name_or_ip_address}" 자리 표시자를 적절 하 게 각 콘솔에 대 한 대체 앱을 시작 하기 전에 XDK 명령 프롬프트에서 다음을 실행 하 여

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> 이 개발 해결 방법은 현재 고만 사용할 수 Xbox One 배타 리소스 응용 프로그램에 대 한 유니버설 Windows 응용 프로그램에 대 한 지원 되지 않습니다. 또한이 파일이 네트워크 환경에 관계 없이 존재할 수도 없는 장치를 사용 하 여이 설정을 사용 하는 콘솔와 일치 하지 것입니다는 따라서 해야 추가 하 고 어디에서 나 파일을 제거 합니다.

## <a name="leaving-a-xim-network-and-cleaning-up"></a>XIM 네트워크를 유지 하 고 정리

로컬 사용자가 완료 될 때 사용자를 초대 XIM 네트워크에서는 종종 이러한 로컬 사용자에 게 허용 하는 새 XIM 네트워크로 이동 됩니다 참여 하 고 "다음" 사용자가 다음 작업을 찾는 친구 들을 사용 하 여 조정 계속 사용할 수 있도록 가입 하도록 합니다. 사용자가 모든 멀티 플레이어 환경을 사용 하 여 완전히 이루어집니다 경우 앱을 XIM 종료를 시작 하려고 하지만 완전히 네트워크 돌아가서 상태 처럼 `xim::initialize()` 및 `xim::set_intended_local_xbox_user_ids()` 호출한 되었습니다. 이렇게 사용 하 여 `xim::leave_network()` 메서드:

```cpp
xim::singleton_instance().leave_network();
```

이 메서드는 비동기적으로 연결을 끊는 중 다른 참가자 로부터 정상적으로 프로세스를 시작 합니다. 이렇게 하면 제공 원격 장치를 `xim_player_left_state_change` 에 연결 되지 않은 각 로컬 플레이어에 대 한 합니다. 로컬 장치 수도 제공 됩니다는 `xim_player_left_state_change` 연결이 끊겼습니다. 각 로컬 및 원격 플레이어에 대 한 합니다. 모든 연결을 끊을 때 작업이 완료 되 면 최종 `xim_network_exited_state_change` 제공 됩니다. 앱을 호출할 수 있습니다 ' xim::cleanup() '를 모든 리소스를 해제 하 고 초기화 되지 않은 상태로 돌아갑니다.

```cpp
xim::singleton_instance().cleanup();
```

호출 `xim::leave_network()` 기다리는 `xim_network_exited_state_change` XIM를 종료 하기 위해 네트워크 정상적으로 항상 좋습니다 경우는 `xim_network_exited_state_change` 이미 지정 되지 않았습니다.

하는 경우 `xim::cleanup()` 없이 직접 라고 `xim::leave_network()` 호출 되 고 나머지 참가자에 대 한 통신 성능 문제가 발생 하 여 단순히 "사라졌습니다" 하는 장치에 배달할 수 없는 메시지를 전송할 정해 집니다 하므로 호출 `xim::cleanup()`합니다.

## <a name="sending-and-receiving-messages"></a>메시지 보내기 및 받기

XIM 및 해당 기본 구성 요소 연결 문제 또는 전부는 아니지만 일부 플레이어 도달할 수에 대해 걱정할 필요가 있으므로 인터넷을 통해 보안 통신 채널을 설정 하는 모든 지루한 작업을 수행 합니다. 기본 피어-투-피어 연결 문제가 없으면 XIM 네트워크 이동에 실패 합니다.

XIM 네트워크 이며 형성 된 승인을 `xim_move_to_network_succeeded_state_change`, 모든 조인 된 장치에서 앱의 모든 인스턴스가 항상 알려 주어 야 하는 것은 모든 `xim_player` XIM 네트워크에 연결 하 고 그 중 하나를 메시지를 보낼 수 있습니다.

XIM 네트워크를 통해 메시지를 전송 하는 방법을 살펴보겠습니다. 다음 예제에서는 'sendingPlayer' 변수가 유효한 로컬 player 개체에 대 한 포인터를 가정 합니다. 'sendingPlayer' 호출 `local()` 호출 `send_data_to_other_players()` 보낼 메시지 구조 'msgData' 보장 되 고 순차적 배달을 통해 모든 플레이어 (로컬 및 원격 모두). 특정 플레이어의 목록이 메서드로 전달 되지 않았습니다 때문에 모든 플레이어 이동이 note 하십시오.

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

모든 플레이어를 사용 하 여 메시지를 배달할 수 `send_data_to_other_players()` 특정 플레이어 배열을 메서드에 전달 하지 않고 있습니다.

메시지의 모든 받는 사람에 게 제공 되는 `xim_player_to_player_data_received_state_change` 포함 하는 데이터의 복사본에 대 한 포인터 뿐만 해당 xim_player에 대 한 포인터를 신뢰 개체를 수신할 로컬로 됩니다.

물론 배달이 보장 되 고 순차 편리 하지만 XIM 재전송 또는 패킷 삭제 하거나 인터넷에서 disordered 경우 메시지를 연기 해야 하므로 비효율적인 송신 형식이 될 수도 있습니다. 다른 송신 형식을 사용 하 여 앱을 손실 하거나 허용할 수 있도록 하는 메시지에 대해 고려해 야 된 순서로 도착 합니다. 원격 컴퓨터에서 메시지 데이터 상태가 되 면 되므로 특정 바이트 순서 ("endian")에서 멀티 바이트 값을 압축 하는 등의 데이터 형식으로 명확 하 게 정의 하는 것이 좋습니다. 앱은 해당 작업을 수행 하기 전에 데이터를 확인할 수도 있습니다.

XIM은 암호화 또는 서명 구성표를 추가 하지 않아도 되므로 네트워크 수준 보안을 제공 합니다. 그러나 좋습니다 항상 서로 다른 버전의 응용 프로그램 프로토콜 동시 정상적으로 (하는 동안 개발, 콘텐츠 업데이트 등)를 처리할 수 및 실수로 인 한 응용 프로그램 버그를 방지 하기 위해 "심층"를 사용 하 여 합니다. 사용자가 인터넷 연결 리소스를 제한 하 고 끊임없이 변화를 수 있습니다. 가장 효율적인 메시지 데이터 형식 및 모든 UI 프레임을 전송 하는 디자인 하지 않고 사용을 좋습니다.

## <a name="assessing-data-pathway-quality"></a>데이터 경로 품질 평가

현재 품질 두 플레이어 간에 데이터 경로에 대 한 자세한 내용은 호출을 `xim_player::network_path_information()` 메서드. 다음 예제에 대 한 포인터를 검색 합니다 `xim_network_path_information` 의 구조는 `xim_player` 'remotePlayer' 변수에 포함 된 포인터:

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

반환 된 구조에는 예상된 왕복 대기 시간 및 메시지 수는 여전히 로컬 큐에 대기 경우에서 전송에 대 한 순간에 대 한 더 많은 데이터를 전송 연결을 지원할 수 없는 포함 됩니다.

큐가 'xim_player' 특정 백업에 표시 하는 경우는 데이터를 전송 하는 속도 줄여야 합니다.

`xim_network_path_information::round_trip_latency_in_milliseconds` 필드 큐 없이 기본 네트워크의 대기 시간 및 XIM의 예상된 대기 시간을 나타냅니다. 효과적인 대기 시간이 증가 하면 `xim_network_path_information::send_queue_size_in_messages` 증가 XIM 큐를 통해 작동 합니다.

에 대 한 호출을 제한 하려면 적절 한 지점을 선택 `send_data_to_other_players` 게임의 사용량 및 요구 사항을 기준으로 합니다. Send queue에 있는 모든 메시지는 유효 네트워크 대기 시간에서 증가 나타냅니다.

XIM의 최대 제한 (현재 3500 메시지)에 가까운 대부분의 게임에 대해 너무 높기 및 가능성이 호출 하는 속도 따라 전송 대기 중인 데이터의 몇 초를 나타내는 `send_data_to_other_players` 및 각 데이터 페이로드는 최대 크기입니다. 대신 함께 어떻게 흔들림 게임의 게임의 대기 시간 요구 사항을 고려 하는 숫자를 선택할 `send_data_to_other_players` 호출 패턴은입니다.

## <a name="sharing-data-using-player-custom-properties"></a>플레이어 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.

대부분의 응용 프로그램 데이터 교환에서 발생 하는 `xim_player::xim_local::send_data_to_other_players()` 메서드 있기 때문에 데이터를 받는 사람, 해당 데이터를 받을 해야 경우 및 패킷 손실 될 수 있는 시스템 처리 하는 방법에 대해 가장 높은 제어 합니다. 그러나 경우 좋을 자신에 대 한 기본, 거의 변경 상태를 간단 하 게 사용 하 여 다른 사용자와 공유 하는 플레이어에 대 한 경우가 있습니다. 예를 들어, 각 플레이어에는 모든 플레이어 게임 내 표현으로 렌더링 하는 데 사용할 수 있는 멀티 플레이어를 입력 하기 전에 선택한 문자 모델을 나타내는 고정된 문자열이 있을 수 있습니다.

플레이어에 대 한 자주 변경 되지 않는 데이터를 사용자 지정 속성 XIM 플레이어를 제공 합니다. 이러한 속성은 null로 끝나는 문자열 쌍 로컬 플레이어에 게 적용할 수 있는 고는 변경 될 때마다 자동으로 모든 장치에 전파 가져오기는 앱 정의 이름 및 값으로 구성 됩니다.

사용자 지정 플레이어 속성에는 오버 헤드 보다 자세한 내부 동기화는 `xim_player::xim_local::send_data_to_other_players()` 메서드 되므로 데이터 (즉, 플레이어 위치)를 신속 하 게 변경 하는 경우에 직접 보냅니다를 사용 해야 합니다.

플레이어 사용자 지정 속성 키-값 쌍의 현재 값은 이러한 장치 XIM 네트워크에 가입 하 고 추가 플레이어를 참조 하는 경우 새 참여 하는 장치에 자동으로 제공 하 게 합니다. 호출 하 여 값을 설정할 수 있습니다 `xim_player::xim_local::set_player_custom_property()` 이름 및 값 문자열입니다. 다음 예제에서는 이름이 "model" 로컬 "무차별 암호 대입" 값이 있어야 하는 속성 설정 `xim_player` 'localPlayer' 변수가 가리키는 개체:

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

Player 속성을 변경 하면는 `xim_player_custom_properties_changed_state_change` 변경 된 속성 이름에 경고 하는 모든 장치에 제공 해야 합니다. 지정 된 이름에 대 한 값을 사용 하 여 플레이어, 로컬 또는 원격에서 검색할 수 있습니다 `xim_player::get_player_custom_property()`합니다. 다음 예제에서 "model" 라는 속성 값을 검색 한 `xim_player` 'ximPlayer' 변수가 가리키는:

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

지정된 된 속성 이름에 대 한 새 값을 설정 하면 기존 값을 바뀝니다. Null 값 문자열 포인터를 값으로 설정에 속성 이름을 처리 됩니다 것을 지정 하지 않았습니다 아직 속성과 동일한 빈 값 문자열을 동일 합니다. 이름 및 값 XIM에서 해석 되지 않습니다, 그리고 필요에 따라 문자열 내용을 유효성을 검사 하려면 앱에서 그대로 따라서 합니다.

사용자 지정 플레이어 속성 하나 XIM 네트워크에서 다른 컴퓨터로 이동 하는 경우 항상 다시 설정 됩니다. 그러나 XIM 네트워크를 조인 하는 새 플레이어는 일부 집합에 있는 모든 플레이어를 위해 현재 플레이어 사용자 지정 속성을 받게 됩니다.

## <a name="sharing-data-using-network-custom-properties"></a>네트워크 사용자 지정 속성을 사용 하 여 데이터를 공유 합니다.

네트워크 사용자 지정 속성의 편의 자주 변경 되지 않는 XIM 네트워크 관련 데이터를 동기화 할 것입니다. 사용자 지정 속성의 작동을 동일 하 게 네트워크 [플레이어 사용자 지정 속성](#sharing-data-using-player-custom-properties)사용 하 여 XIM 단일 개체에 설정 하는 점을 제외 하 고, `xim::set_network_custom_property()`합니다. 다음 예제에서는 "맵" 속성을 "요새" 값을 설정 합니다.

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

네트워크 속성을 변경 하면는 `xim_network_custom_properties_changed_state_change` 변경 된 속성 이름에 경고 하는 모든 장치에 제공 해야 합니다. 지정 된 이름에 대 한 값을 사용 하 여 검색할 수 `xim::get_network_custom_property()`속성 명명 된 "지도"에 대 한 값을 검색 하는 다음 예제에서 처럼,:

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

마찬가지로 [플레이어 사용자 지정 속성](#sharing-data-using-player-custom-properties), 지정된 된 속성 이름에는 기존 값 대체할 새 값을 설정 합니다. Null 값 문자열 포인터를 값으로 설정에 속성 이름을 처리 됩니다 것을 지정 하지 않았습니다 아직 속성과 동일한 빈 값 문자열을 동일 합니다. 이름 및 값 XIM에서 해석 되지 않습니다, 그리고 필요에 따라 문자열 내용을 유효성을 검사 하려면 앱에서 그대로 따라서 합니다.

새로 만든 XIM 네트워크는 항상 설정 된 사용자 지정 속성이 없는 네트워크를 사용 하 여 시작 합니다. 그러나 기존 XIM 네트워크를 조인 하는 새 플레이어는 해당 XIM 네트워크에 설정 하는 네트워크 사용자 지정 속성의 현재 값을 받게 됩니다.

## <a name="matchmaking-using-per-player-skill"></a>플레이어 당 기술을 사용 하 여 매 치메이 킹

특정 앱 지정 게임 모드에서 공통의 관심사를 갖고에서 플레이어를 일치 하는 좋은 기본 전략입니다. 또한 플레이어 숙련 된 플레이어 최신 플레이어 수 단위로 증가 하는 동안 다른 veterans 정상 경쟁 하는 문제를 즐길 수 있도록 개인 기술 또는 게임을 사용 하 여 환경에 따라 일치 하는 고려해 야 사용할 수 있는 플레이어 풀 증가 함에 따라 유사한 기능을 사용 하 여 다른 사용자에 대해 경쟁 합니다.

이 작업을 수행 하려면 해당 플레이어 당 역할 및 기술 구성 구조에 대 한 호출에 지정 된 모든 로컬 플레이어를 위해 기술 수준을 제공 하 여 시작 `xim_player::xim_local::set_roles_and_skill_configuration()` 결혼 정보 회사 연결을 사용 하 여 네트워크를 XIM 이동할 시작 하기 전에 합니다. 기술 수준에는 앱 별 개념 및 수 있다는 점을 제외 하면 매 치메이 킹은 동일한 기술 값을 가진 플레이어를 찾으려면 먼저 다음 기술을 선언 하는 다른 플레이어를 찾으려고 시도를 10 + /-의 단위로 검색을 주기적으로 확장 XIM에서 해석 되지 않습니다. 해당 기술에 관련 된 범위 내의 값입니다. 다음 예에서는 가정 로컬 `xim_player` 개체를 해당 포인터는 'localPlayer', 로컬 또는 Xbox Live 저장소에서 검색 한 'playerSkillValue' 라는 변수를 연결 된 앱 별 uint32_t 기술 값은:

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

이 완료 되 면 모든 참가자가 제공 됩니다는 `xim_player_roles_and_skill_configuration_changed_state_change` 이 나타내는 `xim_player` 해당 플레이어 당 역할 및 기술 구성 변경 되었습니다. 호출 하 여 새 값을 검색할 수 있습니다 `xim_player::roles_and_skill_configuration()`합니다. 모든 플레이어가 null이 아닌 역할 및 기술 구성 적용 하는 경우에 매 치메이 킹을 사용 하 여 값이 true 인 XIM 네트워크를 이동할 수 있습니다는 `require_player_roles_and_skill_configuration` 필드를 `xim_matchmaking_configuration` 에 지정 된 구조의 `xim::move_to_network_using_matchmaking()`합니다. 다음 예제에서는 다른 플레이어와 MYGAMEMODE_DEATHMATCH만 일치 하는 값으로 정의 하는 앱 별 게임 모드 상수 uint64_t를 사용 하 여 아니요 팀 전투 자유에 대 한 총 2 ~ 8 플레이어를 찾을 수 있는 결혼 정보 회사 연결 구성 동일한 값 및는 플레이어 당 역할 및 기술 구성 필요 함을 지정 합니다.

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

이 구조에 제공 되는 경우 `xim::move_to_network_using_matchmaking()`, 이동 작업은 시작 일반적으로 이동 하는 플레이어 호출한 `xim_player::xim_local::set_roles_and_skill_configuration()` null이 아닌을 사용 하 여 `xim_player_roles_and_skill_configuration` 포인터입니다. 모든 플레이어 않은 하 고 매 치메이 킹 프로세스를 일시 중지 됩니다. 모든 참가자가 제공 됩니다 하는 경우는 `xim_matchmaking_progress_updated_state_change` 사용 하 여는 `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` 값입니다. 이후에 다른 수단을 통해 이전에 보낸된 초대를 통하거나 XIM 네트워크를 조인 하는 플레이어 여기에 (예를 들어 호출 `xim::move_to_network_using_joinable_xbox_user_id()`) 매 치메이 킹 완료 되기 전에 합니다. 모든 플레이어를 제공한 후 해당 `xim_player_roles_and_skill_configuration` 구조 매 치메이 킹 다시 시작 됩니다.

다음 섹션에 설명 된 대로 매 치메이 킹 플레이어 당 기술을 사용 하 여 매 치메이 킹 사용자 당 플레이어 역할와 결합할 수도 있습니다. 하나는 원하는 경우 다른 값이 0 지정할 수 있습니다. 선언 하는 모든 플레이어 있기 때문에 이것이 `xim_player_roles_and_skill_configuration` 기술 값 0은 항상 일치 서로 합니다.

한 번를 `xim::move_to_network_using_matchmaking()` 또는 다른 XIM 네트워크 작업이 완료 된 모든 플레이어의 이동 `xim_player_roles_and_skill_configuration` 구조는 null 포인터에 대 한 자동으로 해제 됩니다 (관련 된 `xim_player_roles_and_skill_configuration_changed_state_change` 알림). 호출 해야 플레이어 당 구성이 필요로 하는 결혼 정보 회사 연결을 사용 하 여 다른 XIM 네트워크로 이동 하려는 경우 `xim_player::xim_local::set_roles_and_skill_configuration()` 최신 정보를 포함 하는 새 구조 포인터를 사용 하 여 다시 합니다.

## <a name="matchmaking-using-per-player-role"></a>플레이어 당 역할을 사용 하 여 매 치메이 킹

다른 방법은 플레이어 당 역할 및 기술 구성을 사용 하 여 사용자의 결혼 정보 회사 연결 환경을 개선 하기 위해 필요한 플레이어 역할 사용 하 여 것입니다. 이 다른 협조적 재생 스타일;를 선택할 수 있는 문자 형식을 제공 하는 게임에 적합 즉, 게임 그래픽을 변경 하기만 하면 됩니다 하지는 않습니다 하지만 방어 "치료"의 "근거리" 공격 및 멀리 떨어진 "범위"와 같은 보완, 영향력 있는 특성을 제어 하는 형식 공격 지원 합니다. 사용자의 개성 특정 특수화도 재생 하기를 원하는 수를 의미 합니다. 게임 있지 기능적으로 각 역할을 수행 하는 하나 이상의 사용자 없이 목표를 완료할 수 있도록 설계 된, 경우 때로는 것이 좋습니다에 일치 모든 플레이어 함께 다음 보다 먼저 협상 해야 이러한 플레이어에 맞게 하지만 수집 된 후 사이에서 스타일을 재생 합니다. 첫 번째 지정 된 플레이어의에 지정할 각 역할을 나타내는 고유 비트 플래그를 정의 하 여 이렇게 `xim_player_roles_and_skill_configuration` 구조입니다.

다음 예제에서는 'localPlayer' 포인터가 로컬 xim_player 개체에 대 한 앱 별 MYROLEBITFLAG_HEALER uint8_t 역할 값을 설정 합니다.

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

이 완료 되 면 모든 참가자가 제공 됩니다는 `xim_player_roles_and_skill_configuration_changed_state_change` 이 나타내는 `xim_player` 의 플레이어 당 역할 구성이 변경 되었습니다. 호출 하 여 새 값을 검색할 수 있습니다 `xim_player::roles_and_skill_configuration()`합니다.

전역 `xim_matchmaking_configuration` 에 지정 된 구조의 `xim::move_to_network_using_matchmaking()` 해야 필요한 역할 플래그를 모두 결합 한 비트 OR를 사용 하 여 및 require_player_matchmaking_configuration 필드에 대해 true 값입니다.

다음 예제는 없는 팀 전투 자유에 대 한 총 3 플레이어를 찾을 수 있는 매 치메이 킹 구성을 채웁니다. 또한이 예제에서는 형식 uint64_t는 고 MYGAMEMODE_COOPERATIVE로 필터링 하는 게임 모드를 나타내는 이름은 앱 정의 상수를 사용 합니다. 플레이어 당 역할 및 기술 구성 플레이어 하나 이상의 비트 OR는 각 앱 별 uint8_t 역할을 충족 하는 위치 (MYROLEBITFLAG_HEALER, MYROLEBITFLAG_ 구성 되 고 함께 필요 하도록 구성 설정 또한 근거리, 및 MYROLEBITFLAG_RANGE):

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

이 구조에 제공 되는 경우 `xim::move_to_network_using_matchmaking()`, 위에서 설명한 이동 작업이 시작 됩니다.

또한 매 치메이 킹 사용자 당 플레이어 기술을 사용 하 여 플레이어 당 역할을 사용 하 여 매 치메이 킹을 결합할 수 있습니다. 하나는 원하는 경우 다른 값이 0 지정 합니다. 선언 하는 모든 플레이어 있기 때문에 이것이 `xim_player_roles_and_skill_configuration` 기술 값 0은 항상 일치; 서로 및 모든 비트가 0 이면는 `xim_matchmaking_configuration` 없는 역할 비트와 일치 하는 데 필요한 다음 required_roles 필드입니다.

한 번를 `xim::move_to_network_using_matchmaking()` 또는 다른 XIM 네트워크 작업이 완료 된 모든 플레이어의 이동 `xim_player_roles_and_skill_configuration` 구조는 null 포인터에 대 한 자동으로 해제 됩니다 (관련 된 `xim_player_roles_and_skill_configuration_changed_state_change` 알림). 호출 해야 플레이어 당 구성이 필요로 하는 결혼 정보 회사 연결을 사용 하 여 다른 XIM 네트워크로 이동 하려는 경우 `xim_player::xim_local::set_roles_and_skill_configuration()` 최신 정보를 포함 하는 새 구조 포인터를 사용 하 여 다시 합니다.

## <a name="how-xim-works-with-player-teams"></a>XIM은 플레이어 팀과 함께 작동 하는 방법

다중 접속 종종 게임 반대 팀으로 구성 하는 플레이어를 포함 합니다. XIM는 때를 팀에 할당 하려면 쉽게 설정 하 여 매 치메이 킹 `xim_team_configuration`합니다. 다음 예제에서는 시작 (하지만 4 발견 되지 않은 경우 1 ~ 3 플레이어는도 허용 됨) 4 개 같은 팀에 배치할를 8 플레이어의 합계를 찾도록 구성 하는 결혼 정보 회사 연결을 사용 하 여 이동, 값으로 정의 하는 앱 별 게임 모드 상수 uint64_t를 사용 하 여 동일한 값을 지정 하 고 현재 XIM 네트워크에서 모든 사회에 가입 된 플레이어를 가져오는 다른 플레이어와만 일치 하는 MYGAMEMODE_CAPTURETHEFLAG:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

이러한 XIM 네트워크 이동 작업이 완료 되 면 팀 인덱스 값 1 {n} 통해 {n} 팀 요청에 해당 플레이어 할당 됩니다. 특정 팀 인덱스 값의 의미를 true 앱에 게 달려 있습니다. 플레이어의 팀 인덱스 값을 통해 검색 `xim_player::team_index()`합니다. 사용 하는 경우는 `xim_team_configuration` 둘 이상의 팀을 사용 하 여 플레이어는 절대 할당 해서는 안 팀 인덱스 값이 0 이면 호출에서 `xim::move_to_network_using_matchmaking()`합니다. 플레이어 XIM 네트워크 구성 또는 이동 작업의 형식에 추가 되는 반면입니다 (같은 프로토콜 활성화 초대를 수락에서 결과 통해), 팀 인덱스를 항상 수 있는 사용자를 합니다. 특별 한 "할당 되지 않은" 팀으로 팀 인덱스 0을 처리 하는 데 도움이 수 있습니다.

다음 예에서는 인덱스를 검색 합니다 팀 'ximPlayer' 변수에 있는 포인터가 xim_player 개체:

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

(필요가, 음수 플레이어 행동에 대 한 멘 션 감소 기회) 기본 사용자 환경을 위해 Xbox Live 결혼 정보 회사 연결 서비스는 다른 팀으로 함께 XIM 네트워크를 이동 하는 분할 플레이어 되지 합니다.

결혼 정보 회사 연결 하 여 처음에 할당 된 팀 인덱스 값만 권장 되며 앱 로컬 플레이어가 언제 든 지 변경할 수 `xim_player::xim_local::set_team_index()`입니다. 이 매 치메이 킹을 전혀 사용 하지 않는 XIM 네트워크에서 호출할 수도 있습니다. 다음 예제에서는 플레이어 포인터를 'localPlayer' 하나의 인덱스 값이 새 팀을 구성 합니다.

```cpp
localPlayer->local()->set_team_index(1);
```

모든 장치는 플레이어에 새 팀 인덱스 값을 적용 하 고 제공 하는 경우 알림을 받지는 `xim_player_team_index_changed_state_change` 는 플레이어에 대 한 합니다. 호출할 수 있습니다 `xim_player::xim_local::set_team_index()` 언제 든 지 합니다.

매 치메이 킹 팀에서 독립적으로 필수 플레이어 역할을 평가합니다. 따라서 충족 된 플레이어 역할이 아닌 팀 플레이어 수에 따라 분산 하므로 팀 및 필요한 역할 둘 다 동시 매 치메이 킹 구성 조건으로 사용할 권장 되지 했습니다.

## <a name="working-with-chat"></a>채팅을 사용 하 여 작업

음성 및 텍스트 채팅 통신 XIM 네트워크에서 플레이어 간에 자동으로 활성화 됩니다. XIM를 모든 음성 헤드셋 및 마이크 하드웨어와 상호 작용을 처리 합니다. 앱 채팅을 위한 많은 작업을 수행 할 필요가 없지만 해당 텍스트 채팅에 대 한 요구 사항 중 하나: 입력 및 표시를 지원 합니다. 텍스트 입력 이므로 필요한 플랫폼, 지금까지 사용 하 여 광범위 한 실제 키보드를 보유 하지 않은 게임 장르 에서도 플레이어 텍스트 음성 변환 보조 기술을 사용 하 여 시스템 구성할 수 있습니다. 마찬가지로, 텍스트 표시 되므로 필요한 플레이어 음성-텍스트를 사용 하도록 시스템을 구성할 수 있습니다.

호출 하 여 이러한 기본 설정은 로컬 플레이어에서 검색할 수 있습니다 `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` 고 `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()`, 및 조건에 따라 텍스트 메커니즘을 사용 하도록 설정 하려고 할 수 있습니다. 그러나 텍스트 입력 및 표시 항상 사용할 수 있는 옵션을 고려 하는 것이 좋습니다.

 `Windows::Xbox::UI::Accessability` Xbox One 클래스를 하도록 설계 된 음성-텍스트 보조 기술에 중점을 둔 텍스트 게임 내 채팅의 간단한 렌더링을 제공 합니다.

실제 또는 가상 키보드에서 제공 하는 텍스트 입력을 만든 후에 문자열을 전달 합니다 `xim_player::xim_local::send_chat_text()` 메서드. 다음 코드에서는 로컬에서 예제에서는 하드 코드 된 문자열을 보낼 `xim_player` 'localPlayer' 변수가 가리키는 개체:

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

이 채팅 텍스트는 원래 로컬 플레이어에서 채팅 통신을 수신할 수 있는 XIM 네트워크의 모든 플레이어에 전달 됩니다. 음성 오디오를 합성 될 수 있습니다 및으로 제공 될 수 있습니다는 `xim_chat_text_received_state_change`합니다.

앱 받은 모든 텍스트 문자열의 복사본을 확인 하 고 시간 (또는 스크롤 가능한 창에서) 적절 한 용량에 대 한 원래 플레이어의 일부 id와 함께 표시 해야 합니다.

채팅에 대 한 몇 가지 모범 사례가 있습니다. 어디서 나 플레이어에에서 표시 되어 있는지, 특히는 스코어 같은 게이머 태그의 목록을 사용자에 대 한 피드백으로 말하기 음소거 아이콘도 표시 하는 것이 좋습니다. 호출 하 여 이렇게 `xim_player::chat_indicator()` 검색할는 `xim_player_chat_indicator` 는 플레이어에 대 한 채팅의 즉각적인, 현재 상태를 나타내는입니다. 다음 예제에 대 한 표시기 값을 검색 하는 방법을 보여 줍니다는 `xim_player` 'iconToShow' 변수에 값을 할당할 특정 아이콘 상수 값을 확인 하려면 ' ximPlayer' 변수가 가리키는 개체:

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

보고 된 값 `xim_player::chat_indicator()` 예를 들어 플레이어 시작 및 중지 통신으로 자주 변경 해야 합니다. 폴링 해당 UI 프레임 마다 결과적으로 앱을 지원 하도록 설계 되었습니다.

> [!NOTE]
> 로컬 사용자가 장치 설정으로 인해 통신 권한이 없는 경우 `xim_player::chat_indicator()` 돌아갑니다 `xim_player_chat_indicator::platform_restricted`합니다. 플랫폼에 대 한 요구 사항을 충족 하도록 예상 되는 앱의도 해 음성 채팅 또는 메시징 및 메시지 문제를 나타내는 사용자를 위한 플랫폼 제한을 나타내는 보여 줍니다. 권장 되는 하나의 예제 메시지는: "죄송 합니다. 사용자는 허용 되지 않습니다 지금 채팅."

## <a name="muting-players"></a>플레이어 음소거

또 다른 최선의 음소거 플레이어를 지원 하기 위해서입니다. 시스템 수행자 카드를 통해 사용자가 시작한 음소거 XIM 자동으로 처리 하지만 게임 별 일시적인 음소거 통해 게임 UI 내에서 수행할 수 있는 앱을 지원 해야 합니다 `xim_player::set_chat_muted()` 메서드. 다음 예에서는 원격 음소거 시작 `xim_player` 가리키는 개체 변수의 'remotePlayer' 없음 음성 채팅 들립니다 없는 텍스트 채팅에서 받은 있도록:

```cpp
remotePlayer->set_chat_muted(true);
```

음소거 즉시 적용 되며 없습니다 `xim_state_change` 연관 됩니다. 호출 하 여 실행 취소할 수 `xim_player::set_chat_muted()` false 값으로 다시 합니다. 다음 예에서는 원격 unmutes `xim_player` 'remotePlayer' 변수가 가리키는 개체:

```cpp
remotePlayer->set_chat_muted(false);
```

음을 소거에 계속 적용으로 `xim_player` 플레이어를 사용 하 여 새 XIM 네트워크를 이동 하는 경우를 포함 하 여 존재 합니다. 플레이어를 퇴사 하는 경우 보존 되지 않으므로 동일한 사용자 되었다가 (새 `xim_player` 인스턴스).

일반적으로 플레이어 unmuted 상태에서 시작합니다. 호출할 수 있는 앱에서 플레이어 게임 플레이 이유로 음소거 상태에서 시작 하려는 경우 `xim_player::set_chat_muted()` 에 `xim_player` 연결 된 처리를 마치기 전에 개체 `xim_player_joined_state_change`, XIM가 없는 기간 됩니다 보장 하 고 있는 오디오 음성 플레이어에서 낼 수 있습니다.

플레이어 신뢰도에 따라 자동 음소거 확인을 원격 플레이어 XIM 네트워크에 연결 하는 경우 발생 합니다. 플레이어에 잘못 된 평판 플래그, 플레이어 자동 음소거 상태입니다. 음소거만 로컬 상태를 적용 하 고 따라서 플레이어 네트워크를 통해 이동 하는 경우를 유지 합니다. 한 번 수행 되며 다시 평가 되지 다시에 대 한 자동 평판 기반 음소거 확인으로는 `xim_player` 유효한 상태로 유지 됩니다.

## <a name="configuring-chat-targets-using-player-teams"></a>플레이어 팀을 사용 하 여 채팅 대상 구성

에 명시 된 대로 합니다 [플레이어 팀과 함께 작동 하는 방법을 XIM](#how-xim-works-with-player-teams) 앱은 특정 팀 인덱스 값의 true 의미는이 문서의 섹션. XIM 채팅 대상 구성에 대해 같음 비교를 제외 하 고 해석 하지 않습니다. 채팅 대상 구성을 보고 `xim::chat_targets()` 는 현재 `xim_chat_targets::same_team_index_only`, 지정 된 모든 플레이어 채팅 통신 상호 교환할만 둘 다 동일한 값으로 보고 하는 경우 다음 `xim_player::team_index()` (및 개인 정보 보호 정책도 허용 /).

기본적으로 일반 성과 XIM 네트워크를 새로 만든 경쟁 시나리오는 자동으로 지원 하도록 `xim_chat_targets::same_team_index_only`합니다. 그러나 다른 팀에서 점령한 상대방과 채팅 후 게임 "로비"에서 예를 들어,이 바람직 할 수 있습니다. 모든 사용자가 다른 사용자 (여기서 개인 정보 보호 및 정책 허용)를 호출 하 여 연결할 수 있도록 XIM 지시할 수 있습니다 `xim::set_chat_targets()`합니다. 다음 샘플에서 시작 하는 데 XIM 네트워크의 모든 참가자가 구성 된 `xim_chat_targets::all_players` 값:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

모든 참가자가 제공 하는 경우 새 대상 설정을 적용 되는 내용 알림이 표시 됩니다는 `xim_chat_targets_changed_state_change`합니다.

앞에서 설명한 대로 XIM 네트워크 이동 대부분은 처음에 값을 할당 모든 플레이어 팀 인덱스 0입니다. 즉, 구성이 `xim_chat_targets::same_team_index_only` 가능성이 구별할 `xim_chat_targets::all_players` 기본적으로 합니다. 그러나 다른 경우 인덱스 값을 팀 결혼 정보 회사 연결을 사용 하 여 XIM 네트워크로 이동 하는 플레이어를 갖게 됩니다 결혼 정보 회사 연결 구성의 `xim_team_matchmaking_mode` 값 두 개 이상의 팀을 선언 합니다. 호출할 수도 있습니다 `xim_player::xim_local::set_team_index()` 언제 든 지 위와 같이 합니다. 앱에서 이러한 메서드 중 하나를 통해 0이 아닌 팀 인덱스 값을 사용 중인 경우 현재 채팅 대상을 적절 하 게 설정 관리를 잊지 마십시오.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>플레이어 슬롯 ("백필" 매 치메이 킹)의 자동 채우기

호출 하는 플레이어의 서로 다른 그룹 `xim::move_to_network_using_matchmaking()` 동시에 유연성을 제공 매 치메이 킹 Xbox Live 서비스는 가장 큰 신속 하 게 새 최적의 XIM 네트워크를 구성 합니다. 그러나 일부 게임 플레이 시나리오 그대로 특정 XIM 네트워크 및 빈 플레이어 슬롯을 채웁니다 추가 플레이어 matchmake만 유지 하려고 합니다. 매 치메이 킹 호출 하 여 모드나 "백필"를 입력 하는 자동 백그라운드에서 작동 하도록 구성 하는 XIM 지원 `xim::set_network_configuration()` 사용 하 여는 `xim_network_configuration` 있는 `xim_allowed_player_joins::matchmade` 플래그가 설정에 해당 `xim_network_configuration::allowed_player_joins` 속성.

다음 예제에서는 구성 (하지만 8 없으면 2-7 플레이어는도 허용 됨)는 없는 팀 전투 자유에 대 한 총 8 플레이어를 찾으려고 백필 매 치메이 킹, MYGAMEMODE_ 값으로 정의 된 앱 별 게임 모드 상수 uint64_t를 사용 하 여 동일한 값을 지정 하는 다른 플레이어와만 일치 하는 매치:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

네트워크 기존 XIM를 호출 하는 장치를 사용할 수 있도록이 `xim::move_to_network_using_matchmaking()` 일반적인 방식입니다. 해당 장치에 변경 동작이 없습니다. 백필 XIM 네트워크 참가자 이동 하지 것입니다 하지만 제공 됩니다는 `xim_network_configuration_changed_state_change` 이름임을 켜면, 백필 뿐만 아니라 여러 `xim_matchmaking_progress_updated_state_change` 해당 하는 경우 알림. 일반을 사용 하 여 XIM 네트워크에 추가할 모든 matchmade 플레이어 `xim_player_joined_state_change`합니다.

기본적으로 백필 매 치메이 킹 않지만 여전히 진행에서 무기한 XIM 네트워크에 있는 경우 이미 최대 xim_team_configuration 설정에 의해 지정 플레이어 플레이어를 추가 하려고 시도 하지. 백필은 matchmade를 허용 하지 않도록 설정 xim_allowed_player_joins 하 여 비활성화할 수 있습니다. 다음 예제에서는 다른 모든 기존 플래그 및 네트워크 구성 설정을 계속 사용 하면서 xim_allowed_player_joins::matchmade 플래그를 지우면 백필을 해제 합니다.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

해당 `xim_network_configuration_changed_state_change` 완료 되 면이 비동기 프로세스를 최종 및 모든 장치에 제공 됩니다 `xim_matchmaking_progress_updated_state_change` 제공 됩니다 `xim_matchmaking_status::none` XIM 네트워크에 없는 추가 matchmade 플레이어를 추가할 수를 나타내는입니다.

사용 하 여 백필 결혼 정보 회사 연결을 사용 하도록 설정할 때를 `xim_team_configuration` 두 개 이상의 팀을 선언 하는 설정, 모든 기존 플레이어 1 및 팀의 번호 사이의 유효한 팀 인덱스가 있어야 합니다. 여기에 호출 하는 플레이어 `xim_player::xim_local::set_team_index()` 초대를 사용 하 여 조인한 또는 기타 소셜 네트워크를 통해 의미 있는 사용자 또는 사용자 지정 값을 지정 하려면 (예에 대 한 호출 `xim::move_to_network_using_joinable_xbox_user_id`) 팀 인덱스 기본값은 0 사용 하 여 추가 되었습니다. 모든 플레이어에 유효한 팀 인덱스가 없는 결혼 정보 회사 연결 프로세스를 일시 중지 됩니다 하 고 모든 참가자가 제공 됩니다 하는 경우는 `xim_matchmaking_progress_updated_state_change` 사용 하 여는 `xim_matchmaking_status::waiting_for_player_team_index` 값입니다. 모든 플레이어 제공 되거나 해당 팀 인덱스 값으로 수정 되 면 `xim_player::xim_local::set_team_index()`, 백필 매 치메이 킹 다시 시작 됩니다. 자세한 정보를 찾을 수 있습니다 합니다 [플레이어 팀과 함께 작동 하는 방법을 XIM](#how-xim-works-with-player-teams) 이 문서의 섹션입니다.

마찬가지로, 사용 하 여 백필 결혼 정보 회사 연결을 사용 하도록 설정 하는 경우는 `xim_network_configuration` 구조체를 `require_player_roles_and_skill_configuration` 모든 플레이어 해야 플레이어 당 null이 아닌 결혼 정보 회사 연결 구성을 지정한 다음 역할 또는 기술에 대해 true로 설정 필드. 모든 플레이어 않은 하 고 매 치메이 킹 프로세스를 일시 중지 됩니다. 모든 참가자가 제공 됩니다 하는 경우는 `xim_matchmaking_progress_updated_state_change` 사용 하 여는 `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` 값입니다. 모든 플레이어를 제공한 후 해당 `xim_player_roles_and_skill_configuration` 구조 백필 매 치메이 킹 다시 시작 됩니다. 자세한 정보를 찾을 수 있습니다 합니다 [플레이어 당 기술을 사용 하 여 매 치메이 킹](#matchmaking-using-per-player-skill) 및 [플레이어 당 역할을 사용 하 여 매 치메이 킹](#matchmaking-using-per-player-role) 이 문서의 섹션입니다.

## <a name="querying-joinable-networks"></a>조인 가능한 네트워크를 쿼리합니다.

매 치메이 킹 신속 하 게 플레이어를 함께 연결 하는 훌륭한 방법 이지만, 경우 플레이어의 사용자 지정 검색 조건을 사용 하 여 참가할 수 있는 네트워크 검색을 허용 하 고 가입 하려는 네트워크를 선택 하는 최선의 합니다. 이 게임 세션 큰 게임 구성 가능한 규칙 및 플레이어 설정 집합이 있을 때 특히 유용할 수 있습니다. 이렇게 하려면 기존 네트워크를 먼저 만들어야 쿼리할 수 함으로써 `xim_allowed_player_joins::queried` 호출을 통해 네트워크 외부 다른 사용자에 게 제공 되는 네트워크 정보 구성과 joinability `xim::set_network_configuration()`합니다.

다음 예에서는 `xim_allowed_player_joins::queried` joinability, 집합 1 팀에서는 GAME_MODE_BRAWL 값으로 정의 하는 앱 별 게임 모드 상수 uint64_t 함께 1-8 플레이어의 총 수 있도록 팀 구성 사용 하 여 구성 네트워크 설명 "cat 및 있습니다의 boxing 일치" 앱 별 맵 인덱스 상수 uint32_t MAP_KITCHEN 값으로 정의 된 spectatorallowed"태그"chatrequired","간단"" 포함 되어 있습니다.

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

네트워크 외부의 다른 플레이어를 호출 하 여 네트워크에서 찾을 수 있는 `xim::start_joinable_network_query()` 이전에 네트워크 정보를 일치 하는 필터 집합을 사용 하 여 `xim::set_network_configuration()` 호출 합니다. 다음 예제에서는 GAME_MODE_BRAWL 값으로 정의 된 앱 별 게임 모드를 사용 하 여 네트워크에 대해를 쿼리만 게임 모드 필터 옵션을 사용 하 여 참가할 수 있는 네트워크 쿼리를 시작 합니다.

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

"편리" 태그를 가진 네트워크 및 "spectatorallowed" 쿼리 가능한 공용 구성에서 쿼리 하는 태그 필터 옵션을 사용 하는 또 다른 예는 다음과 같습니다.

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

다른 필터 옵션을 결합할 수도 있습니다. 게임 모드 필터 옵션 및 태그를 사용 하는 다음 예제에서는 필터 옵션 둘 다 있는 앱 별 게임 모드 상수 GAME_MODE_BRAWL 및 태그를 "편리 함" 네트워크에 대 한 쿼리를 시작 하기:

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

쿼리 작업에 성공 하면 앱 받을 `xim_start_joinable_network_query_completed_state_change` 앱 참가할 수 있는 네트워크 목록을 검색할 수 있는 합니다. 앱도 지속적으로 받을 `xim_joinable_network_query_updated_state_change` 수동 또는 자동으로 중지 될 때까지 참가할 수 있는 네트워크 목록이 반환된 하는 모든 변경 내용 또는 추가 조인 가능 네트워크에 대 한 합니다. 호출 하 여 진행 중인 쿼리를 수동으로 중지 `xim::stop_joinable_network_query()`합니다. 호출할 때 자동으로 중지 되 고 `xim::start_joinable_network_query()` 새 쿼리를 시작 합니다.

앱을 호출 하 여 참가할 수 있는 네트워크 목록에서 네트워크에 가입 하도록 시도할 수 `xim::move_to_network_using_joinable_network_information()`입니다. 다음 예제에서는 조인 하려고 하는 `xim_joinable_network_information` (따라서 nullptr 두 번째 매개 변수를 전달) 암호로 보호 되는 포인터 'selectedNetwork' 가리키는:

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

플레이어를 호출 하 여 가입 된 두 개 이상의 팀을 선언 하는 xim_team_configuration 사용 하 여 네트워크 쿼리를 사용 하도록 설정한 경우 `xim::move_to_network_using_joinable_network_information()` 0 기본 팀 인덱스 값을 갖게 됩니다.

> [!NOTE]
> 앱이 여러 로컬 사용자 지정 하 고 로컬 사용자 수보다 더 적은 공간이 있는 네트워크에 연결 되어 있으면 조인 계속 성공할 수 있습니다. 하지만 로컬 사용자만 허용 된 수의 네트워크에 조인할 수 있습니다.