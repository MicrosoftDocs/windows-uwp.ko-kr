---
title: 백그라운드 작업 등록
description: 대부분의 백그라운드 작업을 안전하게 등록하기 위해 다시 사용할 수 있는 함수를 만드는 방법을 알아봅니다.
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 06d9fdfe57ead1e5405a21658654a8992343bb95
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172967"
---
# <a name="register-a-background-task"></a>백그라운드 작업 등록


**중요 API**

-   [**BackgroundTaskRegistration 클래스**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)
-   [**BackgroundTaskBuilder 클래스**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**SystemCondition 클래스**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition)

대부분의 백그라운드 작업을 안전하게 등록하기 위해 다시 사용할 수 있는 함수를 만드는 방법을 알아봅니다.

이 항목은 in-process 백그라운드 작업과 out-of-process 백그라운드 작업에 모두 적용 됩니다. 이 항목에서는 등록 해야 하는 백그라운드 작업이 이미 있다고 가정 합니다. 백그라운드 작업을 작성 하는 방법에 대 한 자세한 내용은 out-of-process를 [실행 하는 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md) 또는 [In-process 백그라운드 작업 생성 및 등록](create-and-register-an-inproc-background-task.md) 을 참조 하세요.

이 항목에서는 백그라운드 작업을 등록 하는 유틸리티 함수를 안내 합니다. 이 유틸리티 함수는 여러 번의 등록 문제를 방지 하기 위해 작업을 여러 번 등록 하기 전에 먼저 기존 등록을 확인 하 고, 백그라운드 작업에 시스템 조건을 적용할 수 있습니다. 이 연습에는이 유틸리티 함수의 완전 한 작업 예가 포함 되어 있습니다.

**참고**  

유니버설 Windows 앱은 백그라운드 트리거 형식을 등록 하기 전에 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다.

업데이트를 릴리스된 후에도 유니버설 Windows 앱이 제대로 실행 되도록 하려면 [**Removeaccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) 를 호출한 다음 앱이 업데이트 된 후에 시작 될 때 [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다. 자세한 내용은 [백그라운드 작업에 대 한 지침](guidelines-for-background-tasks.md)을 참조 하세요.

## <a name="define-the-method-signature-and-return-type"></a>메서드 시그니처와 반환 형식 정의

이 메서드는 작업 진입점, 작업 이름, 미리 생성 된 백그라운드 작업 트리거 및 백그라운드 작업에 대 한 [**Systemcondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) (선택 사항)을 사용 합니다. 이 메서드는 [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 개체를 반환 합니다.

> [!Important]
> `taskEntryPoint` -in-process로 실행 되는 백그라운드 작업의 경우 네임 스페이스 이름, '. ' 및 백그라운드 클래스를 포함 하는 클래스의 이름으로 생성 되어야 합니다. 문자열은 대/소문자를 구분 합니다.  예를 들어, 배경 클래스 코드를 포함 하는 "MyBackgroundTasks" 네임 스페이스와 "BackgroundTask1" 클래스가 있는 경우의 문자열은 `taskEntryPoint` "MyBackgroundTasks. BackgroundTask1"입니다.
> 백그라운드 태스크가 앱과 동일한 프로세스에서 실행 되는 경우 (즉, in-process 백그라운드 작업) 설정 하면 안 됩니다 `taskEntryPoint` .

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```

## <a name="check-for-existing-registrations"></a>기존 등록 확인

태스크가 이미 등록 되어 있는지 확인 합니다. 작업을 여러 번 등록 하는 경우 트리거될 때마다 두 번 이상 실행 되므로이를 확인 하는 것이 중요 합니다. 이 경우 과도 한 CPU를 사용할 수 있으며 예기치 않은 동작이 발생할 수 있습니다.

[**BackgroundTaskRegistration**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) 속성을 쿼리하고 결과를 반복 하 여 기존 등록을 확인할 수 있습니다. 각 인스턴스의 이름을 확인 합니다. 즉, 등록 하는 작업의 이름과 일치 하면 다음 단계에서 코드에서 다른 경로를 선택할 수 있도록 루프를 중단 하 고 플래그 변수를 설정 합니다.

> **참고**    앱에 고유한 백그라운드 작업 이름을 사용 합니다. 각 백그라운드 작업에 고유한 이름이 있는지 확인 합니다.

다음 코드는 마지막 단계에서 만든 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 를 사용 하 여 백그라운드 작업을 등록 합니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == name)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     // We'll register the task in the next step.
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     // We'll register the task in the next step.
> }
> ```

## <a name="register-the-background-task-or-return-the-existing-registration"></a>백그라운드 작업을 등록 하거나 기존 등록을 반환 합니다.


태스크가 기존 백그라운드 작업 등록 목록에 있는지 확인 합니다. 이 경우 해당 작업의 인스턴스를 반환 합니다.

그런 다음 새 [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 개체를 사용 하 여 작업을 등록 합니다. 이 코드는 condition 매개 변수가 null 인지 여부를 확인 하 고, 그렇지 않은 경우 등록 개체에 조건을 추가 합니다. [**BackgroundTaskBuilder**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) 메서드에서 반환 된 [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 를 반환 합니다.

> **참고**    등록 시 백그라운드 작업 등록 매개 변수의 유효성이 검사 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱이 백그라운드 작업 등록에 실패 하는 시나리오를 정상적으로 처리 하는지 확인 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체가 있는 경우 충돌이 발생할 수 있습니다.
> **참고** 앱과 동일한 프로세스에서 실행 되는 백그라운드 작업을 등록 하는 경우 `String.Empty` `null` 매개 변수에 대해 또는를 보냅니다 `taskEntryPoint` .

다음 예에서는 기존 작업을 반환 하거나 백그라운드 작업을 등록 하는 코드를 추가 합니다 (있는 경우 선택적 시스템 조건 포함).

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = name;
>
>     // in-process background tasks don't set TaskEntryPoint
>     if ( taskEntryPoint != null && taskEntryPoint != String.Empty)
>     {
>         builder.TaskEntryPoint = taskEntryPoint;
>     }
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="complete-background-task-registration-utility-function"></a>백그라운드 작업 등록 유틸리티 함수 완료

이 예제에서는 완료 된 백그라운드 작업 등록 함수를 보여 줍니다. 이 함수는 네트워킹 백그라운드 작업을 제외 하 고 대부분의 백그라운드 작업을 등록 하는 데 사용할 수 있습니다.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> public static BackgroundTaskRegistration RegisterBackgroundTask(string taskEntryPoint,
>                                                                 string taskName,
>                                                                 IBackgroundTrigger trigger,
>                                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = taskName;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(Platform::String ^ taskEntryPoint,
>                                                              Platform::String ^ taskName,
>                                                              IBackgroundTrigger ^ trigger,
>                                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="related-topics"></a>관련 항목

* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))