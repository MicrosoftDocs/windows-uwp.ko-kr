---
title: Arena 제목 통합 가이드
description: Xbox Live Arena 플랫폼에 대 한 제목의 빌드하는 방법을 알아보세요.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, arena, 토너먼트
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659888"
---
# <a name="arena-title-integration-guide"></a>Arena 제목 통합 가이드

## <a name="introduction"></a>소개

Xbox 용 Arena 플랫폼 토너먼트 주최측 TOs ()를 만들어 타이틀 Arena 지원에 대 한 토너먼트 운영할 수 있습니다. Arena는 Xbox Live를 게시자 및 사용자 토너먼트와 Arena와 통합 하는 제 3 자 도움말을 지원 합니다. 단일 제거 하거나, 스위스 같은 토너먼트의 스타일을 따라 결정 됩니다.

이 항목에서는, 제목 개발자을 제목에 분야에 대 한 지원을 구현 하는 방법 설명 합니다. 게임 Arena 사용 하도록 설정 되 면을 선택한 모든 지원 되는 토너먼트 형식을 사용 하 여 작동 합니다. TO 토너먼트 구조에 따라 제목에 대 한 일치 항목을 조정합니다. 그런 다음 Arena 지원 제목 게임 내에서 각 일치 항목에 적절 한 사용자를 배치 하 고 TO에 일치 결과 다시 보고.

## <a name="overview"></a>개요

Xbox Arena Xbox 멀티 플레이 게임 개발에 익숙한 개념을 사용합니다. Xbox 멀티 플레이 시스템과 잘 모르는 경우 계속 하기 전에 해당 설명서를 검토 하는 것이 좋습니다. 프로세스 멀티 플레이 게임에는 초대를 수락 하는 사용자의 것과 비슷합니다. 사용자는 Arena 토너먼트에서 일치 하는 항목을 재생 하는 데 시간이 되 면 알림을 사용자에 게 표시 됩니다. 알림을 수락 하면 표준 Arena 프로토콜 활성화 이벤트를 받기 위해을 제목입니다. 제목이 이미 실행 중이 아닌 경우이 시간에 시작 됩니다.

토너먼트 일치 항목 자체는 멀티 플레이 게임 세션 디렉터리 (MPSD) 세션을 사용 하 여 조정 됩니다. 분야에서의 경우 세션에 제목별 대신 Arena 플랫폼에 의해 생성 됩니다. 이 세션 템플릿 및 사용자가 제목 게시자로 생성 된 추가 게임 구성을 사용 하 여 이루어집니다. 토너먼트 일치 참가자가 세션에 참가 이미 됩니다. 제목 게시자로 보고 하는 결과 대 한 중재를 지 원하는 데 분야에 대 한 세션 템플릿을 구성 해야 합니다. 이 문서의 뒷부분에서 세션에 대 한 모든 요구 사항은 포함 됩니다. 제목에 필요한 모든 추가 세션을 만들려면 무료 이기도 합니다.

