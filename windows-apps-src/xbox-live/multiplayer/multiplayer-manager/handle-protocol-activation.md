---
title: 프로토콜 활성화 처리
description: Xbox Live 멀티 플레이어 관리자 프로토콜 활성화 처리를 사용 하는 방법을 알아봅니다.
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어 관리자, 프로토콜 활성화
ms.localizationpriority: medium
ms.openlocfilehash: 0b5dead742e18bbf5f3e9c271109352ae48e8fef
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8926944"
---
# <a name="handle-protocol-activation"></a>프로토콜 활성화 처리

프로토콜 활성화 시스템이 자동으로 시작 되 면 게임 다른 작업에 대 한 응답에서 일반적으로 플레이어가 다른 플레이어에서 게임 초대를 수락 하면 됩니다.

타이틀 다음을 통해 정품 인증 프로토콜을 가져올 수 있습니다.

* 사용자가 게임 초대를 수락 하는 경우
* 때 사용자는 플레이어의 gamercard에서 "게임 가입"를 선택 합니다.

이 시나리오는 타이틀을 시작할 때 프로토콜 활성화를 처리 하 고 (있는 경우) 로비 및 게임에 참가 하는 방법을 설명 합니다.

다음 프로세스의 순서도 피: [순서도-핸들 프로토콜 활성화 플레이어](mpm-flowcharts/mpm-on-protocol-activation.md)합니다.

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

플레이어가 게임 초대를 수락 또는 플레이어의 gamercard 통해 친구의 게임에 가입 하는 경우 프로토콜 활성화를 사용 하 여 디바이스에서 게임을 시작 합니다. 한 번 게임 시작 멀티 플레이어 관리자 수를 사용 하 여 프로토콜 활성화 이벤트 인수 로비 합니다. 필요에 따라를 통해 로컬 사용자를 추가 하지 않은 경우 `lobby_session()::add_local_user()`를 통해 사용자가 목록에 전달할 수는 `join_lobby()` API입니다. 초대 추가 된 사용자 보다 다른 사용자에 대 한 경우 또는 초대 받은 사용자 추가 되지 않은 경우 `join_lobby()` 실패 하 고 제공는 `invited_xbox_user_id()` 의 일부로 초대 받은 `join_lobby_completed_event_args`합니다.

가입 후 로비에 멤버에 대 한 로컬 멤버의 연결 주소 뿐만 아니라 모든 사용자 지정 속성을 설정 하는 것이 좋습니다. 호스트를 통해 설정할 수 있습니다 `set_synchronized_host` 존재 하지 않는 경우.

마지막으로, 멀티 플레이어 관리자 게임 이미 진행 중에서에 있는 경우는 피어에 대 한 공간이 가입 게임 세션에 사용자를 자동 됩니다. 제목 통해 알려주지는 `join_game_completed` 이벤트는 적절 한 오류 코드와 메시지를 제공 합니다.

**예제:**

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

오류/성공을 통해 처리 되는 `join_lobby_completed` 이벤트

**멀티 플레이어 관리자가 수행**

* RTA 및 멀티 플레이 구독 등록
* 로비 세션에 참가
 * 기존 로비 상태 정리
 * 활성 로컬 모든 플레이어에 가입
 * SDA를 업로드 합니다.
 * 멤버 속성 설정
* 세션 변경 이벤트에 대 한 등록
* 활성 세션 로비 세션을 설정 합니다.
* 게임 세션에 참가 (경우 존재)
 * 사용 하 여 전송 핸들
