---
author: TylerMSFT
title: Windows10 UWP 앱 수명 주기
description: 이 항목에서는 활성화된 시점부터 닫힐 때까지의 Windows10 UWP(유니버설 Windows 플랫폼) 앱 수명 주기에 대해 설명합니다.
keywords: 앱 수명 주기 일시 중단됨 다시 시작 실행 활성화
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.author: twhitney
ms.date: 01/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cf8496393c5b500ab30d08608e90a0e156422ce3
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5926831"
---
# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>Windows10 UWP(유니버설 Windows 플랫폼) 앱 수명 주기


이 항목에서는 실행된 시점부터 닫힐 때까지의 UWP(유니버설 Windows 플랫폼) 앱 수명 주기에 대해 설명합니다.

## <a name="a-little-history"></a>배경 정보

Windows8 이전에는 앱에 간단한 수명 주기가 있었습니다. Win32 및 .NET 앱은 실행되거나 실행되지 않습니다. 사용자가 앱을 최소화하거나 벗어날 경우 계속 실행됩니다. 그래도 휴대용 장치와 전원 관리가 점점 중요해질 때까지는 문제가 되지 않았습니다.

Windows 8에서는 UWP 앱을 사용하는 새 응용 프로그램 모델이 도입되었습니다. 상위 수준에서 일시 중단 상태가 새로 추가되었습니다. 사용자가 앱을 최소화하거나 다른 앱으로 전환하면 즉시 UWP 앱이 일시 중단됩니다. 즉, 앱의 스레드가 중지되고 운영 체제에서 리소스를 확보해야 하는 경우가 아니면 앱이 메모리에 남아 있습니다. 사용자가 앱으로 다시 전환하면 실행 상태로 빠르게 복원할 수 있습니다.

백그라운드에 있는 경우에도 계속 실행해야 하는 앱을 위해 [백그라운드 작업](support-your-app-with-background-tasks.md), [확장 실행](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.aspx), 작업 후원 실행(예: 앱이 [백그라운드에서 미디어를 계속 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)할 수 있도록 하는 **BackgroundMediaEnabled** 접근 권한 값) 등 여러 가지 방법이 있습니다. 또한 앱이 일시 중단되거나 종료된 경우에도 백그라운드 전송 작업을 계속할 수 있습니다. 자세한 내용은 [파일을 다운로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer)을 참조하세요.

기본적으로 포그라운드에 없는 앱은 일시 중단되므로 전원이 절약되고 현재 포그라운드에 있는 앱에 더 많은 리소스를 사용할 수 있습니다.

운영 체제에서 리소스를 확보하기 위해 일시 중단된 앱을 종료할 수 있기 때문에 일시 중단 상태는 개발자에게 새로운 요구 사항을 추가합니다. 종료된 앱이 작업 표시줄에 계속 표시됩니다. 사용자는 시스템에서 앱을 닫은 것을 알지 못하기 때문에 앱을 클릭할 때 앱이 종료되기 전의 상태를 복원해야 합니다. 다른 작업을 수행하는 동안 앱이 백그라운드에서 대기 중이라고 생각하며 앱에서 전환할 때와 동일한 상태에 있을 것으로 예상합니다. 이 항목에서는 이 작업을 수행하는 방법을 설명합니다.

Windows10 버전 1607에서는 **Running in foreground** 및 **Running in background**라는 두 가지 앱 모델 상태가 추가로 도입되었습니다. 이후 섹션에서 이러한 새 상태에 대해서도 살펴보겠습니다.

## <a name="app-execution-state"></a>앱 실행 상태

이 그림은 Windows10 버전 1607부터 가능한 앱 모델 상태를 나타냅니다. UWP 앱의 일반적인 수명 주기를 살펴보겠습니다.

![앱 실행 상태의 전환을 보여 주는 상태 다이어그램](images/updated-lifecycle.png)

앱을 실행하거나 활성화하면 백그라운드에서 실행 상태가 됩니다. 포그라운드 앱이 실행되어 앱을 포그라운드로 이동해야 하는 경우 앱은 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 이벤트를 가져옵니다.

"실행됨"과 "활성화됨"은 비슷한 용어같지만 운영 체제에서 앱을 시작할 수 있는 다른 방식을 가리킵니다. 먼저 앱 실행을 살펴보겠습니다.

