---
description: 확장 된 실행을 사용 하 여 최소화 된 상태에서 앱을 실행 상태로 유지 하는 방법을 알아봅니다.
title: 확장 실행을 사용하여 앱 일시 중단 연기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 확장 된 실행, 최소화 된 ExtendedExecutionSession, 백그라운드 작업, 응용 프로그램 수명 주기, 잠금 화면
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.localizationpriority: medium
ms.openlocfilehash: 41a084088ab75293cd4f8f70d5d8c248000645b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167747"
---
# <a name="postpone-app-suspension-with-extended-execution"></a>확장 실행을 사용하여 앱 일시 중단 연기

이 문서에서는 최소화 된 상태에서 또는 잠금 화면에서 실행할 수 있도록 확장 된 실행을 사용 하 여 앱이 일시 중단 된 시간을 연기 하는 방법을 보여 줍니다.

사용자가 앱을 최소화 하거나 중단 하는 경우 일시 중지 됨 상태로 전환 됩니다.  해당 메모리는 유지 되지만 해당 코드는 실행 되지 않습니다. 이는 시각적 사용자 인터페이스를 사용 하는 모든 OS 버전에서 적용 됩니다. 앱이 일시 중단 된 경우에 대 한 자세한 내용은 [응용 프로그램 수명 주기](app-lifecycle.md)를 참조 하세요. 

앱이 일시 중단 되는 것이 아니라, 사용자가 앱에서 이동할 때 또는 최소화 된 상태에서 실행을 유지 해야 하는 경우가 있습니다. 예를 들어 단계 계산 앱은 사용자가 다른 앱을 사용 하기 위해 이동 하는 경우에도 실행 및 추적 단계를 유지 해야 합니다. 

앱이 계속 실행 되어야 하는 경우 OS는 실행을 유지할 수 있습니다. 또는 실행을 계속 하도록 요청할 수 있습니다. 예를 들어 백그라운드에서 오디오를 재생 하는 경우 이러한 단계를 수행 하 여 [백그라운드 미디어를 재생](../audio-video-camera/background-audio.md)하는 경우 OS에서 앱을 더 오래 실행 하 게 됩니다. 그렇지 않으면 더 많은 시간을 수동으로 요청 해야 합니다. 백그라운드 실행을 수행 하는 데 걸리는 시간은 몇 분이 될 수 있지만 언제 든 지 해지 되는 세션을 처리할 수 있도록 준비 해야 합니다. 앱이 디버거에서 실행 되는 동안에는 이러한 응용 프로그램 수명 주기 시간 제약 조건이 사용 되지 않습니다. 따라서 디버거에서 실행 되 고 있지 않거나 Visual Studio에서 제공 하는 수명 주기 이벤트를 사용 하 여 응용 프로그램 일시 중단을 연기 하는 확장 된 실행 및 기타 도구를 테스트 하는 것이 중요 합니다. 
 
