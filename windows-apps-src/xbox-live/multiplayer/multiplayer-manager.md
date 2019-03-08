---
title: 멀티 플레이어 관리자
description: 멀티 플레이 manager Xbox Live, 쉽게 구현 멀티 플레이어를 설계 하는 높은 수준의 API에 알아봅니다.
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 99159b5d126c671b07d37e20f1bcd61452c7d670
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594718"
---
# <a name="multiplayer-manager"></a>멀티 플레이어 관리자

Xbox Live는 광범위 한 지원에 제목, 멀티 플레이 기능 추가 게임 세계 멤버 Xbox Live 연결을 허용 합니다.  이 풍부한 결혼 정보 회사 연결 시나리오의 경우 플레이어 in progress, 그리고 더 친구의 게임에 참가 하는 기능을 포함 합니다. 그러나 멀티 플레이 게임 2015 Api를 직접 사용 하 여 Xbox Live 멀티 플레이어를 구현 하면 복잡 한 작업을 위해 상당 수준의 디자인 요구 및 모범 사례를 따르고 확인 하려면 테스트, 인증 요구 사항을 충족 수 있습니다.

다중 접속 관리자를 사용 하면 멀티 플레이어에 기능을 추가할 게임 상태를 제공 하 고 세션 및 결혼 정보 회사 연결을 관리 하 여 쉽게 및 이벤트 기반 프로그래밍 모델입니다. 것이 있도록 설계 된 Xbox Live에 대 한 멀티 플레이어 시나리오를 구현 하기 쉬운 게임 Api 집합입니다. 처리 게임 초대를 결혼 정보 회사, 진행률 등의 처리 조인에 멀티 플레이 게임 친구 들과 같은 일반적인 멀티 플레이 시나리오 지향 되는 API를 제공 합니다. 여러 로컬 사용자를 지원 하 고 쉽게 타사 결혼 정보 회사 연결 서비스를 사용 하는 경우 다중 접속 세션 디렉터리와 통합 하 여 제목에 대 한 합니다. 이러한 시나리오 중 대부분 몇 가지 API 호출을 사용 하 여 수행할 수 있습니다.

## <a name="key-features"></a>주요 기능
다음은 멀티 플레이 게임 관리자 API의 주요 기능입니다.

* 쉽게 세션 관리 및 Xbox Live 결혼 정보 회사 연결
* 상태 및 이벤트 기반 프로그래밍 모델입니다.
* 다중 접속 XR 호환 되 고 함께 Xbox Live 모범 사례를 확인 합니다.
* Xbox One XDK UWP 모두 타이틀을 지원합니다.
* 구현 [멀티 플레이 게임 2015 순서도](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx)합니다.
* 기존 다중 접속 2015 Api와 함께 작동 합니다.

>**중요 한** -게임 인증을 전달 하기 위해 필요한 이벤트 멀티 플레이어 온라인 게임을 구현 해야 합니다.

## <a name="overview"></a>개요
다중 접속 Manager는 몇 가지 주요 개념 지향:
* `lobby_session` : 이 장치와 함께 재생을 초대 된 친구를 로컬 사용자 관리에 사용 되는 영구 세션입니다. 그룹은 여러 반올림, maps, 수준 등의 게임을 수행할 수 있습니다 하 고 로비 세션 (플레이어 로컬 장치 포함)는 친구 들이 핵심 그룹을 추적 합니다. 일반적으로이 그룹은 호스트 재생 하려는 게임 모드를 결정 합니다. 그룹 멤버를 사용 하 여 채팅 하는 메뉴를 탐색할 수 있습니다 하는 동안 형성 됩니다.

* `game_session` : 게임의 특정 인스턴스를 실행 하는 플레이어를 추적 합니다. 예를 들어, 경합, 지도, 또는 수준입니다. 통해 새 게임 세션을 만들 수 있습니다 `join_game_from_lobby` 로비 세션에서 멤버를 포함 합니다.  멤버 초대를 수락 하는 경우 (공간이 있는 경우)는 로비, 게임 세션에 다시 추가 수는 있습니다. 매 치메이 킹 활성화 되어 있지만 이러한 추가 플레이어는 로비 세션에 추가 하는 경우 추가 플레이어 게임 세션에 추가할 수 있습니다. 이 게임 종료 되 면 로비 세션에서 플레이어는 여전히를 함께 결혼 정보 회사에서 플레이어를 추가 하지는 것을 의미 합니다.

* `multiplayer_member` : 로컬 또는 원격 장치에서 로그인 하는 개별 사용자를 나타냅니다.

* `do_work` : 적절 한 게임 상태 업데이트 제목 및 Xbox Live 멀티 플레이 서비스 간에 유지 되도록 합니다. 최상의 성능을 보장 하기 위해 do_work() 프레임당 한 번 등 자주 호출 되어야 합니다. 목록을 제공 `multiplayer_event` 처리 하도록 게임에 대 한 콜백 이벤트입니다.

