---
Description: 명령 모음 플라이 아웃 인라인 액세스할 수 있게 앱의 가장 일반적인 작업입니다.
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
ms.openlocfilehash: cb87bea001492e39a0f60b96f884db70b5bd28ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592528"
---
# <a name="command-bar-flyout"></a>명령 모음 플라이아웃

명령 모음 플라이 아웃 UI 캔버스에 요소와 관련 된 부동 도구 모음에서 명령을 표시 하 여 일반적인 작업에 쉽게 액세스할 수 있는 사용자를 제공할 수 있습니다.

![확장 된 텍스트 명령 모음 플라이 아웃](images/command-bar-flyout-header.png)

> CommandBarFlyout 필요한 Windows 10 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상, 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)합니다.

> - **플랫폼 Api**: [CommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)를 [AppBarButton 클래스](/uwp/api/windows.ui.xaml.controls.appbarbutton)하십시오 [AppBarToggleButton 클래스](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [ AppBarSeparator 클래스](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Windows UI 라이브러리 Api**: [CommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

같은 [CommandBar](app-bars.md), CommandBarFlyout에 **PrimaryCommands** 하 고 **SecondaryCommands** 속성을 사용 하 여 명령을 추가할 수 있습니다. 컬렉션 또는 둘 다에서 명령을 배치할 수 있습니다. 기본 및 보조 명령 표시 되는 시기와 방법을 디스플레이 모드에 따라 달라 집니다.

명령 모음 플라이 아웃은 두 가지 표시 모드: *축소* 하 고 *확장*합니다.

- 축소 된 모드에 기본 명령만 표시 됩니다. 명령 모음 플라이 아웃 하는 기본 및 보조 하는 경우 명령, 줄임표로 표현 되는 "자세히 보기" 단추 \[•\]에 표시 됩니다. 이 확장 된 모드를 전환 하 여 보조 명령에 액세스할 수가 있습니다.
- 확장된 된 모드의 기본 및 보조 명령은 모두 표시 됩니다. (컨트롤에 보조 항목에 대해서만 있으면 표시 됩니다 MenuFlyout 컨트롤과 비슷하지만 방식에서입니다.)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

명령 모음 단추 및 메뉴 항목을 앱 캔버스에서 요소의 컨텍스트에서 같은 사용자에 게 표시할 CommandBarFlyout 컨트롤을 사용 합니다.

TextCommandBarFlyout 텍스트 "," TextBlock "," RichEditBox "," RichTextBlock, "및" PasswordBox 컨트롤의 텍스트 명령이 표시 됩니다. 현재 선택한 텍스트에 명령은 적절 하 게 자동으로 구성 됩니다. CommandBarFlyout를 사용 하 여 텍스트 컨트롤에서 기본 텍스트 명령으로 바꿉니다.

명령 목록 항목의 상황에 맞는 표시할의 지침을 따르세요 [컬렉션과 목록에 대 한 상황에 맞는 명령](collection-commanding.md)입니다.

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

상황에 맞는 메뉴에서 명령을 표시할 CommandBarFlyout 또는 MenuFlyout를 사용할 수 있습니다. CommandBarFlyout MenuFlyout 보다 더 많은 기능을 제공 하기 때문에 권장 합니다. 동작을 가져오거나는 MenuFlyout의 확인 및 전체 명령 모음 플라이 아웃을 사용 하 여 기본 및 보조 명령을 사용 하 여 CommandBarFlyout 보조 명령과 함께 사용할 수 있습니다.

> 관련된 정보를 참조 하세요 [플라이 아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)를 [메뉴와 상황에 맞는 메뉴](menus.md), 및 [명령 모음](app-bars.md)합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 <a href="xamlcontrolsgallery:/item/CommandBarFlyout">앱을 열고 참조 중인 CommandBarFlyout</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>사전 및 사후 호출

일반적으로 두 가지 플라이 아웃 또는 UI 캔버스에 요소와 연결 된 메뉴를 호출 하: _사전 호출_ 하 고 _사후 호출_합니다.

사전 호출에서 명령을 사용 하 여 연결 된 항목을 사용 하 여 사용자 상호 작용 명령은 자동으로 표시 됩니다. 예를 들어, 텍스트 서식 지정 명령이 있습니다 팝업 사용자가 텍스트 상자에 텍스트를 선택 합니다. 이 경우 명령 모음 플라이 아웃은 포커스를 사용 하지 않습니다. 대신, 사용자가 상호 작용 하는 항목에 가까운 관련 명령으로 표시 됩니다. 사용자는 명령을 상호 작용 하지 않고, 해제 됩니다.

사후 호출에서 명령에에서 표시 됩니다는 명시적 사용자 동작에 대 한 응답 명령을; 요청 예를 들어,를 마우스 오른쪽 단추로 클릭 합니다. 일반적인 개념에 해당 하는이 [상황에 맞는 메뉴](menus.md)합니다.

방식으로 또는 둘의 혼합 에서도 CommandBarFlyout를 사용할 수 있습니다.

## <a name="create-a-command-bar-flyout"></a>명령 모음 플라이 아웃을 만들려면

이 예제에서는 명령 모음 플라이 아웃을 만들고 사전 및 사후 모두 사용 하는 방법을 보여 줍니다. 이미지를 탭 할 플라이 아웃 축소 된 모드에서 표시 됩니다. 상황에 맞는 메뉴를 표시 하는 경우에 플라이 아웃 확장 된 모드에서 표시 됩니다. 두 경우 모두 사용자 확장 하거나 보고서를 열면 플라이 아웃을 축소할 수 있습니다.

![축소 된 명령 모음 플라이 아웃의 예](images/command-bar-flyout-img-collapsed.png)

> _축소 된 명령 모음 플라이 아웃_

![확장 된 명령 모음 플라이 아웃의 예](images/command-bar-flyout-img-expanded.png)

> _확장 된 명령 모음 플라이 아웃_

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

상황에 맞는 명령을 사전에 표시 하면 기본 명령과 (명령 모음 플라이 아웃 축소할지) 기본적으로 표시 됩니다. 상황에 맞는 메뉴에서 일반적으로 보조 명령 컬렉션에 들어가게 추가 명령을 확인 하 고 기본 명령 컬렉션에서 가장 중요 한 명령을 배치 합니다.

명령을 사전에 표시 하려면 일반적으로 처리 합니다 [클릭](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 또는 [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) 명령 모음 플라이 아웃을 표시 하는 이벤트입니다. 설정 플라이 아웃의 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 에 **일시적인** 또는 **TransientWithDismissOnPointerMoveAway** 를 포커스를 전환 하지 않고 플라이 아웃 축소 된 모드에서 엽니다.

Windows 10 Insider Preview부터 텍스트 컨트롤에는 **SelectionFlyout** 속성입니다. 이 속성에는 플라이 아웃을 할당 하면 텍스트를 선택한 경우 자동으로 표시 됩니다.

### <a name="show-commands-reactively"></a>사후 명령 표시

상황에 맞는 명령을 대응적으로 상황에 맞는 메뉴를 표시 하면 보조 명령 (명령 모음 플라이 아웃을 확장할지) 기본적으로 표시 됩니다. 이 경우 기본 및 보조 명령 또는 보조 명령만 명령 모음 플라이 아웃 있을 수 있습니다.

명령에서 상황에 맞는 메뉴를 표시 하려면 일반적으로 플라이 할당 된 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) UI 요소의 속성입니다. 이 이렇게 하면 요소에 의해 처리 되 플라이 아웃을 열고 하 고 작업을 더 수행할 필요가 없습니다.

처리 직접 플라이 아웃을 표시 하는 경우 (예는 [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) 이벤트), 플라이 아웃의 설정 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 에 **표준** 플라이 아웃 확장 된 모드에서 열려는 및 포커스를 제공 합니다.

> [!TIP]
> 플라이 아웃의 배치를 제어 하는 방법과 플라이 아웃을 표시 하는 경우 옵션에 대 한 자세한 내용은 참조 하세요. [플라이 아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)합니다.

## <a name="commands-and-content"></a>명령 및 콘텐츠

CommandBarFlyout 컨트롤에 명령 및 콘텐츠를 추가 하 여 2 개의 속성이 있습니다. [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) 하 고 [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)합니다.

기본적으로 명령 모음 항목은 **PrimaryCommands** 컬렉션에 추가됩니다. 이러한 명령은 명령 모음에서 표시 되 고 축소 및 확장 된 모드에 표시 됩니다. CommandBar를 달리 기본 명령 오버플로 보조 명령에 자동으로 수행 및 잘릴 수 있습니다.

명령을 추가할 수도 있습니다는 **SecondaryCommands** 컬렉션입니다. 보조 명령 컨트롤의 메뉴 영역에서 표시 되 고 확장 된 모드에만 표시 됩니다.

### <a name="app-bar-buttons"></a>앱 바 단추

사용 하 여 직접 SecondaryCommands 고 PrimaryCommands 채울 수 있습니다 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx)를 [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx), 및 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 컨트롤입니다.

앱 바 단추 컨트롤은 아이콘 및 텍스트 레이블에 따라 구분됩니다. 이러한 컨트롤에는 명령 모음에서 사용 하도록 최적화 된 및 명령 모음 오버플로 메뉴에 컨트롤을 표시 하는 여부에 따라 모양을 변경 합니다.

- 기본 명령으로 사용 되는 앱 표시줄 단추만 해당 아이콘을 사용 하 여 명령 모음에 표시 됩니다. 텍스트 레이블을 표시 되지 않습니다. 다음과 같이 명령에 대 한 텍스트 설명을 표시할 도구 설명을 사용 하는 것이 좋습니다.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 보조 명령으로 사용 되는 앱 표시줄 단추 레이블 및 표시 아이콘을 사용 하 여 메뉴에 표시 됩니다.

### <a name="other-content"></a>기타 콘텐츠

AppBarElementContainer에 래핑하여 명령 모음 플라이 아웃을 다른 컨트롤을 추가할 수 있습니다. 이렇게 하면 같은 컨트롤을 추가할 수 있습니다 [DropDownButton](buttons.md) 또는 [SplitButton](buttons.md), 또는 컨테이너와 같은 추가 [StackPanel](buttons.md) 더 복잡 한 UI를 만들기 위해.

명령 모음 플라이 아웃의 기본 또는 보조 명령 컬렉션에 추가 하려면 요소를 구현 해야 합니다 [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) 인터페이스입니다. AppBarElementContainer는 자체 인터페이스를 구현 하지 않는 경우에 명령 모음에 요소를 추가할 수 있도록이 인터페이스를 구현 하는 래퍼입니다.

여기서는 AppBarElementContainer 명령 모음 플라이 아웃에 추가 요소를 추가할 사용 됩니다. SplitButton 색 중에서 선택할 수 있도록 기본 명령에 추가 됩니다. StackPanel 확대/축소 컨트롤에 대 한 더 복잡 한 레이아웃을 허용 하도록 보조 명령에 추가 됩니다.

> [!TIP]
> 기본적으로 앱 캔버스를 위한 요소 보이지 않을 수도 있습니다 오른쪽 명령 모음에서. AppBarElementContainer를 사용 하 여 요소를 추가 하면 일부의 단계 다른 명령 모음 요소와 일치 하는 요소를 만들기 위해 수행 해야 합니다.
>
> - 재정의 된 기본 브러시 [경량 스타일](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) 요소의 배경 및 테두리 앱 모음 단추와 일치 하도록 합니다.
> - 요소의 위치와 크기를 조정 합니다.
> - Viewbox 16px의 높이와 너비에에서 아이콘을 래핑하십시오.

> [!NOTE]
> 이 예제에서는 표시 되는 명령을 구현 하지 않습니다만 명령 모음 플라이 아웃 UI를 보여 줍니다. 명령을 구현 하는 방법에 대 한 자세한 내용은 참조 하세요. [단추가](buttons.md) 하 고 [명령 디자인 기본 사항](../basics/commanding-basics.md)합니다.

![분할 단추를 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-split-button.png)

> _열기는 SplitButton 사용 하 여 축소 명령 모음 플라이 아웃_

![복잡 한 UI 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-custom-ui.png)

> _확장 된 명령 모음 플라이 아웃 메뉴 사용자 지정 확대 UI 사용 하 여_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>보조 명령만 사용 하 여 상황에 맞는 메뉴 만들기

CommandBarFlyout으로 보조 명령만 사용할 수는 [상황에 맞는 메뉴](menus.md)는 MenuFlyout 대신 합니다.

![보조 명령만 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-context-menu.png)

> _상황에 맞는 메뉴 명령 모음 플라이 아웃_

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

표준 메뉴를 만들려면를 DropDownButton를 사용 하 여를 CommandBarFlyout를 사용할 수 있습니다.

![명령 모음 플라이 아웃을 사용 하 여 단추 메뉴에는 드롭다운으로](images/command-bar-flyout-dropdown.png)

> _명령 모음 플라이 아웃에서 단추 메뉴 드롭다운_

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

## <a name="command-bar-flyouts-for-text-controls"></a>텍스트 컨트롤에 대 한 명령 모음 플라이 아웃

합니다 [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) 텍스트 편집에 대 한 명령을 포함 하는 특수 한 명령 모음 플라이 아웃 됩니다. 상황에 맞는 메뉴 (마우스 오른쪽 단추로 클릭) 또는 텍스트를 선택한 경우는 TextCommandBarFlyout를 자동으로 표시 하는 각 텍스트 컨트롤입니다. 텍스트 명령 모음 플라이 아웃 관련 명령을 표시 하도록 선택한 텍스트에 맞게 변경 합니다.

![축소 된 텍스트 명령 모음 플라이 아웃](images/command-bar-flyout-text-selection.png)

> _텍스트 선택 영역에 텍스트 명령 모음 플라이 아웃_

![확장 된 텍스트 명령 모음 플라이 아웃](images/command-bar-flyout-text-full.png)

> _확장 된 텍스트 명령 모음 플라이 아웃_


### <a name="available-commands"></a>사용 가능한 명령

이 테이블에 표시 되는 시점과 TextCommandBarFlyout, 포함 된 명령을 보여 줍니다.

| 명령 | 표시 하는 중... |
| ------- | -------- |
| Bold | 텍스트 컨트롤이 없는 경우 읽기 전용 (RichEditBox만). |
| Italic | 텍스트 컨트롤이 없는 경우 읽기 전용 (RichEditBox만). |
| Underline | 텍스트 컨트롤이 없는 경우 읽기 전용 (RichEditBox만). |
| 교정 | IsSpellCheckEnabled 경우 **true** 철자가 잘못 된 텍스트를 선택 하 고 있습니다. |
| 잘라내기 | 경우 텍스트 컨트롤은 읽기 전용 및 텍스트를 선택 합니다. |
| 복사 | 텍스트를 선택 하는 경우 |
| 붙여넣기 | 텍스트 컨트롤은 읽기 전용으로 클립보드의 콘텐츠가 시점과 합니다. |
| 실행 취소 | 작업을 취소할 수 있는 경우. |
| 모두 선택 | 경우 텍스트를 선택할 수 있습니다. |

### <a name="custom-text-command-bar-flyouts"></a>사용자 지정 텍스트 명령 모음 플라이 아웃

TextCommandBarFlyout은 사용자 지정할 수 없습니다, 하 고 각 텍스트 컨트롤에 의해 자동으로 관리 됩니다. 그러나 사용자 지정 명령을 사용 하 여 기본 TextCommandBarFlyout를 바꿀 수 있습니다.

- TextCommandBarFlyout 텍스트 선택 영역에 표시 되는 기본값을 바꾸려면 만들고 사용자 지정 CommandBarFlyout (또는 다른 플라이 아웃 형식의) 하에 할당 합니다 **SelectionFlyout** 속성입니다. SelectionFlyout를 설정 하면 **null**, 선택에 어떤 명령도 표시 됩니다.
- TextCommandBarFlyout 상황에 맞는 메뉴를 표시 되는 기본값을 바꾸려면 사용자 지정 CommandBarFlyout (또는 다른 플라이 아웃 형식)에 할당 합니다 **ContextFlyout** 텍스트 컨트롤의 속성입니다. ContextFlyout를 설정 하면 **null**표시 메뉴 플라이 아웃, 텍스트의 이전 버전에서의 TextCommandBarFlyout 대신 컨트롤을 표시 합니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [샘플 XAML 명령](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>관련 문서

- [UWP 앱에 대 한 명령 디자인 기본 사항](../basics/commanding-basics.md)
- [CommandBar 클래스](https://msdn.microsoft.com/library/windows/apps/dn279427)
