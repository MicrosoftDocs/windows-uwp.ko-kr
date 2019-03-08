---
title: 게임 초대 보내기
description: 멀티 플레이 관리자 Xbox Live를 사용 하 여 플레이어가 게임 초대를 보낼 수 있도록 하는 방법에 알아봅니다.
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이 게임, 멀티 플레이 manager, 순서도, 게임 초대
ms.localizationpriority: medium
ms.openlocfilehash: 85aa45558d1443638ba7dd50dbea8923125ef664
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645998"
---
# <a name="send-game-invites"></a>게임 초대 보내기

간단한 멀티 플레이어 시나리오 중 하나는 게이머 친구 들과 온라인 게임을 재생할 수 있도록 합니다. 이 시나리오에서는 게임에 다른 플레이어에 게 초대를 전송 하는 데 필요한 단계를 설명 합니다.

했으면 [멀티 플레이 게임 관리자를 초기화](play-multiplayer-with-friends.md), 있고 [로컬 사용자를 추가 하 여 로비 세션을 만들었습니다](play-multiplayer-with-friends.md)를 받을 때까지 대기 해야 합니다는 `user_added` 초대를 전송 하기 전에 이벤트.

두 가지 방법으로 초대를 보낼 수 있습니다.

1. [Xbox 플랫폼 초대 TCUI](#xbox-platform-invite-tcui)
2. [제목은 사용자 지정 UI 구현](#title-implemented-custom-ui)

프로세스의 순서도 확인할 수 있습니다. [순서도 – 다른 플레이어에 게 초대 보내기](mpm-flowcharts/mpm-send-invites.md)합니다.

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>1) Xbox 플랫폼 초대 TCUI <a name="xbox-platform-invite-tcui">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

호출 `invite_friends()` 초대 친구 들을 위한 표준 Xbox UI를 표시 합니다. 친구를 선택 하려면 플레이어 또는 최근 플레이어가 게임에 초대할 수 있는 UI가 표시 됩니다. 플레이어 적중 확인 되 면 다중 접속 관리자가 선택한 플레이어 초대를 보냅니다.

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

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>2) 제목 사용자 지정 UI 구현<a name="title-implemented-custom-ui">

| 메서드 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

제목 온라인 친구 보기 및 초대 하는 사용자 지정 TCUI를 구현할 수 있습니다. 게임 사용할 수는 `invite_users()` 보내기 위한 메서드를 해당 Xbox Live 사용자 Id로 정의 된 사용자 집합에 초대 합니다. 주식 Xbox UI 대신 게임 내 UI를 직접 사용 하려는 경우에 유용 합니다.

**예:**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**다중 접속 관리자가 수행 하는 함수**

* 선택한 플레이어에 직접 보내는 초대