## <a name="app-launch"></a>앱 실행

앱을 실행하면 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드가 호출됩니다. 이 메서드에 전달되는 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 매개 변수는 앱에 전달되는 인수, 앱을 실행한 타일의 식별자, 앱의 이전 상태를 제공합니다.

[ApplicationExecutionState](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.applicationexecutionstate.aspx)를 반환하는 [LaunchActivatedEventArgs.PreviousExecutionState](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate)에서 앱의 이전 상태를 가져옵니다. 해당 값과 각 상태에 따라 수행해야 하는 적절한 작업은 다음과 같습니다.

| ApplicationExecutionState | 설명 | 수행할 작업 |
|-------|-------------|----------------|
| **NotRunning** | 마지막으로 사용자가 다시 부팅하거나 로그인한 후 앱을 실행하지 않은 경우 이 상태일 수 있습니다. 실행 중이었지만 충돌이 발생했거나 사용자가 이전에 앱을 닫은 경우에도 이 상태가 될 수 있습니다.| 현재 사용자 세션에서 처음으로 실행하는 것처럼 앱을 초기화합니다. |
|**Suspended** | 사용자가 앱을 최소화하거나 전환했으며 몇 초 내에 돌아오지 않았습니다. | 앱이 일시 중단된 경우 해당 상태가 메모리에 남아 있습니다. 앱이 일시 중단되었을 때 릴리스한 파일 핸들이나 기타 리소스를 다시 획득하기만 하면 됩니다. |
| **Terminated** | 앱이 이전에 일시 중단되었지만 시스템이 메모리를 확보하기 위해 일정 시점에서 종료되었습니다. | 사용자가 전환했을 때의 앱 상태를 복원합니다.|
|**ClosedByUser** | 사용자가 태블릿 모드에서 닫기 제스처를 사용하거나 Alt+F4를 사용하여 앱을 닫았습니다. 사용자가 앱을 닫으면 먼저 일시 중단된 다음 종료됩니다. | 기본적으로 앱이 Terminated 상태로 진행하는 동일한 단계를 거쳤기 때문에 Terminated 상태와 동일한 방식으로 처리합니다.|
|**Running** | 사용자가 앱을 다시 실행하려고 할 때 앱이 이미 열려 있었습니다. | 없음. 다른 앱 인스턴스가 실행되지 않았습니다. 이미 실행 중인 인스턴스가 활성화된 것뿐입니다. |

**참고** *현재 사용자 세션* 은 Windows 로그온 기준입니다. 현재 사용자가 Windows를 로그오프, 종료 또는 다시 시작하지 않는 한 잠금 화면 인증, 사용자 전환 등의 이벤트가 발생해도 현재 사용자 세션이 유지됩니다. 

주의해야 할 한 가지 중요한 상황은 디바이스에 충분한 리소스가 있을 경우 운영 체제에서 응답성을 최적화하기 위해 해당 동작을 옵트인한 자주 사용하는 앱을 사전 실행한다는 것입니다. 사전 실행된 앱은 백그라운드에서 실행된 후 바로 일시 중단되므로 사용자가 앱으로 전환할 경우 앱을 실행하는 것보다 더 빠르게 다시 시작할 수 있습니다.

사전 실행 때문에 사용자가 아니라 시스템에서 앱의 **OnLaunched()** 메서드를 시작할 수 있습니다. 앱이 백그라운드에서 사전 실행되므로 **OnLaunched()** 에서 다른 작업을 수행해야 할 수도 있습니다. 예를 들어 앱을 실행할 때 음악 재생이 시작되는 경우 앱이 백그라운드에서 사전 실행되었기 때문에 어디서 재생되는지 알 수 없습니다. 앱이 백그라운드에서 사전 실행되고 나면 **Application.Suspending**이 호출됩니다. 그런 다음 사용자가 앱을 실행할 때 **OnLaunched()** 메서드와 resuming 이벤트가 호출됩니다. 사전 실행 시나리오를 처리하는 방법에 대한 자세한 내용은 [앱 사전 실행 처리](handle-app-prelaunch.md)를 참조하세요. 옵트인한 앱만 사전 실행됩니다.

앱이 실행되면 Windows에서 앱 시작 화면을 표시합니다. 시작 화면을 구성하려면 [시작 화면 추가](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331)를 참조하세요.