제목 세션을 사용 하 여 일치 하 고 보고서 결과를 설정 합니다. 프로토콜 활성화에 응답 하 고 MPSD 세션과 상호 작용 통합 Arena 타이틀에 필요한 최소 수준입니다. 검색 및 토너먼트 신청 Xbox Arena UI를 통해 지원 됩니다. 필요에 따라 제목 사용을 보다 편리 하 게 제목에서 토너먼트 허브 서비스에서는 토너먼트에 대 한 추가 정보를 가져올 수 있습니다. 예를 들어 토너먼트 허브를 사용 하 여 제목 팀 이름, 같은 컨텍스트를 표시할 수 있으며 사용자에 대 한 새 일치 세션 검색. 이 데이터 흐름에는 다음 다이어그램에서 녹색 화살표로 표시 하 고에서 자세히 설명 되어는 [요구 사항 및 모범 사례를 경험](#experience-requirements-and-best-practices) 섹션입니다. 가리키더라도 화살표는 토너먼트 내에서 일치를 이동할 때 팀에서 세션 참조 시간이 지남에 따라 변경 되는지 나타냅니다.

![](../../images/arena/tournament-flow.png)


Arena 프로토콜 활성화 URI는 일치 하 고 제목 일치를 위로 가져갈 때 호출할 수 있는 딥 링크에 대 한 세션의 토너먼트에 대 한 정보를 포함 합니다. 딥 링크 Xbox Arena UI에 사용자를 반환합니다. 이러한 URI 구성 요소에서 자세히 설명 되어는 [정품 인증 프로토콜](#1protocol-activation) 부분 [Arena 통합에 대 한 기본 요구 사항을](#basic-requirements-for-arena-integration)합니다.

## <a name="basic-requirements-for-arena-integration"></a>Arena 통합에 대 한 기본 요구 사항

이 섹션을 제목 Arena를 지원 하기 위한 최소 요구 사항을 통합에 대 한 기술 지침 및 세부 정보를 제공 합니다. 이 스타일의 통합은 다음 개요 다이어그램에서 주황색 화살표로 표시 된 데이터 흐름을 활용 합니다.

![](../../images/arena/arena-data-flow.png)

프로그램 제목은 Xbox Arena UI에서 프로토콜 활성화 합니다. 이 알림, 토너먼트, 세부 정보 페이지 또는 일치 하는 모든 다른 항목 지점에서에서 발생할 수 있습니다. 사용자 일치에 참여 하는 경우 발생 하는 단계는:

1.  프로그램 제목은 Xbox Arena UI를 통해 프로토콜 활성화 합니다.
2.  단일 일치 항목을 재생 하려면 활성화 매개 변수를 사용 하는 제목입니다.
3.  통해 일치 하는 경우 제목을 MPSD 게임 세션에 결과 보고 합니다.
4.  프로그램 제목 사용자 Arena UI를 반환 하는 옵션을 제공 합니다.

다음 섹션에서는 각 세부 정보에서 이러한 단계를 통해 이동합니다.

### <a name="1--protocol-activation"></a>1.  프로토콜 활성화

Arena UI 작업을 활성화 하 여 프로토콜-제목이 일치 하는 사용자에 대 한 준비 되 면 시작 됩니다. 두 가지 경우가 있습니다: 이때을 제목을 실행할 수 있습니다 또는 실행 중인 이미 합니다. 두 경우 모두 제목을 멀티 플레이 게임을 하는 초대를 수락 하는 사용자에 대 한 응답에서 활성화 될 때 발생 하는 것과 비슷합니다.

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>제목 프로토콜 활성화 시에 시작 된 경우

합니다 **Activated** 을 제목 시작 될 때 처음으로 이벤트가 발생 합니다. 해당 초기 활성화 Arena 프로토콜을 활성화 하는 경우에 title는 토너먼트에서 재생 하는 사용자가 시작 되었습니다. 메뉴 및 로그인 화면을 무시 하 여 일치 항목으로 가능한 한 빨리 건너뛸 프로그램 타이틀이 합니다. 참여 하는 사용자의 Xbox 사용자 ID (XUID) 활성화 URI에서에서 제공 되 고에 이미 로그인 해야 합니다.

#### <a name="if-your-title-is-already-running"></a>제목이 이미 실행 중인 경우

합니다 **Activated** 을 제목 이미 실행 되는 동안 이벤트 Arena 프로토콜 활성화를 사용 하 여 발생할 수도 있습니다. 합니다 **활성화** 이벤트 (일치 항목을 나타내는 알림에서 역할 또는 Xbox Arena UI에서 일치 항목으로 이동) 하는 명시적 사용자 동작에만 시작 됩니다. 메뉴 화면의 제목을 기다리는 경우 토너먼트 일치 항목으로 바로 이동할 것입니다. 제목 게임 플레이 중 프로토콜 활성화를 수신 하는 경우 사용자에 게 게임을 두고 가능한 것이 가장 좋습니다 토너먼트 일치 항목을 입력 하는 옵션을 제공 하도록. 필요한 경우 해당 게임을 저장할 수 있는 기회를 사용자에 게 제공 합니다.

프로토콜 정품 인증을 통해을 제목에 전달할지를 `CoreApplicationView.Activated` 이벤트입니다. 경우는 `IActivatedEventArgs.Kind` 이벤트 인수의 속성 `Protocol`, 활성화가 프로토콜 활성화 및 인수를 캐스팅할 수는 `ProtocolActivatedEventArgs` 프로토콜 활성화 URI를 통해 사용할 수 있는 클래스는 `Uri` 속성입니다.

프로토콜 활성화 URI에서 XUID 확인 프로그램 타이틀이 합니다. 현재 플레이어, 일치 하지 않는 경우 사용자 컨텍스트를 전환 하려면 제목도 해야 합니다.

#### <a name="the-protocol-activation-uri"></a>프로토콜 활성화 URI

Uri 템플릿은 다음과 같습니다.

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

콘솔에서 연대 제목에 대 한 정품 인증 체계를 약간 다릅니다.

```
ms-xbl-{titleIdHex}://
```

초대와 동일합니다. 이 작업을 위해 제목 ID 하므로 필요한 경우 선행 0을 포함 합니다 8 개의 16 진수 문자 여야 합니다.

먼저 했는지를 `Uri.Host` ("://" 구분 기호 바로 다음) 이름이 "토너먼트"입니다. 게임 초대 등 다른 기능의 활성화에서 Arena의 프로토콜 활성화는 구별 하는 방법입니다.

쿼리 문자열 인수를 URL로 인코딩된 되며 대/소문자 구분 됩니다. 제목, 매개 변수 순서에 의존해 서는 안 및 인식할 수 없는 매개 변수를 무시 해야 합니다.


Paramater(s) | 설명
--- | ---
**action** | 만 지원 되는 동작에는 "joinGame"입니다. 경우는 **동작** 없습니다 또는 다른 값에 대 한 지정 된 경우 활성화를 무시 합니다.
**joinerXuid** | 합니다 **joinerXuid** 토너먼트 일치 하는 재생 하려고 로그인 사용자의 XUID 됩니다. 제목에는이 사용자의이 컨텍스트로 전환 허용 해야 합니다. 경우는 **joinerXuid** 서명 되지 않은 제목에서 사용자 계정 선택기를 표시 하 여 요청 해야 합니다. 항상 XUIDs는 10 진수로 표현 됩니다.
**organizerId, tournamentId** | **organizerId** 하 고 **tournamentId**폼 토너먼트 일치 하는 연결 된 고유 식별자를 결합 하는 경우. 제목에 표시 하려는 경우 토너먼트 허브에서의 토너먼트에 대 한 자세한 정보를 검색 하려면이 식별자를 사용 합니다.
**teamId** | **teamId** 토너먼트 컨텍스트에서 팀에 대 한 고유 식별자는 사용자 (에 지정 된는 **joinerXuid** 매개 변수)의 구성원임을 확인 합니다. 같은 합니다 **organizerId** 및 **tournamentId** 매개 변수를 사용할 수는 **teamId** 토너먼트 허브에서 팀에 대 한 정보를 선택적으로 검색할 합니다.
**scid, templateName, name** | 함께 이러한 세션을 식별 합니다. 다음은 세션 MPSD URI 경로에 동일한 세 매개 변수입니다.</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`을 차례로 클릭합니다.</br></br>세 가지 매개 변수를 있을 때 XSAPI에 `multiplayer_session_reference `생성자입니다.
**returnUri, returnPfn** | 합니다 **returnUri** Xbox Arena UI에 사용자를 반환할 프로토콜 활성화 URI입니다. 합니다 **returnPfn** 매개 변수 수도 없을 수도 있습니다. 인 경우 처리 하기 위한 것에 앱에 대 한 제품 제품군 이름 (PFN)는 **returnUri**합니다. 이러한 값을 사용 하는 방법을 보여주는 샘플 코드를 보려면 [Xbox Arena ui 반환](#4returning-to-the-xbox-arena-ui)합니다.

### <a name="2--playing-the-match"></a>2.  일치 하는 재생

토너먼트 이끌이가 MPSD 세션이 만들어지면 사용자는 해당 세션의 비활성 멤버를으로 설정 됩니다. 제목을 설정 해야 합니다 즉시 플레이어를 활성으로 사용 하 여 `multiplayer_session::join()`입니다. Xbox Live 및 다른 사용자가 제목에 재생할 준비가 되었습니다. 일치 하는 사용자가 표시 됩니다.

일치 하는 시작 시간은에 있는 세션에서 `/servers/arbitration/constants/system/startTime` 표준 UTC 형식으로 날짜 및 시간 값으로 (예를 들어, "2009-06-15T13:45:30.0900000Z"). (시작 시간으로 XSAPI 제공 됩니다. `multiplayer_session_arbitration_server::arbitration_start_time()`1703 XDK에서 시작)입니다. (시작 시간 동시 포함)가을 세션으로 시작 시간 전에 만들 수 있습니다. 일치 알림 제목을 활성화 되지 않음 가져오기 시작 시간 이전에 있으므로 시작 시간에 일치 참가자에 게 전송 됩니다. 시작 시간 MPSD 일치 하는 항목을 보고 결과 허용 하는 가장 빠른 시간 이기도 합니다.

제목 각 멤버를 살펴봅니다 `/member/{index}/constants/system/team` 속성 (`multiplayer_session_member::team_id()`)에 각 사용자는 팀 파악 합니다.

또한을 제목 지도 모드 등의 일치 설정을 확인 하는 세션을 사용 합니다. 이러한 title 별 설정은 세션 템플릿 또는 사용자 지정 상수로 토너먼트 정의의 일부로 설정할 수 있습니다. (자세한 내용은 [분야에 대 한 제목을 구성](#configuring-a-title-for-arena).) 예를 들면 다음과 같습니다.

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

개체 JSON (JavaScript Notation) 개체로 세션에서 이러한 설정을 사용 하 여 검색할 수 있습니다는 `multiplayer_session_constants::session_custom_constants_json()` API.

일반적으로 제목 Arena 세션 자체 MPSD 세션을 취급 하는 동일한 방식으로 처리 해야 합니다. 예를 들어, 핸들을 만들고 RTA 알림에 대 한 구독 수 것입니다. 하지만 몇 가지 차이점이 있습니다. 예를 들어을 제목 게임 세션의 명단을 변경 하거나 세션의 서비스 품질 (QoS) 기능을 사용할 수 없습니다 및 중재에 참여 해야 합니다. 세션에 대 한 자세한 내용은이 문서의 뒷부분에 나와 있습니다.

### <a name="3--reporting-results"></a>3.  결과 보고합니다.

일치 결과 중재 라는 기능을 사용 하 여 Arena를 통해 세션에 다시 보고 됩니다. 중재는 세션을 사용 하 여 안전 하 게 일치 하 고 보고서 결과 재생 하는 프레임 워크입니다.

프로토콜-정품 인증 단계에서 제목에 제공 된 세션을 중재 프레임 워크를 적용 하는 고정된 일정에 즉 arbitrated 세션을 됩니다. 이 다이어그램에서는 중재 타임 라인을 보여 줍니다.

![](../../images/arena/arbitration-timeline.png)

하나 이상의 플레이어를 돌려받지 못하게 시간 시작 시간 전에 세션에 활성 상태 여야 합니다 (`multiplayer_session_arbitration_server::arbitration_start_time()`)를 돌려받지 못하게 제한 또한 있습니다. 돌려받지 못하게 제한 시간은 세션의 시간 (밀리초)에 숫자로 `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`). 아무도 돌려받지 못하게 시간 전에 활성으로 세션에 조인 하 고, 일치 하는 취소 됩니다.

밀리초에서의 중재 제한 시간은 `/constants/system/arbitration/arbitrationTimeout` (`multiplayer_session_constants::arbitration_timeout()`). 중재 시간 시작 시간 + 중재 시간 제한 값 이며 사용 되는 플레이어 있어야 일치 항목을 완료 하 고 결과 보고 시간을 나타냅니다. 이 값은 게시자가 세션 템플릿 또는 게임 모드에서 설정 됩니다. 제목 일치 항목을 완료할 수에 대 한 요구 사항이 많은 시간을 허용 하도록 설정 합니다.

제목을 언제 든 지 시작 시간 사이의 중재에 결과 보고할 수 있습니다. 중재 언제 든 지를 돌려받지 못하게 시간 및 세션의 모든 활성 멤버 결과 전송한 후 중재 시간 사이 발생 합니다. 예를 들어, 하나의 멤버만 돌려받지 못하게 시 세션에서 활성 상태인 경우 수을 게시할 결과 및 중재 발생 합니다. 얼마나 많은 결과 사용할 수 있는 중재 시간에 관계 없이 중재 아직 하지 않은 경우 발생 합니다. 결과 없음 중재 시간에 도달한 경우에 제출 될 경우 일치 하는 모든 참가자가 손실을 제공 됩니다.

또한 언제 든 지 중재 arbitrated 결과 간단히 작성 하 여 강제로 게임 서버도 가능 합니다.

중재 이미이 세션에서 사용자가 인지 (하거나 중재 제한 시간이 만료, 게임 서버 세션을 조정 하면 런타임에 조인)을 제목 일치를 종료 하 고 사용자에 게 arbitrated 결과 표시 합니다.

중재 결과 항상 모든 팀에 대 한 결과 포함합니다. 세션에 결과 기록 하는 개별 된 플레이어, 뿐 아니라 해당 팀의 결과 포함 되지만 모든 팀에 대 한 결과의 전체 집합입니다.

JSON의 결과 다음과 같습니다.

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

이 예제에서는 팀 Id는 "red" 및 "파랑"입니다. 레드 팀 둘째 첫 번째 및 블루 팀에서 제공 합니다. 다른 가능한 값 **결과** 됩니다.

* "win"
* "손실"
* "그리기"
* "noshow"

하는 경우 **결과** 해당 팀을 위해 생략 되어 "rank", 순위 이외의 모든 항목을 말합니다.

개별 사용자 일치 결과를 쓸 `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`). 다음 예제에서는 제목을 수 만들어 프로그램 제목은 팀 기반 가정 하 고 결과 집합을 제출 하는 방법을 보여 줍니다 (즉, 모든 플레이어는 하나의 팀에서).

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

MPSD 배치의 최종 결과 중재 발생 한 후 `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`), 다음과 같이 다른 몇 가지 속성과 함께 합니다.

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

에 대 한 가능성 **resultState** 포함:

* "completed": 모든 활성 플레이어 결과 보고 합니다.
* "partialresults": 전부는 아니지만 일부 활성 플레이어 중재 제한 시간 전에 결과 보고 합니다.
* "noresults": 활성 플레이어 하나도 중재 제한 시간 전에 결과 보고 했습니다.
* "canceled": 없는 활성 플레이어를 돌려받지 못하게 제한 시간 전에 도착 합니다.

"Noresults" 및 "취소" 경우 결과 생략 합니다.

합니다 **resultSource** MPSD 개별 플레이어에서 보고 결과 기반으로 중재를 수행 하는 경우 "중재" 됩니다. 게임 서버를 우회 중재 /servers/arbitration/properties/system 자체를 쓸 수 있습니다. 나타냅니다.이 경우는 **resultSource** "서버의"입니다. 게임 서버를 다시 작성도 가능 (즉, 수정 하거나 조정)는 결과 집합 소문자 **resultSource** "조정"입니다.

합니다 **resultConfidenceLevel** 합의 결과에서 수준을 나타내는 0에서 100 까지의 정수입니다. 100은 모든 플레이어 동의 의미 합니다. 0 없습니다 합의 했습니다 및 결과 기본적으로 임의로 선택 제출 하는 것을 의미 합니다. 중재 결과 설정 하는 게임 서버를 설정 합니다 **resultConfidenceLevel**-100에 일반적으로 게임 서버 자체에 몇 가지 이유로 하는지 (예를 들어 자체 당 플레이어의 결과 보고 하지 않는 한 투표 프로시저)입니다. 설정 된 **resultConfidenceLevel** 결과 조정 된 결과에 대 한 신뢰성을 반영 하도록 조정 됩니다는 경우에 합니다.

경우는 **resultSource** "중재"은 (및 **resultState** "completed" 또는 "partialresults"), 결과 하나 이상의 플레이어 보고 결과의 정확한 복사본을 합니다.

### <a name="4--returning-to-the-xbox-arena-ui"></a>4.  Xbox Arena UI 반환합니다.

통해 (또는 잠재적으로 일치 하는 진행에서을 중단할 플레이어의 요청에 대 한 응답) 일치 하는 경우 일치 하는 가입 된 위치에서 Xbox Arena UI에 반환할 플레이어에 대 한 옵션을 제공 합니다. 이 사후 일치 결과 화면 또는 다른 게임의 최종 UI에서 수행할 수 있습니다.

Arena UI 돌아가려면 호출을 **returnUri** 전달 된 하 여 프로토콜 활성화 URI에서 사용 하 여는 `Windows::System::Launcher` 클래스입니다. 사용자 컨텍스트를 포함 해야 합니다.

시작 API는 유니버설 Windows 플랫폼 (UWP) 게임 보다 연대 게임 약간 다르게 노출 됩니다. PFN을도 무시할 수 있도록 연대 버전의 API 제공 해야 PFN을 허용 하지 않습니다 활성화 URI에 있는 경우.

Arena UI에 사용자를 반환할 달력 종류에 대 한 예제 코드는 다음과 같습니다.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

UWP 게임을 설정할 수는 **TargetApplicationPackageFamilyName** 속성을 **LauncherOptions** 반환 PFN 프로토콜 활성화 URI에 제공 된 경우에 합니다. 그렇게 하면 특정 앱을 실행 하 고는 사용자가 앱을 아직 설치 되지 않은 경우 스토어로 이동 하 게 됩니다.

Arena UI에 사용자를 반환 하는 UWP 앱에 대 한 예제 코드는 다음과 같습니다.

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

Arena UI를 호출한 후에 제목 계속 실행 될 수는 동일한 화면에서 다른 프로토콜 활성화 이벤트를 대기 합니다. 따라서 플레이어를 재생 하려면 또 다른 일치 항목이 있으면을 제목 준비가 되었습니다. 이 속도 높여 사용자 환경을 줍니다 플레이어 면에서 일치 항목을 보내 주신 제목과 Xbox Arena UI를 전환 합니다.

### <a name="handling-errors-and-edge-cases"></a>오류 처리 및에 지 사례

이전 섹션의 흐름은 일치 하는 재생을 통해 골든 경로 설명 합니다. 가지 예기치 않은 동작이 발생할 수 있습니다 하거나 문제가 발생할 수 있는 여러 위치가 있습니다. 이 섹션에서는 다양 한 잠재적인 오류 또는 지 사례를 설명 하 고 제목에 처리 하기 위한 권장 사항을 제공 합니다.

#### <a name="game-session-not-found"></a>게임 세션을 찾을 수 없음

제목이 일치 하는 항목에 대 한 세션 ref가 시작 되어 MPSD에서 해당 세션을 요청할 때 오류가 발생 하는 클라이언트 다음 실패를 나타내는 사용자를 일치 항목을 연결할 수는 해당 오류가 제공 합니다.

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>사용자는 재생 된 일치 하는 조인 하려고 합니다.

사용자가 결과 보고 된 후 일치 하는 조인 하려고 하는 경우 새 일치 항목부터 해당 클라이언트를 차단 하 고 오류가 발생 하 여 사용자를 제공 합니다. 결과 경우 보려는 세션의 멤버 전체 반복에 의해 보고 되었습니다 여부를 확인할 수 있습니다는 `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) "완료"로 설정 합니다.

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>게임 클라이언트가 p2p 연결을 설정할 수 없습니다.

멀티 플레이 게임에 대 한 클라이언트 사이 개인 간 (p2p) 연결을 사용 하 여 제목, 릴레이 대 한 지원을 포함 하지 않는 경우 함께 일치 하는 사용자 p2p 연결을 설정할 수 없는 인스턴스가 있을 수 있습니다.

클라이언트는 일치 항목에 대 한 세션을 검색할 수 없습니다. 다른 클라이언트에 연결 하지만 아래에 있는 각 팀에 대 한 "그리기" 결과 보고 하는 일을 할 경우 `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`). 이 나타냅니다 Xbox Live에 일치 하는 위치를 취하지 않았습니다. 또한 여기서 사용자가 이동할 수는 토너먼트 p2p 연결 실패를 강제 적용 하 여 악용을 방지 합니다.

