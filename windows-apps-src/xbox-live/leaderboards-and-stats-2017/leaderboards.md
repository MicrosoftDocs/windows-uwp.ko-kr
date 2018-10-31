---
title: 순위표
author: aablackm
description: 순위표 Xbox Live를 사용 하 여 플레이어를 비교 하는 방법을 알아봅니다.
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.author: aablackm
ms.date: 09/28/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25b0b16963147328a7ababf58634bcd9c134637a
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5822353"
---
# <a name="leaderboards"></a>순위표

## <a name="introduction"></a>소개

[데이터 플랫폼 개요](../data-platform/data-platform.md)에서 설명한 순위표는 자신의 이전 최고 점수 뿐만 아니라 친구의 경쟁 하려고에 참여 하는 플레이어, 플레이어, 간의 경쟁 것을 권장 하는 훌륭한 방법입니다.

[주요](stats2017.md#configured-stats-and-featured-leaderboards) 통계 순위표 타이틀의 게임 허브에 항상 표시 되며 홈 페이지에 고정 된 경우에 따라 제목에 대 한 UI의 일부로 표시 됩니다. 구성 된 기능을 갖춘 통계를 사용 하 여 순위표 타이틀 내부에서 만들 수 있습니다.

## <a name="choosing-good-leaderboards"></a>좋은 순위표 선택

[플레이어 통계](player-stats.md)에서 설명 했 듯이 순위표 정의 된 상태에 해당 합니다.  플레이어를 개선 하는 방향으로 작동할 수 있는 실적에 해당 하는 순위표를 선택 해야 합니다.

예를 들어, 레이싱 게임에 가장 적합 한 랩 시간 있다는 점에서 좋은 순위표, 플레이어의 가장 적합 한 랩 시간을 개선 하는 방향으로 작동 합니다.  다른 예로 전투 게임에서 인치 슈팅, 또는 최대 콤보 크기에 대 한 Kill/데스 비율 있습니다.

## <a name="when-to-display-leaderboards"></a>순위표 표시 하는 경우

타이틀에서 언제 든 지 순위표를 표시할 수가 있습니다.  순위표 플레이 또는 흐름 타이틀을 방해 하지 않으므로 경우 시간을 선택 해야 합니다.  라운드 사이 그리고 후 좋은 경우가 두 일치 합니다.

## <a name="how-to-display-leaderboards"></a>순위표 표시 하는 방법

Xbox Live SDK에서 제공 하는 순위표를 표시 하기 위한 다양 한 옵션이 있습니다.  Xbox Live 크리에이터 스 프로그램을 사용 하 여 Unity를 사용 하는 경우 순위표 데이터를 표시 하는 순위표 Prefab을 사용 하 여 차별화할 수 있습니다.  자세한 내용은 [Unity에서 Xbox Live 구성](../get-started-with-creators/configure-xbox-live-in-unity.md) 문서를 참조 하세요.

직접, Xbox Live SDK에 대해 코딩 다시 읽어 사용할 수 있는 Api에 알아봅니다.

## <a name="programming-guide"></a>프로그래밍 가이드

여러 순위표 Api는 순위표의 현재 상태에 사용할 수 있습니다.  모든 Api는 비동기 및 차단 하지 않습니다.  순위표 데이터를 가져오고 계속 일반적인 게임 처리를 요청을 하는 것입니다.  순위표 결과, 서비스에서 반환 하는 경우에 적절 한 번에 결과 표시할 수 있습니다.

요청 해야 순위표 데이터 서비스에서 약간 보다 빠른 플레이어 순위표 표시 될 때까지 기다리는 차단 되지 않도록을 표시 하려는 경우.

## <a name="leaderboards-2013-apis"></a>순위표 2013 Api

표시는 `leaderboard_service` 모든 통계 2013 순위표 API에 대 한 네임 스페이스입니다.

<table>

<tr>
<td>C + + API</td><td>설명</td>
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

<td>API의 가장 기본적인 버전입니다.  이 순위표 맨 위에 있는 플레이어에서 시작 하 고 지정 된 순위표 순위표 값을 반환 됩니다.</td>

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

<td>WinRT C# 코드는 순위표를 서비스 구성 ID 및 순위표 이름을 지정 하는 단일 순위표 가져옵니다.</td>

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

<td>이 API는 일부 더 많은 유연성을 제공, 반환할 항목의 최대 값 뿐만 아니라, 표시할 순위 (위치)를 지정할 수 있습니다.  예를 들어 1000 위치부터 순위표 표시 하려는 경우이 API를 사용 합니다.</td>

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

<td>WinRT C# 코드에 단일 순위표에 대 한 결과 순위표의 페이지 Get 제공 서비스 구성 ID 및 순위표 이름, 순위표 결과 "skipToRank" 순위에서 시작 됩니다.</td>

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

특정 사용자에 게는 순위표 하지 않으려면 다음을 사용 합니다.  A `XUID` 는 각 Xbox 사용자에 대 한 고유 식별자.  로그인된 된 사용자 또는 친구 들 중 하나에 대 한 얻을 수 있으며이 함수에 전달 하는 수 있습니다.

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

<td>WinRT C# 코드에 플레이어의 순위에 관계 없이 지정 된 플레이어에서 시작 순위표 가져오기 또는 점수, 플레이어의 % 째 순위에 따라 정렬</td>

</tr>

</table>

## <a name="2013-c-example"></a>2013 c + + 예제

C + + API 계층을 사용 하는 경우 서비스에서 순위표 결과가 반환 되 면 호출에 대 한 콜백을 설정할 수 있습니다.  아래 예를 살펴봅니다.

익숙하지 않은 경우는 `pplx::task` 비동기 작업 개체에서 Microsoft 병렬 프로그래밍 라이브러리 (PPL)는 이러한 Api에서 반환 합니다.  에 대해 자세히 알아보려면 [https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks).

아래 섹션에는 순위표 결과 검색 하 고 세울 수 방법을 보여 줍니다.

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. 순위표 결과 검색 하는 비동기 작업 만들기

첫 번째 단계는 특정 순위표에 대 한 결과 검색 하 여 순위표 서비스를 호출 합니다.

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2. 콜백을 설정 합니다.

순위표 결과가 반환 되 면 호출 되는 [연속 작업](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations) 을 설정할 수 있습니다.  이렇게 하면 다음과 같이 아래 합니다.

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

원래 호출한 하 고 수신 하는 개체의 컨텍스트에서이 연속 작업 이라고 합니다 ```leaderboard_result``` 타이틀에 적합 한 방식으로 표시할 수 있습니다.


### <a name="3-display-leaderboard"></a>3. 순위표를 표시 합니다.

순위표 데이터에 포함 된 ```leaderboard_result``` 필드는 자체 설명 합니다.  예제는 아래를 참조 하세요.

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

별도 콜백 사용 하 여 간단 하 게 필요 하 고 작업을 위해 필요 하지는 WinRT C# 계층을 사용 하는 경우는 `await` 순위표 서비스를 호출할 때 키워드입니다.

### <a name="1-access-the-leaderboardservice"></a>1입니다. 액세스는 LeaderboardService

`LeaderboardService` 에서 검색할 수는 `XboxLiveContext` 게임 사용자가 로그인 할 때 생성 해야 순위표 데이터에 대 한 호출 합니다.

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2.는 LeaderboardService를 호출 합니다.

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3. 순위표 데이터를 검색 합니다.

`GetLeaderboardAsync()` 반환는 `LeaderboardResult` 명명 된 순위표 채우기 통계 들어갑니다.

`LeaderboardResult` 여러 가지 기능 및 순위표 데이터 읽기를 용이 하는 속성에 있습니다.

|속성  |설명  |
|---------|---------|
|공개 IAsyncOperation<LeaderboardResult> GetNextAsync (uint maxItems);     |다음 순위 번호가 maxItems 매개 변수 집합을 검색합니다. 이 기본적으로 다른 호출 `GetLeaderboard()`         |
|공용 LeaderboardQuery GetNextQuery();     |다음 데이터 집합을 검색할 순위표 호출할 수 있도록 사용 될 수 있는 LeaderboardQuery를 검색 합니다.         |
|고객 요청 HasNext {get;}    |검색할 자세한 순위표 행 여부를 지정 합니다.         |
|공개 IReadOnlyList<LeaderboardRow> 행 {get;}     | 순위 당 순위표 데이터를 포함 하는 행        |
|공개 IReadOnlyList<LeaderboardColumn> 열 {get;}     | 열은 순위표 구성 하는 목록        |
|공용 uint TotalRowCount {get;}     | 순위표 행의 총 금액        |
|공개 문자열 DisplayName {get;}     | 순위표에 대 한 표시 이름       |

순위표 데이터 한 번에 하나의 페이지를 제공 됩니다. 루핑하고 수는 `LeaderboardResult` 행과 열 데이터를 검색 합니다.  
사용 하 여는 `HasNext` 부울 및 `GetNextAsync()` 함수 순위표 데이터의 이후 페이지를 검색 합니다.

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

사용할 통계 2017 순위표 서비스를 호출 하는 `StatisticManager` 대신 순위표 api는 `LeaderboardService` 순위표 api입니다.  

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

## <a name="2017-c-example"></a>2017 c + + 예제

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1.는 stats_manager의 의도 하지 않은 단일 인스턴스 가져오기

호출 하기 전에 `stats_manager` 함수를 의도 하지 않은 단일 인스턴스로 변수를 설정 해야 합니다.

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2.는 LeaderboardQuery 만들기

`leaderboard_query` 순서, 금액을 나타내고 및 순위표 호출에서 반환 된 데이터의 시작점입니다.

A `leaderboard_query` 반환 된 데이터에 영향는 설정할 수 있는 몇 가지 특성을 가집니다.

|속성 |설명  |
|---------|---------|
|m_skipResultToRank     |이 uint 변수 반환할 때 사용 하 여 시작 순위표 데이터 순위 무엇을 결정 합니다. 순위 순위 1에서 시작 됩니다.         |
|m_skipResultToMe     |설정 부울 값이 true 하면 시작 반환 순위표 데이터는 `XboxLiveUser` 에서 사용 되는 합니다 `get_leaderboard()` 호출 합니다.  |
|m_order     |열거형 형식의 `xbox::services::leaderboard::sort_order` 오름차순 이나 내림차순으로 두 가지 가능한 값입니다. 쿼리에 대 한이 변수를 설정 하 여 순위표 정렬 순서를 결정 합니다.        |
|m_maxItems     |이 uint 전화를 반환할 행의 최대 수를 결정 합니다. `get_leaderboard` 또는 `get_social_leaderboard()`.         |

`leaderboard_query` 이러한 속성 값에 할당 하는 데 사용할 수는 몇 가지 설정 기능을 갖습니다. 다음 코드에서는 보여 설치 하는 방법에 `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

이 쿼리는 반환는 100에서 시작 순위표의 행이 10 순위 개별 합니다.

> [!WARNING]
> 플레이어는 순위표 내에 포함 된 개수 보다 높은 SkipResultToRank를 설정 순위표 반환할 데이터를 0 개 행을 사용 하 여 수행 됩니다.

### <a name="3-call-getleaderboard"></a>3. get_leaderboard를 호출 합니다.

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName` 에서 사용 되는 합니다 `GetLeaderboard()` 호출의 대/소문자 구분 되는 [Windows 개발자 센터 대시보드에서](https://developer.microsoft.com/en-us/dashboard/windows/overview)타이틀에 대 한 구성 상태 이름과 동일 해야 합니다.

### <a name="4-read-the-leaderboard-data"></a>4. 순위표 데이터 읽기

호출 해야 순위표 데이터를 읽기 위해 합니다 `stats_manager::do_work()` 함수는 목록을 반환 하는 `stat_event` 값입니다. 순위표 데이터에 포함 됩니다는 `stat_event` 형식의 `stat_event_type::get_leaderboard_complete`. 목록에서이 형식의 이벤트를 발견할 때 `stat_event`s 통해 보일 수 있습니다 합니다 `leaderboard_result` 에 포함 된는 `stat_event` 데이터에 액세스할 수 있습니다.

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

순위표 결과에서 순위표 데이터 읽기  

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

데이터의 페이지는 순위표에서 더 이상 검색 합니다.  

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

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1.는 StatisticManager의 의도 하지 않은 단일 인스턴스 가져오기

호출 하기 전에 `StatisticManager` 함수를 의도 하지 않은 단일 인스턴스로 변수를 설정 해야 합니다.

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2.는 LeaderboardQuery 만들기

`LeaderboardQuery` 순서, 금액을 나타내고 및 순위표 호출에서 반환 된 데이터의 시작점입니다.  

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

A `LeaderboardQuery` 반환 된 데이터에 영향는 설정할 수 있는 몇 가지 특성을 가집니다.

|속성 |설명  |
|---------|---------|
|SkipResultToRank     |이 uint 변수 반환할 때 사용 하 여 시작 순위표 데이터 순위 무엇을 결정 합니다. 순위 순위 1에서 시작 됩니다.         |
|SkipResultToMe     |설정 부울 값이 true 하면 시작 반환 순위표 데이터는 `XboxLiveUser` 에서 사용 되는 합니다 `GetLeaderboard()` 호출 합니다.  |
|Order     |열거형 형식의 `Microsoft.Xbox.Services.Leaderboard.SortOrder` 오름차순 이나 내림차순으로 두 가지 가능한 값입니다. 쿼리에 대 한이 변수를 설정 하 여 순위표 정렬 순서를 결정 합니다.        |
|MaxItems     |이 uint 전화를 반환할 행의 최대 수를 결정 합니다. `GetLeaderboard()` 또는 `GetSocialLeaderboard()`.         |

형성 하 여 `LeaderboardQuery` 다음과 같이 보일 수 있습니다.

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

이 쿼리는 반환 순위는 100에서 시작 순위표의 행이 5 개의 개별 합니다.

> [!WARNING]
> 플레이어는 순위표 내에 포함 된 개수 보다 높은 SkipResultToRank를 설정 순위표 반환할 데이터를 0 개 행을 사용 하 여 수행 됩니다.

### <a name="3-call-getleaderboard"></a>3. GetLeaderboard()를 호출 합니다.

이제 호출할 수 있습니다 `GetLeaderboard()` 사용 하 여 사용자 `XboxLiveUser`, 사용자 통계의 이름 및 `LeaderboardQuery`합니다.

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName` 에서 사용 되는 합니다 `GetLeaderboard()` 호출의 대/소문자 구분 되는 [Windows 개발자 센터 대시보드에서](https://developer.microsoft.com/en-us/dashboard/windows/overview)타이틀에 대 한 구성 상태 이름과 동일 해야 합니다.

### <a name="4-read-leaderboard-data"></a>4. 읽기 순위표 데이터

호출 해야 순위표 데이터를 읽기 위해 합니다 `StatisticManager.DoWork()` 함수는 목록을 반환 하는 `StatisticEvent` 값입니다. 순위표 데이터에 포함 됩니다는 `StatisticEvent` 형식의 `GetLeaderboardComplete`. 목록에서이 형식의 이벤트를 발견할 때 `StatisticEvent`s 통해 보일 수 있습니다 합니다 `LeaderboardResult` 에 포함 된는 `StatisticEvent` 데이터에 액세스할 수 있습니다.

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

제목 코드에서 `StatisticManager.DoWork()` 모든 들어오는 통계 관리자 이벤트 처리 및 순위표에 대 한 뿐만 아니라 사용 해야 합니다. 

> [!NOTE]
> 검색 하기 위해 합니다 `LeaderboardResultEventArgs` 캐스팅 해야 합니다 `StatisticEvent.EventArgs` 으로 `LeaderboardResultEventArgs` 변수입니다.

### <a name="5-retrieve-more-leaderboard-data"></a>5. 더 많은 순위표 데이터를 검색 합니다.

이후 페이지 사용 해야 하는 순위표 데이터를 검색 하기 위해 합니다 `LeaderboardResult.HasNext` 속성 및 `LeaderboardResult.GetNextQuery()` 함수를는 `LeaderboardQuery` 하는 방법을 사용 하면 데이터의 다음 페이지 된다는 있습니다.

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```