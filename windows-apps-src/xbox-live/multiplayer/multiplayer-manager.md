---
title: 멀티 플레이어 관리자
author: KevinAsgari
description: Xbox Live 멀티 플레이어 관리자, 쉽게 구현할 멀티 플레이 하도록 설계 하는 높은 수준의 API에 알아봅니다.
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bf931a2811a9a627c40a7dc45178688236437fa8
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4460595"
---
# <a name="multiplayer-manager"></a>멀티 플레이어 관리자

Xbox Live 폭넓은 지원을 제공 하면 제목은 멀티 플레이 기능에 추가 하기 위한 게임 세계의 Xbox Live 멤버 연결을 허용 합니다.  풍부한 매치 메이 킹 시나리오를 진행 중인 등 친구의 게임에 참가할 플레이어에 대 한 기능이 포함 됩니다. 그러나 멀티 플레이 2015 Api를 직접 사용 하 여 Xbox Live 멀티 플레이 구현 디자인은 큰 수준의 요구 및 모범 사례를 따르는 확인 하는 테스트 및 인증 요구 사항을 충족 복잡 한 작업을 수 있습니다.

멀티 플레이어 관리자 간편 하 게 세션 및 매치 메이 킹, 관리 하 고 상태를 제공 하 여 게임에 멀티 플레이 기능을 추가 하 고 이벤트 프로그래밍 모델을 기반으로 합니다. 것이 쉽게 Xbox live 멀티 플레이어 시나리오를 구현할 게임 하도록 설계 된 Api 집합입니다. 친구와 멀티 플레이 게임 등의 일반적인 멀티 플레이어 시나리오 주위 지향 API를 제공, 처리 게임 초대, 진행 중, 매치 메이 킹, 및 처리에 가입 합니다. 여러 로컬 사용자를 지원 하 고는 제 3 자 매치 메이 킹 서비스를 사용 하는 경우 멀티 플레이어 세션 디렉터리와 통합 하 여 제목에 쉽게 합니다. 이러한 시나리오의 많은 몇 가지 API 호출을 사용 하 여 수행할 수 있습니다.

## <a name="key-features"></a>주요 기능
멀티 플레이어 관리자 API의 주요 기능에는 다음과 같습니다.

* 세션을 쉽게 관리 하 고 Xbox Live 매치 메이 킹
* 상태 및 이벤트 프로그래밍 모델을 기반으로 합니다.
* 모범 사례 Xbox Live 멀티 플레이어 XR 호환 되 고 함께 보장 합니다.
* UWP와 Xbox One XDK 타이틀을 지원합니다.
* [멀티 플레이 2015 순서도](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx)구현합니다.
* 기존의 멀티 플레이 2015 Api와 함께 작동 합니다.

>**중요** -게임 해야 인증을 통과 하려면 여전히 온라인 멀티 플레이어에 대 한 이벤트를 구현 해야 합니다.

## <a name="overview"></a>개요
멀티 플레이어 관리자는 몇 가지 주요 개념 방향을 지정 합니다.
* `lobby_session` : 영구 세션에이 장치와 함께 재생 하고자 하는 친구 초대 받은 사용자 관리에 사용 합니다. 여러 차례, 지도, 수준 등 게임의 그룹을 재생할 수 및 로비 세션의 친구 (장치에 로컬 플레이어 포함)이 핵심 그룹을 추적 합니다. 일반적으로이 그룹 채팅 재생 하려는 게임 모드를 결정 하는 그룹 구성원을 사용 하 여 메뉴를 통해 호스트를 탐색 될 수 있는 동안 구성 됩니다.

* `game_session` : 게임의 특정 인스턴스를 재생 하는 플레이어를 추적 합니다. 예를 들어는 경합, 지도, 또는 수준입니다. 새 게임 세션을 통해 만들 수 있습니다 `join_game_from_lobby` 멤버 로비 세션에 포함 합니다.  구성원 초대를 수락 하면 (공간이 있는 경우) 로비 및 게임 세션에 다시 추가 수는 있습니다. 매치 메이 킹 활성화 되지만 해당 추가 플레이어는 로비 세션에 추가 하는 경우 추가 플레이어가 게임 세션에 추가할 수 있습니다. 즉, 게임이 종료 되 면 플레이어 로비 세션에는 여전히를 함께 반면 매치 메이 킹에서 추가 플레이어는 없습니다.

* `multiplayer_member` : 로컬 또는 원격 장치에 로그인 하는 개별 사용자를 나타냅니다.

* `do_work` : 하면 적절 한 게임 상태 업데이트 제목 및 Xbox Live 멀티 플레이어 서비스 간에 유지 됩니다. 최상의 성능을 보장 하기 위해 do_work() 프레임 마다 한 번씩 같은 자주 호출 되어야 합니다. 목록을 제공 `multiplayer_event` 를 처리 하는 게임에 대 한 이벤트 콜백 합니다.

## <a name="state-machine"></a>상태 시스템
`do_work()` 호출은 최신 상태 유지 되도록 해야 합니다.  멀티 플레이어 관리자에 해당 작업을 수행 하려면, 개발자가 호출 해야 합니다 `do_work()` 메서드를 정기적으로 합니다. 이 작업을 수행 하는 가장 안정적인 방법은 프레임당 한 번 이상 호출 됩니다. `do_work()` 너무 자주 호출에 대 한 문제에 대 한 이유가 없습니다 하므로, 수행할 작업이 없는 신속 하 게를 반환 합니다.

