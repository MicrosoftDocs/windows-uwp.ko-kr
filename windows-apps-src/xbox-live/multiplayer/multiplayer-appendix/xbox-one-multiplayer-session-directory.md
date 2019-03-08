---
title: Xbox One 멀티 플레이 세션 디렉터리
description: Xbox Live Mutliplayer 세션 디렉터리 (MPSD) 서비스를 사용 하 여 멀티 플레이 세션 만들기에 대해 알아봅니다.
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 52b363492d1cd17ae54ae5d23d04371c5adf73ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606818"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Xbox One 멀티 플레이 세션 디렉터리

이 항목에서는 새 Xbox 하나 멀티 플레이 세션 디렉터리 (MPSD) 서비스를 사용 하 여 멀티 플레이 세션 만들기에 대 한 개요를 제공 합니다. 용지 다룬다는 것 주로 Xbox One 제목 개발자 세션 템플릿을 직접를 Xbox 개발 포털 (XDP)을 제출 합니다. MPSD 서비스 에서도 파트너 센터를 사용 하 여 구성할 수 있지만이 문서에 한정 되지 않고 됩니다. 사용 약관 사용 MPSD 구성과 연결 및 멀티 플레이 세션의 문제 해결 개념을 잘 이해 하기 위한 것입니다.

## <a name="revision-summary"></a>수정 버전 요약

