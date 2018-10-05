---
author: TylerMSFT
title: 백그라운드 작업 지침
description: 앱이 백그라운드 작업 실행을 위한 요구 사항을 충족하는지 확인합니다.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 7adfecbe216dce25d0f80eb3ef1f528196299db4
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4394384"
---
# <a name="guidelines-for-background-tasks"></a>백그라운드 작업 지침


앱이 백그라운드 작업 실행을 위한 요구 사항을 충족하는지 확인합니다.

## <a name="background-task-guidance"></a>백그라운드 작업 지침

백그라운드 작업을 개발할 때와 앱을 게시하기 전에 다음 지침을 고려하세요.

백그라운드 작업을 사용하여 백그라운드에서 미디어를 재생하는 경우 이 작업을 훨씬 용이하게 하는 Windows10 버전 1607의 향상된 기능에 대한 자세한 내용은 [백그라운드에서 미디어 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)을 참조하세요.

**In-process 및 Out-of-process 백그라운드 작업:** Windows10 버전 1607에서는 포그라운드 앱과 동일한 프로세스에서 백그라운드 코드를 실행할 수 있는 [In-process 프로세스 백그라운드 작업](create-and-register-an-inproc-background-task.md)이 도입되었습니다. 백그라운드 작업에 대해 In-process를 사용할지 또는 Out-of-process를 사용할지를 결정할 때 고려할 요소는 다음과 같습니다.

|고려 사항 | 영향 |
|--------------|--------|
|복원력   | 다른 프로세스에서 백그라운드 프로세스를 실행하는 경우 백그라운드 프로세스에서 충돌이 발생해도 포그라운드 응용 프로그램이 중단되지 않습니다. 또한 실행 시간 제한을 초과해서 실행될 경우 앱 내에서도 백그라운드 작업을 종료할 수 있습니다. In-process 프로세스 백그라운드 작업의 주요 이점 중 하나는 프로세스 간에 통신할 필요가 없다는 것이므로 포그라운드 및 백그라운드 프로세스에서 서로 통신할 필요가 없는 경우 백그라운드 작업을 포그라운드 앱과 별개인 작업으로 구분하는 것이 유용할 수 있습니다. |
|단순함    | In-process 프로세스 백그라운드 작업의 경우 프로세스 간에 통신할 필요가 없으며 보다 간단하게 작성할 수 있습니다.  |
|사용 가능한 트리거 | In-process 백그라운드 작업은 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx), **IoTStartupTask** 등의 트리거를 지원하지 않습니다. |
|VoIP | In-process 프로세스 백그라운드 작업은 응용 프로그램 내에서 VoIP 백그라운드 작업 활성화를 지원하지 않습니다. |  

**트리거 인스턴스 수에 대한 제한:** 앱에서 등록할 수 있는 일부 트리거의 인스턴스 수에 대한 제한이 있습니다. 앱은 앱 인스턴스당 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) 및 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)를 한 번씩만 등록할 수 있습니다. 앱이 이 제한을 초과하는 경우 등록이 예외를 throw합니다.

**CPU 할당량:** 백그라운드 작업은 트리거 유형에 따라 가져오는 벽시계로 측정하는 시간으로 제한됩니다. 대부분의 트리거는 벽시계로 측정하는 30초로 제한되지만 일부는 집중적인 작업을 완료하기 위해 최대 10분을 실행할 수 있습니다. 배터리 사용 시간을 절약하고 포그라운드 앱에 대한 효율적인 사용자 환경을 제공하도록 백그라운드 작업은 간결해야 합니다. 백그라운드 작업에 적용되는 리소스 제약 조건은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

**백그라운드 작업 관리:** 앱에서는 등록된 백그라운드 작업 목록을 가져오고, 진행률 및 완료 처리기를 등록하고, 해당 이벤트를 적절하게 처리해야 합니다. 백그라운드 작업 클래스는 진행률, 취소 및 완료를 보고해야 합니다. 자세한 내용은 [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md) 및 [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)을 참조하세요.

