---
title: Xbox Live 멀티 플레이 플랫폼
author: KevinAsgari
description: Xbox Live에서 지 원하는 멀티 플레이 플랫폼에 알아봅니다.
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: 27b91376bfdb58bd474f7d291fa9f650455461c0
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4506786"
---
# <a name="xbox-live-multiplayer-platform"></a>Xbox Live 멀티 플레이 플랫폼

Xbox Live 멀티 플레이 플랫폼 게임을 Xbox Live 플레이어 인터넷을 통해 종합 강화 하 고 사용 및 사용 현황 일반적인 단독 플레이 넘어 제목의 획기적으로 확장할 수 있습니다.

대상 그룹에 대 한 좋은 멀티 플레이어 경험을 만들어서 큰 소셜 네트워크의 Xbox Live 게임에 대 한 기본 사용자 증가 하 고 함께 재생 전용된 팬의 지속적인된 커뮤니티 홍보를 활용할 수 있습니다.


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>Xbox Live 멀티 플레이 플랫폼 란?

Xbox Live 멀티 플레이 플랫폼은 클라이언트 실시간 멀티 플레이 게임을 구현 하는 데 사용할 수 있는 Api 집합입니다. API 제품군의 주요 하위 시스템은 다음과 같습니다.

-   **Xbox Live 멀티 플레이어 세션 디렉터리 (MPSD)** 서비스입니다. MPSD 서비스 통합 된 UI 환경을 사용자 찾기 및 재생에 대 한 서로 초대를 용이 하 게 작동 합니다. Xbox Live의 서비스와의 통합에는 고객이 조립 Xbox Live 파티 채팅을 사용할 수 있습니다.
-   **단순 하 고 고급 매치 메이 킹 기능입니다.** Xbox Live 높은 사용자 지정된 연결 시나리오에 대 한 기존의 퀵 매치 검색 기능을 하지만 세션 검색 및 지원을 제공합니다. 또한 Xbox Live 그룹 (LFG)를 찾고 플레이어가 서로 파티 채팅에 집결 하 고 게임을 플레이할 수 있습니다.
-   **피어 투 피어 및 클라이언트-서버 네트워킹 Api** 적극적으로 Xbox Live에서 모니터링 및 최신 인터넷 표준을 활용 하는 보안 실시간 통신을 제공 합니다. 표준화 및 문제 해결 경험 Xbox Live 네트워크와의 통합을 신속 하 게 연결 문제 재조정 수 있습니다.  
-   **통합 된 음성 및 텍스트 채팅 솔루션** Xbox Live 소셜 그래프, 미디어 서비스 및 Xbox One 디바이스 인코딩 특수 하드웨어를 활용 하 여 안전한 게임에서 통신을 원활 하.

가장 일반적인 멀티 플레이어 시나리오 및 중 일부에 대 한 개요 Xbox Live 기능 이러한 시나리오를 구현, [멀티 플레이어](multiplayer-scenarios.md)수준 데 도움이 됩니다.

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>Xbox Live 멀티 플레이어 게임의 구현 방법을
시나리오에 따라 Xbox Live 멀티 플레이 플랫폼 게임에 Xbox Live 멀티 플레이어를 구현 하는 여러 가지 방법을 제공 합니다.

### <a name="xbox-integrated-multiplayer-xim"></a>Xbox 통합 멀티 플레이어(XIM)
XIM 실시간 멀티 플레이어 네트워킹 및 통신 Xbox Live 서비스의 기능을 통해 게임에 추가 하기 위한 턴키 솔루션입니다. 이전에 Xbox One 및 Windows 10에서 고품질 피어-투-피어 (P2P) 멀티 플레이어 게임 빌드 보다 쉽게 XIM의 목표가입니다.

XIM은 다음과 같은 기능을 제공합니다.
- 간단한 매치 메이 킹 및 게임 초대에 대 한 지원 합니다.
- 단순 하 고 보안 실시간 네트워크에 Xbox Live 멀티 플레이어 릴레이 서비스에 의해 확대 투명 하 게 됩니다.
- 간단한 음성 및 텍스트 채팅에 복사 하 고 쉽게 최종 사용자 경험에 대 한 통신을 설명 하기 위한 기능을 사용 하 여 합니다.
- 게임 상태를 마이그레이션 뿐 아니라 감지 하 고 네트워크 정체를 관리 하는 시에 도움이 될 합니다.

