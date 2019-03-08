---
title: 멀티 플레이 시스템 소개
description: Xbox Live 멀티 플레이 게임 2015 시스템에 대 한 높은 수준의 소개를 제공합니다.
ms.assetid: d025bd2b-2ca4-4ba9-9394-4950d96ad264
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 멀티 플레이 2015, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 4a739aaa650a7086dfe58b1b8ca170e15b3b2ef0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629418"
---
# <a name="introduction-to-the-multiplayer-system"></a>멀티 플레이 시스템 소개

| 참고                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 이 문서는 고급 API 사용.  시작 지점으로 잠시 살펴보겠습니다 합니다 [멀티 플레이 게임 관리자 API](../multiplayer-manager.md) 개발을 크게 간소화 하는 합니다.  발견 되 면 지원 되지 않는 시나리오에서 멀티 플레이 게임 관리자의 파악에 알려 주시기 바랍니다. |

이 문서에는 다음 섹션이 포함 되어 있습니다.
* 멀티 플레이 시스템에 대 한
* 멀티 플레이 구성 요소, 인터페이스 및 아키텍처
* 2015 멀티 플레이어에서 지원 되는 파티
* 멀티 플레이 용어
* 다중 접속 2015의에서 새로운 기능
* Xbox 360 및 Xbox 하나 MPSD 세션 함수 간의 차이점

## <a name="about-the-multiplayer-system"></a>멀티 플레이 시스템에 대 한


2015 다중 접속 게임 세션과 MPSD를 직접 사용을 최적화합니다. 단일 제목 또는 사용자에 대 한 여러 개의 동시 세션에 대 한 향상 된 지원을 제공 하 고 제목에 사용할 수 있도록 Xbox 서비스 API (XSAPI) 업데이트:

-   사용자의 현재 활동 및 조인에 대 한 가용성을 보급 합니다.
-   사용자가 볼 수 (제목을 지정) 컨텍스트 및 세션에 초대를 보냅니다.
-   검색 및 제목 코드를 통해 세션에 참여 합니다.
-   세션 변경, 예를 들어, 이벤트 및 연결 상태 변경 내용을 변경 하려면 구독을 반영 하는 업데이트에 대 한 간략 한 알림 (자격 증명 입력 탭) 받을 수 있도록 MPSD에 웹 소켓 연결을 유지 합니다. 또한 MPSD 신속 하 게 감지 하 고 클라이언트 연결 끊기 취할 웹 소켓 연결을 사용 합니다.
-   SmartMatch 결혼 정보 회사 연결을 사용 합니다.

## <a name="multiplayer-components-interfaces-and-architectures"></a>멀티 플레이 구성 요소, 인터페이스 및 아키텍처

### <a name="components-of-2015-multiplayer"></a>2015 멀티 플레이 게임의 구성 요소

다중 접속은 여러 구성 요소로 구성 된 시스템. 것 전용된 서버 및 외부 매 치메이 킹 시스템 같은 다른 구성 요소 수 있을 만큼 유연 합니다.
#### <a name="multiplayer-session-directory-mpsd"></a>멀티 플레이 세션 디렉터리(MPSD)


멀티 플레이 세션 디렉터리 (MPSD)는 세션의 컬렉션을 보유 하는 서비스입니다. 세션은 클라우드에 있는 및 게임을 즐기는 사용자 그룹을 나타내는 보안 문서로 정의 됩니다. MPSD 세부 정보를 참조 하세요 [멀티 플레이 게임 세션 디렉터리 (MPSD)](multiplayer-session-directory.md)합니다.


#### <a name="multiplayer-apis"></a>멀티 플레이 Api

2015 멀티 플레이 게임을 통해 구현, 멀티 플레이 게임 WinRT API를 제공 합니다 **Microsoft.Xbox.Services.Multiplayer Namespace**합니다. 해당 구성 요소를 포함 합니다 **MultiplayerService 클래스**, MPSD를 래핑하는 웹 서비스를 정의 합니다.

다중 접속 REST API가도 포함 합니다. WinRT API 메서드에 의해 호출 되는 Uri 및 JSON 개체를 정의 합니다. REST 기능에서 제목에 대 한 직접 호출에 사용할 수 있지만 MPSD WinRT API를 통해 간접적으로 액세스 하는 것이 좋습니다. 자세한 내용은 **MPSD 호출**합니다.

#### <a name="xbox-party-system"></a>Xbox 파티 시스템

