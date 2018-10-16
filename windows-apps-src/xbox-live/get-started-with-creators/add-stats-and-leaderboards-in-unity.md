---
title: 플레이어 통계 및 순위표 Unity 프로젝트에 추가
author: KevinAsgari
description: 플레이어 통계 및 순위표 Unity 프로젝트를 추가 하는 Xbox Live Unity 플러그 인을 사용 하는 방법에 대해 설명 합니다.
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.author: kevinasg
ms.date: 10/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 크리에이터 스 하나, Unity, xbox
ms.localizationpriority: medium
ms.openlocfilehash: b23d2964e541ea9102a704caa187041a2ce57891
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4694324"
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>플레이어 통계 및 순위표 Unity 프로젝트에 추가

> [!IMPORTANT]
> Xbox Live Unity 플러그 인 도전 과제 또는 온라인 멀티 플레이어를 지원 하지 않는 및 [Xbox Live 크리에이터 스 프로그램](../developer-program-overview.md) 구성원 에게만 권장 됩니다.

[Xbox Live 로그인](unity-prefabs-and-sign-in.md) Unity 프로젝트를 추가한 후 다음 단계 플레이어 통계 및 순위표 해당 플레이어 통계에 따라 추가 하는 것입니다.

[Xbox Live Unity 플러그 인](https://github.com/Microsoft/xbox-live-unity-plugin)을 사용 하 여 쉽게 추가할 수 있습니다 플레이어 통계 및 순위표 Unity 프로젝트에서. 부호 단계에서와 마찬가지로 선택할 수 있습니다 포함된 프리 팹 사용 하거나 고유한 사용자 지정 게임 개체에 포함 된 스크립트를 첨부 하세요.

## <a name="prerequisites"></a>필수 구성 요소
1. [구성 Xbox Live Unity에서](configure-xbox-live-in-unity.md)
2. [Unity에서 Live Xbox에 로그인](unity-prefabs-and-sign-in.md)

## <a name="player-stats"></a>플레이어 통계

플레이어 상태 플레이어를 추적 하려는 모든 흥미로운 통계입니다. Xbox live를 추적 하는 통계 플레이어에 게 관련 되 고 일부 방식으로 표시 되는 통계 되어야 합니다. 이러한 플레이어 통계 플레이어 다른 플레이어에 대 한 성능을 순위 하는 방법을 확인 하려면 볼 수 있는 순위표 빌드하는 데 주로 사용 됩니다. 모든 플레이어 통계의 상태는 게임에 대 한 GameHub 페이지에 표시 됩니다. 즉 "주요 통계"으로 간주 됩니다.

플레이어 통계 단일 값을 표시 해야 합니다. Xbox Live Unity 플러그 인에 대 한 정수, double, 프리 팹 및 문자열 통계 포함 되어 있습니다. 또한 다른 데이터 형식으로 확장 될 수 있는 기본 통계 개체에 대 한 스크립트 제공 됩니다.

플레이어 통계에 대 한 자세한 정보를 [플레이어 통계](../leaderboards-and-stats-2017/player-stats.md)를 참조 하세요.

> [!NOTE]
> 플레이어 통계 또는 순위표 Xbox Live 서비스를 사용 하기 위해 있어야 성공적으로 로그인된 사용자 전에 모든 데이터에 액세스할 수 있습니다.

## <a name="using-the-player-stat-prefabs"></a>플레이어 통계 프리 팹을 사용 하 여

플레이어 통계와 관련 된 사용할 수 있는 Xbox Live Unity 플러그 인에 제공 된 여러 프리 팹 가지가 있습니다.

* IntegerStat: 파괴 라운드에서의 총 수 등의 정수 값으로 표현할 수 있는 상태.
* DoubleStat:는 상태 부동 표현할 수 있는 kill/데스 비율 같은 값을 가리킵니다.
* StringStat: 일반적으로 열거형, 라운드 한 순위 등, "골드", "실버" 또는 "청동" 같은 문자열 값으로 표현할 수 있는 상태입니다.
* StatPanel: 샘플은 시작의 현재 값을 표시 하는 데 사용할 수 있는 UI 합니다.

플레이어 상태를 추가 하려면 장면에 상태의 데이터 형식과 일치 하는 프리 팹을 끌면 됩니다. Unity 관리자의 상태에 대 한 세 가지 값을 지정할 수 있습니다.

* ID는 상태입니다. 이 Windows 개발자 센터에서 구성 된 ID와 일치 해야 하며 대/소문자 구분 합니다.
* 표시 이름 (이 이름은 StatPanel prefab UI에 표시 됩니다.) 상태입니다.
* 초기 값 장면 시작 될 때 상태입니다.

**StatPanel** 프리 팹은 상태 값을 표시를 사용할 수 있습니다. 이렇게 하려면 **StatPanel** 프리 팹을 장면 끌어다 놓습니다. 통계 gameobject Unity 관리자에서 **StatPanel** 개체의 **상태** 필드를 끌어 표시 하는 상태를 지정할 수 있습니다.

### <a name="manipulating-the-player-stat-values"></a>플레이어 통계 값 조작

플레이어 통계 개체의 상태 값을 조정 하려면 호출할 수 있는 함수 수가 표시 합니다. 이러한 함수에서 다른 루틴을 호출 하거나 UI 요소에 바인딩된 될 수 있습니다. 함수는 상태 값을 변경 하는 예제를 보려면 **DoubleStat**, **IntegerStat**및 **StringStat** 스크립트 볼 수 있습니다. 수정 하거나 새 스크립트를 나타내는 더 복잡 한 기능 및 논리를 사용 하 여 통계를 만들 수 있습니다. 새 통계 클래스까지 확장 되어야 합니다 `StatBase` 에 정의 된 클래스는 `StatBase` 스크립트입니다.

예를 들어, 간단한 테스트를 추가할 수 있습니다 UI 단추 및를 장면에는 `OnClick` Unity 관리자에서 단추 이벤트는 **IntegerStat** 추가 개체를 호출 합니다 `Increment()` 는 butto 클릭할 때마다는 상태 값 1 씩 증가 하는 함수 n 합니다.

또한 **StatPanel** 개체에 바인딩된 상태, 있는 경우 단추를 클릭할 때마다 업데이트 시작 값을 확인할 수 있습니다.

(증가, 감소 등)에 통계를 업데이트할 때마다 값이 로컬로 업데이트 가져옵니다. 이러한 통계 업데이트 하려면 두 가지 조치가 Xbox Live에 반영 합니다. 먼저 StatisticManager.SetStatistic 함수 중 하나를 사용 하 여 통계 값을 설정 해야 합니다. 3 가지 `StatisticManager` 통계를 설정 하는 함수 `StatisticManager.SetStatisticIntegerData(XboxLiveUser user, String statName, Int64 value)`, `StatisticManager.SetStatisticNumberData(XboxLiveUser user, String statName, Double value)`, 및 `StatisticManager.SetStatisticStringData(XboxLiveUser user, String statName, String value)`. 이러한 함수는 각각에 상태 데이터 유형에 대 한 적절 한 값을 설정 하는 데 사용 됩니다. 서버에서 사용자 통계를 업데이트 하는 데 필요한 것은 두 번째 로컬 데이터를 *플러시합니다* 입니다. 데이터는 자동으로 하 여 5 분 마다 플러시될 가져옵니다 합니다 `StatManagerComponent` 스크립트입니다.  데이터를 수동으로 flush 하기 위해 노력해 필요한 경우 5 분 전에 게임 종료를 먼저 확인 잃지 해당 진행 합니다. 이렇게 하려면 호출 해야 합니다 `statManagerComponent.RequestFlushToService()` 메서드는 상태 **XboxLiveUser** 에 대 한 호출을 쓰고에 대 한 합니다.

> [!TIP]
> 모범 사례를 항상 진행률이 손실 되지 않도록 되도록 게임을 종료 하기 전에 데이터를 플러시합니다 간주 됩니다.

### <a name="checking-and-verifying-stats"></a>검사 및 통계를 확인 합니다.

`StatisticManager` 클래스에 대해 구성 된 통계에 검사에 대 한 유용한 두 함수에는 `XboxLiveUser`, `StatisticManager.GetStatisticNames(XboxLiveUser user)` 및 `StatisticManager.GetStatistic(XboxLiveUser user, String statName)`. `GetStatisticNames()` 제공는 `list<string>` 제공 XboxLiveUser에 대 한 통계의 이름으로 채워집니다. 이러한 이름은 호출 하 여 통계의 현재 값에 대 한 호출에 사용할 수는 `GetStatistic()` 함수입니다. 것이 Xbox Live 통계 서비스에서 통계를 읽을 수 있는 동안 좋습니다 게임 논리를 사용 하는 유의 해야 하지만 푸시 되었는지 후 통계의 상태를 확인 하려면 대신 중요 합니다. 서비스는 순위표와 같은 다른 서비스를 실행 하는 데는 기능만 하며 게임의 통계에 대 한 진실의 소스 다루지는지 않습니다. 것이 중요 타이틀 검사가 수행 되지 않습니다 사용자 통계에 Xbox 서비스 및 현재 상태를 가리키는 값 간단 하 게 수락 대로 모든 통계 논리를 처리 합니다.

## <a name="leaderboards"></a>순위표

순위표 상태는 "최선의" 값을 달성 한 플레이어는 순서가 지정 된, 번호가 매겨진 목록을 나타냅니다. 예를 들어, 순위표는 플레이어가 다른 플레이어 함으로써 최상의 경합 시간에 대 한 최상의 경합 시간 비교할 수 있도록 경합 바퀴를에서 가장 빠른 시간을 구현 하는 사람을 나열 될 수 있습니다.

순위표 게임에 Xbox Live 서비스에 전송 되는 플레이어 통계를 기반으로 합니다. 따라서 순위표 데이터는 읽기 전용를 직접 수정할 수 없습니다.

Xbox Live Unity 플러그 인 순위표 게임에서 구현 하는 방법을 이해 하는 데 사용할 수 있는 샘플 순위표 프리 팹을 제공 합니다.

순위표에 대 한 자세한 내용은 [순위표](../leaderboards-and-stats-2017/leaderboards.md)를 참조 하세요.

## <a name="using-the-leaderboard-prefabs"></a>순위표 프리 팹을 사용 하 여

Xbox Live Unity 플러그 인 순위표에 대 한 두 프리 팹을 포함 되어 있습니다.

* 순위표: 순위표, 나타내는 순위표에서 값을 표시 하려면 간단한 UI를 포함 하는 개체입니다.
* LeaderboardEntry:는 순위표의 단일 행을 나타내는 개체.

장면으로 **순위표** 프리 팹을 끌어올 수 있습니다. Unity 관리자에서 다음과 같은 특성을 설정할 수 있습니다.

* 상태:의 통계 gameobject이이 순위표 연관 된 합니다.
* 순위표 유형: 순위표 항목에 대 한 반환 되어야 하는 결과의 범위.
* 항목 수: 페이지당 표시할 데이터의 행의 수입니다.

> [!NOTE]
> 순위표 프리 팹의 시작 부분 처음 비어 있습니다. 테스트를 위해 gameobject 슬롯에 위에서 언급 한 통계 프리 팹 중 하나를 끌어 해 보세요.

Unity 편집기에서 **순위표** 프리 팹 관리자 설정에 관계 없이 동일한 모의 데이터를 항상 표시 됩니다. 빌드 및 실제 데이터 값을 확인 하는 권한 있는 사용자가 로그인을 Visual Studio 프로젝트 내보내기를 수행 해야 합니다. 자세한 내용은 [Unity에서 Xbox Live 구성](configure-xbox-live-in-unity.md)을 참조 하세요.

## <a name="see-also"></a>참고 항목

* [Unity에서 로그인 Xbox Live](unity-prefabs-and-sign-in.md)
* [구성 Xbox Live Unity에서](configure-xbox-live-in-unity.md)
* [점수 판 예제 장면](setup-leaderboard-example-scene.md)
* [순위표 데이터 가져오기](unity-leaderboard-from-scratch.md)
