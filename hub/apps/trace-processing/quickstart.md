---
title: 빠른 시작 프로세스 추적-.NET TraceProcessing
description: 이 빠른 시작에서는 ETW 추적의 데이터에 액세스 하는 방법에 대해 알아봅니다.
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 162646baff9b2d08f6fc0ea4862802216cff9619
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629105"
---
# <a name="quickstart-process-your-first-trace"></a>빠른 시작: 첫 번째 추적 처리

ETW(Windows용 이벤트 추적) (ETW) 추적의 데이터에 액세스 하려면 TraceProcessor를 사용해 보세요. TraceProcessor를 사용 하면 .NET 개체로 ETW 추적 데이터에 액세스할 수 있습니다.

이 빠른 시작에서는 다음 방법에 대해 알아봅니다.

1. TraceProcessing NuGet 패키지를 설치 합니다.
2. TraceProcessor를 만듭니다.
3. TraceProcessor를 사용 하 여 추적에 포함 된 프로세스 명령줄에 액세스 합니다.

## <a name="prerequisites"></a>필수 조건

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>TraceProcessing NuGet 패키지를 설치 합니다.

.NET TraceProcessing은 다음 패키지 ID를 사용 하 여 [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) 에서 사용할 수 있습니다.

Microsoft. Windows. EventTracing

콘솔 앱에서이 패키지를 사용 하 여 ETW 추적 (.etl 파일)에 포함 된 프로세스 명령줄을 나열할 수 있습니다.

1. 새 .NET Core 콘솔 앱을 만듭니다. Visual Studio에서 파일, 새로 만들기, 프로젝트 ...를 차례로 선택 하 고에 대 한 C#콘솔 앱 (.net Core) 템플릿을 선택 합니다.

    프로젝트 이름 (예: TraceProcessorQuickstart)을 입력 하 고 만들기를 선택 합니다.

2. 솔루션 탐색기에서 종속성을 마우스 오른쪽 단추로 클릭 하 고 NuGet 패키지 관리 ...를 선택 합니다. 찾아보기 탭으로 전환 합니다.

3. 검색 상자에 Microsoft. w i m a. w i m a. l i v a. 모두 및 검색을 입력 합니다.

    해당 이름을 사용 하 여 NuGet 패키지에서 설치를 선택 하 고 NuGet 창을 닫습니다.

## <a name="create-a-traceprocessor"></a>TraceProcessor 만들기

1. Program.cs를 다음 내용으로 변경 합니다.

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
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. 프로젝트를 실행할 때 사용할 추적 이름을 제공 합니다.

    솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 속성을 선택 합니다. 디버그 탭으로 전환 하 고 응용 프로그램 인수에 추적 (.etl 파일)의 경로를 입력 합니다.

    추적 파일이 아직 없는 경우 [Windows 성능 레코더](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording) 를 사용 하 여 만들 수 있습니다.

3. 응용 프로그램을 실행합니다.

    디버그, 디버깅 하지 않고 시작을 선택 하 여 코드를 실행 합니다.

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>TraceProcessor를 사용 하 여 추적에 포함 된 프로세스 명령줄에 액세스

1. Program.cs를 다음 내용으로 변경 합니다.

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

2. 응용 프로그램을 다시 실행 합니다.

    이번에는 추적을 기록 하는 동안 실행 중 이었던 모든 프로세스의 목록 명령줄이 표시 됩니다.

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 콘솔 응용 프로그램을 만들고, TraceProcessor를 설치 하 고,이를 사용 하 여 ETW 추적에서 프로세스 명령줄에 액세스 했습니다. 이제 추적 데이터에 액세스 하는 응용 프로그램이 있습니다.

프로세스 정보는 응용 프로그램이 액세스할 수 있는 ETW 추적에 저장 된 많은 종류의 데이터 중 하나일 뿐입니다.

다음 단계는 TraceProcessor 및 액세스할 수 있는 다른 데이터 소스를 [더 자세히 확인](tutorial.md) 하는 것입니다.
