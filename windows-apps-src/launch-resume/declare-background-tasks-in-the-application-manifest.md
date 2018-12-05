---
title: 응용 프로그램 매니페스트에서 백그라운드 작업 선언
description: 앱 매니페스트에서 백그라운드 작업을 확장으로 선언하여 사용할 수 있습니다.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 4527cface4681bf4866249c6398d43e6af782725
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8735563"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>응용 프로그램 매니페스트에서 백그라운드 작업 선언




**중요 API**

-   [**BackgroundTasks 스키마**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

앱 매니페스트에서 백그라운드 작업을 확장으로 선언하여 사용할 수 있습니다.

> [!Important]
>  이 문서는 Out-of-process 백그라운드 작업과 관련이 있습니다. In-process 백그라운드 작업은 매니페스트에 선언되지 않습니다.

Out-of-process 백그라운드 작업은 앱 매니페스트에서 선언해야 합니다. 그렇지 않으면 앱에서 등록할 수 없습니다(예외가 발생함). 또한 인증을 통과하려면 응용 프로그램 매니페스트에서 Out-of-process 백그라운드 작업을 선언해야 합니다.

이 항목에서는 백그라운드 작업 클래스를 하나 이상 만들었으며 앱이 하나 이상의 트리거에 응답하여 실행할 각 백그라운드 작업을 등록한다고 가정합니다.

## <a name="add-extensions-manually"></a>수동으로 확장 추가


응용 프로그램 매니페스트(Package.appxmanifest)를 열고 Application 요소로 이동합니다. Extensions 요소가 없는 경우 새로 만듭니다.

다음 조각은 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)에서 가져온 것입니다.

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

첫 번째 백그라운드 작업을 선언합니다.

이 코드를 Extensions 요소에 복사합니다. 다음 단계에 따라 특성을 추가합니다.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  백그라운드 작업(**namespace.classname**)을 등록할 때 코드에 사용된 것과 동일한 문자열을 진입점으로 사용하도록 EntryPoint 특성을 변경합니다.

    이 예에서 진입점은 ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName입니다.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  Task Type 특성 목록을 변경하여 이 백그라운드 작업과 함께 사용된 작업 등록 유형을 나타냅니다. 백그라운드 작업을 여러 트리거 유형과 함께 등록할 경우 유형별로 다른 Task 요소 및 Type 특성을 추가합니다.

    **참고**각 트리거 유형을 나열 사용 중인 그렇지 ( [**등록**](https://msdn.microsoft.com/library/windows/apps/br224772) 메서드가 실패 하 고 예외를 throw) 선언 되지 않은 트리거 유형과 함께 백그라운드 작업이 등록 되지 것입니다.

    이 조각 예에서는 시스템 이벤트 트리거 및 푸시 알림 사용을 나타냅니다.

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>여러 백그라운드 작업 확장 추가

앱에서 등록한 각 추가 백그라운드 작업 클래스에 대해 2단계를 반복합니다.

다음 예제는 [백그라운드 작업 샘플]( http://go.microsoft.com/fwlink/p/?linkid=227509)의 전체 Application 요소입니다. 총 3개의 트리거 유형이 있는 백그라운드 작업 클래스 2개를 사용하는 방법을 보여 줍니다. 응용 프로그램 매니페스트에서 백그라운드 작업을 선언하려면 이 예의 Extensions 섹션을 복사하고 필요에 따라 수정합니다.

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

## <a name="declare-where-your-background-task-will-run"></a>어디에서 백그라운드 작업이 실행될지 선언

어디에서 백그라운드 작업이 실행될지 지정할 수 있습니다.

* 백그라운드 작업은 기본적으로 BackgroundTaskHost.exe 프로세스에서 실행됩니다.
* 포그라운드 응용 프로그램과 동일한 프로세스에서
* 여러 백그라운드 작업을 동일한 호스팅 프로세스에 넣거나 다른 프로세스로 나누려면 `ResourceGroup`을 사용합니다.
* 새 트리거가 실행될 때마다 리소스 제한을 가져오는 새 프로세스로 백그라운드 프로세스를 실행하려면 `SupportsMultipleInstances`를 사용합니다.

### <a name="run-in-the-same-process-as-your-foreground-application"></a>포그라운드 응용 프로그램과 동일한 프로세스에서 실행

다음은 포그라운드 응용 프로그램과 같은 프로세스에서 실행되는 백그라운드 작업을 선언하는 XML 예입니다.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

**EntryPoint**를 지정하면 응용 프로그램에서 트리거가 실행될 때 지정된 메서드에 대한 콜백을 받습니다. **EntryPoint**를 지정하지 않으면 응용 프로그램에서 [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)를 통해 콜백을 받습니다.  자세한 내용은 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)을 참조하세요.

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>어디에서 ResourceGroup 특성을 사용하여 백그라운드 작업을 실행할지 지정합니다.

다음은 동일한 앱에서 백그라운드 작업의 다른 인스턴스와 별개인 BackgroundTaskHost.exe 프로세스에서 실행되는 백그라운드 작업을 선언하는 예제 XML입니다. 함께 실행되는 백그라운드 작업을 식별하는 `ResourceGroup` 특성에 유의하세요.

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

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>SupportsMultipleInstances 특성을 사용하여 트리거가 실행될 때마다 새 프로세스에서 실행

이 예에서는 새 트리거가 실행될 때마다 자체 리소스 제한(메모리 및 CPU)을 가져오는 새 프로세스에서 실행되는 백그라운드 작업을 선언합니다. `SupportsMultipleInstances`를 사용하면 이 동작이 활성화됩니다. 이 특성을 사용 하려면 SDK 버전 '10.0.15063' (Windows 10 크리에이터 스 업데이트)를 대상으로 해야 이상.

```xml
<Package
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances=“True”>
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> `SupportsMultipleInstances`와 함께 `ResourceGroup` 또는 `ServerName`을 지정할 수 없습니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 작업 디버그](debug-a-background-task.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