클라이언트 쪽 XSAPI (Xbox 서비스 API) 라이브러리는 현재 MPSD 서비스와 통신할 때 계약 버전 104를 사용 합니다. 이 버전의 문서 MPSD services에서 지 원하는 최신 계약 버전인 버전 107, 계약 세션 템플릿 스키마를 업데이트 합니다. 버전 104 및 107 스냅숏 간의 변경 사항을 섹션에 요약 되어 있습니다 [계약 스키마 업데이트](#_Contract_schema_update)합니다.

참고 계약 버전 104 용으로 작성 된 템플릿은 107로 다시 게시 하는 경우 업데이트 해야 합니다. 그러나 서비스 되므로 이전 버전과 호환 업데이트 될 현재 라이브러리를 사용 하 여 계속 수 이후 XDK 버전에서.

이 문서의 이전 수정 버전 서버 세션에 대 한 정보를 업데이트 하 고 Xbox Live 서비스 Api 및 RESTful 서비스를 호출 하는 방법에 대 한 새로운 섹션이 추가 되었습니다. 또한 세션 상태 정보 섹션에 대 한 쿼리를 있는 테이블이 업데이트 및 서비스 품질 (QoS) 템플릿 섹션이 수정 되었습니다.

## <a name="introduction"></a>소개

Xbox One에서 멀티 플레이 세션에는 멀티 플레이 세션 디렉터리 (MPSD), 클라우드에 상주 하는 보안 문서 이며이 문서는 게임을 즐기는 사용자의 그룹을 나타냅니다. 특히, 멀티 플레이 세션 다음 품질에

-   각 세션에는 고유한 URI를 갖습니다.

-   세션 문서 현재 구성원, 게임 설정을 부트스트랩 하는 데이터 및 게임 서버 정보를 포함 합니다.

-   제목을 만들고 자체 세션을 관리 합니다.

-   세션에서 구성원 간의 연결을 사용 합니다.

다중 접속 세션 디렉터리를 사용 하는 모든 클라이언트에서 게임 세션 메타 데이터를 중앙 집중화합니다. 세션에 대 한 클라이언트 간의 보안 장치 연결을 설정 하는 데 필요한 기본 정보를 제공 합니다. 또한 보다 구체적인 게임 데이터 전달을 시작 하기 전에 게임에 연결할 클라이언트에 대 한 기본 첫 번째 부팅 메타 데이터를 제공 합니다. MPSD 관리 PLM (프로세스 수명) 및 Xbox One에 응용 프로그램의 작업 전환 특성을 사용 하 여 클라이언트 피어 및 좌표 고 활성 게임 세션의 일부로 식별 되는 서버에 연결 하는 것에 대 한 올바른 정보가 있는지을 보장합니다 셸 및 콘솔 운영 체제에 예약을 사용 하 여 활성화 및 플레이어 게임 세션 수명을 관리 합니다.

## <a name="terminology-used-in-this-document"></a>이 문서에 사용 되는 용어

| 용어                 | 정의                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 멀티 플레이 세션  | Xbox Live 클라우드에 상주 하는 (또는 됩니다) 사용자의 그룹을 나타내는 보안 문서 제목을 Xbox One에서 재생 하는 동안 함께 연결 합니다. 멀티 플레이 게임의 모든 측면을-매 치메이 킹, 파티, 진행 중인 조인 등과 같은-멀티 플레이 세션을 활용 합니다. |
| 게임 세션         | 사용자는 재생 함께 MPSD에 노출 된 실제 게임 세션입니다. 모든 멀티 플레이어 시나리오는 궁극적으로 게임 세션에서 종료 합니다.                                                                                                                               |
| 티켓 세션 일치 | 이 세션은 세션 결혼 정보 회사 연결 하는 동안 티켓 제출 일치 항목을 추적 하는 데 사용 합니다.                                                                                                                                                                                                                 |
| 비활성 플레이어      | 세션 내에서 비활성 상태로 설정 되어 있는 플레이어입니다. 제목 일시 중단 된 게임 제한 된 경우 상태 또는 타이틀에 정의 된 대로 그렇지 않은 경우 비활성는 Inactive로 사용자를 설정 합니다.                                                                                     |

## <a name="the-multiplayer-session-directory"></a>멀티 플레이 세션 디렉터리

MPSD 용이 하 게를 제목 온라인 플레이어 간에 세션 정보를 조정 합니다. 다양 한 유형의 멀티 플레이어 게임 실행의 다양 한 작업을 위해 만들어진 세션 있을 수 있습니다. 다음 표에서 Xbox 360에서 Xbox One에서 수행 하는 방법 및 이러한 작업 된 수행 하는 방법의 차이 배치 합니다.

| 함수 또는 작업                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **게임 세션 정보 가져오기**     | **XSessionGetDetails**, **XSessionSearchByID**, 또는 사용자가 직접 추적 합니다.                                              | 서비스에서 세션 정보를 요청 합니다.                                                          |
| **마이그레이션할 호스트**                     | 필요한 경우 제목 호출 **XSessionMigrateHost 합니다.**                                                           | 새 세션을 만들고, SessionID에 대 한 새 호스트를 할당할 필요가 없습니다.                                  |
| **여러 플레이어 세션 관리**  | 한 번에 둘 이상의 세션 처리 하기 어렵습니다. 예를 들어 **XNetReplaceKey** 비교 **XNetUnregisterKey**합니다. | 서비스 기반 세션 수행 되는 클리너 한 세션을 관리할 수 있도록 하 고 쉽게 여러 세션을 처리 합니다.    |
| **로그인 제한 처리 및 연결 해제** | 처리 하 게 연결을 끊습니다 및 다르게 로그 아웃 **XCloseHandle** 또는 **XSessionDelete**, 각각. | 중앙 집중식된 서비스 기호 입력을 단순화 하 고 처리 하 고, 연결 끊기 및 게임 구성에서 시간 제한이 설정 되어 있습니다. |

### <a name="session-patterns"></a>세션 패턴

-   게임 세션

    -   플레이어의 XUIDs, 안전한 장치 주소 데이터 및 속성 상태를 사용 하 여 세션입니다. 이 실제 게임 플레이 세션으로 간주 됩니다.

    -   피어-투-피어, 피어 및 호스트 간, 피어-투-서버, 또는 하이브리드 수 있습니다.

-   티켓 세션 일치

    -   작업을 진행 하는 다른 플레이어를 찾으려면 결혼 정보 회사에 제출 된 세션입니다. 더 많은 플레이어 게임은 찾고 게임 세션 티켓 세션을 수도 있습니다 note 합니다.

-   서버 세션

    -   게임 세션 생성 및 Xbox Live 계산 처리를 통해 사용 합니다.

그림 1에서는 MPSD 세션에 있는 파랑 상자로 MPSD 세션 나타내며 빨간색 상자는 사용 중인 서비스의 사용을 보여 줍니다.

그림 1. MPSD 세션 사용 합니다.

## <a name="mpsd-sessions"></a>MPSD 세션

세션에는 관련 엔터티의 두 목록을 유지 관리합니다.

1.  멤버 (사용자) 조인 또는 세션에 초대 되었습니다.

2.  (클라우드 서버 또는 서버 전용된) 하는 서버 세션에 참가 했습니다.

각 엔터티에:

1.  생성 시 설정 하는 상수 값입니다.

2.  변경 가능한 속성입니다.

3.  읽기 전용 메타 데이터입니다.

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>Xbox Live 서비스 Api 및 RESTful 서비스 호출

Xbox 하나의 세션 및 결혼 정보 회사 연결 서비스와 상호 작용 하는 방법은 두 가지가 있습니다. 첫 번째 방법은 RESTful Xbox Live 서비스 Uri에 대 한 표준 HTTPS 호출을 사용 하는 것입니다. 이 호출 하 고 해당 서버 및 게임 구성에 따라 이러한 서비스와 상호 작용 유연을 타이틀 선택할 수 있습니다. 이러한 Uri의 목록에서 찾을 수 있습니다 합니다 [Xbox 하나 개발 키트 (XDK) 설명서](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) 아래 "Xbox Live Services RESTful 참조 합니다." [1]

두 번째 방법은 Api 사용 하 여 Xbox Live 서비스, RESTful 서비스 Uri에 대 한 래퍼로 작동 하는 것입니다. 각 호출에 대 한 HTTPS 트래픽을 처리 하지 않고도 코드에서 Api를 사용 하는 보다 일반적인 방법 수 있습니다. Xbox Live 서비스 Api에 대 한 소스 코드를 Xbox 개발 키트 (XDK)와 함께 제공 되는 수정 고 필요에 따라 제목에 통합 될 수 있습니다. 샘플은 Xbox Live 서비스 Api를 사용 하 여 작성 되었습니다. Xbox One에서 Xbox Live Services Api에 대 한 자세한 정보를 찾을 수 있습니다 [XDK 설명서](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) "Xbox Live 서비스 참조입니다."에서 [2]

### <a name="mpsd-sessions-and-templates"></a>MPSD 세션 및 템플릿

Xbox 개발 포털 (XDP)를 통해 수집 된 템플릿을 사용 하 여 MPSD 세션이 생성 됩니다. 템플릿은 생성 되는 세션에 대 한 프레임 워크를 정의 하는 JSON 문서입니다. 템플릿에 새 세션에 대 한 상수를 제공합니다.

다음에서 발췌를 [플레이어 Rendezvous 코드 샘플](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) 은 템플릿 구성 예제입니다.

```json
// Used for creating the session that you then pass into your match ticket request. This *should* not have any QoS requirements.
MatchTicketSession (Contract Version: 107)
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

// This is the new game session that is returned after you’ve been matched.
// Note: This is used for in-game QoS. For out-of-game QoS, you will need P2P/HTP requirements.
GameSession (Contract Version:107)
{
    "constants": {
        "system": {
            "maxMembersCount": 12,
            "capabilities": {"connectivity": true }
        },
        "memberInitialization": {
         "joinTimeout": 20000,
         "measurementTimeout": 15000,
         "externalEvaluation": false,
         "membersNeededToStart": 4
         },

   "custom": {}
  }
}
```

일치 티켓 세션에서 QoS 시간 제한 값을 사용 하 여 설정 게임 세션 템플릿을 사용 하 여 사용 해야 해당 **memberInitialization** 개체입니다.

그림 2 샘플 hopper 합니다.

![](../../images/whitepapers/mpsd_image1.png)

인용 된 코드 뒤에 오는 피어-투-피어 게임 세션 템플릿 (제목에서 관리 되는 QoS)의 예시입니다.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}

```

이것이 피어-투-서버 세션 및 원시 템플릿의 예입니다.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {}
        },
        "custom": {}
    }
}
```

다음 코드는 스마트 일치에 사용 되는 일치 티켓 세션 템플릿의 예를 보여 줍니다.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

