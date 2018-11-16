---
title: 아레나 타이틀 통합 가이드
author: KevinAsgari
description: Xbox Live 아레나 플랫폼에 대 한 지원에서 제목을 수 구축 하는 방법을 알아봅니다.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 아레나, 토너먼트 참가
ms.localizationpriority: medium
ms.openlocfilehash: 3da29f8e93ed8ce98648cad4711e35577f6d4823
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6841719"
---
# <a name="arena-title-integration-guide"></a>아레나 타이틀 통합 가이드

## <a name="introduction"></a>소개

Xbox 아레나 플랫폼 토너먼트 이끌이가 (TOs)를 만들고 지 원하는 아레나 타이틀에 대 한 토너먼트 작동할 수 있습니다. 아레나 모두는 Xbox Live를 게시자 및 사용자 실행 토너먼트 수 있도록 뿐 아니라 아레나와 통합 하는 제 3 자 TOs 지원 합니다. 토너먼트 토너먼트, 스위스 등의 스타일을 따라 결정 됩니다.

이 항목에서는 안내, 제목 개발자 타이틀에 아레나에 대 한 지원을 구현 하는 방법 알아봅니다. 게임 아레나 활성화 되 면 TO로 선택 하는 지원 되는 토너먼트 형식으로 작동 합니다. TO는 토너먼트 구조에 따라 타이틀에 대 한 일치를 조정 합니다. 아레나 활성화 제목 올바른 사용자가 게임 내에서 각 일치에 배치 합니다. 그리고 일치 결과를 보고 합니다.

## <a name="overview"></a>개요

Xbox 아레나 Xbox 멀티 플레이 게임 개발에 게 친숙 한 개념을 사용합니다. Xbox 멀티 플레이 시스템에 익숙하지 계속 하기 전에 해당 설명서를 검토 하는 것이 좋습니다. 프로세스는 멀티 플레이어 게임에 초대를 수락 하는 사용자의 비슷합니다. 아레나 토너먼트에서 일치 하는 항목을 재생 하는 사용자에 대 한 시간 때 사용자에 게 알려주는 알림 설정 이메일이 나타납니다. 알림 메시지를 수락 하면 타이틀 표준 아레나 프로토콜 활성화 이벤트를 수신 하도록 합니다. 타이틀 이미 실행 하는 경우이 시간에 시작 됩니다.

자체 토너먼트 일치 하는 멀티 플레이어 세션 디렉터리 (MPSD) 세션을 사용 하 여 조정 됩니다. 아레나의 경우 세션이 만들어집니다 대신 아레나 플랫폼에서 타이틀. 세션 템플릿 및 제목 게시자로 사용자에 의해 생성 된 추가 게임 구성을 사용 하 여 수행 됩니다. 토너먼트 일치 참가자 세션에 참가 이미 됩니다. 제목 게시자로 보고 하는 결과 위해 중재가 지 원하는 데 아레나에 대 한 세션 템플릿을 구성 해야 합니다. 이 문서의 뒷부분에서 세션에 대 한 전체 요구 사항은 포함 됩니다. 타이틀을 만드는 데 필요한 모든 추가 세션 무료 이기도 합니다.

