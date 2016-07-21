---
author: TylerMSFT
title: "응용 프로그램 매니페스트에서 백그라운드 작업 선언"
description: "앱 매니페스트에서 백그라운드 작업을 확장으로 선언하여 사용할 수 있습니다."
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: d7dbdab0e8d404e6607585045d49bb3dd1407de6

---

# 응용 프로그램 매니페스트에서 백그라운드 작업 선언


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**BackgroundTasks 스키마**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

앱 매니페스트에서 백그라운드 작업을 확장으로 선언하여 사용할 수 있습니다.

백그라운드 작업은 앱 매니페스트에서 선언해야 합니다. 그렇지 않으면 앱에서 등록할 수 없습니다(예외가 발생함). 또한 인증을 통과하려면 응용 프로그램 매니페스트에서 백그라운드 작업을 선언해야 합니다.

이 항목에서는 백그라운드 작업 클래스를 하나 이상 만들었으며 앱이 하나 이상의 트리거에 응답하여 실행할 각 백그라운드 작업을 등록한다고 가정합니다.

## 수동으로 확장 추가


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

## 백그라운드 작업 확장 추가


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

    **참고** 사용 중인 각 트리거 유형을 나열해야 합니다. 그러지 않으면 선언되지 않은 트리거 유형과 함께 백그라운드 작업이 등록되지 않습니다([**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 메서드가 실패하고 예외가 발생함).

    이 조각 예에서는 시스템 이벤트 트리거 및 푸시 알림 사용을 나타냅니다.

    ```xml
                <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
                  <BackgroundTasks>
                    <Task Type="systemEvent" />
                    <Task Type="pushNotification" />
                  </BackgroundTasks>
                </Extension>
    ```

    > **참고** 일반적으로 앱은 “BackgroundTaskHost.exe”라는 특수 프로세스에서 실행됩니다. Extension 요소에 Executable 요소를 추가하여 백그라운드 작업이 앱의 컨텍스트에서 실행되도록 할 수 있습니다. 백그라운드 작업에는 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 같이 필요한 Executable 요소만 사용하세요.    

## 백그라운드 작업 실행 추가


앱에서 등록한 각 추가 백그라운드 작업 클래스에 대해 2단계를 반복합니다.

다음 예제는 [백그라운드 작업 샘플]( http://go.microsoft.com/fwlink/p/?linkid=227509)의 전체 Application 요소입니다. 총 3개의 트리거 유형이 있는 백그라운드 작업 클래스 2개를 사용하는 방법을 보여 줍니다. 응용 프로그램 매니페스트에서 백그라운드 작업을 선언하려면 이 예제의 Extensions 섹션을 복사하고 필요에 따라 수정합니다.

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

## 관련 항목

* [백그라운드 작업 디버그](debug-a-background-task.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 지침](guidelines-for-background-tasks.md)



<!--HONumber=Jun16_HO5-->


