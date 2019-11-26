---
title: 백그라운드 작업 실행 조건 설정
description: 백그라운드 작업이 실행되는 시간을 제어하는 조건을 설정하는 방법에 대해 알아봅니다.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 618c8891551d851c27414968be76fb465eb89bf0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260415"
---
# <a name="set-conditions-for-running-a-background-task"></a>백그라운드 작업 실행 조건 설정

**중요 API**

- [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)
- [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

백그라운드 작업이 실행되는 시간을 제어하는 조건을 설정하는 방법에 대해 알아봅니다.

경우에 따라 백그라운드 작업을 성공적으로 수행하려면 특정한 조건을 충족해야 합니다. 백그라운드 작업을 등록할 때 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)에 지정된 조건을 하나 이상 지정할 수 있습니다. 트리거가 실행되면 조건을 확인합니다. 그런 다음 백그라운드 작업이 큐에 보관되지만 모든 필요한 조건이 충족될 때에만 실행됩니다.

백그라운드 작업에 조건을 설정하면 작업이 불필요하게 실행되지 않으므로 배터리 사용 시간과 CPU가 절약됩니다. 예를 들어 백그라운드 작업이 타이머에 따라 실행되고 인터넷 연결이 필요한 경우 작업을 등록하기 전에 **InternetAvailable** 조건을 [**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)에 추가합니다. 그러면 타이머가 경과*되고* 인터넷을 사용할 수 있을 때 백그라운드 작업만 실행하여 작업에서 시스템 리소스와 배터리를 불필요하게 사용하는 것을 방지할 수 있습니다.

동일한TaskBuilder[**에서** AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)을 여러 번 호출하여 여러 조건을 결합할 수도 있습니다. **UserPresent** 및 **UserNotPresent**와 같은 충돌하는 조건을 추가하지 않도록 주의하세요.

## <a name="create-a-systemcondition-object"></a>SystemCondition 개체 만들기

이 항목에서는 앱에 백그라운드 작업이 이미 연결되어 있으며, [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 개체(**taskBuilder**)를 만드는 코드가 앱에 포함되어 있다고 가정합니다.  백그라운드 작업을 먼저 만들어야 하는 경우 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요.

이 항목은 포그라운드 앱과 동일한 프로세스에서 실행되는 백그라운드 작업뿐만 아니라 out-of-process에서 실행되는 백그라운드 작업에도 적용됩니다.

조건을 추가하기 전에 백그라운드 작업이 실행되기 위해 충족해야 하는 조건을 나타낼 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 개체를 만듭니다. 생성자에서 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 열거형 값으로 충족해야 하는 조건을 지정합니다.

다음 코드는 [InternetAvailable**조건을 지정하는**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)SystemCondition 개체를 만듭니다.

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>백그라운드 작업에 SystemCondition 개체 추가

조건을 추가하려면 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 개체에 대해 [**AddCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 메서드를 호출하고 [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 개체를 전달합니다.

다음 코드는 **taskBuilder**를 사용하여 **InternetAvailable** 조건을 추가합니다.

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>백그라운드 작업 등록

이제 [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) 메서드를 사용하여 백그라운드 작업을 등록할 수 있으며, 지정한 조건을 충족하는 경우에만 백그라운드 작업이 시작됩니다.

다음 코드는 작업을 등록하고 결과 BackgroundTaskRegistration 개체를 저장합니다.

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> 유니버설 Windows 앱에서 백그라운드 트리거 형식을 등록하기 전에 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)를 호출해야 합니다.

업데이트를 릴리스한 후 유니버설 Windows 앱이 계속해서 제대로 실행되도록 하려면 앱이 업데이트된 후 시작될 때 [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) 및 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)를 차례로 호출해야 합니다. 자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

> [!NOTE]
> 백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.

## <a name="place-multiple-conditions-on-your-background-task"></a>백그라운드 작업에 여러 조건 배치

여러 조건을 추가하려면 앱에서 [**AddCondition**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 메서드를 여러 번 호출합니다. 이러한 호출은 작업 등록이 적용되기 전에 수행되어야 합니다.

> [!NOTE]
> 충돌 하는 조건을 백그라운드 작업에 추가 하지 않도록 주의 합니다.

다음 코드 조각에서는 백그라운드 작업을 만들고 등록 하는 컨텍스트에서 여러 조건을 보여 줍니다.

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>설명

> [!NOTE]
> 필요한 경우에만 실행 되 고, 그렇지 않으면 실행 되지 않도록 백그라운드 작업에 대 한 조건을 선택 합니다. 다른 백그라운드 작업 조건에 대한 자세한 내용은 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업의 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
