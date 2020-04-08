---
ms.assetid: 00ECF6C7-0970-4D5F-8055-47EA49F92C12
title: 앱 시작 성능 모범 사례
description: 시작 및 활성화 처리 방법을 개선하여 시작 시간이 최적화된 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ae37ab763b6705fbb3f341569904972ebb181412
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254680"
---
# <a name="best-practices-for-your-apps-startup-performance"></a>앱 시작 성능 모범 사례


시작 및 활성화 처리 방법을 개선하여 시작 시간이 최적화된 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.

## <a name="best-practices-for-your-apps-startup-performance"></a>앱 시작 성능 모범 사례

부분적으로 앱이 시작하는 데 걸리는 시간에 따라 사용자는 앱이 빠르거나 느리다고 느끼게 됩니다. 이 항목에서 앱의 시작 시간은 사용자가 앱을 시작하는 시점부터 사용자가 앱과 의미 있는 상호 작용을 할 수 있는 시점까지입니다. 이 섹션에서는 앱을 시작할 때 더 우수한 성능을 얻을 수 있는 방법을 제안합니다.

### <a name="measuring-your-apps-startup-time"></a>앱의 시작 시간 측정

실제로 시작 시간을 측정할 시점보다 조금 앞서서 앱을 시작해야 합니다. 그러면 측정을 위한 기준을 갖게 되어 가능한 한 합리적으로 시작 시간을 짧게 측정할 수 있습니다.

UWP 앱이 고객의 컴퓨터에 도착할 때까지 앱은 .NET 네이티브 도구 체인을 사용하여 컴파일되었습니다. .NET 네이티브는 MSIL을 기본적으로 실행 가능 컴퓨터 코드로 변환하는 미리 컴파일 기술입니다. .NET 네이티브 앱은 해당 MSIL 앱보다 더 빠르게 시작되고 메모리와 배터리를 더 적게 사용합니다. .NET 네이티브로 작성된 응용 프로그램이 사용자 지정 런타임 및 모든 디바이스에서 실행할 수 있는 새 수렴형 .NET Core에서 정적으로 연결되므로 제공 .NET 구현에 따라 다르지 않습니다. 개발 컴퓨터에서 앱은 "릴리스" 모드로 작성하는 경우 기본적으로 .NET 네이티브를 사용하고 "디버그" 모드로 작성하는 경우 CoreCLR을 사용합니다. Visual Studio의 C#인 경우 "속성"에서 빌드 페이지, VB인 경우 “내 프로젝트"에서 컴파일-&gt;고급을 통해 이를 구성할 수 있습니다. ".NET 네이티브 도구 체인으로 컴파일"이라는 확인란을 찾습니다.

물론 최종 사용자의 경험을 대표할 측정값을 얻어야 합니다. 따라서 개발 컴퓨터의 네이티브 코드로 앱을 컴파일하고 있는지 확신할 수 없는 경우 시작 시간을 측정하기 전에 네이티브 이미지 생성기(Ngen.exe) 도구를 실행하여 앱을 미리 컴파일할 수 있습니다.

다음 절차는 Ngen.exe를 실행하여 앱을 프리컴파일하는 방법을 설명합니다.

**Ngen.exe를 실행하려면**

1.  앱을 한 번 이상 실행하여 Ngen.exe에서 이를 검색할 수 있게 합니다.
2.  다음 중 한 가지를 수행하여 **작업 스케줄러**를 엽니다.
    -   시작 화면에서 "작업 스케줄러"를 검색합니다.
    -   "taskschd.msc"를 실행합니다.
3.  **작업 스케줄러**의 왼쪽 창에서 **작업 스케줄러 라이브러리**를 확장합니다.
4.  **Microsoft**를 확장합니다.
5.  **Windows**를 확장합니다.
6.  **.NET Framework**를 선택합니다.
7.  작업 목록에서 **.NET Framework NGEN 4.x**를 선택합니다.

    64비트 컴퓨터를 사용하는 경우 **.NET Framework NGEN v4.x 64**도 있습니다. 64비트 앱을 빌드하는 경우 **.NET Framework NGEN v4.x 64**를 선택합니다.

