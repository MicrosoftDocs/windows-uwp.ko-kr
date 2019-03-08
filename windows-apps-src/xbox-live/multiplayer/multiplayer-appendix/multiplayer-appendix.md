---
title: 멀티 플레이 부록
description: 멀티 플레이 2015 Xbox Live 서비스에 자세히 알아보려면 링크를 제공 합니다.
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 703e48c0f200c59fa6f9181905756ef834aabe4a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643388"
---
# <a name="multiplayer-appendix"></a>멀티 플레이 부록

Xbox One에서 멀티 플레이 시스템 게임 및 그룹으로 게임 플레이어의 어셈블리를 사용합니다. 시스템이 안전 하 고 사용 하기 쉽고 유연 하므로 뿐 아니라 간단한 기능을 신속 하 게 빌드 하지만 또한을 더 복잡 한 기능을 빌드하고 사용자 고유의 서비스 플러그 인입니다.

> **참고**  
이 문서는 고급 API 사용.  시작 지점으로 잠시 살펴보겠습니다 합니다 [멀티 플레이 게임 관리자 API](../multiplayer-manager.md) 개발을 크게 간소화 하는 합니다.  발견 되 면 지원 되지 않는 시나리오에서 멀티 플레이 게임 관리자의 파악에 알려 주시기 바랍니다.

멀티 플레이 시스템의 현재 버전은 2015 다중 접속 합니다. 이 버전 "게임 파티" 개념을 추상화 하 고 게임 세션 제어를 멀티 플레이 세션 디렉터리 (MPSD)를 사용 합니다.

> **참고**  
멀티 플레이 시스템의 이전 버전이 2014 다중 접속 합니다. 이 버전은 게임 파티와 파티를 통해 게임 참여의 개념을 기반으로 합니다. 에 대 한 소스 코드는 XDK를 사용 하 여 계속 제공 되지만,이 버전 더 됩니다. 2014 멀티 플레이 설명서는 XDK 포함 되어 더 이상 없습니다. 이 설명서를 해야 하는 경우에 XDK 2014 릴리스를 사용 하세요.


## <a name="in-this-section"></a>이 섹션의 내용

[소개](introduction-to-the-multiplayer-system.md)  
멀티 플레이 시스템을 소개합니다.

[다중 접속 세션 디렉터리 (mpsd)](multiplayer-session-directory.md)  
모든 타이틀에서 멀티 플레이 게임의 API 메타 데이터를 중앙 집중화 하 고 게임 세션을 관리 하는 멀티 플레이 세션 디렉터리를 (MPSD)에 대해 설명 합니다.

[MPSD 세션 정보](mpsd-session-details.md)  
MPSD 세션의 세부 정보를 제공합니다.

[2015 멀티 플레이어 게임 제목을 조정 하는 경우 일반적인 문제](common-issues-when-adapting-multiplayer.md)  
다중 접속 2015와 함께 작동 하도록 제목을 조정할 때 고려해 야 할 일반적인 문제를 설명 합니다.

[SmartMatch Matchmaking](smartmatch-matchmaking.md)  
Xbox Live에서 사용 하는 결혼 정보 시스템을 설명 합니다.

[여기서 중재자는 마이그레이션](migrating-an-arbiter.md)  
중재자 마이그레이션용 MPSD 프로세스를 설명합니다.

[SmartMatch 결혼 정보 회사 연결을 사용 하 여](using-smartmatch-matchmaking.md)  
SmartMatch 결혼 정보 회사 연결을 사용 하는 방법을 설명 합니다.

[멀티 플레이 방법의](multiplayer-how-tos.md)  
2015 멀티 플레이어를 사용 하 여 제목에 대 한 절차를 제공 합니다.

[멀티 플레이 세션 상태 코드](multiplayer-session-status-codes.md)  
Xbox One에 대 한 멀티 플레이 세션 상태 코드를 정의합니다.

[2015 멀티 플레이 Faq 및 문제 해결](multiplayer-2015-faq.md)  
Faq 및 멀티 플레이 게임에 대 한 문제 해결을 정의 합니다.

[Xbox One 멀티 플레이 세션 디렉터리](xbox-one-multiplayer-session-directory.md) 새 Xbox 하나 멀티 플레이 세션 디렉터리 (MPSD) 서비스를 사용 하 여 멀티 플레이 세션 만들기에 간략하게 설명 합니다.

[멀티 플레이 게임 초대에 대 한 흐름](flows-for-multiplayer-game-invites.md) 다른 멀티 플레이 게임을 플레이어가 초대에 관련 된 환경의 흐름을 설명 합니다.

[게임 세션 및 게임 파티 유형과 joinability](game-session-and-game-party-visibility-and-joinability.md) 가시성과 멀티 플레이 게임 세션 관련이 joinability 간의 차이점을 설명 합니다.
