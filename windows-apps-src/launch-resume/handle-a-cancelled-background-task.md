---
author: TylerMSFT
title: 취소된 백그라운드 작업 처리
description: 영구적 저장소를 통해 앱에 취소를 보고하여 취소 요청을 인식하고 작업을 중지하는 백그라운드 작업을 만드는 방법을 알아봅니다.
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 91de18af818113d79564ee8dfba7519a0f131246
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5921489"
---
# <a name="handle-a-cancelled-background-task"></a>취소된 백그라운드 작업 처리

**중요 API**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

영구적 저장소를 통해 앱에 취소를 보고하여 취소 요청을 인식하고 작업을 중지하는 백그라운드 작업을 만드는 방법을 알아봅니다.

이 항목에서는 백그라운드 작업 진입점으로 사용 되는 **Run** 메서드를 포함 하 여 백그라운드 작업 클래스를 이미 만든 가정 합니다. 백그라운드 작업 구축을 빠르게 시작하려면 [ 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md) 또는 [ 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)을 참조하세요. 조건 및 트리거에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

이 항목은 In-process 백그라운드 작업에도 적용할 수 있습니다. 그러나 **Run** 메서드 대신 **OnBackgroundActivated**로 대체 합니다. In-process 백그라운드 작업은 백그라운드 작업이 포그라운드 앱과 같은 프로세스에서 실행되므로 앱 상태를 사용하여 취소 통신이 가능하기 때문에 취소 신호를 보내기 위해 영구적 저장소를 사용할 필요가 없습니다.

## <a name="use-the-oncanceled-method-to-recognize-cancellation-requests"></a>OnCanceled 메서드를 사용하여 취소 요청 인식

취소 이벤트를 처리하는 메서드를 씁니다.

> [!NOTE]
> 데스크톱을 제외한 모든 디바이스 패밀리의 경우 장치의 메모리가 부족해지면 백그라운드 작업이 종료될 수 있습니다. 메모리 부족 예외가 표시 되지, 앱에서 처리 하지 않는 경우, 다음 백그라운드 작업이 종료 됩니다 경고 없이 OnCanceled 이벤트를 발생 하지 않고. 이는 포그라운드에서 앱의 사용자 환경을 확인하는 데 도움이 됩니다. 백그라운드 작업은 이 시나리오를 처리하도록 설계되어야 합니다.

다음과 같이 **OnCanceled**라는 메서드를 만듭니다. 이 메서드는 백그라운드 작업에 대한 취소 요청이 생성될 때 Windows 런타임에서 호출되는 진입점입니다.

```csharp
private void OnCanceled(
    IBackgroundTaskInstance sender,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(
    IBackgroundTaskInstance^ taskInstance,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

**\_CancelRequested**라는 플래그 변수를 백그라운드 작업 클래스에 추가합니다. 이 변수는 취소가 요청된 시점을 나타내는 데 사용됩니다.

```csharp
volatile bool _CancelRequested = false;
```

```cppwinrt
private:
    volatile bool m_cancelRequested;
```

```cpp
private:
    volatile bool CancelRequested;
```

1 단계에서 만든 **OnCanceled** 메서드에서 **\_CancelRequested** 플래그 변수 **true로**설정 합니다.

**OnCanceled** 메서드 전체 [백그라운드 작업 샘플]( http://go.microsoft.com/fwlink/p/?linkid=227509) **\_CancelRequested** 를 **true** 로 설정 하 고 잠재적으로 유용한 디버그 출력을 씁니다.

```csharp
private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    _cancelRequested = true;

    Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    m_cancelRequested = true;
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    CancelRequested = true;
}
```

백그라운드 작업의 **Run** 메서드 작업을 시작 하기 전에 **OnCanceled** 이벤트 처리기 메서드를 등록 합니다. In-process 백그라운드 작업에서 응용 프로그램 초기화의 일부로 이 등록을 수행할 수 있습니다. 예를 들어 다음 코드 줄을 사용 합니다.

```csharp
taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
```

```cppwinrt
taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });
```

```cpp
taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);
```

## <a name="handle-cancellation-by-exiting-your-background-task"></a>백그라운드 작업을 종료하여 취소 처리

취소 요청이 수신되면 백그라운드 작업을 수행하는 메서드는 **\_cancelRequested**가 **true**로 설정되는 것을 인식하여 작업을 중지하고 종료해야 합니다. In-process 백그라운드 작업의 경우 **OnBackgroundActivated** 메서드에서 반환을 의미 합니다. Out of process 백그라운드 작업 **실행** 메서드에서 반환을 의미 합니다.

작업 중인 동안 플래그 변수를 확인하도록 백그라운드 작업 클래스 코드를 수정합니다. **\_CancelRequested** 인덱싱이 true이 고, 중지 작업으로 설정 됩니다.

[백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666) 백그라운드 작업이 취소 될 경우 추기 적 타이머 콜백을 중지 하는 검사가 포함 되어 있습니다.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

> [!NOTE]
> 위에 표시 된 코드 샘플 [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)를 사용 합니다. 백그라운드 작업 진행률을 기록 하는 데 사용 되는 [**진행률**](https://msdn.microsoft.com/library/windows/apps/br224800) 속성입니다. [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 클래스를 사용하여 진행률이 앱에 다시 보고됩니다.

작업이 완료 또는 취소 되었는지 여부를 기록 작업을 중지 한 후 **Run** 메서드를 수정 합니다. 백그라운드 작업이 취소되면 프로세스 간에 통신할 방법이 필요하므로 이 단계는 Out-of-process 백그라운드 작업에 적용됩니다. In-process 백그라운드 작업에서는 단순하게 응용 프로그램과 상태를 공유하여 작업이 취소되었음을 나타낼 수 있습니다.

[백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666) 에서는 LocalSettings에 상태를 기록 합니다.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();

    var settings = ApplicationData.Current.LocalSettings;
    var key = _taskInstance.Task.TaskId.ToString();

    // Write to LocalSettings to indicate that this background task ran.
    if (_cancelRequested)
    {
        settings.Values[key] = "Canceled";
    }
    else
    {
        settings.Values[key] = "Completed";
    }
        
    Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
        
    // Indicate that the background task has completed.
    _deferral.Complete();
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();

    // Write to LocalSettings to indicate that this background task ran.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    auto key{ m_taskInstance.Task().Name() };
    settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

    // Indicate that the background task has completed.
    m_deferral.Complete();
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
        
    // Write to LocalSettings to indicate that this background task ran.
    auto settings = ApplicationData::Current->LocalSettings;
    auto key = TaskInstance->Task->Name;
    settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
        
    // Indicate that the background task has completed.
    Deferral->Complete();
}
```

