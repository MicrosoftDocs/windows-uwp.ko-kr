---
title: 멀티 플레이 관리자 API 개요
description: Xbox Live 멀티 플레이 게임 관리자 API에 알아봅니다.
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이 게임, 멀티 플레이 관리자
ms.localizationpriority: medium
ms.openlocfilehash: 7838de6845bc6c49acf649c0e859cd0d7020490f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628028"
---
# <a name="multiplayer-manager-api-overview"></a>멀티 플레이 게임 관리자 API 개요

이 페이지는 멀티 플레이 게임 관리자 API 및 게임에서 사용 되는 방식을는 광범위 한 개요를 설명 합니다. 가장 중요 한 클래스 및 API의 메서드를 호출합니다. API 자세한 참조 설명서를 참조 하십시오. 응용 프로그램에서 이러한 Api를 사용 하는 방법의 예는 멀티 플레이 게임 샘플을 참조 하세요.

## <a name="namespace"></a>네임스페이스
다중 접속 Manager 클래스는 다음 네임 스페이스:

| 언어 | 네임스페이스 |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

있습니다 목록 다음 이해 해야 하는 주요 클래스를 설명 합니다.

* [다중 접속 Manager 클래스](#multiplayer-manager-class)
* [다중 접속 이벤트 클래스](#multiplayer-event-class)
* [다중 접속 멤버 클래스](#multiplayer-member-class)
* [다중 접속 로비 세션 클래스](#multiplayer-lobby-session-class)
* [다중 접속 게임 세션 클래스](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>다중 접속 Manager 클래스 <a name="multiplayer-manager-class">

| 언어 | class |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

이 클래스는 다중 접속 관리자와 상호 작용 하기 위한 기본 클래스입니다. 이므로 단일 클래스를 만들고 클래스를 한 번 초기화 해야 합니다.
이 클래스는 단일 로비 세션 개체를 단일 게임 세션 개체를 포함합니다.

최소한 호출 해야 합니다는 `initialize()` 및 `do_work()` 함수에서 다중 접속 Manager에 대 한 순서로이 클래스의 메서드.

다음 표에서 일부에 대해 설명 하지만이 클래스의 메서드 및 속성을 사용 중 일반적으로 더 합니다. 클래스 멤버의 전체 설명이 포함 된 목록, 참조를 참조 하십시오.

| C++/CX | C++ | description |
| --- | --- | --- |
| **메서드** | | |
| Initialize) | initialize() | 다중 접속 관리자를 초기화 합니다. 다중 접속 관리자를 사용 하기 전에이 메서드를 호출 해야 합니다. |
| DoWork() | do_work() | 앱 표시 세션 상태를 업데이트합니다. 프레임당 한 번 이상이 메서드를 호출 해야 하 고 게임 메서드에 의해 반환 되는 멀티 플레이 이벤트를 처리 해야 합니다. |
| JoinLobby() | join_lobby() | 친구의 로비 세션 로비를 고유 하 게 식별 하는 handleId 통해 참여 하 시겠습니까 또는 사용자가 활성화 되는 프로토콜을 가져오려는 제목은 초대를 수락 하는 경우 조인 하는 방법을 제공 합니다. |
| JoinGameFromLobby() | join_game_from_lobby() | 있는 경우 및 공간이 있는 경우 로비의 게임 세션에 참가 합니다. 세션 없는 경우 기존 로비 멤버를 사용 하 여 새 게임 세션을 만듭니다. 이 게임 세션을 통해 기존 로비 세션 속성을 마이그레이션하지 않습니다. 조인한 후 속성 또는 호스트를 통해 설정할 수 있습니다 `game_session()::set_synchronized_*` Api. 제목 게임 세션에 참여 하는 모든 클라이언트에서이 API를 호출 해야 합니다.|
| JoinGame() | join_game() | 일반적으로 타사 결혼 정보 회사 연결 서비스를 통해 찾을 수는 전역적으로 고유한 세션 이름이 지정 된 기존 게임 세션을 조인 합니다. Xbox 사용자 게임의 일부가 되도록 하려는 Id의 목록에 전달할 수 있습니다.|
| FindMatch() | find_match() | 매 치메이 킹 Xbox Live를 사용 하 여 찾기 및 게임 참가할 수 있습니다. |
| LeaveGame() | leave_game() | 게임을 유지 하 고 로비에 멤버 및 모든 로컬 멤버를 반환 합니다. |
| **속성** | | |
| LobbySession | lobby_session() | 로비 세션을 나타내는 개체에 대 한 핸들입니다. |
| GameSession |  game_session() | 게임 세션을 나타내는 개체에 대 한 핸들입니다. |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>다중 접속 이벤트 클래스 <a name="multiplayer-event-class">

| 언어 | class |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

호출 하는 경우 `do_work()`, 멀티 플레이 게임 관리자에 마지막으로 호출한 이후 변경 내용을 세션을 나타내는 이벤트의 목록을 반환 합니다 `do_work()`합니다. 멤버는 세션에 참가 했습니다 같은 변경 내용을 포함 하는 이러한 이벤트, 멤버는 세션을 떠난, 멤버 속성 변경, 호스트 클라이언트 변경 등입니다.

모든 가능한 이벤트 종류의 목록을 보려면 참조는 `multiplayer_event_type` 열거형입니다.

반환 되는 각 `multiplayer_event` 포함는 `event_args`, 이벤트 형식에 대 한 적절 한 event_args 클래스에 캐스팅 해야 하 합니다. 예를 들어 경우는 `event_type` 됩니다 `member_joined`, 캐스트 한 다음는 `event_args` 에 대 한 참조를 `member_joined_event_args` 클래스.

게임 호출한 후 각 필요에 따라 이벤트를 처리 해야 `do_work()`합니다.

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>다중 접속 멤버 클래스 <a name="multiplayer-member-class">

| 언어 | class |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

이 클래스는 로비 또는 게임 세션의 플레이어를 나타냅니다. 클래스는 XboxUserID를 포함 하 여 각 플레이어 등에 대 한 플레이어, 플레이어에 대 한 네트워크 연결 주소, 사용자 지정 속성에 대 한 멤버에 대 한 속성을 포함 합니다.

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>다중 접속 로비 세션 클래스 <a name="multiplayer-lobby-session-class">

| 언어 | class |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

이 장치와 함께 재생을 초대 된 친구를 로컬 사용자 관리에 사용 되는 영구 세션입니다. 로비 세션 멀티 플레이 작업을 수행할 다중 접속 Manager에 대 한 순서로 하나 이상의 멤버를 포함 해야 합니다. 호출 하 여 새 로비 세션을 만들면 처음에 `add_local_user()` 메서드.

다음 표에서 일부에 대해 설명 하지만이 클래스의 메서드 및 속성을 사용 중 일반적으로 더 합니다. 클래스 멤버의 전체 설명이 포함 된 목록, 참조를 참조 하십시오.

| C++/CX | C++ | description |
| --- | --- | --- |
| **메서드** | | |
| AddLocalUser() | add_local_user() | 로컬 사용자 (로컬 장치에 로그인 하는 플레이어) 로비 세션에 추가 합니다. 로비 세션에 추가할 첫 번째 구성원 인 새 로비 세션을 만듭니다 합니다. |
| RemoveLocalUser() | remove_local_user() | 로비, 게임 세션에서 지정된 된 멤버를 제거합니다. |
| InviteFriends() | invite_friends() | 표준 Xbox Live는 UI를, 친구 목록에서 사용자를 선택 플레이어 있으며 그런 다음 이러한 플레이어 게임으로 초대를 엽니다. |
| InviteUsers() | invite_users() | 게임 sepcified Xbox Live 사용자를 초대 합니다. |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | 로컬 멤버에 대 한 네트워크 주소를 설정합니다. 게임 멤버 간 네트워크 통신을 설정 하려면이 네트워크 주소를 사용할 수 있습니다. |
| SetLocalMemberProperties() | set_local_member_properties() | 로컬 멤버에 대 한 사용자 지정 속성을 설정합니다. 속성은 JSON 문자열에 저장 됩니다. |
| DeleteLocalMemberProperties() | delete_local_member_properties() | 로컬 멤버에 대 한 사용자 지정 속성을 제거합니다. |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 로비 세션에 대 한 사용자 지정 속성을 설정합니다. 속성은 JSON 문자열에 저장 됩니다. 속성 장치 간에 공유 되 고 동시에 여러 장치에서 업데이트 될 수 있습니다, 동기화 된 버전의 메서드를 사용 합니다. |
| IsHost() | is_host() | 현재 장치 역할을 로비 호스트 하는 경우를 나타냅니다. |
| SetSynchronizedHost() | set_synchronized_host() | 로비의 호스트를 설정합니다. |
| **속성** | | |
| LocalMembers | local_members() | 로컬 장치에 로그인 하는 멤버의 컬렉션입니다. |
| 구성원 | members() | 로비 세션에 있는 멤버의 컬렉션입니다. |
| 속성 | properties() | 로비 세션에 대 한 속성의 컬렉션을 나타내는 JSON 개체입니다. |
| 호스트 | host() | 로비에 대 한 호스트 멤버입니다. |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>다중 접속 게임 세션 클래스 <a name="multiplayer-game-session-class">

| 언어 | class |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

게임 세션 인스턴스의 실제 게임 플레이에 참여 하는 Xbox Live 구성원 그룹을 나타냅니다. 이 결혼 정보 회사 연결 서비스를 통해 일치 된 플레이어를 포함할 수 있습니다.

멤버를 포함 하는 새 게임 세션을 시작 하는 `lobby_session`를 호출할 수 있습니다 `multiplayer_manager::join_game_from_lobby()`합니다. 매 치메이 킹 Xbox Live를 사용 하려는 경우에 호출 하면 `multiplayer_manager::find_match()`합니다. 타사 결혼 정보 회사 연결 서비스를 사용 하는 경우에 호출 하면 `multiplayer_manager::join_game()`합니다.

다음 표에서 일부에 대해 설명 하지만이 클래스의 메서드 및 속성을 사용 중 일반적으로 더 합니다. 클래스 멤버의 전체 설명이 포함 된 목록, 참조를 참조 하십시오.

| C++/CX | C++ | description |
| --- | --- | --- |
| **메서드** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 게임 세션에 대 한 사용자 지정 속성을 설정합니다. 속성은 JSON 문자열에 저장 됩니다. 속성 장치 간에 공유 되 고 동시에 여러 장치에서 업데이트 될 수 있습니다, 동기화 된 버전의 메서드를 사용 합니다. |
| IsHost() | is_host() | 현재 장치 역할을 게임 호스트 하는 경우를 나타냅니다. |
| SetSynchronizedHost() | set_synchronized_host() | 게임의 호스트를 설정합니다. |
| **속성** | | |
| 구성원 | members() | 게임 세션에 있는 멤버의 컬렉션입니다. |
| 속성 | properties() | 게임 세션에 대 한 속성의 컬렉션을 나타내는 JSON 개체입니다. |
| 호스트 | host() | 게임에 대 한 호스트 멤버입니다. |
