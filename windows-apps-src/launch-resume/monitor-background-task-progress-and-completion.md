---
author: TylerMSFT
title: "백그라운드 작업 진행 및 완료 모니터링"
description: "앱에서 백그라운드 작업이 보고하는 진행 및 완료를 인식하는 방법을 알아봅니다."
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 704511b388a6dbcdbddf95c74108b5a463efef98
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="monitor-background-task-progress-and-completion"></a>백그라운드 작업 진행 및 완료 모니터링


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

out-of-process로 실행되는 백그라운드 작업이 보고하는 진행 및 완료를 앱에서 인식하는 방법에 대해 알아봅니다. in-process 백그라운드 작업의 경우 공유 변수를 설정하여 진행 및 완료를 나타낼 수 있습니다.

 앱 코드에 의해 백그라운드 작업 진행 및 완료를 모니터링할 수 있습니다. 이렇게 하려면 시스템에 등록된 백그라운드 작업의 이벤트에 앱을 가입합니다.

-   이 항목에서는 백그라운드 작업을 등록하는 앱이 있다고 가정합니다. 백그라운드 작업 구축을 빠르게 시작하려면 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요. 조건 및 트리거에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

## <a name="create-an-event-handler-to-handle-completed-background-tasks"></a>완료된 백그라운드 작업을 처리하는 이벤트 처리기를 만듭니다.

1.  완료된 백그라운드 작업을 처리하는 이벤트 처리기 함수를 만듭니다. 이 코드는 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) 및 [**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778) 개체에서 사용되는 특정 공간을 따라야 합니다.

    OnCompleted 백그라운드 작업 이벤트 처리기 메서드에 대해 다음 공간을 사용합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >  private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >  {
    >      // TODO: Add code that deals with background task completion.
    >  }
    > ```
    > ```cpp
    >  auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >  {
    >      // TODO: Add code that deals with background task completion.
    >  };
    > ```

2.  백그라운드 작업 완료를 처리하는 이벤트 처리기에 코드를 추가합니다.

    예를 들어 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)은 UI를 업데이트합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         UpdateUI();
    >     }
    > ```
    > ```cpp
    >     auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {    
    >         UpdateUI();
    >     };
    > ```

## <a name="create-an-event-handler-function-to-handle-background-task-progress"></a>백그라운드 작업 진행률을 처리하는 이벤트 처리기 함수를 만듭니다.

1.  완료된 백그라운드 작업을 처리하는 이벤트 처리기 함수를 만듭니다. 이 코드는 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) 및 [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 개체에서 사용되는 특정 공간을 따라야 합니다.

    OnProgress 백그라운드 작업 이벤트 처리기 메서드에 대해 다음 공간을 사용합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    > ```
    > ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     };
    > ```

2.  백그라운드 작업 완료를 처리하는 이벤트 처리기에 코드를 추가합니다.

    예를 들어 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)에서는 *args* 매개 변수를 통해 전달된 진행 상태로 UI를 업데이트합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         var progress = "Progress: " + args.Progress + "%";
    >         BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     }
    > ```
    > ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         auto progress = "Progress: " + args->Progress + "%";
    >         BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     };
    > ```

## <a name="register-the-event-handler-functions-with-new-and-existing-background-tasks"></a>새 백그라운드 작업 및 기존 백그라운드 작업과 함께 이벤트 처리기 함수를 등록합니다.


1.  앱에서는 백그라운드 작업을 처음으로 등록할 때 앱이 포그라운드에서 활성화된 상태에서 작업이 실행될 경우 작업에 대한 진행 및 완료 업데이트를 수신하도록 등록해야 합니다.

    예를 들어 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)에서는 등록된 각 백그라운드 작업에 대해 다음 함수를 호출합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    >     {
    >         task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    >         task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    >     }
    > ```
    > ```cpp
    >     void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
    >     {
    >         auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >         {
    >             auto progress = "Progress: " + args->Progress + "%";
    >             BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >             UpdateUI();
    >         };
    >
    >         task->Progress += ref new BackgroundTaskProgressEventHandler(progress);
    >         
    >
    >         auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >         {
    >             UpdateUI();
    >         };
    >
    >         task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
    >     }
    > ```

2.  앱은 시작되거나 백그라운드 작업 상태와 관련된 새 페이지로 이동할 때 현재 등록된 백그라운드 작업의 목록을 가져와서 진행 및 완료 이벤트 처리기 함수에 연결합니다. 응용 프로그램에 의해 현재 등록된 백그라운드 작업 목록은 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 속성에 보관됩니다.

    예를 들어 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)에서는 SampleBackgroundTask 페이지로 이동할 때 다음 코드를 사용하여 이벤트 처리기를 연결합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     protected override void OnNavigatedTo(NavigationEventArgs e)
    >     {
    >         foreach (var task in BackgroundTaskRegistration.AllTasks)
    >         {
    >             if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(task.Value);
    >                 BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
    >             }
    >         }
    >
    >         UpdateUI();
    >     }
    > ```
    > ```cpp
    >     void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
    >     {
    >         // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    >         // as NotifyUser()
    >         rootPage = MainPage::Current;
    >
    >         //
    >         // Attach progress and completed handlers to any existing tasks.
    >         //
    >         auto iter = BackgroundTaskRegistration::AllTasks->First();
    >         auto hascur = iter->HasCurrent;
    >         while (hascur)
    >         {
    >             auto cur = iter->Current->Value;
    >
    >             if (cur->Name == SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(cur);
    >                 break;
    >             }
    >
    >             hascur = iter->MoveNext();
    >         }
    >
    >         UpdateUI();
    >     }
    > ```

## <a name="related-topics"></a>관련 항목

* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md).
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [Windows 스토어 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)
