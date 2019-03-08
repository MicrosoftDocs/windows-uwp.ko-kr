---
title: 새로운 기능-2016 년 4 월 Xbox Live SDK 용
description: 새로운 기능-2016 년 4 월 Xbox Live SDK 용
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9ce63a0174fa0c4158764b8bca2443d58d0aefd9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660768"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>새로운 기능-2016 년 4 월 Xbox Live SDK 용

하십시오 합니다 [새로운 기능-2016 년 3 월](1603-whats-new.md) 1603에 추가 된 기능에 대 한 문서

## <a name="os-and-tool-support"></a>OS 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [Version 10.0.10240] 및 Visual Studio 2015 RTM [Version 14.0.23107.0]를 지원합니다.

## <a name="tournaments"></a>토너먼트
- Xbox Live 토너먼트 도구는 현재 SDK에 포함 합니다.  도구 디렉터리를 사용 하는 방법에 대 한 정보에서 해당 데이터를 볼 수 있습니다.
- 토너먼트 Api도 사용할 수 있습니다.  Xbox::Services::Tournaments 네임 스페이스를 참조 하세요.
- 설명서 프로그래밍 가이드에서 사용할 수도 있습니다.

## <a name="documentation"></a>설명서
- [로그인 문제 해결 가이드](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md) 목록 오류 코드 기반 디버깅 로그인 오류 뿐만 아니라 단계를 수행 하려면 몇 가지 일반적인 전략입니다.
- 합니다 [Marketplace](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content) 개발자가 Xbox One에 대 한 문서만 찾을 수 있습니다 프로그래밍 가이드에서.  UWP 개발자가 파트너 센터의 저장소에 대 한 설명서 참조 계속 해야 합니다.
- 한 [UWP에 이식 하는 XDK 가이드](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 유니버설 Windows 플랫폼에는 Xbox One 제목을 제공 하려는 경우에 참조할 수 있습니다.
- 참조 하십시오 합니다 [세분화 된 속도 제한을](../using-xbox-live/best-practices/fine-grained-rate-limiting.md) 문서에 대 한 설명은 여기는 어떻게 적용 뿐만 대 한 다양 한 Xbox Live 서비스 끝점 및 시나리오에 어떤 제한이 있는지에 대 한 정보입니다.

## <a name="multiplayer-manager"></a>멀티 플레이어 관리자
합니다 [멀티 플레이 게임 Manager](../multiplayer/multiplayer-manager.md) 실험적에 파일이 없습니다.  이 API를 사용 하는 개발자의 의견을 통합 하 고 일부 Api 서로 더 일관 되 게 합니다.  사용 하십시오 멀티 플레이 게임 관리자를 시작 지점으로 멀티 플레이어 개발을 수행 하는 경우 다중 접속 2015 API의 복잡 한 문제를 관리 하는 간단한 API를 제공 하므로.

에 아래 섹션에서는 나열 했습니다 소수의 주요 변경 내용 뿐만 아니라 API에는 새로운 기능 중 일부입니다.

#### <a name="completed-events"></a>완료 된 이벤트
모든 Api는 이제 해당가지고``` _competed``` 이벤트 및 모든 이벤트를 성공 또는 실패 여부에 관계 없이 발생 합니다. 이전 동작 것만 트리거한에서 조치를 취하여 제목에 실패 하면 했습니다. 완료 되 면 제목 받습니다 여러 의미 하므로 호출은 일괄 처리 및 ```_competed``` 이벤트입니다.

| API | 반환 된 이벤트 |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

에 마찬가지 ```game_session()```합니다.

#### <a name="application-defined-context"></a>응용 프로그램 정의 컨텍스트
이제 시작 호출에 멀티 플레이 이벤트 상관 관계를 각 set_ * 메서드에 대 한 선택적 응용 프로그램 정의 컨텍스트를 전달할 수 있습니다.
예를 들면 다음과 같습니다.

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

컨텍스트를 통해 제목에 다시 다음 반환 되는 여 ```_completed``` 이벤트입니다.

#### <a name="breaking-changes"></a>주요 변경 내용

1.  둘 다 초대 Api (```invite_friends``` & ```invite_users```) 동기 됩니다. 완료 되 면 invite_sent 이벤트를 반환합니다.

2.  ```write_synchronized_properties_and_commit``` 로 이름이 변경 되었습니다 ```set_synchronized_properties```합니다. 완료 되 면 반환 된 ```session_synchronized_property_write_completed``` 이벤트입니다.

3.  ```write_synchronized_host_and_commit``` 로 이름이 변경 되었습니다 ```set_synchronized_host```합니다. 완료 되 면 반환 된 ```synchronized_host_write_completed``` 이벤트입니다.

4.  에서 ```lobby_session()```

  *제거*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *추가*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  에서 ```game_session()```

  *제거*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *추가*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  이벤트 유형을 변경 합니다.

  *제거*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *이름이*

  ```write_synchronized_properties_completed``` 이름이 ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` 이름이 ```synchronized_host_write_completed```
