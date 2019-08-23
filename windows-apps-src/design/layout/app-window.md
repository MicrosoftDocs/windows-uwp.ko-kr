---
Description: AppWindow 클래스를 사용 하 여 별도의 창에서 앱의 서로 다른 부분을 볼 수 있습니다.
title: AppWindow 클래스를 사용 하 여 앱에 대 한 보조 창 표시
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b89d9100157cf40266bb983e258aa187f65dc93
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867459"
---
# <a name="show-multiple-views-with-appwindow"></a>AppWindow를 사용 하 여 여러 뷰 표시

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 및 관련 된 api를 사용 하면 각 창에서 동일한 UI 스레드를 계속 사용 하면서 보조 창에 앱 콘텐츠를 표시할 수 있으므로 다중 창 앱을 간편 하 게 만들 수 있습니다.

> [!NOTE]
> AppWindow는 현재 미리 보기로 제공 됩니다. 즉, AppWindow를 사용 하는 앱을 저장소에 제출할 수 있지만, 일부 플랫폼 및 프레임 워크 구성 요소는 AppWindow에서 작동 하지 않는 것으로 알려져 있습니다 ( [제한 사항](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)참조).

여기서는 샘플 앱 `HelloAppWindow`이 있는 여러 창에 대 한 몇 가지 시나리오를 보여 줍니다. 샘플 앱은 다음과 같은 기능을 보여 줍니다.

- 주 페이지에서 컨트롤의 도킹을 해제 하 고 새 창에서 엽니다.
- 새 창에서 페이지의 새 인스턴스를 엽니다.
- 앱에서 프로그래밍 방식으로 새 창의 크기를 조정 하 고 위치를 조정 합니다.
- 콘텐츠 대화 상자를 앱의 적절 한 창에 연결 합니다.

![단일 창이 있는 샘플 앱](images/hello-app-window-single.png)
  
> _단일 창이 있는 샘플 앱_

![도킹 되지 않은 색 선택 및 보조 윈도를 사용 하는 샘플 앱](images/hello-app-window-multi.png)

> _도킹 되지 않은 색 선택 및 보조 윈도를 사용 하는 샘플 앱_

