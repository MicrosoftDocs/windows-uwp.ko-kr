---
title: 순위표
description: Xbox Live 순위표를 사용 하 여 플레이어를 비교 하는 방법에 알아봅니다.
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.date: 09/28/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8fd7e30b99418fda614a888d9269548cdc57a88a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662028"
---
# <a name="leaderboards"></a>순위표

## <a name="introduction"></a>소개

에 설명 된 대로 [데이터 플랫폼 개요](../data-platform/data-platform.md), 순위표는 경쟁이 플레이어를 독려 및 플레이어 참여의 이전 최상의 점수와 친구 들의 비트를 유지 하는 훌륭한 방법입니다.

에 대 한 순위표 [주요 통계](stats2017.md#configured-stats-and-featured-leaderboards) 항상 타이틀의 게임 허브에 표시 되어 홈 페이지에 고정 되 면 경우에 따라 제목에 대 한 UI의 일부로 표시 합니다. 또한 프로그램 제목 내의 순위표를 만들려는 구성된에 주요 통계를 사용할 수 있습니다.

## <a name="choosing-good-leaderboards"></a>좋은 순위표를 선택합니다.

에 설명 된 대로 [플레이어 통계](player-stats.md), 순위표 정의 된 상태에 해당 합니다.  플레이어를 향상 시키는 사용할 수 있는 실적에 해당 하는 순위표를 선택 해야 합니다.

예를 들어 레이싱 게임 최상의 랩 시간은 플레이어 최상의 랩 시간을 향상 시키는 작업 하려고 하기 때문에 좋은 leaderboard 않습니다.  다른 예는 전투 게임에서 shooters, 또는 최대 콤보 크기에 대 한 종료/사망 비율입니다.

## <a name="when-to-display-leaderboards"></a>순위표를 표시 하는 경우

순위표를 제목에서 언제 든 지 표시할 수가 있습니다.  Leaderboard 플레이 또는 흐름을 제목을 사용 하 여 방해 하지는 경우 한 번을 선택 해야 합니다.  일치 항목이 모두 좋은 시간 후 및 반올림 사이입니다.

## <a name="how-to-display-leaderboards"></a>순위표를 표시 하는 방법

Xbox Live SDK에서 제공 하는 순위표를 표시 하기 위한 다양 한 옵션이 있습니다.  Unity를 Xbox Live 크리에이터 스 프로그램을 사용 하 여 사용 하는 경우 순위표 데이터를 표시할 순위표 Prefab을 사용 하 여 시작할 수 있습니다.  참조 된 [Unity에서 Xbox Live 구성](../get-started-with-creators/configure-xbox-live-in-unity.md) 기사입니다.

Xbox Live SDK에 대 한 직접 코딩은 다음에 읽을 경우 사용할 수는 Api에 알아봅니다.

## <a name="programming-guide"></a>프로그래밍 가이드

여러 순위표 Api leaderboard의 현재 상태를 가져오는 데 사용할 수 있습니다.  모든 Api는 비동기 이며 차단 하지 않습니다.  순위표 데이터 가져오기 및 일반적인 게임 처리를 계속 하는 요청을 해야 합니다.  순위표 결과 서비스에서 반환 된 경우에 적절 한 시간에 결과 표시할 수 있습니다.

요청 해야 순위표 데이터 서비스에서 약간 앞 플레이어 표시할 겠죠 기다리는 차단 되지 않도록 표시 하려는 경우.

## <a name="leaderboards-2013-apis"></a>순위표 2013 Api

보시는 `leaderboard_service` 모든 통계 2013 순위표 API에 대 한 네임 스페이스입니다.

<table>

<tr>
<td>C++ API</td><td>설명</td>
</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
        const string_t& scid,
        const string_t& name
        );
```

</td>

<td>API의 가장 기본적인 버전입니다.  순위표의 맨 위에 있는 플레이어에서 시작 하는 지정 된 순위표에 대 한 순위표 값을 반환 합니다.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
        _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName
        ) 
```

</td>

<td>WinRT C# 코드-순위표 이름과 서비스 구성 ID를 지정 하는 단일 순위표에 대 한 순위표를 가져옵니다.</td>

