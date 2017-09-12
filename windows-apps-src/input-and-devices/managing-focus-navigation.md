---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 8389d1f089d507a087693879a793b95572d7860b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/22/2017
---
# <a name="managing-focus-navigation"></a>포커스 탐색 관리

많은 입력 장치, 다시 말해서 키보드, Windows 내레이터, 게임 패드, 리모컨 등의 접근성 도구는 응용 프로그램 UI 주변에서 포커스 화면 효과를 이동하는 공통적인 기본 메커니즘을 공유합니다. 포커스 화면 효과 및 탐색에 대한 자세한 내용은 [키보드 조작](keyboard-interactions.md) 문서 및 [Xbox 및 TV용 디자인](designing-for-tv.md#xy-focus-navigation-and-interaction) 문서를 읽어 보세요.

다음은 입력 유형에 관계없이 응용 프로그램이 포커스를 응용 프로그램 UI 주위로 이동할 수 있는 방법을 제공하며, 이를 통해 응용 프로그램이 여러 입력 유형을 원활하게 지원하는 단일 코드를 작성할 수 있습니다.

## <a name="navigation-strategies-properties-to-fine-tune-focus-movements"></a>포커스 이동을 미세하게 조정할 수 있는 탐색 전략 속성

XYFocus 탐색 전략 속성을 사용하여 화살표 키 입력에 따라 포커스를 받을 컨트롤을 지정합니다. 이러한 속성은 다음과 같습니다.

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

이러한 속성에는 **XYFocusNavigationStrategy** 유형의 값이 있습니다. 이 값을 **자동**으로 설정하면 요소의 상위 항목에 따라 요소의 동작이 결정됩니다. 모든 요소가 **자동**으로 설정되면 **프로젝션**이 사용됩니다.

이러한 탐색 전략은 키보드, 게임 패드 및 리모컨에 적용됩니다.

### <a name="projection"></a>프로젝션

프로젝션 전략은 탐색 방향에서 현재 포커스된 요소의 테두리를 표시할 때 발생하는 첫 번째 요소로 포커스를 이동합니다.

> [!NOTE]
> 이전에 포커스가 설정된 요소, 탐색 방향 축과의 근접성 등의 기타 요소가 결과에 영향을 미칠 수 있습니다.

![프로젝션](images/keyboard/projection.png)

*A의 아래쪽 가장자리 프로젝션을 기반으로 아래쪽 탐색 시 포커스가 A에서 D로 이동*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

NavigationDirectionDistance 전략은 탐색 방향의 축과 가장 가까운 요소로 포커스를 이동합니다.

탐색 방향과 일치하는 경계 직사각형의 가장자리가 확장 및 프로젝션되어 후보 대상을 식별합니다. 발생하는 첫 번째 요소는 대상으로 식별됩니다. 후보가 여러 개인 경우 가장 가까운 요소가 대상으로 식별됩니다. 그래도 후보가 여러 개인 경우 맨 위/맨 왼쪽에 있는 요소가 후보로 식별됩니다.

![탐색 방향 거리](images/keyboard/navigation-direction-distance.png)

*아래쪽 탐색 시 포커스가 A에서 C로 이동한 다음 C에서 B로 이동*


### <a name="rectilineardistance"></a>RectilinearDistance

RectilinearDistance 전략은 가장 짧은 2D 거리를 기준으로 가장 가까이에 있는 요소에 포커스를 이동합니다(맨해튼 메트릭). 이 거리는 각 후보의 기본 거리와 보조 거리를 추가하여 계산됩니다. 거리가 같은 경우 방향이 위쪽 또는 아래쪽이면 왼쪽 첫 번째 요소가 선택되고, 방향이 왼쪽 또는 오른쪽이면 위쪽 첫 번째 요소가 선택됩니다.

다음 그림에서는 A에 포커스가 있으면 포커스가 B로 이동되는 것을 보여 주며 그 이유는 다음과 같습니다.

-   거리(A, B, 아래쪽) = 10 + 0 = 10
-   거리(A, C, 아래쪽) = 0 + 30 = 30
-   거리(A, D, 아래쪽) 30 + 0 = 30

![일직선 거리](images/keyboard/rectilinear-distance.png)

*A에 포커스가 있을 때 RectilinearDistance 전략을 사용하면 포커스가 B로 이동*

다음 그림에서는 XYFocusKeyboardNavigation과는 달리 자식 요소가 아닌 요소 자체에 탐색 전략이 적용되는 방식을 보여 줍니다.

예를 들어 E에 포커스가 있을 때 오른쪽 화살표를 누르면 RectilinearDistance 전략을 사용하여 포커스가 F로 이동하지만, D에 포커스가 있을 때 오른쪽 화살표를 누르면 프로젝션 전략을 사용하여 포커스가 H로 이동합니다.

주 컨테이너 전략(탐색 방향 거리)은 적용되지 않습니다. 포커스가 H, F 또는 G에 있을 때에만 사용됩니다.

![여러 탐색 전략](images/keyboard/different-navigation-strategies.png)

*적용되는 탐색 전략에 따른 여러 포커스 동작*

다음 예에서는 Windows 10 시작 메뉴의 타일 패널 동작을 시뮬레이션합니다. 이 예에서 위쪽 및 아래쪽 화살표를 누르면 NavigationDirectionDistance 전략이 사용되고, 왼쪽 및 오른쪽 화살표를 누르면 프로젝션 전략이 사용됩니다.

```XAML
<start:TileGridView
        XYFocusKeyboardNavigation ="Default"
        XYFocusUpNavigationStrategy=" NavigationDirectionDistance "
        XYFocusDownNavigationStrategy=" NavigationDirectionDistance "
        XYFocusLeftNavigationStrategy="Projection"
        XYFocusRightNavigationStrategy="Projection"
/>
```

## <a name="move-focus-programmatically"></a>프로그래밍 방식으로 포커스 이동

포커스를 프로그래밍 방식으로 이동하려면 [FocusManager.TryMoveFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.TryMoveFocus) 메서드 또는 [FocusManager.FindNextElement](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.FindNextFocusableElement) 메서드를 사용합니다.

탐색 키를 누르면 TryMoveFocus가 포커스를 설정합니다.
FindNextElement는 TryMoveFocus가 이동할 요소(DependencyObject)를 반환합니다.

**참고** **FindNextFocusableElement** 대신 **FindNextElement** 메서드를 사용하는 것이 좋습니다. FindNextFocusableElement는 UIElement를 반환하려고 시도하지만, 그 다음 포커스 가능 요소가 하이퍼링크이면 null을 반환합니다. 왜냐하면 하이퍼링크는 UIElement가 아니기 때문입니다. FindNextElement 메서드는 그 대신 DependencyObject를 반환합니다.

### <a name="find-a-focus-candidate-in-a-scope"></a>범위에서 포커스 후보 찾기

FocusManager.FindNextElement 및 FocusManager.TryMoveFocus는 그 다음 포커스 가능 요소 검색 동작을 사용자 지정할 수 있습니다. 특정 UI 트리 내에서 검색하거나 특정 탐색 지역의 요소를 제외할 수 있습니다.

이 예는 TicTacToe 게임 구현에서 가져온 것이며 FindNextElement 및 TryMoveFocus 메서드 사용 방법을 보여 줍니다.

```XAML
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
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="0" x:Name="Cell00" />
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="0" x:Name="Cell10"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="0" x:Name="Cell20"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="1" x:Name="Cell01"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="1" x:Name="Cell11"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="1" x:Name="Cell21"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="2" x:Name="Cell02"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="2" x:Name="Cell22"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="2" x:Name="Cell32"/>
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
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Up, options);
                    break;
                case Windows.System.VirtualKey.Down:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Down, options);
                    break;
                case Windows.System.VirtualKey.Left:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Left, options);
                    break;
                case Windows.System.VirtualKey.Right:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Right, options);
                    break;
            }
         //Note you should also consider whether is a Hyperlink, WebView, or TextBlock.
            if (candidate != null && candidate is Control)
            {
                (candidate as Control).Focus(FocusState.Keyboard);
            }
        }
```

**참고** FocusNavigationDirection.Left 및 FocusNavigationDirection.Right는 RTL 시나리오를 지원하는 데 적합합니다(가까운 곳에서 먼 곳으로). (이는 Xaml의 나머지 부분과 일치합니다.)

FindNextElementOptions 클래스를 사용하여 포커스 후보 검색을 조정할 수 있습니다. 이 개체는 다음과 같은 속성을 갖고 있습니다.

-   **SearchRoot** DependencyObject: 후보 검색 범위를 이 DependencyObject의 자식으로 지정합니다. Null 값은 앱의 시각적 트리 전체를 나타냅니다.
-   **ExclusionRect** Rect: 렌더링되면 이 사각형에서 하나 이상의 픽셀과 겹치는 항목을 검색에서 제외합니다.
-   **HintRect** Rect: 포커스가 설정된 항목을 참조로 사용하여 잠재적 후보를 계산합니다. 이 사각형을 통해 개발자는 포커스가 설정된 요소 대신 다른 참조를 지정할 수 있습니다. 사각형은 계산에만 사용되는 "가상" 사각형입니다. 절대로 실제 사각형으로 변환되어 시각적 트리에 추가되지 않습니다.
-   **XYFocusNavigationStrategyOverride** XYFocusNavigationStrategyOverride: 포커스를 이동하는 데 사용되는 탐색 전략입니다. 유형은 열거형이고 값은 없음(0 값), 자동, RectilinearDistance, NavigationDirectionDistance 및 프로젝션입니다.
    **중요** **SearchRoot**는 렌더링된(기하) 영역을 계산하거나 영역 내 후보를 가져오는 데 사용되지 않습니다. 따라서 방향 영역 외부에 후보를 배치하는 DependencyObject 하위 항목에 변환이 적용되면 이러한 요소는 여전히 후보로 간주됩니다.

FindNextElement 오버로드 옵션은 탭 탐색에 사용할 수 없으며 방향 탐색만 지원합니다.

다음 그림은 이러한 옵션을 보여 줍니다.

예를 들어 B에 포커스가 있을 때 FindNextElement(포커스가 오른쪽으로 움직임을 나타내는 다양한 옵션 포함)를 호출하면 I에 포커스가 설정되며 그 이유는 다음과 같습니다.

1.  HintRect 때문에 참조는 B가 아닌 A
2.  잠재적 후보는 MyPanel(SearchRoot)에 있어야 하므로 C는 제외
3.  F는 제외 사각형과 겹치므로 제외

![탐색 힌트](images/keyboard/navigation-hints.png)

*탐색 힌트 기반의 포커스 동작*

### <a name="no-focus-candidate-available"></a>사용 가능한 포커스 후보 없음

사용자가 Tab 또는 화살표 키로 포커스를 이동하려고 시도하면 UIElement.NoFocusCandidateFound 이벤트가 발생하지만, 지정된 방향에 가능성 있는 후보가 없습니다. 이 이벤트는 FocusManager.TryMoveFocus()에 대해서는 발생하지 않습니다.

이 이벤트는 라우트된 이벤트이기 때문에 포커스가 설정된 요소에서 다음 부모 개체를 지나 개체 트리의 루트로 버블링됩니다. 이러한 방식으로, 해당하는 경우 이벤트를 처리할 수 있습니다.

<a name="split-view-code-sample"></a> 다음 예에서는 [TV 설명서 디자인](designing-for-tv.md#navigation-pane)에 설명된 것처럼 사용자가 맨 왼쪽에 있는 포커스 가능 컨트롤의 왼쪽으로 포커스를 이동하려고 시도하면 분할 보기를 여는 그리드를 보여 줍니다

```XAML
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">  ...</Grid>
```

```csharp
private void OnNoFocusCandidateFound (UIElement sender, NoFocusCandidateFoundEventArgs args)
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

## <a name="focus-events"></a>포커스 이벤트

컨트롤에 포커스가 설정되거나 설정된 포커스가 사라지면 [UIElement.GotFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.GotFocus) 및 UIElement.LostFocus 이벤트가 발생합니다. 이 이벤트는 FocusManager.TryMoveFocus()에 대해서는 발생하지 않습니다.

이 이벤트는 라우트된 이벤트이기 때문에 포커스가 설정된 요소에서 다음 부모 개체를 지나 개체 트리의 루트로 버블링됩니다. 이러한 방식으로, 해당하는 경우 이벤트를 처리할 수 있습니다.

**UIElement.GettingFocus** 및 **UIElement.LosingFocus** 이벤트 역시 컨트롤에서 버블링되지만 포커스 변경이 발생하기 *전*에 버블링됩니다. 따라서 앱에서 포커스 변경을 리디렉션 또는 취소할 수 있는 기회가 있습니다.

GettingFocus 이벤트와 LosingFocus 이벤트는 동기적인 반면 GotFocus 이벤트와 LostFocus 이벤트는 비동기적입니다. 예를 들어 앱에서 Control.Focus() 메서드를 호출하여 포커스가 이동하는 경우 GettingFocus 이벤트는 호출하는 동안 발생하지만 GotFocus 이벤트는 호출 후 약간의 시간이 지난 후 발생합니다.

UIElements는 GettingFocus 및 LosingFocus 이벤트에 연결하고 포커스가 이동되기 전에 대상을 변경할 수 있습니다(NewFocusedElement 속성을 사용하여). 대상이 변경되더라도 이 이벤트는 계속 버블링되며 다른 부모가 대상을 다시 변경할 수 있습니다.

재진입 문제를 방지하기 위해 이러한 이벤트가 버블링되는 동안 포커스를 이동하려고(FocusManager.TryMoveFocus 또는 Control.Focus를 사용하여) 시도하면 예외가 발생합니다.

이러한 이벤트는 포커스가 이동되는 이유(예: 탭 탐색, XYFocus 탐색, 프로그래밍 방식)에 관계없이 발생합니다.

다음 그림은 A에서 오른쪽으로 이동하는 방법과 시기를 보여 주며, XYFocus는 LVI4를 후보로 선택합니다. 그런 다음 LVI4가 GettingFocus 이벤트를 발생시키고 ListView는 LVI3에 포커스를 다시 할당할 수 있습니다.

![포커스 이벤트](images/keyboard/focus-events.png)

*포커스 이벤트*

이 코드 예제는 이전에 포커스가 설정된 위치에 따라 OnGettingFocus 이벤트를 처리하고 포커스를 리디렉션하는 방법을 보여 줍니다.

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
            if (MyListView.SelectedIndex != -1 && IsNotAChildOf(MyListView, args.OldFocusedElement))
            {
                var selectedContainer = MyListView.ContainerFromItem(MyListView.SelectedItem);
                if (args.FocusState == FocusState.Keyboard && args.NewFocusedElement != selectedContainer)
                {
                    args.NewFocusedElement = MyListView.ContainerFromItem(MyListView.SelectedItem);
                    args.Handled = true;
                }
            }        
  }
```

이 코드 예제는 CommandBar 개체에 대한 OnLosingFocus 이벤트를 처리하고 드롭다운 메뉴가 닫힐 때까지 기다렸다가 포커스를 설정하는 방법을 보여 줍니다.

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
          if (MyCommandBar.IsOpen == true && IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
          {
              args.Cancel = true;
              args.Handled = true;
          }
}
```

### <a name="losingfocus-and-gettingfocus-event-order"></a>LosingFocus 및 GettingFocus 이벤트 순서

LosingFocus 및 GettingFocus는 동기 이벤트이므로 두 이벤트가 버블링하는 동안 포커스가 이동되지 않습니다. 그러나 LostFocus 및 GotFocus는 비동기 이벤트이므로 처리기가 실행되기 전에 포커스가 다시 이동하지 않는다는 보장이 없습니다. LostFocus 처리기가 GotFocus 처리기보다 먼저 실행된다는 것만 보장할 수 있습니다.

실행 순서는 다음과 같습니다.

1.  동기 LosingFocus: LosingFocus가 포커스를 잃는 요소 또는 Cancel=true로 설정하는 요소에 포커스를 다시 설정하는 경우 더 이상 이벤트가 발생하지 않습니다.
2.  동기 GettingFocus: GettingFocus가 포커스를 잃는 요소 또는 Cancel=true로 설정하는 요소에 포커스를 다시 설정하는 경우 더 이상 이벤트가 발생하지 않습니다.
3.  비동기 LostFocus
4.  비동기 GotFocus

## <a name="find-the-first-and-last-focusable-element-a-namefindfirstfocusableelement"></a>첫 번째 및 마지막 포커스 가능 요소 찾기 <a name="findfirstfocusableelement">

**FocusManager.FindFirstFocusableElement** 및 **FocusManager.FindLastFocusableElement** 메서드를 사용하여 개체 범위(UIElement의 요소 트리 또는 TextElement의 텍스트 트리) 내의 첫 번째 또는 마지막 포커스 가능 요소로 포커스를 이동할 수 있습니다. 이러한 메서드를 호출할 때 범위를 지정합니다. 인수가 null이면 범위는 현재 창이 됩니다.

**참고** 범위 내에 포커스 가능 항목이 없으면 이러한 메서드가 null을 반환할 수 있습니다.

<a name="popup-ui-code-sample"></a> 다음 코드 예제는 명령 모음의 단추가 [키보드 조작](keyboard-interactions.md#popup-ui) 문서에 설명된 둘러싸기 방향 동작을 갖도록 지정하는 방법을 보여 줍니다.

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
