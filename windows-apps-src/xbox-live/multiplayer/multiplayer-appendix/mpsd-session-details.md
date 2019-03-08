---
title: 멀티 플레이 세션 정보
description: 멀티 플레이 세션 Xbox Live의 세부 정보에 알아봅니다.
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 멀티 플레이 하나 xbox 2015 세션, mpsd
ms.localizationpriority: medium
ms.openlocfilehash: 3eb92af2d478fa41150f375dd19ef347d2b6f2e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616088"
---
# <a name="mpsd-session-details"></a>MPSD 세션 세부 정보

이 문서에는 다음과 같은 섹션이 포함되어 있습니다.
* 세션 개요
* 세션 기능
* 세션 크기
* 세션 사용자 상태
* 표시 유형 및 Joinability
* 세션 시간 제한
* 단일 콘솔에서 여러 로그인 사용자
* 프로세스 수명 주기 관리
* 비활성 세션의 정리
* 세션 중재자

## <a name="session-overview"></a>세션 개요

다중 접속 세션 디렉터리 (MPSD) 세션 세션 이름이 및 세션에 대 한 기본 설정을 제공 하는 JSON 문서는 세션 템플릿의 인스턴스로 식별 됩니다. 템플릿은은 GUID에는 서비스 구성 식별자 (서비스 안내)를 사용 하 여 서비스 구성의 일부입니다. 이 서식 파일에서 찾을 수 있습니다 [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com) 하 고 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 서비스 구성에는 수집, 관리 및 보안 정책에 사용 되는 개발자에 게 리소스가 있습니다. MPSD 통해 세션에 액세스할 때 보안 주체 권한 부여 XDP 또는 파트너 센터를 통해 개발자가 설정한 액세스 정책에 따라 서비스 구성에 대해 수행 됩니다. 보조 액세스 검사 세션 멤버 자격 유효성 검사와 같은 서비스 구성에 대 한 액세스 권한이 부여 된 후 세션 로드 될 때 세션 수준에서 수행 됩니다.

이 항목에서는 서식 파일에는 계약 버전 107, Xbox One에 대 한 현재 MPSD 사용한 버전인 가정 합니다. 계약 버전 105 (104 동일)을 기반으로 템플릿을 정의한 경우 107 버전을 지원 하도록 변경 해야 합니다. 자세한 내용은 [일반적인 멀티 플레이 게임 2015 마이그레이션 문제](common-issues-when-adapting-multiplayer.md)합니다.

| 중요                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 템플릿을 통해 설정 된 기능을 MPSD에 쓰기를 통해 변경할 수 없습니다. 값을 변경 하려면 해야 만들고 필요에 따라 변경을 사용 하 여 새 템플릿을 제출 합니다. MPSD에 쓰기를 통해 템플릿을 통해 설정 되지 않은 모든 항목을 변경할 수 있습니다. |

### <a name="session-reference"></a>세션 참조

각 MPSD 세션 고유 하 게 라고 멀티 플레이 WinRT api에서 표시 하는 세션 참조에 의해 합니다 **MultiplayerSessionReference 클래스**합니다. 이러한 문자열 값을 포함 하는 세션 참조:

-   서비스 구성 식별자 (서비스 안내)
-   세션 템플릿 이름
-   세션 이름

세션 참조는 아래와 같이 세션을 식별 하는 것에 대 한 URI를 매핑합니다. 다음 예제에서는 매핑을 "기관" sessiondirectory.xboxlive.com 경우

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>세션의 요소

각 세션에는 읽기 전용으로 정리 작업 정보 (메타 데이터)와 함께 세션 별로 달라 지는 반복성 및 보안 규칙을 적용 하는 요소 그룹을 포함 합니다. 이 섹션에서는 사용자 세션을 구성 하 여 JSON 파일에 선택한 템플릿에 대 한 JSON 파일을 포함 하는 세션 요소 그룹을을 설명 합니다.

| 참고                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WinRT 래퍼 구현은 HTTP/REST를 사용 하는 세션 및 템플릿 WinRT 기능을 정확 하 게 반영 하는 JSON 개체를 정의 해야 합니다. |

각 요소가 그룹 내에서 두 가지 내부 개체:

-   시스템 개체-이러한 개체에 적용 되며 MPSD에서 해석 하는 고정된 스키마를 있습니다. 유효성이 검사 되며 병합 합니다. MPSD 정의 하 고 어떤 의미가 있는지 알고 있으므로 모아 작동할 수 있습니다. 각 시스템 개체의 전체 정의 참조에 대 한 참조를 **MultiplayerSession 클래스** 에 대 한 참조를 **세션 디렉터리 Uri**합니다.
-   사용자 지정 개체-이러한 개체는 선택 사항 있고 스키마가 없습니다. 멀티 플레이 게임을 관련 된 메타 데이터를 저장할 사용 됩니다. MPSD이이 데이터를 해석할 수 없으면, 이후는 하지 시 처리 됩니다. 게임 데이터 나 저장 된 정보는 title 관리 저장소 (TMS)에 저장 되어야 합니다. TMS 대 한 자세한 내용은 참조 하세요 **Xbox Live 제목 저장소**합니다.

