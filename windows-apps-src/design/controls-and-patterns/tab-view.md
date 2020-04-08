---
Description: TabView를 사용하면 동적 탭에서 여러 문서를 유연하게 구성할 수 있습니다.
title: 탭 보기
template: detail.hbs
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ce9e3775f4b0f78d17f0ffdf3d6381f2e8a233d9
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081528"
---
# <a name="tabview"></a>TabView

TabView 컨트롤을 사용하여 탭 세트와 각 탭의 콘텐츠를 표시할 수 있습니다. TabView는 여러 페이지의 콘텐츠를 표시하는 데 유용하며 사용자에게 새 탭을 다시 정렬하고, 열고, 닫을 수 있는 기능을 제공합니다.

![TabView 예제](images/tabview/tab-introduction.png)

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | **TabView** 컨트롤은 UWP 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되었습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

> **Windows UI 라이브러리 API**: [TabView 클래스](/uwp/api/microsoft.ui.xaml.controls.tabview), [TabViewItem 클래스](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

일반적으로 여러 탭으로 구성되는 UI는 기능과 모양이 다른 다음 두 가지 고유한 스타일 중 하나로 제공됩니다. **정적 탭**은 설정 창에서 자주 볼 수 있는 형태의 탭입니다. 이 탭은 일반적으로 미리 정의된 콘텐츠가 들어 있는 여러 페이지를 고정된 순서로 포함합니다.
**문서 탭**은 Microsoft Edge 같은 브라우저에서 볼 수 있는 형태의 탭입니다. 사용자는 탭을 만들고, 제거하고, 다시 정렬하고, 창 간에 탭을 이동하고, 탭의 내용을 변경할 수 있습니다.

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)는 UWP 앱에 사용할 수 있는 문서 탭을 제공합니다. 다음과 같은 경우 TabView를 사용합니다.

- 사용자가 탭을 동적으로 열고, 닫고, 다시 정렬할 수 있습니다.
- 사용자가 문서나 웹 페이지를 탭에서 직접 열 수 있습니다.
- 사용자가 창 간에 탭을 끌어서 놓을 수 있습니다.

TabView가 앱에 적합하지 않은 경우 [Pivot](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot) 또는 [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) 같은 컨트롤을 고려해 보세요.

## <a name="anatomy"></a>구조

아래 이미지는 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 컨트롤을 보여줍니다. TabStrip에는 머리글과 바닥글이 있지만, 문서와 달리 TabStrip의 머리글 및 바닥글은 각각 스트립의 왼쪽 끝과 오른쪽 끝에 있습니다.

![TabView 컨트롤의 구조](images/tabview/tab-view-anatomy.png)

다음 이미지는 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) 컨트롤을 보여줍니다. 콘텐츠가 TabView 컨트롤 내에 표시되지만, 콘텐츠는 사실 TabViewItem의 일부입니다.

![TabViewItem 컨트롤의 구조](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>탭 보기 만들기

이 예제에서는 탭 열기 및 닫기를 지원하기 위해 이벤트 처리기와 함께 간단한 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)를 만듭니다.

```xaml
<TabView AddTabButtonClick="Tabs_AddTabButtonClick"
         TabCloseRequested="Tabs_TabCloseRequested" />
```

```csharp
// Add a new Tab to the TabView
private void Tabs_AddTabButtonClick(TabView sender, TabViewAddTabButtonClickEventArgs e)
{
    var newTab = new TabViewItem();
    newTab.IconSource = new SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(BaconIpsumPage));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void Tabs_TabCloseRequested(TabView sender, TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>동작

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)의 기능을 활용하거나 확장하는 여러 가지 방법이 있습니다.

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>TabItemsSource를 TabViewItemCollection에 바인딩

```csharp
<TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>창 제목 표시줄에 TabView 탭 표시

탭이 창 제목 표시줄 아래의 고유한 행을 점유하는 대신, 두 탭을 같은 영역에 통합할 수 있습니다. 이렇게 하면 콘텐츠를 위한 세로 공간이 확보되고, 앱에 현대적인 느낌을 줄 수 있습니다.

