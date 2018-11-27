---
title: 유지 관리 트리거 사용
description: 디바이스가 연결되어 있는 동안 MaintenanceTrigger 클래스를 사용하여 경량 코드를 실행하는 방법을 알아봅니다.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 87df67c480c18ef2c75a9c63d538f0107908ca10
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7698997"
---
# <a name="use-a-maintenance-trigger"></a>유지 관리 트리거 사용

**중요 API**

- [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

디바이스가 연결되어 있는 동안 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 클래스를 사용하여 경량 코드를 실행하는 방법을 알아봅니다.

## <a name="create-a-maintenance-trigger-object"></a>유지 관리 트리거 개체 만들기

이 예에서는 장치가 연결되어 있는 동안 백그라운드에서 실행하여 앱의 성능을 향상시킬 수 있는 경량 코드가 있다고 가정합니다. 이 항목에서는 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)와 유사한 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)에 중점을 둡니다.

백그라운드 작업 클래스를 작성하는 방법은 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요.

새 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 개체를 만듭니다. 두 번째 매개 변수인 *OneShot*은 유지 관리 작업이 한 번만 실행되는지, 아니면 정기적으로 계속 실행되는지를 지정합니다. *OneShot*이 true로 설정된 경우 첫 번째 매개 변수(*FreshnessTime*)는 백그라운드 작업을 예약하기 전에 대기할 시간(분)을 지정합니다. *OneShot*이 false로 설정된 경우 *FreshnessTime*은 백그라운드 작업의 실행 빈도를 지정합니다.

> [!NOTE]
> *FreshnessTime* 15 분 미만으로 설정 되 면 백그라운드 작업을 등록 하려고 하면 예외가 throw 됩니다.

이 예제 코드에서는 한 시간에 한 번 실행 되는 트리거를 만듭니다.

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>(옵션) 조건 추가

- 필요한 경우 작업이 실행되는 시간을 제어하는 백그라운드 작업 조건을 만듭니다. 그러면 조건이 충족되는 경우에만 백그라운드 작업이 실행됩니다. 자세한 내용은 [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)을 참조하세요.

이 예에서는 인터넷을 사용할 수 있거나 인터넷이 사용할 수 있게 될 때 유지 관리 작업이 실행되도록 조건을 **InternetAvailable**로 설정합니다. 가능한 백그라운드 작업 조건 목록은 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)을 참조하세요.

다음 코드는 유지 관리 작업 작성기에 조건을 추가합니다.

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>백그라운드 작업 등록

- 백그라운드 작업 등록 함수를 호출하여 백그라운드 작업을 등록합니다. 백그라운드 작업 등록에 대한 자세한 내용은 [백그라운드 작업 등록](register-a-background-task.md)을 참조하세요.

다음 코드는 유지 관리 작업을 등록합니다. 이 코드는 `entryPoint`를 지정하므로 백그라운드 작업이 앱과 별도의 프로세스로 실행된다고 간주됩니다. 백그라운드 작업이 앱과 동일한 프로세스에서 실행되는 경우 `entryPoint`를 지정하지 마세요.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> 데스크톱을 제외한 모든 디바이스 패밀리의 경우 장치의 메모리가 부족해지면 백그라운드 작업이 종료될 수 있습니다. 메모리 부족 예외가 표시되지 않거나 앱에서 처리하지 않는 경우 백그라운드 작업이 OnCanceled 이벤트를 발생시키지 않고 경고 없이 종료됩니다. 이는 포그라운드에서 앱의 사용자 환경을 확인하는 데 도움이 됩니다. 백그라운드 작업은 이 시나리오를 처리하도록 설계되어야 합니다.

> [!NOTE]
> 유니버설 Windows 플랫폼 앱 백그라운드 트리거 형식을 등록 하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 를 호출 해야 합니다.

앱에 대한 업데이트를 릴리스한 후 유니버설 Windows 앱이 계속해서 제대로 실행되도록 하려면 앱이 업데이트된 후 시작될 때 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) 및 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 차례로 호출해야 합니다. 자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

> [!NOTE]
> 백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md).
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)