2015 멀티 플레이 게임을 Xbox 파티 시스템 외부 엔터티로 채팅 파티만을 지원합니다. 제목 채팅 파티의 멤버 자격을 검색할 파티 시스템과 상호 작용할 수 있습니다. 자세한 내용은 **2015 멀티 플레이어에서 지원 되는 파티**합니다.

타사 시스템에는 이제 게임 게임 파티를 사용 하는 대신 MPSD 세션을 통해 직접 지원 합니다. 그가 제목을 멤버 상호 작용, 공간을 사용할 수 있게 되 면 플레이어의 게임을 끌어오기 및 대기 기간 동안 사용자 참여와 같은 작업을 사용 하도록 설정 하려면 세션을 사용 합니다.


### <a name="2015-multiplayer-interfaces"></a>2015 멀티 플레이 인터페이스

2015 다중 접속 다른 Xbox 구성 요소에 여러 인터페이스를 사용합니다.
#### <a name="xbox-secure-sockets"></a>보안 소켓, Xbox

2015 다중 접속 보안 소켓, Xbox 및 Winsock를 사용 하 여 장치 간 하위 수준의 네트워크 통신을 지원 합니다. 네트워킹 기능 보안 장치 연결을 제공 하는 타이틀에 인터넷 프로토콜 보안 (IPSec)를 사용 합니다. 참조 **네트워킹 개요** Xbox의 세부 정보에 대 한 secure sockets 기능입니다.


#### <a name="xbox-live-real-time-activity-service"></a>Xbox Live의 실시간 활동 서비스

2015 멀티 플레이어를 사용 합니다 **의 실시간 활동 서비스** 구독 MPSD 세션 변경 하 고 클라이언트의 자동 검색을 사용 하는 제목에 있도록의 연결을 끊습니다. 자세한 정보에 제공 됩니다 [활동 RTA (실시간) 서비스](../../real-time-activity-service/real-time-activity-service.md)합니다.


#### <a name="xbox-live-matchmaking-service"></a>Xbox Live 결혼 정보 회사 연결 서비스

합니다 **결혼 정보 회사 서비스** 해당 기본 설정 및 데이터를 SmartMatch 결혼 정보 회사 연결 하는 동안 제공 되는 정보에 따라 플레이어에서 그룹을 형성 합니다. 멀티 플레이 게임의 세부 정보를이 서비스의 사용에 대 한 참조 [SmartMatch 매 치메이 킹](smartmatch-matchmaking.md)합니다.


#### <a name="xbox-live-reputation-service"></a>Xbox Live Reputation Service

합니다 *평판 서비스* 결혼 정보 회사 평판 필터링 하는 동안 및 평판에 대 한 사용자 통계를 관리 합니다. SmartMatch 결혼 정보 회사 연결을 통해 2015 멀티 플레이 게임에서 사용 됩니다. 자세한 내용은 [평판](../../social-platform/people-system/reputation.md)합니다.


#### <a name="xbox-live-compute"></a>Xbox Live 계산

Xbox Live Compute 클라우드 2015 멀티 플레이어를 사용 하 여 타이틀에 대 한 처리량을 제공 합니다. Xbox Live Compute 사용에 대 한 자세한 내용은 참조 하세요. [를 사용 하 여 Xbox Live Compute 멀티 플레이 게임에서는](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)합니다.


### <a name="typical-2015-multiplayer-architectures"></a>일반적인 2015 멀티 플레이 아키텍처

이 섹션에서는 가장 일반적인 2015 멀티 플레이 아키텍처를 제공 합니다.
#### <a name="peer-to-peer-architecture"></a>피어 투 피어 아키텍처

피어 투 피어 아키텍처 제목 피어 주소를 검색할 MPSD 및 SmartMatch 결혼 정보 회사 연결을 사용 합니다. 주소를 Xbox 보안 소켓을 사용 하 여 피어 연결에 사용 됩니다. 자세한 내용은 *Xbox One에서 Winsock 소개*합니다.

![피어 투 피어 아키텍처 다이어그램](../../images/multiplayer/Mult2015ArchPeer.png)


#### <a name="client-server-architecture"></a>클라이언트-서버 아키텍처

클라이언트-서버 멀티 플레이 아키텍처 제목 전용된 서버 주소를 검색할 MPSD 및 SmartMatch 결혼 정보 회사 연결을 사용 합니다. 이러한 보안 소켓, Xbox를 사용 하는 전용된 서버에 연결할 사용 됩니다. 자세한 내용은 *Xbox One에서 Winsock 소개*합니다.