사용자가 제목 표시줄을 통해 창을 끌어 창의 위치를 변경할 수 있으므로, 제목 표시줄을 탭으로 꽉 채우면 안 됩니다. 따라서 제목 표시줄에 탭을 표시할 때 제목 표시줄의 일부를 끌기 가능 영역으로 예약해 두어야 합니다. 끌기 가능 영역을 지정하지 않으면 제목 표시줄 전체가 끌기 가능 영역으로 되며, 이렇게 되면 탭이 입력 이벤트를 받을 수 없습니다. TabView가 창의 제목 표시줄에 표시되는 경우 항상 [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter)를 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)에 포함하고 끌기 가능 영역으로 표시해야 합니다.

자세한 내용은 [제목 표시줄 사용자 지정](https://docs.microsoft.com/windows/uwp/design/shell/title-bar)을 참조하세요.

![제목 표시줄의 탭](images/tabview/tab-extend-to-title.png)

```xaml
<Page>
    <TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
        <TabViewItem Icon="Home" Header="Home" IsClosable="False" />
        <TabViewItem Icon="Document" Header="Document 1" />
        <TabViewItem Icon="Document" Header="Document 2" />
        <TabViewItem Icon="Document" Header="Document 3" />

        <TabView.TabStripHeader>
            <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
        </TabView.TabStripHeader>
        <TabView.TabStripFooter>
            <Grid x:Name="CustomDragRegion" Background="Transparent" />
        </TabView.TabStripFooter>
    </TabView>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> 셸 콘텐츠가 제목 표시줄의 탭을 가리지 않도록, 왼쪽 및 오른쪽 오버레이를 고려해야 합니다. LTR 레이아웃에서는 오른쪽 인세트에 캡션 단추와 끌기 영역이 포함됩니다. RTL에서는 그 반대입니다. SystemOverlayLeftInset 및 SystemOverlayRightInset 값은 실제로 왼쪽과 오른쪽이므로 RTL에서는 반대로 해야 합니다.

### <a name="control-overflow-behavior"></a>오버플로 동작 제어

탭 표시줄에 탭이 많아지면 [TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode)를 설정하여 탭 표시 방법을 제어할 수 있습니다.

| TabWidthMode 값 | 동작                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 같음              | 새 탭이 추가되면 모든 탭이 매우 작은 최소 너비에 도달할 때까지 가로 방향으로 축소됩니다.                                                       |
| SizeToContent      | 탭은 항상 아이콘과 머리글을 표시하는 데 필요한 최소 크기인 "기본 크기"로 표시됩니다. 탭은 추가되거나 닫힐 때 확장되거나 축소되지 않습니다. |

어떤 값을 선택해도 탭이 너무 많아 탭 스트립에 모두 표시할 수 없는 경우가 있습니다. 이 경우 사용자가 TabStrip을 좌우로 스크롤할 수 있는 스크롤 범퍼가 나타납니다.

### <a name="guidance-for-tab-selection"></a>탭 선택에 대한 지침

대부분의 사용자는 간단하게 웹 브라우저를 사용하여 문서 탭을 경험합니다. 사용자가 앱에서 문서 탭을 사용하는 방식을 살펴보면 사용자가 어떤 탭 동작을 예상하는지 알 수 있습니다.

사용자가 어떤 방식으로 문서 탭 세트와 상호 작용하든, 항상 활성 탭이 하나 있습니다. 사용자가 선택한 탭을 닫거나 선택한 탭을 다른 창으로 분할하면 다른 탭이 활성 탭으로 변합니다. [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)는 다음 탭을 자동으로 선택하여 이 작업을 처리하려고 시도합니다. 선택하지 않은 탭이 포함된 TabView를 앱에서 허용해야 하는 합리적 이유가 있는 경우 TabView의 콘텐츠 영역이 비어 있게 됩니다.

## <a name="keyboard-navigation"></a>키보드 탐색

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)는 기본적으로 여러 일반적인 키보드 탐색 시나리오를 지원합니다. 이 섹션에서는 기본 제공 기능에 대해 설명하고, 일부 앱에 유용할 수 있는 추가 기능에 대한 권장 사항을 제공합니다.

### <a name="tab-and-cursor-key-behavior"></a>탭 및 커서 키 동작

포커스가 _TabStrip_ 영역으로 이동하면 선택한 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)에 포커스가 맞춰집니다. 그러면 사용자는 왼쪽 및 오른쪽 화살표 키를 사용하여 포커스(선택 영역 아님)를 TabStrip의 다른 탭으로 이동할 수 있습니다. 화살표 포커스는 탭 스트립 및 추가 탭(+) 단추(있는 경우) 안쪽에 트래핑됩니다. 포커스를 TabStrip 영역 밖으로 이동하려면 사용자는 Tab 키를 눌러 포커스를 다음 포커스 가능 요소로 이동할 수 있습니다.

Tab 키로 포커스 이동

![Tab 키로 포커스 이동](images/tabview/tab-keyboard-behavior-1.png)

화살표 키는 포커스를 순환하지 않음

![화살표 키는 포커스를 순환하지 않음](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>탭 선택

TabViewItem에 포커스가 있는 경우 스페이스바 또는 Enter 키를 누르면 해당 TabViewItem이 선택됩니다.

화살표 키를 사용하여 포커스를 이동한 다음, 스페이스바를 눌러 탭을 선택합니다.

![스페이스바로 탭 선택](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>인접한 탭을 선택하는 바로 가기

Ctrl+Tab을 누르면 다음 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)이 선택됩니다. Ctrl+Shift+Tab 키를 누르면 이전 TabViewItem이 선택됩니다. 이러한 목적을 위해 탭 목록은 "순환"되므로, 마지막 탭이 선택된 상태에서 다음 탭을 선택하면 첫 번째 탭이 선택됩니다.

### <a name="closing-a-tab"></a>탭 닫기

Ctrl + F4를 누르면 [TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested) 이벤트가 발생합니다. 이벤트를 처리하고 필요한 경우 탭을 닫습니다.

### <a name="keyboard-guidance-for-app-developers"></a>앱 개발자를 위한 키보드 지침

일부 애플리케이션에는 보다 고급 키보드 제어가 필요할 수 있습니다. 앱에 적합한 경우 다음 바로 가기를 구현해 보세요.

> [!WARNING]
> 기존 앱에 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)를 추가하는 경우 권장 TabView 바로 가기의 키 조합에 매핑되는 바로 가기 키를 이미 만들었을 수도 있습니다. 이 경우에는 기존 바로 가기를 유지할 것인지 아니면 사용자에게 직관적인 탭 환경을 제공할 것인지 고민해야 합니다.

- Ctrl + T 키를 누르면 새 탭이 열립니다. 일반적으로 이 탭은 미리 정의된 문서로 채워지거나, 빈 상태로 만들어지고 콘텐츠를 선택할 수 있는 간단한 방법이 제공됩니다. 사용자가 새 탭의 콘텐츠를 선택해야 하는 경우 콘텐츠 선택 컨트롤에 입력 포커스를 제공하는 방법을 고려해 보세요.
- Ctrl + W 키를 누르면 선택한 탭이 닫힙니다. 다시 말씀드리지만, TabView는 다음 탭을 자동으로 선택합니다.
- Ctrl + Shift + T 키를 누르면 최근에 닫힌 탭이 열립니다(또는 보다 정확하게 설명하자면, 최근에 닫힌 탭과 동일한 콘텐츠가 있는 새 탭이 열림). 가장 최근에 닫은 탭부터 시작하여 이후에 바로 가기가 호출될 때마다 뒤로 이동합니다. 이렇게 하려면 최근에 닫힌 탭 목록을 유지해야 합니다.
- Ctrl + 1 키를 누르면 탭 목록의 첫 번째 탭이 선택됩니다. 마찬가지로 Ctrl + 2 키를 누르면 두 번째 탭이 선택되고, Ctrl + 3 키는 세 번째 탭, 이러한 식으로 Ctrl + 8까지 있습니다.
- Ctrl + 9 키를 누르면 목록에 있는 탭 수에 관계없이 탭 목록의 마지막 탭이 선택됩니다.
- 탭에서 닫기 명령 외에도 탭 복제, 탭 고정 등의 기능을 제공하는 경우 상황에 맞는 메뉴를 사용하여 탭에서 수행할 수 있는 모든 작업을 표시합니다.

### <a name="implement-browser-style-keyboarding-behavior"></a>브라우저 스타일의 키보드 동작 구현

이 예제에서는 위에서 설명한 여러 권장 사항을 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)에서 구현합니다. 특히 이 예제에서는 Ctrl + T, Ctrl + W, Ctrl + 1-8 및 Ctrl + 9 키를 구현합니다.

```xaml
<controls:TabView x:Name="TabRoot">
    <controls:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </controls:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</controls:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // See previous sample
    CreateNewTab();
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only close the selected tab if it is closeable
    if (((TabViewItem)TabRoot.SelectedItem).IsCloseable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>관련된 문서

- [MasterDetails](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/master-details)
- [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview)
- [피벗](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot)