## <a name="remarks"></a>설명

[백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)을 다운로드하여 메서드 컨텍스트에서 이러한 코드 예제를 확인할 수 있습니다.

설명을 위해 샘플 코드는 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)에서 **Run** 메서드 (및 콜백 타이머)의 일부만 보여줍니다.

## <a name="run-method-example"></a>Run 메서드 예

전체 **Run** 메서드 및 타이머 콜백 코드, [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666) 은 아래 표시 된 컨텍스트에 대 한 합니다.

```csharp
// The Run method is the entry point of a background task.
public void Run(IBackgroundTaskInstance taskInstance)
{
    Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");

    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
    var settings = ApplicationData.Current.LocalSettings;
    settings.Values["BackgroundWorkCost"] = cost.ToString();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance;
    _deferral = taskInstance.GetDeferral();
    _taskInstance = taskInstance;

    _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
}

// Simulate the background task activity.
private void PeriodicTimerCallback(ThreadPoolTimer timer)
{
    if ((_cancelRequested == false) && (_progress < 100))
    {
        _progress += 10;
        _taskInstance.Progress = _progress;
    }
    else
    {
        _periodicTimer.Cancel();

        var settings = ApplicationData.Current.LocalSettings;
        var key = _taskInstance.Task.Name;

        // Write to LocalSettings to indicate that this background task ran.
        settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
        Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);

        // Indicate that the background task has completed.
        _deferral.Complete();
    }
}
```

```cppwinrt
void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost{ Windows::ApplicationModel::Background::BackgroundWorkCost::CurrentBackgroundWorkCost() };
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    std::wstring costAsString{ L"Low" };
    if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::Medium) costAsString = L"Medium";
    else if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::High) costAsString = L"High";
    settings.Values().Insert(L"BackgroundWorkCost", winrt::box_value(costAsString));

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    m_deferral = taskInstance.GetDeferral();
    m_taskInstance = taskInstance;

    Windows::Foundation::TimeSpan period{ std::chrono::seconds{1} };
    m_periodicTimer = Windows::System::Threading::ThreadPoolTimer::CreatePeriodicTimer([this](Windows::System::Threading::ThreadPoolTimer timer)
    {
        if (!m_cancelRequested && m_progress < 100)
        {
            m_progress += 10;
            m_taskInstance.Progress(m_progress);
        }
        else
        {
            m_periodicTimer.Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
            auto key{ m_taskInstance.Task().Name() };
            settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

            // Indicate that the background task has completed.
            m_deferral.Complete();
        }
    }, period);
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
    auto settings = ApplicationData::Current->LocalSettings;
    settings->Values->Insert("BackgroundWorkCost", cost.ToString());

    // Associate a cancellation handler with the background task.
    taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    TaskDeferral = taskInstance->GetDeferral();
    TaskInstance = taskInstance;

    auto timerDelegate = [this](ThreadPoolTimer^ timer)
    {
        if ((CancelRequested == false) &&
            (Progress < 100))
        {
            Progress += 10;
            TaskInstance->Progress = Progress;
        }
        else
        {
            PeriodicTimer->Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings = ApplicationData::Current->LocalSettings;
            auto key = TaskInstance->Task->Name;
            settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");

            // Indicate that the background task has completed.
            TaskDeferral->Complete();
        }
    };

    TimeSpan period;
    period.Duration = 1000 * 10000; // 1 second
    PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
}
```

## <a name="related-topics"></a>관련 항목

- [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md).
- [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
- [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
- [백그라운드 작업 지침](guidelines-for-background-tasks.md)
- [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
- [백그라운드 작업 등록](register-a-background-task.md)
- [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
- [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
- [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
- [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
- [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
- [백그라운드 작업 디버그](debug-a-background-task.md)
- [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)
