---
title: In-process 백그라운드 작업 만들기 및 등록
description: 포그라운드 앱과 동일한 프로세스에서 실행 되는 in-process 작업을 만들고 등록 합니다.
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 489de52a3c592bac9d715b679470b84c2af7e621
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155997"
---
# <a name="create-and-register-an-in-process-background-task"></a>In-process 백그라운드 작업 만들기 및 등록

**중요 API**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

이 항목에서는 앱과 동일한 프로세스에서 실행 되는 백그라운드 작업을 만들고 등록 하는 방법을 보여 줍니다.

In-process 백그라운드 작업은 out-of-process 백그라운드 작업 보다 구현 하기가 더 간단 합니다. 그러나 복원 력이 낮습니다. In-process 백그라운드 작업에서 실행 되는 코드의 작동이 중단 되 면 앱이 중단 됩니다. 또한 [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger), DeviceServicingTrigger 및 [i](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) **startuptask** 는 in-process 모델과 함께 사용할 수 없습니다. 응용 프로그램 내에서 VoIP 백그라운드 작업을 활성화 하는 것도 가능 하지 않습니다. 이러한 트리거와 태스크는 in-process 백그라운드 작업 모델을 사용 하 여 계속 지원 됩니다.

이전 실행 시간 제한을 실행 하는 경우 응용 프로그램의 포그라운드 프로세스 내에서 실행 되는 경우에도 백그라운드 작업을 종료할 수 있습니다. 작업을 별도의 프로세스에서 실행 되는 백그라운드 작업으로 분리 하는 경우에도 유용 합니다. 포그라운드 응용 프로그램과 별도의 작업으로 백그라운드 작업을 유지 하는 것은 포그라운드 응용 프로그램과의 통신이 필요 하지 않은 작업에 가장 적합 한 옵션 일 수 있습니다.

## <a name="fundamentals"></a>기본 사항

In-process 모델은 응용 프로그램이 포그라운드 또는 백그라운드에 있을 때 알림을 개선 하는 응용 프로그램 수명 주기를 향상 시킵니다. 응용 프로그램 개체에서 두 개의 새 이벤트 (예: [**Enteredbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 및 [**leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground))를 사용할 수 있습니다. 이러한 이벤트는 응용 프로그램의 표시 상태에 따라 응용 프로그램 수명 주기에 따라 달라 지 며, 이러한 이벤트와 [앱 수명 주기의](app-lifecycle.md)응용 프로그램 수명 주기에 영향을 주는 방법에 대해 자세히 알아보세요.

상위 수준에서 앱이 백그라운드에서 실행 되는 동안 실행 되는 코드를 실행 하 고, 앱이 포그라운드로 이동 된 시기를 알 수 있도록 **Leavingbackground** 를 처리 하는 사용자 코드를 처리 **하는 것** 이 좋습니다.

## <a name="register-your-background-task-trigger"></a>백그라운드 작업 트리거 등록

In-process 백그라운드 작업은 out-of-process 백그라운드 작업과 거의 동일 하 게 등록 됩니다. 모든 백그라운드 트리거는 [BackgroundTaskBuilder](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder?f=255&MSPPError=-2147217396)을 사용 하 여 등록으로 시작 합니다. 이 작성기를 사용 하면 모든 필수 값을 한 곳에 설정 하 여 백그라운드 작업을 쉽게 등록할 수 있습니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> 유니버설 Windows 앱은 백그라운드 트리거 형식을 등록 하기 전에 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다.
> 업데이트를 릴리스된 후에도 유니버설 Windows 앱이 제대로 실행 되도록 하려면 [**Removeaccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) 를 호출한 다음 앱이 업데이트 된 후에 시작 될 때 [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다. 자세한 내용은 [백그라운드 작업에 대 한 지침](guidelines-for-background-tasks.md)을 참조 하세요.

In-process 백그라운드 활동의 경우 `TaskEntryPoint.` 기본 진입점은 [OnBackgroundActivated ()](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)이라는 응용 프로그램 개체에 대 한 새 보호 된 메서드인 기본 진입점을 사용 하도록 설정 하지 않습니다.

트리거를 등록 한 후에는 [SetTrigger](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.settrigger) 메서드에 설정 된 트리거 유형에 따라 발생 합니다. 위의 예제에서 [TimeTrigger](/uwp/api/windows.applicationmodel.background.timetrigger) 가 사용 됩니다 .이는 등록 된 시간부터 15 분이 발생 합니다.

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>작업이 실행 되는 시기를 제어 하는 조건 추가 (선택 사항)

트리거 이벤트가 발생 한 후 태스크가 실행 되는 시기를 제어 하는 조건을 추가할 수 있습니다. 예를 들어 사용자가 있을 때까지 작업을 실행 하지 않으려면 **Userpresent**조건을 사용 합니다. 가능한 조건 목록은 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)를 참조 하세요.

다음 샘플 코드에서는 사용자를 표시 해야 하는 조건을 할당 합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>백그라운드 활동 코드를 OnBackgroundActivated ()에 저장 합니다.

백그라운드 작업 코드를 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) 에 배치 하 여 발생 하는 백그라운드 트리거에 응답 합니다. **OnBackgroundActivated** 는 [IBackgroundTask](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396)와 마찬가지로 처리할 수 있습니다. 메서드에는 **실행** 메서드에서 제공 하는 모든 항목을 포함 하는 [BackgroundActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.backgroundactivatedeventargs) 매개 변수가 있습니다. 예를 들어 App.xaml.cs에서 다음을 수행 합니다.

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

