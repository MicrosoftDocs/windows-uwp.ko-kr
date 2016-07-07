---
author: TylerMSFT
title: "백그라운드 작업 지침"
description: "앱이 백그라운드 작업 실행을 위한 요구 사항을 충족하는지 확인합니다."
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 3fb6884a968afb87e8de303bbd17feba4993c597

---

# 백그라운드 작업 지침


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


앱이 백그라운드 작업 실행을 위한 요구 사항을 충족하는지 확인합니다.

## 백그라운드 작업 지침


백그라운드 작업을 개발할 때와 앱을 게시하기 전에 다음 지침을 고려하세요.

**CPU 할당량:**백그라운드 작업은 트리거 유형에 따라 가져오는 벽시계로 측정하는 시간으로 제한됩니다. 대부분의 트리거는 벽시계로 측정하는 30초로 제한되지만 일부는 집중적인 작업을 완료하기 위해 최대 10분을 실행할 수 있습니다. 배터리 사용 시간을 절약하고 포그라운드 앱에 대한 효율적인 사용자 환경을 제공하도록 백그라운드 작업은 간결해야 합니다. 백그라운드 작업에 적용되는 리소스 제약 조건은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

**백그라운드 작업 관리:**앱에서는 등록된 백그라운드 작업 목록을 가져오고, 진행률 및 완료 처리기를 등록하고, 해당 이벤트를 적절하게 처리해야 합니다. 백그라운드 작업 클래스는 진행률, 취소 및 완료를 보고해야 합니다. 자세한 내용은 [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md) 및 [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)을 참조하세요.

**[**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499) 사용:**백그라운드 작업 클래스가 비동기 코드를 실행하는 경우 지연을 사용해야 합니다. 그렇지 않으면 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드가 완료된 후 백그라운드 작업이 영구히 종료될 수 있습니다. 자세한 내용은 [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요.

또는 지연을 요청하고 **async/await**를 사용하여 비동기 메서드 호출을 완료합니다. **await** 메서드가 호출된 후 지연을 닫습니다.

**앱 매니페스트 업데이트:**응용 프로그램 매니페스트에서 각 백그라운드 작업을 사용되는 트리거 형식과 함께 선언합니다. 그렇지 않으면 앱에서 런타임에 백그라운드 작업을 등록할 수 없습니다. 자세한 내용은 [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)을 참조하세요.

**앱 업데이트 준비:**앱을 업데이트할 경우 포그라운드 실행 컨텍스트 외부에서 필요한 앱 업데이트를 실행할 수 있도록 **ServicingComplete** 백그라운드 작업([**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) 참조)을 만들고 등록합니다.

**백그라운드 작업 실행 요청:  **

> **중요** Windows 10부터 더 이상 백그라운드 작업을 실행하기 위해 앱을 잠금 화면에 배치하지 않아도 됩니다.

UWP(유니버설 Windows 플랫폼) 앱은 잠금 화면에 고정되지 않아도 지원되는 모든 작업 형식을 실행할 수 있습니다. 그러나 모든 형식의 백그라운드 작업을 등록하기 전에 앱이 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다. 사용자가 장치 설정에서 해당 앱에 대해 명시적으로 백그라운드 작업 권한을 거부한 경우에는 이 메서드가 [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439)를 반환합니다.
## 백그라운드 작업 검사 목록


다음 검사 목록은 모든 백그라운드 작업에 적용됩니다.

-   Windows 런타임 구성 요소에서 백그라운드 작업을 만듭니다.
-   백그라운드 작업을 올바른 트리거에 연결합니다.
-   백그라운드 작업이 성공적으로 실행되도록 조건을 추가합니다.
-   백그라운드 작업 진행률, 완료 및 취소를 처리합니다.
-   백그라운드 작업에서 알림, 타일 및 배지 업데이트 이외의 UI를 표시하지 않습니다.
-   [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드에서 각 비동기 메서드 호출에 대한 지연을 요청하고 메서드가 완료되면 지연을 닫습니다. 또는 지연을 **async/await**와 함께 사용합니다.
-   영구적 저장소를 사용하여 백그라운드 작업과 앱 간에 데이터를 공유합니다.
-   응용 프로그램 매니페스트에서 각 백그라운드 작업을 사용되는 트리거 형식과 함께 선언합니다. 진입점과 트리거 형식이 올바른지 확인합니다.
-   지속 시간이 짧은 백그라운드 작업을 씁니다. 백그라운드 작업은 벽시계로 측정하는 30초로 제한됩니다.
-   백그라운드 작업에서 사용자 조작을 요구하지 않습니다.
-   앱 실행 중에 백그라운드 작업을 다시 등록합니다. 이렇게 하면 앱이 처음 실행될 때 백그라운드 작업이 등록됩니다. 또한 이벤트 등록에 실패했을 경우 사용자가 앱의 백그라운드 실행 기능을 사용 중지로 설정했는지 검색하는 방법도 제공합니다.
-   앱과 동일한 컨텍스트에서 실행되어야 하는 트리거(예: [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032))를 사용하지 않는 한 매니페스트에서 실행 파일 요소를 지정하지 않습니다.
-   백그라운드 작업 등록 오류를 확인합니다. 해당되는 경우 다른 매개 변수 값을 사용하여 백그라운드 작업을 다시 등록합니다.
-   데스크톱을 제외한 모든 디바이스 패밀리의 경우 장치의 메모리가 부족해지면 백그라운드 작업이 종료될 수 있습니다. 메모리 부족 예외가 표시되지 않거나 앱에서 처리하지 않는 경우 백그라운드 작업이 OnCanceled 이벤트를 발생시키지 않고 경고 없이 종료됩니다. 이는 포그라운드에서 앱의 사용자 환경을 확인하는 데 도움이 됩니다. 백그라운드 작업은 이 시나리오를 처리하도록 설계되어야 합니다.

## Windows: 잠금 화면 지원 앱에 대한 백그라운드 작업 검사 목록


잠금 화면에 배치할 수 있는 앱에 대한 백그라운드 작업을 개발하는 경우 다음 지침을 따릅니다. [잠금 화면 타일에 대한 지침 및 검사 목록](https://msdn.microsoft.com/library/windows/apps/hh465403)의 지침을 따르세요.

-   앱을 잠금 화면 지원으로 개발하기 전에 앱을 잠금 화면에 배치해야 하는지를 확인합니다. 자세한 내용은 [잠금 화면 개요](https://msdn.microsoft.com/library/windows/apps/hh779720)를 참조하세요.

-   잠금 화면에 배치하지 않아도 앱이 작동하는지 확인합니다.

-   [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 또는 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)를 사용하여 등록된 백그라운드 작업을 포함하며 앱 매니페스트에서 백그라운드 작업을 선언합니다. 진입점과 트리거 형식이 올바른지 확인합니다. 이는 인증하는 데 필요하며 사용자가 앱을 잠금 화면에 배치할 수 있도록 해줍니다.

**참고**  
이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

## 관련 항목

* [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)
* [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [Windows 스토어 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


