---
Description: Windows 앱에서 키보드, 게임 패드 및 내게 필요한 옵션 도구를 사용 하 여 프로그래밍 방식으로 포커스 탐색을 관리 하는 방법을 알아봅니다.
title: 키보드, 게임 패드 및 접근성 도구를 사용한 프로그래밍 방식 포커스 탐색
label: Programmatic focus navigation
keywords: 키보드, 게임 컨트롤러, 원격 제어, 탐색, 탐색 전략, 입력, 사용자 조작, 접근성, 유용성
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 887d8329cc95d735ba33ff8dafc5105874206eaf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172547"
---
# <a name="programmatic-focus-navigation"></a>프로그래밍 방식 포커스 탐색

![키보드, 원격 및 D-패드](images/dpad-remote/dpad-remote-keyboard.png)

Windows 응용 프로그램에서 프로그래밍 방식으로 포커스를 이동 하려면 [System.windows.input.focusmanager> 포커스](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드 또는 [Findnextelement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드 중 하나를 사용할 수 있습니다.

[Trymovefocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 는 지정 된 방향에서 포커스가 있는 다음 요소로 포커스를 이동 하는 요소에서 포커스를 변경 하려고 시도 하는 반면, [Findnextelement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 는 지정 된 탐색 방향에 따라 포커스를 받는 요소 ( [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject))를 검색 합니다.

> [!NOTE]
> FindNextFocusableElement는 다음 포커스 가능 요소가 UIElement가 아닌 경우 null을 반환 하는 UIElement를 검색 하기 때문에 [FindNextFocusableElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 대신 [findnextelement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드를 사용 하는 것이 좋습니다 (예: Hyperlink 개체). 

## <a name="find-a-focus-candidate-within-a-scope"></a>범위 내에서 포커스 후보 찾기

특정 UI 트리 내에서 다음 포커스 후보를 검색 하거나 고려 사항에서 특정 요소를 제외 하는 것을 포함 하 여 [Trymovefocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 및 [Findnextelement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)모두에 대해 포커스 탐색 동작을 사용자 지정할 수 있습니다.

이 예제에서는 TicTacToe game을 사용 하 여 전 [Movefocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 및 [Findnextelement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 메서드를 보여 줍니다.

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
        XYFocusNavigationStrategyOverride = XYFocusNavigationStrategyOverride.Projection
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

[Findnextelementoptions](/uwp/api/windows.ui.xaml.input.findnextelementoptions) 를 사용 하 여 포커스 후보가 식별 되는 방법을 추가로 사용자 지정 합니다. 이 개체는 다음과 같은 속성을 제공 합니다.

- [Searchroot](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) -이 DependencyObject의 자식에 대 한 포커스 탐색 후보 검색의 범위입니다. Null은 시각적 트리의 루트에서 검색을 시작 함을 나타냅니다.

> [!Important] 
> 해당 요소를 방향성 영역 외부에 배치한 **Searchroot** 의 하위 항목에 하나 이상의 변환이 적용 되는 경우 이러한 요소는 여전히 후보로 간주 됩니다.

- [ExclusionRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) 탐색 후보는 모든 겹치는 개체가 탐색 포커스에서 제외 되는 "가상의" 경계 사각형을 사용 하 여 식별 됩니다. 이 사각형은 계산에만 사용 되며 시각적 트리에는 추가 되지 않습니다.
- [Hintrect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) -포커스 탐색 후보는 포커스를 받을 가능성이 가장 높은 요소를 식별 하는 "가상의" 경계 사각형을 사용 하 여 식별 됩니다. 이 사각형은 계산에만 사용 되며 시각적 트리에는 추가 되지 않습니다.
- [XYFocusNavigationStrategyOverride](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride) -포커스를 받을 최적의 후보 요소를 식별 하는 데 사용 되는 포커스 탐색 전략입니다.

다음 그림에서는 이러한 개념 중 일부를 보여 줍니다. 

요소 B에 포커스가 있을 때 FindNextElement는 오른쪽으로 탐색할 때 포커스 후보로 I를 식별 합니다. 그 이유는 다음과 같습니다.
- 의 [Hintrect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) 로 인해 시작 참조는 B가 아니라 a입니다.
- MyPanel가 [Searchroot](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) 로 지정 되었기 때문에 C는 후보가 아닙니다.
- F는 [ExclusionRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) 이 겹칩니다.

![탐색 힌트를 사용 하 여 사용자 지정 포커스 탐색 동작](images/keyboard/navigation-hints.png)

*탐색 힌트를 사용 하 여 사용자 지정 포커스 탐색 동작*

## <a name="navigation-focus-events"></a>포커스 이벤트 탐색

### <a name="nofocuscandidatefound-event"></a>NoFocusCandidateFound 이벤트

[NoFocusCandidateFound](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) 이벤트는 tab 또는 화살표 키를 누르고 지정 된 방향에 포커스 후보가 없는 경우에 발생 합니다. 이 이벤트는 [Trymovefocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)에 대해 발생 하지 않습니다.

이 이벤트는 라우트된 이벤트 이므로 연속 된 요소부터 연속 된 부모 개체까지 개체 트리의 루트로 버블링 됩니다. 그러면 적절 한 경우 이벤트를 처리할 수 있습니다.

<a name="split-view-code-sample"></a>

여기서는 사용자가 포커스를 받을 수 있는 왼쪽 컨트롤의 왼쪽으로 포커스를 이동 하려고 할 때 그리드에서 [SplitView](/uwp/api/windows.ui.xaml.controls.splitview) 를 여는 방법을 보여 줍니다 ( [Xbox 및 TV에 대 한 디자인](../devices/designing-for-tv.md#navigation-pane)참조).

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

### <a name="gotfocus-and-lostfocus-events"></a>GotFocus 및 LostFocus 이벤트
[Uielement](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 및 [uielement](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 이벤트는 요소가 포커스를 얻거나 포커스를 잃을 때 발생 합니다. 이 이벤트는 [Trymovefocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)에 대해 발생 하지 않습니다.

이러한 이벤트는 라우트된 이벤트 이므로 연속 된 요소부터 연속 된 부모 개체까지 개체 트리의 루트까지 버블링 됩니다. 그러면 적절 한 경우 이벤트를 처리할 수 있습니다.

### <a name="gettingfocus-and-losingfocus-events"></a>GetLosingFocus Focus 및 이벤트 이벤트

[Uielement. GetLosingFocus focus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 및 uielement [UIElement.LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 이벤트는 각각의 [uielement](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 및 [uielement](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 이벤트 이전에 발생 합니다. 

이러한 이벤트는 라우트된 이벤트 이므로 연속 된 요소부터 연속 된 부모 개체까지 개체 트리의 루트까지 버블링 됩니다. 포커스 변경이 발생 하기 전에 발생 하는 것 처럼 포커스 변경을 리디렉션하거나 취소할 수 있습니다.

[GetLosingFocus focus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 및 [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 는 동기 이벤트 이므로 이러한 이벤트가 버블링 되는 동안 포커스를 이동할 수 없습니다. 그러나 [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 와 [LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 는 비동기 이벤트입니다. 즉, 처리기가 실행 되기 전에 포커스가 다시 이동 하지 않을 수 있습니다.

포커스가 컨트롤에 대 한 호출을 통해 이동 하는 경우 호출 하는 동안 [get로](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 포커스가 이동 하 고 호출 후에는 [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 가 발생 합니다 [.](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)

포커스 탐색 대상은 [GettingFocusEventArgs](/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) 속성을 통해 [getLosingFocus 포커스](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 및 이벤트 ( [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 포커스 이동 전)를 통해 변경할 수 있습니다. 대상이 변경 되어도 이벤트는 계속 버블링 되며 대상을 다시 변경할 수 있습니다.

재진입 문제를 방지 하기 위해 이러한 이벤트가 버블링 되는 동안 포커스를 이동 하려고 하면 ( [Trymovefocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 또는 [컨트롤](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)을 사용 하 여) 예외가 throw 됩니다.

이러한 이벤트는 포커스 이동의 원인 (탭 탐색, 방향 탐색, 프로그래밍 방식 탐색 등)에 관계 없이 발생 합니다.

포커스 이벤트의 실행 순서는 다음과 같습니다.

1.  [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 포커스가 손실 된 포커스 요소로 다시 설정 되거나, 추가 [취소가](/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) 성공 하면 추가 이벤트가 발생 하지 않습니다.
2.  [Get이상 포커스](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 포커스가 손실 된 포커스 요소로 다시 설정 되거나, 추가 [취소가](/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) 성공 하면 추가 이벤트가 발생 하지 않습니다.
3.  [LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [System.windows.uielement.gotfocus>](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

다음 이미지는에서 오른쪽으로 이동할 때 XYFocus에서 B4를 후보로 선택 하는 방법을 보여 줍니다. B4는 ListView에 포커스를 다시 할당할 기회가 있는 Get로 포커스 이벤트를 발생 시킵니다.

![Get로 포커스 이벤트에서 포커스 탐색 대상 변경](images/keyboard/focus-events.png)

*Get로 포커스 이벤트에서 포커스 탐색 대상 변경*

여기서는 [get로 포커스](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 이벤트를 처리 하 고 포커스를 리디렉션하는 방법을 보여 줍니다.

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

여기서는 메뉴를 닫을 때 [명령 모음](/uwp/api/windows.ui.xaml.controls.commandbar) 에 대 한 [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 이벤트를 처리 하 고 포커스를 설정 하는 방법을 보여 줍니다.

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

## <a name="find-the-first-and-last-focusable-element"></a>포커스를 받을 첫 번째 및 마지막 요소 찾기

FindFirstFocusableElement 및 [system.windows.input.focusmanager>](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) 메서드는 개체의 범위 내에서 포커스를 받을 수 있는 첫 번째 요소 ( [UIElement](/uwp/api/windows.ui.xaml.uielement) 의 요소 트리 또는 [textelement](/uwp/api/windows.ui.xaml.documents.textelement)의 텍스트 트리)로 포커스를 이동 [system.windows.input.focusmanager>](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) 합니다. 범위는 호출에서 지정 됩니다. 인수가 null 이면 범위는 현재 창이 됩니다.

범위에서 포커스 후보를 식별할 수 없으면 null이 반환 됩니다.

여기서는 명령 모음 단추가 줄 바꿈 방향 동작 ( [키보드 상호 작용](keyboard-interactions.md#popup-ui)참조)을 갖도록 지정 하는 방법을 보여 줍니다.

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

## <a name="related-articles"></a>관련된 문서

- [키보드, 게임 패드, 원격 제어 및 내게 필요한 옵션 도구에 대 한 포커스 탐색](focus-navigation.md)
- [키보드 조작](keyboard-interactions.md)
- [키보드 접근성](../accessibility/keyboard-accessibility.md)