```

### <a name="checking-which-templates-are-configured-for-your-scid"></a>서식 파일에 서비스 안내에 대해 구성 된 검사

#### <a name="session-templates"></a>세션 템플릿

MPSD에서 서비스 안내, 내에 있는 세션 템플릿 목록 뿐 아니라, 특정 세션 템플릿의 세부 정보를 검색할 수 있습니다.

```
GET /serviceconfigs/{scid}/sessiontemplates

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}
```

#### <a name="query-for-session-state-information"></a>세션 상태 정보에 대 한 쿼리

서비스 구성 및 세션 템플릿 수준에서 세션을 쿼리할 수 있습니다.

```
GET /serviceconfigs/{scid}/sessions

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}/sessions
```

기본적으로 최대 100 개의 private이 아닌 세션 돌아갑니다. 이러한 쿼리 문자열 매개 변수를 사용 하 여 쿼리를 수정할 수 있습니다.

| 쿼리 문자열             | 효과                                                                                                         | 설명                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| keyword=foo              | "Foo"로 지정 합니다. 키워드가 있는 세션에만 포함                                                             |                                                                                                     |
| XUID=123                 | 세션에 있는 "123" 사용자만 포함 합니다.                                                               | 기본적으로 사용자에 포함 되도록 세션에서 활성 상태 여야 합니다.                                  |
| *reservations*=**true** | 사용자는 예약 된 플레이어로 설정 되어 있지만 되는 활성 플레이어에 조인 되지 않은 세션을 포함 합니다. | 고유한 세션을 쿼리 하는 경우에 또는 특정 사용자의 세션 서버-투-서버를 쿼리 하는 경우. |
| *inactive*=**true**      | 사용자가 수락 하지만 적극적으로 재생 하지 않을 세션을 포함 합니다.                                     | 사용자가 "ready" 하지만 "비활성" 세션 비활성으로 계산 합니다.                           |
| *private*=**true**       | 개인 세션을 포함 합니다.                                                                                      | 고유한 세션을 쿼리 하는 경우에 또는 서버-투-서버를 쿼리 하는 경우.                            |
| *visibility*=open        | ". 열려" 있는 세션에만 포함                                                                         | 경우 개인으로 설정 ","는 "private = true" 지시문 설정 해야 합니다.                                 |
| *take*=5                 | 최대 5 개의 세션을 반환 합니다.                                                                                    | 0과 100 사이 여야 합니다.                                                                          |

결과 세션 참조의 JSON 배열입니다. 일부 세션 데이터가 인라인으로 포함된 됩니다.

**참고** 모든 쿼리 키워드 필터를, XUID 필터 또는 둘 다 포함 해야 합니다.

설정 *사설* (반환 하는 개인 세션) 또는 *예약* (가입 되지 않은 사용자 세션을 반환 하는)를 **true** 하려면 호출자가 필요 합니다. 서버 수준 세션 또는 URI에 XUID 필터와 일치 하는 호출자의 XUID 클레임에 대 한 액세스. 그렇지 않은 경우 403 사용할 수 없음 (표시할지, 아니면 실제로 존재 하는 이러한 세션) 반환 됩니다.

다음 코드 발췌 구문에는 쿼리 응답의 예가 나와 있습니다.

```
{
"results": [ {
"xuid": "9876",  // If the session was found from a xuid, that xuid.
"startTime": "2009-06-15T13:45:30.0900000Z",
"sessionRef": {
  "scid": "foo",
  "templateName": "bar",
  "name": "session-seven"
},
"accepted": 4,        // Approximate number of non-reserved members.
"status": "active",   // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
"visibility": "open", // or "private", "visible", or "full"
"myTurn": true,       // Not present is the same as 'false'. Only present if the session was found using a xuid.
"keywords": [ "one", "two" ]
} ]
}

