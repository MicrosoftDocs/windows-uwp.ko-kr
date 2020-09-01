---
title: 백그라운드 작업 지침
description: 응용 프로그램에서 in-process 백그라운드 작업을 개발 하 고 실행 하는 방법에 대 한 자세한 지침을 확인 하세요.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: fb585b46399d7b24eaafa531b2aae34f397dbeb2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155847"
---
# <a name="guidelines-for-background-tasks"></a>백그라운드 작업 지침


앱이 백그라운드 작업을 실행 하기 위한 요구 사항을 충족 하는지 확인 합니다.

## <a name="background-task-guidance"></a>백그라운드 작업 지침

백그라운드 작업을 개발할 때와 앱을 게시 하기 전에 다음 지침을 고려 하세요.

백그라운드에서 미디어를 재생 하는 데 백그라운드 작업을 사용 하는 경우 Windows 10 버전 1607의 향상 된 기능에 대 한 자세한 내용을 보려면 [백그라운드에서 미디어 재생](../audio-video-camera/background-audio.md) 을 참조 하세요.

In-process **및 out-of-process 백그라운드 작업:** Windows 10 버전 1607은 포그라운드 앱과 동일한 프로세스에서 백그라운드 코드를 실행할 수 있는 [in-process 백그라운드 작업](create-and-register-an-inproc-background-task.md) 을 소개 했습니다. In-process 및 out-of-process 백그라운드 작업을 사용할지 여부를 결정할 때는 다음 요인을 고려 하십시오.

|고려 사항 | 영향 |
|--------------|--------|
|복원력   | 백그라운드 프로세스가 다른 프로세스에서 실행 되는 경우 백그라운드 프로세스의 작동이 중단 되 면 포그라운드 응용 프로그램이 작동 하지 않습니다. 또한 이전 실행 시간 제한을 실행 하는 경우 앱 내 에서도 백그라운드 작업을 종료할 수 있습니다. 포그라운드 앱과 분리 된 작업으로 백그라운드 작업을 분리 하는 것은 포그라운드 및 백그라운드 프로세스가 서로 통신 하는 데 필요 하지 않은 경우에 더 적합할 수 있습니다 (in-process 백그라운드 작업의 주요 이점 중 하나는 프로세스 간 통신에 대 한 필요성을 제거 하기 때문). |
|단순함    | In-process 백그라운드 작업은 프로세스 간 통신이 필요 하지 않으며 작성 하기 복잡 합니다.  |
|사용 가능한 트리거 | In-process 백그라운드 작업은 [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) 및 **i startuptask**트리거를 지원 하지 않습니다. |
|VoIP | In-process 백그라운드 작업은 응용 프로그램 내에서 VoIP 백그라운드 작업을 활성화 하는 것을 지원 하지 않습니다. |  

**트리거 인스턴스 수에 대 한 제한:** 앱이 등록할 수 있는 일부 트리거의 인스턴스 수에는 제한이 있습니다. 앱은 앱 인스턴스당   [Applicationtrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) 및 [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) 를 한 번만 등록할 수 있습니다. 앱이이 제한을 초과 하면 등록 시 예외가 throw 됩니다.

**CPU 할당량:** 백그라운드 작업은 트리거 유형에 따라 가져오는 벽 시계 사용 시간의 양에 따라 제한 됩니다. 대부분의 트리거는 벽 시계 사용을 30 초로 제한 하지만, 일부는 집약적 작업을 완료 하기 위해 최대 10 분까지 실행할 수 있습니다. 백그라운드 작업은 배터리 수명을 절약 하 고 포그라운드 앱에 더 나은 사용자 환경을 제공 하기 위해 간단 해야 합니다. 백그라운드 작업에 적용 되는 리소스 제약 조건에 대 한 [백그라운드 작업으로 앱 지원](support-your-app-with-background-tasks.md) 을 참조 하세요.

