---
author: normesta
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: 정기 작업 항목 만들기
description: 주기적으로 반복되는 작업 항목을 만드는 방법을 알아봅니다.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 정기 작업 항목, 스레딩, 타이머
ms.localizationpriority: medium
ms.openlocfilehash: 4afa137b01738c42f8e15c95ef09ec921d1e44ae
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7570316"
---
# <a name="create-a-periodic-work-item"></a>정기 작업 항목 만들기


** 중요 API **

-   [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915)
-   [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587)

주기적으로 반복되는 작업 항목을 만드는 방법을 알아봅니다.

## <a name="create-the-periodic-work-item"></a>주기적 작업 항목 만들기

[**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) 메서드를 사용하여 주기적 작업 항목을 만듭니다. 작업을 수행하는 람다를 제공하고 *period* 매개 변수를 사용하여 제출 간격을 지정합니다. 이 기간은 [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996) 구조를 사용하여 지정합니다. 기간이 경과할 때마다 작업 항목이 다시 제출되므로 작업을 완료할 만큼 기간이 긴지 확인합니다.

[**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.createtimer.aspx)는 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587) 개체를 반환합니다. 타이머를 취소해야 하는 경우를 위해 이 개체를 저장합니다.

> **참고**값으로 0 지정 하지 마세요 (또는 1 밀리초 보다 작은 값) 간격입니다. 지정하면 주기적 타이머가 대신 일회성 타이머로 동작합니다.

> **참고** [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) UI에 액세스 하 고 작업 항목의 진행률을 표시 하는 데 사용할 수 있습니다.

다음 예제에서는 60초마다 실행되는 작업 항목을 만듭니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>주기적 작업 항목의 취소 처리(옵션)

필요한 경우 [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)를 사용하여 주기적 타이머의 취소를 처리할 수 있습니다. [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) 오버로드를 사용하여 주기적 작업 항목의 취소를 처리하는 추가 람다를 제공합니다.

다음 예제에서는 60초마다 반복되는 주기적 작업 항목을 만들고 취소 처리기도 제공합니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>타이머 취소

필요한 경우 [**Cancel**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.cancel.aspx) 메서드를 호출하여 주기적 작업 항목이 반복되지 않도록 합니다. 주기적 타이머를 취소할 때 작업 항목이 실행 중이면 작업 항목을 완료할 수 있습니다. 주기적 작업 항목의 모든 인스턴스가 완료되면 [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)(제공된 경우)가 호출됩니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>설명

일회성 타이머에 대한 자세한 내용은 [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [스레드 풀을 사용하기 위한 모범 사례](best-practices-for-using-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)
 
