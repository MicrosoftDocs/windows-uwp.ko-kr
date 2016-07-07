---
author: TylerMSFT
title: "취소된 백그라운드 작업 처리"
description: "영구적 저장소를 통해 앱에 취소를 보고하여 취소 요청을 인식하고 작업을 중지하는 백그라운드 작업을 만드는 방법을 알아봅니다."
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: ab575415e5e6a091fb45dab49af21d0552834406

---

# 취소된 백그라운드 작업 처리

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요 API**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

영구적 저장소를 통해 앱에 취소를 보고하여 취소 요청을 인식하고 작업을 중지하는 백그라운드 작업을 만드는 방법을 알아봅니다.

> **참고** 데스크톱을 제외한 모든 디바이스 패밀리의 경우 디바이스의 메모리가 부족해지면 백그라운드 작업이 종료될 수 있습니다. 메모리 부족 예외가 표시되지 않거나 앱에서 처리하지 않는 경우 백그라운드 작업이 OnCanceled 이벤트를 발생시키지 않고 경고 없이 종료됩니다. 이는 포그라운드에서 앱의 사용자 환경을 확인하는 데 도움이 됩니다. 백그라운드 작업은 이 시나리오를 처리하도록 설계되어야 합니다.

이 항목에서는 백그라운드 작업 진입점으로 사용되는 Run 메서드를 비롯하여 백그라운드 작업 클래스를 이미 만들었다고 가정합니다. 백그라운드 작업을 빠르게 작성하려면 [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요. 조건 및 트리거에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

## OnCanceled 메서드를 사용하여 취소 요청 인식

취소 이벤트를 처리하는 메서드를 씁니다.

다음과 같은 공간을 가진 OnCanceled 메서드를 만듭니다. 이 메서드는 백그라운드 작업에 대한 취소 요청이 생성될 때마다 Windows 런타임에서 호출되는 진입점입니다.

OnCanceled 메서드는 다음과 같은 공간이 있어야 합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
>    private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```
> ```cpp
>    void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```

[!div class="tabbedCodeSnippets"] **\_CancelRequested**라는 플래그 변수를 백그라운드 작업 클래스에 추가합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
>   volatile bool _CancelRequested = false;
> ```
> ```cpp
>   private:
>     volatile bool CancelRequested;
> ```

이 변수는 취소가 요청된 시점을 나타내는 데 사용됩니다.

[!div class="tabbedCodeSnippets"]

> [!div class="tabbedCodeSnippets"]
> ```cs
>     private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         _cancelRequested = true;
>
>         Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
>     }
> ```
> ```cpp
>     void SampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         CancelRequested = true;
>     }
> ```

1단계에서 만든 OnCanceled 메서드에서 **\_CancelRequested** 플래그 변수를 **true**로 설정합니다. 전체 [백그라운드 작업 샘플]( http://go.microsoft.com/fwlink/p/?linkid=227509) OnCanceled 메서드에서는 **\_CancelRequested**를 **true**로 설정하고 잠재적으로 유용한 디버그 출력을 씁니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> ```
> ```cpp
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
> ```

## [!div class="tabbedCodeSnippets"]


백그라운드 작업의 Run 메서드에서 작업을 시작하기 전에 OnCanceled 이벤트 처리기 메서드를 등록합니다.

예를 들면 다음 코드 줄을 사용합니다. [!div class="tabbedCodeSnippets"]

Run 메서드를 종료하여 취소 처리

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```

> 취소 요청이 수신되면 Run 메서드는 **\_cancelRequested**가 **true**로 설정되는 것을 인식하여 작업을 중지하고 종료해야 합니다. 작업 중인 동안 플래그 변수를 확인하도록 백그라운드 작업 클래스 코드를 수정합니다.

**\_cancelRequested**가 true로 설정된 경우 작업을 중지합니다.

[백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)(영문)에는 백그라운드 작업이 취소될 경우 추기적 타이머 콜백을 중지하는 검사가 포함되어 있습니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.TaskId.ToString();
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>
>         if (_cancelRequested)
>         {
>             settings.Values[key] = "Canceled";
>         }
>         else
>         {
>             settings.Values[key] = "Completed";
>         }
>         
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
>         
>         //
>         // Indicate that the background task has completed.
>         //
>
>         _deferral.Complete();
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>         
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         
>         auto settings = ApplicationData::Current->LocalSettings;
>         auto key = TaskInstance->Task->Name;
>         settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
>         
>         //
>         // Indicate that the background task has completed.
>         //
>         
>         Deferral->Complete();
>     }
> ```

## [!div class="tabbedCodeSnippets"]

**참고** 위에 표시된 코드 샘플에서는 백그라운드 작업 진행률을 기록하는 데 사용 중인 [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797).[**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) 속성을 사용합니다.

[**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 클래스를 사용하여 진행률이 앱에 다시 보고됩니다.

## 작업을 중지한 후에 작업이 완료되었는지 취소되었는지 여부를 기록하도록 Run 메서드를 수정합니다.

[백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)(영문)에서는 LocalSettings에 상태를 기록합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // The Run method is the entry point of a background task.
> //
> public void Run(IBackgroundTaskInstance taskInstance)
> {
>     Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");
>
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
>     var settings = ApplicationData.Current.LocalSettings;
>     settings.Values["BackgroundWorkCost"] = cost.ToString();
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance;
>     //
>     _deferral = taskInstance.GetDeferral();
>     _taskInstance = taskInstance;
>
>     _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
> }
>
> //
> // Simulate the background task activity.
> //
> private void PeriodicTimerCallback(ThreadPoolTimer timer)
> {
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.Name;
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);
>
>         //
>         // Indicate that the background task has completed.
>         //
>         _deferral.Complete();
>     }
> }
> ```
> ```cpp
> void SampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
> {
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
>     auto settings = ApplicationData::Current->LocalSettings;
>     settings->Values->Insert("BackgroundWorkCost", cost.ToString());
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance.
>     //
>     TaskDeferral = taskInstance->GetDeferral();
>     TaskInstance = taskInstance;
>
>     auto timerDelegate = [this](ThreadPoolTimer^ timer)
>     {
>         if ((CancelRequested == false) &&
>             (Progress < 100))
>         {
>             Progress += 10;
>             TaskInstance->Progress = Progress;
>         }
>         else
>         {
>             PeriodicTimer->Cancel();
>
>             //
>             // Write to LocalSettings to indicate that this background task ran.
>             //
>             auto settings = ApplicationData::Current->LocalSettings;
>             auto key = TaskInstance->Task->Name;
>             settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");
>
>             //
>             // Indicate that the background task has completed.
>             //
>             TaskDeferral->Complete();
>         }
>     };
>
>     TimeSpan period;
>     period.Duration = 1000 * 10000; // 1 second
>     PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
> }
> ```

> [!div class="tabbedCodeSnippets"] 설명

## [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)을 다운로드하여 메서드 컨텍스트에서 이러한 코드 예제를 확인할 수 있습니다.

* [설명을 위해 샘플 코드에서는 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)에서 Run 메서드와 콜백 타이머의 일부만 표시합니다.](create-and-register-a-background-task.md)
* [Run 메서드 예](declare-background-tasks-in-the-application-manifest.md)
* [컨텍스트에 대한 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)(영문)의 전체 Run 메서드 및 타이머 콜백 코드는 아래와 같습니다.](guidelines-for-background-tasks.md)
* [[!div class="tabbedCodeSnippets"]](monitor-background-task-progress-and-completion.md)
* [**참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다.](register-a-background-task.md)
* [Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.](respond-to-system-events-with-background-tasks.md)
* [관련 항목](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 만들기 및 등록](set-conditions-for-running-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](update-a-live-tile-from-a-background-task.md)
* [백그라운드 작업 지침](use-a-maintenance-trigger.md)

* [백그라운드 작업 진행 및 완료 모니터링](debug-a-background-task.md)
* [백그라운드 작업 등록](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Jun16_HO5-->


