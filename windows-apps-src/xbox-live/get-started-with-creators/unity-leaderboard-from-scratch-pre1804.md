---
title: Unity에서 Leaderboard 빌드
description: Unity에서 고유한 순위표 빌드 가이드
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity, 순위표
ms.openlocfilehash: a62cb2b3bd4afebcc7aa9060db60ac167f052977
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657828"
---
# <a name="leaderboards-data-in-unity"></a>Unity에서 순위표 데이터

사용자 지정 순위표 환경을 원하는 사용자의 경우에 대해이 문서에서는 제공 됩니다 Unity 개발자에 게 사용할 수 있는 Api를 통해 이동 하 여 사용자 고유의 순위표를 구현 하는 도구. 순위표 데이터 풀다운합니다 하는 방법을 이해 하 고 나면 선택한의 사용자 인터페이스에 적용할 수입니다.

[!IMPORTANT]
> 이 문서 (1804 릴리스)를 2018 년 5 월에 대 한 업데이트 하기 전에 플러그 인 버전에 적용 됩니다. 해당 시간 이후에 Xbox Live 플러그 인을 설치 하거나 아직 다운로드 하지 않은 경우에 순위표 데이터를 수집 하려면 다른 호출을 사용 하는 최신 버전을 해야 합니다. 이 외에도이 플러그 인의 스크린샷에서 최신 버전의 정의와 일치 하지 않습니다를 찾을 수 있습니다. 대신 하세요 합니다 [스크래치 문서에서 최신 unity 순위표](unity-leaderboard-from-scratch.md)합니다.


## <a name="call-for-leaderboard-data"></a>순위표 데이터에 대 한 호출

호출 해야 Xbox Live 서비스에서 순위표 데이터를 요청.  `void GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)`

성공적으로 획득 해야 하는이 호출을 수행 하는 `XboxLiveUser` 에서 [로그인](unity-prefabs-and-sign-in.md)가 [stat 구성](add-stats-and-leaderboards-in-unity.md) 하나 이상의 플레이어 및 폼에 대 한 값을 사용 하 여를 `LeaderboardQuery`입니다. 로그인 사용자 또는 사용자 순위표에 대 한 통계를 초기화 해야 하는 방법을 이미 알 수 없는 경우 연결 된 문서를 읽을 수 있습니다. 초기화 하 고 나면 통계 순위표 스크립트를 사용 하 여 연결 하는 가장 쉬운 방법은 포함 하는 것 통계 prefabs 중 하나: `IntegerStat`, `DoubleStat`, 또는 `StringStat` 공용 변수입니다. 에 stat 속성이 해당 ID에 최소한으로 구성 해야 합니다.이를 사용 하는 것에 대 한 합니다 **statName** 순위표 데이터에 대 한 호출 될 때 매개 변수입니다. 마지막으로 해야 폼을 `LeaderboardQuery` 개체입니다.
`LeaderboardQuery` 반환 되는 데이터에 적용 됩니다는 설정할 수 있는 몇 가지 특성이 있습니다.

- **StatName(Required):** 통계 ID 값으로 설정 되지 않은 설정 되지 않은 경우에 순위표에 대 한 데이터를 검색 하는 상태 ID를이 데이터가 반환 됩니다.
- **SocialGroup:** 이 문자열의 값에 따라 반환 된 순위표 데이터를 필터링 됩니다 친구, 즐겨 찾는 친구 또는 설정 되지 않은 경우에는 필터링 되지 않은 전체 순위표를 검색 합니다. 값 ""는 필터링 되지 않은 목록에서 "즐겨찾기" 친구 들과 목록에는 친구 들과 즐겨 찾는 있는 목록을 반환 합니다 반환 하는 데는 "all" 값을 반환 됩니다.
- **SkipResultToRank:** 설정이 uint 변수는 결정 순위표 데이터 순위 항목 반환 하는 경우 사용 하 여 시작 됩니다. 순위는 1 순위부터 시작합니다.
- **SkipResultToMe:** 설정이 부울 값을 true로 인해 시작에 반환 되는 순위표 데이터를 `XboxLiveUser` 에 사용 되는 여 `GetLeaderboard()` 호출 합니다.
- **배포 순서:** 열거형 형식의 `Microsoft.Xbox.Services.Leaderboard.SortOrder` 에 오름차순 이나 내림차순으로 두 개의 가능한 값입니다. 쿼리에 대 한이 변수를 설정 하면 순위표의 정렬 순서를 결정 됩니다. 기본적으로 순위표 내림차순 데이터를 반환 합니다.
- **MaxItems:** 호출당 반환할 행의 최대 수를 결정 하는이 uint `GetLeaderboard()`합니다.

