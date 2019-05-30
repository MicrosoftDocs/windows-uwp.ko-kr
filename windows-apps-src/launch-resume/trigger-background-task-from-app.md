---
title: 앱 내에서 백그라운드 작업 트리거
description: 응용 프로그램 내에서 백그라운드 작업을 트리거하는 방법을 설명합니다.
ms.date: 07/06/2018
ms.topic: article
keywords: 백그라운드 작업 트리거를 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: d5d163d36b51e414a403986d1fdd73db7925cc0b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371890"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>앱 내에서 백그라운드 작업 트리거

[ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)를 사용하여 앱 내에서 백그라운드 작업을 활성화하는 방법을 알아봅니다.

응용 프로그램 트리거를 만드는 방법의 예제를 참조 하세요 [예제에서는](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)합니다.

이 항목에서는 응용 프로그램에서 활성화하려는 백그라운드 작업이 이미 있다고 가정합니다. 백그라운드 작업이 아직 없는 경우 [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)에 샘플 백그라운드 작업이 있습니다. 또는 [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)의 단계에 따라 백그라운드 작업을 하나 만듭니다.

## <a name="why-use-an-application-trigger"></a>응용 프로그램 트리거를 사용하는 이유

**ApplicationTrigger**를 사용하여 포그라운드 앱에서 별도의 프로세스로 코드를 실행합니다. 앱에 사용자가 포그라운드 앱을 닫더라도 배경에서 수행해야 하는 작업이 있는 경우 **ApplicationTrigger**가 적합합니다. 백그라운드 작업이 앱이 닫힐 때 정지되어야 하거나 프로그라운드 프로세스의 상태에 연결되어야 하는 경우 [확장 실행](run-minimized-with-extended-execution.md)을 대신 사용해야 합니다.

## <a name="create-an-application-trigger"></a>응용 프로그램 트리거를 만듭니다.

새 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)를 만듭니다. 아래 조각에서 완성되면 필드에 저장할 수 있습니다. 이는 편의상 나중에 트리거 신호를 보내려고 할 때 새 인스턴스를 만들 필요가 없게 하기 위함입니다. 그러나 **ApplicationTrigger** 인스턴스를 사용하여 트리거 신호를 보낼 수 있습니다.

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>(옵션) 조건 추가

작업이 실행되는 시간을 제어하는 백그라운드 작업 조건을 만들 수 있습니다. 조건이 맞을 때까지 조건이 백그라운드 작업 실행을 막습니다. 자세한 내용은 [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)을 참조하세요.

이 예제에서는 조건으로 설정 됩니다 **InternetAvailable** 트리거할 수 있도록, 한 번에 작업 인터넷 액세스를 사용할 수 있으면을 실행 합니다. 가능한 조건 목록은 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)을 참조하세요.

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

조건 및 백그라운드 트리거 유형에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

##  <a name="call-requestaccessasync"></a>RequestAccessAsync() 호출

사용자가 앱에 대한 백그라운드 작업을 비활성화했을 수 있으므로 **ApplicationTrigger** 백그라운드 작업을 등록하기 전에 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)를 호출하여 사용자가 허용하는 백그라운드 작업 수준을 결정합니다. 사용자가 백그라운드 작업에 대한 설정을 제어하는 방법은 [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)를 참조하세요.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>백그라운드 작업 등록

백그라운드 작업 등록 함수를 호출하여 백그라운드 작업을 등록합니다. 백그라운드 작업 등록에 대한 자세한 내용과 아래 샘플 코드의 **RegisterBackgroundTask()** 메서드 정의를 보려면 [백그라운드 작업 등록](register-a-background-task.md)을 참조하세요.

Application 트리거를 사용하여 포그라운드 프로세스의 수명을 연장하려는 경우 [확장 실행](run-minimized-with-extended-execution.md)을 대신 사용합니다. Application 트리거는 별도로 호스트되는 작업용 프로세스를 만들도록 설계되었습니다. 다음 코드 조각은 Out-of-process 백그라운드 트리거를 등록합니다.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.

## <a name="trigger-the-background-task"></a>백그라운드 작업 트리거

백그라운드 작업을 트리거하기 전에 [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)을 사용하여 백그라운드 작업이 등록되었는지 확인합니다. 앱 시작 중에 모든 백그라운드 작업이 등록되었는지 확인하는 것이 좋습니다.

[ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger)를 호출하여 백그라운드 작업을 트리거합니다. **ApplicationTrigger** 인스턴스라면 다 됩니다.

앱이 백그라운드 실행 상태일 때 또는 백그라운드 작업 자체에서 **ApplicationTrigger.RequestAsync**를 호출할 수 없습니다. 응용 프로그램 상태에 대한 자세한 내용은 [앱 수명 주기](app-lifecycle.md)를 참조하세요.
사용자가 앱의 백그라운드 작업 실행을 차단하는 에너지 정책이나 개인정보 보호정책을 설정한 경우 [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)를 반환할 수 있습니다.
또한 한 번에 한 AppTrigger만 실행할 수 있습니다. AppTrigger가 실행 중인 동안 다른 AppTrigger를 실행하려고 하면 [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)이 반환됩니다.

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>백그라운드 작업에 대한 리소스 관리

[BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager)를 사용하면 사용자가 앱의 백그라운드 작업을 제한하기로 결정했는지 확인할 수 있습니다. 배터리 사용 정보를 파악하고 사용자가 원하는 작업을 완료해야 하는 경우에 백그라운드에서만 실행해야 합니다. 사용자가 백그라운드 작업에 대한 설정을 제어하는 방법은 [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)를 참조하세요.  

- 메모리: 앱의 메모리 및 에너지 사용 하 여 튜닝는 운영 체제를 실행 하 여 백그라운드 작업을 사용할 수 있음을 보장 하는 키입니다. [메모리 관리 API](https://docs.microsoft.com/uwp/api/windows.system.memorymanager)를 사용하면 백그라운드 작업에서 사용 중인 메모리의 양을 확인할 수 있습니다. 백그라운드 작업이 사용하는 메모리가 많을수록 다른 앱이 포그라운드에 있을 때 OS에서 백그라운드 작업을 계속 실행하기 어렵습니다. 사용자는 앱이 수행할 수 있는 모든 백그라운드 작업을 근본적으로 제어하며 앱이 배터리 사용에 미치는 영향을 한 눈에 볼 수 있게 됩니다.  
- CPU 시간: 백그라운드 작업 트리거 형식에 따라 가져오기는 벽 시계 사용 기간으로 제한 됩니다. Application 트리거로 트리거된 백그라운드 작업은 약 10초로 제한됩니다.

백그라운드 작업에 적용되는 리소스 제약 조건은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

## <a name="remarks"></a>설명

Windows 10 부터는 더 이상 필요는 백그라운드 작업을 사용 하려면 잠금 화면에 앱을 추가 하려면 사용자에 대 한.

백그라운드 작업은 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)를 먼저 호출한 경우에만 **ApplicationTrigger**를 사용하여 실행됩니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [앱이 백그라운드로 이동할 때 메모리 회수](reduce-memory-usage.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [트리거하는 방법 일시 중단, 다시 시작 및 백그라운드 UWP 앱에는 이벤트 (디버깅) 하는 경우](https://go.microsoft.com/fwlink/p/?linkid=254345)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [확장 실행을 사용하여 앱 일시 중단 연기](run-minimized-with-extended-execution.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업의 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
