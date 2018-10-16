---
title: XIM의 대역 예약
author: KevinAsgari
description: Xbox 통합 멀티 플레이어 (XIM)-대역 외 예약을 통해 전용된 채팅 솔루션으로 사용 하는 방법을 설명 합니다.
ms.assetid: 0ed26d19-defb-414d-a414-c4877bd0ed37
ms.author: kevinasg
ms.date: 01/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xbox 통합된 멀티 플레이어, xim, 채팅
ms.localizationpriority: medium
ms.openlocfilehash: aeae8316aae29c548c1f2e0463ae1db916a259e3
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4686960"
---
# <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>대역 외 예약을 통해 전용 채팅 솔루션으로 XIM 사용

대부분의 앱은 XIM을 사용하여 플레이어를 통합하는 모든 측면을 처리할 수 있습니다. 즉, 일반 멀티 플레이어 시나리오를 포괄적으로 지원하는 데 필요한 기능을 모두 어셈블하는 것에 초점을 맞춘 것은 이것이 "Xbox 통합 멀티 플레이어"라고 불리는 이유입니다. 그러나 전용된 인터넷 서버를 사용 하 여 멀티 플레이 자체 솔루션을 구현 하는 일부 앱 안정적이 고 짧은 대기 시간이 짧은, 저렴 한 피어 투 피어 채팅 통신의 이점을 원합니다 수도 있습니다. XIM은이 요구를 인식 하 고 XIM API 외부에서 발생 하는 외부 플레이어 관리에 대 한 XIM의 간소화 된 피어 통신을 활용 하는 확장 모드를 지원 합니다. "예약"를 사용 하 여 플레이어가 이동 소셜 수단 또는 연결을 통해 XIM 네트워크로 이동 하는 대신 수 있는 특정 사용자에 대해 보장 된 자리 표시자는 앱의 외부 플레이어 접선 메커니즘을 통해 "대역"를 교환 합니다.

이동 프로세스 외의 대역 예약을 사용 하 여 관리 XIM 네트워크를 효과적으로 다른 XIM 네트워크 같습니다. 모든 통신 기능이 동일 하 게 작동 합니다. 그러나 매치 메이 킹 및 소셜 검색 API 메서드 반드시 앱의 외부 구현을 충돌할 이후 대역의 예약을 사용 하 여 관리 XIM 네트워크에 대 한 비활성화 됩니다. 예를 들어 초대에서 이러한 XIM 네트워크에 보낼 수 없습니다.

>XIM 간단한 종단 간 솔루션을 제공 하도록 최적화 되어 있습니다. 따라서 모든 복잡 한 토폴로지 또는 시나리오의 대역 예약에 대 한 완벽 한 맞는 수 있습니다. 여부에 대 한 질문 또는 XIM의 통신 기능을 활용 하는 방법, 있는 경우 Microsoft 담당자에 게 문의 합니다.

후속 항목 XIM에서 대역의 예약을 활용 하는 방법에 설명 합니다. 이전 섹션에 설명 된 "표준" XIM 사용에서 상대적으로 몇 가지 차이점을 인해 일부 토론 축약 됩니다. [XIM 사용 하 여](using-xim.md) 익숙한 것이 좋습니다.

항목:

