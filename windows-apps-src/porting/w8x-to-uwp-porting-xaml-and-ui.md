---
description: 선언적 XAML 태그의 형태로 UI를 정의 하는 방법은 유니버설 8.1 앱에서 UWP (유니버설 Windows 플랫폼) 앱으로 매우 잘 변환 합니다.
title: Windows 런타임 .x XAML 및 UI를 UWP로 이식
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 62085377da89d64c8ba0799dc6bab13c17675f90
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750679"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Windows 런타임 .x XAML 및 UI를 UWP로 포팅


이전 항목은 [문제 해결](w8x-to-uwp-troubleshooting.md)이었습니다.

선언적 XAML 태그의 형태로 UI를 정의 하는 방법은 유니버설 8.1 앱에서 UWP (유니버설 Windows 플랫폼) 앱으로 매우 잘 변환 합니다. 사용 중인 사용자 지정 템플릿 또는 시스템 리소스 키를 약간 조정 해야 할 수 있지만 대부분의 태그가 호환 되는 것을 알 수 있습니다. 뷰 모델의 명령적 코드에는 변경이 거의 필요 하지 않습니다. UI 요소를 조작 하는 프레젠테이션 계층의 코드 중 상당 부분에도 포트를 사용할 수 있어야 합니다.

## <a name="imperative-code"></a>명령적 코드

프로젝트를 빌드하는 단계를 가져오려는 경우에는 필수적이 지 않은 코드를 주석 처리 하거나 스텁 할 수 있습니다. 그런 다음, 한 번에 하나의 문제를 반복 하 고이 섹션의 다음 항목 (및 이전 항목의 [문제 해결](w8x-to-uwp-troubleshooting.md))을 참조 합니다.

## <a name="adaptiveresponsive-ui"></a>적응/반응 형 UI

앱은 각자의 화면 크기와 해상도를 포함 하 여 다양 한 범위의 장치에서 실행할 수 있기 때문에 앱을 이식 하는 최소한의 단계를 수행 하 고 해당 장치에서 가장 잘 보이도록 UI를 조정 하는 것이 좋습니다. 적응 시각적 상태 관리자 기능을 사용 하 여 창 크기를 동적으로 검색 하 고 응답의 레이아웃을 변경할 수 있습니다 .이 작업을 수행 하는 방법의 예는 Bookstore2 사례 연구 항목의 [적응 UI](w8x-to-uwp-case-study-bookstore2.md) 섹션에 나와 있습니다.

## <a name="back-button-handling"></a>뒤로 단추 처리