사용자 지정 JSON 개체의 예는 다음과 같습니다.
```JSON
    "custom": {
      "myField1": true,
      "myField2": "string",
      "myField3": 5.5,
      "myField4": { "myObject": null },
      "myField5": [ "my", "array" ]
    }
```

#### <a name="session-constants"></a>세션 상수

세션 상수 작성자에 의해 하거나 세션 템플릿을 만들 때에만 설정 됩니다. /Constants/system 개체는 MPSD 통해 알려진 멀티 플레이 시스템에 대 한 상수를 정의 됩니다. 이 개체와 연결 된 WinRT 래퍼는 표현 합니다 **MultiplayerSessionConstants 클래스**합니다.

/Constants/system 개체 기능 개체, 메트릭 개체, managedInitialization (템플릿 계약 버전 104/105) 또는 peerToPeerRequirements memberInitialization (계약 버전 107) 개체를 포함 하는 항목의 수를 정의할 수 있습니다. 개체, peerToHostRequirements 개체 및 measurementsServerAddresses 개체입니다.


#### <a name="session-properties"></a>세션 속성

/Properties/system 개체를 사용 하 여 MPSD에 대 한 세션 속성을 정의 합니다. 이 개체와 연결 된 WinRT 래퍼는 합니다 **MultiplayerSessionProperties 클래스**합니다. 세션 속성을 언제 든 지 세션 멤버에서 쓸 수 있습니다. JSON 형식의 세션 속성의 예로: joinRestriction, initializationSucceeded 결혼 정보 회사 연결 개체입니다. 이 요소 그룹 사용 예제를 참조 하세요 [대상 세션 초기화 및 QoS](smartmatch-matchmaking.md)합니다.


#### <a name="member-constants"></a>멤버 상수

멤버를 설정할 상수 조인 시 각 세션 멤버에 대 한 합니다. JSON 개체는 /members/ {index} / 상수/시스템입니다. 세션 멤버를 나타내는 WinRT 래퍼 클래스를 **MultiplayerSessionMember 클래스**합니다.


## <a name="member-properties"></a>멤버 속성

멤버 속성은 세션 멤버만 쓰기 가능 합니다. /Members/ {index} / 속성/시스템 개체에 설정 되어 있으며의 요소를 반영 합니다 **MultiplayerSessionMember 클래스**합니다. 다음 예를 참조하세요.

```
    {
      // These flags control the member status and "activeTitle", and are mutually exclusive (it's an error to set both to true).
      // For each, false is the same as not present. The default status is "inactive", i.e. neither present.
      "ready": true,
      "active": false,

      // Base-64 blob, or not present. Empty string is the same as not present.
      "secureDeviceAddress": "ryY=",

      // During member initialization, if any members in the list fail, this member will also fail.
      // Can't be set on large sessions.
      "initializationGroup": [ 5 ],

      // List of the groups I'm in and the encounters I just had.
      // An encounter is a brief interaction with a group. When an encounter is reported, it counts as retroactively joining the group 30 seconds ago and just now leaving.
      // Group names use the session name validation rules (e.g. case-insensitive).
      // On large sessions, groups are used to report who played with who (rather than just session membership). Members
      // that are active in at least one group together at the same time are counted as playing together.
      // Empty lists are the same as no value specified.
      // The set of encounters is a point-in-time property, so it's immediately consumed and will never appear on a response.
      "groups": [ "team-buzz", "posse.99" ],
      "encounters": [ "CoffeeShop-757093D8-E41F-49D0-BB13-17A49B20C6B9" ],

      // Optional list of role preferences the player has specified for role-based game modes.
      // All role names have to match across all members in the session. Role weights are
      // defined from 0-100.
      "RolePreference": { "medic": 75, "sniper": 25, "assault": 50, "support": 100 },

      // QoS measurements by lower-case device token.
      // Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
      // Metrics can be omitted if they weren't successfully measured, i.e. the peer is unreachable.
      // If a "measurements" object is set, it can't contain an entry for the member's own address.
      "measurements": {
        "e69c43a8": {
          "bandwidthDown": 19342,  // Kilobits per second
          "bandwidthUp": 944,  // Kilobits per second
          "custom": { }
        }

      // QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
      // If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
      "serverMeasurements": {
        "server farm a": {
          "latency": 233  // Milliseconds
        }
      },

      // Subscriptions for shoulder taps on session changes. The "profile" indicates which session changes to tap as well as other properties of the registration like the min time between taps.
      // The subscription is named with a title-generated GUID that is also sent back with the tap as a context ID.
      // Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
      // To remove a subscription, set its context ID to null.
      // (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
      // Can't be set on large sessions.
      "subscriptions": {
        "961dc162-3a8c-4982-b58b-0347ed086bc9": {
          "profile": "party",  // Or "matchmaking", "initialization", "roster", "queuehost", or "queue"
          "onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
        },
        "709fef70-4638-4b94-905b-24cb02706eb5": null
      }
    }
```

#### <a name="server-elements"></a>Server 요소

