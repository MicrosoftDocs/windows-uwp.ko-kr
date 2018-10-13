---
author: TylerMSFT
title: 앱 내에서 백그라운드 작업 트리거
description: 응용 프로그램 내에서 백그라운드 작업을 트리거하는 방법에 설명
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 백그라운드 작업 트리거 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 5ccd171f53795ef71830ffb022d0468facb3ac4f
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4569638"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>앱 내에서 백그라운드 작업 트리거

[ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)를 사용하여 앱 내에서 백그라운드 작업을 활성화하는 방법을 알아봅니다.

Application 트리거를 만드는 방법의 예제에서는이 [예제](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)를 참조 하세요.

이 항목에서는 응용 프로그램에서 활성화 하려는 백그라운드 작업이 있다고 가정 합니다. 백그라운드 작업을 아직 없는 경우 [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)회 샘플 백그라운드 작업이입니다. 또는 [out of process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md) 중 하나를 만드는 단계를 따릅니다.

## <a name="why-use-an-application-trigger"></a>Application 트리거를 사용 하는 이유

**ApplicationTrigger** 를 사용 하 여 포그라운드 앱에서 코드는 별도 프로세스를 실행 합니다. **ApplicationTrigger** 앱이 포그라운드 앱을 닫은 경우에-백그라운드에서 수행 해야 하는 작업 하는 경우에 적합 합니다. 백그라운드 작업을 중지 해야 하는 경우 때 앱을 닫거나 [확장 실행](run-minimized-with-extended-execution.md) 을 대신 사용 해야 하는 다음 포그라운드 프로세스의 상태에 연결 해야 합니다.

## <a name="create-an-application-trigger"></a>응용 프로그램 트리거 만들기

새 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)를 만듭니다. 아래 코드 조각 에서처럼 필드에서이 저장할 수 있습니다. 이 편의 위해 우리는 트리거를 알리기 위해 때 나중에 새 인스턴스를 만들 필요가 없습니다. 하지만 트리거를 알리기 위해 **ApplicationTrigger** 인스턴스를 사용할 수 있습니다.

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

작업을 실행 하는 경우 제어 하는 백그라운드 작업 조건을 만들 수 있습니다. 조건이 충족 되는 백그라운드 작업을 조건이 있습니다. 자세한 내용은 [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)을 참조 하세요.

이 예제에서는 한 번 발생 되도록 조건을 **InternetAvailable** 로 설정 되어 작업 인터넷 액세스를 사용할 수에 실행 합니다. 가능한 조건 목록은 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)을 참조하세요.

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

자세한 내용은 조건 및 백그라운드 트리거 유형에 대 한 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)참조 하세요.

##  <a name="call-requestaccessasync"></a>RequestAccessAsync() 호출

**ApplicationTrigger** 백그라운드 작업을 등록 하기 전에 사용자를 사용할 경우 사용자가 앱에 대 한 백그라운드 작업을 비활성화 한 수 있으므로 백그라운드 작업의 수준을 확인 하려면 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) 호출 합니다. [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) 방법으로 사용자에 대 한 자세한 정보에 대 한 백그라운드 작업에 대 한 설정을 제어할 수를 참조 하세요.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>백그라운드 작업 등록

백그라운드 작업 등록 함수를 호출하여 백그라운드 작업을 등록합니다. 자세한 내용은에 백그라운드 작업을 등록 하 고 아래 샘플 코드에서는 **RegisterBackgroundTask()** 메서드 정의를 [백그라운드 작업 등록](register-a-background-task.md)을 참조 하세요.

포그라운드 프로세스의 수명을 연장 하는 응용 프로그램 트리거를 사용 하 여를 고려 하는 경우 [확장 실행](run-minimized-with-extended-execution.md) 을 대신 사용 하는 것이 좋습니다. Application 트리거에서 작업을 수행 하려면 별도로 호스트 프로세스를 만들기 위해 설계 되었습니다. 다음 코드 조각은 out of process 백그라운드 트리거를 등록합니다.

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

백그라운드 작업을 트리거하기 전에 [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 사용 하 여 백그라운드 작업 등록 되었는지 확인 합니다. 이때 모든 백그라운드 작업 등록 되어 있는지 확인 하는 앱 실행 중입니다.

[ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger)를 호출 하 여 백그라운드 작업을 트리거하십시오. **ApplicationTrigger** 인스턴스를 수행 합니다.

자체는 백그라운드 작업 또는 앱이 백그라운드 상태를 실행 하는 경우 **ApplicationTrigger.RequestAsync** 은 호출 수 없습니다 ( [앱 수명 주기](app-lifecycle.md) 응용 프로그램 상태에 대 한 자세한 내용은 참조).
사용자가 앱 백그라운드 작업을 수행 하는 것을 방지 하는 에너지 또는 개인 정보 보호 정책을 설정 하는 경우 [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) 반환할 수 있습니다.
또한 한 번에 하나의 AppTrigger 실행할 수 있습니다. 다른 이미 실행 중인 동안에 AppTrigger 실행 하려고 하면 [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)반환 합니다.

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>백그라운드 작업에 대 한 리소스를 관리 합니다.

[BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx)를 사용하면 사용자가 앱의 백그라운드 작업을 제한하기로 결정했는지 확인할 수 있습니다. 배터리 사용 정보를 파악하고 사용자가 원하는 작업을 완료해야 하는 경우에 백그라운드에서만 실행해야 합니다. [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) 방법으로 사용자에 대 한 자세한 정보에 대 한 백그라운드 작업에 대 한 설정을 제어할 수를 참조 하세요.  

- 메모리: 키 있는지 확인 하는 운영 체제는 백그라운드 작업을 실행 하는 앱의 메모리와 에너지 사용을 조정 합니다. [메모리 관리 Api](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 를 사용 하 여 백그라운드 작업을 사용 하 여 메모리를 확인 합니다. 백그라운드 작업 사용 하 여 더 많은 메모리, 다른 앱이 포그라운드에서 실행 상태로 유지 하려면 운영 체제에 대 한 더 어려워집니다. 사용자는 앱이 수행할 수 있는 모든 백그라운드 작업을 근본적으로 제어하며 앱이 배터리 사용에 미치는 영향을 한 눈에 볼 수 있게 됩니다.  
- CPU 시간: 백그라운드 작업 트리거 유형에 따라 가져오는 벽 시계 시간으로 제한 됩니다. Application 트리거에 의해 트리거된 백그라운드 작업은 약 10 분으로 제한 됩니다.

백그라운드 작업에 적용되는 리소스 제약 조건은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

## <a name="remarks"></a>설명

Windows 10부터 더 이상 필요는 백그라운드 작업을 활용 하기 위해 잠금 화면에 앱을 추가 하려면 사용자에 대 한.

백그라운드 작업을 먼저 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 호출한 경우 **ApplicationTrigger** 를 사용 하 여 실행 됩니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md).
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [앱이 백그라운드로 이동할 때 메모리 회수](reduce-memory-usage.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [확장 실행을 사용하여 앱 일시 중단 연기](run-minimized-with-extended-execution.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