시작 화면이 표시되는 동안 앱이 이벤트 처리기를 등록하고 초기 페이지에 필요한 사용자 지정 UI를 설정해야 합니다. **OnLaunched()** 및 응용 프로그램의 생성자에서 실행 중인 이러한 작업이 몇 초 내에 완료되도록 합니다. 완료되지 않으면 시스템에서 앱이 응답하지 않는다고 생각하고 종료할 수도 있습니다. 앱이 네트워크에서 데이터를 요청해야 하거나 디스크에서 대량의 데이터를 검색해야 하는 경우 이러한 작업이 실행 외의 기간에 완료되어야 합니다. 앱은 오래 실행되는 작업이 완료될 때까지 고유한 사용자 지정 로드 UI나 확장된 시작 화면을 사용할 수 있습니다. 자세한 내용은 [시작 화면을 더 오래 표시](create-a-customized-splash-screen.md) 및 [시작 화면 샘플](http://go.microsoft.com/fwlink/p/?linkid=234889)을 참조하세요.

앱 실행이 완료되면 **Running** 상태가 되고 시작 화면이 사라지며 모든 시작 화면 리소스와 개체가 지워집니다.

## <a name="app-activation"></a>앱 활성화

사용자가 실행하는 경우와 달리 시스템에서 앱을 활성화할 수 있습니다. 공유 계약 등의 계약을 통해 앱이 활성화될 수 있습니다. 또는 사용자 지정 URI 프로토콜이나 앱이 처리하도록 등록된 확장명을 가진 파일을 처리하기 위해 활성화될 수 있습니다. 앱이 활성화될 수 있는 방법 목록은 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693)를 참조하세요.

[**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 클래스는 앱이 활성화될 수 있는 다양한 방법을 처리하기 위해 재정의할 수 있는 메서드를 정의합니다.
[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)는 가능한 모든 활성화 유형을 처리할 수 있습니다. 그러나 가장 일반적인 활성화 유형 처리에는 특정 메서드를 사용하고 **OnActivated**는 덜 일반적인 활성화 유형을 위한 대체 메서드로 사용하는 것이 더 일반적입니다. 특정 활성화를 위한 추가 메서드는 다음과 같습니다.

[**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797)  
[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)  
[**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799)  [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801)  
[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336)  
[**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806)

이러한 메서드에 대한 이벤트 데이터에는 활성화되기 전의 앱 상태를 알려주는, 위에서 살펴본 것과 동일한 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 속성이 포함되어 있습니다. 위의 [앱 실행](#app-launch) 섹션에서 설명한 것과 동일한 방식으로 상태와 수행해야 하는 작업을 해석합니다.

**참고**컴퓨터의 관리자 계정을 사용 하 여 로그온 한 경우에 UWP 앱을 활성화할 수 없습니다.

## <a name="running-in-the-background"></a>백그라운드에서 실행 ##

Windows10, 버전 1607부터 앱은 앱 자체와 동일한 프로세스에서 백그라운드 작업을 실행할 수 있습니다. 자세한 내용은 [단일 프로세스 모델을 사용하는 백그라운드 작업](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)을 참조하세요. 이 문서에서는 In-process 백그라운드 처리를 다루지 않지만 앱 수명 주기에 미치는 영향은 앱이 백그라운드에 있는 경우와 관련된 두 가지 이벤트가 새로 추가되었다는 것입니다. 해당 이벤트는 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground)와 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)입니다.

이러한 이벤트는 사용자가 앱의 UI를 볼 수 있는지 여부도 반영합니다.

백그라운드에서 실행은 응용 프로그램이 실행, 활성화 또는 다시 시작되는 기본 상태입니다. 이 상태에서는 응용 프로그램 UI가 아직 표시되지 않습니다.

## <a name="running-in-the-foreground"></a>포그라운드에서 실행 ##

포그라운드에서 실행은 앱의 UI가 표시됨을 의미합니다.

**LeavingBackground** 이벤트는 응용 프로그램 UI가 표시되기 직전, 포그라운드에서 실행 상태로 전환되기 전에 발생합니다. 사용자가 앱으로 다시 전환하는 경우에도 발생합니다.

