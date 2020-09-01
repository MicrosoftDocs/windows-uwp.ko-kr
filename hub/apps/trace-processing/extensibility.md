---
title: 확장성-.NET TraceProcessing
description: 이 자습서에서는 .NET TraceProcessing을 확장 하는 방법에 대해 알아봅니다.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: f5680bdc6502c4b917667e5a59084286b445063c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166737"
---
# <a name="extend-traceprocessor"></a>TraceProcessor 확장

많은 종류의 추적 데이터에는 [Traceprocessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor)의 기본 제공 지원이 있지만 분석 하려는 다른 공급자가 있는 경우 (사용자 지정 공급자 포함) 처리를 수행 하는 동안 추적에서 해당 데이터를 사용할 수도 있습니다.

> [!NOTE]
> 이 API 부분은 미리 보기 상태 이며 활성 개발 중에 있습니다. 이후 릴리스에서 변경 될 수 있습니다.

예를 들어 추적에서 공급자 Id 목록을 가져오는 간단한 방법은 다음과 같습니다.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

다음 예제에서는 간단한 사용자 지정 데이터 원본을 보여 줍니다.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>다음 단계

이 자습서에서는 TraceProcessor를 확장 하는 방법을 배웠습니다.

다음 단계는 추적 [기호를 로드](symbols.md) 하는 방법을 설명 하는 것입니다.