> **중요 API**: [Windows. UI. WindowManagement 네임 스페이스](/uwp/api/windows.ui.windowmanagement), [appwindow 클래스](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>API 개요

[Windowmanagement](/uwp/api/windows.ui.windowmanagement) 네임 스페이스의 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 클래스 및 기타 api는 Windows 10 버전 1903 (SDK 18362)부터 사용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 [ApplicationView를 사용 하 여 보조 창을 만들어야](application-view.md)합니다. WindowManagement Api는 아직 개발 중 이며 API 참조 문서에 설명 된 것과 같은 [제한](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) 사항이 있습니다.

다음은 AppWindow에서 콘텐츠를 표시 하는 데 사용 하는 몇 가지 중요 한 Api입니다.

### <a name="appwindow"></a>AppWindow

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 클래스를 사용 하 여 Windows 런타임 앱의 일부를 보조 창에 표시할 수 있습니다. 이는 응용 프로그램 [보기](/uwp/api/windows.ui.viewmanagement.applicationview)와 비슷하지만 동작 및 수명에서 동일 하지는 않습니다. AppWindow의 주요 기능은 각 인스턴스가 생성 된 것과 동일한 UI 처리 스레드 (이벤트 디스패처 포함)를 공유 하 여 다중 창 앱을 간소화 하는 것입니다.

AppWindow에만 XAML 콘텐츠를 연결할 수 있으며, 네이티브 DirectX 또는 Holographic 콘텐츠를 지원 하지 않습니다. 그러나 DirectX 콘텐츠를 호스팅하는 XAML [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 를 표시할 수 있습니다.

### <a name="windowingenvironment"></a>WindowingEnvironment

[Windowingenvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) API를 사용 하면 앱이 제공 되는 환경을 파악 하 여 필요에 따라 앱을 조정할 수 있습니다. 환경에서 지 원하는 창의 종류를 설명 합니다. 예를 `Overlapped` 들어 앱이 PC에서 실행 되는 경우 또는 `Tiled` 앱이 Xbox에서 실행 되는 경우입니다. 또한 앱이 논리적으로 표시 될 수 있는 영역을 설명 하는 DisplayRegion 개체 집합을 제공 합니다.

### <a name="displayregion"></a>DisplayRegion

[Displayregion](/uwp/api/windows.ui.windowmanagement.displayregion) API는 보기를 논리 표시의 사용자에 게 표시할 수 있는 영역을 설명 합니다. 예를 들어 데스크톱 PC에서이는 작업 표시줄의 전체 표시를 뺀 값입니다. 지원 모니터의 물리적 표시 영역에 대 한 1:1 매핑이 반드시 필요 하지는 않습니다. 동일한 모니터 내에 표시 영역이 여러 개 있을 수 있으며, 이러한 모니터가 모든 측면에서 동일 하면 여러 모니터에 걸쳐 있는 DisplayRegion을 구성할 수 있습니다.

### <a name="appwindowpresenter"></a>AppWindowPresenter

[Appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) API를 사용 하면 windows를 또는 `FullScreen` `CompactOverlay`와 같은 미리 정의 된 구성으로 쉽게 전환할 수 있습니다. 이러한 구성은 구성을 지 원하는 모든 장치에서 사용자에 게 일관 된 환경을 제공 합니다.

### <a name="uicontext"></a>UIContext

[Uicontext](/uwp/api/windows.ui.uicontext) 는 앱 창이 나 보기의 고유 식별자입니다. 자동으로 만들어지므로 [UIElement. uicontext](/uwp/api/windows.ui.xaml.uielement.uicontext) 속성을 사용 하 여 uicontext를 검색할 수 있습니다. XAML 트리의 모든 UIElement에는 동일한 UIContext가 있습니다.

 [창과](/uwp/api/Windows.UI.Xaml.Window.Current) 패턴은 `GetForCurrentView` 작업에 사용할 단일 XAML 트리를 사용 하 여 단일 applicationview/CoreWindow를 사용 하는 것 이므로 uicontext는 중요 합니다. 이는 AppWindow를 사용 하는 경우에 해당 되지 않으므로 UIContext를 사용 하 여 특정 창을 대신 식별할 수 있습니다.

### <a name="xamlroot"></a>XamlRoot

[Xamlroot](/uwp/api/windows.ui.xaml.xamlroot) 클래스는 XAML 요소 트리를 보유 하 고, 창 호스트 개체 (예: [Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 또는 [applicationview](/uwp/api/windows.ui.viewmanagement.applicationview))에 연결 하 고, 크기 및 표시 유형과 같은 정보를 제공 합니다. XamlRoot 개체를 직접 만들지 않습니다. 대신 AppWindow에 XAML 요소를 연결할 때 하나를 만듭니다. 그런 다음 [UIElement. xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 속성을 사용 하 여 xamlroot를 검색할 수 있습니다.

UIContext 및 XamlRoot에 대 한 자세한 내용은 창 고 지 [호스트에서 코드를 이식 가능 하 게 만들기](show-multiple-views.md#make-code-portable-across-windowing-hosts)를 참조 하세요.

## <a name="show-a-new-window"></a>새 창 표시

새 AppWindow에 콘텐츠를 표시 하는 단계를 살펴보겠습니다.

**새 창을 표시 하려면**

1. 정적 [Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync) 메서드를 호출 하 여 새 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)를 만듭니다.

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. 창 콘텐츠를 만듭니다.

    일반적으로 XAML [프레임](/uwp/api/Windows.UI.Xaml.Controls.Frame)을 만든 다음, 프레임을 앱 콘텐츠를 정의한 xaml [페이지로](/uwp/api/Windows.UI.Xaml.Controls.Page) 이동 합니다. 프레임 및 페이지에 대 한 자세한 내용은 [두 페이지 간 피어 투 피어 탐색](../basics/navigate-between-two-pages.md)을 참조 하세요.

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    그러나 모든 XAML 콘텐츠는 프레임과 페이지 뿐만 아니라 AppWindow에서 표시할 수 있습니다. 예를 들어 [Colorpicker](/uwp/api/windows.ui.xaml.controls.colorpicker)와 같은 단일 컨트롤을 표시 하거나 DirectX 콘텐츠를 호스트 하는 [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 을 표시할 수 있습니다.

1. [ElementCompositionPreview](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) 을 호출 하 여 XAML 콘텐츠를 appwindow에 연결 합니다.

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    이 메서드에 대 한 호출은 [xamlroot](/uwp/api/windows.ui.xaml.xamlroot) 개체를 만들고 지정 된 UIElement의 [xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 속성으로 설정 합니다.

    AppWindow 인스턴스당 한 번만이 메서드를 호출할 수 있습니다. 콘텐츠를 설정한 후에는이 AppWindow 인스턴스의 SetAppWindowContent에 대 한 추가 호출이 실패 합니다. 또한 null UIElement 개체를 전달 하 여 AppWindow 콘텐츠의 연결을 해제 하려고 하면 호출이 실패 합니다.

1. [Appwindow. TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) 메서드를 호출 하 여 새 창을 표시 합니다.

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>창이 닫힐 때 리소스를 해제 합니다.

항상 [appwindow. Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed) 이벤트를 처리 하 여 XAML 리소스 (appwindow 콘텐츠)와 appwindow에 대 한 참조를 해제 해야 합니다.

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>AppWindow의 인스턴스 추적

앱에서 여러 창을 사용 하는 방법에 따라 사용자가 만드는 AppWindow의 인스턴스를 추적 하거나 추적 하지 않아도 됩니다. 이 `HelloAppWindow`예에서는 일반적으로 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)를 사용할 수 있는 몇 가지 다른 방법을 보여 줍니다. 여기서는 이러한 창을 추적 해야 하는 이유와이를 수행 하는 방법을 살펴보겠습니다.

### <a name="simple-tracking"></a>간단한 추적

색 선택 창은 단일 XAML 컨트롤을 호스팅하고, 색 선택기와 상호 작용 하는 코드는 모두 `MainPage.xaml.cs` 파일에 상주 합니다. 색 선택 창은 단일 인스턴스만 허용 하며 기본적으로의 `MainWindow`확장입니다. 하나의 인스턴스만 생성 되도록 하려면 페이지 수준 변수를 사용 하 여 색 선택 창을 추적 합니다. 새 색 선택 창을 만들기 전에 인스턴스가 있는지 여부를 확인 하 고, 인스턴스가 있는지 확인 하 고, 새 창을 만드는 단계를 건너뛰고 기존 창에서 [Tryshowasync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) 를 호출 하면 됩니다.

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>호스팅된 콘텐츠에서 AppWindow 인스턴스 추적

창은 전체 XAML 페이지를 호스팅하고 페이지와 상호 작용 하는 코드는에 `AppWindowPage.xaml.cs`있습니다. `AppWindowPage` 각각 독립적으로 작동 하는 여러 인스턴스를 허용 합니다.

페이지의 기능을 사용 하 여 창을 조작 하 고, 또는 `FullScreen` `CompactOverlay`로 설정 하 고, [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow.changed) 를 수신 대기 하 고, 창에 대 한 정보를 표시 하는 이벤트를 변경할 수 있습니다. 이러한 api를 호출 하려면를 `AppWindowPage` 호스트 하는 appwindow 인스턴스에 대 한 참조가 필요 합니다.

필요한 경우에서 `AppWindowPage` 속성을 만들고 해당 속성을 만들 때 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 인스턴스를 할당할 수 있습니다.

**AppWindowPage.xaml.cs**

에서 `AppWindowPage`appwindow 참조를 저장할 속성을 만듭니다.

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

에서 `MainPage`페이지 인스턴스에 대 한 참조를 가져오고 새로 만든 appwindow를의 `AppWindowPage`속성에 할당 합니다.

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>UIContext를 사용 하 여 앱 창 추적

응용 프로그램의 다른 부분에서 [Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 인스턴스에 액세스할 수도 있습니다. 예를 들어 `MainPage` 에는 appwindow의 추적 된 인스턴스를 모두 닫는 ' 모두 닫기 ' 단추가 있을 수 있습니다.

이 경우 [Uicontext](/uwp/api/windows.ui.uicontext) 고유 식별자를 사용 하 여 [사전](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0)에 있는 창 인스턴스를 추적 해야 합니다.

**MainPage.xaml.cs**

에서 `MainPage`사전을 정적 속성으로 만듭니다. 그런 다음 페이지를 만들 때 사전에 추가 하 고 페이지가 닫히면 제거 합니다. [ElementCompositionPreview](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)를 호출한 후에 콘텐츠 [프레임](/uwp/api/Windows.UI.Xaml.Controls.Frame) (`appWindowContentFrame.UIContext`)에서 uicontext를 가져올 수 있습니다.

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

`AppWindowPage` 코드에서 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 인스턴스를 사용 하려면 페이지의 [uicontext](/uwp/api/windows.ui.uicontext) 를 사용 하 여의 `MainPage`정적 사전에서 해당 인스턴스를 검색 합니다. UIContext가 null이 되지 않도록 생성자 대신 페이지의 [로드](/uwp/api/windows.ui.xaml.frameworkelement.loaded) 된 이벤트 처리기에서이 작업을 수행 해야 합니다. 페이지에서 UIContext를 가져올 수 있습니다 `this.UIContext`.

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> 이 예제에서는에서 `AppWindowPage`창을 추적 하는 두 가지 방법을 모두 보여 주지만 일반적으로 둘 중 하나만 사용 합니다. `HelloAppWindow`

## <a name="request-window-size-and-placement"></a>요청 창 크기 및 배치

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 클래스에는 창의 크기와 배치를 제어 하는 데 사용할 수 있는 여러 메서드가 있습니다. 메서드 이름에서 암시 하는 것 처럼 시스템은 환경 요인에 따라 요청 된 변경을 허용할 수도 있고 그렇지 않을 수도 있습니다.

[Requestsize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) 를 호출 하 여 다음과 같이 원하는 창 크기를 지정 합니다.

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

창 배치를 관리 하는 방법에는 _Requestmove *_ 라는 이름이 지정 됩니다. [RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview), [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow), [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion), [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion).

이 예제에서이 코드는 창이 생성 된 주 뷰 옆으로 창을 이동 합니다.

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

창의 현재 크기 및 배치에 대 한 정보를 가져오려면 [Getplacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement)를 호출 합니다. 그러면 창의 현재 [Displayregion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion), [오프셋](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)및 [크기](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size) 를 제공 하는 [appwindowplacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement) 개체가 반환 됩니다.

예를 들어이 코드를 호출 하 여 창을 디스플레이의 오른쪽 위 모퉁이로 이동할 수 있습니다. 이 코드는 창이 표시 된 후에 호출 해야 합니다. 그렇지 않으면 GetPlacement 호출에서 반환 되는 창 크기가 0, 0이 되며 오프셋이 올바르지 않습니다.

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>프레젠테이션 구성 요청

[Appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) 클래스를 사용 하면 표시 되는 장치에 적합 한 미리 정의 된 구성을 사용 하 여 [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 를 표시할 수 있습니다. [AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) 값을 사용 하 여 또는 `FullScreen` `CompactOverlay` 모드에서 창을 저장할 수 있습니다.

이 예제에서는 다음을 수행 하는 방법을 보여 줍니다.

- 사용 가능한 창 프레젠테이션이 변경 될 경우 알리도록 [Appwindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) 이벤트를 사용 합니다.
- [Appwindow. presenter](/uwp/api/windows.ui.windowmanagement.appwindow.presenter) 속성을 사용 하 여 현재 [appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)를 가져옵니다.
- [IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported) 를 호출 하 여 특정 [AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) 지원 되는지 확인 합니다.
- [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration) 을 호출 하 여 현재 사용 중인 구성 종류를 확인 합니다.
- [Requestpresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation) 를 호출 하 여 현재 구성을 변경 합니다.

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>XAML 요소 다시 사용

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 를 사용 하면 동일한 UI 스레드가 있는 여러 XAML 트리를 사용할 수 있습니다. 그러나 xaml 요소는 XAML 트리에 한 번만 추가할 수 있습니다. UI의 일부를 한 창에서 다른 창으로 이동 하려면 XAML 트리에서 배치를 관리 해야 합니다.

이 예제에서는 기본 창과 보조 창 간에 이동 하는 동안 [Colorpicker](/uwp/api/windows.ui.xaml.controls.colorpicker) 컨트롤을 다시 사용 하는 방법을 보여 줍니다.

색 선택은 xaml에 선언 `MainPage`되어 `MainPage` xaml 트리에 배치 됩니다.

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

색 선택이 새 appwindow에 배치 되도록 분리 된 경우 먼저 부모 컨테이너에서 제거 하 여 `MainPage` XAML 트리에서 제거 해야 합니다. 필수는 아니지만이 예제에서는 부모 컨테이너도 숨깁니다.

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

그런 다음 새 XAML 트리에 추가할 수 있습니다. 여기서는 먼저 ColorPicker의 부모 컨테이너가 될 [그리드](/uwp/api/windows.ui.xaml.controls.grid) 를 만들고 Colorpicker를 표의 자식으로 추가 합니다. 이를 통해 나중에이 XAML 트리에서 ColorPicker를 쉽게 제거할 수 있습니다. 그런 다음 새 창에서 표를 XAML 트리의 루트로 설정 합니다.

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) 가 닫히면 프로세스를 반대로 합니다. 먼저 [표에서](/uwp/api/windows.ui.xaml.controls.grid) [colorpicker](/uwp/api/windows.ui.xaml.controls.colorpicker) 를 제거한 다음에서 `MainPage` [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) 의 자식으로 추가 합니다.

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>대화 상자 표시

기본적으로 콘텐츠 대화 상자는 루트 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)를 기준으로 모달 형식으로 표시됩니다. [Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)내에서 [contentdialog](/uwp/api/windows.ui.xaml.controls.contentdialog) 를 사용 하는 경우 대화 상자의 xamlroot를 XAML 호스트의 루트로 수동으로 설정 해야 합니다.

이렇게 하려면 ContentDialog의 [xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 속성을 appwindow에 이미 있는 요소와 동일한 [xamlroot](/uwp/api/windows.ui.xaml.xamlroot) 로 설정 합니다. 여기서는이 코드가 단추의 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트 처리기 내에 있으므로 _발신자_ (클릭 한 단추)를 사용 하 여 xamlroot를 가져올 수 있습니다.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

주 창 (ApplicationView) 외에 하나 이상의 AppWindows가 열려 있는 경우에는 모달 대화 상자에서 루트가 있는 창만 차단 하기 때문에 각 창에서 대화 상자를 열려고 시도할 수 있습니다. 그러나 한 번에 하나의 [Contentdialog](/uwp/api/windows.ui.xaml.controls.contentdialog) 만 열 수 있습니다. 두 ContentDialog를 열려는 하면 별도의 AppWindow에서 열려고 하더라도 예외가 throw됩니다.

이를 관리 하려면 다른 대화 상자가 이미 열려 있는 경우 예외를 catch `try/catch` 하기 위해 블록에서 대화 상자를 최소한으로 열어야 합니다.

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

대화 상자를 관리 하는 또 다른 방법은 현재 열려 있는 대화 상자를 추적 하 고 새 대화 상자를 열기 전에 닫는 것입니다. 여기서는이 용도로 호출 `MainPage` `CurrentDialog` 된에서 정적 속성을 만듭니다.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

그런 다음 현재 열려 있는 대화 상자가 있는지 여부를 확인 하 고, 있는 경우 [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide) 메서드를 호출 하 여 닫습니다. 마지막으로에 `CurrentDialog`새 대화 상자를 할당 하 고 표시를 시도 합니다.

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

대화 상자를 프로그래밍 방식으로 닫지 않는 것이 바람직하지 않은 경우로 `CurrentDialog`할당 하지 마십시오. 여기서는 `MainPage` 사용을 클릭할 `Ok`때만 해제 해야 하는 중요 한 대화 상자를 보여 줍니다. 로 `CurrentDialog`할당 되지 않았기 때문에 프로그래밍 방식으로 닫도록 시도 하지 않습니다.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>전체 코드

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage .xaml

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>관련 항목

- [여러 뷰 표시](show-multiple-views.md)
- [ApplicationView를 사용 하 여 여러 뷰 표시](application-view.md)