</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ uint32_t skipToRank,
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>이 API는 더 많은 유연성이 제공, 순위 (위치)를 표시할 뿐만 아니라 반환할 항목의 최 댓 값을 지정할 수 있습니다.  예를 들어 1000 위치부터 순위표를 표시 하려는 경우이 API를 사용 하면 됩니다.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_ uint32 skipToRank,
         _In_ uint32 maxItems
        ) 
```

</td>

<td>WinRT C# 코드-단일 leaderboard 제공 서비스 구성 ID 및 순위표 이름이 결과 "skipToRank" 순위에 시작 됩니다 순위표에 대 한 순위표 결과 페이지를 가져옵니다.</td>

</tr>

<tr>

<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard_skip_to_xuid(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ const string_t& skipToXuid = string_t(),
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>

특정 사용자에 게는 순위표를 건너 뛰 려는 경우이 사용 합니다.  `XUID` 각 Xbox 사용자에 대 한 고유 식별자입니다.  로그인된 한 사용자에 게 친구 중 하나가 가져올 수 있으며이 함수에 전달.

</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardWithSkipToUserAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_opt_ Platform::String^ skipToXboxUserId,
         _In_ uint32 maxItems
        )
```

</td>

<td>WinRT C# 코드-플레이어의 순위 또는 플레이어의 백분위 수 등급에 따라 순서가 지정 된 점수에 관계 없이 지정 된 플레이어부터 leaderboard 가져오기</td>

</tr>

</table>

## <a name="2013-c-example"></a>C + + 2013 예제

C + + API 계층을 사용 하는 경우 서비스에서 순위표 결과가 반환 되 면 호출 될 콜백을 설정할 수 있습니다.  아래 예를 살펴보겠습니다.

익숙한 없는 경우는 `pplx::task` 이러한 Api에서 반환 되 고,이 비동기 작업 개체에서 Microsoft 프로그래밍 라이브러리 PPL (병렬).  이 대해 자세히 알아볼 수 있습니다 [ https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks ](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks)합니다.

아래 섹션에는 순위표 결과 검색 하 고 사용 될 수 있습니다 하는 방법을 보여 줍니다.

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. 순위표 결과 검색 하는 비동기 작업 만들기

먼저 특정 순위표에 대 한 결과 검색 하기 위해 순위표 서비스를 호출 하는 것입니다.

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2. 콜백을 설정 합니다.

설정할 수 있습니다는 [연속 작업](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations) 순위표 결과가 반환 되 면 호출 됩니다.  이렇게 하면 다음과 같이 아래.

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

이 연속 작업을 호출 하 고 받는 원래 개체의 컨텍스트에서 라고는 ```leaderboard_result``` 을 제목 적합 한 방식으로 표시할 수 있습니다.


### <a name="3-display-leaderboard"></a>3. 순위표 표시

순위표 데이터에 포함 된 ```leaderboard_result``` 필드 자체 설명 되며 합니다.  예를 들어 아래를 참조 하세요.

```cpp
auto leaderboard = result.payload();

for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
{
    string_t colValues;
    for (auto columnValue : row.column_values())
    {
        colValues = colValues + L" ";
        colValues = colValues + columnValue;
    }
    m_console->Format(L"%18s %8d %14f %10s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
}

```

## <a name="2013-winrt-c-example"></a>2013 WinRT C# 예제

WinRT를 사용 하는 경우 C# 계층을 별도 콜백 작업 하 고 사용할 필요가 단순히 확인 해야 합니다는 `await` 순위표 서비스를 호출할 때 키워드입니다.

### <a name="1-access-the-leaderboardservice"></a>1. 액세스는 LeaderboardService

`LeaderboardService` 에서 검색할 수 있습니다는 `XboxLiveContext` 게임으로 사용자를 로그인 할 때 만든 해야 순위표 데이터에 대 한 호출 되도록 합니다.

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2. LeaderboardService 호출

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3. 순위표 데이터 검색

`GetLeaderboardAsync()` 반환 된 `LeaderboardResult` 명명된 순위표를 채우는 통계가 포함 됩니다.

`LeaderboardResult` 여러 함수 및 순위표 데이터 읽기를 용이 하 게 하는 속성에 있습니다.