서버는 비 사용자에 게 조인 되거나 세션에 초대 되었습니다. 연결된 된 JSON 개체/서버 {서버 이름} / 상수/시스템 및 {서버 이름} / 서버/속성/시스템 됩니다. 이러한 개체는 서버에서만 쓸 수 있습니다.

| 참고                                                         |
|---------------------------------------------------------------------------|
| {서버 이름} / 서버/상수/시스템 개체를 현재 사용 되지 않습니다. |


### <a name="session-configuration"></a>세션 구성

두 가지 방법으로 세션의 구성을 제어할 수 있습니다.

-   XDP 또는 파트너 센터를 통해 수집 세션 템플릿을 사용 합니다.
-   멀티 플레이 게임을 매 치메이 킹 WinRT Api 또는 REST Api 호출을 사용 합니다. 서식 파일을 반드시 사용 해야 하지만 템플릿을 구성 하려면 값을 포함할 필요가 없습니다. 참고를 제목 서식 파일에 이미 설정 하는 상수를 재정의할 수 없습니다.

별도 JSON 문서 자체 세션 정의에 제공 됩니다. 또한 개발자는 특정 타이틀에 필요한 모든 WinRT 래퍼 기능을 구현 해야 합니다. JSON 문서 및 WinRT 래퍼 코드의 내용을 정확 하 게 서로 반영 해야 및 최신 템플릿 계약 버전을 반영 해야 합니다.

세션에 대 한 스키마는 세션 버전 (주 버전) 및 프로토콜 수정 (부 버전) 버전이 있습니다. 버전 X Xbl-계약 버전 헤더도 결합 됩니다 "100 \* 주 + 부"입니다. 예를 들어, v1.7 제목을 템플릿 계약의 최신 107 가정 하 고 모든 REST 요청에 다음 헤더를 포함 합니다. X-Xbl-Contract-Version: 107.

| 참고                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| 것이 좋습니다 (XSAPI 사용) 하는 대부분의 타이틀에 대 한 105, 계약 버전 및 세션 템플릿 버전 107 사용 합니다. |


### <a name="session-templates"></a>세션 템플릿

각 세션 템플릿은 JSON 문서를 만들어지는 세션에 대 한 프레임 워크를 정의 하 고 새 세션에 대 한 상수를 제공 하는 서비스 구성의 일부분입니다. 자세한 내용은 [MPSD 세션 템플릿을](../service-configuration/session-templates.md)합니다.

## <a name="session-capabilities"></a>세션 기능

기능은 MPSD 세션에서의 MPSD 해당 세션에 적용 해야 하는 동작을 구성 하는 상수입니다. 가장 일반적으로 사용할 XDP 및 파트너 센터 세션 템플릿에서 기능을 설정 합니다. /Constants/system/capabilities 개체에 설정 됩니다. 기능이 없으며, 필요한 경우에 빈 기능 개체를 사용 합니다.

| 참고                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| 제목이 거의 발생 하지 않는다는 변경 하거나 멀티 플레이 WinRT API 또는 매 치메이 킹 WinRT API를 사용 하 여 세션 기능에 액세스 합니다. |

세션 기능으로 표시 됩니다는 **MultiplayerSessionCapabilities 클래스**합니다. 세션에서 지원할 수 있는 대상을 나타내는 부울 값 됩니다.

-   연결
-   게임 플레이
-   큰 크기
-   현재 멤버에 필요한 연결

합니다 **MultiplayerSessionConstants 클래스** 세션 기능에 관련 된 다음 속성을 정의 합니다.

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| 참고                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| 해당 속성 제목을 동적 세션 기능을 정의 하는 경우 세션 상수에 대해 true입니다. |

## <a name="session-size"></a>세션 크기

MPSD 세션의 크기는 해당 세션에서 멤버 수가 결정 됩니다.


### <a name="maximum-session-size"></a>최대 세션 크기

세션의 최대 크기는 최대 세션 멤버를 수용할 수입니다. 으로 표시 됩니다는 **MultiplayerSessionConstants.MaxMembersInSession 속성**합니다. 최대 멤버 크기 /constants/system 개체에 설정 됩니다.

최대 세션 크기는 1 자에서 100 세션 멤버 간 및 100 경우 기본값은 생성 시 설정 하지. 필요한 크기를 100을 초과 하는 경우 세션 "대형" 세션 이라고 하 고 특수 한 방식으로 설정 됩니다.

세션에 대 한 최대 크기를 설정 하면 전체 중에 특정 시나리오를 분리 하는 대로 표시할 열기 슬롯을 발생할 수 있습니다. 예를 들어, 네트워크 또는 전원 오류로 인해 플레이어가 끊어지면 지연을 반영 되지 않습니다 즉시 세션에서. 멤버에서 설명 하는 연결 끊기 검색 기능을 사용 하 여 비활성으로 설정 됩니다 [MPSD 변경 알림을 처리 하 고 검색 연결 끊기](multiplayer-session-directory.md)합니다.

