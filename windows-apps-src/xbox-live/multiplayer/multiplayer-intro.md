---
title: Xbox Live 멀티 플레이 플랫폼
description: Xbox Live에서 지원 되는 멀티 플레이 플랫폼에 알아봅니다.
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이 게임
ms.localizationpriority: medium
ms.openlocfilehash: c71fc28a9bb8668d0253834c1a2c9c6ca7736fd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651158"
---
# <a name="xbox-live-multiplayer-platform"></a>Xbox Live 멀티 플레이 플랫폼

Xbox Live 멀티 플레이 게임 플랫폼 게임 플레이어 Xbox Live를 인터넷을 통해 결합을 통해 개발 역량을 및 수명 및 일반적인 누구나 play 이외의 제목의 사용량 크게 확장할 수 있습니다.

대상 그룹에 대 한 뛰어난 멀티 플레이어 환경을 빌드하여를 게임에 대 한 기본 사용자를 늘리고 함께 재생 하는 전용된 팬의 지속적인된 커뮤니티 승격 Xbox Live 게이머의 대규모 소셜 네트워크를 활용할 수 있습니다.


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>Xbox Live 멀티 플레이 게임 플랫폼 이란?

Xbox Live 멀티 플레이 게임 플랫폼은 클라이언트 실시간 멀티 플레이 게임을 구현 하는 데 사용할 수 있는 Api의 집합. API 제품군의 주요 하위 시스템은 같습니다.

-   합니다 **Xbox Live 멀티 플레이 게임 세션 디렉터리 (MPSD)** 서비스입니다. 사용자 찾기 및 재생을 초대 하는 서로 용이 하 게 통합 된 UI 환경을 MPSD 서비스에서 작동 합니다. Xbox Live의 서비스와의 통합에는 고객이 어셈블하기 Xbox Live 파티 채팅을 사용 하 여 수 있습니다.
-   **간단 하 고 고급 매 치메이 킹 기능입니다.** Xbox Live 고도로 사용자 지정 된 매 치메이 킹 시나리오에 대 한 찾아보기 세션을 지원도 기존의 퀵 매치 검색 기능을 제공합니다. Xbox Live 그룹 (LFG)를 찾고 서로 파티 채팅에서 rally 찾고, 다음 게임을 플레이 하며 플레이어를 수도 있습니다.
-   **피어 투 피어 및 클라이언트-서버 네트워킹 Api** 최신 인터넷 표준을 활용 하 여 보안 실시간 통신을 제공 하 고 Xbox live 적극적으로 모니터링 합니다. 표준화 및 통합 문제 해결 경험 Xbox Live 네트워크를 사용 하 여 신속 하 게 연결 문제를 해결할 수가 있습니다.  
-   **통합된 음성 및 텍스트 채팅 솔루션** 소셜 그래프 Xbox Live, media services 및 Xbox One 장치에서 특수 인코딩 하드웨어를 활용 하 여 안전 하 게 게임 내 의사 소통을 촉진 하는 합니다.

몇 가지 가장 일반적인 멀티 플레이어 시나리오와 이러한 시나리오를 구현 하는 Xbox Live 기능 도움이 개요를 보려면 [멀티 플레이 수준](multiplayer-scenarios.md)합니다.

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>Xbox Live 다중 접속 게임에서 구현할 수는 방법
시나리오에 따라 Xbox Live 멀티 플레이 게임 플랫폼 게임에 Xbox Live 멀티 플레이 게임을 구현 하는 데 몇 가지 방법을 제공 합니다.

### <a name="xbox-integrated-multiplayer-xim"></a>Xbox 통합 멀티 플레이어(XIM)
XIM는 Xbox Live 서비스의 기능을 통해 게임에 실시간 다중 접속 네트워킹 및 통신을 추가 하기 위한 턴키 솔루션입니다. 이전에 Xbox One 및 Windows 10에서 고품질 피어-투-피어 (P2P) 멀티 플레이 게임 빌드 보다 쉽게 수행할 수 있도록 XIM의 목표가입니다.

XIM 다음 기능을 제공 합니다.
- 간단한 결혼 정보 회사 연결 및 게임 초대를 지원 합니다.
- 간단 하 고 안전한 실시간 네트워크 Xbox Live 멀티 플레이 게임 릴레이 서비스에 의해 투명 하 게 확대 됩니다.
- 간단한 텍스트 및 음성 채팅, 화하고 통신 더욱 최종 사용자 환경을 설명 하는 기능을 사용 하 여 합니다.
- 장애인이 게임 상태 마이그레이션 뿐 아니라 검색 및 네트워크 정체를 관리 합니다.

