---
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: 인쇄 워크플로 사용자 지정
description: 조직의 요구에 맞게 사용자 지정 인쇄 워크플로 환경을 만듭니다.
ms.date: 07/03/2020
ms.topic: article
keywords: windows 10, uwp, 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: 779965ca46efe7fb63adac46ef2568c2ecff66f1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172197"
---
# <a name="customize-the-print-workflow"></a>인쇄 워크플로 사용자 지정

## <a name="overview"></a>개요

개발자는 인쇄 워크플로 앱을 사용 하 여 인쇄 워크플로 환경을 사용자 지정할 수 있습니다. 인쇄 워크플로 앱은 [WSDAs (Microsoft Store 장치 앱)](/windows-hardware/drivers/devapps/)의 기능을 확장 하는 UWP 앱 이므로, 추가를 진행 하기 전에 WSDAs에 대 한 지식이 있으면 도움이 됩니다.

WSDAs의 경우와 마찬가지로 원본 응용 프로그램의 사용자가 무언가를 인쇄 하 고 인쇄 대화 상자를 탐색 하면 시스템에서 워크플로 앱이 해당 프린터와 연결 되어 있는지 여부를 확인 합니다. 이 경우 인쇄 워크플로 앱이 시작 됩니다 (주로 백그라운드 작업으로, 아래에 자세히 나와 있습니다). 워크플로 앱은 인쇄 티켓 (현재 인쇄 작업의 프린터 장치 설정을 구성 하는 XML 문서)과 인쇄 될 실제 XPS 콘텐츠를 모두 변경할 수 있습니다. 프로세스를 통해 중간에 UI를 시작 하 여 사용자에 게이 기능을 선택적으로 노출할 수 있습니다. 작업을 수행한 후에는 인쇄 콘텐츠와 인쇄 티켓을 드라이버에 전달 합니다.

백그라운드 및 포그라운드 구성 요소가 필요 하 고 다른 앱과 기능적으로 결합 되기 때문에 인쇄 워크플로 앱은 UWP 앱의 다른 범주 보다 구현 하기가 더 복잡할 수 있습니다. 이 가이드를 읽는 동안 [워크플로 앱 샘플](https://github.com/Microsoft/print-oem-samples) 을 검사 하 여 다양 한 기능을 구현 하는 방법을 더 잘 이해 하는 것이 좋습니다. 다양 한 오류 검사 및 UI 관리와 같은 일부 기능은이 가이드에서 간단 하 게 사용할 수 없습니다.

## <a name="getting-started"></a>시작

워크플로 앱은 적절 한 시간에 시작할 수 있도록 인쇄 시스템에 대 한 진입점을 나타내야 합니다. 이 작업을 수행 하려면 `Application/Extensions` UWP 프로젝트의 *appxmanifest.xml* 파일에 있는 요소에 다음 선언을 삽입 합니다.

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT]
> 인쇄 사용자 지정에는 사용자 입력이 필요 하지 않은 여러 가지 시나리오가 있습니다. 따라서 인쇄 워크플로 앱은 기본적으로 백그라운드 작업으로 실행 됩니다.

워크플로 앱이 인쇄 작업을 시작한 원본 응용 프로그램에 연결 된 경우 (뒷부분의 지침 참조) 인쇄 시스템은 백그라운드 작업 진입점에 대 한 매니페스트 파일을 검사 합니다.

## <a name="do-background-work-on-the-print-ticket"></a>인쇄 티켓에 대 한 백그라운드 작업 수행

인쇄 시스템이 워크플로 앱에서 가장 먼저 수행 하는 작업은 백그라운드 작업 (이 경우 `WfBackgroundTask` 네임 스페이스의 클래스)을 활성화 한다는 것입니다 `WFBackgroundTasks` . 백그라운드 작업의 `Run` 메서드에서 작업의 트리거 세부 정보를 **[Printworkflowtriggerdetails](/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)** 인스턴스로 캐스팅 해야 합니다. 이렇게 하면 인쇄 워크플로 백그라운드 작업에 대 한 특수 기능이 제공 됩니다. **[PrintWorkFlowBackgroundSession](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)** 의 인스턴스인 **[printworkflowsession](/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)** 속성을 노출 합니다. 인쇄 워크플로 세션 클래스 (배경 및 전경)는 인쇄 워크플로 앱의 순차적 단계를 제어 합니다.

그런 다음이 세션 클래스에서 발생 하는 두 이벤트에 대해 처리기 메서드를 등록 합니다. 이러한 메서드는 나중에 정의 합니다.

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

