---
title: 백그라운드 작업으로 시스템 이벤트에 응답
description: SystemTrigger 이벤트에 응답 하는 백그라운드 작업을 만드는 방법에 대해 알아봅니다.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: b66415b7f334eeaff7d29e2e11f111c15c718401
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164747"
---
# <a name="respond-to-system-events-with-background-tasks"></a>백그라운드 작업으로 시스템 이벤트에 응답

**중요 API**

- [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
- [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**형식의 systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger)

[**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 이벤트에 응답 하는 백그라운드 작업을 만드는 방법에 대해 알아봅니다.

이 항목에서는 사용자에 게 앱에 대해 작성 된 백그라운드 작업 클래스가 있고,이 작업은 인터넷 가용성 변경 또는 사용자 로그인 등 시스템에 의해 트리거되는 이벤트에 대 한 응답으로 실행 되어야 한다고 가정 합니다. 이 항목에서는 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 클래스에 대해 중점적으로 설명 합니다. 백그라운드 작업 클래스 작성에 대 한 자세한 내용은 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 또는 [In-process 백그라운드 작업 생성 및 등록](create-and-register-a-background-task.md)에서 사용할 수 있습니다.

## <a name="create-a-systemtrigger-object"></a>SystemTrigger 개체 만들기

앱 코드에서 새 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 개체를 만듭니다. 첫 번째 매개 변수인 *Triggertype*은이 백그라운드 작업을 활성화할 시스템 이벤트 트리거의 유형을 지정 합니다. 이벤트 유형 목록은 [**Systemtriggertype**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)을 참조 하세요.

두 번째 매개 변수인 *OneShot*는 다음에 시스템 이벤트가 발생 하거나 작업 등록이 취소 될 때까지 시스템 이벤트가 발생 될 때마다 백그라운드 작업을 실행할지 여부를 지정 합니다.

다음 코드는 인터넷을 사용할 수 있게 될 때마다 백그라운드 작업을 실행 하도록 지정 합니다.

```csharp
SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemTrigger internetTrigger{
    Windows::ApplicationModel::Background::SystemTriggerType::InternetAvailable, false};
```

```cpp
SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
```

## <a name="register-the-background-task"></a>백그라운드 작업 등록

백그라운드 작업 등록 함수를 호출 하 여 백그라운드 작업을 등록 합니다. 백그라운드 작업 등록에 대 한 자세한 내용은 [백그라운드 작업 등록](register-a-background-task.md)을 참조 하세요.

다음 코드는 out-of-process를 실행 하는 백그라운드 프로세스에 대 한 백그라운드 작업을 등록 합니다. 호스트 앱과 동일한 프로세스에서 실행 되는 백그라운드 작업을 호출 하는 경우 다음을 설정 하지 않습니다 `entrypoint` .

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass"; // Namespace name, '.', and the name of the class containing the background task
string taskName   = "Internet-based background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" }; // don't set for in-process background tasks.
std::wstring taskName{ L"Internet-based background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass"; // don't set for in-process background tasks
String ^ taskName   = "Internet-based background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

> [!NOTE]
> 유니버설 Windows 플랫폼 앱은 백그라운드 트리거 형식을 등록 하기 전에 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다.

업데이트를 릴리스된 후에도 유니버설 Windows 앱이 제대로 실행 되도록 하려면 [**Removeaccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) 를 호출한 다음 앱이 업데이트 된 후에 시작 될 때 [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다. 자세한 내용은 [백그라운드 작업에 대 한 지침](guidelines-for-background-tasks.md)을 참조 하세요.

> [!NOTE]
> 등록 시 백그라운드 작업 등록 매개 변수의 유효성이 검사 됩니다. 등록 매개 변수가 잘못 된 경우 오류가 반환 됩니다. 앱이 백그라운드 작업 등록에 실패 하는 시나리오를 정상적으로 처리 하는지 확인 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체가 있는 경우 충돌이 발생할 수 있습니다.
 
## <a name="remarks"></a>설명

백그라운드 작업 등록의 작동 방식을 확인 하려면 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)을 다운로드 합니다.

백그라운드 작업은 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 및 [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 이벤트에 대 한 응답으로 실행할 수 있지만 여전히 [응용 프로그램 매니페스트에서 백그라운드 작업을 선언](declare-background-tasks-in-the-application-manifest.md)해야 합니다. 또한 백그라운드 작업 형식을 등록 하기 전에 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다.

앱은 [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger), [**Pushnotificationtrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)및 [**NetworkOperatorNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.NetworkOperatorNotificationTrigger) 이벤트에 응답 하는 백그라운드 작업을 등록 하 여 앱이 전경에 있지 않아도 사용자와 실시간 통신을 제공할 수 있도록 합니다. 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))