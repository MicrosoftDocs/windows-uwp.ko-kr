---
title: 순위표
author: KevinAsgari
description: 순위표 Xbox Live를 사용 하 여 플레이어를 비교 하는 방법을 알아봅니다.
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 190d54fb53192a1cc798b46a0a4b76d7bdd3e074
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4385444"
---
# <a name="leaderboards"></a>순위표

## <a name="introduction"></a>소개

[데이터 플랫폼 개요](../data-platform/data-platform.md), 설명 된 대로 순위표 플레이어, 간의 경쟁 유도 하 고 플레이어의 이전 최고 점수 뿐만 아니라 친구의 경쟁 하려고에 참여 하는 훌륭한 방법 됩니다.

[주요](stats2017.md#configured-stats-and-featured-leaderboards) 통계 순위표 타이틀의 게임 허브에 항상 표시 되며 홈 페이지에 고정 된 경우 제목에 대 한 UI의 일부로 때로는 표시 됩니다. 순위표 타이틀 내부에서 만드는 구성 된 기능을 갖춘 통계를 사용할 수도 있습니다.

## <a name="choosing-good-leaderboards"></a>좋은 순위표 선택

[플레이어 통계](player-stats.md)에서 설명 했 듯이 순위표 정의 된 상태에 해당 합니다.  플레이어를 개선 하는 방향으로 작동할 수 있는 실적에 해당 하는 순위표를 선택 해야 합니다.

예를 들어 자동차 레이싱 게임에에서 가장 적합 한 랩 시간 있다는 점에서 좋은 순위표, 플레이어 최상의 랩 시간을 개선 하는 방향으로 작동 합니다.  다른 예제는 전투 게임에서 Kill/데스 비율 또는 최대 콤보 크기입니다.

## <a name="when-to-display-leaderboards"></a>순위표 표시 하는 경우

타이틀에서 언제 든 지 순위표 표시할 수가 있습니다.  순위표는 게임 플레이 또는 흐름 타이틀을 방해 하지 않으므로 때 시간을 선택 해야 합니다.  라운드 사이 일치 등은 항상 좋은 합니다.

## <a name="how-to-display-leaderboards"></a>순위표 표시 하는 방법

Xbox Live SDK에서 제공 하는 순위표 표시 하기 위한 여러 옵션이 있습니다.  Unity Xbox Live 크리에이터 스 프로그램을 사용 하는 경우 순위표 Prefab 순위표 데이터를 표시를 사용 하 여 시작 수 있습니다.  자세한 내용은 [Unity에 Xbox Live 구성](../get-started-with-creators/configure-xbox-live-in-unity.md) 문서를 참조 하세요.

직접, Xbox Live SDK에 대해 코딩 다시 읽어 사용할 수 있는 Api에 알아봅니다.

### <a name="programming-guide"></a>프로그래밍 가이드

여러 순위표 Api는 순위표의 현재 상태를 가져오는 데 사용할 수 있습니다.  모든 Api는 비동기 및 차단 하지 않습니다.  순위표 데이터를 가져오고 계속 일반적인 게임 처리를 요청을 하는 것입니다.  서비스에서 순위표 결과 반환 하는 경우 적절 한 번에 결과 표시할 수 있습니다.

요청 해야 순위표 데이터 서비스에서 약간의 보다 빠른 플레이어 순위표 표시 될 때까지 기다리는 차단 되지 않도록을 표시 하려는 경우.

표시는 `leaderboard_service` 모든 순위표 API에 대 한 네임 스페이스입니다.

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

<td>API의 가장 기본적인 버전입니다.  이 순위표 맨 위에 있는 플레이어에서 시작 하는 특정된 순위표에 대 한 순위표 값을 반환 됩니다.</td>

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

특정 사용자에 게는 순위표 건너뛸 하려는 경우이 사용 합니다.  A `XUID` 는 각 Xbox 사용자에 대 한 고유 식별자.  로그인된 된 사용자 또는 친구 들 중 하나에 대 한 얻을 수 있으며,이 함수에 전달 하는 수 있습니다.

</td>

</tr>

</table>

그런 다음 순위표 결과 서비스에서 반환 되 면 호출 될 수 있는 콜백을 설정할 수 있습니다.  아래 예는 보여 줍니다.

익숙하지 않은 경우는 `pplx::task` 이러한 Api에서 반환 되 고, 이것은 비동기 작업 개체에서 Microsoft 병렬 프로그래밍 라이브러리 (PPL).  에 대해 자세히 알아볼 수 있습니다 [https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks).

아래 섹션에는 순위표 결과 검색 하 고 사용 수 방법을 보여 줍니다.

### <a name="example"></a>예

#### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. 순위표 결과 검색 하는 비동기 작업 만들기

첫 번째 단계는 특정 순위표에 대 한 결과 검색 하 여 순위표 서비스를 호출 합니다.

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

#### <a name="2-setup-a-callback"></a>2. 콜백을 설정합니다

순위표 결과 한 번 호출 하는 [연속 작업](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations) 을 설정할 수 있습니다.  이렇게 하면 다음과 같이 아래 합니다.

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

원래 호출한 하 고 수신 하는 개체의 컨텍스트에서이 연속 작업이 라고 합니다 ```leaderboard_result``` 타이틀에 적합 한 방식으로 표시할 수 있습니다.


#### <a name="3-display-leaderboard"></a>3. 순위표를 표시 합니다.

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