---
title: 스트리밍 사용-.NET TraceProcessing
description: 이 자습서에서는 스트리밍을 사용 하 여 더 짧은 메모리를 사용 하는 즉시 추적 데이터에 액세스 하는 방법을 알아봅니다.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 6ad0f5977ed4d739ce3133c9e67c0eefc6e0cbd9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168827"
---
# <a name="use-streaming-with-traceprocessor"></a>TraceProcessor와 함께 스트리밍 사용

기본적으로 TraceProcessor는 추적이 처리 될 때 메모리에 로드 하 여 데이터에 액세스 합니다. 이 버퍼링 방법은 사용 하기 쉽지만 메모리 사용 측면에서 비용이 많이 들 수 있습니다.

TraceProcessor는 추적도 제공 합니다. UseStreaming ()-스트리밍 방식으로 여러 유형의 추적 데이터에 액세스할 수 있도록 지원 합니다 (데이터를 메모리에 버퍼링 하는 대신 추적 파일에서 읽을 때 데이터 처리). 예를 들어, syscall 추적은 매우 클 수 있으며, 전체 syscall 목록을 추적에 버퍼링 하는 것은 비용이 많이 들 수 있습니다.

## <a name="accessing-buffered-data"></a>버퍼링 된 데이터 액세스

다음 코드에서는 추적을 통해 일반적인 버퍼링 방식으로 syscall 데이터에 액세스 하는 방법을 보여 줍니다. UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

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
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>스트리밍 데이터 액세스

큰 syscall 추적을 사용 하는 경우 메모리의 syscall 데이터를 버퍼링 하려고 하면 비용이 많이 들 수 있습니다. 또는 가능 하지 않을 수도 있습니다. 다음 코드에서는 추적을 대체 하 여 스트리밍 방식으로 동일한 syscall 데이터에 액세스 하는 방법을 보여 줍니다. UseSyscalls () with trace. UseStreaming(). UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

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
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>스트리밍 작동 방법

기본적으로 모든 스트리밍 데이터는 추적을 통해 첫 번째 패스에서 제공 되며 다른 원본에서 버퍼링 된 데이터는 사용할 수 없습니다. 위의 예제에서는 버퍼링과 스트리밍을 결합 하는 방법을 보여 줍니다. 스레드 데이터는 syscall 데이터가 스트리밍되 기 전에 버퍼링 됩니다. 따라서 버퍼링 된 스레드 데이터를 가져오기 위해 추적을 두 번 읽고 버퍼링 된 스레드 데이터를 사용 하 여 스트리밍 syscall 데이터에 액세스 하는 두 번째를 사용 가능 하 게 합니다. 이러한 방식으로 스트리밍 및 버퍼링을 결합 하기 위해이 예제에서는 ConsumerSchedule을 추적에 전달 합니다. UseStreaming(). UseSyscalls ()-syscall 처리가 추적을 통해 두 번째 패스에 발생 합니다. Syscall 콜백은 추적에서 보류 중인 결과에 액세스할 수 있습니다. 각 syscall를 처리 하는 경우에는 UseThreads ()를 사용할 수 있습니다. 이 선택적 인수를 사용 하지 않으면 syscall streaming이 추적을 통해 첫 번째 패스에서 실행 되 고 (패스가 하나만 있음) 추적에서 보류 중인 결과가 발생 합니다. UseThreads ()를 아직 사용할 수 없습니다. 이 경우 콜백은 여전히 syscall에서 ThreadId에 액세스할 수 있지만, 연결을 처리 하는 스레드는 아직 처리 되지 않았을 수 있는 다른 이벤트를 통해 제공 되기 때문에 스레드 프로세스에 대 한 액세스 권한이 없습니다.

버퍼링과 스트리밍 간 사용의 몇 가지 주요 차이점은 다음과 같습니다.

1. 버퍼링은 [IPendingResult &lt; T &gt; ](/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)를 반환 하 고이에 포함 된 결과는 추적이 처리 된 후에만 사용할 수 있습니다. 추적이 처리 된 후 foreach 및 LINQ와 같은 기술을 사용 하 여 결과를 열거할 수 있습니다.
2. 스트리밍은 void를 반환 하 고 대신 콜백 인수를 사용 합니다. 각 항목을 사용할 수 있게 되 면 콜백을 한 번 호출 합니다. 데이터가 버퍼링 되지 않으므로 foreach 또는 LINQ를 사용 하 여 열거할 결과 목록이 없습니다. 스트리밍 콜백은 처리가 완료 된 후에 사용 하기 위해 저장 하려는 데이터의 모든 부분을 버퍼링 해야 합니다.
3. 버퍼링 된 데이터를 처리 하는 코드는 trace 호출 후에 표시 됩니다. Process ()-보류 중인 결과를 사용할 수 있습니다.
4. 스트리밍 데이터를 처리 하는 코드는 trace 호출 앞에 표시 됩니다. Process (), 추적에 대 한 콜백입니다. UseStreaming ... () 메서드.
5. 스트리밍 소비자는 스트림의 일부만 처리 하 고 컨텍스트를 호출 하 여 이후 콜백을 취소 하도록 선택할 수 있습니다. Cancel (). 버퍼링 소비자는 항상 전체 버퍼링 된 목록을 제공 합니다.

## <a name="correlated-streaming-data"></a>상호 관련 된 스트리밍 데이터

경우에 따라 추적 데이터가 이벤트 시퀀스에서 발생 합니다. 예를 들어 syscall는 별도의 enter 및 exit 이벤트를 통해 기록 되지만 두 이벤트의 결합 된 데이터는 더 유용할 수 있습니다. 메서드 추적입니다. UseStreaming(). UseSyscalls ()은 이러한 이벤트의 데이터를 상호 연결 하 고 쌍을 사용할 수 있게 될 때이를 제공 합니다. 추적을 통해 상관 관계가 지정 된 데이터의 몇 가지 유형이 있습니다. UseStreaming():