타이틀 일치 및 보고서 결과 설정 하는 세션을 사용 합니다. 응답 프로토콜 활성화 하 고 MPSD 세션와 상호 작용은의 통합 아레나 타이틀에 필요한 최소 수준입니다. 검색 및 토너먼트 신청 Xbox 아레나 UI를 통해 지원 됩니다. 선택적으로 제목을 풍부한 제목에서 환경을 토너먼트 허브 서비스에서의 토너먼트에 대 한 추가 정보를 가져올 수 있습니다. 예를 들어 토너먼트 허브를 사용 하 여 타이틀 컨텍스트 팀 이름 등을 표시할 수 있으며 사용자에 대 한 새로운 일치 세션 검색. 이 데이터 흐름에서 녹색 화살표 다음 다이어그램에 표시 됩니다 하 고 [경험 요구 사항 및 모범 사례](#experience-requirements-and-best-practices) 섹션에서 자세히 설명 됩니다. 페이드 화살표 일치 하는 토너먼트 내에서 이동할 때 팀의 참조를 세션에 시간이 지남에 따라 변경 있는지를 나타냅니다.

![](../../images/arena/tournament-flow.png)


아레나 프로토콜 활성화 URI는 일치 하 고 일치 하는 위에 올 때 타이틀 호출할 수 있는 딥 링크에 대 한 세션은 토너먼트에 대 한 정보를 포함 합니다. 딥 링크 Xbox 아레나 UI에 사용자를 반환합니다. 이러한 URI 구성 요소는 [아레나 통합에 대 한 기본](#basic-requirements-for-arena-integration)요구 사항을 [프로토콜 활성화](#protocol-activation) 섹션에서 자세히 설명 되어 있습니다.

## <a name="basic-requirements-for-arena-integration"></a>아레나 통합에 대 한 기본 요구 사항

이 섹션에서는 아레나 지원 하기 위한 최소 요구 사항을 타이틀 통합에 대 한 기술 지침 및 세부 정보를 제공 합니다. 이 스타일의 통합 개요 다음 다이어그램에 주황색 화살표에 데이터 흐름을 활용 합니다.

![](../../images/arena/arena-data-flow.png)

타이틀은 Xbox 아레나 UI에서 프로토콜 활성화 됩니다. 이 토너먼트에 대 한 세부 정보 페이지 또는 일치에 대 한 다른 진입점 모든 알림 메시지에서 발생할 수 있습니다. 사용자가 일치 하는 항목에 참여 하는 경우 발생 하는 단계는 합니다.

1.  타이틀은 Xbox 아레나 UI에서 프로토콜 활성화 됩니다.
2.  타이틀 일치 하는 단일 항목을 재생 하려면 정품 인증 매개 변수를 사용 합니다.
3.  일치 하는 끝나면 타이틀 MPSD 게임 세션에 결과 보고 합니다.
4.  타이틀 사용자 아레나 UI를 반환 하는 옵션을 제공 합니다.

다음 섹션에서는 각 자세히이 단계를 거칩니다.

### <a name="1--protocol-activation"></a>1. 프로토콜 활성화

아레나 UI 후 작업을 활성화 하 여 프로토콜-타이틀 일치 하는 사용자에 대 한 준비가 되었을 때 됩니다. 다음 두 가지 경우: 실행 중인 이미 또는 현재, 타이틀을 시작할 수 있습니다. 두 경우 모두에 대 한 응답을 멀티 플레이 게임 초대를 수락 하는 사용자에 게는 제목이 활성화 될 때 발생 하는 것과 비슷합니다.

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>프로토콜 활성화 시 타이틀 시작 되 면

타이틀을 시작할 때 처음으로 **활성화** 이벤트가 발생 합니다. 아레나 프로토콜 활성화 초기 정품 인증 하는 경우, 타이틀을 토너먼트를 재생 하는 사용자가 시작 했습니다. 메뉴 및 로그인 화면을 우회 하 여 일치 항목을 최대한 빨리 건너뛸 타이틀을 했습니다. 참여 하는 사용자의 Xbox 사용자 ID (XUID) URI 정품 인증에서 제공 되 고 이미 서명 되어 있어야 합니다.

#### <a name="if-your-title-is-already-running"></a>타이틀 이미 실행 중인 경우

타이틀 이미 실행 중인 동안에 **활성화** 이벤트 아레나 프로토콜 활성화를 사용 하 여 발생할 수 있습니다. 명시적인 사용자 작업 (일치 하는 항목을 나타내는 알림 메시지에서 역할 또는 Xbox 아레나 UI에서 일치 항목으로 이동)에 의해서만 **활성화** 이벤트가 트리거됩니다. 타이틀 메뉴 화면에서 대기, 있으면 토너먼트 일치 항목으로 바로 이동할 수 있습니다. 타이틀 게임 플레이 하는 동안 프로토콜 활성화를 받은 경우 사용자에 게 게임을 두고 수 있는 가장 편리한 방법으로 토너먼트 일치를 입력 하는 옵션을 제공 하도록. 사용자가 필요한 경우 게임을 저장 하는 기회를 제공 합니다.

프로토콜 활성화를 통해 타이틀에 제공 되는 `CoreApplicationView.Activated` 이벤트입니다. 경우는 `IActivatedEventArgs.Kind` 이벤트 인수의 속성이 설정 됩니다 `Protocol`, 활성화는 프로토콜 활성화 및 인수를 캐스팅할 수는 `ProtocolActivatedEventArgs` 프로토콜 활성화 URI를 통해 사용할 수 있는 클래스는 `Uri` 속성입니다.

타이틀에 프로토콜 활성화 URI XUID 확인 했습니다. 현재 플레이어, 일치 하지 않을 경우 타이틀 사용자 컨텍스트 전환 해야 합니다.

#### <a name="the-protocol-activation-uri"></a>프로토콜 활성화 URI

URI에 대 한 템플릿은 다음과 같습니다.

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

콘솔에서 종말 제목에 대 한 활성화 스키마는 약간 다릅니다.

```
ms-xbl-{titleIdHex}://
```

이 초대 같아야 합니다. 이 위해 제목 ID 필요한 경우 앞에 오는 0 포함 됩니다 8 진수 여야 합니다.

먼저 있는지 확인 합니다 `Uri.Host` (바로 뒤에 "://" 구분 기호 이름)은 "토너먼트"입니다. 게임 초대 같은 다른 기능에 대 한 정품 인증에서 아레나의 프로토콜 활성화는 구별 하는 방법입니다.

URL 인코딩 및 대/소문자는 쿼리 문자열 인수 됩니다. 타이틀 매개 변수 순서 의존해 서는 안 하 고 인식할 수 없는 매개 변수를 무시 해야 합니다.


Paramater(s) | 설명
--- | ---
**action** | 지원 되는 작업만 "joinGame"입니다. **작업** 은 누락 되었는지에 대 한 다른 값을 지정 하는 정품 인증을 무시 합니다.
**joinerXuid** | **JoinerXuid** 토너먼트 검색에서 재생 하려고 로그인 사용자의 XUID입니다. 타이틀이이 사용자의이 컨텍스트를 전환할 수 있어야 합니다. **JoinerXuid** 서명 되지 않은 경우 타이틀 계정 선택기를 표시 하 여 사용자를 표시 해야 합니다. XUIDs 10 진수에서 항상 표시 됩니다.
**organizerId, tournamentId** | **OrganizerId** 및 **tournamentId**, 조합 되 면 일치 하는 연결 된 토너먼트에 대 한 고유 식별자를 구성 합니다. 이 식별자를 사용 하 여 타이틀에 표시 하려는 경우 토너먼트 허브에서의 토너먼트에 대 한 더 자세한 정보를 검색 합니다.
**팀 Id** | **팀 Id** ( **joinerXuid** 매개 변수에 의해 지정) 사용자의 구성원 인 토너먼트의 컨텍스트에서 팀에 대 한 고유 식별자입니다. 같은 **organizerId** 및 **tournamentId** 매개 변수를 선택적으로 토너먼트 허브에서 팀에 대 한 정보를 검색 **팀 Id** 를 사용할 수 있습니다.
**서비스 이름, templateName 안내** | 함께 이러한 세션을 식별합니다. 세션에 MPSD URI 경로에서 동일한 세 가지 매개 변수는 다음과 같습니다.</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`.</br></br>XSAPI, 세 가지 매개 변수를 되는 `multiplayer_session_reference `생성자입니다.
**returnUri, returnPfn** | **ReturnUri** Xbox 아레나 UI에 사용자를 반환 하는 프로토콜 활성화 URI입니다. **ReturnPfn** 매개 변수 수도, 없을 수도 있습니다. 인 경우, **returnUri**처리 하는 것이 목적 하는 앱에 대 한 제품 제품군 이름 (PFN) 됩니다. 이러한 값을 사용 하는 방법을 보여 주는 샘플 코드를 [반환 Xbox 아레나 UI를](#returning-to-the-xbox-arena-ui)참조 하세요.

### <a name="2--playing-the-match"></a>2. 일치 항목을 재생합니다.

토너먼트 이끌이가 MPSD 세션을 만들면 사용자가 해당 세션의 비활성 멤버 설정 됩니다. 타이틀 즉시 설정 해야 플레이어가 활성화를 사용 하 여 `multiplayer_session::join()`. Xbox Live와 사용자가 재생할 준비가 및 제목에 있는 일치에 다른 사용자에 게 나타냅니다.

일치 하는 시작 시간은 세션에서 `/servers/arbitration/constants/system/startTime` 표준 UTC 형식으로 날짜 및 시간 값으로 (예를 들어 "2009-06-15T13:45:30.0900000Z"). (시작 시간으로 XSAPI 제공 됩니다. `multiplayer_session_arbitration_server::arbitration_start_time()`, 1703 XDK에서 시작). 시작 시간은 동시 등 원하는 대로 TO 시작 시간 보다 오래 세션을 만들 수 있습니다. 일치 알림은 제목 시작 시간 전에 되지 활성화 하므로 시작 시 일치 참가자에 게 전송 됩니다. 시작 시간은 MPSD 일치에 대 한 보고 되는 결과 허용 하는 가장 빠른 시간 이기도 합니다.

타이틀 각 구성원을 살펴보고 `/member/{index}/constants/system/team` 속성 (`multiplayer_session_member::team_id()`) 파악 하기에 각 사용자는 팀입니다.

또한 타이틀 지도 및 모드와 같은 일치 설정을 확인 하려면 세션을 사용 합니다. 이러한 제목 관련 설정 세션 템플릿 또는 사용자 지정 상수로 토너먼트 정의의 일부로 설정할 수 있습니다. (자세한 내용은 참조 [아레나 제목을 구성](#configuring-a-title-for-arena)). 예는 다음과 같습니다.

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

JavaScript Object Notation (JSON) 개체와 세션에서 사용 하 여 이러한 설정을 검색할 수는 `multiplayer_session_constants::session_custom_constants_json()` API입니다.

일반적으로 타이틀 아레나 세션 자체 MPSD 세션을 취급 동일한 방식으로 처리 해야 합니다. 예를 들어 핸들을 만드는 고 RTA 알림에 대 한 가입할 수 있습니다. 하지만 몇 가지 차이점이 있습니다. 예를 들어, 타이틀 세션의 서비스 품질 (QoS) 기능을 사용 하거나 게임 세션 명단을 변경할 수 없습니다 및 중재에 참여 해야 합니다. 세션의 자세한 내용은이 문서의 뒷부분에 제공 됩니다.

### <a name="3--reporting-results"></a>3. 결과 보고 합니다.

일치 하는 결과 중재 라는 기능을 사용 하 여 아레나, 세션에 다시 보고 됩니다. 중재 세션을 사용 하 여 안전 하 게 일치 및 보고서 결과 재생 하는 프레임 워크입니다.

프로토콜 활성화 단계에서 타이틀을 제공 하는 세션 중재 프레임 워크에서 적용 되는 고정된 일정 있다는 것을 의미 하는 arbitrated 세션이 됩니다. 이 다이어그램은 중재 타임 라인을 보여 줍니다.

![](../../images/arena/arbitration-timeline.png)

하나 이상의 플레이어 시작 시간은 forfeit 시간 이전 세션에서 활성화 되어 있어야 합니다 (`multiplayer_session_arbitration_server::arbitration_start_time()`)와 forfeit 제한 합니다. Forfeit 제한 시간 (밀리초)에서 숫자로 세션에는 `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`). 아무도 forfeit 시간 전에 활성 세션에 연결 하 고, 일치 하는 취소 됩니다.

중재 제한에 밀리초에서입니다 `/constants/system/arbitration/arbitrationTimeout` (`multiplayer_session_constants::arbitration_timeout()`). 중재 시간 시작 시간 및 중재가 제한 시간 값 이며 플레이어 일치를 완료 하 고 결과 보고 있는 해야 있는 시간을 나타냅니다. 이 값은 게시자가 세션 템플릿 또는 게임 모드에서 설정 됩니다. 완료 되는 일치에 대 한 요구 사항에 맞춰 타이틀 많은 시간을 허용 하도록 설정 합니다.

타이틀은 언제 든 지 시작 시간과 중재 시간 사이의 결과 보고할 수 있습니다. 언제 든 지 forfeit 시간과 세션의 활성 구성원 모든 결과 전송한 후 중재 시간 사이의 중재가 발생 합니다. 예를 들어 하나의 멤버 forfeit 시 세션에서 활성화 되 면 수 있습니다 (및 해야) 게시할 결과 및 중재가 발생 합니다. 얼마나 많은 결과 사용할 수 있는 중재 시간에 관계 없이 이미 되지 않은 경우 중재가 발생 합니다. 중재 시간에 도달 하면 결과가 전송 하는 경우 모든 참가자 일치에서 손실을 제공 됩니다.

단순히 arbitrated 결과 작성 하 여 언제 든 지 중재가 되도록 게임 서버에 대 한도 가능 합니다.

사용자가 세션을 위해 중재 이미에 경우 (두 중재 제한 시간이 만료, 게임 서버 arbitrates 세션 또는 사용자가 런타임에 연결할 때문에), 타이틀 일치를 종료 하 고 사용자에 게 arbitrated 결과 표시 합니다.

중재 결과 항상 모든 팀에 대 한 결과 포함합니다. 개별 플레이어 세션에 결과 프로그램이 뿐만 아니라 팀의 결과 포함 하지만 모든 팀에 대 한 결과의 전체 집합입니다.

JSON에서 결과 다음과 같습니다.

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

이 예제에서는 Id 팀은 "빨간색" 및 "파랑"입니다. 빨간색 팀 두 번째 첫 번째 및 blue team을 제공 합니다. **결과** 대 한 다른 가능한 값은:

* "성공"
* "손실"
* "그리기"
* "noshow"

"순위" 이외의 **결과** 이면 해당 팀에 대 한 순위는 생략 됩니다.

개별 사용자에 게 작성 일치 결과를 `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`). 다음 예제에서는 제목을 수도 있습니다 만들고 타이틀을 기반으로 팀을 찾아야 한다고 가정 하 고 결과 집합을 제출 하는 방법을 보여 줍니다 (즉, 모든 플레이어는 하나의 팀).

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

MPSD 배치에서 최종 결과 중재가 발생 한 후 `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`), 다음과 같이 다른 몇 가지 속성과 함께 합니다.

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

**ResultState** 예는 다음과 같습니다.

* "완료": 모든 활성 플레이어 결과 보고 합니다.
* "partialresults": 전부가 아닌 일부 활성 플레이어 중재가 제한 하기 전에 결과 보고 합니다.
* "noresults": None 활성 플레이어 중재가 제한 하기 전에 결과 보고 합니다.
* "취소": forfeit 제한 시간 전에 도착 한 활성 플레이어 없습니다.

"Noresults" 및 "취소"의 경우 결과 생략 됩니다.

**ResultSource** "조정" 때 MPSD는 개별 플레이어에서 보고 결과에 따라 중재를 수행 합니다. 게임 서버 중재가 무시 허용 안 함 /servers/arbitration/properties/system 자체에 쓸 수 있습니다. 이 경우 "서버의" **resultSource** 나타냅니다. 것도 가능 게임 서버를 다시 작성 (즉, 수정 또는 조정)는 **resultSource** 로 설정 하는 경우 "조정" 결과입니다.

**ResultConfidenceLevel** 은 결과에 동의 수준을 나타내는 0과 100 사이의 정수입니다. 100 동의 하는 모든 플레이어를 의미 합니다. 0 없는 도출 했습니다 결과 선택 기본적으로 임의로 제출에서 의미 합니다. 게임 서버 중재 결과 설정 하는 경우 **resultConfidenceLevel**설정-100, 일반적으로 게임 서버 자체에 몇 가지 이유로 있는지 (예를 들어 싱글 플레이어 응답 프로시저는 자체의 결과 이라고 보고) 되지 않는 한 합니다. 결과 조정 된 결과에 대 한 신뢰도 반영 하도록 조정 하는 경우 **resultConfidenceLevel** 도 설정 합니다.

**ResultSource** "조정" (이며 **resultState** "완료" 또는 "partialresults")을 제공 하면 플레이어가 보고 결과 중 하나 이상이의 복사본을 정확 하 게 됩니다.

### <a name="4--returning-to-the-xbox-arena-ui"></a>4. Xbox 아레나 UI 반환합니다.

일치 하는 동안 (또는 잠재적으로 진행 중에서 일치를 플레이어의 요청에 대 한 응답에서) 이면 일치 가입 된 위치에서 Xbox 아레나 UI 돌아가려면 플레이어에 대 한 옵션을 제공 합니다. 이후 일치 결과 화면 또는 기타 게임 종료 UI에서이 수행할 수 있습니다.

아레나 UI 돌아가려면 **returnUri** 사용 하 여 프로토콜 활성화 URI에서에 전달 된 함수를 호출 합니다 `Windows::System::Launcher` 클래스. 사용자 컨텍스트를 포함 해야 합니다.

시작 API는 유니버설 Windows 플랫폼 (UWP) 게임에 보다 종말 게임 약간 다르게 노출 됩니다. 종말 버전의 API의 PFN도 무시할 수 있도록 제공 되어야 할 PFN를 허용 하지 않음 URI 활성화에 존재 하는 경우.

사용자 아레나 UI 돌아가려면 시대의 종말에 대 한 예제 코드는 다음과 같습니다.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

프로토콜 활성화 URI에 제공 된 경우 UWP 게임에서 반환 PFN을 **LauncherOptions** **TargetApplicationPackageFamilyName** 속성을 설정할 수 있습니다. 그러면 특정 앱이 시작 하 고 앱이 설치 되어 있지 않으면 이미 스토어에 사용자는 가져옵니다.

아레나 UI에 사용자를 반환 하는 UWP 앱에 대 한 예제 코드는 다음과 같습니다.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

아레나 UI를 호출한 후 타이틀 계속 실행 될 수는 동일한 화면에서 다른 프로토콜 활성화 이벤트를 대기 합니다. 이렇게 하면 플레이어의 재생 하는 데 다른 일치 하는 경우 타이틀은 준비 완료 이 신속 하 게 플레이어가 지나도 일치, 제목 및 Xbox 아레나 UI 전환 사용자 환경을 처리 합니다.

### <a name="handling-errors-and-edge-cases"></a>오류 처리 및 edge 사례

이전 섹션의 흐름 일치 하는 재생을 통해 골든 경로 설명 합니다. 많은 곳 예기치 않은 동작이 발생할 수 있습니다 위치나 문제가 발생할 수 있습니다. 이 섹션에서는 다양 한 잠재적인 오류 또는 edge의 경우 및 제목에이 처리 하기 위한 권장 사항을 제공 합니다.

#### <a name="game-session-not-found"></a>게임 세션을 찾을 수 없음

일치 하는 항목에 대 한 세션 ref 타이틀 시작 됩니다 MPSD에서 해당 세션을 요청할 때 오류가 발생 하는 클라이언트 이르러, 사용자 일치에 참가할 수 없는 하는지 나타내는 오류를 제공 합니다.

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>사용자가 재생 되는 일치 하는 가입 하려고 합니다.

사용자가 결과 보고 후 일치 하는 항목에 가입 하려고 하는 경우 해당 클라이언트 새 일치 하는 시작을 차단 하 고 오류와 함께 표시 합니다. 결과 모든 지니는지 확인 하기 세션의 멤버를 반복 하 여 보고 된 여부를 확인할 수는 `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) "완료"로 설정 합니다.

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>게임 클라이언트 p2p 연결을 설정할 수 없습니다.

타이틀 멀티 플레이어는 클라이언트 간에 개인 간 (p2p) 연결을 사용 하 여 릴레이 대 한 지원이 없는 경우 사용자에 게 함께 일치 하는 p2p 연결을 설정할 수 없는 인스턴스가 있을 수 있습니다.

클라이언트는 수 일치에 대 한 세션 검색 하지만 수 없는 다른 클라이언트에 연결에서 각 팀에 대 한 "그리기" 결과 보고 하는 경우 `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`). 이 Xbox Live 일치 하는 장소를 수행 하지 했습니다. 또한 p2p 연결이 실패 하 여 사용자는 토너먼트를 진행 하는 위치는 악용을 방지 합니다.

