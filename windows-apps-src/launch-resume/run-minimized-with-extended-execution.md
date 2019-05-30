---
description: 확장 실행을 사용하여 앱이 최소화된 상태에서 계속 실행되도록 하는 방법을 알아봅니다.
title: 확장 실행을 사용하여 앱 일시 중단 연기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 확장 실행, 최소화, ExtendedExecutionSession, 백그라운드 작업, 응용 프로그램 수명 주기, 잠금 화면
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.localizationpriority: medium
ms.openlocfilehash: 68d2c9937b02d60bb8509aedaf6277512a4e0c4a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371421"
---
# <a name="postpone-app-suspension-with-extended-execution"></a>확장 실행을 사용하여 앱 일시 중단 연기

이 문서에서는 앱이 최소화된 상태 또는 잠금 화면에서도 실행될 수 있도록 확장 실행을 사용하여 앱이 일시 중단되는 시기를 연장하는 방법을 보여 줍니다.

사용자가 앱을 최소화하거나 벗어날 경우 앱이 일시 중단됩니다.  메모리는 유지되지만 코드는 실행되지 않습니다. 이는 시각적 사용자 인터페이스를 갖춘 모든 OS 버전에서 마찬가지입니다. 앱 일시 중단의 경우에 대해 자세한 내용은 [응용 프로그램 수명 주기](app-lifecycle.md)를 참조하세요. 

사용자가 앱을 떠나거나 앱이 최소화된 상태에서 일시 중단되지 않고 계속 실행되어야 하는 경우도 있습니다. 예를 들어, 사용자가 다른 앱을 사용하기 위해 앱을 떠날 때도 단계 수 계산 앱은 계속 실행되고 추적해야 합니다. 

앱을 계속 실행해야 하는 경우 OS에서 앱을 계속 실행시키거나 계속 실행시키도록 요청할 수 있습니다. 예를 들어 백그라운드에서 오디오를 재생할 때 [백그라운드 미디어 재생](../audio-video-camera/background-audio.md)에 대한 몇 가지 단계를 따르면 OS에서 앱 실행 시간을 길게 지속할 수 있습니다. 그렇지 않으면 수동으로 추가 시간을 요청해야 합니다. 백그라운드 실행을 수행할 수 있는 시간은 대개 몇 분이지만 언제든지 취소 중인 세션을 처리할 수 있는 준비가 되어 있어야 합니다. 이러한 응용 프로그램 수명 주기 시간 제약은 디버거 아래에서 앱이 실행 중인 동안 사용되지 않습니다. 이런 이유 때문에 디버거에서 또는 Visual Studio에서 사용할 수 있는 수명 주기 이벤트를 사용하여 실행되지 않는 동안 앱 일시 중단 연기를 위한 확장 실행 및 기타 도구를 테스트하는 것이 중요합니다. 
 
백그라운드에서 작업을 완료하기 위해 추가 시간을 요청하려면 [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession)을 만듭니다. 이때 만드는 **ExtendedExecutionSession**의 종류는 만들 때 제공하는 [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason)에 의해 결정됩니다. 세 가지 **ExtendedExecutionReason** 열거형 값: **지정 하지 않으면 LocationTracking** 하 고 **SavingData**합니다. 언제든지 하나의 **ExtendedExecutionSession**만 요청할 수 있습니다. 승인된 세션 요청이 현재 활성일 때 다른 세션을 만들려고 하면 그룹 또는 리소스가 요청된 작업을 수행하기 위해 올바른 상태가 아님을 나타내는 **ExtendedExecutionSession** 생성자에서 예외 0x8007139F를 throw합니다. [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) 및 [ExtendedExecutionForegroundReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason)을 사용하면 안 됩니다. 이들은 제한된 기능을 필요로 하며 스토어 응용 프로그램에 사용할 수 없습니다.

## <a name="run-while-minimized"></a>최소화된 상태로 실행

다음 두 가지 경우 확장된 실행을 사용할 수 있습니다.
- 응용 프로그램 실행 중인 상태일 때 일반 전경 실행 중 언제든지.
- 응용 프로그램의 일시 중단 이벤트 처리기에서 응용 프로그램이 일시 중단 이벤트를 받은 후(운영 체제가 앱을 일시 중단 상태로 이동하려고 할 때).