8.  **작업** 메뉴에서 **실행**을 클릭합니다.

Ngen.exe가 시스템에서 사용된 적이 있고 네이티브 이미지가 없는 모든 앱을 프리컴파일합니다. 프리컴파일할 앱이 많을 경우 오랜 시간이 걸릴 수 있으나 그 다음 실행부터는 훨씬 빨라집니다.

앱을 다시 컴파일할 때 네이티브 이미지는 더 이상 사용되지 않습니다. 그 대신 앱이 Just-in-Time 컴파일됩니다. 즉 앱이 실행될 때 컴파일됩니다. 새 네이티브 이미지를 얻으려면 Ngen.exe를 다시 실행해야 합니다.

### <a name="defer-work-as-long-as-possible"></a>가급적 오래 작업 연기

앱의 시작 시간을 단축하려면 사용자가 앱과의 상호 작용을 시작하는 데 꼭 필요한 작업만 수행하세요. 추가 어셈블리의 로드를 연기할 수 있다면 특히 효과적입니다. 어셈블리가 처음으로 사용될 때 공용 언어 런타임에서 이를 로드합니다. 로드되는 어셈블리의 수를 최소화하는 것이 가능할 경우 앱의 시작 시간을 단축하고 메모리 사용량을 줄일 수 있습니다.

### <a name="do-long-running-work-independently"></a>실행 시간이 긴 작업은 별개로 수행

앱의 일부가 제대로 작동하지 않더라도 앱과의 상호 작용이 가능할 수 있습니다. 예를 들어 앱에서 검색하는 데 오랜 시간이 걸리는 데이터를 표시할 경우 데이터를 비동기적으로 검색함으로써 코드가 앱의 시작 코드와 별개로 실행되게 할 수 있습니다. 데이터를 사용할 수 있게 되면 앱의 사용자 인터페이스를 그 데이터로 채웁니다.

데이터를 검색하는 UWP(유니버설 Windows 플랫폼) API 중 상당수가 비동기적이므로, 어떤 식으로든 비동기적으로 데이터를 검색하고 있을 것입니다. 비동기 API에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기 API 호출](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)을 참조하세요. 비동기 API를 사용하지 않는 작업을 하는 경우 장기간 실행되는 작업에 Task 클래스를 사용하여 사용자의 앱 조작을 차단하지 않을 수 있습니다. 그러면 데이터가 로드되는 동안에도 앱이 사용자에게 응답 가능한 상태로 유지됩니다.

앱에서 해당 UI의 일부를 로드하는 데 특히 오랜 시간이 걸릴 경우 그 영역에 "최신 데이터를 가져오고 있습니다"와 같은 문자열을 추가해 보세요. 그러면 사용자는 앱이 계속 처리하고 있음을 알 수 있습니다.

## <a name="minimize-startup-time"></a>시작 시간 최소화

가장 단순한 앱을 제외한 모든 앱에는 리소스 로드, XAML 구문 분석, 데이터 구조 설정 및 활성화 시 논리 실행에 인식할 정도의 시간이 필요합니다. 활성화 프로세스를 세 개 단계로 구분하여 분석해 보겠습니다. 또한 각 단계에 소요된 시간을 줄이는 팁과 앱 시작의 각 단계를 사용자에게 보다 친숙하게 만드는 방법을 제공합니다.

활성화 기간은 사용자가 앱을 시작한 순간과 앱이 작동하는 순간 사이에 경과된 시간입니다. 이 기간은 사용자가 느끼는 앱의 첫 인상이므로 중요한 시간입니다. 사용자는 시스템과 앱으로부터 즉각적이고 지속적인 피드백을 기대합니다. 앱이 빠르게 시작되지 않으면 시스템과 앱이 중단되거나 잘못 디자인되었다고 인식됩니다. 더 나쁜 경우 앱이 활성화되는 데 너무 오랜 시간이 걸리면 PLM(프로세스 수명 관리자)이 앱을 중지하거나 사용자가 앱을 제거할 수 있습니다.