#### <a name="game-client-disconnects-mid-match"></a>게임 클라이언트는 중간 일치 하는 연결을 끊습니다.

가장 일반적인 오류 시나리오 중 하나에 일치 하는 항목을 재생 하는 동안 클라이언트 연결을 끊을 때입니다. 구현 및 일치 하는 상태에 따라 가지이 사례를 처리 하기 위한 몇 가지 옵션이 있습니다.

* 토너먼트 일치 하는 비교 하나를 하나인 경우 좋습니다 팀의 모든 멤버는 다른 클라이언트 및/또는 게임 서비스에서 연결을 끊을 경우 더 이상 연결 하는 팀에 대 한 "손실" 보고 합니다. 이렇게 하면 사용자가 수에서 보고 되 고 결과 방지 하기 위해 손실 되는 일치에서 연결을 끊도록을 적용할 수 있는 경우를 않습니다.
* 일치 하는 여러 멤버를 사용 하 여 팀 간에 팀에 대 한 클라이언트의 하위 집합을 중간 일치를 분리 하는 경우 일치 하는 시점 종료 또는 나머지 사용자에 게 계속할 수 있도록 선택할 수 있습니다. 검색을 계속 하려는 경우 일치 하는 다시 가입 되도록 연결이 끊긴 사용자 또한 필요에 따라 허용할 수 있습니다.