```

## <a name="session-template-attributes"></a>세션 템플릿 특성

<div id="_Contract_schema_update"/>

## <a name="contract-schema-update"></a>계약 스키마 업데이트

이 문서의 시작 부분에서 언급 한 것 처럼 최신 세션 템플릿을 계약 버전이 107, 104의 이전 버전에서 스키마를 일부 변경 내용이 도입 하는 합니다. 계약 버전 104 용으로 작성 된 템플릿은 107로 다시 게시 하는 경우 업데이트 해야 합니다. 다음은 변경 내용 요약 합니다.

-   **/constants/system/managedInitialization** 로 바뀌었습니다 **/constants/system/memberInitialization**합니다.

    -   합니다 **autoEvaluate** 필드에 이름이 **externalEvaluation** 극성 (polarity) 변경 내용을 제외 하 고 **false** 은 기본값을 유지 합니다.

    -   기본값인 **membersNeededToStart** 2에서 1로 변경 합니다.

    -   기본값인 **joinTimeout** 5 초에서 10 초로 변경 합니다.

    -   합니다 **measurementTimeout** 기본 5 초에서 30 초로 변경 합니다.

-   **/constants/system/timeouts** 제거 및 시간 제한을 변경 되 고 아래 재배치 **/상수/시스템**입니다.

    -   합니다 **예약** 제한 됩니다 **reservedRemovalTimeout**합니다.

    -   합니다 **비활성** 제한 됩니다 **inactiveRemovalTimeout** 이며 해당 새 기본값은 0 2 시간 대신 합니다.

    -   합니다 **준비** 제한 됩니다 **readyRemovalTimeout**합니다.

    -   합니다 **sessionEmpty** 제한 됩니다 **sessionEmptyTimeout**합니다.

-   **/constants/system/capabilities/gameplay** 로 지정 해야 **true** 최근 플레이어를 사용 하도록 설정 하려면 (일치 하 고 로비 세션과 같은 도우미 세션) 대신 실제 게임 플레이 나타낼 수 있는 세션에 및 평판 보고 합니다.

### <a name="system-objects"></a>시스템 개체

각 세션 문서에서 시스템 개체에 적용 되며 MPSD에서 해석 하는 고정된 스키마를 있습니다.

PUT 요청의 본문 내에서 시스템 개체는 유효성이 검사 되 고 사용자 지정 개체와 마찬가지로 병합 합니다. 하지만 사용자 지정 개체와 달리 시스템 개체를 추가로 유효성을 검사 하 고 취해야 병합 된 후이 스키마를 기반으로 이러한 합니다.

**/constants/system**

```json
{
"version": 1,  //Document Version (FAL release version 1, service contract 107)
"maxMembersCount": 100,  // Defaults to 100 if not set on creation. Must be between 1 and 100.
"visibility": "private",  // Or "visible" or "open", defaults to "open" if not set on creation.

"initiators": [ "1234" ],  // If specified on a new session, the creators xuid must be in the list (or the creator must be a server).
"inviteProtocol": "party",  // Optional URI scheme of the launch URI for invite toasts.

"reservedRemovalTimeout": 10000, // Default is 30 seconds. Member is removed from the session.
"inactiveRemovalTimeout": 0, // Default is zero. Member is removed from the session.
"readyRemovalTimeout": 60000, // Default is three minutes. Member is removed from the session.
"sessionEmptyTimeout": 0, // Default is zero. Session is deleted.

// Capabilities are boolean values that are optionally set in the session template. If no capabilities are needed, an empty "capabilities" object should be in the template in order to prevent capabilities from being specified on session creation, unless the title desires dynamic session capabilities.
"capabilities": {
"clientMatchmaking": true,
"connectivity": true, // Cannot be set if ‘large’ is specified.
     "suppressPresenceActivityCheck": false,
     "gameplay": false,
     "large": false
},
/* If a "memberInitialization" object is set, the session expects the client system or title to perform initialization following session creation and/or as new members join the session. The timeouts and initialization stages are automatically tracked by the session, including QoS measurements if any metrics are set. These timeouts override the session's reservation and ready timeouts for members that have 'initializationEpisode' set. */
"memberInitialization": {
"joinTimeout": 20000,  // Milliseconds. Unspecified counts as 10 seconds.
"externalEvaluation": false,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 00 and the session is created with no members to initialize, episode 1 is skipped..
},
```

**/properties/system**

```
{
// Optional array of case-insensitive strings. Cannot be set if the session's visibility is "private".
"keywords": [ "hello" ],

// Array of integer indices of members whose turn it is. Defaults to empty. Can't be set (and doesn't appear) on large sessions.
"turn": [ 0 ],

/* Restricts who can join "open" sessions. (Has no effect on reservations, which means it has no impact on "private" and "visible" sessions.) Defaults to "none". On large sessions, "none" is the only valid value.
If "local", only users whose token's DeviceId matches someone else already in the session and "active": true.
If "followed", only local users (as defined above) and users who are followed by an existing (not reserved) member of the session can join without a reservation. */
"joinRestriction": "none",

// Device token of the host. Must match the "deviceToken" of at least one member, otherwise this field is deleted.
// If "peerToHostRequirements" is set and "host" is set, the measurement stage assumes the given host is the correct host and only measures metrics to that host.
// Can't be set on large sessions.
"host": "99e4c701",

// Can only be set while "initializing/stage" is "evaluating". True indicates success, and false indicates failure. Once set, "initializing/stage" is immediately updated, and this field is removed.
"initializationSucceeded": true,

/* The ordered list of case-insensitive connection strings that the session could use to connect to a game server. Generally titles should use the first on the list, but sophisticated titles could use a custom mechanism (e.g. Thunderhead) for choosing one of the others (e.g. based on load). */
"serverConnectionStringCandidates": [ "datacenter b", "serverfarm a" ],

"matchmaking": {
    "targetSessionConstants": { },
    // Force a specific connection string to be used (useful in preserveSession=always cases).
    "serverConnectionString": "datacenter b",
},

// True if the match that was found didn't work out and needs to be resubmitted. Set to false to signal that the match did work, and the matchmaking service can release the session.
"matchmakingResubmit": true
}

```

### <a name="timeouts"></a>시간 제한

타이머 및 기타 외부 이벤트 세션을 변경할 수 있습니다. 합니다 **시간 제한을** MPSD에서 개체 세션 수명 및 멤버를 관리 하는 기본 메커니즘을 제공 합니다.

합니다 **nextTimer** 세션의 필드는 다음 타이머의 시간을 제공 합니다. 클라이언트가이 정보를 사용 하 여 다음 폴링에 대 한 적절 한 간격을 선택할 수 있습니다. 이 값에도 반환 되는 **Expires** HTTP 헤더입니다.

시간 제한은 밀리초 단위로 지정 됩니다. 0은 허용 및 제한 시간이 즉시 되어야 함을 나타냅니다. 지정 된 제한 시간을 지정 하지 않으면 무한 간주 됩니다. 시간 제한을 기본값이 있으므로 세션 템플릿을 명시적으로 "null" 이면 시간 제한이 무한입니다를 지정 해야 합니다.

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

합니다 **/constants/system/sessionEmptyTimeout** 값 구성 (밀리초) 후 세션 비게는 삭제 됩니다. 기본값은 0, 즉 세션은 즉시 삭제 됩니다. 값을 지정 하지 않으면 빈 세션 영원히 지속 됩니다.

#### <a name="member-timeouts"></a>멤버 시간 제한

세 가지 다른 시간 제한 내 **/상수/시스템** 멤버는 특정 상태에서 유지할 수 있는 시간을 제어 합니다.

-   **reservedRemovalTimeout**

    -   예약 시간 제한이 만료 되 면 삭제 됩니다. 기본값은 30초입니다.

-   **inactiveRemovalTimeout**

    -   비활성 멤버를 기본적으로 두 시간 후 세션에서 제거 됩니다.

-   **readyRemovalTimeout**

    -   구성원은 "준비"를 비활성 상태로 되돌아갑니다 3 분 후 기본적으로 합니다.

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>세션 문서의 멤버 초기화

### <a name="member-initialization"></a>멤버 초기화


합니다 **memberInitialization** 개체 컨트롤을 초기화 하는 세션을 만든 다음 및/또는 새 멤버로 준비 세션에 참가 합니다. 시간 제한 및 초기화 단계에서 메트릭을 설정 된 경우 QoS 측정을 포함 하 여 세션에서 자동으로 추적 됩니다.

이러한 시간 제한 없는 멤버에 대 한 세션의 예약 및 준비 시간 제한을 재정의 합니다 **initializationEpisode** 속성 집합입니다.

예제:

```
"memberInitialization": {
        "joinTimeout": 5000,
        "measurementTimeout": 5000,
        "evaluationTimeout": 5000,  // only specify for external evaluation
        "externalEvaluation": true,
       "membersNeededToStart": 2
    },
```


![](../../images/whitepapers/mpsd_image2.png)

**그림 3입니다. 멤버 초기화 흐름입니다.**

멤버 초기화의 세 단계 각각 시간 초과 될 수 있습니다.

1.  **joiningTimeout**

    -   사용자 세션에 참가 하기가 밀리초 단위로 시간의 양입니다. 조인에 실패 하는 멤버의 예약 제거 됩니다.

2.  **measuringTimeout**

    -   멤버는 측정값을 업로드 해야 하는 시간의 양입니다. 사용 하 여 표시 된 측정을 업로드 하지 못한 멤버가 *failureReason* "timeout"입니다.

3.  **evaluationTimeout**

    -   만들고 평가 의사 결정을 업로드 하는 멤버에 대 한 시간의 양입니다. 없는 의사 결정을 받으면 실패로 계산 됩니다.

**externalEvaluation**

-   MPSD 세션 템플릿에서 제공 하는 요구 사항에 따라 자동 QoS 평가 수행할 수 있습니다. 평가는 externalEvaluation false로 설정 된 경우에 수행 됩니다. **externalEvaluation** 야 **true** 경우를 **evaluationTimeout** 설정 됩니다. 여전히 설정 해야 피어-투-피어 나 피어와 호스트 간 두 가지 요구 사항이 없으면 **externalEvaluation** 하 **false** 세션을 자동으로 초기화를 완료 하기 위해.

**membersNeededToStart**

-   "초기화"를 사용 하 여 존재 해야 하는 멤버의 최소 수: "true" 및 성공 하려면 자동 평가 대 한 QoS를 전달 합니다.

### <a name="initialization-episodes"></a>초기화 에피소드


경우는 **memberInitialization** 개체가 설정 된, MPSD 에피소드에서 초기화 단계를 진행 합니다. 첫 번째 에피소드에는 세션을 만들고 템플릿에 정의 된 모든 단계를 포함 하는 경우 시작 됩니다.

새 멤버 초대 또는 조인 에피소드가 이미 실행 되는 동안 다음 에피소드에 대 한 표시 됩니다. 에피소드가의 상태 또는 **memberInitialization** 일반적에서 검색 될 수는 **초기화** 세션의 개체입니다.

**참고** 주의이 개체는 초기화가 완료 된 후 제거 됩니다.

예제:

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

스테이지 평가 위한 측정에 조인에서 이동 합니다. 에피소드가 실패할 경우 스테이지로 설정 되어 *실패 한* 및 세션을 초기화할 수 없습니다. 초기화 에피소드가 완료 되 면 그러지 합니다 **초기화** 개체가 제거 됩니다.

각 멤버에 대 한 초기화 오류 추적할 수 있습니다. 이 멤버를 통과 하지 못한 경우 조인 또는 측정 단계에서 전환 될 때 설정 됩니다.

예제:
```
"initializationFailure": "latency",
```
로 설정 수 값이이 특성에 대 한 우선 순위 순서로 *시간 제한, 대기 시간 "," bandwidthdown "," bandwidthup "," 네트워크 "," group*, 또는 *에피소드*. 네트워크 구성 및/또는 조건 네트워크 값 의미 (같은 충돌 하는 네트워크 주소 변환 \[NAT\]) QoS 메트릭 측정에서 차단 합니다. 조인 끝날 때 가능한 유일한 값은 *그룹*합니다. (조인에서 시간 제한의 예약 제거 됩니다.)

하는 경우 **memberInitialization** 설정 된 "초기화"를 사용 하 여 멤버를 추가 하 고: true 이면이 값을 설정 멤버 참여할 초기화 에피소드입니다. 생성 시 새 세션에 추가 된 멤버에 대 한 값이 1는 및 초기화 에피소드 종료 될 때 제거 됩니다.
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>티켓 세션 일치

일치 티켓 세션으로 MPSD 세션을 사용 중인 경우에 몇 가지 특수 한 세션 속성 및 상수가 사용 됩니다.

**/members/{index}/constants/system**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

일부 컨텍스트의 세션으로 일치 된 방법과 이유를 결혼 정보 회사 연결 서비스를 추가 하면 사용자 세션에 제공 된 **matchmakingResult** 필드입니다.

```
"matchmakingResult": {
```

이 사용자의 사본을 **serverMeasurements** 결혼 정보 회사 연결 세션에서.

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>품질 (QoS) 서비스 템플릿

게임 세션 템플릿에 대 한 값 MPSD 네트워크 계층 및 콘솔 소셜 플랫폼을 사용 하 여 조정 하 게 추가할 수 있습니다. 이 조정의 목적은 사용자는 게임 조인할 준비가 되었는지 합리적인가 세션을 만들기 전에 연결 상태의 품질의 유효성을 검사 하는 것입니다.

다음 발췌 한 코드는 다음과 같습니다. QoS 사용 하 여 피어와 호스트 간 게임 세션 템플릿의 예

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToHostRequirements": {
                "latencyMaximum": 350,
                "bandwidthDownMinimum": 1000,
                "bandwidthUpMinimum": 100,
                "hostSelectionMetric": "latency"
            }
        },
        "custom": {}
    }
}
```

이 코드 예제는 QoS 사용 하 여 피어-투-피어 게임 세션 템플릿의 예제:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToPeerRequirements": {
                "latencyMaximum": 250,
                "bandwidthMinimum": 10000
            }
        },
        "custom": {}
    }
}

