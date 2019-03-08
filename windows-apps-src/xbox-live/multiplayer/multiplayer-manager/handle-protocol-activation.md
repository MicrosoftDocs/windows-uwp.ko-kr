---
title: 프로토콜 활성화 처리
description: 멀티 플레이 manager Xbox Live를 사용 하 여 프로토콜 활성화를 처리 하는 방법에 알아봅니다.
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이 관리자, 프로토콜 활성화
ms.localizationpriority: medium
ms.openlocfilehash: 0b5dead742e18bbf5f3e9c271109352ae48e8fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655838"
---
# <a name="handle-protocol-activation"></a>프로토콜 활성화 처리

프로토콜 활성화 시스템 자동으로 시작 되 면 게임 다른 작업에 대 한 응답에서 일반적으로 플레이어에서 다른 플레이어 게임 초대를 수락 하는 경우는입니다.

제목에 다음을 통해 활성화 하는 프로토콜을 가져올 수 있습니다.

* 사용자가 게임 초대를 수락 하는 경우
* 때 사용자는 플레이어의 gamercard에서 "조인 Game"을 선택 합니다.

이 시나리오에서는 제목 시작 될 때 프로토콜 활성화를 처리 하 고 (있는 경우) 로비, 게임을 조인 하는 방법을 설명 합니다.

프로세스의 순서도 확인할 수 있습니다. [순서도-핸들 프로토콜 활성화 플레이어](mpm-flowcharts/mpm-on-protocol-activation.md)합니다.

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

플레이어가 게임 초대를 수락 또는 플레이어의 gamercard 통해 친구의 게임을 조인 하는 경우 프로토콜 활성화를 사용 하 여 자신의 장치에 게임을 시작 합니다. 한 번 게임 시작 멀티 플레이 관리자 인수를 사용 프로토콜 활성화 이벤트 로비로 합니다. 필요에 따라을 통해 로컬 사용자를 추가 하지 않았다면 `lobby_session()::add_local_user()`를 통해 사용자 목록에 전달할 수 있습니다는 `join_lobby()` API. 초대 된 사용자를 추가 하지 않은 경우 또는 추가 된 사용자 다른 사용자에 대 한 초대 되었으면 `join_lobby()` 실패 하 고 제공 됩니다 합니다 `invited_xbox_user_id()` 초대에 대 한의 일부로 보낸는 `join_lobby_completed_event_args`.

로비 조인한 후 멤버에 대 한 로컬 멤버의 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다. 통해 호스트를 설정할 수도 있습니다 `set_synchronized_host` 없는 경우.

마지막으로, 멀티 플레이 게임 관리자 게임 진행에서 이미 있고 초대 대 상자에 대 한 공간이 경우 조인 게임 세션으로 사용자 자동 됩니다. 제목을 통해 알림을 받게 됩니다는 `join_game_completed` 적절 한 오류 코드 및 메시지를 제공 하는 이벤트입니다.

**예:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args, users);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);
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
