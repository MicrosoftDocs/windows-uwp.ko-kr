---
title: 세션 템플릿 상수
author: KevinAsgari
description: Xbox Live 멀티 플레이 세션 템플릿에 정의 된 시스템 상수를 설명 합니다.
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one 멀티 플레이어 세션 템플릿
ms.localizationpriority: medium
ms.openlocfilehash: 17c7fa48ee637dbca20b67872113a8d43e04da84
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5867346"
---
# <a name="session-template-constants"></a>세션 템플릿 상수

다음 표에서 107 세션 템플릿 버전을 사용 하 여 멀티 플레이 세션 템플릿의 미리 정의 된 요소를 설명 합니다.

## <a name="system"></a>시스템

시스템 상수  | 설명 | 유효한 값 | 기본값
--|-- | -- | --
버전 | 세션 템플릿의 버전입니다. | 1-n | none
maxMembersCount | 멀티 플레이 활동에 대 한 지원 되는 총 세션 멤버 슬롯의 수입니다. | 1-일반 101 + 큰 세션에 대 한 세션에서는 100 | 100
visibility | 다른 사용자에 게 표시 하거나 세션에 참가할 수 있습니다 경우를 나타내는 세션의 표시 상태. | 개인, 표시, 열기 | 열기
inviteProtocol | "게임"이이 상수를 설정 하면 초대 받은 세션에 초대 하는 경우 알림 메시지를 받을 수 있습니다. | 게임, tournamentgame, 채팅, gameparty | none
reservedRemovalTimeout  | 밀리초에서 멤버 예약에 대 한 시간 제한 합니다. 값이 0 즉시 적용할 제한 시간을 나타냅니다. 시간 제한 null 이면 무한 간주 됩니다. | 0-n null | 30000
inactiveRemovalTimeout  | 비활성 시간 (밀리초)에서 고려해 야 할 구성원에 대 한 시간 제한 합니다. 값이 0 즉시 적용할 제한 시간을 나타냅니다. 시간 제한 null 이면 무한 간주 됩니다. | 0-n null | 0
readyRemovalTimeout | 밀리초에서 준비 고려해 야 할 구성원에 대 한 시간 제한 합니다. 값이 0 즉시 적용할 제한 시간을 나타냅니다. 시간 제한 null 이면 무한 간주 됩니다. | 0-n null | 180000
sessionEmptyTimeout | 밀리초에서는 빈 세션에 대 한 시간 제한 합니다. 값이 0 즉시 적용할 제한 시간을 나타냅니다. 시간 제한 null 이면 무한 간주 됩니다. | 0-n null | 0
[**capabilities**](#capabilities) | 세션의 기능을 지정 합니다. 아래 접근 권한 값 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**메트릭**](#metrics) | 세션의 멤버를 충족 해야 하는 대기 시간 및 대역폭 속도 같은 서비스 요구 사항을 정의 제목 품질의 집합을 지정 합니다.  | 해당 없음 | 해당 없음
[**memberInitialization**](#memberInitialization) | 새 멤버 세션에 참가할 때 적용 되는 시간 제한 및 초기화 요구를 지정 합니다. 아래 멤버 초기화 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**peerToPeerRequirements**](#peerToPeerRequirements) | 피어 투 피어 메시 연결에 대 한 서비스 요구 사항 중 네트워크 품질을 지정합니다. 피어 투 피어 요구 사항 섹션 아래를 참조 하세요. |해당 없음 | 해당 없음
[**peerToHostRequirements**](#peerToHostRequirements) | 호스트 연결에 대 한 피어에 대 한 서비스 요구 사항 중 네트워크 품질을 지정합니다. 피어 호스트 요구 사항 섹션 아래를 참조 하세요. | 해당 없음 | 해당 없음
[**measurementServerAddresses**](#measurementserveraddresses) | QoS 측정 값을 결정 하는 데 사용 되는 잠재적인 데이터 센터의 컬렉션을 지정 합니다. 아래 measurementServerAddresses 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**cloudComputePackage**](#cloudComputePackage) | ? | 해당 없음 | 해당 없음
[**중재**](#arbitration) | 토너먼트에서 중재 결과 제출 하는 멤버에 대 한 시간 제한을 지정 합니다. 아래 cloudComputePackage 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | 제목 세션에 대 한 읽기 항상 해야 하는 Id 목록을 지정 합니다. 아래 broadcastViewerTitleIds 섹션을 참조 하세요. | 해당 없음 | 해당 없음
[**ownershipPolicies**](#ownershipPolicies) | 세션 소유권 관련 정책을 지정 합니다. 아래 OwnershipPolicies 섹션을 참조 하세요. | 해당 없음 | 해당 없음


## <a name="capabilities"></a>capabilities
필요에 따라 세션 템플릿에서 설정 된 부울 값은 기능입니다. 기능은 필요한 경우에 제목에서 동적 세션 기능을 원하는 암호 복잡도 하지 않는 한 세션 만들기에 지정 되 고에서 기능을 방지 하기 위해 빈 '기능' 개체 템플릿에서 이어야 합니다.

접근 권한 값 |  description | 유효한 값 | 기본값
-- | -- | -- | -- |
연결 | 세션 피어 연결을 지원 하는지 나타냅니다. 이 값이 false 이면 다음 세션 모든 메트릭을 활성화할 수 없습니다 및 세션 멤버는 해당 SecureDeviceAddress 설정할 수 없습니다. 큰 세션에서 설정할 수 없습니다. | true 인 false | false
suppressPresenceActivityCheck | True 인 경우 현재 상태 검사를 끕니다. | true 인 false | false
게임 플레이 | 세션이 설정/메뉴 시간 로비 또는 매치 메이 킹 같은 것이 아니라 실제 게임 플레이 나타내는지 여부를 나타냅니다. True 인 경우 게임 플레이 모드로 세션 되어 있습니다. | true 인 false | false
큼 | 세션 큰 세션 (100 개 이상의 구성원) 인지 여부를 나타냅니다. 큰 세션 멀티 플레이어 관리자에 사용할 수 없습니다. | true 인 false | false
connectionRequiredForActiveMembers | 멤버를 활성화할 수에 대 한 연결이 순서로 필요한 경우 나타냅니다. | true 인 false | false
cloudCompute | 클라이언트가 세션을 대신 하 여 클라우드 컴퓨팅 인스턴스에 할당을 요청할 수 있습니다. | true 인 false | false
autoPopulateServerCandidates | 자동으로 계산 하 고 'serverMeasurements'에서 'serverConnectionStringCandidates'를 설정 합니다. 큰 세션에서이 접근 권한이 값을 설정할 수 없습니다. | true 인 false | false
userAuthorizationStyle | 세션의 강력한 제목 identity 없이 플랫폼 호출을 지원 하는지 나타냅니다. 큰 세션에서이 접근 권한이 값을 설정할 수 없습니다.</br></br>설정의 `userAuthorizationStyle` 접근 권한 값을 `true` 기본값은 `readRestriction` 및 `joinRestriction`세션의 `local` 대신 `none`. 즉, 제목 검색 핸들을 사용 하거나 게임 세션에 대 한 핸들을 전송 해야 합니다.| true 인 false | false
crossplay | 세션 PC 및 Xbox One 장치 간에 교차 플레이 지원 하는지 나타냅니다. | true 인 false | false
브로드캐스트 | 세션 브로드캐스트를 나타냅니다. 세션의 이름에는 방송국의 xuid 이어야 합니다. "큰" 기능에 필요합니다. | true 인 false | false
팀 | 세션 토너먼트 팀을 나타냅니다. '큰' 또는 '게임 플레이' 세션에서이 접근 권한이 값을 설정할 수 없습니다. | true 인 false | false
중재 | '중재' 서버 항목을 추가 하는 서비스 사용자가 세션을 만들어야 함을 나타냅니다. '게임 플레이' 필요 하지만 '큰' 세션을 설정할 수 없습니다. | true 인 false | false
hasOwners | 세션 소유자 되 고 특정 멤버에 따라 보안 정책에 있음을 나타냅니다. | true 인 false | false
검색 가능한 | 세션 검색 핸들의 대상 세션 수 있음을 나타냅니다. 'UserAuthorizationStyle' 접근 권한 값을 설정 하는 경우 다음 '검색 가능한' 접근 권한 값 설정할 수 없습니다 'hasOwners' 접근 권한 값이 설정 되지 않은 경우. | true 인 false | false

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
하는 경우는 `metrics` 속성을 지정 하지 않으면, 기본적으로 품질 서비스 요구 사항 충족 하기 위해 필요한 값입니다.  
지정 된 경우 값 품질 서비스 요구 사항 충족 하기 위해 충분 한 이어야 합니다.
이 요소는만 유효 세션에는 `connectivity` 접근 권한 값을 설정 합니다.

미터법 | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
대기 시간 | | true 인 false | 설명을 참조 하세요.
bandwidthDown | | true 인 false | 설명을 참조 하세요.
bandwidthUp | | true 인 false | 설명을 참조 하세요.
사용자 지정 | | true 인 false | 설명을 참조 하세요.

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
경우에 `memberInitialization` 개체에서 설정 되 면 세션에 클라이언트 시스템 또는 세션 만들기를 수행 하는 초기화를 수행 및/또는 새 멤버로 세션에 참가할 제목 것으로 예상 합니다.  
시간 제한 하는 초기화 단계는 모든 메트릭을 설정 된 경우 QoS 측정 값을 포함 한 세션에서 자동으로 추적 됩니다.  
'InitializationEpisode' 구성원에 대 한 세션의 예약 및 준비 시간 초과 재정의 하는 이러한 시간 제한을 설정 합니다.  
큰 세션에 지정할 수 없습니다.

요소  | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
joinTimeout | 구성원에서 세션에 참가 하는 시간 (밀리초) 수를 나타냅니다. 가입에 실패 하는 사용자의 예약 제거 됩니다.</br>**참고:** 기본 기간은 일반 제목 실행을 위해 충분 한 이지만 제목을 MPSD 흐름 하는 동안 디버깅 하는 경우 시간 제한에 가입 시킬 수 있습니다. 이러한 시나리오에 대 한 재정의 하 고 세션에 대 한이 기본값을 증가 합니다.| 0-n | 10000
measurementTimeout | 세션 구성원에서 측정을 업로드 하는 시간 (밀리초) 수를 나타냅니다. 구성원 측정 업로드 하지 않는 한 "제한" 오류 원인을 표시 됩니다.  | 0-n | 30000
evaluationTimeout | 외부 평가 측정을 업로드 하는 시간 (밀리초) 수를 나타냅니다. | 0-n | 5000
externalEvaluation | True 인 경우에 제목 코드 조인을 기반 QoS 측정 값으로는 사용자의 평가 수행 함을 나타냅니다. 멀티 플레이 서비스는 모든 QoS 논리를 수행 하지 않습니다 및 제목은 초기화 단계를 진행 하는 일을 담당 합니다. 제목은은 일반적으로 이렇게 할 필요가 없습니다. | true 인 false | false
membersNeededToStart | 초기화 에피소드 0만에 대 한 세션을 시작 하는 데 필요한 멤버의 수입니다. | 1-maxMembersCount | 1

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
latencyMaximum | 두 하는 클라이언트 간에 밀리초 최대 대기 합니다. | 250
bandwidthMinimum | 두 하는 클라이언트 간에 k b / 초 최소 대역폭. | 10000

예제:
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

피어 투 호스트 네트워크 요구 사항 | 설명 | 유효한 값 | 기본값
-- | -- | -- | --
latencyMaximum | 호스트에 연결 하는 피어에 대 한 밀리초 최대 대기 합니다. | | 250
bandwidthDownMinimum | K b / 초 피어 호스트에서 전송 된 정보에 대 한 최소 대역폭. | | 100000
bandwidthUpMinimum | K b / 초 호스트 피어 로부터 전송 된 정보에 대 한 최소 대역폭. | | 1000
hostSelectionMetric | 호스트를 선택 하는 메트릭을 사용 되는지 나타냅니다. | bandwidthup, bandwidthdown, 대역폭 및 대기 시간 | 대기 시간

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
평가 해야 하는 잠재적인 서버 연결 문자열 집합입니다. 연결 문자열 소문자 여야 합니다.
큰 세션에 지정할 수 없습니다.

연결 문자열은 다음 형식으로 정의 됩니다.

`"<server name>" : {deviceAddress}`

여기서 장치 주소는 다음과 같이 설명 되어 있습니다.

서버 연결 문자열 | 설명
-- | --
secureDeviceAddress | Base 64 인코딩된 서버의 보안 장치 주소

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
패키지를 할당할 수를 계산 하는 클라우드의 속성을 지정 합니다. 있어야 합니다 `cloudCompute` 접근 권한 값을 설정 합니다.

클라우드 컴퓨팅 속성 | 설명
-- | -- | -- | --
titleId | 클라우드 ID를 할당 하는 패키지를 계산 타이틀을 나타냅니다.
gsiSet | 클라우드 컴퓨팅 패키지 할당할 GSI 집합을 나타냅니다.
변형 | 변형 할당에 클라우드 컴퓨팅 패키지의 압축을 나타냅니다.

예제:
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>중재
중재 프로세스에 대 한 시간 제한을 지정합니다. 있어야 합니다 `arbitration` 접근 권한 값을 설정 합니다. 중재 시작 시간은 */servers/arbitration/constants/system/startTime* 요소에서 세션에서 정의 됩니다.

timeout | 설명 | 유효한 값 | default
-- | -- | -- | --
forfeitTimeout | 중재 시작 시간은 밀리초 시간을 나타냅니다 하는 TBD | 0-n | 60000
arbitrationTimeout | 밀리초 중재 시작 시간부터 중재 결과 시간이 초과 하는 시간을 나타냅니다. 이 값 보다 작은 `forfeitTimeout` 값 | 0-n | 300000

예제:
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

배열을 제목 브로드캐스트 세션에 대 한 읽기 항상 해야 하는 타이틀의 Id 지정 합니다.

예제:
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
마지막 소유자 세션을 벗어날 때 세션을 처리 하는 방법을 지정 합니다. 있어야 합니다 `hasOwners` 접근 권한 값을 설정 합니다.

소유권 정책 | 설명 | 유효한 값 | default
-- | -- | -- | --
마이그레이션 | 마지막 소유자가 세션을 종료할 때 발생 하는 동작을 나타냅니다. "Endsession"에 마이그레이션 정책이 설정 된 경우 만료 세션 합니다. "가장 오래 된" 마이그레이션 정책 설정 되 면 세션의 새 소유자가 가장 오래 된 조인 시간을 사용 하 여 구성원을 선택 합니다. | "오래 된", "endsession" | "endsession"

예제:
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```