#### <a name="full-teams-not-present"></a>전체 팀 없음

제목 일치를 지 원하는 경우 둘 이상의 팀을 사용 하 여 팀의 일부 멤버는 일치 항목을 제목 시작 되지 않습니다 여기서는 경우 발생할 수 있습니다. 이 경우 해당 팀 멤버 없이 재생 하 고 필요에 따라 진행에서 일치 항목을 조인 하는 팀 멤버를 허용 하려면 팀의 멤버를 허용 하도록 선택할 수 있습니다.

또는 결과 자동으로 적용할 수 있습니다. 이렇게 하면 모든 참가자가 결과 적용 하기 전에 일치 항목을 연결할 수 있는 기회를 제공 하는 forfeitTimeout 시간까지 기다립니다. 이 게임의 제목을 업데이트 하는 대신 토너먼트 모드를 변경 하 여 창을 조정할 수 있습니다.

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>일치의 길이 초과 arbitrationTimeout

값을 설정 **arbitrationTimeout** 일치를 수행할 수도 있습니다 수 시간의 최대 길이 보다 커야 합니다. 즉을 제목 있습니다 기능 모드는 가능한 일치 항목 길이 매우 긴 또는는 최대값은 없습니다. 이러한 경우에 고려해 야 하나.

* 토너먼트 play 시간의 고정, 최대 길이 사용 하 여 전용 모드를 제한 합니다.
* 시간 제한의 사용자에 게 알리는 및 하기 전에 결과 보고 합니다 **arbitrationTimeout** 를 일치에서 일종의 기반입니다.