### <a name="introduction-to-the-stages-of-startup"></a>시작 단계 소개

시작 단계에서는 많은 부분이 이동하며 모든 부분이 최상의 사용자 환경을 위해 올바르게 조정되어야 합니다. 다음 단계는 사용자의 앱 타일 클릭 및 애플리케이션의 콘텐츠 표시 사이에 발생합니다.

-   Windows 셸에서 프로세스가 시작되고 Main이 호출됩니다.
-   응용 프로그램 개체가 만들어집니다.
    -   (프로젝트 템플릿) 생성자가 InitializeComponent를 호출하면 App.xaml이 구문 분석되고 개체가 만들어집니다.
-   Application.OnLaunched 이벤트가 발생합니다.
    -   (ProjectTemplate) 앱 코드가 프레임을 만들고 MainPage로 이동합니다.
    -   (ProjectTemplate) Mainpage 생성자가 MainPage.xaml을 구문 분석하고 개체를 만드는 InitializeComponent를 호출합니다.
    -   (ProjectTemplate) Window.Current.Activate()가 호출됩니다.
-   XAML 플랫폼에서 측정 및 정렬을 포함한 레이아웃 단계가 실행됩니다.
    -   ApplyTemplate은 컨트롤 템플릿 콘텐츠가 각 컨트롤에 대해 만들어지도록 하는데 이는 일반적으로 시작에 필요한 대량의 레이아웃 시간입니다.
-   렌더가 호출되어 모든 창 콘텐츠에 대해 시각 효과를 만듭니다.
-   프레임이 DWM(데스크톱 창 관리자)에 표시됩니다.

### <a name="do-less-in-your-startup-path"></a>시작 경로에서는 되도록 적게 수행

시작 코드 경로에서 첫 번째 프레임에 필요하지 않은 것은 모두 제외합니다.

-   첫 번째 프레임 동안 필요하지 않은 컨트롤이 포함된 사용자 DLL이 있는 경우 이러한 파일의 로드를 연기하는 것이 좋습니다.
-   UI 일부가 클라우드 데이터에 종속되어 있는 경우 해당 UI를 분할합니다. 먼저 클라우드 데이터에 종속되지 않은 UI를 가져온 다음 비동기적으로 클라우드 종속 UI를 가져옵니다. 또한 애플리케이션이 오프라인으로 작동하거나 느린 네트워크 연결에 영향을 받지 않도록 로컬로 데이터를 캐시하는 것도 고려해야 합니다.
-   UI가 데이터를 기다리는 경우 진행 UI를 표시합니다.
-   코드에서 동적으로 생성되는 UI 또는 구성 파일의 많은 구문 분석과 관련된 앱 디자인에 주의해야 합니다.

### <a name="reduce-element-count"></a>요소 수 줄이기

XAML 앱의 시작 성능은 시작하는 동안 만드는 요소 수와 직접적으로 연관되어 있습니다. 요소를 적게 만들수록 앱이 시작하는 데 걸리는 시간도 줄어듭니다. 대략적인 벤치마크로 각 요소를 만드는 데 1ms가 걸린다고 간주합니다.

-   항목 컨트롤에 사용되는 템플릿에는 여러 번 반복될 때 가장 큰 영향을 미칠 수 있습니다. [ListView 및 GridView UI 최적화](optimize-gridview-and-listview.md)를 참조하세요.
-   UserControls 및 컨트롤 템플릿은 확장되므로 이러한 템플릿들도 고려해야 합니다.
-   화면에 표시되지 않는 XAML을 만드는 경우 이러한 XAML 부분이 시작 중 만들어져야 하는지 여부를 확인해야 합니다.

