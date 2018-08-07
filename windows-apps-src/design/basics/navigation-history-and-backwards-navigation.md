---
author: serenaz
Description: The Universal Windows Platform (UWP) provides a consistent back navigation system for traversing the user's navigation history within an app and, depending on the device, from app to app.
title: 탐색 기록 및 뒤로 탐색(Windows 앱)
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/22/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 824f0e83408893bf95d856067282b1fea1313876
ms.sourcegitcommit: 588171ea8cb629d2dd6aa2080e742dc8ce8584e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/17/2018
ms.locfileid: "1895410"
---
#  <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>UWP 앱에 대한 탐색 기록 및 뒤로 탐색

> **중요 APIs**: [BackRequested event](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [SystemNavigationManager class](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

UWP(유니버설 Windows 플랫폼)에서 앱 내에서 그리고 장치에 따라 앱 간에 사용자의 탐색 기록을 트래버스할 수 있도록 일관된 뒤로 탐색 시스템을 제공합니다.

앱에 뒤로 탐색을 구현하려면 앱 UI 왼쪽 위 모서리에 [뒤로 단추](#Back-button)를 배치합니다. 앱이 [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤을 사용하는 경우 [NavigationView의 기본 뒤로 단추](../controls-and-patterns/navigationview.md#backwards-navigation)를 사용할 수 있습니다. 

사용자는 뒤로 단추를 누르면 앱의 탐색 기록에서 이전 위치로 이동될 것으로 예상합니다. 탐색 기록에 추가할 탐색 동작과 뒤로 단추 누르기에 응답하는 방식은 사용자가 결정해야 합니다.

## <a name="back-button"></a>뒤로 단추

뒤로 단추를 만들려면 `NavigationBackButtonNormalStyle` 스타일의 [단추](../controls-and-patterns/buttons.md) 컨트롤을 사용하고, 앱 UI의 왼쪽 위 모서리에 단추를 배치합니다.

![앱 UI 왼쪽 위 모서리의 뒤로 단추](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

앱의 위쪽에 [CommandBar](../controls-and-patterns/app-bars.md)가 있다면, 높이가 44px인 단추 컨트롤과 48px인 AppBarButton이 매끄럽게 정렬되지 않을 것입니다. 이런 문제를 방지하려면 단추 컨트롤 위쪽을 48px 범위 내로 맞춥니다.

![위쪽 명령 모음의 뒤로 단추](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

앱의 이동 UI 요소를 최소화 하려면, 백스택에 아무 것도 없을 때 사용할 수 없는 뒤로 단추를 표시하세요.

![뒤로 단추 상태](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>코드 예제

다음 코드 예제는 뒤로 단추로 뒤로 탐색 동작을 구현하는 방법을 설명하고 있습니다. 코드는 버튼 [**클릭**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click)이벤트에 응답하고, 새 페이지 탐색 시 호출되는 [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)의 버튼 표시를 활성화/비활성화 합니다. 또한 코드 예제는 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 이벤트에 대한 수신기를 등록해 하드웨어 및 소프트웨어 시스템 Back 키의 입력을 처리합니다.

```xaml
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

코드 숨김:

```csharp
public MainPage()
{
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    BackButton.IsEnabled = this.Frame.CanGoBack;
}

private void Back_Click(object sender, RoutedEventArgs e)
{
    On_BackRequested();
}

// Handles system-level BackRequested events and page-level back button Click events
private bool On_BackRequested()
{
    if (this.Frame.CanGoBack)
    {
        this.Frame.GoBack();
        return true;
    }
    return false;
}

private void BackInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}
```

여기서는 `App.xaml` 코드 숨김 파일에 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested)의 글로벌 수신기를 등록합니다. 뒤로 탐색에서 특정 페이지를 제외하려는 경우 각 페이지에 이 이벤트를 등록하거나, 페이지를 표시하기 전에 페이지 수준 코드를 실행할 수 있습니다.

App.xaml 코드 숨김:

```csharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed = e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>여러 장치와 폼 팩터에 최적화

이 뒤로 탐색 디자인 지침을 모든 장치에 적용할 수 있습니다. 그러나 장치와 폼 팩터 별로 최적화를 하는 것이 좋습니다. 또한 여러 셸이 지원하는 하드웨어 뒤로 버튼에 따라 달라집니다.

- **전화/태블릿**: 휴대폰 및 태블릿에는 하드웨어나 소프트웨어 뒤로 단추가 있습니다. 그러나 명료함을 위해 인-앱 단추를 구현하는 것이 좋습니다.
- **데스크톱/허브**: 앱 UI 왼쪽 위 모서리에 인-앱 단추를 구현합니다.
- **Xbox/TV**: UI가 불필요하게 혼잡해지지 않도록 뒤로 단추를 구현하지 않습니다. 대신 뒤로 탐색에 게임패드의 B 버튼을 사용합니다.

앱이 여러 장치에서 실행되는 경우, 단추 표시 여부를 전환하는 [사용자 지정 트리거를 Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox)를 대상으로 만듭니다. NavigationView 컨트롤은 앱이 Xbox에서 실행되는 경우 자동으로 뒤로 단추를 토글합니다. 

뒤로 탐색에 다음 입력을 지원하는 것이 좋습니다. (일부 입력은 시스템 BackRequested가 지원하지 않기 때문에 별도 이벤트로 처리해야 합니다.).

| 입력 | 이벤트 |
| --- | --- |
| Windows-Backspace 키 | BackRequested |
| 하드웨어 뒤로 버튼 | BackRequested |
| 셸 태블릿 모드 뒤로 단추 | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Alt+LeftArrow 키 | KeyboardAccelerator.BackInvoked |

위에 제공된 코드 예제는 이런 입력 모두를 처리하는 방법을 설명하고 있습니다.

## <a name="system-back-behavior-for-backward-compatibilities"></a>시스템의 이전 버전과의 호환성 지원 동작

이전에는 UWP 앱이 뒤로 탐색을 지원하기 위해 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)를 사용했습니다. API는 계속해서 이전 버전과의 호환성을 지원할 것입니다. 그러나 제목 표시줄 뒤로 단추를 사용하는 것은 더 이상 권장하지 않습니다. 대신 앱은 인-앱 뒤로 단추를 호출해야 합니다.

앱이 계속 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)를 사용한다면, 뒤로 단추가 제목 표시줄 내부에 렌더링 됩니다.

![제목 표시줄 뒤로 단추](images/nav-back-pc.png)

## <a name="guidelines-for-custom-back-navigation-behavior"></a>사용자 지정 뒤로 탐색 동작 지침

고유한 뒤로 스택 탐색 기능을 제공하려는 경우 다른 앱과 일관된 환경을 구현해야 합니다. 탐색 작업에 대해 다음과 같은 패턴을 따르는 것이 좋습니다.

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">탐색 작업</th>
<th align="left">탐색 기록에 추가되나요?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>페이지 간, 다른 피어 그룹</strong></td>
<td style="vertical-align:top;"><strong>예</strong>
<p>이 그림에서 사용자는 피어 그룹을 교차해서 앱의 수준 1에서 수준 2로 이동하므로 탐색이 탐색 기록에 추가됩니다.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>다음 그림에서 사용자는 동일한 수준의 두 피어 그룹 간을 이동하고 피어 그룹을 교차하므로 탐색이 탐색 기록에 추가됩니다.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>화면의 탐색 요소를 사용하지 않고 동일한 피어 그룹의 페이지 간</strong>
<p>동일한 피어 그룹을 사용하여 페이지 간을 이동합니다. 두 페이지로 직접 이동할 수 있도록 하는 항상 존재하는 탐색 요소(예: 상단 탐색 창 또는 도킹된 왼쪽 탐색 창)가 없습니다.</p></td>
<td style="vertical-align:top;"><strong>예</strong>
<p>다음 그림에서 사용자는 동일한 피어 그룹의 두 페이지 간을 이동합니다. 페이지에서 상단 탐색 모음 또는 도킹된 왼쪽 탐색 창이 사용되지 않으므로 해당 탐색 내용이 탐색 기록에 추가됩니다.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>화면의 탐색 요소를 사용하여 페이지 간, 동일한 피어 그룹</strong>
<p>동일한 피어 그룹에서 페이지 간을 이동합니다. 두 페이지 모두 동일한 탐색 요소에 표시됩니다. 예를 들어 두 페이지 모두 동일한 상단 창 요소를 사용하거나 두 페이지 모두 도킹된 왼쪽 탐색 창에 표시됩니다.</p></td>
<td style="vertical-align:top;"><strong>유동적입니다.</strong>
<p>예, 탐색 기록을 추가하지만 주목할 만한 예외가 2개 있습니다. 앱 사용자가 피어 그룹 간에 자주 전환할 것으로 예상되는 경우, 또는 피어 그룹의 페이지 내에서 탐색 상태/기록을 유지하려는 경우 탐색 기록에 추가하지 마십시오. 이 경우 사용자가 뒤로를 누르면 현재 피어 그룹으로 이동하기 전에 있던 마지막 페이지로 돌아갑니다. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>임시 UI 표시</strong>
<p>앱은 대화 상자, 시작 화면 또는 화상 키보드와 같은 팝업 또는 자식 창을 표시하거나 다중 선택 모드와 같은 특수 모드를 시작합니다.</p></td>
<td style="vertical-align:top;"><strong>아니요</strong>
<p>사용자가 뒤로 단추를 누르면 임시 UI를 해제하고(화상 키보드 숨기기, 대화 상자 취소 등) 임시 UI를 생성한 페이지로 돌아갑니다.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>항목 열거</strong>
<p>앱은 마스터/세부 정보 목록에서 선택한 항목에 대한 세부 정보와 같이 화면상의 항목에 대한 콘텐츠를 표시합니다.</p></td>
<td style="vertical-align:top;"><strong>아니요</strong>
<p>항목을 열거하는 것은 피어 그룹 내를 탐색하는 것과 비슷합니다. 사용자가 뒤로를 누르면 항목이 열거된 현재 페이지의 이전 페이지로 이동됩니다.</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>다시 시작

사용자가 다른 앱으로 전환했다가 해당 앱으로 돌아올 경우 탐색 기록의 마지막 페이지로 돌아오는 것이 좋습니다.

## <a name="related-articles"></a>관련 문서

- [탐색 기본 사항](navigation-basics.md)