```

### <a name="qos-session-template-attributes"></a>QoS 세션 템플릿 특성

경우는 **memberInitialization** 개체를 설정, 클라이언트 시스템 또는 세션을 만든 다음 초기화를 수행 및/또는 새 멤버로 세션에 참가 하려면 제목 세션이 필요 합니다.

시간 제한 및 초기화 단계에서 메트릭을 설정 된 경우 QoS 측정을 포함 하 여 세션에서 자동으로 추적 됩니다.

이러한 시간 제한 없는 멤버에 대 한 세션의 예약 및 준비 시간 제한을 재정의 합니다 **initializationEpisode** 속성 집합입니다.

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

QoS 사용 하 여 게임 세션 연결 기능에 필요합니다. 메트릭은 지정 된 경우 QoS 요구 사항을 충족 하는 데 필요한 기본값이 됩니다. 지정 된 경우 QoS 요구 사항을 충족 하기에 충분 해야 합니다.

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

다음 임계값 세션에서 모든 멤버에 대 한 쌍이 들어 있는 각 연결에 적용 됩니다.

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

다음 임계값 호스트 후보에서 각 연결에 적용 됩니다.

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

다음으로 잠재적인 서버 연결 문자열 이어야 합니다 (연결 문자열을 소문자 여야 하는 참고)를 평가 합니다.

```json
"measurementServerAddresses": {
        "east.thunderhead.azure.com": {
            "secureDeviceAddress": "r5Y="  // Base-64 encoded secure-device-address
        },
        "west.thunderhead.azure.com": {
            "secureDeviceAddress": "rwY="
        }
    }
}

