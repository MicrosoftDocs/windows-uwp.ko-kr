---
author: TylerMSFT
title: 타이머에 따라 백그라운드 작업 실행
description: 일회성 백그라운드 작업을 예약하거나 정기적 백그라운드 작업을 실행하는 방법을 알아봅니다.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 25e3c76ae09ed6835f89f0d98c308f11c7a99624
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4426919"
---
# <a name="run-a-background-task-on-a-timer"></a>타이머에 따라 백그라운드 작업 실행

[**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 일회성 백그라운드 작업을 예약 하거나 정기적 백그라운드 작업 실행을 사용 하는 방법을 알아봅니다.

이 항목에서 트리거된 백그라운드 작업을 설명 하는 시간을 구현 하는 방법의 예제를 보려면 [백그라운드 활성화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) **Scenario4** 참조 하세요.

이 항목에서는 정기적으로 또는 특정 시간에 실행 해야 하는 백그라운드 작업이 있다고 가정 합니다. 백그라운드 작업을 아직 없는 경우 백그라운드 작업 샘플 [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)에 없습니다. 또는 [in-process 프로세스 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [out of process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md) 중 하나를 만드는 단계를 수행 합니다.

## <a name="create-a-time-trigger"></a>시간 트리거 만들기

새 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)를 만듭니다. 두 번째 매개 변수인 *OneShot*은 백그라운드 작업을 한 번만 실행할지, 아니면 정기적으로 계속 실행할지를 지정합니다. *OneShot*이 true로 설정된 경우 첫 번째 매개 변수(*FreshnessTime*)는 백그라운드 작업을 예약하기 전에 대기할 시간(분)을 지정합니다. *OneShot*이 false로 설정된 경우 *FreshnessTime*은 백그라운드 작업의 실행 빈도를 지정합니다.

데스크톱 또는 모바일 장치 패밀리를 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 기본 제공 타이머는 15분 간격으로 백그라운드 작업을 실행합니다. (타이머가 15 분 간격으로 시스템만 15 분 마다 절전 모드 해제 요청한 TimerTriggers-전원 저장 하는 앱을 절전 모드 해제 해야 할 수 있도록 합니다.)

- *FreshnessTime*이 15분으로 설정되고 *OneShot*이 true이면 작업이 등록된 시간부터 15분에서 30분 사이에 한 번 실행되도록 예약됩니다. 25분으로 설정되고 *OneShot*이 true이면 작업이 등록된 시간부터 25분에서 40분 사이에 한 번 실행되도록 예약됩니다.

- *FreshnessTime*이 15분으로 설정되고 *OneShot*이 false이면 작업이 등록된 시간부터 15분에서 30분 사이에 시작하여 15분마다 실행되도록 예약됩니다. n분으로 설정되고 *OneShot*이 false이면 작업이 등록 후 n+15분 사이에 시작하여 n분마다 실행되도록 예약됩니다.

> [!NOTE]
> *FreshnessTime* 15 분 미만으로 설정 되 면 백그라운드 작업을 등록 하려고 하면 예외가 throw 됩니다.
 
예를 들어이 트리거를 사용 하면 백그라운드 작업을 한 시간에 한 번 실행 합니다.

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>(옵션) 조건 추가

작업을 실행 하는 경우 제어 하는 백그라운드 작업 조건을 만들 수 있습니다. 조건은 조건이 충족 될 때까지 실행에서 백그라운드 작업을 방지 합니다. 자세한 내용은 [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)을 참조 하세요.

이 예제에서는 트리거되면 사용자가 활성 상태일 때만 작업이 실행되도록 조건을 **UserPresent**로 설정합니다. 가능한 조건 목록은 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)을 참조하세요.

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

자세한 내용은 조건 및 백그라운드 트리거 유형에 대 한 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)를 참조 하세요.

##  <a name="call-requestaccessasync"></a>RequestAccessAsync() 호출

**ApplicationTrigger** 백그라운드 작업을 등록 하기 전에 사용자가 앱에 대 한 백그라운드 작업을 비활성화 한 수 있으므로 사용자가 허용 되는 백그라운드 작업의 수준을 확인 하려면 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) 호출 합니다. [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) 방법으로 사용자에 대 한 자세한 정보에 대 한 백그라운드 작업에 대 한 설정을 제어할 수를 참조 하세요.

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>백그라운드 작업 등록

백그라운드 작업 등록 함수를 호출하여 백그라운드 작업을 등록합니다. 백그라운드 작업 등록 및 아래 샘플 코드에서는 **RegisterBackgroundTask()** 메서드 정의 확인 합니다. 자세한 내용은 [백그라운드 작업 등록](register-a-background-task.md)을 참조 하세요.

> [!IMPORTANT]
> 앱과 동일한 프로세스에서 실행 되는 백그라운드 작업을 설정 하지 않으면 `entryPoint`. 앱에서 별도 프로세스에서 실행 되는 백그라운드 작업을 설정 `entryPoint` 네임 스페이스, '.', 백그라운드 작업 구현을 포함 하는 클래스의 이름.

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

백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.

## <a name="manage-resources-for-your-background-task"></a>백그라운드 작업에 대 한 리소스를 관리 합니다.

[BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx)를 사용하면 사용자가 앱의 백그라운드 작업을 제한하기로 결정했는지 확인할 수 있습니다. 배터리 사용 정보를 파악하고 사용자가 원하는 작업을 완료해야 하는 경우에 백그라운드에서만 실행해야 합니다. [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) 방법으로 사용자에 대 한 자세한 정보에 대 한 백그라운드 작업에 대 한 설정을 제어할 수를 참조 하세요.

- 메모리: 키가 있는지 확인 하는 운영 체제를 실행 하려면 백그라운드 작업은 앱의 메모리와 에너지 사용을 조정 합니다. [메모리 관리 Api](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 를 사용 하 여 백그라운드 작업을 사용 하는 메모리 양을 확인 합니다. 백그라운드 작업 사용 하 여 더 많은 메모리, 다른 앱이 포그라운드에서 실행 상태로 유지 하려면 운영 체제에 대 한 더 어려워집니다. 사용자는 앱이 수행할 수 있는 모든 백그라운드 작업을 근본적으로 제어하며 앱이 배터리 사용에 미치는 영향을 한 눈에 볼 수 있게 됩니다.  
- CPU 시간: 백그라운드 작업은 트리거 유형에 따라 가져오는 벽 시계 시간으로 제한 됩니다.

백그라운드 작업에 적용되는 리소스 제약 조건은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

## <a name="remarks"></a>설명

Windows 10부터 더 이상 필요는 사용자가 앱을 잠금 화면에 백그라운드 작업을 활용 하기 위해 추가.

백그라운드 작업을 먼저 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 호출한 경우 **TimeTrigger** 를 사용 하 여 실행 됩니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
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
