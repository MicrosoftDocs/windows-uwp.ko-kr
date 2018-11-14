---
title: 친구와 멀티 플레이 게임
author: KevinAsgari
description: Xbox Live 멀티 플레이어 관리자를 사용 하 여 친구와 멀티 플레이 게임을 플레이 하는 플레이어 수 있도록 하는 방법을 알아봅니다.
ms.assetid: 6eefee0e-6c0d-473a-97e7-f3e45f574712
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어, 멀티 플레이어 관리자, 순서도
ms.localizationpriority: medium
ms.openlocfilehash: 8af1979a92821a62a625ea9d637a7dbcf9a59edb
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6194370"
---
# <a name="play-a-multiplayer-game-with-friends"></a>친구와 멀티 플레이 게임

간단한 멀티 플레이어 시나리오 중 하나는 게이머가 친구와 온라인 게임을 플레이할 수 있도록 합니다. 이 시나리오는 멀티 플레이어 관리자를 사용 하 여 구현 해야 하는 기본 단계를 설명 합니다.

## <a name="inviting-friends"></a>친구 초대

네 가지 단계가 있습니다 관련 된 사용자의 친구가 이동한 다음 진행 중인 게임에 참가 하려면 해당 친구를 초대 하는 멀티 플레이어 관리자를 사용 하는 경우.

