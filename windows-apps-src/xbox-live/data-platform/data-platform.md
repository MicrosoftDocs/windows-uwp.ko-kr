---
title: Xbox Live 데이터 플랫폼
author: KevinAsgari
description: Xbox Live 데이터 플랫폼에 구성 된 서비스 도전 과제, 플레이어 통계 및 순위표 관리를 간략하게 설명 합니다.
ms.assetid: a8bb7c4f-09fe-4dba-b3ce-1fab60453831
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 통계, 도전 과제, 순위표, 데이터 플랫폼
ms.localizationpriority: medium
ms.openlocfilehash: fa9f3e9fa697e3ec7ca474a6e0a38cf5ac9536ad
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4503430"
---
# <a name="xbox-live-data-platform---stats-leaderboards-achievements"></a>Xbox Live 데이터 플랫폼-통계, 순위표, 도전 과제

Xbox Live 데이터 플랫폼에 게임 데이터를 쓰는 서비스로 실행 타이틀 수 있습니다. 또한 Xbox Live 데이터 플랫폼 통계, 순위표, 도전 과제를 사용 하 여 타이틀을 사용 하 여 사용자의 참여를 드라이브 및 표면 콘솔 셸 및 Xbox 앱에 대 한 통계를 추천 합니다.

레이싱 게임에 대 한 상태에 대 한 예로 총 경합 (예: 순위표 점수는 커뮤니티에 대 한 비교를 만드는 데 사용할 수 있는 실시간으로 다양 한 상태 문자열에 반영 되나요 끌기 스트립 모드에서 실행 될 수 있습니다. "GoTeamEmily 재생 끌기 스트립 모드: 완료 523 경합이"). 총 경합이 끌기 스트립 모드로 사용 될 수도 기준으로 연결에 대 한, 유사한 경험을 사용 하 여 보장 플레이어 함께 일치 시킬 가능성이 훨씬 더 높습니다.

타이틀은 플레이어 통계에 따라 순위표를 표시할 수 있습니다. 예를 들어는 순위표 완료 경합이의 글로벌 순위 수 있습니다. 게임 엔진에 래퍼 Unity 같은 또는 Xbox Live Api를 직접 사용 하 여 이러한 서비스를 호출 합니다.

주요 통계와 특정 통계를 지정할 수 있습니다. 주요 통계 타이틀의 GameHub에 표시 되 고 게이머가 친구 들에 대 한 플레이어를 비교 합니다.

도전 과제는 통계를 통해 제공 하지 및 타이틀 성과 잠금 때를 결정 합니다. 예를 들어 타이틀 1 분 이내에 경합을 완료 하기 위한 성과 있을 수 있습니다. 타이틀 도전 과제를 잠금 해제 하는 데 필요한 매개 변수는 추적 합니다. 최대 것이 예제에서는 점으로부터 경합 측정 타이틀을를 진행 하 고 도전 과제를 부여 합니다. 일반적으로 게임 점수 도전 과제 완료와 함께 수는 있습니다. 그가 각 도전 과제에 대 한 게이머의 양을 결정 해야 합니다.

## <a name="features"></a>기능 ##
다음 항목에서는 Xbox Live 데이터 플랫폼 기능에 대 한 자세한 정보를 제공합니다.

* [플레이어 통계](../leaderboards-and-stats-2017/player-stats.md)
* [도전 과제](../achievements-2017/achievements.md)
* [순위표](../leaderboards-and-stats-2017/leaderboards.md)
