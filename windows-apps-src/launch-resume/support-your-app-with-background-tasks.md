---
title: 백그라운드 작업을 사용하여 앱 지원
description: 이 단원의 항목에서는 트리거에 대 한 응답으로 백그라운드에서 경량 코드를 실행 하는 방법을 보여 줍니다.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.date: 08/21/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: ac3a20afc75cc7a6cf3c9f874fa26e1b6387dd89
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171847"
---
# <a name="support-your-app-with-background-tasks"></a>백그라운드 작업을 사용하여 앱 지원


이 단원의 항목에서는 트리거에 대 한 응답으로 백그라운드에서 경량 코드를 실행 하는 방법을 보여 줍니다. 백그라운드 작업을 사용 하 여 앱이 일시 중단 되거나 실행 되지 않는 경우 기능을 제공할 수 있습니다. VOIP, 메일, IM 등의 실시간 통신 앱에 대해 백그라운드 작업을 사용할 수도 있습니다.

## <a name="playing-media-in-the-background"></a>백그라운드에서 미디어 재생

Windows 10 버전 1607부터 백그라운드에서 오디오를 재생 하는 작업이 훨씬 쉬워졌습니다. 자세한 내용은 [배경의 미디어 재생](../audio-video-camera/background-audio.md) 을 참조 하세요.

## <a name="in-process-and-out-of-process-background-tasks"></a>In-process 및 out-of-process 백그라운드 작업

백그라운드 작업을 구현 하는 방법에는 두 가지가 있습니다.

* In-process: 앱과 백그라운드 프로세스가 동일한 프로세스에서 실행 됩니다.
* Out-of-process: 앱 및 백그라운드 프로세스가 별도의 프로세스에서 실행 됩니다.

백그라운드 작업 작성을 간소화 하기 위해 in-process 백그라운드 지원이 Windows 10 버전 1607에서 도입 되었습니다. 하지만 in-process 백그라운드 작업은 계속 작성할 수 있습니다. In-process 및 in-process 백그라운드 작업을 작성 하는 시기에 대 한 권장 사항은 [백그라운드 작업 지침](guidelines-for-background-tasks.md) 을 참조 하세요.

오류가 발생 하는 경우 백그라운드 프로세스에서 앱 프로세스를 중단 하지 않으므로 out-of-process 백그라운드 작업은 더 복원 력이 뛰어납니다. 그러나 복원 력은 앱과 백그라운드 작업 간의 크로스 프로세스 통신을 관리 하는 것이 더 복잡해 집니다.

Out-of-process 백그라운드 작업은 OS가 별도의 프로세스 (backgroundtaskhost.exe)에서 실행 되는 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하는 경량 클래스로 구현 됩니다. [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 클래스를 사용 하 여 백그라운드 작업을 등록 합니다. 클래스 이름은 백그라운드 작업을 등록할 때 진입점을 지정 하는 데 사용 됩니다.

Windows 10 버전 1607에서는 백그라운드 작업을 만들지 않고도 백그라운드 작업을 사용 하도록 설정할 수 있습니다. 대신 포그라운드 응용 프로그램의 프로세스 내에서 직접 백그라운드 코드를 실행할 수 있습니다.

In-process 백그라운드 작업을 빠르게 시작 하려면 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)을 참조 하세요.

Out-of-process 백그라운드 작업을 빠르게 시작 하려면 out-of-process [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조 하세요.

> [!TIP]
> Windows 10부터 백그라운드 작업을 등록 하기 위한 필수 조건으로 앱을 잠금 화면에 더 이상 추가할 필요가 없습니다.

## <a name="background-tasks-for-system-events"></a>시스템 이벤트에 대 한 백그라운드 작업

앱은 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) 클래스를 사용 하 여 백그라운드 작업을 등록 하 여 시스템 생성 이벤트에 응답할 수 있습니다. 앱은 다음과 같은 시스템 이벤트 트리거 ( [**Systemtriggertype**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)에 정의 됨)를 사용할 수 있습니다.

| 트리거 이름                     | Description                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | 인터넷을 사용할 수 있게 됩니다.                                                                                |
| **NetworkStateChange**           | 비용 또는 연결 변경과 같은 네트워크 변경이 발생 합니다.                                              |
| **OnlineIdConnectedStateChange** | 계정과 연결 된 온라인 ID입니다.                                                                 |
| **SmsReceived**                  | 설치 된 모바일 광대역 장치에서 새 SMS 메시지를 수신 합니다.                                         |
| **TimeZoneChange**               | 장치에서 표준 시간대가 변경 됩니다. 예를 들어 시스템에서 일광 절약 시간제에 대 한 시계를 조정 하는 경우입니다. |