**[BackgroundTaskDeferral](https://msdn.microsoft.com/library/windows/apps/hh700499) 사용:** 백그라운드 작업 클래스가 비동기 코드를 실행하는 경우 지연을 사용해야 합니다. 사용하지 않으면 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드(또는 In-process 프로세스 백그라운드 작업의 경우 [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 메서드)가 완료될 때 백그라운드 작업이 중간에 종료될 수 있습니다. 자세한 내용은 [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요.

또는 지연을 요청하고 **async/await**를 사용하여 비동기 메서드 호출을 완료합니다. **await** 메서드가 호출된 후 지연을 닫습니다.

**앱 매니페스트 업데이트:** Out-of-process로 실행되는 백그라운드 작업의 경우 응용 프로그램 매니페스트에서 각 백그라운드 작업을 사용되는 트리거 유형과 함께 선언합니다. 그렇지 않으면 앱에서 런타임에 백그라운드 작업을 등록할 수 없습니다.

백그라운드 작업이 여러 개 있는 경우 동일한 호스트 프로세스에서 실행할지 다른 호스트 프로세스에 분리할지 고려하는 것이 좋습니다. 한 백그라운드 작업의 장애가 다른 백그라운드 작업을 중단시킬 것이 우려되는 경우 별도 호스트 프로세스에 놓습니다.  매니페스트 디자이너에서 **리소스 그룹** 항목을 사용하여 백그라운드 작업을 다른 호스트 프로세스로 그룹화합니다. 

**리소스 그룹**을 설정하려면, Package.appxmanifest 디자이너를 열고, **선언**을 선택하고 **앱 서비스** 선언을 추가합니다.

![리소스 그룹 설정](images/resourcegroup.png)

리소스 그룹 설정에 대한 자세한 내용은 [응용 프로그램 스키마 참조](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)를 참조하세요.

포그라운드 앱과 동일한 프로세스에서 실행되는 백그라운드 작업은 응용 프로그램 매니페스트에서 선언할 필요가 없습니다. Out-of-process를 실행하는 백그라운드 작업을 매니페스트에서 선언하는 방법에 대한 자세한 내용은 [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)을 참조하세요.

**앱 업데이트 준비:** 앱을 업데이트할 경우 **ServicingComplete** 백그라운드 작업([SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839) 참조)을 만들고 등록하여 이전 버전의 앱에 대한 백그라운드 작업을 등록 취소하고 새 버전에 대한 백그라운드 작업을 등록합니다. 이때 포그라운드에서 실행 컨텍스트 외부에서 필요한 앱 업데이트도 수행하는 것이 좋습니다.

**백그라운드 작업 실행 요청:**

> **중요** Windows 10부터 백그라운드 작업을 실행하기 위한 필수 조건으로 앱을 잠금 화면에 배치하지 않아도 됩니다.

UWP(유니버설 Windows 플랫폼) 앱은 잠금 화면에 고정되지 않아도 지원되는 모든 작업 형식을 실행할 수 있습니다. 그러나 모든 형식의 백그라운드 작업을 등록하기 전에 앱이 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다. 사용자가 장치 설정에서 해당 앱에 대해 명시적으로 백그라운드 작업 권한을 거부한 경우에는 이 메서드가 [**BackgroundAccessStatus.DeniedByUser**](https://msdn.microsoft.com/library/windows/apps/hh700439)를 반환합니다. 백그라운드 작업 및 배터리 절약 모드와 관련된 사용자 선택에 대한 자세한 내용은 [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)를 참조하세요. 
## <a name="background-task-checklist"></a>백그라운드 작업 검사 목록

*In-process 및 out of process 백그라운드 작업 모두에 적용*

-   백그라운드 작업을 올바른 트리거에 연결합니다.
-   백그라운드 작업이 성공적으로 실행되도록 조건을 추가합니다.
-   백그라운드 작업 진행률, 완료 및 취소를 처리합니다.
-   앱 실행 중에 백그라운드 작업을 다시 등록합니다. 이렇게 하면 앱이 처음 실행될 때 백그라운드 작업이 등록됩니다. 또한 이벤트 등록에 실패했을 경우 사용자가 앱의 백그라운드 실행 기능을 사용 중지로 설정했는지 검색하는 방법도 제공합니다.
-   백그라운드 작업 등록 오류를 확인합니다. 해당되는 경우 다른 매개 변수 값을 사용하여 백그라운드 작업을 다시 등록합니다.
-   데스크톱을 제외한 모든 디바이스 패밀리의 경우 장치의 메모리가 부족해지면 백그라운드 작업이 종료될 수 있습니다. 메모리 부족 예외가 표시되지 않거나 앱에서 처리하지 않는 경우 백그라운드 작업이 OnCanceled 이벤트를 발생시키지 않고 경고 없이 종료됩니다. 이는 포그라운드에서 앱의 사용자 환경을 확인하는 데 도움이 됩니다. 백그라운드 작업은 이 시나리오를 처리하도록 설계되어야 합니다.

*Out-of-process 백그라운드 작업에만 적용*

-   Windows 런타임 구성 요소에서 백그라운드 작업을 만듭니다.
-   백그라운드 작업에서 알림, 타일 및 배지 업데이트 이외의 UI를 표시하지 않습니다.
-   [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드에서 각 비동기 메서드 호출에 대한 지연을 요청하고 메서드가 완료되면 지연을 닫습니다. 또는 지연을 **async/await**와 함께 사용합니다.
-   영구적 저장소를 사용하여 백그라운드 작업과 앱 간에 데이터를 공유합니다.
-   응용 프로그램 매니페스트에서 각 백그라운드 작업을 사용되는 트리거 형식과 함께 선언합니다. 진입점과 트리거 유형이 올바른지 확인합니다.
-   앱과 동일한 컨텍스트에서 실행되어야 하는 트리거(예: [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032))를 사용하지 않는 한 매니페스트에서 실행 파일 요소를 지정하지 않습니다.

*In-process 백그라운드 작업에만 적용*

- 작업을 취소하는 경우 취소가 발생하기 전에 `BackgroundActivated` 이벤트 처리기가 종료되는지 확인합니다. 종료되지 않을 경우 전체 프로세스가 종료됩니다.
-   지속 시간이 짧은 백그라운드 작업을 씁니다. 백그라운드 작업은 벽시계로 측정하는 30초로 제한됩니다.
-   백그라운드 작업에서 사용자 조작을 요구하지 않습니다.

## <a name="related-topics"></a>관련 항목

* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md).
* [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [백그라운드에서 미디어 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [UWP 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 
