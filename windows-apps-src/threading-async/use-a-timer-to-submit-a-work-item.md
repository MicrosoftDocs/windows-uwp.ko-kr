---
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: 타이머를 사용하여 작업 항목 제출
description: 타이머가 경과 된 후에 실행 되는 작업 항목을 만드는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 타이머, 스레드
ms.localizationpriority: medium
ms.openlocfilehash: 1b5c0982c10cde25fc5f61314c540c194d6519a2
ms.sourcegitcommit: 2dbf4a3f3473c1d3a0ad988bcbae6e75dfee3640
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82619337"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>타이머를 사용하여 작업 항목 제출


<b>중요 한 Api</b>

-   [**Windows 네임 스페이스**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
-   [**Windows. System.object 네임 스페이스**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)

타이머가 경과 된 후에 실행 되는 작업 항목을 만드는 방법에 대해 알아봅니다.

## <a name="create-a-single-shot-timer"></a>일회성 타이머 만들기

[**Createtimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) 메서드를 사용 하 여 작업 항목에 대 한 타이머를 만듭니다. 작업을 수행 하는 람다를 제공 하 고 *delay* 매개 변수를 사용 하 여 스레드 풀이 작업 항목을 사용 가능한 스레드에 할당할 수 있을 때까지 대기 하는 시간을 지정 합니다. 지연 시간은 [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan) 구조체를 사용 하 여 지정 합니다.

> **참고**   [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 를 사용 하 여 UI에 액세스 하 고 작업 항목에서 진행률을 표시할 수 있습니다.

다음 예제에서는 3 분 내에 실행 되는 작업 항목을 만듭니다.

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

필요한 경우 [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler)를 사용 하 여 작업 항목 취소 및 완료를 처리 합니다. [**Createtimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) 오버 로드를 사용 하 여 추가 람다를 제공 합니다. 타이머가 취소 되거나 작업 항목이 완료 될 때 실행 됩니다.

다음 예제에서는 작업 항목을 전송 하는 타이머를 만들고 작업 항목이 완료 되거나 타이머가 취소 될 때 메서드를 호출 합니다.

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

타이머가 계속 계산 되지만 작업 항목이 더 이상 필요 하지 않은 경우에는 [**Cancel**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.cancel)을 호출 합니다. 타이머가 취소 되 고 작업 항목이 스레드 풀로 전송 되지 않습니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>설명

UWP (유니버설 Windows 플랫폼) 앱은 UI 스레드를 차단할 수 있으므로 스레드를 사용할 수 없습니다 **.** [**Windows.system.threading.threadpooltimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) 를 사용 하 여 작업 항목을 만들 수 있습니다. 이렇게 하면 UI 스레드를 차단 하지 않고 작업 항목에서 수행 하는 작업이 지연 됩니다.

작업 항목, 타이머 작업 항목 및 정기적인 작업 항목을 보여 주는 전체 코드 샘플은 [스레드 풀 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample) 을 참조 하세요. 코드 샘플은 원래 Windows 8.1 용으로 작성 되었지만 Windows 10에서는 코드를 다시 사용할 수 있습니다.

타이머를 반복 하는 방법에 대 한 자세한 내용은 [주기적 작업 항목 만들기](create-a-periodic-work-item.md)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [스레드 풀 사용 모범 사례](best-practices-for-using-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)
 

 
