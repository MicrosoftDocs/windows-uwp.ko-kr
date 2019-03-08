---
title: SmartMatch를 사용 하 여 멀티 플레이어 게임
description: 멀티 플레이 관리자 Xbox Live를 사용 하 여 플레이어가 SmartMatch를 사용 하 여 멀티 플레이 게임을 찾을 수 있도록 하는 방법에 알아봅니다.
ms.assetid: f9001364-214f-4ba0-a0a6-0f3be0b2f523
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이 게임, 멀티 플레이 manager, 순서도, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 0c0ba897f23eb690c3044b00cdb4ce3bea975e71
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655958"
---
# <a name="find-a-multiplayer-game-by-using-smartmatch"></a>SmartMatch를 사용 하 여 멀티 플레이 게임을 찾기

경우에 따라 퇴근 없을 충분 한 친구 온라인 게임을 것인지 또는 임의 사용자 온라인에 대 한 재생 하고자 하는 경우. Xbox Live SmartMatch 서비스를 사용 하 여 다른 Xbox Live 플레이어를 찾을 수

이 페이지에서는 다중 접속 관리자를 사용 하 여 SmartMatch 결혼 정보 회사 연결을 구현 하는 데 필요한 기본 단계를 설명 합니다.

## <a name="find-a-match"></a>일치 하는 것

4 단계와 관련 된 사용자의 friend 차례로 진행에서 게임에 참가 하려면 해당 friend에 대 한 초대를 보낼 멀티 플레이 게임 관리자를 사용 하는 경우

