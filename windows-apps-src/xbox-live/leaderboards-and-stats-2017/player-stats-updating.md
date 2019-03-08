---
title: 통계 2017 업데이트
description: 통계 2017을 사용 하 여 Xbox Live player 통계를 업데이트 하는 방법에 알아봅니다.
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.date: 08/24/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 플레이어 통계를 통계 2017
ms.localizationpriority: medium
ms.openlocfilehash: d5b37c008e6aa719b641321c5e5a1c3360b20786
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631828"
---
# <a name="updating-stats-2017"></a>통계 2017 업데이트

사용 하 여 Xbox Live 서비스에 대 한 최신 값을 전송 하 여 통계를 업데이트 합니다 `StatsManager` 아래 설명할 Api입니다.

플레이어 통계를 추적 하 여 제목까지 이며 호출 `StatsManager` 적절 하 게 업데이트 합니다.  `StatsManager` 변경 내용을 버퍼링 하 고 이러한 서비스에 주기적으로 플러시합니다.  제목도 수동으로 플러시할 수 있습니다.

> [!NOTE]
> 너무 자주 통계를 플러시하지 않습니다.  그렇지 않으면을 제목 속도 제한 됩니다.  5 분 마다 한 번만 플러시 하는 것이 좋습니다.

### <a name="multiple-devices"></a>여러 장치

여러 장치에서 선수가 제목에 대 한 것 같습니다.  이 경우 작업을 동기화 상태로 유지 하기 위해 특정 노력 해야 합니다.

예를 들어, 플레이어는 Xbox 집에서 15 헤드샷 발생 합니다.  나중에 볼 수 10 자세한 헤드샷이 Xbox에서 친구의 집에서 합니다.  두 번째 장치에서 25 상태 값을 전송 해야 합니다.  하지만 어떤 이유로 든이 정보를 동기화 하지 않고이 알 수 없습니다를 포함 됩니다.

몇 가지 방법으로 이렇게 할 수 있습니다.

1. 저장할를 사용 하 여 [연결 된 저장소](../storage-platform/connected-storage/connected-storage-technical-overview.md)합니다.  일반적으로 사용자별 데이터 저장에 대 한 연결 된 저장소를 사용할 수 있습니다.  지정된 된 사용자에 대 한 여러 장치에서이 데이터를 동기화 유지 됩니다.
2. 이미을 제목에 대 한 보조 작업을 수행 하는 경우 동기화 통계를 유지 하려면 사용자 고유의 웹 서비스를 사용 합니다.

### <a name="offline"></a>오프라인

위에서 설명한 것 처럼을 제목은 플레이어 통계 및 따라서 오프 라인 시나리오를 지원 하도록 기록한을 추적 합니다. 

### <a name="examples"></a>예

이러한 개념을 함께 연결 하는 예제를 거치게 됩니다.

레이싱 게임의 일반적인 상태는 랩 시간입니다.  이러한 통계에 대 한 더 낮은 일반적으로 합니다.  상태 및 연결된 순위표를, 발생할 수 있는 낮은 것이 좋습니다.  즉,이 순위표 오름차순 정렬 됩니다.

제목은는 추적 재생 세션 중 사용자의 랩 시간.  랩 시간 보다 이전 가장 낮은 해야 하는 경우에 통계 관리자를 업데이트 합니다.

다음 방법 중 하나에서 이전 가장을 추적할 수 있습니다.
1. 저장에서 연결 된 저장소를 사용 하는 파일입니다.
2. 사용자 고유의 웹 서비스입니다.

서비스는 내용에 관계 없이 상태 값을 바꿉니다.  따라서 이전 가장 크거나 랩 시간을 사용 하 여 업데이트 하려는 경우에 다음 이전 최고의 덮어씁니다.

따라서을 제목에서 확인 하세요 게임 플레이 시나리오에 따라 적절 한 상태 값만 보내집니다.  경우에 따라 값이 낮을수록 더 나은 있을 수 있으며, 더 높은 일부 다른 경우에는 보다 효율적으로 또는 그 밖의 내용은 수 있습니다 완전히 합니다.

## <a name="programming-guide"></a>프로그래밍 가이드

일반적으로 통계를 사용 하기 위한 흐름이입니다.