#### <a name="game-client-disconnects-mid-match"></a>게임 클라이언트의 연결을 끊습니다 중간 일치

가장 일반적인 오류 시나리오 중 하나에 일치 하는 항목을 재생 하는 동안 클라이언트 연결을 끊을 때입니다. 일치 하는 및 구현 체의 상태에 따라 가지이 경우를 처리 하기 위한 몇 가지 옵션이 있습니다.

* 토너먼트 일치는 한 비교 하나 또는 팀의 모든 멤버는 다른 클라이언트 및/또는 게임 서비스에서 연결을 끊을 경우 좋습니다 하는 경우 더 이상 연결 되어 있는 팀에 대 한 "손실"를 보고 합니다. 이렇게 하면 결과 보고 하지 않도록 하려면 손실 되는 일치에서 사용자가 연결을 끊은 실행할 수 있는 경우.
* 일치 하는 여러 멤버를 사용 하 여 팀 간에 이며 팀에 대 한 클라이언트의 하위 집합 분리 중간 일치 하는 경우 해당 지점에서 일치를 종료 또는 나머지 사용자를 진행할 수 있도록 선택할 수 있습니다. 일치를 계속 하기로 선택한 경우 일치 항목이 다시 가입에 연결 되지 않은 사용자도 필요에 따라 허용할 수 있습니다.