자세한 내용은 [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)을 참조하세요.

## <a name="conditions-for-background-tasks"></a>백그라운드 작업에 대 한 조건

트리거 후에도 조건을 추가 하 여 백그라운드 태스크가 실행 되는 시간을 제어할 수 있습니다. 트리거된 후에는 모든 조건이 충족 될 때까지 백그라운드 작업이 실행 되지 않습니다. 다음 조건 ( [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) 열거로 표시 됨)을 사용할 수 있습니다.

| 조건 이름           | Description                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | 인터넷을 사용할 수 있어야 합니다.   |
| **InternetNotAvailable** | 인터넷을 사용할 수 없는 상태 여야 합니다. |
| **SessionConnected**     | 세션이 연결 되어 있어야 합니다.    |
| **SessionDisconnected**  | 세션의 연결을 끊어야 합니다. |
| **UserNotPresent**       | 사용자는 자리를 두어야 합니다.            |
| **UserPresent**          | 사용자가 있어야 합니다.         |

백그라운드 작업에 **InternetAvailable** 조건을 추가[BackgroundTaskBuilder.AddCondition](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)하여 네트워크 스택이 실행될 때까지 백그라운드 작업의 트리거를 지연시킵니다. 이 조건은 네트워크를 사용할 수 있을 때까지 백그라운드 작업이 실행 되지 않기 때문에 전력을 절약 합니다. 이 조건은 실시간 정품 인증을 제공하지 않습니다.

