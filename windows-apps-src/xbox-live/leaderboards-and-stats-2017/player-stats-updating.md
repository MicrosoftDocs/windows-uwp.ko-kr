---
title: 통계 2017 업데이트
author: KevinAsgari
description: 통계 2017을 사용 하 여 Xbox Live 플레이어 통계를 업데이트 하는 방법을 알아봅니다.
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.author: kevinasg
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 플레이어 통계, 통계 2017
ms.localizationpriority: medium
ms.openlocfilehash: 57d52b102d46efa1a2e6d35dedd46e6aba577977
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4530224"
---
# <a name="updating-stats-2017"></a>통계 2017 업데이트

통계를 사용 하 여 Xbox Live 서비스에 대 한 최신 값을 전송 하 여 업데이트는 `StatsManager` Api는 아래에서 설명 합니다.

타이틀 플레이어 통계를 추적 하기 위해 사용자가 및 호출 하면 `StatsManager` 를 적절 하 게이 업데이트 합니다.  `StatsManager` 버퍼 변경 하 고 서비스에 이러한 주기적으로 플러시 하겠습니다.  타이틀 플러시 수동으로 수 있습니다.

> [!NOTE]
> 통계를 너무 자주 플러시지 않습니다.  그렇지 않으면 타이틀 속도 제한 됩니다.  5 분 마다 한 번만 flush 하는 것이 좋습니다.

### <a name="multiple-devices"></a>여러 장치

여러 장치에서 타이틀을 재생 하는 플레이어에 대 한 것이 가능 합니다.  이 경우 특정 작업을 동기화 상태로 유지 하기 위해 노력 해야 합니다.

예를 들어, 플레이어의 Xbox 집에서 15 헤드샷을 확인 합니다.  나중에 다운로드할 때 더 많은 10 헤드샷 자신의 xbox 친구의 집에서.  두 번째 장치에 25 시작 값을 전송 해야 합니다.  하지만 해야 어떤 방식으로든이 정보를 동기화 하지 않고이 알 수 없습니다.

몇 가지 방법으로 설정할 수 있습니다.

1. [연결 된 저장소](../storage-platform/connected-storage/connected-storage-technical-overview.md)를 사용 하 여 저장 합니다.  일반적으로 사용자별 데이터 저장에 대 한 연결 된 저장소를 사용 합니다.  이 데이터는 특정된 사용자에 대 한 다른 디바이스 간에 동기화 상태로 유지 합니다.
2. 웹 서비스를 사용 하 여 타이틀에 대 한 보조 작업을 수행 하기 위한 이미 있는 경우 통계를 동기화 상태로 유지 합니다.

### <a name="offline"></a>오프라인

위에서 언급 한 것 처럼 타이틀은 플레이어 통계와 따라서 오프 라인 시나리오를 지 원하는 기록한을 추적 합니다. 

### <a name="examples"></a>예

이러한 개념을 함께 연결 하는 예를 수행 합니다.

레이싱 게임에 일반적인 상태 랩 시간입니다.  낮은 일반적으로 이러한 통계에 대 한 좋습니다.  상태와 관련 된 순위표 만들어야 하므로 저하는 것이 좋습니다.  즉,이 순위표 오름차순 정렬 됩니다.

타이틀은는 추적의 재생 세션 동안 사용자의 랩 시간.  이전가 가장 보다 낮은 랩 타임 사용 했던 경우에 통계 관리자를 업데이트 하 합니다.

다음 방법 중 하나에 이전 최고를 추적할 수 있습니다.
1. 파일에서 연결 된 저장소를 사용 하 여 파일.
2. 고유한 웹 서비스입니다.

서비스 관계 없이 시작 값을으로 바꿉니다.  따라서 이전가 가장 보다 큰 랩 시간으로 업데이트 하는 경우에 다음의 이전 최고를 덮어씁니다.