때문에 **arbitrationTimeout** 만료 일치 항목에 대 한 자동 결과 트리거할 전체 알리기 **arbitrationTimeout** 제목에서 결과 보고 하기 전까지 기간입니다.  대신 시간 버퍼에 작성 (예를 들어 **arbitrationTimeout** -1000ms), 또는 별도 값을 사용 하 여 최대 일치 수가 길이 제어 합니다.

#### <a name="other-cases"></a>다른 경우

다른 오류 사례에 대 한 일반적인 지침은 오류의 사용자에 게 알립니다 Xbox Arena UI에 사용자를 반환 하입니다.

## <a name="experience-requirements-and-best-practices"></a>환경 요구 사항 및 모범 사례

Arena을 제목 개별 일치만 제공, 때문에 프로그램 제목에 대 한 업데이트를 요구 하지 않고 시간이 지남에 따라 새 형식에 도입할 수 있습니다. 따라서 초기 사용자 환경이 통합된 제목 간단 하 고 여러 경쟁을 작업할 수 있을 만큼 유연 해야 하는 분야에 대 한 서식을 지정 합니다.

필요에 따라 게임에서 환경의 보다 통합 된 부분이 토너먼트 있도록 제목에 토너먼트 허브에서에서 데이터를 사용할 수 있습니다.  이 기능은 'xbox::services::tournaments'에서 찾을 수 있습니다.  이러한 Api를 사용 하는 기능을 해야 합니다.
* 제목 UI (토너먼트 이름, 팀 이름 등)에 노출 토너먼트 세부 정보
* 토너먼트 제목에 대 한 검색을 제공 합니다.
* 사용자가 제목 Arena 일치 항목 중간에 유지