```

**members/{index}/properties/system**

멤버 상태를 제어 하는 이러한 플래그 및 **activeTitle**, 및 상호 배타적인 (둘 다로 설정 하면 오류가 발생 **true**). 각각 **false** "present 없습니다."와 동일 기본 상태가 "활성" 즉, 둘 다가 합니다.

```json
"ready": true,
"active": false,

// Base-64 blob, or not present. Empty-string is the same as not present.
// 'capabilities/connectivity' must be enabled in order for this to be set.
"secureDeviceAddress": "ryY=",

// List of members in my group, by index. Each element must be an integer 0 <= n < 'membersInfo/next'.  
// During member initialization, if any members in the list fail, this member will also fail.
"group": [ 5 ],

// QoS measurements by lower-case device token.
// Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
// Metrics can me omitted if they weren't successfully measured, i.e. the peer is unreachable.
// If a "measurements" object is set, it can't contain an entry for the member's own address.
"measurements": {
"e69c43a8": {
"latency": 5953,  // Milliseconds
"bandwidthDown": 19342,  // Kilobits per second
"bandwidthUp": 944,  // Kilobits per second
"custom": { }
     }
},

// QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
// If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
    "serverMeasurements": {
        "east.thunderhead.azure.com": {
            "latency": 233  // Milliseconds
        }
    },

// Subscriptions for shoulder taps on session changes. The 'profile' indicates which session changes to tap as well as other properties of the registration like the min time between taps.
// The subscription is named with a client-generated GUID that is also sent back with the tap as a context ID.
// Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
// To remove a subscription, set its context ID to null.
// (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
"subscriptions": {
"961dc162-3a8c-4982-b58b-0347ed086bc9": {
"profile": "party",  // Or "matchmaking", "initialization", "roster", "queueHost", or "queue"
"onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
},
"709fef70-4638-4b94-905b-24cb02706eb5": null
}
}

```

## <a name="qos-phase-and-session-initialization-details"></a>QoS 단계 및 세션 초기화 세부 정보

MPSD 추적를 템플릿 멤버 초기화를 완료 된 후 게임 만들기에 대 한 QoS 결과 보고 합니다. 이 작업의 진행률을 나타내는 됩니다는 *초기화* 세션 문서에 설명 된 대로 개체를 [멤버 초기화](#_Member_initialization_in) 위의 섹션입니다. 합니다 *초기화* 개체에는 *단계* 초기화의 현재 단계를 나타내는 특성입니다. 단계에서 진행 *조인* 하 *측정* 에 *평가*합니다.

-   에피소드를 초기화 하는 경우 \#1 실패 하는 단계를로 *못했습니다* 및 세션을 초기화할 수 없습니다. 그렇지 않으면 초기화 에피소드가 완료 되 면 "초기화" 개체가 제거 됩니다. 하는 경우 **externalEvaluation** 로 설정 된 **false**의 평가 단계를 건너뜁니다. 모두 **메트릭을** 도 **measurementServerAddresses** 설정 측정 단계를 건너뜁니다.

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   호스트 후보의 기본 설정 순서 대로 나열 하는 장치 토큰입니다. 후 설정 됩니다는 *측정* 초기화 에피소드 단계의 \#경우 1 **peerToHostRequirements** 설정 되어 및 **/properties/system/host** 설정 되지 않았습니다. 후에 제거 되는 **/properties/system/host** 개체를 설정 합니다.

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   합니다 *게이머 태그* 특성 멤버는 수락 하 고 해당 멤버에 대 한 게이머 태그 클레임을 찾을 경우 설정 됩니다.

```json
"gamertag": "stacy",
```

-   합니다 *deviceToken* 특성 멤버에 안전한 장치 주소를 업로드할 때 설정 됩니다. 같음 비교에 사용할 수 있는 대/소문자 문자열입니다.

```json
"deviceToken": "9f4032ba7",
```

-   합니다 *예약 된* 사용자 세션 문서에 자신의 첫 번째 PUT 요청을 실행 한 후 값을 제거 합니다. 플레이어 예약 된 경우는 게임 세션에 초대 되었습니다 있지만 모두 수락 하거나 연결 평가 의미 합니다.

```json
"reserved": true,
```

-   멤버가 활성 상태 이면 *activeTitleId* 멤버인 활성, 10 진수의 제목입니다.

```json
"activeTitleId": "8397267",
```

-   이 특성은 사용자 세션에 참가 하는 시간을 가리킵니다. 경우 *예약* 됩니다 **true**, 한 다음 *joinTime* 예약을 만든 시간이 됩니다.

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   이 멤버가 없는 속성/시스템/turn 배열의 경우를 제공 합니다.

```json
"turn": true,
```

-   합니다 *initializationFailure* 조인에서 전환 또는 멤버 단계를 성공적으로 통과 하지 않은 경우 단계를 측정 하는 경우 멤버 개체에 특성이 설정 되어 있습니다. 우선 순위 순서로 설정 될 수 *timeout*, *대기 시간*를 *bandwidthdown*, *bandwidthup*, *네트워크* 하십시오 *그룹*, 또는 *에피소드*합니다.
    합니다 *네트워크* 값 즉, 네트워크 구성 및/또는 조건 (같은 충돌 하는 네트워크 주소 변환 \[Nat\]) QoS 메트릭 측정에서 차단 합니다. 조인 끝날 때 가능한 유일한 값은 *그룹*합니다. (조인에서 시간 제한의 예약 제거 됩니다.) 합니다 *에피소드* 후 실패 한 '평가' 단계인 측정 또는 조인 중에 실패 하지 않은 초기화 에피소드의 모든 멤버에 값이 설정 합니다.

```json
"initializationFailure": "latency",
```

-   하는 경우 **memberInitialization** 설정 된 "초기화"를 사용 하 여 멤버를 추가 하 고: true 이면이 값을 설정 멤버 참여할 초기화 에피소드입니다. 값이 1에는 만든 시간에서 새 세션에 추가 된 멤버에 대 한 합니다. 초기화 에피소드 종료 되 면 제거 합니다.

```json
"initializationEpisode": 1,
```

-   합니다 *다음* 세션에서 다음 멤버의 인덱스 값을 나타내는 특성입니다. 것이 동일한 값으로는 *다음* 특성을 **membersInfo** 추가할 멤버를 더 이상 없으면 아래 개체.

```json
            "next": 4
        },
        "4": { "next": 5 /* etc */ }
    },
    "membersInfo": {
        "first": 1,  // The first member's index.
        "next": 5,  // The index that the next member added will get.
        "count": 2,  // The number of members.
        "accepted": 1  // The number of members that are no longer 'pending'.
    },
    "servers": {
        "name": {
            "constants": { /* Property Bag */ },
            "properties": { /* Property Bag */ }
        }
    }
}