XIM은 Xbox Live 멀티 플레이 플랫폼에 있지만 이상 가능한 통해 사용할 수 있는 가장 간단한 멀티 플레이 솔루션 및 P2P 게임에만 적합 합니다. XIM에 대 한 자세한 내용은 [Xbox 통합 멀티 플레이어](xbox-integrated-multiplayer.md)를 참조 하세요.

### <a name="xbox-multiplayer-manager"></a>Xbox 멀티 플레이어 관리자
Xbox 멀티 플레이어 관리자 (MPM)은 클라이언트 Xbox Live의 멀티 플레이 세션 디렉터리, 초대 및 매치 메이 킹 서비스에 대 한 유연한 액세스를 제공 하는 API입니다.

모범 사례에 따라도 다양 한 게임 인증을 통과 하려면 구현 해야 하는 Xbox 요구 사항 (XRs)를 처리 하는 효율적인 방식으로 여러 일반적인 멀티 플레이어 시나리오를 구현 합니다.

네트워킹 또는 채팅 계층에는 Xbox 멀티 플레이어 관리자 구현 하지 않습니다. MPM 유연 하지만 간소화 및 통합 멀티 플레이어 관리 API Windows.Networking.XboxLive 통해 구현 된 보안 네트워킹 계층 페어링된 게임으로 설계 되었습니다. [게임 채팅 2](chat/game-chat-2-overview.md) API 사용 하 여 또는 XIM 채팅 예약을 통해 게임에서 통신을 추가할 수 있습니다. 네트워킹 및 게임 채팅 2 Api는 Xbox One XDK 및 Xbox Live 플랫폼 확장 SDK에 기록 됩니다.

MPM 멀티 플레이어 게임에 대 한 전용된 서버를 호스팅할 경우 최선의 선택 합니다. Xbox Live 토너먼트와의 통합 등의 고급 시나리오에 대 한 올바른 MPM 적합합니다. MPM에 대 한 자세한 내용은 [소개 멀티 플레이어 관리자를](multiplayer-manager/multiplayer-manager-api-overview.md)참조 하세요.

멀티 플레이어 관리자를 사용 하려면 멀티 플레이어 시나리오에 대 한 Xbox Live 서비스를 구성 해야 합니다. 이 구성에 대 한 자세한 내용은 [구성을 멀티 플레이 서비스를](service-configuration/configure-the-multiplayer-service.md)참조 하세요.

>멀티 플레이어 관리자 현재 세션 검색을 지원 하지 않습니다. 정보 [멀티 플레이어 세션 검색](session-browse.md)을 참조 하세요.

### <a name="api-capabilites"></a>API 기능

기능 | Xbox 통합 멀티 플레이어| 멀티 플레이어 관리자
--  | -- | --
표시 여부 |  XIM 소스 없이 컴파일된 라이브러리 제공 됩니다.  | 직접 XSAPI 또는 Xbox Live 서비스와 상호 작용 하 여 동작을 사용자 지정할 수 있도록 MPM 소스를 함께 제공 됩니다.
세션 및 연결 | XIM 간단한 미리 구성 된 매치 메이 킹 규칙을 제공 하 고 멀티 플레이 구성이 필요 하지 않습니다. | MPM [멀티 플레이 서비스 구성 하려면](service-configuration/configure-the-multiplayer-service.md), 매치 메이 킹 및 세션 동작의 복잡 한 사양 수 있도록 해줍니다.
네트워킹 | XIM 필요할 때 Xbox Live 릴레이 서비스에서 백업 간단한 및 보안 플레이어를 네트워크에 제공 합니다. | MPM은 Windows.Networking.XboxLive를 사용 하 여 보안 자체 네트워킹 솔루션에 연결할 수 있도록 설계 되었습니다.
게임 채팅 | XIM 통합된 음성 및 텍스트 채팅을 제공합니다. | 게임 채팅 2 API를 사용 하 여 게임에서 통신을 구현할 수 또는 XIM을 사용 하 여 대역의 예약 된 MPM 채팅을 활성화 하는 명단 관리 합니다.