유니버설 8.1 앱의 경우 Windows 런타임 8.x 앱 및 Windows Phone 스토어 앱에는 표시 되는 UI와 뒤로 단추에 대해 처리 하는 이벤트에 대 한 다양 한 방법이 있습니다. 그러나 Windows 10 앱의 경우 앱에서 단일 접근 방식을 사용할 수 있습니다. 모바일 장치에서이 단추는 장치에서 용량 단추 또는 셸에서 단추로 제공 됩니다. 앱 내에서 백 탐색이 가능 하면 데스크톱 장치에서 앱의 chrome에 단추를 추가 하 고,이는 기간 업무 앱의 제목 표시줄 또는 태블릿 모드의 작업 표시줄에 표시 됩니다. 뒤로 단추 이벤트는 모든 장치 패밀리의 유니버설 개념으로, 하드웨어 또는 소프트웨어에서 구현 된 단추는 동일한 [**백 요청**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 된 이벤트를 발생 시킵니다.

아래 예제는 모든 장치 제품군에 대해 작동 하며, 동일한 처리가 모든 페이지에 적용 되 고 탐색을 확인할 필요가 없는 경우 (예: 저장 되지 않은 변경 내용에 대해 경고 하기 위해)에 적합 합니다.

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

프로그래밍 방식으로 앱을 종료 하는 모든 장치 제품군에 대 한 단일 접근 방법도 있습니다.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>참 메뉴

참 메뉴과 통합 되는 코드를 변경할 필요는 없지만, Windows 10 shell의 일부가 아닌 참 메뉴 표시줄을 대신 하는 UI를 앱에 추가 해야 합니다. Windows 10에서 실행 되는 Universal 8.1 앱에는 응용 프로그램의 제목 표시줄에 시스템에서 렌더링 한 chrome이 제공 하는 자체 대체 UI가 있습니다.

## <a name="controls-and-control-styles-and-templates"></a>컨트롤, 컨트롤 스타일 및 템플릿

Windows 10에서 실행 되는 Universal 8.1 앱은 컨트롤에 대 한 8.1 모양 및 동작을 유지 합니다. 그러나 해당 앱을 Windows 10 앱에 이식할 때는 모양과 동작에서 알아야 할 몇 가지 차이점이 있습니다. 컨트롤의 아키텍처와 디자인은 기본적으로 Windows 10 앱에서 변경 되지 않으므로 대부분의 디자인 언어, 단순화 및 유용성 향상을 기반으로 합니다.

**참고**    PointerOver 상태는 Windows 10 앱의 사용자 지정 스타일/템플릿과 관련 되 Windows 런타임 고, Windows Phone 스토어 앱에서는 제외 됩니다. 이러한 이유로 Windows 10 앱에 대해 지원 되는 시스템 리소스 키로 인해 앱을 Windows 10으로 포팅 하는 경우 Windows 런타임 8.x 앱에서 사용자 지정 스타일/템플릿을 다시 사용 하는 것이 좋습니다.
사용자 지정 스타일/템플릿이 최신 시각적 상태 집합을 사용 하 고 있고 기본 스타일/템플릿에 대 한 성능 향상에서 애플리케이션을 빌드하고 하는 것을 확실히 하려면 새 Windows 10 기본 템플릿의 복사본을 편집 하 고 사용자 지정을 다시 적용 합니다. 성능 향상의 한 가지 예는 이전에 **ContentPresenter** 또는 Panel로 포함 된 모든 **테두리가** 제거 되었고 자식 요소가 이제 테두리를 렌더링 한다는 것입니다.

다음은 컨트롤 변경에 대 한 몇 가지 구체적인 예입니다.

| 컨트롤 이름 | 변경 |
|--------------|--------|
| **AppBar**   | **AppBar** 컨트롤을 사용하는 경우(이 컨트롤보다는 [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar)를 사용하는 것이 좋음) Windows 10 앱에서는 기본적으로 해당 컨트롤을 숨기지 않습니다. [**ClosedDisplayMode**](/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode) 속성을 사용 하 여이를 제어할 수 있습니다. |
| **Appbar**, [ **CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows 10 앱에서 **Appbar** 및 [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) 는 **자세히 보기** 단추 (줄임표)를 포함 합니다. |
| [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows 런타임 .x 앱에서 [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) 의 보조 명령은 항상 표시 됩니다. Windows Phone 스토어 앱 및 Windows 10 앱에서 명령 모음이 열릴 때까지가 표시 되지 않습니다. |
| [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows Phone 스토어 앱의 경우 표시줄의 값이 해제 가능한 인지 여부는 영향을 받지 않습니다 [**.**](/uwp/api/windows.ui.xaml.controls.appbar.issticky) Windows 10 앱의 경우 **Issticky** 이 true로 설정 된 경우 **명령 모음** 에서 광원 해제 제스처를 무시 합니다. |
| [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Windows 10 앱에서 [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) 은 [**EdgeGesture**](/uwp/api/windows.ui.input.edgegesture.completed) 및 [**UIElement. righttapped**](/uwp/api/windows.ui.xaml.uielement.righttapped) 이벤트를 처리 하지 않습니다. 또한 누르기 나 살짝 밀기에도 응답 하지 않습니다. 이러한 이벤트를 처리 하 고 [**IsOpen**](/uwp/api/windows.ui.xaml.controls.appbar.isopen)을 설정 하는 옵션이 여전히 있습니다. |
| [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [ **timepicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker) 및 [**timepicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)의 시각적 변경 내용으로 앱이 어떻게 보이는지 검토 합니다. 모바일 장치에서 실행 되는 Windows 10 앱의 경우 이러한 컨트롤은 더 이상 선택 페이지로 이동 하지 않고 해제 가능한 popup을 사용 합니다. |
| [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [ **timepicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Windows 10 앱에서는 [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker) 또는 [**timepicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) 를 날아가기 내에 배치할 수 없습니다. 이러한 컨트롤이 팝업 컨트롤에 표시 되도록 하려면 [**Datepickerflyout 아웃**](/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) 및 [**timepickerflyout 아웃**](/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout)을 사용할 수 있습니다. |
| **GridView**, **ListView** | **Gridview** / **listview**의 경우 [gridview 및 ListView 변경 내용](#gridview-and-listview-changes)을 참조 하세요. |
| [**허브**](/uwp/api/Windows.UI.Xaml.Controls.Hub) | Windows Phone 스토어 앱에서는 [**허브**](/uwp/api/Windows.UI.Xaml.Controls.Hub) 컨트롤이 마지막 섹션에서 첫 번째 섹션으로 래핑 됩니다. Windows 런타임 .x 앱 및 Windows 10 앱에서 허브 섹션은 래핑 되지 않습니다. |
| [**허브**](/uwp/api/Windows.UI.Xaml.Controls.Hub) | Windows Phone 스토어 앱에서 허브 컨트롤의 배경 이미지 [**는 허브 섹션**](/uwp/api/Windows.UI.Xaml.Controls.Hub) 을 기준으로 시차에서 이동 합니다. Windows 런타임 8.x 앱 및 Windows 10 앱에서 시차는 사용 되지 않습니다. |
| [**허브**](/uwp/api/Windows.UI.Xaml.Controls.Hub)  | 유니버설 8.1 앱에서 [**HubSection IsHeaderInteractive**](/uwp/api/windows.ui.xaml.controls.hubsection.isheaderinteractive) 속성은 섹션 헤더와 그 옆에 렌더링 되는 펼침 문자 모양을 대화형으로 만듭니다. Windows 10 앱에서 헤더 옆에 대화형 "참조" affordance 헤더 자체는 대화형이 아닙니다. **IsHeaderInteractive** 는 상호 작용이 허브를 발생 시키는 지 여부를 계속 결정 합니다 [**. 섹션 headerclick**](/uwp/api/windows.ui.xaml.controls.hub.sectionheaderclick) 이벤트 |
| **은 windows.ui.popups.messagedialog** | **Messagedialog**를 사용 하는 경우 보다 유연한 [**contentdialog**](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)를 대신 사용 하는 것이 좋습니다. [XAML UI 기본](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) 샘플도 참조 하세요. |
| **Listpickerflyout 아웃** **, ...**  | **Listpickerflyout 아웃** **및가는 Windows** 10 앱에 대해 사용 되지 않습니다. 단일 선택 항목의 경우 [**Menuflyout 아웃**](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)을 사용 합니다. 더 복잡 한 환경을 위해 [**플라이 아웃**](/uwp/api/Windows.UI.Xaml.Controls.Flyout)을 사용 합니다. |
| [**PasswordBox**](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) | [**IsPasswordRevealButtonEnabled**](/uwp/api/windows.ui.xaml.controls.passwordbox.ispasswordrevealbuttonenabled) 속성은 Windows 10 앱에서 더 이상 사용 되지 않으며 설정 해도 아무런 영향을 주지 않습니다. 대신 [**PasswordRevealMode**](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) 를 사용 합니다 .이는 기본적으로 **피킹 (peeking** )을 사용 합니다 (예 Windows 런타임: 8gb 앱). 또한 [암호 상자에 대 한 지침](../design/controls-and-patterns/password-box.md)을 참조 하세요. |
| [**Pivot**](/uwp/api/Windows.UI.Xaml.Controls.Pivot) | 이제 [**피벗**](/uwp/api/Windows.UI.Xaml.Controls.Pivot) 컨트롤이 유니버설 이므로 모바일 장치에서 더 이상 사용할 수 없습니다. |
| [**SearchBox**](/uwp/api/Windows.UI.Xaml.Controls.SearchBox) | [**Searchbox**](/uwp/api/windows.ui.xaml.controls.searchbox) 는 범용 장치 제품군에서 구현 되지만 모바일 장치에서는 완벽 하 게 작동 하지 않습니다. [AutoSuggestBox에서 사용 되지 않는 Searchbox](#searchbox-deprecated-in-favor-of-autosuggestbox)를 참조 하세요. |
| **SemanticZoom** | **SemanticZoom**의 경우 [SemanticZoom changes](#semanticzoom-changes)를 참조 하세요. |
| [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)  | [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 의 일부 기본 속성이 변경 되었습니다. [**HorizontalScrollMode**](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) 은 **auto**이 고 [**VerticalScrollMode**](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) 가 **Auto 이며 자동**확대 [**모드**](/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode) 를 **사용할 수**없습니다. 새 기본값이 응용 프로그램에 적합 하지 않은 경우 스타일에서 또는 컨트롤 자체의 로컬 값으로 변경할 수 있습니다.  |
| [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Windows 런타임 8pt 앱에서 [**텍스트 상자**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)에 대 한 맞춤법 검사는 기본적으로 해제 되어 있습니다. Windows Phone 스토어 앱 및 Windows 10 앱에서 기본적으로 설정 되어 있습니다. |
| [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) | [**텍스트 상자**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 에 대 한 기본 글꼴 크기가 11에서 15로 변경 되었습니다. |
| [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) | [**TextReadingOrder**](/uwp/api/windows.ui.xaml.controls.textblock.textreadingorder) 의 기본값은 **기본값** 에서 **DetectFromContent**로 변경 되었습니다. 원치 않는 경우 **UseFlowDirection**를 사용 합니다. **기본값** 은 사용 되지 않습니다. |
| 다양 | 강조 색은 Windows Phone 스토어 앱 및 Windows 10 앱에 적용 되지만 Windows 런타임 8.x 앱에는 적용 되지 않습니다.  |

UWP 앱 컨트롤에 대 한 자세한 내용은 [함수 별 컨트롤](../design/controls-and-patterns/controls-by-function.md), 컨트롤 [목록](../design/controls-and-patterns/index.md)및 [컨트롤에 대 한 지침](../design/controls-and-patterns/index.md)을 참조 하세요.

##  <a name="design-language-in-windows10"></a>Windows 10의 디자인 언어

유니버설 8.1 앱과 Windows 10 앱 간의 디자인 언어에는 약간의 중요 한 차이점이 있습니다. 모든 세부 정보는 [디자인](https://developer.microsoft.com/windows/apps/design)을 참조 하세요. 디자인 언어 변경에도 불구 하 고 설계 원칙은 일관 되 게 유지 됩니다. attentive, fiercely, 시각적 요소를 줄이고, 디지털 도메인에 대 한 인증에 집중 하는 것이 좋습니다. 특히 입력 체계와 함께 비주얼 계층 구조 사용 표에서 디자인 그리고 유체 애니메이션을 사용 하 여 환경을 만들 수 있습니다.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>효과적인 픽셀, 거리 보기 및 배율 인수

이전에는 UI 요소의 크기 및 레이아웃을 실제 물리적 크기와 장치의 해상도에서 벗어나 표시 하는 것이 더 좋습니다. 보기 픽셀은 이제 유효 픽셀로 진화 되었으며 해당 용어, 의미 및 제공 되는 추가 값에 대 한 설명입니다.

"해결 방법" 이라는 용어는 일반적으로 픽셀 수와 같이 픽셀 밀도를 측정 하는 것을 의미 합니다. "효과적인 해결 방법"은 이미지 또는 문자 모양을 구성 하는 실제 픽셀이 장치의 거리와 물리적 픽셀 크기 (픽셀 밀도가 실제 픽셀 크기의 상호)를 확인 하는 경우의 차이를 확인 하는 방법입니다. 효과적인 해결 방법은 사용자 중심 이므로 경험을 구축 하는 좋은 메트릭입니다. 모든 요인을 이해 하 고 UI 요소의 크기를 제어 하 여 사용자의 경험을 적절 하 게 만들 수 있습니다.

서로 다른 장치는 가장 작은 장치에 대해 320 window.epx.codesnippet부터, 적당 한 크기의 모니터의 경우 1024 window.epx.codesnippet까지, 그리고 훨씬 더 높은 너비까지 다양 한 유효 픽셀 수를 가집니다. 항상 자동 크기 요소 및 동적 레이아웃 패널을 사용 하는 것이 좋습니다. UI 요소의 속성을 XAML 태그의 고정 크기로 설정 하는 경우도 있습니다. 크기 조정 요소는 앱이 실행 되는 장치 및 사용자가 만든 표시 설정에 따라 앱에 자동으로 적용 됩니다. 그리고 이러한 크기 조정 요소를 사용 하면 다양 한 화면 크기를 통해 사용자에 게 일관 된 크기의 고정 된 (및 읽기) 대상을 제공 하는 고정 크기의 UI 요소를 유지할 수 있습니다. 뿐만 아니라 동적 레이아웃을 사용 하는 경우에는 UI가 다른 장치에서 optically 크기 조정 되지 않습니다. 대신, 사용 가능한 공간에 적절 한 양의 콘텐츠를 맞추는 데 필요한 작업을 수행 합니다.

앱이 모든 디스플레이에서 최상의 환경을 제공 하기 위해 각각 특정 배율 인수에 적합 한 크기의 범위로 각 비트맵 자산을 만드는 것이 좋습니다. 100% 규모, 200% 규모 및 400% 규모 (해당 우선 순위)로 자산을 제공 하면 대부분의 경우 중간 규모의 모든 요소에서 뛰어난 결과를 얻을 수 있습니다.

**참고**    어떤 이유로 든, 여러 크기의 자산을 만들 수 없는 경우 100% 규모의 자산을 만들 수 있습니다. Microsoft Visual Studio에서 UWP 앱에 대 한 기본 프로젝트 템플릿은 브랜딩 자산 (타일 이미지 및 로고)을 한 크기로 제공 하지만 100% 눈금은 제공 하지 않습니다. 사용자 고유의 앱에 대 한 자산을 제작 하는 경우이 섹션의 지침에 따라 100%, 200% 및 400% 크기를 제공 하 고 자산 팩을 사용 합니다.

복잡 한 아트 워크가 있는 경우 더 많은 크기의 자산을 제공 하는 것이 좋습니다. 벡터 아트를 사용 하 여 시작 하는 경우에는 규모에 관계 없이 고품질의 자산을 생성 하기가 비교적 쉽습니다.

모든 크기 조정 요인을 지원 하지 않는 것이 좋지만 Windows 10 앱에 대 한 크기 조정 요소의 전체 목록은 100%, 125%, 150%, 200%, 250%, 300% 및 400%입니다. 제공 하는 경우 저장소에서 각 장치에 대 한 올바른 크기의 자산을 선택 하 고 해당 자산만 다운로드 됩니다. 스토어는 장치의 DPI에 따라 다운로드할 자산을 선택 합니다. 140% 및 220%와 같은 크기 조정 요소에서 Windows 런타임 8.x 앱의 자산을 다시 사용할 수 있지만 앱은 새로운 크기 조정 요소 중 하나에서 실행 되므로 일부 비트맵 크기 조정은 피할 수 없게 됩니다. 다양 한 장치에서 앱을 테스트 하 여 사용자의 사례 결과에 만족 하는지 확인 합니다.

리터럴 차원 값이 태그에 사용 되는 Windows 런타임 .x 앱에서 XAML 태그를 다시 사용할 수 있습니다 (예를 들어, 모양 또는 다른 요소의 크기를 입력 하는 경우). 그러나 경우에 따라 유니버설 8.1 앱에 비해 Windows 10 앱에 대 한 장치에서 더 큰 규모의 요소가 사용 됩니다. 예를 들어 150%는 140%가 이전에 사용 되 고 200%는 180% was에서 사용 됩니다. 따라서 Windows 10에서 이러한 리터럴 값이 너무 크면 0.8으로 곱합니다. 자세한 내용은 [UWP 앱 용 반응 형 디자인 101](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)을 참조 하세요.

## <a name="gridview-and-listview-changes"></a>GridView 및 ListView 변경 내용

[**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 의 기본 스타일 setter에 대해 몇 가지 변경 내용이 적용 되어 이전에는 기본적으로는 가로를 사용 하지 않고 컨트롤을 세로로 스크롤할 수 있습니다. 프로젝트에서 기본 스타일의 복사본을 편집한 경우 복사본에 이러한 변경 내용이 없으므로 수동으로 설정 해야 합니다. 다음은 변경 내용 목록입니다.

-   [**ScrollViewer.HorizontalScrollBarVisibility**](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility)에 대한 setter가 **Auto**에서 **Disabled**로 변경되었습니다.
-   VerticalScrollBarVisibility의 setter가 ScrollViewer에서 **Auto** **로 변경** 되었습니다 [**.**](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility)
-   [**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) 의 Setter가 **Enabled** 에서 **Disabled**로 변경 되었습니다.
-   VerticalScrollMode의 setter가 **Disabled** 에서 [**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) **로 변경 되었습니다.**
-   [**ItemsPanel**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)의 Setter에서 ItemsWrapGrid의 값이 **세로** 에서 **가로로**변경 되었습니다 [**.**](/uwp/api/windows.ui.xaml.controls.itemswrapgrid.orientation)

마지막 변경 내용 ( **방향**변경)이 모순 되는 경우 래핑 그리드에 대해 이야기 하 고 있다는 것을 명심 하세요. 가로 방향 줄 바꿈 그리드 (새 값)는 텍스트를 가로로 이동 하 고 페이지 끝에서 다음 줄로 나누는 쓰기 시스템과 유사 합니다. 세로 방향으로 스크롤 하는 것과 같은 텍스트 페이지입니다. 반대로 세로 방향 줄 바꿈 모눈 (이전 값)은 텍스트가 세로로 흐르는 쓰기 시스템과 유사 하므로 가로로 스크롤됩니다.

Windows 10에서 변경 되거나 지원 되지 않는 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 및 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 의 측면은 다음과 같습니다.

-   [**IsSwipeEnabled**](/uwp/api/windows.ui.xaml.controls.listviewbase.isswipeenabled) 속성 (Windows 런타임 .x 앱에만 해당)은 Windows 10 앱에 대해 지원 되지 않습니다. API는 여전히 존재 하지만이를 설정 해도 아무런 효과가 없습니다. 모든 이전 선택 제스처는 하향 살짝 밀기 (데이터가 검색 되지 않는 것으로 표시 됨)를 제외 하 고 마우스 오른쪽 단추를 클릭 하 여 (상황에 맞는 메뉴를 표시 하도록 예약 됨)에도 지원 됩니다.
-   [**ReorderMode**](/uwp/api/windows.ui.xaml.controls.listviewbase.reordermode) 속성 (Windows Phone 스토어 앱에만 해당)은 Windows 10 앱에 대해 지원 되지 않습니다. API는 여전히 존재 하지만이를 설정 해도 아무런 효과가 없습니다. 대신 **GridView** 또는 **ListView** 에서 [**allowdrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) 및 [**CanReorderItems**](/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) 를 true로 설정 하면 사용자가 누르고 있음 (또는 클릭 후 끌기) 제스처를 사용 하 여 순서를 변경할 수 있습니다.
-   Windows 10 용으로 개발 하는 경우 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView)에 대해 항목 컨테이너 스타일에서 [**GridViewItemPresenter**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemPresenter) 대신 [**ListViewItemPresenter**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter) 를 사용 합니다. 기본 항목 컨테이너 스타일의 복사본을 편집 하는 경우 올바른 형식을 얻게 됩니다.
-   Windows 10 앱에 대 한 선택 시각적 개체가 변경 되었습니다. [**SelectionMode**](/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) 를 **Multiple**으로 설정 하면 기본적으로 각 항목에 대해 확인란이 렌더링 됩니다. **ListView** 항목의 기본 설정은 확인란이 항목 옆에 인라인으로 배치 됨을 의미 하며, 그 결과 항목의 나머지 부분이 차지 하는 공간은 약간 줄어들고 이동 됩니다. **GridView** 항목의 경우 확인란은 기본적으로 항목의 맨 위에 중첩 됩니다. 그러나 두 경우 모두, 아래 예제와 같이 [**Checkmode**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode) 속성을 사용 하 여 확인란의 레이아웃 (인라인 또는 오버레이)과 항목 컨테이너 스타일 내의 [**ListViewItemPresenter**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) 요소에 모두 ( [**selectioncheckmarkvisualenabled**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) 속성 사용)를 표시할지 여부를 제어할 수 있습니다.
-   Windows 10에서 [**ContainerContentChanging**](/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트는 UI 가상화 중에 항목당 두 번 발생 합니다. 즉, 회수에 대해 한 번, 다시 사용 하기 위해 한 번 발생 합니다. [**InRecycleQueue**](/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.inrecyclequeue) 의 값이 **true** 이 고 수행할 작업을 수행할 수 없는 경우, 동일한 항목이 다시 사용 될 때 다시 입력 된다는 보증을 사용 하 여 이벤트 처리기를 즉시 종료할 수 있습니다. 이때 **InRecycleQueue** 는 **false**가 됩니다.

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![인라인 확인란이 있는 listviewitempresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

인라인 확인란이 있는 ListViewItemPresenter

![겹친 listviewitempresenter 확인란](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

중첩 된 확인란을 사용 하는 ListViewItemPresenter

-   위에서 설명한 이유 때문에 선택 항목에 대해 하향 살짝 밀기 및 오른쪽 클릭 제스처를 제거 하면 [**ItemClick**](/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) 및 [**selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트를 더 이상 사용할 수 없다는 결과가 발생 합니다. Windows 10 앱의 경우 시나리오를 검토 하 고 "선택" 또는 "호출" 상호 작용 모델을 채택할 지 여부를 결정 합니다. 자세한 내용은 [상호 작용 모드를 변경 하는 방법](/previous-versions/windows/apps/hh780625(v=win.10))을 참조 하세요.
-   [**ListViewItemPresenter**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter)스타일을 적용 하는 데 사용 하는 속성에 대 한 몇 가지 변경 내용이 있습니다. 새로 만들기 인 속성은 [**CheckBoxBrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkboxbrush), [**보도 sedbackground**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pressedbackground), [**Selected보도 sedbackground**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpressedbackground)및 [**FocusSecondaryBorderBrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.focussecondaryborderbrush)입니다. Windows 10 앱에 대해 무시 되는 속성은 [**패딩**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.padding) (대신 [**contentmargin**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.contentmargin) 사용), [**checkhintbrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkhintbrush), [**CheckSelectingBrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkselectingbrush), [**PointerOverBackgroundMargin**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pointeroverbackgroundmargin), [**ReorderHintOffset**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.reorderhintoffset), [**SelectedBorderThickness**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedborderthickness)및 [**SelectedPointerOverBorderBrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpointeroverborderbrush)입니다.

이 표에서는 [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) 및 [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) 컨트롤 템플릿의 시각적 상태 및 시각적 상태 그룹에 대 한 변경 내용을 설명 합니다.

| 8.1                 | 기능 상태           | 윈도우 10        | 기능 상태       |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | 정상                  |                   | 정상              |
|                     | PointerOver             |                   | PointerOver         |
|                     | 누름                 |                   | 누름             |
|                     | Pointer과잉 누름      |                   | 사용할 수 없습니다       |
|                     | 사용 안 함                |                   | 사용할 수 없습니다       |
|                     | 사용할 수 없습니다           |                   | PointerOverSelected |
|                     | 사용할 수 없습니다           |                   | 선택함            |
|                     | 사용할 수 없습니다           |                   | PressedSelected     |
| 사용할 수 없습니다       |                         | DisabledStates    |                     |
|                     | 사용할 수 없습니다           |                   | 사용 안 함            |
|                     | 사용할 수 없습니다           |                   | 사용             |
| SelectionHintStates |                         | 사용할 수 없습니다     |                     |
|                     | VerticalSelectionHint   |                   | 사용할 수 없습니다       |
|                     | HorizontalSelectionHint |                   | 사용할 수 없습니다       |
|                     | NoSelectionHint         |                   | 사용할 수 없습니다       |
| 사용할 수 없습니다       |                         | MultiSelectStates |                     |
|                     | 사용할 수 없습니다           |                   | MultiSelectDisabled |
|                     | 사용할 수 없습니다           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | 사용할 수 없습니다     |                     |
|                     | 선택을             |                   | 사용할 수 없습니다       |
|                     | 선택 취소              |                   | 사용할 수 없습니다       |
|                     | UnselectedPointerOver   |                   | 사용할 수 없습니다       |
|                     | UnselectedSwiping 밀기       |                   | 사용할 수 없습니다       |
|                     | 선택               |                   | 사용할 수 없습니다       |
|                     | 선택함                |                   | 사용할 수 없습니다       |
|                     | SelectedSwiping 밀기         |                   | 사용할 수 없습니다       |
|                     | SelectedUnfocused       |                   | 사용할 수 없습니다       |

사용자 지정 [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) 또는 [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) 컨트롤 템플릿이 있는 경우 위의 변경 내용에 대 한 밝은 내용을 검토 합니다. 새 기본 템플릿의 복사본을 편집 하 고 사용자 지정을 다시 적용 하 여 처음부터 다시 시작 하는 것이 좋습니다. 어떤 이유로 든 이러한 작업을 수행할 수 없으며 기존 템플릿을 편집 해야 하는 경우에는이 작업을 수행 하는 방법에 대 한 일반적인 지침을 제공 합니다.

-   새 MultiSelectStates 시각적 상태 그룹을 추가 합니다.
-   새 MultiSelectDisabled 시각적 상태를 추가 합니다.
-   새 MultiSelectEnabled 시각적 상태를 추가 합니다.
-   새 DisabledStates 시각적 상태 그룹을 추가 합니다.
-   사용 가능한 새 시각적 상태를 추가 합니다.
-   CommonStates 시각적 상태 그룹에서 Pointer눌린 시각적 상태를 제거 합니다.
-   사용할 수 없는 시각적 상태를 DisabledStates 시각적 상태 그룹으로 이동 합니다.
-   새 Pointer선택한 시각적 상태를 추가 합니다.
-   새 새 보도 Sed선택한 시각적 상태를 추가 합니다.
-   SelectedHintStates 시각적 상태 그룹을 제거 합니다.
-   SelectionStates 시각적 상태 그룹에서 선택한 시각적 상태를 CommonStates 시각적 상태 그룹으로 이동 합니다.
-   전체 SelectionStates 시각적 상태 그룹을 제거 합니다.

## <a name="localization-and-globalization"></a>지역화 및 세계화

UWP 앱 프로젝트에서 Universal 8.1 프로젝트의 리소스. resw 파일을 다시 사용할 수 있습니다. 파일을 복사한 후에는 프로젝트에 추가 하 고 **빌드 작업** 을 대상 **리소스로** 설정 하 고 **출력 디렉터리로 복사** 를 **복사 하지**않도록 설정 합니다. [**QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) 항목에서는 장치 패밀리 리소스 선택 요소를 기준으로 장치 패밀리 특정 리소스를 로드 하는 방법을 설명 합니다.

## <a name="play-to"></a>재생 대상

[**PlayTo**](/uwp/api/Windows.Media.PlayTo) 네임 스페이스의 api는 windows 10 앱에 대해 더 이상 사용 되지 않으며,이 api를 [**사용 합니다.**](/uwp/api/Windows.Media.Casting)

## <a name="resource-keys-and-textblock-style-sizes"></a>리소스 키 및 TextBlock 스타일 크기

디자인 언어가 Windows 10에 대해 진화 하 고 결과적으로 특정 시스템 스타일이 변경 되었습니다. 일부 경우에는 변경 된 스타일 속성을 사용 하 여 조화 되도록 보기의 시각적 디자인을 다시 방문 하는 것이 좋습니다.

리소스 키가 더 이상 지원 되지 않는 경우도 있습니다. Visual Studio의 XAML 태그 편집기는 확인할 수 없는 리소스 키에 대 한 참조를 강조 표시 합니다. 예를 들어 XAML 태그 편집기는 빨간색 물결선으로 스타일 키에 대 한 참조에 밑줄을 표시 합니다 `ListViewItemTextBlockStyle` . 이 문제가 해결 되지 않으면 에뮬레이터 또는 장치에 앱을 배포 하려고 할 때 앱이 즉시 종료 됩니다. 따라서 XAML 태그 정확성에 참석 하는 것이 중요 합니다. Visual Studio는 이러한 문제를 식별하는 데 유용한 도구입니다.

계속 지원 되는 키의 경우 디자인 언어를 변경 하면 일부 스타일로 설정 된 속성이 변경 됩니다. 예를 들어는 `TitleTextBlockStyle` Windows Phone 스토어 앱의 Windows 런타임 .x 앱 및 18.14 px에서 **FontSize** 를 14.667 px로 설정 합니다. 하지만 동일한 스타일을 Windows 10 앱에서 훨씬 더 큰 24px **로 설정 합니다** . 디자인 및 레이아웃을 검토 하 고 올바른 스타일을 사용 합니다. 자세한 내용은 [글꼴에 대 한 지침](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) 및 [UWP 앱 디자인](https://developer.microsoft.com/windows/apps/design)을 참조 하세요.

이는 더 이상 지원 되지 않는 키의 전체 목록입니다.

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   Controlheadercontent보도 서 스타일
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   Headercontent보도 서 스타일
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   왼쪽 Fullwindowmargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>AutoSuggestBox의 경우에 사용 되지 않는 SearchBox

[**Searchbox**](/uwp/api/windows.ui.xaml.controls.searchbox) 는 범용 장치 제품군에서 구현 되지만 모바일 장치에서는 완벽 하 게 작동 하지 않습니다. 유니버설 검색 환경에 [**AutoSuggestBox**](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) 를 사용 합니다. **AutoSuggestBox**를 사용 하 여 일반적으로 검색 환경을 구현 하는 방법은 다음과 같습니다.

사용자가 입력을 시작 하면 **TextChanged** 이벤트가 발생 하 고 **userinput**의 이유가 발생 합니다. 그런 다음 제안 목록을 채우고 [**AutoSuggestBox**](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)의 **system.windows.controls.itemscontrol.itemssource** 를 설정 합니다. 사용자가 목록을 탐색할 때 **SuggestionChosen** 이벤트가 발생 합니다. **TextMemberDisplayPath**를 설정 하면 텍스트 상자가 지정 된 속성을 사용 하 여 자동으로 채워집니다. 사용자가 Enter 키를 사용 하 여 선택 항목을 제출 하면 **Querysubmitted** 이벤트가 발생 합니다 .이 경우 해당 제안에 대해 작업을 수행할 수 있습니다 .이 경우에는 지정 된 콘텐츠에 대 한 자세한 내용이 포함 된 다른 페이지로 이동 하는 경우가 많습니다. **SearchBoxQuerySubmittedEventArgs** 의 **LinguisticDetails** 및 **Language** 속성은 더 이상 지원 되지 않습니다 (해당 기능을 지 원하는 동일한 api가 있습니다). 및 **Keymodifiers** 는 더 이상 지원 되지 않습니다.

[**AutoSuggestBox**](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) 에는 ime (입력기)도 지원 됩니다. "찾기" 아이콘을 표시 하려는 경우에도이 작업을 수행할 수 있습니다 (아이콘과 상호 작용 하면 **Querysubmitted** 된 이벤트가 발생 함).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

[AutoSuggestBox 포팅 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)도 참조 하세요.

## <a name="semanticzoom-changes"></a>SemanticZoom 변경

[**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 에 대 한 확대/축소 제스처는 Windows Phone 모델에서 수렴 했습니다. 즉, 그룹 머리글을 탭 하거나 클릭 하 여 데스크톱 컴퓨터에서 affordance를 축소 하기 위해 빼기 단추가 더 이상 표시 되지 않습니다. 이제 모든 장치에서 동일 하 고 일관 된 동작이 가능 합니다. Windows Phone 모델과의 한 가지 외관 차이는 축소 된 보기 (점프 목록)가 축소 된 보기를 겹치지 않고 확대 된 뷰로 대체 한다는 것입니다. 따라서 축소 된 뷰에서 반 불투명 배경을 제거할 수 있습니다.

Windows Phone 스토어 앱에서는 축소 된 보기가 화면 크기로 확장 됩니다. Windows 런타임 8.x 앱 및 Windows 10 앱에서 축소 된 보기의 크기는 **SemanticZoom** 컨트롤의 범위로 제한 됩니다.

Windows Phone 스토어 앱에서 축소 된 보기의 콘텐츠 (z 순서)는 축소 된 보기의 배경에 투명도가 있는 경우를 표시 합니다. Windows 런타임 8.x 앱 및 Windows 10 앱에서는 축소 된 보기 뒤에 아무 것도 표시 되지 않습니다.

Windows 런타임 8.x 앱에서 앱을 비활성화 하 고 다시 활성화 하면 축소 된 보기가 해제 되 고 (표시 되는 경우), 확대 보기가 대신 표시 됩니다. Windows Phone 스토어 앱 및 Windows 10 앱에서 축소 된 보기는 표시 되 고 있는 경우에도 계속 표시 됩니다.

Windows Phone 스토어 앱 및 Windows 10 앱에서는 뒤로 단추를 누를 때 축소 된 보기가 해제 됩니다. Windows 런타임 8.x 앱의 경우에는 기본 제공 되는 뒤로 단추를 처리 하지 않으므로 질문은 적용 되지 않습니다.

## <a name="settings"></a>설정

Windows 런타임 .x **SettingsPane** 클래스는 Windows 10에 적합 하지 않습니다. 대신, 설정 페이지를 빌드하는 것 외에도 사용자에 게 앱 내에서 액세스할 수 있는 방법을 제공 해야 합니다. 탐색 창에서 마지막으로 고정 된 항목으로 최상위 수준에이 앱 설정 페이지를 표시 하는 것이 좋지만, 여기서는 전체 옵션 집합을 제공 합니다.

-   탐색 창. 설정은 선택 항목의 탐색 목록에서 마지막 항목으로, 아래쪽에 고정 되어 있어야 합니다.
-   Appbar/toolbar (탭 보기 또는 피벗 레이아웃 내). 설정은 appbar 또는 도구 모음 메뉴 플라이 아웃의 마지막 항목 이어야 합니다. 설정에 탐색 내의 최상위 항목 중 하나를 설정 하는 것은 권장 되지 않습니다.
-   Hub-and-spoke. 설정은 메뉴 플라이 아웃의 내부에 있어야 합니다 (앱 바 메뉴 또는 허브 레이아웃 내의 도구 모음 메뉴에 있을 수 있음).

또한 마스터-세부 창 내에서 설정을 bury 하지 않는 것이 좋습니다.

설정 페이지는 앱 창의 전체를 채워야 하며, 설정 페이지는 및 피드백이 어디에도 있어야 합니다. 설정 페이지의 디자인에 대 한 지침은 [앱 설정에 대 한 지침](../design/app-settings/guidelines-for-app-settings.md)을 참조 하세요.

## <a name="text"></a>텍스트

텍스트 (또는 입력 체계)는 UWP 앱의 중요 한 측면이 며 포팅 하는 동안 새 디자인 언어와 조화 되도록 보기의 시각적 디자인을 다시 확인할 수 있습니다. 다음 그림을 사용 하면 사용할 수 있는 UWP (유니버설 Windows 플랫폼) **TextBlock** 시스템 스타일을 찾을 수 있습니다. 사용한 Windows Phone Silverlight 스타일에 해당 하는 항목을 찾습니다. 또는 고유한 범용 스타일을 만들고 Windows Phone Silverlight 시스템 스타일에서 해당 속성을 복사할 수 있습니다.

![windows 10 앱에 대 한 시스템 textblock 스타일](images/label-uwp10stylegallery.png) <br/>Windows 10 앱에 대 한 시스템 TextBlock 스타일

Windows 런타임 .x 앱 및 Windows Phone 스토어 앱에서 기본 글꼴 패밀리는 전역 사용자 인터페이스입니다. Windows 10 앱에서 기본 글꼴 패밀리는 맑은 고딕 됩니다. 따라서 앱의 글꼴 메트릭이 다를 수 있습니다. 8.1 텍스트의 모양을 재현 하려는 경우 [**Lineheight**](/uwp/api/windows.ui.xaml.controls.textblock.lineheight) 및 [**LineStackingStrategy**](/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy)와 같은 속성을 사용 하 여 고유한 메트릭을 설정할 수 있습니다.

Windows 런타임 .x 앱 및 Windows Phone 스토어 앱에서 텍스트의 기본 언어는 빌드의 언어나 en-us로 설정 됩니다. Windows 10 앱에서 기본 언어는 최상위 앱 언어 (글꼴 대체)로 설정 됩니다. [**FrameworkElement. 언어**](/uwp/api/windows.ui.xaml.frameworkelement.language) 를 명시적으로 설정할 수 있지만 해당 속성에 대 한 값을 설정 하지 않으면 글꼴 대체 동작을 향상 시킬 수 있습니다.

자세한 내용은 [글꼴에 대 한 지침](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) 및 [UWP 앱 디자인](https://developer.microsoft.com/)을 참조 하세요. 또한 텍스트 컨트롤에 대 한 변경 사항은 위의 [Controls](#controls-and-control-styles-and-templates) 섹션을 참조 하세요.

## <a name="theme-changes"></a>테마 변경

유니버설 8.1 앱의 경우 기본 테마는 기본적으로 짙은 색입니다. Windows 10 장치의 경우 기본 테마가 변경 되었지만 app.xaml에서 요청 된 테마를 선언 하 여 사용 되는 테마를 제어할 수 있습니다. 예를 들어 모든 장치에서 짙은 테마를 사용 하려면 `RequestedTheme="Dark"` 루트 응용 프로그램 요소에를 추가 합니다.

## <a name="tiles-and-toasts"></a>타일 및 알림을

타일 및 알림을의 경우 현재 사용 중인 템플릿은 Windows 10 앱에서 계속 작동 합니다. 그러나 사용할 수 있는 새로운 적응 템플릿이 있으며 [알림, 타일, 알림을 및 배지](../design/shell/tiles-and-notifications/index.md)에 설명 되어 있습니다.

이전에는 데스크톱 컴퓨터에서 toast 알림이 일시적 메시지 였습니다. 누락 되거나 무시 된 후에는 더 이상 검색할 수 없습니다. Windows Phone에서 알림 메시지를 무시 하거나 일시적으로 해제 하면 작업 센터로 이동 합니다. 이제 관리 센터는 더 이상 모바일 장치 제품군으로 제한 되지 않습니다.

알림 메시지를 보내기 위해 더 이상 기능을 선언할 필요가 없습니다.

## <a name="window-size"></a>창 크기

유니버설 8.1 앱의 경우 [**Applicationview**](/uwp/schemas/appxpackage/appxmanifestschema2013/element-applicationview) 앱 매니페스트 요소는 최소 창 너비를 선언 하는 데 사용 됩니다. UWP 앱에서 명령적 코드를 사용 하 여 최소 크기 (너비와 높이 모두)를 지정할 수 있습니다. 기본 최소 크기는 500x320epx 이며이는 허용 되는 최소 최소 크기 이기도 합니다. 허용 되는 가장 큰 최소 크기는 500x500epx입니다.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

다음 항목은 [i/o, 장치 및 앱 모델에 대해 포팅](w8x-to-uwp-input-and-sensors.md)하는 항목입니다.