---
title: 타이머에 따라 백그라운드 작업 실행
description: 일회성 백그라운드 작업을 예약 하거나 정기적인 백그라운드 작업을 실행 하는 방법에 대해 알아봅니다.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 7f9bf2ad18402976a7e089c2b2273e473819af1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175137"
---
# <a name="run-a-background-task-on-a-timer"></a>타이머에 따라 백그라운드 작업 실행

[**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger) 를 사용 하 여 일회성 백그라운드 작업을 예약 하거나 정기적인 백그라운드 작업을 실행 하는 방법에 대해 알아봅니다.

이 항목에서 설명 하는 시간 트리거된 백그라운드 작업을 구현 하는 방법의 예제를 보려면 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) 에서 **Scenario4** 를 참조 하세요.

이 항목에서는 정기적으로 또는 특정 시간에 실행 해야 하는 백그라운드 작업이 있다고 가정 합니다. 백그라운드 작업이 아직 없는 경우 [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)에 샘플 백그라운드 태스크가 있습니다. 또는 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [in-process 백그라운드 작업](create-and-register-a-background-task.md) 만들기 및 등록의 단계에 따라 하나를 만듭니다.

## <a name="create-a-time-trigger"></a>시간 트리거 만들기

새 [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)을 만듭니다. 두 번째 매개 변수인 *OneShot*는 백그라운드 작업을 한 번만 실행할지 아니면 정기적으로 실행할지를 지정 합니다. *OneShot* 가 true로 설정 된 경우 첫 번째 매개 변수 (*FreshnessTime*)는 백그라운드 작업을 예약 하기 전에 대기할 시간 (분)을 지정 합니다. *OneShot* 가 false로 설정 된 경우 *FreshnessTime* 은 백그라운드 작업이 실행 되는 빈도를 지정 합니다.

데스크톱 또는 모바일 장치 제품군을 대상으로 하는 UWP (유니버설 Windows 플랫폼) 앱에 대 한 기본 제공 타이머는 15 분 간격으로 백그라운드 작업을 실행 합니다. (타이머는 15 분 간격으로 실행 되므로 시스템이 15 분 마다 한 번만 실행 하 여 요청 된 타이머를 포함 하는 앱의 절전 모드를 해제 해야 합니다. 그러면 전원이 절약 됩니다.)

- *FreshnessTime* 을 15 분으로 설정 하 고 *OneShot* 를 true로 설정 하면 작업이 등록 된 시간부터 15 분부터 30 분 사이에 실행 되도록 예약 됩니다. 25 분으로 설정 되 고 *OneShot* 가 true 인 경우, 작업은 등록 된 시간부터 25 ~ 40 분부터 실행 되도록 예약 됩니다.

- *FreshnessTime* 가 15 분으로 설정 되 고 *OneShot* 이 false 인 경우, 작업은 등록 된 시간부터 15 분부터 30 분 마다 실행 되도록 예약 됩니다. N 분으로 설정 되 고 *OneShot* 가 false 이면 태스크가 등록 된 후 n 분에서 15 분 사이부터 n 분 마다 실행 되도록 예약 됩니다.

> [!NOTE]
> *FreshnessTime* 을 15 분 미만으로 설정 하면 백그라운드 작업을 등록 하려고 할 때 예외가 throw 됩니다.

예를 들어이 트리거는 백그라운드 작업이 한 시간에 한 번 실행 되도록 합니다.

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>필드 조건 추가

백그라운드 작업 조건을 만들어 태스크가 실행 되는 시간을 제어할 수 있습니다. 조건을 충족 하면 조건이 충족 될 때까지 백그라운드 작업이 실행 되지 않습니다. 자세한 내용은 [백그라운드 작업 실행을 위한 조건 설정](set-conditions-for-running-a-background-task.md)을 참조 하세요.

이 예제에서 조건은 사용자가 활성화 된 후에만 트리거되는, 트리거된 후에는 **Userpresent** 로 설정 됩니다. 가능한 조건 목록은 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)를 참조 하세요.

