---
author: TylerMSFT
title: "Out-of-process 백그라운드 작업 만들기 및 등록"
description: "Out-of-process 백그라운드 작업 클래스를 만든 다음 앱이 포그라운드에 없는 경우 실행하도록 등록합니다."
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 1974c90b89a34f3252face47b9f18786b638adf8
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/26/2017
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Out-of-process 백그라운드 작업 만들기 및 등록

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

백그라운드 작업 클래스를 만든 다음 앱이 포그라운드에 없는 경우 실행하도록 등록합니다. 이 항목에서는 앱 프로세스가 아닌 별도 프로세스에서 실행되는 백그라운드 작업을 만들고 등록하는 방법을 보여 줍니다. 포그라운드 응용 프로그램에서 직접 백그라운드 작업을 수행하려면 [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)을 참조하세요.

> [!Note]
> 백그라운드 작업을 사용하여 백그라운드에서 미디어를 재생하는 경우 이 작업을 훨씬 용이하게 하는 Windows10 버전 1607의 향상된 기능에 대한 자세한 내용은 [백그라운드에서 미디어 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)을 참조하세요.

## <a name="create-the-background-task-class"></a>백그라운드 작업 클래스 만들기

[**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 인터페이스를 구현하는 클래스를 작성하여 백그라운드로 코드를 실행할 수 있습니다. 이 코드는 예를 들면, [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 또는 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)를 사용하여 특정 이벤트가 발생할 때 실행됩니다.

다음 단계에서는 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 인터페이스를 구현하는 새 클래스를 작성하는 방법을 보여 줍니다. 시작하기 전에 솔루션에서 백그라운드 작업에 대한 새 프로젝트를 만듭니다. 백그라운드 작업에 대한 빈 클래스를 새로 추가하고 [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847) 네임스페이스를 가져옵니다.

1.  백그라운드 작업에 대한 새 프로젝트를 만들고 솔루션에 추가합니다. 이렇게 하려면 **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고 추가-&gt;새 프로젝트를 선택합니다. 그런 다음 **Windows 런타임 구성 요소(유니버설 Windows)** 프로젝트 형식을 선택하고 프로젝트 이름을 지정한 후 확인을 클릭합니다.
2.  UWP(유니버설 Windows 플랫폼) 앱 프로젝트에서 백그라운드 작업 프로젝트를 참조합니다.

    C++ 앱의 경우 앱 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. 그런 다음 **공용 속성** 으로 이동하여 **새 참조 추가**를 클릭한 후 백그라운드 작업 프로젝트 옆의 확인란을 선택하고 두 대화 상자에서 모두 **확인** 을 클릭합니다.

    C# 앱의 경우 앱 프로젝트에서 **참조** 를 마우스 오른쪽 단추로 클릭하고 **새 참조 추가**를 선택합니다. **솔루션** 아래에서 **프로젝트**를 선택하고 백그라운드 작업 프로젝트의 이름을 선택한 후 **확인**을 클릭합니다.

3.  [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 인터페이스를 구현하는 새 클래스를 만듭니다. [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드는 지정한 이벤트가 트리거될 때 호출되는 필수 진입점입니다. 이 메서드는 모든 백그라운드 작업에 필요합니다.

    > [!NOTE]
    > 백그라운드 작업 클래스 자체와 백그라운드 작업 프로젝트의 다른 모든 클래스가 **sealed**인 **public** 클래스여야 합니다.

    다음 샘플 코드는 백그라운드 작업 클래스의 기본 시작 지점을 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     //
    >     // ExampleBackgroundTask.cs
    >     //
    >
    >     using Windows.ApplicationModel.Background;
    >
    >     namespace Tasks
    >     {
    >         public sealed class ExampleBackgroundTask : IBackgroundTask
    >         {
    >             public void Run(IBackgroundTaskInstance taskInstance)
    >             {
    >                 
    >             }        
    >         }
    >     }
    > ```
    > ```cpp
    >     //
    >     // ExampleBackgroundTask.h
    >     //
    >
    >     #pragma once
    >
    >     using namespace Windows::ApplicationModel::Background;
    >
    >     namespace RuntimeComponent1
    >     {
    >         public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    >         {
    >
    >         public:
    >             ExampleBackgroundTask();
    >
    >             virtual void Run(IBackgroundTaskInstance^ taskInstance);
    >             void OnCompleted(
    >                     BackgroundTaskRegistration^ task,
    >                     BackgroundTaskCompletedEventArgs^ args
    >                     );
    >         };
    >     }
    >
    >     //
    >     // ExampleBackgroundTask.cpp
    >     //
    >
    >     #include "ExampleBackgroundTask.h"
    >
    >     using namespace Tasks;
    >
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >
    >     }
    >  ```

4.  백그라운드 작업에서 비동기 코드를 실행할 경우 백그라운드 작업은 deferral을 사용해야 합니다. deferral을 사용하지 않을 경우 비동기 메서드 호출이 완료되기 전에 Run 메서드가 완료되면 백그라운드 작업 프로세스가 예기치 않게 종료될 수 있습니다.

    비동기 메서드를 호출하기 전에 Run 메서드에서 deferral을 요청합니다. 비동기 메서드에서 액세스할 수 있도록 deferral을 전역 변수에 저장합니다. 비동기 코드가 완료된 후에 deferral 완료를 선언합니다.

    다음 샘플 코드는 deferral을 가져와서 저장하고, 비동기 코드가 완료되면 해제합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskDeferral _deferral; // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     public async void Run(IBackgroundTaskInstance taskInstance)
    >     {
    >         _deferral = taskInstance.GetDeferral()
    >         //
    >         // TODO: Insert code to start one or more asynchronous methods using the
    >         //       await keyword, for example:
    >         //
    >         // await ExampleMethodAsync();
    >         //
    >         
    >         _deferral.Complete();
    >     }
    > ```
    > ```cpp
    >     BackgroundTaskDeferral^ deferral = taskInstance->GetDeferral(); // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >         //
    >         // TODO: Modify the following line of code to call a real async function.
    >         //       Note that the task<void> return type applies only to async
    >         //       actions. If you need to call an async operation instead, replace
    >         //       task<void> with the correct return type.
    >         //
    >         task<void> myTask(ExampleFunctionAsync());
    >         
    >         myTask.then([=] () {
    >             deferral->Complete();
    >         });
    >     }
    > ```

> [!NOTE]
> C#에서는 **async/await** 키워드를 사용하여 백그라운드 작업의 비동기 메서드를 호출할 수 있습니다. C++에서는 작업 체인을 사용하여 유사한 결과를 얻을 수 있습니다.

비동기 패턴에 대한 자세한 내용은 [비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187335)을 참조하세요. deferral을 사용하여 백그라운드 작업이 일찍 중지되지 않도록 하는 방법의 추가 예제를 보려면 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)을 참조하세요.

다음 단계는 앱 클래스 중 하나(예제: MainPage.xaml.cs)에서만 완료합니다.

> [!NOTE]
> 백그라운드 작업을 등록하는 데에만 사용되는 함수를 만들 수도 있습니다. [백그라운드 작업 등록](register-a-background-task.md)을 참조하세요. 이 경우 다음 세 단계를 사용하지 않고 트리거를 생성하여 작업 이름, 작업 진입점 및 조건(옵션)과 함께 등록 함수에 제공하면 됩니다.

## <a name="register-the-background-task-to-run"></a>실행할 백그라운드 작업 등록

1.  [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 속성을 반복하여 백그라운드 작업이 이미 등록되어 있는지 확인합니다. 이 단계는 중요합니다. 앱이 기존 백그라운드 작업 등록을 확인하지 않는 경우 작업을 여러 번 등록하기 쉬우므로 성능에 문제가 발생하거나 작업이 완료되기 전에 작업의 가용 CPU 시간을 모두 사용할 수 있습니다.

    다음 예제에서는 AllTasks 속성을 반복하고 작업이 이미 등록된 경우 플래그 변수를 true로 설정합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var taskRegistered = false;
    >     var exampleTaskName = "ExampleBackgroundTask";
    >
    >     foreach (var task in BackgroundTaskRegistration.AllTasks)
    >     {
    >         if (task.Value.Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >     }
    > ```
    > ```cpp
    >     boolean taskRegistered = false;
    >     Platform::String^ exampleTaskName = "ExampleBackgroundTask";
    >
    >     auto iter = BackgroundTaskRegistration::AllTasks->First();
    >     auto hascur = iter->HasCurrent;
    >
    >     while (hascur)
    >     {
    >         auto cur = iter->Current->Value;
    >
    >         if(cur->Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >
    >         hascur = iter->MoveNext();
    >     }
    > ```

2.  백그라운드 작업이 아직 등록되지 않은 경우 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)를 사용하여 백그라운드 작업의 인스턴스를 만듭니다. 작업 진입점이 네임스페이스가 앞에 오는 백그라운드 작업 클래스의 이름이어야 합니다.

    백그라운드 작업 트리거는 백그라운드 작업이 실행되는 시간을 제어합니다. 가능한 트리거 목록은 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)를 참조하세요.

    예를 들어 다음 코드는 새 백그라운드 작업을 만들고 **TimeZoneChanged** 트리거가 발생할 때 실행되도록 설정합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var builder = new BackgroundTaskBuilder();
    >
    >     builder.Name = exampleTaskName;
    >     builder.TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
    > ```
    > ```cpp
    >     auto builder = ref new BackgroundTaskBuilder();
    >
    >     builder->Name = exampleTaskName;
    >     builder->TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
    > ```

3.  트리거 이벤트가 발생한 후 작업이 실행될 시간을 제어하는 조건을 추가할 수도 있습니다(옵션). 예를 들어 사용자가 있을 때까지 작업이 실행되지 않게 하려면 **UserPresent** 조건을 사용합니다. 가능한 조건 목록은 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)을 참조하세요.

    다음 샘플 코드에서는 사용자가 있어야 하는 조건을 할당합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```
    > ```cpp
    >     builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
    > ```

4.  [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 개체에 대해 Register 메서드를 호출하여 백그라운드 작업을 등록합니다. [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 결과를 다음 단계에서 사용할 수 있도록 저장합니다.

    다음 코드는 백그라운드 작업을 등록하고 결과를 저장합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskRegistration task = builder.Register();
    > ```
    > ```cpp
    >     BackgroundTaskRegistration^ task = builder->Register();
    > ```

> [!NOTE]
> 유니버설 Windows 앱에서 백그라운드 트리거 형식을 등록하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다.

업데이트를 릴리스한 후에 유니버설 Windows 앱이 계속 제대로 실행되도록 하려면 **ServicingComplete**([SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839) 참조) 트리거를 사용하여 앱의 데이터베이스를 마이그레이션하고 백그라운드 작업을 등록하는 등 사후 업데이트 구성 변경을 수행합니다. 이때 이전 버전의 앱과 관련된 백그라운드 작업의 등록을 취소([**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) 참조)하고 새로운 버전의 앱에 대한 백그라운드 작업을 등록([**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 참조)하는 것이 가장 좋습니다.

자세한 내용은 [백그라운드 작업에 대한 지침](guidelines-for-background-tasks.md)을 참조하세요.

## <a name="handle-background-task-completion-using-event-handlers"></a>이벤트 처리기를 사용하여 백그라운드 작업 완료 처리

앱에서 백그라운드 작업의 결과를 가져올 수 있도록 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)를 사용하여 메서드를 등록해야 합니다. 앱을 포그라운드에서 마지막으로 실행한 이후에 백그라운드 작업이 완료된 경우 앱을 시작하거나 다시 시작하면 mark 메서드가 호출됩니다. 앱이 포그라운드에서 현재 실행 중인 경우에는 백그라운드 작업이 완료되면 OnCompleted 메서드가 즉시 호출됩니다.

1.  백그라운드 작업 완료를 처리하는 OnCompleted 메서드를 씁니다. 예를 들어 백그라운드 작업의 결과로 UI가 업데이트될 수 있습니다. 이 예제에서는 *args* 매개 변수를 사용하지 않지만 여기에 표시된 메서드 공간은 OnCompleted 이벤트 처리기 메서드에 필요합니다.

    다음 샘플 코드에서는 백그라운드 작업 완료를 인식하고 메시지 문자열을 가져오는 예제 UI 업데이트 메서드를 호출합니다.

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    >         var key = task.TaskId.ToString();
    >         var message = settings.Values[key].ToString();
    >         UpdateUI(message);
    >     }
    > ```
    > ```cpp
    >     void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {
    >         auto settings = ApplicationData::Current->LocalSettings->Values;
    >         auto key = task->TaskId.ToString();
    >         auto message = dynamic_cast<String^>(settings->Lookup(key));
    >         UpdateUI(message);
    >     }
    > ```

    > [!NOTE]
    > UI 스레드가 중지되는 것을 방지하려면 UI 업데이트를 비동기적으로 수행해야 합니다. 예제를 보려면 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)의 UpdateUI 메서드를 참조하세요.


2.  백그라운드 작업을 등록한 위치로 돌아갑니다. 해당 코드 줄 뒤에 새 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 개체를 추가합니다. OnCompleted 메서드를 **BackgroundTaskCompletedEventHandler** 생성자에 대한 매개 변수로 제공합니다.

    다음 샘플 코드에서는 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)를 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)에 추가합니다.

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    > ```
    > ```cpp
    >     task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
    > ```

## <a name="declare-that-your-app-uses-background-tasks-in-the-app-manifest"></a>앱 매니페스트에서 앱이 백그라운드 작업을 사용한다고 선언

백그라운드 작업을 실행하려면 먼저 앱 매니페스트에서 각 백그라운드 작업을 선언해야 합니다. 앱에서 매니페스트에 나열되지 않은 트리거를 사용하여 백그라운드 작업을 등록하려고 시도하면 등록이 실패합니다.

1.  Package.appxmanifest라는 파일을 열어 패키지 매니페스트 디자이너를 엽니다.
2.  **선언** 탭을 엽니다.
3.  **사용 가능한 선언** 드롭다운에서 **백그라운드 작업**을 선택하고 **추가**를 클릭합니다.
4.  **시스템 이벤트** 확인란을 선택합니다.
5.  **진입점:** 텍스트 상자에 백그라운드 클래스의 이름 및 네임스페이스(이 예제의 경우 RuntimeComponent1.ExampleBackgroundTask)를 입력합니다.
6.  매니페스트 디자이너를 닫습니다.

    백그라운드 작업을 등록하기 위해 다음 Extensions 요소가 Package.appxmanifest 파일에 추가됩니다.

    ```xml
    <Extensions>
      <Extension Category="windows.backgroundTasks" EntryPoint="RuntimeComponent1.ExampleBackgroundTask">
        <BackgroundTasks>
          <Task Type="systemEvent" />
        </BackgroundTasks>
      </Extension>
    </Extensions>
    ```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이제 백그라운드 작업 클래스를 작성하는 방법, 앱 내에서 백그라운드 작업을 등록하는 방법 및 백그라운드 작업이 완료될 때 앱에서 인식하는 방법을 이해해야 합니다. 또한 앱에서 백그라운드 작업을 등록할 수 있도록 응용 프로그램 매니페스트를 업데이트하는 방법을 이해해야 합니다.

> [!NOTE]
> 백그라운드 작업을 사용하는, 완벽하고 강력한 기능을 갖춘 UWP 앱의 컨텍스트에서 비슷한 코드 예제를 보려면 [백그라운드 작업 샘플](http://go.microsoft.com/fwlink/p/?LinkId=618666)을 다운로드하세요.

API 참조, 백그라운드 작업 개념 지침, 백그라운드 작업을 사용하는 앱을 작성하는 자세한 방법에 대해서는 다음 관련 항목을 참조하세요.

> [!NOTE]
> 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

## <a name="related-topics"></a>관련 항목

**자세한 백그라운드 작업 지침 항목**

* [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)
* [백그라운드 작업 등록](register-a-background-task.md)
* [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)
* [유지 관리 트리거 사용](use-a-maintenance-trigger.md)
* [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)
* [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)
* [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)
* [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md).
* [Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환](convert-out-of-process-background-task.md)  

**백그라운드 작업 지침**

* [백그라운드 작업 지침](guidelines-for-background-tasks.md)
* [백그라운드 작업 디버그](debug-a-background-task.md)
* [Windows 스토어 앱에서 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법(디버깅 시)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**백그라운드 작업 API 참조**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)