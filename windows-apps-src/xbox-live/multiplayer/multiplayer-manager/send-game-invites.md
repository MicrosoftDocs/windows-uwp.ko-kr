---
title: 게임 초대 보내기
author: KevinAsgari
description: Xbox Live 멀티 플레이어 관리자를 사용 하 여 플레이어가 게임 초대를 보낼 수 있도록 하는 방법을 알아봅니다.
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어, 멀티 플레이어 관리자 순서도, 게임 초대
ms.localizationpriority: medium
ms.openlocfilehash: 512e0d33fb71d85486b1e12513977af250c431c6
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5828857"
---
# <a name="send-game-invites"></a>게임 초대 보내기

간단한 멀티 플레이어 시나리오 중 하나는 게이머가 친구와 온라인 게임을 플레이할 수 있도록 합니다. 이 시나리오는 게임에 다른 플레이어에 게 초대를 보내려면 필요한 단계를 다룹니다.

나타날 때까지 기다려야 [멀티 플레이어 관리자를 초기화](play-multiplayer-with-friends.md)한 하 고 [만든 로컬 사용자를 추가 하 여 로비 세션에](play-multiplayer-with-friends.md)는 `user_added` 초대 보내기 시작 하기 전에 이벤트입니다.

초대 하는 방법은 두 가지가 있습니다.

1. [Xbox 플랫폼 초대 TCUI](#xbox-platform-invite-tcui)
2. [사용자 지정 UI를 구현 제목](#title-implemented-custom-ui)

다음 프로세스의 순서도 피: [순서도-다른 플레이어에 게 초대 보내기](mpm-flowcharts/mpm-send-invites.md).

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>1) Xbox 플랫폼 초대 TCUI<a name="xbox-platform-invite-tcui">

| 메서드 | 트리거되는 이벤트 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

호출 `invite_friends()` 친구 초대를 위한 표준 Xbox UI를 표시 합니다. 친구 선택 플레이어나 최근 플레이어 게임에 초대할 수 있도록 UI를 표시 합니다. 플레이어 적중 확인 되 면 멀티 플레이어 관리자 선택한 플레이어의 초대를 보냅니다.

**예제:**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**멀티 플레이어 관리자가 수행 기능**

* 표시는 Xbox를 주식 제목 호출할 UI (TCUI)
* 선택한 플레이어에 직접 초대 보내기

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>2) 제목 사용자 지정 UI를 구현<a name="title-implemented-custom-ui">

| 메서드 | 트리거되는 이벤트 |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

타이틀을 온라인 친구를 보고 하 라는 사용자 지정 TCUI를 구현할 수 있습니다. 게임에서 사용할 수는 `invite_users()` Xbox Live 사용자 Id가 정의한 사용자 집합에 대 한 보내는 방법을 초대 합니다. Xbox UI 주식 대신 고유한 게임에서 UI를 사용 하려는 경우에 유용 합니다.

**예제:**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**멀티 플레이어 관리자가 수행 기능**

* 선택한 플레이어에 직접 초대 보내기