프로그램 LeaderboardQuery 형성 다음과 같을 수 있습니다.

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            StatName = stat.ID,
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

이 쿼리를 사용 하는 반환는 100부터 순위표의 5 개 행 순위 지정 된 상태에 대 한 개별 합니다.

> [!WARNING]
> 플레이어는 순위표 내에 포함 된 개수 보다 더 높은 SkipResultToRank를 설정 순위표 반환할 데이터를 0 개 행을 사용 하 여 수행 됩니다.

호출할 수 있으므로 모든 부분이 함께 `GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)` 함수를 사용 하 여이 서명 된에서 `XboxLiveUser` (가능성이 `XboxLiveUserInfo.User`)에서 로그인이 사용자로 가져온 및 `LeaderboardQuery` 우리의 쿼리로 방금 만든 합니다.

함수를 호출 하 여 순위표 다음과 같을 수 있습니다.

```csharp
using Microsoft.Xbox.Services.Leaderboard;

public void LoadLeaderboard()
    {
        // check to make sure you have a valid stat
        if (this.stat == null)
        {
            // TO DO: Display "Stat not specified" error message!
            return;
        }

        // check to make sure you have an XboxLiveUser
        if (this.xboxLiveUser == null)
        {
            if (XboxLiveUserManager.Instance.UserForSingleUserMode != null
                && XboxLiveUserManager.Instance.UserForSingleUserMode.User != null)
            {
                // set the XboxLiveUser if one is available
                this.xboxLiveUser = XboxLiveUserManager.Instance.UserForSingleUserMode.User;
            }
            else // if you don't have a user signed in display an error message and exit. 
            {
                // TO DO: Display "User Not logged In" error message
                return;
            }
        }

        LeaderboardQuery query;

        query = new LeaderboardQuery
        {
            StatName = this.stat.ID,
            MaxItems = this.maxItems,

        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
    }
```

이제 했을 수 있는 함수를 검색 하는 순위표 `GetLeaderboard()` void를 반환 하 고 따라서 우리가 찾고 순위표 데이터를 반환 하지 않습니다. 실제로 다음 섹션에서 설명 하는 이벤트 함수에서 순위표 데이터를 가져오게 됩니다 것입니다.

## <a name="receive-the-leaderboard-data"></a>순위표 데이터 수신

수신 함수를 추가 해야 순위표 데이터를 검색 하기 위해는 `StatsManagerComponent` 을 제목에 대 한 인스턴스. 코드를 다음 줄을 추가 해야 합니다 `Awake()` 코드의 함수: `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`합니다.

```csharp
private void Awake()
    {
        this.EnsureEventSystem();
        XboxLiveServicesSettings.EnsureXboxLiveServicesSettings();

        StatsManagerComponent.Instance.GetLeaderboardCompleted += this.GetLeaderboardCompleted;
    }
```

`StatsManagerComponent` 순위표 완료 이벤트를 수신 합니다. 이 코드 줄을 실행 하 여 순위표 완료 이벤트가 발생할 때 호출할 함수 목록에는 함수를 추가 합니다. 해당 함수가 호출 되어이 예에서 `MyGetLeaderBoardCompletedFunction()`, 사용자 고유의 스크립트에서 원하는 만큼 함수 이름을 지정할 수 있습니다. "MyGetLeaderboardCompletedFunction" 두 매개 변수는 보낸 사람을 나타내는 개체를 수행 해야 하는 함수 및 `Microsoft.Xbox.Services.Client.StatEventArgs` 매개 변수입니다. 셸은 함수의 코드는 다음과 같을 수 있습니다.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

가장 먼저 수행 해야이 함수는에서 찾을 수 있는 오류를 확인 합니다 `StatEventArgs` statArgs 매개 변수입니다. StatArgs 포함을 `StatisticEvent` 호출을 포함 하는 EventData는 `System.Exception` ErrorInfo를 호출 합니다. 순위표 데이터를 검색할 때는 오류가 발생 한 경우 statArgs.EventData.ErrorInfo에서 찾을 수 있습니다. 간단한 경우에는 추가할 수 있습니다 오류를 확인 하는 이전 코드에는 문입니다.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }
    }