이러한 두 가지 경우에 대한 코드는 같지만 응용 프로그램이 각 경우에서 약간 다르게 동작합니다. 첫 번째 경우 응용 프로그램은 일반적으로 일시 중단을 트리거하는 이벤트가 발생한 경우에도 실행 중인 상태가 유지됩니다(예: 사용자가 응용 프로그램에서 다른 페이지로 이동). 응용 프로그램은 확장된 실행이 적용되는 동안 일시 중단된 이벤트를 받지 않습니다. 확장이 삭제될 때 응용 프로그램은 다시 일시 중단에 대한 대상이 됩니다.

두 번째 경우 응용 프로그램이 일시 중단된 상태로 전환되면 확장 기간에 대해 일시 중단된 상태가 유지됩니다. 확장이 만료되면 응용 프로그램은 추가로 알리지 않고 일시 중단된 상태에 들어갑니다.

**ExtendedExecutionSession**을 만들 때 **ExtendedExecutionReason.Unspecified**를 사용하여 미디어 처리 또는 프로젝트 컴파일, 네트워크 연결 활성 상태 유지와 같은 시나리오에서 앱이 백그라운드로 이동하기 전에 추가 시간을 요청할 수 있습니다. Windows 10 데스크톱 버전(Home, Pro, Enterprise, Education)을 실행하는 데스크톱 장치에서 앱이 최소화된 상태로 일시 중단되지 않도록 해야 할 경우 이러한 방법을 사용합니다.

**일시 중단** 상태 전환을 지연시키기 위해서는 장기 실행 작업을 시작할 때 확장을 요청합니다. 그렇게 하지 않으면 앱이 백그라운드로 이동할 때 일시 중단 상태가 됩니다. 데스크톱 장치에서 **ExtendedExecutionReason.Unspecified**를 사용하여 만든 확장 실행 세션에는 배터리 인식 시간 제한이 있습니다. 장치가 벽부 전원에 연결되어 있으면 확장 실행 시간의 길이에 제한이 없습니다. 한편 장치가 배터리 전원으로 연결되어 있는 경우 확장 실행 시간은 백그라운드에서 최대 10분 실행 가능한 정도입니다.

