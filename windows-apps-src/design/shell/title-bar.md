---
author: jwmsft
description: 앱의 퍼스낼리티와 일치하도록 데스크톱 앱의 제목 표시줄을 사용자 지정하세요.
title: 제목 표시줄 사용자 지정
template: detail.hbs
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, uwp, 제목 표시줄
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 2ebe590f98afef031ab183589fc7dcfc29cd9493
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7419079"
---
# <a name="title-bar-customization"></a>제목 표시줄 사용자 지정



앱이 데스크톱 창에서 실행될 때 앱의 퍼스낼리티와 일치하도록 제목 표시줄을 사용자 지정할 수 있습니다. 제목 표시줄 사용자 지정 API를 사용하여 제목 표시줄 요소의 색상을 지정하거나 앱 콘텐츠를 제목 표시줄 영역으로 확장하고 모든 내용을 제어할 수 있습니다.

> **중요 API**: [ApplicationView.TitleBar 속성](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview), [ApplicationViewTitleBar 클래스](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [CoreApplicationViewTitleBar 클래스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>제목 표시줄의 사용자 지정 수준

제목 표시줄에 두 가지 수준으로 사용자 지정을 적용할 수 있습니다.

간단한 색상을 사용자 지정하는 경우 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 속성을 설정하여 제목 표시줄 요소에 사용할 색상을 지정할 수 있습니다. 이 경우 시스템은 앱 이름 그리기 및 끌기 가능한 영역 정의와 같은 제목 표시줄의 다른 모든 측면에 대한 책임을 유지합니다.

다른 옵션은 기본 제목 표시줄을 숨기고 자체 XAML 콘텐츠로 바꾸는 것입니다. 예를 들어, 제목 표시줄 영역에 텍스트, 버튼 또는 사용자 지정 메뉴를 배치할 수 있습니다. [아크릴](../style/acrylic.md) 배경을 제목 표시줄 영역으로 확장하려면 이 옵션을 사용해야 합니다.

전체 사용자 지정을 선택하면 제목 표시줄 영역에 콘텐츠를 배치해야 하며 자체 끌기 가능 영역을 정의할 수 있습니다. 시스템 뒤로, 닫기, 최소화 및 최대화 버튼은 계속 사용할 수 있고 시스템에서 처리되지만 앱 이름과 같은 요소는 지원되지 않습니다. 앱에서 필요에 따라 직접 요소를 만들어야 합니다.

> [!NOTE]
> 간단한 색상 사용자 지정은 XAML, DirectX 및 HTML을 사용하는 UWP 앱에서 사용할 수 있습니다. 전체 사용자 지정은 XAML을 사용하는 UWP 앱에서만 사용할 수 있습니다.

## <a name="simple-color-customization"></a>간단한 색상 사용자 지정

제목 표시줄의 색상만 사용자 지정하려고 하며, 제목 표시줄에 탭을 배치하는 것과 같은 고급 작업은 수행하지 않으려면 앱 창에 대해 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar)의 색상 속성을 설정할 수 있습니다.

이 예제에서는 ApplicationViewTitleBar의 인스턴스를 가져와서 색상 속성을 설정하는 방법을 보여 줍니다.

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
> [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate)를 호출한 이후 이 코드를 앱의 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 메서드(_App.xaml.cs_)에 배치하거나 앱의 첫 번째 페이지에 배치할 수 있습니다.

> [!TIP]
> Windows 커뮤니티 도구 키트는 XAML에서 이러한 색상 속성을 설정할 수 있는 확장을 제공합니다. 자세한 내용은 [Windows 커뮤니티 도구 키트 설명서](https://docs.microsoft.com/windows/uwpcommunitytoolkit/extensions/viewextensions)를 참조하세요.

제목 표시줄 색상을 설정할 때 주의해야 할 몇 가지 사항이 있습니다.

- 버튼 배경색은 닫기 버튼을 마우스로 가리키는 상태 및 눌린 상태에는 적용되지 않습니다. 닫기 버튼은 항상 해당 상태에 대해 시스템 지정 색상을 사용합니다.
- 버튼 색상 속성은 시스템 뒤로 버튼을 사용할 때 이에 적용됩니다. ([탐색 기록 및 뒤로 탐색 참조](../basics/navigation-history-and-backwards-navigation.md).)
- 색상 속성을 **null**로 설정하면 기본 시스템 색상으로 재설정됩니다.
- 투명한 색을 설정할 수 없습니다. 색상의 알파 채널은 무시됩니다.

Windows는 사용자에게 선택한 [테마 컬러](https://docs.microsoft.com/windows/uwp/style/color#accent-color)를 제목 표시줄에 적용할 수 있는 옵션을 제공합니다. 제목 표시줄 색을 설정하는 경우 모든 색상을 명시적으로 설정하는 것이 좋습니다. 이렇게 하면 사용자 지정된 색상 설정으로 인한 의도하지 않은 색상 조합이 발생하지 않습니다.

## <a name="full-customization"></a>전체 사용자 지정

전체 제목 표시줄 사용자 지정을 선택하면 앱의 클라이언트 영역이 제목 표시줄 영역을 포함하여 전체 창을 포함하도록 확장됩니다. 자막 버튼을 제외한 전체 창의 그리기 및 입력 처리는 앱의 캔버스 상단에 중첩됩니다.

기본 제목 표시줄을 숨기고 제목 표시줄 영역으로 내용을 확장하려면, [CoreApplicationViewTitleBar.ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) 속성을 **true**로 설정합니다.

이 예제는 CoreApplicationViewTitleBar를 가져오고 ExtendViewIntoTitleBar 속성을 **true**로 설정하는 방법을 보여 줍니다. 이 작업은 앱의 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 메서드(_App.xaml.cs_) 또는 앱의 첫 번째 페이지에서 수행될 수 있습니다.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> 앱을 닫았다가 다시 시작하면 이 설정이 유지됩니다. Visual Studio에서 ExtendViewIntoTitleBar를 **true**로 설정하고 기본값으로 되돌리려는 경우 이를 **false** 로 명시적으로 설정하고 앱을 실행하여 저장된 설정을 덮어써야 합니다.

### <a name="draggable-regions"></a>끌기 가능 영역

제목 표시줄의 끌기 가능 영역은 사용자가 클릭하고 끌어서 창을 이동할 수 있는 위치를 정의합니다(단순히 앱의 캔버스에서 콘텐츠를 끄는 것과 반대임). [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) 메서드를 호출하고 끌기 가능 영역을 정의하는 UIElement를 전달하여 끌기 가능 영역을 지정합니다. (UIElement는 종종 다른 요소가 포함된 패널임)

제목 표시줄의 끌기 가능 영역으로 콘텐츠 그리드를 설정하는 방법은 다음과 같습니다. 이 코드는 앱의 첫 번째 페이지에 대한 XAML 및 코드 숨김에 포함됩니다. 전체 코드는 [전체 사용자 지정 예제](./title-bar.md#full-customization-example) 섹션을 참조하세요.

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

    // Set XAML element as a draggable region.
    AppTitleBar.Height = coreTitleBar.Height;
    Window.Current.SetTitleBar(AppTitleBar);
}
```

UIElement(`AppTitleBar`)는 앱에 대한 XAML의 일부입니다. 변경되지 않는 루트 페이지에서 제목 표시줄을 선언하거나 설정할 수 있습니다. 또는 앱이 탐색할 수 있는 각 페이지에서 제목 표시줄 영역을 선언하고 설정할 수 있습니다. 각 페이지에서 설정하는 경우 사용자가 앱을 탐색할 때 끌기 가능한 영역이 일관성을 유지하는지 확인해야 합니다.

앱 실행 중에 SetTitleBar를 호출하여 새 제목 표시줄 요소로 전환할 수 있습니다. 또한 SetTitleBar의 매개 변수로 **null**을 전달하여 기본 끌기 동작으로 되돌릴 수도 있습니다. (자세한 내용은 "기본 끌기 가능 영역" 참조)

> [!IMPORTANT]
> 지정한 끌기 가능 영역은 테스트할 수 있어야 합니다. 즉, 일부 요소의 경우 투명 배경 브러시를 설정해야 할 수도 있습니다. 자세한 내용은 [VisualTreeHelper.FindElementsInHostCoordinates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)의 설명을 참조하세요.
>
>예를 들어 그리드를 끌기 가능 영역으로 정의하는 경우 `Background="Transparent"`를 설정하여 끌기 가능하도록 만듭니다.
>
>`<Grid x:Name="AppTitleBar">` 그리드는 끌 수 없습니다(내부에 있는 요소는 보임).
>
>`<Grid x:Name="AppTitleBar" Background="Transparent">` 그리드는 동일하게 보이지만 전체 그리드를 끌 수 있습니다.

#### <a name="default-draggable-region"></a>기본 끌기 가능 영역

끌기 가능 영역을 지정하지 않으면 창의 너비, 자막 버튼의 높이 및 창의 위쪽 가장자리를 따라 위치한 사각형이 기본 끌기 가능 영역으로 설정됩니다.

끌기 가능 영역을 정의하면 시스템은 기본 끌기 가능 영역을 자막 버튼의 왼쪽에 해당 자막 버튼의 크기와 같은 작은 영역으로 축소합니다(또는 자막 버튼이 창의 왼쪽에 있는 경우 오른쪽으로 축소됨). 이렇게 하면 항상 사용자가 끌 수 있는 일관된 영역이 생깁니다.

### <a name="system-caption-buttons"></a>시스템 자막 버튼

시스템은 자막 버튼(뒤로, 최소화, 최대화, 닫기)을 위해 앱 창의 왼쪽 상단 또는 오른쪽 상단 모서리를 유지합니다. 시스템은 자막 제어 영역을 제어하여 창의 끌기, 최소화, 최대화 및 닫기를 위한 최소한의 기능을 제공합니다. 시스템은 왼쪽에서 오른쪽으로 쓰는 언어의 경우 오른쪽 상단에, 오른쪽에서 왼쪽으로 쓰는 언어의 경우 왼쪽 상단에 닫기 버튼을 그립니다.

자막 제어 영역의 크기와 위치는 CoreApplicationViewTitleBar 클래스에 의해 전달되므로 제목 표시줄 UI의 레이아웃에서 확인할 수 있습니다. 각 측면의 영역 너비는 [SystemOverlayLeftInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) 또는 [SystemOverlayRightInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) 속성이 지정하며 높이는 [Height](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height) 속성이 지정합니다.

앱 배경과 같은 속성에 의해 정의된 자막 제어 영역 아래에 콘텐츠를 그릴 수 있지만 사용자가 사용할 것으로 예상되는 UI는 배치하면 안 됩니다. 자막 제어에 대한 입력이 시스템에 의해 처리되므로 입력을 수신하지 못합니다.

자막 버튼 크기 변경에 응답하기 위해 [LayoutMetricsChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) 이벤트를 처리할 수 있습니다. 예를 들어, 시스템 뒤로 버튼이 표시되거나 숨겨져 있을 때 이 문제가 발생할 수 있습니다. 제목 표시줄의 크기에 종속되는 UI 요소의 위치를 확인하고 업데이트하려면 이 이벤트를 처리합니다.

이 예제는 제목 표시줄의 레이아웃을 조정하여 시스템 뒤로 버튼을 표시하거나 숨기는 것과 같은 변경 사항을 처리하는 방법을 보여 줍니다. `AppTitleBar``LeftPaddingColumn` 및 `RightPaddingColumn`은 이전에 표시된 XAML에서 선언됩니다.

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

버튼, 메뉴 또는 검색 상자와 같은 대화형 컨트롤을 앱 상단에 배치하여 제목 표시줄에 표시되도록 할 수 있습니다. 하지만 대화형 요소에 사용자 입력이 수신되는지 확실하지 않은 경우 몇 가지 규칙을 따라야 합니다.
- 영역을 끌기 가능 제목 표시줄 영역으로 정의하려면 SetTitleBar를 호출해야 합니다. 그렇게 않으면 시스템은 페이지 상단에 기본 끌기 가능 영역을 설정합니다. 그러면 시스템에서 이 영역에 대한 모든 사용자 입력을 처리하고 입력이 해당 컨트롤에 도달하지 못하게 합니다.
- 대화형 컨트롤을 SetTitleBar(z-순서가 더 높음) 호출에 의해 정의된 끌기 가능 영역의 위쪽에 놓습니다. UIElement의 대화형 컨트롤 하위 요소를 SetTitleBar에 전달하지 마세요. SetTitleBar에 요소를 전달하면 시스템은 이를 시스템 제목 표시줄과 같이 처리하고 해당 요소에 대한 모든 포인터 입력을 처리합니다.

여기에서 `TitleBarButton` 요소는 `AppTitleBar`보다 높은 z-순서가 있으므로 사용자 입력을 수신합니다.

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

### <a name="transparency-in-caption-buttons"></a>자막 버튼의 투명도

ExtendViewIntoTitleBar를 **true**로 설정하면 자막 버튼의 배경을 투명하게 만들어 앱 배경이 보이도록 할 수 있습니다. 일반적으로 전체 투명도에 대해 배경을 [Colors.Transparent](https://docs.microsoft.com/uwp/api/windows.ui.colors.Transparent)로 설정합니다. 부분적인 투명도의 경우 속성을 설정할 [색상](https://docs.microsoft.com/uwp/api/windows.ui.color)의 알파 채널을 설정합니다.

다음과 같은 ApplicationViewTitleBar 속성은 투명할 수 있습니다.

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

버튼 배경색은 닫기 버튼을 마우스로 가리키는 상태 및 눌린 상태에는 적용되지 않습니다. 닫기 버튼은 항상 해당 상태에 대해 시스템 지정 색상을 사용합니다.

다른 모든 색상 속성은 알파 채널을 계속 무시합니다. ExtendViewIntoTitleBar가 **false**로 설정된 경우 알파 채널은 모든 ApplicationViewTitleBar 색상 속성에 대해 항상 무시합니다.

### <a name="full-screen-and-tablet-mode"></a>전체 화면 및 태블릿 모드

앱을 _전체 화면_ 또는 _태블릿 모드_에서 실행하는 경우 시스템은 제목 표시줄과 자막 제어 버튼을 숨깁니다. 하지만 사용자는 제목 표시줄을 호출하여 앱의 UI 상단에 오버레이로 표시되도록 할 수 있습니다.
제목 표시줄이 숨겨지거나 호출되면 알릴 [CoreApplicationViewTitleBar.IsVisibleChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) 이벤트를 처리할 수 있으며, 필요에 따라 사용자 지정 제목 표시줄을 표시하거나 숨길 수 있습니다.

이 예제는 이전에 표시한 `AppTitleBar` 요소를 표시하거나 숨기도록 IsVisibleChanged를 처리하는 방법을 보여 줍니다.

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
>_전체 화면_ 모드는 앱에서 지원하는 경우에만 진입할 수 있습니다. 자세한 내용은 [ApplicationView.IsFullScreenMode](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode)를 참조하세요. [_태블릿 모드_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet)는 지원되는 하드웨어의 사용자 옵션이므로, 사용자는 태블릿 모드로 앱을 실행하도록 선택할 수 있습니다.

## <a name="full-customization-example"></a>전체 사용자 지정의 예

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

- 창이 활성 또는 비활성 상태일 때 만든 내용이 명확하게 나타납니다. 최소한 제목 표시줄의 텍스트, 아이콘 및 버튼 색상을 변경하세요.
- 앱 캔버스의 위쪽 가장자리를 따라 끌기 가능 영역을 정의합니다. 시스템 제목 표시줄의 배치를 맞추면 사용자가 쉽게 찾을 수 있습니다.
- 앱 캔버스의 시각적 제목 표시줄(있는 경우)과 일치하는 끌기 가능 영역을 정의하세요.

## <a name="related-articles"></a>관련 문서

- [아크릴](../style/acrylic.md)
- [색상](../style/color.md)