## <a name="configuring-a-title-for-arena"></a>분야에 대 한 제목을 구성

제목에 Arena를 사용 하려면 몇 가지 추가 단계가 필요한 Xbox 개발자 포털 (XDP)에서 구성 하는 경우 또는 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.

### <a name="enabling-arena-for-your-title"></a>제목에 대 한 Arena를 사용 하도록 설정

Arena 있도록 XDP에 제목에 대 한 서비스 구성 페이지로 이동 하 고 'Arena'를 선택 합니다.

![](../../images/arena/arena-configure-xdp.png)

여기에서는 몇 가지 옵션이 있습니다.

* **Arena 사용** –이 샌드박스에 대 한 Arena를 사용 하도록 설정 하려면이 확인란을 선택 합니다.
* **Arena 기능** -이 섹션에서는 확인란은 샌드박스에서 토너먼트 사용자 생성을 사용 하도록 설정 하는 데 교차 동일한 토너먼트 참여할 수 있도록 여러 플랫폼에서 사용자를 허용 하는 게임을 사용 하도록 설정 합니다.
* **Arena 플랫폼** –을 제목에 대 한 토너먼트 재생할 수 있는 플랫폼을 선택할 수 있습니다.
* **토너먼트 자산** – (이전의 '멀티 플레이 게임 및 결혼 정보 회사 연결' 섹션에서.) 이들은 제목에 대 한 토너먼트 이미지입니다.