#### <a name="full-teams-not-present"></a>전체 팀 없음

타이틀 일치를 지 원하는 경우 두 개 이상의 팀 들과 일치 하는 타이틀을 실행 하 팀의 일부 멤버 실패 있는 경우 발생할 수 있습니다. 이 경우 해당 팀 구성원 없이 재생 하 고 필요에 따라 해당 팀 구성원 일치 하는 진행 중에서에 가입 하도록 허용 하는 부서의 구성원을 허용 하도록 선택할 수 있습니다.

또는 결과 자동으로 적용할 수 있습니다. 이렇게 하면 모든 참가자가 일치 하는 결과 적용 하기 전에 먼저 연결할 수 있는 기회를 제공 하도록 forfeitTimeout 때까지 기다립니다. 이렇게 하면 창을 토너먼트 타이틀을 업데이트 하는 대신에 대 한 게임 모드에 변경 사항으로 조정할 수 있습니다.

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>일치 길이가 arbitrationTimeout

**ArbitrationTimeout** 시간 일치 하는 항목이 적을 수의 최대 길이 보다 클 수에 대 한 값을 설정 합니다. 말한 타이틀 수 기능 모드 일치 길이 매우 긴 또는에 무제한입니다. 이러한 경우 고려해 중 하나:

* 토너먼트 플레이 시간의 고정, 최대 길이 사용 하 여 전용 모드를 제한 합니다.
* 시간 제한의 사용자에 게 알려주는 및 **arbitrationTimeout** 하기 전에 결과 보고 일치에 일종에 기반 합니다.