|속성  |설명  |
|---------|---------|
|공용 IAsyncOperation<LeaderboardResult> GetNextAsync (uint maxItems);     |MaxItems 매개 변수 개수까지 순위 다음 집합을 검색합니다. 이 기본적으로 다른 호출 `GetLeaderboard()`         |
|public LeaderboardQuery GetNextQuery();     |다음 데이터 집합을 검색할 순위표 호출에 사용할 수 있는 LeaderboardQuery를 검색 합니다.         |
|public bool HasNext { get; }    |행이 더 순위표를 검색 하는 여부를 지정 합니다.         |
|public IReadOnlyList<LeaderboardRow> Rows { get; }     | 순위 당 순위표 데이터가 포함 된 행        |
|공용 IReadOnlyList<LeaderboardColumn> 열 {get;}     | 순위표를 구성 하는 열 목록        |
|public uint TotalRowCount { get; }     | 순위표에 대 한 행의 총 크기        |
|public string DisplayName { get; }     | 순위표에 대 한 표시 이름       |

순위표 데이터 한 페이지 한 번에 제공 됩니다. 반복할 수 있습니다는 `LeaderboardResult` 행과 열 데이터를 검색 합니다.  
사용 합니다 `HasNext` 부울 및 `GetNextAsync()` 순위표 데이터의 뒷부분에 나오는 페이지를 검색 하는 함수입니다.

```csharp
if (boardResult != null)
{
    foreach (LeaderboardRow row in boardResult.Rows)
    {
        Debug.Write(string.Format("Rank: {0} | Gamertag: {1} | {2}\n", row.Rank, row.Gamertag, row.Values.ToString()));
    }
}
```

## <a name="leaderboard-2017"></a>순위표 2017

사용 하 여 통계 2017 순위표 서비스를 호출 하는 `StatisticManager` 순위표 api 대신는 `LeaderboardService` 순위표 api.  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_leaderboard (
     _In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ leaderboard::leaderboard_query query
     ) 
```  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_social_leaderboard (_In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ const string_t &socialGroup,
     _In_ leaderboard::leaderboard_query query
)
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetLeaderboard`  

```csharp
public void GetLeaderboard(
    XboxLiveUser user,
    string statName,
    LeaderboardQuery query
    )
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetSocialLeaderboard`  

```csharp
public void GetSocialLeaderboard(
    XboxLiveUser user,
    string statName,
    string socialGroup,
    LeaderboardQuery query
    )
```  

## <a name="2017-c-example"></a>C + + 2017 예제

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1. stats_manager의 Singleton 인스턴스 가져오기

호출 하기 전에 `stats_manager` 함수 Singleton 인스턴스를 변수에 설정 해야 합니다.

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2. LeaderboardQuery 만들기

`leaderboard_query` 금액을 순서에 따라 결정 됩니다 및 순위표 호출에서 반환 된 데이터의 시작점입니다.

`leaderboard_query` 반환 되는 데이터에 적용 됩니다는 설정할 수 있는 몇 가지 특성이 있습니다.

|속성 |설명  |
|---------|---------|
|m_skipResultToRank     |이 uint 변수를 반환할 때 사용 하 여 시작 됩니다 순위 순위표 데이터 항목을 결정 합니다. 순위는 1 순위부터 시작합니다.         |
|m_skipResultToMe     |설정이 부울 값을 true로 인해 시작에 반환 되는 순위표 데이터를 `XboxLiveUser` 사용을 `get_leaderboard()` 호출 합니다.  |
|m_order     |열거형 형식의 `xbox::services::leaderboard::sort_order` 에 오름차순 이나 내림차순으로 두 개의 가능한 값입니다. 쿼리에 대 한이 변수를 설정 하면 순위표의 정렬 순서를 결정 됩니다.        |
|m_maxItems     |호출당 반환할 행의 최대 수를 결정 하는이 uint `get_leaderboard` 또는 `get_social_leaderboard()`합니다.         |

