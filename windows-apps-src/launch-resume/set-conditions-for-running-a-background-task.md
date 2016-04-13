---
백그라운드 작업 실행 조건 설정
백그라운드 작업이 실행되는 시간을 제어하는 조건을 설정하는 방법을 알아봅니다.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
---

# 백그라운드 작업 실행 조건 설정


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

백그라운드 작업이 실행되는 시간을 제어하는 조건을 설정하는 방법에 대해 알아봅니다.

백그라운드 작업은 성공적으로 수행하려면 작업을 트리거하는 이벤트 이외에 특정한 조건을 충족해야 합니다. 백그라운드 작업을 등록할 때 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)에 지정된 조건을 하나 이상 지정할 수 있습니다. 트리거가 실행되면 조건을 확인합니다. 백그라운드 작업은 큐에 보관되지만 모든 필요한 조건이 충족될 때에만 실행됩니다.

백그라운드 작업에 조건을 설정하면 작업이 불필요하게 실행되지 않으므로 배터리 사용 시간과 CPU 런타임이 절약됩니다. 예를 들어 백그라운드 작업이 타이머에 따라 실행되고 인터넷 연결이 필요한 경우 작업을 등록하기 전에 **InternetAvailable** 조건을 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)에 추가합니다. 그러면 타이머가 경과되고 인터넷을 사용할 수 있을 때 작업을 실행하여 작업에서 시스템 리소스와 배터리를 불필요하게 사용하는 것을 방지할 수 있습니다.

**참고** 동일한 [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)에서 AddCondition을 여러 번 호출하여 여러 조건을 결합할 수 있습니다. **UserPresent** 및 **UserNotPresent**와 같은 충돌하는 조건을 추가하지 않도록 주의하세요.

 

## SystemCondition 개체 만들기


이 항목에서는 앱에 백그라운드 작업이 이미 연결되어 있으며, [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 개체(**taskBuilder**)를 만드는 코드가 앱에 포함되어 있다고 가정합니다.

조건을 추가하기 전에 백그라운드 작업이 실행되기 위해 충족해야 하는 조건을 나타내는 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 개체를 만듭니다. 생성자에서 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 열거형 값을 제공하여 충족해야 하는 조건을 지정합니다.

다음 코드는 조건부 요구 사항으로 인터넷 가용성을 지정하는 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 개체를 만듭니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## 백그라운드 작업에 SystemCondition 개체 추가


조건을 추가하려면 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 개체에 대해 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 메서드를 호출하고 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) 개체를 전달합니다.

다음 코드는 InternetAvailable 백그라운드 작업 조건을 TaskBuilder에 등록합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> taskBuilder.AddCondition(internetCondition);
> ```
> ```cpp
> taskBuilder->AddCondition(internetCondition);
> ```

## 백그라운드 작업 등록


이제 [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 메서드를 사용하여 백그라운드 작업을 등록할 수 있으며, 지정한 조건을 충족하는 경우에만 작업이 시작됩니다.

다음 코드는 작업을 등록하고 결과 BackgroundTaskRegistration 개체를 저장합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> BackgroundTaskRegistration task = taskBuilder.Register();
> ```
> ```cpp
> BackgroundTaskRegistration ^ task = taskBuilder->Register();
> ```

> **참고** 유니버설 Windows 앱에서 백그라운드 트리거 형식을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다.

업데이트를 릴리스한 후 유니버설 Windows 앱이 계속해서 제대로 실행되도록 하려면 앱이 업데이트된 후 시작될 때 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) 및 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 차례로 호출해야 합니다. 자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

> **참고** 백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.

## 백그라운드 작업에 여러 조건 배치

여러 조건을 추가하려면 앱에서 [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) 메서드를 여러 번 호출합니다. 이러한 호출은 작업 등록이 적용되기 전에 수행되어야 합니다.

> **참고** 백그라운드 작업에 충돌하는 조건을 추가하지 않도록 주의하세요.
 

다음 코드 조각은 백그라운드 작업을 만들고 등록하는 컨텍스트 내의 여러 조건을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
```cs
> // 
> // Set up the background task.
> // 
> 
> TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
> 
> var recurringTaskBuilder = new BackgroundTaskBuilder();
> 
> recurringTaskBuilder.Name           = "Hourly background task";
> recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder.SetTrigger(hourlyTrigger);
> 
> // 
> // Begin adding conditions.
> // 
> 
> SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> 
> recurringTaskBuilder.AddCondition(userCondition);
> recurringTaskBuilder.AddCondition(internetCondition);
> 
> // 
> // Done adding conditions, now register the background task.
> // 
> 
> BackgroundTaskRegistration task = recurringTaskBuilder.Register();
> ```
> ```cpp
> // 
> // Set up the background task.
> // 
> 
> TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
> 
> auto recurringTaskBuilder = ref new BackgroundTaskBuilder();
> 
> recurringTaskBuilder->Name           = "Hourly background task";
> recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder->SetTrigger(hourlyTrigger);
> 
> // 
> // Begin adding conditions.
> // 
> 
> SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> 
> recurringTaskBuilder->AddCondition(userCondition);
> recurringTaskBuilder->AddCondition(internetCondition);
> 
> // 
> // Done adding conditions, now register the background task.
> // 
> 
> BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
> ```

## Remarks


> **Note**  Chose the right conditions for your background task so that it only runs when it's needed, and doesn't run when it won't work. See [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) for descriptions of the different background task conditions.

> **Note**  This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Related topics


****

* [Create and register a background task](create-and-register-a-background-task.md)
* [Declare background tasks in the application manifest](declare-background-tasks-in-the-application-manifest.md)
* [Handle a cancelled background task](handle-a-cancelled-background-task.md)
* [Monitor background task progress and completion](monitor-background-task-progress-and-completion.md)
* [Register a background task](register-a-background-task.md)
* [Respond to system events with background tasks](respond-to-system-events-with-background-tasks.md)
* [Update a live tile from a background task](update-a-live-tile-from-a-background-task.md)
* [Use a maintenance trigger](use-a-maintenance-trigger.md)
* [Run a background task on a timer](run-a-background-task-on-a-timer-.md)
* [Guidelines for background tasks](guidelines-for-background-tasks.md)

****

* [Debug a background task](debug-a-background-task.md)
* [How to trigger suspend, resume, and background events in Windows Store apps (when debugging)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


