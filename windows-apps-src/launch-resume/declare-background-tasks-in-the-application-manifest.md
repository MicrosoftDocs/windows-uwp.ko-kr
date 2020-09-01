---
title: 애플리케이션 매니페스트에서 백그라운드 작업 선언
description: 앱 매니페스트에서 백그라운드 작업을 확장으로 선언하여 사용할 수 있습니다.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 5b66cffa25dce28be22a1347b10e121e75936c25
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155957"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>애플리케이션 매니페스트에서 백그라운드 작업 선언




**중요 API**

-   [**BackgroundTasks 스키마**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)

앱 매니페스트에서 백그라운드 작업을 확장으로 선언하여 사용할 수 있습니다.

> [!Important]
>  이 문서는 out-of-process 백그라운드 작업에만 적용 됩니다. In-process 백그라운드 작업은 매니페스트에서 선언 되지 않습니다.

Out-of-process 백그라운드 작업은 응용 프로그램 매니페스트에서 선언 해야 합니다. 그렇지 않으면 앱에서 해당 작업을 등록할 수 없습니다 (예외가 throw 됨). 또한 응용 프로그램 매니페스트에서 out-of-process 백그라운드 작업을 선언 하 여 인증을 통과 해야 합니다.

이 항목에서는 하나 이상의 백그라운드 작업 클래스를 만들었으며 앱이 하나 이상의 트리거에 대 한 응답으로 실행 되도록 각 백그라운드 작업을 등록 한다고 가정 합니다.

## <a name="add-extensions-manually"></a>수동으로 확장 추가


응용 프로그램 매니페스트 (appxmanifest.xml)를 열고 응용 프로그램 요소로 이동 합니다. 확장 요소 (없는 경우)를 만듭니다.

다음 코드 조각은 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)에서 가져온 것입니다.

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## <a name="add-a-background-task-extension"></a>백그라운드 작업 확장 추가  

첫 번째 백그라운드 작업을 선언 합니다.

다음 단계에서 특성을 추가 하는 확장 요소에이 코드를 복사 합니다.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  백그라운드 작업 (**namespace. classname**)을 등록할 때 코드에서 진입점으로 사용 하는 것과 동일한 문자열을 사용 하도록 EntryPoint 특성을 변경 합니다.

    이 예제에서 진입점은 ExampleBackgroundTaskNameSpace입니다. ExampleBackgroundTaskClassName:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  작업 유형 특성 목록을 변경 하 여이 백그라운드 작업에 사용 되는 작업 등록 유형을 표시 합니다. 여러 트리거 형식으로 백그라운드 작업을 등록 한 경우에는 각 작업 요소에 대해 다른 작업 요소 및 형식 특성을 추가 합니다.

    **참고**    사용 중인 각 트리거 형식을 나열 해야 합니다. 그렇지 않으면 백그라운드 작업이 선언 되지 않은 트리거 형식에 등록 되지 않습니다. [**register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) 메서드가 실패 하 고 예외가 throw 됩니다.

    이 코드 조각 예제는 시스템 이벤트 트리거와 푸시 알림을 사용 하는 것을 나타냅니다.

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>여러 백그라운드 작업 확장 추가

앱에서 등록 된 각 추가 백그라운드 작업 클래스에 대해 2 단계를 반복 합니다.

다음 예제는 [백그라운드 작업 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)의 전체 응용 프로그램 요소입니다. 이는 총 3 개의 트리거 형식으로 2 개의 백그라운드 작업 클래스를 사용 하는 방법을 보여 줍니다. 이 예제의 확장 섹션을 복사 하 고 필요에 따라 수정 하 여 응용 프로그램 매니페스트에서 백그라운드 작업을 선언 합니다.

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## <a name="declare-where-your-background-task-will-run"></a>백그라운드 작업이 실행 될 위치 선언

백그라운드 작업이 실행 되는 위치를 지정할 수 있습니다.

* 기본적으로 BackgroundTaskHost.exe 프로세스에서 실행 됩니다.
* 포그라운드 응용 프로그램과 동일한 프로세스에서.
* `ResourceGroup`를 사용 하 여 여러 백그라운드 작업을 동일한 호스팅 프로세스에 추가 하거나 다른 프로세스로 구분 합니다.
* `SupportsMultipleInstances`를 사용 하 여 새 트리거가 발생할 때마다 자체 리소스 제한 (메모리, cpu)을 가져오는 새 프로세스에서 백그라운드 프로세스를 실행 합니다.

### <a name="run-in-the-same-process-as-your-foreground-application"></a>포그라운드 응용 프로그램과 동일한 프로세스에서 실행

다음은 포그라운드 응용 프로그램과 동일한 프로세스에서 실행 되는 백그라운드 작업을 선언 하는 예제 XML입니다.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

**EntryPoint**를 지정 하면 트리거가 발생 했을 때 응용 프로그램에서 지정 된 메서드에 대 한 콜백을 받습니다. **EntryPoint**를 지정 하지 않으면 응용 프로그램은 [OnBackgroundActivated ()](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)를 통해 콜백을 수신 합니다.  자세한 내용은 [in-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md) 을 참조 하세요.

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>ResourceGroup 특성으로 백그라운드 태스크가 실행 되는 위치를 지정 합니다.

다음은 BackgroundTaskHost.exe 프로세스에서 실행 되지만 동일한 앱의 다른 백그라운드 작업 인스턴스와는 다른 작업을 실행 하는 백그라운드 작업을 선언 하는 XML 예제입니다. 특성은 `ResourceGroup` 함께 실행할 백그라운드 작업을 식별 합니다.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>SupportsMultipleInstances 특성을 사용 하 여 트리거가 발생 될 때마다 새 프로세스에서 실행

이 예에서는 새 트리거가 발생할 때마다 자체 리소스 제한 (메모리 및 CPU)을 가져오는 새 프로세스에서 실행 되는 백그라운드 작업을 선언 합니다. 를 사용 하면 `SupportsMultipleInstances` 이 동작을 사용할 수 있습니다. 이 특성을 사용 하려면 SDK 버전 ' 10.0.15063 ' (Windows 10 크리에이터 업데이트) 이상을 대상으로 해야 합니다.

```xml
<Package
    xmlns:uap4="https://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances="True">
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> 과 함께 또는를 지정할 수 없습니다 `ResourceGroup` `ServerName` `SupportsMultipleInstances` .

## <a name="related-topics"></a>관련 항목

* [백그라운드 작업 디버그](debug-a-background-task.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)