---
title: 게임 개발용 CPUSets
description: 이 문서에서는 UWP (유니버설 Windows 플랫폼)에 대 한 새로운 CPUSets API의 개요를 제공 하 고 게임 및 응용 프로그램 개발과 관련 된 핵심 정보에 대해 설명 합니다.
ms.topic: article
ms.localizationpriority: medium
ms.date: 02/08/2017
ms.openlocfilehash: a20838a6d58eede75efb2441680aba6629db1700
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154207"
---
# <a name="cpusets-for-game-development"></a>게임 개발용 CPUSets

## <a name="introduction"></a>소개

UWP (유니버설 Windows 플랫폼)는 광범위 한 소비자 전자 장치에서 핵심입니다. 따라서 게임에서 포함 된 앱으로 서버에서 실행 되는 모든 유형의 응용 프로그램에 대 한 요구 사항을 해결 하기 위해 범용 API가 필요 합니다. API에서 제공 하는 올바른 정보를 활용 하 여 게임이 모든 하드웨어에서 가장 잘 실행 되도록 할 수 있습니다.

## <a name="cpusets-api"></a>CPUSets API

CPUSets API는 스레드를 예약 하는 데 사용할 수 있는 CPU 집합을 제어 합니다. 스레드가 예약 되는 위치를 제어 하는 데 사용할 수 있는 두 가지 함수는 다음과 같습니다.
- **SetProcessDefaultCpuSets** –이 함수는 특정 cpu 집합에 할당 되지 않은 경우 새 스레드가 실행 될 수 있는 cpu 집합을 지정 하는 데 사용할 수 있습니다.
- **Setthreadselectedcpusets** –이 함수를 사용 하면 특정 스레드가 실행 될 수 있는 CPU 집합을 제한할 수 있습니다.

**SetProcessDefaultCpuSets** 함수를 사용 하지 않으면 프로세스에 사용할 수 있는 모든 CPU 집합에서 새로 만든 스레드가 예약 될 수 있습니다. 이 섹션에서는 CPUSets API의 기본 사항에 대해 설명 합니다.

### <a name="getsystemcpusetinformation"></a>GetSystemCpuSetInformation

정보를 수집 하는 데 사용 되는 첫 번째 API는 **GetSystemCpuSetInformation** 함수입니다. 이 함수는 제목 코드에서 제공 하는 **SYSTEM_CPU_SET_INFORMATION** 개체의 배열로 정보를 채웁니다. 대상에 대 한 메모리는 게임 코드에 의해 할당 되어야 하며, 크기는 **GetSystemCpuSetInformation** 를 호출 하 여 결정 됩니다. 이렇게 하려면 다음 예제에서 보여 주는 것 처럼 **GetSystemCpuSetInformation** 에 대 한 두 호출이 필요 합니다.

```
unsigned long size;
HANDLE curProc = GetCurrentProcess();
GetSystemCpuSetInformation(nullptr, 0, &size, curProc, 0);

std::unique_ptr<uint8_t[]> buffer(new uint8_t[size]);

PSYSTEM_CPU_SET_INFORMATION cpuSets = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(buffer.get());
  
GetSystemCpuSetInformation(cpuSets, size, &size, curProc, 0);
```

반환 된 **SYSTEM_CPU_SET_INFORMATION** 의 각 인스턴스에는 하나의 고유한 처리 단위에 대 한 정보 (예를 들어, CPU 집합이 라고도 함)가 포함 됩니다. 이는 반드시 고유한 물리적 하드웨어 부분을 나타내는 것은 아닙니다. 하이퍼스레딩을 활용 하는 Cpu는 단일 물리적 처리 코어에서 실행 되는 여러 논리 코어를 가집니다. 동일한 실제 코어에 있는 서로 다른 논리 코어에서 여러 스레드를 예약 하면 하드웨어 수준 리소스를 최적화 하 여 커널 수준에서 추가 작업을 수행 해야 하는 경우를 들 수 있습니다. 동일한 실제 코어에서 별도의 논리 코어에 예약 된 두 스레드는 CPU 시간을 공유 해야 하지만 동일한 논리 코어로 예약 된 경우 보다 더 효율적으로 실행 됩니다.

### <a name="system_cpu_set_information"></a>SYSTEM_CPU_SET_INFORMATION

**GetSystemCpuSetInformation** 에서 반환 된이 데이터 구조의 각 인스턴스에 있는 정보에는 스레드가 예약 될 수 있는 고유한 처리 단위에 대 한 정보가 포함 되어 있습니다. 가능한 대상 장치 범위가 지정 된 경우 **SYSTEM_CPU_SET_INFORMATION** 데이터 구조의 많은 정보를 게임 개발에 적용할 수 없습니다. 표 1에는 게임 개발에 유용한 데이터 멤버에 대 한 설명이 나와 있습니다.

 **표 1. 데이터 멤버는 게임 개발에 유용 합니다.**

