---
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: 이 문서에서는 앱이 백그라운드로 이동할 때 메모리를 줄이는 방법을 보여 줍니다.
title: 앱이 백그라운드 상태로 이동할 때 메모리 사용 줄이기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a457b5eb976d1c34daa79a88113174fa664804ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175157"
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>앱이 백그라운드로 이동할 때 메모리 회수

이 문서에서는 일시 중단 및 종료 되지 않도록 백그라운드 상태로 이동할 때 앱에서 사용 하는 메모리 양을 줄이는 방법을 보여 줍니다.

## <a name="new-background-events"></a>새 백그라운드 이벤트

Windows 10 버전 1607에는 두 가지 새로운 응용 프로그램 수명 주기 이벤트 인 이벤트, 즉 [**Enteredbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 및 [**leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)이 도입 되었습니다. 이러한 이벤트를 통해 앱이 시작 되 고 배경을 벗어날 때이를 알 수 있습니다.

앱이 백그라운드로 이동 하면 시스템에서 적용 하는 메모리 제약 조건이 변경 될 수 있습니다. 이러한 이벤트를 사용 하 여 응용 프로그램이 백그라운드에서 일시 중단 되 고 종료 될 수 있도록 제한 보다 낮게 유지 하기 위해 현재 메모리 사용량과 사용 가능한 리소스를 확인 합니다.

### <a name="events-for-controlling-your-apps-memory-usage"></a>앱 메모리 사용량을 제어 하기 위한 이벤트

[AppMemoryUsageLimitChanging](/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) 는 앱에서 사용할 수 있는 총 메모리 한도가 변경 되기 직전에 발생 합니다. 예를 들어 앱이 배경으로 이동 하 고 Xbox에서 메모리 제한은 1024MB에서 128MB로 변경 됩니다.  
이 이벤트는 플랫폼에서 앱을 일시 중단 하거나 종료 하는 것을 방지 하기 위해 처리 하는 가장 중요 한 이벤트입니다.

[AppMemoryUsageIncreased](/uwp/api/windows.system.memorymanager.appmemoryusageincreased) 는 응용 프로그램의 메모리 소비가 [AppMemoryUsageLevel](/uwp/api/windows.system.appmemoryusagelevel) 열거에서 더 높은 값으로 증가 했을 때 발생 합니다. 예를 들어, **낮음** 에서 **중간**까지입니다. 이 이벤트를 처리 하는 것은 선택 사항 이지만 응용 프로그램이 여전히 제한을 초과 하는 경우에는 권장 됩니다.

[AppMemoryUsageDecreased](/uwp/api/windows.system.memorymanager.appmemoryusagedecreased) 는 응용 프로그램의 메모리 소비가 **AppMemoryUsageLevel** 열거에서 더 낮은 값으로 감소할 때 발생 합니다. 예를 들어, **High** 에서 **Low**사이입니다. 이 이벤트를 처리 하는 것은 선택 사항 이지만 필요한 경우 응용 프로그램에서 추가 메모리를 할당할 수 있음을 나타냅니다.

## <a name="handle-the-transition-between-foreground-and-background"></a>포그라운드 및 백그라운드 간 전환 처리

앱이 포그라운드에서 배경으로 이동 하면, 사용자의 [**배경**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 이벤트가 발생 합니다. 앱이 포그라운드로 반환 되 면 [**Leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 이벤트가 발생 합니다. 앱을 만들 때 이러한 이벤트에 대 한 처리기를 등록할 수 있습니다. 기본 프로젝트 템플릿에서는 App.xaml.cs의 **App** class 생성자에서이 작업을 수행 합니다.

백그라운드에서 실행 하면 앱이 유지할 수 있는 메모리 리소스가 줄어들지만, 앱의 현재 메모리 사용량과 현재 제한을 확인 하는 데 사용할 수 있는 [**AppMemoryUsageIncreased**](/uwp/api/windows.system.memorymanager.appmemoryusageincreased) 및 [**AppMemoryUsageLimitChanging**](/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) 이벤트에도 등록 해야 합니다. 이러한 이벤트에 대 한 처리기는 다음 예제에 나와 있습니다. UWP 앱의 응용 프로그램 수명 주기에 대 한 자세한 내용은 [앱 수명 주기](..//launch-resume/app-lifecycle.md)를 참조 하세요.

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

지정 된 [**background**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 이벤트를 발생 시키면 현재 백그라운드에서 실행 중임을 나타내기 위해 추적 변수를 설정 합니다. 이는 메모리 사용을 줄이기 위해 코드를 작성할 때 유용 합니다.

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

앱이 백그라운드로 전환 되 면 시스템은 응용 프로그램에 대 한 메모리 제한을 줄여 현재 포그라운드 앱에 응답성이 뛰어난 사용자 환경을 제공 하기에 충분 한 리소스가 있는지 확인 합니다.

[**AppMemoryUsageLimitChanging**](/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) 이벤트 처리기를 사용 하면 할당 된 메모리가 감소 되었음을 앱에 알려 주고 처리기에 전달 된 이벤트 인수에 새 제한을 제공할 수 있습니다. 앱의 현재 사용량을 제공 하는 [**AppMemoryUsage**](/uwp/api/windows.system.memorymanager.appmemoryusage) 속성을 이벤트 인수의 [**newlimit**](/uwp/api/windows.system.appmemoryusagelimitchangingeventargs.newlimit) 속성에 비교 합니다 .이 속성은 새 제한을 지정 합니다. 메모리 사용량이 제한을 초과 하는 경우 메모리 사용량을 줄여야 합니다.

이 예제에서이 작업은이 문서의 뒷부분에서 정의 되는 도우미 메서드 **ReduceMemoryUsage**에서 수행 됩니다.

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> 일부 장치 구성을 사용 하면 시스템에서 리소스 압력을 발생 시킬 때까지 응용 프로그램을 새 메모리 제한을 초과 하 여 계속 실행할 수 있으며 일부는 그렇지 않습니다. 특히 Xbox에서 2 초 내에 새 제한에 따라 메모리를 줄이지 않는 경우 앱이 일시 중단 되거나 종료 됩니다. 즉,이 이벤트를 사용 하 여 가장 넓은 범위의 장치에서 최상의 환경을 제공할 수 있습니다 .이 이벤트는 발생 하는 이벤트의 2 초 이내에 리소스 사용량을 줄입니다.

응용 프로그램의 메모리 사용량이 현재 백그라운드로 전환 될 때 백그라운드 앱에 대 한 메모리 제한을 초과 하는 경우 시간이 지남에 따라 메모리 사용이 증가 하 고 제한에 접근 하기 시작할 수 있습니다. [**AppMemoryUsageIncreased**](/uwp/api/windows.system.memorymanager.appmemoryusageincreased) 는 현재 사용 중인 사용을 확인 하 고 필요한 경우 메모리를 확보할 수 있는 기회를 제공 합니다.

[**AppMemoryUsageLevel**](/uwp/api/Windows.System.AppMemoryUsageLevel) 가 크거나 **상한** **인지 확인** 하 고, 있는 경우 메모리 사용량을 줄입니다. 이 예제에서는 도우미 메서드인 **ReduceMemoryUsage**에서이를 처리 합니다. [**AppMemoryUsageDecreased**](/uwp/api/windows.system.memorymanager.appmemoryusagedecreased) 이벤트를 구독 하 고, 앱이 제한에 도달 하 고 있는지 확인 하 고, 그렇다면 추가 리소스를 할당할 수 있습니다.

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** 는 백그라운드에서 실행 하는 동안 앱이 사용 제한을 초과 하는 경우 메모리를 해제 하기 위해 구현할 수 있는 도우미 메서드입니다. 메모리를 해제 하는 방법은 앱의 세부 사항에 따라 달라 지지만 메모리를 확보 하는 데 권장 되는 방법 중 하나는 UI와 앱 보기와 관련 된 기타 리소스를 삭제 하는 것입니다. 이렇게 하려면 백그라운드 상태에서 실행 중인지 확인 하 고, 앱 창의 [**Content**](/uwp/api/windows.ui.xaml.window.content) 속성을로 설정 하 `null` 고, UI 이벤트 처리기의 등록을 취소 하 고 페이지에 있을 수 있는 다른 모든 참조를 제거 합니다. UI 이벤트 처리기의 등록을 취소 하 고 페이지에 있을 수 있는 다른 참조를 지우면 페이지 리소스가 해제 되지 않습니다. 그런 다음 **GC를 호출 합니다. 을 수집** 하 여 확보 된 메모리를 즉시 회수 합니다. 일반적으로 시스템에서 사용자에 대해 처리 하기 때문에 가비지 수집을 강제 적용 하지 않습니다. 이 특정 경우에는 시스템에서 메모리를 회수할 수 있도록 응용 프로그램을 종료 해야 하는 가능성을 줄이기 위해 백그라운드에 전환 될 때이 응용 프로그램에 부과 되는 메모리의 양을 줄일 수 있습니다.

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

창 콘텐츠가 수집 되 면 각 프레임은 연결 끊기 프로세스를 시작 합니다. 시각적 개체 트리의 창 내용 아래에 페이지가 있으면 언로드된 이벤트를 발생 시킵니다. 페이지에 대 한 모든 참조가 제거 되지 않으면 페이지를 메모리에서 완전히 지울 수 없습니다. 언로드된 콜백에서 다음 작업을 수행 하 여 메모리를 빠르게 해제할 수 있습니다.
* 페이지의 모든 대량 데이터 구조를 지우고로 설정 `null` 합니다.
* 페이지 내에 콜백 메서드가 있는 모든 이벤트 처리기를 등록 취소 합니다. 페이지에 대해 로드 된 이벤트 처리기 중에 이러한 콜백을 등록 해야 합니다. 로드 된 이벤트는 UI가 다시 구성 고 페이지가 시각적 개체 트리에 추가 되었을 때 발생 합니다.
* `GC.Collect`방금 설정한 모든 대량 데이터 구조를 빠르게 수집 하기 위해 언로드된 콜백의 끝에서를 호출 `null` 합니다. 다시 말하지만, 일반적으로 시스템이 사용자를 위해 처리 하기 때문에 가비지 수집을 강제 적용 하지 않습니다. 이 특정 경우에는 시스템에서 메모리를 회수할 수 있도록 응용 프로그램을 종료 해야 하는 가능성을 줄이기 위해 백그라운드에 전환 될 때이 응용 프로그램에 부과 되는 메모리의 양을 줄일 수 있습니다.

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

[**Leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 이벤트 처리기에서 추적 변수 ()를 설정 `isInBackgroundMode` 하 여 앱이 백그라운드에서 더 이상 실행 되지 않음을 표시 합니다. 다음으로, [**Content**](/uwp/api/windows.ui.xaml.window.content) `null` 백그라운드에서 실행 하는 동안 메모리를 지우도록 응용 프로그램 보기를 삭제 한 경우 현재 창의 콘텐츠가 인지 확인 합니다. 창 콘텐츠가 인 경우 `null` 앱 보기를 다시 빌드합니다. 이 예제에서는 도우미 메서드 **CreateRootFrame**에 창 콘텐츠가 만들어집니다.

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

**CreateRootFrame** helper 메서드는 앱에 대 한 보기 콘텐츠를 다시 만듭니다. 이 메서드의 코드는 기본 프로젝트 템플릿에 제공 된 [**Onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 된 처리기 코드와 거의 동일 합니다. 한 가지 차이점은 **시작** 처리기가 [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 의 [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate) 속성에서 이전 실행 상태를 결정 하 고 **CreateRootFrame** 메서드는 단순히 인수로 전달 된 이전 실행 상태를 가져오는 것입니다. 중복 된 코드를 최소화 하기 위해 기본 **시작** 이벤트 처리기 코드를 리팩터링하여 **CreateRootFrame**을 호출할 수 있습니다.

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>지침

### <a name="moving-from-the-foreground-to-the-background"></a>전경에서 배경으로 이동

앱이 포그라운드에서 백그라운드로 이동 하는 경우 시스템은 백그라운드에서 필요 하지 않은 리소스를 확보 하기 위해 앱을 대신 하 여 작동 합니다. 예를 들어, UI 프레임 워크는 캐시 된 질감을 플러시하고 비디오 하위 시스템은 앱을 대신 하 여 할당 된 메모리를 해제 합니다. 그러나 응용 프로그램은 시스템에 의해 일시 중단 되거나 종료 되는 것을 방지 하기 위해 메모리 사용을 신중 하 게 모니터링 해야 합니다.

앱이 포그라운드에서 배경으로 이동 하면 먼저 **AppMemoryUsageLimitChanging** 이벤트가 발생 하 고,이 **이벤트가 발생** 합니다.

- 응용 프로그램을 백그라운드에서 실행 하는 동안 필요 하지 않은 UI 리소스를 확보 하기 위해 **사용자의 응용** 프로그램 **을 사용 합니다** . 예를 들어, 노래의 커버 아트 이미지를 해제할 수 있습니다.
- **AppMemoryUsageLimitChanging** 이벤트 **를 사용 하 여 앱** 이 새 백그라운드 제한 보다 더 작은 메모리를 사용 하 고 있는지 확인 합니다. 그렇지 않으면 리소스를 확보 해야 합니다. 이렇게 하지 않으면 장치 특정 정책에 따라 앱이 일시 중단 되거나 종료 될 수 있습니다.
- **AppMemoryUsageLimitChanging** 이벤트가 발생할 때 앱이 새 메모리 제한을 초과 하는 경우 가비지 수집기 **를 수동으로 호출** 합니다.
- 변경 해야 하는 경우 백그라운드에서 실행 하는 동안 앱의 메모리 사용량을 계속 모니터링 하려면 **AppMemoryUsageIncreased** 이벤트 **를 사용 합니다** . **AppMemoryUsageLevel** **가 크거나** 같은 경우 리소스를 **확보 해야 합니다** .
- **AppMemoryUsageLimitChanging** 이벤트 처리기에서 UI 리소스를 해제 하는 **것이** 성능 최적화로 **서는 안** 됩니다. 앱이 배경 또는 전경에 있는지 여부를 추적 **하는 데** 사용할 수 있는 부울 값을 지정 합니다. 그런 다음 **AppMemoryUsageLimitChanging** 이벤트 처리기에서 **AppMemoryUsage** 가 제한을 초과 하 고 앱이 백그라운드 (부울 값을 기반으로) 인 경우 UI 리소스를 해제할 수 있습니다.
- 응용 프로그램 간의 전환이 사용자에 게 느리게 **EnteredBackground** 표시 될 수 있기 때문에 사용자에 게 장기 실행 작업을 수행 **하지 마십시오** .

### <a name="moving-from-the-background-to-the-foreground"></a>백그라운드에서 전경으로 이동

앱이 백그라운드에서 포그라운드로 이동할 때 앱은 먼저 **AppMemoryUsageLimitChanging** 이벤트와 **Leavingbackground** 이벤트를 가져옵니다.

- **Leavingbackground** 이벤트 **를 사용 하 여 백그라운드** 로 이동할 때 앱이 삭제 한 UI 리소스를 다시 만듭니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 미디어 재생 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback) -앱이 백그라운드 상태로 이동할 때 메모리를 해제 하는 방법을 보여 줍니다.
* [진단 도구](https://devblogs.microsoft.com/devops/diagnostic-tools-debugger-window-in-visual-studio-2015/) -진단 도구를 사용 하 여 가비지 수집 이벤트를 관찰 하 고 앱에서 원하는 방식으로 메모리를 해제 하 고 있는지 확인 합니다.