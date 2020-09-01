---
title: 추적 데이터 액세스-.NET TraceProcessing
description: 이 자습서에서는 TraceProcessor를 사용 하 여 추적 데이터에 액세스 하는 방법에 대해 알아봅니다.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: ef4d3df6e5a5dd93dbcb2caadc8e3f299aad581c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173697"
---
# <a name="access-trace-data"></a>추적 데이터 액세스

.NET TraceProcessing은 다음 패키지 ID를 사용 하 여 [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) 에서 사용할 수 있습니다.

Microsoft. Windows. EventTracing

이 패키지를 사용 하 여 추적 파일의 데이터에 액세스할 수 있습니다. 추적 파일이 아직 없는 경우 [Windows 성능 레코더](/windows-hardware/test/wpt/start-a-recording) 를 사용 하 여 만들 수 있습니다.

다음 예제 콘솔 앱에서는 추적에 포함 된 모든 프로세스의 명령줄에 액세스 하는 방법을 보여 줍니다.

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using System;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

            trace.Process();

            IProcessDataSource processData = pendingProcessData.Result;

            foreach (IProcess process in processData.Processes)
            {
                Console.WriteLine(process.CommandLine);
            }
        }
    }
}
```

## <a name="using-traceprocessor"></a>TraceProcessor 사용

추적을 처리 하려면 [Traceprocessor. Create](/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create)를 호출 합니다. 핵심 인터페이스는 [ITraceProcessor](/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)이 인터페이스를 사용 하면 다음과 같은 패턴이 포함 됩니다.

1. 먼저, 추적에서 사용 하려는 데이터를 프로세서에 알려 주십시오.
2. 둘째, 추적을 처리 합니다. 하거나
3. 마지막으로 결과에 액세스 합니다.

프로세서에 필요한 데이터 종류를 알려 주는 것은 가능한 모든 종류의 추적 데이터를 처리 하는 데 시간을 소비할 필요가 없음을 의미 합니다. 대신, [Traceprocessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) 는 요청 하는 특정 종류의 데이터를 제공 하는 데 필요한 작업만 수행 합니다.

## <a name="recommended-project-settings"></a>권장 프로젝트 설정

TraceProcessor와 함께 사용 하는 몇 가지 프로젝트 설정이 있습니다.

1. Exe를 64 비트로 실행 하는 것이 좋습니다.

    새 c # .NET Framework 콘솔 응용 프로그램에 대 한 Visual Studio 기본값은 32 비트를 선호 하는 CPU입니다. .NET Core의 기본값은 이미 권장 설정입니다.

    추적 처리는 메모리를 많이 사용 하는 경우, 특히 추적 크기가 크면 TraceProcessor를 사용 하는 exe에서 플랫폼 대상을 x 64로 변경 하거나 32 비트를 사용 하지 않는 것이 좋습니다. 이러한 설정을 변경 하려면 프로젝트의 속성 아래에 있는 빌드 탭을 참조 하세요. 모든 구성에 대해 이러한 설정을 변경 하려면 구성 드롭다운이 현재 구성의 기본값 대신 모든 구성으로 설정 되어 있는지 확인 합니다.

2. 이전 packages.config 모드가 아닌 최신 스타일의 PackageReference 모드를 사용 하 여 NuGet을 사용 하는 것이 좋습니다.

    새 프로젝트에 대 한 기본값을 변경 하려면 도구, NuGet 패키지 관리자, 패키지 관리자 설정, 패키지 관리, 기본 패키지 관리 형식을 참조 하세요.

## <a name="built-in-data-sources"></a>기본 제공 데이터 원본

.Etl 파일은 추적에서 많은 종류의 데이터를 캡처할 수 있습니다. .Etl 파일에 있는 데이터는 추적이 캡처될 때 사용 하도록 설정 된 공급자에 따라 달라 집니다. 다음 목록에서는 TraceProcessor에서 사용할 수 있는 추적 데이터의 종류를 보여 줍니다.

| 코드                                      | Description                                                                                                                | 관련 된 WPA 항목                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| 추적. UseClassicEvents()                  | 스키마 정보를 포함 하지 않는 추적의 클래식 ETW 이벤트를 제공 합니다.                                         | 일반 이벤트 테이블 (이벤트 유형이 클래식 또는 WPP 인 경우)             |
| 추적. UseConnectedStandbyData()           | 연결 된 대기를 시작 하 고 종료 하는 시스템에 대 한 추적 데이터를 제공 합니다.                                        | CS 요약 테이블                                                     |
| 추적. UseCpuIdleStates()                  | CPU C 상태에 대 한 추적의 데이터를 제공 합니다.                                                                             | CPU 유휴 상태 테이블 (형식이 Actual 인 경우)                          |
| 추적. UseCpuSamplingData()                | 명령 포인터의 주기적인 샘플링을 기반으로 CPU 사용량에 대 한 추적 데이터를 제공 합니다.                          | CPU 사용 (샘플링) 테이블                                            |
| 추적. UseCpuSchedulingData()              | 컨텍스트 전환 및 준비 스레드 이벤트를 포함 하 여 CPU 스레드 예약에 대 한 추적의 데이터를 제공 합니다.                | CPU 사용량 (정확한) 테이블                                            |
| 추적. UseDevicePowerData ()                | 장치 D 상태에 대 한 추적의 데이터를 제공 합니다.                                                                          | 장치 d 상태 테이블                                                  |
| 추적. UseDirectXData()                    | DirectX 활동에 대 한 추적 데이터를 제공 합니다.                                                                         | GPU 사용률 테이블                                                |
| traceUseDiskIOData ()                      | 디스크 i/o 작업에 대 한 추적의 데이터를 제공 합니다.                                                                        | 디스크 사용 테이블                                                     |
| 추적. UseEnergyEstimationData()           | 에너지 추정 엔진에서 예상 프로세스별 에너지 사용량에 대 한 추적 데이터를 제공 합니다.                         | 에너지 추정 엔진 요약 (프로세스별) 테이블                  |
| 추적. UseEnergyMeterData()                | 에너지 측정기 인터페이스 (EMI)에서 측정 된 에너지 사용량에 대 한 추적 데이터를 제공 합니다.                                  | 에너지 추정 엔진 (Emi) 테이블                              |
| 추적. UseFileIOData()                     | 파일 i/o 작업에 대 한 추적의 데이터를 제공 합니다.                                                                        | 파일 i/o 테이블                                                       |
| 추적. UseGenericEvents ()                  | 추적에서 매니페스트 및 TraceLogging 이벤트를 제공 합니다.                                                                  | 일반 이벤트 테이블 (이벤트 유형이 매니페스트 되거나 TraceLogging 인 경우) |
| 추적. UseHandles ()                        | 활성 커널 핸들에 대 한 추적의 부분 데이터를 제공 합니다.                                                            | Handles 테이블                                                        |
| 추적. Use하드 폴트 ()                     | 하드 페이지 폴트에 대 한 추적의 데이터를 제공 합니다.                                                                         | 하드 오류 테이블                                                    |
| 추적. UseHeapSnapshots ()                  | 프로세스 힙 사용에 대 한 추적의 데이터를 제공 합니다.                                                                       | 힙 스냅숏 테이블                                                  |
| 추적. UseHypercalls ()                     | 추적 하는 동안 발생 한 Hyper-v hypercall에 대 한 데이터를 제공 합니다.                                                        |                                                                      |
| 추적. UseImageSections()                  | 이미지의 섹션에 대 한 추적의 데이터를 제공 합니다.                                                                 | CPU 사용 (샘플링) 테이블의 섹션 이름 열                 |
| 추적. UseInterruptHandlingData()          | ISR (인터럽트 서비스 루틴) 및 DPC (지연 된 프로시저 호출) 작업에 대 한 추적의 데이터를 제공 합니다.               | DPC/ISR 테이블                                                        |
| 추적. UseMarks()                          | 추적에서 표시 (타임 스탬프 레이블 지정)를 제공 합니다.                                                                      | 표시 테이블                                                          |
| 추적. UseMemoryUtilizationData()          | 전체 시스템 메모리 사용률에 대 한 추적 데이터를 제공 합니다.                                                          | 메모리 사용률 테이블                                             |
| 추적. UseMetadata()                       | 추가 처리 없이 사용할 수 있는 추적 메타 데이터를 제공 합니다.                                                              | 시스템 구성, 추적 및 일반                             |
| 추적. UsePlatformIdleStates()             | 시스템의 대상 및 실제 플랫폼 유휴 상태에 대 한 추적의 데이터를 제공 합니다.                                   | 플랫폼 유휴 상태 테이블                                            |
| 추적. UsePoolAllocations ()                | 커널 풀 메모리 사용에 대 한 추적의 데이터를 제공 합니다.                                                                 | 풀 요약 테이블                                                   |
| 추적. UsePowerConfigurationData()         | 시스템 전원 구성에 대 한 추적의 데이터를 제공 합니다.                                                               | 시스템 구성, 전원 설정                                 |
| 추적. UsePowerDependencyCoordinatorData() | 활성 전원 종속성 코디네이터 단계에 대 한 추적의 데이터를 제공 합니다.                                               | 알림 단계 요약 테이블                                     |
| 추적. UseProcesses ()                      | 추적과 Pdb 뿐만 아니라 추적 중에 활성화 된 프로세스에 대 한 데이터를 제공 합니다.                                      | 프로세스 테이블 Images 테이블 기호 허브                           |
| 추적. UseProcessorCounters()              | PCM (프로세서 카운터 모니터)의 프로세서 성능 카운터 값에 대 한 추적의 데이터를 제공 합니다.                |                                                                      |
| 추적. UseProcessorFrequencyData()         | 프로세서가 실행 된 빈도에 대 한 추적 데이터를 제공 합니다.                                                    | 프로세서 빈도 테이블 (형식이 Actual 인 경우)                      |
| 추적. UseProcessorProfileData()           | 활성 프로세서 전원 프로필에 대 한 추적의 데이터를 제공 합니다.                                                       | 프로세서 프로필 테이블                                             |
| 추적. UseProcessorParkingData()           | 파킹 또는 파킹 해제 된 프로세서에 대 한 추적의 데이터를 제공 합니다.                                                 | 프로세서 파킹 상태 테이블                                        |
| 추적. UseProcessorParkingLimits()         | 최대 허용 파킹 해제 프로세서 수에 대 한 추적 데이터를 제공 합니다.                                        | 코어 파킹 캡 상태 테이블                                         |
| 추적. UseProcessorQualityOfServiceData()  | 각 프로세서의 서비스 품질 수준에 대 한 추적의 데이터를 제공 합니다.                                          | 프로세서 Qos 클래스 테이블                                            |
| 추적. UseProcessorThrottlingData()        | 프로세서 최대 빈도 제한에 대 한 추적의 데이터를 제공 합니다.                                                   | 프로세서 제약 조건 테이블                                          |
| 추적. UseReadyBootData ()                  | 부팅 준비에서 부팅 프리페치 작업에 대 한 추적의 데이터를 제공 합니다.                                                | 준비 된 부팅 이벤트 테이블                                              |
| 추적. UseReferenceSetData()               | 각 프로세스에서 사용 하는 가상 메모리의 페이지에 대 한 추적의 데이터를 제공 합니다.                                             | 참조 집합 테이블                                                  |
| 추적. UseRegionsOfInterest()              | Xml 구성 파일에 지정 된 대로 추적에서 관심 간격의 명명 된 영역을 제공 합니다.                       | 관심 영역 테이블                                            |
| 추적. UseRegistryData()                   | 추적 하는 동안 레지스트리 동작에 대 한 데이터를 제공 합니다.                                                                      | 레지스트리 테이블                                                       |
| 추적. UseResidentSetData()                | 실제 메모리에 상주 하는 각 프로세스에 대 한 가상 메모리의 페이지에 대 한 추적 데이터를 제공 합니다.       | 상주 집합 테이블                                                   |
| 추적. UseRundownData ()                    | 추적 런다운 데이터 수집이 발생 한 간격에 대 한 추적의 데이터를 제공 합니다.                            | 그래프 타임 라인의 음영 처리 된 영역                                 |
| 추적. UseScheduledTasks()                 | 추적 하는 동안 실행 된 예약 된 작업에 대 한 데이터를 제공 합니다.                                                               | 예약 된 작업 테이블                                                |
| 추적. UseServices()                       | 활성 상태 이거나 추적 하는 동안 상태를 캡처한 서비스에 대 한 데이터를 제공 합니다.                                  | 서비스 테이블; 시스템 구성, 서비스                       |
| 추적. UseStacks ()                         | 추적 하는 동안 기록 된 스택에 대 한 데이터를 제공 합니다.                                                                        |                                                                      |
| 추적. UseStackEvents ()                    | 추적 하는 동안 기록 된 스택과 관련 된 이벤트에 대 한 데이터를 제공 합니다.                                                 | 스택 테이블                                                         |
| 추적. UseStackTags ()                      | 추적의 스택을 XML 구성 파일에 지정 된 스택 태그로 그룹화 하는 맵 편집기를 제공 합니다.               | 스택 태그 및 스택 (프레임 태그)과 같은 열                     |
| 추적. UseSymbols()                        | 추적 기호를 로드 하는 기능을 제공 합니다.                                                                          | 기호 경로 구성 기호 로드                                 |
| 추적. UseSyscalls()                       | 추적 하는 동안 발생 한 syscall에 대 한 데이터를 제공 합니다.                                                                 | Syscall 테이블                                                       |
| 추적. UseSystemMetadata()                 | 추적에서 시스템 차원의 일반적인 메타 데이터를 제공 합니다.                                                                       | 시스템 구성                                                 |
| 추적. UseSystemPowerSourceData()          | 활성 시스템 전원 (AC vs DC)에 대 한 추적의 데이터를 제공 합니다.                                                | 시스템 전원 원본 테이블                                            |
| 추적. UseSystemSleepData()                | 전반적인 시스템 전원 상태에 대 한 추적의 데이터를 제공 합니다.                                                               | 전원 전환 표                                               |
| 추적. UseTargetCpuIdleStates()            | 대상 CPU C 상태에 대 한 추적의 데이터를 제공 합니다.                                                                      | CPU 유휴 상태 테이블 (형식이 Target 인 경우)                          |
| 추적. UseTargetProcessorFrequencyData()   | 대상 프로세서 빈도에 대 한 추적의 데이터를 제공 합니다.                                                             | 프로세서 빈도 테이블 (형식이 Target 인 경우)                      |
| 추적. UseThreads ()                        | 추적 하는 동안 활성화 된 스레드에 대 한 데이터를 제공 합니다.                                                                         | 스레드 수명 테이블                                               |
| 추적. UseTraceStatistics()                | 추적에서 이벤트에 대 한 통계를 제공 합니다.                                                                           | 시스템 구성, 통계 추적                               |
| 추적. UseUtcData ()                        | UTC (유니버설 원격 분석 클라이언트)를 사용 하 여 Microsoft 원격 분석 작업에 대 한 추적 데이터를 제공 합니다.                      | Utc 테이블                                                            |
| 추적. UseWindowInFocus()                  | 포커스가 있는 활성 UI 창의 변경 내용에 대 한 추적의 데이터를 제공 합니다.                                                 | 포커스 테이블의 창                                                |
| 추적. UseWindowsTracePreprocessorEvents() | 추적의 Windows 소프트웨어 추적 전처리기 (WPP) 이벤트를 제공 합니다.                                                    | WPP 추적 테이블 일반 이벤트 테이블 (이벤트 유형이 WPP 인 경우)       |
| 추적. UseWinINetData()                    | Windows 인터넷 (WinINet)을 통한 인터넷 작업에 대 한 추적의 데이터를 제공 합니다.                                         | 다운로드 정보 테이블                                               |
| 추적. UseWorkingSetData()                 | 각 프로세스 또는 커널 범주에 대 한 작업 집합에 있던 가상 메모리의 페이지에 대 한 추적의 데이터를 제공 합니다. | 가상 메모리 스냅숏 테이블                                       |

[ITraceSource](/dotnet/api/microsoft.windows.eventtracing.itracesource) 에서 사용 가능한 모든 추적 데이터에 대 한 확장 메서드를 참조 하거나 "trace"에서 사용할 수 있는 메서드를 검사 합니다. IntelliSense에서 표시 됩니다.

## <a name="next-steps"></a>다음 단계

이 개요에서는 TraceProcessor 및 액세스할 수 있는 기본 제공 데이터 원본을 사용 하 여 추적 데이터에 액세스 하는 방법을 배웠습니다.

다음 단계에서는 traceprocessor를 [확장](extensibility.md) 하 여 사용자 지정 추적 데이터에 액세스 하는 방법을 알아봅니다.