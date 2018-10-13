---
title: 스마트를 사용 하 여 멀티 플레이 게임
author: KevinAsgari
description: Xbox Live 멀티 플레이어 관리자를 사용 하 여 플레이어가 스마트를 사용 하 여 멀티 플레이어 게임을 찾을 수 있도록 하는 방법을 알아봅니다.
ms.assetid: f9001364-214f-4ba0-a0a6-0f3be0b2f523
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어, 멀티 플레이어 관리자 순서도, 스마트
ms.localizationpriority: medium
ms.openlocfilehash: 3bc23c73df1bdd8a030165ae623fc59c036104bd
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4572398"
---
# <a name="find-a-multiplayer-game-by-using-smartmatch"></a>스마트를 사용 하 여 멀티 플레이어 게임을 찾기

경우에 따라 게이머는 없을 수 있습니다 충분 한 친구 온라인 게임을 플레이 하려는 하거나 온라인 임의 사용자가 플레이 하 려 합니다. Xbox Live 스마트 서비스를 사용 하 여 다른 Xbox Live 플레이어를 찾을 수 있습니다.

이 페이지에서는 스마트 매치 메이 킹 멀티 플레이어 관리자를 사용 하 여 구현 해야 하는 기본 단계를 설명 합니다.

## <a name="find-a-match"></a>일치를 찾지

네 가지 단계가 있습니다 관련 된 사용자의 친구 이동한 다음 진행 중인 게임에 참가 하는 친구를 초대 하는 멀티 플레이어 관리자를 사용 하는 경우.

