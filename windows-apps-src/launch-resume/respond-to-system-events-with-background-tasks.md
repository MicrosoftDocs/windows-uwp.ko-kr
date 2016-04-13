---
title: 백그라운드 작업으로 시스템 이벤트에 응답
description: SystemTrigger 이벤트에 응답하는 백그라운드 작업을 만드는 방법을 알아봅니다.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
---

# 백그라운드 작업으로 시스템 이벤트에 응답


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

[
            **SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 이벤트에 응답하는 백그라운드 작업을 만드는 방법을 알아봅니다.

이 항목에서는 앱에 대해 작성된 백그라운드 작업 클래스가 있고, 인터넷을 사용할 수 있게 되거나 사용자가 로그인하는 등 시스템에서 트리거된 이벤트에 응답하여 이 작업을 실행해야 한다고 가정합니다. 이 항목에서는 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 클래스에 중점을 둡니다. 백그라운드 작업 클래스를 작성하는 방법에 대해서는 [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요.

## SystemTrigger 개체 만들기


-   앱 코드에서 새 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 개체를 만듭니다. 첫 번째 매개 변수인 *triggerType*은 이 백그라운드 작업을 활성화할 시스템 이벤트 트리거 형식을 지정합니다. 이벤트 형식 목록은 [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)을 참조하세요.

    두 번째 매개 변수인 *OneShot*은 다음에 시스템 이벤트가 발생되어 백그라운드 작업을 트리거할 때 백그라운드 작업을 한 번 실행할지, 아니면 작업을 등록 해제할 때까지 시스템 이벤트가 발생될 때마다 실행할지를 지정합니다.

    다음 코드는 인터넷을 사용할 수 있게 될 때마다 백그라운드 작업을 실행하도록 지정합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
    > ```
    > ```cpp
    > SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
    > ```

## 백그라운드 작업 등록


-   백그라운드 작업 등록 함수를 호출하여 백그라운드 작업을 등록합니다. 백그라운드 작업 등록에 대한 자세한 내용은 [백그라운드 작업 등록](register-a-background-task.md)을 참조하세요.

    다음 코드는 백그라운드 작업을 등록합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Internet-based background task";
    > 
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Internet-based background task";
    > 
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```

    > **참고** 유니버설 Windows 앱에서 백그라운드 트리거 형식을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다.

    업데이트를 릴리스한 후 유니버설 Windows 앱이 계속해서 제대로 실행되도록 하려면 앱이 업데이트된 후 시작될 때 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) 및 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 차례로 호출해야 합니다. 자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

    > **참고** 백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.

     

## 설명


적용 중인 백그라운드 작업 등록을 보려면 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)을 다운로드합니다.

[
            **SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 및 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 이벤트에 대한 응답으로 백그라운드 작업을 실행할 수 있지만 [응용 프로그램 매니페스트에서 백그라운드 작업을 선언](declare-background-tasks-in-the-application-manifest.md)해야 합니다. 백그라운드 작업 형식을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)도 호출해야 합니다.

앱은 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 및 [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831) 이벤트에 응답하는 백그라운드 작업을 등록하여, 앱이 포그라운드에 있지 않은 경우에도 사용자와 실시간으로 통신할 수 있습니다. 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

> **참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 
## 관련 항목


****

* [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)

****

* [백그라운드 작업 디버그](debug-a-background-task.md)
* [Windows 스토어 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