[Visual Studio 실시간 시각적 트리](https://devblogs.microsoft.com/visualstudio/introducing-the-ui-debugging-tools-for-xaml/) 창은 트리의 각 노드에 대해 자식 요소 수를 표시합니다.

![실시간 시각적 트리.](images/live-visual-tree.png)

**지연 사용**. 요소를 축소하거나 불투명도를 0으로 설정하면 요소가 만들어지는 것을 방지하지 않습니다. x:Load 또는 x:DeferLoadStrategy를 사용하면 UI 일부의 로드를 지연시켰다가 필요할 때 로드할 수 있습니다. 이는 시작 화면 중에 표시되지 않는 UI 처리를 지연시키는 좋은 방법이므로 필요할 때 로드하거나 지연된 논리 집합의 일부로 로드할 수 있습니다. 로드를 트리거하려면 요소의 FindName만 호출하면 됩니다. 예제 및 자세한 내용은 [x:Load attribute](../xaml-platform/x-load-attribute.md) 및 [x:DeferLoadStrategy 특성](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute)을 참조하세요.

**가상화**. UI에 목록 또는 반복기 콘텐츠가 있는 경우 UI 가상화를 사용하는 것이 좋습니다. 목록 UI가 가상화되지 않은 경우 먼저 모든 요소를 만드는 비용을 지불하고 시작을 늦출 수 있습니다. [ListView 및 GridView UI 최적화](optimize-gridview-and-listview.md)를 참조하세요.

응용 프로그램의 성능은 원시 성능에 대한 것일 뿐만 아니라 인식에 대한 것이기도 합니다. 시각적 측면이 먼저 발생하도록 작업 순서를 변경하면 사용자는 일반적으로 애플리케이션이 더 빠른 것처럼 느끼게 됩니다. 사용자는 콘텐츠가 화면에 있을 때 애플리케이션이 로드되었다고 간주합니다. 일반적으로 애플리케이션은 시작 부분에서 여러 가지 작업을 수행해야 하지만 UI를 가져오기 위해 모든 작업이 필요한 것은 아니므로 그러한 작업은 연기되거나 UI보다 낮은 우선 순위가 되어야 합니다.

이 항목에서는 애니메이션/TV에서 나오는 "첫 번째 프레임"에 대해 설명하며 이는 콘텐츠가 최종 사용자에게 보일 때까지 걸리는 시간을 측정한 것입니다.

### <a name="improve-startup-perception"></a>시작에 대한 인식 향상

단순한 온라인 게임의 예제를 사용하여 시작의 각 단계를 식별하고 다양한 방법으로 프로세스 전체에서 피드백을 사용자에게 제공해 보겠습니다. 이 예제의 경우 첫 번째 활성화 단계는 사용자가 게임 타일을 탭한 후 게임이 해당 코드를 실행하기 시작할 때까지 걸리는 시간입니다. 이 시간 동안 시스템에는 올바른 게임이 시작되었다는 것을 나타내기 위해서도 사용자에게 표시할 콘텐츠가 없습니다. 하지만 시작 화면을 제공하면 시스템에 해당 콘텐츠가 제공됩니다. 게임이 코드 실행을 시작할 때 정적 시작 화면을 자체 UI로 바꿔서 첫 번째 활성화 단계가 완료되었음을 사용자에게 알립니다.

두 번째 활성화 단계에서는 게임에 중요한 구조를 만들고 초기화합니다. 앱에서 첫 번째 활성화 단계 후에 사용 가능한 데이터로 초기 UI를 빠르게 만들 수 있으면 두 번째 단계가 쉽게 진행되고 UI를 즉시 표시할 수 있습니다. 그렇지 않으면 앱에서 초기화되는 동안 로드 페이지를 표시하는 것이 좋습니다.

로드 페이지의 모양은 개발자가 결정하며 진행률 표시줄이나 진행률 링을 표시하는 것처럼 단순할 수 있습니다. 핵심 사항은 앱이 응답 가능해지기 전에 작업을 수행하고 있음을 표시하는 것입니다. 게임의 경우 초기 화면을 표시하지만 해당 UI를 사용하려면 일부 이미지와 사운드가 디스크에서 메모리로 로드되어야 합니다. 이러한 작업에는 몇 초가 걸리므로 앱에서 시작 화면을 게임 테마에 관련된 간단한 애니메이션을 표시하는 로드 페이지로 바꿔서 사용자가 정보를 받도록 유지합니다.

세 번째 단계는 게임에 로드 페이지를 대체하는 대화형 UI를 만들기 위한 최소 정보가 포함된 후 시작됩니다. 이때 온라인 게임에 사용 가능한 정보는 앱이 디스크에서 로드한 콘텐츠뿐입니다. 게임에는 대화형 UI를 만들 만큼 충분한 콘텐츠가 함께 제공될 수 있지만 온라인 게임의 경우 인터넷에 연결되고 일부 추가 정보를 다운로드할 때까지 작동하지 않습니다. 작동하는 데 필요한 모든 정보를 확보할 때까지 사용자는 UI를 조작할 수 있지만 웹에서 추가 데이터가 필요한 기능은 콘텐츠가 아직 로드되고 있다는 피드백을 제공해야 합니다. 앱이 완벽하게 작동하는 데는 다소 시간이 걸릴 수 있으므로 최대한 빨리 기능을 사용 가능하게 만드는 것이 중요합니다.

온라인 게임의 세 개 활성화 단계를 살펴봤으므로 실제 코드에 연결해 보겠습니다.

### <a name="phase-1"></a>1단계

앱이 시작되기 전에 시작 화면으로 표시할 내용을 시스템에 알려야 합니다. 예제와 같이 앱 매니페스트의 SplashScreen 요소에 이미지와 배경색을 제공하여 이 작업을 수행합니다. 앱이 활성화를 시작한 후 이 내용이 표시됩니다.

```xml
<Package ...>
  ...
  <Applications>
    <Application ...>
      <VisualElements ...>
        ...
        <SplashScreen Image="Images\splashscreen.png" BackgroundColor="#000000" />
        ...
      </VisualElements>
    </Application>
  </Applications>
</Package>
```

자세한 내용은 [시작 화면 추가](https://docs.microsoft.com/windows/uwp/launch-resume/add-a-splash-screen)를 참조하세요.

앱 생성자를 사용하여 앱에 중요한 데이터 구조만 초기화합니다. 생성자는 앱이 처음 실행될 때만 호출되며 앱이 활성화될 때마다 호출되지는 않습니다. 예를 들어 생성자는 이미 실행되고 백그라운드에 배치된 후 검색 계약을 통해 활성화된 앱에 대해 호출되지 않습니다.

### <a name="phase-2"></a>2단계

앱을 활성화하는 다양한 이유가 있고 각 이유를 다르게 처리할 수 있습니다. [  **OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated), [**OnCachedFileUpdaterActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.oncachedfileupdateractivated), [**OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated), [**OnFileOpenPickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileopenpickeractivated), [**OnFileSavePickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfilesavepickeractivated), [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched), [**OnSearchActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsearchactivated) 및 [**OnShareTargetActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsharetargetactivated) 메서드를 재정의하여 각 활성화 이유를 처리할 수 있습니다. 앱이 이러한 메서드로 수행해야 하는 작업 중 하나는 UI를 만들고 [**Window.Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content)에 할당한 다음 [**Window.Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate)를 호출하는 것입니다. 이때 시작 화면이 앱에서 만든 UI로 바뀝니다. 이 표시는 로드 화면이 되거나, 활성화할 때 UI를 만들 충분한 정보가 있는 경우 앱의 실제 UI가 될 수 있습니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public partial class App : Application
> {
>     // A handler for regular activation.
>     async protected override void OnLaunched(LaunchActivatedEventArgs args)
>     {
>         base.OnLaunched(args);
> 
>         // Asynchronously restore state based on generic launch.
> 
>         // Create the ExtendedSplash screen which serves as a loading page while the
>         // reader downloads the section information.
>         ExtendedSplash eSplash = new ExtendedSplash();
> 
>         // Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash;
> 
>         // Notify the Window that the process of activation is completed
>         Window.Current.Activate();
>     }
> 
>     // a different handler for activation via the search contract
>     async protected override void OnSearchActivated(SearchActivatedEventArgs args)
>     {
>         base.OnSearchActivated(args);
> 
>         // Do an asynchronous restore based on Search activation
> 
>         // the rest of the code is the same as the OnLaunched method
>     }
> }
> 
> partial class ExtendedSplash : Page
> {
>     // This is the UIELement that's the game's home page.
>     private GameHomePage homePage;
> 
>     public ExtendedSplash()
>     {
>         InitializeComponent();
>         homePage = new GameHomePage();
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Public Class App
>     Inherits Application
> 
>     ' A handler for regular activation.
>     Protected Overrides Async Sub OnLaunched(ByVal args As LaunchActivatedEventArgs)
>         MyBase.OnLaunched(args)
> 
>         ' Asynchronously restore state based on generic launch.
> 
>         ' Create the ExtendedSplash screen which serves as a loading page while the
>         ' reader downloads the section information.
>         Dim eSplash As New ExtendedSplash()
> 
>         ' Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash
> 
>         ' Notify the Window that the process of activation is completed
>         Window.Current.Activate()
>     End Sub
> 
>     ' a different handler for activation via the search contract
>     Protected Overrides Async Sub OnSearchActivated(ByVal args As SearchActivatedEventArgs)
>         MyBase.OnSearchActivated(args)
> 
>         ' Do an asynchronous restore based on Search activation
> 
>         ' the rest of the code is the same as the OnLaunched method
>     End Sub
> End Class
> 
> Partial Friend Class ExtendedSplash
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' Downloading the data necessary for 
>         ' initial UI on a background thread.
>         Task.Run(Sub() DownloadData())
>     End Sub
> 
>     Private Sub DownloadData()
>         ' Download data to populate the initial UI.
> 
>         ' Create the first page. 
>         Dim firstPage As New MainPage()
> 
>         ' Add the data just downloaded to the first page
> 
>         ' Replace the loading page, which is currently 
>         ' set as the window's content, with the initial UI for the app
>         Window.Current.Content = firstPage
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class 
> ```

활성화 처리기에서 로드 페이지를 표시하는 앱은 UI를 만드는 작업을 백그라운드에서 시작합니다. 요소가 생성된 후 [**FrameworkElement.Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 이벤트가 발생합니다. 이벤트 처리기에서 현재 로드 화면을 표시하는 창의 내용을 새로 생성된 홈페이지로 바꿉니다.

초기화 기간이 긴 앱에서는 로드 페이지를 표시해야 합니다. 활성화 프로세스에 대한 사용자 피드백을 제공하는 것 외에 활성화 프로세스가 시작된 후 15초 내에 [**Window.Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate)가 호출되지 않으면 프로세스가 종료됩니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> partial class GameHomePage : Page
> {
>     public GameHomePage()
>     {
>         InitializeComponent();
> 
>         // add a handler to be called when the home page has been loaded
>         this.Loaded += ReaderHomePageLoaded;
> 
>         // load the minimal amount of image and sound data from disk necessary to create the home page.        
>     }
>     
>     void ReaderHomePageLoaded(object sender, RoutedEventArgs e)
>     {
>         // set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = this;
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Friend Class GameHomePage
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' add a handler to be called when the home page has been loaded
>         AddHandler Me.Loaded, AddressOf ReaderHomePageLoaded
> 
>         ' load the minimal amount of image and sound data from disk necessary to create the home page.        
>     End Sub
> 
>     Private Sub ReaderHomePageLoaded(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         ' set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = Me
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class
> ```

연장된 시작 화면 사용의 예를 보려면 [시작 화면 샘플](https://code.msdn.microsoft.com/windowsapps/Splash-screen-sample-89c1dc78)을 참조하세요.

### <a name="phase-3"></a>3단계

앱에서 UI를 표시했다는 것이 완벽하게 사용할 준비가 되었다는 의미는 아닙니다. 게임의 경우 UI는 인터넷의 데이터가 필요한 기능에 사용할 자리 표시자와 함께 표시됩니다. 이때 게임이 앱을 완벽하게 작동하는 데 필요한 추가 데이터를 다운로드하고 데이터를 획득함에 따라 점차 기능을 활성화합니다.

경우에 따라 활성화에 필요한 많은 콘텐츠가 앱과 함께 패키지로 제공될 수 있습니다. 단순한 게임이 이 경우에 해당합니다. 이 경우 활성화 프로세스가 매우 단순합니다. 그러나 많은 프로그램(예: 뉴스 뷰어 및 사진 뷰어)이 작동하려면 웹에서 정보를 끌어와야 합니다. 이 데이터가 용량이 크고 다운로드하는데 상당한 시간이 걸릴 수 있습니다. 앱이 활성화 프로세스 중에 이 데이터를 가져오는 방법은 앱에 대한 인식된 성능에 큰 영향을 미칠 수 있습니다.

앱이 활성화 1단계 또는 2단계에서 기능에 필요한 전체 데이터 집합을 다운로드할 경우 몇 분 동안 로드 페이지나 시작 화면을 표시할 수 있습니다. 이렇게 하면 앱이 정지된 것처럼 보이거나 시스템에서 앱을 종료할 수 있습니다. 앱이 2단계에서 자리 표시자 요소와 함께 대화형 UI를 표시할 최소한의 데이터를 다운로드한 다음 3단계에서 자리 표시자 요소를 대체할 데이터를 점차 로드하는 것이 좋습니다. 데이터 처리에 대한 자세한 내용은 [ListView 및 GridView 최적화](optimize-gridview-and-listview.md)를 참조하세요.

앱이 각 시작 단계에 정확히 반응하는 정도는 완전히 개발자에게 달려 있지만 사용자에게 가능한 많은 피드백(데이터가 로드되는 동안 시작 화면, 로드 화면, UI)을 제공하면 사용자는 앱과 시스템이 전체적으로 빠른 것처럼 느낍니다.

### <a name="minimize-managed-assemblies-in-the-startup-path"></a>시작 경로에서 관리되는 어셈블리 최소화

재사용 가능 코드는 종종 프로젝트에 포함된 모듈(DLL) 형식으로 나타납니다. 이 모듈을 로드하려면 디스크에 액세스해야 하고 예상대로 작업을 수행하는 부담이 추가될 수 있습니다. 이 부담은 콜드 부팅에 가장 큰 영향을 미치지만 웜 부팅에도 영향을 미칠 수 있습니다. C# 및 Visual Basic의 경우 CLR이 요청 시 어셈블리를 로드하는 방식으로 해당 부담을 가능한 많이 지연시킵니다. 즉, 실행된 메서드가 가리킬 때까지 CLR이 모듈을 로드하지 않습니다. 따라서 CLR이 불필요한 모듈을 로드하지 않도록 시작 코드에서 앱 실행에 필요한 어셈블리만 가리키세요. 시작 경로에 불필요한 참조가 포함된 사용되지 않는 코드가 있을 경우 이 코드 경로를 다른 메서드로 이동하여 불필요한 로드를 방지할 수 있습니다.

모듈 로드를 줄이는 다른 방법은 앱 모듈을 결합하는 것입니다. 큰 어셈블리 하나를 로드하는 것이 보통 작업 어셈블리 두 개를 로드하는 것보다 시간이 덜 걸립니다. 항상 가능하지는 않으며 개발자 생산성이나 코드 재사용 가능성에 실질적 차이를 만들지 않는 경우에만 모듈을 결합해야 합니다. [PerfView](https://www.microsoft.com/download/details.aspx?id=28567) 또는 [WPA(Windows 성능 분석기)](https://docs.microsoft.com/previous-versions/windows/desktop/xperf/windows-performance-analyzer--wpa-)와 같은 도구를 사용하여 시작 시 로드되는 모듈을 찾을 수 있습니다.

### <a name="make-smart-web-requests"></a>효율적인 웹 요청

XAML, 이미지, 앱에 중요한 기타 파일을 포함한 콘텐츠를 로컬에서 패키징함으로써 앱의 로딩 시간을 크게 단축할 수 있습니다. 디스크 작업이 네트워크 작업보다 더 빠릅니다. 앱에서 시작 시 특정 파일이 필요한 경우 파일을 원격 서버에서 검색하지 않고 디스크에서 로드하면 전체 시작 시간을 줄일 수 있습니다.

## <a name="journal-and-cache-pages-efficiently"></a>효율적인 저널 및 캐시 페이지

프레임 컨트롤이 탐색 기능을 제공합니다. 페이지(Navigate 메서드), 탐색 저널링(BackStack/ForwardStack 속성, GoForward/GoBack 메서드), 페이지 캐싱(Page.NavigationCacheMode) 및 serialization 지원(GetNavigationState 메서드)에 대한 탐색을 제공합니다.

프레임에서 알아야 하는 성능은 주로 저널링 및 페이지 캐싱에 대한 것입니다.

**프레임 저널링**. Frame.Navigate()가 있는 페이지로 이동할 때 현재 페이지의 PageStackEntry가 Frame.BackStack 컬렉션에 추가됩니다. PageStackEntry가 상대적으로 작지만 BackStack 컬렉션의 크기에는 기본 제한이 없습니다. 잠재적으로 사용자는 루프에서 탐색하므로 이 컬렉션은 무한정 커질 수 있습니다.

또한 PageStackEntry에는 Frame.Navigate() 메서드에 전달된 매개 변수도 있습니다. Frame.GetNavigationState() 메서드가 작동할 수 있으려면 매개 변수가 직렬화할 수 있는 기본 형식(예: int 또는 string)인 것이 좋습니다. 하지만 해당 매개 변수가 상당히 많은 양의 작업 집합 또는 다른 리소스를 차지하는 개체를 잠재적으로 참조하게 되면 BackStack의 각 항목을 만들 때 그만큼 더 비싼 대가를 치러야 할 수 있습니다. 예를 들어 잠재적으로 StorageFile을 매개 변수로 사용할 수 있는데 그 결과 BackStack은 무한한 수의 파일을 열어 놓게 됩니다.

그러므로 탐색 매개 변수는 작게 유지하고 BackStack의 크기는 제한하는 것이 좋습니다. BackStack은 표준 벡터(C#에서는 IList, C++/CX에서는 Platform::Vector)이므로 항목을 제거하여 간단하게 자를 수 있습니다.

**페이지 캐싱**. 기본적으로 Frame.Navigate 메서드를 사용하여 임의 페이지로 이동하면 해당 페이지의 새 인스턴스가 인스턴스화됩니다. 마찬가지로 Frame.GoBack을 사용하여 이전 페이지로 다시 이동하면 이전 페이지의 새 인스턴스가 할당됩니다.

그러나 프레임은 이러한 인스턴스화를 피할 수 있는 선택적 페이지 캐시를 제공합니다. 페이지를 캐시에 넣으려면 Page.NavigationCacheMode 속성을 사용합니다. 해당 모드를 필수로 설정하면 페이지가 강제로 캐시되고 사용으로 설정하면 페이지가 캐시되는 것을 허용합니다. 기본적으로 캐시 크기는 10페이지이지만 Frame.CacheSize 속성으로 재정의할 수 있습니다. 모든 필수 페이지가 캐시된 다음에 CacheSize 필수 페이지보다 적은 경우 사용 페이지도 캐시될 수 있습니다.

페이지 캐싱은 인스턴스화를 방지하여 성능에 도움이 되어 탐색 성능을 향상시킬 수 있습니다. 페이지 캐싱은 과도한 캐싱으로 성능에 손상을 입혀서 작업 집합에 영향을 미칠 수 있습니다.

따라서 애플리케이션에 적절하게 페이지 캐싱을 사용하는 것이 좋습니다. 예를 들어 프레임의 항목 목록을 표시하는 앱이 있다고 가정했을 때 한 항목을 탭하면 해당 항목에 대한 세부 정보 페이지로 프레임을 이동합니다. 목록 페이지는 캐시하도록 설정되어 있을 것입니다. 세부 정보 페이지가 모든 항목에 대해 동일한 경우 이 페이지 역시 캐시될 것입니다. 하지만 세부 정보 페이지의 형식이 더 다른 유형이면 캐시하지 않는 것이 더 나을 수 있습니다.
