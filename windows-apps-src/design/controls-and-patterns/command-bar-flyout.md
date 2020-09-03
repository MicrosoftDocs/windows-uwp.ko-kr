---
Description: 명령 모음 플라이아웃을 통해 앱에서 가장 많이 수행하는 작업에 쉽게 액세스할 수 있습니다.
title: 명령 모음 플라이아웃
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7f273f3eb92efd30b432691f9faa05db0d6d013
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173507"
---
# <a name="command-bar-flyout"></a>명령 모음 플라이아웃

명령 모음 플라이아웃은 사용자가 일반적인 작업에 쉽게 액세스할 수 있도록 요소와 관련된 부동 도구 모음을 UI 캔버스에 표시합니다.

![확장된 텍스트 명령 모음 플라이아웃](images/command-bar-flyout-text-full.png)

[CommandBar](app-bars.md)처럼 CommandBarFlyout에는 명령을 추가하는 데 사용할 수 있는 **PrimaryCommands** 및 **SecondaryCommands** 속성이 있습니다. 두 컬렉션 중 하나에 또는 둘 모두에 명령을 배치할 수 있습니다. 기본 및 보조 명령이 표시되는 시기와 방법은 디스플레이 모드에 따라 달라집니다.

명령 모음 플라이아웃은 *축소* 및 *확장*의 두 가지 표시 모드를 제공합니다.

- 축소 모드에서는 기본 명령만 표시됩니다. 명령 모음 플라이아웃에 기본 명령과 보조 명령이 모두 있는 경우 줄임표(\[***\])로 표시되는 "자세히 보기" 단추가 표시됩니다. 사용자는 이 단추를 통해 확장 모드로 전환하여 보조 명령에 액세스할 수 있습니다.
- 확장 모드에서는 기본 명령과 보조 명령이 모두 표시됩니다. (컨트롤에 보조 항목만 있는 경우 보조 항목이 MenuFlyout 컨트롤과 비슷한 방식으로 표시됩니다.)

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | **CommandBarFlyout** 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되어 있습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요. |