이전에는 UI 자산을 로드하기에 가장 적합한 위치가 **Activated** 또는 **Resuming** 이벤트 처리기였습니다. 이제 **LeavingBackground**에서 UI가 준비되었는지 확인하는 것이 가장 좋습니다.

응용 프로그램이 사용자에게 표시되기 전에 작업을 수행할 수 있는 마지막 기회이므로 이 시간까지 시각적 자산이 준비되었는지 확인하는 것이 중요합니다. 이 이벤트 처리기의 모든 UI 작업은 사용자가 경험하는 실행 및 다시 시작 시간에 영향을 주기 때문에 신속하게 완료되어야 합니다. **LeavingBackground** 시간까지 UI의 첫 번째 프레임이 준비되었는지 확인해야 합니다. 그런 다음 이벤트 처리기가 반환될 수 있도록 장기 실행 저장소 또는 네트워크 호출을 비동기적으로 처리해야 합니다.

사용자가 응용 프로그램에서 전환하면 앱이 다시 백그라운드에서 실행 상태가 됩니다.

## <a name="reentering-the-background-state"></a>백그라운드 상태 다시 시작

**EnteredBackground** 이벤트는 앱이 더 이상 포그라운드에 표시되지 않음을 나타냅니다. 데스크톱에서는 앱을 최소화할 때 **EnteredBackground**가 발생하고, 휴대폰에서는 홈 화면이나 다른 앱으로 전환할 때 발생합니다.

### <a name="reduce-your-apps-memory-usage"></a>앱의 메모리 사용량 줄이기

앱이 더 이상 사용자에게 표시되지 않으므로 여기서 UI 렌더링 작업 및 애니메이션을 중지하는 것이 가장 좋습니다. **LeavingBackground**를 사용하여 해당 작업을 다시 시작할 수 있습니다.

백그라운드에서 작업하려는 경우 여기서 준비해야 합니다. [MemoryManager.AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelevel.aspx)을 확인하고, 필요한 경우 시스템에서 리소스를 확보하기 위해 앱을 종료하지 않도록 백그라운드에서 실행될 때 앱에 사용되는 메모리 양을 줄이는 것이 가장 좋습니다.

자세한 내용은 [앱이 백그라운드 상태로 이동할 때 메모리 사용량 줄이기](reduce-memory-usage.md)를 참조하세요.

### <a name="save-your-state"></a>상태 저장

suspending 이벤트 처리기가 앱 상태를 저장하기에 가장 적합합니다. 그러나 백그라운드에서 작업하는 경우(예: 확장된 실행 세션이나 in-proc 백그라운드 작업을 사용하여 오디오 재생) **EnteredBackground** 이벤트 처리기에서 비동기적으로 데이터를 저장하는 것도 좋은 방법입니다. 앱이 백그라운드에서 우선 순위가 더 낮은 경우 종료될 수 있기 때문입니다. 또한 이 경우 앱이 일시 중단 상태를 거치지 않았기 때문에 데이터가 손실됩니다.

