---
title: 통계 및 순위표 2017 구성
description: 데이터 플랫폼 2017을 사용 하 여 파트너 센터에서 Xbox Live 주요 통계 및 순위표를 구성 하는 방법에 알아봅니다.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ea2baf4bc27e6d1cfd5beb9ef0386acda72a39d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598858"
---
# <a name="configuring-featured-stats-or-leaderboards-in-partner-center-with-data-platform-2017"></a>데이터 플랫폼 2017 사용 하 여 파트너 센터에서 주요 통계 또는 순위표 구성

데이터 플랫폼 2017을 사용 하 여 두 가지 경우에는 상태를 구성 해야 합니다.

* 전체 순위표를 기준으로 되는 상태 또는
* 상태는 게임 허브 페이지에 표시 되는 주요 플레이어 상태입니다.

두 경우 모두 구성 해야 통계 및 순위표 함께 합니다. 모든 순위표 stat를 기반으로 하며 주요 플레이어 상태에 대 한 순위표를 사용 하지 않으려는 경우에 또한는 연결된 전체 순위표를 구성 하지 않고는 stat를 구성할 수 없습니다.

주요 플레이어 통계 없고 전체 순위표에서 사용 하지 않는 통계를 구성할 필요가 없습니다.

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>전체 순위표 및 연결 된 플레이어 stat를 구성 합니다.

먼저 아래와 같이 제목에 대 한 플레이어 통계 섹션으로 이동 합니다.

![](../images/omega/dev_center_player_stats_creators.png)

클릭 된 `Create your first featured stat/leaderboard` 단추를 한 후이 대화 상자를 채웁니다 및 저장을 클릭 합니다.

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

`Display name` 필드는 사용자에 게는 GameHub에 표시 됩니다.  이 문자열을 지역화할 수 있는 여 `Localize strings` 섹션입니다.  클릭 `Show Options` 에 `Localize strings` 이러한 문자열을 지역화 하는 방법에 대 한 자세한 내용은 섹션입니다.

`ID` 필드 상태 이름 및 참조 하는 방법 됩니다에 stat 제목 코드에서 업데이트 시 됩니다.   참조 된 [통계 업데이트](player-stats-updating.md) 대 한 자세한 내용을 보려면 아래 섹션입니다.

`Format` 는 stat의 데이터 형식입니다.

합니다 `Display Logic` 에서는 중 선택할 `Player progression`를 `Personal best`, 및 `Counter`:
- 플레이어 진행 합니다. 개별 플레이어 수준이 나 게임에서 진행을 나타냅니다.  설정 된 마지막 값은 사용자에 게 표시 됩니다.  예를 들어, 현재 라운드에 놓습니다.
- 개인 가장 적합 합니다. 현재 최상의 점수가 플레이어 게시를 나타냅니다. 정렬 순서에 따라 설정 된 최대값 또는 최소값 값은 사용자에 게 표시 됩니다.  예를 들어, 가장 빠른 살펴보기입니다.
- 카운터: 다른 플레이어의 누적 수를 계산 하려면에 추가할 수 있습니다.  

`Sort` 필드 순위표의 정렬 순서를 변경할 수 있습니다.

이 통계를 만들 수도 있습니다는 `Featured Stat`를 클릭 하지만 `Feature on players' profiles`합니다.  

## <a name="configure-featured-stats"></a>주요 통계를 구성 합니다.

검사 옵션이 플레이어 stat를 정의할 때 `Featured Stat`합니다.  다음 요구 사항을 note 하십시오.

| 개발자 유형 | 요구 사항 |
|----------------|-------------|
| Xbox Live 크리에이터스 프로그램 | 주요 통계와 모든 통계를 지정 하는 요구 사항이 있습니다.  최대 10 개로 제한 됩니다 있지만 |
| ID@Xbox Microsoft 파트너 | 주요 통계 3 자에서 10 사이 지정 해야 합니다. |

## <a name="next-steps"></a>다음 단계

그런 다음 제목 코드에서 상태를 업데이트 해야 합니다.  참조 [통계 업데이트](player-stats-updating.md) 자세한 세부 정보에 대 한 합니다.