---
author: TylerMSFT
title: In-process 백그라운드 작업 만들기 및 등록
description: 포그라운드 앱과 같은 프로세스에서 실행되는 In-process 작업을 만들고 등록합니다.
ms.author: twhitney
ms.date: 11/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 5879977662dc2bd609d09e5fe53fc2a2f0b9180f
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4687760"
---
# <a name="create-and-register-an-in-process-background-task"></a>In-process 백그라운드 작업 만들기 및 등록

**중요 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

이 항목에서는 앱과 같은 프로세스에서 실행되는 백그라운드 작업을 만들고 등록하는 방법을 보여 줍니다.

In-process 백그라운드 작업은 Out-of-process 백그라운드 작업보다 구현하기가 더 쉽습니다. 그러나 복원 가능성이 더 낮습니다. In-process 백그라운드 작업에서 실행되는 코드가 중지되면 앱이 중단됩니다. 또한 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 및 **IoTStartupTask**는 In-process 모델에서 사용할 수 없습니다. 응용 프로그램 내에서 VoIP 백그라운드 작업 활성화도 가능하지 않습니다. 이러한 트리거 및 작업은 Out-of-process 백그라운드 작업 모델을 사용하여 계속 지원됩니다.

백그라운드 작업은 실행 시간 제한을 초과하여 실행되는 경우 앱의 포그라운드 프로세스 내에서 실행 중인 경우에도 종료될 수 있습니다. 몇 가지 용도로 작업을 별도 프로세스에서 실행되는 백그라운드 작업으로 구분하는 복원력은 계속 유용합니다. 백그라운드 작업을 포그라운드 응용 프로그램에서 분리된 작업으로 유지하는 것이 포그라운드 응용 프로그램과의 통신이 필요하지 않은 작업에 대한 가장 적합한 옵션일 수 있습니다.

## <a name="fundamentals"></a>기본 사항

In-process 모델은 앱이 포그라운드 또는 백그라운드에 있는 때를 더 잘 알려줌으로써 응용 프로그램 수명 주기를 향상시킵니다. 이러한 전환에 대해 응용 프로그램 개체에서 두 가지 새로운 이벤트인 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 및 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)를 사용할 수 있습니다. 이러한 이벤트는 응용 프로그램의 표시 상태를 기반하는 응용 프로그램 수명 주기에 맞습니다. [앱 수명 주기](app-lifecycle.md)에서 이러한 이벤트에 대한 내용과 이러한 이벤트가 응용 프로그램의 수명 주기에 어떻게 영향을 미치는지를 참조하세요.

개략적으로 **EnteredBackground** 이벤트를 처리하여 앱이 백그라운드에서 실행되는 동안 실행할 코드를 실행하고 **LeavingBackground**를 처리하여 앱이 포그라운드로 이동한 때를 알게 됩니다.

## <a name="register-your-background-task-trigger"></a>백그라운드 작업 트리거 등록

In-process 백그라운드 작업은 Out-of-process 백그라운드 작업과 거의 동일하게 등록됩니다. 모든 백그라운드 트리거는 [BackgroundTaskBuilder](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.aspx?f=255&MSPPError=-2147217396)를 사용하여 등록을 시작합니다. 빌더를 사용하면 필요한 모든 값을 하나의 위치에서 설정하여 백그라운드 작업을 등록하기가 쉽습니다.

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
> 유니버설 Windows 앱에서 백그라운드 트리거 형식을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다.
> 업데이트를 릴리스한 후 유니버설 Windows 앱이 계속해서 제대로 실행되도록 하려면 앱이 업데이트된 후 시작될 때 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) 및 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 차례로 호출해야 합니다. 자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

In-process 백그라운드 작업의 경우 `TaskEntryPoint.`를 설정하지 않습니다. 빈 상태로 두면 기본 진입점, [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)라는 응용 프로그램 개체의 새 보호된 메서드를 사용할 수 있습니다.

트리거가 등록되면 [SetTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.settrigger.aspx) 메서드에 설정된 트리거 유형에 따라 발생합니다. 위 예제에서는 등록된 시간부터 15분 후 발생하는 [TimeTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.timetrigger.aspx)가 사용됩니다.

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>작업 실행 시간을 제어하는 조건 추가(옵션)

트리거 이벤트가 발생한 후 작업이 실행될 시간을 제어하는 조건을 추가할 수도 있습니다. 예를 들어 사용자가 있을 때까지 작업이 실행되지 않게 하려면 **UserPresent** 조건을 사용합니다. 가능한 조건 목록은 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)을 참조하세요.

다음 샘플 코드에서는 사용자가 있어야 하는 조건을 할당합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>OnBackgroundActivated()에서 백그라운드 작업 코드 배치

백그라운드 트리거가 발생할 때 응답을 [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 백그라운드 작업 코드를 배치 합니다. **OnBackgroundActivated** [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396)마찬가지로 처리할 수 있습니다. 메서드에 **Run** 메서드가 제공 하는 모든 항목이 포함 된 [BackgroundActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.backgroundactivatedeventargs.aspx) 매개를 변수가 있습니다. 예 App.xaml.cs에서:

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

풍부한 **OnBackgroundActivated** 예제에서는 [앱 서비스가 호스트 앱과 동일한 프로세스에서 실행 되도록 변환](convert-app-service-in-process.md)을 참조 하세요.

## <a name="handle-background-task-progress-and-completion"></a>백그라운드 작업 진행 및 완료 처리

작업 진행 및 완료는 다중 프로세스 백그라운드 작업과 동일한 방식으로 모니터링할 수 있지만([백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md) 참조) 앱에서 진행 또는 완료 상태를 추적하는 변수를 사용하면 더 쉽게 추적할 수 있습니다. 이런 점이 백그라운드 작업 코드를 앱과 같은 프로세스에서 실행하는 장점 중 하나입니다.

## <a name="handle-background-task-cancellation"></a>백그라운드 작업 취소 처리

In-process 백그라운드 작업은 Out-of-process 백그라운드 작업과 동일한 방식으로 취소됩니다([취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md) 참조). 취소가 발생하기 전에 **BackgroundActivated** 이벤트 처리기가 종료되어야 하며 그렇지 않은 경우 전체 프로세스가 종료됩니다. 백그라운드 작업을 취소할 때 포그라운드 앱이 예기치 않게 닫히는 경우 취소가 발생하기 전에 처리기가 종료되었는지 확인합니다.

## <a name="the-manifest"></a>매니페스트

Out-of-process 백그라운드 작업과 달리 In-process 백그라운드 작업을 실행하기 위해 패키지 매니페스트에 백그라운드 작업 정보를 추가하지 않아도 됩니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이제 In-process 백그라운드 작업을 작성하는 방법의 기본 사항을 이해해야 합니다.

API 참조, 백그라운드 작업 개념 지침, 백그라운드 작업을 사용하는 앱을 작성하는 자세한 방법에 대해서는 다음 관련 항목을 참조하세요.

## <a name="related-topics"></a>관련 항목

**자세한 백그라운드 작업 지침 항목**

* [Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환](convert-out-of-process-background-task.md)
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [백그라운드에서 미디어 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
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
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**백그라운드 작업 API 참조**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)
