---
author: TylerMSFT
title: "타이머에 따라 백그라운드 작업 실행"
description: "일회성 백그라운드 작업을 예약하거나 정기적 백그라운드 작업을 실행하는 방법을 알아봅니다."
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 3fc1e3efa742ff8ab24f78856872fe322703f152

---

# 타이머에 따라 백그라운드 작업 실행


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

일회성 백그라운드 작업을 예약하거나 정기적 백그라운드 작업을 실행하는 방법을 알아봅니다.

-   이 예제에서는 앱을 지원하기 위해 정기적으로 또는 특정 시간에 실행해야 하는 백그라운드 작업이 있다고 가정합니다. 백그라운드 작업은 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출한 경우에만 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)를 사용하여 실행됩니다.
-   이 항목에서는 백그라운드 작업 진입점으로 사용되는 Run 메서드를 비롯하여 백그라운드 작업 클래스를 이미 만들었다고 가정합니다. 백그라운드 작업을 빠르게 작성하려면 [백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)을 참조하세요. 조건 및 트리거에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

## 시간 트리거 만들기


-   새 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)를 만듭니다. 두 번째 매개 변수인 *OneShot*은 백그라운드 작업이 한 번 실행되는지, 아니면 정기적으로 계속 실행되는지를 지정합니다. *OneShot*이 true로 설정된 경우 첫 번째 매개 변수(*FreshnessTime*)는 백그라운드 작업을 예약하기 전에 대기할 시간(분)을 지정합니다. *OneShot*이 false로 설정된 경우 *FreshnessTime*은 백그라운드 작업의 실행 빈도를 지정합니다.

    데스크톱 또는 모바일 디바이스 패밀리를 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 기본 제공 타이머는 15분 간격으로 백그라운드 작업을 실행합니다.

    -   *FreshnessTime*이 15분으로 설정되고 *OneShot*이 true이면 작업이 등록된 시간부터 0분에서 15분 사이에 한 번 실행됩니다.

    -   *FreshnessTime*이 15분으로 설정되고 *OneShot*이 false이면 작업이 등록된 시간부터 0분에서 15분 사이에 시작하여 15분마다 실행됩니다.

    **참고** *FreshnessTime*이 15분 이하로 설정된 경우 백그라운드 작업을 등록하려면 예외가 발생합니다.

     

    예를 들어 다음 트리거는 백그라운드 작업을 한 시간에 한 번 실행합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## (옵션) 조건 추가


-   필요한 경우 작업이 실행되는 시간을 제어하는 백그라운드 작업 조건을 만듭니다. 그러면 조건이 충족되는 경우에만 백그라운드 작업이 실행됩니다. 자세한 내용은 [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)을 참조하세요.

    이 예제에서는 트리거되면 사용자가 활성 상태일 때만 작업이 실행되도록 조건을 **UserPresent**로 설정합니다. 가능한 조건 목록은 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)을 참조하세요.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  RequestAccessAsync() 호출


-   [
            **TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 백그라운드 작업을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)를 호출합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## 백그라운드 작업 등록


-   백그라운드 작업 등록 함수를 호출하여 백그라운드 작업을 등록합니다. 백그라운드 작업 등록에 대한 자세한 내용은 [백그라운드 작업 등록](register-a-background-task.md)을 참조하세요.

    다음 코드는 백그라운드 작업을 등록합니다.

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```

    > **참고** 백그라운드 작업 등록 매개 변수는 등록 시 유효성이 검사됩니다. 등록 매개 변수가 하나라도 유효하지 않으면 오류가 반환됩니다. 백그라운드 작업 등록이 실패할 경우 앱이 시나리오를 적절하게 처리하도록 해야 합니다. 대신 앱이 작업 등록을 시도한 후 유효한 등록 개체를 사용하면 충돌할 수 있습니다.


## 설명

> **참고** Windows 10부터 사용자가 백그라운드 작업을 활용하기 위해 더 이상 잠금 화면에 앱을 추가할 필요가 없습니다. 백그라운드 작업 트리거 유형에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

> **참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.


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
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)

* [백그라운드 작업 디버그](debug-a-background-task.md)
* [Windows 스토어 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO4-->