1. 초기화는 `StatsManager` 로컬 사용자를 전달 하 여 API.
1. 사용자 재생을 제목, 상태 값을 사용 하 여 업데이트를 `set_stat` 함수입니다.
1. 이러한 상태 업데이트는 정기적으로 플러시 및 Xbox Live에 기록 합니다.  또한 이렇게 하려면 수동으로.

### <a name="initialization"></a>초기화

호출 하 여 `StatsManager` 필요한 정보를 사용 하 여 API를 초기화 하는 로컬 사용자를 사용 하 여 합니다.

아래 예제를 참조 하세요

```cpp
std::shared_ptr<stats_manager> statsManager = stats_manager::get_singleton_instance();
statsManager->add_local_user(user);
statsManager->do_work();  // returns stat_event_type::local_user_added
```

```csharp
Microsoft.Xbox.Services.Statistics.Manager.StatisticManager statManager = StatisticManager.SingletonInstance;
statManager.AddLocalUser(user);
statEvent = statManager.DoWork();
```

### <a name="writing-stats"></a>쓰기 통계

사용 하 여 통계를 작성 하는 `stats_manager::set_stat` 함수 패밀리입니다.  각 데이터 형식에 대 한이 함수의 세 가지 변형이 있습니다.

* `set_stat_number` 에 대 한 부동 소수점 수입니다.
* `set_stat_integer` 에 대 한 정수입니다.
* `set_stat_string` 문자열입니다.

이 호출 하면 상태 업데이트를 장치에 로컬로 캐시 됩니다.  주기적으로 이러한 Xbox Live를 플러시할 수 됩니다.

수동으로 통계를 통해 플러시 옵션이 `stats_manager::request_flush_to_service` API.  이 함수를 너무 자주 호출 속도 제한 수는 note 하십시오.  상태는 업데이트 하지는 것은 아닙니다.  단순히 제한 시간이 만료 되 면 업데이트는 발생 함을 의미 합니다.

```cpp
statsManager->set_stat_integer(user, L"numHeadshots", 20);
statsManager->request_flush_to_service(user); // requests flush to service, performs a do_work
statsManager->do_work();  // applies the stat changes, returns stat_update_complete after flush to service
```

```csharp
statManager.SetStatisticIntegerData(user, statName, (long)statValue);
statManager.RequestFlushToService(user);
statManager.DoWork();
```

#### <a name="example"></a>예

1 인칭 슈팅 있다고 가정해 보겠습니다.  일치 하는 동안 다음 통계를를 누적 될 수 있습니다.

| 상태 이름 | 형식 |
|-----------|--------|
| 최상의 파괴 라운드 당 | 정수 |
| 수명 파괴 | 정수 |
| 수명 살려 | 정수 |
| 수명 종료/사망 비율 | 번호 |

증가 플레이어가 일치 항목을 통해 지나면서 합니다 *라운드 당 종료*를 *수명 종료* 및 *수명 살려* 로컬로 합니다.

일치 항목의 끝에 다음을 수행 합니다.
1. 이전 가장을 사용 하 여 라운드에서 운영할 파괴를 비교 합니다.  큰 인 경우 업데이트 `StatsManager`합니다.
2. 새 값으로 해당 수명 종료 및 살려 업데이트 및 업데이트 `StatsManager`합니다.
3. 파괴/살려 계산 및 업데이트 `StatsManager`

1과 2에 대해 알아야 할 이전 상태 값을 참고 합니다.  이러한 검색에 위의 모범 사례 섹션을 참조 하세요.

이러한 통계 중 하나 다음 기사에서 설명할 leaderboard 일치 수 없습니다.

### <a name="flushing-stats"></a>플러시 통계

사용 하 여 통계를 수동으로 플러시할 수 있습니다 `stats_manager::request_flush_to_service`합니다.  순위표를 표시할 경우이 작업을 수행 하는 것이 좋습니다.

예를 들어 순위표에 대 한 경우 `Lifetime Kills` 위의 예제는 순위표를 표시 하기 전에이 상태에 해당 하는 상태 업데이트 서버로 플러시 되었습니다 했습니다 되도록 하려는 합니다.  이런 방식으로 순위표 플레이어의 최신 진행 상황을 반영합니다.

### <a name="cleanup"></a>정리
제목 닫을 때 통계 manager에서 사용자를 제거 합니다. 서비스에 최신 상태 값을 플러시합니다.

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```