반면에서 하트 비트를 사용 하 여 연결 끊김을 감지 하는 피어 메시 연결을 끊도록 인식 2 ~ 3 초 이내 있고 즉시 플레이어 슬롯 발생할 수 있습니다. 그러나 중재자를 다른 멤버를 제거할 수 없습니다.

### <a name="large-sessions"></a>많은 세션

큰 MPSD 세션 최대 1,000 명의 멤버를 수 있지만 해당 기능이 일부 세션의 모든 멤버 목록을 가져오는 같은 사용할 수 없습니다. 세션 largeness은 표현 합니다 **MultiplayerSessionCapabilities.Large 속성**합니다. 이 속성은 많은 세션을 나타내려면 true로 설정 하 고 "대형" 기능 /constants/system/capabilities 개체에 표시 됩니다. 

## <a name="session-user-states"></a>세션 사용자 상태



MPSD 상태는 세션에 추가한 사용자의 사용자 상태를 정의 합니다. 사용자 상태에 의해 정의 됩니다 합니다 **MultiplayerSessionStatus 열거형**합니다. 또한 사용자는 세션에 추가 되기 전에 "사용 가능한" 상태인 것으로 간주 됩니다.

합니다 **MultiplayerSession.SetCurrentUserStatus 메서드** 세션 사용자 상태를 변경 데 사용할 수 있습니다. 게임에 JSON 문서에서 올바르게 설정 /members/ {index} / 속성/시스템에서 REST에 대 한이 변경은 만들 수 있습니다.


### <a name="reserved-user-state"></a>예약 된 사용자 상태

사용자는 중재자가 세션 내에서 열린 슬롯 중 하나는 fill로 사용자를 선택한 경우 예약 된 사용자 상태에 배치 됩니다. 이 상태에서는 사용자가 아직 공식적으로 세션에 초대를 수락 하거나 피어와 연결을 시작 하려면 세션에 참가 합니다.


### <a name="active-user-state"></a>활성 사용자 상태

사용자 활성 상태인 경우 제목 사용자를 대신 하 여 세션에 참가 했습니다 및 사용자 세션에 적극적으로 참여 합니다. 사용자는 게임을 재생 하는 권한으로이 상태에서 계속 합니다.

제목을 처음 시작 되 면 하는 경우 사용자가 이미 모든 세션의 멤버인 일반적으로 세션 상태를 확인 하 여 참조를 확인 합니다. 사용자 세션 멤버인 경우 제목 게임을로 직접 이동 하 고 참여 하는 모든 로컬 멤버 사용자 상태가 활성으로 설정 수 있습니다.

사용자는 세션에서 재생 하는 동안 활성 상태로 유지 되어야 합니다. 사용자는 게임 내 UI 통해 세션을 퇴사, 하는 경우 사용자와의 세션에서 제거 해야 합니다 **MultiplayerSession.Leave 메서드**합니다. 사용자가 일시적 으로만 떨어진 게임에서 제목 제한 된 경우 제목 해야으로 유지 하면서 사용자 활성 상태인 합당 한 시간에 대 한 합니다. 사용자는 제목을 지정 시간이 지나면 반환 되지 않은 경우 비활성으로 사용자 상태를 변경 하는 것이 적합 합니다.


### <a name="inactive-user-state"></a>비활성 사용자 상태

비활성 상태의 사용자는 게임을 사용 하 여 현재 포함 되지 있지만 세션에서 저장 된 슬롯에 아직. 즉, 사용자는 "비활성."

사용자의 콘솔 세션에서 비활성 사용자 상태를 설정 하는 것에 대 한 책임을 가진 것입니다. 중재자를이 작업을 수행할 수 없습니다. 사용자가 비활성 상태로 전환 하는 예제 시나리오에는 다음이 포함 됩니다.

-   제목 일시 중단 이벤트를 받습니다.
-   사용자 활성화 된 (입력 또는 컨트롤러 응답이) 타이틀에 정의 된 기간에 대 한 합니다. 경쟁력 있는 멀티 플레이어에 대 일 분이 좋습니다.
-   제목 2 분 이상 또는 제목으로 정의한 기간에 대 한 제한 된 모드에서 되었습니다. 이 제한 된 모드 제한 시간은 예상 되는 사용자 관련된 앱 또는 제목에 관련 된 다른 환경을 사용 하 여 제목에서 않을 시간의 양입니다.
-   사용자 세션에서 비정상적으로 연결 되지 않았습니다. 참조 [MPSD 알림 처리를 변경 하 고 검색 연결 끊기](multiplayer-session-directory.md)합니다.

시작 하는 제목의 특정 세션 멤버에 대 한 사용자 상태가 비활성으로 설정 된 경우 제목 일시 중단 됨 또는 사용자에 대 한 활성 세션에서 너무 오래 되었습니다. 제목을 다시 시작 표시 되므로 사용자가 자신이 속한 게임 세션을 진행 하는. 사용자의 상태를 활성 경우 제목 시작 시이 이런 것으로 인해 네트워크 연결을 끊도록 또는 다른 시나리오 제목을 인터럽트 되기 전에 Inactive로 사용자를 설정할 수 없습니다. 두 경우 모두, 제목 게임을 사용 하 여 사용자 및 다른 사용자가 재생을 계속 하려면 다시 연결 하려고 하거나 세션에서 사용자를 제거 해야 합니다.

