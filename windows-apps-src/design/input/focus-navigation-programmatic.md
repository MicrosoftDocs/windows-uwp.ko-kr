---
author: Karl-Bridge-Microsoft
Description: Learn how to programmatically manage focus navigation with keyboard, gamepad, and accessibility tools in a UWP app.
title: 키보드, 게임 패드 및 접근성 도구를 사용한 프로그래밍 방식 포커스 탐색
label: Programmatic focus navigation
keywords: 키보드, 게임 컨트롤러, 원격 제어, 탐색, 탐색 전략, 입력, 사용자 조작, 접근성, 유용성
ms.author: kbridge
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d2317b419a2679d13e846690bbaca0eb212a245e
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5876876"
---
# <a name="programmatic-focus-navigation"></a>프로그래밍 방식 포커스 탐색

![키보드, 원격 및 D 패드](images/dpad-remote/dpad-remote-keyboard.png)

UWP 응용 프로그램에서 포커스를 프로그래밍 방식으로 이동하려면 [FocusManager.TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드 또는 [FocusManager.FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드를 사용하면 됩니다.

[TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)는 지정된 방향으로 현재 포커스가 있는 요소에서 다음 포커스 가능한 요소로 포커스를 변경하려고 하는 반면, [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)는 지정된 탐색 방향을 토대로 포커스를 받을 요소를 [DependencyObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject)) 형식으로 검색합니다(방향 탐색에만 해당됨, 탭 탐색 에뮬레이션에는 사용할 수 없음).

> [!NOTE]
> FindNextFocusableElement는 UIElement를 검색하며, 다음 포커스 가능 요소가 UIElement가 아닌 경우(하이퍼링크 개체 등) null을 반환하므로 [FindNextFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)보다는 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드를 사용하는 것이 좋습니다. 

## <a name="find-a-focus-candidate-within-a-scope"></a>범위 내 포커스 후보 찾기

[TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 및 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 모두에 대해 포커스 탐색 동작을 사용자 지정할 수 있습니다. 여기에는 특정 UI 트리 내 다음 포커스 후보 검색 또는 특정 요소 제외 등이 포함됩니다.

이 예에서는 TicTacToe 게임을 사용하여 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 및 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드를 시연합니다.

```xaml
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
    <Button Content="Start Game" />
    <Button Content="Undo Movement" />
    <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="0" 
            x:Name="Cell00" />
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="0" 
            x:Name="Cell10"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="0" 
            x:Name="Cell20"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="1" 
            x:Name="Cell01"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="1" 
            x:Name="Cell11"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="1" 
            x:Name="Cell21"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="2" 
            x:Name="Cell02"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="2" 
            x:Name="Cell22"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="2" 
            x:Name="Cell32"/>
    </Grid>
</StackPanel>
```

```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
{
    DependencyObject candidate = null;

    var options = new FindNextElementOptions ()
    {
        SearchRoot = TicTacToeGrid,
        NavigationStrategy = NavigationStrategyMode.Heuristic
    };

    switch (e.Key)
    {
        case Windows.System.VirtualKey.Up:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Up, options);
            break;
        case Windows.System.VirtualKey.Down:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Down, options);
            break;
        case Windows.System.VirtualKey.Left:
            candidate = FocusManager.FindNextElement(
                FocusNavigationDirection.Left, options);
            break;
        case Windows.System.VirtualKey.Right:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Right, options);
            break;
    }
    // Also consider whether candidate is a Hyperlink, WebView, or TextBlock.
    if (candidate != null && candidate is Control)
    {
        (candidate as Control).Focus(FocusState.Keyboard);
    }
}
```

포커스 후보 식별 방식을 더 자세히 사용자 지정하려면 [FindNextElementOptions](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.findnextelementoptions)을 사용하세요. 이 개체는 다음과 같은 속성을 제공합니다.

- [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) - 이 DependencyObject의 하위 항목으로 포커스 탐색 후보를 검색하도록 범위 지정합니다. Null은 시각적 트리의 루트에서부터 검색을 시작함을 나타냅니다.

> [!Important] 
> 방향 영역 외부에 후보를 배치하는 **SearchRoot** 하위 항목에 하나 이상의 변환이 적용되면 이러한 요소는 여전히 후보로 간주됩니다.

- [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) - 중첩되는 모든 개체가 탐색 포커스에서 제외되는 "가상" 경계 사각형을 사용하여 포커스 탐색 후보가 식별됩니다. 이 사각형은 계산에만 사용되며 시각적 트리에는 추가되지 않습니다.
- [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) - 포커스를 받을 가능성이 가장 높은 요소를 식별하는 "가상" 경계 사각형을 사용하여 포커스 탐색 후보가 식별됩니다. 이 사각형은 계산에만 사용되며 시각적 트리에는 추가되지 않습니다.
- [XYFocusNavigationStrategyOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride) - 포커스 탐색 전략을 사용하여 포커스를 받는 최상의 후보 요소가 식별됩니다.

다음은 이러한 개념 중 일부를 보여주는 이미지입니다. 

요소 B에 포커스가 있으면, 오른쪽으로 탐색 시 FindNextElement가 I를 포커스 후보로 식별합니다. 이유는 다음과 같습니다.
- [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect)가 A에 있으므로 시작 참조가 B가 아닌 A입니다.
- MyPanel이 [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)로 지정되었으므로 C는 후보가 아닙니다.
- [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect)가 F와 중첩되므로 F는 후보가 아닙니다.

![탐색 힌트를 사용한 사용자 지정 포커스 탐색 동작](images/keyboard/navigation-hints.png)

*탐색 힌트를 사용한 사용자 지정 포커스 탐색 동작*

## <a name="navigation-focus-events"></a>탐색 포커스 이벤트

### <a name="nofocuscandidatefound-event"></a>NoFocusCandidateFound 이벤트

탭 및 화살표 키를 눌렀는데 지정된 방향에 포커스 후보가 없는 경우 [UIElement.NoFocusCandidateFound](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) 이벤트가 발생합니다. 이 이벤트는 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)에 대해서는 발생하지 않습니다.

이 이벤트는 라우트된 이벤트이기 때문에 포커스가 설정된 요소에서 다음 부모 개체를 지나 개체 트리의 루트로 버블링됩니다. 이에 따라 해당하는 경우 이벤트를 처리할 수 있습니다.

<a name="split-view-code-sample"></a>

다음은 사용자가 가장 왼쪽에 있는 포커스 가능 컨트롤의 왼쪽으로 포커스를 이동하려고 시도할 때 그리드가 [SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)를 여는 과정을 보여줍니다([Xbox 및 TV용 디자인](../devices/designing-for-tv.md#navigation-pane) 참조).

```xaml
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">
...
</Grid>
```

```csharp
private void OnNoFocusCandidateFound (
    UIElement sender, NoFocusCandidateFoundEventArgs args)
{
    if(args.NavigationDirection == FocusNavigationDirection.Left)
    {
        if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
        args.InputDevice == FocusInputDeviceKind.GameController )
            {
                OpenSplitPaneView();
            }
        args.Handled = true;
    }
}
```

### <a name="gotfocus-and-lostfocus-events"></a>GotFocus와 LostFocus 이벤트
요소가 포커스가 얻거나 잃게 되면 각각 [UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 및 [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 이벤트가 발생합니다. 이 이벤트는 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)에 대해서는 발생하지 않습니다.

이러한 이벤트는 라우트된 이벤트이기 때문에 포커스가 설정된 요소에서 다음 부모 개체를 지나 개체 트리의 루트로 버블링됩니다. 이에 따라 해당하는 경우 이벤트를 처리할 수 있습니다.

### <a name="gettingfocus-and-losingfocus-events"></a>GettingFocus 및 LosingFocus 이벤트

[UIElement.GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 및 [UIElement.LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 이벤트는 각각 [UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 및 [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 이벤트가 발생하기 전에 발생합니다. 

이러한 이벤트는 라우트된 이벤트이기 때문에 포커스가 설정된 요소에서 다음 부모 개체를 지나 개체 트리의 루트로 버블링됩니다. 이 이벤트는 포커스 변경이 발생하기 전에 발생하므로 포커스 변경을 리디렉팅하거나 취소할 수 있습니다.

[GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 및 [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus)는 동기 이벤트이므로 두 이벤트가 버블링하는 동안 포커스가 이동되지 않습니다. 그러나 [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 및 [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)는 비동기 이벤트이므로 처리기가 실행되기 전에 포커스가 다시 이동하지 않는다는 보장이 없습니다.

[Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_) 호출 동안 포커스가 이동하는 경우, 호출 도중 [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus)가 발생하는 반면, [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)는 호출 후 발생합니다.

포커스 탐색 대상은 [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 및 [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 이벤트 동안(포커스 이동 이전) [GettingFocusEventArgs.NewFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) 속성을 통해 변경될 수 있습니다. 대상이 변경되더라도 이벤트는 계속 버블링되며 대상이 다시 변경될 수도 있습니다.

재진입 문제를 방지하기 위해 이러한 이벤트가 버블링되는 동안 포커스를 이동하려고([TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 또는 [Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_) 사용) 시도하면 예외가 발생합니다.

이러한 이벤트는 포커스가 이동되는 이유(예: 탭 탐색, 방향 탐색 및 프로그래밍 방식 탐색)에 관계없이 발생합니다.

다음은 포커스 이벤트 실행 순서입니다.

1.  [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 포커스가 포커스를 잃은 요소로 다시 재설정되거나 [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel)이 성공하는 경우 추가 이벤트가 발생하지 않습니다.
2.  [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 포커스가 포커스를 잃은 요소로 다시 재설정되거나 [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel)이 성공하는 경우 추가 이벤트가 발생하지 않습니다.
3.  [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

다음 그림은 A에서 오른쪽으로 이동하는 방법과 시기를 보여 주며, XYFocus는 B4를 후보로 선택합니다. 그런 다음 B4가 GettingFocus 이벤트를 발생시키고 ListView는 B3에 포커스를 다시 할당할 수 있습니다.

![GettingFocus 이벤트에서 포커스 탐색 대상 변경](images/keyboard/focus-events.png)

*GettingFocus 이벤트에서 포커스 탐색 대상 변경*

다음은 [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 이벤트를 처리하고 포커스를 리디렉팅하는 방법을 보여줍니다.

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
{
    //Redirect the focus only when the focus comes from outside of the ListView.
    // move the focus to the selected item
    if (MyListView.SelectedIndex != -1 && 
        IsNotAChildOf(MyListView, args.OldFocusedElement))
    {
        var selectedContainer = 
            MyListView.ContainerFromItem(MyListView.SelectedItem);
        if (args.FocusState == 
            FocusState.Keyboard && 
            args.NewFocusedElement != selectedContainer)
        {
            args.TryRedirect(
                MyListView.ContainerFromItem(MyListView.SelectedItem));
            args.Handled = true;
        }
    }
}
```

다음은 [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar)에 대해 [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 이벤트를 처리하고 메뉴가 닫힐 때 포커스를 설정하는 방법을 보여줍니다.

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (MyCommandBar.IsOpen == true && 
        IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
    {
        if (args.TryCancel())
        {
            args.Handled = true;
        }
    }
}
```

## <a name="find-the-first-and-last-focusable-element"></a>첫 번째 및 마지막 포커스 가능 요소 찾기

[FocusManager.FindFirstFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) 및 [FocusManager.FindLastFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) 메서드는 개체의 범위 내에 있는 첫 번째 또는 마지막 포커스 가능 요소로 포커스를 이동합니다([UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)의 요소 트리 또는 [TextElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.documents.textelement)의 텍스트 트리). 범위는 호출에 지정되어 있습니다(인수가 null인 경우 범위는 현재 창임).

범위 내에서 포커스 후보를 식별할 수 없으면 null이 반환됩니다.

다음은 CommandBar 단추에 랩어라운드 방향 동작을 지정하는 방법을 보여줍니다([키보드 조작](keyboard-interactions.md#popup-ui) 참조).

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
    <AppBarButton Icon="Back" Label="Back" />
    <AppBarButton Icon="Stop" Label="Stop" />
    <AppBarButton Icon="Play" Label="Play" />
    <AppBarButton Icon="Forward" Label="Forward" />

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" />
        <AppBarButton Icon="ReShare" Label="Share" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
    {
        DependencyObject candidate = null;
        if (args.Direction == FocusNavigationDirection.Left)
        {
            candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
        }
        else if (args.Direction == FocusNavigationDirection.Right)
        {
            candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
        }
        if (candidate != null)
        {
            args.NewFocusedElement = candidate;
            args.Handled = true;
        }
    }
}
```

## <a name="related-articles"></a>관련 문서

- [키보드, 게임 패드, 원격 제어 및 접근성 도구에 대한 포커스 탐색](focus-navigation.md)
- [키보드 조작](keyboard-interactions.md)
- [키보드 접근성](../accessibility/keyboard-accessibility.md)