```

## <a name="xbox-cloud-compute-session"></a>Xbox에 클라우드 계산 세션

Xbox에 클라우드 계산 세션을 정렬 된 목록을 세션 게임 서버에 연결 하는 데 사용할 수 있는 대/소문자 연결 문자열을 포함 합니다. 일반적으로 제목 목록에서 첫 번째 문자열을 사용 해야 하지만 복잡 한 제목 (예를 들어에 따라 로드) 다른 하나를 선택 하는 것에 대 한 사용자 지정 메커니즘 (예: Xbox Live 계산)를 사용할 수 있습니다.

```json
    "serverConnectionStringCandidates": [ "west.thunderhead.azure.com", "east.thunderhead.azure.com" ],

    "matchmaking": {
        "clientResult": {  // Requires the clientMatchmaking property.
            "status": "searching",  // Or "expired", "found", "failed", or "canceled".
            "statusDetails": "Description",  // Default is empty string.
            "typicalWait": 30,  // The expected number of seconds waiting as a non-negative integer.
            "targetSessionRef": {
                "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
                "templateName": "capture-the-flag",
                "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
            }
        },

        "targetSessionConstants": { },

        // Force a specific connection string to be used (useful in preserveSession=always cases).
        "serverConnectionString": "west.thunderhead.azure.com",

        // True if the match that was found didn't work out and needs to be resubmitted. Set to false
        // to signal that the match did work, and the matchmaking service can release the session.
        "resubmit": true
    }
}

```

**/servers/{server-name}/properties/system **

```json
{
    "lockId": "opaque56789",  // If set, a matchmaking service is servicing this session.
    "status": "searching",  // Or "expired", "found", "failed", or "canceled". Optional.
    "statusDetails": "Description",  // Optional free-form text. Default is empty string.
    "typicalWait": 30,  // Optional. The expected number of seconds waiting as a non-negative integer.
    "targetSessionRef": {  // Optional.
        "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
        "templateName": "capture-the-flag",
        "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
    }
}