### <a name="user-state-when-the-session-is-over"></a>사용자 상태 때의 세션을 통해

세션을 통해 경우 게임 플레이 중단 되었습니다. 제목을 사용 하 여 자신을 제거 하려면 모든 사용자를 허용 해야 합니다 **MultiplayerSession.Leave 메서드**합니다. 세션을 벗어날 때 사용자에 게 연결 된 세션 활동을 자동으로 지워집니다.

## <a name="visibility-and-joinability"></a>표시 유형 및 Joinability

두 가지 설정으로 MPSD 수준 세션 액세스가 제어 됩니다: 세션 가시성과 세션 joinability 합니다. 이 항목의 표시 유형 및 joinability 권장 제목은 가장 일반적인 시나리오에 적용 됩니다. 가능한 경우 제목에 이러한 설정을 따라야 합니다. 및-제목 논리 세션에 새 플레이어는 참석할 여부에 대 한 최종, 신뢰할 수 있는 대상을 사용 해야 합니다.


### <a name="session-visibility"></a>세션 표시

세션 가시성 세션 생성 시 설정 된 상수를 기준으로 표시 됩니다. 일반적으로 세션 템플릿에서 정의 하 고 어떤 유형의 사용자 읽기 및 세션에 대 한 쓰기 액세스를 결정 합니다. 으로 정의 된 세션 표시에 대 한 가능한 값은 **MultiplayerSessionVisibility 열거형**합니다. JSON 파일에 가시성 상수에 대 한 허용 설정은 열기, 명백 하 고 개인입니다.


#### <a name="recommended-game-session-visibility-open"></a>권장 되는 게임 세션 표시 유형: 열기

열려 있는 게임 세션 초대 프로세스를 간소화 하는 플레이어 예약이 필요 하지 않습니다. 중재자를 MPSD에서 플레이어 예약 하지 초대를 전송 된 후 트랙만 로컬로 플레이어를 초대 합니다. 따라서 플레이어 즉시 중재자를 연결 하 고 확인할 수는 세션에 참가 해야은 거부 여부 (대기 플레이어 지원 됨) 하는 경우 대기 해야 합니다. 중재자의 궁극적인 권한을 응답 및 유지 또는 세션에 새 멤버를 지시 합니다.

게임 세션 열기 표시를 사용 하 여 초대 플레이어가 제목 시작 되 고 최종 결정에 사용 되기 전에 중재자를 연결할 필요 합니다. 세션은 전체 또는 초대 거부 된 경우 사용자에 게 오류 메시지를 표시 하는 것이 좋습니다.

중재자를에 대 한 연결을 설정 하는 보안 장치 주소가 필요 합니다. 합니다 **MultiplayerSessionProperties.HostDeviceToken 속성** 세션 멤버를 확인 하는 데 사용 된 세션의 현재 중재자 이며는 보안 연결에 대 한 초대 된 플레이어를 사용 해야 하는 장치 주소입니다.

### <a name="session-joinability"></a>세션 Joinability

세션 joinability 어떤 유형의 사용자는 세션에 참가할 수를 결정 합니다. 세션 중 동적으로 설정할 수 있습니다. 세션 joinability에 대 한 가능한 값은:

-   None (기본값)-세션에 참가할 수 있는 사용자에 대 한 제한은 없습니다.
-   로컬-로컬 사용자만 세션에 참가할 수 합니다.
-   다음-로컬 사용자 및 다른 세션 멤버 뒤에 사용자 세션에 참가할 수는 예약이 없는 합니다.

세션 중재자 joinability 설정을 통해 개인 세션을 만들 수 있습니다. 로컬 또는 다음 joinability 만드는 세션에 대 한 액세스를 제한 하 고 개인 만듭니다.

또한 중재자를 해야 기록해 joinability는 이전 세션에 초대 거부 될 수 있습니다 필요한 경우 호스트 수준 세션입니다. 예를 들어, 모든 초대 플레이어가 세션이 이미 가득 찰 때까지 세션에 참가할 도착 하지, 하는 경우 세션이 잠 궜 습니다을 자동으로 세션을 유지 하는 데 필요한 중재자를 조인 플레이어를 지시할 수 있습니다.

## <a name="session-timeouts"></a>세션 시간 제한

타이머 및 기타 외부 이벤트 세션을 변경할 수 있습니다. 세션 제한 시간은 세션 멤버 상태로 있을 수 있는 특정 상태에 자동으로 비활성 인지 세션에서 제거 하기 전에 기간을 정의 합니다. 또한 MPSD 세션 수명을 관리 하는 시간을 지원 합니다.

| 참고                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Constants/system/timeouts, 또는 관리 되는 초기화 개체 템플릿 계약 버전 104/105 내에서 제한 시간 설정 됩니다. 107 또는 이후 버전의 경우 설정은 / 상수/시스템에서 또는 관리 되는 초기화는 개체 내에서 개별적으로 이루어집니다. |

