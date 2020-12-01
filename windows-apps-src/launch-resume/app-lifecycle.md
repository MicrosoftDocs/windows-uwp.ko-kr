---
title: Windows 10 UWP 앱 수명 주기
description: 이 항목에서는 유니버설 Windows 플랫폼 Windows (UWP) 앱을 닫을 때까지 활성화 된 시간부터 수명 주기를 설명 합니다.
keywords: 앱 수명 주기 일시 중단 된 다시 시작 활성화
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.date: 01/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9e21d52567a8bc88fdb9f73ba1a960b5e6d24462
ms.sourcegitcommit: 25063560ff0a37fb404bc50e3b6e66759ee1051d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96420382"
---
# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>Windows 10 UWP (유니버설 Windows 플랫폼) 앱 수명 주기


이 항목에서는 유니버설 Windows 플랫폼 (UWP) 앱이 시작 될 때부터 닫힐 때 까지의 수명 주기를 설명 합니다.

## <a name="a-little-history"></a>적은 기록

Windows 8 이전에는 앱이 간단한 수명 주기를가지고 있습니다. Win32 및 .NET 앱이 실행 되 고 있거나 실행 되 고 있지 않습니다. 사용자가이를 최소화 하거나 다른 위치로 전환 하면 계속 실행 됩니다. 이는 휴대용 장치 및 전원 관리가 점점 더 중요할 때까지 문제가 되지 않습니다.

Windows 8에는 UWP 앱과 함께 새로운 응용 프로그램 모델이 도입 되었습니다. 높은 수준에서 새 일시 중단 됨 상태가 추가 되었습니다. 사용자가 최소화 하거나 다른 앱으로 전환 하면 UWP 앱이 즉시 일시 중단 됩니다. 즉, 운영 체제에서 리소스를 회수 해야 할 경우를 제외 하 고 앱의 스레드가 중지 되 고 앱이 메모리에 남아 있습니다. 사용자가 다시 앱으로 전환 하면 실행 중 상태로 신속 하 게 복원할 수 있습니다.

백그라운드 [작업](support-your-app-with-background-tasks.md), [확장 실행](/uwp/api/windows.applicationmodel.extendedexecution)및 활동 후원 실행 (예: 앱이 [백그라운드에서 미디어](../audio-video-camera/background-audio.md)를 계속 재생할 수 있도록 허용 하는 **BackgroundMediaEnabled** 기능)과 같은 백그라운드에 있을 때 응용 프로그램을 계속 실행 해야 하는 다양 한 방법이 있습니다. 또한 앱이 일시 중단 되거나 심지어 종료 되더라도 백그라운드 전송 작업을 계속할 수 있습니다. 자세한 내용은 [파일을 다운로드 하는 방법](/previous-versions/windows/apps/jj152726(v=win.10))을 참조 하세요.

기본적으로 포그라운드에 없는 앱은 일시 중단되므로 전원이 절약되고 현재 포그라운드에 있는 앱에 더 많은 리소스를 사용할 수 있습니다.

일시 중단 된 상태는 리소스를 확보 하기 위해 운영 체제가 일시 중단 된 앱을 종료 하도록 선택할 수 있기 때문에 개발자에 게 새로운 요구 사항을 추가 합니다. 종료 된 앱은 작업 표시줄에 계속 표시 됩니다. 사용자가 앱을 클릭 하면 앱이 종료 되기 전에 있던 상태를 복원 해야 합니다. 사용자는 시스템이 앱을 닫은 것을 인식 하지 못합니다. 이러한 작업은 다른 작업을 수행 하는 동안 백그라운드에서 대기 중 이었던 것으로 간주 하며, 남은 상태와 동일한 상태가 될 것으로 예상 합니다. 이 항목에서는이를 수행 하는 방법을 살펴보겠습니다.

Windows 10 버전 1607에는 두 가지 추가 앱 모델 상태 ( **포그라운드로 실행 되** 고 **백그라운드에서 실행**)가 도입 되었습니다. 또한 다음 섹션에서 이러한 새 상태를 살펴보겠습니다.

## <a name="app-execution-state"></a>앱 실행 상태

이 그림은 Windows 10 버전 1607부터 사용할 수 있는 앱 모델 상태를 나타냅니다. UWP 앱의 일반적인 수명 주기를 살펴보겠습니다.

