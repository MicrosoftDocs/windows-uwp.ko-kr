---
author: TylerMSFT
title: 백그라운드 작업 등록
description: 대부분의 백그라운드 작업을 안전하게 등록하기 위해 다시 사용할 수 있는 함수를 만드는 방법을 알아봅니다.
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 6bd0361886181d3c5a3395112c728db3bf57d58f
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2018
ms.locfileid: "4177184"
---
# <a name="register-a-background-task"></a>백그라운드 작업 등록


**중요 API**

-   [**BackgroundTaskRegistration 클래스**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskBuilder 클래스**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition 클래스**](https://msdn.microsoft.com/library/windows/apps/br224834)

대부분의 백그라운드 작업을 안전하게 등록하기 위해 다시 사용할 수 있는 함수를 만드는 방법을 알아봅니다.

이 항목은 in-process 백그라운드 작업과 out-of-process 백그라운드 작업에 모두 적용됩니다. 이 항목에서는 등록해야 하는 백그라운드 작업이 이미 있다고 가정합니다. 백그라운드 작업을 작성하는 방법에 대한 자세한 내용은 [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md) 또는 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)을 참조하세요.

이 항목에서는 백그라운드 작업을 등록하는 유틸리티 함수를 안내합니다. 이 유틸리티 함수는 작업을 여러 번 등록하기 전에 기존 등록을 확인하여 여러 번 등록과 관련된 문제를 방지하며 백그라운드 작업에 시스템 조건을 적용할 수 있습니다. 이 연습에는 이 유틸리티의 전체 작업 예제가 포함됩니다.

**참고**  

유니버설 Windows 앱에서 백그라운드 트리거 형식을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다.

업데이트를 릴리스한 후 유니버설 Windows 앱이 계속해서 제대로 실행되도록 하려면 앱이 업데이트된 후 시작될 때 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) 및 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 차례로 호출해야 합니다. 자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

## <a name="define-the-method-signature-and-return-type"></a>메서드 서명 및 반환 형식 정의

이 메서드는 작업 진입점, 작업 이름, 미리 구성된 백그라운드 작업 트리거 및 백그라운드 작업에 대한 [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)(옵션)을 받아들입니다. 이 메서드는 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 개체를 반환합니다.

> [!Important]
> `taskEntryPoint` - Out of process로 실행되는 백그라운드 작업의 경우 네임스페이스 이름, '.' 및 백그라운드 클래스가 포함된 클래스 이름으로 구성되어야 합니다. 문자열은 대/소문자를 구분합니다.  예를 들어 백그라운드 클래스 코드가 포함된 "BackgroundTask1" 클래스와 "MyBackgroundTasks" 네임스페이스가 있는 경우 `taskEntryPoint` 문자열은 "MyBackgroundTasks.BackgroundTask1"이 됩니다.
> 백그라운드 작업이 앱과 동일한 프로세스로 실행되는 경우(즉, in-process 백그라운드 작업) `taskEntryPoint`를 설정하지 않아야 합니다.

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

작업이 이미 등록되었는지 확인합니다. 작업이 여러 번 등록된 경우 작업이 트리거될 때마다 두 번 이상 실행되어 CPU가 초과되거나 예기치 않은 동작이 발생할 수 있기 때문에 이것을 확인해야 합니다.

기존 등록을 확인하려면 [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 속성을 쿼리하고 결과를 반복합니다. 각 인스턴스의 이름을 확인합니다. 이 이름이 등록하려는 작업의 이름과 일치하는 경우 루프를 종료하고 플래그 변수를 설정하여 코드에서 다음 단계에는 다른 경로를 선택할 수 있게 합니다.

> **참고**  앱에 고유한 백그라운드 작업 이름을 사용하세요. 각 백그라운드 작업의 이름이 고유해야 합니다.

다음 코드에서는 마지막 단계에서 만든 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)를 사용하여 백그라운드 작업을 등록합니다.

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

## <a name="register-the-background-task-or-return-the-existing-registration"></a>백그라운드 작업 등록(또는 기존 등록 반환)


작업이 기존 백그라운드 작업 등록 목록에 있는지 확인합니다. 그럴 경우 작업의 해당 인스턴스를 반환합니다.

그런 다음 새 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 개체를 사용하여 작업을 등록합니다. 이 코드에서는 조건 매개 변수가 null인지 확인하고 그렇지 않으면 등록 개체에 조건을 추가해야 합니다. [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224786) 메서드에서 반환된 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224772)을 반환합니다.

> **참고**  백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.
> **참고** 앱과 동일한 프로세스에서 실행되는 백그라운드 작업을 등록하는 경우 `taskEntryPoint` 매개 변수에 대해 `String.Empty` 또는 `null`을 전송합니다.

다음 예제에서는 기존 작업을 반환하거나 백그라운드 작업(선택적 시스템 조건(있는 경우) 포함)을 등록하는 코드를 추가합니다.

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

## <a name="complete-background-task-registration-utility-function"></a>전체 백그라운드 작업 등록 유틸리티 함수

다음 예제에서는 완성된 백그라운드 작업 등록 함수를 보여 줍니다. 이 함수는 네트워킹 백그라운드 작업을 제외하고 대부분의 백그라운드 작업을 등록하는 데 사용할 수 있습니다.

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
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)