더 다양 한 **OnBackgroundActivated** 예제는 [호스트 앱과 동일한 프로세스에서 실행 되도록 app service 변환](convert-app-service-in-process.md)을 참조 하세요.

## <a name="handle-background-task-progress-and-completion"></a>백그라운드 작업 진행률 및 완료 처리

작업 진행률 및 완료는 다중 프로세스 백그라운드 작업과 동일한 방식으로 모니터링할 수 있습니다 ( [백그라운드 작업 진행률 및 완료 모니터링](monitor-background-task-progress-and-completion.md)참조). 그러나 변수를 사용 하 여 앱에서 진행률 또는 완료 상태를 추적 하 여 보다 쉽게 추적할 수 있습니다. 이는 백그라운드 작업 코드를 앱과 동일한 프로세스에서 실행 하는 경우의 이점 중 하나입니다.

## <a name="handle-background-task-cancellation"></a>백그라운드 작업 취소 처리

In-process 백그라운드 작업과 동일한 방식으로 in-process 백그라운드 작업이 취소 됩니다 ( [취소 된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)참조). 취소를 수행 하기 전에 **BackgroundActivated** 이벤트 처리기를 종료 해야 합니다. 그렇지 않으면 전체 프로세스가 종료 됩니다. 백그라운드 작업을 취소할 때 포그라운드 앱이 예기치 않게 닫히는 경우 취소가 발생 하기 전에 처리기가 종료 되었는지 확인 합니다.

## <a name="the-manifest"></a>매니페스트

In-process 백그라운드 작업과 달리 in-process 백그라운드 작업을 실행 하기 위해 패키지 매니페스트에 백그라운드 작업 정보를 추가할 필요는 없습니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이제 In-process 백그라운드 작업을 작성하는 방법의 기본 사항을 이해해야 합니다.

백그라운드 작업을 사용 하는 앱을 작성 하는 방법에 대 한 API 참조, 백그라운드 작업 개념 지침 및 자세한 지침은 다음 관련 항목을 참조 하세요.

## <a name="related-topics"></a>관련 항목

**자세한 백그라운드 작업 지침 항목**

* [Out-of-process 백그라운드 작업을 in-process 백그라운드 작업으로 변환](convert-out-of-process-background-task.md)
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [백그라운드에서 미디어 재생](../audio-video-camera/background-audio.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)

**백그라운드 작업 지침**

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))

**백그라운드 작업 API 참조**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)