**앱에 의한 배터리 사용** 설정에서 **앱이 백그라운드 작업을 실행하도록 허용** 옵션을 선택하면 태블릿이나 노트북에서도 배터리 사용 시간이 줄어드는 대신 동일하게 오랜 시간 실행시킬 수 있습니다. (노트북에서 이 옵션을 찾으려면 **설정** > **시스템** > **배터리** > **앱에 의한 배터리 사용**(남은 배터리 전력 비율 아래 링크)으로 이동하여 > 앱을 선택하고 > **Windows에서 관리됨**을 끈 후 > **앱이 백그라운드 작업을 실행하도록 허용**을 선택합니다.  

모든 OS 버전에서 이와 같은 확장 실행 세션은 장치가 연결된 대기 상태로 전환되면 중지됩니다. Windows 10 Mobile을 실행하는 모바일 장치에서 이와 같은 확장 실행 세션은 화면이 켜져 있는 동안 계속 실행되며 화면이 꺼지면 장치는 즉시 절전 모드 연결된 대기 상태로 전환됩니다. 데스크톱 장치에서 잠금 화면이 나타나면 해당 세션이 계속 실행됩니다. 화면이 꺼지면 장치는 즉시 절전 모드 연결된 대기 상태로 전환됩니다. Xbox OS 버전의 경우 사용자가 기본값을 변경하지 않으면 장치는 1시간 후 연결 대기 상태로 전환됩니다.

## <a name="track-the-users-location"></a>사용자 위치 추적

앱이 [GeoLocator](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator)에서 위치를 정기적으로 기록해야 하는 경우라면 **ExtendedExecutionSession**을 만들 때 **ExtendedExecutionReason.LocationTracking**을 지정합니다. 사용자의 위치를 정기적으로 모니터링해야 하는 피트니스 추적 및 탐색 앱은 그러한 이유를 사용해야 합니다.

모바일 장치에서 화면이 잠긴 경우를 포함하여 위치 추적 확장 실행 세션을 필요한 만큼 실행할 수 있습니다. 그러나 장치마다 그러한 세션은 하나만 실행 가능합니다. 위치 추적 확장 실행 세션은 포그라운드에서만 요청할 수 있으며 앱 상태는 **실행 중**이어야 합니다. 이를 통해 사용자는 앱에서 확장된 위치 추적 세션이 시작되었음을 알 수 있습니다. 앱이 위치 추적 확장 실행 세션의 요청 없이 백그라운드 작업 또는 앱 서비스를 사용하여 백그라운드에 있는 동안에도 GeoLocator를 사용할 수 있습니다.

## <a name="save-critical-data-locally"></a>중요한 데이터를 로컬에 저장

**ExtendedExecutionSession**을 만들 때 **ExtendedExecutionReason.SavingData**를 지정하면 앱이 종료되기 전에 데이터를 저장하지 않아 데이터 손실과 함께 부정적인 사용자 경험이 발생할 수 있는 상황에서 사용자의 데이터를 보호할 수 있습니다.

이러한 세션을 사용하여 앱의 수명을 연장하고 데이터를 업로드하거나 다운로드하면 안 됩니다. 데이터를 업로드해야 하는 경우에는 [백그라운드 전송](https://docs.microsoft.com/windows/uwp/networking/background-transfers)을 요청하거나 **MaintenanceTrigger**를 등록하여 AC 전원을 사용할 수 있을 때 전송을 처리합니다. **ExtendedExecutionReason.SavingData** 확장 실행 세션은 앱이 포그라운드에 있으며 **실행 중** 상태일 때, 또는 백그라운드에 있으며 **일시 중단** 상태일 때 요청할 수 있습니다.

**일시 중단** 상태는 앱 수명 주기 중 앱이 종료되기 전에 작업을 수행할 수 있는 마지막 기회입니다. **ExtendedExecutionReason.SavingData**는 **일시 중단** 상태에서 요청할 수 있는 유일한 **ExtendedExecutionSession**입니다. 앱이 **일시 중단** 상태일 때 **ExtendedExecutionReason.SavingData** 확장 실행 세션을 요청하면 잠재적인 문제가 발생할 수 있다는 사실을 알아 두어야 합니다. **Suspending** 상태에서 확장 실행 세션을 요청한 후 사용자가 앱을 다시 시작하도록 요청하면 시작되기까지 시간이 오래 걸릴 수 있습니다. 이는 앱의 이전 인스턴스를 닫고 앱의 새 인스턴스를 시작하기 전에 확장 실행 세션 시간부터 완료해야 하기 때문입니다. 사용자 상태가 손실되지 않도록 하려면 시작 작업 수행 시간이 길어집니다.

## <a name="request-disposal-and-revocation"></a>요청, 삭제, 해지

확장 실행 세션과 관련된 기본 조작으로는 요청, 삭제, 해지의 세 가지가 있습니다.  요청 수행은 다음 코드 조각에서 모델링됩니다.

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
[코드 샘플을 참조 하세요](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

**RequestExtensionAsync** 호출로 앱의 백그라운드 작업을 사용자가 승인했는지 여부와 백그라운드 실행에 사용 가능한 리소스가 시스템에 있는지 여부를 운영 체제에서 확인합니다. 언제든지 한 앱에 대해 한 세션만 승인되며 **RequestExtensionAsync**에 대한 추가 호출을 발생시켜 세션이 거부됩니다.

[BackgroundExecutionManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager)를 통하여 [BackgroundAccessStatus](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundaccessstatus?f=255&MSPPError=-2147217396)를 미리 확인할 수 있습니다. 이 사용자 설정은 사용자의 앱을 백그라운드에서 실행할 수 있는지 여부를 나타냅니다. 이러한 사용자 설정에 대해 자세한 내용은 [백그라운드 작업 및 에너지 인식](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97)을 참조하세요.

**ExtendedExecutionReason**은 사용자의 앱이 백그라운드에서 수행 중인 작업을 나타냅니다. **Description** 문자열은 사람이 읽을 수 있는 형태의 문자열로, 앱이 해당 작업을 수행해야 하는 이유를 설명합니다. 이 문자열은 사용자에게 표시되지 않지만 추후 Windows 릴리스에서 사용할 수 있게 될 수 있습니다. **Revoked** 이벤트 처리기는 사용자 또는 시스템의 판단으로 앱이 더 이상 백그라운드에서 실행될 수 없는 경우 확장 실행 세션이 정상적으로 중지되도록 하는 데 필요합니다.

### <a name="revoked"></a>Revoked

앱이 활성화된 확장 실행 세션을 갖추고 있으며 전경 응용 프로그램에 리소스가 필요하여 시스템이 백그라운드 작업을 중지해야 하는 경우 세션이 해지됩니다. **Revoked** 이벤트 처리기를 먼저 실행하지 않으면 확장 실행 세션 시간이 종료되지 않습니다.

**Revoked** 이벤트가 **ExtendedExecutionReason.SavingData** 확장 실행 세션에 대해 실행되면 앱은 1초만에 수행 중이던 작업을 완료하고 **일시 중단** 상태를 종료합니다.

해지는 여러 가지 이유로 발생할 수 있습니다. 실행 시간 제한에 도달한 경우, 백그라운드 에너지 할당량에 도달한 경우, 사용자가 포그라운드에서 새 앱을 열기 위해 메모리를 다시 회수해야 하는 경우 등이 이에 해당합니다.

다음은 Revoked에 대한 이벤트 처리기 예제입니다.

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
[코드 샘플을 참조 하세요](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>Dispose

마지막 단계는 확장 실행 세션의 삭제입니다. 세션 닫힘 대기 중에 앱이 사용하는 에너지는 앱의 에너지 할당량에 따라 계산되기 때문에 해당 세션 및 그 밖에 메모리를 많이 사용하는 자산을 삭제하는 것이 좋습니다. 앱의 에너지 할당량을 가급적 충분하게 유지하려면 세션에 대한 작업 완료 시 해당 세션을 반드시 삭제해야 합니다. 그렇게 하면 앱이 **일시 중단** 상태로 더욱 신속하게 이동할 수 있습니다.

해지 이벤트를 기다리지 않고 직접 세션을 삭제하면 앱의 에너지 할당량 사용이 줄어듭니다. 즉, 사용 가능한 에너지 할당량이 늘어나기 때문에 향후 세션에서 앱의 백그라운드 실행을 더 오래 할 수 있게 됩니다. 작업이 끝날 때까지 **ExtendedExecutionSession** 개체에 대한 참조를 유지해야만 **Dispose** 메서드를 호출할 수 있습니다.

확장 실행 세션을 삭제하는 조각은 다음과 같습니다.

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
[코드 샘플을 참조 하세요](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

앱은 한 번에 단 하나의 **ExtendedExecutionSession**만 활성화할 수 있습니다. 저장소 또는 네트워크, 네트워크 기반 서비스와 같은 리소스에 액세스해야 하는 복잡한 작업을 완료하기 위해 비동기 작업을 사용하는 앱이 많습니다. 여러 비동기 작업의 완료가 필요한 작업일 경우 **ExtendedExecutionSession**을 삭제하고 앱을 일시 중지하기 전에 여러 비동기 작업 각각의 상태를 고려해야 합니다. 이를 위해서는 아직 실행 중인 작업의 수를 계산하는 참조가 필요하며, 그 값이 0이 될 때까지 세션을 삭제하지 않아야 합니다.

다음은 확장 실행 세션 기간 중 여러 작업을 관리하기 위한 몇 가지 예제 코드입니다. 앱에서 이를 사용하는 방법에 대한 자세한 내용은 아래 링크 코드 예제를 참조하십시오.

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
[코드 샘플을 참조 하세요](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>앱이 리소스를 제대로 사용하고 있는지 확인

앱의 메모리와 에너지 사용을 조정하는 일은 앱이 포그라운드에 있지 않아도 운영 체제에서 계속 실행될 수 있는지 확인하는 것이 관건입니다. [메모리 관리 API](https://docs.microsoft.com/uwp/api/windows.system.memorymanager)를 사용하면 앱에서 사용 중인 메모리의 양을 확인할 수 있습니다. 앱이 사용하는 메모리가 많을수록 다른 앱이 포그라운드에 있을 때 OS에서 앱을 계속 실행시키기가 어렵습니다. 사용자는 앱이 수행할 수 있는 모든 백그라운드 작업을 근본적으로 제어하며 앱이 배터리 사용에 미치는 영향을 한 눈에 볼 수 있게 됩니다.

[BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager)를 사용하면 사용자가 앱의 백그라운드 작업을 제한하기로 결정했는지 확인할 수 있습니다. 배터리 사용 정보를 파악하고 사용자가 원하는 작업을 완료해야 하는 경우에 백그라운드에서만 실행해야 합니다.

## <a name="see-also"></a>참조

[확장 된 실행 예제](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[응용 프로그램 수명 주기](https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle)  
[앱 수명 주기 - 앱을 백그라운드 작업 및 확장된 실행 활성 상태로 유지](https://msdn.microsoft.com/en-us/magazine/mt590969.aspx)
[백그라운드 메모리 관리](https://docs.microsoft.com/windows/uwp/launch-resume/reduce-memory-usage)  
[백그라운드 전송](https://docs.microsoft.com/windows/uwp/networking/background-transfers)  
[배터리 인식 및 백그라운드 작업](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[MemoryManager 클래스](https://docs.microsoft.com/uwp/api/windows.system.memorymanager)  
[백그라운드에서 미디어 재생](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)  