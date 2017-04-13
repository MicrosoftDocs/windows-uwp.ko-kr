---
author: TylerMSFT
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: "이 문서에서는 앱이 백그라운드로 이동할 때 메모리를 줄이는 방법을 보여 줍니다."
title: "앱이 백그라운드 상태로 이동할 때 메모리 사용량 줄이기"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 7c8b8eb3ae3c097a346144c57d7899cb5e9584f5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>앱이 백그라운드로 이동할 때 메모리 회수

이 문서에서는 앱이 백그라운드 상태로 이동할 때 일시 중단되거나 종료되지 않도록 앱에서 사용하는 메모리 양을 줄이는 방법을 알아봅니다.

## <a name="new-background-events"></a>새 백그라운드 이벤트

Windows 10 버전 1607에는 두 개의 새 응용 프로그램 수명 주기 이벤트 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 및 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)가 추가되었습니다. 이러한 이벤트를 사용하면 백그라운드 상태가 되거나 백그라운드 상태에서 벗어날 때 앱이 알 수 있습니다.

앱이 백그라운드로 이동하면 시스템에 의해 메모리 제약 조건이 변경될 수 있습니다. 이러한 이벤트를 사용하여 현재 메모리 소비를 확인하고 리소스를 해제하여 앱이 백그라운드 상태일 때 일시 중단되거나 종료되지 않도록 제한값 이하로 유지합니다.

### <a name="events-for-controlling-your-apps-memory-usage"></a>앱의 메모리 사용량 제어를 위한 이벤트

[MemoryManager.AppMemoryUsageLimitChanging](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelimitchanging.aspx)은 앱에서 사용할 수 있는 총 메모리 제한이 변경되기 직전에 발생합니다. 예를 들어 앱이 백그라운드로 이동하면 Xbox의 메모리 제한이 1024MB에서 128MB로 변경됩니다.  
이 이벤트는 앱이 일시 중단 또는 종료되지 않도록 플랫폼을 유지하기 위해 처리해야 하는 가장 중요한 이벤트입니다.

[MemoryManager.AppMemoryUsageIncreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusageincreased.aspx)는 [AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.appmemoryusagelevel.aspx) 열거에서 앱의 메모리 소비가 더 높은 값으로 증가할 때 발생합니다. 예를 들어 **Low**에서 **Medium**으로 변경될 때 발생합니다. 이 이벤트의 처리는 선택 사항이지만 응용 프로그램을 제한 이하로 유지해야 하므로 권장됩니다.

[MemoryManager.AppMemoryUsageDecreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagedecreased.aspx)는 **AppMemoryUsageLevel** 열거에서 앱의 메모리 소비가 더 낮은 값으로 줄어들 때 발생합니다. 예를 들어 **High**에서 **Low**로 변경될 때 발생합니다. 이 이벤트의 처리는 선택 사항이지만 필요한 경우 응용 프로그램에서 추가 메모리를 할당할 수 있다는 것을 나타냅니다.

## <a name="handle-the-transition-between-foreground-and-background"></a>포그라운드와 백그라운드 간 전환 처리

앱이 포그라운드에서 백그라운드로 이동하면 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 이벤트가 발생합니다. 앱이 포그라운드로 반환되면 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 이벤트가 발생합니다. 앱을 만들 때 이러한 이벤트에 대한 처리기를 등록할 수 있습니다. 기본 프로젝트 템플릿에서 이는 App.xaml.cs의 **App** 클래스 생성자에서 수행됩니다.

백그라운드에서 실행할 경우 앱에 허용되는 메모리 리소스가 줄어들기 때문에 앱의 현재 메모리 사용량과 현재 제한을 확인하는 데 사용할 수 있는 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 및 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 이벤트도 등록해야 합니다. 이러한 이벤트 처리기는 다음 예제에 표시되어 있습니다. UWP 앱의 응용 프로그램 수명 주기에 대한 자세한 내용은 [앱 수명 주기](../\launch-resume\app-lifecycle.md)를 참조하세요.

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

