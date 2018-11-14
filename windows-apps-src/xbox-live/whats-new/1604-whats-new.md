---
title: 새로운 기능에 대 한 Xbox Live SDK-2016 년 4 월
author: KevinAsgari
description: 새로운 기능에 대 한 Xbox Live SDK-2016 년 4 월
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0efeb16866628a4d318964f9b1a7a293c7679be8
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6442540"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>새로운 기능에 대 한 Xbox Live SDK-2016 년 4 월

1603에 추가 된 항목에 대 한 [새로운-2016 년 3 월](1603-whats-new.md) 문서를 참조 하세요

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [버전 10.0.10240] 및 Visual Studio 2015 RTM [버전 14.0.23107.0]를 지원합니다.

## <a name="tournaments"></a>토너먼트
- Xbox Live 토너먼트 도구는 이제 SDK에 포함 되어 있습니다.  도구 디렉터리 함께 사용 하는 방법에 대 한 정보에에서 것을 볼 수 있습니다.
- 토너먼트에 대 한 Api를 사용할 수도 있습니다.  Xbox::Services::Tournaments 네임 스페이스를 참조 하세요.
- 설명서 프로그래밍 가이드에서 제공 됩니다.

## <a name="documentation"></a>설명서
- [로그인 문제 해결 가이드](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md) 로그인 오류 뿐 아니라 단계 오류 코드에 따라 수행 하려면 디버그 하려면 몇 가지 일반적인 전략을 나열 합니다.
- Xbox One 개발자를 위한 [시장](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content) 문서만 이제에서 찾을 수 있습니다 프로그래밍 가이드입니다.  UWP 개발자는 파트너 센터 스토어에 대 한 설명서에 대 한 참조를 계속 합니다.
- 유니버설 Windows 플랫폼에는 Xbox One 제목 가져오기에 관심이 있는 경우를 참조할 수 [XDK UWP 포팅 가이드에](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 있습니다.
- 다양 한 Xbox Live 서비스 끝점 및 시나리오 뿐 아니라 제한은 기능에 대 한 정보에 대 한 적용 이러한 방법에 대 한 설명에 대 한 [세분화 속도 제한](../using-xbox-live/best-practices/fine-grained-rate-limiting.md) 문서를 참조 하십시오.

## <a name="multiplayer-manager"></a>멀티 플레이어 관리자
[멀티 플레이어 관리자](../multiplayer/multiplayer-manager.md) 는 더 이상 실험에 없습니다.  이 API를 사용 하 여 개발자의 피드백, 일부 Api 서로 더 일관 되 게 합니다.  사용 하십시오 멀티 플레이어 관리자를 시작 점으로 멀티 플레이 개발을 수행할 때 다양 한 멀티 플레이 2015 API의 복잡성을 관리 하는 단순한 API를 제공 합니다.

에 섹션 아래 적은 수의 주요 변경 뿐만 아니라 API, 새로운 기능 중 일부 나열 될 것입니다.

#### <a name="completed-events"></a>완료 이벤트
모든 Api는 이제는 해당에``` _competed``` 이벤트 및 모든 이벤트는 성공 또는 실패 여부에 관계 없이 발생 합니다. 이전 동작 것만 작업을 수행할 제목에 대 한 오류 발생 시 트리거되는 했습니다. 호출을 일괄 이후는 작업이 완료 되 면 제목 가져옴 여러 의미 하 고 ```_competed``` 이벤트입니다.

| API | 반환 된 이벤트 |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

마찬가지입니다 ```game_session()```.

#### <a name="application-defined-context"></a>응용 프로그램 정의 된 컨텍스트
멀티 플레이 이벤트를 시작 하는 호출을 연결할 각 set_ * 방법에 대 한 선택적 응용 프로그램 정의 컨텍스트에 전달할 수 있습니다.
예를 들면 다음과 같습니다.

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

컨텍스트를 통해 제목에 다시 반환 됩니다 다음의 ```_completed``` 이벤트입니다.

#### <a name="breaking-changes"></a>주요 변경 내용

1.  둘 다 초대 Api (```invite_friends``` & ```invite_users```) 동기 됩니다. 작업이 완료 되 면 invite_sent 이벤트를 반환합니다.

2.  ```write_synchronized_properties_and_commit``` 로 바뀌었습니다 ```set_synchronized_properties```. 작업이 완료 되 면 반환는 ```session_synchronized_property_write_completed``` 이벤트입니다.

3.  ```write_synchronized_host_and_commit``` 로 바뀌었습니다 ```set_synchronized_host```. 작업이 완료 되 면 반환는 ```synchronized_host_write_completed``` 이벤트입니다.

4.  Windows ```lobby_session()```

  *제거됨*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *추가*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  Windows ```game_session()```

  *제거됨*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *추가*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  이벤트 형식에 변경 합니다.

  *제거됨*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *이름이 변경*

  ```write_synchronized_properties_completed``` 로 이름이 변경 ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` 로 이름이 변경 ```synchronized_host_write_completed```