| 참고                                                                         |
|-------------------------------------------------------------------------------------------|
| 클라이언트-서버 아키텍처의 서버와 Xbox Live 계산 인스턴스를 사용할 수 있습니다. |

![클라이언트 서버 아키텍처 다이어그램](../../images/multiplayer/Mult2015ArchClientServer.png)


## <a name="parties-supported-by-2015-multiplayer"></a>2015 멀티 플레이어에서 지원 되는 파티
Xbox One 용 2015 멀티 플레이어는 시스템 수준 구문으로 "게임"파티 "를 노출 하지 않습니다. 그러나 "채팅 파티" 2014 멀티 플레이어와 같이 시스템 수준 지원지 않습니다.

| 참고                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------|
| 제목을 것과 유사한 구현 게임 파티를 사용 하지만 대신 MPSD 세션을 사용 하 여 사용자 환경을 계속 구현할 수 있습니다. |


### <a name="chat-party"></a>채팅 파티

채팅 파티는 서로 Xbox 한 타사 앱을 통해 사용자가 관리를 사용 하 여 채팅 하는 사람들의 그룹입니다. 채팅 파티 게임 세션에서 재생 하는 동안 또는 다른 콘솔 작업을 수행 하는 동안 사용자가 될 수 있습니다. 그러나 채팅 파티의 사용자 및 해당 사용자에 대 한 다른 활동 간에 없습니다 ties는.

채팅 파티를 사용 하 여 노출 되는 *PartyChat 클래스*, 제목을 채팅 파티의 상태를 검사할 수 있습니다. 예를 들어 제목을 사용 하 여 채팅 파티의 멤버를 열거할 수는 *PartyChat.GetPartyChatViewAsync 메서드*합니다.


### <a name="implementing-features-similar-to-those-related-to-the-game-party"></a>게임 파티와 관련 된 것과 비슷한 기능을 구현 합니다.

2014 다중 접속 게임 파티는 여러 목적을 제공합니다. 제목을에 허용 되는 다음과 같습니다.

-   사용자의 현재 활동 및 조인에 대 한 가용성 보급
-   세션에 초대 (초대) 보내기
-   검색 하 고 세션에 참여
-   파티에 등록 된 세션의 특정 변경 시 알림 수신
-   사용 하 여 SmartMatch 매치 메이 킹
-   여러 게임 세션에서 사용자 그룹을 함께 유지

2015 다중 접속 MPSD 세션을 통해 직접 위의 모든 기능을 지원합니다.

## <a name="multiplayer-terminology"></a>멀티 플레이 용어