[**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 이벤트가 발생하면 추적 변수를 설정하여 현재 백그라운드에서 실행 중임을 나타냅니다. 이 처리기는 메모리 사용량을 줄이기 위한 코드를 작성할 때 유용합니다.

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

앱이 백그라운드로 전환하면 현재 포그라운드 앱이 응답하는 사용자 환경을 제공하기에 충분한 리소스를 사용할 수 있도록 시스템에서 앱의 메모리 제한을 줄입니다.

[**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 이벤트 처리기를 사용하면 할당된 메모리가 축소되었음을 앱이 알 수 있으며 처리기에 전달되는 이벤트 인수에서 새 제한이 제공됩니다. 앱의 현재 사용량을 제공하는 [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage) 속성과 새 제한을 지정하는 이벤트 인수의 [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) 속성을 비교합니다. 메모리 사용량이 제한을 초과할 경우 메모리 사용량을 줄여야 합니다.

이 예제에서는 이 문서의 뒷부분에서 정의하는 도우미 메서드 **ReduceMemoryUsage**에서 이 작업이 수행됩니다.

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> 일부 디바이스 구성에서는 시스템에서 리소스 압력이 발생할 때까지 새 메모리 제한을 초과해도 응용 프로그램을 계속 실행할 수 있지만 실행할 수 없는 경우도 있습니다. 특히 Xbox에서는 2초 내에 새 메모리 제한 아래로 메모리를 줄이지 않으면 앱이 일시 중단되거나 종료됩니다. 즉, 이 이벤트를 사용하여 이벤트 발생 후 2초 내에 리소스 사용량을 제한 아래로 줄이면 가장 광범위한 디바이스에서 최상의 환경을 제공할 수 있습니다.

앱이 처음 백그라운드로 전환될 때 앱의 메모리 사용량이 현재 백그라운드 앱의 메모리 제한 이하이더라도 시간이 지남에 따라 메모리 소비가 계속 증가하여 제한값에 도달할 수 있습니다. [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 처리기는 증가 시 현재 사용량을 확인하고 필요한 경우 메모리를 해제할 수 있는 기회를 제공합니다.

[**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel)이 **High** 또는 **OverLimit**인지 여부를 확인하고, 맞으면 메모리 사용량을 줄입니다. 이 예제에서는 도우미 메서드 **ReduceMemoryUsage**에서 처리됩니다. [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased) 이벤트를 구독하고 앱이 제한 아래인지 확인한 다음 맞으면 추가 리소스를 할당할 수도 있습니다.

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage**는 앱이 백그라운드에서 실행 중인 동안 사용량 제한을 초과할 경우 메모리를 해제하기 위해 구현할 수 있는 도우미 메서드입니다. 메모리를 해제하는 방법은 앱의 특징에 따라 다르지만, 메모리를 해제하는 한 가지 권장 방법은 UI 및 앱 보기와 관련된 기타 리소스를 삭제하는 것입니다. 이렇게 하려면 현재 백그라운드 상태에서 실행 중인지 확인하고 앱 창의 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 속성을 `null`로 설정하여 UI 이벤트 처리기의 등록을 해제하고 페이지에 대한 다른 참조를 제거합니다. UI 이벤트 처리기의 등록 해제에 실패하고 페이지에 대한 다른 참조를 모두 지우면 페이지 리소스가 해제되지 않습니다. 그런 다음 **GC.Collect**를 호출하여 해제된 메모리를 즉시 회수합니다.

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

창 콘텐츠를 수집하는 경우 각 프레임이 해당 연결 끊기 프로세스를 시작합니다. 창 콘텐츠 아래의 시각적 개체 트리에 페이지가 있는 경우 페이지에서 해당 Unloaded 이벤트가 발생하기 시작합니다. 페이지에 대한 모든 참조를 제거하지 않으면 메모리에서 페이지를 완전히 지울 수 없습니다. Unloaded 콜백에서 다음을 수행하여 메모리가 빠르게 해제되도록 합니다.
* 페이지에 있는 큰 데이터 구조를 지우고 `null`로 설정합니다.
* 페이지 내에 콜백 메서드가 있는 모든 이벤트 처리기를 등록 취소합니다. 페이지에 대한 Loaded 이벤트 처리기에서 해당 콜백을 등록해야 합니다. UI가 재구성되고 페이지가 시각적 개체 트리에 추가되면 Loaded 이벤트가 발생합니다.
* Unloaded 콜백의 끝에서 `GC.Collect`를 호출하여 방금 `null`로 설정한 큰 데이터 구조를 빠르게 가비지 수집합니다.

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

[**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 이벤트 처리기에서 추적 변수(`isInBackgroundMode`)를 설정하여 앱이 더 이상 백그라운드에서 실행되지 않음을 나타내야 합니다. 그런 다음 현재 창의 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content)가 `null`인지 확인합니다. 백그라운드에서 실행하는 동안 메모리를 지우기 위해 앱 보기를 삭제한 경우 null이 됩니다. 창 콘텐츠가 `null`이면 앱 보기를 다시 작성합니다. 이 예제에서는 창 콘텐츠가 도우미 메서드 **CreateRootFrame**에서 만들어집니다.

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

**CreateRootFrame** 도우미 메서드는 앱 보기 콘텐츠를 다시 만듭니다. 이 메서드의 코드는 기본 프로젝트 템플릿에 제공된 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 처리기 코드와 거의 같습니다. 한 가지 차이점은 **Launching** 처리기는 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)의 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) 속성에서 이전 실행 상태를 확인하고 **CreateRootFrame** 메서드는 단순히 인수로 전달된 이전 실행 상태를 가져온다는 것입니다. 중복 코드를 최소화하기 위해 **CreateRootFrame**을 호출하도록 기본 **Launching** 이벤트 처리기 코드를 리팩터링할 수 있습니다.

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>지침

### <a name="moving-from-the-foreground-to-the-background"></a>포그라운드에서 백그라운드로 이동

앱이 포그라운드에서 백그라운드로 이동하면 시스템에서 앱 대신 백그라운드에 필요하지 않은 리소스를 해제합니다. 예를 들어 UI 프레임워크가 캐시된 텍스처를 플러시하고 비디오 하위 시스템에서 앱 대신 할당된 메모리를 해제합니다. 그러나 앱은 시스템에 의해 일시 중단 또는 종료되지 않도록 메모리 사용량을 계속 주의 깊게 모니터링합니다.

앱이 포그라운드에서 백그라운드로 이동할 때 앱에서는 **EnteredBackground** 이벤트와 **AppMemoryUsageLimitChanging** 이벤트를 차례로 가져옵니다.

- **작업** **EnteredBackground** 이벤트를 사용하여 앱이 백그라운드에서 실행되는 동안 필요하지 않은 UI 리소스를 해제합니다. 예를 들어 노래의 커버 아트 이미지를 지울 수 있습니다.
- **작업** **AppMemoryUsageLimitChanging** 이벤트를 사용하여 앱이 새 백그라운드 제한보다 낮은 메모리를 사용 중인지 확인합니다. 그렇지 않은 경우 리소스를 해제해야 합니다. 그러지 않으면 디바이스 정책에 따라 앱이 일시 중단되거나 종료될 수 있습니다.
- **작업** **AppMemoryUsageLimitChanging** 이벤트가 발생할 때 앱이 새 메모리 제한을 초과하는 경우 가비지 수집기를 수동으로 호출합니다.
- **작업** **AppMemoryUsageIncreased** 이벤트를 사용하여 앱이 백그라운드에서 실행되는 동안 메모리 사용량을 계속 모니터링합니다. **AppMemoryUsageLevel**이 **High** 또는 **OverLimit**인 경우 리소스를 해제해야 합니다.
- **고려 사항** 성능 최적화로 **EnteredBackground** 처리기 대신 **AppMemoryUsageLimitChanging** 이벤트 처리기에서 UI 리소스를 해제합니다. **EnteredBackground/LeavingBackground** 이벤트 처리기에 설정된 부울 값을 사용하여 앱이 백그라운드 또는 포그라운드에서 실행 중인지 추적합니다. 그런 다음 **AppMemoryUsageLimitChanging** 이벤트 처리기에서 **AppMemoryUsage**가 제한을 초과하고 앱이 백그라운드에서 실행되는 경우(부울 값 기준) UI 리소스를 해제할 수 있습니다.
- **작업** 응용 프로그램 간의 전환 속도가 사용자에게 느리게 보일 수 있으므로 **EnteredBackground** 이벤트에서 장기 실행 작업을 수행하면 안 됩니다.

### <a name="moving-from-the-background-to-the-foreground"></a>백그라운드에서 포그라운드로 이동

앱이 백그라운드에서 포그라운드로 이동할 때 앱에서는 **AppMemoryUsageLimitChanging** 이벤트와 **LeavingBackground** 이벤트를 차례로 가져옵니다.

- **작업** **LeavingBackground** 이벤트를 사용하여 백그라운드로 이동할 때 앱에서 취소한 UI 리소스를 다시 만듭니다.

## <a name="related-topics"></a>관련 항목

* [백그라운드 미디어 재생 샘플](http://go.microsoft.com/fwlink/p/?LinkId=800141) - 앱이 백그라운드 상태로 이동할 때 메모리를 해제하는 방법을 보여 줍니다.
* [진단 도구](https://blogs.msdn.microsoft.com/visualstudioalm/2015/01/16/diagnostic-tools-debugger-window-in-visual-studio-2015/) - 진단 도구를 사용하여 가비지 수집 이벤트를 관찰하고 앱이 올바른 방법으로 메모리를 해제하고 있는지 확인합니다.
