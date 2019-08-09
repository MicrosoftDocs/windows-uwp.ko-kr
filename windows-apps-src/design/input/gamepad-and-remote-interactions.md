---
Description: Xbox 게임 패드 및 원격 제어에서 입력을 위해 앱을 최적화 합니다.
title: 게임 패드 및 리모컨 조작
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 440f758e5db8bd77d3f26290eb59d7684e5f87a3
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867657"
---
# <a name="gamepad-and-remote-control-interactions"></a>게임 패드 및 리모컨 조작

![키보드 및 게임 패드 이미지](images/keyboard/keyboard-gamepad.jpg)

***많은 상호 작용 환경은 게임 패드, 원격 제어 및 키보드 간에 공유 됩니다.***

UWP (유니버설 Windows 플랫폼) 응용 프로그램에서의 빌드 상호 작용 환경으로, 앱을 사용 하 고 기존 입력 유형 (마우스, 키보드, 터치 등)을 모두 통해 사용 하 고 액세스할 수 있도록 합니다. TV 및 Xbox *10 피트* 환경 (예: 게임 패드 및 원격 제어).

*10 피트* 환경에서 UWP 응용 프로그램에 대 한 일반 디자인 지침은 [Xbox 및 TV를 위한 디자인](../devices/designing-for-tv.md) 을 참조 하세요.

## <a name="overview"></a>개요

이 항목에서는 상호 작용 디자인에서 고려해 야 할 사항 (또는 플랫폼이 그 뒤에 표시 되는 경우)에 대해 설명 하 고,와 상관 없이 사용 하기에는 UWP 응용 프로그램을 빌드하기 위한 지침, 권장 사항 및 제안 사항을 제공 합니다. 장치, 입력 유형, 사용자 기능 및 기본 설정입니다.

응용 프로그램은 *10 피트* 환경에서와 마찬가지로 *2 피트* 환경에서 직관적이 고 쉽게 사용할 수 있어야 합니다 (그 반대의 경우도 마찬가지). 사용자가 선호 하는 장치를 지원 하 고, UI 포커스를 명확 하 고 unmistakable 하 고, 탐색을 일관적이 고 예측 가능 하도록 콘텐츠를 정렬 하 고, 사용자에 게 원하는 작업에 가장 짧은 경로를 제공 합니다.

> [!NOTE]
> 이 항목의 코드 조각은 대부분 XAML/C#으로 작성되었지만 원칙과 개념은 모든 UWP 앱에 적용됩니다. Xbox용 HTML/JavaScript UWP 앱을 개발하는 경우 GitHub 라이브러리에서 뛰어난 [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) 라이브러리를 확인하세요.


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>2 피트 및 10 피트 환경 모두에 맞게 최적화

