---
author: jwmsft
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: 명령 모음 플라이 아웃
label: Command bar flyout
template: detail.hbs
ms.author: jimwalk
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: ec532749fc2dacfc56e80ee2830da36f71c75b2f
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4118501"
---
# <a name="command-bar-flyout"></a>명령 모음 플라이 아웃

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다. 미리 보기 기능에는 [최신 Windows 10 Insider Preview 빌드 및 SDK](https://insider.windows.com/for-developers/) 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)필요합니다.

명령 모음 플라이 아웃 UI 캔버스에 요소와 관련 된 부동 도구 모음에서 명령을 표시 하 여 일반적인 작업에 쉽게 액세스할 수 있는 사용자에 게 제공할 수 있습니다.

![확장 된 텍스트 명령 모음 플라이 아웃](images/command-bar-flyout-text-full.png)

> 관련된 정보 [플라이 아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md) [메뉴 및 상황에 맞는 메뉴](menus.md), 및 [명령 모음](app-bars.md)을 참조 하세요.

[CommandBar](app-bars.md)같은 CommandBarFlyout 속성이 **PrimaryCommands** 와 **SecondaryCommands** 는 명령을 추가 하는 데 사용할 수 있습니다. 명령, 컬렉션 또는 둘 다에 배치할 수 있습니다. 기본 및 보조 명령을 표시 되는 시기와 방법을 디스플레이 모드에 따라 달라 집니다.

명령 모음 플라이 아웃에 두 가지 표시 모드: *확장*및 *축소* 합니다.

- 축소 된 모드에서 기본 명령만 표시 됩니다. 기본 및 보조 명령 모음 플라이 아웃에 있으면 명령, 줄임표로 표현 되는 "자세히" 단추, \ [• • •] 표시 됩니다. 이 확장 모드로 전환 하 여 보조 명령에 액세스할 수가 있습니다.
- 확장된 모드에서 기본 및 보조 명령이 표시 됩니다. (컨트롤만 보조 항목에 있는 경우에 나타나는 MenuFlyout 컨트롤에 유사한 방식으로.)

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 Windows UI 라이브러리 새 컨트롤 및 UWP 앱에 대 한 UI 기능을 포함 하는 NuGet 패키지의 일부로 포함 합니다. 설치 지침을 비롯 한 자세한 내용은 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조 하세요. |

| **플랫폼 Api** | **Windows UI 라이브러리 Api** |
| - | - |
| [CommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton 클래스](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton 클래스](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [AppBarSeparator 클래스](/uwp/api/windows.ui.xaml.controls.appbarseparator) | [CommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout 클래스](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) |

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

명령 모음 단추 및 앱 캔버스에서 요소의 컨텍스트 메뉴 항목 등의 사용자에 게 표시 하도록 CommandBarFlyout 컨트롤을 사용 합니다.

TextCommandBarFlyout TextBox, TextBlock, RichEditBox, RichTextBlock, 및 PasswordBox 컨트롤에서 텍스트 명령을 표시합니다. 현재 선택한 텍스트에 명령은 적절 하 게 자동으로 구성 됩니다. CommandBarFlyout를 사용 하 여 텍스트 컨트롤의 기본 텍스트 명령은 바꿉니다.

상황에 맞는 표시 하려면 명령 목록 항목에는 [상황별 컬렉션 및 리스트에 대 한 명령](collection-commanding.md)에서 지침을 따릅니다.

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

명령을 상황에 맞는 메뉴를 표시 하려면 CommandBarFlyout MenuFlyout을 사용할 수 있습니다. MenuFlyout 보다 더 많은 기능을 제공 하기 때문에 CommandBarFlyout을 좋습니다. 동작을 가져오거나 MenuFlyout을의 모양 및 전체 명령 모음 플라이 아웃을 사용 하 여 기본 및 보조 명령으로 CommandBarFlyout만 보조 명령으로 사용할 수 있습니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 된 경우 여기를 클릭 <a href="xamlcontrolsgallery:/item/CommandBarFlyout">앱을 열고 중인 CommandBarFlyout를 참조 하세요</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>사전 대응식 호출 비교

일반적으로 두 가지 플라이 아웃 또는 UI 캔버스에 요소와 연결 된 메뉴를 호출 하: _사전 호출_ 및 _사후 호출_합니다.

사전 호출에서 사용자가 명령을 연결 된 항목을 조작 명령이 자동으로 표시 됩니다. 예를 들어 텍스트 서식 명령이 수 팝업 사용자가 텍스트 상자에 텍스트를 선택 합니다. 이 경우 명령 모음 플라이 아웃은 포커스를 고려 하지 않습니다. 대신, 사용자가 상호 작용 항목 가까운 관련 명령을 표시 됩니다. 명령을 사용 하 여 사용자 상호 작용 하지 않는 경우 해제 됩니다.

사후 호출 명령은 표시 되어 명시적인 사용자 작업에 대 한 응답에서 명령을 요청 예를 들어, 오른쪽 클릭 합니다. [상황에 맞는 메뉴](menus.md)의 일반적인 개념에 해당 합니다.

CommandBarFlyout 방식으로 또는 둘의 혼합도 사용할 수 있습니다.

## <a name="create-a-command-bar-flyout"></a>명령 모음 플라이 아웃 만들기

> **미리 보기**: [최신 Windows 10 Insider Preview 빌드 및 SDK](https://insider.windows.com/for-developers/) 나 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)CommandBarFlyout 필요 합니다.

이 예제에서는 명령 모음 플라이 아웃을 만들고 사전 및 사후 방식으로 사용 하는 방법을 보여 줍니다. 이미지는 터치 할 때 축소 모드에서 플라이 아웃이 표시 됩니다. 상황에 맞는 메뉴로 표시 되 면 플라이 아웃이 확장 모드로 표시 됩니다. 두 경우 모두 사용자가 확장 하거나를 연 후 플라이 아웃을 축소할 수 있습니다.

:::row:::
    :::column:::
        축소 된 명령 모음 플라이 아웃<br/>
        ![축소 된 명령 모음 플라이 아웃의 예](images/command-bar-flyout-img-collapsed.png)
    :::column-end:::
    :::column:::
        확장 된 명령 모음 플라이 아웃<br/>
        ![확장 된 명령 모음 플라이 아웃의 예](images/command-bar-flyout-img-expanded.png)
    :::column-end:::
:::row-end:::

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Rotate" Icon="Rotate"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
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

사전에 상황에 맞는 명령을 표시 한 경우 기본 명령이 (명령 모음 플라이 아웃은 축소)는 기본적으로 표시 되어야 합니다. 기본 명령 컬렉션 및 보조 명령이 컬렉션에 상황에 맞는 메뉴에서 일반적으로 이동 하는 추가 명령에서 가장 중요 한 명령을 배치 합니다.

사전 명령이 표시, 일반적으로 명령 모음 플라이 아웃을 표시 하려면 [클릭](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 하거나 [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) 이벤트를 처리 합니다. 설정 플라이 아웃의 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) **임시** 또는 **TransientWithDismissOnPointerMoveAway** 를 축소 된 모드에서 포커스를 수행 하지 않고 플라이 아웃을 엽니다.

Windows 10 Insider Preview부터 텍스트 컨트롤에는 **SelectionFlyout** 속성이 있습니다. 이 속성에 플라이 아웃을 할당할 때 텍스트를 선택 하면 자동으로 표시 됩니다.

### <a name="show-commands-reactively"></a>사후 명령 표시

사후 상황에 맞는 메뉴에 상황에 맞는 명령을 표시 한 경우 (명령 모음 플라이 아웃을 확장 되어야 한다는)는 기본적으로 보조 명령이 표시 됩니다. 이 경우 기본 및 보조 명령 또는 보조 명령이 명령 모음 플라이 아웃 있을 수 있습니다.

명령을 상황에 맞는 메뉴를 표시 하려면 일반적으로 UI 요소의 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 속성에는 플라이 아웃을 할당 합니다. 이렇게이 하면 요소에서 처리 하는 플라이 아웃을 열 및 더 많은 아무 작업도 수행할 필요가 없습니다.

설정에서 보여 주는 플라이 아웃 직접 (예를 들어, [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) 이벤트)를 처리 하는 경우의 **표준** 확장 모드로 플라이 아웃을 열고 포커스를 플라이 아웃의 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 합니다.

> [!TIP]
> 플라이 아웃 및 플라이 아웃의 위치를 제어 하는 방법을 보여 주는 때 옵션에 대 한 자세한 내용은 [플라이 아웃](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)을 참조 하세요.

## <a name="commands-and-content"></a>명령 및 콘텐츠

명령 및 콘텐츠를 추가 하는 데 사용할 수 있는 2 속성이 CommandBarFlyout 컨트롤에: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) 와 [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)합니다.

기본적으로 명령 모음 항목은 **PrimaryCommands** 컬렉션에 추가됩니다. 이러한 명령은 명령 모음에 표시 되 고 축소 또는 확장 모드에 표시 됩니다. CommandBar 달리 기본 명령이 오버플로 보조 명령에 자동으로 수행 하 고 잘릴 수 있습니다.

**SecondaryCommands** 컬렉션에 명령을 추가할 수도 있습니다. 보조 명령이 컨트롤의 메뉴 부분에 표시 되 고 확장된 모드에만 표시 됩니다.

### <a name="app-bar-buttons"></a>앱 바 단추

PrimaryCommands와 SecondaryCommands [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)및 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 컨트롤을 사용 하 여 직접 채울 수 있습니다.

앱 바 단추 컨트롤은 아이콘 및 텍스트 레이블에 따라 구분됩니다. 이러한 컨트롤은 명령 모음에 사용 하기 위해 최적화 하 고는 컨트롤이 명령 모음 또는 오버플로 메뉴에 표시 되는 여부에 따라 모양이 변경 합니다.

- 기본 명령으로 사용 하는 앱 바 단추 아이콘;만 있는 명령 모음에 표시 됩니다. 텍스트 레이블이 표시 되지 않습니다. 다음과 같이 명령에 대 한 설명을 표시 하는 도구 설명이 사용 하는 것이 좋습니다.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 보조 명령으로 사용 되는 앱 바 단추 레이블과 아이콘 표시를 사용 하 여 메뉴에 표시 됩니다.

### <a name="other-content"></a>기타 콘텐츠

명령 모음 플라이 아웃을 AppBarElementContainer에 래핑하여 다른 컨트롤을 추가할 수 있습니다. 이렇게 하면 [DropDownButton]() 또는 [분할 단추]()같은 컨트롤을 추가 하거나 더 복잡 한 UI를 만드는 [StackPanel]() 같은 컨테이너를 추가할 수 있습니다.

> [!NOTE]
> 명령 모음 플라이 아웃의 기본 또는 보조 명령 모음에 추가 하기 위해 요소 [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) 인터페이스를 구현 해야 합니다. AppBarElementContainer 자체 인터페이스를 구현 하지 않는 경우에 명령 모음에 요소를 추가할 수 있도록이 인터페이스를 구현 하는 래퍼를입니다.

여기서는 AppBarElementContainer 명령 모음 플라이 아웃에 추가 요소를 추가 하는 데 사용 됩니다. 분할 단추는 색 중에서 선택할 수 있도록 기본 명령에 추가 됩니다. StackPanel 확대/축소 컨트롤에 대 한 더 복잡 한 레이아웃을 허용 하도록 보조 명령에 추가 됩니다.

> [!NOTE]
> 이 예제에 표시 되는 명령을 구현 하지 않습니다만 명령 모음 플라이 아웃 UI를 보여줍니다. 명령을 구현에 대 한 자세한 내용은 [단추](buttons.md) 및 [명령 디자인 기본 사항](../basics/commanding-basics.md)을 참조 하세요.

:::row:::
    :::column:::
        열려 있는 분할 단추를 사용 하 여 축소 된 명령 모음 플라이 아웃<br/>
        ![분할 단추를 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-split-button.png)
    :::column-end:::
    :::column:::
        사용자 지정 확대/축소 UI 메뉴에 있는 확장 된 명령 모음 플라이 아웃<br/>
        ![복잡 한 UI 사용 하 여 명령 모음 플라이 아웃](images/command-bar-flyout-complex-ui.png)
    :::column-end:::
:::row-end:::

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Color controls -->
    <AppBarElementContainer>
        <SplitButton Height="Auto" Margin="0,4,0,0"
                     ToolTipService.ToolTip="Colors"
                     Background="{ThemeResource AppBarItemBackgroundThemeBrush}">
            <SplitButton.Content>
                <Rectangle Width="20" Height="20">
                    <Rectangle.Fill>
                        <SolidColorBrush Color="Red"/>
                    </Rectangle.Fill>
                </Rectangle>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Text="Red"/>
                    <MenuFlyoutItem Text="Yellow"/>
                    <MenuFlyoutItem Text="Green"/>
                    <MenuFlyoutItem Text="Blue"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Color controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <Style TargetType="Button">
                    <Setter Property="Background"
                            Value="{ThemeResource AppBarItemBackgroundThemeBrush}"/>
                </Style>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,0">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="86"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Zoom"/>
                <StackPanel Orientation="Horizontal" Grid.Column="1">
                    <Button>
                        <SymbolIcon Symbol="Remove"/>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button>
                        <SymbolIcon Symbol="Add"/>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>보조 명령에만 사용 하 여 상황에 맞는 메뉴 만들기

[상황에 맞는 메뉴](menus.md)는 MenuFlyout 대신는 CommandBarFlyout만 보조 명령으로 사용할 수 있습니다.

![만 보조 명령으로 명령 모음 플라이 아웃](images/command-bar-flyout-context-menu.png)

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Pin" Icon="Pin"/>
                <AppBarButton Label="Unpin" Icon="UnPin"/>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

표준 메뉴를 만들려면는 DropDownButton를 사용 하 여는 CommandBarFlyout를 사용할 수 있습니다.

![명령 모음 플라이 아웃으로 드롭다운 메뉴 단추](images/command-bar-flyout-button-menu.png)

```xaml
<DropDownButton Content="Mail">
    <DropDownButton.Flyout>
        <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Icon="MailForward" Label="Forward"/>
                <AppBarButton Icon="MailReply" Label="Reply"/>
                <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

## <a name="command-bar-flyouts-for-text-controls"></a>텍스트 컨트롤에 대 한 명령 모음 플라이 아웃

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) 텍스트 편집에 대 한 명령을 포함 하는 특수 명령 모음 플라이 아웃입니다. 각 텍스트 컨트롤과 텍스트를 선택한 경우 또는 상황에 맞는 메뉴 (마우스 오른쪽 단추)으로 TextCommandBarFlyout 자동으로 표시 됩니다. 텍스트 명령 모음 플라이 아웃 관련 명령을 표시 하도록 선택한 텍스트에 적응 합니다.

:::row:::
    :::column:::
        텍스트 선택에 텍스트 명령 모음 플라이 아웃<br/>
        ![축소 된 텍스트 명령 모음 플라이 아웃](images/command-bar-flyout-text-selection.png)
    :::column-end:::
    :::column:::
        확장 된 텍스트 명령 모음 플라이 아웃<br/>
        ![확장 된 텍스트 명령 모음 플라이 아웃](images/command-bar-flyout-text-full.png)
    :::column-end:::
:::row-end:::

### <a name="available-commands"></a>사용 가능한 명령

이 표는 TextCommandBarFlyout 및 때 나타나는 포함 되어 있는 명령을 보여 줍니다.

| 명령 | 표시 된... |
| ------- | -------- |
| 굵게 | 텍스트 컨트롤 없는 경우 읽기 전용 (RichEditBox만) |
| 기울임꼴 | 텍스트 컨트롤 없는 경우 읽기 전용 (RichEditBox만) |
| 밑줄 | 텍스트 컨트롤 없는 경우 읽기 전용 (RichEditBox만) |
| 검사 | IsSpellCheckEnabled **true** 이 고 철자가 텍스트 선택 됩니다. |
| Cut | 시점과 텍스트 컨트롤은 읽기 전용 텍스트를 선택 합니다. |
| Copy | 텍스트를 선택 하는 경우 |
| Paste | 텍스트 컨트롤은 읽기 전용 및 클립보드의 내용이 때. |
| 실행 취소 | 작업을 취소할 수 있는 경우. |
| 모두 선택 | 때 텍스트를 선택할 수 있습니다. |

### <a name="custom-text-command-bar-flyouts"></a>사용자 지정 텍스트 명령 모음 플라이 아웃

TextCommandBarFlyout 인쇄할 수 및 각 텍스트 컨트롤에 의해 자동으로 관리 됩니다. 그러나 사용자 지정 명령을 사용 하 여 기본 TextCommandBarFlyout 바꿀 수 있습니다.

- 기본 텍스트 선택에 표시 되는 TextCommandBarFlyout로 사용자 지정 CommandBarFlyout (또는 다른 플라이 아웃 유형)을 만들 수 있으며 **SelectionFlyout** 속성에 할당 키를 누릅니다. SelectionFlyout를 **null로**설정 하면 명령이 없는 선택에 표시 됩니다.
- TextCommandBarFlyout 상황에 맞는 메뉴로 표시 되는 기본으로 사용자 지정 CommandBarFlyout (또는 다른 플라이 아웃 유형) 텍스트 컨트롤에 **ContextFlyout** 속성에 할당 합니다. ContextFlyout를 **null로**설정 하면 이전 버전의 텍스트 컨트롤에 표시 된 메뉴 플라이 아웃은 TextCommandBarFlyout 대신 표시 됩니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [XAML 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>관련 문서

- [UWP 앱의 명령 디자인 기본 사항](../basics/commanding-basics.md)
- [CommandBar 클래스](https://msdn.microsoft.com/library/windows/apps/dn279427)