타이머가 만료 되 면 MPSD 세션 업데이트 고 변경 내용을 해당 시점 중재자를 알립니다 자동으로 않습니다. 세션 및 시간 제한 상태를 읽기 직전 업데이트 되는 또는 쓰기 요청이 전송 됩니다. 즉시 업데이트 반환 되는 데이터를 최신 인지 확인 합니다.

| 참고                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| 세션 제한 시간은 누적형 되며 업데이트 시 각 세션 멤버에 대 한 상태 전환에 대 한 하나에만 적용 됩니다. |


### <a name="currently-defined-timeouts"></a>현재 정의 된 시간 제한

이 섹션에서는 MPSD 하 여 현재 정의 된 시간 제한을 설명 합니다. 모든 시간 제한은 밀리초 단위로 지정 됩니다. 값 0은 허용 하 고 즉시 시간이 초과 나타냅니다. 값이 없는 제한 시간을 무한으로 간주 됩니다. 시간 제한을 기본값이 없으므로 null 이면 시간 제한이 무한을 명시적으로 지정 해야 있습니다.
#### <a name="evaluationtimeout"></a>evaluationTimeout

이 시간 제한 세션 멤버를 만들고 업로드 평가 의사 결정에 대 한 시간을 나타냅니다. 없는 의사 결정을 받으면 결정 실패로 계산 합니다. 이 시간 제한은 관리 되는 초기화 개체에 배치 됩니다.


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

이 시간 초과 세션에 연결 하지만 게임에 현재 참여 하지 하는 세션 멤버에 대해 설정 됩니다. 멤버는 기본적으로 2 시간 후 세션에서 제거 됩니다.

| 참고                                                                      |
|----------------------------------------------------------------------------------------|
| 이 시간 제한은 템플릿 계약 버전 104/105 비활성 시간 제한을 지정 됩니다. |

대부분의 경우에서 것이 좋습니다 비활성 제한 시간을 0으로 설정에서 세션 및 해당 슬롯을 지우기 즉시 제거할 비활성 상태로 설정 되어 있는 모든 사용자를 유발 합니다. 이 동작은 새 플레이어 사용자가 비활성 완료 또는 비활성 상태에 도달 하는 경우 신속 하 게 추가할 수 있습니다 있도록 가장 경쟁력 있는 멀티 플레이 게임에 적합 합니다. Co-op 또는 기타 멀티 플레이 디자인을 제목 끊어졌거나 하지 기간에 대 한 제목에 참여 하는 경우 다시 연결 하는 데 시간이 더 사용자를 허용 하도록 좋습니다. 단일 솔루션이 없을 모든 설계 시나리오에 맞는 참고 합니다.

#### <a name="jointimeout"></a>joinTimeout

이 제한 시간은 사용자가 세션에 참가 하는 시간 (밀리초)의 수를 나타냅니다. 조인에 실패 하는 사용자의 예약 제거 됩니다. 이 시간 제한은 관리 되는 초기화 개체에 배치 됩니다.


#### <a name="measurementtimeout"></a>measurementTimeout

이 시간 제한 세션 멤버인 측정을 업로드 해야 하는 시간을 나타냅니다. 측정 업로드에 실패 한 멤버 "timeout"의 실패 이유와 함께 표시 됩니다. 이 시간 제한은 관리 되는 초기화 개체에 배치 됩니다.

| 참고                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 매 치메이 킹을 하는 동안 QoS 측정 한 45 초 시간 제한을 적용 됩니다. 따라서 결혼 정보 회사 연결 되는 동안 30 초 보다 작거나 측정 시간 사용을 사용 하는 것이 좋습니다. |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

이 시간 제한 세션에 연결 하 고 게임을 가져오려고 시도 하는 세션 멤버에 대해 설정 됩니다. 이 문제는 일반적으로 셸의 제목을 대신 하 여 사용자에 연결 하 고 제목 시작 될 의미 합니다. 멤버는 세션에서 제거 하 고 기본적으로 3 분이 지나면 비활성 상태에 배치 됩니다.

| 참고                                                          |
|----------------------------------------------------------------------------|
| 이 시간 제한은 계약 버전 104/105 준비 시간 제한을 지정 됩니다. |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

이 시간 제한은 다른 사용자가 세션에 추가 하지만 세션에 아직 조인 되지 않은 세션 멤버에 대해 설정 됩니다. 예약이 삭제 되 고 멤버는 비활성 제한 시간이 만료 되 면 합니다. 기본값은 30 초입니다.

| 참고                                                             |
|-------------------------------------------------------------------------------|
| 이 시간 제한은 104/105 계약 버전에 대 한 예약 된 시간 제한을 지정 됩니다. |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

이 시간 후 삭제 될 때 세션 비게 시간 (밀리초) 수를 나타냅니다. 기본값은 0입니다

| 참고                                                                 |
|-----------------------------------------------------------------------------------|
| 이 시간 제한은 계약 버전 104/105 sessionEmpty 시간 제한을 지정 됩니다. |


### <a name="session-timeout-example"></a>세션 시간 제한 예제

