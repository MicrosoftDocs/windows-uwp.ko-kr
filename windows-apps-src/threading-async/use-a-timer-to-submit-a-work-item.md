---
author: normesta
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: 타이머를 사용하여 작업 항목 제출
description: 타이머가 경과된 후 실행되는 작업 항목을 만드는 방법을 알아봅니다.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 타이머, 스레드
ms.localizationpriority: medium
ms.openlocfilehash: d65faebfc2be0e9ed254185d00932da9a57f718b
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6200157"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>타이머를 사용하여 작업 항목 제출


** 중요 API **

-   [**Windows.UI.Core 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/BR208383)
-   [**Windows.System.Threading 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/BR229642)

타이머가 경과된 후 실행되는 작업 항목을 만드는 방법을 알아봅니다.

## <a name="create-a-single-shot-timer"></a>일회성 타이머 만들기

[**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) 메서드를 사용하여 작업 항목에 대한 타이머를 만듭니다. 작업을 수행하는 람다를 제공하고 *delay* 매개 변수를 사용하여 스레드 풀이 사용 가능한 스레드에 작업 항목을 할당하기 전에 대기해야 하는 시간을 지정합니다. 이 지연은 [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996) 구조를 사용하여 지정합니다.

> **참고** [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) UI에 액세스 하 고 작업 항목의 진행률을 표시 하는 데 사용할 수 있습니다.

다음 예제에서는 3분 후에 실행되는 작업 항목을 만듭니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     ExampleUIUpdateMethod("Timer completed.");
>
>                 }));
>
>         }), delay);
> ```

## <a name="provide-a-completion-handler"></a>완료 처리기 제공

필요한 경우 [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)를 사용하여 작업 항목의 취소와 완료를 처리합니다. [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) 오버로드를 사용하여 추가 람다를 제공합니다. 이 람다는 타이머가 취소되거나 작업 항목이 완료될 때 실행됩니다.

다음 예제에서는 작업 항목을 제출하는 타이머를 만들고, 작업 항목이 완료되거나 타이머가 취소되면 메서드를 호출합니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
>
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 });
>
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         //
>         // TODO: Handle work cancellation/completion.
>         //
>
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
>
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> completed = false;
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Work
>             //
>
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>             completed = true;
>
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle work cancellation/completion.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // Update the UI thread by using the UI core dispatcher.
>                     //
>
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>타이머 취소

타이머가 여전히 카운트다운되고 있지만 작업 항목이 더 이상 필요하지 않은 경우 [**Cancel**](https://msdn.microsoft.com/library/windows/apps/BR230588)을 호출합니다. 타이머가 취소되고 작업 항목이 스레드 풀에 제출되지 않습니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>설명

**Thread.Sleep**은 UI 스레드를 차단할 수 있으므로 UWP(유니버설 Windows 플랫폼) 앱에서 사용할 수 없습니다. 대신 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587)를 사용하여 작업 항목을 만들 수 있습니다. 이 메서드는 UI 스레드를 차단하지 않고 작업 항목이 수행하는 작업을 지연합니다.

작업 항목, 타이머 작업 항목 및 주기적 작업 항목을 보여 주는 전체 코드 샘플은 [스레드 풀 샘플](http://go.microsoft.com/fwlink/p/?linkid=255387)을 참조하세요. 코드 샘플은 원래 Windows8.1 용으로 작성 하지만 Windows10에서 코드를 다시 사용할 수 있습니다.

타이머 반복에 대한 자세한 내용은 [정기 작업 항목 만들기](create-a-periodic-work-item.md)를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [스레드 풀을 사용하기 위한 모범 사례](best-practices-for-using-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)
 

 