| 용어                                 | 설명|
| --- | --- |
| 활성 플레이어                        | 세션 내에서 활성 상태로 설정 되어 있는 플레이어입니다. 플레이어가 게임에서 참여 하는 경우이 상태로 플레이어를 설정 하는 제목입니다. 자세한 내용은 [세션 사용자 상태](mpsd-session-details.md)합니다.                                                                                                                                                                                                                                                                                          |
| 중재자                              | 예를 들어 매 치메이 킹 찾으려면 더 많은 플레이어 게임 세션 광고의 게임에서 멀티 플레이 세션 디렉터리 (MPSD) 세션의 상태를 관리 하는 게임 세션에서 단일 콘솔. 중재자를 제목으로 설정 됩니다. 항상 중재자는 게임의 호스트가 없습니다. 참조 [세션 중재자](mpsd-session-details.md)합니다.                                                                                                                                                                            |
| 정렬 된 게임                        | 유형 매 치메이 킹 부서의 도움 없이 조인 하는 다른 플레이어를 초대 하는 단일 플레이어를 통해서만 만든 게임입니다.                                                                                                                                                                                                                                                                                                                                                                                                    |
| 채팅 파티                           | 그룹 채팅 함께 사람들입니다. 동일한 활동에서 사용자를 맺을 수 있습니다 또는 게임, 음악, 앱 등의 다른 활동을 맺을 수 있습니다. 참조 **2015 멀티 플레이어에서 지원 되는 파티**합니다.                                                                                                                                                                                                                                                                                 |
| 게임 초대                          | 게임 세션에 참가 하 라는 초대장이 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 게임 세션                         | 사용자가 재생 하는 실제로 함께 세션입니다. 모든 멀티 플레이어 시나리오의 경우 매 치메이 킹 또는 조인 로비, 예를 들어, 궁극적으로 게임 세션에서. 세션은 종종 조인을 사용 하도록 설정 하려면 사용자의 현재 활동으로 알려집니다. 최신 빌드는 또한 플레이어 목록입니다. 참조 [MPSD 세션 정보](mpsd-session-details.md)합니다.                                                                                                                                                           |
| 게임 세션 호스트                    | 게임을 실행 하는 콘솔 피어-투-피어 네트워크 호스트 기반 아키텍처를 기반으로 하는 타이틀에 대 한 시뮬레이션을 재생 합니다. 이 콘솔은 일반적으로 중재자와 동일 하지만 동일 하지 않아도 됩니다.                                                                                                                                                                                                                                                                                                                            |
| 핸들 (또는 세션 핸들)           | 추가 상태 및 연결 된 동작이 있는 MPSD 세션에 대 한 참조입니다. 참조 [MPSD 세션 핸들](multiplayer-session-directory.md)합니다.                                                                                                                                                                                                                                                                                                                                                                    |
| 비활성 플레이어                      | 세션 내에서 비활성 상태로 설정 되어 있는 플레이어입니다. 제목 플레이어를이 상태로 설정 게임을 일시 중단 하거나, 그렇지 않으면 비활성 제목에 정의 된 대로 합니다. 일부 경우에 MPSD 플레이어를 비활성 속성도 설정할 수 이지만 주로 제목 이렇게 해야 합니다. 자세한 내용은 [세션 사용자 상태](mpsd-session-details.md)합니다.                                                                                                                           |
| Hopper                               | Hopper은 일치 티켓을 논리 기반 컬렉션입니다. 제목을 여러 hoppers 있지만 동일한 hopper 내 티켓만 일치 시킬 수 있습니다. 예를 들어, 제목을 하나 hopper는 플레이어에 대 한 기술은 일치 하는 가장 중요 한 항목을 만들 수 있습니다. 다른 hopper는 플레이어만 일치 시 키도 같은 다운로드 가능한 콘텐츠를 구매한 경우 사용할 수 있습니다. Hoppers SmartMatch 워크플로에 적합 한 위치에 대 한 자세한 내용은 참조 하세요. [SmartMatch 런타임 작업](smartmatch-matchmaking.md) |
| 진행 중인 조인                     | 게임 시작 된 후 다른 플레이어를 조인 하는 개념의 게임입니다. 플레이어는 친구의 게이머 카드를 통해 친구의 게임을 조인할 수 있습니다. 그런 다음 제목을 적절 한 시간 게임 세션으로 이러한 플레이어를 이동할 수 있습니다.                                                                                                                                                                                                                                                                                                      |
| 로비 세션                        | 초대 된 플레이어 게임 세션에 참가 하려고 대기 하는 사용자에 대 한 도우미 세션입니다. 참조 [MPSD 세션 정보](mpsd-session-details.md)합니다.                                                                                                                                                                                                                                                                                                                                                                                       |
| 일치 대상 세션                 | 일치 세션 SmartMatch 결혼 정보 회사 연결 하는 동안 일치 항목을 나타내는로 설정 합니다. 참조 [SmartMatch 매 치메이 킹](smartmatch-matchmaking.md)합니다.                                                                                                                                                                                                                                                                                                                                                                                              |
| 일치 티켓 세션                 | SmartMatch 결혼 정보 회사 연결 하는 동안 예비 검색 세션을 설정합니다. 참조 [SmartMatch 매 치메이 킹](smartmatch-matchmaking.md)합니다.                                                                                                                                                                                                                                                                                                                                                                                                         |
| MPSD 세션                         | Xbox Live 클라우드 내에서 멀티 플레이 세션 디렉터리 (MPSD)에 상주 하는 보안 문서입니다. 사용자 및 게임에 대 한 메타 데이터와 함께 Xbox One에 타이틀을 실행 하는 동안 연결 될 수도 있고 사용자의 그룹을 포함 합니다. 참조 [MPSD 세션 정보](mpsd-session-details.md)합니다.                                                                                                                                                                                                                  |
| 멀티 플레이 세션 디렉터리(MPSD) | 멀티 플레이 시스템 저장 하 고 세션을 검색 하는 데 사용 하는 클라우드에서 작동 하는 서비스입니다. 참조 [멀티 플레이 세션 디렉터리 (MPSD)](multiplayer-session-directory.md)합니다.                                                                                                                                                                                                                                                                                                                                                                |
| 타사 앱                            | Xbox One 시스템을 사용 하면 보기 및 해당 파티를 관리 하는 앱을 맞춥니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 서버 세션                       | 게임 세션이 Xbox Live 계산 처리에 의해 만들어집니다. 참조 [MPSD 세션 정보](mpsd-session-details.md)합니다.                                                                                                                                                                                                                                                                                                                                                                                                            |
| 자격 증명 입력 탭                         | 서비스에서 잠재적으로 흥미로운 변경 내용이 발생 하는 제목으로 MPSD에서 알림입니다. 자격 증명 입력 탭은 쉽게 말해 덜 자주 정기적인 알림을 보다 정보입니다. 참조 [MPSD 알림 처리를 변경 하 고 검색 연결 끊기](multiplayer-session-directory.md)합니다.                                                                                                                                                                                                                     |
| 스마트 매치 매치 메이킹               | Xbox Live 매 치메이 킹 기능을 매 치메이 킹 서비스에서 구현 하는 Xbox One 제목에 사용할 수 있습니다. MPSD 및 결혼 정보 회사 연결을 사용 하 여, 제목 일치 시킬 요청 하 고 알립니다 나중 일치 하는 그룹을 발견 되었습니다. 참조 [SmartMatch 매 치메이 킹](smartmatch-matchmaking.md)합니다.                                                                                                                                                                                                                                  |

