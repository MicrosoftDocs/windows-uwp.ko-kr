---
title: 통계 및 순위표 2017 구성
author: KevinAsgari
description: 데이터 플랫폼 2017을 사용 하 여 유니버설 개발자 센터에서 Xbox Live 기능을 갖춘 통계 및 순위표를 구성 하는 방법을 알아봅니다.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6de9aa9c1e85655a9295370a963a485de5eb474a
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5739591"
---
# <a name="configuring-featured-stats-or-leaderboards-on-universal-dev-center-with-data-platform-2017"></a>데이터 플랫폼 2017 사용 하 여 유니버설 개발자 센터에서 주요 통계 또는 순위표 구성

데이터 플랫폼 2017로만 두 경우에는 상태를 구성 해야 합니다.

* 전역 순위표를 기준으로 메니페스트에 상태 또는
* 상태는 게임 허브 페이지에 표시 되는 추천된 플레이어 상태가입니다.

두 경우 모두 구성 해야 통계 및 순위표 함께. 모든 순위표 상태를 기반으로 하며는 순위표 추천된 플레이어 상태에 대 한 사용 하지 않으려는 경우에 연결된 전역 순위표도 구성 하지 않고는 상태를 구성할 수 없습니다.

추천된 플레이어 통계 고 전역 순위표 사용 되지 않는 통계 구성할 필요가 없습니다.

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>전역 순위표 및 관련된 플레이어 상태를 구성 합니다.

먼저, 아래와 같이 타이틀에 대 한 플레이어 통계 섹션으로 이동 합니다.

![](../images/omega/dev_center_player_stats_creators.png)

클릭은 `Create your first featured stat/leaderboard` 단추를 선택한 다음이 대화 상자를 작성 하 고 저장을 클릭 합니다.

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

`Display name` 필드는 사용자는 GameHub에 표시 됩니다.  이 문자열을 지역화할 수 있는 합니다 `Localize strings` 섹션.  클릭 `Show Options` 에 `Localize strings` 이러한 문자열을 지역화 하는 방법에 자세한 내용을 보려면 섹션.

`ID` 필드는 통계 이름이 고가 참조 하 여 상태 제목 코드에서 업데이트할 때 됩니다.   자세한 내용은 아래 [업데이트 통계](player-stats-updating.md) 섹션을 참조 하세요.

`Format` 의 상태는 데이터 형식입니다.

`Display Logic` 간에 선택 `Player progression`, `Personal best`, 및 `Counter`:
- 플레이어 진행: 개별 플레이어 수준 또는 게임에서 진행을 나타냅니다.  설정 된 마지막 값은 사용자가 표시 됩니다.  예를 들어, 현재 라운드에 놓습니다.
- 개인 가장: 플레이어 게시에 현재 최고 점수를 나타냅니다. 정렬 순서에 따라 설정 된 최소 또는 최대 값은 사용자가 표시 됩니다.  예를 들어 가장 빠른 랩 합니다.
- 카운터: 다른 플레이어의 누적 수를 계산 하려면에 추가할 수 있습니다.  

`Sort` 필드는 순위표 정렬 순서를 변경할 수 있습니다.

이 상태를 만들 수도 `Featured Stat`를 클릭 하지만 `Feature on players' profiles`.  

## <a name="configure-featured-stats"></a>주요 통계를 구성 합니다.

검사 옵션이 플레이어 상태를 정의 하는 경우 `Featured Stat`.  다음 요구를 note 하십시오.

| 개발자 유형 | 요구 사항 |
|----------------|-------------|
| Xbox Live 크리에이터스 프로그램 | 주요 통계도 모든 통계를 지정 하는 요구 사항이 있습니다.  경우에 최대 10 제한 됩니다 |
| ID@XboxMicrosoft 파트너 | 3-10 기능을 갖춘 통계 간에 지정 해야 합니다. |

## <a name="next-steps"></a>다음 단계

그런 다음 제목 코드에서 상태를 업데이트 해야 합니다.  자세한 내용은 [업데이트 통계](player-stats-updating.md) 를 참조 하세요.