## <a name="state-machine"></a>상태 시스템
`do_work()` 호출이 최신 상태 유지 되도록 해야 합니다.  개발자를 호출 해야 멀티 플레이 게임 Manager에 대 한 작업을 수행 하는 `do_work()` 메서드를 정기적으로 합니다. 이 작업을 수행 하는 가장 안정적인 방법은 프레임당 한 번 이상 호출 하는 것입니다. `do_work()` 너무 자주 호출 하는 방법에 대 한 문제에 대 한 이유 없이 이므로 해야 없는 작업을 수행 하는 경우 신속 하 게 반환 합니다.

멀티 플레이 게임 관리자 API에 의해 반환 된 모든 개체 간주 되지 않아야 스레드로부터 안전 합니다. 하지만 제공, 여러 스레드에서 호출 하는 경우 스레드 동기화를 수행 하는 컨트롤입니다. 라이브러리 내부 다중 스레딩 보호를 갖지만 여전히 해야 하나의 스레드를 예를 들어, 모든 값에 액세스 해야 하는 경우 사용자 고유의 잠금을 구현 다른 스레드를 호출할 수 있지만 members() 목록 walking `do_work()`합니다.

다중 접속 Manager 플레이어 조인, 유지, 또는 세션 중 하나를 업데이트할 때 백그라운드에서 세션을 업데이트를 기반으로 모델을 유지 관리 합니다. 다중 접속 Manager UI 스레드와 게임 스레드 간 스레드 동기화 문제를 방지 하기 위해 호출할 때까지 세션의 앱 표시 상태를 업데이트 하지 않습니다는 `do_work()` 메서드. 일반적으로 백그라운드 스레드에서 세션 변경 내용과 같은 이벤트에 대 한 알림을 수신 하는 이러한 변경 사항을 표시 하려면 UI 스레드와 동기화 해야 합니다. 다중 접속 관리자를 사용 하 여 백그라운드를이 작업 수행 됩니다.  호출할 수 있습니다 `do_work()` 에 주 스레드에서 원하는 시점에는 다중 접속 상태의 최신 스냅숏을 가져오려면 관리자 버퍼링 되를 백그라운드에서.

## <a name="events-and-notifications"></a>이벤트 및 알림
다중 접속 관리자가 중요 한 이벤트 집합을 정의 (참조 `multiplayer_event_type`)를 통해 제목에 게 알리는 `do_work()` 메서드 발생 하는 경우. 원격 플레이어가 들어오거나 나가는, 멤버 속성을 변경 하거나 변경 하는 세션 상태와 같은 이벤트입니다. 모든 멀티 플레이 게임 관리자 Api는 동기입니다. `do_work()` 메서드 비동기 작업 완료 되 면 이벤트의 목록을 반환 합니다. 제목 적절 하 게 이러한 이벤트를 처리 해야 합니다. 참조 하십시오는 `multiplayer_event` 클래스에 대 한 자세한 문서.

이벤트 유형에 따라 각 이벤트에 대 한 캐스팅 해야 합니다 `event_args` 적절 한 이벤트 인수 클래스에 이벤트 속성을 가져옵니다. 다음 예제에서는 `do_work()` 이벤트를 처리 합니다.

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

이 섹션에서 몇 가지 일반적인 시나리오 및 각 시나리오에서 호출 하는 Api를 거치게 됩니다.  백그라운드에서 수행 하는 다중 접속 Manager에 대 한 정보가 제공 됩니다.

* [친구 들과 재생](multiplayer-manager/play-multiplayer-with-friends.md)
* [일치 하는 것](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [게임 초대 보내기](multiplayer-manager/send-game-invites.md)
* [핸들 프로토콜 활성화](multiplayer-manager/handle-protocol-activation.md)

API의 높은 수준의 개요를 확인할 수 있습니다 [멀티 플레이 게임 관리자 API 개요](multiplayer-manager/multiplayer-manager-api-overview.md)합니다.

## <a name="what-multiplayer-manager-does-not-do"></a>다중 접속 관리자는 수행 하지
다중 접속 Manager 훨씬 쉽게 멀티 플레이어 시나리오를 구현 하 고 일부 개발자에 게 데이터를 추상화 하는 동안는 몇 가지 사항을 처리 하지 않습니다 또는 적합 하지 않습니다.

* 영구 온라인 서버 게임 Mmo, 큰 세션 (세션에서 100 개 이상의 플레이어) 해야 하는 다른 게임 형식 등입니다.
* 서버 세션 관리 서버

>다중 접속 관리자 모든 특정 네트워크 기술에 연결 되지 않은 한 모든 네트워크 계층을 사용 해야 합니다.

## <a name="next-steps"></a>다음 단계

C + + 또는 WinRT를 참조 하세요 *멀티 플레이 게임* API의 작업 예제에 대 한 샘플.

API 설명서 Microsoft::Xbox::Services::Multiplayer::Manager 네임 스페이스의 c + + 또는 WinRT 가이드에 있습니다.  확인할 수도 있습니다는 `multiplayer_manager.h` 헤더입니다.

피드백, 질문, 했거나 멀티 플레이 게임 관리자를 사용 하 여 문제가 발생 한 경우에 파악에 게 문의 하거나 지원 스레드에서 포럼에 게시 [ https://forums.xboxlive.com ](https://forums.xboxlive.com)합니다.