## <a name="whats-new-in-2015-multiplayer"></a>다중 접속 2015의에서 새로운 기능

| 주의                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2015 멀티 플레이어를 사용 하는 경우 반드시 2014 멀티 플레이어의 파티 관련 클래스를 사용 하는 더 이상 사용 되지 해야 합니다. 2015 멀티 플레이 기능 파티 관련 클래스를 사용 하 여 혼합 경도의 동작 시키고 시도 하지 마십시오. |


### <a name="new-concepts-in-2015-multiplayer"></a>멀티 플레이 게임 2015에서는 새로운 개념


#### <a name="web-socket-connections-to-mpsd"></a>웹 소켓 연결은 MPSD

이제 MPSD에 제목이 된 웹 소켓 연결을 유지할 수 있습니다. 이러한 연결 하면 클라이언트 세션 변경 될 때 알림을 받을 수 있습니다. 자세한 내용은 [MPSD 변경 알림을 처리 하 고 검색 연결 끊기](multiplayer-session-directory.md)합니다.


#### <a name="mpsd-session-handles"></a>MPSD 세션 핸들

2015 멀티 플레이 게임에는 형식화 된 데이터를 포함할 수 있는 세션에 대 한 참조 인 MPSD 세션 핸들에 대 한 지원이 추가 되었습니다. 자세한 내용은 [MPSD 세션 핸들](multiplayer-session-directory.md)합니다.


### <a name="summary-of-new-2015-multiplayer-winrt-api-functionality"></a>새 2015 멀티 플레이 WinRT API 기능 요약

새로운 멀티 플레이 WinRT API 기능 쉽게 전환을 타이틀 이미 2014 다중 접속 게임 파티 API 함께 사용 하는 기존 XSAPI를 기반으로 합니다.

2015 멀티 플레이 게임을 추가 합니다 *MultiplayerActivityDetails 클래스*, 예를 들어 사용자의 현재 활동에 대 한 세부 정보는 사용자가 조인 가능한 세션을 나타내는입니다.

새로운 기능에 추가 되었습니다 합니다 *MultiplayerService 클래스*합니다. 예로 메서드 및 사용자와 소셜 그룹, 검색의 다양 한 필터 및 핸들을 사용 하 여 세션에 대 한 활동을 사용 하 여 처리 하는 속성을 읽고 핸들을 사용 하 여 기록 게임 초대 및 세션 전송.

합니다 *MultiplayerSession 클래스* 기능 세션 변경 형식을 사용 하 여 작업, 알림, 세션 비교 및 닫힘으로 세션 설정에 대 한 구독을 추가 합니다.

합니다 *MultiplayerSessionReference 클래스* 에서 상속 하도록 변경 됩니다 **IMultiplayerSessionReference**네임 스페이스 간 호출을 지원 하기 위해. 클래스는 또한 메서드를 구문 분석 하는 새 URI 경로 있습니다.

| 참고                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 이벤트 및 알림 구독을 지원 하려면 2015 다중 접속 하는 기능을 추가 합니다 *Microsoft.Xbox.Services.RealTimeActivity Namespace*합니다. 새도 포함 된 기능은 *SystemUI.ShowSendGameInvitesAsync 메서드*2015 멀티 플레이어 게임 게임 초대 UI를 표시 하는 데 사용 합니다. |

