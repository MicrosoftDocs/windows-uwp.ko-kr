---
title: Xbox One 멀티 플레이 세션 디렉터리
author: KevinAsgari
description: Xbox Live Mutliplayer 세션 디렉터리 (MPSD) 서비스를 사용 하 여 멀티 플레이 세션을 만드는 방법을 알아봅니다.
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c4e10d3a9c194ff5c191ccf33370bad2d9981650
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4506575"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Xbox One 멀티 플레이 세션 디렉터리

이 항목에서는 새 Xbox One 멀티 플레이 세션 디렉터리 (MPSD) 서비스를 사용 하 여 멀티 플레이 세션 만들기의 개요를 제공 합니다. 용지 세션 템플릿에도 직접를 Xbox 개발 포털 (XDP)을 제출 하는 Xbox One 제목 개발자 주로으로 리디렉션됩니다. MPSD 서비스도 Windows 개발자 센터를 사용 하 여 구성할 수 있지만이 문서의 하지 초점. 용어 및 개념의 멀티 플레이 세션 문제 해결 및 MPSD 구성, 사용, 관련 알아두는 것입니다.

## <a name="revision-summary"></a>요약 수정

클라이언트 쪽 (Xbox 서비스 API) XSAPI 라이브러리 MPSD 서비스와 통신 하는 경우 현재 계약 버전 104를 사용 합니다. 이 버전의 문서 계약 최신 MPSD 서비스에서 지원 되는 버전 107, 축소 세션 템플릿 스키마를 업데이트 합니다. 버전 104와 107 간의 변경 내용은 [계약 스키마 업데이트](#_Contract_schema_update)섹션 요약 되어 있습니다.

참고 104 계약 버전용으로 작성 된 템플릿 107으로 다시 게시 되 면 업데이트 해야 합니다. 하지만이 업데이트는 현재 라이브러리를 사용 하 여 계속 수 있도록 서비스 이전 버전과 호환은 XDK 릴리스에서 합니다.

이 문서의 이전 수정 버전 서버 세션에 대 한 정보를 업데이트 하 고 Xbox Live 서비스 Api 및 RESTful 서비스 호출에 대 한 새 섹션이 추가 되었습니다. 또한 세션 상태 정보 섹션에 대 한 쿼리에 있는 표를 업데이트 하 고 서비스 품질 (QoS) 템플릿 섹션 수정 되었습니다.

## <a name="introduction"></a>소개

Xbox one 멀티 플레이 세션에는 멀티 플레이 세션 디렉터리 (MPSD), 클라우드에 거주 하는 보안 문서 이며이 문서는 게임을 플레이 하는 사용자의 그룹을 나타냅니다. 특히, 멀티 플레이 세션에 다음 품질:

-   각 세션에 고유한 URI입니다.

-   세션 문서 현재 구성원을, 게임 설정, 데이터 및 게임 서버 정보 부트스트랩 포함 되어 있습니다.

-   타이틀 만들기 및 자신의 세션을 관리 합니다.

-   세션의 멤버를 연결할 수 있습니다.

멀티 플레이어 세션 디렉터리는 모든 클라이언트에서 게임 세션 메타 데이터를 제공 합니다. 세션에 대 한 클라이언트 간 보안 장치 연결을 설정 하는 데 필요한 기본 정보를 제공 합니다. 또한 더욱 구체적인 게임 데이터 간에 전달을 시작 하기 전에 게임에 연결 하는 클라이언트에 대 한 기본 첫 번째 부팅 메타 데이터를 제공 합니다. 프로세스 수명 관리 (PLM) 및 Xbox One에서 응용 프로그램의 작업 전환 특성은 MPSD 하면 클라이언트가 피어 및 현재 게임 세션 및 좌표의 일환으로 식별 되는 서버에 연결에 대 한 올바른 정보 셸 및 콘솔 운영 체제를 예약을 활성화 하 고 게임 세션에 대 한 플레이어 수명을 관리 합니다.

## <a name="terminology-used-in-this-document"></a>이 문서에서 사용 된 용어

| 용어                 | 정의                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 멀티 플레이 세션  | Xbox Live 클라우드에 있으며 그룹 (또는 됩니다)을 표시 하는 보안 문서 제목을 Xbox One에서 재생 하는 동안 함께 연결 합니다. 멀티 플레이어의 모든 측면-매치 메이 킹, 당사자, 진행 중인 조인 등의 멀티 플레이 세션을 활용 합니다. |
| 게임 세션         | 이는 사용자가 재생 하는 함께 MPSD에 노출 된 실제 게임 세션을 합니다. 모든 멀티 플레이어 시나리오는 결국 게임 세션에서 종료 합니다.                                                                                                                               |
| 티켓 세션 일치 | 이 세션은 세션 연결 하는 동안 티켓 제출 일치를 추적 하는 데 사용 합니다.                                                                                                                                                                                                                 |
| 비활성 플레이어      | 플레이어가 세션 내에서 비활성 상태로 설정 되었습니다. 제목 게임 제한 된 경우, 일시 중단 상태 또는 제목에 정의 된 대로 그렇지 비활성는 비활성으로 사용자를 설정 합니다.                                                                                     |

## <a name="the-multiplayer-session-directory"></a>멀티 플레이 세션 디렉터리

MPSD 용이 하 게 하며 타이틀 좌표 온라인 플레이어 간에 세션 정보. 다양 한 유형의 멀티 플레이의 서로 다른 작업을 수행 하기 위해 만든 세션 수 있습니다. 다음 표에서 차이와 Xbox One에서 수행 하는 방법 등의 작업 Xbox 360에서 수행 하는 방법에 배치 합니다.

| 기능이 나 작업                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **게임 세션 정보 가져오기**     | **XSessionGetDetails**, **XSessionSearchByID**또는 트랙 직접 합니다.                                              | 서비스에서 세션 정보를 요청 합니다.                                                          |
| **호스트 마이그레이션**                     | 필요한 경우 제목 호출 **XSessionMigrateHost.**                                                           | 방금 세션 Id에 대 한 새 호스트를 할당, 새 세션을 만들 필요가 없습니다.                                  |
| **여러 플레이어 세션을 관리**  | 한 번에 둘 이상의 세션을 처리 하기 어렵습니다. 예를 들어 **XNetReplaceKey** **XNetUnregisterKey**비교 합니다. | 서비스 기반 세션 확연히 한 세션을 관리 하면 하 고 손쉽게 여러 세션을 처리할 수 있습니다.    |
| **아웃 사이 니 지를 처리 하 고 연결을 끊습니다** | 처리 하 게 끊을 **XCloseHandle** 또는 **XSessionDelete**다르게 로그 아웃 하 고 각각 합니다. | 중앙 집중식된 서비스 사이 니 지 사용해보는 단순화 하 고 처리를 분리 하 고 게임 구성에서 시간 제한 설정 됩니다. |

### <a name="session-patterns"></a>세션 패턴

-   게임 세션

    -   플레이어의 XUIDs, 보안 장치 주소 데이터 및 상태를 사용 하 여 세션입니다. 이 실제 게임 플레이 세션으로 간주 됩니다.

    -   피어 투 피어, 피어 호스트, 피어-서버 또는 하이브리드 수 있습니다.

-   티켓 세션 일치

    -   세션으로 게임을 다른 플레이어를 찾으려고 매치 메이 킹 제출입니다. Note 더 많은 플레이어에 대 한 게임을 찾고 있는 경우 게임 세션 티켓 세션 수도 있습니다.

-   서버 세션

    -   게임 세션 만들고 Xbox Live 계산 처리를 통해 사용 합니다.

그림 1은 MPSD 세션, 여기서 파란색 상자 MPSD 세션을 표시 하 고 빨간색 상자는 사용 중인 서비스의 사용법을 보여 줍니다.

그림 1. MPSD 세션에 사용 합니다.

## <a name="mpsd-sessions"></a>MPSD 세션

세션에 관련 된 엔터티 두 목록을 유지 관리합니다.

1.  멤버 (사용자)에 가입 있거나 세션에 초대 된 합니다.

2.  서버 (클라우드 서버 또는 서버) 세션에 참가입니다.

각 엔터티에 있습니다.

1.  상수 값이 생성 시 설정 합니다.

2.  속성 변경 가능 합니다.

3.  읽기 전용 메타 데이터입니다.

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>Xbox Live 서비스 Api 및 RESTful 서비스 호출

Xbox One 세션 및 매치 메이 킹 서비스와 상호 작용 하는 방법은 두 가지가 있습니다. 첫 번째 방법은 RESTful Xbox Live 서비스 Uri에 대 한 표준 HTTPS 호출을 사용 하는 것입니다. 이 호출 및 서버 및 게임 구성에 따라 이러한 서비스와 상호 작용의 제목을 유연성을 수 있습니다. 이러한 Uri의 목록에서 [Xbox One 개발 키트 (XDK) 설명서](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) 에서 찾을 수 있습니다 "Xbox Live Services RESTful 참조 합니다." [1]

두 번째 방법은 Api를 사용 하는 Xbox Live 서비스, RESTful 서비스 Uri에 대 한 래퍼입니다도 작용 하는 것입니다. 코드에서 Api를 사용 하 여 각 호출에 대 한 HTTPS 트래픽을 처리 하지 않고도 보다 일반적인 접근 방식을 수 있습니다. Xbox Live 서비스 Api에 대 한 소스 코드 Xbox 개발 키트 (XDK)으로 제공 수정 및 필요에 따라 타이틀과 통합 될 수 있습니다. 샘플은 Xbox Live 서비스 Api를 사용 하 여 기록 됩니다. Xbox Live 서비스 Api에 대 한 자세한 내용은 "Xbox Live 서비스 참조입니다." 아래에서 Xbox One [XDK 설명서](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) 에서 찾을 수 있습니다. [2]

### <a name="mpsd-sessions-and-templates"></a>MPSD 세션 및 템플릿

MPSD 세션 템플릿 Xbox 개발 포털 (XDP)을 통해 수집을 사용 하 여 생성 됩니다. 템플릿은 만들어지는 세션에 대 한 프레임 워크를 정의 하는 JSON 문서입니다. 템플릿에서 새 세션에 대 한 상수를 제공 합니다.

[플레이어 접선 코드 샘플](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) 에서 발췌 한 다음는 템플릿을 구성 예입니다.

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

일치 티켓 세션 **memberInitialization** 개체의 QoS 제한 시간 값을 사용 하 여 설정 게임 세션 템플릿으로 사용 해야 합니다.

그림 2. 샘플 hopper 합니다.

![](../../images/whitepapers/mpsd_image1.png)

다음에 나오는 발췌 한 코드는 피어 투 피어 게임 세션 템플릿 (제목 관리 QoS)의 예로 나와 있습니다.

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

피어 서버 세션과 원시 서식 파일의 예입니다.

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

다음 코드는 스마트 일치에 사용 되는 일치 티켓 세션 템플릿 예제를 보여 줍니다.

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

### <a name="checking-which-templates-are-configured-for-your-scid"></a>서식 파일에 서비스 안내에 대 한 구성 확인

#### <a name="session-templates"></a>세션 템플릿

MPSD에서 서비스 안내, 내에 있는 세션 템플릿 목록 뿐만 아니라 특정 세션 템플릿의 세부 정보를 검색할 수 있습니다.

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

기본적으로이 최대 100 개인 세션 반환 됩니다. 이러한 쿼리 문자열 매개 변수를 사용 하 여 쿼리를 수정할 수 있습니다.

| 쿼리 문자열             | 효과                                                                                                         | 설명                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| 키워드 foo =              | "Foo" 키워드가 있는 세션 포함                                                             |                                                                                                     |
| XUID 123 =                 | "123" 사용자가 세션 포함 됩니다.                                                               | 기본적으로 사용자를 포함할 수 있는 세션에서 활성 상태 여야 합니다.                                  |
| *예약*=**true** | 세션의 사용자는 예약 된 플레이어로 설정 되어 있지만 활성 플레이어를 조인 하지 않은 포함 됩니다. | 고유한 세션을 쿼리 하는 경우에 또는 특정 사용자의 세션-서버를 쿼리 합니다. |
| *비활성*=**true**      | 사용자는 수락 하지만 재생 되지 세션을 포함 합니다.                                     | 사용자가 "준비" 하지만 "비활성" 세션 비활성으로 계산 됩니다.                           |
| *개인*=**true**       | 개인 세션을 포함 합니다.                                                                                      | 고유한 세션을 쿼리 하는 경우에 하거나, 서버를 쿼리 합니다.                            |
| *가시성*= 열림        | ". 열려" 있는 세션 포함                                                                         | 경우에 "개인" 설정의 "개인 = true" 지시문 설정 해야 합니다.                                 |
| *시험*= 5                 | 최대 5 개의 세션을 반환 합니다.                                                                                    | 0과 100 사이 여야 합니다.                                                                          |

결과 JSON 배열 세션에 대 한 참조입니다. 일부 세션 데이터가 인라인에 포함된 됩니다.

**참고** 모든 쿼리 키워드 필터, XUID 필터 또는 둘 다 포함 해야 합니다.

*개인* (반환 하는 개인 세션) 또는 *예약* 설정 (가입 되지 않은 경우 사용자 세션을 반환 하는)를 **true** 로 세션 또는 일치 하도록 호출자의 XUID 클레임에 대 한 서버 수준 액세스 호출자가 필요 URI의 XUID 필터입니다. 그렇지 않은 경우 (여부 실제로 존재 하는 이러한 세션) 403 반환 됩니다.

다음 코드에서는 쿼리 응답의 예를 보여 줍니다.

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

이 문서 시작 부분에 언급 한 것 처럼 최신 세션 템플릿 계약 버전은 107, 104의 이전 버전의 스키마에 몇 가지 변경 내용을 도입 하였습니다. 템플릿 계약 버전 104 용으로 작성 된 107으로 다시 게시 되 면 업데이트 해야 합니다. 다음은 변경 내용의 요약 합니다.

-   **/constants/system/managedInitialization** **/constants/system/memberInitialization**로 이름이 변경 됩니다.

    -   **AutoEvaluate** 필드는 **false** 기본값을 유지 **externalEvaluation** 및 해당 극성 변경 바뀝니다.

    -   **MembersNeededToStart** 기본값 2에서 1로 변경합니다.

    -   기본값인 **joinTimeout** 5 초에서 10 초 변경 됩니다.

    -   5 초에서 30 초 **measurementTimeout** 기본 변경 됩니다.

-   **/constants/system/timeouts** 제거 되 고 시간 제한을 바꾸고 **/constants/system**에서 재배치 합니다.

    -   **예약 된** 시간 초과 **reservedRemovalTimeout**됩니다.

    -   **비활성** 시간 제한 **inactiveRemovalTimeout** 되며 새의 기본값은 0으로 하는 대신 2 시간.

    -   **준비** 시간 초과 **readyRemovalTimeout**됩니다.

    -   **SessionEmpty** 시간 제한 **sessionEmptyTimeout**됩니다.

-   최근 플레이어와 신뢰도 보고를 사용 하도록 설정 하려면 (반대 일치 및 로비 세션 같은 도우미 세션) 실제 게임 플레이 나타내는 세션에 **/constants/system/capabilities/gameplay** **true** 로 지정 되어야 합니다.

### <a name="system-objects"></a>시스템 개체

각 세션 문서에서 시스템 개체에 적용 되 고 MPSD에 의해 해석 되는 고정된 스키마 있습니다.

PUT 요청 본문 내에서 시스템 개체 유효성이 검사 되 고 사용자 지정 개체와 마찬가지로 병합 합니다. 하지만 사용자 지정 개체와 달리 시스템 개체 추가 유효성을 검사 및 처리를 병합 된 후이 스키마에 따라 이러한 합니다.

**/ 상수/시스템**

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

**/ 속성/시스템**

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

타이머와 외부의 다른 이벤트에서 세션을 변경할 수 있습니다. MPSD의 **시간 제한** 개체 세션 수명 및 멤버를 관리 하는 기본 메커니즘을 제공 합니다.

세션의 **nextTimer** 필드는 다음 타이머의 시간을 제공합니다. 클라이언트는 좋은 다음 폴링 간격을 선택 하려면이 정보를 사용할 수 있습니다. 이 값은 또한 **만료** HTTP 헤더에 반환 됩니다.

시간 제한 밀리초로 지정 됩니다. 0 수 있으며 시간 제한 즉시 함을 나타냅니다. 지정 된 시간 제한 지정 하지 않으면 무한 간주 됩니다. 시간 제한을 기본값 없으므로 세션 템플릿 명시적으로 "null" 무한 시간 제한 지정 해야 합니다.

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

**/Constants/system/sessionEmptyTimeout** 값 구성 밀리 초 수 세션 빌 후는 삭제 됩니다. 기본값은 0, 즉 세션은 즉시 삭제 됩니다. 값 지정 되지 않으면 빈 세션 무기한 만들겠습니다.

#### <a name="member-timeouts"></a>구성원 시간 제한

세 가지 다른 시간 제한을 **/constants/system** 컨트롤 기간 내에서 특정 상태에 구성원 유지할 수 있습니다.

-   **reservedRemovalTimeout**

    -   예약 제한 시간이 만료 되 면 삭제 됩니다. 기본값은 30 초입니다.

-   **inactiveRemovalTimeout**

    -   비활성 멤버는 기본적으로 2 시간 후 세션에서 제거 됩니다.

-   **readyRemovalTimeout**

    -   구성원 "준비" 상태로 되돌릴 수는 비활성 3 분 후 기본적으로.

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>세션 문서에서 멤버 초기화

### <a name="member-initialization"></a>구성원 초기화


**MemberInitialization** 개체 제어 세션 만들기에 따라 초기화 단계 및/또는 새 멤버로 세션에 참가 합니다. 시간 제한 및 초기화 단계는 자동으로 모든 메트릭을 설정 된 경우 QoS 측정 값을 포함 한 세션에서 추적 됩니다.

이러한 시간 초과 **initializationEpisode** 속성이 설정 구성원에 대 한 세션의 예약 및 준비 제한 시간을 재정의 합니다.

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

**그림 3. 구성원 초기화 흐름**

각 멤버 초기화의 세 단계 시간 초과 될 수 있습니다.

1.  **joiningTimeout**

    -   시간 (밀리초), 사용자 세션에 참가할가 금액입니다. 가입에 실패 하는 멤버의 예약 제거 됩니다.

2.  **measuringTimeout**

    -   크기 멤버의 측정을 업로드 하는 시간입니다. 측정 업로드에 실패 하는 멤버의 "제한" *failureReason* 으로 표시 됩니다.

3.  **evaluationTimeout**

    -   크기를 높이고 평가 결정 업로드 구성원에 대 한 시간입니다. 아무 결정을 받은 경우 오류로 계산 됩니다.

**externalEvaluation**

-   MPSD 세션 템플릿에서 제공 하는 요구 사항에 따라 자동 QoS 평가 수행할 수 있습니다. 평가는 externalEvaluation false로 설정 된 경우 수행 됩니다. **externalEvaluation** ** **evaluationTimeout** 설정 된 경우에 적용** 해야 합니다. 두 가지 피어 투 피어 또는 피어 호스트 요구 인 경우 여전히 설정 해야 **externalEvaluation** **false** 로 세션을 자동으로 초기화를 완료 하기 위해 합니다.

**membersNeededToStart**

-   이 "초기화"를 사용 하 여 존재 하는 멤버 예약의 최소 수: "true" 및 성공 하려면 QoS 자동 평가 통과 합니다.

### <a name="initialization-episodes"></a>초기화 에피소드


**MemberInitialization** 개체를 설정 하는 경우 MPSD 에피소드 하 여 초기화 단계를 진행 됩니다. 첫 번째 에피소드 세션이 만들어지고 서식 파일에 정의 된 모든 단계를 포함 하는 경우 시작 됩니다.

다음 에피소드에 대 한 새 멤버 초대 또는 에피소드 이미 실행 중인 동안 가입 표시 됩니다. 일반적 에피소드 또는 **memberInitialization** 상태 세션의 **초기화** 개체에서 검색 될 수 있습니다.

**참고** 주의이 개체는 초기화가 완료 된 후 제거 됩니다.

예제:

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

가입 평가를 측정 하는 단계 전송 됩니다. 에피소드 실패 단계 후 *실패* 설정 및 세션을 초기화할 수 없습니다. 그렇지 않으면 초기화 에피소드 완료 되 면 **초기화** 개체가 제거 됩니다.

각 구성원에 대 한 초기화 오류를 추적할 수도 있습니다. 이 멤버 통과 하지 못한 경우 조인 또는 측정 단계에서 전환할 때 설정 됩니다.

예제:
```
"initializationFailure": "latency",
```
우선 순위에 따라이 특성의 값을를 *시간 제한, 대기 시간, bandwidthdown, bandwidthup, 네트워크, 그룹*, 또는 *에피소드*설정 수 있습니다. 네트워크 값 의미 네트워크 구성 및/또는 조건 (예: 충돌 하는 네트워크 주소 변환 \[NAT\])에서 측정 되 고 있는 QoS 메트릭을 수 없습니다. 가입 후에만 가능한 값은 *그룹*입니다. (가입 시간, 예약 제거 됩니다.)

**MemberInitialization** 설정 되 고 구성원 "초기화"를 사용 하 여 추가 된 경우: 멤버에 참여할 초기화 에피소드로 설정 true 됩니다. 1의 값은 만들어질 때 새 세션에 추가 된 멤버에 사용 하 고 초기화 에피소드 끝날 때 제거 됩니다.
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>티켓 세션 일치

MPSD 세션 일치 티켓 세션 사용량을 몇 가지 특별 한 세션 속성 및 상수 사용 됩니다.

**{인덱스} /members//상수/시스템**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

연결 서비스를 추가 하면 사용자가 세션에 방법과 이유 항목의 세션에 **matchmakingResult** 필드에 몇 가지 컨텍스트를 제공 합니다.

```
"matchmakingResult": {
```

이 사용자의 **serverMeasurements** 매치 메이 킹 세션에서 복사 합니다.

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>품질 (Qos) 템플릿

게임 세션 템플릿에 대 한 값 네트워크 계층 및 콘솔 소셜 플랫폼에 맞게 MPSD 알려주는 추가할 수 있습니다. 이렇게이 하려면의 목적은 세션을 만들기 전에 및 사용자가 알림을 게임이 가입할 준비가 되기 전에 연결 상태 품질 유효성 검사를 합니다.

다음 코드에서는 QoS 사용 하 여 피어 호스트 게임 세션 템플릿의 예로 나와 있습니다.

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

이 코드 발췌 QoS 사용 하 여 피어 투 피어 게임 세션 템플릿의 예로 나와 있습니다.

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

### <a name="qos-session-template-attributes"></a>QoS 세션 템플릿 속성

**MemberInitialization** 개체를 설정 하는 경우 세션에 클라이언트 시스템 또는 세션 만들기를 수행 하는 초기화를 수행 및/또는 새 멤버로 세션에 참가할를 제목 필요 합니다.

시간 제한 및 초기화 단계는 자동으로 모든 메트릭을 설정 된 경우 QoS 측정 값을 포함 한 세션에서 추적 됩니다.

이러한 시간 초과 **initializationEpisode** 속성이 설정 구성원에 대 한 세션의 예약 및 준비 제한 시간을 재정의 합니다.

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

QoS 사용 하 여 게임 세션 연결 기능에 필요합니다. 없는 메트릭을 지정 하는 경우 QoS 요구 사항을 충족 하기 위해 필요한 기본 합니다. 를 지정 하는 경우 QoS 요구 사항을 충족 하는 데 충분 이어야 합니다.

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

다음과 같은 요구 세션의 모든 구성원에 대 한 쌍이 들어 있는 각 연결에 적용 됩니다.

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

다음과 같은 요구 호스트 후보에서 각 연결에 적용 됩니다.

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

다음 잠재적인 서버 연결 문자열 이어야 평가 (참고 연결 문자열 소문자 여야 합니다).

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

**멤버 {인덱스} / 속성/시스템 /**

구성원 상태 및 **activeTitle**이 플래그를 제어 하 고 상호 배타적 ( **true**로 설정 하는 오류는). 각각에 대해 **false** 와 동일 "표시 되지 않습니다." 기본 상태 비활성 상태임 "," 즉, 있는지 어느 것도 포함 합니다.

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

## <a name="qos-phase-and-session-initialization-details"></a>QoS 단계와 세션 초기화 세부 정보

MPSD 추적 및 템플릿이 멤버 초기화가 완료 된 후 게임 만들기에 대 한 QoS 결과 보고 됩니다. 이 작업의 진행률 위의 [멤버 초기화](#_Member_initialization_in) 섹션에 설명 된 대로 세션 문서에서 *초기화* 개체에 의해 표시 됩니다. *초기화* 개체 초기화의 현재 단계를 나타내는 *단계* 특성에 있습니다. *평가*를 *측정* *가입* 단계로 진행 됩니다.

-   초기화 에피소드 \#1 실패 단계 후 *실패* 설정 하 고 세션을 초기화할 수 없습니다. 그렇지 않으면 초기화 에피소드 완료 되 면 "초기화" 개체가 제거 됩니다. **ExternalEvaluation** **false**로 설정 되 면 정확 하 게 평가 단계를 건너뜁니다. **메트릭** 아니며, **measurementServerAddresses** 설정 되 면 측정 단계를 건너뜁니다.

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   호스트 후보는 우선 순위에 나열 된 장치 토큰입니다. **PeerToHostRequirements** 설정 되 고 **/properties/system/host** 설정 되지 않은 경우 초기화 에피소드 \#1의 *측정* 단계 후 설정 됩니다. **/Properties/system/host** 개체를 설정한 후 지워집니다.

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   *게이머* 특성 멤버는 수락 하 고 해당 멤버에 대 한 게이머 클레임이 발견 된 경우에 설정 됩니다.

```json
"gamertag": "stacy",
```

-   멤버 업로드 보안 장치 주소 *deviceToken* 특성 설정 됩니다. 이 일치 비교에 사용할 수 있는 대/소문자 구분 문자열입니다.

```json
"deviceToken": "9f4032ba7",
```

-   *예약 된* 값은 사용자가 자신의 첫 번째 PUT 요청 세션 문서를 실행 한 후 제거 됩니다. 플레이어가 예약 된 경우는 게임 세션에 초대 하지만 허용 모두 나 연결 평가 의미 합니다.

```json
"reserved": true,
```

-   멤버가 활성 상태 이면 *activeTitleId* 멤버인 십진수에서 활성 제목입니다.

```json
"activeTitleId": "8397267",
```

-   이 특성은 사용자 세션에 참가 하는 시간을 가리킵니다. *예약 된* **true**인 경우 *joinTime* 은 예약 벌어 들인 시간 됩니다.

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   이 멤버는 속성/시스템/턴 배열 그렇지 않으면 하지 않을 경우 제공 합니다.

```json
"turn": true,
```

-   *InitializationFailure* 특성 멤버 스테이지 통과 하지 않은 경우 조인 또는 측정 단계에서 전환할 때 구성원 개체에 설정 됩니다. 우선 순위에 따라에서 *시간 제한*, *대기 시간*, *bandwidthdown*, *bandwidthup*, *네트워크*, *그룹*또는 *에피소드*설정할 수 있습니다.
    *네트워크* 값 덕분에 네트워크 구성 및/또는 조건 (같은 충돌 하는 네트워크 주소 변환 \[NATs\])에서 측정 되 고 있는 QoS 메트릭을 수 없습니다. 가입 후에만 가능한 값은 *그룹*입니다. (가입 시간, 예약 제거 됩니다.) *에피소드* 값 또는 측정 하는 동안 실패 하지 않았으므로 초기화 에피소드의 모든 구성원에 실패 한 "평가" 단계 후 설정 됩니다.

```json
"initializationFailure": "latency",
```

-   **MemberInitialization** 설정 되 고 구성원 "초기화"를 사용 하 여 추가 된 경우: 멤버에 참여할 초기화 에피소드로 설정 true 됩니다. 값 1은 사용에 새 세션에 추가 된 멤버 만들기 시간입니다. 초기화 에피소드 끝날 때 제거 됩니다.

```json
"initializationEpisode": 1,
```

-   *다음* 특성 세션에서 다음 멤버의 인덱스 값을 나타냅니다. 추가 더 많은 구성원이 있는 경우 아래 **membersInfo** 개체는 *다음* 특성을 기준으로 동일한 값 됩니다.

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

## <a name="xbox-cloud-compute-session"></a>Xbox 클라우드 계산 세션

Xbox 클라우드 계산 세션 순서가 지정 된 세션 게임 서버에 연결 하는 데 사용할 수 있는 대/소문자 구분 연결 문자열 목록이 포함 되어 있습니다. 일반적으로 제목 목록에서 첫 번째 문자열을 사용 해야 하지만 정교한 제목 (예를 들어, 기준된 로드) 다른 중 하나를 선택 하기 위한 사용자 지정 메커니즘 (예: Xbox Live 계산)를 사용할 수 있습니다.

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

* * / 서버 / {서버-name} / 속성/시스템 * *

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

멀티 플레이어 게임에 대 한 사용자 지정 정리 정보 (메타 데이터)를 저장 하는 세션을 사용할 수 있습니다. TMS + + 게임 데이터 나 저장된 한 정보를 저장 해야 합니다.

### <a name="property-bags"></a>속성 모음

위의 개체 각각에 두 개의 선택적 내부 개체, 시스템 및 사용자 지정 속성 모음 구성 된다고 표시 합니다.

사용자 지정 개체가 있는 JSON을 포함할 수 있습니다.

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>활성 구성원 감쇠

활성 구성원은 MPSD 제목에 사용자가 더 이상 연결을 감지할 때 자동으로 비활성 상태로 표시 됩니다. 예를 들어 있는지 시간이 사용자 레코드를 초과 하는 경우이 오류가 발생할 수 있습니다. 이 메커니즘은 백업을 뿐입니다. 즉, 타이틀은 여전히 사전 멤버 비활성으로 표시 하거나 세션에서 제거 해야 사이 니 지 만료, 또는 기타 될 모드가 해제 될 때 멤버 두거나 제목에서 전환, 합니다.

## <a name="faq-and-troubleshooting"></a>FAQ 및 문제 해결

### <a name="how-do-i-call-mpsd-"></a>MPSD를 호출 하는 방법을 하나요?

인증서 인증을 사용 하 여: 클라이언트 sessiondirectory.xboxlive.com

예제:

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

XToken 인증을 사용 하 여: sessiondirectory.xboxlive.com

예제:

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>어떤 서비스 안내, 세션 템플릿 및 샌드박스를 사용 확인 하려면 어떻게 하나요?

타이틀에 대 한 XDP 사이트에서이 정보를 찾을 수 있습니다. XDP에 대 한 액세스 아직 없는 경우 수에 대 한 정보를 얻는 방법에 대해 지원할 수 있는 개발자 계정 관리자에 게 문의 합니다.

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>I 비교할 수 있는 요청 본문의 예제를 직접?


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>MPSD를 호출할 때 404 오류가 발생 합니다.

자세한 정보를 가져오고 다음을 수행 하는 데 Fiddler 추적 정보를 수집 합니다.

1.  일반적인 404 메시지에 대 한 HttpResponse 본문의 일환으로 반환 된 메시지를 확인 합니다.

    -   *요청 된 서비스 구성 잘못 되었거나 세션에 대 한 구성 되지 않음*. 사용 중인 서비스 안내 올바른지 확인 합니다.

    -   *요청 된 세션 찾을 수 없습니다*. 세션을 만든를 검색 하기 전에 세션 템플릿 올바른지 확인 합니다. PUT 호출 하 여 세션을 만들 수 있습니다.

2.  사용 하는 URI를 확인 합니다.

3.  콘솔 다시 부팅 및/또는 새 사용자를 사용 하 여 다시 시도 하세요.

4.  오류 코드 또는 기타 잠재적인 솔루션에 대 한 [엔터테인먼트 개발자 포럼](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) 을 검색 합니다.

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>MPSD를 호출할 때 403 오류가 발생 합니다.

이 일반적으로 사용 권한이 나 범위 문제입니다. 자세한 정보를 가져오고 다음을 수행 하는 데 Fiddler 추적 정보를 수집 합니다.

1.  일반적인 403 메시지에 대 한 HttpResponse 본문의 일환으로 반환 된 메시지를 확인 합니다.

-   *는 요청 된 서비스 구성에 액세스할 수 없습니다. *

    -   해당 샌드박스에 액세스 권한이 있는 계정을 사용 하 고 있는지 확인 합니다.

    -   올바른 샌드박스 내에 있는지 확인 합니다.

    -   인증서 인증을 사용 하 여이 오류는 경우에 댐을 문의 합니다.

-   *요청 된 세션에 액세스할 수 없습니다. 개인 세션 세션의 구성원만 읽을 수 있습니다.*

    -   "개인"의 표시 여부를 세션에 액세스 하려고 합니다. 세션 내에서 구성원만 세션 문서를 읽을 수 있습니다.

-   *요청 본문 계정 인증 서버를 포함 하지 않는 한 기존 구성원에 대 한 참조를 포함할 수 없습니다.*

    -   사용자를 대신 하 여 다른 사용자 세션에 참가할 수 없습니다. 만 요청할 수 있습니다. 인덱스 설정 "reserve\_&lt;번호&gt;" 플레이어를 초대할 수 있습니다.

### <a name="i-am-getting-a-412-precondition-failed-error"></a>412 전제 조건 실패 오류가 발생 합니다.

이 세션에 이미 존재 하는 경우 412 전제 조건 실패 반환 됩니다.

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/빠른/세션/foo HTTP/1.1 콘텐츠 형식: 응용 프로그램/j 경우 없음-일치: \ *

이 세션 etag는 헤더와 일치 하지 않으면 412 전제 조건 실패 반환 됩니다.

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/빠른/세션/foo HTTP/1.1 콘텐츠 형식: 응용 프로그램/j 경우-일치: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>405, 409, 503, 400when 호출 MPSD 등 오류가 발생 합니다.

자세한 정보를 가져오고 메시지를 확인 하는 데 Fiddler 추적을 수집 HttpResponse 본문의 일환으로 반환 됩니다. 이 식별 하 고 오류를 해결 하거나 가능한 솔루션에 대 한 [엔터테인먼트 개발자 포럼](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) 을 검색 하려면 충분 한 정보를 제공 해야 합니다.

디버그 출력에 정보를 출력 하는 오류를 **DiagnosticsTraceLevel** 속성을 설정 하 여 Xbox Live 서비스 Api를 사용 하는 경우 응답 본문을 가져올 수도 있습니다. 하거나 합니다 **를 사용할 수 있습니다. XboxLiveContextSettings.ServiceCallRouted** 타이틀 UI 출력 여러 샘플에서와 같이 이벤트입니다.

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>바꾸어야 템플릿에서 내 제목에 대 한 하거나 어떻게 수 있습니까?

기본값은 몰드 중은 아니지만 세션 템플릿. 그러나을 추가할 수 있지만 템플릿에 이미 설정 되어 상수를 재정의할 수 없습니다.

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>세션 초기화 되지 있다는 오류가 발생 합니다.

있는지 확인 해야 하는 멤버 초기화 (게임, 파티 및 일치 티켓 시나리오에서는 일반적으로) 템플릿에 있는 경우 "초기화 = true" 충분히 멤버 예약 (구성원을 시작 하는 데 필요한)에 대 한 설정이 문제를 해결 하려면 QoS 전달 합니다.

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>세션을 생성 되는 및 HTTP 204 오류가 발생 합니다.

사용자가 만든 경우 세션에 추가 되었음을 나타냅니다. 생성 시 세션에 대 한 사용자가 있는 경우 세션 생성 되지 않습니다. 세션 만들기에 하나 이상의 플레이어를 추가 해야 합니다. 전용된 서버 시나리오에 대 한 사용자가 일치 하는 항목을 만드는 하려고 하거나 일치 하는 항목을 만들고에서 일치 하는 초기 플레이어가 해당 사용자를 확인 해야 하는 플레이어를 가져옵니다. 또는 변경 하거나 **sessionEmptyTimeout**제거할 수도 있습니다.

### <a name="when-should-i-poll-the-mpsd"></a>MPSD 폴링하지 경우

MPSD 세션 폴링 하지 않아야 합니다. 상위 수준에서 클라이언트 또는 연결이 끊겼을 클라이언트에 대 한 네트워크 상태를 다시 설정 및 각 클라이언트에 대 한 네트워크 연결의 초기 설정에 대 한 MPSD 세션을 사용 하는 방식으로 코드를 디자인 하 여 설정할 수 있습니다. 경합 상태를 해결 하려면 세션 상태를 새로 고치려면 필요성을 제거할 MPSD의 etag 기반 동기화 원형을 기능을 활용 하는 것이 중요 이기도 합니다.

이러한 원칙의 일반적인 용도 N 클라이언트 집합이 있는 경우 모든 함께 연결 하는 피어 투 피어 메시의 재생 필요 합니다. 새 멤버에 대 한 세션은 정기적으로 쿼리 하는 대신 각 구성원 수 세션에 참가할 세션에서 이미 멤버에 연결 및 가정 같은 모든 이후 조 이너 수행 됩니다. 이 구현 하는 방법에 대 한 예제 채팅 및 플레이어 접선 샘플을 참조 하세요.

경우가 거의 사용 되지 않는 폴링; 짧은 시간 동안 필요할 수 있습니다. 이 작업을 수행 해야 할 경우 개발자 계정 관리자에 게 문의 합니다.
