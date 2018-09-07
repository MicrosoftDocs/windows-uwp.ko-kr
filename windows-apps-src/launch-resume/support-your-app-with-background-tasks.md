---
author: TylerMSFT
title: 백그라운드 작업을 사용하여 앱 지원
description: 이 섹션의 항목에서는 트리거에 대한 응답으로 백그라운드에서 경량 코드가 실행되도록 하는 방법을 보여 줍니다.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.author: twhitney
ms.date: 08/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 9e5db1e03ac86768e2b1b1181cd2cc416a151a80
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3664835"
---
# <a name="support-your-app-with-background-tasks"></a>백그라운드 작업을 사용하여 앱 지원


이 섹션의 항목에서는 트리거에 대한 응답으로 백그라운드에서 경량 코드가 실행되도록 하는 방법을 보여 줍니다. 앱이 일시 중단되거나 실행되지 않을 때 백그라운드 작업을 사용하여 기능을 제공할 수 있습니다. VOIP, 메일 및 IM 같은 실시간 통신 앱에 백그라운드 작업을 사용할 수도 있습니다.

## <a name="playing-media-in-the-background"></a>백그라운드에서 미디어 재생

Windows10 버전 1607부터 백그라운드에서 오디오를 재생하는 작업이 훨씬 쉬워졌습니다. 자세한 내용은 [백그라운드에서 미디어 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)을 참조하세요.

## <a name="in-process-and-out-of-process-background-tasks"></a>In-process 및 out-of-process 백그라운드 작업

백그라운드 작업을 구현 하는 방법은 두 가지가 있습니다.

* 동일한 프로세스에서 실행 앱과 백그라운드 프로세스가 in process:
* Out of process: 앱과 백그라운드 프로세스가 별도 프로세스에서 실행합니다.

In-process 백그라운드 지원은 쓰기 백그라운드 작업을 단순화하기 위해 Windows10 버전 1607에서 처음 소개되었습니다. 그러나 Out of Process 백그라운드 작업도 계속 작성할 수 있습니다. In Process 및 Out of Process 백그라운드 작업을 작성하는 경우에 대한 자세한 내용은 [백그라운드 작업 지침](guidelines-for-background-tasks.md)을 참조하세요.

Out of process 백그라운드 작업은 없기 때문에 백그라운드 프로세스 앱 프로세스에 문제가 있으면 훨씬 복원성이 큽니다. 그러나 가격으로 앱과 백그라운드 작업 간의 프로세스 간 통신 관리가 더 복잡 합니다.

Out of process 백그라운드 작업은 OS가 별도 프로세스 (backgroundtaskhost.exe)에서 실행 되는 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 인터페이스를 구현 하는 경량 클래스로 구현 됩니다. [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 클래스를 사용 하 여 백그라운드 작업을 등록 합니다. 클래스 이름은 백그라운드 작업을 등록할 때 진입점을 지정하는 데 사용됩니다.

Windows10 버전 1607에서는 백그라운드 작업을 만들지 않고 백그라운드 활동을 사용하도록 설정할 수 있습니다. 대신 포그라운드 응용 프로그램 프로세스 내에서 직접 백그라운드 코드를 실행할 수 있습니다.

In-process 백그라운드 작업을 빠르게 시작하려면 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)을 참조하세요.