백그라운드 작업이 시작되기 전에 **EnteredBackground** 이벤트 처리기에서 데이터를 저장하면 사용자가 앱을 다시 포그라운드로 전환할 때 적절한 사용자 환경을 제공할 수 있습니다. 응용 프로그램 데이터 API를 사용하여 데이터와 설정을 저장할 수 있습니다. 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](https://msdn.microsoft.com/library/windows/apps/mt299098)을 참조하세요.

데이터를 저장한 후 메모리 사용량 제한을 초과할 경우 나중에 다시 로드할 수 있으므로 메모리에서 데이터를 해제할 수 있습니다. 그러면 백그라운드 작업에 필요한 자산에서 사용할 수 있는 메모리가 확보됩니다.

앱에 진행 중인 백그라운드 작업이 있는 경우 일시 중단 상태에 도달하지 않고 백그라운드에서 실행 상태에서 포그라운드에서 실행 상태로 이동할 수 있습니다.

### <a name="asynchronous-work-and-deferrals"></a>비동기 작업 및 지연

처리기 내에서 비동기 호출을 수행하면 컨트롤이 해당 비동기 호출에서 즉시 반환됩니다. 즉, 비동기 호출이 아직 완료되지 않은 경우에도 실행이 이벤트 처리기에서 반환될 수 있고 앱이 다음 상태로 이동합니다. 이벤트 처리기에 전달된 [**EnteredBackgroundEventArgs**](http://aka.ms/Ag2yh4) 개체의 [**GetDeferral**](http://aka.ms/Kt66iv) 메서드를 사용하여 반환된 [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 개체에서 [**Complete**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx) 메서드가 호출될 때까지 일시 중단을 지연할 수 있습니다.

지연을 사용해도 앱이 종료되기 전에 코드를 실행할 수 있는 시간이 증가하지는 않습니다. 단지 지연의 *Complete* 메서드 호출이나 기한 경과 중 *더 빠른 시간*까지 종료가 지연됩니다.

상태를 저장할 시간이 더 필요한 경우 **EnteredBackground** 이벤트 처리기에서 저장할 내용이 감소하도록 앱이 백그라운드 상태가 되기 전의 단계에서 상태를 저장할 방법을 조사합니다. 또는 더 많은 시간을 얻기 위해 [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx)을 요청할 수 있습니다. 그러나 요청이 승인된다는 보장이 없으므로 상태를 저장하는 데 필요한 시간을 최소화할 방법을 찾는 것이 가장 좋습니다.

## <a name="app-suspend"></a>앱 일시 중단

사용자가 앱을 최소화할 때 Windows는 사용자가 바로 앱으로 다시 전환하는지 확인하기 위해 몇 초간 기다립니다. 이 기간 내에 다시 전환하지 않으면 활성화된 확장 실행, 백그라운드 작업 또는 작업 후원 실행이 없을 경우 Windows에서 앱을 일시 중단합니다. 앱에서 활성화된 확장 실행 세션 등이 없는 한 잠금 화면이 나타나는 경우에도 앱이 일시 중단됩니다.

앱이 일시 중단되면 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 이벤트를 호출합니다. Visual Studio의 UWP 프로젝트 템플릿은 **App.xaml.cs**에 **OnSuspending**이라는 이 이벤트의 처리기를 제공합니다. Windows10 버전 1607 이전에는 여기에 상태를 저장하는 코드를 넣었습니다. 이제 위에서 설명한 대로 백그라운드 상태를 시작할 때 상태를 저장하는 것이 좋습니다.

앱이 일시 중단된 동안 다른 앱이 액세스할 수 있도록 단독 리소스와 파일 핸들을 해제해야 합니다. 단독 리소스의 예로는 카메라, I/O 디바이스, 외부 디바이스 및 네트워크 리소스가 있습니다. 단독 리소스와 파일 핸들을 명시적으로 해제하면 앱이 일시 중단된 동안 다른 앱이 액세스하는 데 도움이 됩니다. 앱이 다시 시작되면 단독 리소스와 파일 핸들을 다시 획득해야 합니다.

### <a name="be-aware-of-the-deadline"></a>기한 주의

빠르고 즉각적으로 응답하는 디바이스를 위해 suspending 이벤트 처리기에서 코드를 실행할 수 있는 시간이 제한됩니다. 시간 제한은 디바이스마다 다르며, 기한이라는 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 개체의 속성을 사용하여 확인할 수 있습니다.

**EnteredBackground** 이벤트 처리기와 마찬가지로, 처리기에서 비동기 호출을 수행하면 컨트롤이 해당 비동기 호출에서 즉시 반환됩니다. 즉, 비동기 호출이 아직 완료되지 않은 경우에도 실행이 이벤트 처리기에서 반환될 수 있고 앱이 일시 중단 상태로 이동합니다. 이벤트 인수를 통해 사용할 수 있는 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 개체의 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 메서드를 사용하여 반환된 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 개체에서 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 메서드가 호출될 때까지 일시 중단 상태 시작을 지연할 수 있습니다.

시간이 더 필요한 경우 [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx)을 요청할 수 있습니다. 그러나 요청이 승인된다는 보장이 없으므로 **Suspended** 이벤트 처리기에서 필요한 시간을 최소화할 방법을 찾는 것이 가장 좋습니다.

### <a name="app-terminate"></a>앱 종료

앱이 일시 중단된 동안 시스템은 앱과 데이터를 메모리에 유지하려고 합니다. 그러나 앱을 메모리에 유지할 리소스가 없으면 시스템에서 앱을 종료합니다. 앱은 종료된다는 알림을 받지 않으므로 앱 데이터를 저장할 수 있는 기회는 **OnSuspension** 이벤트 처리기뿐이거나 **EnteredBackground** 처리기에서 비동기적으로 저장해야 합니다.

앱이 종료된 후 활성화된 것을 확인하면 앱은 종료되기 전과 동일한 상태에 있도록 저장한 응용 프로그램 데이터를 로드해야 합니다. 사용자가 일시 중단 후 일시 중단된 앱으로 다시 돌아오면, 앱이 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드에서 응용 프로그램 데이터를 복원해야 합니다. 앱이 종료될 때 시스템에서 알리지 않으므로, 앱은 일시 중단되기 전에 응용 프로그램 데이터를 저장하고 단독 리소스와 파일 핸들을 해제한 다음 종료 후 앱이 활성화될 때 복원해야 합니다.

**Visual Studio를 사용하는 디버깅에 대한 참고 사항:** Visual Studio에서는 Windows가 디버거에 연결되어 있는 앱을 일시 중단하지 못하도록 합니다. 이렇게 하는 것은 앱이 실행되는 동안 Visual Studio 디버그 UI를 사용자가 볼 수 있도록 하기 위한 것입니다. 앱을 디버그할 때에는 Visual Studio를 사용하여 앱을 일시 중단 이벤트로 보낼 수 있습니다. **디버그 위치** 도구 모음이 표시되는지 확인한 다음 **일시 중단** 아이콘을 클릭합니다.

## <a name="app-resume"></a>앱 다시 시작

일시 중단된 앱은 사용자가 앱으로 다시 전환하거나 디바이스의 전원 부족 상태가 해소되어 활성 상태가 되면 다시 시작됩니다.

앱이 **Suspended** 상태에서 다시 시작될 경우 **Running in background** 상태가 되며, 계속 실행된 것처럼 사용자에게 표시되도록 시스템이 중단된 위치에서 앱을 복원합니다. 메모리에 저장된 앱 데이터는 손실되지 않습니다. 따라서 대부분의 앱은 다시 시작될 때 상태를 복원할 필요가 없지만 일시 중단될 때 해제한 파일 또는 디바이스 핸들을 다시 획득하고 앱이 일시 중단될 때 명시적으로 해제된 상태를 복원해야 합니다.

앱이 몇 시간이나 며칠 동안 일시 중단되었을 수 있습니다. 앱의 콘텐츠나 네트워크 연결이 최신 상태가 아닌 경우 다시 시작할 때 새로 고쳐야 합니다. 앱이 [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트에 대한 이벤트 처리기를 등록했다면 앱이 **Suspended** 상태에서 다시 시작될 경우 이 이벤트 처리기가 호출됩니다. 이 이벤트 처리기에서 앱 콘텐츠와 데이터를 새로 고칠 수 있습니다.

일시 중단된 앱이 앱 계약 또는 확장에 참여하기 위해 활성화되는 경우 **Resuming** 이벤트를 먼저 받은 다음 **Activated** 이벤트를 받습니다.

일시 중단된 앱이 종료된 경우 **Resuming** 이벤트가 없으며, 대신 **ApplicationExecutionState**를 **Terminated**로 지정하여 **OnLaunched()** 가 호출됩니다. 앱이 일시 중단될 때 상태를 저장했기 때문에 사용자가 앱에서 전환했을 때의 상태로 앱이 표시되도록 **OnLaunched()** 에서 해당 상태를 복원할 수 있습니다.

일시 중단된 동안에는 앱이 수신되도록 등록한 네트워크 이벤트를 받지 못합니다. 이러한 네트워크 이벤트는 대기하지 않으며, 그대로 누락됩니다. 따라서 앱이 다시 시작될 때 네트워크 상태를 테스트해야 합니다.

**참고** [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트는 UI 스레드에서 발생 되지, 때문에 다시 시작 처리기의 코드가 UI와 통신 하는 경우 디스패처를 사용 해야 합니다. 이 작업을 수행하는 방법의 코드 예제는 [백그라운드 스레드에서 UI 스레드 업데이트](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md)를 참조하세요.

일반적인 지침은 [앱 일시 중단 및 다시 시작에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465088)을 참조하세요.

## <a name="app-close"></a>앱 종료

일반적으로 사용자는 앱을 닫을 필요가 없으며 Windows에서 관리합니다. 그러나 사용자는 닫기 제스처를 사용하거나 Alt+F4를 누르거나 Windows Phone에서 작업 전환기를 사용하여 앱을 닫도록 선택할 수 있습니다.

사용자가 앱을 닫았음을 나타내는 이벤트는 없습니다. 사용자가 앱을 닫으면 먼저 일시 중단되어 상태를 저장할 수 있는 기회를 제공합니다. Windows8.1 이상, 사용자가 앱이 닫힌 후 앱 화면에서 제거 되 및 않고 전환 목록 확실히 종료 되지.

**사용자가 종료 동작:** 앱을 Windows에서 닫을 때 보다 사용자가 닫을 때와 다른 작업을 수행 해야 하는 경우 사용자 인지 Windows에서 앱을 종료 여부를 확인할 활성화 이벤트 처리기를 사용할 수 있습니다. [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 열거형에 대한 참조에서 **ClosedByUser** 및 **Terminated** 상태에 대한 설명을 참조하세요.

반드시 필요한 경우가 아니면 앱이 자동으로 닫히지 않도록 하는 것이 좋습니다. 예를 들어 앱이 메모리 누수를 발견한 경우 사용자의 개인 데이터 보안을 위해 스스로 닫을 수 있습니다.

## <a name="app-crash"></a>앱 크래시

시스템 크래시 환경은 사용자가 가능한 한 빨리 수행하던 작업으로 되돌아가도록 디자인되었습니다. 경고 대화 상자나 다른 알림은 사용자를 지체하게 하므로 제공하지 않아야 합니다.

앱이 중지되거나, 응답하지 않거나, 예외를 생성하는 경우 사용자의 [피드백 및 진단 설정](http://go.microsoft.com/fwlink/p/?LinkID=614828)별로 Microsoft에 문제 보고서가 전송됩니다. Microsoft는 문제 보고서의 오류 데이터 중 일부를 앱을 개선하는 데 사용할 수 있도록 귀하에게 제공합니다. 이 데이터는 대시보드의 앱 품질 페이지에서 볼 수 있습니다.

앱이 중지된 후 사용자가 앱을 활성화하면 앱의 활성화 이벤트 처리기는 **NotRunning**의 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 값을 수신하고 앱의 초기 UI 및 데이터를 표시해야 합니다. 충돌 후에는 데이터가 손상되었을 수 있으므로 **Suspended**에서 **Resuming**에 대해 평소처럼 앱 데이터를 사용하지 마세요. [앱 일시 중단 및 다시 시작에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465088)을 참조하세요.

## <a name="app-removal"></a>앱 제거

사용자가 앱을 삭제하면 모든 로컬 데이터와 함께 앱이 제거됩니다. 앱을 제거하더라도 문서 또는 사진 라이브러리 등 공통 위치에 저장되었던 사용자의 데이터에는 영향을 주지 않습니다.

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>앱 수명 주기 및 Visual Studio 프로젝트 템플릿

앱 수명 주기와 관련된 기본 코드는 Visual Studio 프로젝트 템플릿에서 제공됩니다. 기본 앱은 실행 활성화를 처리하고, 앱 데이터를 복원할 수 있는 위치를 제공하며, 사용자가 직접 코드를 추가하기도 전에 기본 UI를 표시합니다. 자세한 내용은 [앱용 C#, VB 및 C++ 프로젝트 템플릿](https://msdn.microsoft.com/library/windows/apps/hh768232)을 참조하세요.

## <a name="key-application-lifecycle-apis"></a>주요 응용 프로그램 수명 주기 API

-   [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 네임스페이스
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766) 네임스페이스
-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865) 네임스페이스
-   [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 클래스(XAML)
-   [**Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 클래스(XAML)

## <a name="related-topics"></a>관련 항목

* [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)
* [앱 일시 중단 및 다시 시작에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [앱 사전 실행 처리](handle-app-prelaunch.md)
* [앱 활성화 처리](activate-an-app.md)
* [앱 일시 중단 처리](suspend-an-app.md)
* [앱 다시 시작 처리](resume-an-app.md)
* [단일 프로세스 모델을 사용하는 백그라운드 작업](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [백그라운드에서 미디어 재생](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)

 

 
