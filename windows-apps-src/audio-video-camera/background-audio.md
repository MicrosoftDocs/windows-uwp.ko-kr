---
author: drewbatgit
ms.assetid: 
description: "이 문서에서는 앱이 백그라운드에서 실행되는 동안 미디어를 재생하는 방법을 보여 줍니다."
title: "백그라운드에서 미디어 재생"
translationtype: Human Translation
ms.sourcegitcommit: c8cbc538e0979f48b657197d59cb94a90bc61210
ms.openlocfilehash: a477827553ac1780ac625deeee08d84ab638d4c2

---

# 백그라운드에서 미디어 재생
이 문서에서는 앱이 포그라운드에서 백그라운드로 이동될 때 미디어가 계속 재생되도록 앱을 구성하는 방법을 보여 줍니다. 즉, 사용자가 홈 화면에서 반환된 앱을 최소화했거나 다른 방법으로 앱에서 외부로 이동한 후에도 앱은 오디오를 계속 재생할 수 있습니다. 

백그라운드 오디오 재생 시나리오는 다음과 같습니다.

-   **오래 실행되는 재생 목록:** 사용자가 간단하게 포그라운드 앱을 표시하여 재생 목록을 선택하고 시작할 수 있으며, 그 후 사용자는 예상대로 백그라운드에서 재생 목록을 계속 재생할 수 있습니다.

-   **작업 전환기 사용:** 사용자가 간단하게 포그라운드 앱을 표시하여 오디오 재생을 시작한 다음 작업 전환을 사용하여 열려 있는 다른 앱으로 전환할 수 있습니다. 사용자는 예상대로 백그라운드에서 오디오를 계속 재생할 수 있습니다.

이 문서에 설명된 배경 오디오 구현을 사용하면 모바일, 데스크톱, Xbox 등의 모든 Windows 디바이스에서 앱을 실행할 수 있습니다.

> [!NOTE]
> 이 문서의 코드는 UWP [배경 오디오 샘플](http://go.microsoft.com/fwlink/p/?LinkId=800141)에서 조정되었습니다.

## 단일 프로세스 모델에 대한 설명.
Windows 10 버전 1607에서는 배경 오디오를 사용하도록 설정하는 프로세스를 훨씬 간소화하는 새로운 단일 프로세스 모델이 도입되었습니다. 이전에는 앱이 포그라운드 앱뿐 아니라 백그라운드 프로세스도 관리한 다음 두 프로세스 간에 상태 변경을 수동으로 통신해야 했습니다. 새 모델에서는 앱 매니페스트에 배경 오디오 접근 권한 값을 추가하기만 하면 앱이 백그라운드로 이동될 때 자동으로 오디오 재생을 계속합니다. 새로운 두 가지 응용 프로그램 수명 주기 이벤트인 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 및 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)를 사용하면 앱이 백그라운드로 이동하고 나가는 시기를 알 수 있습니다. 앱이 백그라운드로 이동하거나 나갈 때 시스템에서 적용하는 메모리 제약 조건이 변경될 수 있으므로 이러한 이벤트를 사용하여 현재 메모리 사용을 확인하고 제한 아래로 유지하기 위해 리소스를 해제할 수 있습니다.

새 모델을 사용하면 복잡한 프로세스 간 통신 및 상태 관리가 제거되므로 코드 작업이 훨씬 감소하며 배경 오디오를 보다 신속하게 구현할 수 있습니다. 그러나 이전 버전과의 호환성을 위해 현재 릴리스에서는 두 프로세스 모델도 계속 지원됩니다. 자세한 내용은 [레거시 배경 오디오 모델](background-audio.md)을 참조하세요.

## 배경 오디오에 대한 요구 사항
앱이 백그라운드에 있는 동안 오디오를 재생하려면 다음 요구 사항을 충족해야 합니다.