**ArbitrationTimeout** 만료 일치에 대 한 자동 결과 트리거하고, 때문에 전체 **arbitrationTimeout** 기간 제목에서 결과 보고 하기 전에 대기할 필요가 있습니다.  대신, 시간 버퍼 (예를 들어 **arbitrationTimeout** -1000ms가), 빌드 또는 별도 값을 사용 하 여 일치 최대 길이 제어 합니다.

#### <a name="other-cases"></a>다른 경우

다른 오류 사례에 대 한 일반적인 지침은 오류 사용자에 게 알리는 Xbox 아레나 UI에 사용자를 반환 하입니다.

## <a name="experience-requirements-and-best-practices"></a>환경 요구 사항과 모범 사례

아레나 타이틀 개별 일치에 의해서만 제공, 때문에 새 형식은 타이틀을 업데이트 하지 않고도 시간이 지남에 따라 도입 수 있습니다. 따라서 간단 하 고 여러 경쟁 작업할 수 있을 만큼 유연 통합된 제목을 지정 해야 하는 아레나에 대 한 초기 사용자 환경을 형식을 지정 합니다.

선택적으로 제목을 토너먼트 보다 통합 된 게임 환경 포함 되도록 토너먼트 허브에서 데이터를 사용할 수 있습니다.  이 기능은 'xbox::services::tournaments'에서 찾을 수 있습니다.  이러한 Api를 사용 하 여 기능을 해야 합니다.
* 제목 (예: 토너먼트 이름, 팀 이름) UI에에서 surface 토너먼트 세부 정보
* 토너먼트 제목에 대 한 검색을 제공 합니다.
* 사용자가 제목 아레나 일치 항목 사이 유지