Out-of-process 백그라운드 작업을 빠르게 시작하려면 [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요.

> [!TIP]
> Windows 10부터 더 이상 백그라운드 작업을 등록하기 위해 앱을 잠금 화면에 배치할 필요가 없습니다.

## <a name="background-tasks-for-system-events"></a>시스템 이벤트에 대한 백그라운드 작업

[**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 클래스를 사용하여 백그라운드 작업을 등록하면 개발자 앱에서 시스템 생성 이벤트에 응답할 수 있습니다. 앱에서 다음과 같은 시스템 이벤트 트리거([**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)에 정의되어 있음)를 사용할 수 있습니다.

| 트리거 이름                     | 설명                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | 인터넷을 사용할 수 있게 됩니다.                                                                                |
| **NetworkStateChange**           | 네트워크 변경(예제: 비용 또는 연결 변경)이 발생합니다.                                              |
| **OnlineIdConnectedStateChange** | 계정에 연결된 온라인 ID가 변경됩니다.                                                                 |
| **SmsReceived**                  | 설치된 모바일 광대역 장치에 새 SMS 메시지가 수신됩니다.                                         |
| **TimeZoneChange**               | 장치의 표준 시간대가 변경됩니다(예제: 시스템이 일광 절약 시간에 맞게 시계를 조정할 때). |

자세한 내용은 [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)을 참조하세요.

## <a name="conditions-for-background-tasks"></a>백그라운드 작업의 조건

백그라운드 작업이 트리거된 후에도 조건을 추가하면 해당 작업이 실행되는 시기를 제어할 수 있습니다. 트리거된 백그라운드 작업은 조건이 모두 충족될 때까지 실행되지 않습니다. 다음과 같은 조건([**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 열거로 표시됨)을 사용할 수 있습니다.

| 조건 이름           | 설명                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | 인터넷을 사용할 수 있어야 합니다.   |
| **InternetNotAvailable** | 인터넷을 사용할 수 없어야 합니다. |
| **SessionConnected**     | 세션이 연결되어야 합니다.    |
| **SessionDisconnected**  | 세션 연결이 끊어져야 합니다. |
| **UserNotPresent**       | 사용자가 없어야 합니다.            |
| **UserPresent**          | 사용자가 있어야 합니다.         |

백그라운드 작업에 **InternetAvailable** 조건을 추가[BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)하여 네트워크 스택이 실행될 때까지 백그라운드 작업의 트리거를 지연시킵니다. 이 조건은 네트워크를 사용할 수 있을 때까지 백그라운드 작업이 실행 전원을 절약 됩니다. 이 조건은 실시간 정품 인증을 제공하지 않습니다.

백그라운드 작업이 필요한 경우 네트워크 연결, [IsNetworkRequested](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 백그라운드 작업이 실행 되는 동안 네트워크 가동 되도록 설정 합니다. 이렇게 하면 디바이스가 연결된 대기 상태 모드인 경우에도 작업 실행 중 네트워크를 계속 유지하도록 백그라운드 작업 인프라에 지시할 수 있습니다. 백그라운드 작업 **IsNetworkRequested**을 설정 하지 않으면 다음 백그라운드 작업이 됩니다 네트워크 연결 된 대기 상태 모드 (예: 휴대폰 화면이 꺼져 있습니다.)에 액세스할 수
 
백그라운드 작업 조건에 대 한 자세한 정보는 [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)을 참조 하세요.

## <a name="application-manifest-requirements"></a>응용 프로그램 매니페스트 요구 사항

Out-of-process에서 실행되는 백그라운드 작업을 성공적으로 등록하려면 먼저 응용 프로그램 매니페스트에서 앱을 선언해야 합니다. 호스트 앱과 동일한 프로세스에서 실행되는 백그라운드 작업은 응용 프로그램 매니페스트에서 선언할 필요가 없습니다. 자세한 내용은 [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)을 참조하세요.

## <a name="background-tasks"></a>백그라운드 작업

다음과 같은 실시간 트리거를 사용하면 백그라운드에서 경량 사용자 지정 코드를 실행할 수 있습니다.

| 실시간 트리거  | 설명 |
|--------------------|-------------|
| **컨트롤 채널** | 백그라운드 작업은 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)를 사용하여 연결을 활성 상태로 유지하고 컨트롤 채널에서 메시지를 수신할 수 있습니다. 앱에서 소켓을 수신 대기하는 경우 **ControlChannelTrigger** 대신 소켓 브로커를 사용할 수 있습니다. 소켓 브로커 사용에 대한 자세한 내용은 [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009)를 참조하세요. **ControlChannelTrigger**는 Windows Phone에서 지원되지 않습니다. |
| **타이머** | [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)를 사용하여 백그라운드 작업을 자주(15분마다) 실행하거나 특정 시간에 실행하도록 설정할 수 있습니다. 자세한 내용은 [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)을 참조하세요. |
| **푸시 알림** | 백그라운드 작업은 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)에 응답하여 원시 푸시 알림을 수신합니다. |

**참고**  

유니버설 Windows 앱에서 백그라운드 트리거 형식을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다.

업데이트를 릴리스한 후 유니버설 Windows 앱이 계속해서 제대로 실행되도록 하려면 앱이 업데이트된 후 시작될 때 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) 및 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 차례로 호출합니다. 자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

**트리거 인스턴스 수에 대한 제한:** 앱에서 등록할 수 있는 일부 트리거의 인스턴스 수에 대한 제한이 있습니다. 앱은 앱 인스턴스당 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) 및 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)를 한 번씩만 등록할 수 있습니다. 앱이 이 제한을 초과하는 경우 등록이 예외를 throw합니다.