## <a name="differences-between-xbox-360-and-xbox-one-mpsd-session-functions"></a>Xbox 360 및 Xbox 하나 MPSD 세션 함수 간의 차이점

| 함수 | Xbox 360 | Xbox One |
|---|---|---|
| **게임 세션 정보 가져오기** | XSessionGetDetails, XSessionSearchByID, 또는 제목에는 추적을 수행 하지 않습니다. | 제목은 MPSD에서 세션 정보를 요청합니다. |
|**마이그레이션할 호스트** | 필요한 경우 제목 XSessionMigrateHost를 호출 합니다. | 마이그레이션의 원인에 따라 제목 세션에 대해 새 호스트를 할당할 수 있습니다 또는 새 MPSD 세션을 만들 수 있습니다. |
| **여러 플레이어 세션** | XNetUnregisterKey와 XNetReplaceKey 예를 들어, 한 번에 둘 이상의 세션 처리 하기 어렵습니다. | 서비스 기반 세션 한 세션의 관리를 간결 하 게 만들고 여러 세션의 처리를 간소화 합니다. |
| **Signouts 및 연결 해제** | 제목 처리에 대 한 연결을 끊습니다 및 signout XCloseHandle 또는 XSessionDelete와 다르게 합니다. | MPSD signouts 단순 해지고 처리 및 게임 구성에서 설정한 시간 제한을 연결을 끊습니다. |
| **결혼 정보 회사 연결** | 클라이언트 기반의 결혼 정보 쿼리 | 품질 및 제목 내에서 쉽게 백그라운드 매 치메이 킹 더 잘 일치를 허용 하는 서비스 기반의 결혼 정보입니다. |


### <a name="sessions"></a>세션

Xbox 360 게임 플레이 인스턴스의 세션이 표시 됩니다. 사용자 세션 결혼 정보 회사 서비스에 대 한 검색 및 세션의 끝에 통계를 보고 합니다.

Xbox One에 세션은 보다 일반적인 이며 플레이어의 그룹을 나타냅니다. 세션 콘솔 간의 네트워크 연결에 대 한 필수 이며 세션에서 모든 사용자 간에 공유 해야 하는 정보를 보유 합니다. 몇 가지 예가이 정보에는 보안 세션 및 사용자 지정 게임 데이터의 각 콘솔 주소의 세션에서 허용 하는 플레이어의 수가 포함 됩니다.


### <a name="xbox-matchmaking"></a>Xbox 매치 메이 킹

Xbox 360, 제목 결혼 정보 회사 연결 특성의 스키마 및 해당 특성을 통해 검색 하는 쿼리 집합을 구성 하 여 수행 합니다. 런타임에 제목을 선택 하거나 호스트에 세션 또는 하나에 대 한 검색 합니다.

Xbox One에 매 치메이 킹은 서버를 기반으로 플레이어와 제목 이상 여부를 결정할 호스트 하거나 검색 합니다. 대신 플레이어의 미리 구성 된 각 그룹 "티켓" 세션을 만들고 해당 세션 결혼 정보 회사 서비스에 제출 합니다. 그런 다음 서비스는 다른 세션을 찾습니다 하 고 새 "target" 세션을 구성 하려면 그룹을 결합 합니다. 클라이언트는 일치 항목의 알리고 서비스 품질 (QoS) 게임 플레이 시작 하기 전에 다른 세션 멤버를 사용 하 여 연결 유효성 검사를 수행 합니다.


### <a name="xbox-live-compute-service"></a>Xbox Live 계산 서비스

Xbox Live 계산 서비스는 Xbox One에 새 제품. 개발자가 클라우드로의 탄력적 계산 기능을 활용할 수 있도록 하 고 피어-투-피어 네트워크에서 가능한 것 보다 더 큰 멀티 플레이어 시나리오를 지원 합니다. Xbox Live 계산 서비스에 대 한 자세한 내용은 XDK docs에서 Xbox Live Compute 설명서를 참조 합니다.


## <a name="see-also"></a>참고 항목

[멀티 플레이 세션 디렉터리 (MPSD)](multiplayer-session-directory.md)

[SmartMatch Matchmaking](smartmatch-matchmaking.md)

[(RTA)의 실시간 활동 서비스](../../real-time-activity-service/real-time-activity-service.md)

[평판](../../social-platform/people-system/reputation.md)

[Xbox Live Compute을 사용 하 여 멀티 플레이 게임 (관리 되는 파트너의 액세스 필요)에서는](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)
