---
title: 앱 내에서 백그라운드 작업 트리거
description: 응용 프로그램 트리거를 사용 하 여 앱 내에서 활성화 하려는 백그라운드 작업을 실행 하는 방법에 대해 알아봅니다.
ms.date: 07/06/2018
ms.topic: article
keywords: 백그라운드 작업 트리거, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 446adc9921d2d124fb6e1304a06b70fa72da3d96
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155837"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>앱 내에서 백그라운드 작업 트리거

[ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)를 사용하여 앱 내에서 백그라운드 작업을 활성화하는 방법을 알아봅니다.

응용 프로그램 트리거를 만드는 방법에 대 한 예제는이 [예제](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)를 참조 하세요.

이 항목에서는 응용 프로그램에서 활성화 하려는 백그라운드 작업이 있다고 가정 합니다. 백그라운드 작업이 아직 없는 경우 [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)에 샘플 백그라운드 태스크가 있습니다. 또는 [out-of-process 백그라운드 작업 만들기 및 등록 작업](create-and-register-a-background-task.md) 의 단계에 따라 하나를 만듭니다.

## <a name="why-use-an-application-trigger"></a>응용 프로그램 트리거를 사용 하는 이유

**Applicationtrigger** 를 사용 하 여 포그라운드 앱과 별도의 프로세스에서 코드를 실행 합니다. 사용자가 포그라운드 앱을 닫은 경우에도 앱이 백그라운드에서 수행 해야 하는 작업을 수행 하는 경우에는 **Applicationtrigger** 를 사용 하는 것이 좋습니다. 앱을 닫을 때 백그라운드 작업을 중지 하거나 포그라운드 프로세스의 상태와 연결 해야 하는 경우 대신 [확장 된 실행](run-minimized-with-extended-execution.md) 을 사용 해야 합니다.

## <a name="create-an-application-trigger"></a>응용 프로그램 트리거 만들기

새 [Applicationtrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)를 만듭니다. 아래 코드 조각에서와 같이 필드에 저장할 수 있습니다. 이는 나중에 트리거를 신호를 보낼 때 새 인스턴스를 만들 필요가 없도록 편의를 위한 것입니다. 그러나 모든 **applicationtrigger** 인스턴스를 사용 하 여 트리거에 신호를 보낼 수 있습니다.

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

## <a name="optional-add-a-condition"></a>필드 조건 추가

백그라운드 작업 조건을 만들어 태스크가 실행 되는 시간을 제어할 수 있습니다. 조건을 충족 하면 조건이 충족 될 때까지 백그라운드 작업이 실행 되지 않습니다. 자세한 내용은 [백그라운드 작업 실행을 위한 조건 설정](set-conditions-for-running-a-background-task.md)을 참조 하세요.

이 예제에서 조건은 **Internetavailable** 로 설정 되므로 트리거된 후에는 인터넷 액세스를 사용할 수 있게 되 면 태스크가 실행 됩니다. 가능한 조건 목록은 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)를 참조 하세요.

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

백그라운드 트리거의 조건 및 유형에 대 한 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)을 참조 하세요.

##  <a name="call-requestaccessasync"></a>RequestAccessAsync () 호출