| 코드                                        | Description                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| 추적. UseStreaming(). UseContextSwitchData() | 압축 및 비 압축 이벤트에서 상호 관련 된 컨텍스트 스위치 데이터를 스트림 하 고 원시 비 압축 이벤트 보다 더 정확 하 게 SwitchInThreadIds. |
| 추적. UseStreaming(). UseScheduledTasks()    | 예약 된 작업 데이터와 상관 관계가 지정 된 스트림입니다.                                                                                                         |
| 추적. UseStreaming(). UseSyscalls()          | 스트림에서 상관 관계가 지정 된 시스템 호출 데이터입니다.                                                                                                            |
| 추적. UseStreaming(). UseWindowInFocus()     | 스트림이 상호 관련 된 창 포커스 데이터입니다.                                                                                                        |

## <a name="standalone-streaming-events"></a>독립 실행형 스트리밍 이벤트

또한 추적. UseStreaming ()는 여러 개의 독립 실행형 이벤트 형식에 대해 구문 분석 된 이벤트를 제공 합니다.

| 코드                                               | Description                                     |
|----------------------------------------------------|-------------------------------------------------|
| 추적. UseStreaming(). UseLastBranchRecordEvents()   | 스트림이 LBR (last branch record) 이벤트를 구문 분석 했습니다. |
| 추적. UseStreaming(). UseReadyThreadEvents()        | 스트림이 준비 된 스레드 이벤트를 구문 분석 했습니다.             |
| 추적. UseStreaming(). UseThreadCreateEvents ()       | 스트림은 스레드 생성 이벤트를 구문 분석 합니다.            |
| 추적. UseStreaming(). UseThreadExitEvents()         | Stream은 스레드 종료 이벤트를 구문 분석 합니다.              |
| 추적. UseStreaming(). UseThreadRundownStartEvents () | 스트림이 구문 분석 된 스레드 런다운 시작 이벤트입니다.     |
| 추적. UseStreaming(). UseThreadRundownStopEvents ()  | 스트림은 스레드 런다운 중지 이벤트를 구문 분석 합니다.      |
| 추적. UseStreaming(). UseThreadSetNameEvents()      | 스트림은 스레드 집합 이름 이벤트를 구문 분석 합니다.          |

## <a name="underlying-streaming-events-for-correlated-data"></a>상관 관계가 지정 된 데이터에 대 한 기본 스트리밍 이벤트

마지막으로 추적입니다. UseStreaming ()는 위의 목록에서 데이터를 상호 연결 하는 데 사용 되는 기본 이벤트도 제공 합니다. 이러한 기본 이벤트는 다음과 같습니다.

| 코드                                                        | Description                                                                                | 포함된 운영 체제                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| 추적. UseStreaming(). UseCompactContextSwitchEvents()        | 스트림은 압축 컨텍스트 전환 이벤트를 구문 분석 합니다.                                              | 추적. UseStreaming(). UseContextSwitchData() |
| 추적. UseStreaming(). UseContextSwitchEvents()               | 스트림을 구문 분석 한 컨텍스트 전환 이벤트입니다. 일부 경우에는 SwitchInThreadIds 정확 하지 않을 수 있습니다. | 추적. UseStreaming(). UseContextSwitchData() |
| 추적. UseStreaming(). UseFocusChangeEvents()                 | 스트림 구문 분석 창 포커스 변경 이벤트입니다.                                                 | 추적. UseStreaming(). UseWindowInFocus()     |
| 추적. UseStreaming(). UseScheduledTaskStartEvents()          | 스트림이 예약 된 작업 시작 이벤트를 구문 분석 했습니다.                                                | 추적. UseStreaming(). UseScheduledTasks()    |
| 추적. UseStreaming(). UseScheduledTaskStopEvents()           | 스트림이 예약 된 작업 중지 이벤트를 구문 분석 했습니다.                                                 | 추적. UseStreaming(). UseScheduledTasks()    |
| 추적. UseStreaming(). UseScheduledTaskTriggerEvents()        | 스트림이 예약 된 작업 트리거 이벤트를 구문 분석 했습니다.                                              | 추적. UseStreaming(). UseScheduledTasks()    |
| 추적. UseStreaming(). UseSessionLayerSetActiveWindowEvents() | 스트림이 구문 분석 된 세션 계층은 활성 창 이벤트를 설정 합니다.                                     | 추적. UseStreaming(). UseWindowInFocus()     |
| 추적. UseStreaming(). UseSyscallEnterEvents ()                | 스트림이 syscall enter 이벤트를 구문 분석 했습니다.                                                       | 추적. UseStreaming(). UseSyscalls()          |
| 추적. UseStreaming(). UseSyscallExitEvents ()                 | 스트림이 syscall 종료 이벤트를 구문 분석 했습니다.                                                        | 추적. UseStreaming(). UseSyscalls()          |

## <a name="next-steps"></a>다음 단계

이 자습서에서는 스트리밍을 사용 하 여 더 작은 메모리를 사용 하는 즉시 추적 데이터에 액세스 하는 방법을 배웠습니다.

다음 단계는 추적에서 원하는 데이터에 대 한 액세스를 확인 하는 것입니다. [샘플](https://github.com/microsoft/eventtracing-processing-samples) 에서 몇 가지 아이디어를 살펴보세요. 모든 추적에 지원 되는 모든 데이터 형식이 포함 되는 것은 아닙니다.