```

오류가 있는지를 확인 한 후에 있는 순위표 요청의 결과 저장할 `statArgs.EventData.EventArgs.Result`합니다. `Result` 가 `LeaderBoardResult` 에 순위표를 채우는 데 필요한 데이터를 포함 하는 개체입니다. 예제 코드에서이 데이터를 추출 하 고 LoadResult를 호출 하는 다른 함수에 보냅니다.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

합니다 `LeaderboardResult` LoadResult 함수에 전송 결과 반환 된 순위표 데이터 읽기와 아직 원래 호출에서 반환 하는 순위를 검색 하는 추가 호출을 수행 해야 하는 모든 데이터를 갖습니다. 순위표 스크립트에 대 한 클래스 변수에 결과 저장 하려는 다음과 같이 합니다.

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

이 중요 하기 때문에 `LeaderboardResult` 검색할 수 있는 순위표의 이후 섹션 인지 여부를 결정 하는 HasNext 속성을 포함 합니다. `LeaderboardResult` 도 순위표를 구성 하는 행의 합계에 대 한 변수를 저장 합니다. 이러한 속성에 순위표를 탐색 해야 됩니다. 데이터를 프로그램 `LeaderBoardResult` 구현 하기만 하면 됩니다을 사용 하 여 루프에 대 한는 `LeaderboardResults` 목록을 `LeaderboardRow` 호출 `Rows`합니다. 샘플 코드에서 단순히 하겠습니다 각각의 값을 연결할 `LeaderboardRow` 문자열로 표시할 수 있습니다.

```csharp
using Microsoft.Xbox.Services.Leaderboard

void LoadResult(LeaderboardResult result)
{

    this.leaderboardData = result;

    this.leaderBoardText.text = "" // leaderBoardText is a UnityEngine.UI text box.

    foreach (LeaderboardRow row in this.leaderboardData.Rows)
    {
        leaderBoardText.text += string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", row.Rank, row.Gamertag, this.stat.DisplayName, row.Values[0]);
    }
}
```

예에서 차수와 게이머 태그, 값 속성을 사용 했습니다 `LeaderBoardResult` 겠죠와 관련 된 통계의 DisplayName 뿐만 아니라이 문자열을 채우려면.

물론 모든 순위표 데이터를 사용 하 여 창의적인 기능을 수행할 수 있습니다.

## <a name="navigating-the-leaderboard-data"></a>순위표 데이터 탐색

가장 일반적인 경우에에서 로드 되지 않습니다 모든 단일 순위에 순위표를 한 번에 하 고 사용자에 대 한 순위표의 다른 섹션에 표시할 순위를 탐색할 수 있게 해야 합니다. 1-백 플레이어 순위를 사용 하 여 leaderboard 해야 한다고 가정해 보겠습니다. 초기 호출에서 `GetLeaderboard()` 10 개를 검색 `LeaderboardRows` 플레이어에 대 한 표시 해야 합니다. 플레이어는 상위 10 명의 플레이어 보다 더 보려고 할 수 있습니다. 10 명의 사용자가 다음 집합을 얻을 수 있는 가장 쉬운 방법은 결정 하는 여부는 `LeaderboardResult` 마지막에서 저장 된 쿼리에 많은 검색할 행 및 호출 `GetLeaderboard()` 해당 LeaderboardResult를 사용 하 여 `NextQuery` 속성. 다음 코드는 다음 순위표 데이터 집합을 검색 합니다.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.NextQuery;
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
        }
        else
        {
            //TO DO: Display an error message or go back to the beginning of the leaderboard as the situation demands.
        }
    }
```

프로그램 순위표를 뒤로 이동 하는 것은 이전에 순위표에서 순위 수가 X 끌어오기 함수가 없기 때문에 좀 더 어렵습니다. 이전 순위를 검색 하기 위해 사용자 고유의 논리를 작성 해야 합니다. 한 가지 방법은 저장 하는 것에 `MaxItems` 당 `LeaderboardQuery` 사용 하 여 이동 해야 하는 순위를 계산 하 고는 `SkipToRank` 특성에 `LeaderboardQuery`입니다. 해당 코드는 코드는 다음과 같을 수 있습니다.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetPreviousLeader()
{
    if(leaderboardData == null || leaderboardData.Rows.Count < 1)
    {
        return;
    }

    uint topRank = leaderboardData.Rows[0].Rank; //get the first rank of the leaderboard.
    uint targetRank = topRank - this.maxItems > 0 ? topRank - this.maxItems : 0; //set your targetRank equal to the current top rank of your leaderboard minus the configured number of rows to display at a time.

    LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
    {
        StatName = stat.ID,
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
}
```

최종 가장 일반적인 시나리오는 플레이어가 겠죠에 해당 위치를 표시 하려는 경우도입니다. 호출 하 여 쉽게 이렇게 합니다 `GetLeaderboard()` 쿼리를 사용 하 여 함수 여기서는 `SkipResultToMe` 특성이 설정 되어 true로 합니다.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

    void GetRankForTag()
    {

        LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
        {
            StatName = stat.ID,
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
    }
```

순위표는 자세한 예제를 보려는 경우 항상 자산 아래의 XboxLive 플러그 인 폴더에 Leaderboard.cs 스크립트를 읽을 수 있습니다 >> XboxLive >> 스크립트 >> Leaderboard.cs 합니다.