`leaderboard_query` 이러한 속성에 값을 할당 하 여 여러 집합 함수가 있습니다. 다음 코드에서는 설정 하는 방법에 `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

이 쿼리는 반환은 100부터 순위표의 10 개 행 순위 개별 합니다.

> [!WARNING]
> 플레이어는 순위표 내에 포함 된 개수 보다 더 높은 SkipResultToRank를 설정 순위표 반환할 데이터를 0 개 행을 사용 하 여 수행 됩니다.

### <a name="3-call-getleaderboard"></a>3. Get_leaderboard 호출

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName` 에 사용 되는 `GetLeaderboard()` 호출을 stat에 제목에 대 한 구성의 이름과 동일 해야 [파트너 센터](https://partner.microsoft.com/dashboard), 대/소문자 구분입니다.

### <a name="4-read-the-leaderboard-data"></a>4. 순위표 데이터 읽기

호출 해야 순위표 데이터를 읽기 위해 합니다 `stats_manager::do_work()` 목록을 반환 하는 함수 `stat_event` 값입니다. 순위표 데이터에 포함 되어야 합니다는 `stat_event` 형식의 `stat_event_type::get_leaderboard_complete`합니다. 목록에서이 형식의 이벤트에서 제공 하는 경우 `stat_event`s를 통해 확인할 수 있습니다 합니다 `leaderboard_result` 에 포함 된를 `stat_event` 데이터에 액세스할 수 합니다.

샘플 `do_work()` 처리기

```cpp
void Sample::UpdateStatsManager()
{
    // Process events from the stats manager
    // This should be called each frame update

    auto statsEvents = m_statsManager->do_work();
    std::wstring text;

    for (const auto& evt : statsEvents)
    {
        switch (evt.event_type())
        {
            case stat_event_type::local_user_added: 
                text = L"local_user_added"; 
                break;

            case stat_event_type::local_user_removed: 
                text = L"local_user_removed"; 
                break;

            case stat_event_type::stat_update_complete: 
                text = L"stat_update_complete"; 
                break;

            case stat_event_type::get_leaderboard_complete: //leaderboard data is read here
                text = L"get_leaderboard_complete";
                auto getLeaderboardCompleteArgs = std::dynamic_pointer_cast<leaderboard_result_event_args>(evt.event_args());
                ProcessLeaderboards(evt.local_user(), getLeaderboardCompleteArgs->result());
                break;
        }

        stringstream_t source;
        source << _T("StatsManager event: ");
        source << text;
        source << _T(".");
        m_console->WriteLine(source.str().c_str());
    }
}
```

순위표 결과로 순위표 데이터 읽기  

```cpp
void Sample::PrintLeaderboard(const xbox::services::leaderboard::leaderboard_result& leaderboard)
{
    if (leaderboard.rows().size() > 0)
    {
        m_console->Format(L"%16s %6s %12s %12s\n", L"Gamertag", L"Rank", L"Percentile", L"Values");
    }

    for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
    {
        string_t colValues;
        for (auto columnValue : row.column_values())
        {
            colValues = colValues + L" ";
            colValues = colValues + columnValue;
        }
        m_console->Format(L"%16s %6d %12f %12s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
    }
}
```  

겠죠에서 추가 데이터 페이지를 검색 합니다.  

```cpp
void Sample::ProcessLeaderboards(
    _In_ std::shared_ptr<xbox::services::system::xbox_live_user> user,
    _In_ xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result
    )
{
    if (!result.err())
    {
        auto leaderboardResult = result.payload();
        PrintLeaderboard(leaderboardResult);

        // Keep processing if there is more data in the leaderboard
        if (leaderboardResult.has_next())
        {
            if (!leaderboardResult.get_next_query().err())
            {               
                auto lbQuery = leaderboardResult.get_next_query().payload();
                if (lbQuery.social_group().empty())
                {
                    m_statsManager->get_leaderboard(user, lbQuery.stat_name(), lbQuery);
                }
                else
                {
                    m_statsManager->get_social_leaderboard(user, lbQuery.stat_name(), lbQuery.social_group(), lbQuery);
                }
            }
        }
    }
}
```  

## <a name="2017-winrt-c-example"></a>2017 WinRT C# 예제

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1. StatisticManager의 singleton 인스턴스 가져오기

호출 하기 전에 `StatisticManager` 함수 Singleton 인스턴스를 변수에 설정 해야 합니다.

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2. LeaderboardQuery 만들기

`LeaderboardQuery` 금액을 순서에 따라 결정 됩니다 및 순위표 호출에서 반환 된 데이터의 시작점입니다.  

```csharp
public sealed class LeaderboardQuery : __ILeaderboardQueryPublicNonVirtuals
    {
        [Overload("CreateInstance1")]
        public LeaderboardQuery();

        public bool HasNext { get; }
        public string SocialGroup { get; }
        public string StatName { get; }
        public SortOrder Order { get; set; }
        public uint MaxItems { get; set; }
        public uint SkipResultToRank { get; set; }
        public bool SkipResultToMe { get; set; }
    }
```

`LeaderboardQuery` 반환 되는 데이터에 적용 됩니다는 설정할 수 있는 몇 가지 특성이 있습니다.

|속성 |설명  |
|---------|---------|
|SkipResultToRank     |이 uint 변수를 반환할 때 사용 하 여 시작 됩니다 순위 순위표 데이터 항목을 결정 합니다. 순위는 1 순위부터 시작합니다.         |
|SkipResultToMe     |설정이 부울 값을 true로 인해 시작에 반환 되는 순위표 데이터를 `XboxLiveUser` 사용을 `GetLeaderboard()` 호출 합니다.  |
|Order     |열거형 형식의 `Microsoft.Xbox.Services.Leaderboard.SortOrder` 에 오름차순 이나 내림차순으로 두 개의 가능한 값입니다. 쿼리에 대 한이 변수를 설정 하면 순위표의 정렬 순서를 결정 됩니다.        |
|MaxItems     |호출당 반환할 행의 최대 수를 결정 하는이 uint `GetLeaderboard()` 또는 `GetSocialLeaderboard()`합니다.         |

형성에 `LeaderboardQuery` 다음 처럼 보일 수 있습니다.

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

이 쿼리는 반환 된 100부터 순위표의 5 개 행 순위 개별 합니다.

> [!WARNING]
> 플레이어는 순위표 내에 포함 된 개수 보다 더 높은 SkipResultToRank를 설정 순위표 반환할 데이터를 0 개 행을 사용 하 여 수행 됩니다.

### <a name="3-call-getleaderboard"></a>3. Call GetLeaderboard()

이제 호출할 수 있습니다 `GetLeaderboard()` 사용 하 여 프로그램 `XboxLiveUser`, 프로그램 통계의 이름 및 `LeaderboardQuery`합니다.

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName` 에 사용 되는 `GetLeaderboard()` 호출을 stat에 제목에 대 한 구성의 이름과 동일 해야 [파트너 센터](https://partner.microsoft.com/dashboard), 대/소문자 구분입니다.

### <a name="4-read-leaderboard-data"></a>4. 읽기 순위표 데이터

호출 해야 순위표 데이터를 읽기 위해 합니다 `StatisticManager.DoWork()` 목록을 반환 하는 함수 `StatisticEvent` 값입니다. 순위표 데이터에 포함 되어야 합니다는 `StatisticEvent` 형식의 `GetLeaderboardComplete`합니다. 목록에서이 형식의 이벤트에서 제공 하는 경우 `StatisticEvent`s를 통해 확인할 수 있습니다 합니다 `LeaderboardResult` 에 포함 된를 `StatisticEvent` 데이터에 액세스할 수 합니다.

```csharp
IReadOnlyList<StatisticEvent> statEvents = statManager.DoWork(); //In practice this should be called every update frame

foreach(StatisticEvent statEvent in statEvents)
{
    if(statEvent.EventType == StatisticEventType.GetLeaderboardComplete
        && statEvent.ErrorCode == 0)
    {
        LeaderboardResultEventArgs leaderArgs = (LeaderboardResultEventArgs)statEvent.EventArgs;
        LeaderboardResult leaderboardResult = leaderArgs.Result;
        foreach(LeaderboardRow leaderRow in leaderboardResult.Rows)
        {
            Debug.WriteLine(string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", leaderRow.Rank, leaderRow.Gamertag, "test", leaderRow.Values[0]));
        }
    }
}
```

제목 코드에서 `StatisticManager.DoWork()` 들어오는 모든 통계 관리자 이벤트를 처리 및 순위표에 대 한 뿐만 아니라 사용 해야 합니다. 

> [!NOTE]
> 검색 하기 위해 합니다 `LeaderboardResultEventArgs` 캐스팅 해야 합니다는 `StatisticEvent.EventArgs` 으로 `LeaderboardResultEventArgs` 변수입니다.

### <a name="5-retrieve-more-leaderboard-data"></a>5. 더 많은 순위표 데이터를 검색 합니다.

순위표는 데 필요한 데이터를 사용 하 여의 뒷부분에 나오는 페이지를 검색 하기 위해 합니다 `LeaderboardResult.HasNext` 속성 및 `LeaderboardResult.GetNextQuery()` 검색을 위한 함수를 `LeaderboardQuery` 그러면 표시 하면 데이터의 다음 페이지입니다.

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```