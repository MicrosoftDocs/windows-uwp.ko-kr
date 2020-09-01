---
title: 백그라운드 작업 실행 조건 설정
description: 백그라운드 작업이 실행되는 시간을 제어하는 조건을 설정하는 방법을 알아봅니다.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 9339472d1f7d601accdc3791644ba328211e5191
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167737"
---
# <a name="set-conditions-for-running-a-background-task"></a>백그라운드 작업 실행 조건 설정

**중요 API**

- [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition)
- [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)
- [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

백그라운드 작업이 실행되는 시간을 제어하는 조건을 설정하는 방법을 알아봅니다.

백그라운드 작업에 성공 하려면 특정 조건이 충족 되어야 하는 경우가 있습니다. 백그라운드 작업을 등록할 때 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 에서 지정 하는 조건 중 하나 이상을 지정할 수 있습니다. 트리거를 실행 한 후 조건이 검사 됩니다. 백그라운드 작업은 큐에 대기 되지만 모든 필수 조건이 충족 될 때까지 실행 되지 않습니다.

백그라운드 작업에 조건을 배치 하면 작업이 불필요 하 게 실행 되지 않도록 하 여 배터리 수명과 CPU가 절약 됩니다. 예를 들어 백그라운드 작업을 타이머에서 실행 하 고 인터넷에 연결 해야 하는 경우 작업을 등록 하기 전에 [**Internetavailable**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 에 **internetavailable** 조건을 추가 합니다. 이렇게 하면 타이머가 경과 *하* 여 인터넷을 사용할 수 있는 경우에만 백그라운드 작업을 실행 하 여 작업에서 시스템 리소스 및 배터리 수명을 불필요 하 게 사용할 수 있습니다.

동일한 [**Taskbuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)에서 **addcondition** 을 여러 번 호출 하 여 여러 조건을 결합할 수도 있습니다. **Userpresent** 및 **usernotpresent**와 같이 충돌 하는 조건을 추가 하지 않도록 주의 합니다.

## <a name="create-a-systemcondition-object"></a>SystemCondition 개체 만들기

이 항목에서는 사용자에 게 앱과 이미 연결 된 백그라운드 작업이 있으며, 앱에 **Taskbuilder**라는 [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 개체를 만드는 코드가 이미 포함 되어 있다고 가정 합니다.  먼저 백그라운드 작업을 만들어야 하는 경우 in-process 백그라운드 작업 [만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md) 을 참조 하세요.

이 항목은 포그라운드 앱과 동일한 프로세스에서 실행 되는 작업 뿐만 아니라 out-of-process를 실행 하는 백그라운드 작업에 적용 됩니다.

조건을 추가 하기 전에 백그라운드 작업을 실행 하는 데 적용 해야 하는 조건을 나타내는 [**systemcondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 개체를 만듭니다. 생성자에서 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 열거형 값을 사용 하 여 충족 해야 하는 조건을 지정 합니다.

다음 코드는 **Internetavailable** 조건을 지정 하는 [**systemcondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 개체를 만듭니다.

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

조건을 추가 하려면 [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 개체에서 [**addcondition**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 메서드를 호출 하 고 [**systemcondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition) 개체를 전달 합니다.

다음 코드는 **Taskbuilder** 를 사용 하 여 **internetavailable** 조건을 추가 합니다.

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

이제 [**register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) 메서드를 사용 하 여 백그라운드 작업을 등록할 수 있으며, 백그라운드 작업은 지정 된 조건이 충족 될 때까지 시작 되지 않습니다.

다음 코드는 작업을 등록 하 고 결과 BackgroundTaskRegistration 개체를 저장 합니다.

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
> 등록 시 백그라운드 작업 등록 매개 변수의 유효성이 검사 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱이 백그라운드 작업 등록에 실패 하는 시나리오를 정상적으로 처리 하는지 확인 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체가 있는 경우 충돌이 발생할 수 있습니다.

## <a name="place-multiple-conditions-on-your-background-task"></a>백그라운드 작업에 여러 조건 추가

여러 조건을 추가 하기 위해 앱은 [**Addcondition**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) 메서드를 여러 번 호출 합니다. 이러한 호출은 작업을 효과적으로 수행 하기 전에 발생 해야 합니다.

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
> 필요한 경우에만 실행 되 고, 그렇지 않으면 실행 되지 않도록 백그라운드 작업에 대 한 조건을 선택 합니다. 다른 백그라운드 작업 조건에 대 한 설명은 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))