| 멤버 이름  | 데이터 형식 | Description |
| ------------- | ------------- | ------------- |
| 형식  | CPU_SET_INFORMATION_TYPE  | 구조체의 정보 형식입니다. 이 값이 **CpuSetInformation**않는 경우 무시 해야 합니다.  |
| Id  | unsigned long  | 지정 된 CPU 집합의 ID입니다. 이 ID는 **Setthreadselectedcpusets**와 같은 CPU 집합 함수에 사용 해야 합니다.  |
| 그룹  | unsigned short  | CPU 집합의 "프로세서 그룹"을 지정 합니다. 프로세서 그룹을 사용 하면 PC에 논리 코어를 64 개 이상 포함할 수 있으며 시스템이 실행 되는 동안 Cpu의 핫 교체가 허용 됩니다. 둘 이상의 그룹이 있는 서버가 아닌 PC가 표시 되는 경우가 종종 있습니다. 큰 서버나 서버 팜에서 실행 되는 응용 프로그램을 작성 하지 않는 한 대부분의 소비자 Pc에는 하나의 프로세서 그룹만 있으므로 단일 그룹에서 CPU 집합을 사용 하는 것이 가장 좋습니다. 이 구조체의 다른 모든 값은 그룹을 기준으로 합니다.  |
| LogicalProcessorIndex  | unsigned char  | CPU 집합의 그룹 상대 인덱스  |
| CoreIndex  | unsigned char  | CPU 집합이 있는 실제 CPU 코어의 그룹 상대 인덱스  |
| LastLevelCacheIndex  | unsigned char  | 이 CPU 집합에 연결 된 마지막 캐시의 그룹 상대 인덱스입니다. 시스템이 NUMA 노드 (일반적으로 L2 또는 L3 캐시)를 활용 하지 않는 한 가장 느린 캐시입니다.  |

<br />