1. [다중 접속 관리자 초기화](#initialize-multiplayer-manager)
2. [로컬 사용자를 추가 하 여 로비 세션 만들기](#create-lobby)
3. [친구 초대 보내기](#send-invites)
4. [초대를 수락 합니다.](#accept-invites)
5. [일치 하는 것](#find-match)

1, 2, 3 및 5 단계는 초대를 수행 하는 장치에서 수행 됩니다.  4 단계는 일반적으로 프로토콜 활성화를 통해 앱 시작을 수행 하는 초대 대 상자의 컴퓨터에서 시작할 수 있습니다.

프로세스의 순서도 확인할 수 있습니다. [순서도 – SmartMatch 결혼 정보 회사 연결을 사용 하 여 멀티 플레이 게임을 플레이](mpm-flowcharts/mpm-play-with-smartmatch-matchmaking.md)합니다.

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>1)-멀티 플레이 게임 관리자를 초기화 하는 중 <a name="initialize-multiplayer-manager">

| 호출 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | 해당 없음 |

로비 세션 개체 (서비스 구성에서 구성 됨)를 유효한 세션 템플릿 이름이 지정 되어 있는지 가정 하 고 다중 접속 관리자를 초기화 시 자동으로 만들어집니다. 만들어지지 않습니다 로비 세션 인스턴스 서비스에서 note 합니다.

**예:**

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

로컬로 서명 된 추가한 여기서 로비 세션 사용자가 Xbox Live에서. 이 첫 번째 사용자를 추가할 때 새 로비를 호스팅합니다. 다른 모든 사용자에 대해서는 추가할 기존 로비 보조 사용자로 합니다. 이 API는 친구 들이 가입할 셸에서 로비를 제시할 수도 됩니다. 초대를 보낼 하, 로비 속성을 설정, 로컬 사용자를 추가한 다음 한 번만 lobby() 통해 로비 멤버에 액세스할 수 있습니다.

로비 조인한 후 멤버에 대 한 로컬 멤버의 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다.

모든 로컬 사용자를 로그인에 대해이 프로세스를 반복 해야 합니다.

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
    xboxLiveContext->user(), connectionAddress);

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


다음에는 변경 내용이 일괄 처리 `do_work()` 호출 합니다.  
멀티 플레이 manager 발생을 `user_added` 사용자 로비 세션에 추가 될 때마다 이벤트입니다. 해당 권장 되는 경우 해당 사용자가 성공적으로 추가 하는 경우 이벤트의 오류 코드를 조사 하는 합니다. 오류를 발생 시 실패의 이유를 자세히 보여 주는 오류 메시지가 제공 됩니다.

**다중 접속 관리자가 수행 하는 함수**

* Xbox Live 멀티 플레이 서비스를 사용 하 여 실시간으로 작업 및 다중 접속 구독 등록
* 로비 세션 만들기
* 활성으로 모든 로컬 플레이어를 조인 합니다.
* SDA를 업로드 합니다.
* 멤버 속성 설정
* 세션 변경 이벤트에 등록
* 현재 세션으로 로비 세션 설정

### <a name="3-send-invites-to-friends-optional-a-namesend-invites"></a>(선택 사항) 친구를 초대합니다. 3) 보내기 <a name="send-invites">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

다음으로 초대 친구 들을 위한 표준 Xbox UI를 표시 해야 합니다. 친구를 선택 하려면 플레이어 또는 최근 플레이어가 게임에 초대할 수 있는 UI가 표시 됩니다. 플레이어 적중 확인 되 면 다중 접속 관리자가 선택한 플레이어 초대를 보냅니다.

게임을 이용할 수 있습니다는 `invite_users()` 보내기 위한 메서드를 해당 Xbox Live 사용자 Id로 정의 된 사용자 집합에 초대 합니다. 주식 Xbox UI 대신 게임 내 UI를 직접 사용 하려는 경우에 유용 합니다.

**예:**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**다중 접속 관리자가 수행 하는 함수**

* Xbox 등록 재고 제목 호출 가능 UI (TCUI)
* 선택한 플레이어에 직접 보내는 초대

### <a name="4-accept-invites-optional-a-nameaccept-invites"></a>4)-(선택 사항) 초대를 수락 하는 중 <a name="accept-invites">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

초대 된 플레이어 게임 초대를 수락 또는 UI 셸을 통해 친구 들과 게임을 조인 하는 경우 프로토콜 활성화를 사용 하 여 자신의 장치에 게임을 시작 합니다. 한 번 게임 시작 멀티 플레이 관리자 인수를 사용 프로토콜 활성화 이벤트 로비로 합니다. 필요에 따라 lobby_session()::add_local_user() 통해 로컬 사용자를 추가 하지 않았다면, 전달할 수 있습니다 사용자 목록의 join_lobby() API 통해. 초대 된 사용자 lobby_session()::add_local_user() 통하거나 join_lobby()를 통해 추가 되지 join_lobby() 실패 하 고는 join_lobby_completed_event_args() 일부로 대 한 초대 보낸 invited_xbox_user_id() 제공 됩니다 경우.

로비 조인한 후 멤버에 대 한 로컬 멤버의 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다. 없는 경우에 set_synchronized_host 통해 호스트를 설정할 수 있습니다.

마지막으로, 멀티 플레이 게임 관리자 게임 진행에서 이미 있고 초대 대 상자에 대 한 공간이 경우 조인 게임 세션으로 사용자 자동 됩니다. 제목에 적절 한 오류 코드 및 메시지 제공 join_game_completed 이벤트를 통해 알림을 받게 됩니다.

**예:**

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

**다중 접속 관리자가 수행 하는 함수**

* RTA 멀티 플레이 구독 등록
* 로비 세션에 참가
 * 기존 로비 상태 정리
 * 활성으로 모든 로컬 플레이어를 조인 합니다.
 * SDA를 업로드 합니다.
 * 멤버 속성 설정
* 세션 변경 이벤트에 등록
* 현재 세션으로 로비 세션 설정
* 게임 세션에 참가 (하는 경우 존재)
 * 사용 하 여 전송 핸들


### <a name="5-find-match-a-namefind-match"></a>5)-일치 항목을 찾을 하는 중 <a name="find-match">

| 호출 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_manager::find_match()` | 대부분의 이벤트에 설명 된 대로 ```match_status``` (예: ```searching```를 ```found```, ```measuring```등) |

후 초대를 호스트는 게임을 시작할 준비가 있는 수락 된 경우 사용할 수 있습니다 SmartMatch 충분 한 플레이어에 있는 기존 게임을 슬롯의 모든 멤버에 대 한 로비 세션에서 호출 하 여 두 찾을 `find_match()`를 만들거나는 새 게임 se 호출 하 여 동일한 게임 형식의 일치 항목을 찾는 다른 사용자와 모든 로비 세션에서 열려 스폿 위쪽으로 채우기 멤버를 포함 하는 세션 `join_game_from_lobby()` 와 함께 표시 `set_auto_fill_members_during_matchmaking()`합니다.

호출 하기 전에 `find_match()`, 서비스 구성에서 hoppers를 먼저 구성 해야 합니다. hopper SmartMatch 플레이어를 일치 시키는 데 사용 되는 규칙을 정의 합니다.

**예:**

```cpp
auto result = mpInstance.find_match(HOPPER_NAME);
if (result.err())
{
  // handle error
}
```

**다중 접속 관리자가 수행 하는 함수**

* 일치 티켓 만들기
* 모든 QoS 단계 처리
* 명단 변경 내용을 처리합니다
 * (필요한 경우)을 다시 제출
* 조인 게임 세션 대상
* 게임 로비 세션을 통해 보급