1. [멀티 플레이어 관리자 초기화](#initialize-multiplayer-manager)
2. [로컬 사용자를 추가 하 여 로비 세션 만들기](#create-lobby)
3. [친구 초대 보내기](#send-invites)
4. [초대를 수락 합니다.](#accept-invites)
5. [로비에서 게임 세션에 참가](#join-game)

1, 2, 3 일 및 5 단계는 초대를 수행 하는 장치에서 수행 됩니다.  4 단계는 일반적으로 프로토콜 활성화를 통해 앱 실행 다음 초대 대의 컴퓨터에서 시작할 수 있습니다.

여기에 프로세스의 순서도 피: [순서도-친구와 멀티 플레이/협동 게임을 실행](mpm-flowcharts/mpm-play-with-friends.md)합니다.

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>1) 멀티 플레이어 관리자를 초기화 합니다.<a name="initialize-multiplayer-manager">

| 전화 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | 해당 없음 |

대기실 세션 개체 (구성 서비스 구성에서) 유효한 세션 템플릿 이름이 지정 된 것으로 가정 멀티 플레이어 관리자는 초기화 시 자동으로 만들어집니다. Note는 만들어지지 않습니다 로비 세션 인스턴스를 서비스 합니다.

**예제:**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

mpInstance->initialize(lobbySessionTemplateName);
```


### <a name="2-create-the-lobby-session-by-adding-local-usersa-namecreate-lobby"></a>2) 로컬 사용자를 추가 하 여 로비 세션 만들기<a name="create-lobby">

| 메서드 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_lobby_session::add_local_user()` | `user_added_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

로컬로 서명 된 추가 여기서 로비 세션에 사용자가 Xbox Live. 이 첫 번째 사용자가 추가 되 면 새 로비를 호스팅합니다. 다른 모든 사용자에 대 한 추가 됩니다 기존 대기실으로 보조 사용자로 합니다. 이 API에 참가 하도록 친구에 대 한 셸 로비를 보급 됩니다. 초대 보내기, 로비 속성을 설정 하, 로컬 사용자를 추가 하 고 나면에 lobby() 통해 로비 멤버에 액세스할 수 있습니다.

가입 후 로비에 멤버에 대 한 로컬 멤버의 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다.

모든 로컬 사용자가 로그인에 대 한이 프로세스를 반복 해야 합니다.

**예: (단일 로컬 사용자)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

auto result = mpInstance->lobby_session()->add_local_user(xboxLiveContext->user());
if (result.err())
{
  // handle error
}

// Set member connection address
string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);

// Set custom member properties
mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
```

**예: (여러 로컬 사용자)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();
string_t connectionAddress = L"1.1.1.1";

for (User^ user : User::Users)
{
  if( user->IsSignedIn )
  {
    auto result = mpInstance.lobby_session()->add_local_user(user);
    if (result.err())
    {
      // handle error
    }

    // Set member connection address
    mpInstance->lobby_session()->set_local_member_connection_address(
        xboxLiveContext->user(), connectionAddress);

    // Set custom member properties
    mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
  }
}

```


다음에 변경 내용을 일괄 처리할 `do_work()` 를 호출 합니다.  
멀티 플레이어 관리자 발생 한 `user_added` 이벤트는 사용자가 로비 세션에 추가 될 때마다 합니다. 해당 권장 해당 사용자가 성공적으로 추가 된 경우 확인 하려면 이벤트의 오류 코드를 검사 합니다. 오류가 발생 하는 경우 오류가 발생 하는 이유를 자세히 설명 오류 메시지가 제공 됩니다.

**멀티 플레이어 관리자가 수행 하는 기능**

* Xbox Live 멀티 플레이 서비스를 사용 하 여 실시간으로 활동 및 멀티 플레이어 구독 등록
* 대기실 세션 만들기
* 활성으로 모든 로컬 플레이어가 참가
* SDA를 업로드 합니다.
* 멤버 속성 설정
* 세션 변경 이벤트에 대 한 등록
* 활성 세션 로비 세션을 설정 합니다.

### <a name="3-send-invites-to-friends-a-namesend-invites"></a>친구 초대 보내기 3)<a name="send-invites">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

다음으로, 친구 초대에 대 한 표준 Xbox UI를 불러오는 데 싶을 것입니다. 친구 선택 플레이어나 최근 플레이어 게임에 초대할 수 있도록 UI를 표시 합니다. 플레이어 적중 확인 되 면 멀티 플레이어 관리자 선택한 플레이어의 초대를 보냅니다.

게임도 사용할 수는 `invite_users()` Xbox Live 사용자 Id가 정의한 사용자 집합에 대 한 보내는 방법을 초대 합니다. Xbox UI 주식 대신 고유한 게임에서 UI를 사용 하려는 경우에 유용 합니다.

**예제:**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**멀티 플레이어 관리자가 수행 하는 기능**

* 표시는 Xbox를 주식 제목 호출할 UI (TCUI)
* 선택한 플레이어에 직접 초대 보내기

### <a name="4-accept-invites-a-nameaccept-invites"></a>4) 초대를 수락 합니다.<a name="accept-invites">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

초대 받은 플레이어가 게임 초대를 수락 또는 셸 UI 통해 친구 게임 가입 하는 경우 게임 프로토콜 활성화를 사용 하 여 디바이스에서 시작 됩니다. 한 번 게임 시작 멀티 플레이어 관리자 수를 사용 하 여 프로토콜 활성화 이벤트 인수 로비 합니다. 필요에 따라 lobby_session()::add_local_user() 통해 로컬 사용자를 추가 하지 않은 경우 전달할 수 사용자 목록 join_lobby() API 통해 합니다. 초대 받은 사용자 join_lobby()가 실패 하 고는 join_lobby_completed_event_args()의 일부로 대 한 초대 받은 invited_xbox_user_id() 제공 lobby_session()::add_local_user() 통해 또는 join_lobby()를 통해을 추가 되지 않습니다 하는 경우.

가입 후 로비에 멤버에 대 한 로컬 멤버의 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다. 존재 하지 않는 경우에 set_synchronized_host 통해 호스트를 설정할 수 있습니다.

마지막으로, 멀티 플레이어 관리자 게임 이미 진행 중인 하 고 회의실에 초대 받은 경우 가입 게임 세션에 사용자를 자동 됩니다. 제목 적절 한 오류 코드와 메시지를 제공 join_game_completed 이벤트를 통해 알림을 받게 됩니다.

**예제:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);
```

오류/성공 통해 처리 되는 `join_lobby_completed` 이벤트

**멀티 플레이어 관리자가 수행 하는 기능**

* RTA 및 멀티 플레이 구독 등록
* 대기실 세션에 참가
 * 기존 로비 상태 정리
 * 활성으로 모든 로컬 플레이어가 참가
 * SDA를 업로드 합니다.
 * 멤버 속성 설정
* 세션 변경 이벤트에 대 한 등록
* 활성 세션 로비 세션을 설정 합니다.
* 게임 세션에 참가 (경우 존재)
 * 사용 하 여 전송 핸들

### <a name="5-join-a-game-session-from-the-lobby-a-namejoin-game"></a>5) 로비에서 게임 세션에 참가<a name="join-game">

| 메서드 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_manager::join_game_from_lobby()` | `join_game_completed_event` |

초대를 수락한 후 호스트가 게임을 시작할 수 있는 로비 세션의 멤버를 호출 하 여 포함 된 새 게임을 시작할 수 있습니다 `join_game_from_lobby()`.

**예제:**

```cpp
auto result = mpInstance.join_game_from_lobby();
if (result.err())
{
  // handle error
}
```

오류/성공 통해 처리 됩니다 `join_game_completed` 이벤트

**멀티 플레이어 관리자가 수행 하는 기능**

* 게임 세션 만들기
 * 활성으로 모든 로컬 플레이어가 참가
 * SDA를 업로드 합니다.
 * 멤버 속성 설정
* 세션 변경 이벤트에 대 한 등록
* 대기실 세션을 통해 게임 광으십시오
