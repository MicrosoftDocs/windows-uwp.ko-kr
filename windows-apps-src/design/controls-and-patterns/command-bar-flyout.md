---
author: jwmsft
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: 명령 모음 플라이아웃
label: Command bar flyout
template: detail.hbs
ms.author: jimwalk
ms.date: 10/2/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 95d99c41ff2679e3ef3e0471dd583fe78458922c
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919161"
---
# <a name="command-bar-flyout"></a>명령 모음 플라이아웃

명령 모음 팝업 창 UI 캔버스의 요소와 관련 하 여 부동 도구 모음에서 명령을 표시 하 여 일반 작업에 쉽게 액세스할 수 있는 사용자를 제공할 수 있습니다.

![확장 된 텍스트 명령 모음 팝업 창](images/command-bar-flyout-header.png)

> CommandBarFlyout 10 Windows 버전 1809 ([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 필요 이상 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **플랫폼 Api**: [CommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton 클래스](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton 클래스](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [AppBarSeparator 클래스](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Windows UI 라이브러리 Api**: [CommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

[명령 모음](app-bars.md)같은 CommandBarFlyout 속성이 **PrimaryCommands** 및 **SecondaryCommands** 명령을 추가 하는 데 사용할 수 있습니다. 컬렉션, 또는 둘 다에 명령을 배치할 수 있습니다. 시기와 방법에 기본 및 보조 명령이 표시 되는 디스플레이 모드에 따라 달라 집니다.

명령 모음 팝업 창에는 두 가지 디스플레이 모드: *확장*하 고 *축소* 합니다.

- 축소 모드로 기본 명령만 표시 됩니다. 기본 및 보조 프로그램 명령 모음 팝업 창에 명령을 줄임표로 표현 되는 "참조 추가" 단추 \ [•••\]이 표시 됩니다. 사용자를 확장 모드로 전환 하 여 보조 명령에 대 한 액세스를 얻을 수 있습니다.
- 확장 된 모드에서 기본 및 보조 명령이 표시 됩니다. (컨트롤 보조 항목만 있으면 나타나는 MenuFlyout 컨트롤에 유사한 방법으로.)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

CommandBarFlyout 컨트롤을 사용 하 여 명령 모음 단추 및 메뉴 항목의 app canvas에서 요소의 컨텍스트에서 같은 사용자에 게 표시.

TextCommandBarFlyout는 TextBox, TextBlock, RichEditBox, RichTextBlock, 및 PasswordBox 컨트롤에 명령 텍스트를 표시합니다. 현재 텍스트 선택 영역에 자동으로 명령은 적절 하 게 구성 됩니다. 텍스트 컨트롤의 기본 텍스트 명령을 대체 하는 CommandBarFlyout를 사용 합니다.

상황별 표시 하려면 목록 항목에 대 한 명령을 [상황별 명령 모음 및 목록에 대 한](collection-commanding.md)지침을 따릅니다.

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout 및 MenuFlyout

명령을 상황에 맞는 메뉴를 표시 하려면 CommandBarFlyout 또는 MenuFlyout를 사용할 수 있습니다. CommandBarFlyout MenuFlyout 보다 더 많은 기능을 제공 하기 때문에 권장 합니다. 보조 명령만 사용 하 여 동작 하 게 하 고는 MenuFlyout의 모양 또는 전체 명령 모음 플라이 아웃을 사용 하 여 기본 및 보조 명령으로 CommandBarFlyout를 사용할 수 있습니다.

> 관련된 정보, [플라이 아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md) [메뉴와 상황에 맞는 메뉴와](menus.md) [명령 모음](app-bars.md)을 참조 하십시오.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하는 경우 여기를 클릭 <a href="xamlcontrolsgallery:/item/CommandBarFlyout">응용 프로그램을 열고 실행 중인 CommandBarFlyout를 참조 하십시오</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>사전 및 사후 호출

일반적으로 두 가지 플라이 아웃 또는 UI 캔버스의 요소와 연관 된 메뉴를 호출할 수: _사전 호출_ 및 _사후 호출_합니다.

사전 호출에서 명령이 관련 된 항목과 상호 작용할 명령이 자동으로 표시 됩니다. 예를 들어, 텍스트 서식 명령을 수 팝업 텍스트 상자에 텍스트를 선택할 때. 이 경우 명령 모음 플라이 아웃 포커스를 고려 하지 않습니다. 사용자가 상호 작용 하는 항목에 가깝게 관련 명령을 제시 합니다. 명령을 사용 하 여 사용자 상호 작용 하지 않는 경우 해제 됩니다.

사후 호출 명령이 표시 명시적인 사용자 동작에 대 한 응답에서 요청 명령; 예를 들어, 오른쪽 클릭 합니다. [상황에 맞는 메뉴](menus.md)의 일반적인 개념에 해당 합니다.

방식으로, 또는 둘의 혼합도 CommandBarFlyout를 사용할 수 있습니다.

## <a name="create-a-command-bar-flyout"></a>플라이 아웃 명령 모음 만들기

플라이 아웃 명령 모음을 만들고 사전 대처 및 반응성이 뛰어난 방식에 사용 하는 방법을 보여 주는이 예제입니다. 이미지가 변하는데 플라이 아웃 축소 모드에서 표시 됩니다. 상황에 맞는 메뉴를 표시 하는 경우 확장된 모드에서 플라이 아웃 표시 됩니다. 두 경우 모두 사용자 확장 하거나 열 후 플라이 아웃을 축소 수 있습니다.

![축소 된 명령 모음 플라이 아웃의 예](images/command-bar-flyout-img-collapsed.png)

> _축소 된 명령 모음 플라이 아웃_

![확장된 명령 모음 팝업 창에는의 예](images/command-bar-flyout-img-expanded.png)

> _확장 된 명령 모음 팝업 창_

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

### <a name="show-commands-proactively"></a>사전 명령 표시

명령 컨텍스트를 사전 표시 (명령 모음 팝업 창 해야 축소 된 경우)는 기본적으로 기본 명령만 표시 됩니다. 가장 중요 한 명령은 기본 명령 컬렉션과 전통적으로 속하게 될 상황에 맞는 메뉴에서 보조 명령 컬렉션에 추가 명령에 놓습니다.

명령이 표시 사전, 일반적으로 명령 모음 팝업 창 표시를 [클릭](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 하거나 [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) 이벤트를 처리 합니다. 플라이 아웃의 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) **임시** 또는 **TransientWithDismissOnPointerMoveAway** 초점을 수행 하지 않고 축소 된 모드에서 플라이 아웃을 열려면 설정 합니다.

텍스트 컨트롤 Windows 10 내부자 미리 부터는 **SelectionFlyout** 속성을 가집니다. 플라이 아웃이이 속성에 지정 하면 텍스트를 선택한 경우 자동으로 표시 됩니다.

### <a name="show-commands-reactively"></a>반응성이 뛰어난 방식 명령 표시

상황별 명령, 반응성이 뛰어난 방식, 상황에 맞는 메뉴로 표시 하는 경우 보조 명령 (명령 모음 팝업 창 확장 되어야 한다는) 기본적으로 표시 됩니다. 이 경우 명령 모음 팝업 창에 기본 및 보조 명령 또는 보조 명령에만 있을 수 있습니다.

명령을 상황에 맞는 메뉴를 표시 하려면 일반적으로 UI 요소의 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 속성에는 플라이 아웃을 할당 합니다. 이러한 방식으로 요소에 의해 처리 되는 열 플라이 아웃 하 고 굳이 응답할 필요는 없습니다.

(예를 들어, [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) 이벤트)에서 직접 플라이 아웃을 표시를 처리 하는 경우 **표준** 확장된 모드에서 플라이 아웃을 열고 포커스를 플라이 아웃의 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 를 설정 합니다.

> [!TIP]
> 플라이 아웃 및 플라이 아웃의 위치를 제어 하는 방법을 표시 하는 옵션에 대 한 자세한 내용을 보려면 [플라이 아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)을 참조 하십시오.

## <a name="commands-and-content"></a>명령 및 콘텐츠

CommandBarFlyout 컨트롤에 추가 명령 및 콘텐츠를 사용할 수 있습니다 2 속성: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) 및 [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

기본적으로 명령 모음 항목은 **PrimaryCommands** 컬렉션에 추가됩니다. 이 명령은 명령 모음에 표시 되 고 축소 또는 확장 모드에 표시 됩니다. 명령 모음을 달리 기본 명령을 오버플로 보조 명령에 자동으로 수행 하 고 잘릴 수 있습니다.

**SecondaryCommands** 컬렉션에 명령을 추가할 수도 있습니다. 보조 명령을 컨트롤의 메뉴에에서 표시 되 고 확장된 모드 에서만에서 표시 됩니다.

### <a name="app-bar-buttons"></a>앱 바 단추

PrimaryCommands 및 SecondaryCommands [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)및 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 컨트롤을 직접 채울 수 있습니다.

앱 바 단추 컨트롤은 아이콘 및 텍스트 레이블에 따라 구분됩니다. 이러한 컨트롤에서 명령 모음을 사용 하기 위해 최적화 된 하 고 컨트롤이 명령 모음 또는 오버플로 메뉴에 표시 되는 여부에 따라 모양을 변경 합니다.

- 기본 명령으로 사용 하는 응용 프로그램 표시줄 단추 아이콘;만 사용 하 여 명령 모음에 표시 됩니다. 텍스트 레이블이 표시 되지 않습니다. 다음과 같이 도구 명령에 대 한 텍스트 설명을 표시 하는 데 사용 하는 것이 좋습니다.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 보조 명령으로 사용 하는 응용 프로그램 표시줄 단추 레이블과 아이콘을 볼 수 있는 메뉴에 표시 됩니다.

### <a name="other-content"></a>기타 콘텐츠

AppBarElementContainer에 래핑하여 플라이 아웃 명령 모음에 다른 컨트롤을 추가할 수 있습니다. 이렇게 하면 [DropDownButton]() 또는 [분할 단추]()컨트롤을 추가 하거나 복잡 한 UI를 만들기 위해 [StackPanel]() 같은 컨테이너를 추가 합니다.

요소 명령 모음 플라이 아웃의 주 또는 보조 명령 컬렉션에 추가 하려면 [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) 인터페이스를 구현 해야 합니다. AppBarElementContainer는 자체 인터페이스를 구현 하지 않더라도 요소 명령 모음에 추가할 수 있도록이 인터페이스를 구현 하는 래퍼입니다.

여기서는 AppBarElementContainer 명령 모음 플라이 아웃을 추가 요소를 추가 하는 데 사용 됩니다. 분할 단추는 기본 색 중에서 선택할 수 있도록 명령에 추가 됩니다. StackPanel은 확대/축소 컨트롤에 대 한 복잡 한 배치를 허용 하기 위해 보조 명령에 추가 됩니다.

> [!TIP]
> 기본적으로 앱 캔버스 디자인 요소가 보이지 않을 수도 있습니다 오른쪽 명령 모음에 있습니다. AppBarElementContainer를 사용 하 여 요소를 추가할 때 일부의 단계를 다른 명령 모음 요소와 일치 하는 요소를 확인 하기 위해 취해야 할.
>
> - [간단한 스타일](/design/controls-and-patterns/xaml-styles#lightweight-styling) 요소의 배경과 응용 프로그램 표시줄 단추와 일치 하는 테두리를 사용 하 여 기본 브러시를 재정의 합니다.
> - 요소의 위치와 크기를 조정 합니다.
> - Viewbox 16px의 높이와 너비에에서 아이콘을 배치.

> [!NOTE]
> 이 예제에 표시 된 명령을 구현 하지 않습니다만 명령 모음 플라이 아웃 UI를 보여줍니다. 명령을 구현 하는 방법에 대 한 추가 정보 [단추](buttons.md) 및 [명령 디자인의 기초](../basics/commanding-basics.md)를 참조 하십시오.

![분할 단추를 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-split-button.png)

> _열려 있는 분할 단추를 사용 하 여 축소 된 명령 모음 플라이 아웃_

![복잡 한 UI 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-custom-ui.png)

> _사용자 지정 확대/축소 UI 메뉴에 있는 확장된 명령 모음 팝업 창_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>보조 명령과 상황에 맞는 메뉴를 만듭니다.

보조 명령만 사용 하 여 [상황에 맞는 메뉴](menus.md)를 대신 한 MenuFlyout는 CommandBarFlyout를 사용할 수 있습니다.

![보조 명령만 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-context-menu.png)

> _상황에 맞는 메뉴와 명령 모음 팝업 창_

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

표준 메뉴를 만드는 데는 DropDownButton으로는 CommandBarFlyout를 사용할 수 있습니다.

![단추 메뉴를 드롭다운으로 플라이와 명령 모음 아웃](images/command-bar-flyout-dropdown.png)

> _명령 모음 플라이 아웃 메뉴 단추 드롭다운_

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

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) 는 텍스트 편집 명령이 들어 있는 특수 한 명령 모음 플라이 아웃입니다. 각 텍스트 컨트롤의 텍스트를 선택 하거나 상황에 맞는 메뉴 (마우스 오른쪽 단추 클릭)으로 TextCommandBarFlyout는 자동으로 표시 됩니다. 텍스트 명령 모음 팝업 창 관련 명령을 표시 하려면 선택한 텍스트에 적응 합니다.

![축소 된 텍스트 명령 모음 팝업 창](images/command-bar-flyout-text-selection.png)

> _텍스트 선택 영역에 텍스트 명령 모음 플라이 아웃_

![확장 된 텍스트 명령 모음 팝업 창](images/command-bar-flyout-text-full.png)

> _확장 된 텍스트 명령 모음 팝업 창_


### <a name="available-commands"></a>사용 가능한 명령

이 테이블에는 표시 되는 시점과 TextCommandBarFlyout에 포함 되어 있는 명령을 보여 줍니다.

| 명령 | 표시... |
| ------- | -------- |
| 굵게 | 텍스트 컨트롤이 없는 경우 읽기 전용 (RichEditBox만). |
| 기울임꼴 | 텍스트 컨트롤이 없는 경우 읽기 전용 (RichEditBox만). |
| 밑줄 | 텍스트 컨트롤이 없는 경우 읽기 전용 (RichEditBox만). |
| 교정 | IsSpellCheckEnabled가 **true이** 고 텍스트가 선택 됩니다. |
| Cut | 때 텍스트 컨트롤은 읽기 전용 및 텍스트를 선택 합니다. |
| Copy | 텍스트를 선택 하는 경우 |
| Paste | 텍스트 컨트롤은 읽기 전용 및 클립보드 내용이 시기. |
| 실행 취소 | 작업을 취소할 수 있을 때. |
| 모두 선택 | 경우 텍스트를 선택할 수 있습니다. |

### <a name="custom-text-command-bar-flyouts"></a>사용자 정의 텍스트 명령 모음 플라이 아웃

TextCommandBarFlyout 인쇄할 수 및 각 텍스트 컨트롤에 의해 자동으로 관리 됩니다. 그러나 사용자 지정 명령을 사용 하 여 기본 TextCommandBarFlyout를 바꿀 수 있습니다.

- 기본 텍스트 선택 영역에 표시 되는 TextCommandBarFlyout를 대체 하려면 사용자 지정 CommandBarFlyout (또는 다른 플라이 아웃 형식)를 만들 수 있으며 **SelectionFlyout** 속성에 할당 키를 누릅니다. SelectionFlyout를 **null**로 설정 하면 명령이 없는 선택에 표시 됩니다.
- 기본으로 상황에 맞는 메뉴에 표시 되는 TextCommandBarFlyout를 대체 하려면 사용자 지정 CommandBarFlyout 또는 다른 플라이 아웃 형식을 텍스트 컨트롤의 **ContextFlyout** 속성에 할당 합니다. ContextFlyout를 **null**로 설정 하면 이전 버전의 텍스트 컨트롤에 표시 된 메뉴 팝업 창에서 TextCommandBarFlyout 대신 표시 됩니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [XAML 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>관련 문서

- [UWP 앱의 명령 디자인 기본 사항](../basics/commanding-basics.md)
- [CommandBar 클래스](https://msdn.microsoft.com/library/windows/apps/dn279427)
