---
title: 게임 패드 및 리모컨 조작
description: Xbox 게임 및 원격 제어에서 입력 하기 위해 앱을 최적화 하는 방법에 대 한 지침, 권장 사항 및 제안을 확인 하세요.
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34bdda207350d980e323b27a7b98e3c0112d06f4
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970181"
---
# <a name="gamepad-and-remote-control-interactions"></a>게임 패드 및 리모컨 조작

![키보드 및 게임 패드 이미지](images/keyboard/keyboard-gamepad.jpg)

***많은 상호 작용 환경은 게임 패드, 원격 제어 및 키보드 간에 공유 됩니다.***

Windows 응용 프로그램에서 Windows 응용 프로그램의 빌드 상호 작용 환경을 사용 하 여 앱이 사용 가능 하 고 기존 입력 유형 (마우스, 키보드, 터치 등) 뿐만 아니라 TV 및 Xbox *10 피트* 환경 (예: 게임 패드 및 원격 제어)을 통해 제공 되는 입력 유형을 통해 앱을 사용할 수 있고 액세스할 수 있는지 확인 합니다.

*10 피트* 환경에서 Windows 응용 프로그램에 대 한 일반 디자인 지침은 [Xbox 및 TV를 위한 디자인](../devices/designing-for-tv.md) 을 참조 하세요.

## <a name="overview"></a>개요

이 항목에서는 상호 작용 디자인에서 고려해 야 할 사항 (또는 플랫폼이 사용자에 게 표시 되는 경우에는 그렇지 않음)에 대해 설명 하 고 장치, 입력 유형, 사용자 기능 및 기본 설정에 관계 없이 사용할 수 있는 Windows 응용 프로그램을 빌드하기 위한 지침, 권장 사항 및 제안을 제공 합니다.

응용 프로그램은 *10 피트* 환경에서와 마찬가지로 *2 피트* 환경에서 직관적이 고 쉽게 사용할 수 있어야 합니다 (그 반대의 경우도 마찬가지). 사용자가 선호 하는 장치를 지원 하 고, UI 포커스를 명확 하 고 unmistakable 하 고, 탐색을 일관적이 고 예측 가능 하도록 콘텐츠를 정렬 하 고, 사용자에 게 원하는 작업에 가장 짧은 경로를 제공 합니다.

> [!NOTE]
> 이 항목의 코드 조각은 대부분 XAML/c #에 있습니다. 그러나 원칙과 개념은 모든 Windows 앱에 적용 됩니다. Xbox 용 HTML/JavaScript Windows 앱을 개발 하는 경우 GitHub의 뛰어난 [Tvhelpers](https://github.com/Microsoft/TVHelpers/wiki) 라이브러리를 확인 하세요.


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>2 피트 및 10 피트 환경 모두에 맞게 최적화