1.  세션은 4 명의 플레이어를 사용 하 여 시작 합니다.
2.  A와 B 라는 두 플레이어는 정전으로 인해 연결이 끊어집니다. 게임에서 관련 상태는 활성 상태로 유지 됩니다.
3.  다른 두 플레이어, C 및 D, 올바르게 사용 하 여 종료 합니다 **MultiplayerSession.Leave 메서드**합니다.
4.  세션 유지 열기, A와 B 연결이 끊긴 플레이어를 사용 하 여 있지만 여전히 활성 상태입니다.
5.  몇 일이 지난 후 플레이어는 반환 하 고 게임을 시작 합니다.
6.  A의 플레이어 게임 A의 구성원 인 세션에 대 한 확인 (읽기 수행) 며칠 전에서 분리 된 세션을 찾습니다.
7.  세션 (A 및 B) 세션에 여전히 두 선수에 대해 상태 검사를 수행 합니다.
    1.  플레이어는 제목, 실행 중 이므로 플레이어에 대해 존재 여부 확인에 성공 하 고 일치 항목에서 활성 상태인 플레이어의 동일 하 게 유지 합니다.
    2.  플레이어 B 제목을 실행 되지 않습니다. 따라서 플레이어 B 실패 하 고 서비스에 대 한 현재 상태 검사를 비활성으로 플레이어 B의 상태를 설정합니다. 이 시점에서 비활성 제한 시간이 시작 플레이어 2.

8.  플레이어는 올바르게 사용 하 여 세션을 종료 합니다 **둡니다** 메서드.
9.  모든 사용자가 다음 읽기 또는 쓰기 세션에서 제거 되는 플레이어 B에 대 한 비활성 시간 제한이 만료 됩니다.
10. 이제 세션 0 멤버가 하 게 되 고 서비스에서 제거 됩니다.

예제에서는 세션에 대 한 비활성 시간 제한 있는지 확인 한 후 즉시 0, 플레이어 B 시간 초과 설정 된 경우 7a 단계에서 및 세션 쓰기가 제거할 수 있습니다. 이 경우 추가 하지 않고도 세션 닫히는 읽거나 쓰는 세션에 있습니다.


## <a name="multiple-signed-in-users-on-a-single-console"></a>단일 콘솔에서 여러 로그인 사용자


여러 사용자가 콘솔에 로그인 세션에서 다른 사용자에 게 표시 되지 않는 반면 게임 세션에 포함 되도록 일부 사용자에 대 한 가능한 또는 현재 제목에 활성화 되지 됩니다. 또한 게임 초대 받은 고 여러 사용자가 게임 세션 멤버 자격에는 영향을 주는 것에 대 한 허용 수 있습니다. 제목을 모든 세션의 멤버 자격 시나리오를 올바르게 처리 하려면 이러한 요소를 고려해 야 합니다.

일반적인 시나리오에서는 새 플레이어 로그인, 게임을 활성화 하 고 기존 게임 세션에 추가 해야 합니다. 으로 만드는 새 게임 세션을 사용 하 여 제목을 추가 해야 사용자 게임 플레이 중 적합 합니다.

여러 사용자가 로그인을 사용 하 여 하나 이상의 사용자가 다른 게임 세션에 초대를 받을 수도 있습니다. 제목 어떤 특정 방식으로 이러한 시나리오를 처리할 필요가 없습니다. 세션 상태 및 멤버 이벤트에 게임 세션 및 사용자 멤버 자격 업데이트의 제목을 게 알립니다.

온라인 세션에 대 한 여러 사용자가 로그인을 처리 하려면 제목 구독 자격 증명 입력을 별도 사용 하 여 모든 사용자에 대 한 탭에 대 한 **XboxLiveContext 클래스** 각 사용자에 대 한 개체입니다. 제목을 사용 하는 **MultiplayerSession.ChangeNumber 속성** 탭 세션의 특정 변경 내용을 확인 하 고 중복 어깨를 무시 합니다.


## <a name="process-lifecycle-management"></a>프로세스 수명 주기 관리


다중 접속 아닌 제목을 마찬가지로 멀티 플레이 세션에서 제목을 제목 일시 중단 및 프로세스 수명 주기 이벤트의 종료 발생할 수 있습니다. 따라서 세션 중재자를 세션 상태를 주기적으로 저장 해야 합니다. 중재자를 일시 중단 하는 경우 제목을 중재자 마이그레이션 시도 하 고 새 중재자 세션 상태를 복원할 수 있도록 게임 상태를 적절 하 게 저장 해야 합니다. 세션 MPSD에서 여전히 유효으로 전체 멀티 플레이 세션을 일시 중단 했다가 나중에 다시 시작 하려면 다음 가능 합니다. 하나의 지정 된 피어 호스트 일반적으로 게임 전역 게임 상태를 업데이트 해야 합니다.


### <a name="storage-of-game-metadata"></a>게임 메타 데이터 저장소

