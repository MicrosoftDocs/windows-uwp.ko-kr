---
title: 멀티 플레이어 관리자 API 개요
author: KevinAsgari
description: Xbox Live 멀티 플레이어 관리자 API에 알아봅니다.
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어, 멀티 플레이어 관리자
ms.localizationpriority: medium
ms.openlocfilehash: 2aa975544fb1e53b75fa32b1163086fc4aa77aaa
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4339900"
---
# <a name="multiplayer-manager-api-overview"></a>멀티 플레이어 관리자 API 개요

이 페이지는 멀티 플레이어 관리자 API 및 게임에서 사용 방법을 간략하게를 설명 합니다. 가장 중요 한 클래스와 API에서 메서드를 호출합니다. 자세한 API 정보에 대 한 참조 설명서를 참조 하세요. 응용 프로그램에서 이러한 Api를 사용 하는 방법의 예제를 보려면 멀티 플레이어 샘플을 참조 하세요.

## <a name="namespace"></a>네임스페이스
멀티 플레이어 관리자 클래스는 다음 네임 스페이스:

| language | 네임스페이스 |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

있습니다 목록 다음 이해 해야 하는 주요 클래스를 설명 합니다.

* [멀티 플레이어 관리자 클래스](#multiplayer-manager-class)
* [멀티 플레이어 이벤트 클래스](#multiplayer-event-class)
* [멀티 플레이어 멤버 클래스](#multiplayer-member-class)
* [멀티 플레이어 로비 세션 클래스](#multiplayer-lobby-session-class)
* [멀티 플레이어 게임 세션 클래스](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>멀티 플레이어 관리자 클래스<a name="multiplayer-manager-class">

| language | 클래스 |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

이 클래스는 멀티 플레이어 관리자와 상호 작용 하기 위한 기본 클래스입니다. 이므로 의도 하지 않은 단일 클래스를 생성 하 고 클래스를 한 번 초기화 하기만 하면 합니다.
이 클래스는 단일 로비 세션 개체 및 단일 게임 세션 개체에 포함 되어 있습니다.

최소한 호출 해야 합니다 `initialize()` 및 `do_work()` 함수를 멀티 플레이어 관리자에 대 한 순서로이 클래스의 메서드.

다음 표에서 일부를 설명 하지만이 클래스에 대 한 메서드 및 속성 중 더 일반적으로 사용 합니다. 클래스 멤버의 전체는 설명이 포함 된 목록에 대 한 참조를 참조 하세요.

| C++/CX | C++ | description |
| --- | --- | --- |
| **메서드** | | |
| Initialize) | initialize) | 멀티 플레이어 관리자를 초기화 합니다. 멀티 플레이어 관리자를 사용 하기 전에이 메서드를 호출 해야 합니다. |
| DoWork() | do_work() | 앱 표시 세션 상태를 업데이트합니다. 프레임당 한 번 이상이 메서드를 호출 해야 하 고 게임 메서드에 의해 반환 되는 멀티 플레이어 이벤트를 처리 해야 합니다. |
| JoinLobby() | join_lobby() | 친구의 로비 세션 로비 고유 하 게 식별 하는 handleId 가입 하려는 또는 사용자가 프로토콜 활성화를 가져오려면 제목 초대를 수락 하는 경우에 조인 하는 방법을 제공 합니다. |
| JoinGameFromLobby() | join_game_from_lobby() | 그리고 공간이 있는 경우 로비의 게임 세션에 참가 합니다. 세션 존재 하지 않는 경우 기존 로비 멤버를 사용 하 여 새 게임 세션을 만듭니다. 이 마이그레이션되지 않습니다 기존 로비 세션 속성을 통해 게임 세션에. 가입 후에 속성 또는 통해 호스트를 설정할 수 있습니다 `game_session()::set_synchronized_*` Api입니다. 제목은 게임 세션에 참가 하고자 하는 모든 클라이언트에서이 API를 호출 해야 합니다.|
| JoinGame() | join_game() | 제 3 자 매치 메이 킹 서비스를 통해 일반적으로 발견 된 고유한 globally 세션 이름이 지정 된 기존 게임 세션을 결합 합니다. Xbox 사용자 Id 게임의 일부가 될 목록에 전달할 수 있습니다.|
| FindMatch() | find_match() | 매치 메이 킹 Xbox Live를 찾아 게임에 사용 합니다. |
| LeaveGame() | leave_game() | 게임 에서만 유지 하 고 대기실 멤버 및 모든 로컬 멤버를 반환 합니다. |
| **특성** | | |
| LobbySession | lobby_session() | 핸들 로비 세션을 나타내는 개체입니다. |
| GameSession |  game_session() | 게임 세션을 나타내는 개체 핸들입니다. |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>멀티 플레이어 이벤트 클래스<a name="multiplayer-event-class">

| language | 클래스 |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

호출 하는 경우 `do_work()`, 마지막으로 호출 되므로 변경 내용을 세션을 나타내는 이벤트는 목록을 반환 하는 멀티 플레이어 관리자 `do_work()`. 이러한 이벤트 구성원 세션에 참가 같은 변경 사항을 포함, 구성원 세션 떠난 멤버 속성이 변경, 호스트 클라이언트 변경 된 등.

모든 가능한 이벤트 형식 목록은 참조는 `multiplayer_event_type` 열거 합니다.

반환 된 `multiplayer_event` 포함 된 `event_args`, 이벤트 형식에 대 한 적절 한 event_args 클래스에 캐스팅 해야 하는. 예를 들어 경우는 `event_type` 는 `member_joined`를 캐스팅 하는 다음는 `event_args` 에 대 한 참조는 `member_joined_event_args` 클래스.

게임 호출한 후 각 필요에 따라 이벤트를 처리 해야 `do_work()`.

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>멀티 플레이어 멤버 클래스<a name="multiplayer-member-class">

| language | 클래스 |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

이 클래스는 로비 또는 게임 세션에서 플레이어를 나타냅니다. 클래스 멤버, 플레이어, 플레이어에 대 한 네트워크 연결 주소, 사용자 지정 속성은 XboxUserID를 포함 하 여 각 플레이어 등에 대 한 속성을 포함 합니다.

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>멀티 플레이어 로비 세션 클래스<a name="multiplayer-lobby-session-class">

| language | 클래스 |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

영구 세션에이 장치와 함께 재생 하고자 하는 친구 초대 받은 사용자 관리에 사용 합니다. 로비 세션 멀티 플레이어 관리자 멀티 플레이 작업을 수행할 멤버를 하나 이상 포함 해야 합니다. 호출 하 여 새 로비 세션을 처음 만들 수는 `add_local_user()` 메서드.

다음 표에서 일부를 설명 하지만이 클래스에 대 한 메서드 및 속성 중 더 일반적으로 사용 합니다. 클래스 멤버의 전체는 설명이 포함 된 목록에 대 한 참조를 참조 하세요.

| C++/CX | C++ | description |
| --- | --- | --- |
| **메서드** | | |
| AddLocalUser() | add_local_user() | 로비 세션에 로컬 사용자 (로컬 장치에 로그인 하는 한 명)을 추가 합니다. 로비 세션에 추가 된 첫 번째 구성원 인 새 로비 세션을 만듭니다 합니다. |
| RemoveLocalUser() | remove_local_user() | 로비 및 게임 세션에서 지정된 된 구성원을 제거합니다. |
| InviteFriends() | invite_friends() | 표준 Xbox Live UI 수 플레이어가, 친구 목록에서 사용자를 선택 하 고 다음 해당 플레이어의 게임 초대를 엽니다. |
| InviteUsers() | invite_users() | 게임에 sepcified Xbox Live 사용자를 초대합니다. |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | 로컬 멤버에 대 한 네트워크 주소를 설정합니다. 게임 멤버 간 네트워크 통신을 설정 하려면이 네트워크 주소를 사용할 수 있습니다. |
| SetLocalMemberProperties() | set_local_member_properties() | 로컬 멤버에 대 한 사용자 지정 속성을 설정합니다. 속성은 JSON 문자열에 저장 됩니다. |
| DeleteLocalMemberProperties() | delete_local_member_properties() | 로컬 멤버에 대 한 사용자 지정 속성을 제거합니다. |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 로비 세션에 대 한 사용자 지정 속성을 설정합니다. 속성은 JSON 문자열에 저장 됩니다. 속성은 장치 간에 공유을 동시에 여러 장치에서 업데이트 될 수 있습니다 동기화 된 버전의 메서드를 사용 합니다. |
| IsHost() | is_host() | 하는 경우 현재 장치 역할을 하는 로비 호스트를 나타냅니다. |
| SetSynchronizedHost() | set_synchronized_host() | 로비의 호스트를 설정합니다. |
| **특성** | | |
| LocalMembers | local_members() | 로컬 장치에 로그인 하는 멤버의 컬렉션입니다. |
| 멤버 | members() | 로비 세션에 있는 구성원의 컬렉션입니다. |
| 특성 | properties() | 로비 세션에 대 한 속성 컬렉션을 표시 하는 JSON 개체입니다. |
| 호스트 | host) | 로비에 대 한 호스트 멤버입니다. |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>멀티 플레이어 게임 세션 클래스<a name="multiplayer-game-session-class">

| language | 클래스 |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

게임 세션 인스턴스의 실제 게임에 참여 하는 Xbox Live 멤버의 그룹을 나타냅니다. 이 매치 메이 킹 서비스를 통해 일치 하는 플레이어를 포함할 수 있습니다.

구성원을 포함 하는 새 게임 세션을 시작 하는 `lobby_session`를 호출할 수 있습니다 `multiplayer_manager::join_game_from_lobby()`. 매치 메이 킹 Xbox Live를 사용 하려는 경우 호출할 수 있습니다 `multiplayer_manager::find_match()`. 제 3 자 매치 메이 킹 서비스를 사용 하는 경우 호출할 수 있습니다 `multiplayer_manager::join_game()`.

다음 표에서 일부를 설명 하지만이 클래스에 대 한 메서드 및 속성 중 더 일반적으로 사용 합니다. 클래스 멤버의 전체는 설명이 포함 된 목록에 대 한 참조를 참조 하세요.

| C++/CX | C++ | description |
| --- | --- | --- |
| **메서드** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 게임 세션에 대 한 사용자 지정 속성을 설정합니다. 속성은 JSON 문자열에 저장 됩니다. 속성은 장치 간에 공유을 동시에 여러 장치에서 업데이트 될 수 있습니다 동기화 된 버전의 메서드를 사용 합니다. |
| IsHost() | is_host() | 하는 경우 현재 장치 역할을 하는 게임 호스트를 나타냅니다. |
| SetSynchronizedHost() | set_synchronized_host() | 게임의 호스트를 설정합니다. |
| **특성** | | |
| 멤버 | members() | 게임 세션에 있는 구성원의 컬렉션입니다. |
| 특성 | properties() | 게임 세션에 대 한 속성 컬렉션을 표시 하는 JSON 개체입니다. |
| 호스트 | host) | 게임에 대 한 호스트 멤버입니다. |
