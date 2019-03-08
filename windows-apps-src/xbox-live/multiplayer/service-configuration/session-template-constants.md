---
title: 세션 템플릿 상수
description: Xbox Live 멀티 플레이 세션 템플릿에 정의 된 시스템 상수를 설명 합니다.
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one, 멀티 플레이 세션 템플릿
ms.localizationpriority: medium
ms.openlocfilehash: 5bb4c6f39bab8779b5675a7b9c81b883965676c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646908"
---
# <a name="session-template-constants"></a>세션 템플릿 상수

다음 표에서 107 세션 템플릿 버전을 사용 하 여 멀티 플레이 세션 템플릿의 미리 정의 된 요소를 설명 합니다.

## <a name="system"></a>시스템

시스템 상수  | 설명 | 유효한 값 | 기본값
--|-- | -- | --
version | 세션 템플릿의 버전입니다. | 1-n | 없음
maxMembersCount | 멀티 플레이 작업에 대해 지원 되는 총 세션 멤버 슬롯의 수입니다. | 1-100 일반 101 + 큰 세션에 대 한 세션에서는 | 100
visibility | 다른 사용자에 게 참조 하거나 세션에 참가할 수 하는 경우를 나타내는 세션의 표시 여부 상태입니다. | 개인, 표시, 열기 | 열기
inviteProtocol | 이 상수 "game"를 설정 합니다. 초대를를 세션에 초대 하는 경우 알림 메시지를 받을 수 있습니다. | tournamentgame, 게임, 채팅, gameparty | 없음
reservedRemovalTimeout  | 제한 시간을 멤버 예약 시간 (밀리초)입니다. 값이 0 즉시 제한 시간을 나타냅니다. 제한 시간 null 인 경우 무한 간주 됩니다. | 0-n, null | 30000
inactiveRemovalTimeout  | 밀리초 단위로 비활성으로 간주 하는 멤버에 대 한 제한 시간입니다. 값이 0 즉시 제한 시간을 나타냅니다. 제한 시간 null 인 경우 무한 간주 됩니다. | 0-n, null | 0
readyRemovalTimeout | 준비 시간 (밀리초)로 간주 하는 멤버에 대 한 제한 시간입니다. 값이 0 즉시 제한 시간을 나타냅니다. 제한 시간 null 인 경우 무한 간주 됩니다. | 0-n, null | 180000
sessionEmptyTimeout | 시간 (밀리초)에는 빈 세션에 대 한 제한 시간입니다. 값이 0 즉시 제한 시간을 나타냅니다. 제한 시간 null 인 경우 무한 간주 됩니다. | 0-n, null | 0
[**capabilities**](#capabilities) | 세션의 기능을 지정 합니다. 아래 기능 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**metrics**](#metrics) | 멤버는 세션에서 충족 해야 하는 대기 시간 및 대역폭 속도 등의 서비스 요구 사항 정의 하는 제목 품질의 집합을 지정 합니다.  | 해당 없음 | 해당 없음
[**memberInitialization**](#memberInitialization) | 새 멤버는 세션에 참가 하는 경우 적용 되는 시간 제한 및 초기화 요구 사항을 지정 합니다. 멤버 초기화 섹션 아래를 참조 하세요. | 해당 없음 | 해당 없음
[**peerToPeerRequirements**](#peerToPeerRequirements) | 피어 투 피어 메시 연결에 대 한 서비스 요구 사항의 네트워크 품질을 지정합니다. 피어 투 피어 요구 사항 섹션 아래를 참조 하세요. |해당 없음 | 해당 없음
[**peerToHostRequirements**](#peerToHostRequirements) | 피어 호스트 연결에 대 한 서비스 요구 사항의 네트워크 품질을 지정합니다. 피어 호스트 요구 사항 섹션 아래를 참조 하세요. | 해당 없음 | 해당 없음
[**measurementServerAddresses**](#measurementserveraddresses) | QoS 측정을 결정 하는 데 사용 되는 잠재적인 데이터 센터의 컬렉션을 지정 합니다. 아래 measurementServerAddresses 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**cloudComputePackage**](#cloudComputePackage) | ? | 해당 없음 | 해당 없음
[**arbitration**](#arbitration) | 토너먼트에서 중재 결과 제출 하는 멤버에 대 한 제한 시간을 지정 합니다. 아래 cloudComputePackage 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | 제목 항상 세션에 대 한 읽기 액세스 해야 하는 Id의 목록을 지정 합니다. 아래 broadcastViewerTitleIds 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**ownershipPolicies**](#ownershipPolicies) | 세션 소유권와 관련 된 정책을 지정 합니다. 아래 OwnershipPolicies 섹션을 참조 하세요. | 해당 없음 | 해당 없음


## <a name="capabilities"></a>capabilities
기능은 세션 템플릿에서 필요에 따라 설정 된 부울 값입니다. 기능이 없으며, 필요한 경우 제목 동적 세션 기능이 필요한 경우가 아니면 세션 생성 시 지정에서 기능을 방지 하기 위해 빈 'capabilities' 개체 서식 파일에 있어야 합니다.

기능 |  description | 유효한 값 | 기본값
-- | -- | -- | -- |
방해될 수 있음 | 세션 피어 연결을 지 원하는 경우를 나타냅니다. 이 값이 false 이면 세션 모든 메트릭을 사용 하도록 설정할 수 없습니다는 다음 및 세션 멤버는 해당 SecureDeviceAddress를 설정할 수 없습니다. 많은 세션을 설정할 수 없습니다. | true, false | false
suppressPresenceActivityCheck | True 이면 현재 상태 검사를 해제 합니다. | true, false | false
게임 플레이 | 세션 설정/메뉴 시간 로비 또는 매 치메이 킹 같은 것이 아니라 실제 게임 플레이 나타내는지 여부를 나타냅니다. True 인 경우 다음 세션 게임 모드입니다. | true, false | false
큼 | 세션 큰 세션 (100 개가 넘는 멤버) 인지 여부를 나타냅니다. 멀티 플레이 manager 사용에 대 한 많은 세션을 사용할 수 없습니다. | true, false | false
connectionRequiredForActiveMembers | 활성화 될 멤버에 대 한 연결이 순서에 필요한 경우를 나타냅니다. | true, false | false
cloudCompute | 클라이언트가 세션을 대신 하 여 클라우드 계산 인스턴스를 할당할 수 있다는 요청할 수 있습니다. | true, false | false
autoPopulateServerCandidates | 자동으로 계산 하 고 'serverMeasurements'에서 'serverConnectionStringCandidates'를 설정 합니다. 이 기능은 많은 세션을 설정할 수 없습니다. | true, false | false
userAuthorizationStyle | 세션에서 강력한 제목 id 없이 플랫폼 호출을 지원 하는지 나타냅니다. 이 기능은 많은 세션을 설정할 수 없습니다.</br></br>설정 합니다 `userAuthorizationStyle` 하는 기능 `true` 기본값은 `readRestriction` 및 `joinRestriction`세션의 `local` 대신 `none`합니다. 즉, 제목 검색 핸들을 사용 하거나 게임 세션에 참가 대 한 핸들을 전송 해야 합니다.| true, false | false
crossplay | 세션 PC 및 Xbox One 장치 간에 교차 게임을 지원함을 나타냅니다. | true, false | false
브로드캐스트 | 세션 브로드캐스트를 나타냅니다. 세션의 이름을 브로드캐스터의 xuid 이어야 합니다. "대형" 기능이 필요합니다. | true, false | false
팀 | 세션 토너먼트 팀을 나타낸다는 것을 나타냅니다. 이 기능은 ' 또는 '게임 플레이' 세션을 설정할 수 없습니다. | true, false | false
중재 | '중재' 서버 항목을 추가 하는 서비스 사용자가 세션을 만들어야 함을 나타냅니다. ' 세션을 설정할 수 없지만 '게임 플레이' 필요 합니다. | true, false | false
hasOwners | 세션 소유자 특정 멤버를 기반으로 보안 정책을 나타냅니다. | true, false | false
검색 가능 | 세션 대상 세션 검색 핸들을 수 있음을 나타냅니다. 'UserAuthorizationStyle' 기능을 설정 하는 경우 다음 '검색' 기능을 설정할 수 없습니다 'hasOwners' 기능을 설정 하지 않은 경우. | true, false | false

예제:

```json
"capabilities": {
    "connectivity": true,  
    "suppressPresenceActivityCheck": true,
    "gameplay": true,
    "large": true,
    "connectionRequiredForActiveMembers": true,
    "cloudCompute": true,
    "autoPopulateServerCandidates": true,
    "userAuthorizationStyle": true,
    "crossPlay": true,  
    "broadcast": true,  
    "team": true,   
    "arbitration": true,   
    "hasOwners": true,   
    "searchable": true  
},
```

## <a name="metrics"></a>메트릭
경우는 `metrics` 속성 지정 되지 않은는 서비스 품질 요구 사항 충족 하는 데 필요한 값이 기본값이 됩니다.  
지정 된 경우 값은 서비스 품질 요구 사항 충족 하기에 충분 있어야 합니다.
이 요소는 유효한 세션에 있는 경우에는 `connectivity` 기능 집합입니다.

메트릭 | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
latency | | true, false | 설명을 참조 하세요.
bandwidthDown | | true, false | 설명을 참조 하세요.
bandwidthUp | | true, false | 설명을 참조 하세요.
사용자 지정 | | true, false | 설명을 참조 하세요.

예제:
```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true
},
```

## <a name="memberinitialization"></a>memberInitialization
경우는 `memberInitialization` 개체를 설정, 클라이언트 시스템 또는 세션을 만든 다음 초기화를 수행 및/또는 새 멤버로 세션에 참가 하려면 제목 세션이 필요 합니다.  
시간 제한 및 초기화 단계에서 메트릭을 설정 된 경우 QoS 측정을 포함 하 여 세션에서 자동으로 추적 됩니다.  
'InitializationEpisode'에 있는 멤버에 대 한 세션의 예약 및 준비 시간 초과 재정의 하는 시간이 제한을 설정 합니다.  
많은 세션을 지정할 수 없습니다.

요소  | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
joinTimeout | 멤버는 세션에 참가 하는 시간 (밀리초)의 수를 나타냅니다. 조인에 실패 하는 사용자의 예약 제거 됩니다.</br>**참고:** 기본 기간은 일반 제목 실행 충분히 이지만 제목을 MPSD 흐름 중 디버깅 중인 경우 시간 초과 조인 발생할 수 있습니다. 이러한 시나리오에 대 한 재정의 하 고 세션에 대해이 기본값을 늘립니다.| 0-n | 10000
measurementTimeout | 세션 멤버인 측정을 업로드 하는 시간 (밀리초)의 수를 나타냅니다. 측정 업로드에 실패 한 멤버 "timeout"의 실패 이유와 함께 표시 됩니다.  | 0-n | 30000
evaluationTimeout | 외부 평가 측정을 업로드 하는 시간 (밀리초)의 수를 나타냅니다. | 0 -n | 5000
externalEvaluation | True 인 경우에 제목 코드는 조인을 측정값을 기반으로 QoS의 평가 수행 함을 나타냅니다. 멀티 플레이 서비스는 모든 QoS 논리 수행 되지 않으며 제목은 초기화 단계를 진행 해야 합니다. 제목은 일반적으로 이렇게 할 필요가 없습니다. | true, false | false
membersNeededToStart | 초기화 에피소드 0만에 대 한 세션을 시작 하는 데 필요한 멤버 수입니다. | 1 - maxMembersCount | 1

예제:
```json
"memberInitialization": {
    "joinTimeout": 10000,  
    "measurementTimeout": 30000,  
    "evaluationTimeout": 5000,
    "externalEvaluation": false,
    "membersNeededToStart": 1
},
```


## <a name="peertopeerrequirements"></a>peerToPeerRequirements

피어 투 피어 네트워크 요구 사항 | 설명 | 기본값
-- | -- |--
latencyMaximum | 최대 대기 시간, 모든 두 클라이언트 간의 밀리초입니다. | 250
bandwidthMinimum | 두 클라이언트 간의 초당 킬로 비트의에서 최소 대역폭입니다. | 10000

예제:
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

호스트 네트워크 요구 사항에 피어 링 | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
latencyMaximum | 최대 대기 시간 (밀리초)는 피어 호스트 연결에 대 한 합니다. | | 250
bandwidthDownMinimum | 피어 호스트에서 전송 되는 정보에 대 한 초당 킬로 비트의에서 최소 대역폭입니다. | | 100000
bandwidthUpMinimum | 호스트 피어에서 보내는 정보에 대 한 초당 킬로 비트의에서 최소 대역폭입니다. | | 1000
hostSelectionMetric | 호스트를 선택 하려면 메트릭이 사용 됨을 나타냅니다. | bandwidthup, bandwidthdown, 대역폭 및 대기 시간 | latency

예제:
```json
"peerToHostRequirements": {
    "latencyMaximum": 250,
    "bandwidthDownMinimum": 100000,
    "bandwidthUpMinimum": 1000,  
    "hostSelectionMetric": "bandwidthup"
},
```

## <a name="measurementserveraddresses"></a>measurementServerAddresses
집합 평가 되어야 하는 잠재적인 server 연결 문자열입니다. 연결 문자열을 소문자로 변환 해야 합니다.
많은 세션을 지정할 수 없습니다.

연결 문자열은 다음 형식으로 정의 됩니다.

`"<server name>" : {deviceAddress}`

여기서 장치 주소는 다음과 같이 설명 합니다.

서버 연결 문자열 | 설명
-- | --
secureDeviceAddress | Base-64 인코딩된 서버의 안전한 장치 주소

예제:
```json
"measurementServerAddresses": {
    "server farm a": {
        "secureDeviceAddress": "r5Y="
    },
    "datacenter b": {
        "secureDeviceAddress": "rwY="
    }
},
```

## <a name="cloudcomputepackage"></a>cloudComputePackage
클라우드 속성 계산 할당 하는 패키지를 지정 합니다. 에서는 `cloudCompute` 기능이 설정 됩니다.

클라우드 계산 속성 | 설명
-- | -- | -- | --
titleId | 클라우드 ID 계산 할당할 패키지 제목을 나타냅니다.
gsiSet | 할당할 클라우드 계산 패키지의 GSI 집합을 나타냅니다.
Variant | 할당할 클라우드 계산 패키지의 변형을 나타냅니다.

예제:
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>중재
중재 프로세스에 대 한 제한 시간을 지정합니다. 에서는 `arbitration` 기능이 설정 됩니다. 중재 시작 시간에서 세션에 정의 되어는 */servers/arbitration/constants/system/startTime* 요소입니다.

제한 시간 | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
forfeitTimeout | 중재 시작 시간에서 시간 (밀리초)의 시간을 나타냅니다.는 tbd입니다 | 0-n | 60000
arbitrationTimeout | 중재 시작 시간부터 중재 결과 시간이 초과 되는 밀리초 단위로 시간을 나타냅니다. 이 값 보다 작은 `forfeitTimeout` 값 | 0-n | 300000

예제:
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

제목의 항상 브로드캐스트 세션에 대 한 읽기 액세스 해야 하는 제목의 Id 배열을 지정 합니다.

예제:
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
마지막 소유자 세션을 유지 하는 경우 세션을 처리 하는 방법을 지정 합니다. 에서는 `hasOwners` 기능이 설정 됩니다.

소유권 정책 | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
마이그레이션 | 마지막 소유자가 세션을 떠날 때 발생 하는 동작을 나타냅니다. 마이그레이션 정책 "endsession"으로 설정 된 경우 만료 된 세션입니다. "가장 오래 된" 마이그레이션 정책을 설정 하는 경우 가장 오래 된 조인에 사용 하 여 세션의 새 소유자가 될 멤버를 선택 합니다. | "가장 오래 된", "endsession" | "endsession"

예제:
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```
