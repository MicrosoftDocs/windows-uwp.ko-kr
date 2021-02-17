---
description: 앱의 개성과 일치 하도록 데스크톱 앱의 제목 표시줄을 사용자 지정 합니다.
title: 제목 표시줄 사용자 지정
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 제목 표시줄
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 5fdc3f6a38e6115e211eb5ea0644ad82df840301
ms.sourcegitcommit: 06d59b59a95aad009acb947a0dac7432116bdb60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100544643"
---
# <a name="title-bar-customization"></a>제목 표시줄 사용자 지정



앱이 데스크톱 창에서 실행 되는 경우 응용 프로그램의 개성과 일치 하도록 제목 표시줄을 사용자 지정할 수 있습니다. 제목 표시줄 사용자 지정 Api를 사용 하 여 제목 표시줄 요소에 대 한 색을 지정 하거나, 제목 표시줄 영역으로 앱 콘텐츠를 확장 하 고 모든 권한을 사용할 수 있습니다.

> **중요 한 api**: [applicationview. TitleBar 속성](/uwp/api/windows.ui.viewmanagement.applicationview), [applicationviewtitlebar 클래스](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [CoreApplicationViewTitleBar 클래스](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>제목 표시줄을 사용자 지정 하는 크기

제목 표시줄에 적용할 수 있는 사용자 지정 수준에는 두 가지가 있습니다.

간단한 색 사용자 지정을 위해, [Applicationviewtitlebar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 속성을 설정 하 여 제목 표시줄 요소에 사용 하려는 색을 지정할 수 있습니다. 이 경우 시스템은 앱 제목 그리기 및 끌기 가능 영역 정의와 같이 제목 표시줄의 다른 모든 측면에 대 한 책임을 유지 합니다.

다른 옵션은 기본 제목 표시줄을 숨기고 고유한 XAML 콘텐츠로 바꾸는 것입니다. 예를 들어 제목 표시줄 영역에 텍스트, 단추 또는 사용자 지정 메뉴를 놓을 수 있습니다. 또한이 옵션을 사용 하 여 [아크릴](../style/acrylic.md) 배경을 제목 표시줄 영역으로 확장 해야 합니다.

전체 사용자 지정을 선택 하는 경우 제목 표시줄 영역에 콘텐츠를 배치할 책임이 있으며, 사용자가 끌 수 있는 영역을 직접 정의할 수 있습니다. 시스템 뒤로, 닫기, 최소화 및 최대화 단추는 계속 사용할 수 있으며 시스템에서 처리 되지만 앱 제목과 같은 요소는 그렇지 않습니다. 앱에서 필요에 따라 이러한 요소를 직접 만들어야 합니다.

> [!NOTE]
> 단순 색 사용자 지정은 XAML, DirectX 및 HTML을 사용 하는 Windows 앱에서 사용할 수 있습니다. XAML을 사용 하는 Windows 앱에만 전체 사용자 지정을 사용할 수 있습니다.

## <a name="simple-color-customization"></a>단순 색 사용자 지정

제목 표시줄 색을 사용자 지정 하 고 너무 많은 작업을 수행 하지 않으려는 경우 (예: 제목 표시줄에 탭 배치) 앱 창의 [Applicationviewtitlebar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 에서 색 속성을 설정할 수 있습니다.

이 예제에서는 ApplicationViewTitleBar의 인스턴스를 가져오고 색 속성을 설정 하는 방법을 보여 줍니다.

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> 이 코드는 _App.xaml.cs_(응용 프로그램의 [onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched) 메서드)에 배치 하거나, [창](/uwp/api/windows.ui.xaml.window.Activate)에 대 한 호출 후, 또는 앱의 첫 페이지에서 지정할 수 있습니다.

> [!TIP]
> Windows 커뮤니티 도구 키트는 XAML에서 이러한 색 속성을 설정할 수 있도록 하는 확장을 제공 합니다. 자세한 내용은 [Windows 커뮤니티 도구 키트 설명서](/windows/uwpcommunitytoolkit/extensions/viewextensions)를 참조 하세요.

제목 표시줄 색을 설정할 때 알아야 할 몇 가지 사항이 있습니다.

- 단추 배경색은 닫기 단추 가리키기 및 누름 상태에 적용 되지 않습니다. 닫기 단추는 항상 해당 상태에 대 한 시스템 정의 색을 사용 합니다.
- 단추 색 속성은 사용 시 시스템 뒤로 단추에 적용 됩니다. [탐색 기록 및 역방향 탐색을 참조](../basics/navigation-history-and-backwards-navigation.md)하세요.
- Color 속성을 **null** 로 설정 하면이를 기본 시스템 색으로 다시 설정 합니다.
- 투명 색은 설정할 수 없습니다. 색의 알파 채널이 무시 됩니다.
- 예를 들어 색 필터 또는 고대비 모드와 같은 설정으로 인해 화면의 색은 선택 항목과 다를 수 있습니다. 중요 한 정보를 전달 하기 위해 색에만 의존해 서는 안 됩니다.

Windows에서는 제목 표시줄에 선택한 [강조 색](../style/color.md#accent-color) 을 적용할 수 있는 옵션을 사용자에 게 제공 합니다. 제목 표시줄 색을 설정 하는 경우 모든 색을 명시적으로 설정 하는 것이 좋습니다. 이렇게 하면 사용자 정의 색 설정으로 인해 발생 하는 의도 하지 않은 색 조합이 발생 하지 않습니다.

## <a name="full-customization"></a>전체 사용자 지정

전체 제목 표시줄 사용자 지정을 옵트인 (opt in) 할 때 응용 프로그램의 클라이언트 영역이 제목 표시줄 영역을 포함 하 여 전체 창을 포함 하도록 확장 됩니다. 응용 프로그램 캔버스의 맨 위에 겹쳐서 표시 되는 캡션 단추를 제외 하 고 전체 창의 그리기 및 입력 처리를 담당 합니다.

기본 제목 표시줄을 숨기고 콘텐츠를 제목 표시줄 영역으로 확장 하려면 [CoreApplicationViewTitleBar ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) 속성을 **true** 로 설정 합니다.

이 예제에서는 CoreApplicationViewTitleBar를 가져오고 ExtendViewIntoTitleBar 속성을 **true** 로 설정 하는 방법을 보여 줍니다. 앱의 [onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched) 메서드 (_App.xaml.cs_) 또는 앱의 첫 페이지에서 수행할 수 있습니다.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> 이 설정은 앱을 닫았다가 다시 시작할 때 유지 됩니다. Visual Studio에서 ExtendViewIntoTitleBar를 **true** 로 설정 하 고 기본으로 되돌리려면 명시적으로 **false** 로 설정 하 고 응용 프로그램을 실행 하 여 지속형 설정을 덮어써야 합니다.

### <a name="draggable-regions"></a>끌기 가능 영역

제목 표시줄의 끌기 가능 영역에 따라 사용자가 클릭 하 여 창을 이동할 수 있는 위치가 정의 됩니다 (단순히 앱의 캔버스 내에서 콘텐츠를 끄는 것이 아님). [창. SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) 메서드를 호출 하 고 끌기 가능 영역을 정의 하는 UIElement를 전달 하 여 끌기 가능 영역을 지정 합니다. (일반적으로 UIElement는 다른 요소를 포함 하는 패널입니다.)

콘텐츠 그리드를 끌기 가능 제목 표시줄 영역으로 설정 하는 방법은 다음과 같습니다. 이 코드는 응용 프로그램의 첫 페이지에 대 한 XAML 및 코드 숨김으로 이동 합니다. 전체 코드는 [전체 사용자 지정 예제](./title-bar.md#full-customization-example) 섹션을 참조 하세요.


> [!IMPORTANT]
> 기본적으로 Grid와 같은 일부 UI 요소는 배경 집합이 없는 경우 적중 테스트에 참여 하지 않습니다.
> 이러한 `AppTitleBar` 끌기를 허용 하려면 아래 샘플의 표에 대해 배경을로 설정 해야 `Transparent` 합니다.

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;
    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    AppTitleBar.Height = sender.Height;
}
```

UIElement ( `AppTitleBar` )는 앱에 대 한 XAML의 일부입니다. 변경 되지 않는 루트 페이지에서 제목 표시줄을 선언 하 고 설정 하거나, 앱이 탐색할 수 있는 각 페이지에서 제목 표시줄 영역을 선언 하 고 설정할 수 있습니다. 각 페이지에서 설정 하는 경우 사용자가 앱을 탐색할 때 끌기 가능 지역이 일관 되 게 유지 되도록 해야 합니다.

앱이 실행 되는 동안 새 제목 표시줄 요소로 전환 하려면 SetTitleBar를 호출할 수 있습니다. 또한 **null** 을 매개 변수로 전달 하 여 기본 끌기 동작으로 되돌릴 수 있습니다. 자세한 내용은 "기본 끌 영역"을 참조 하세요.

> [!IMPORTANT]
> 지정 하는 끌기 가능한 영역은 적중 테스트를 수행 해야 합니다. 즉, 일부 요소의 경우 투명 한 배경 브러시를 설정 해야 할 수도 있습니다. 자세한 내용은 VisualTreeHelper에 대 한 설명을 참조 하십시오 [.](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)
>
>예를 들어, 그리드를 끌기 가능 지역으로 정의 하는 경우 `Background="Transparent"` 이를 끌기 가능 하도록 설정 합니다.
>
>이 그리드는 드래그할 수 없지만, 그 안에 표시 되는 요소는 `<Grid x:Name="AppTitleBar">` 입니다.
>
>이 그리드는 동일 하지만 전체 그리드를 끌 수 `<Grid x:Name="AppTitleBar" Background="Transparent">` 있습니다.

#### <a name="default-draggable-region"></a>기본 끌기 가능 영역

끌 수 있는 영역을 지정 하지 않으면 창의 너비, 캡션 단추의 높이 및 창의 위쪽 가장자리를 따라 배치 되는 사각형을 기본 끌기 가능 영역으로 설정 합니다.

끌기 가능 영역을 정의 하는 경우 시스템에서는 기본 끌기 가능 영역을 캡션 단추의 왼쪽 (캡션 단추가 창의 왼쪽에 있는 경우 오른쪽)의 왼쪽에 배치 된 작은 영역으로 축소 합니다. 이렇게 하면 사용자가 끌 수 있는 일관 된 영역을 항상 사용할 수 있습니다.

### <a name="system-caption-buttons"></a>시스템 캡션 단추

시스템은 시스템 캡션 단추 (뒤로, 최소화, 최대화, 닫기)에 대 한 앱 창의 왼쪽 위 또는 오른쪽 위 모퉁이를 예약 합니다. 시스템은 창에서 끌기, 최소화, 최대화 및 닫기에 대 한 최소 기능이 제공 되도록 하기 위해 캡션 컨트롤 영역에 대 한 제어를 유지 합니다. 시스템에서는 왼쪽에서 오른쪽으로의 언어와 오른쪽에서 왼쪽으로 쓰기 언어의 경우 왼쪽 위에 닫기 단추를 그립니다.

제목 표시줄 UI의 레이아웃에서 사용할 수 있도록 CoreApplicationViewTitleBar 클래스에서 캡션 컨트롤 영역의 크기와 위치를 전달 합니다. 각 면의 예약 된 영역 너비는 [SystemOverlayLeftInset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) 또는 [SystemOverlayRightInset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) 속성으로 제공 되며 [height 속성에 따라 높이가 지정](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height) 됩니다.

응용 프로그램 배경과 같은 이러한 속성으로 정의 된 캡션 컨트롤 영역 아래에 콘텐츠를 그릴 수 있지만 사용자가 상호 작용할 수 있는 것으로 간주 되는 UI는 넣지 말아야 합니다. 캡션 컨트롤의 입력이 시스템에서 처리 되기 때문에 입력을 받지 않습니다.

[LayoutMetricsChanged](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) 이벤트를 처리 하 여 캡션 단추 크기의 변경 내용에 응답할 수 있습니다. 예를 들어 시스템 뒤로 단추가 표시 되거나 숨겨질 때 이러한 현상이 발생할 수 있습니다. 이 이벤트를 처리 하 여 제목 표시줄 크기에 따라 달라 지는 UI 요소의 위치를 확인 하 고 업데이트 합니다.

이 예제에서는 시스템 뒤로 단추 표시 또는 숨기기와 같은 변경 내용을 고려 하 여 제목 표시줄의 레이아웃을 조정 하는 방법을 보여 줍니다. `AppTitleBar`, `LeftPaddingColumn` 및 `RightPaddingColumn` 은 이전에 표시 된 XAML에서 선언 됩니다.

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>대화형 콘텐츠

응용 프로그램의 위쪽 부분에 단추, 메뉴 또는 검색 상자와 같은 대화형 컨트롤을 추가 하 여 제목 표시줄에 표시 되도록 할 수 있습니다. 그러나 대화형 요소가 사용자 입력을 받도록 하기 위해 수행 해야 하는 몇 가지 규칙이 있습니다.
- 영역을 끌기 가능 제목 표시줄 영역으로 정의 하려면 SetTitleBar를 호출 해야 합니다. 그렇지 않으면 시스템은 페이지 맨 위에 있는 기본 끌기 가능 영역을 설정 합니다. 그러면 시스템이이 영역에 대 한 모든 사용자 입력을 처리 하 고 입력이 컨트롤에 도달 하지 않도록 합니다.
- SetTitleBar (z 순서는 더 높음) 호출로 정의 된 끌기 가능 영역의 맨 위에 대화형 컨트롤을 배치 합니다. 설정 된 UIElement의 대화형 컨트롤 자식을 설정 하지 마세요. 요소를 SetTitleBar에 전달 하면 시스템에서 시스템 제목 표시줄 처럼 처리 하 고 해당 요소에 대 한 모든 포인터 입력을 처리 합니다.

여기에서 `TitleBarButton` 요소의 z 순서는 보다 높기 `AppTitleBar` 때문에 사용자 입력이 수신 됩니다.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>캡션 단추의 투명도

ExtendViewIntoTitleBar를 **true** 로 설정 하면 캡션 단추의 배경을 투명 하 게 만들어 앱 배경을 통해 표시 되도록 할 수 있습니다. 일반적으로 배경을 색으로 설정 [합니다.](/uwp/api/windows.ui.colors.Transparent) 전체 투명도를 위해 투명 합니다. 부분 투명도의 경우 속성을 설정 하는 [색](/uwp/api/windows.ui.color) 에 대 한 알파 채널을 설정 합니다.

이러한 ApplicationViewTitleBar 속성은 다음과 같이 투명 합니다.

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

단추 배경색은 닫기 단추 가리키기 및 누름 상태에 적용 되지 않습니다. 닫기 단추는 항상 해당 상태에 대 한 시스템 정의 색을 사용 합니다.

다른 모든 색 속성은 알파 채널을 계속 무시 합니다. ExtendViewIntoTitleBar을 **false** 로 설정 하면 모든 ApplicationViewTitleBar 색 속성에 대해 알파 채널이 항상 무시 됩니다.

### <a name="full-screen-and-tablet-mode"></a>전체 화면 및 태블릿 모드

앱이 _전체 화면_ 또는 _태블릿 모드_ 에서 실행 되 면 시스템에서 제목 표시줄과 캡션 컨트롤 단추를 숨깁니다. 그러나 사용자는 제목 표시줄을 호출 하 여 앱 UI 위에 오버레이로 표시 되도록 할 수 있습니다.
제목 표시줄이 숨겨지거나 호출 될 때 알리도록 [CoreApplicationViewTitleBar IsVisibleChanged](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) 이벤트를 처리 하 고 필요에 따라 사용자 지정 제목 표시줄 콘텐츠를 표시 하거나 숨길 수 있습니다.

이 예제에서는 이전에 표시 된 요소를 표시 하 고 숨기기 위해 IsVisibleChanged를 처리 하는 방법을 보여 줍니다 `AppTitleBar` .

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>_전체 화면_ 모드는 앱에서 지원 되는 경우에만 입력할 수 있습니다. 자세한 정보는 [Applicationview. IsFullScreenMode](/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode) 를 참조 하세요. [_태블릿 모드_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) 는 지원 되는 하드웨어에 대 한 사용자 옵션 이므로 사용자가 태블릿 모드에서 앱을 실행 하도록 선택할 수 있습니다.

## <a name="full-customization-example"></a>전체 사용자 지정 예제

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 창이 활성 상태 이거나 비활성 상태 이면이를 명확 하 게 합니다. 최소한 제목 표시줄에서 텍스트, 아이콘 및 단추의 색을 변경 합니다.
- 앱 캔버스의 위쪽 가장자리를 따라 끌 있는 영역을 정의 합니다. 시스템 제목 표시줄의 배치를 일치 하면 사용자가 보다 쉽게 찾을 수 있습니다.
- 앱의 캔버스에서 시각적 제목 표시줄 (있는 경우)과 일치 하는 끌기 가능 영역을 정의 합니다.

## <a name="related-articles"></a>관련 문서

- [아크릴](../style/acrylic.md)
- [색상](../style/color.md)
