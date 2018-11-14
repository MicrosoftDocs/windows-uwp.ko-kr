---
author: PatrickFarley
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: 인쇄 워크플로 사용자 지정
description: 조직의 요구 사항을 충족하는 사용자 지정 인쇄 워크플로 경험을 만듭니다.
ms.author: pafarley
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp, 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: f58c0c8397831595c237b7bd9fe4eafb25594ab3
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6156979"
---
# <a name="customize-the-print-workflow"></a>인쇄 워크플로 사용자 지정

## <a name="overview"></a>개요
개발자는 인쇄 워크플로 앱을 사용하여 인쇄 워크플로 경험을 사용자 지정할 수 있습니다. 인쇄 워크플로 앱은 [Microsoft Store 장치 앱(WSDA)](https://docs.microsoft.com/windows-hardware/drivers/devapps/)의 기능을 기반으로 확장한 UWP 앱이므로, 계속 진행하기 전에 WSDA에 대해 숙지하고 있으면 도움이 될 것입니다. 

WSDA의 경우와 마찬가지로 소스 응용 프로그램 사용자가 무언가를 인쇄하기로 하고 인쇄 대화 상자를 탐색하면 시스템은 워크플로 앱이 해당 프린터와 연결되어 있는지 확인합니다. 확인되면 인쇄 워크플로 앱이 시작됩니다(주로 백그라운드 작업으로, 아래에서 자세히 설명). 워크플로 앱에서 인쇄 티켓(현재 인쇄 작업에 대한 프린터 장치 설정을 구성하는 XML 문서)과 인쇄할 실제 XPS 콘텐츠를 모두 변경할 수 있습니다. 프로세스의 중간 단계에 UI를 시작하여 사용자에게 이 기능을 선택적으로 노출할 수 있습니다. 작업을 수행한 후 인쇄 콘텐츠와 인쇄 티켓을 드라이버로 전달합니다.

백그라운드 및 포그라운드 구성 요소와 관련되어 있고, 다른 앱과 기능적으로 결합되어 있기 때문에 인쇄 워크플로 앱은 다른 범주의 UWP 앱보다 구현이 더 복잡할 수 있습니다. 다양한 기능을 구현하는 방법을 보다 잘 이해하려면 이 가이드의 [워크플로 앱 샘플](https://github.com/Microsoft/print-oem-samples)을 살펴보는 것이 좋습니다. 간결함을 위해 다양한 오류 검사 및 UI 관리 등 일부 기능은 이 설명서에서 생략되었습니다.

## <a name="getting-started"></a>시작하기

워크플로 앱은 적절한 시점에 시작할 수 있도록 인쇄 시스템에 대한 진입점을 표시해야 합니다. 이를 위해 UWP 프로젝트의 *package.appxmanifest* 파일의 `Application/Extensions` 요소에 다음 선언을 삽입합니다. 

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT] 
> 인쇄 사용자 지정에 사용자 입력이 필요하지 않은 시나리오가 많습니다. 그래서 인쇄 워크플로 앱은 기본적으로 백그라운드 작업으로 실행됩니다.

워크플로 앱이 인쇄 작업을 시작한 원본 응용 프로그램과 연결되어 있으면(자세한 지침은 이후 섹션 참조) 인쇄 시스템이 매니페스트 파일에서 백그라운드 작업 진입점을 검사합니다.

## <a name="do-background-work-on-the-print-ticket"></a>인쇄 티켓에 대한 백그라운드 작업 수행

인쇄 시스템이 워크플로 앱에서 가장 먼저 수행하는 작업은 백그라운드 작업을 활성화하는 것입니다(이 경우 `WFBackgroundTasks` 네임스페이스의 `WfBackgroundTask` 클래스). 백그라운드 작업의 `Run` 메서드에서 작업의 트리거 세부 정보를 **[PrintWorkflowTriggerDetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)** 인스턴스로 캐스트해야 합니다. 그러면 인쇄 워크플로 백그라운드 작업을 위한 특수한 기능이 제공됩니다. **[PrintWorkFlowBackgroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)** 의 인스턴스인 **[PrintWorkflowSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)** 속성을 노출합니다. 인쇄 워크플로 세션 클래스(백그라운드 및 포그라운드 유형 모두)가 인쇄 워크플로 앱의 순차적인 단계를 제어합니다. 

그런 다음 이 세션 클래스에서 발생시킬 두 이벤트에 대한 처리기 메서드를 등록합니다. 나중에 계속 이러한 메서드를 정의하게 됩니다.

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

`Start` 메서드가 호출되면 세션 관리자가 **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** 이벤트를 먼저 발생시킵니다. 이 이벤트는 인쇄 작업과 인쇄 티켓에 대한 일반적인 정보를 노출합니다. 이 단계에 인쇄 티켓을 백그라운드에서 편집할 수 있습니다. 

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

중요한 것은, **SetupRequested** 처리 중에 앱이 포그라운드 구성 요소를 시작할지 여부를 결정한다는 점입니다. 이는 이전에 로컬 스토리지에 저장한 설정이나 인쇄 티켓 편집 중에 발생한 이벤트 또는 특정 앱의 정적 설정에 따라 달라질 수 있습니다.

```csharp
    // ...
    
    if (UIrequested) {
        printTaskSetupArgs.SetRequiresUI();

        // Any data that is to be passed to the foreground task must be stored the app's local storage.
        // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
        // it can be identified as pertaining to this workflow app session.
    }

    // Complete the deferral taken out at the start of OnSetupRequested
    setupRequestedDeferral.Complete();
}
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>인쇄 작업에 대한 포그라운드 작업 수행(선택 사항)

**[SetRequiresUI](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** 메서드가 호출되면 인쇄 시스템이 포그라운드 응용 프로그램의 진입점에 대한 매니페스트 파일을 검사합니다. *package.appxmanifest* 파일의 `Application/Extensions` 요소에 다음 행이 있어야 합니다. `EntryPoint`의 값을 포그라운드 앱의 이름으로 바꿉니다.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" /> 
```

그런 다음 인쇄 시스템이 해당 앱 진입점에 대해 **OnActivated** 메서드를 호출합니다. _App.xaml.cs_ 파일의 **OnActivated** 메서드에서 워크플로 앱이 활성화 종류를 확인하여 워크플로 활성화인지 확인해야 합니다. 확인이 되면 워크플로 앱은 **[PrintWorkflowForegroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** 개체를 속성으로 노출하는 **[PrintWorkflowUIActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)** 개체에 활성화 인수를 캐스트할 수 있습니다. 이 개체는 이전 섹션의 해당 백그라운드 개체와 마찬가지로 인쇄 시스템에서 발생시킨 이벤트를 포함하며 이 개체에 처리기를 할당할 수 있습니다. 이 경우 이벤트 처리 기능은 `WorkflowPage`라는 별도의 클래스로 구현됩니다.

먼저 _App.xaml.cs_ 파일에서,

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when 
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

UI에 이벤트 처리기가 연결되고 **OnActivated**메서드가 종료되면 인쇄 시스템은 처리할 UI에 대해 **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** 이벤트를 발생시킵니다. 이 이벤트는 인쇄 작업 정보 및 인쇄 티켓 문서를 포함하지만 추가 UI의 실행을 요청할 수 없는, 제공된 백그라운드 작업 설정 이벤트와 동일한 데이터를 제공합니다. _WorkflowPage.xaml.cs_ 파일에서,

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);
    
    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

다음으로 인쇄 시스템은 UI에 대한 **[XpsDataAvailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** 이벤트를 발생시킵니다. 이 이벤트 처리기에서 워크플로 앱은 설치 이벤트에서 사용할 수 있는 모든 데이터에 액세스할 수 있으며, Raw 바이트 스트림이나 개체 모델로 직접 XPS 데이터를 읽을 수 있습니다. XPS 데이터에 액세스하면 UI에서 인쇄 미리 보기 서비스를 제공하고 워크플로 앱이 데이터에서 실행할 작업에 대한 추가 정보를 사용자에게 제공할 수 있습니다. 

이 이벤트 처리기의 일부로 워크플로 앱은 사용자와 계속 상호 작용할 경우 deferral 개체를 획득해야 합니다. deferral이 없으면 **XpsDataAvailable** 이벤트 처리기가 종료되거나 비동기 메서드를 호출할 때 인쇄 시스템은 UI 작업 완료로 간주합니다. 앱이 사용자와 UI의 상호 작용에서 필요한 모든 정보를 수집하면 인쇄 시스템이 계속 진행할 수 있도록 deferral을 완료해야 합니다.

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent(); 
 
    IInputStream inputStream = xpsStream.GetInputSpoolStream(); 
 
    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream)) 
    { 
        // Read the XPS data from input stream 
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength]; 
        while (inputReader.UnconsumedBufferLength > 0) 
        { 
            inputReader.ReadBytes(xpsData); 
            // Do something with the XPS data, e.g. preview 
            // ...                  
        } 
    } 

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