다른 데이터 구성원은 소비자 Pc 또는 기타 소비자 장치에서 Cpu를 설명할 가능성이 낮은 정보를 제공 하므로 유용 하지 않을 수 있습니다. 반환 된 데이터에서 제공 하는 정보를 사용 하 여 다양 한 방식으로 스레드를 구성할 수 있습니다. 이 백서의 [게임 개발에 대 한 고려 사항](#considerations-for-game-development) 섹션에서는이 데이터를 활용 하 여 스레드 할당을 최적화 하는 몇 가지 방법을 설명 합니다.

다음은 다양 한 유형의 하드웨어에서 실행 되는 UWP 응용 프로그램에서 수집 되는 정보 유형의 몇 가지 예입니다.

**표 2. Microsoft Lumia 950에서 실행 되는 UWP 앱에서 반환 되는 정보입니다. 이는 마지막 수준 캐시가 여러 개인 시스템의 예입니다. Lumia 950는 듀얼 코어 ARM Cortex A57 및 쿼드 코어 ARM Cortex A53 Cpu를 포함 하는 Qualcomm 808 Snapdragon 프로세스를 제공 합니다.**

  ![표 2](images/cpusets-table2.png)

**표 3. 일반 PC에서 실행 되는 UWP 앱에서 반환 되는 정보입니다. 이는 하이퍼 스레딩을 사용 하는 시스템의 예입니다. 각 실제 코어에는 스레드를 예약할 수 있는 두 개의 논리 코어가 있습니다. 이 경우 시스템은 Intel 크 크 CPU E5-2620을 포함 합니다.**

  ![표 3](images/cpusets-table3.png)

**표 4. 쿼드 코어 Microsoft Surface Pro 4에서 실행 되는 UWP 앱에서 반환 된 정보입니다. 이 시스템에는 Intel Core i5-6300 CPU가 있습니다.**

  ![표 4](images/cpusets-table4.png)

### <a name="setthreadselectedcpusets"></a>SetThreadSelectedCpuSets

이제 CPU 집합에 대 한 정보를 사용할 수 있으므로 스레드를 구성 하는 데 사용할 수 있습니다. **CreateThread** 를 사용 하 여 만든 스레드의 핸들은 스레드를 예약할 수 있는 CPU 집합의 id 배열과 함께이 함수에 전달 됩니다. 다음 코드에서는 사용법의 한 가지 예를 보여 줍니다.

```
HANDLE audioHandle = CreateThread(nullptr, 0, AudioThread, nullptr, 0, nullptr);
unsigned long cores [] = { cpuSets[0].CpuSet.Id, cpuSets[1].CpuSet.Id };
SetThreadSelectedCpuSets(audioHandle, cores, 2);
```
이 예제에서 스레드는 **오디오 스레드로**선언 된 함수를 기반으로 생성 됩니다. 그러면이 스레드를 두 CPU 집합 중 하나에서 예약할 수 있습니다. CPU 집합의 스레드 소유권이 단독으로 적용 되지 않습니다. 특정 CPU 집합에 잠기지 않고 생성 된 스레드는 **오디오 스레드에서**시간을 걸릴 수 있습니다. 마찬가지로, 만든 다른 스레드도 나중에 이러한 CPU 집합 중 하나 또는 둘 다로 잠길 수도 있습니다.

### <a name="setprocessdefaultcpusets"></a>SetProcessDefaultCpuSets

**Setthreadselectedcpusets** 는 반대입니다. **SetProcessDefaultCpuSets** 스레드를 만들 때 스레드를 특정 CPU 집합으로 잠글 필요가 없습니다. 이러한 스레드를 특정 CPU 집합에서 실행 하지 않으려면 (예를 들어 렌더링 스레드나 오디오 스레드에서 사용 하는 경우),이 함수를 사용 하 여 이러한 스레드가 예약 되도록 허용할 코어를 지정할 수 있습니다.

## <a name="considerations-for-game-development"></a>게임 개발에 대 한 고려 사항

앞서 살펴본 것 처럼 CPUSets API는 스레드를 예약 하는 데 많은 정보와 유연성을 제공 합니다. 이 데이터에 대 한 사용을 찾으려고 하는 하향식 접근 방법을 사용 하는 대신 데이터를 사용 하 여 일반적인 시나리오를 수용 하는 방법을 찾는 하향식 방법을 사용 하는 것이 더 효과적입니다.

### <a name="working-with-time-critical-threads-and-hyperthreading"></a>시간이 중요 한 스레드 및 하이퍼스레딩 작업

이 메서드는 CPU 시간이 비교적 적은 다른 작업자 스레드와 함께 실시간으로 실행 해야 하는 몇 개의 스레드가 게임에 있는 경우에 효과적입니다. 지속적인 백그라운드 음악과 같은 일부 작업은 최적의 게임 환경을 위해 중단 없이 실행 해야 합니다. 오디오 스레드에 대해 고갈 된 단일 프레임의 경우에도 팝 또는 glitching 발생할 수 있으므로 모든 프레임에 필요한 CPU 시간을 수신 하는 것이 중요 합니다.

**SetProcessDefaultCpuSets** 와 함께 **Setthreadselectedcpusets** 를 사용 하면 작업자 스레드가 많은 스레드를 중단 없이 유지 하도록 할 수 있습니다. **Setthreadselectedcpusets** 를 사용 하 여 많은 스레드를 특정 CPU 집합에 할당할 수 있습니다. 그런 다음 **SetProcessDefaultCpuSets** 를 사용 하 여 생성 된 할당 되지 않은 스레드가 다른 CPU 집합에 배치 되도록 할 수 있습니다. 하이퍼스레딩을 활용 하는 Cpu의 경우 동일한 물리적 코어에서 논리 코어를 고려 하는 것도 중요 합니다. 작업자 스레드는 실시간 응답성을 사용 하 여 실행 하려는 스레드와 동일한 실제 코어를 공유 하는 논리 코어에서 실행 되도록 허용 하면 안 됩니다. 다음 코드에서는 PC가 하이퍼 스레딩을 사용 하는지 여부를 확인 하는 방법을 보여 줍니다.

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation( nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data( new uint8_t[retsize] );
if ( !GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>( data.get() ),
    retsize, &retsize, GetCurrentProcess(), 0) )
{
    // Error!
}
 
std::set<DWORD> cores;
std::vector<DWORD> processors;
uint8_t const * ptr = data.get();
for( DWORD size = 0; size < retsize; ) {
    auto info = reinterpret_cast<const SYSTEM_CPU_SET_INFORMATION*>( ptr );
    if ( info->Type == CpuSetInformation ) {
         processors.push_back( info->CpuSet.Id );
         cores.insert( info->CpuSet.CoreIndex );
    }
    ptr += info->Size;
    size += info->Size;
}
 
bool hyperthreaded = processors.size() != cores.size();
```

시스템에서 하이퍼스레딩을 활용 하는 경우에는 기본 CPU 집합 집합에 실제 시간 스레드와 동일한 실제 코어의 논리적 코어가 포함 되지 않아야 합니다. 시스템이 하이퍼스레딩을 사용 하지 않는 경우에는 기본 CPU 집합이 오디오 스레드를 실행 하는 CPU와 동일한 코어를 포함 하지 않는지 확인 해야 합니다.

실제 코어를 기반으로 스레드를 구성 하는 예제는 [추가 리소스](#additional-resources) 섹션에 연결 된 GitHub 리포지토리에서 사용할 수 있는 cpusets 샘플에서 찾을 수 있습니다.

### <a name="reducing-the-cost-of-cache-coherence-with-last-level-cache"></a>마지막 수준 캐시를 사용 하 여 캐시 일관성 비용 절감

캐시 일관성은 캐시 된 메모리가 동일한 데이터에 대해 작동 하는 여러 하드웨어 리소스에서 동일 하다는 개념입니다. 스레드가 서로 다른 코어에서 예약 되었지만 동일한 데이터에 대해 작업을 수행 하는 경우 서로 다른 캐시에서 해당 데이터의 개별 복사본에 대해 작업을 수행할 수 있습니다. 올바른 결과를 얻기 위해서는 이러한 캐시를 서로 일관 되 게 유지 해야 합니다. 여러 캐시 간에 일관성을 유지하는 것은 상대적으로 비용이 많이 들지만 모든 다중 코어 시스템이 작동하는 데 필수적입니다. 또한 클라이언트 코드를 완전히 제어할 수 있습니다. 기본 시스템은 독립적으로 작동 하 여 코어 간의 공유 메모리 리소스에 액세스 하 여 캐시를 최신 상태로 유지 합니다.

게임에 특히 많은 양의 데이터를 공유 하는 스레드가 여러 개 있는 경우 마지막 수준 캐시를 공유 하는 CPU 집합에 대해 예약 된 공간을 확보 하 여 캐시 일관성을 최소화할 수 있습니다. 마지막 수준 캐시는 NUMA 노드를 활용 하지 않는 시스템에서 코어에 사용할 수 있는 가장 느린 캐시입니다. 게임 PC에서 NUMA 노드를 활용 하는 것은 매우 드뭅니다. 코어가 마지막 수준 캐시를 공유 하지 않는 경우 일관성을 유지 하려면 더 높은 수준에 액세스 하 여 메모리 리소스를 더 느리게 액세스 해야 합니다. 캐시를 공유 하는 개별 CPU 집합에 대해 두 스레드를 잠그면 지정 된 프레임에서 50% 이상이 필요 하지 않은 경우 별도의 물리적 코어에서 일정을 지정 하는 것 보다 성능이 향상 될 수 있습니다. 

이 코드 예제에서는 자주 통신 하는 스레드가 마지막 수준 캐시를 공유할 수 있는지 여부를 확인 하는 방법을 보여 줍니다.

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation(nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data(new uint8_t[retsize]);
if (!GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get()),
    retsize, &retsize, GetCurrentProcess(), 0))
{
    // Error!
}
 
unsigned long count = retsize / sizeof(SYSTEM_CPU_SET_INFORMATION);
bool sharedcache = false;
 
std::map<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> cachemap;
for (size_t i = 0; i < count; ++i)
{
    auto cpuset = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get())[i];
    if (cpuset.Type == CPU_SET_INFORMATION_TYPE::CpuSetInformation)
    {
        if (cachemap.find(cpuset.CpuSet.LastLevelCacheIndex) == cachemap.end())
        {
            std::pair<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> newvalue;
            newvalue.first = cpuset.CpuSet.LastLevelCacheIndex;
            newvalue.second.push_back(cpuset);
            cachemap.insert(newvalue);
        }
        else
        {
            sharedcache = true;
            cachemap[cpuset.CpuSet.LastLevelCacheIndex].push_back(cpuset);
        }
    }
}
```

그림 1에 설명 된 캐시 레이아웃은 시스템에서 볼 수 있는 레이아웃 형식의 예입니다. 이 그림은 Microsoft Lumia 950에 있는 캐시의 그림입니다. CPU 256와 CPU 260 간에 발생 하는 스레드 간 통신은 시스템이 L2 캐시를 일관 되 게 유지 해야 하기 때문에 상당한 오버 헤드가 발생 합니다.

**그림 1. Microsoft Lumia 950 장치에 캐시 아키텍처가 있습니다.**

![Lumia 950 캐시](images/cpusets-lumia950cache.png)

## <a name="summary"></a>요약

UWP 개발에 사용할 수 있는 CPUSets API는 상당한 양의 정보를 제공 하 고 다중 스레딩 옵션에 대 한 제어를 제공 합니다. Windows 개발을 위한 이전 다중 스레드 Api와 비교할 때 추가 된 복잡성은 몇 가지 학습 곡선이 있지만 궁극적으로 유연 하 게 다양 한 소비자 Pc 및 기타 하드웨어 대상에서 더 나은 성능을 얻을 수 있습니다.

## <a name="additional-resources"></a>추가 리소스
- [CPU 집합 (MSDN)](/windows/desktop/ProcThread/cpu-sets)
- [ATG에서 제공 하는 CPUSets 샘플](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/CPUSets)
- [Xbox One의 UWP](index.md)