응용 프로그램을 테스트 하 여 2 피트와 10mbps 시나리오 모두에서 잘 작동 하는지, 그리고 모든 기능이 검색 가능 하 고 Xbox [게임 및 원격 제어](#gamepad-and-remote-control)에서 액세스할 수 있도록 하는 것이 좋습니다.

다음은 2 피트 및 10-및 모든 입력 장치에서 사용할 수 있도록 앱을 최적화할 수 있는 몇 가지 다른 방법입니다 (각 항목의 해당 섹션에 대 한 링크).

> [!NOTE]
> Xbox gamepads 및 원격 컨트롤이 많은 Windows 키보드 동작 및 환경을 지원 하기 때문에 이러한 권장 사항은 두 입력 유형에 모두 적합 합니다. 자세한 키보드 정보는 [키보드 상호 작용](keyboard-interactions.md) 을 참조 하세요.

| 기능        | 설명           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 포커스 탐색 및 상호 작용](#xy-focus-navigation-and-interaction) | **XY 포커스 탐색** 을 사용 하면 사용자가 앱의 UI를 탐색할 수 있습니다. 그러나이 경우 사용자가 위로, 아래로, 왼쪽 및 오른쪽으로 이동 하도록 제한 됩니다. 이에 대 한 권장 사항 및 기타 고려 사항은이 섹션에 설명 되어 있습니다. |
| [마우스 모드.](#mouse-mode)|지도, 그리기 및 그리기와 같은 일부 유형의 응용 프로그램에서는 XY 포커스 탐색이 실용적이 지 않거나 가능 하지 않습니다. 이러한 경우 **마우스 모드** 를 사용 하면 사용자가 PC의 마우스와 마찬가지로 게임 패드 또는 원격 제어로 자유롭게 이동할 수 있습니다.|
| [포커스 시각적 개체](#focus-visual)  | 포커스 시각적 개체는 현재 포커스가 있는 UI 요소를 강조 표시 하는 테두리입니다. 이렇게 하면 사용자가 탐색 하거나 상호 작용 하는 UI를 빠르게 식별할 수 있습니다.  |
| [포커스 연결](#focus-engagement) | 포커스 engagement를 사용 하려면 UI 요소에 포커스가 있을 때 사용자가 게임 패드 또는 원격 제어에서 **A/Select** 단추를 눌러야 합니다. |
| [하드웨어 단추](#hardware-buttons) | 게임 패드 및 원격 제어는 매우 다양 한 단추와 구성을 제공 합니다. |

## <a name="gamepad-and-remote-control"></a>게임 패드 및 리모컨

키보드와 마우스는 PC 용 이며 터치는 휴대폰 및 태블릿에 대 한 터치 이며, 게임 패드 및 원격 제어는 10 피트 환경을 위한 기본 입력 장치입니다.
이 섹션에서는 하드웨어 단추가 무엇 이며 어떤 기능을 수행 하는지 소개 합니다.
[XY 포커스 탐색 및 상호 작용](#xy-focus-navigation-and-interaction) 및 [마우스 모드](#mouse-mode)에서 이러한 입력 장치를 사용 하는 경우 앱을 최적화 하는 방법을 알아봅니다.

기본적으로 제공 되는 게임 패드 및 원격 동작의 품질은 앱에서 키보드가 얼마나 잘 지원 되는지에 따라 달라 집니다. 앱이 게임 패드/원격에서 잘 작동 하는지 확인 하는 좋은 방법은 PC에서 키보드를 사용 하 여 제대로 작동 하는지 확인 하 고 게임 패드/원격으로 테스트 하 여 UI에서 weak 지점을 찾는 것입니다.

### <a name="hardware-buttons"></a>하드웨어 단추

이 문서 전체에서 단추는 다음 다이어그램에 지정 된 이름으로 참조 됩니다.

![게임 패드 및 원격 단추 다이어그램](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

다이어그램에서 볼 수 있듯이 원격 제어에서 지원 되지 않는 게임 패드에서 지원 되는 몇 가지 단추가 있으며 그 반대의 경우도 마찬가지입니다. 한 입력 장치 에서만 지원 되는 단추를 사용 하 여 UI를 더 빠르게 탐색할 수 있지만이를 중요 한 상호 작용에 사용 하면 사용자가 UI의 특정 부분과 상호 작용할 수 없는 상황이 발생할 수 있습니다.

다음 표에서는 Windows 앱에서 지원 되는 모든 하드웨어 단추와 이러한 단추를 지 원하는 입력 장치를 보여 줍니다.

| 단추                    | 게임 패드   | 원격 제어    |
|---------------------------|-----------|-------------------|
| A/Select 단추           | 예       | 예               |
| B/뒤로 단추             | 예       | 예               |
| 방향 패드 (D-패드)   | 예       | 예               |
| 메뉴 버튼               | 예       | 예               |
| 보기 단추               | 예       | 예               |
| X 및 Y 단추           | 예       | 아니요                |
| 왼쪽 스틱                | 예       | 아니요                |
| 오른쪽 스틱               | 예       | 아니요                |
| 왼쪽 및 오른쪽 트리거   | 예       | 아니요                |
| 왼쪽 및 오른쪽 범퍼    | 예       | 아니요                |
| OneGuide 단추           | 아니요        | 예               |
| 볼륨 단추             | 아니요        | 예               |
| 채널 단추            | 아니요        | 예               |
| 미디어 컨트롤 단추     | 아니요        | 예               |
| 음소거 단추               | 아니요        | 예               |

### <a name="built-in-button-support"></a>기본 제공 단추 지원

UWP는 기존 키보드 입력 동작을 게임 패드 및 원격 제어 입력에 자동으로 매핑합니다. 다음 표에서는 이러한 기본 제공 매핑을 보여 줍니다.

| 키보드              | 게임 패드/원격                        |
|-----------------------|---------------------------------------|
| 화살표 키            | D-패드 (게임 패드에도 왼쪽 스틱)    |
| 스페이스바              | A/Select 단추                       |
| Enter                 | A/Select 단추                       |
| 이스케이프                | B/뒤로 단추 *                        |

\*응용 프로그램에서 B 단추에 대 한 [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) 또는 [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트를 처리 하지 않으면 [systemnavigationmanager. 요청](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 된 이벤트가 발생 하 여 앱 내에서 다시 탐색이 발생 합니다. 그러나 다음 코드 조각과 같이이를 직접 구현 해야 합니다.

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
> 뒤로 이동 하는 데 B 단추가 사용 되 면 UI에 뒤로 단추를 표시 하지 않습니다. [탐색 뷰](../controls-and-patterns/navigationview.md)를 사용 하는 경우에는 뒤로 단추가 자동으로 숨겨집니다. 뒤로 탐색 하는 방법에 대 한 자세한 내용은 [탐색 기록 및 Windows 앱에 대 한 역방향 탐색](../basics/navigation-history-and-backwards-navigation.md)을 참조 하세요.

Xbox One의 Windows 앱은 **메뉴** 단추를 눌러 상황에 맞는 메뉴를 여는 것도 지원 합니다. 자세한 내용은 [CommandBar 및 ContextFlyout 아웃](#commandbar-and-contextflyout)을 참조 하세요.

### <a name="accelerator-support"></a>액셀러레이터 키 지원

액셀러레이터 단추는 UI를 통해 탐색 속도를 높이는 데 사용할 수 있는 단추입니다. 그러나 이러한 단추는 특정 입력 장치에 대해 고유할 수 있으므로 모든 사용자가 이러한 기능을 사용할 수 있는 것은 아닙니다. 실제로 게임 기는 현재 Xbox One의 Windows 앱에 대 한 액셀러레이터 기능을 지 원하는 유일한 입력 장치입니다.

다음 표에서는 UWP에 기본 제공 되는 액셀러레이터 지원 뿐만 아니라 사용자가 직접 구현할 수 있는 기능을 보여 줍니다. 사용자 지정 UI에서 이러한 동작을 활용 하 여 일관 되 고 친숙 한 사용자 환경을 제공 합니다.

| 상호 작용   | 키보드/마우스   | 게임 패드      | 기본 제공:  | 권장 사항: |
|---------------|------------|--------------|----------------|------------------|
| Page up/down  | Page up/down | 왼쪽/오른쪽 트리거 | [Calendarview](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer` , [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 세로 스크롤을 지 원하는 보기
| 페이지 왼쪽/오른쪽 | 없음 | 왼쪽/오른쪽 범퍼 | [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer` , [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 가로 스크롤을 지 원하는 보기
| 확대/축소        | Ctrl +/- | 왼쪽/오른쪽 트리거 | 없음 | `ScrollViewer`확대 및 축소를 지 원하는 뷰 |
| 탐색 창 열기/닫기 | 없음 | 보기 | 없음 | 탐색 창 |
| 검색 | 없음 | Y 단추 | 없음 | 앱의 기본 검색 기능에 대 한 바로 가기입니다. |
| [상황에 맞는 메뉴 열기](#commandbar-and-contextflyout) | 그런 다음 | 메뉴 버튼 | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | 상황에 맞는 메뉴 |

## <a name="xy-focus-navigation-and-interaction"></a>XY 포커스 탐색 및 상호 작용

앱에서 키보드에 대해 적절 한 포커스 탐색을 지 원하는 경우이는 게임 패드 및 원격 제어로 잘 변환 됩니다.
화살표 키를 사용 하 여 탐색은 **D 패드** 에 매핑되고 (게임 패드의 **왼쪽 스틱** ) UI 요소와의 상호 작용은 **Enter/Select** 키 ( [게임 패드 및 원격 제어](#gamepad-and-remote-control)참조)에 매핑됩니다.

대부분의 이벤트와 속성은 키보드와 이벤트를 모두 사용 하 여 및 이벤트를 실행 하 고, 두 속성 모두 및 &mdash; `KeyDown` 속성을 `KeyUp` 가진 컨트롤만 탐색 합니다 `IsTabStop="True"` `Visibility="Visible"` . 키보드 디자인 지침은 [키보드 상호 작용](../input/keyboard-interactions.md)을 참조 하세요.

키보드 지원이 제대로 구현 되 면 앱이 합리적으로 작동 합니다. 그러나 모든 시나리오를 지 원하는 데 필요한 몇 가지 추가 작업이 있을 수 있습니다. 가능한 최상의 사용자 환경을 제공 하기 위해 앱의 특정 요구 사항에 대해 생각해 보세요.

> [!IMPORTANT]
> Xbox One에서 실행 되는 Windows 앱에 대해서는 마우스 모드가 기본적으로 사용 하도록 설정 되어 있습니다. 마우스 모드를 사용 하지 않도록 설정 하 고 XY 포커스 탐색을 사용 하도록 설정 하려면를 설정 `Application.RequiresPointerMode=WhenRequested` 합니다.

### <a name="debugging-focus-issues"></a>포커스 문제 디버깅

[System.windows.input.focusmanager> system.windows.input.focusmanager.getfocusedelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) 메서드는 현재 포커스가 있는 요소를 알려 줍니다. 이는 포커스 시각적 개체의 위치가 명확 하지 않을 수 있는 경우에 유용 합니다. 다음과 같이이 정보를 Visual Studio 출력 창에 기록할 수 있습니다.

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

* [Istabstop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) 또는 [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) 속성이 잘못 설정 되었습니다.
* 포커스를 가져오는 컨트롤이 실제로는 관심 있는 항목을 &mdash; 렌더링 하는 컨트롤 부분만이 아니라 전체 컨트롤 ([System.windows.frameworkelement.actualwidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 및 [system.windows.frameworkelement.actualheight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight))의 전체 크기를 확인 하는 것 보다 더 큽니다.
* 포커스를 받을 수 있는 한 컨트롤이 다른 &mdash; XY 탐색 위에 있는 경우 겹쳐진 컨트롤을 지원 하지 않습니다.

XY 탐색이 이러한 문제를 해결 한 후에도 계속 작동 하지 않는 경우 [기본 탐색 재정의](#overriding-the-default-navigation)에서 설명 하는 메서드를 사용 하 여 포커스를 가져올 요소를 수동으로 가리킬 수 있습니다.

XY 탐색이 의도 한 대로 작동 하지만 포커스 시각적 개체를 표시 하지 않는 경우 다음 문제 중 하나가 원인일 수 있습니다.

* 컨트롤의 템플릿을 다시 작성 하 고 포커스 시각적 개체를 포함 하지 않았습니다. `UseSystemFocusVisuals="True"`포커스 시각적 개체를 수동으로 설정 하거나 추가 합니다.
* 을 호출 하 여 포커스를 이동 `Focus(FocusState.Pointer)` 했습니다. [FocusState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FocusState) 매개 변수는 포커스 시각적 개체에 대 한 결과를 제어 합니다. 일반적으로이를로 설정 하 여 `FocusState.Programmatic` 이전에 표시 된 경우 포커스 시각적 표시를 유지 하 고, 이전에 숨겨진 경우에는 숨깁니다.

이 섹션의 나머지 부분에서는 XY 탐색을 사용할 때의 일반적인 디자인 문제에 대해 자세히 설명 하 고 이러한 문제를 해결 하는 여러 가지 방법을 제공 합니다.

### <a name="inaccessible-ui"></a>액세스할 수 없는 UI

XY 포커스 탐색은 사용자가 위쪽, 아래쪽, 왼쪽 및 오른쪽으로 이동 하도록 제한 하므로 UI의 파트에 액세스할 수 없는 시나리오가 발생할 수 있습니다.
다음 다이어그램에서는 XY 포커스 탐색에서 지원 하지 않는 UI 레이아웃 종류의 예를 보여 줍니다.
세로 및 가로 탐색의 우선 순위를 지정 하 고 중간 요소는 포커스를 얻기에 충분 한 우선 순위를 갖지 않으므로 게임 패드/원격을 사용 하 여 가운데에 있는 요소에 액세스할 수 없습니다.

![중간에 액세스할 수 없는 요소가 있는 네 모퉁이의 요소](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

몇 가지 이유로 UI를 다시 정렬 하지 못할 경우 다음 섹션에서 설명 하는 방법 중 하나를 사용 하 여 기본 포커스 동작을 재정의 합니다.

### <a name="overriding-the-default-navigation"></a>기본 탐색 재정의

유니버설 Windows 플랫폼는 D-패드/왼쪽 스틱 탐색이 사용자에 게 적합 한지 확인 하는 동안 앱의 의도에 맞게 최적화 된 동작을 보장할 수 없습니다.
앱에 대해 탐색을 최적화 하는 가장 좋은 방법은 게임 패드를 사용 하 여 테스트 하 고 앱의 시나리오에 적합 한 방식으로 사용자가 모든 UI 요소에 액세스할 수 있는지 확인 하는 것입니다. 응용 프로그램의 시나리오가 제공 된 XY 포커스 탐색을 통해 수행 되지 않는 동작을 호출 하는 경우 다음 섹션의 권장 사항을 따르거나 동작을 재정의 하 여 논리적 항목에 포커스를 두기를 고려 합니다.

다음 코드 조각에서는 XY 포커스 탐색 동작을 재정의할 수 있는 방법을 보여 줍니다.

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

이 경우 포커스가 `Home` 단추에 있고 사용자가 왼쪽으로 이동 하면 포커스가 단추로 이동 하 `MyBtnLeft` 고, 사용자가 오른쪽으로 이동 하면 포커스가 단추로 이동 하 게 `MyBtnRight` 됩니다.

포커스가 컨트롤에서 특정 방향으로 이동 하지 않도록 하려면 속성을 사용 `XYFocus*` 하 여 동일한 컨트롤을 가리킵니다.

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

이러한 속성을 사용 하는 경우 `XYFocus` 포커스를 가진 자식이 동일한 속성을 사용 하지 않는 한, 컨트롤 부모는 다음 포커스 후보가 시각적 트리 외부에 있을 때 해당 자식을 강제로 탐색할 수도 있습니다 `XYFocus` .

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

위의 샘플에서 포커스가 `Button` 2에 있고 사용자가 오른쪽으로 이동 하는 경우 가장 좋은 포커스 후보는 4 이지만, `Button` `Button` 부모는 `UserControl` 시각적 트리 외부에 있을 때이를 탐색 하도록 하기 때문에 포커스가 3으로 이동 합니다.

### <a name="path-of-least-clicks"></a>최소 클릭 경로

사용자가 가장 적은 수의 클릭으로 가장 일반적인 작업을 수행할 수 있도록 합니다. 다음 예제에서는 **재생** 단추 (처음에 포커스를 놓기)와 일반적으로 사용 되는 요소 사이에 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 이 배치 되므로 불필요 한 요소가 우선 순위 작업 사이에 배치 됩니다.

![탐색 모범 사례에서 가장 적은 클릭으로 경로 제공](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

다음 예제에서는 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 이 **재생** 단추 위에 배치 됩니다.
불필요 한 요소가 우선 순위 작업 사이에 배치 되지 않도록 UI를 다시 정렬 하면 앱의 유용성이 크게 향상 됩니다.

![재생 단추 위로 이동 된 TextBlock이 더 이상 우선 순위 작업 사이에 있지 않습니다.](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar 및 ContextFlyout 아웃

[명령 모음](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)을 사용 하는 경우 [문제: 긴 스크롤 목록/그리드 후에 위치한 UI 요소](#problem-ui-elements-located-after-long-scrolling-list-grid)에 설명 된 대로 목록을 스크롤 하는 문제를 염두에 두어야 합니다. 다음 이미지는 `CommandBar` 목록/그리드 아래쪽에가 있는 UI 레이아웃을 보여 줍니다. 사용자는 목록/그리드 전체를 스크롤하여에 연결 해야 합니다 `CommandBar` .

![목록/그리드 아래쪽에 있는 명령 모음](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

`CommandBar`목록/그리드 *위에* 을 배치 하면 어떻게 되나요? 목록/그리드를 아래로 스크롤 하는 사용자는에 도달 하기 위해 다시 스크롤해야 하는 반면 `CommandBar` , 이전 구성 보다는 탐색이 약간 낮습니다. 이는 앱의 초기 포커스가의 옆 또는 위에 배치 된 것으로 가정 `CommandBar` 합니다. 초기 포커스가 목록/그리드 아래에 있는 경우에는이 방법이 작동 하지 않습니다. 이러한 `CommandBar` 항목이 자주 액세스 하지 않아도 되는 전역 작업 항목 (예: **동기화** 단추) 인 경우 목록/그리드 위에 포함 하는 것이 적합할 수 있습니다.

의 항목을 세로로 쌓을 수는 없지만 `CommandBar` 스크롤 방향 (예: 세로 스크롤 목록의 왼쪽 또는 오른쪽)에 배치 하거나 가로 스크롤 목록의 맨 위 또는 맨 아래에 배치 하는 것은 UI 레이아웃에 잘 작동 하는 경우 고려해 야 할 또 다른 옵션입니다.

앱에 사용자가 쉽게 액세스할 수 있어야 하는 항목을 포함 하는이 있는 경우 `CommandBar` [contextflyout 아웃](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 안에 이러한 항목을 배치 하 고에서 제거 하는 것이 좋습니다 `CommandBar` . `ContextFlyout` 는 [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 의 속성 이며 해당 요소와 연결 된 [상황에 맞는 메뉴](../controls-and-patterns/dialogs-and-flyouts/index.md) 입니다. PC에서가 있는 요소를 마우스 오른쪽 단추로 클릭 하면 상황에 `ContextFlyout` 맞는 메뉴가 팝업 됩니다. Xbox One에서는 포커스가 이러한 요소에 있는 동안 **메뉴** 단추를 누르면 발생 합니다.

### <a name="ui-layout-challenges"></a>UI 레이아웃 문제

일부 UI 레이아웃은 XY 포커스 탐색의 특성 때문에 더 어려운 문제 이며 사례별로 평가 해야 합니다. 단일 "권한"은 없고 응용 프로그램의 특정 요구 사항에 따라 선택 하는 솔루션은 있지만 훌륭한 TV 환경을 만들기 위해 활용할 수 있는 몇 가지 방법이 있습니다.

이에 대 한 이해를 돕기 위해 이러한 문제를 해결 하는 데 도움이 되는 허수 앱을 살펴보겠습니다.

> [!NOTE]
> 이 가짜 앱은 UI 문제 및 잠재적 해결 방법을 보여 주기 위한 것 이며 특정 앱에 대 한 최상의 사용자 환경을 보여 주기 위한 것이 아닙니다.

다음은 판매에 사용할 수 있는 집, 지도, 속성 설명 및 기타 정보를 표시 하는 가상의 부동산 앱입니다. 이 앱은 다음 기술을 사용 하 여 해결할 수 있는 세 가지 과제를 발생 합니다.

- [UI 다시 정렬](#ui-rearrange)
- [포커스 연결](#engagement)
- [마우스 모드.](#mouse-mode)

![가짜 부동산 앱](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### <a name="problem-ui-elements-located-after-long-scrolling-listgrid"></a>문제: 긴 스크롤 목록/그리드 후에 있는 UI 요소 <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

다음 이미지에 표시 된 속성의 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 는 매우 긴 스크롤 목록입니다. 에 [engagement](#focus-engagement) 가 필요 *하지 않은* 경우 `ListView` 사용자가 목록으로 이동 하면 목록의 첫 번째 항목에 포커스가 배치 됩니다. 사용자가 **이전** 또는 **다음** 단추에 연결 하려면 목록의 모든 항목을 통과 해야 합니다. 사용자가 전체 목록을 트래버스 하도록 요구 하는 경우는 매우 어려운 일입니다 &mdash; .이 경우에는이 환경이 적절 하기에 충분 하지 않을 경우 &mdash; 다른 옵션을 고려해 야 할 수 있습니다.

![부동산 앱: 50 항목을 포함 하는 목록 아래 단추를 클릭 하는 데 51 클릭](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>솔루션

**UI 다시 정렬 <a name="ui-rearrange"></a>**

초기 포커스가 페이지 아래쪽에 배치 되지 않는 한 긴 스크롤 목록 위에 배치 된 UI 요소는 일반적으로 아래 배치 된 것 보다 더 쉽게 액세스할 수 있습니다.
이 새로운 레이아웃이 다른 장치에 대해 작동 하는 경우 Xbox One에 대 한 특별 한 UI 변경을 수행 하는 대신 모든 장치 패밀리의 레이아웃을 변경 하는 것이 비용이 저렴 한 방법이 될 수 있습니다.
또한 스크롤 방향에 대 한 UI 요소를 배치 하는 경우 (즉, 가로 스크롤 목록에 가로 방향으로 또는 가로 스크롤 목록에 수직으로) 액세스 가능성이 향상 될 수 있습니다.

![부동산 앱: 긴 스크롤 목록 위에 단추 놓기](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**포커스 참여 <a name="engagement"></a>**

Engagement가 *필요한*경우 전체는 `ListView` 단일 포커스 대상이 됩니다. 사용자는 목록 내용을 무시 하 여 다음으로 포커스를 받을 수 있는 요소로 이동할 수 있습니다. 참여를 지 원하는 컨트롤 및이를 사용 하는 방법에 대 한 자세한 내용은 [포커스 참여](#focus-engagement)를 참조 하세요.

![부동산 앱: 한 번만 클릭 하 여 이전/다음 단추에 연결 하도록 engagement를 필수로 설정 합니다.](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>문제: 포커스를 받을 요소가 없는 ScrollViewer

XY 포커스 탐색은 한 번에 포커스를 받을 수 있는 UI 요소 하나를 탐색 하는 데 의존 하므로,이 예제와 같이 텍스트만 있는 요소를 포함 하지 않는 [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 사용자가의 모든 콘텐츠를 볼 수 없는 시나리오가 발생할 수 있습니다 `ScrollViewer` .
이 시나리오 및 기타 관련 시나리오에 대 한 해결 방법은 [포커스 참여](#focus-engagement)를 참조 하세요.

![부동산 앱: 텍스트만 있는 ScrollViewer](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>문제: 자유 스크롤 UI

앱에 그리기 화면 등의 자유롭게 스크롤 UI가 필요 하면이 예제에서 지도, XY 포커스 탐색은 작동 하지 않습니다.
이러한 경우, 사용자가 UI 요소 내에서 자유롭게 탐색할 수 있도록 [마우스 모드](#mouse-mode) 를 켤 수 있습니다.

![마우스 모드를 사용 하 여 UI 요소 매핑](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>마우스 모드.

[Xy 포커스 탐색 및 상호 작용](#xy-focus-navigation-and-interaction)에 설명 된 바와 같이 Xbox ONE에서는 xy 탐색 시스템을 사용 하 여 포커스를 이동 하 여 사용자가 위쪽, 아래쪽, 왼쪽 및 오른쪽으로 이동 하 여 컨트롤에서 컨트롤로 포커스를 이동할 수 있도록 합니다.
그러나 [웹 보기](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) 및 [없습니다](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)같은 일부 컨트롤에는 사용자가 포인터를 컨트롤의 경계 안으로 자유롭게 이동할 수 있는 마우스 모양의 상호 작용이 필요 합니다.
사용자가 포인터를 전체 페이지로 이동 하 여 사용자가 마우스를 사용 하 여 PC에서 찾을 수 있는 것과 비슷한 게임 패드/원격 환경을 사용할 수 있도록 하는 응용 프로그램도 있습니다.

이러한 시나리오에서는 전체 페이지 또는 페이지 내의 컨트롤에 대 한 포인터 (마우스 모드)를 요청 해야 합니다.
예를 들어, 응용 프로그램은 컨트롤 내에서 마우스 모드를 사용 하는 컨트롤이 있는 페이지를 포함할 수 있으며, 그 밖의 경우에는 `WebView` XY 포커스를 탐색할 수 있습니다.
포인터를 요청 하려면 **컨트롤이 나 페이지** 를 사용할 때 또는 **페이지에 포커스가**있는지 여부를 지정할 수 있습니다.

> [!NOTE]
> 컨트롤이 포커스를 받을 때 포인터를 요청하는 기능은 지원되지 않습니다.

Xbox One에서 실행 되는 XAML 및 호스팅된 웹 앱 모두에서 마우스 모드가 전체 앱에 대해 기본적으로 켜져 있습니다. 이 기능을 해제 하 고 XY 탐색을 위해 앱을 최적화 하는 것이 좋습니다. 이렇게 하려면 `Application.RequiresPointerMode` `WhenRequested` 컨트롤 또는 페이지에서 호출 하는 경우에만 마우스 모드를 사용 하도록 속성을로 설정 합니다.

XAML 앱에서이 작업을 수행 하려면 클래스에서 다음 코드를 사용 합니다 `App` .

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

HTML/JavaScript에 대 한 샘플 코드를 비롯 한 자세한 내용은 [마우스 모드를 사용 하지 않도록 설정 하는 방법](../../xbox-apps/how-to-disable-mouse-mode.md)을 참조 하세요.

다음 다이어그램에서는 마우스 모드의 게임 패드/원격에 대 한 단추 매핑을 보여 줍니다.

![마우스 모드의 게임 패드/원격에 대 한 단추 매핑](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> 마우스 모드는 게임 패드/원격을 사용 하는 Xbox One 에서만 지원 됩니다. 다른 장치 패밀리 및 입력 형식에서는 자동으로 무시 됩니다.

컨트롤 또는 페이지에서 [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer) 속성을 사용 하 여 마우스 모드를 활성화 합니다. 이 속성에는 세 가지 가능한 값 `Never` (기본값), `WhenEngaged` 및가 `WhenFocused` 있습니다.

### <a name="activating-mouse-mode-on-a-control"></a>컨트롤에서 마우스 모드 활성화

사용자가 컨트롤을 사용 하는 경우 `RequiresPointer="WhenEngaged"` 사용자가 disengages 때까지 컨트롤에서 마우스 모드가 활성화 됩니다. 다음 코드 조각에서는 `MapControl` 마우스를 사용할 때 마우스 모드를 활성화 하는 간단한 방법을 보여 줍니다.

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> 컨트롤에서 작업할 때 마우스 모드를 활성화 하는 경우에도 engagement가 필요 합니다. `IsEngagementRequired="true"` 그렇지 않으면 마우스 모드가 활성화 되지 않습니다.

컨트롤이 마우스 모드에 있을 때 해당 컨트롤은 마우스 모드에도 있습니다. 자식에 대해 요청 된 모드는 무시 됩니다 &mdash; . 부모는 마우스 모드에 있지만 자식이 아닐 수는 없습니다.

또한 요청 된 컨트롤 모드는 포커스를 가져오는 경우에만 검사 되므로 포커스가 있는 동안에는 모드가 동적으로 변경 되지 않습니다.

### <a name="activating-mouse-mode-on-a-page"></a>페이지에서 마우스 모드 활성화

페이지에 속성이 있는 경우 `RequiresPointer="WhenFocused"` 포커스를 받을 때 전체 페이지에 대해 마우스 모드가 활성화 됩니다. 다음 코드 조각에서는이 속성을 페이지에 제공 하는 방법을 보여 줍니다.

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> `WhenFocused`값은 [페이지](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 개체에 대해서만 지원 됩니다. 컨트롤에이 값을 설정 하려고 하면 예외가 throw 됩니다.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>전체 화면 콘텐츠에 대해 마우스 모드 사용 안 함

일반적으로 전체 화면에 비디오 또는 다른 유형의 콘텐츠를 표시 하는 경우 사용자를 방해 수 있으므로 커서를 숨길 수 있습니다. 이 시나리오는 응용 프로그램의 나머지 부분에서 마우스 모드를 사용 하지만 전체 화면 콘텐츠를 표시할 때 해제 하려는 경우에 발생 합니다. 이렇게 하려면 전체 화면 콘텐츠를 자체에 배치 하 `Page` 고 다음 단계를 수행 합니다.

1. 개체에서 `App` 를 설정 `RequiresPointerMode="WhenRequested"` 합니다.
2. `Page`전체 화면을 *제외한* 모든 개체에서 `Page` 을 설정 `RequiresPointer="WhenFocused"` 합니다.
3. 전체 화면에 대해 `Page` 를 설정 `RequiresPointer="Never"` 합니다.

이러한 방식으로 전체 화면 콘텐츠를 표시할 때 커서가 나타나지 않습니다.

## <a name="focus-visual"></a>포커스 시각적 개체

포커스 시각적 개체는 현재 포커스가 있는 UI 요소 주위의 테두리입니다. 이렇게 하면 사용자가 손실 없이 UI를 쉽게 탐색할 수 있도록 사용자에 게 알려줄 수 있습니다.

시각적 개체를 시각적으로 업데이트 하 고 다양 한 사용자 지정 옵션을 사용 하 여 시각적 개체를 시각적으로 볼 수 있으며, 개발자는 Pc 및 Xbox One 뿐만 아니라 키보드 및/또는 게임/원격을 지 원하는 다른 Windows 10 장치 에서도 단일 포커스 시각적 개체를 잘 작동 하도록 신뢰할 수 있습니다.

동일한 포커스 시각적 개체는 서로 다른 플랫폼에서 사용할 수 있지만 사용자가 발견 한 컨텍스트는 10 피트 환경에서 약간 다릅니다. 사용자가 전체 TV 화면에 완전히 집중하지 않는다고 가정해야 하므로 시각 효과를 쉽게 검색할 수 있도록 현재 포커스가 있는 요소가 항상 사용자에게 명확하게 표시되어야 합니다.

또한 키보드를 사용 *하지* 않고 게임 패드 또는 원격 제어를 사용 하는 경우 포커스 시각적 개체는 기본적으로 표시 된다는 점에 유의 해야 합니다. 따라서 구현 하지 않는 경우에도 Xbox One에서 앱을 실행할 때 표시 됩니다.

### <a name="initial-focus-visual-placement"></a>초기 포커스 시각적 배치

앱을 시작 하거나 페이지로 이동 하는 경우 사용자가 작업을 수행 하는 첫 번째 요소로 사용 하는 UI 요소에 포커스를 둡니다. 예를 들어 사진 앱은 갤러리의 첫 번째 항목에 포커스를 놓을 수 있으며, 곡의 자세히 보기로 탐색 하는 음악 앱은 음악을 쉽게 재생할 수 있도록 재생 단추에 포커스를 둘 수 있습니다.

앱의 왼쪽 위 영역 (오른쪽에서 왼쪽 흐름의 경우 오른쪽 위)에 초기 포커스를 배치 해 봅니다. 앱 콘텐츠 흐름이 일반적으로 시작 되는 위치 이기 때문에 대부분의 사용자는 먼저 해당 모퉁이에 집중 하는 경향이 있습니다.

### <a name="making-focus-clearly-visible"></a>포커스를 명확 하 게 표시

사용자가 포커스를 검색 하지 않고 남아 있는 위치를 선택할 수 있도록 항상 화면에 포커스를 표시 해야 합니다. 마찬가지로 항상 포커스를 받을 수 있는 항목이 있어야 합니다 &mdash; . 예를 들어 텍스트만 사용 하 고 포커스를 받을 수 있는 요소만 사용 하지 마세요.

이 규칙의 예외는 비디오 시청 또는 이미지 보기와 같은 전체 화면 환경을 위한 것입니다 .이 경우에는 포커스 시각적 개체를 표시 하는 것이 적절 하지 않을 수 있습니다.

### <a name="reveal-focus"></a>포커스 표시

포커스 표시는 사용자가 게임 패드 또는 키보드 포커스를 이동할 때 단추와 같이 포커스를 받을 수 있는 요소의 테두리에 애니메이션 효과를 주는 조명 효과입니다. 포커스가 있는 요소의 테두리 주위에 광선에 애니메이션을 적용 하 여 포커스의 위치 및 포커스의 위치를 보다 잘 이해할 수 있습니다.

포커스 표시는 기본적으로 해제 되어 있습니다. 10 피트 환경의 경우 앱 생성자에서 [FocusVisualKind 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind) 을 설정 하 여 포커스를 노출 하도록 옵트인 해야 합니다.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

자세한 내용은 [포커스를 보이기](/windows/uwp/design/style/reveal-focus)위한 지침을 참조 하세요.

### <a name="customizing-the-focus-visual"></a>포커스 시각적 개체 사용자 지정

포커스 시각적 개체를 사용자 지정 하려는 경우 각 컨트롤에 대 한 포커스 시각적 개체와 관련 된 속성을 수정 하 여이 작업을 수행할 수 있습니다. 응용 프로그램을 개인 설정 하기 위해 활용할 수 있는 몇 가지 속성이 있습니다.

시각적 상태를 사용 하 여 직접 그려 시스템에서 제공 하는 포커스 시각적 개체를 옵트아웃 (opt out) 할 수도 있습니다. 자세히 알아보려면 [Visualstate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)를 참조 하세요.

### <a name="light-dismiss-overlay"></a>광원 오버레이 해제

사용자가 현재 게임 컨트롤러나 원격 제어를 사용 하 여 조작 하는 UI 요소에 대 한 사용자의 주의가 필요 하면 UWP는 Xbox One에서 앱이 실행 중일 때 팝업 UI 외부 영역을 포함 하는 "스모크" 계층을 자동으로 추가 합니다. 이렇게 하려면 추가 작업이 필요 하지 않지만 UI를 디자인할 때는 주의 해야 합니다. 에서 속성을 설정 `LightDismissOverlayMode` `FlyoutBase` 하 여 스모크 계층을 사용 하거나 사용 하지 않도록 설정할 수 있습니다. 기본값은 `Auto` Xbox에서 사용 하도록 설정 되 고 다른 곳에서는 사용 하지 않도록 설정 됨을 의미 합니다. 자세한 내용은 [모달 vs light 해제](../controls-and-patterns/menus.md)를 참조 하세요.

## <a name="focus-engagement"></a>포커스 연결

포커스 참여는 게임 패드 또는 원격을 사용 하 여 앱과 상호 작용할 수 있도록 하기 위한 것입니다.

> [!NOTE]
> 포커스 참여 설정은 키보드 또는 기타 입력 장치에 영향을 주지 않습니다.

`IsFocusEngagementEnabled` [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 개체의 속성이로 설정 되 면 포커스 참여를 요구 하는 것 `True` 으로 컨트롤을 표시 합니다. 즉, 사용자가 컨트롤을 "참여" 하 고 상호 작용 하려면 **A/Select** 단추를 눌러야 합니다. 완료 되 면 **B/뒤로** 단추를 눌러 컨트롤을 분리 하 고이를 탐색할 수 있습니다.

> [!NOTE]
> `IsFocusEngagementEnabled` 는 새로운 API 이며 아직 문서화 되지 않았습니다.

### <a name="focus-trapping"></a>포커스 트래핑

포커스 트래핑은 사용자가 앱의 UI를 탐색 하려고 하지만 컨트롤 내에서 "트래핑" 되는 경우에 발생 합니다 .이는 해당 컨트롤의 외부에서 이동 하기 어렵거나 불가능 합니다.

다음 예제에서는 포커스 트래핑을 만드는 UI를 보여 줍니다.

![가로 슬라이더의 왼쪽과 오른쪽에 있는 단추](images/designing-for-tv/focus-engagement-focus-trapping.png)

사용자가 왼쪽 단추에서 오른쪽 단추로 이동 하려는 경우에는 사용자가 D-패드/왼쪽 스틱에서 오른쪽을 두 번 눌러야 한다고 가정 하 여 논리적입니다.
그러나 [슬라이더가](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) 참여 하지 않아도 되는 경우 다음 동작이 발생 합니다. 사용자가 처음으로 마우스를 누르면 포커스가로 이동 하 `Slider` 고, 다시 누를 때 `Slider` 의 핸들이 오른쪽으로 이동 합니다. 사용자는 핸들을 오른쪽으로 계속 이동 하 여 단추를 가져올 수 없습니다.

이 문제를 해결 하는 방법에는 여러 가지가 있습니다. 하나는 위의 **이전** 및 **다음** 단추를 재배치 한 [XY 포커스 탐색 및 상호 작용](#xy-focus-navigation-and-interaction) 의 부동산 앱 예제와 유사한 다른 레이아웃을 디자인 하는 것입니다 `ListView` . 다음 이미지와 같이 세로로 컨트롤을 가로로 누적 하 여 문제를 해결 합니다.

![가로 슬라이더의 위쪽 및 아래쪽 단추](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

이제 사용자는 D-패드/왼쪽 스틱에서 위쪽 및 아래쪽을 눌러 각 컨트롤을 탐색 하 고,에 포커스가 있을 때 `Slider` 왼쪽 및 오른쪽을 눌러 `Slider` 필요한 대로 핸들을 이동할 수 있습니다.

이 문제를 해결 하는 또 다른 방법은에 대 한 참여를 요구 하는 것입니다 `Slider` . 를 설정 하 `IsFocusEngagementEnabled="True"` 는 경우 다음과 같은 동작이 발생 합니다.

![사용자가 오른쪽 단추로 탐색할 수 있도록 슬라이더에 포커스 참여 요구](images/designing-for-tv/focus-engagement-slider.png)

에서 `Slider` 포커스 참여를 요구 하는 경우 사용자는 마우스 오른쪽 단추로 D-패드/왼쪽 스틱을 두 번 눌러 오른쪽 단추까지 이동할 수 있습니다. 이 솔루션은 UI 조정이 필요 하지 않으며 필요한 동작을 생성 하기 때문에 유용 합니다.

### <a name="items-controls"></a>Items 컨트롤

[Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) 컨트롤 외에도 다음과 같이 참여를 요구 하는 다른 컨트롤이 있습니다.

- [상자가](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)

컨트롤과 달리 `Slider` 이러한 컨트롤은 자체적으로 포커스를 트래핑 하지 않지만 많은 양의 데이터를 포함할 때 유용성 문제를 일으킬 수 있습니다. 다음은 `ListView` 많은 양의 데이터를 포함 하는의 예입니다.

![더 많은 양의 데이터와 단추가 포함 된 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

예제와 유사 하 게 `Slider` 위쪽의 단추를 클릭 하 여 게임 패드/원격을 사용 하 여 아래쪽의 단추로 이동 합니다.
위쪽 단추에서 포커스를 시작 하면 D-패드/스틱에서 아래로를 누르면 `ListView` ("항목 1")의 첫 번째 항목에 포커스가 추가 됩니다.
사용자가 다시 누르면 목록의 다음 항목이 포커스를 가져오며 맨 아래에 있는 단추는 표시 되지 않습니다.
단추를 가져오려면 사용자가 첫 번째의 모든 항목을 탐색 해야 합니다 `ListView` .
에 `ListView` 많은 양의 데이터가 포함 되어 있는 경우이는 불편할 수 있으며 최적의 사용자 환경이 아닙니다.

이 문제를 해결 하려면에 `IsFocusEngagementEnabled="True"` 대해 engagement를 요구 하도록의 속성을 설정 합니다 `ListView` .
이를 통해 사용자는 간단히를 눌러를 빠르게 건너뛸 수 `ListView` 있습니다. 그러나 포커스가 있을 때 **A/선택** 단추를 누르고 **B/뒤로** 단추를 눌러 분리 하는 경우를 제외 하 고는 목록에서 스크롤하거나 항목을 선택할 수 없습니다.

![참여 해야 하는 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

이러한 컨트롤과 약간 다른 것은 [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)입니다. 포커스를 받을 수 있는 콘텐츠를 포함 하는가 있는 경우 `ScrollViewer` 기본적으로로 이동 하면 `ScrollViewer` 포커스를 받을 수 있는 요소를 이동할 수 있습니다. 에서와 마찬가지로 `ListView` 각 항목을 스크롤하여 외부 탐색 해야 합니다 `ScrollViewer` .

에 포커스를 받을 `ScrollViewer` 수 있는 콘텐츠가 *없는* 경우 &mdash; 예를 들어 &mdash; `IsFocusEngagementEnabled="True"` 사용자가 `ScrollViewer` **A/Select** 단추를 사용 하 여에 참여할 수 있도록 설정할 수 있는 텍스트만 포함 되어 있습니다. 사용 된 후에는 **D-pad/왼쪽 스틱**를 사용 하 여 텍스트를 스크롤하고, **B/뒤로** 단추를 눌러 완료 되 면 분리 합니다.

다른 방법으로에를 설정 하 여 `IsTabStop="True"` `ScrollViewer` 사용자가 컨트롤에 대 한 포커스를 설정 하지 않아도 되 고 내에 포커스 &mdash; 가능 요소가 없을 때 **D-패드/왼쪽 스틱을** 사용 하 여 스크롤할 수 있습니다 `ScrollViewer` .

### <a name="focus-engagement-defaults"></a>포커스 참여 기본값

일부 컨트롤은 포커스 참여를 요구 하는 기본 설정을 보장할 수 있을 정도로 자주 초점을 맞출 수 있는 반면, 다른 일부 컨트롤은 기본적으로 포커스 계약이 꺼져 있지만이 기능을 설정 하 여 이점을 누릴 수 있습니다. 다음 표에서는 이러한 컨트롤과 해당 기본 포커스 참여 동작을 보여 줍니다.

| 제어               | 포커스 참여 기본값  |
|-----------------------|---------------------------|
| CalendarDatePicker    | 켜기                        |
| FlipView              | 끄기                       |
| GridView              | 끄기                       |
| ListBox               | 끄기                       |
| ListView              | 끄기                       |
| ScrollViewer          | 끄기                       |
| SemanticZoom          | 끄기                       |
| 슬라이더                | 켜기                        |

다른 모든 Windows 컨트롤을 사용할 경우에는 동작이 나 시각적 변화가 발생 하지 않습니다 `IsFocusEngagementEnabled="True"` .

## <a name="summary"></a>요약

특정 장치 또는 환경에 맞게 최적화 된 Windows 응용 프로그램을 빌드할 수는 있지만,이 유니버설 Windows 플랫폼을 사용 하면 2 피트 및 10-환경 모두에서 장치 간에 성공적으로 사용할 수 있는 앱과 입력 장치 또는 사용자 기능에 상관 없이 앱을 빌드할 수도 있습니다. 이 문서의 권장 사항을 사용 하면 앱이 TV와 PC 둘 다에서 가능 하다는 것을 확인할 수 있습니다.

## <a name="related-articles"></a>관련 문서

- [Xbox 및 TV용 디자인](../devices/designing-for-tv.md)
- [Windows 앱 용 장치 입문](index.md)