## <a name="system-event-triggers"></a>시스템 이벤트 트리거

[**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) 열거형은 다음과 같은 시스템 이벤트 트리거를 나타냅니다.

| 트리거 이름            | 설명                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | 사용자가 있게 되면 백그라운드 작업이 트리거됩니다.   |
| **UserAway**            | 사용자가 없게 되면 백그라운드 작업이 트리거됩니다.    |
| **ControlChannelReset** | 컨트롤 채널이 초기화되면 백그라운드 작업이 트리거됩니다. |
| **SessionConnected**    | 세션이 연결되면 백그라운드 작업이 트리거됩니다.   |

   
다음과 같은 시스템 이벤트는 사용자가 앱을 잠금 화면에서 옮겼거나 잠금 화면 밖으로 옮길 때 신호를 트리거합니다.

| 트리거 이름                     | 설명                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | 앱 타일이 잠금 화면에 추가되었습니다.     |
| **LockScreenApplicationRemoved** | 앱 타일이 잠금 화면에서 제거되었습니다. |

 
## <a name="background-task-resource-constraints"></a>백그라운드 작업 리소스 제약 조건

백그라운드 작업은 경량입니다. 백그라운드 실행을 최소로 유지하여 포그라운드 앱 및 배터리 수명을 위한 최적의 사용자 환경을 보장해야 합니다. 이렇게 하려면 백그라운드 작업에 리소스 제약 조건을 적용합니다.

백그라운드 작업은 벽시계로 측정하는 30초로 제한됩니다.

### <a name="memory-constraints"></a>메모리 제약 조건

메모리가 부족한 장치에 대한 리소스 제약 조건 때문에 백그라운드 작업에는 백그라운드 작업에서 사용할 수 있는 최대 메모리 양을 결정하는 메모리 제한이 있을 수 있습니다. 백그라운드 작업에서 이 제한을 초과하는 작업을 시도하는 경우 작업이 실패하고 작업이 처리할 수 있는 메모리 부족 예외가 생성될 수 있습니다. 작업에서 메모리 부족 예외를 처리하지 않거나 시도된 작업의 특성상 메모리 부족 예외가 생성되지 않는 경우에는 작업이 즉시 종료됩니다.  

[**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) API를 사용하면 한도를 검색하기 위해(있는 경우) 현재 메모리 사용량 및 제한을 쿼리하고 백그라운드 작업의 지속적인 메모리 사용량을 모니터링할 수 있습니다.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>메모리가 부족한 장치에서 백그라운드 작업이 있는 앱의 장치별 제한

메모리가 제한된 장치에서는 장치에 설치되고 지정된 시간에 백그라운드 작업을 사용할 수 있는 앱 수가 제한됩니다. 이 개수를 초과할 경우 모든 백그라운드 작업을 등록하는 데 필요한 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 호출이 실패합니다.

### <a name="battery-saver"></a>배터리 절약 모드

배터리 절약 모드가 켜져 있을 때 앱이 계속해서 백그라운드 작업을 실행하고 푸시 알림을 받을 수 있도록 앱에 예외를 적용하지 않으면, 배터리 절약 모드를 사용하도록 설정한 경우 장치가 외부 전원에 연결되어 있지 않고 배터리가 지정된 전원 잔량보다 적을 때 백그라운드 작업이 실행되지 않습니다. 백그라운드 작업을 등록할 수는 있습니다.

그러나 엔터프라이즈 앱 및 Microsoft Store에 게시 하지는 앱에 대 한 백그라운드 작업 또는 확장된 실행 세션을 백그라운드에서 무기한 실행 하는 기능을 사용 하는 방법을 알아보려면 [백그라운드에서 무기한 실행](run-in-the-background-indefinetly.md) 를 표시 합니다.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>백그라운드 작업 리소스는 실시간 통신을 보장합니다.

