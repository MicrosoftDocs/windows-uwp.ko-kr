---
title: 기호 로드-.NET TraceProcessing
description: 이 자습서에서는 추적을 처리할 때 기호를 로드 하는 방법에 대해 알아봅니다.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 72264b51edcc0b02aa335b8766100c196a0d5090
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168817"
---
# <a name="use-symbols-in-net-traceprocessing"></a>.NET TraceProcessing에서 기호 사용

[Traceprocessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) 는 여러 데이터 원본에서 기호를 로드 하 고 스택을 가져오는 것을 지원 합니다. 다음 콘솔 응용 프로그램은 CPU 샘플을 살펴보고 특정 함수가 실행 되 고 있던 예상 기간 (추적의 CPU 사용량에 따라)을 출력 합니다.

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

이 프로그램을 실행 하면 다음과 유사한 출력이 생성 됩니다.

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

출력 세부 정보는 추적에 따라 달라 집니다.

## <a name="symbols-format"></a>기호 형식

내부적으로 TraceProcessor는 PDB에 저장 된 일부 데이터의 캐시 인 [symcache](/windows-hardware/test/wpt/loading-symbols#symcache-path) 형식을 사용 합니다. 기호를 로드할 때 TraceProcessor는 이러한 SymCache 파일 (SymCache 경로)에 사용할 위치를 지정 해야 하며, 선택적으로 Pdb에 액세스 하기 위한 기호 경로를 지정할 수 있도록 지원 합니다. 기호 경로가 제공 되는 경우 TraceProcessor는 필요에 따라 PDB 파일에서 SymCache 파일을 만들며, 이후에 동일한 데이터를 처리 하면 SymCache 파일을 직접 사용 하 여 성능을 향상 시킬 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 추적을 처리할 때 기호를 로드 하는 방법을 배웠습니다.

다음 단계는 메모리의 모든 항목을 버퍼링 하지 않고 [스트리밍을 사용](streaming.md) 하 여 추적 데이터에 액세스 하는 방법을 배우는 것입니다.