백그라운드 작업에 네트워크 연결이 필요한 경우 [Isnetworkrequested](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 을 설정 하 여 백그라운드 태스크가 실행 되는 동안 네트워크가 유지 되도록 합니다. 이렇게 하면 디바이스가 연결된 대기 상태 모드인 경우에도 작업 실행 중 네트워크를 계속 유지하도록 백그라운드 작업 인프라에 지시할 수 있습니다. 백그라운드 작업이 **Isnetworkrequested**을 설정 하지 않으면 연결 된 대기 모드에 있을 때 백그라운드 작업에서 네트워크에 액세스할 수 없게 됩니다. 예를 들어 휴대폰의 화면이 꺼져 있는 경우입니다.  
백그라운드 작업 조건에 대 한 자세한 내용은 [백그라운드 작업 실행을 위한 조건 설정](set-conditions-for-running-a-background-task.md)을 참조 하세요.

## <a name="application-manifest-requirements"></a>애플리케이션 매니페스트 요구 사항

앱이 out-of-process로 실행 되는 백그라운드 작업을 성공적으로 등록할 수 있기 전에 응용 프로그램 매니페스트에서 선언 해야 합니다. 호스트 앱과 동일한 프로세스에서 실행 되는 백그라운드 작업은 응용 프로그램 매니페스트에서 선언 하지 않아도 됩니다. 자세한 내용은 [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)을 참조 하세요.

## <a name="background-tasks"></a>백그라운드 작업

다음과 같은 실시간 트리거를 사용하면 백그라운드에서 경량 사용자 지정 코드를 실행할 수 있습니다.

| 실시간 트리거  | Description |
|--------------------|-------------|
| **컨트롤 채널** | 백그라운드 작업은 [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)를 사용 하 여 연결을 활성 상태로 유지 하 고 컨트롤 채널에서 메시지를 받을 수 있습니다. 앱이 소켓을 수신 대기 하는 경우 **ControlChannelTrigger**대신 소켓 브로커를 사용할 수 있습니다. Socket Broker 사용에 대 한 자세한 내용은 [Socketactivitytrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)를 참조 하세요. Windows Phone에서는 **ControlChannelTrigger** 이 지원 되지 않습니다. |
| **타이머** | 백그라운드 작업은 15 분 마다 자주 실행 되며, [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)를 사용 하 여 특정 시간에 실행 되도록 설정할 수 있습니다. 자세한 내용은 [타이머에서 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)을 참조 하세요. |
| **푸시 알림** | 백그라운드 작업은 푸시 알림 [**트리거에**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) 응답 하 여 원시 푸시 알림을 받습니다. |

**참고**  

유니버설 Windows 앱은 백그라운드 트리거 형식을 등록 하기 전에 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 해야 합니다.

업데이트를 릴리스된 후에도 유니버설 Windows 앱이 제대로 실행 되도록 하려면 [**Removeaccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) 를 호출한 다음 앱이 업데이트 된 후에 시작 될 때 [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 합니다. 자세한 내용은 [백그라운드 작업에 대 한 지침](guidelines-for-background-tasks.md)을 참조 하세요.

**트리거 인스턴스 수에 대 한 제한:** 앱이 등록할 수 있는 일부 트리거의 인스턴스 수에는 제한이 있습니다. 앱은 앱 인스턴스당   [Applicationtrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) 및 [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) 를 한 번만 등록할 수 있습니다. 앱이이 제한을 초과 하면 등록 시 예외가 throw 됩니다.

## <a name="system-event-triggers"></a>시스템 이벤트 트리거

[**Systemtriggertype**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 열거형은 다음 시스템 이벤트 트리거를 나타냅니다.

| 트리거 이름            | Description                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | 백그라운드 작업은 사용자가 있을 때 트리거됩니다.   |
| **UserAway**            | 백그라운드 작업은 사용자가 없을 때 트리거됩니다.    |
| **ControlChannelReset** | 백그라운드 작업은 컨트롤 채널이 다시 설정 될 때 트리거됩니다. |
| **SessionConnected**    | 세션이 연결 되 면 백그라운드 태스크가 트리거됩니다.   |

   
다음 시스템 이벤트는 사용자가 잠금 화면에서 앱을 이동 하거나 해제할 때 신호를 트리거합니다.

| 트리거 이름                     | Description                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded 됨**   | 앱 타일이 잠금 화면에 추가 됩니다.     |
| **LockScreenApplicationRemoved 됨** | 앱 타일이 잠금 화면에서 제거 됩니다. |

 
## <a name="background-task-resource-constraints"></a>백그라운드 작업 리소스 제약 조건

백그라운드 작업은 간단 합니다. 백그라운드 실행을 최소한으로 유지 하면 포그라운드 앱과 배터리 수명이 가장 뛰어난 사용자 환경을 유지할 수 있습니다. 이는 리소스 제약 조건을 백그라운드 작업에 적용 하 여 적용 됩니다.

백그라운드 작업은 30 초에서 벽 시계 사용으로 제한 됩니다.

### <a name="memory-constraints"></a>메모리 제약 조건

메모리 부족 장치에 대 한 리소스 제약 조건으로 인해 백그라운드 작업에는 백그라운드 작업에서 사용할 수 있는 최대 메모리 양을 결정 하는 메모리 제한이 있을 수 있습니다. 백그라운드 작업에서이 한도를 초과 하는 작업을 시도 하면 작업이 실패 하 고 작업에서 처리할 수 있는 메모리 부족 예외가 생성 될 수 있습니다. 태스크가 메모리 부족 예외를 처리 하지 않거나 시도 된 작업의 특성으로 인해 메모리 부족 예외가 생성 되지 않은 경우에는 작업이 즉시 종료 됩니다.  

[**Memorymanager**](/uwp/api/Windows.System.MemoryManager) api를 사용 하 여 현재 메모리 사용량을 쿼리하고 한도 (있는 경우)를 검색 하 여 백그라운드 작업의 진행 중인 메모리 사용을 모니터링할 수 있습니다.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>메모리가 부족 한 장치에 대 한 백그라운드 작업이 있는 앱에 대 한 장치 단위 제한

메모리 제한 장치에는 지정 된 시간에 장치에 설치 하 고 백그라운드 작업을 사용할 수 있는 앱 수에 제한이 있습니다. 이 수를 초과 하는 경우 모든 백그라운드 작업을 등록 하는 데 필요한 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)호출이 실패 합니다.

### <a name="battery-saver"></a>배터리 절약 모드

배터리 절약 기능이 설정 되어 있을 때 백그라운드 작업을 실행 하 고 푸시 알림을 받을 수 있도록 앱을 제외 하지 않는 한, 배터리 절약 기능을 사용 하도록 설정 하면 장치가 외부 전원에 연결 되어 있지 않고 배터리가 지정 된 총 전원으로 떨어지면 백그라운드 작업이 실행 되지 않습니다. 이렇게 하면 백그라운드 작업을 등록할 수 없습니다.

그러나 엔터프라이즈 응용 프로그램 및 Microsoft Store에 게시 되지 않는 앱의 경우 백그라운드 [에서 무기한 실행](run-in-the-background-indefinetly.md) 을 참조 하세요. 기능을 사용 하 여 백그라운드에서 백그라운드 작업 또는 확장 된 실행 세션을 무기한으로 실행 하는 방법을 알아보세요.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>실시간 통신을 위한 백그라운드 작업 리소스 보장

리소스 할당량이 실시간 통신 기능을 방해 하지 않도록 방지 하기 위해 [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 및 [**pushnotificationtrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) 를 사용 하는 백그라운드 작업은 실행 중인 모든 작업에 대해 보장 된 CPU 리소스 할당량을 받습니다. 리소스 할당량은 위에서 언급 한 것 이며 이러한 백그라운드 작업에 대해 일정 하 게 유지 됩니다.

앱은 [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 및 [**pushnotificationtrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) 백그라운드 작업에 대해 보장 된 리소스 할당량을 가져오기 위해 다른 작업을 수행할 필요가 없습니다. 시스템은 항상 이러한 작업을 중요 한 백그라운드 작업으로 처리 합니다.

## <a name="maintenance-trigger"></a>유지 관리 트리거

유지 관리 작업은 장치가 AC 전원에 연결 된 경우에만 실행 됩니다. 자세한 내용은 [유지 관리 트리거 사용](use-a-maintenance-trigger.md)을 참조 하세요.

## <a name="background-tasks-for-sensors-and-devices"></a>센서 및 장치에 대 한 백그라운드 작업

앱은 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 클래스를 사용 하 여 백그라운드 작업에서 센서 및 주변 장치에 액세스할 수 있습니다. 데이터 동기화 또는 모니터링과 같은 장기 실행 작업에이 트리거를 사용할 수 있습니다. 시스템 이벤트에 대 한 작업과 달리 **DeviceUseTrigger** 작업은 응용 프로그램이 포그라운드에서 실행 되는 동안에만 트리거될 수 있으며이 작업에는 조건을 설정할 수 없습니다.

> [!IMPORTANT]
> **DeviceUseTrigger** 및 **DeviceServicingTrigger** 는 in-process 백그라운드 작업과 함께 사용할 수 없습니다.

장기 실행 펌웨어 업데이트와 같은 일부 중요 한 장치 작업은 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)를 사용 하 여 수행할 수 없습니다. 이러한 작업은 PC 에서만 수행할 수 있으며 [**DeviceServicingTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger)를 사용 하는 권한 있는 앱 에서만 수행할 수 있습니다. *권한 있는 앱* 은 장치의 제조업체에서 해당 작업을 수행할 수 있는 권한을 부여 받은 앱입니다. 장치 메타 데이터는 장치에 대해 권한 있는 앱으로 지정 된 앱 (있는 경우)을 지정 하는 데 사용 됩니다. 자세한 내용은 장치 [동기화 및 Microsoft Store 장치 앱 업데이트](/windows-hardware/drivers/devapps/device-sync-and-update-for-uwp-device-apps) 를 참조 하세요.

## <a name="managing-background-tasks"></a>백그라운드 작업 관리

백그라운드 작업은 이벤트와 로컬 저장소를 사용 하 여 응용 프로그램에 진행률, 완료 및 취소를 보고할 수 있습니다. 앱은 백그라운드 작업에서 throw 된 예외를 catch 하 고 앱 업데이트 중에 백그라운드 작업 등록을 관리할 수도 있습니다. 자세한 내용은

[취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)  
[백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)

앱을 시작 하는 동안 백그라운드 작업 등록을 확인 합니다. 앱의 그룹화 되지 않은 백그라운드 작업이 BackgroundTaskBuilder 작업에 있는지 확인 합니다. 존재 하지 않는 항목을 다시 등록 합니다. 더 이상 필요 하지 않은 작업의 등록을 취소 합니다. 이렇게 하면 앱이 시작 될 때마다 모든 백그라운드 작업 등록이 최신 상태가 됩니다.

## <a name="related-topics"></a>관련 항목

**Windows 10의 멀티태스킹을 위한 개념적 지침**

* [Launching, resuming, and multitasking](index.md)

**관련 백그라운드 작업 지침**

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업에서 센서 및 디바이스에 액세스](access-sensors-and-devices-from-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [Out-of-process 백그라운드 작업을 in-process 백그라운드 작업으로 변환](convert-out-of-process-background-task.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [백그라운드 작업 등록 그룹화](group-background-tasks.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드에서 미디어 재생](../audio-video-camera/background-audio.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [UWP 앱 업데이트 시 백그라운드 작업 실행](run-a-background-task-during-updatetask.md)
* [백그라운드에서 무기한 실행](run-in-the-background-indefinetly.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [앱에서 백그라운드 작업 트리거](trigger-background-task-from-app.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)