Arena의 파트너 센터에서 사용할 수도 있습니다는 **토너먼트** Xbox Live 서비스 아래에 있는 메뉴.

![파트너 센터에서 arena 메뉴](../../images/arena/Arena_On_WDC.JPG)

서비스 구성 변경 내용을 적용 하려면 게시 해야 합니다. 셀프 서비스 Arena 구성 현재 UDC를 통해 지원 되지 않습니다. UDC를 서비스 구성에 사용 하는 경우 사용 하 여 개발 계정 관리자 온 보 딩 Arena 사용 하 여 작동 합니다.

### <a name="setting-up-game-modes"></a>게임 모드 설정

게임 모드는 토너먼트 일치에 대 한 설정을 미리 구성 하기 위한 게시자를 사용 하도록 설정 하는 Xbox Live 기능입니다. 이러한 게임 모드는 토너먼트 만들면 Xbox Live 및는 토너먼트 제목에 적용할 규칙을 설정 하는 데 사용 됩니다. 제목 게시자로 하나 이상의 게임 모드를 제목에 대 한 사용자에서 생성 된 토너먼트를 사용 하도록 설정 해야 합니다. 게임 모드의 요소는 다음과 같습니다.

#### <a name="supports-ugt"></a>UGT 지원?

사용자가 생성 토너먼트 (UGTs) 사용에 대 한 게임 모드를 사용할 수 있습니다. 제목 UGT를 지원 하도록 구성 됩니다, 경우에 제목에 대 한 토너먼트를 만들 때 Xbox Live 사용자가 선택할 수 있습니다 게임 모드를 표시할 수 있습니다.

#### <a name="team-size"></a>팀 규모

팀으로 토너먼트에 참여할 수 있는 사용자의 수입니다. 단일 사용자 제목 또는 토너먼트, 게임 모드를 하나의 팀 크기가 되었습니다. 현재 arena 토너먼트에 대 한 변수 팀 크기를 지원 하지 않습니다.

