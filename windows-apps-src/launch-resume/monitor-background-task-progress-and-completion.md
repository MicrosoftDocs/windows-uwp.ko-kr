---
title: 백그라운드 작업 진행 및 완료 모니터링
description: 앱이 백그라운드 작업에서 보고 한 진행률 및 완료를 인식할 수 있는 방법에 대해 알아봅니다.
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 9c6fb40d64d0826a5cbbfa7c97694e6650df9dbe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172977"
---
# <a name="monitor-background-task-progress-and-completion"></a>백그라운드 작업 진행 및 완료 모니터링

**중요 API**

- [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)
- [**BackgroundTaskProgressEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskprogresseventhandler)
- [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

앱이 out-of-process로 실행 되는 백그라운드 작업에서 보고 한 진행률 및 완료를 인식할 수 있는 방법에 대해 알아봅니다. In-process 백그라운드 작업의 경우 진행률 및 완료를 나타내기 위해 공유 변수를 설정할 수 있습니다.

백그라운드 작업 진행률 및 완료를 앱 코드에서 모니터링할 수 있습니다. 이렇게 하기 위해 앱은 시스템에 등록 한 백그라운드 작업에서 이벤트를 구독 합니다.

- 이 항목에서는 백그라운드 작업을 등록 하는 앱이 있다고 가정 합니다. 백그라운드 작업을 빠르게 빌드하기 시작 하려면 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조 하세요. 조건 및 트리거에 대 한 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)을 참조 하세요.

## <a name="create-an-event-handler-to-handle-completed-background-tasks"></a>완료 된 백그라운드 작업을 처리 하는 이벤트 처리기 만들기

### <a name="step-1"></a>1단계
완료 된 백그라운드 작업을 처리 하는 이벤트 처리기 함수를 만듭니다. 이 코드는 [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) 개체 및 [**BackgroundTaskCompletedEventArgs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskCompletedEventArgs) 개체를 사용 하는 특정 공간을 따라야 합니다.

**Oncompleted** background task 이벤트 처리기 메서드에 대해 다음 공간을 사용 합니다.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    // TODO: Add code that deals with background task completion.
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task completion.
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    // TODO: Add code that deals with background task completion.
};
```

### <a name="step-2"></a>2단계
백그라운드 작업 완료를 처리 하는 이벤트 처리기에 코드를 추가 합니다.

예를 들어 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) 은 UI를 업데이트 합니다.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    UpdateUI();
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    UpdateUI();
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{    
    UpdateUI();
};
```

## <a name="create-an-event-handler-function-to-handle-background-task-progress"></a>백그라운드 작업 진행률을 처리 하는 이벤트 처리기 함수 만들기

### <a name="step-1"></a>1단계
완료 된 백그라운드 작업을 처리 하는 이벤트 처리기 함수를 만듭니다. 이 코드는 [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) 개체 및 [**BackgroundTaskProgressEventArgs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskProgressEventArgs) 개체를 사용 하는 특정 공간을 따라야 합니다.

OnProgress background 작업 이벤트 처리기 메서드에 다음 공간을 사용 합니다.

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    // TODO: Add code that deals with background task progress.
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task progress.
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    // TODO: Add code that deals with background task progress.
};
```

### <a name="step-2"></a>2단계
백그라운드 작업 완료를 처리 하는 이벤트 처리기에 코드를 추가 합니다.

예를 들어 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) 은 *args* 매개 변수를 통해 전달 된 진행률 상태를 사용 하 여 UI를 업데이트 합니다.

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    var progress = "Progress: " + args.Progress + "%";
    BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    UpdateUI();
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
{
    winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    auto progress = "Progress: " + args->Progress + "%";
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
};
```

## <a name="register-the-event-handler-functions-with-new-and-existing-background-tasks"></a>새 및 기존 백그라운드 작업에 이벤트 처리기 함수를 등록 합니다.

### <a name="step-1"></a>1단계
앱이 처음으로 백그라운드 작업을 등록 하는 경우 응용 프로그램이 포그라운드에서 활성 상태인 동안 태스크가 실행 되는 경우 해당 작업에 대 한 진행률 및 완료 업데이트를 수신 하도록 등록 해야 합니다.

예를 들어 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) 은 등록 하는 각 백그라운드 작업에서 다음 함수를 호출 합니다.

```csharp
private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
{
    task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
}
```

```cppwinrt
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(Windows::ApplicationModel::Background::IBackgroundTaskRegistration const& task)
{
    auto progress{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
    {
        winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    } };

    task.Progress(progress);

    auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
    {
        UpdateUI();
    } };

    task.Completed(completed);
}
```

```cpp
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
{
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        auto progress = "Progress: " + args->Progress + "%";
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    };

    task->Progress += ref new BackgroundTaskProgressEventHandler(progress);

    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {
        UpdateUI();
    };

    task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
}
```

### <a name="step-2"></a>2단계
앱이 시작 되거나 백그라운드 작업 상태와 관련 된 새 페이지로 이동 하는 경우 현재 등록 된 백그라운드 작업 목록이 표시 되 고 진행률 및 완료 이벤트 처리기 함수에 연결 됩니다. 응용 프로그램에서 현재 등록 한 백그라운드 작업의 목록은 [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)에 유지 됩니다. [**Alltasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) 속성입니다.

예를 들어 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) 에서는 다음 코드를 사용 하 여 SampleBackgroundTask 페이지가 탐색 될 때 이벤트 처리기를 연결 합니다.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    foreach (var task in BackgroundTaskRegistration.AllTasks)
    {
        if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value);
            BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
        }
    }

    UpdateUI();
}
```

```cppwinrt
void SampleBackgroundTask::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& /* e */)
{
    // A pointer back to the main page. This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    m_rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

    for (auto const& task : allTasks)
    {
        if (task.Value().Name() == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value());
            break;
        }
    }

    UpdateUI();
}
```

```cpp
void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
{
    // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto iter = BackgroundTaskRegistration::AllTasks->First();
    auto hascur = iter->HasCurrent;
    while (hascur)
    {
        auto cur = iter->Current->Value;

        if (cur->Name == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(cur);
            break;
        }

        hascur = iter->MoveNext();
    }

    UpdateUI();
}
```

## <a name="related-topics"></a>관련 항목

* [In-process 백그라운드 작업을 만들고 등록](create-and-register-an-inproc-background-task.md)합니다.
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))