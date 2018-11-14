---
title: 멀티 플레이 부록
author: KevinAsgari
description: 멀티 플레이 2015 Xbox Live 서비스에 대 한 자세한 내용을 보려면 링크를 제공 합니다.
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 051a81f68440b293d7198dcdd68479cf3f28e22a
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6161886"
---
# <a name="multiplayer-appendix"></a>멀티 플레이 부록

멀티 플레이 시스템 Xbox One에서 게임 플레이 게임 플레이어의 어셈블리를 그룹으로 수 있습니다. 안전 하 고 사용 하기 쉽고 유연한 수 있도록 뿐만 아니라 간단한 기능을 신속 하 게 빌드할 수 있지만 더 복잡 한 기능을 빌드하고 고유한 서비스에 연결 됩니다.

> **참고**  
이 문서에서는 고급 API 사용법입니다.  시작 지점으로 개발을 크게 간소화 [멀티 플레이어 관리자 API](../multiplayer-manager.md) 대해를 보십시오.  찾을 경우 지원 되지 않는 시나리오 멀티 플레이어 관리자를 알고 있는 사용자 댐을 알려 주시기 바랍니다.

멀티 플레이 시스템의 현재 버전은 2015 멀티 플레이어 합니다. 이 버전 추상화 "게임 자" 개념 및 멀티 플레이 세션 디렉터리 (MPSD)를 사용 하 여 게임 세션을 제어 합니다.

> **참고**  
이전 버전의 멀티 플레이 시스템 2014 멀티 플레이어입니다. 이 버전은 게임 타사 및 참여 당사자를 통해 게임 개념을 기반으로 합니다. 이 버전 이제 더 하지만에 대 한 소스 코드는 XDK를 사용 하 여 계속 제공 됩니다. 2014 멀티 플레이 설명서는 XDK 포함 되어 더 이상 없습니다. 이 설명서에 필요 하면는 XDK의 2014 릴리스를 사용 하십시오.


## <a name="in-this-section"></a>이 섹션의 내용

[소개](introduction-to-the-multiplayer-system.md)  
멀티 플레이 시스템을 소개합니다.

[멀티 플레이어 세션 디렉터리 (mpsd)](multiplayer-session-directory.md)  
모든 제목에서 게임의 멀티 플레이 API 메타 데이터를 제공 하 고 게임 세션을 관리 하는 멀티 플레이 세션 디렉터리 (MPSD)에 대해 설명 합니다.

[MPSD 세션 세부 정보](mpsd-session-details.md)  
MPSD 세션 세부 정보를 제공 합니다.

[2015 멀티 플레이어에 대 한 제목에 적응 때 일반적인 문제](common-issues-when-adapting-multiplayer.md)  
멀티 플레이 2015와 함께 작동 하도록 타이틀을 조정할 때 고려해 야 할 일반적인 문제에 설명 합니다.

[스마트 매치 매치 메이킹](smartmatch-matchmaking.md)  
Xbox Live에서 사용 되는 매치 메이 킹 시스템에 설명 합니다.

[에 중재자 마이그레이션](migrating-an-arbiter.md)  
중재자 마이그레이션 MPSD 프로세스를 설명합니다.

[스마트 매치를 사용 하 여](using-smartmatch-matchmaking.md)  
스마트 매치를 사용 하는 방법을 설명 합니다.

[멀티 플레이 하는 방법의](multiplayer-how-tos.md)  
타이틀에서 2015 멀티 플레이어를 사용 하기 위한 절차를 제공 합니다.

[멀티 플레이 세션 상태 코드](multiplayer-session-status-codes.md)  
Xbox one 멀티 플레이 세션 상태 코드를 정의합니다.

[멀티 플레이 2015 Faq 및 문제 해결](multiplayer-2015-faq.md)  
멀티 플레이어 문제 해결 및 Faq를 정의 합니다.

[Xbox One 멀티 플레이 세션 디렉터리](xbox-one-multiplayer-session-directory.md) 새로운 Xbox One 멀티 플레이 세션 디렉터리 (MPSD) 서비스를 사용 하 여 멀티 플레이 세션 만들기의 개요를 제공 합니다.

[멀티 플레이 게임에 대 한 흐름 초대 합니다.](flows-for-multiplayer-game-invites.md) 멀티 플레이 게임에 다른 플레이어를 초대 하는 것과 관련 된 경험의 흐름에 설명 합니다.

[게임 세션 및 타사 게임 유형과 파티](game-session-and-game-party-visibility-and-joinability.md) 표시 여부와 멀티 플레이 게임 세션에 관련 된 대로 파티 간의 차이점을 설명 합니다.