![앱 실행 상태 간의 전환을 보여 주는 상태 다이어그램](images/updated-lifecycle.png)

앱이 시작 되거나 활성화 될 때 백그라운드 상태에서 실행 중인를 입력 합니다. 포그라운드 앱 시작으로 인해 앱이 포그라운드로 이동 해야 하는 경우 앱은 [**Leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 이벤트를 가져옵니다.

"시작 됨" 및 "활성화 됨"은 비슷한 용어 처럼 보일 수 있지만 운영 체제에서 앱을 시작할 수 있는 다양 한 방법을 참조 합니다. 먼저 앱을 시작 하는 방법을 살펴보겠습니다.

## <a name="app-launch"></a>앱 시작

[**Onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 된 메서드는 앱이 시작 될 때 호출 됩니다. 응용 프로그램에 전달 되는 인수, 앱을 시작한 타일의 식별자 및 앱이 있었던 이전 상태를 제공 하는 [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 매개 변수가 전달 됩니다.

[Applicationexecutionstate](/uwp/api/windows.applicationmodel.activation.applicationexecutionstate)를 반환 하는 LaunchActivatedEventArgs에서 앱의 이전 상태를 가져옵니다 [.](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate) 해당 값 및 해당 상태로 인해 수행할 적절 한 작업은 다음과 같습니다.

| ApplicationExecutionState | 설명 | 수행할 작업 |
|-------|-------------|----------------|
| **실행 중 아님** | 앱은 사용자가 마지막으로 다시 부팅 하거나 로그인 한 이후 실행 되지 않았기 때문에이 상태가 될 수 있습니다. 또한 실행 중이 고 충돌 하는 경우 또는 사용자가 이전에 닫은 경우에도이 상태가 될 수 있습니다.| 현재 사용자 세션에서를 처음 실행 하는 것 처럼 앱을 초기화 합니다. |
|**일시 중지됨** | 사용자가 앱에서 최소화 또는 전환 되 고 몇 초 내에 반환 되지 않았습니다. | 앱이 일시 중단 되 면 상태는 메모리에 남아 있습니다. 앱이 일시 중단 되었을 때 릴리스된 파일 핸들이 나 기타 리소스를 다시 얻어야 합니다. |
| **종료됨** | 앱이 이전에 일시 중단 되었지만 시스템이 메모리를 회수 하는 데 필요 하기 때문에 특정 시점에 종료 되었습니다. | 사용자가 외부에서 전환할 때 앱이 있었던 상태를 복원 합니다.|
|**ClosedByUser** | 사용자가 태블릿 모드에서 닫기 제스처를 사용 하거나 Alt + F4를 사용 하 여 앱을 닫았습니다. 사용자가 앱을 닫으면 먼저 일시 중단 된 다음 종료 됩니다. | 기본적으로 앱이 Terminated 상태로 진행하는 동일한 단계를 거쳤기 때문에 Terminated 상태와 동일한 방식으로 처리합니다.|
|**실행 중** | 사용자가 앱을 다시 시작 하려고 할 때 앱이 이미 열려 있습니다. | 아무 일도 일어나지 않습니다. 앱의 다른 인스턴스가 시작 되지 않습니다. 이미 실행 중인 인스턴스는 단순히 활성화 됩니다. |

**참고**  *현재 사용자 세션* 은 Windows 로그온을 기반으로 합니다. 현재 사용자가 로그 오프 하거나 종료 하거나 Windows를 다시 시작 하지 않은 경우에는 현재 사용자 세션이 잠금 화면 인증, 스위치 사용자 등의 여러 이벤트에서 지속 됩니다. 

중요 한 한 가지 중요 한 점은 장치에 충분 한 리소스가 있는 경우 운영 체제에서 응답성을 최적화 하기 위해 해당 동작에 옵트인 한 자주 사용 하는 앱을 사전 설치 하는 것입니다. 사전에 시작 된 앱은 백그라운드에서 시작 된 후 신속 하 게 일시 중단 되므로 사용자가 해당 앱으로 전환할 때 앱을 시작 하는 것 보다 더 빠르게 다시 시작할 수 있습니다.

사전 실행으로 인해 사용자가 아닌 시스템에서 앱의 **onlaunched ()** 메서드를 시작할 수 있습니다. 앱이 백그라운드에서 사전 실행 되기 때문에 **Onlaunched ()** 에서 다른 작업을 수행 해야 할 수도 있습니다. 예를 들어 앱이 시작 될 때 음악의 재생을 시작 하는 경우 앱이 백그라운드에서 사전 실행 되기 때문에 앱이 어디에 있는지 알 수 없습니다. 앱이 백그라운드에서 사전 시작 되 면 응용 프로그램을 호출 합니다 **. 일시 중단** 합니다. 그런 다음 사용자가 앱을 시작할 때 다시 시작 하는 이벤트와 **onlaunched ()** 메서드를 호출 합니다. 사전 처리 시나리오를 처리 하는 방법에 대 한 자세한 내용은 [앱 사전 처리 처리](handle-app-prelaunch.md) 를 참조 하세요. 옵트인 (opt in) 된 앱만 사전에 실행 됩니다.

앱이 시작 되 면 시작 화면이 표시 됩니다. 시작 화면을 구성 하려면 [시작 화면 추가](/previous-versions/windows/apps/hh465331(v=win.10))를 참조 하세요.

시작 화면이 표시 되는 동안 앱에서 이벤트 처리기를 등록 하 고 초기 페이지에 필요한 사용자 지정 UI를 설정 해야 합니다. 응용 프로그램의 생성자 및 **onlaunched ()** 에서 실행 되는 이러한 작업은 몇 초 내에 완료 되거나 시스템에서 앱이 응답 하지 않는 것으로 간주 되 고 종료 될 수 있습니다. 앱이 네트워크에서 데이터를 요청 해야 하거나 디스크에서 많은 양의 데이터를 검색 해야 하는 경우 이러한 작업은 시작 외부에서 완료 해야 합니다. 앱은 장기 실행 작업이 완료 될 때까지 대기 하는 동안 자체 사용자 지정 로드 UI 또는 확장 된 시작 화면을 사용할 수 있습니다. 자세한 내용은 [시작 화면 표시](create-a-customized-splash-screen.md) 및 [시작 화면 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/SplashScreen) 을 참조 하세요.

앱이 시작 되 면 **실행 중** 상태로 전환 되 고 시작 화면이 사라지고 시작 화면 리소스 및 개체가 모두 지워집니다.

## <a name="app-activation"></a>앱 활성화

사용자가 시작 하는 것과 달리 시스템에서 앱을 활성화할 수 있습니다. 공유 계약과 같은 계약에 따라 앱을 활성화할 수 있습니다. 또는 응용 프로그램에서 처리 하기 위해 등록 된 확장 프로그램을 사용 하 여 사용자 지정 URI 프로토콜 또는 파일을 처리 하도록 활성화할 수 있습니다. 앱을 활성화할 수 있는 방법 목록은 [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)를 참조 하세요.

[**응용 프로그램**](/uwp/api/Windows.UI.Xaml.Application) 을 활성화 하는 다양 한 방법을 처리 하기 위해 재정의할 수 있는 메서드를 정의 합니다.
[**Onactivated**](/uwp/api/windows.ui.xaml.application.onactivated) 은 가능한 모든 활성화 형식을 처리할 수 있습니다. 그러나 가장 일반적인 활성화 형식을 처리 하는 데 특정 메서드를 사용 하는 것이 일반적 이며, 일반 정품 인증 형식에 대 한 대체 방법으로 **onactivated** 을 사용 하는 것이 더 일반적입니다. 다음은 특정 활성화의 추가 방법입니다.

[**OnCachedFileUpdaterActivated**](/uwp/api/windows.ui.xaml.application.oncachedfileupdateractivated)  
[**OnFileActivated 됨**](/uwp/api/windows.ui.xaml.application.onfileactivated)  
[**OnfileopenOnFileSavePickerActivated ker활성화**](/uwp/api/windows.ui.xaml.application.onfileopenpickeractivated)된 [**OnFileSavePickerActivated**](/uwp/api/windows.ui.xaml.application.onfilesavepickeractivated)    
[**OnSearchActivated 됨**](/uwp/api/windows.ui.xaml.application.onsearchactivated)  
[**OnShareTargetActivated**](/uwp/api/windows.ui.xaml.application.onsharetargetactivated)

이러한 메서드에 대 한 이벤트 데이터에는 위에서 확인 한 것과 동일한  [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) 속성이 포함 되어 있습니다 .이 속성은 앱이 활성화 되기 전에 있었던 상태를 알려 줍니다. [앱 시작](#app-launch) 섹션에서 위에 설명 된 것과 동일한 방식으로 상태를 해석 합니다.

**참고** 컴퓨터의 관리자 계정을 사용 하 여 로그온 하는 경우 UWP 앱을 활성화할 수 없습니다.

## <a name="running-in-the-background"></a>백그라운드에서 실행 ##

Windows 10, 버전 1607부터 앱은 앱 자체와 동일한 프로세스 내에서 백그라운드 작업을 실행할 수 있습니다. [단일 프로세스 모델을 사용 하 여 백그라운드 작업](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)에서 자세히 알아보세요. 이 문서에서는 in-process 백그라운드 처리에 영향을 주지 않지만 앱 수명 주기에 영향을 주는 방법은 앱이 백그라운드에 있는 경우와 관련 된 두 개의 새로운 이벤트가 추가 되었다는 것입니다. 사용자 [**는 다음을 수행 합니다**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) . [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)

이러한 이벤트는 사용자가 앱의 UI를 볼 수 있는지도 반영 합니다.

백그라운드에서 실행 하는 것은 응용 프로그램이 시작 되거나 활성화 되거나 다시 시작 되는 기본 상태입니다. 이 상태에서는 응용 프로그램 UI가 아직 표시 되지 않습니다.

## <a name="running-in-the-foreground"></a>포그라운드로 실행 ##

전경에서를 실행 하는 것은 앱의 UI가 표시 됨을 의미 합니다.

**Leavingbackground** 이벤트는 응용 프로그램 UI가 표시 되기 직전에 실행 되 고를 포그라운드 상태로 들어가기 전에 발생 합니다. 사용자가 앱으로 다시 전환 하는 경우에도 발생 합니다.

이전에는 UI 자산을 로드 하는 가장 좋은 위치가 **활성화** 되거나 다시 **시작** 된 이벤트 처리기에 있었습니다. 이제 **Leavingbackground** 은 UI가 준비 되었는지 확인 하는 가장 좋은 장소입니다.

응용 프로그램이 사용자에 게 표시 되기 전에 작업을 수행할 수 있는 마지막 기회 이기 때문에 이번에는 시각적 자산이 준비 되었는지 확인 하는 것이 중요 합니다. 이 이벤트 처리기의 모든 UI 작업은 사용자가 경험 하는 시작 및 다시 시작 시간에 영향을 주므로 신속 하 게 완료 되어야 합니다. **Leavingbackground** 는 UI의 첫 번째 프레임이 준비 되었는지 확인 하는 시간입니다. 그런 다음 이벤트 처리기가를 반환할 수 있도록 장기 실행 저장소 또는 네트워크 호출을 비동기식으로 처리 해야 합니다.

사용자가 응용 프로그램에서 외부로 전환할 때 앱은 백그라운드 상태에서 실행을 후 합니다.

## <a name="reentering-the-background-state"></a>배경 상태를

**Enteredbackground** 이벤트는 앱이 더 이상 포그라운드에서 표시 되지 않음을 나타냅니다. **바탕 화면** 에서 앱이 최소화 되 면이 이벤트가 발생 합니다. 휴대폰에서 홈 화면 또는 다른 앱으로 전환 하는 경우

### <a name="reduce-your-apps-memory-usage"></a>앱의 메모리 사용량을 줄입니다.

앱이 더 이상 사용자에 게 표시 되지 않으므로 UI 렌더링 작업 및 애니메이션을 중지 하는 것이 가장 좋은 위치입니다. **Leavingbackground** 를 사용 하 여 해당 작업을 다시 시작할 수 있습니다.

백그라운드에서 작업을 수행 하려는 경우이 작업을 준비 하는 것이 좋습니다. AppMemoryUsageLevel를 확인 하 고, 필요한 경우 응용 프로그램이 백그라운드에서 실행 될 때 응용 프로그램에서 사용 되는 메모리의 양을 줄여 응용 프로그램이 리소스를 확보 하기 위해 시스템에 의해 종료 될 위험이 없도록 하는 것이 가장 좋습니다 [.](/uwp/api/windows.system.memorymanager.appmemoryusagelevel)

자세한 내용은 [앱이 백그라운드 상태로 이동할 때 메모리 사용 감소](reduce-memory-usage.md) 를 참조 하세요.

### <a name="save-your-state"></a>상태 저장

일시 중단 이벤트 처리기는 앱 상태를 저장 하기에 가장 적합 한 위치입니다. 그러나 백그라운드에서 작업을 수행 하는 경우 (예: 확장 된 실행 세션이 나 **in-process 백그라운드 작업** 을 사용 하 여) 백그라운드에서 데이터를 비동기적으로 저장 하는 것도 좋은 방법입니다. 앱이 백그라운드에서 낮은 우선 순위로 종료 될 수 있기 때문입니다. 이 경우에는 앱이 일시 중단 된 상태를 거치지 않기 때문에 데이터가 손실 됩니다.

사용자가 응용 프로그램을 포그라운드로 다시 가져올 때 사용자가 응용 프로그램을 다시 시작 하기 전에 사용자가 응용 프로그램을 다시 시작 하기 전에 사용자 **가 원하는 대로** 데이터를 저장 하는 것이 좋습니다. 응용 프로그램 데이터 Api를 사용 하 여 데이터 및 설정을 저장할 수 있습니다. 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)을 참조하세요.

데이터를 저장 한 후 메모리 사용 제한을 초과 하는 경우 나중에 다시 로드할 수 있으므로 메모리에서 데이터를 해제할 수 있습니다. 그러면 백그라운드 작업에 필요한 자산에서 사용할 수 있는 메모리가 확보 됩니다.

앱이 백그라운드 작업을 진행 중인 경우 백그라운드 상태에서 실행 중인에서 일시 중단 된 상태에 도달 하지 않고 포그라운드 상태에서 실행 중인로 이동할 수 있습니다.

### <a name="asynchronous-work-and-deferrals"></a>비동기 작업 및 지연과

처리기 내에서 비동기 호출을 수행 하는 경우 해당 비동기 호출에서 제어가 즉시 반환 됩니다. 즉, 실행은 이벤트 처리기에서 반환 될 수 있으며 비동기 호출이 아직 완료 되지 않은 경우에도 앱이 다음 상태로 이동 됩니다. 반환 된 [**GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) 개체에서 [**Complete**](/uwp/api/windows.foundation.deferral.complete) 메서드를 호출할 때까지 일시 중단을 지연 시키려면 이벤트 처리기에 전달 되는 [**EnteredBackgroundEventArgs**](/uwp/api/Windows.ApplicationModel) 개체의 메서드를 사용 [**합니다.**](/uwp/api/windows.foundation.deferral)

지연이 발생 하면 앱이 종료 되기 전에 코드를 실행 해야 하는 양이 증가 하지 않습니다. 이는 지연의 *Complete* 메서드를 호출 *하거나 최종 기한* 에 도달할 때까지 종료를 지연 합니다.

상태를 저장 하는 데 시간이 더 필요한 경우 앱이 백그라운드 상태를 들어가기 전에 상태를 저장 하는 방법을 조사 하 여 사용자의 **배경** 이벤트 처리기에 저장 하는 것이 더 떨어질 수 있습니다. 또는 [ExtendedExecutionSession](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution) 를 요청 하 여 더 많은 시간을 얻을 수 있습니다. 그러나 요청을 부여 하는 것은 보장 되지 않으므로 상태를 저장 하는 데 필요한 시간을 최소화 하는 방법을 찾는 것이 가장 좋습니다.

## <a name="app-suspend"></a>앱 일시 중단

사용자가 앱을 최소화 하면 몇 초 후에 사용자가 다시 전환 하는지 확인 합니다. 이 기간 내에 다시 전환 되지 않고 확장 된 실행, 백그라운드 작업 또는 활동으로 후원 된 실행이 활성화 되어 있지 않으면 Windows에서 앱을 일시 중단 합니다. 응용 프로그램은 해당 앱에서 확장 된 실행 세션 등이 활성화 되어 있지 않은 경우 잠금 화면이 표시 되는 경우에도 일시 중단 됩니다.

앱이 일시 중단 되 면 응용 프로그램을 호출 합니다. 이벤트를 [**일시 중단**](/uwp/api/windows.ui.xaml.application.suspending) 합니다. Visual Studio의 UWP 프로젝트 템플릿은 **App.xaml.cs** 에서 **onsuspending 중단** 이라는이 이벤트에 대 한 처리기를 제공 합니다. Windows 10, 버전 1607 이전에는 여기에 상태를 저장 하는 코드를 저장 합니다. 이제 위에서 설명한 대로 배경 상태를 입력할 때 상태를 저장 하는 것이 좋습니다.

앱이 일시 중단 된 상태에서 다른 앱이 액세스할 수 있도록 배타 리소스 및 파일 핸들을 해제 해야 합니다. 전용 리소스의 예로는 카메라, i/o 장치, 외부 장치 및 네트워크 리소스가 있습니다. 배타적 리소스 및 파일 핸들을 명시적으로 해제 하면 앱이 일시 중단 된 동안 다른 앱에서 해당 리소스에 액세스할 수 있습니다. 앱이 다시 시작 되 면 단독 리소스 및 파일 핸들을 다시 가져와야 합니다.

### <a name="be-aware-of-the-deadline"></a>최종 기한 파악

신속 하 고 응답성이 뛰어난 장치를 보장 하기 위해 일시 중단 이벤트 처리기에서 코드를 실행 하는 데 필요한 시간에 제한이 있습니다. 각 장치 마다 다르며 최종 기한 이라는 [**SuspendingOperation**](/uwp/api/Windows.ApplicationModel.SuspendingOperation) 개체의 속성을 사용 하는 것을 확인할 수 있습니다.

사용자가 처리기에서 비동기 호출을 수행 하는 경우에는 사용자 지정 **Edbackground** 이벤트 처리기와 마찬가지로 해당 비동기 호출에서 제어가 즉시 반환 됩니다. 즉, 실행은 이벤트 처리기에서 반환 될 수 있으며 비동기 호출이 아직 완료 되지 않은 경우에도 앱은 일시 중단 상태로 전환 됩니다. 반환 된 [**SuspendingDeferral**](/uwp/api/Windows.ApplicationModel.SuspendingDeferral) 개체에서 [**Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete) 메서드를 호출할 때까지 [**SuspendingOperation**](/uwp/api/Windows.ApplicationModel.SuspendingOperation) 개체 (이벤트 인수를 통해 사용 가능)에서 [**GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) 메서드를 사용 하 여 일시 중단 된 상태를 지연 시킬 수 있습니다.

시간이 더 필요한 경우 [ExtendedExecutionSession](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution)를 요청할 수 있습니다. 그러나 요청을 부여 하는 것은 보장 되지 않으므로 **일시 중단** 된 이벤트 처리기에서 필요한 시간을 최소화 하는 방법을 찾는 것이 가장 좋습니다.

### <a name="app-terminate"></a>앱 종료

시스템은 일시 중단 된 상태에서 앱과 해당 데이터를 메모리에 유지 하려고 합니다. 그러나 시스템에 앱을 메모리에 보관 하는 리소스가 없으면 앱이 종료 됩니다. 앱은 종료 되는 알림을 받지 않으므로 앱의 데이터를 저장 하는 유일한 기회는 **Onsuspending 중단** 이벤트 처리기에 있거나 사용자의 사용자의 **백그라운드** 처리기에서 비동기적으로 저장 하는 것입니다.

앱이 종료 된 후에 활성화 된 것으로 확인 되 면 앱이 종료 되기 전에 있던 응용 프로그램 데이터를 로드 해야 합니다. 사용자가 종료 된 일시 중단 된 앱으로 다시 전환 되 면 앱은 해당 응용 프로그램 데이터를 해당 [**Onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 된 메서드로 복원 해야 합니다. 시스템이 종료 될 때 앱에 알리지 않으므로 앱이 일시 중단 되기 전에 응용 프로그램 데이터를 저장 하 고 전용 리소스 및 파일 핸들을 해제 한 후 앱이 종료 된 후에 활성화 될 때이를 복원 해야 합니다.

**Visual Studio를 사용한 디버깅에 대 한 참고 사항:** Visual Studio는 Windows에서 디버거에 연결 된 앱을 일시 중단 하지 못하도록 합니다. 이는 앱이 실행 되는 동안 사용자가 Visual Studio 디버그 UI를 볼 수 있도록 하기 위한 것입니다. 앱을 디버깅 하는 경우 Visual Studio를 사용 하 여 일시 중단 이벤트를 보낼 수 있습니다. **디버그 위치** 도구 모음이 표시 되는지 확인 하 고 **일시 중지** 아이콘을 클릭 합니다.

## <a name="app-resume"></a>앱 다시 시작

일시 중단 된 앱은 사용자가 전환 될 때 또는 장치가 전원이 부족 한 상태에서 들어오는 앱 인 경우 다시 시작 됩니다.

**일시 중단** 된 상태에서 앱이 다시 시작 되 면 백그라운드 상태에서 **실행 중** 으로 전환 되 고 시스템은 사용자에 게 표시 되도록 앱을 복원 합니다. 메모리에 저장 된 앱 데이터가 손실 되지 않습니다. 따라서 대부분의 앱은 일시 중단 될 때 릴리스된 파일 또는 장치 핸들을 다시 가져와야 하지만 앱이 일시 중단 될 때 명시적으로 릴리스된 모든 상태를 복원 해야 하는 경우 다시 시작 될 때 상태를 복원할 필요가 없습니다.

앱이 몇 시간 또는 며칠 동안 일시 중단 될 수 있습니다. 앱이 오래 된 상태로 있을 수 있는 콘텐츠 또는 네트워크 연결을 포함 하는 경우 앱을 다시 시작할 때 새로 고쳐야 합니다. 앱이 [**응용 프로그램**](/uwp/api/windows.ui.xaml.application.resuming) 에 대 한 이벤트 처리기를 등록 한 경우 이벤트를 다시 시작 하면 **일시 중단** 된 상태에서 앱이 다시 시작 될 때 호출 됩니다. 이 이벤트 처리기에서 앱 콘텐츠 및 데이터를 새로 고칠 수 있습니다.

일시 중단 된 앱이 앱 계약 또는 확장에 참여 하도록 활성화 된 경우 다시 **시작** 이벤트를 먼저 받은 다음 **활성화** 된 이벤트를 수신 합니다.

일시 중단 된 앱이 종료 된 경우 다시 **시작** 이벤트가 없으며 대신 **Applicationexecutionstate** 가 **종료** 된 상태에서 **onlaunched ()** 이 호출 됩니다. 앱이 일시 중단 되었을 때 상태를 저장 했으므로 **onlaunched ()** 중에 해당 상태를 복원 하 여 앱이 사용자에 게 표시 되지 않는 것 처럼 사용자에 게 표시 되도록 할 수 있습니다.

앱은 일시 중단 되지만 수신 하도록 등록 된 네트워크 이벤트는 수신 되지 않습니다. 이러한 네트워크 이벤트는 큐에 저장 되지 않으며 단순히 누락 됩니다. 따라서 앱이 다시 시작 될 때 네트워크 상태를 테스트 해야 합니다.

**참고**  다시 시작 [**이벤트는**](/uwp/api/windows.ui.xaml.application.resuming) ui 스레드에서 발생 하지 않기 때문에 resume 처리기의 코드가 ui와 통신할 경우 디스패처를 사용 해야 합니다. 이 작업을 수행 하는 방법에 대 한 코드 예제는 [백그라운드 스레드에서 UI 스레드 업데이트](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md) 를 참조 하세요.

일반적인 지침은 [앱 일시 중단 및 다시 시작에 대 한 지침](./index.md)을 참조 하세요.

## <a name="app-close"></a>앱 닫기

일반적으로 사용자는 앱을 닫을 필요가 없으며,이를 통해 Windows에서 앱을 관리할 수 있습니다. 그러나 사용자는 닫기 제스처를 사용 하거나 Alt + F4 키를 누르거나 Windows Phone에서 작업 전환기를 사용 하 여 앱을 닫을 수 있습니다.

사용자가 앱을 닫 았 음을 나타내는 이벤트가 없습니다. 사용자가 앱을 닫으면 해당 상태를 저장할 수 있는 기회를 제공 하기 위해 먼저 일시 중단 됩니다. Windows 8.1 이상에서 사용자가 앱을 닫은 후에는 앱이 화면에서 제거 되 고 목록으로 전환 되지만 명시적으로 종료 되지는 않습니다.

**사용자가 닫은 동작:**  응용 프로그램이 Windows에서 닫힐 때가 아닌 다른 작업을 수행 해야 하는 경우에는 활성화 이벤트 처리기를 사용 하 여 앱이 사용자에 의해 종료 되었는지 또는 Windows에서 종료 되었는지 확인할 수 있습니다. [**Applicationexecutionstate**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) 열거에 대 한 참조에서 **closed 사용자** 및 **종료** 된 상태에 대 한 설명을 참조 하세요.

반드시 필요한 경우가 아니면 앱을 프로그래밍 방식으로 닫지 않는 것이 좋습니다. 예를 들어 앱에서 메모리 누수를 검색 하는 경우 사용자의 개인 데이터에 대 한 보안을 유지 하기 위해 자신을 닫을 수 있습니다.

## <a name="app-crash"></a>앱 충돌

시스템 충돌 환경은 최대한 빠르게 수행 하는 작업을 사용자에 게 제공 하도록 설계 되었습니다. 경고 대화 상자 또는 다른 알림은 사용자를 지연 하므로 제공 하면 안 됩니다.

앱이 충돌 하거나, 응답을 중지 하거나, 예외를 생성 하는 경우 사용자의 [피드백 및 진단 설정](https://support.microsoft.com/help/4468236/diagnostics-feedback-and-privacy-in-windows-10-microsoft-privacy)에 따라 문제 보고서가 Microsoft에 전송 됩니다. Microsoft는이를 사용 하 여 앱을 향상 시킬 수 있도록 문제 보고서의 일부 오류 데이터를 제공 합니다. 대시보드의 앱 품질 페이지에서이 데이터를 볼 수 있습니다.

사용자가 충돌 후에 앱을 활성화 하면 해당 활성화 이벤트 처리기는 **NotRunning** 의 [**applicationexecutionstate**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) 값을 수신 하 고 초기 UI와 데이터를 표시 해야 합니다. 충돌이 발생 한 후에는 해당 데이터가 손상 될 수 있으므로 **일시 중단** 된 상태에서 다시 **시작** 하는 데 사용한 앱 데이터를 정기적으로 사용 하지 마세요. [앱 일시 중단 및 다시 시작에 대 한 지침](./index.md)을 참조 하세요.

## <a name="app-removal"></a>앱 제거

사용자가 앱을 삭제 하면 해당 로컬 데이터와 함께 앱이 제거 됩니다. 앱을 제거 하면 문서 또는 사진 라이브러리와 같은 공통 위치에 저장 된 사용자의 데이터에 영향을 주지 않습니다.

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>앱 수명 주기 및 Visual Studio 프로젝트 템플릿

앱 수명 주기와 관련 된 기본 코드는 Visual Studio 프로젝트 템플릿에서 제공 됩니다. 기본 앱은 시작 활성화를 처리 하 고, 앱 데이터를 복원할 수 있는 장소를 제공 하 고, 사용자 고유의 코드를 추가 하기 전에도 기본 UI를 표시 합니다. 자세한 내용은 [앱 용 c #, VB 및 c + + 프로젝트 템플릿](/previous-versions/windows/apps/hh768232(v=win.10))을 참조 하세요.

## <a name="key-application-lifecycle-apis"></a>주요 응용 프로그램 수명 주기 Api

-   [**Windows ApplicationModel**](/uwp/api/Windows.ApplicationModel) 네임 스페이스
-   [**Windows ApplicationModel. Activation**](/uwp/api/Windows.ApplicationModel.Activation) 네임 스페이스
-   [**Windows. ApplicationModel. Core**](/uwp/api/Windows.ApplicationModel.Core) 네임 스페이스
-   [**Windows. .xaml. 응용 프로그램**](/uwp/api/Windows.UI.Xaml.Application) 클래스 (Xaml)
-   [**Windows. .xaml**](/uwp/api/Windows.UI.Xaml.Window) 클래스 (xaml)

## <a name="related-topics"></a>관련 항목

* [**ApplicationExecutionState**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)
* [앱 일시 중단 및 다시 시작에 대 한 지침](./index.md)
* [앱 사전 실행 처리](handle-app-prelaunch.md)
* [앱 활성화 처리](activate-an-app.md)
* [앱 일시 중단 처리](suspend-an-app.md)
* [앱 다시 시작 처리](resume-an-app.md)
* [단일 프로세스 모델을 사용 하는 백그라운드 작업](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [백그라운드에서 미디어 재생](../audio-video-camera/background-audio.md)

 

 