1. [새 대역의 예약 XIM 네트워크에 이동](#moving)
1. [대역의 예약을 사용 하 여 관리 플레이어를 XIM 네트워크에 추가](#adding)
1. [대역의 예약을 사용 하 여 관리 XIM 네트워크에서 채팅 대상 구성](#targets)
1. [대역의 예약을 사용 하 여 관리 플레이어가 XIM 네트워크에서 제거](#remove)
1. [대역의 예약을 사용 하 여 관리 XIM 네트워크를 정리](#clean)

## <a name="moving-to-a-new-out-of-band-reservation-xim-network"></a>새 대역의 예약 XIM 네트워크에 이동

대역의 예약 사용을 시작 하려면이 모드에서 만든 새 XIM 네트워크에 이동 해야 수집한 참가자 중 하나입니다. 사용자가 디바이스의 참여 한 피어 선택 합니다. 게임 호스트 또는 서버 프로세스를 시작 하는 자연 스러운 적합 이며의 개념을 이미 있을 수 있지만 그럴 필요는 없습니다. 가장 빠른 연결 설치 시간을 달성 하기 위해 "열기" 네트워크 액세스 종류를 보고 하는 장치 선택 않는 것이 좋습니다. 참조는 `Windows::Networking::XboxLive` 자세한 내용은 플랫폼 설명서.

XIM 초기화 및 표준 XIM 사용 연습에서와 같이 의도 한 로컬 Xbox 사용자 Id를 선언 하 여 네트워크의 대역 예약을 통해 관리 수행 되지만 메서드를 호출 하는 대신 같은 XIM 이동 `xim::move_to_new_network()`을 호출 `xim::move_to_network_using_out_of_band_reservation()` null 예약 문자열입니다. 예를 들면 다음과 같습니다.

```cpp
 xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
 xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
 xim::singleton_instance().move_to_network_using_out_of_band_reservation(nullptr);
```

표준 `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, 및 `xim_move_to_network_succeeded_state_change` 일반적인의 상태 변경 처리 하는 동안 시간이 지남에 따라 제공 될 예정 `xim::start_processing_state_changes()` 및 `xim::finish_processing_state_changes()` 루프 합니다. 예를 들면 다음과 같습니다.

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
             MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change *>(stateChange));
             break;
         }

         case xim_state_change_type::player_left:
         {
             MyHandlePlayerLeft(static_cast<const xim_player_left_state_change *>(stateChange));*
             break;
         }

         ...
     }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

초기 장치에 상태 변경을 처리 하 고 적 되 면 해당 플레이어가 성공적으로 이동 XIM 네트워크에 추가 사용자에 대 한 예약을 만들어야 합니다.

## <a name="adding-players-to-a-xim-network-managed-using-out-of-band-reservations"></a>대역의 예약을 사용 하 여 관리 플레이어를 XIM 네트워크에 추가

값을 보고 하는 관리 되는 항상 대역의 예약을 사용 하 여 XIM 네트워크 `xim_allowed_player_joins::out_of_band_reservation` 에서 `xim::allowed_player_joins()` 메서드. 호출 하 여 자신의 Xbox 사용자 Id에 대 한 예약 된 지점을 사용 하 여 제외 하 고 모든 플레이어를 마칠는 `xim::create_out_of_band_reservation()`. `xim::create_out_of_band_reservation()` 변수가 배열 사용자를 한 번에 또는 시간별 수집된 외부 플레이어를 위해 이러한 예약을 만들 수 있습니다. 또한 플레이어가 XIM 네트워크에 참여 하 고 이미 있는 사용자 무시 되므로 더 편리 하 게 델타 변경 또는 완전히 대체 집합으로 추가 Xbox 사용자 Id를 제공할 수도 있습니다. 다음 예제에서는 이미 있다고 가정 Xbox 사용자 Id 문자열 포인터 배열에 완벽 하 게 수집된 집합을 변수 'xboxUserIds' 'xboxUserIdCount' 숫자 요소와 함께:

```cpp
 xim::singleton_instance().create_out_of_band_reservation(xboxUserIdCount, xboxUserIds);
```

이 예약 된 지정 된 Xbox 사용자 Id에 대해 만드는 비동기 프로세스를 시작 합니다. 작업이 완료 되 면 XIM가 제공는 `xim_create_out_of_band_reservation_completed_state_change` 성공 또는 실패 보고 합니다. 성공 하면 예약 문자열로 사용 가능 하 게 작동에 제공 된 Xbox 사용자 Id를 제공 하기 위해 시스템에 대 한 합니다. 성공적으로 생성 하는 예약 문자열은만 정해진 시간에 대 한 유효 합니다. 해당 시간 내에 반환 됩니다 `xim_create_out_of_band_reservation_completed_state_change`.

유효한 예약 문자열로 해당 열거 하는 문자열을 배포 하 XIM 외부 플레이어를 수집 하는 데 "대역" 외부 메커니즘을 사용할 수 있습니다. 예를 들어 멀티 플레이어 세션 디렉터리 (MPSD) Xbox Live 서비스를 사용 하는 경우 작성할 수 있습니다이 문자열은 세션 문서에서 사용자 지정 속성으로 (참고: 예약 문자열은 제한 된 집합에서 사용 하기 위해 안전한 문자는 항상 JSON 이스케이프 또는 Base64 인코딩 필요 없이).

다른 사용자가 예약 문자열이 되 면 다음을 시작할 수의 매개 변수로 사용 하는 XIM 네트워크로 이동 `xim::move_to_network_using_out_of_band_reservation()`. 다음 예제에서는 예약 문자열 'reservationString' 라는 변수에 압축이 가정 합니다.

```cpp
xim::singleton_instance().move_to_network_using_out_of_band_reservation(reservationString);

```

이동 작업 예약 문자열에 대 한 null 포인터를 지정 하는 초기 디바이스와 마찬가지로 비동기적으로 실행 합니다. 상태가 변경 `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, 및 `xim_move_to_network_succeeded_state_change` 이동 하 여 생성 됩니다. 성공적으로 이동 하는 경우 로컬 및 원격 플레이어 추가 됩니다. XIM 네트워크에서 기존 디바이스를 제공 될 것을 `xim_player_joined_state_change` 이러한 새 플레이어에 대 한 합니다. 이 시점에서 음성 및 문자 채팅 통신 (여기서 개인 정보 및 정책 허용)이 XIM 네트워크에서 다양 한 장치에서 플레이어 간에 자동으로 활성화 됩니다.

잘못 된 예약 문자열 인해 이동 작업이 실패 해야, XIM는 반환 되는 `xim_network_exited_state_change` 이유를 사용 하 여 `xim_network_exit_reason::invalid_reservation`. 이 여러 가지 이유로 인해 발생할 수 있습니다.

1. 제목 호출 하려고 `xim::move_to_network_using_out_of_band_reservation()` 하기 전에 원격 장치에는 `xim_create_out_of_band_reservation_completed_state_change` 호스트 디바이스에서 반환 됩니다
1. 제목 호출 하려고 `xim::move_to_network_using_out_of_band_reservation()` 예약 만료 된 후 원격 디바이스에서.
1. 제목 호출 하려고 `xim::move_to_network_using_out_of_band_reservation()` 여러 번 호출 하는 동안만 원격 디바이스에서 `xim::create_out_of_band_reservation()` 되 면 합니다.

성공 또는 실패에 관계 없이 예약을 사용 하는 이동 작업에 의해 사용 됩니다. 따라서 각 사용 하 여 시도 후 호스트 생성할지 새 예약 문자열을 호출 하 여 `xim::create_out_of_band_reservation()` 다시 합니다.

## <a name="configuring-chat-targets-in-a-xim-network-managed-using-out-of-band-reservations"></a>대역의 예약을 사용 하 여 관리 XIM 네트워크에서 채팅 대상 구성

XIM 네트워크 대역의 예약을 사용 하 여 관리 되는 채팅 통신에 대해 기존의 XIM 네트워크에 동일 하 게 동작 합니다. 경쟁 지원 하기 위해 시나리오의 경우 XIM 네트워크를 새로 만든만; 같은 팀의 구성원 인 다른 플레이어와 채팅을 지원 하도록 구성 자동으로 구성 된 값에서 보고 하는 즉, `xim::chat_targets()` 는 기본적으로 `xim_chat_targets::same_team_index_only`. 이 호출 하 여 다른 모든 (여기서 개인 정보 및 정책 허용)에 게 모든 변경할 수 있습니다 `xim::set_chat_targets()`. 다음 예제에서는 시작 사용 하 여 XIM 네트워크에서 구성 하는 모든 사용자는 `xim_chat_targets::all_players` 값:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

설정을 적용 하는 새 대상 때 제공 하며 모든 참가자는 정보는 `xim_chat_targets_changed_state_change`.

모든 플레이어의 대역 예약을 사용 하 여 관리 XIM 네트워크에서 처음에 0으로 동일한 팀 인덱스 값을 사용 하 여 구성 합니다. 즉,의 구성 `xim_chat_targets::same_team_index_only` 구분 되지 않습니다 `xim_chat_targets::all_players` 기본적으로 합니다. 하지만 호출 하 여 언제 든 지 로컬 플레이어의 팀 인덱스를 변경할 수는 `xim_player::xim_local::set_team_index()`. 다음 예제에서는 하나의 새 팀 인덱스 값을 가질 ' localPlayer' 플레이어가 포인터를 구성 합니다.

```cpp
 localPlayer->local()->set_team_index(1);
```

모든 디바이스는 플레이어가 때에 있는 경우 새 팀 인덱스 값을 적용 한 xim_player_team_index_changed_state_change 해당 플레이어에 대 한 제공 하며 알림을 받습니다. 채팅 대상 구성 xim_chat_targets::same_team_index_only 현재 경우, 동일한 새 팀 인덱스를 사용 하 여 다른 플레이어는 섬 청각 음성 및 텍스트 채팅 (개인 정보 및 정책 허용)를 제공 하 고 변경 플레이어와 그 반대로에서. 이전 팀 인덱스를 사용 하 여 플레이어 채팅 통신 이러한 교환 중지 됩니다. 채팅 대상 구성이 xim_chat_targets::all_players 현재 경우 팀 인덱스에는 함께 작업할 chat 수에 영향을 주지 않습니다.

## <a name="removing-players-from-a-xim-network-managed-using-out-of-band-reservations"></a>대역의 예약을 사용 하 여 관리 플레이어가 XIM 네트워크에서 제거

플레이어의 명단을 관리 하 고 외부 심해 대역의 예약 나타나므로 자연스럽 플레이어를 XIM 네트워크에서 제거 해야 할 수 있습니다. 일반적인 방법은 XIM 네트워크 및 후속 예약 원래 또한 플레이어 제거를 관리 하 고 호출 하 여 작업을 만드는 데 사용한 동일한 게임 호스트를 활용 하 여 앱에 대 한 `xim::kick_player()`. XIM 네트워크에서 지정 된 플레이어와 동일한 디바이스에서 모든 플레이어를 제거합니다. 다음 예제에서는 앱에서 제거 하려고 한다는 것을 확인 하는 것으로 가정 합니다 `xim_player` 'playerToRemove' 변수 가리키는 개체:

```cpp
xim::singleton_instance().kick_player(playerToRemove);
```

해당 플레이어가 (또는 플레이어) 제공 될 예정 필요한 `xim_player_left_state_change` 모든 플레이어에 대 한 및 `xim_network_exited_state_change` 나타내는 네트워크에서 시작 되었습니다. 또는 각 플레이어를 호출할 수 있습니다 `xim::leave_network()` 자체은 같은 효과 종료 하려면.

XIM 피어-투-피어 통신 언제 든 지 플레이어 요소가 환경 어려움이 인해 XIM 네트워크를 유지 하는 자체 결정을 만들 수는 명심 하세요. 앱 처리할 준비가 되어 있어야 "원치 않는"는 `xim_player_left_state_change` XIM의 상태 및 특정 앱에 대 한 적절 한 방식으로 외부 플레이어 관리 스키마에 차이가 조정 합니다.

## <a name="cleaning-up-a-xim-network-managed-using-out-of-band-reservations"></a>대역의 예약을 사용 하 여 관리 XIM 네트워크를 정리

모든 플레이어를 XIM에서 시작 이미 하지 않은 네트워크를 마치 유일한 상태로 반환 하려면 xim::initialize() 및 `xim::set_intended_local_xbox_user_ids()` 를 호출한, 이렇게 하면을 사용 하 여 시작할 수는 `xim::leave_network()` 메서드:

```cpp
xim::singleton_instance().leave_network();
```

이 다른 참가자에서 연결 끊기 비동기적으로 시작 합니다. 이렇게 하면 원격 디바이스 명시 하는 `xim_player_left_state_change` 제공 되는 로컬 플레이어가 및 로컬 장치에 대 한는 `xim_player_left_state_change` 로컬 또는 원격 각 플레이어에 대 한 합니다. 때 모든 안전한 연결 끊기 완료 되 면 최종 `xim_network_exited_state_change` 가 제공 됩니다. 앱을 호출할 수 `xim::cleanup()` 에서 모든 리소스를 확보 하 고 초기화 되지 않은 상태로 돌아갑니다.

```cpp
 xim::singleton_instance().cleanup();
```

호출 `xim::leave_network()` 기다리는 및 합니다 `xim_network_exited_state_change` 는 XIM을 종료 하려면 네트워크 정상적으로 항상 좋습니다 시기는 `xim_network_exited_state_change` 에서 이미 제공 하지 않았습니다. 호출 `xim::cleanup()` 직접 통신 성능에 문제가 발생할 수 나머지 참가자 시간 초과 메시지는 단순히 "사라질" 디바이스를 강제로 중인 동안 합니다.