따라서을 제목 확인 하십시오 게임 플레이 시나리오에 따라 적절 한 시작 값만 보내집니다.  경우에 따라 값이 낮으면 것이 더, 높은 몇 가지 다른 경우에는 다른 요소 인지, 더 나은 수 있습니다 완전히 합니다.

## <a name="programming-guide"></a>프로그래밍 가이드

일반적으로 통계를 사용 하는 흐름이입니다.

1. 초기화는 `StatsManager` 로컬 사용자를 전달 하 여 API입니다.
1. 타이틀을 통해 사용자가 재생, 업데이트를 사용 하 여 시작 값은 `set_stat` 함수입니다.
1. 이러한 통계 업데이트 주기적으로 플러시 고 Xbox Live에 기록 됩니다.  또한 이렇게 하려면 수동으로 합니다.

### <a name="initialization"></a>초기화

호출의 `StatsManager` 로컬 사용자가 필요한 정보를 사용 하 여 API를 초기화 합니다.

아래 예제 참조

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

사용 하 여 통계를 작성 하는 `stats_manager::set_stat` 함수의 제품군 합니다.  각 데이터 유형에 대 한이 함수는 세 개의 변형이 있습니다.

* `set_stat_number` 부동 소수점에 대 한.
* `set_stat_integer` 정수입니다.
* `set_stat_string` 문자열입니다.

이 호출할 때 통계 업데이트가 장치에 로컬로 캐시 됩니다.  주기적으로 Xbox Live에 이러한 플러시됩니다 됩니다.

통계를 통해 수동으로 플러시 옵션에는 `stats_manager::request_flush_to_service` API입니다.  너무 자주이 함수를 호출 하면 속도 제한을 수는 note 하십시오.  상태는 업데이트 되지 않습니다는 것은 아닙니다.  단순히 업데이트 이루어지도록 제한 시간이 만료 되 면 의미 합니다.

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

1 인칭 슈팅 있다고 가정해 보겠습니다.  일치 하는 동안 다음 통계 누적 될 수 있습니다.

| 통계 이름 | 형식 |
|-----------|--------|
| 최상의 파괴 라운드 당 | 정수 |
| 수명 파괴 | 정수 |
| 수명 용사 | 정수 |
| 수명 Kill/데스 비율 | 숫자 |

플레이어는 일치를 통과 하는 대로 증가 시키면는 *원형 당 중단*, *수명 중단* 및 *수명 용사* 로컬로 합니다.

일치 항목의 끝에 다음을 수행 합니다.
1. 자신의 이전 최고 기록 된 라운드에서 가져온 파괴 비교 합니다.  큰 경우 업데이트 한 다음 `StatsManager`.
2. 새 값을 사용 하 여 자신의 수명을 중단 및 용사 업데이트 및 업데이트 `StatsManager`.
3. 대/용사 계산 및 업데이트 `StatsManager`

1과 2에 대 한 이전 시작 값을 알 필요 하면 메모 합니다.  이러한 검색에 모범 사례에 대 한 위의 섹션을 참조 하세요.

이러한 통계 중 하나에 해당 다음 문서에서 설명 하는 순위표 수 있습니다.

### <a name="flushing-stats"></a>플러시 통계

통계를 사용 하 여 수동으로 플러시한 수 `stats_manager::request_flush_to_service`.  순위표 표시 하려는 경우이 작업을 수행 하는 것이 좋습니다.

예를 들어, 순위표에 대 한 경우 `Lifetime Kills` 위의 예제에서는 순위표 표시 하기 전에이 상태에 해당 통계 업데이트 서버도 플러시됩니다 된 적 있는지 확인 하려고 합니다.  이렇게는 순위표 플레이어의 최신 진행률을 반영합니다.

### <a name="cleanup"></a>정리
제목 닫으면 통계 관리자에서 사용자를 제거 합니다. 서비스에 최신 시작 값 플러시합니다.

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```