**백그라운드 작업 관리:** 앱은 등록 된 백그라운드 작업 목록을 가져오고, 진행률 및 완료 처리기를 등록 하 고, 해당 이벤트를 적절 하 게 처리 해야 합니다. 백그라운드 작업 클래스는 진행률, 취소 및 완료를 보고 해야 합니다. 자세한 내용은 [취소 된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)및 [백그라운드 작업 진행률 및 완료 모니터링](monitor-background-task-progress-and-completion.md)을 참조 하세요.

** [BackgroundTaskDeferral](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)사용:** 백그라운드 작업 클래스가 비동기 코드를 실행 하는 경우 지연과를 사용 해야 합니다. 그렇지 않으면 [실행](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드가 반환 될 때 백그라운드 작업이 중간에 종료 될 수 있습니다 (또는 in-process 백그라운드 작업의 경우 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) 메서드). 자세한 내용은 out-of-process [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조 하세요.

또는 한 지연을 요청 하 고 비동기 **/** 대기를 사용 하 여 비동기 메서드 호출을 완료 합니다. **Wait** 메서드가를 호출한 후 지연을 닫습니다.

**응용 프로그램 매니페스트를 업데이트 합니다.**  Out-of-process로 실행 되는 백그라운드 작업의 경우 응용 프로그램 매니페스트에서 각 백그라운드 작업을 사용 되는 트리거의 형식과 함께 선언 합니다. 그렇지 않으면 앱은 런타임에 백그라운드 작업을 등록할 수 없습니다.

백그라운드 작업이 여러 개인 경우 동일한 호스트 프로세스에서 실행 하거나 다른 호스트 프로세스로 분리 해야 하는지 여부를 고려 합니다. 한 백그라운드 작업의 오류로 인해 다른 백그라운드 작업이 중단 될 수 있는 경우 별도의 호스트 프로세스에 배치 합니다.  매니페스트 디자이너의 **리소스 그룹** 항목을 사용 하 여 백그라운드 작업을 여러 호스트 프로세스로 그룹화 할 수 있습니다. 

**리소스 그룹**을 설정 하려면 appxmanifest.xml 디자이너를 열고 **선언**을 선택한 다음 **App Service** 선언을 추가 합니다.

![리소스 그룹 설정](images/resourcegroup.png)

리소스 그룹 설정에 대 한 자세한 내용은 [응용 프로그램 스키마 참조](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 를 참조 하세요.

포그라운드 앱과 동일한 프로세스에서 실행 되는 백그라운드 작업은 응용 프로그램 매니페스트에서 자신을 선언 하지 않아도 됩니다. 매니페스트에서 out-of-process를 실행 하는 백그라운드 작업을 선언 하는 방법에 대 한 자세한 내용은 [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)을 참조 하세요.

**앱 업데이트 준비:** 앱이 업데이트 되는 경우 **ServicingComplete** 백그라운드 작업 ( [systemtriggertype](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)참조)을 만들고 등록 하 여 이전 버전의 앱에 대 한 백그라운드 작업의 등록을 취소 하 고 새 버전에 대 한 백그라운드 작업을 등록 합니다. 이는 포그라운드로 실행 되는 컨텍스트 외부에서 필요할 수 있는 앱 업데이트를 수행 하는 데 적절 한 시간 이기도 합니다.

**백그라운드 작업 실행 요청:**

> **중요**    Windows 10부터 백그라운드 작업을 실행 하기 위한 필수 조건으로 앱이 더 이상 잠금 화면에 있을 필요가 없습니다.

UWP (유니버설 Windows 플랫폼) 앱은 잠금 화면에 고정 되지 않고 지원 되는 모든 작업 유형을 실행할 수 있습니다. 그러나 앱은 [**Getaccessstate**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) 를 호출 하 고 앱이 백그라운드에서 실행 되지 못하도록 거부 되지 않았는지 확인 해야 합니다. [**Getaccessstatus**]가 거부 된 [**BackgroundAccessStatus**](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) 열거 중 하나를 반환 하지 않는지 확인 하십시오. 예를 들어 https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundAccessStatus) 사용자가 장치 설정에서 앱에 대 한 백그라운드 작업 권한을 명시적으로 거부 한 경우이 메서드는를 반환 합니다.

앱이 백그라운드에서 실행 되지 못하도록 거부 되 면 앱은 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) 를 호출 하 고 백그라운드 작업을 등록 하기 전에 응답이 거부 되지 않았는지 확인 해야 합니다.

백그라운드 작업 및 배터리 보호기와 관련 한 사용자 선택에 대 한 자세한 내용은 [백그라운드 작업 최적화](../debug-test-perf/optimize-background-activity.md)를 참조 하세요. 
## <a name="background-task-checklist"></a>백그라운드 작업 검사 목록

*In-process 및 out-of-process 백그라운드 작업 모두에 적용 됩니다.*

-   백그라운드 작업을 올바른 트리거와 연결 합니다.
-   백그라운드 작업이 성공적으로 실행 되는지 확인 하는 데 도움이 되는 조건을 추가 합니다.
-   백그라운드 작업 진행률, 완료 및 취소를 처리 합니다.
-   앱을 시작 하는 동안 백그라운드 작업을 다시 등록 합니다. 이렇게 하면 앱이 처음 시작 될 때 등록 됩니다. 또한 사용자가 앱의 백그라운드 실행 기능을 사용 하지 않도록 설정 했는지 여부를 검색 하는 방법을 제공 합니다 (이벤트 등록에 실패 함).
-   백그라운드 작업 등록 오류를 확인 합니다. 해당 하는 경우 다른 매개 변수 값으로 백그라운드 작업을 다시 등록 하려고 시도 합니다.
-   데스크톱을 제외한 모든 장치 패밀리의 경우 장치의 메모리가 부족 해지면 백그라운드 작업이 종료 될 수 있습니다. 메모리 부족 예외가 표시 되지 않거나 앱에서 처리 하지 않는 경우 백그라운드 작업은 경고 없이 종료 되 고 OnCanceled 이벤트를 발생 시 키 지 않습니다. 이를 통해 포그라운드에서 앱의 사용자 환경을 보장할 수 있습니다. 백그라운드 작업은이 시나리오를 처리 하도록 디자인 되어야 합니다.

*Out-of-process 백그라운드 작업에만 적용 됩니다.*

-   Windows 런타임 구성 요소에서 백그라운드 작업을 만듭니다.
-   백그라운드 작업에서 알림을, 타일 및 배지 업데이트 이외의 UI는 표시 하지 않습니다.
-   [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드에서 각 비동기 메서드 호출에 대해 지연과를 요청 하 고 메서드가 완료 되 면 닫습니다. 또는 **비동기/** 대기에서 하나의 지연을 사용 합니다.
-   영구 저장소를 사용 하 여 백그라운드 작업과 앱 간에 데이터를 공유 합니다.
-   응용 프로그램 매니페스트에서 각 백그라운드 작업을 사용 하는 트리거의 형식과 함께 선언 합니다. 진입점 및 트리거 형식이 올바른지 확인 하십시오.
-   앱과 동일한 컨텍스트에서 실행 해야 하는 트리거 (예: [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger))를 사용 하지 않는 경우 매니페스트에 실행 가능 요소를 지정 하지 마십시오.

*In-process 백그라운드 작업에만 적용 됩니다.*

- 작업을 취소 하는 경우 `BackgroundActivated` 취소가 발생 하거나 전체 프로세스가 종료 되기 전에 이벤트 처리기가 종료 되는지 확인 합니다.
-   수명이 짧은 백그라운드 작업을 작성 합니다. 백그라운드 작업은 30 초에서 벽 시계 사용으로 제한 됩니다.
-   백그라운드 작업의 사용자 상호 작용에 의존 하지 마십시오.

## <a name="related-topics"></a>관련 항목

* [In-process 백그라운드 작업을 만들고 등록](create-and-register-an-inproc-background-task.md)합니다.
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [애플리케이션 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [백그라운드에서 미디어 재생](../audio-video-camera/background-audio.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법 (디버깅 시)](/previous-versions/hh974425(v=vs.110))

 

 