**Applicationtrigger** 백그라운드 작업을 등록 하기 전에 [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 하 여 사용자가 앱에 대 한 백그라운드 작업을 사용 하지 않도록 설정 했을 수 있으므로 사용자가 허용 하는 백그라운드 작업의 수준을 확인 합니다. 백그라운드 작업에 대 한 설정을 사용자가 제어할 수 있는 방법에 대 한 자세한 내용은 [백그라운드 작업 최적화](../debug-test-perf/optimize-background-activity.md) 를 참조 하세요.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>백그라운드 작업 등록

백그라운드 작업 등록 함수를 호출 하 여 백그라운드 작업을 등록 합니다. 백그라운드 작업 등록에 대 한 자세한 내용 및 아래 샘플 코드에서 **RegisterBackgroundTask ()** 메서드의 정의를 확인 하려면 [백그라운드 작업 등록](register-a-background-task.md)을 참조 하세요.

응용 프로그램 트리거를 사용 하 여 포그라운드 프로세스의 수명을 연장 하려는 경우에는 [확장 된 실행](run-minimized-with-extended-execution.md) 을 대신 사용 하는 것이 좋습니다. 응용 프로그램 트리거는에서 작업을 수행 하는 별도의 호스팅된 프로세스를 만들기 위한 것입니다. 다음 코드 조각은 out-of-process 백그라운드 트리거를 등록 합니다.

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

등록 시 백그라운드 작업 등록 매개 변수의 유효성이 검사 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱이 백그라운드 작업 등록에 실패 하는 시나리오를 정상적으로 처리 하는지 확인 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체가 있는 경우 충돌이 발생할 수 있습니다.

## <a name="trigger-the-background-task"></a>백그라운드 작업 트리거

백그라운드 작업을 트리거하기 전에 [BackgroundTaskRegistration](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 를 사용 하 여 백그라운드 태스크가 등록 되었는지 확인 합니다. 앱을 시작 하는 동안 모든 백그라운드 작업이 등록 되었는지 확인 하는 것이 좋습니다.

[Applicationtrigger. RequestAsync](/uwp/api/windows.applicationmodel.background.applicationtrigger)를 호출 하 여 백그라운드 작업을 트리거합니다. 모든 **Applicationtrigger** 인스턴스는이 작업을 수행 합니다.

**Applicationtrigger. RequestAsync** 는 백그라운드 작업 자체에서 호출 하거나 앱이 백그라운드 실행 상태에 있는 경우에는 응용 프로그램 상태에 대 한 자세한 내용은 [앱 수명 주기](app-lifecycle.md) 를 참조 하세요.
앱이 백그라운드 작업을 수행할 수 없도록 하는 에너지 또는 개인 정보 취급 방침을 설정한 경우 [Disabledbypolicy](/uwp/api/windows.applicationmodel.background.applicationtriggerresult) 가 반환 될 수 있습니다.
또한 한 번에 하나의 AppTrigger만 실행할 수 있습니다. 다른 사용자가 이미 실행 중인 상태에서 AppTrigger를 실행 하려고 하면 함수는 [Currentlyrunning](/uwp/api/windows.applicationmodel.background.applicationtriggerresult)을 반환 합니다.

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>백그라운드 작업에 대 한 리소스 관리

[BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) 를 사용 하 여 사용자가 앱의 백그라운드 활동을 제한 하도록 결정 했는지 확인 합니다. 사용자의 배터리 사용을 인식 하 고 사용자가 원하는 작업을 완료 하는 데 필요한 경우 백그라운드 에서만 실행 합니다. 백그라운드 작업에 대 한 설정을 사용자가 제어할 수 있는 방법에 대 한 자세한 내용은 [백그라운드 작업 최적화](../debug-test-perf/optimize-background-activity.md) 를 참조 하세요.  

- 메모리: 응용 프로그램의 메모리 및 에너지 사용 조정은 운영 체제에서 백그라운드 작업을 실행할 수 있도록 하기 위한 핵심입니다. [메모리 관리 api](/uwp/api/windows.system.memorymanager) 를 사용 하 여 백그라운드 작업에서 사용 중인 메모리 양을 확인 합니다. 백그라운드 작업에서 사용 하는 메모리가 많을 수록 다른 응용 프로그램이 전경에 있을 때 OS가 실행을 유지 하는 것이 더 어려워집니다. 사용자는 궁극적으로 앱에서 수행할 수 있는 모든 백그라운드 작업을 제어 하 고 앱이 배터리 사용에 미치는 영향을 볼 수 있습니다.  
- CPU 시간: 백그라운드 작업은 트리거 유형에 따라 표시 되는 벽 시계 사용 시간의 양에 따라 제한 됩니다. 응용 프로그램 트리거에서 트리거한 백그라운드 작업은 약 10 분으로 제한 됩니다.

백그라운드 작업에 적용 되는 리소스 제약 조건에 대 한 [백그라운드 작업으로 앱 지원](support-your-app-with-background-tasks.md) 을 참조 하세요.

## <a name="remarks"></a>설명

Windows 10부터 백그라운드 작업을 활용 하기 위해 사용자가 잠금 화면에 앱을 추가 하는 것은 더 이상 필요 하지 않습니다.

먼저 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출한 경우에만 백그라운드 작업이 **applicationtrigger** 를 사용 하 여 실행 됩니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [In-process 백그라운드 작업을 만들고 등록](create-and-register-an-inproc-background-task.md)합니다.
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [앱이 백그라운드로 이동할 때 메모리 회수](reduce-memory-usage.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [확장 실행을 사용하여 앱 일시 중단 연기](run-minimized-with-extended-execution.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)