>**Windows UI 라이브러리 API**: [CommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)
>
>**플랫폼 API**: [CommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton 클래스](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton 클래스](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [AppBarSeparator 클래스](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>
> CommandBarFlyout에는 Windows 10, 버전 1809([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상 또는 [Windows UI 라이브러리](/uwp/toolkits/winui/)가 필요합니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

앱 캔버스의 요소 컨텍스트에서 사용자에게 단추 및 메뉴 항목 같은 명령 컬렉션을 표시하려면 CommandBarFlyout 컨트롤을 사용합니다.

TextCommandBarFlyout은 TextBox, TextBlock, RichEditBox, RichTextBlock 및 PasswordBox 컨트롤에 텍스트 명령을 표시합니다. 명령은 현재 선택한 텍스트에 적절하게 자동 구성됩니다. CommandBarFlyout을 사용하여 텍스트 컨트롤의 기본 텍스트 명령을 바꿉니다.

목록 항목에 상황에 맞는 명령을 표시하려면 [컬렉션 및 목록에 대한 상황에 맞는 명령](collection-commanding.md)의 지침을 따릅니다.

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout과 MenuFlyout의 비교

상황에 맞는 메뉴에 명령을 표시하려면 CommandBarFlyout 또는 MenuFlyout을 사용합니다. MenuFlyout보다 더 많은 기능을 제공하는 CommandBarFlyout을 권장합니다. CommandBarFlyout에서 보조 명령만 사용하여 MenuFlyout의 동작과 모양을 가져올 수도 있고, 전체 명령 모음 플라이아웃에서 기본 명령과 보조 명령을 사용할 수도 있습니다.

> 관련 정보는 [플라이아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [메뉴 및 상황에 맞는 메뉴](menus.md) 및 [명령 모음](app-bars.md)을 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/CommandBarFlyout">앱을 열고 작동 중인 CommandBarFlyout을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>사전 및 사후 호출

일반적으로 UI 캔버스의 요소와 연결된 플라이아웃 또는 메뉴를 호출하는 두 가지 방법이 있는데, 하나는 _사전 호출_이고 다른 하나는 _사후 호출_입니다.

사전 호출에서는 사용자가 명령이 연결되는 항목과 상호 작용할 때 명령이 자동으로 나타납니다. 예를 들어 사용자가 텍스트 상자의 텍스트를 선택할 때 텍스트 형식 지정 명령이 나타날 수 있습니다. 이 경우 명령 모음 플라이아웃이 포커스를 받지 않습니다. 대신, 사용자가 상호 작용하는 항목 근처에 관련 명령을 표시합니다. 사용자가 명령과 상호 작용하지 않는 경우 사라집니다.

사후 호출에서는 명령을 요청하는 명시적인 사용자 작업(예: 마우스 오른쪽 단추 클릭)에 대응하여 명령이 표시됩니다. 이는 전통적인 [상황에 맞는 메뉴](menus.md) 개념과 일치합니다.

CommandBarFlyout은 둘 중 원하는 방식으로 사용 가능하며, 심지어 두 가지를 혼합한 방식으로도 사용할 수 있습니다.

## <a name="create-a-command-bar-flyout"></a>명령 모음 플라이아웃 만들기

이 예제에서는 명령 모음 플라이아웃을 만들어서 사전에 그리고 사후에 사용하는 방법을 보여줍니다. 이미지를 탭하면 플라이아웃이 축소 모드로 표시됩니다. 상황에 맞는 메뉴로 표시하는 경우 플라이아웃이 확장 모드로 표시됩니다. 두 경우 모두 플라이아웃이 열린 후 사용자가 플라이아웃을 확장하거나 축소할 수 있습니다.

![축소된 명령 모음 플라이아웃의 예](images/command-bar-flyout-img-collapsed.png)

> _축소된 명령 모음 플라이아웃_

![확장된 명령 모음 플라이아웃의 예](images/command-bar-flyout-img-expanded.png)

> _확장된 명령 모음 플라이아웃_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>사전에 명령 표시

상황에 맞는 명령을 사전에 표시할 경우 기본적으로 기본 명령만 표시해야 합니다(명령 모음 플라이아웃이 축소되어야 함). 가장 중요한 명령을 기본 명령 컬렉션에 배치하고, 이전에는 상황에 맞는 메뉴에 배치하던 추가 명령을 보조 명령 컬렉션에 배치합니다.

명령을 사전에 표시하려면 일반적으로 명령 모음 플라이아웃을 표시하는 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 또는 [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) 이벤트를 처리합니다. 플라이아웃이 포커스 없이 축소 모드에서 열리도록 플라이아웃의 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)를 **Transient** 또는 **TransientWithDismissOnPointerMoveAway**로 설정합니다.

Windows 10 Insider Preview부터 텍스트 컨트롤에는 **SelectionFlyout** 속성이 있습니다. 이 속성에 플라이아웃을 할당하면 텍스트를 선택할 때 플라이아웃이 자동으로 표시됩니다.

### <a name="show-commands-reactively"></a>사후에 명령 표시

상황에 맞는 명령을 사후에 상황에 맞는 메뉴로 표시할 경우 기본적으로 보조 명령이 표시됩니다(명령 모음 플라이아웃을 확장해야 함). 이 경우 명령 모음 플라이아웃에 기본 명령과 보조 명령이 모두 있거나 보조 명령만 있을 수 있습니다.

상황에 맞는 메뉴에 명령을 표시하려면 일반적으로 UI 요소의 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 속성에 플라이아웃을 할당합니다. 이 방식으로 요소를 사용하여 플라이아웃 열기가 처리되므로 개발자는 아무 것도 할 필요가 없습니다.

플라이아웃을 표시를 직접 처리하는 경우(예: [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) 이벤트에서) 플라이아웃이 확장 모드에서 열리고 포커스를 받도록 플라이아웃의 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)를 **Standard**로 설정합니다.

> [!TIP]
> 플라이아웃을 배치하는 옵션과 플라이아웃 배치를 제어하는 방법에 대한 자세한 내용은 [플라이아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)을 참조하세요.

## <a name="commands-and-content"></a>명령 및 콘텐츠

CommandBarFlyout 컨트롤에는 명령 및 콘텐츠를 추가하는 데 사용할 수 있는 다음과 같은 2가지 속성이 있습니다. 그것은 바로 [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) 및 [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)입니다.

기본적으로 명령 모음 항목은 **PrimaryCommands** 컬렉션에 추가됩니다. 이러한 명령은 명령 모음에서 표시되고 축소 모드와 확장 모드 둘 모두에 표시됩니다. CommandBar와는 달리, 기본 명령이 자동으로 보조 명령으로 오버플로되지 않으며 잘릴 수 있습니다.

**SecondaryCommands** 컬렉션에 명령을 추가할 수도 있습니다. 보조 명령은 컨트롤의 메뉴 영역에 표시되며 확장 모드에만 표시됩니다.

### <a name="app-bar-buttons"></a>앱 바 단추

PrimaryCommands 및 SecondaryCommands를 [AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 및 [AppBarSeparator](/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 컨트롤로 직접 채울 수 있습니다.

앱 바 단추 컨트롤은 아이콘 및 텍스트 레이블에 따라 구분됩니다. 이러한 컨트롤은 명령 모음에 사용하도록 최적화되었으며, 컨트롤이 명령 모음에 표시되는지 아니면 오버플로 메뉴에 표시되는지에 따라 모양이 변합니다.

- 기본 명령으로 사용되는 앱 바 단추는 명령 모음에 아이콘으로만 표시되고, 텍스트 레이블은 표시되지 않습니다. 다음과 같이 도구 설명을 사용하여 명령의 텍스트 설명을 표시하는 것이 좋습니다.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 보조 명령으로 사용되는 앱 바 단추는 레이블과 아이콘을 모두 사용하여 메뉴에 표시됩니다.

### <a name="other-content"></a>기타 콘텐츠

명령 모음 플라이아웃을 AppBarElementContainer에 래핑하여 다른 컨트롤을 명령 모음 플라이아웃에 추가할 수 있습니다. 이렇게 하면 [DropDownButton](buttons.md) 또는 [SplitButton](buttons.md) 같은 컨트롤을 추가하거나, [StackPanel](buttons.md) 같은 컨테이너를 추가하여 복합 UI를 만들 수 있습니다.

명령 모음 플라이아웃의 기본 또는 보조 명령 컬렉션에 추가하려면 요소에서 [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) 인터페이스를 구현해야 합니다. AppBarElementContainer는 명령 모음이 인터페이스 자체를 구현하지 않는 경우 명령 모음에 요소를 추가할 수 있도록 이 인터페이스를 구현하는 래퍼입니다.

여기는 AppBarElementContainer는 명령 모음 플라이아웃에 요소를 추가하는 데 사용됩니다. SplitButton은 색을 선택할 수 있도록 기본 명령에 추가됩니다. StackPanel은 확대/축소 컨트롤에 좀 더 복잡한 레이아웃이 가능하도록 보조 명령에 추가됩니다.

> [!TIP]
> 기본적으로 앱 캔버스용으로 설계된 요소는 명령 모음에 적합하지 않은 것처럼 보일 수 있습니다. AppBarElementContainer를 사용하여 요소를 추가할 때 요소가 다른 명령 모음 요소와 일치하도록 몇 가지 단계를 수행해야 합니다.
>
> - 요소의 배경과 테두리가 앱 바 단추와 일치하도록 기본 브러시를 [경량 스타일](./xaml-styles.md#lightweight-styling)로 재정의합니다.
> - 요소의 위치와 크기를 조정합니다.
> - 높이와 너비가 각각 16픽셀인 Viewbox에 아이콘을 래핑합니다.

> [!NOTE]
> 이 예제에서는 명령 모음 플라이아웃 UI를 보여주기만 하고, 표시되는 명령을 구현하지는 않습니다. 명령 구현에 대한 자세한 내용은 [단추](buttons.md) 및 [명령 디자인 기본 사항](../basics/commanding-basics.md)을 참조하세요.

![분할 단추가 있는 명령 모음 플라이아웃](images/command-bar-flyout-split-button.png)

> _SplitButton이 열려 있는 축소된 명령 모음 플라이아웃_

![복합 UI가 있는 명령 모음 플라이아웃](images/command-bar-flyout-custom-ui.png)

> _메뉴에 사용자 지정 확대/축소 UI가 있는 확장된 명령 모음 플라이아웃_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>보조 명령만 있는 상황에 맞는 메뉴 만들기

MenuFlyout 대신 보조 명령만 있는 CommandBarFlyout을 [상황에 맞는 메뉴](menus.md)로 사용할 수 있습니다.

![보조 명령만 있는 명령 모음 플라이아웃](images/command-bar-flyout-context-menu.png)

> _상황에 맞는 메뉴로 사용되는 명령 모음 플라이아웃_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

DropDownButton이 있는 CommandBarFlyout을 사용하여 표준 메뉴를 만들 수도 있습니다.

![드롭다운 단추 메뉴가 있는 명령 모음 플라이아웃](images/command-bar-flyout-dropdown.png)

> _명령 모음 플라이아웃의 단추 드롭다운 메뉴_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>텍스트 컨트롤에 대한 명령 모음 플라이아웃

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)은 텍스트를 편집하는 명령을 포함하고 있는 특수한 명령 모음 플라이아웃입니다. 마우스 오른쪽 단추를 클릭하거나 텍스트를 선택하면 각 텍스트 컨트롤은 자동으로 TextCommandBarFlyout을 상황에 맞는 메뉴로 표시합니다. 텍스트 명령 모음 플라이아웃은 관련 명령만 표시하도록 선택한 텍스트에 맞게 조정됩니다.

![축소된 텍스트 명령 모음 플라이아웃](images/command-bar-flyout-text-selection.png)

> _선택한 텍스트의 텍스트 명령 모음 플라이아웃_

![확장된 텍스트 명령 모음 플라이아웃](images/command-bar-flyout-text-full.png)

> _확장된 텍스트 명령 모음 플라이아웃_


### <a name="available-commands"></a>사용 가능한 명령

다음 표는 TextCommandBarFlyout에 포함된 명령과 각 명령이 표시되는 시기를 보여줍니다.

| 명령 | Shown... |
| ------- | -------- |
| 굵게 | 텍스트 컨트롤이 읽기 전용이 아닌 경우(RichEditBox만 해당) |
| 기울임꼴 | 텍스트 컨트롤이 읽기 전용이 아닌 경우(RichEditBox만 해당) |
| 밑줄 | 텍스트 컨트롤이 읽기 전용이 아닌 경우(RichEditBox만 해당) |
| Proofing | IsSpellCheckEnabled가 **true**이고 철자가 잘못된 텍스트를 선택한 경우 |
| 잘라내기 | 텍스트 컨트롤이 읽기 전용이 아니고 텍스트를 선택한 경우 |
| 복사 | 텍스트를 선택한 경우 |
| 붙여넣기 | 텍스트 컨트롤이 읽기 전용이 아니고 클립보드에 콘텐츠가 있는 경우 |
| 실행 취소 | 실행 취소할 수 있는 작업이 있는 경우 |
| 모두 선택 | 텍스트를 선택할 수 있는 경우 |

### <a name="custom-text-command-bar-flyouts"></a>사용자 지정 명령 모음 플라이아웃

TextCommandBarFlyout은 사용자 지정할 수 없으며, 각 텍스트 컨트롤에 의해 자동으로 관리됩니다. 그러나 기본 TextCommandBarFlyout을 사용자 지정 명령으로 바꿀 수 있습니다.

- 선택한 텍스트에 표시되는 기본 TextCommandBarFlyout을 바꾸려면 사용자 지정 CommandBarFlyout(또는 다른 플라이아웃 형식)을 만들어서 **SelectionFlyout** 속성에 할당하면 됩니다. SelectionFlyout을 **null**로 설정하면 선택 시 어떤 명령도 표시되지 않습니다.
- 상황에 맞는 메뉴로 표시되는 기본 TextCommandBarFlyout을 바꾸려면 텍스트 컨트롤의 **ContextFlyout** 속성에 사용자 지정 CommandBarFlyout(또는 다른 플라이아웃 형식)을 할당합니다. ContextFlyout을 **null**로 설정하면 TextCommandBarFlyout 대신 이전 버전의 텍스트 컨트롤에 표시된 메뉴 플라이아웃이 표시됩니다.

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.
- [XAML 명령 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>관련된 문서

- [Windows 앱용 명령 디자인 기본 사항](../basics/commanding-basics.md)
- [CommandBar 클래스](/uwp/api/Windows.UI.Xaml.Controls.CommandBar)