```

## <a name="raw-session-and-custom-title-properties"></a>원시 세션 및 사용자 지정 제목 속성

멀티 플레이 게임에 대 한 사용자 지정 정리 작업 정보 (메타 데이터)를 저장할 세션을 사용할 수 있습니다. TMS + +의 게임 데이터 나 저장 된 정보를 저장 되어야 합니다.

### <a name="property-bags"></a>속성 모음

위의 개체의 각 두 선택적 내부 개체, 시스템 및 사용자 지정 구성 속성 모음으로 표시 합니다.

사용자 지정 개체에는 모든 JSON 포함 될 수 있습니다.

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>활성 구성원 decay

사용자는 제목에 더 이상 참여 하 고는 MPSD 감지할 때에 자동으로 활성 구성원 비활성 표시 됩니다. 이 경우 발생할 수 있습니다, 예를 들어, 있는지 시간이 사용자 레코드를 확인 합니다. 이 메커니즘은 방금 방어벽을; 즉, 제목은 사전에 멤버를 비활성으로 표시 (또는 세션에서 제거) 여전히 필요한 sign out, 또는 변조 될 멤버를 유지 또는 제목에서 전환 하는 경우.

## <a name="faq-and-troubleshooting"></a>FAQ 및 문제 해결

### <a name="how-do-i-call-mpsd-"></a>MPSD를 어떻게 호출 합니까?

인증서 인증을 사용 하 여: sessiondirectory.xboxlive.com 클라이언트

예제:

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

XToken 인증을 사용 하 여: sessiondirectory.xboxlive.com

예제:

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>어떻게 찾나요 어떤 서비스 안내, 세션 템플릿 및 샌드박스 사용?

XDP 사이트 제목에이 정보를 찾을 수 있습니다. 아직 필요가 없습니다 XDP에 대 한 액세스 수에 대 한 정보를 가져오는 데 도움이 여 개발자 계정 관리자에 게 문의 합니다.

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>비교 수는 요청 본문의 예제를 직접?


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>MPSD를 호출할 때 404 오류가 발생 했습니다.

자세한 정보를 가져오고 다음을 수행 하는 데 Fiddler 추적을 수집 합니다.

1.  일반적인 404 메시지에 대 한 HttpResponse 본문의 일부로 반환 된 메시지를 확인 합니다.

    -   *요청 된 서비스 구성 파일 잘못 되었거나 구성 되지 않은 세션에 대 한*합니다. 사용 중인 서비스 안내 올바른지 확인 합니다.

    -   *요청한 세션 찾을*합니다. 세션이 만들어진를 검색 하기 전에 세션 템플릿 올바른지 확인 합니다. PUT 호출을 사용 하 여 세션을 만들 수 있습니다.

2.  사용 하는 URI를 확인 합니다.

3.  콘솔을 다시 부팅 하거나 새 사용자를 사용 하 여 다시 시도 합니다.

4.  검색 된 [엔터테인먼트 개발자 포럼](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) 오류 코드 또는 기타 잠재적 솔루션에 대 한 합니다.

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>MPSD를 호출 하는 경우 403 오류가 발생 했습니다.

이 일반적으로 사용 권한 또는 범위 문제입니다. 자세한 정보를 가져오고 다음을 수행 하는 데 Fiddler 추적을 수집 합니다.

1.  일반적인 403 메시지에 대 한 HttpResponse 본문의 일부로 반환 된 메시지를 확인 합니다.

-   * 요청한 서비스 구성에 액세스할 수 없습니다. *

    -   샌드박스에 대 한 액세스 권한이 있는 계정을 사용 하 고 있는지 확인 합니다.

    -   올바른 샌드박스 내에서 되는지 확인 합니다.

    -   인증서 인증을 사용 하 고이 오류가 발생 하는 경우에 파악에 게 문의 합니다.

-   *요청 된 세션에 액세스할 수 없습니다. 개인 세션은 세션의 멤버에 의해만 읽을 수 있습니다.*

    -   "개인."의 표시 유형이 있는 세션에 액세스 하려고 합니다. 세션 내에서 멤버만 세션 문서를 읽을 수 있습니다.

-   *보안 주체 인증 서버를 포함 하지 않는 한 요청 본문에서 기존 멤버 참조를 포함할 수 없습니다.*

    -   사용자를 대신 하 여 다른 사용자 세션에 참가할 수 없습니다. 만 초대할 수 있습니다. 인덱스 설정 "예약\_&lt;수&gt;" 플레이어를 초대할 수 있습니다.

### <a name="i-am-getting-a-412-precondition-failed-error"></a>412 사전 조건 실패 오류를 발생 했습니다.

412 사전 조건 실패 세션이 이미 존재 하는 경우 반환 합니다.

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/빠른/세션/foo/1.1 콘텐츠 형식: application/json 경우-없음-일치 합니다. \*

세션 etag는 If-match 헤더와 일치 하지 않으면 412 사전 조건 실패 반환 합니다.

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/빠른/세션/foo/1.1 콘텐츠 형식: application/json If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>405, 409, 503, 400when 호출 MPSD 등의 오류가 발생 합니다.

자세한 정보를 가져오고 다음 메시지를 확인 하는 데 Fiddler 추적 수집 HttpResponse 본문의 일부로 반환 합니다. 이렇게 하면 충분 한 정보를 확인 하 고 오류를 해결 하거나 검색 하는 [엔터테인먼트 개발자 포럼](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) 가능한 해결 방법에 대 한 합니다.

설정 하 여 Xbox Live 서비스 Api를 사용 하는 경우 응답 본문을 가져올 수도 있습니다는 **DiagnosticsTraceLevel** 오류 속성에서이 되는 출력 디버그에서 정보를 출력 하거나 사용할 수는  **XboxLiveContextSettings.ServiceCallRouted** 을 제목 UI 출력 하는 여러 샘플에서와 같이 이벤트입니다.

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>어떻게 또는 바꾸어야 템플릿에 내 제목에 대 한?

세션 템플릿 설정은 기본값을 아니지만 더는 유형에 합니다. 그러나에 추가할 수 있지만 템플릿에서 설정한 상수를 재정의할 수 없습니다.

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>세션 초기화 되지 라는 되는 오류가 발생 했습니다.

있는지 확인 해야 하는 멤버 초기화 (게임, 파티 및 일치 티켓 시나리오에서는 일반적으로) 템플릿에 존재 하는 경우 "초기화 = true" 충분히 멤버 예약 (시작 하는 데 필요한 멤버)에 대해 설정 되어이 문제를 해결 하려면 QoS를 전달 합니다.

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>내 세션 생성 되지 및 HTTP 204 오류가 발생 합니다.

이 사용자가 만든 경우 세션에 추가 되었음을 나타냅니다. 생성 시 세션에 대 한 사용자가 있는 경우 세션이 만들어지지 않습니다. 세션 만들기에 하나 이상의 플레이어를 추가 하는 있는지 확인 합니다. 전용된 서버 시나리오의 경우 플레이어를 일치를 만드는 하려고 하거나 일치 하는 항목을 만들고 해당 사용자 초기 플레이어에서 일치 하는 누구를 가져옵니다. 또는 변경 하거나 제거할 수 있습니다 합니다 **sessionEmptyTimeout**합니다.

### <a name="when-should-i-poll-the-mpsd"></a>MPSD 폴링할 경우

폴링 MPSD 세션을 피해 야 합니다. 높은 수준에서 각 클라이언트 및 클라이언트 또는 클라이언트의 연결이 끊어졌을 실행 하는 것에 대 한 네트워크 상태를 다시 설정 하기 위한 초기 네트워크 연결 설정에 대해서만 MPSD 세션을 활용 하는 방식으로 코드를 디자인 하 여이 수행할 수 있습니다. 경합 상황을 해결 하는 세션 상태를 새로 고칠 필요가 MPSD의 etag 기반 동기화 기본형을 활용 해야 이기도 합니다.

이러한 원칙을 일반적인 응용 프로그램은 N 클라이언트 집합에 있는 경우 해야 하는 서로 연결 하 고 피어-투-피어 메시의 재생 합니다. 새 멤버에 대 한 세션 정기적으로 쿼리 하는 대신 각 멤버 수 세션에 참가, 세션에서 이미 멤버에 연결 및는 뒷부분에 나오는 모든 져는 동일한 작업을 수행 하는 것으로 가정 합니다. 이 구현 하는 방법의 예제에 대 한 채팅 및 플레이어 Rendezvous 샘플을 참조 하세요.

폴링 단기간 동안; 할 수도 있는 드문 경우도 일부 이 작업을 수행 해야 하는 것이 생각 되 면 개발자 계정 관리자에 게 문의 합니다.