* 이 문서의 뒷부분에 설명된 대로 앱 매니페스트에 **백그라운드 미디어 재생** 접근 권한 값을 추가합니다.
* 앱이 [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 속성을 false로 설정하는 등 **MediaPlayer**와 SMTC(시스템 미디어 전송 컨트롤)의 자동 통합을 사용하지 않도록 설정하는 경우 백그라운드 미디어 재생을 사용하려면 SMTC와 수동 통합을 구현해야 합니다. **MediaPlayer** 이외의 API(예: [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph))를 사용하여 오디오를 재생하는 경우에도 앱이 백그라운드로 이동할 때 오디오 재생을 계속하려면 수동으로 SMTC와 통합해야 합니다. 최소 SMTC 통합 요구 사항은 [시스템 미디어 전송 컨트롤의 수동 컨트롤](system-media-transport-controls.md)에서 "배경 오디오에 대한 시스템 미디어 전송 컨트롤 사용" 섹션에 설명되어 있습니다.
* 앱이 백그라운드에 있는 동안 시스템에서 백그라운드 앱에 대해 설정한 메모리 사용량 제한 아래로 유지해야 합니다. 백그라운드에 있는 동안 메모리를 관리하기 위한 지침은 이 문서의 뒷부분에 제공되어 있습니다.

## 백그라운드 미디어 재생 매니페스트 접근 권한 값
배경 오디오를 사용하려면 Package.appxmanifest 앱 매니페스트 파일에 백그라운드 미디어 재생 접근 권한 값을 추가해야 합니다. 

**매니페스트 디자이너를 사용하여 앱 매니페스트에 접근 권한 값을 추가하려면**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **백그라운드 미디어 재생** 확인란을 선택합니다.

앱 매니페스트 xml을 수동으로 편집하여 접근 권한 값을 설정하려면 먼저 *uap3* 네임스페이스 접두사가 **Package** 요소에 정의되어 있는지 확인합니다. 정의되지 않은 경우 아래와 같이 추가합니다.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

**Capabilities** 요소에 *backgroundMediaPlayback* 접근 권한 값을 추가합니다.
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

##포그라운드와 백그라운드 간 전환 처리
앱이 포그라운드에서 백그라운드로 이동하면 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 이벤트가 발생합니다. 앱이 포그라운드로 반환되면 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 이벤트가 발생합니다. 이러한 이벤트는 앱 수명 주기 이벤트이기 때문에 앱을 만들 때 해당 이벤트 처리기를 등록해야 합니다. 기본 프로젝트 템플릿에서 이는 App.xaml.cs의 **App** 클래스 생성자에 추가하는 것을 의미합니다. 백그라운드에서 실행할 경우 시스템에서 앱에 허용하는 메모리 리소스가 줄어들기 때문에 앱의 현재 메모리 사용량과 현재 제한을 확인하는 데 사용되는 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 및 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 이벤트도 등록해야 합니다. 이러한 이벤트 처리기는 다음 예제에 표시되어 있습니다. UWP 앱의 응용 프로그램 수명 주기에 대한 자세한 내용은 [ 수명 주기](../\launch-resume\app-lifecycle.md)를 참조하세요.

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

현재 백그라운드에서 실행 중인지 여부를 추적하기 위한 변수를 만듭니다.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

[**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 이벤트가 발생하면 추적 변수를 설정하여 현재 백그라운드에서 실행 중임을 나타냅니다. 백그라운드로의 전환 속도가 사용자에게 느려지는 것처럼 보일 수 있으므로 **EnteredBackground** 이벤트에서 장기 실행 작업을 수행하면 안 됩니다.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

앱이 백그라운드로 전환하면 현재 포그라운드 앱이 즉각적으로 응답하는 사용자 환경을 제공하기에 충분한 리소스를 사용할 수 있도록 시스템에서 앱의 메모리 제한을 줄입니다. [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 이벤트 처리기를 사용하면 할당된 메모리가 축소되었음을 앱이 알 수 있으며 처리기에 전달되는 이벤트 인수에서 새 제한이 제공됩니다. 앱의 현재 사용량을 제공하는 [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage) 속성과 새 제한을 지정하는 이벤트 인수의 [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) 속성을 비교합니다. 메모리 사용량이 제한을 초과할 경우 메모리 사용량을 줄여야 합니다. 이 예제에서는 이 문서의 뒷부분에서 정의하는 도우미 메서드 **ReduceMemoryUsage**에서 이 작업이 수행됩니다.

[!code-cs[MemoryUsageLimitChanging](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE] 
> 일부 디바이스 구성에서는 시스템에서 리소스 압력이 발생할 때까지 새 메모리 제한을 초과해도 응용 프로그램을 계속 실행할 수 있지만 실행할 수 없는 경우도 있습니다. 특히 Xbox에서는 2초 내에 새 메모리 제한 아래로 메모리를 줄이지 않으면 앱이 일시 중단되거나 종료됩니다. 즉, 이 이벤트를 사용하여 이벤트 발생 후 2초 내에 리소스 사용량을 제한 아래로 줄이면 가장 광범위한 디바이스에서 최상의 환경을 제공할 수 있습니다.


앱이 처음 백그라운드로 전환될 때는 메모리 사용량이 백그라운드 앱에 대한 메모리 제한 아래지만 이후에 사용량이 증가하여 제한에 가까워지는 경우도 있습니다. [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 처리기는 증가 시 현재 사용량을 확인하고 필요한 경우 메모리를 해제할 수 있는 기회를 제공합니다. [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel)이 **High** 또는 **OverLimit**인지 여부를 확인하고, 맞으면 메모리 사용량을 줄입니다. 이 예제에서는 이 프로세스도 도우미 메서드 **ReduceMemoryUsage**에서 처리됩니다. [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased) 이벤트를 구독하고 앱이 제한 아래인지 확인한 다음 맞으면 추가 리소스를 할당할 수도 있습니다.

[!code-cs[MemoryUsageIncreased](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage**는 앱이 백그라운드에서 실행되는 앱에 대한 사용량 제한을 초과할 경우 메모리를 해제하기 위해 구현할 수 있는 도우미 메서드입니다. 메모리를 해제하는 방법은 앱의 특징에 따라 다르지만, 메모리를 해제하는 한 가지 권장 방법은 UI 및 앱 보기와 관련된 기타 리소스를 삭제하는 것입니다. 먼저, 백그라운드 모드에서 실행 중인지 확인한 다음 앱 창의 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 속성을 null로 설정합니다. **GC.Collect**를 호출하여 해제된 메모리를 즉시 회수하도록 시스템에 지시합니다.

[!code-cs[UnloadViewContent](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetUnloadViewContent)]

창 콘텐츠를 수집하는 경우 각 프레임이 해당 연결 끊기 프로세스를 시작합니다. 창 콘텐츠 아래의 시각적 개체 트리에 페이지가 있는 경우 페이지에서 해당 [**Unloaded**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Unloaded) 이벤트가 발생하기 시작합니다. 페이지에 대한 모든 참조를 제거하지 않으면 메모리에서 페이지를 완전히 지울 수 없습니다. **Unloaded** 콜백에서 다음을 수행하여 메모리가 빠르게 해제되도록 합니다.
* 페이지에 있는 큰 데이터 구조를 선택 취소하고 null로 설정합니다.
* 페이지 내에 콜백 메서드가 있는 모든 이벤트 처리기를 등록 취소합니다. 페이지에 대한 Loaded 이벤트 처리기에서 해당 콜백을 등록해야 합니다. UI가 재구성되고 페이지가 시각적 개체 트리에 추가되면 Loaded 이벤트가 발생합니다.
* Unloaded 콜백의 끝에서 **GC.Collect**를 호출하여 방금 null로 설정한 큰 데이터 구조를 빠르게 가비지 수집합니다.

[!code-cs[Unloaded](./code/BackgroundAudio_RS1/cs/MainPage.xaml.cs#SnippetUnloaded)]

[**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 이벤트 처리기에서 추적 변수를 설정하여 앱이 더 이상 백그라운드에서 실행되지 않음을 나타내야 합니다. 그런 다음 현재 창의 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content)가 null인지 확인합니다. 백그라운드에서 실행하는 동안 메모리를 지우기 위해 앱 보기를 삭제한 경우 null이 됩니다. 창 콘텐츠가 null이면 앱 보기를 다시 작성합니다. 이 예제에서는 창 콘텐츠가 도우미 메서드 **CreateRootFrame**에서 만들어집니다.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

**CreateRootFrame** 도우미 메서드는 앱 보기 콘텐츠를 다시 만듭니다. 이 메서드의 코드는 기본 프로젝트 템플릿에 제공된 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 처리기 코드와 같습니다. 한 가지 차이점은 **Launching** 처리기는 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)의 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) 속성에서 이전 실행 상태를 확인하고 **CreateRootFrame** 메서드는 단순히 인수로 전달된 이전 실행 상태를 가져온다는 것입니다. 중복 코드를 최소화하기 위해 필요한 경우 **CreateRootFrame**을 호출하도록 기본 **Launching** 이벤트 처리기 코드를 리팩터링할 수 있습니다.

[!code-cs[CreateRootFrame](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetCreateRootFrame)]

## 백그라운드 미디어 앱에 대한 네트워크 가용성
스트림이나 파일에서 생성되지 않은 모든 네트워크 인식 미디어 원본은 원격 콘텐츠를 검색하는 동안 네트워크 연결을 활성 상태로 유지하고 검색하지 않을 때 해제합니다. 특히 [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource)의 경우 응용 프로그램이 [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762)를 사용하여 올바른 버퍼 범위를 플랫폼에 정확하게 보고해야 합니다. 전체 콘텐츠가 완전히 버퍼링된 후에는 더 이상 앱을 위해 네트워크가 예약되지 않습니다.

미디어를 다운로드하지 않을 때 백그라운드에서 발생하는 네트워크 호출을 수행해야 하는 경우 [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger), [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger), [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger) 등의 적절한 작업에 해당 호출을 래핑해야 합니다. 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks)을 참조하세요.

## 관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)
* [백그라운드 오디오 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 







<!--HONumber=Aug16_HO3-->


