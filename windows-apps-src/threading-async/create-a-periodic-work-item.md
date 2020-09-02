---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: 정기 작업 항목 만들기
description: 유니버설 Windows 플랫폼 (UWP) Windows.system.threading.threadpooltimer API의 CreatePeriodicTimer 메서드를 사용 하 여 주기적으로 반복 되는 작업 항목을 만드는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 정기 작업 항목, 스레딩, 타이머
ms.localizationpriority: medium
ms.openlocfilehash: e1b50858b2c7e3ce4cd60f9401cedb75eb950c7d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304445"
---
# <a name="create-a-periodic-work-item"></a>정기 작업 항목 만들기


<b>중요 API</b>

-   [**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)
-   [**Windows.system.threading.threadpooltimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer)

정기적으로 반복 되는 작업 항목을 만드는 방법에 대해 알아봅니다.

## <a name="create-the-periodic-work-item"></a>정기 작업 항목 만들기

[**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) 메서드를 사용 하 여 정기적인 작업 항목을 만듭니다. 작업을 수행 하는 람다를 제공 하 고 *period* 매개 변수를 사용 하 여 제출 간 간격을 지정 합니다. 기간은 [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan) 구조를 사용 하 여 지정 됩니다. 작업 항목은 기간이 경과할 때마다 다시 전송 되므로 작업이 완료 될 때까지 충분 한지 확인 해야 합니다.

[**Createtimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) 는 [**windows.system.threading.threadpooltimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) 개체를 반환 합니다. 타이머를 취소 해야 하는 경우이 개체를 저장 합니다.

> **참고**    간격에 0 (또는 1 밀리초 미만) 값을 지정 하지 마십시오. 이렇게 하면 주기적인 타이머가 단일 샷 타이머로 작동 합니다.

> **참고**    [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) 를 사용 하 여 UI에 액세스 하 고 작업 항목에서 진행률을 표시할 수 있습니다.

다음 예제에서는 60 초 마다 한 번씩 실행 되는 작업 항목을 만듭니다.

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

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>정기 작업 항목 취소를 처리 합니다 (선택 사항).

필요한 경우 [**TimerDestroyedHandler**](/uwp/api/windows.system.threading.timerdestroyedhandler)를 사용 하 여 주기적 타이머의 취소를 처리할 수 있습니다. [**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) 오버 로드를 사용 하 여 정기 작업 항목의 취소를 처리 하는 추가 람다를 제공 합니다.

다음 예제에서는 60 초 마다 반복 되는 주기적 작업 항목을 만들고 취소 처리기도 제공 합니다.

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

필요한 경우 [**취소**](/uwp/api/windows.system.threading.threadpooltimer.cancel) 메서드를 호출 하 여 정기적으로 작업 항목을 반복 하지 않도록 합니다. 정기 타이머가 취소 될 때 작업 항목을 실행 하는 경우에는 작업을 완료할 수 있습니다. [**TimerDestroyedHandler**](/uwp/api/windows.system.threading.timerdestroyedhandler) (제공 된 경우)는 정기적인 작업 항목의 모든 인스턴스가 완료 될 때 호출 됩니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>설명

단일 사용 타이머에 대 한 자세한 내용은 [타이머를 사용 하 여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [스레드 풀을 사용하기 위한 모범 사례](best-practices-for-using-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)
 