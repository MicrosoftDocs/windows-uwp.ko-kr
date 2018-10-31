---
title: 멀티 플레이 세션 세부 정보
author: KevinAsgari
description: Xbox Live 멀티 플레이 세션의 세부 정보에 알아봅니다.
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나 멀티 플레이 2015 세션, mpsd
ms.localizationpriority: medium
ms.openlocfilehash: ad551d07d94f90d89f8abda0b188ea281b4930c2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5834693"
---
# <a name="mpsd-session-details"></a>MPSD 세션 세부 정보

이 문서에는 다음과 같은 섹션이 포함되어 있습니다.
* 세션 개요
* 세션 기능
* 세션 크기
* 세션 사용자 상태
* 표시 여부 및 파티
* 세션 시간 제한
* 단일 콘솔에서 여러 로그인 사용자
* 프로세스 수명 주기 관리
* 비활성 세션 정리
* 세션 중재자

## <a name="session-overview"></a>세션 개요

멀티 플레이어 세션 디렉터리 (MPSD) 세션 세션 이름이 고 세션에 대 한 기본 설정을 제공 하는 JSON 문서가 세션 서식 파일의 인스턴스로 식별 됩니다. 템플릿은은 GUID는 서비스 구성 id (서비스 안내) 서비스 구성의 일부입니다. [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com) 에서이 템플릿을 찾을 수 있고 [Windows 개발자 센터](https://partner.microsoft.com/dashboard/windows/overview) 서비스 구성을 수집, 관리 및 보안 정책을 사용 하는 개발자 지향 리소스. 세션 MPSD를 통해 액세스 되 면, 사용자 권한 부여 XDP 또는 Windows 개발자 센터를 통해 개발자가 설정할 액세스 정책에 따라 서비스 구성에 대해 수행 됩니다. 세션 멤버십 유효성 검사와 같은 보조 액세스 검사 세션 서비스 구성에 대 한 액세스 권한이 부여 된 후에 로드 될 때 세션 수준에서 수행 됩니다.

이 항목에서는 서식 파일에서는 계약 버전 107, Xbox One 용 현재 MPSD 휠체어 버전 가정 합니다. 계약 버전 105 (104와 동일)를 기반으로 템플릿을 정의한 경우 107 버전을 지원 하기 위해 이러한 변경 해야 합니다. 지침, [일반 멀티 플레이 2015 마이그레이션 문제](common-issues-when-adapting-multiplayer.md)를 참조 하세요.

| 중요                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 템플릿을 통해 설정 된 기능에는 MPSD 쓰기를 통해 변경할 수 없습니다. 값을 변경 하려면 만들기 하 고 새 템플릿을 필요한 변경 사항으로 제출 해야 합니다. 모든 항목 템플릿을 통해 설정 되지 않은 MPSD 쓰기를 통해 변경 될 수 있습니다. |

### <a name="session-reference"></a>세션 참조

각 MPSD 세션 **MultiplayerSessionReference 클래스**에서 멀티 플레이 WinRT API를 표현 하는 세션 참조로 참조 고유 하 게 됩니다. 세션 참조 이러한 문자열 값이 포함 됩니다.

-   서비스 구성 id (서비스 안내)
-   세션 템플릿 이름
-   Session name(세션 이름)

세션 참조 아래와 같이 세션을 식별 하는 데 URI에 매핑합니다. 다음 예제에서는 매핑의 "기관은" sessiondirectory.xboxlive.com입니다.

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>세션의 요소

각 세션에 읽기 전용 정리 작업 정보 (메타 데이터)와 함께 세션 요소에 따라 보안과 가변성 규칙을 적용 하는 요소의 그룹이 있습니다. 이 섹션에서는 세션 요소 및 템플릿 선택 하는 JSON 파일 세션을 구성 하는 JSON 파일에 포함 된 그룹을 설명 합니다.

| 참고                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HTTP/REST 구현의 WinRT 래퍼를 사용 중인 경우 세션 및 템플릿 정확 하 게 WinRT 기능을 반영 하는 JSON 개체를 정의 해야 합니다. |

각 요소 그룹 내에서 두 개의 내부 개체를 가지 있습니다.

-   시스템 개체-이러한 개체에 적용 되 고 MPSD에 의해 해석 고정된 스키마를 있습니다. 유효성을 검사 되며 병합 합니다. MPSD를 정의 하 고 그 의미를 알고, 이후 의존 작동할 수 있습니다. 시스템 개체 각각의 전체 정의 대 한 참조를 **세션 디렉터리 Uri**에 대 한 **MultiplayerSession 클래스** 와 참조에 대 한 참조.
-   사용자 지정 개체-이러한 개체는 선택 사항, 있고 스키마 없습니다. 멀티 플레이 게임 관련 메타 데이터 저장에 사용 됩니다. MPSD이이 데이터를 해석할 수 없는, 이후 시 하지 처리 됩니다. 게임 데이터 나 저장된 한 정보는 제목 관리 저장소 (TMS)에 저장 되어야 합니다. TMS에 대 한 자세한 **Xbox Live 타이틀 저장소**를 참조 하세요.

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

세션 상수 세션 템플릿 또는 작성자가 생성 시만 설정 됩니다. /Constants/system 개체는 MPSD 통해 알려진 멀티 플레이 시스템에 대 한 상수를 정의할 수 사용 됩니다. 이 개체와 연결 된 WinRT 래퍼 **MultiplayerSessionConstants 클래스**에 의해 표시 됩니다.

/Constants/system 개체는 다양 한 기능 개체, 메트릭 개체, managedInitialization (템플릿 계약 버전 104/105) 또는 peerToPeerRequirements memberInitialization (계약 버전 107) 개체를 포함 하 여 항목을 정의할 수 있습니다. 개체, peerToHostRequirements 개체 및 measurementsServerAddresses 개체입니다.


#### <a name="session-properties"></a>세션 속성

/Properties/system 개체를 사용 하 여 MPSD 세션 속성을 정의 합니다. 이 개체와 연결 된 WinRT 래퍼는 **MultiplayerSessionProperties 클래스**입니다. 세션 속성은 언제 든 지 세션 구성원이 쓰기 가능 합니다. JSON 형식의 세션 속성의 예로: joinRestriction, initializationSucceeded, 및 매치 메이 킹 개체입니다. 이 요소 그룹의 사용 예, [대상 세션 초기화 단계 및 QoS를](smartmatch-matchmaking.md)참조 하세요.


#### <a name="member-constants"></a>멤버 상수

멤버 상수 가입 시 각 세션 멤버 설정 합니다. JSON 개체에는 /members/ {인덱스} / 상수/시스템입니다. 세션 구성원을 나타내는 WinRT 래퍼 클래스는 **MultiplayerSessionMember 클래스**입니다.


## <a name="member-properties"></a>구성원 속성

구성원 속성은 세션 멤버에 의해서만 쓰기 가능 합니다. 이러한 /members/ {인덱스} / 속성/시스템 개체에 설정 되며 **MultiplayerSessionMember 클래스**의 요소를 반영 합니다. 다음 예제를 참조하세요.

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

#### <a name="server-elements"></a>서버 요소

서버는 가입 있거나 세션에 초대 된 비-사용자가 있습니다. 연결 된 JSON 개체는 / 서버 {server name} / 상수/시스템 및/서버 {server name} / 속성/시스템입니다. 이러한 개체는 서버에 의해서만 쓰기 가능 합니다.

| 참고                                                         |
|---------------------------------------------------------------------------|
| {Server name} / 서버/상수/시스템 개체 현재 사용 되지 않습니다. |


### <a name="session-configuration"></a>세션 구성

세션의 구성을 제어 하는 방법은 두 가지가 있습니다.

-   세션 템플릿 XDP 또는 Windows 개발자 센터를 통해 수집을 사용 합니다.
-   멀티 플레이어 및 매치 메이 킹 WinRT Api 또는 REST Api에 대 한 호출을 사용 합니다. 템플릿을 사용 해야 하지만 템플릿을 구성 하려는 값을 포함할 필요가 없습니다. 참고 타이틀 템플릿에서 이미 설정 되어 상수를 재정의할 수 없습니다.

자체 세션을 정의 하는 별도 JSON 문서 제공 됩니다. 또한 개발자는 특정 제목에 필요한 모든 WinRT 래퍼 기능을 구현 해야 합니다. JSON 문서 및 WinRT 래퍼 코드의 내용을 서로 정확 하 게 반영 해야 하 고 템플릿 계약 최신 반영 해야 합니다.

세션에 대 한 스키마 세션 버전 (주 버전) 및 프로토콜 버전 (부 버전)를 사용 하 여 버전입니다. 버전으로 Xbl 계약 버전 X 헤더에 결합 됩니다 "100 \ * 주요 + 부". V1.7 제목 템플릿 계약의 최신 107 가정 하 고 모든 REST 요청에서 다음 헤더를 포함 하는 예를 들어: Xbl 계약 버전 X: 107 합니다.

| 참고                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| 계약 버전 105, 및 107 세션 템플릿 버전을 사용 하 여 대부분의 제목 (XSAPI 사용)이 좋습니다. |


### <a name="session-templates"></a>세션 템플릿

각 세션 템플릿 만들어지는 세션에 대 한 프레임 워크를 정의 하 고 새 세션에 대 한 상수를 제공 하는 서비스 구성의 일부로 JSON 문서를입니다. 자세한 내용은 [MPSD 세션 템플릿](../service-configuration/session-templates.md)을 참조 하세요.

## <a name="session-capabilities"></a>세션 기능

MPSD 세션에서 해당 세션에는 MPSD 적용 해야 하는 동작을 구성 하는 상수는 기능입니다. 가장 일반적으로 세션 템플릿에서 접근 권한 값을 설정 하려면 XDP 및 Windows 개발자 센터 사용 합니다. /Constants/system/capabilities 개체에서 설정 됩니다. 기능은 필요한 경우 빈 기능 개체를 사용 합니다.

| 참고                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| 타이틀 거의 없습니다 변경 하거나 멀티 플레이 WinRT API 또는 매치 메이 킹 WinRT API를 사용 하 여 세션 기능에 액세스 합니다. |

세션 기능 **MultiplayerSessionCapabilities 클래스**에 의해 표시 됩니다. 된 세션에서 지원할 수 있는 여부를 나타내는 부울 값:

-   Connectivity
-   게임 플레이
-   큰 크기
-   활성 구성원에 필요한 연결

**MultiplayerSessionConstants 클래스** 세션 접근 권한 값과 관련 된 다음 속성을 정의 합니다.

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| 참고                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| 해당 속성 제목에 동적 세션 기능을 정의 하는 경우 세션 상수의 경우도 마찬가지입니다. |

## <a name="session-size"></a>세션 크기

MPSD 세션의 크기는 해당 세션에서 멤버의 수에 따라 결정 됩니다.


### <a name="maximum-session-size"></a>최대 세션 크기

세션의 최대 크기는 수용할 수 있는 세션 멤버의 최대 수입니다. **MultiplayerSessionConstants.MaxMembersInSession 속성**으로 표현 됩니다. 최대 구성원 크기가 /constants/system 개체에서 설정 됩니다.

최대 세션 크기 세션 멤버 1과 100 사이 이며 생성 설정 되지 않은 100 경우 기본값은. 필요한 크기가 100으로 경우 세션 "큰" 세션 라고 하 고 특별 한 방식으로 설정 됩니다.

세션에 대 한 최대 크기를 설정 하면 전체 중에 특정 시나리오를 분리 하는 대로 표시를 열고 슬롯을 발생할 수 있습니다. 예를 들어, 네트워크 또는 전원 실패의 결과로 플레이어 연결이 끊어지는, 지연은 즉시 반영 되지 세션 합니다. 멤버가는 [MPSD 변경 알림을 처리 하 고 연결 끊기 감지에](multiplayer-session-directory.md)설명 된 연결 끊기 검색 기능을 사용 하 여 비활성으로 설정 됩니다.

비교에서 하트 비트를 사용 하 여 연결 끊김을 감지 하는 피어 메시 2 ~ 3 초 이내 연결을 끊은의 인식 종종 작업이 아니며 즉시 플레이어 슬롯 발생할 수 있습니다. 그러나 중재자 다른 멤버를 제거할 수 없습니다.

### <a name="large-sessions"></a>큰 세션

큰 MPSD 세션은 최대 1000 명의 구성원을 가질 수 있지만 일부 세션 기능 등의 모든 멤버 목록을 가져오는 비활성화. 세션 largeness **MultiplayerSessionCapabilities.Large 속성**으로 표시 됩니다. 이 속성은 큰 세션을 나타내는 true로 설정 되어 및 "큰" 접근 권한 값은 /constants/system/capabilities 개체에 표시 됩니다. 자세한 내용은 [세션 기능]()을 참조 하세요.

## <a name="session-user-states"></a>세션 사용자 상태



MPSD 세션에 추가 된 사용자의 상태와 사용자 상태를 정의 합니다. 사용자 상태 **MultiplayerSessionStatus 열거형**에서 정의 됩니다. 사용자 세션에 추가 되는 "사용할 수 있는" 상태로 두어야 할 간주 됩니다.

사용자 세션 상태를 변경 하려면 **MultiplayerSession.SetCurrentUserStatus 메서드** 를 사용할 수 있습니다. 이 변경 /members/ {인덱스} / 속성/시스템 게임 세션 JSON 문서에서 올바르게 설정 하 여 나머지 만들 수 있습니다.


### <a name="reserved-user-state"></a>예약 된 사용자 상태

중재자 세션 내에서 사용자가 채우기 열린 슬롯 중 하나를 선택 하면 사용자는 예약 된 사용자 환경에 배치 됩니다. 이 상태에서는 사용자가 아직 공식적으로 세션에 초대를 수락 하거나 피어를 사용 하 여 연결을 시작 하려면 세션에 참가 합니다.


### <a name="active-user-state"></a>활성 사용자 상태

사용자가 활성 상태 이면 제목에 사용자를 대신 하 여 세션에 참가 하 고 사용자 세션에 참여 적극적으로 합니다. 사용자가 게임을 플레이 하는 합리적인으로이 상태에서 계속 됩니다.

타이틀을 처음 실행할 때 경우 사용자가 이미 모든 세션의 구성원 일반적으로 세션 상태를 확인 하 여 확인 해야 합니다. 사용자 세션 구성원 인 경우 제목 게임에 직접 이동 하 고 참여 로컬 구성원이 활성 사용자 상태를 설정할 수 있습니다.

사용자 세션에서 재생 하는 동안 활성 상태를 유지 해야 합니다. 사용자가 게임에서 UI 통해 세션을 **MultiplayerSession.Leave 메서드**를 사용 하 여 세션에서 사용자를 제거 해야 합니다. 사용자가 일시적 으로만 떨어져 게임에서 제목, 제한 된 경우 제목 해야으로 유지 하면서 사용자 활성 상태에서 적절 한 시간에 대 한 합니다. 비활성 사용자 제목을 지정 시간이 지난 후 반환 되지 않은 경우 사용자 상태를 변경 하는 것이 좋습니다.


### <a name="inactive-user-state"></a>비활성 사용자 상태

비활성 상태에서 사용자가 현재는 게임에 참여 하지는 하지만 세션에 저장 된 슬롯에 아직. 즉, 사용자가 "비활성."

사용자의 콘솔 세션에서 비활성 사용자 상태를 설정 하는 작업을 담당 하는 경우 중재자가이 작업을 수행할 수 없습니다. 사용자는 비활성 상태로 전환 되는 예제 시나리오는 다음과 같습니다.

-   제목 일시 중단 이벤트를 받습니다.
-   사용자가 활성화 되지 않은 (입력 또는 컨트롤러 응답이) 제목에서 정의한 기간입니다. 경쟁 멀티 플레이어 2 분이 좋습니다.
-   2 분 이상 또는 제목에 정의 된 기간에 대 한 제한 된 모드로 제목이 이었습니다. 이 제한 된 모드 제한 시간은 예상 시간 관련된 앱 또는 제목에 관련 된 다른 환경을 사용 하 여 제목 로부터 사용자 수 있습니다.
-   사용자 세션에서 비정상적 끊어졌습니다. [MPSD 변경 알림을 처리 하 고 연결 해제 감지를](multiplayer-session-directory.md)참조 하세요.

제목 시작 하는 경우 특정 세션 구성원에 대 한 사용자 상태가 비활성으로 설정 되어 제목 일시 중단 되었거나 사용자 세션에서 시간이 너무 길면 활성화 되었습니다. 제목을 다시 시작 하기 때문에 사용자가 자신이 속해 있는 게임 세션을 계속 표시가입니다. 사용자의 상태는 활성 경우 제목 시작 시이 상황 때문일 네트워크 연결을 끊은 또는 다른 시나리오 제목 중단 하기 전에 사용자 비활성으로 설정할 수 없습니다. 타이틀 두이 경우 모두에서 게임을 사용 하 여 사용자와 다른 사용자가 계속 재생을 다시 연결을 시도 하거나 세션에서 사용자를 제거 해야 합니다.

### <a name="user-state-when-the-session-is-over"></a>사용자 상태 때 the 세션을 통해

세션을 게임 플레이 필터링 합니다. 제목은 **MultiplayerSession.Leave 메서드**를 사용 하 여 자신을 제거 하려면 모든 사용자를 허용 해야 합니다. 세션을 벗어날 때 자동으로 사용자와 관련 된 세션 활동 지워집니다.

## <a name="visibility-and-joinability"></a>표시 여부 및 파티

두 설정으로 MPSD 수준에서 세션 액세스 제어 됩니다: 세션 유형과 세션 파티 합니다. 이 항목의 표시 여부 및 파티 권장 가장 일반적인 제목 시나리오에 적용 됩니다. 가능한 경우 제목에 따라 이러한 설정 및 제목에 논리를 사용 하 여 새 플레이어는 세션에 참석할 여부에 대 한 최종, 신뢰할 수 있는 결정할 해야 합니다.


### <a name="session-visibility"></a>세션의 표시 여부

세션 가시성 세션 만들기에 설정 된 상수도 표시 됩니다. 세션 템플릿에 일반적으로 정의 하 고 어떤 유형의 사용자 읽기 및 세션에 대 한 쓰기 액세스를 결정 합니다. 세션 표시 여부에 대 한 가능한 값은 **MultiplayerSessionVisibility 열거형**에서 정의 됩니다. JSON 파일의 표시 여부 상수에 허용 된 설정은 open을 표시 하 고 개인 합니다.


#### <a name="recommended-game-session-visibility-open"></a>게임 세션 가시성 권장: 열기

열려 있는 게임 세션에 초대 프로세스를 간소화 하는 플레이어 예약 필요 하지 않습니다. 중재자는 MPSD에서 플레이어 예약 하지 초대 집합이 전송 되었음을 후만 트랙 로컬로 플레이어를 초대 합니다. 따라서 플레이어 수 즉시 연결 중재자 하 고 지를 결정은 세션에 참가 해야, 거부 됩니다 (대기 플레이어는 지원) 하는 경우 기다려야 합니다. 중재자 ultimate 기관이 응답 하며 유지 하거나 세션을 새 멤버에 지시 합니다.

게임 세션을 열고 표시 여부를 사용 하 여 초대 받은 플레이어가 제목을 실행 되 고 최종 결정 했습니다. 전에 중재자에 연결 해야 합니다. 세션은 전체 또는 초대 거부 되 면 사용자에 게 오류 메시지를 표시 하는 것이 좋습니다.

중재자에 대 한 연결을 설정 하는 보안 장치 주소가 필요 합니다. **MultiplayerSessionProperties.HostDeviceToken 속성** 세션 멤버는 세션의 현재 중재자 확인을 사용 하 고 연결에 대 한 사용 해야 하는 보안 장치 주소는 초대 받은 플레이어 합니다.

### <a name="session-joinability"></a>세션 파티

세션 파티 어떤 유형의 사용자는 세션에 참가할 수를 결정 합니다. 세션 중 동적으로 설정할 수 있습니다. 파티 세션에 대 한 가능한 값은 다음과 같습니다.

-   없음 (기본값)-사용자 세션에 참가할 수에 제한이 없습니다.
-   로컬-로컬 사용자만 세션을 가입할 수 있습니다.
-   뒤에-예약 없이 세션 로컬 사용자 및 다른 세션 멤버 뒤에 사용자를 만들 가입할 수 있습니다.

세션 중재자 파티 설정을 통해 개인 세션을 만들 수 있습니다. 로컬 또는 열어 파티 만들기 세션에 대 한 액세스를 제한 하 고 개인 만듭니다.

또한 중재자 해야는 추적 세션 파티 이전 세션에 초대 되도록 거부할 수 필요한 경우 호스트 수준에서. 예를 들어 세션 이미 가득 찰 때까지 세션에 초대 받은 모든 플레이어 하지 되었는지를, 하는 경우 세션이 잠긴 및 세션을 자동으로 유지 하는 데 필요한 중재자 조인 플레이어 지시할 수 있습니다.

## <a name="session-timeouts"></a>세션 시간 제한

타이머와 외부의 다른 이벤트에서 세션을 변경할 수 있습니다. 세션 시간 제한 비활성 또는 세션에서 제거 자동으로 전에 특정 상태에 세션 구성원 수 남아 기간을 정의 합니다. 또한 MPSD 세션 수명을 관리 하는 시간을 지원 합니다.

| 참고                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Constants/system/timeouts, 또는 관리 되는 초기화 개체 템플릿 계약 버전 104/105 내 시간 제한 설정 됩니다. 107 이상 버전에 대 한 설정은/상수/시스템에서 또는 관리 되는 초기화 개체 내에서 개별적으로 수행 됩니다. |

타이머가 만료 되 면 MPSD 세션 업데이트 및 변경 내용이 즉시 중재자 알림 자동으로 않습니다. 세션 시간 제한 상태의 읽기 전에 즉시 업데이트 되는 페이지 또는 요청을 작성 합니다. 즉시 업데이트는 반환 된 데이터는 가장 최신 상태임을 보장 합니다.

| 참고                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| 세션 시간 제한, 누적 되며 각 세션 구성원을 업데이트에 대 한 상태 전환에 대 한 하나에만 적용 됩니다. |


### <a name="currently-defined-timeouts"></a>현재 정의 된 시간 제한

이 섹션에서는 현재 MPSD에 정의 된 시간 제한을 설명 합니다. 시간 제한은 밀리초로 지정 됩니다. 값이 0 수 있으며 즉시 적용할 제한 시간을 나타냅니다. 값이 없는 시간 제한 무한으로 간주 됩니다. 시간 제한을 기본값에 수 있으므로 시간 제한을 무한 null로 명시적으로 지정 해야 있습니다.
#### <a name="evaluationtimeout"></a>evaluationTimeout

이 제한 시간을 높이고 평가 의사 결정을 업로드 세션 구성원에 대 한 시간을 나타냅니다. 아무 결정을 받은 경우 결정 오류가로 계산 됩니다. 이 시간 제한 관리 되는 초기화 개체에 배치 됩니다.


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

현재 게임에 참여 하지는 않지만 세션에 참가 세션 구성원에 대해이 시간 제한 설정 됩니다. 멤버는 기본적으로 2 시간 후 세션에서 제거 됩니다.

| 참고                                                                      |
|----------------------------------------------------------------------------------------|
| 이 시간 제한 템플릿 계약 버전 104/105 비활성 시간 제한을 지정 됩니다. |

대부분의 경우에서 좋습니다 비활성 시간 초과를 0으로 설정 일으키는 모든 사용자가 세션에 지울 해당 슬롯에서 즉시 제거할 비활성 상태로 설정 됩니다. 이 동작은 새 플레이어를 신속 하 게 추가할 수 하므로 비활성 사용자의 비활성 상태에 도달한 경우, 있도록 가장 경쟁 멀티 플레이어 게임에 적합 합니다. 게임이 협동 또는 기타 멀티 플레이 디자인에 대 한 사용자가 더 끊어지거나 하지 기간에 대 한 제목에 참여 하는 경우 다시 연결 시간을 허용 하도록 타이틀을 할 수 있습니다. 단일 솔루션에 없는 모든 디자인 시나리오 맞는지 참고 합니다.

#### <a name="jointimeout"></a>joinTimeout

이 시간 제한 사용자가 세션에 참가 하는 시간 (밀리초) 수를 나타냅니다. 가입에 실패 하는 사용자의 예약 제거 됩니다. 이 시간 제한 관리 되는 초기화 개체에 배치 됩니다.


#### <a name="measurementtimeout"></a>measurementTimeout

이 시간 제한 세션 구성원 측정을 업로드 하는 시간을 나타냅니다. 구성원 측정 업로드 하지 않는 한 "제한" 오류 원인을 표시 됩니다. 이 시간 제한 관리 되는 초기화 개체에 배치 됩니다.

| 참고                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 매치 메이 킹, 하는 동안 QoS 측정 값에는 45 초 시간 제한이 적용 됩니다. 따라서이 매치 메이 킹 중에 30 초 보다 작거나 측정 시간 제한 사용을 사용 하는 것이 좋습니다. |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

세션에 연결 하 고 게임을 갖도록 하려는 세션 구성원에 대해이 시간 제한 설정 됩니다. 이 일반적으로 셸 제목을 대신 하 여 사용자에 연결 하 고 제목 시작 되는 의미 합니다. 멤버가 세션에서 제거 하 고 기본적으로 3 분 후 비활성 상태에 배치 됩니다.

| 참고                                                          |
|----------------------------------------------------------------------------|
| 이 시간 제한 104/105 계약 버전에 대 한 준비 시간 제한을 지정 됩니다. |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

이 시간 제한, 다른 사용자가 세션에 추가 되었지만 아직 세션에 참가 하지 했습니다 사용자 세션 구성원에 대해 설정 됩니다. 예약을 삭제 하 고 제한 시간이 만료 멤버가 비활성 간주 됩니다. 기본값은 30 초입니다.

| 참고                                                             |
|-------------------------------------------------------------------------------|
| 이 시간 제한 104/105 계약 버전용으로 예약 된 시간 제한을 지정 됩니다. |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

이 제한 시간 후 세션을 삭제 하면 빌 밀리 초 수를 나타냅니다. 기본값은 0입니다.

| 참고                                                                 |
|-----------------------------------------------------------------------------------|
| 이 시간 제한 계약 버전 104/105 sessionEmpty 시간 제한을 지정 됩니다. |


### <a name="session-timeout-example"></a>세션 시간 제한 예제

1.  세션은 네 명의 플레이어를 사용 하 여 시작 됩니다.
2.  A와 B 플레이어 정전이 인해 끊어집니다. 게임 상태가 활성 유지 됩니다.
3.  다른 두 플레이어, C 및 D **MultiplayerSession.Leave 메서드**를 사용 하 여 제대로 종료 합니다.
4.  세션 남아 열기, A와 B 분리 되어 플레이어와 함께 하지만 여전히 활성 상태입니다.
5.  몇 가지 일 후, 플레이어 A 반환 하 고 게임을 시작 합니다.
6.  A의 플레이어 게임은 A를 세션에 대 한 확인 (읽기를 수행) 며칠 전에서 분리 된 세션을 찾습니다.
7.  세션 (A 및 B) 세션에 여전히 두 플레이어에 대 한 상태 검사를 수행 합니다.
    1.  플레이어는 제목, 실행 중 이므로 플레이어 A에 대해 존재 여부 확인에 성공 하면 및 플레이어의 활성 상태 일치에 동일 하 게 유지 합니다.
    2.  플레이어 B 제목을 실행 되지 않습니다. 따라서 플레이어 B 실패 하 고 서비스 상태 확인 비활성으로 플레이어 B 상태를 설정합니다. 플레이어 b 비활성 시간 제한 시작이 시점에서

8.  플레이어는 제대로 **유지** 메서드를 사용 하 여 세션을 종료 합니다.
9.  비활성 시간 제한 사용자가 다음 읽기 또는 쓰기에 세션에서 제거 되는 플레이어가 B에 대 한 만료 됩니다.
10. 이제 세션 0 멤버가 하 고 서비스에서 제거 합니다.

예제에서는 세션에 대 한 비활성 시간 제한 설정 되어 있는지 확인 한 후 즉시 0, 플레이어 B 시간이 초과 7a 단계에서 하 고 아마도 세션 쓰기에서 제거 됩니다. 이 경우 추가로 필요 없이 세션 종료 읽기 또는 세션에 작성 합니다.


## <a name="multiple-signed-in-users-on-a-single-console"></a>단일 콘솔에서 여러 로그인 사용자


여러 사용자가 동일한 콘솔에서 로그인 일부 사용자 세션에서 다른 사용자에 게 표시 되지 않는 반면 게임 세션에 있을 수 또는 현재 타이틀에서 활성화 되지 않습니다. 게임 초대 받은 고 게임 세션 구성원에 영향을 미치고, 여러 사용자를 위해서도 수 있습니다. 타이틀 모든 세션 멤버십 시나리오를 올바르게 처리할 수 있도록 다음이 사항을 고려 되어야 합니다.

일반적인 시나리오에서는 새 플레이어 로그인 활성화 게임에서 되 고 기존 게임 세션에 추가 해야 합니다. 새 게임 세션을 만들고, 타이틀 되었을 때 적절 한 게임 플레이 하는 동안 사용자를 추가할만 해야 합니다.

여러 로그인 사용자가 하나 이상의 사용자는 다른 게임 세션에 초대 받을 수도 있습니다. 타이틀 특정 어떤 방식으로든에서 이러한 시나리오를 처리할 필요가 없습니다. 세션 상태 및 멤버 이벤트 게임 세션 및 사용자 구성원 업데이트의 제목을 게 알립니다.

온라인 세션에 대 한 여러 사용자 로그인을 처리 하려면 제목 어깨 임 별도 **XboxLiveContext 클래스** 개체를 사용 하 여 각 사용자에 대 한 모든 사용자를 등록 합니다. 제목 **MultiplayerSession.ChangeNumber 속성** 을 사용 하 여 세션에서 특정 변경 내용을 확인 하 고 중복 어깨 누르기를 무시 합니다.


## <a name="process-lifecycle-management"></a>프로세스 수명 주기 관리


멀티 플레이어 비 제목에서와 마찬가지로 멀티 플레이 세션에서 제목 제목 일시 중단 및 종료 시 프로세스 수명 주기 이벤트를 발생할 수 있습니다. 따라서 세션 중재자 세션 상태를 정기적으로 저장 해야 합니다. 중재자 일시 중단 되는 경우 제목 중재자 마이그레이션 시도 하 고 새 중재자 세션 상태를 복원할 수 있도록 적절 하 게 게임 상태를 저장 해야 합니다. 세션 MPSD에서 여전히 유효으로 일시 중단 하 고 나중에 다시 시작에 대 한 전체 멀티 플레이 세션에 대 한 다음 가능 합니다. 지정 된 피어 하나의 게임 호스트 일반적으로 전역 게임 상태를 업데이트 해야 합니다.


### <a name="storage-of-game-metadata"></a>게임 메타 데이터의 저장소

타이틀은 MPSD 세션에서 게임 메타 데이터를 저장합니다. 게임 메타 데이터는 세션 데이터를 표시 하 고 제목를 찾아 게임 세션에 참가 하는 데 필요한 정보입니다. 제목 세션 멤버, 예를 들어, 플레이어 색 등 세션에 대 한 기본 플레이어 무기에 대 한 사용자 지정 속성 섹션에서 플레이어 관련 메타 데이터를 저장합니다. 예를 들어, 현재 지도 세션 전체 메타 데이터 MPSD 세션의 글로벌 사용자 지정 속성 섹션에 저장 됩니다.


### <a name="storage-of-game-state"></a>게임 상태 저장

게임 상태 저장소 제목 관리 (TMS), **타이틀 저장소 서비스**를 사용 하 여에 저장 됩니다. 이 위치를 사용 하 여 저장소 권한 문제 없이 중재자 마이그레이션 제목을 수 있습니다. [에 중재자 마이그레이션](migrating-an-arbiter.md)참조 하세요.

| 참고                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| 일시 중단 되는 경우가 아니면 5 분 마다 한 번씩, 보다 더 자주 TMS에 게임 상태를 저장 하려면 제목 하지 않아야 합니다. |

## <a name="cleanup-of-inactive-sessions"></a>비활성 세션 정리

SessionEmptyTimeout는 0으로 설정 되 면 마지막 플레이어 세션을 벗어나면 MPSD 세션은 자동으로 삭제 됩니다. 참조를 사용 하지 않는 세션 크래시 또는 연결 해제 한 후 플레이어를 포함 하는 것을 방지 하는 방법을 알아보려면 [MPSD 변경 알림을 처리 하 고 연결 끊기 감지.](multiplayer-session-directory.md)합니다. 충돌 후 사용 하지 않는 세션의 잘못 된 처리 또는 연결 끊기 타이틀은 플레이어 세션 쿼리할 때 문제가 발생할 수 있습니다.

비활성 세션을 정리 하려는 **MultiplayerService.GetSessionsAsync 메서드** 를 호출 하 고 다음 세션을 평가 하 여 특정 사용자에 대 한 모든 세션 제목 쿼리를가 하는 것이 좋습니다. 오래 된 세션을 만날 제목은 **MultiplayerSession.Leave 메서드** 세션에서 로컬 모든 플레이어에 대 한 호출 합니다. 이 호출을 0으로 구성원 횟수를 결국 놓고 세션 정리 됩니다.

## <a name="session-arbiter"></a>세션 중재자


멀티 플레이어는 몇 가지 메서드는 게임 세션 내에서 하나의 클라이언트만 호출 해야 합니다. 이 클라이언트는 중재자 또는 호스트 세션에 참여 하 고 콘솔 중 하나입니다. 게임에서 하나 이상의 세션 멤버를 세션에 중재자 조인 진행에서을 모니터링할 수 있어야 합니다.


### <a name="setting-the-arbiter"></a>중재자 설정

세션을 만들 때 클라이언트는 하나의 콘솔 중재자로 지정 합니다. 참조 [하는 방법:에 중재자 MPSD 세션에 대 한 설정](multiplayer-how-tos.md).


### <a name="saving-session-state"></a>세션 상태를 저장합니다.

**프로세스 수명 주기 관리**에서 설명 했 듯이 중재자 세션 상태를 정기적으로 저장 해야 합니다. 새 중재자 제목 여 중재자 마이그레이션의 경우 세션 상태를 복원할 수 있어야 합니다. 자세한 내용은 [중재자 마이그레이션](multiplayer-how-tos.md)을 참조 하세요.


### <a name="managing-game-session-members-and-joins-in-progress"></a>게임 세션 멤버, 진행 중에서 조인 관리

세션 중재자의 가장 중요 한 역할을 재생 하려면 게임 세션에 사용자에 게 관리 하는 것입니다. 게임 초대를 처리 하 고 대기 플레이어에 게 알리는 게임을 종료 하는 플레이어 작업 포함 됩니다.


#### <a name="receiving-notifications"></a>알림 받기

중재자는 **RealTimeActivityService.MultiplayerSessionChanged 이벤트**를 사용 하 여 게임 세션에 참가 하려면 새 플레이어에 대 한 수신 대기 해야 합니다.


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>플레이어의 빈 게임 세션 슬롯 채우기 찾기

중재자 발견 플레이어의 이러한 작업 중 하나에 의해 빈 게임 세션 슬롯 채우기: 타이틀 로비 세션이 나 다른 메커니즘을 사용 하 여 지연 된 조인을 하는 경우 해당 메커니즘을 사용 하 여 새 세션 멤버 찾기.
-   다른 일치 티켓 세션을 만듭니다.

참고 [하는 방법: 매치 메이 킹 하는 동안 열려 세션 슬롯 채우기](multiplayer-how-tos.md).


#### <a name="handling-invited-session-members"></a>처리 세션 구성원 초대

중재자 초대 받은 세션 멤버를 모니터링 하 고 최소 간격 단일 사용자에 게 초대를 적용 해야 합니다. 참고 [하는 방법: 게임 초대 보내기](multiplayer-how-tos.md).