또한 이벤트 인수에 의해 노출된 **[PrintWorkflowSubmittedOperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** 인스턴스는 인쇄 작업을 취소하거나 작업이 성공적이지만 출력물 인쇄 작업이 필요하지 않음을 나타낼 수 있는 옵션을 제공합니다. 이 작업은 **[PrintWorkflowSubmittedStatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** 값을 사용해 **[Complete](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** 메서드를 호출하여 수행됩니다.

> [!NOTE]
> 워크플로 앱이 인쇄 작업을 취소할 때 작업 취소 이유를 나타내는 알림 메시지를 제공하는 것이 좋습니다. 


## <a name="do-final-background-work-on-the-print-content"></a>인쇄 콘텐츠에 대한 최종 백그라운드 작업 수행

UI가 **PrintTaskXpsDataAvailable** 이벤트에서 deferral을 완료하면(또는 UI 단계가 바이패스된 경우) 인쇄 시스템은 백그라운드 작업에 대해 **[Submitted](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** 이벤트를 발생시킵니다. 이 이벤트의 처리기에서 워크플로 앱은 **XpsDataAvailable** 이벤트에서 제공하는 동일한 모든 데이터에 액세스할 수 있습니다. 그러나 **Submitted**는 이전 이벤트와 달리 **[PrintWorkflowTarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)** 인스턴스를 통해 최종 인쇄 작업 콘텐츠에 *쓰기* 액세스 권한을 제공합니다. 

원본 데이터가 Raw 바이트 스트림으로 액세스되는지 아니면 XPS 개체 모델로 액세스되는지에 따라, 최종 인쇄를 위해 데이터를 스풀링하는 데 사용되는 개체가 달라집니다. 워크플로 앱이 바이트 스트림을 통해 원본 데이터에 액세스하면 최종 작업 데이터를 기록할 수 있는 출력 바이트 스트림이 제공됩니다. 워크플로 앱이 개체 모델을 통해 원본 데이터에 액세스하면 출력 작업에 개체를 쓸 수 있는 문서 작성기가 제공됩니다. 두 경우 모두 워크플로 앱은 모든 원본 데이터를 읽고, 필요한 데이터를 수정하고, 수정된 데이터를 출력 대상에 써야 합니다.

백그라운드 작업이 데이터 쓰기를 완료하면 해당 **PrintWorkflowSubmittedOperation** 개체에서 **Complete**를 호출해야 합니다. 워크플로 앱이 이 단계를 완료하고 **Submitted** 이벤트 처리기가 종료되면 워크플로 세션이 닫히고 사용자는 표준 인쇄 대화 상자를 통해 최종 인쇄 작업의 상태를 모니터링할 수 있습니다.

## <a name="final-steps"></a>마지막 단계

### <a name="register-the-print-workflow-app-to-the-printer"></a>프린터에 인쇄 워크플로 앱 등록

워크플로 앱은 WSDA에 사용되는 것과 동일한 유형의 메타데이터 파일 제출을 사용하는 프린터와 연결됩니다. 사실 하나의 UWP 응용 프로그램이 인쇄 작업 설정 기능을 제공하는 WSDA와 워크플로 앱 역할을 동시에 수행할 수 있습니다. [메타데이터 연결을 생성하는 해당 WSDA 단계](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata)를 따르세요.

차이가 있다면, WSDA는 사용자에 대해 자동으로 활성화되지만(해당 사용자가 연결된 디바이스에서 인쇄할 때 앱이 항상 실행됨) 워크플로 앱은 그렇지 않다는 것입니다. 설정해야 하는 별도의 정책이 있습니다.

### <a name="set-the-workflow-apps-policy"></a>워크플로 앱의 정책 설정
워크플로 앱 정책은 워크플로 앱을 실행할 디바이스에서 Powershell 명령에 의해 설정됩니다. Set-Printer, Add-Printer(기존 포트) 및 Add-Printer(새 WSD 포트) 명령은 워크플로 정책을 설정할 수 있도록 수정됩니다. 
* `Disabled`: 워크플로 앱이 활성화되지 않습니다.
* `Uninitialized`: 워크플로 DCA가 시스템에 설치되어 있는 경우 워크플로 앱이 활성화됩니다. 앱이 설치되지 않은 경우에도 인쇄가 계속 진행됩니다. 
* `Enabled`: 워크플로 DCA가 시스템에 설치되어 있는 경우 워크플로 contract가 활성화됩니다. 앱이 설치되지 않은 경우 인쇄에 실패합니다. 

다음 명령을 사용하면 지정된 프린터에 워크플로 앱이 필요합니다.
```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy On
```

로컬 사용자는 로컬 프린터에서 이 정책을 실행할 수 있습니다. 또는 엔터프라이즈 구현을 위해 프린터 관리자가 인쇄 서버에서 이 정책을 실행할 수 있습니다. 그러면 정책이 모든 클라이언트 연결과 동기화됩니다. 프린터 관리자는 새 프린터가 추가될 때마다 이 정책을 사용할 수 있습니다.

## <a name="see-also"></a>참고 항목

[워크플로 앱 샘플](https://github.com/Microsoft/print-oem-samples)

[Windows.Graphics.Printing.Workflow 네임스페이스](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)


