---
title: 유지 관리 트리거 사용
description: 장치가 연결 되어 있는 동안 MaintenanceTrigger 클래스를 사용 하 여 백그라운드에서 경량 코드를 실행 하는 방법에 대해 알아봅니다.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: c390280e084cd8557633f591b1686a0550f6a656
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155767"
---
# <a name="use-a-maintenance-trigger"></a>유지 관리 트리거 사용

**중요 API**

- [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)
- [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition)

장치가 연결 되어 있는 동안 [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 클래스를 사용 하 여 백그라운드에서 경량 코드를 실행 하는 방법에 대해 알아봅니다.

## <a name="create-a-maintenance-trigger-object"></a>유지 관리 트리거 개체 만들기

이 예에서는 장치가 연결 되어 있는 동안 백그라운드에서 실행 하 여 앱을 향상 시킬 수 있는 경량 코드가 있다고 가정 합니다. 이 항목에서는 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)유사한 [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)에 중점을 둘 수 있습니다.

백그라운드 작업 클래스 작성에 대 한 자세한 내용은 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [In-process 백그라운드 작업 생성 및 등록](create-and-register-a-background-task.md)에서 사용할 수 있습니다.

새 [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 개체를 만듭니다. 두 번째 매개 변수인 *OneShot*는 유지 관리 작업을 한 번만 실행할지 아니면 정기적으로 실행할지를 지정 합니다. *OneShot* 가 true로 설정 된 경우 첫 번째 매개 변수 (*FreshnessTime*)는 백그라운드 작업을 예약 하기 전에 대기할 시간 (분)을 지정 합니다. *OneShot* 가 false로 설정 된 경우 *FreshnessTime* 는 백그라운드 작업을 실행할 빈도를 지정 합니다.

> [!NOTE]
> *FreshnessTime* 을 15 분 미만으로 설정 하면 백그라운드 작업을 등록 하려고 할 때 예외가 throw 됩니다.

이 예제 코드는 한 시간에 한 번 실행 되는 트리거를 만듭니다.

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

## <a name="optional-add-a-condition"></a>필드 조건 추가

- 필요한 경우 백그라운드 작업 조건을 만들어 태스크가 실행 되는 시기를 제어 합니다. 조건을 충족 하면 조건이 충족 될 때까지 백그라운드 작업이 실행 되지 않습니다. 자세한 내용은 [백그라운드 작업을 실행 하기 위한 조건 설정](set-conditions-for-running-a-background-task.md) 을 참조 하세요.

이 예제에서 조건은 인터넷을 사용할 수 있을 때 (또는 사용할 수 있게 되 면) 유지 관리가 실행 되도록 **Internetavailable** 로 설정 됩니다. 가능한 백그라운드 작업 조건 목록은 [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)를 참조 하세요.

다음 코드는 유지 관리 작업 빌더에 조건을 추가 합니다.

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

- 백그라운드 작업 등록 함수를 호출 하 여 백그라운드 작업을 등록 합니다. 백그라운드 작업 등록에 대 한 자세한 내용은 [백그라운드 작업 등록](register-a-background-task.md)을 참조 하세요.

다음 코드는 유지 관리 작업을 등록 합니다. 백그라운드 작업은를 지정 하므로 응용 프로그램에서 별도의 프로세스를 실행 하는 것으로 가정 합니다 `entryPoint` . 백그라운드 태스크가 앱과 동일한 프로세스에서 실행 되는 경우을 지정 하지 않습니다 `entryPoint` .

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
> 데스크톱을 제외한 모든 장치 패밀리의 경우 장치의 메모리가 부족 해지면 백그라운드 작업이 종료 될 수 있습니다. 메모리 부족 예외가 표시 되지 않거나 앱에서 처리 하지 않는 경우 백그라운드 작업은 경고 없이 종료 되 고 OnCanceled 이벤트를 발생 시 키 지 않습니다. 이를 통해 포그라운드에서 앱의 사용자 환경을 보장할 수 있습니다. 백그라운드 작업은이 시나리오를 처리 하도록 디자인 되어야 합니다.

> [!NOTE]
> 유니버설 Windows 플랫폼 앱은 백그라운드 트리거 형식을 등록 하기 전에 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다.

앱에 대 한 업데이트를 릴리스된 후에도 유니버설 Windows 앱이 제대로 실행 되도록 하려면 [**Removeaccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) 를 호출한 다음 앱이 업데이트 된 후에 시작 될 때 [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다. 자세한 내용은 [백그라운드 작업에 대 한 지침](guidelines-for-background-tasks.md)을 참조 하세요.

> [!NOTE]
> 등록 시 백그라운드 작업 등록 매개 변수의 유효성이 검사 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱이 백그라운드 작업 등록에 실패 하는 시나리오를 정상적으로 처리 하는지 확인 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체가 있는 경우 충돌이 발생할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [In-process 백그라운드 작업을 만들고 등록](create-and-register-an-inproc-background-task.md)합니다.
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))