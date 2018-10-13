---
title: 멀티 플레이 부록
author: KevinAsgari
description: 멀티 플레이 2015 Xbox Live 서비스에 대 한 자세한 정보에 대 한 링크를 제공 합니다.
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a85df1f0d5e58a89edffe8afe192563eecbfb60b
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4573204"
---
# <a name="multiplayer-appendix"></a>멀티 플레이 부록

멀티 플레이 시스템 Xbox One에서 게임 플레이 게임 플레이어의 어셈블리를 그룹으로 수 있습니다. 안전 하 고 사용 하기 쉽고 유연한 수 있도록 뿐만 아니라 간단한 기능을 신속 하 게 빌드할 수 있지만 더 복잡 한 기능을 빌드하고 고유한 서비스에 연결 됩니다.

> **참고**  
이 문서는 고급 API 사용에 대 한입니다.  시작 지점으로 개발을 크게 간소화 [멀티 플레이어 관리자 API](../multiplayer-manager.md) 대해를 보십시오.  찾을 경우 지원 되지 않는 시나리오 멀티 플레이어 관리자를 알고 있는 댐을 알려 주시기 바랍니다.

멀티 플레이 시스템의 현재 버전은 2015 멀티 플레이어. 이 버전 "게임 자" 개념을 추상화 하 고 멀티 플레이 세션 디렉터리 (MPSD)를 사용 하 여 게임 세션을 제어 합니다.

> **참고**  
멀티 플레이 시스템의 이전 버전은 2014 멀티 플레이어입니다. 이 버전은 게임 패리티 및 당사자를 통해 게임에 참여의 개념을 기반으로 합니다. 이 버전 이제 더 하지만에 대 한 소스 코드는 XDK를 사용 하 여 계속 제공 됩니다. 2014 멀티 플레이 설명서가 더 이상는 XDK 포함 되었습니다. 이 설명서가 필요는 XDK의 2014 릴리스를 사용 하십시오.


## <a name="in-this-section"></a>이 섹션 내용

[소개](introduction-to-the-multiplayer-system.md)  
멀티 플레이 시스템을 소개합니다.

[멀티 플레이 세션 디렉터리 (mpsd)](multiplayer-session-directory.md)  
모든 제목에서 게임의 멀티 플레이 API 메타 데이터를 제공 하 고 게임 세션을 관리 하는 멀티 플레이 세션 디렉터리 (MPSD)에 대해 설명 합니다.

[MPSD 세션 세부 정보](mpsd-session-details.md)  
MPSD 세션 세부 정보를 제공 합니다.

[2015 멀티 플레이어에 대 한 제목에 맞게 조정 하는 경우에 일반적인 문제](common-issues-when-adapting-multiplayer.md)  
멀티 플레이 2015와 함께 작동 하도록 타이틀을 조정할 때 고려할 일반적인 문제에 설명 합니다.

[스마트 매치 매치 메이킹](smartmatch-matchmaking.md)  
Xbox Live에서 사용 하는 매치 메이 킹 시스템에 설명 합니다.

[에 중재자 마이그레이션](migrating-an-arbiter.md)  
중재자 마이그레이션 MPSD 프로세스를 설명합니다.

[스마트 매치를 사용 하 여](using-smartmatch-matchmaking.md)  
스마트 매치를 사용 하는 방법에 설명 합니다.

[멀티 플레이 하는 방법의](multiplayer-how-tos.md)  
타이틀에서 2015 멀티 플레이어를 사용 하기 위한 절차를 제공 합니다.

[멀티 플레이 세션 상태 코드](multiplayer-session-status-codes.md)  
Xbox one 멀티 플레이 세션 상태 코드를 정의합니다.

[멀티 플레이 2015 Faq 및 문제 해결](multiplayer-2015-faq.md)  
Faq 및 문제 해결 멀티 플레이어를 정의 합니다.

[Xbox One 멀티 플레이 세션 디렉터리](xbox-one-multiplayer-session-directory.md) 새로운 Xbox One 멀티 플레이 세션 디렉터리 (MPSD) 서비스를 사용 하 여 멀티 플레이 세션 만들기의 개요를 제공 합니다.

[멀티 플레이 게임에 대 한 흐름 초대 합니다.](flows-for-multiplayer-game-invites.md) 환경의 다른 플레이어 멀티 플레이 게임 초대에 관련 된 흐름에 설명 합니다.

[게임 세션 및 타사 게임 유형과 파티](game-session-and-game-party-visibility-and-joinability.md) 표시 여부와 멀티 플레이 게임 세션에 관련 된 대로 파티 간의 차이점을 설명 합니다.