1. [멀티 플레이어 관리자 초기화](#initialize-multiplayer-manager)
2. [로컬 사용자를 추가 하 여 로비 세션 만들기](#create-lobby)
3. [친구 초대 보내기](#send-invites)
4. [초대를 수락 합니다.](#accept-invites)
5. [일치를 찾지](#find-match)

1, 2, 3, 5 단계 초대를 수행 하는 장치에서 수행 됩니다.  4 단계는 일반적으로 프로토콜 활성화를 통해 앱 실행에 따라 초대 대의 컴퓨터에서 시작할 수 있습니다.

다음 프로세스의 순서도 표시: [순서도-스마트 매치를 사용 하 여 멀티 플레이어 게임을 플레이](mpm-flowcharts/mpm-play-with-smartmatch-matchmaking.md)합니다.

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>1) 멀티 플레이어 관리자를 초기화 합니다.<a name="initialize-multiplayer-manager">

| 전화 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | 해당 없음 |

로비 세션 개체 (구성 서비스 구성에서) 유효한 세션 템플릿 이름 지정 한다고 가정 하는 멀티 플레이어 관리자 초기화 시 자동으로 만들어집니다. Note이 서비스에서 로비 세션 인스턴스를 만들고 하지는 않습니다.

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

로컬로 서명 된 추가 여기서 로비 세션에 사용자가 Xbox Live. 이 첫 번째 사용자가 추가 하는 경우 새 로비를 호스팅합니다. 다른 모든 사용자에 대 한 추가 됩니다 기존 대기실 보조 사용자로 합니다. 이 API에 참가 하도록 친구에 대 한 셸 로비를 보급 됩니다. 초대 보내기, 로비 속성을 설정 하, 로컬 사용자를 추가 하 고 나면에 lobby() 통해 로비 멤버에 액세스할 수 있습니다.

가입 후 로비에 멤버에 대 한 로컬 멤버 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다.

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


다음에 변경 내용을 일괄 처리할 `do_work()` 를 호출 합니다.  
멀티 플레이어 관리자 발생 한 `user_added` 이벤트는 사용자가 로비 세션에 추가 될 때마다 합니다. 것이 좋습니다 해당 사용자가 성공적으로 추가 된 이벤트의 오류 코드를 검사 합니다. 오류를 발생 시 실패 이유를 자세히 오류 메시지가 제공 됩니다.

**멀티 플레이어 관리자가 수행**

* Xbox Live 멀티 플레이 서비스를 사용 하 여 실시간으로 활동 및 멀티 플레이어 구독을 등록
* 로비 세션 만들기
* 활성 로컬 모든 플레이어에 가입
* SDA를 업로드 합니다.
* 멤버 속성 설정
* 세션 변경 이벤트에 대 한 등록
* 활성 세션 로비 세션을 설정 합니다.

### <a name="3-send-invites-to-friends-optional-a-namesend-invites"></a>3) 보내기 (선택 사항) 친구 초대<a name="send-invites">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

다음으로, 친구 초대를 위한 표준 Xbox UI를 표시 합니다. 친구 선택 플레이어나 최근 플레이어 게임에 초대할 수 있도록 UI를 표시 합니다. 플레이어 수를 확인 되 면 멀티 플레이어 관리자 선택한 플레이어의 초대를 보냅니다.

게임도 사용할 수는 `invite_users()` Xbox Live 사용자 Id가 정의한 사용자 집합에 대 한 보내는 방법을 초대 합니다. Xbox UI 주식 대신 고유한 게임에서 UI를 사용 하려는 경우 유용 합니다.

**예제:**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**멀티 플레이어 관리자가 수행**

* 표시는 Xbox를 주식 제목 호출할 UI (TCUI)
* 선택한 플레이어에 직접 초대 보내기

### <a name="4-accept-invites-optional-a-nameaccept-invites"></a>4) (선택 사항) 초대를 수락 합니다.<a name="accept-invites">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

초대 받은 플레이어가 게임 초대를 수락 하거나 셸 UI 통해 친구 게임 가입을 프로토콜 활성화를 사용 하 여 디바이스에서 게임을 시작 합니다. 한 번 게임 시작 멀티 플레이어 관리자 수를 사용 하 여 프로토콜 활성화 이벤트 인수 로비 합니다. 필요에 따라 lobby_session()::add_local_user() 통해 로컬 사용자를 추가 하지 않은 경우 전달할 수 사용자 목록 join_lobby() API 통해 합니다. 초대 받은 사용자는 lobby_session()::add_local_user()를 통해 또는 join_lobby()를 통해 추가 하지 join_lobby()가 실패 하 고는 join_lobby_completed_event_args()의 일환으로 초대 받은 invited_xbox_user_id()를 제공 합니다.

가입 후 로비에 멤버에 대 한 로컬 멤버 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다. 존재 하지 않는 경우에 set_synchronized_host 통해 호스트를 설정할 수 있습니다.

마지막으로, 멀티 플레이어 관리자 게임 이미 진행 중인과 피어에 대 한 공간이 가입 게임 세션에 사용자를 자동 됩니다. 제목 적절 한 오류 코드와 메시지를 제공 하는 join_game_completed 이벤트를 통해 알림을 받게 됩니다.

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

**멀티 플레이어 관리자가 수행**

* RTA 및 멀티 플레이 구독을 등록 합니다.
* 로비 세션에 참가
 * 기존 로비 상태 정리
 * 활성 로컬 모든 플레이어에 가입
 * SDA를 업로드 합니다.
 * 멤버 속성 설정
* 세션 변경 이벤트에 대 한 등록
* 활성 세션 로비 세션을 설정 합니다.
* 게임 세션에 참가 (경우 존재)
 * 사용 하 여 전송 핸들


### <a name="5-find-match-a-namefind-match"></a>5) 일치를 찾기<a name="find-match">

| 전화 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_manager::find_match()` | 많은 이벤트에 설명 된 대로 ```match_status``` (예: ```searching```, ```found```, ```measuring```등) |

초대를 후, 받았음을 호스트는 게임을 시작할 준비가 경우 스마트는 데 사용할 수 충분 한 플레이어를 사용 하는 기존 게임 슬롯의 모든 구성원에 대 한 로비 세션에서 호출 하 여 두 찾기 `find_match()`, 또는 새 게임 세션 만들기  호출 하 여 동일한 게임 유형의 일치를 찾고 다른 사용자와의 모든 멤버 로비 세션 및 채우기 열기 지점 구성에서 포함 된 `join_game_from_lobby()` 아래에 `set_auto_fill_members_during_matchmaking()`.

호출 하기 전에 `find_match()`, 서비스 구성에 hoppers를 먼저 구성 해야 합니다. hopper 스마트 플레이어를 사용 하 여 규칙을 정의 합니다.

**예제:**

```cpp
auto result = mpInstance.find_match(HOPPER_NAME);
if (result.err())
{
  // handle error
}
```

**멀티 플레이어 관리자가 수행**

* 일치 티켓 만들기
* 모든 QoS 단계를 처리 합니다.
* 명단 변경 처리
 * (필요한 경우)을 다시 제출
* 조인 대상 게임 세션
* 게임 로비 세션을 통해 광으십시오