#### <a name="forfeittimeout"></a>forfeitTimeout

일치 하는 시작 시간, 일치 하는 경우 플레이어가 표시 하기 위해 취소 되기 전 후 밀리초의 수입니다 (즉, 경우 없는 플레이어는 자체 세션에서 활성으로 설정). 이 플레이어가 제목 시작 되 고 알림 메시지 표시 시간에서 토너먼트 일치로 가져오기에 대 한 충분히 긴 이상 시간을 설정 합니다.

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

일치 하는 시작 시간, 지나면 더 이상 결과가 수락 후 밀리초의 수입니다. 돌려받지 못하게 제한 시간 설정 뿐만 아니라 가장 긴 일치 항목 보다 긴 시간 걸릴 수 있습니다 수 있습니다. 따라서 플레이어를 돌려받지 못하게 시간 직전 재생을 시작 하는 경우에 여전히 가집니다 일치 하는 시간이 부족. 결과가 보고 되지 않습니다의 시간을 기준으로 하는 경우는 **arbitrationTimeout**, 모든 일치 하는 요소가 손실을 수신 합니다. Xbox Arena도 구성 될 때까지 대기 합니다 **arbitrationTimeout** 중재를 시작 하기 전에 결과 보고 하는 모든 활성 구성원에 대 한 합니다.

이 제한 시간은 경우에 문제가 플레이어에 대 한 결과 보고 하지 않습니다 방어벽을 처럼 작동 합니다. 전체 토너먼트 지연 하는 대신이 제한 시간은 토너먼트에서 대기 하는 기간에 상한값을 배치 합니다. 이런 이유로 설정 되지 않아야 너무 높은 경우 각 토너먼트 라운드의 최대 길이 결정 하므로 합니다.

#### <a name="name"></a>이름

게임 모드에 대 한 사용자 측 이름입니다. 이 값은 지역화할 수 있습니다.

#### <a name="description"></a>설명

게임 모드에 대 한 사용자 용 설명입니다. 이 값은 지역화할 수 있습니다.

#### <a name="custom"></a>사용자 지정

합니다 **사용자 지정** 게임 모드는 개발자 토너먼트 일치 하는 title 별 구성 설정에 삽입할 수 있는 속성 모음에 대 한 섹션입니다. 사용자 지정 섹션의 일부로 정의 된 값에서 일치 MPSD 세션에 기록 됩니다 `/properties/custom/`합니다.

현재, 게임 모드 설치 XDP 또는 UDC를 통해 지원 되지 않습니다. 제목에 대 한 게임 모드를 만들려면 개발자 계정 관리자에 게 문의 합니다.

### <a name="requirements-for-the-session-template"></a>세션 템플릿에 대 한 요구 사항

제목 개발자로 서 일치 항목에 대 한 세션을 만들 때 사용할에 세션 템플릿을 제공 해야 합니다. 서식 파일의 콘텐츠에 대 한 몇 가지 기대 있습니다.

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

그러나 원하는 여기 표시 되지 않는 다른 속성을 설정할 수 있습니다.

Arena 세션은 항상 버전 1입니다. 설정 된 **inviteProtocol** "tournamentGame" 일치 준비 알림을 토너먼트 참가자에 게 보낼 수 있습니다. **memberInitialization** QoS를 사용 하지 않도록 설정 하려면 null로 설정 됩니다. 게임 플레이 기능은 세션 일치 항목을 나타내며 중재 기능은 결과 보고에 필요한 때문에 설정 되어야 합니다. 합니다 **큰**를 **브로드캐스트**, 및 **blockBadMsaReputation** 세션의 작업을 방해 하 게 하기 때문에 기능을 비활성화 해야 합니다.

제목 템플릿을 사용 하는 모든 토너먼트에 대 한 고정된 값이 있는 설정에 대 한 템플릿의 사용자 지정 섹션에서 자체 설정이 지정할 수 있습니다. 예를 들면 다음과 같습니다.

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

마지막으로, 합니다 **maxMembersCount** 시스템 설정은 필수입니다. 총 토너먼트 일치에서 재생할 수 있는 플레이어를 설정 합니다. 플레이어의 수가 게임 모드 설정에 따라 더욱 제한할 수 있습니다, 그리고 세션에 설정 된 값을 제목에 대 한 지원 플레이어의 가장 높은 총 수를 나타내는 하 고 있는지 확인 합니다. 예를 들어 다음과 같이 일치 항목을 지 원하는 게임 플레이어의 최대 수는 5 vs. 5 설정할 **maxMembersCount** 10입니다. 이 MPSD 템플릿을 최대 플레이어를 개수에 관계 없이 일치 하는 항목을 지 원하는 데 사용할 수는 **maxMembersCount**합니다.