리소스 할당량으로 인해 실시간 통신 기능이 방해 받지 않도록 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 및 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)를 사용하는 백그라운드 작업은 모든 실행 중인 작업에 대해 보장된 CPU 리소스 할당량을 수신합니다. 리소스 할당량은 위에서 설명한 것과 같으며, 이러한 백그라운드 작업에 대해 일정하게 유지됩니다.

앱에서 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 및 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 백그라운드 작업에 대해 보장된 리소스 할당량을 가져오기 위해 별도의 작업을 수행할 필요는 없습니다. 시스템에서는 항상 이러한 작업을 중요 백그라운드 작업으로 처리합니다.

## <a name="maintenance-trigger"></a>유지 관리 트리거

유지 관리 작업은 장치가 AC 전원에 연결되는 경우에만 실행됩니다. 자세한 내용은 [유지 관리 트리거 사용](use-a-maintenance-trigger.md)을 참조하세요.

## <a name="background-tasks-for-sensors-and-devices"></a>센서 및 장치에 대한 백그라운드 작업

앱은 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 클래스를 사용하여 백그라운드 작업에서 센서 및 주변 장치에 액세스할 수 있습니다. 데이터 동기화나 모니터링 같이 오래 실행되는 작업에 이 트리거를 사용할 수 있습니다. 시스템 이벤트에 대한 작업과 달리 **DeviceUseTrigger** 작업은 앱이 포그라운드로 실행되고 조건이 설정되지 않은 경우에만 트리거할 수 있습니다.

> [!IMPORTANT]
> **DeviceUseTrigger** 및 **DeviceServicingTrigger**를 in-process 백그라운드 작업과 함께 사용할 수 없습니다.

장기 실행 펌웨어 업데이트와 같은 일부 중요한 장치 작업은 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)로 수행할 수 없습니다. 이러한 작업은 PC에서만 수행할 수 있으며 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315)를 사용하는 권한 있는 앱만 수행할 수 있습니다. *권한 있는 앱*은 제조업체가 이러한 작업을 수행할 권한을 부여한 앱입니다. 장치 메타데이터는 장치에 대한 권한 있는 앱(있는 경우)을 지정하는 데 사용합니다. 자세한 내용은 [Microsoft Store 장치 앱 용 장치 동기화 및 업데이트를](http://go.microsoft.com/fwlink/p/?LinkId=306619) 참조 하세요.

## <a name="managing-background-tasks"></a>백그라운드 작업 관리

백그라운드 작업에서는 이벤트 및 로컬 저장소를 사용하여 진행률, 완료 및 취소를 앱에 보고할 수 있습니다. 또한 앱은 백그라운드 작업에서 발생된 예외를 catch할 수 있으며 앱 업데이트 중 백그라운드 작업 등록을 관리할 수 있습니다. 자세한 내용은 다음을 참조하세요.

[취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)  
[백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)

앱 실행 중에 백그라운드 작업 등록을 확인 합니다. 앱의 그룹화 되지 않은 백그라운드 작업이 BackgroundTaskBuilder.AllTasks에 있는지 확인 합니다. 존재 하지 않은 것을 다시 등록 합니다. 더 이상 필요 없는 모든 작업을 등록 취소 합니다. 이렇게 하면 모든 백그라운드 작업 등록은 최신 앱이 실행 될 때마다 합니다.

## <a name="related-topics"></a>관련 항목

**Windows 10의 멀티태스킹에 대한 개념적 지침**

* [실행, 다시 시작 및 멀티태스킹](index.md)

**관련 백그라운드 작업 지침**

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업에서 센서 및 장치에 액세스](access-sensors-and-devices-from-a-background-task.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환](convert-out-of-process-background-task.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [백그라운드 작업 등록 그룹화](group-background-tasks.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드에서 미디어 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [UWP 앱 업데이트 시 백그라운드 작업을 실행합니다.](run-a-background-task-during-updatetask.md)
* [백그라운드에서 무기한 실행](run-in-the-background-indefinetly.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [앱에서 백그라운드 작업 트리거](trigger-background-task-from-app.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