멀티 플레이어 관리자 API에서 반환 된 모든 개체가 간주 하지 말아야 스레드 안전 합니다. 하지만 제공 컨트롤을 여러 스레드에서 호출 하는 경우 스레드 동기화를 수행 합니다. 라이브러리에 내부 다중 스레딩 보호 하지만 여전히 해야 –의 모든 값에 액세스 하는 하나의 스레드가 필요한 경우 고유한 잠금 구현 다른 스레드에서 호출 될 수 있는 동안 members() 목록 walking `do_work()`.

멀티 플레이어 관리자 플레이어에 가입 하지, 또는 세션을 업데이트할 때 백그라운드에서 세션을 업데이트를 기반으로 모델을 유지 합니다. 멀티 플레이어 관리자 UI 스레드 및 게임에 스레드 간의 스레드 동기화 문제를 방지 하기 위해 호출할 때까지 세션의 앱 표시 상태를 업데이트 되지 않는 합니다 `do_work()` 메서드. 일반적으로 백그라운드 스레드에서 세션 변경 등의 이벤트에 대 한 알림을 수신 하는 이러한 변경 내용을 표시 하도록 UI 스레드와 동기화 해야 합니다. 멀티 플레이어 관리자를 사용 하 여이 작업 백그라운드 작업을 수행 됩니다.  호출할 수 있습니다 `do_work()` 사용자가 선택한 당시에 주 스레드에서 상태의 최신 스냅숏을 멀티 플레이어를 가져오려면 관리자는 버퍼링 한 백그라운드 작업 합니다.

## <a name="events-and-notifications"></a>이벤트 및 알림
중요 한 이벤트 집합을 정의 하는 멀티 플레이어 관리자 (참조 `multiplayer_event_type`)를 통해 제목에 게 알리는 합니다 `do_work()` 메서드를 발생 했을 때. 원격 플레이어에 가입 하거나, 벗어나지 구성원 속성 변경, 또는 세션 상태 변경 이벤트입니다. 모든 멀티 플레이어 관리자 Api는 동기식입니다. `do_work()` 메서드는 이러한 비동기 작업이 완료 될 때 이벤트의 목록을 반환 합니다. 적절 한 피 제목에서 이러한 이벤트를 처리 해야 합니다. 참조는 `multiplayer_event` 클래스에 대 한 자세한 내용은 설명서.

이벤트 형식에 따라 각 이벤트에 캐스팅 해야 합니다 `event_args` 클래스에 해당 이벤트를 적절 한 arg 이벤트 속성을 가져옵니다. 다음 예제를 사용 하 여 `do_work()` 이벤트를 처리 합니다.

```cpp
auto eventQueue = mpInstance.do_work();
for (auto& event : eventQueue)
{
    switch (event.event_type())
    {
      case multiplayer_event_type::member_joined:
      {
        auto memberJoinedArgs =  std::dynamic_pointer_cast<member_joined_event_args>(event.event_args());
        HandleMemberJoined(memberJoinedArgs);
        break;
      }
      case multiplayer_event_type::session_property_changed:
      {
        auto sessionPropertyChangedArgs =  std::dynamic_pointer_cast<session_property_changed_event_args>(event.event_args());
        HandlePropertiesChanged(sessionPropertyChangedArgs);
        break;
      }
      . . .
    }
}

```

## <a name="scenarios"></a>시나리오

이 섹션에서는 몇 가지 일반적인 시나리오 및 때 각 시나리오에서 호출할 Api를 통해 살펴봅니다 됩니다.  백그라운드에서 수행 하는 멀티 플레이어 관리자에 대 한 정보 제공 됩니다.

* [친구와 재생](multiplayer-manager/play-multiplayer-with-friends.md)
* [일치를 찾지](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [게임 초대 보내기](multiplayer-manager/send-game-invites.md)
* [프로토콜 활성화 처리](multiplayer-manager/handle-protocol-activation.md)

[멀티 플레이어 관리자 API 개요](multiplayer-manager/multiplayer-manager-api-overview.md)에서 API의 간략 한 개요를 확인할 수 있습니다.

## <a name="what-multiplayer-manager-does-not-do"></a>멀티 플레이어 관리자 수행 하지 않는 기능
멀티 플레이어 관리자 훨씬 쉽게 멀티 플레이어 시나리오를 구현 하 고 개발자의 데이터 중 일부를 추상화, 동안 처리 하지 않는 또는에 가장 적합 하지는 몇 가지 있습니다.

* 영구 온라인 서버 게임 Mmo, 큰 세션 (세션에서 100 개 이상의 플레이어) 해야 하는 다른 게임 형식 등입니다.
* 서버를 서버 세션 관리

>멀티 플레이어 관리자는 모든 특정 네트워크 기술에 연결 되지 않은 및 모든 네트워크 계층으로 작동 해야 합니다.

## <a name="next-steps"></a>다음 단계

API의 실제 예는 c + + 또는 WinRT *멀티 플레이어* 예제를 참조 하십시오.

C + + 또는 WinRT 가이드 Microsoft::Xbox::Services::Multiplayer::Manager 네임 스페이스에서에서 API 설명서를 찾을 수 있습니다.  또한 볼 수는 `multiplayer_manager.h` 헤더.

질문이 있으면, 피드백, 멀티 플레이어 관리자를 사용 하 여 문제가 발생할 경우 댐을 사용자에 게 문의 하거나 지원 스레드에서 포럼에서 게시 [https://forums.xboxlive.com](https://forums.xboxlive.com).