응용 프로그램을 테스트 하 여 2 피트와 10mbps 시나리오 모두에서 잘 작동 하는지, 그리고 모든 기능이 검색 가능 하 고 Xbox [게임 및 원격 제어](#gamepad-and-remote-control)에서 액세스할 수 있도록 하는 것이 좋습니다.

다음은 2 피트 및 10-및 모든 입력 장치에서 사용할 수 있도록 앱을 최적화할 수 있는 몇 가지 다른 방법입니다 (각 항목의 해당 섹션에 대 한 링크).

> [!NOTE]
> Xbox gamepads 및 원격 컨트롤이 많은 UWP 키보드 동작 및 환경을 지원 하기 때문에 이러한 권장 사항은 두 입력 유형에 모두 적합 합니다. 자세한 키보드 정보는 [키보드 상호 작용](keyboard-interactions.md) 을 참조 하세요.

| 기능        | 설명           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 포커스 탐색 및 상호 작용](#xy-focus-navigation-and-interaction) | **XY 포커스 탐색** 을 사용 하면 사용자가 앱의 UI를 탐색할 수 있습니다. 그러나 사용자가 위, 아래, 왼쪽 및 오른쪽으로만 탐색할 수 있도록 제한됩니다. 이 섹션에는 이 문제를 해결하기 위한 권장 사항과 기타 고려 사항이 요약되어 있습니다. |
| [마우스 모드](#mouse-mode)|지도, 그리기 및 그리기와 같은 일부 유형의 응용 프로그램에서는 XY 포커스 탐색이 실용적이 지 않거나 가능 하지 않습니다. 이러한 경우 **마우스 모드** 를 사용 하면 사용자가 PC의 마우스와 마찬가지로 게임 패드 또는 원격 제어로 자유롭게 이동할 수 있습니다.|
| [포커스 시각적 개체](#focus-visual)  | 포커스 시각적 개체는 현재 포커스가 있는 UI 요소를 강조 표시 하는 테두리입니다. 이렇게 하면 사용자가 탐색 하거나 상호 작용 하는 UI를 빠르게 식별할 수 있습니다.  |
| [포커스 참여](#focus-engagement) | 포커스 engagement를 사용 하려면 UI 요소에 포커스가 있을 때 사용자가 게임 패드 또는 원격 제어에서 **A/Select** 단추를 눌러야 합니다. |
| [하드웨어 단추](#hardware-buttons) | 게임 패드 및 원격 제어는 매우 다양 한 단추와 구성을 제공 합니다. |

## <a name="gamepad-and-remote-control"></a>게임 패드 및 리모컨

PC의 키보드 및 마우스, 휴대폰과 태블릿의 터치와 마찬가지로 게임 패드 및 리모컨은 3m 환경의 주요 입력 장치입니다.
이 섹션에서는 하드웨어 버튼이란 무엇이며 어떤 기능을 수행하는지를 소개합니다.
[XY 포커스 탐색 및 조작](#xy-focus-navigation-and-interaction)과 [마우스 모드](#mouse-mode)에서 이러한 입력 장치를 사용할 때 앱을 최적화하는 방법을 살펴보겠습니다.

기본적으로 사용할 수 있는 게임 패드 및 리모컨 동작의 품질은 앱에서 키보드가 얼마나 잘 지원되는지에 따라 달라집니다. 앱이 게임 패드/리모컨으로 잘 작동하게 하는 한 가지 효율적인 방법은 PC에서 키보드로 잘 작동하는지 확인하고 게임 패드/리모컨으로 테스트하여 UI에서 취약한 부분을 찾는 것입니다.

### <a name="hardware-buttons"></a>하드웨어 버튼

이 설명서 전체에서 버튼은 다음 다이어그램에 제공된 이름으로 나타냅니다.

![게임 패드 및 리모컨 버튼 다이어그램](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

다이어그램에서 알 수 있듯이, 게임 패드에서 지원되지만 리모컨에서는 지원되지 않는 버튼도 있고 그 반대의 경우도 있습니다. UI를 더 빠르게 탐색할 수 있도록 하나의 입력 장치에서만 지원되는 버튼을 사용할 수 있지만 중요한 조작에 이러한 버튼을 사용할 경우 사용자가 UI의 특정 부분을 조작할 수 없는 상황이 생길 수 있습니다.

다음 표에서는 UWP 앱이 지원하는 모든 하드웨어 버튼 및 해당 버튼을 지원하는 입력 장치를 보여 줍니다.

| 단추                    | 게임 패드   | 원격 제어    |
|---------------------------|-----------|-------------------|
| A/선택 버튼           | 예       | 예               |
| B/뒤로 버튼             | 예       | 예               |
| 방향 패드(D 패드)   | 예       | 예               |
| 메뉴 버튼               | 예       | 예               |
| 보기 버튼               | 예       | 예               |
| X 및 Y 버튼           | 예       | 아니요                |
| 왼쪽 스틱                | 예       | 아니요                |
| 오른쪽 스틱               | 예       | 아니요                |
| 왼쪽 및 오른쪽 트리거   | 예       | 아니요                |
| 왼쪽 및 오른쪽 범퍼    | 예       | 아니요                |
| OneGuide 버튼           | 아니요        | 예               |
| 볼륨 버튼             | 아니요        | 예               |
| 채널 버튼            | 아니요        | 예               |
| 미디어 컨트롤 버튼     | 아니요        | 예               |
| 음소거 버튼               | 아니요        | 예               |

### <a name="built-in-button-support"></a>기본 제공 버튼 지원

UWP는 기존의 키보드 입력 동작을 게임 패드 및 리모컨 입력에 자동으로 매핑합니다. 다음 표에서는 이러한 기본 제공 매핑을 보여 줍니다.

| 키보드              | 게임 패드/리모컨                        |
|-----------------------|---------------------------------------|
| 화살표 키            | D 패드(또한 게임 패드의 왼쪽 스틱)    |
| 스페이스바              | A/선택 버튼                       |
| Enter 키                 | A/선택 버튼                       |
| 이스케이프                | B/뒤로 버튼*                        |

\*응용 프로그램에서 B 단추에 대 한 [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) 또는 [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트를 처리 하지 않으면 [systemnavigationmanager. 요청](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 된 이벤트가 발생 하 여 앱 내에서 다시 탐색이 발생 합니다. 그러나 다음 코드 조각과 같이 이를 직접 구현해야 합니다.

```csharp
// This code goes in the MainPage class

public MainPage()
{
    this.InitializeComponent();

    // Handling Page Back navigation behaviors
    SystemNavigationManager.GetForCurrentView().BackRequested +=
        SystemNavigationManager_BackRequested;
}

private void SystemNavigationManager_BackRequested(
    object sender,
    BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = this.BackRequested();
    }
}

public Frame AppFrame { get { return this.Frame; } }

private bool BackRequested()
{
    // Get a hold of the current frame so that we can inspect the app back stack
    if (this.AppFrame == null)
        return false;

    // Check to see if this is the top-most page on the app back stack
    if (this.AppFrame.CanGoBack)
    {
        // If not, set the event to handled and go back to the previous page in the
        // app.
        this.AppFrame.GoBack();
        return true;
    }
    return false;
}
```

> [!NOTE]
> B 버튼을 뒤로 가기에 사용하는 경우에는 UI에 뒤로 버튼을 표시하지 마세요. [탐색 보기](../controls-and-patterns/navigationview.md)를 사용하는 경우에는 뒤로 버튼이 자동으로 숨겨집니다. 뒤로 탐색에 대한 자세한 내용은 [UWP 앱의 탐색 기록 및 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)을 참조하세요.

Xbox One의 UWP 앱은 **메뉴** 버튼을 눌러 상황에 맞는 메뉴를 여는 기능을 지원합니다. 자세한 내용은 [CommandBar 및 ContextFlyout](#commandbar-and-contextflyout)을 참조하세요.

### <a name="accelerator-support"></a>바로 연결 지원

바로 연결 버튼은 UI 탐색 속도를 높이는 데 사용할 수 있는 버튼입니다. 그러나 이러한 버튼은 특정 입력 장치에 고유할 수 있으므로 모든 사용자가 이러한 기능을 사용할 수 있는 것은 아닙니다. 실제로, 현재 Xbox One에서 UWP 앱에 대해 바로 연결 기능을 지원하는 입력 장치는 게임 패드뿐입니다.

다음 표에서는 UWP에 기본 제공되는 바로 연결 지원 및 직접 구현할 수 있는 바로 연결 지원을 보여 줍니다. 사용자 지정 UI에서 이러한 동작을 활용하여 일관되고 친숙한 사용자 환경을 제공합니다.

| 조작   | 키보드/마우스   | 게임 패드      | 기본 제공:  | 맞춤 항목: |
|---------------|------------|--------------|----------------|------------------|
| 페이지 위로/아래로  | 페이지 위로/아래로 | 왼쪽/오른쪽 트리거 | [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 세로 스크롤을 지원하는 보기
| 페이지 왼쪽/오른쪽으로 | 없음 | 왼쪽/오른쪽 범퍼 | [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 가로 스크롤을 지원하는 보기
| 확대/축소        | Ctrl +/- | 왼쪽/오른쪽 트리거 | 없음 | `ScrollViewer`확대 및 축소를 지 원하는 뷰 |
| 탐색 창 열기/닫기 | 없음 | 보기 | 없음 | 탐색 창 |
| 검색 | 없음 | Y 버튼 | 없음 | 앱에서 기본 검색 기능에 대한 바로 가기 |
| [상황에 맞는 메뉴 열기](#commandbar-and-contextflyout) | 마우스 오른쪽 단추 클릭 | 메뉴 버튼 | [ContextFlyout 아웃](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | 상황에 맞는 메뉴 |

## <a name="xy-focus-navigation-and-interaction"></a>XY 포커스 탐색 및 조작

앱이 키보드에 대해 적절한 포커스 탐색을 지원하는 경우 게임 패드 및 리모컨에도 잘 전송됩니다.
화살표 키를 사용한 탐색은 **D-패드**(및 게임 패드의 **왼쪽 스틱**)에 매핑되고, UI 요소 조작은 **Enter/선택** 키에 매핑됩니다([게임 패드 및 리모컨 참조](#gamepad-and-remote-control)).

많은 이벤트와 속성은 키보드와 게임 패드 둘 다에서 사용됩니다.&mdash;둘 다 `KeyDown` 및 `KeyUp` 이벤트를 발생시키며 `IsTabStop="True"` 및 `Visibility="Visible"` 속성이 있는 컨트롤만 탐색합니다. 키보드 디자인 지침은 [키보드 조작](../input/keyboard-interactions.md)을 참조하세요.

키보드 지원을 제대로 구현하면 앱이 제대로 작동합니다. 그러나 모든 시나리오를 지원하려면 몇 가지 추가 작업이 필요할 수 있습니다. 가능한 최상의 사용자 환경을 제공하기 위해 앱의 특정 요구를 고려해 보세요.

> [!IMPORTANT]
> 마우스 모드는 Xbox One에서 실행되는 UWP 앱에 대해 기본적으로 사용됩니다. 마우스 모드를 사용하지 않고 XY 포커스 탐색을 사용하도록 설정하려면 `Application.RequiresPointerMode=WhenRequested`를 설정합니다.

### <a name="debugging-focus-issues"></a>포커스 문제 디버그

[FocusManager.GetFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) 메서드는 현재 포커스가 있는 요소를 알려줍니다. 포커스 화면 효과의 위치가 명확하지 않을 수 있는 경우에 유용합니다. Visual Studio 출력 창에 이 정보를 다음과 같이 기록할 수 있습니다.

```csharp
page.GotFocus += (object sender, RoutedEventArgs e) =>
{
    FrameworkElement focus = FocusManager.GetFocusedElement() as FrameworkElement;
    if (focus != null)
    {
        Debug.WriteLine("got focus: " + focus.Name + " (" +
            focus.GetType().ToString() + ")");
    }
};
```

XY 탐색이 예상대로 작동하지 않을 수 있는 세 가지 일반적인 이유는 다음과 같습니다.

* [IsTabStop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) 또는 [가시성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) 속성이 잘못 설정되었습니다.
* 포커스를 받는 컨트롤이 실제로 예상보다 더 큽니다.&mdash;XY 탐색은 흥미로운 사항을 렌더링하는 컨트롤의 일부만이 아니라 컨트롤의 총 크기([ActualWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 및 [ActualHeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight))를 확인합니다.
* 포커스 가능한 컨트롤이 다른 컨트롤 위에 있습니다.&mdash;XY 탐색은 겹치는 컨트롤을 지원하지 않습니다.

이러한 문제를 해결한 후에도 XY 탐색이 예상대로 작동하지 않는 경우 [기본 탐색 재정의](#overriding-the-default-navigation)에 설명된 메서드를 사용하여 포커스를 설정하려는 요소를 수동으로 가리킬 수 있습니다.

XY 탐색이 의도한 대로 작동하지만 포커스 화면 효과가 표시되지 않는 경우 다음 문제 중 하나가 원인일 수 있습니다.

* 컨트롤을 다시 템플릿으로 지정했으며 포커스 화면 효과를 포함하지 않았습니다. `UseSystemFocusVisuals="True"`를 설정하거나 포커스 화면 효과를 수동으로 추가합니다.
* `Focus(FocusState.Pointer)`를 호출하여 포커스를 이동했습니다. [FocusState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FocusState) 매개 변수는 포커스 화면 효과에 수행되는 작업을 제어합니다. 일반적으로 `FocusState.Programmatic`으로 설정해야 하며, 이 경우 포커스 화면 효과가 이전에 표시되었다면 계속 표시되고 이전에 숨겨졌다면 계속 숨겨집니다.

이 섹션의 나머지 부분에서는 XY 탐색을 사용하는 경우의 일반적인 디자인 문제에 대해 자세히 설명하고 여러 가지 해결 방법을 제공합니다.

### <a name="inaccessible-ui"></a>액세스할 수 없는 UI

XY 포커스 탐색은 사용자가 위쪽, 아래쪽, 왼쪽 및 오른쪽으로 이동하도록 제한하기 때문에 UI 부분에 액세스할 수 없는 시나리오가 발생할 수 있습니다.
다음 다이어그램은 XY 포커스 탐색이 지원하지 않는 UI 레이아웃 종류의 예를 보여 줍니다.
세로 및 가로 탐색에 우선 순위가 지정되고 가운데 요소는 포커스를 받을 만큼 우선 순위가 높게 지정되지 않기 때문에 가운데 요소는 게임 패드/리모컨으로 액세스할 수 없습니다.

![액세스할 수 없는 요소가 가운데에 포함된 네 모서리의 요소](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

어떤 이유로든 UI를 다시 정렬할 수 없는 경우 다음 섹션에서 설명하는 방법 중 하나를 사용하여 기본 포커스 동작을 재정의합니다.

### <a name="overriding-the-default-navigation"></a>기본 탐색 재정의

유니버설 Windows 플랫폼은 D 패드/왼쪽 스틱 탐색이 사용자에게 타당한지 확인하지만, 앱의 의도에 최적화된 동작을 보장할 수는 없습니다.
탐색이 앱에 최적화되었는지 확인하는 가장 좋은 방법은 게임 패드로 테스트하여 사용자가 앱의 시나리오에 타당한 방식으로 모든 UI 요소에 액세스할 수 있는지 확인하는 것입니다. 앱의 시나리오에서 제공된 XY 포커스 탐색으로 얻을 수 없는 동작을 요구하는 경우 다음 섹션에 있는 권장 사항을 따르고 동작을 재정의하여 논리적 항목에 포커스를 배치하는 것이 좋습니다.

다음 코드 조각은 XY 포커스 탐색 동작을 재정의하는 방법을 보여 줍니다.

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft"
            Content="Search" />
    <Button x:Name="MyBtnRight"
            Content="Delete"/>
    <Button x:Name="MyBtnTop"
            Content="Update" />
    <Button x:Name="MyBtnDown"
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}"
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

이 경우 `Home` 버튼에 포커스가 있고 사용자가 왼쪽으로 이동하면 포커스가 `MyBtnLeft` 버튼으로 이동합니다. 사용자가 오른쪽에는 이동하면 포커스가 `MyBtnRight` 버튼으로 이동합니다.

포커스가 컨트롤에서 특정 방향으로 이동하지 않도록 하려면 `XYFocus*` 속성을 사용하여 동일한 컨트롤을 가리킵니다.

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

포커스가 있는 자식이 동일한 `XYFocus` 속성을 사용하지 않는 한 다음 포커스 후보가 해당 시각적 트리를 벗어난 경우 컨트롤 부모는 이러한 `XYFocus` 속성을 사용하여 해당 자식의 탐색을 강제할 수도 있습니다.

```xml
<StackPanel Orientation="Horizontal" Margin="300,300">
    <UserControl XYFocusRight="{x:Bind ButtonThree}">
        <StackPanel>
            <Button Content="One"/>
            <Button Content="Two"/>
        </StackPanel>
    </UserControl>
    <StackPanel>
        <Button x:Name="ButtonThree" Content="Three"/>
        <Button Content="Four"/>
    </StackPanel>
</StackPanel>
```

위 샘플에서 포커스가 `Button` Two에 있으며 사용자가 오른쪽으로 탐색하는 경우 가장 적합한 포커스 후보는 `Button` Four입니다. 그러나 부모 `UserControl`이 해당 시각적 트리를 벗어난 경우 그곳을 탐색하도록 강제하기 때문에 포커스가 `Button` Three로 이동됩니다.

### <a name="path-of-least-clicks"></a>최소 클릭 경로

사용자가 가장 일반적인 작업을 최소 클릭 수로 수행할 수 있게 합니다. 다음 예제에서는 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)이 **재생** 버튼(처음 포커스를 받음)과 자주 사용하는 요소 사이에 있으므로 우선 순위 작업 사이에 불필요한 요소가 배치됩니다.

![탐색 모범 사례에서는 최소 클릭 경로를 제공합니다.](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

다음 예제에서는 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)이 **재생** 단추 위에 있습니다.
우선 순위 작업 사이에 불필요한 요소가 배치되지 않도록 UI를 다시 정렬하기만 해도 앱의 유용성이 크게 향상됩니다.

![우선 순위 작업 사이에 배치되지 않도록 TextBlock이 재생 버튼 위로 이동됨](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar 및 ContextFlyout

[명령 모음](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)을 사용 하는 경우 문제에 [설명 된 대로 목록을 스크롤 하는 문제를 염두에 두어야 합니다. 긴 스크롤 목록/그리드](#problem-ui-elements-located-after-long-scrolling-list-grid)후에 위치한 UI 요소입니다. 다음 그림은 `CommandBar`가 목록/그리드 맨 아래에 있는 UI 레이아웃을 보여 줍니다. 사용자는 `CommandBar`에 접근하기 위해 목록/그리드 맨 아래까지 스크롤해야 합니다.

![CommandBar가 목록/그리드 맨 아래에 있음](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

목록/그리드 *위에* 을 `CommandBar` 배치 하면 어떻게 되나요? 목록/그리드를 스크롤한 사용자가 `CommandBar`에 접근하기 위해 다시 위로 스크롤해야 하지만 이전 구성보다는 탐색이 약간 줄어듭니다. 여기서는 앱의 초기 포커스가 `CommandBar` 옆이나 위에 있다고 가정합니다. 초기 포커스가 목록/그리드 아래에 있으면 이 방식은 제대로 작동하지 않습니다. 이러한 `CommandBar` 항목이 자주 액세스할 필요가 없는 전역 작업 항목인 경우(예: **동기화** 단추) 목록/그리드 위에 배치해도 됩니다.

`CommandBar`의 항목을 세로로 겹칠 수는 없지만 스크롤 방향과 반대로(예: 세로로 스크롤되는 목록의 왼쪽 또는 오른쪽이나 가로로 스크롤되는 목록의 위쪽 또는 아래쪽) 배치하는 것도 UI 레이아웃에 적합한 경우 고려할 수 있는 또 다른 옵션입니다.

사용자가 항목에 쉽게 액세스할 수 있어야 하는 `CommandBar`이 앱에 있는 경우 이러한 항목을 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 내부에 배치하고 `CommandBar`에서 제거하는 것이 좋습니다. `ContextFlyout`는 [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 의 속성 이며 해당 요소와 연결 된 [상황에 맞는 메뉴](../controls-and-patterns/dialogs-and-flyouts/index.md) 입니다. PC에서 `ContextFlyout`이 있는 요소를 마우스 오른쪽 단추로 클릭하면 해당 상황에 맞는 메뉴가 팝업됩니다. Xbox One에서는 이러한 요소에 포커스가 있는 동안 **메뉴** 단추를 누를 때 이 작업이 수행됩니다.

### <a name="ui-layout-challenges"></a>UI 레이아웃 문제

일부 UI 레이아웃은 XY 포커스 탐색의 특성으로 인해 더 까다로우며, 사례별로 평가해야 합니다. 하나의 "올바른" 방법은 없으며 선택하는 해결 방법은 앱의 특정 요구에 따라 다르지만 멋진 TV 환경을 만들기 위해 활용할 수 있는 몇 가지 방법이 있습니다.

이 내용을 이해하기 쉽도록 이러한 몇 가지 문제와 해결 방법을 보여 주는 가상 앱을 살펴보겠습니다.

> [!NOTE]
> 이 가상 앱은 UI 문제와 잠재적인 해결 방법을 설명하는 데 사용되며, 특정 앱에 대한 최상의 사용자 환경을 보여 주기 위한 것이 아닙니다.

다음은 판매용 주택 목록, 지도, 속성 설명 및 기타 정보를 보여 주는 가상 부동산 앱입니다. 이 앱은 다음과 같은 방법으로 해결할 수 있는 세 가지 문제를 제기합니다.

- [UI 다시 정렬](#ui-rearrange)
- [포커스 참여](#engagement)
- [마우스 모드](#mouse-mode)

![가상 부동산 앱](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### 문제: 긴 스크롤 목록/그리드 후에 있는 UI 요소<a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

다음 그림에 표시된 속성의 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)는 매우 긴 스크롤 목록입니다. `ListView`에서 [연결](#focus-engagement)이 요구되지 *않는* 경우 사용자가 목록으로 이동하면 목록의 첫 번째 항목에 포커스가 배치됩니다. 사용자가 **이전** 또는 **다음** 단추에 접근하려면 목록에 있는 모든 항목을 탐색해야 합니다. 이처럼 사용자가 전체 목록을 트래버스하는 것이 힘든 경우&mdash;즉, 이러한 경험이 허용될 만큼&mdash;목록이 짧지 않은 경우에는 다른 옵션을 고려하는 것이 좋습니다.

![부동산 앱: 50개 항목이 포함된 목록 아래에 있는 단추에 접근하기 위해 51번 클릭해야 함](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>해결 방법

**UI 다시 정렬<a name="ui-rearrange"></a>**

초기 포커스가 페이지 맨 아래에 배치되지 않는 경우 일반적으로 긴 스크롤 목록 위에 있는 UI 요소가 아래에 배치된 UI 요소보다 액세스하기 더 쉽습니다.
이 새로운 레이아웃이 다른 디바이스에도 적합한 경우 Xbox One용으로 특수 UI 변경을 수행하는 대신 모든 디바이스 패밀리에 대한 레이아웃을 변경하는 것이 비용을 줄이는 방법일 수 있습니다.
또한 스크롤 방향과 반대로 UI 요소를 배치하면(즉, 세로로 스크롤되는 목록에 가로로 배치 또는 가로로 스크롤되는 목록에 세로로 배치) 접근성이 더 향상됩니다.

![부동산 앱: 긴 스크롤 목록 위에 단추 배치](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**포커스 참여<a name="engagement"></a>**

연결이 *필수*이면 전체 `ListView`가 단일 포커스 대상이 됩니다. 사용자가 다음 포커스 가능 요소에 액세스하기 위해 목록 내용을 무시할 수 있습니다. 연결을 지원하는 컨트롤 및 사용 방법에 대한 자세한 내용은 [포커스 연결](#focus-engagement)을 참조하세요.

![부동산 앱: 한 번만 클릭하면 이전/다음 단추에 접근할 수 있도록 연결을 필수로 설정](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>문제: 포커스를 받을 요소가 없는 ScrollViewer

XY 포커스 탐색은 한 번에 하나씩 포커스 가능 UI요소로 이동하므로 포커스 가능 요소가 없는 [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)(예: 이 예제와 같이 텍스트만 포함된 경우)에서는 사용자가 `ScrollViewer`의 일부 콘텐츠를 볼 수 없는 시나리오가 발생할 수 있습니다.
이 시나리오 및 기타 관련 시나리오에 대한 해결 방법은 [포커스 연결](#focus-engagement)을 참조하세요.

![부동산 앱: 텍스트만 있는 ScrollViewer](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>문제: 자유 스크롤 UI

앱에 그리기 화면 또는 이 예제의 지도와 같은 자유 스크롤 UI가 필요한 경우 XY 포커스 탐색이 작동하지 않습니다.
이런 경우에는 [마우스 모드](#mouse-mode)를 켜서 사용자가 UI 요소 내에서 자유롭게 탐색하도록 할 수 있습니다.

![마우스 모드를 사용하여 UI 요소 매핑](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>마우스 모드

[XY 포커스 탐색 및 조작](#xy-focus-navigation-and-interaction)에 설명된 대로 Xbox One에서는 XY 탐색 시스템을 통해 포커스가 이동되어 사용자가 위쪽, 아래쪽, 왼쪽 및 오른쪽으로 이동하여 컨트롤 간에 포커스를 전환할 수 있게 합니다.
그러나 [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) 및 [MapControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)과 같은 일부 컨트롤에서는 사용자가 컨트롤의 경계 안으로 포인터를 자유롭게 이동할 수 있는 마우스와 유사한 조작이 필요합니다.
사용자가 전체 페이지에서 포인터를 이동하여 PC에서 마우스로 찾을 수 있는 것과 유사한 경험을 게임 패드/리모컨으로 얻을 수 있어야 하는 앱도 있습니다.

이러한 시나리오의 경우 전체 페이지 또는 페이지 내부 컨트롤에 대한 포인터(마우스 모드)를 요청해야 합니다.
예를 들어 컨트롤 내부에 있는 동안에만 마우스 모드를 사용하고 다른 곳에서는 XY 포커스 탐색을 사용하는 `WebView` 컨트롤이 포함된 페이지가 앱에 있을 수 있습니다.
포인터를 요청하려면 필요한 경우가 **컨트롤 또는 페이지를 연결할 때**인지, 아니면 **페이지에 포커스가 있을 때**인지를 지정할 수 있습니다.

> [!NOTE]
> 컨트롤이 포커스를 받을 때 포인터를 요청하는 기능은 지원되지 않습니다.

Xbox One에서 실행되는 XAML 및 호스트된 웹앱 둘 다에서 마우스 모드는 전체 앱에 대해 기본적으로 켜져 있습니다. 이 기능을 끄고 앱을 XY 탐색에 최적화하는 것이 좋습니다. 이렇게 하려면 `Application.RequiresPointerMode` 속성을 `WhenRequested`로 설정하여 컨트롤 또는 페이지에서 요청하는 경우에만 마우스 모드를 사용하도록 설정합니다.

XAML 앱에서 이 작업을 수행하려면 `App` 클래스에 다음 코드를 사용합니다.

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

HTML/JavaScript용 샘플 코드를 포함하여 자세한 내용은 [마우스 모드를 사용하지 않도록 설정하는 방법](../../xbox-apps/how-to-disable-mouse-mode.md)을 참조하세요.

다음 다이어그램은 마우스 모드의 게임 패드/리모컨에 대한 단추 매핑을 보여 줍니다.

![마우스 모드의 게임 패드/리모컨에 대한 단추 매핑](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> 마우스 모드는 Xbox One에서 게임 패드/리모컨으로만 지원됩니다. 다른 디바이스 패밀리 및 입력 형식에서는 자동으로 무시됩니다.

컨트롤 또는 페이지의 [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer) 속성을 사용하여 마우스 모드를 활성화합니다. 이 속성에 사용 가능한 세 가지 값은 `Never`(기본값), `WhenEngaged` 및 `WhenFocused`입니다.

### <a name="activating-mouse-mode-on-a-control"></a>컨트롤에서 마우스 모드 활성화

사용자가 컨트롤을 `RequiresPointer="WhenEngaged"`에 연결하면 연결 해제할 때까지 컨트롤에서 마우스 모드가 활성화됩니다. 다음 코드 조각은 연결 시 마우스 모드를 활성화하는 간단한 `MapControl`을 보여 줍니다.

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> 연결 시 컨트롤이 마우스 모드를 활성화하는 경우 `IsEngagementRequired="true"`로도 연결을 요구해야 합니다. 요구하지 않으면 마우스 모드가 활성화되지 않습니다.

컨트롤이 마우스 모드이면 중첩된 컨트롤도 마우스 모드가 됩니다. 요청된 자식 모드는 무시됩니다. 부모가 마우스 모드인데&mdash; 자식이 마우스 모드가 아닌 경우는 불가능합니다.

또한 요청된 컨트롤 모드는 포커스를 받을 때만 적용되므로 포커스가 있는 동안에는 모드가 동적으로 변경되지 않습니다.

### <a name="activating-mouse-mode-on-a-page"></a>페이지에서 마우스 모드 활성화

페이지에 `RequiresPointer="WhenFocused"` 속성이 있는 경우 포커스를 받을 때 전체 페이지에 대해 마우스 모드가 활성화됩니다. 다음 코드 조각은 페이지에 이 속성을 제공하는 방법을 보여 줍니다.

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> `WhenFocused` 값은 [페이지](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 개체에서만 지원됩니다. 컨트롤에 이 값을 설정하려고 하면 예외가 발생합니다.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>전체 화면 콘텐츠에 마우스 모드를 사용하지 않도록 설정

동영상 또는 다른 유형의 콘텐츠를 전체 화면으로 표시하는 경우 일반적으로 커서가 사용자의 주의를 방해할 수 있으므로 커서를 숨기려고 합니다. 이 시나리오는 앱의 나머지에서는 마우스 모드를 사용하지만, 전체 화면 콘텐츠를 표시할 때는 마우스 모드를 끄려는 경우에 발생합니다. 이렇게 하려면 전체 화면 콘텐츠를 고유한 `Page`에 배치하고 다음 단계를 따릅니다.

1. `App` 개체에서 `RequiresPointerMode="WhenRequested"`를 설정합니다.
2. 전체 화면 `Page`를 *제외한* 모든 `Page` 개체에서 `RequiresPointer="WhenFocused"`를 설정합니다.
3. 전체 화면 `Page`에 대해 `RequiresPointer="Never"`를 설정합니다.

이렇게 하면 전체 화면 콘텐츠를 표시하는 경우에 커서가 표시되지 않습니다.

## <a name="focus-visual"></a>포커스 화면 효과

포커스 화면 효과는 현재 포커스가 있는 UI 요소를 둘러싸는 테두리입니다. 이 기능은 사용자가 헤매지 않고 UI를 쉽게 탐색하는 데 도움이 됩니다.

포커스 화면 효과에 추가된 다양한 사용자 지정 옵션 및 시각적 업데이트를 통해 개발자는 단일 포커스 화면 효과가 원격 PC와 Xbox One은 물론 키보드 및/또는 게임 패드/리모컨을 지원하는 다른 Windows&nbsp;10 디바이스에서도 잘 작동할 것이라고 신뢰할 수 있습니다.

여러 플랫폼에서 동일한 포커스 화면 효과를 사용할 수 있지만 3m 환경의 경우 사용자에게 발생하는 컨텍스트가 약간 다릅니다. 사용자가 전체 TV 화면에 완전히 집중하지 않는다고 가정해야 하므로 시각 효과를 쉽게 검색할 수 있도록 현재 포커스가 있는 요소가 항상 사용자에게 명확하게 표시되어야 합니다.

또한 게임 패드 또는 리모컨을 사용할 때는 포커스 화면 효과가 기본적으로 표시되지만 키보드를 사용할 때는 표시되지 *않는다는* 것에 유의해야 합니다. 따라서 구현하지 않더라도 Xbox One에서 앱을 실행할 때는 표시됩니다.

### <a name="initial-focus-visual-placement"></a>초기 포커스 화면 효과 배치

앱을 시작하거나 페이지로 이동할 때 사용자가 작업을 수행하는 첫 번째 요소로 타당한 UI 요소에 포커스를 배치합니다. 예를 들어 사진 앱은 갤러리의 첫 번째 항목에 포커스를 배치할 수 있고, 노래 자세히 보기로 이동된 음악 앱은 음악을 재생하기 쉽도록 재생 단추에 포커스를 배치할 수 있습니다.

앱의 왼쪽 위 영역(또는 오른쪽에서 왼쪽 흐름의 경우 오른쪽 위)에 초기 포커스를 배치합니다. 일반적으로 여기서 앱 콘텐츠 흐름이 시작되기 때문에 대부분의 사용자는 먼저 해당 모서리에 집중하는 경향이 있습니다.

### <a name="making-focus-clearly-visible"></a>포커스가 명확하게 표시되도록 만들기

사용자가 포커스를 검색하지 않고도 중단한 위치를 선택할 수 있도록 항상 포커스 화면 효과가 화면에 표시되어야 합니다. 마찬가지로, 항상 화면에 포커스 가능 항목이 있어야 합니다. &mdash;예를 들어 텍스트만 있고 포커스 가능 요소가 없는 팝업은 사용하지 마세요.

이 규칙의 예외는 동영상 시청, 이미지 보기 등의 전체 화면 환경으로, 포커스 화면 효과를 표시하는 것이 부적절한 경우입니다.

### <a name="reveal-focus"></a>포커스 표시

포커스 표시는 사용자가 게임 패드 또는 키보드 포커스를 이동하면 버튼 같이 포커스를 맞출 수 있는 요소의 테두리를 애니메이션화하는 조명 효과입니다. 포커스 표시에서 포커스를 맞춘 요소의 경계 주위로 빛 애니메이션을 적용하면 포커스가 어디에 있는지, 그리고 포커스가 어디로 이동하는지 쉽게 알아볼 수 있습니다.

기본적으로 포커스 표시는 꺼져 있습니다. 3m 환경에서는 앱 생성자에서 [Application.FocusVisualKind 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind)을 설정하여 포커스를 표시하도록 선택해야 합니다.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

자세한 내용은 [포커스 표시](/windows/uwp/design/style/reveal-focus) 지침을 참조하세요.

### <a name="customizing-the-focus-visual"></a>포커스 화면 효과 사용자 지정

포커스 화면 효과를 사용자 지정하려는 경우 각 컨트롤의 포커스 화면 효과와 관련된 속성을 수정하면 됩니다. 앱을 개인 설정하기 위해 활용할 수 있는 여러 가지 속성이 있습니다.

시각적 상태를 사용하여 고유한 화면 효과를 그려 시스템 제공 포커스 화면 효과를 옵트아웃할 수도 있습니다. 자세한 내용은 [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)를 참조하세요.

### <a name="light-dismiss-overlay"></a>오버레이 빠른 해제

사용자가 현재 게임 컨트롤러 또는 리모컨으로 조작하는 UI 요소에 집중하도록 UWP는 Xbox One에서 앱을 실행할 경우 팝업 UI 외부 영역을 덮는 "연기" 계층을 자동으로 추가합니다. 추가 작업은 필요 없지만 UI를 디자인할 때 이 점에 유의해야 합니다. `FlyoutBase`의 `LightDismissOverlayMode` 속성을 설정하여 연기 계층을 사용하거나 사용하지 않도록 설정할 수 있습니다. 기본적으로 `Auto`로 설정되어 있으며, 이 경우 Xbox에서는 사용되고 다른 곳에서는 사용되지 않습니다. 자세한 내용은 [모달 vs 빠른 해제](../controls-and-patterns/menus.md)를 참조하세요.

## <a name="focus-engagement"></a>포커스 연결

포커스 연결은 게임 패드 또는 리모컨을 사용하여 앱을 조작하기 쉽게 하려는 것입니다.

> [!NOTE]
> 포커스 연결을 설정해도 키보드 또는 기타 입력 장치에는 영향을 주지 않습니다.

[FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 개체의 `IsFocusEngagementEnabled` 속성이 `True`로 설정된 경우 컨트롤이 포커스 연결 필요로 표시됩니다. 즉, 사용자가 **A/선택** 단추를 눌러 컨트롤을 "연결"하고 조작해야 합니다. 작업이 끝나면 **B/뒤로** 버튼을 눌러 컨트롤 연결을 해제하고 벗어날 수 있습니다.

> [!NOTE]
> `IsFocusEngagementEnabled`는 새로운 API 이며 아직 문서화 되지 않았습니다.

### <a name="focus-trapping"></a>포커스 트래핑

포커스 트래핑은 사용자가 앱의 UI를 탐색하는 동안 컨트롤 내에 "트래핑"되어 해당 컨트롤 외부로 이동하기 어렵거나 불가능할 때 발생합니다.

다음 예제에서는 포커스 트래핑을 만드는 UI를 보여 줍니다.

![가로 슬라이더의 왼쪽 및 오른쪽에 있는 단추](images/designing-for-tv/focus-engagement-focus-trapping.png)

사용자가 왼쪽 단추에서 오른쪽 단추로 이동하려는 경우 D 패드/왼쪽 스틱에서 오른쪽을 두 번 누르기만 하면 된다고 가정하는 것이 논리적입니다.
그러나 [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider)에 연결이 필요하지 않은 경우 다음과 같은 동작이 발생합니다. 사용자가 처음 오른쪽을 누르면 포커스가 `Slider`으로 전환되고, 다시 오른쪽을 누르면 `Slider`의 핸들이 오른쪽으로 이동합니다. 사용자는 핸들을 계속 오른쪽으로 이동하려 하지만 단추에 접근할 수 없습니다.

이 문제를 해결하는 방법에는 여러 가지가 있습니다. 하나는 [XY 포커스 탐색 및 조작](#xy-focus-navigation-and-interaction)의 부동산 앱 예제와 유사하게 다른 레이아웃을 디자인하는 것입니다. 해당 예제에서는 **이전** 및 **다음** 단추의 위치를 `ListView` 위로 옮겼습니다. 다음 이미지와 같이 가로가 아니라 세로로 컨트롤을 겹치면 문제가 해결됩니다.

![가로 슬라이더 위와 아래에 있는 단추](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

이제 사용자가 D 패드/왼쪽 스틱에서 위쪽 및 아래쪽을 눌러 각 컨트롤로 이동할 수 있으며, `Slider`에 포커스가 있을 경우 왼쪽 및 오른쪽을 눌러 `Slider` 핸들을 예상대로 이동할 수 있습니다.

이 문제를 해결하는 또 다른 방법은 `Slider`에서 연결을 요구하는 것입니다. `IsFocusEngagementEnabled="True"`를 설명하면 다음과 같은 동작이 발생합니다.

![사용자가 오른쪽에 있는 단추로 이동할 수 있도록 슬라이더에서 포커스 연결 요구](images/designing-for-tv/focus-engagement-slider.png)

`Slider`에 포커스 연결이 필요한 경우 사용자가 D 패드/왼쪽 스틱에서 오른쪽을 두 번 누르기만 하면 오른쪽에 있는 단추에 접근할 수 있습니다. 이 해결 방법은 UI를 조정할 필요 없이 필요한 동작을 생성하기 때문에 유용합니다.

### <a name="items-controls"></a>항목 컨트롤

[Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) 컨트롤 외에도 연결을 요구할 수 있는 다음과 같은 컨트롤이 있습니다.

- [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)

`Slider` 컨트롤과 달리 이러한 컨트롤은 해당 컨트롤 내에 포커스를 트래핑하지 않습니다. 그러나 대량 데이터를 포함할 경우 유용성 문제가 발생할 수 있습니다. 다음은 대량 데이터를 포함하는 `ListView`의 예입니다.

![대량 데이터를 포함하며 위와 아래에 단추가 있는 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

`Slider` 예제와 유사하게, 게임 패드/리모컨을 사용하여 위쪽에 있는 단추에서 아래쪽에 있는 단추로 이동해 보겠습니다.
먼저 위쪽 단추에 포커스를 놓고 D 패드/스틱에서 아래쪽을 누르면 포커스가 `ListView`의 첫 번째 항목("항목 1")에 배치됩니다.
사용자가 다시 아래쪽을 누르면 아래쪽에 있는 단추가 아니라 목록의 다음 항목이 포커스를 받습니다.
단추에 접근하려면 사용자가 `ListView`에 있는 모든 항목을 먼저 탐색해야 합니다.
`ListView`에 대량 데이터가 포함되어 있는 경우 이는 불편하고 최적 사용자 경험이 아닙니다.

이 문제를 해결하려면 연결을 요구하도록 `ListView`의 `IsFocusEngagementEnabled="True"` 속성을 설정합니다.
이렇게 하면 사용자가 간단하게 아래쪽을 눌러 `ListView`를 건너뛸 수 있습니다. 하지만 포커스가 있을 때 **A/선택** 버튼을 눌러 연결하고 **B/뒤로** 버튼을 눌러 연결 해제하지 않을 경우 목록을 스크롤하고 목록에서 항목을 선택할 수 없습니다.

![연결이 필요한 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

[ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)는 이러한 컨트롤과 약간 다르며 고려해야 하는 자체 쿼크가 있습니다. 포커스 가능 콘텐츠를 포함하는 `ScrollViewer`가 있는 경우 기본적으로 `ScrollViewer`로 이동하면 포커스 가능 요소를 탐색할 수 있습니다. `ListView`에서와 마찬가지로, `ScrollViewer` 외부로 이동하려면 각 항목을 스크롤해야 합니다.

`ScrollViewer`에 포커스 가능 콘텐츠가 *없는* 경우&mdash;예: 텍스트만 포함된 경우&mdash; 사용자가 **A/선택** 버튼을 사용하여 `ScrollViewer`를 연결할 수 있도록 `IsFocusEngagementEnabled="True"`를 설정할 수 있습니다. 연결한 후 **D-패드/왼쪽 스틱**을 사용하여 텍스트를 스크롤한 다음 작업이 끝나면 **B/뒤로** 단추를 눌러 연결 해제할 수 있습니다.

또 다른 방법은 사용자가 컨트롤을 연결할 필요가 없도록 `IsTabStop="True"`의 `ScrollViewer`를 설정하는 것입니다. &mdash;사용자는 포커스를 배치한 다음 `ScrollViewer` 내에 포커스 가능 요소가 있을 경우 **D-패드/왼쪽 스틱**을 사용하여 스크롤하면 됩니다.

### <a name="focus-engagement-defaults"></a>포커스 연결 기본값

일부 컨트롤은 기본 설정에서 연결을 요구할 만큼 포커스 트래핑이 자주 발생하는 반면, 다른 컨트롤은 포커스 연결이 기본적으로 꺼져 있지만 켜면 도움이 될 수 있습니다. 다음 표에서는 이러한 컨트롤 및 해당 기본 포커스 연결 동작을 보여 줍니다.

| 컨트롤               | 포커스 연결 기본값  |
|-----------------------|---------------------------|
| CalendarDatePicker    | 켜짐                        |
| FlipView              | 끄기                       |
| GridView              | 끄기                       |
| ListBox               | 끄기                       |
| ListView              | 끄기                       |
| ScrollViewer          | 끄기                       |
| SemanticZoom          | 끄기                       |
| 슬라이더                | 켜짐                        |

다른 모든 UWP 컨트롤은 `IsFocusEngagementEnabled="True"`일 때 동작 또는 시각적 변경이 없습니다.

## <a name="summary"></a>요약

특정 장치 또는 환경에 최적화 된 UWP 응용 프로그램을 빌드할 수는 있지만, 유니버설 Windows 플랫폼을 사용 하면 2 피트 및 10-면에서 모두 장치에 성공적으로 사용할 수 있는 앱을 빌드할 수 있으며, 입력에 상관 없이 앱을 빌드할 수도 있습니다. 장치 또는 사용자 기능. 이 문서의 권장 사항을 사용 하면 앱이 TV와 PC 둘 다에서 가능 하다는 것을 확인할 수 있습니다.

## <a name="related-articles"></a>관련 문서

- [Xbox 및 TV용 디자인](../devices/designing-for-tv.md)
- [UWP (유니버설 Windows 플랫폼 용 장치 입문) 앱](index.md)