`Start`메서드가 호출 되 면 세션 관리자가 **[setuprequested](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** 이벤트를 먼저 발생 시킵니다. 이 이벤트는 인쇄 작업 뿐만 아니라 인쇄 티켓에 대 한 일반 정보를 제공 합니다. 이 단계에서 인쇄 티켓은 백그라운드에서 편집할 수 있습니다.

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

중요 한 것은 응용 프로그램에서 포그라운드 구성 요소를 시작할지 여부를 결정 하는 **Setuprequested** 을 처리 하는 것입니다. 이는 이전에 로컬 저장소에 저장 된 설정이 나 인쇄 티켓을 편집 하는 동안 발생 한 설정에 따라 달라 지 며 특정 앱의 정적 설정 일 수 있습니다.

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
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>인쇄 작업에서 포그라운드 작업 수행 (옵션)

**[SetRequiresUI](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** 메서드가 호출 되 면 인쇄 시스템이 전경 응용 프로그램에 대 한 진입점에 대 한 매니페스트 파일을 검사 합니다. `Application/Extensions` *Appxmanifest.xml* 파일의 요소에는 다음 줄이 있어야 합니다. 의 값을 `EntryPoint` 전경 앱 이름으로 바꿉니다.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" />
```

그런 다음, 인쇄 시스템은 지정 된 앱 진입점에 대해 **Onactivated** 된 메서드를 호출 합니다. _App.xaml.cs_ 파일의 **onactivated** 메서드에서 워크플로 앱은 활성화 종류를 확인 하 여 워크플로 활성화 인지 확인 해야 합니다. 그렇다면 워크플로 앱은 **[PrintWorkflowForegroundSession](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** 개체를 속성으로 노출 하는 **[PrintWorkflowUIActivatedEventArgs](/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)** 개체에 대 한 활성화 인수를 캐스팅할 수 있습니다. 이전 섹션에서와 같이이 개체는 인쇄 시스템에서 발생 하는 이벤트를 포함 하 고 이러한 개체에 처리기를 할당할 수 있습니다. 이 경우 이벤트 처리 기능은 라는 별도의 클래스에서 구현 됩니다 `WorkflowPage` .

먼저 _App.xaml.cs_ 파일에서 다음을 수행 합니다.

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

UI에 연결 된 이벤트 처리기가 있고 **Onactivated** 된 메서드가 종료 된 후에는 인쇄 시스템에서 ui가 처리할 **[setuprequested](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** 이벤트를 발생 시킵니다. 이 이벤트는 인쇄 작업 정보 및 인쇄 티켓 문서를 비롯 하 여 백그라운드 작업 설치 이벤트에서 제공 하는 것과 동일한 데이터를 제공 하지만 추가 UI의 시작을 요청 하는 기능은 제공 하지 않습니다. _WorkflowPage.xaml.cs_ 파일에서 다음을 수행 합니다.

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

그런 다음 인쇄 시스템이 UI에 대 한 **[XpsDataAvailable](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** 이벤트를 발생 시킵니다. 이 이벤트에 대 한 처리기에서 워크플로 앱은 설치 이벤트에 사용할 수 있는 모든 데이터에 액세스할 수 있으며, XPS 데이터를 원시 바이트 스트림 또는 개체 모델로 직접 읽을 수 있습니다. XPS 데이터에 액세스 하면 UI에서 인쇄 미리 보기 서비스를 제공 하 고 워크플로 앱이 데이터에 대해 실행할 작업에 대 한 추가 정보를 사용자에 게 제공할 수 있습니다.

이 이벤트 처리기의 일부로 워크플로 앱이 사용자와 계속 상호 작용 하는 경우 지연 개체를 획득 해야 합니다. 지연이 없으면 **XpsDataAvailable** 이벤트 처리기가 종료 되거나 비동기 메서드를 호출할 때 인쇄 시스템에서 UI 작업이 완료 된 것으로 간주 합니다. 앱이 사용자의 UI와의 상호 작용에서 필요한 모든 정보를 수집한 경우 인쇄 시스템이 진행 될 수 있도록 지연을 완료 해야 합니다.

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

또한 이벤트 args에서 노출 하는 **[PrintWorkflowSubmittedOperation](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** 인스턴스는 인쇄 작업을 취소 하거나 작업이 성공 하지만 출력 인쇄 작업이 필요 하지 않음을 나타내는 옵션을 제공 합니다. 이 작업을 수행 하려면 **[Printworkflowsubmittedstatus](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** 값을 사용 하 여 **[Complete](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** 메서드를 호출 합니다.

> [!NOTE]
> 워크플로 앱이 인쇄 작업을 취소 하는 경우 작업이 취소 된 이유를 나타내는 알림 메시지를 제공 하는 것이 좋습니다.

## <a name="do-final-background-work-on-the-print-content"></a>인쇄 콘텐츠에서 최종 백그라운드 작업 수행

UI가 **PrintTaskXpsDataAvailable** 이벤트에서 지연을 완료 하면 (또는 ui 단계가 바이패스 된 경우) 인쇄 시스템이 백그라운드 작업에 대해 **[제출](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** 된 이벤트를 발생 시킵니다. 이 이벤트에 대 한 처리기에서 워크플로 앱은 **XpsDataAvailable** 이벤트에서 제공 하는 모든 동일한 데이터에 대 한 액세스 권한을 얻을 수 있습니다. 그러나 이전 이벤트와 달리 **제출** 된는 **[printworkflowtarget](/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)** 인스턴스를 통해 최종 인쇄 작업 콘텐츠에 대 한 *쓰기* 액세스를 제공 합니다.

최종 인쇄를 위해 데이터를 스풀 하는 데 사용 되는 개체는 원본 데이터가 원시 바이트 스트림 또는 XPS 개체 모델로 액세스 되는지에 따라 달라 집니다. 워크플로 앱은 바이트 스트림을 통해 원본 데이터에 액세스 하는 경우 최종 작업 데이터를에 쓰도록 출력 바이트 스트림을 제공 합니다. 워크플로 앱은 개체 모델을 통해 원본 데이터에 액세스 하는 경우 출력 작업에 개체를 쓰도록 문서 작성기가 제공 됩니다. 어떤 경우 든 워크플로 앱은 모든 원본 데이터를 읽고, 필요한 데이터를 수정 하 고, 수정 된 데이터를 출력 대상에 써야 합니다.

백그라운드 작업에서 데이터 쓰기를 마치면 해당 **PrintWorkflowSubmittedOperation** 개체에서 **Complete** 를 호출 해야 합니다. 워크플로 앱이이 단계를 완료 하 고 **제출** 된 이벤트 처리기가 종료 되 면 워크플로 세션이 닫히고 사용자가 표준 인쇄 대화 상자를 통해 최종 인쇄 작업의 상태를 모니터링할 수 있습니다.

## <a name="final-steps"></a>최종 단계

### <a name="register-the-print-workflow-app-to-the-printer"></a>인쇄 워크플로 앱을 프린터에 등록

워크플로 앱은 WSDAs와 동일한 유형의 메타 데이터 파일 전송을 사용 하 여 프린터와 연결 됩니다. 실제로 단일 UWP 응용 프로그램은 워크플로 앱과 인쇄 작업 설정 기능을 제공 하는 WSDA 모두 작동할 수 있습니다. [메타 데이터 연결을 만들기 위한 해당 WSDA 단계](/windows-hardware/drivers/devapps/step-2--create-device-metadata)를 따릅니다.

차이점은 사용자에 대해 WSDAs이 자동으로 활성화 되는 동안 (사용자가 연결 된 장치에서 인쇄 될 때 앱이 항상 시작 됨) 워크플로 앱은 그렇지 않는다는 것입니다. 설정 해야 하는 별도의 정책이 있습니다.

### <a name="set-the-workflow-apps-policy"></a>워크플로 앱 정책 설정

워크플로 앱 정책은 워크플로 앱을 실행 하는 장치에서 Powershell 명령에 의해 설정 됩니다. 설정-프린터, 프린터 추가 (기존 포트) 및 프린터 추가 (새 WSD 포트) 명령이 워크플로 정책을 설정할 수 있도록 수정 됩니다.

* `Disabled`: 워크플로 앱은 활성화 되지 않습니다.
* `Uninitialized`: 워크플로 DCA가 시스템에 설치 되어 있는 경우 워크플로 앱이 활성화 됩니다. 앱이 설치 되지 않은 경우에도 인쇄는 계속 진행 됩니다.
* `Enabled`: 워크플로 DCA가 시스템에 설치 된 경우 워크플로 계약이 활성화 됩니다. 앱이 설치 되지 않은 경우 인쇄는 실패 합니다.

다음 명령을 사용 하 여 지정 된 프린터에서 워크플로 앱을 요구 합니다.

```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy Enabled
```

로컬 사용자가 로컬 프린터에서이 정책을 실행할 수도 있고, 엔터프라이즈 구현의 경우 프린터 관리자가 인쇄 서버에서이 정책을 실행할 수 있습니다. 그러면 정책이 모든 클라이언트 연결에 동기화 됩니다. 프린터 관리자는 새 프린터가 추가 될 때마다이 정책을 사용할 수 있습니다.

## <a name="see-also"></a>참고 항목

[워크플로 앱 샘플](https://github.com/Microsoft/print-oem-samples)

[창. Workflow 네임 스페이스](/uwp/api/windows.graphics.printing.workflow)