제목을은 MPSD 세션에서 게임 메타 데이터를 저장합니다. 게임 메타 데이터에 세션 데이터를 표시 하 고 찾고 게임 세션에 참가 제목을 사용 하는 데 필요한 정보입니다. 사용자 지정 속성 섹션에는 세션 멤버에 대 한 예를 들어, 플레이어 색 등 세션에 대 한 기본 플레이어 무기 player 관련 메타 데이터를 저장 하는 제목. 세션 전체 메타 데이터를 현재 map 예를 들어 MPSD 세션의 전역 사용자 지정 속성 섹션에 저장 됩니다.


### <a name="storage-of-game-state"></a>게임 상태 저장소

게임 상태 제목 관리 저장소 (TMS)에 저장 됩니다 사용 하는 **저장소 서비스를 제목**합니다. 이 위치를 사용 하 여 storage 제목을 사용 권한 문제 없이 중재자를 마이그레이션할 수 있습니다. 참조 [마이그레이션하는 중재자](migrating-an-arbiter.md)합니다.

| 참고                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| 일시 중단 됩니다 하지 않는 일 분 마다 한 번, 보다 더 자주 TMS를 게임 상태를 저장 하려면 제목 하지 않아야 합니다. |

## <a name="cleanup-of-inactive-sessions"></a>비활성 세션의 정리

sessionEmptyTimeout을 0으로 설정 하는 경우 마지막 플레이어 세션을 벗어나면 MPSD 세션을 자동으로 삭제 됩니다. 참조를 사용 하지 않는 세션 중단 되거나 연결이 끊어진 후 플레이어를 포함 하지 못하도록 하는 방법에 알아보려면 [MPSD 변경 알림 처리 및 연결 끊기 감지.](multiplayer-session-directory.md)합니다. 충돌 후 사용 하지 않는 세션의 잘못 된 처리 또는 연결 끊기 플레이어 제목을 세션을 쿼리 하는 경우 문제가 발생할 수 있습니다.

제목 쿼리 모든 세션을 특정 사용자에 대 한 호출 하 여 비활성 세션을 정리 하는 방법이 권장된 되는 **MultiplayerService.GetSessionsAsync 메서드** 세션을 평가한 후 합니다. 유효 하지 않은 세션을 발견할 경우 제목 호출을 **MultiplayerSession.Leave 메서드** 세션에서 모든 로컬 플레이어에 대 한 합니다. 이 호출은 멤버 개수를 0 최종적으로 삭제 하 고 세션을 정리 합니다.

## <a name="session-arbiter"></a>세션 중재자


만 일부 멀티 플레이 메서드 게임 세션 내에서 하나의 클라이언트에서 호출 되어야 합니다. 이 클라이언트는 콘솔 세션에 참여 하는 중재자 또는 호스트 중 하나입니다. 하나 이상의 세션 멤버가 게임 이면 세션 조인 진행에서을 모니터링 하는 중재자가 있어야 합니다.


### <a name="setting-the-arbiter"></a>중재자를 설정합니다.

세션을 만들 때 클라이언트는 단일 콘솔 중재자를로 지정 합니다. 참조 [방법: 여기서 중재자는 MPSD 세션 설정](multiplayer-how-tos.md)합니다.


### <a name="saving-session-state"></a>세션 상태를 저장합니다.

에 설명 된 대로 **프로세스 수명 주기 관리**, 중재자를 세션 상태를 주기적으로 저장 해야 합니다. 새 중재자 제목에 중재자 마이그레이션의 경우 세션 상태를 복원할 수 있어야 합니다. 자세한 내용은 [마이그레이션하는 중재자](multiplayer-how-tos.md)합니다.


### <a name="managing-game-session-members-and-joins-in-progress"></a>진행 중인 조인 및 게임 세션 멤버 관리

세션 중재자의 가장 중요 한 역할 재생 게임 세션으로 사용자를 관리 하는 것입니다. 이 게임 초대를 처리 하 고 대기 플레이어에 게 알리지 고 플레이어가 게임을 종료 하는 작업을 포함 합니다.


#### <a name="receiving-notifications"></a>알림 수신

중재자를 사용 하 여 게임 세션에 참가 하려는 새 플레이어를 위해 수신 해야 합니다 **RealTimeActivityService.MultiplayerSessionChanged 이벤트**합니다.


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>플레이어가 게임에 빈 슬롯에 맞게 찾기

중재자를 이러한 작업 중 하나에서 게임에 빈 슬롯을 채우기 위해 플레이어를 찾습니다.   제목 로비 세션이 나 다른 메커니즘을 사용 하 여 지연 된 조인을, 하는 경우이 메커니즘을 사용 하 여 새 세션 멤버를 찾습니다.
-   다른 일치 티켓 세션을 만듭니다.

참고 [방법: 결혼 정보 회사 연결 하는 동안 열려 있는 세션 슬롯 채우기](multiplayer-how-tos.md)합니다.


#### <a name="handling-invited-session-members"></a>처리 세션 멤버 초대

중재자 멤버 초대 된 세션을 모니터링 하 고 단일 사용자로 초대 사이의 최소 간격을 적용 해야 합니다. 참고 [방법: 게임 초대 보내기](multiplayer-how-tos.md)합니다.