백그라운드에서 작업을 완료 하는 데 더 많은 시간을 요청 하는 [ExtendedExecutionSession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) 를 만듭니다. 만든 **ExtendedExecutionSession** 의 종류는 만들 때 제공 하는  [ExtendedExecutionReason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) 에 의해 결정 됩니다. **ExtendedExecutionReason** 열거형 값 **으로는 지정 되지 않음, Locationtracking** 및 **SavingData**세 가지가 있습니다. 언제 든 지 하나의 **ExtendedExecutionSession** 을 요청할 수 있습니다. 승인 된 세션 요청이 현재 활성화 되어 있는 동안 다른 세션을 만들려고 하면 그룹이 나 리소스가 요청 된 작업을 수행할 수 있는 상태가 아닌 것을 나타내는 **ExtendedExecutionSession** 생성자에서 예외 0x8007139F이 throw 됩니다. [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 및 [ExtendedExecutionForegroundReason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason)사용 안 함 제한 된 기능이 필요 하며 스토어 응용 프로그램에서 사용할 수 없습니다.

## <a name="run-while-minimized"></a>최소화 된 상태에서 실행

확장 된 실행을 사용할 수 있는 두 가지 경우는 다음과 같습니다.
- 정기적으로 실행 되는 동안 언제 든 지 응용 프로그램이 실행 중 상태입니다.
- 응용 프로그램이 응용 프로그램의 일시 중단 이벤트 처리기에서 일시 중단 이벤트 (OS가 일시 중단 된 상태로 앱을 이동 하려고 합니다.)를 받은 후

이러한 두 경우에 대 한 코드는 동일 하지만 응용 프로그램은 서로 약간 다르게 동작 합니다. 첫 번째 경우에는 일반적으로 일시 중단을 트리거하는 이벤트 (예: 사용자가 응용 프로그램에서 벗어나 이동)가 발생 하더라도 응용 프로그램은 실행 중 상태로 유지 됩니다. 실행 확장이 적용 되는 동안에는 응용 프로그램에서 일시 중단 이벤트를 수신 하지 않습니다. 확장이 삭제 되 면 응용 프로그램을 다시 일시 중단 하는 것이 가능 합니다.

두 번째 경우에는 응용 프로그램이 일시 중단 된 상태로 전환 되는 경우 확장 기간 동안 일시 중단 상태가 유지 됩니다. 확장이 만료 되 면 응용 프로그램은 추가 알림 없이 일시 중단 됨 상태로 전환 됩니다.

미디어 처리, 프로젝트 컴파일 또는 네트워크 연결 유지와 같은 시나리오에 대해 앱이 백그라운드로 이동 하기 전에 추가 시간을 요청 하는 **ExtendedExecutionSession** 를 만들 때 ExtendedExecutionReason을 사용 합니다 **.** 데스크톱 버전 (Home, Pro, Enterprise 및 교육용)에 대해 Windows 10을 실행 하는 데스크톱 장치에서는 앱이 최소화 된 상태에서 일시 중단 되는 것을 방지 해야 하는 경우이 방법을 사용할 수 있습니다.

앱이 백그라운드에서 이동할 때 발생 하는 **일시 중단** 상태 전환을 지연 하기 위해 장기 실행 작업을 시작할 때 확장을 요청 합니다. 데스크톱 장치에서 ExtendedExecutionReason를 사용 하 여 만든 확장 된 실행 세션에는 배터리 인식 시간 제한이 있습니다 **.** 장치가 벽면 전원에 연결 된 경우에는 확장 된 실행 기간의 길이에 제한이 없습니다. 장치가 배터리 전원을 사용할 경우 확장 된 실행 기간은 백그라운드에서 최대 10 분까지 실행할 수 있습니다.

태블릿 또는 랩톱 사용자는 앱 설정에 **따라** **앱이 백그라운드 작업을 실행할 수 있도록 허용** 옵션을 선택 하는 경우와 동일한 장기 실행 동작을 얻을 수 있습니다. (랩톱에서이 옵션을 찾으려면 **설정**  >  으로 이동 합니다. **시스템**  >  **배터리**  >  **앱의 배터리 사용** (남은 배터리 전력 비율에 따라) > 앱을 선택 하 > **Windows에서 관리** 기능 해제 > **앱이 백그라운드 작업을 실행 하도록 허용을**선택 합니다.  

모든 OS 버전에서 장치가 연결 된 대기 모드로 들어가면 이러한 종류의 확장 실행 세션이 중지 됩니다. Windows 10 Mobile을 실행 하는 모바일 장치에서는 이러한 종류의 확장 된 실행 세션이 화면에 있는 동안 실행 됩니다. 화면이 꺼질 때 장치는 즉시 저전력 연결-대기 모드로 전환 하려고 합니다. 데스크톱 장치에서는 잠금 화면이 표시 되 면 세션은 계속 실행 됩니다. 장치는 화면이 꺼진 후 일정 기간 동안 연결 된 대기 시간을 입력 하지 않습니다. 사용자가 기본값을 변경 하지 않는 한, Xbox OS 버전에서 장치는 1 시간 후에 Connect 대기 상태를 전환 합니다.

## <a name="track-the-users-location"></a>사용자의 위치를 추적 합니다.

앱이 [Geolocator](/uwp/api/windows.devices.geolocation.geolocator)에서 위치를 정기적으로 기록해 야 하는 경우 **ExtendedExecutionSession** 를 만들 때 **ExtendedExecutionReason 추적을 지정 합니다.** 사용자의 위치를 정기적으로 모니터링 해야 하며 이러한 이유를 사용 해야 하는 적합성 추적 및 탐색을 위한 앱입니다.

모바일 장치에서 화면이 잠겨 있는 경우를 포함 하 여 확장 된 실행 세션을 필요한 기간 동안 실행 하는 위치를 추적할 수 있습니다. 그러나 이러한 세션은 장치당 하나만 실행 될 수 있습니다. 위치 추적 확장 실행 세션은 포그라운드에서 요청할 수 있으며 앱은 **실행 중** 상태 여야 합니다. 이렇게 하면 사용자가 앱에서 확장 된 위치 추적 세션을 시작 하는 것을 알 수 있습니다. 백그라운드 작업 또는 앱 서비스를 사용 하 여 백그라운드에서 응용 프로그램이 백그라운드에 있는 동안에도 GeoLocator를 사용할 수 있습니다. 위치 추적 확장 실행 세션을 요청 하지 않으면 됩니다.

## <a name="save-critical-data-locally"></a>중요 한 데이터를 로컬로 저장

앱이 종료 되기 전에 데이터를 저장 하지 않는 경우에 사용자 데이터를 저장 하기 위해 **ExtendedExecutionSession** 를 만들 때 ExtendedExecutionReason를 지정 **합니다** .

이러한 종류의 세션을 사용 하 여 응용 프로그램의 수명을 연장 하 고 데이터를 업로드 하거나 다운로드 하지 마세요. 데이터를 업로드 해야 하는 경우에는 AC 전원을 사용할 수 있을 때 전송을 처리 하기 위해 [백그라운드 전송을](../networking/background-transfers.md) 요청 하거나 **MaintenanceTrigger** 를 등록 합니다. **ExtendedExecutionReason.SavingData** 확장 실행 세션은 앱이 포그라운드에 있으며 **실행 중** 상태일 때, 또는 백그라운드에 있으며 **일시 중단** 상태일 때 요청할 수 있습니다.

**일시 중단** 상태는 앱이 종료 되기 전에 앱이 작업을 수행할 수 있는 마지막 기회입니다. **ExtendedExecutionReason SavingData** 는 **일시 중단** 상태에서 요청할 수 있는 **ExtendedExecutionSession** 의 유일한 유형입니다. 앱이 **일시 중단** 상태에 있는 동안 **ExtendedExecutionReason. SavingData** 확장 실행 세션을 요청 하면 알아 두어야 할 잠재적인 문제가 발생 합니다. **일시 중단** 상태에 있는 동안 확장 된 실행 세션이 요청 되 고 사용자가 앱을 다시 시작 하도록 요청 하는 경우 시작 하는 데 시간이 오래 걸리는 것 처럼 보일 수 있습니다. 이는 앱의 이전 인스턴스를 닫고 앱의 새 인스턴스를 시작할 수 있을 때까지 확장 된 실행 세션 기간이 완료 되어야 하기 때문입니다. 사용자 상태가 손실 되지 않도록 하기 위해 시작 성능 시간이 부족 합니다.

## <a name="request-disposal-and-revocation"></a>요청, 삭제 및 해지

확장 된 실행 세션의 세 가지 기본적인 상호 작용 (요청, 삭제 및 해지)이 있습니다.  다음 코드 조각에서 요청을 모델링 합니다.

### <a name="request"></a>요청

```csharp
var newSession = new ExtendedExecutionSession();
newSession.Reason = ExtendedExecutionReason.Unspecified;
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```
[코드 샘플 참조](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

운영 체제를 사용 하 여 **Requestextensionasync** 를 호출 하면 사용자가 앱에 대 한 백그라운드 작업을 승인 했는지 여부와 시스템에서 백그라운드 실행을 가능 하 게 하는 데 사용할 수 있는 리소스가 있는지 여부를 확인 합니다. 언제 든 지 한 번에 하나의 세션만 앱에 대해 승인 되므로 **Requestextensionasync** 에 대 한 추가 호출로 인해 세션이 거부 됩니다.

[BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) 를 미리 확인 하 여 [BackgroundAccessStatus](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus?f=255&MSPPError=-2147217396)를 확인할 수 있습니다 .이 설정은 앱을 백그라운드에서 실행할 수 있는지 여부를 나타내는 사용자 설정입니다. 이러한 사용자 설정에 대 한 자세한 내용은 [백그라운드 작업 및 에너지 인식](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97)을 참조 하세요.

**ExtendedExecutionReason** 는 앱이 백그라운드에서 수행 하는 작업을 나타냅니다. **설명** 문자열은 응용 프로그램에서 작업을 수행 해야 하는 이유를 설명 하는 사람이 인식할 수 있는 문자열입니다. 이 문자열은 사용자에 게 제공 되지 않지만 이후 버전의 Windows에서 사용할 수 있습니다. 사용자 또는 시스템에서 앱이 백그라운드에서 더 이상 실행 되지 않는 것으로 판단 되는 경우 확장 된 실행 세션이 정상적으로 중단 될 수 있도록 **해지** 된 이벤트 처리기가 필요 합니다.

### <a name="revoked"></a>해지됨

응용 프로그램에 활성 확장 실행 세션이 있고 포그라운드 응용 프로그램에 리소스가 필요 하기 때문에 백그라운드 작업이 중단 되어야 하는 경우에는 세션이 해지 됩니다. 확장 된 실행 세션 시간은 먼저 **해지** 된 이벤트 처리기를 발생 시 키 지 않고 종료 되지 않습니다.

**ExtendedExecutionReason. SavingData** 확장 실행 세션에 대해 **취소** 된 이벤트가 발생 하면 앱은 실행 중인 작업을 완료 하 고 **일시 중단**을 완료 하는 1 초를 갖습니다.

해지는 여러 가지 이유로 발생할 수 있습니다. 실행 시간 제한에 도달 했거나, 백그라운드 에너지 할당량에 도달 했거나, 사용자가 포그라운드에서 새 앱을 열 수 있도록 메모리를 회수 해야 합니다.

다음은 취소 된 이벤트 처리기의 예제입니다.

```cs
private async void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        switch (args.Reason)
        {
            case ExtendedExecutionRevokedReason.Resumed:
                rootPage.NotifyUser("Extended execution revoked due to returning to foreground.", NotifyType.StatusMessage);
                break;

            case ExtendedExecutionRevokedReason.SystemPolicy:
                rootPage.NotifyUser("Extended execution revoked due to system policy.", NotifyType.StatusMessage);
                break;
        }

        EndExtendedExecution();
    });
}
```
[코드 샘플 참조](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>Dispose

마지막 단계는 확장 된 실행 세션을 삭제 하는 것입니다. 세션이 종료 될 때까지 대기 하는 동안 앱에서 사용 하는 에너지는 앱의 에너지 할당량에 대해 계산 되기 때문에 세션 및 기타 메모리 집약적 자산을 삭제 하려고 합니다. 앱에 대 한 에너지 할당량을 최대한 많이 보존 하려면 앱이 **일시 중단** 된 상태로 신속 하 게 이동할 수 있도록 세션 작업을 완료 하면 세션을 삭제 하는 것이 중요 합니다.

세션을 직접 삭제 하면 해지 이벤트를 기다리지 않고 응용 프로그램의 에너지 할당량 사용이 줄어듭니다. 즉, 더 많은 에너지 할당량을 사용할 수 있기 때문에 앱이 이후 세션에서 더 긴 백그라운드에서 실행 되도록 허용 됩니다. 해당 **Dispose** 메서드를 호출할 수 있도록 작업 끝까지 **ExtendedExecutionSession** 개체에 대 한 참조를 유지 해야 합니다.

확장 된 실행 세션을 삭제 하는 코드 조각은 다음과 같습니다.

```cs
void ClearExtendedExecution(ExtendedExecutionSession session)
{
    if (session != null)
    {
        session.Revoked -= SessionRevoked;
        session.Dispose();
        session = null;
    }
}
```
[코드 샘플 참조](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

앱은 한 번에 하나의 **ExtendedExecutionSession** 활성화 될 수 있습니다. 많은 앱은 저장소, 네트워크 또는 네트워크 기반 서비스와 같은 리소스에 액세스 해야 하는 복잡 한 작업을 완료 하기 위해 비동기 작업을 사용 합니다. 작업에서 여러 비동기 작업을 완료 해야 하는 경우에는 **ExtendedExecutionSession** 을 삭제 하 고 앱이 일시 중단 되도록 허용 하기 전에 이러한 각 작업의 상태를 고려해 야 합니다. 이 경우 값이 0이 될 때까지 세션을 삭제 하지 않고 계속 실행 중인 작업 수를 계산 하는 참조가 필요 합니다.

다음은 확장 된 실행 세션 기간 동안 여러 작업을 관리 하는 예제 코드입니다. 앱에서이를 사용 하는 방법에 대 한 자세한 내용은 아래 링크 된 코드 샘플을 참조 하세요.

```cs
static class ExtendedExecutionHelper
{
    private static ExtendedExecutionSession session = null;
    private static int taskCount = 0;

    public static bool IsRunning
    {
        get
        {
            if (session != null)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

    public static async Task<ExtendedExecutionResult> RequestSessionAsync(ExtendedExecutionReason reason, TypedEventHandler<object, ExtendedExecutionRevokedEventArgs> revoked, String description)
    {
        // The previous Extended Execution must be closed before a new one can be requested.       
        ClearSession();

        var newSession = new ExtendedExecutionSession();
        newSession.Reason = reason;
        newSession.Description = description;
        newSession.Revoked += SessionRevoked;

        // Add a revoked handler provided by the app in order to clean up an operation that had to be halted prematurely
        if(revoked != null)
        {
            newSession.Revoked += revoked;
        }

        ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

        switch (result)
        {
            case ExtendedExecutionResult.Allowed:
                session = newSession;
                break;
            default:
            case ExtendedExecutionResult.Denied:
                newSession.Dispose();
                break;
        }
        return result;
    }

    public static void ClearSession()
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }

        taskCount = 0;
    }

    public static Deferral GetExecutionDeferral()
    {
        if (session == null)
        {
            throw new InvalidOperationException("No extended execution session is active");
        }

        taskCount++;
        return new Deferral(OnTaskCompleted);
    }

    private static void OnTaskCompleted()
    {
        if (taskCount > 0)
        {
            taskCount--;
        }
        
        //If there are no more running tasks than end the extended lifetime by clearing the session
        if (taskCount == 0 && session != null)
        {
            ClearSession();
        }
    }

    private static void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
    {
        //The session has been prematurely revoked due to system constraints, ensure the session is disposed
        if (session != null)
        {
            session.Dispose();
            session = null;
        }
        
        taskCount = 0;
    }
}
```
[코드 샘플 참조](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>앱에서 리소스 웰을 사용 하는지 확인

앱의 메모리 및 에너지 사용을 튜닝 하는 것은 운영 체제에서 응용 프로그램이 더 이상 포그라운드 앱이 아닌 경우 응용 프로그램을 계속 실행할 수 있도록 하기 위한 핵심입니다. [메모리 관리 api](/uwp/api/windows.system.memorymanager) 를 사용 하 여 앱에서 사용 중인 메모리 양을 확인 합니다. 앱에서 사용 하는 메모리가 많을 수록 다른 앱이 전경에 있을 때 OS에서 앱을 계속 실행 하는 것이 더 어려워집니다. 사용자는 궁극적으로 앱에서 수행할 수 있는 모든 백그라운드 작업을 제어 하 고 앱이 배터리 사용에 미치는 영향을 볼 수 있습니다.

[BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) 를 사용 하 여 사용자가 앱의 백그라운드 활동을 제한 하도록 결정 했는지 확인 합니다. 사용자의 배터리 사용을 인식 하 고 사용자가 원하는 작업을 완료 하는 데 필요한 경우 백그라운드 에서만 실행 합니다.

## <a name="see-also"></a>참고 항목

[확장 된 실행 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[응용 프로그램 수명 주기](./app-lifecycle.md)  
[앱 수명 주기-앱을 백그라운드 작업과 확장 된 실행](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution) 
 으로 유지 [백그라운드 메모리 관리](./reduce-memory-usage.md)  
[백그라운드 전송](../networking/background-transfers.md)  
[배터리 인식 및 백그라운드 작업](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[MemoryManager 클래스](/uwp/api/windows.system.memorymanager)  
[백그라운드에서 미디어 재생](../audio-video-camera/background-audio.md)