XIM은 Xbox Live 멀티 플레이 플랫폼 하지만 또한 이상 사용자 지정이 가능한를 통해 사용할 수 있는 가장 간단한 멀티 플레이어 솔루션 이며 P2P 게임에만 적합 합니다. XIM에 대 한 자세한 내용은 참조 하세요. [Xbox 통합 멀티 플레이 게임](xbox-integrated-multiplayer.md)합니다.

### <a name="xbox-multiplayer-manager"></a>Xbox Multiplayer Manager
Xbox 멀티 플레이 게임 관리자 (MPM)는 Xbox Live의 멀티 플레이 세션 디렉터리, 초대 및 결혼 정보 회사 서비스에 대 한 유연한 액세스를 제공 하는 API 클라이언트.

모범 사례를 따르고도 다양 한 게임 인증을 전달 하기 위해 구현 해야 하는 Xbox 요구 (XRs)를 처리 하는 효율적인 방식으로 많은 일반적인 멀티 플레이 시나리오를 구현 합니다.

Xbox 멀티 플레이 게임 Manager 네트워킹 또는 채팅 레이어를 구현 하지 않습니다. MPM 유연 하 고 있지만 간소화 된 통합 멀티 플레이어를 위한 관리 API Windows.Networking.XboxLive를 통해 구현 하는 네트워킹 보안 계층을 사용 하 여 쌍을 이루는 게임으로 설계 되었습니다. 게임 내 통신을 사용 하 여 추가할 수는 [게임 채팅 2](chat/game-chat-2-overview.md) API 또는 XIM 채팅 예약 합니다. 네트워킹 및 게임 채팅 2 Api Xbox One XDK 및 Xbox Live 플랫폼 확장 SDK에 설명 되어 있습니다.

MPM 멀티 플레이 게임에 대 한 전용된 서버를 호스트 하는 경우 적합 합니다. Xbox Live 토너먼트와의 통합 등 고급 시나리오에 잘 MPM 적합합니다. MPM에 대 한 자세한 내용은 참조 하세요. [멀티 플레이 게임 Manager 소개](multiplayer-manager/multiplayer-manager-api-overview.md)합니다.

다중 접속 관리자를 사용 하려면 멀티 플레이 시나리오에 Xbox Live 서비스를 구성 해야 합니다. 이 구성에 대 한 자세한 내용은 참조 하세요. [멀티 플레이 서비스를 구성](service-configuration/configure-the-multiplayer-service.md)합니다.

>다중 접속 관리자 현재 찾아보기 세션을 지원 하지 않습니다. 에 대해서 [멀티 플레이 게임 세션 찾아보기](session-browse.md)합니다.

### <a name="api-capabilites"></a>API 기능

기능 | Xbox 통합된 멀티 플레이 게임| 멀티 플레이어 관리자
--  | -- | --
표시 여부 |  XIM은 원본 사용 하지 않고 컴파일된 라이브러리로 제공 됩니다.  | 직접 XSAPI 또는 Xbox Live 서비스를 사용 하 여 상호 작용 하 여 동작을 사용자 지정할 수 있도록 MPM 원본을 사용 하 여 제공 됩니다.
세션 및 결혼 정보 회사 연결 | XIM 미리 구성 된 간단한 결혼 정보 회사 연결 규칙을 제공 하 고 멀티 플레이 구성이 필요 하지 않습니다. | MPM [멀티 플레이 서비스를 구성 해야](service-configuration/configure-the-multiplayer-service.md), 결혼 정보 회사 연결 및 세션 동작의 정교한 사양 수 있습니다.
네트워킹 | XIM 필요한 경우 Xbox Live 릴레이 서비스에서 지 원하는 간단한 & 보안 플레이어를 네트워크를 제공 합니다. | MPM은 Windows.Networking.XboxLive를 사용 하 여 사용자 고유의 보안 네트워킹 솔루션에 연결할 수 있도록 설계 되었습니다.
게임 채팅 | XIM 통합된 음성 및 텍스트 채팅을 제공합니다. | 게임 채팅 2 API를 사용 하 여 게임 내 통신을 구현할 수 있습니다 또는 채팅을 MPM 있도록 대역의 예약 XIM를 사용 하 여 명단을 관리 합니다.