## <a name="configuring-a-title-for-arena"></a>아레나 타이틀 구성

아레나에 대 한 타이틀을 사용 하려면 몇 가지 추가 단계가 필요한 Xbox 개발자 포털 (XDP) 또는 [파트너 센터](https://partner.microsoft.com/dashboard)에서 구성 합니다.

### <a name="enabling-arena-for-your-title"></a>아레나 타이틀에 대 한 설정

아레나를 활성화 하려면 XDP에 제목에 대 한 서비스 구성 페이지로 이동 하 고 '아레나'을 선택 합니다.

![](../../images/arena/arena-configure-xdp.png)

여기에서는 몇 가지 옵션을 해야 합니다.

* **아레나 활성화** -아레나가이 샌드박스에 대 한 사용 하도록 설정 하려면이 확인란을 선택 합니다.
* **아레나 기능** –이 섹션에는 샌드박스에서 토너먼트 사용자 생성을 사용 하도록 설정 하 고 동일한 토너먼트 참여할 수 있도록 여러 플랫폼에는 사용자가 교차 플레이 사용 하도록 확인란 포함 됩니다.
* **아레나 플랫폼** – 기반이 토너먼트 타이틀에 대 한 재생할 수 있는 플랫폼을 선택할 수 있습니다.
* **토너먼트 자산** – (이전 섹션에서 '멀티 플레이어 및 매치 메이 킹'.) 이 타이틀에 대 한 토너먼트 이미지입니다.

또한 아레나 Xbox Live 서비스 **토너먼트** 메뉴에 있는 파트너 센터에서 사용할 수 있습니다.

![파트너 센터에서 아레나 메뉴](../../images/arena/Arena_On_WDC.JPG)

변경 내용을 적용에 대 한 서비스 구성을 게시 해야 합니다. 아레나 구성 셀프 서비스 현재 UDC 통해 지원 되지 않습니다. UDC 서비스 구성에 대 한를 사용 하는 경우에 개발 계정 관리자와 상관 없이 별도 등록이 아레나를 사용 하 여 작동 합니다.

### <a name="setting-up-game-modes"></a>게임 모드 설정

게임 모드는 Xbox Live 기능 토너먼트 일치에 대 한 설정을 미리 구성 하는 게시자입니다. 이러한 게임 모드는 Xbox Live 타이틀은 토너먼트에 대 한을 적용 하는 규칙을 설정할 토너먼트 만들 때 사용 됩니다. 제목 게시자 하나 이상 게임 모드 타이틀에 대 한 사용자 생성 토너먼트에 필요 합니다. 게임 모드의 요소는 다음과 같습니다.

#### <a name="supports-ugt"></a>지원 UGT?

사용자 생성 토너먼트 (UGTs)와 함께 사용에 대 한 게임 모드를 사용할 수 있습니다. 타이틀 UGT를 지원 하도록 구성 된, 타이틀에 대 한 토너먼트 만들 때 Xbox Live 사용자가 선택할 수 있습니다 되도록 게임 모드를 표시할 수 있습니다.

#### <a name="team-size"></a>팀 크기

팀으로 토너먼트에 참여할 수 있는 사용자의 수입니다. 단일 사용자 제목 또는 토너먼트의 경우 게임 모드에는 하나의 팀 크기를 있습니다. 현재 아레나 토너먼트에 대 한 변수 팀 크기를 지원 하지 않습니다.

#### <a name="forfeittimeout"></a>forfeitTimeout

일치 하는 시작 시간, 일치 하는 경우 그에 대해 플레이어 표시 되지 않음 취소 되기 전 밀리초의 수입니다 (즉, 경우 없는 플레이어는 자체 세션에서 활성으로 설정). 타이틀을 시작 하 고 알림 메시지를 표시 하는 시간 으로부터 토너먼트 일치 갖도록를 플레이어에 대 한 충분 한 시간 이상 되는 시간을 설정 합니다.

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

일치 하는 더 이상 결과가 허용 되는 시간을 시작한 후 밀리초의 수입니다. Forfeit 시간 제한 설정 뿐만 아니라 긴 일치 보다 긴 시간을 적을 수 있습니다. 이렇게 하면 플레이어 forfeit 시간 직전 재생을 시작 하는 경우에 여전히 있는 충분 한 시간에 일치를 완료 합니다. 결과가 **arbitrationTimeout**쯤에 보고 되는 일치 모든 참가자 손실을 받습니다. Xbox 아레나도 기다렸다가 **arbitrationTimeout** 까지 결과를 보고 하는 모든 활성 구성원에 대 한 중재가 시작 합니다.

이 제한 시간은 역할 백업을 경우 문제가 발생 하 고 결과 보고 하는 플레이어에 대 한 경우가 하지 않습니다. 전체 토너먼트 멈춤, 대신이 제한 시간은 토너먼트 대기 시간에 상한을 둡니다. 이런 이유로 것 설정 해서는 안됩니다 너무 높아 라운드 각 토너먼트의 최대 길이 결정 하기 때문에 있습니다.

#### <a name="name"></a>이름

게임 모드에 대 한 사용자에 게 이름입니다. 이 값은 지역화할 수 있습니다.

#### <a name="description"></a>설명

게임 모드에 대 한 사용자에 게 설명입니다. 이 값은 지역화할 수 있습니다.

#### <a name="custom"></a>사용자 지정

게임 모드에 대 한 **사용자 지정** 부분은 속성 모음 개발자 토너먼트 일치에 대 한 제목 관련 구성 설정을 삽입할 수 있습니다. 사용자 지정 섹션의 일부로 정의 된 값에서 일치 MPSD 세션에 작성 된 `/properties/custom/`.

현재 게임 모드 설치 XDP 또는 UDC 통해 지원 되지 않습니다. 타이틀에 대 한 게임 모드를 만들려면 개발자 계정 관리자에 게 문의 합니다.

### <a name="requirements-for-the-session-template"></a>세션 템플릿에 대 한 요구 사항

제목 개발자는 일치 하는 세션을 만들 때 사용 하기 위해 대상에 대 한 세션 템플릿을 제공 해야 합니다. 서식 파일의 내용에 대 한 몇 가지 기대 가지가 있습니다.

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

그러나 원하는 여기에 나와 있지 다른 속성을 설정할 수 있습니다.

아레나 세션은 항상 버전 1입니다. "TournamentGame"를 **inviteProtocol** 설정 일치 준비 알림을 토너먼트 참가자에 게 보낼 수 있습니다. **memberInitialization** QoS를 사용 하지 않도록 null로 설정 됩니다. 세션 일치 하는 항목을 나타내며 중재 기능은 보고 결과 위해 필요 하기 때문에 게임 플레이 접근 권한 값을 설정 해야 합니다. 세션의 작업을 방해 하 게 되므로 **큰**, **브로드캐스트**및 **blockBadMsaReputation** 기능을 비활성화 해야 합니다.

타이틀의 템플릿을 사용 하 여 모든 토너먼트의 고정 된 값이 설정에 대 한 템플릿 사용자 지정 섹션에 고유한 설정을 지정할 수 있습니다. 예를 들면 다음과 같습니다.

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

마지막으로, **maxMembersCount** 시스템 설정은 필수입니다. 토너먼트 검색에서 재생할 수 있는 플레이어의 총 수로 설정 합니다. 플레이어 수에는 게임 모드 설정에 의해 제한 될 수, 세션에 설정 된 값 타이틀에 대 한 지원 플레이어의 최고 총 수를 나타내는 있는지 확인 합니다. 예를 들어, 일치 하는 게임에서 지 원하는 플레이어의 최대 5 대 5 이면 **maxMembersCount** 10으로 설정 합니다. 이 MPSD 템플릿은 임의 개수의 **maxMembersCount**최대 플레이어의 일치 항목을 지원 하기 위해 사용할 수 있습니다.