```cs
SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
```

```cpp
SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent);
```

백그라운드 트리거의 조건 및 유형에 대 한 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)을 참조 하세요.

##  <a name="call-requestaccessasync"></a>RequestAccessAsync () 호출

**Applicationtrigger** 백그라운드 작업을 등록 하기 전에 [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 하 여 사용자가 앱에 대 한 백그라운드 작업을 사용 하지 않도록 설정 했을 수 있으므로 사용자가 허용 하는 백그라운드 작업의 수준을 확인 합니다. 백그라운드 작업에 대 한 설정을 사용자가 제어할 수 있는 방법에 대 한 자세한 내용은 [백그라운드 작업 최적화](../debug-test-perf/optimize-background-activity.md) 를 참조 하세요.

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>백그라운드 작업 등록

백그라운드 작업 등록 함수를 호출 하 여 백그라운드 작업을 등록 합니다. 백그라운드 작업 등록에 대 한 자세한 내용 및 아래 샘플 코드에서 **RegisterBackgroundTask ()** 메서드의 정의를 확인 하려면 [백그라운드 작업 등록](register-a-background-task.md)을 참조 하세요.

> [!IMPORTANT]
> 앱과 동일한 프로세스에서 실행 되는 백그라운드 작업의 경우를 설정 하지 마십시오 `entryPoint` . 응용 프로그램에서 별도의 프로세스로 실행 되는 백그라운드 작업의 경우를 `entryPoint` 네임 스페이스, '. ' 및 백그라운드 작업 구현이 포함 된 클래스의 이름으로 설정 합니다.

```cs
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example hourly background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example hourly background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example hourly background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

등록 시 백그라운드 작업 등록 매개 변수의 유효성이 검사 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱이 백그라운드 작업 등록에 실패 하는 시나리오를 정상적으로 처리 하는지 확인 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체가 있는 경우 충돌이 발생할 수 있습니다.

## <a name="manage-resources-for-your-background-task"></a>백그라운드 작업에 대 한 리소스 관리

[BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) 를 사용 하 여 사용자가 앱의 백그라운드 활동을 제한 하도록 결정 했는지 확인 합니다. 사용자의 배터리 사용을 인식 하 고 사용자가 원하는 작업을 완료 하는 데 필요한 경우 백그라운드 에서만 실행 합니다. 백그라운드 작업에 대 한 설정을 사용자가 제어할 수 있는 방법에 대 한 자세한 내용은 [백그라운드 작업 최적화](../debug-test-perf/optimize-background-activity.md) 를 참조 하세요.

- 메모리: 응용 프로그램의 메모리 및 에너지 사용 조정은 운영 체제에서 백그라운드 작업을 실행할 수 있도록 하기 위한 핵심입니다. [메모리 관리 api](/uwp/api/windows.system.memorymanager) 를 사용 하 여 백그라운드 작업에서 사용 중인 메모리 양을 확인 합니다. 백그라운드 작업에서 사용 하는 메모리가 많을 수록 다른 응용 프로그램이 전경에 있을 때 OS가 실행을 유지 하는 것이 더 어려워집니다. 사용자는 궁극적으로 앱에서 수행할 수 있는 모든 백그라운드 작업을 제어 하 고 앱이 배터리 사용에 미치는 영향을 볼 수 있습니다.  
- CPU 시간: 백그라운드 작업은 트리거 유형에 따라 표시 되는 벽 시계 사용 시간의 양에 따라 제한 됩니다.

백그라운드 작업에 적용 되는 리소스 제약 조건에 대 한 [백그라운드 작업으로 앱 지원](support-your-app-with-background-tasks.md) 을 참조 하세요.

## <a name="remarks"></a>설명

Windows 10부터 백그라운드 작업을 활용 하기 위해 사용자가 잠금 화면에 앱을 추가 하는 것은 더 이상 필요 하지 않습니다.

[**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 먼저 호출한 경우에만 백그라운드 작업이 **TimeTrigger** 를 사용 하 여 실행 됩니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
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