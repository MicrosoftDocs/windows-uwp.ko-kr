---
Description: Windows 앱 내에서 사용자의 탐색 기록을 살펴볼 수 있도록 뒤로 탐색하는 기능을 구현하는 방법을 알아봅니다.
title: 탐색 기록 및 뒤로 탐색
template: detail.hbs
op-migration-status: ready
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 705b0ecb474c0bb821c3a21f4b8b66073984827f
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469578"
---
# <a name="navigation-history-and-backwards-navigation-for-windows-apps"></a>Windows 앱을 위한 탐색 기록 및 뒤로 탐색

> **중요 API**: [BackRequested 이벤트](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [SystemNavigationManager 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

Windows 앱은 앱 내에서 그리고 디바이스에 따라 앱 간에 사용자의 탐색 기록을 탐색할 수 있도록 일관적인 뒤로 탐색 시스템을 제공합니다.

앱에 뒤로 탐색 기능을 구현하려면 앱 UI 왼쪽 위 모서리에 [뒤로 단추](#back-button)를 배치합니다. 앱에서 [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤을 사용하는 경우 [NavigationView의 기본 뒤로 단추](../controls-and-patterns/navigationview.md#backwards-navigation)를 사용할 수 있습니다.

사용자는 뒤로 단추를 누르면 앱의 탐색 기록에서 이전 위치로 이동될 것으로 예상합니다. 탐색 기록에 추가할 탐색 동작과 뒤로 단추 누르기에 응답하는 방식은 사용자가 결정해야 합니다.

## <a name="back-button"></a>뒤로 단추

뒤로 단추를 만들려면 `NavigationBackButtonNormalStyle` 스타일의 [단추](../controls-and-patterns/buttons.md) 컨트롤을 사용하고, 앱 UI의 왼쪽 위 모서리에 단추를 배치합니다(자세한 내용은 아래의 XAML 코드 예제 참조).

![앱 UI의 왼쪽 위에 있는 뒤로 단추](images/back-nav/BackEnabled.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>

    </Grid>
</Page>
```

앱의 위쪽에 [CommandBar](../controls-and-patterns/app-bars.md)가 있다면 높이가 44px인 단추 컨트롤과 48px인 AppBarButton이 매끄럽게 정렬되지 않을 것입니다. 이런 문제를 방지하려면 단추 컨트롤 위쪽을 48px 범위 내로 맞춥니다.

![위쪽 명령 모음의 뒤로 단추](images/back-nav/CommandBar.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <CommandBar>
            <CommandBar.Content>
                <Button Style="{StaticResource NavigationBackButtonNormalStyle}" VerticalAlignment="Top"/>
            </CommandBar.Content>
        
            <AppBarButton Icon="Delete" Label="Delete"/>
            <AppBarButton Icon="Save" Label="Save"/>
        </CommandBar>
    </Grid>
</Page>
```

앱의 이동 UI 요소를 최소화 하려면 백스택에 아무 것도 없을 때 사용할 수 없는 뒤로 단추를 표시하세요. 하지만 앱에 백스택이 없어야 하는 경우에는 뒤로 단추를 표시할 필요가 없습니다.

![뒤로 단추 상태](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>코드 예제

다음 코드 예제에서는 뒤로 단추로 뒤로 탐색 동작을 구현하는 방법을 설명합니다. 이 코드는 단추 [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) 이벤트에 응답하고, 새 페이지 탐색 시 호출되는 [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)의 단추 표시를 활성화/비활성화합니다. 또한 이 코드 예제는 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 이벤트에 대한 수신기를 등록하여 하드웨어 및 소프트웨어 시스템 뒤로 키의 입력을 처리합니다.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

코드 숨김:

```csharp
// MainPage.xaml.cs
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

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Navigation.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;

namespace winrt::PageNavTest::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        altLeft.Key(Windows::System::VirtualKey::Left);
        altLeft.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);
        KeyboardAccelerators().Append(altLeft);
        // ALT routes here.
        altLeft.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
    }

    void MainPage::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
    {
        BackButton().IsEnabled(Frame().CanGoBack());
    }

    void MainPage::Back_Click(IInspectable const&, RoutedEventArgs const&)
    {
        On_BackRequested();
    }

    // Handles system-level BackRequested events and page-level back button Click events.
    bool MainPage::On_BackRequested()
    {
        if (Frame().CanGoBack())
        {
            Frame().GoBack();
            return true;
        }
        return false;
    }

    void MainPage::BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& sender,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        args.Handled(On_BackRequested());
    }
}
```

위에서는 단일 페이지의 뒤로 탐색을 처리했습니다. 뒤로 탐색에서 특정 페이지를 제외하려는 경우 또는 페이지를 표시하기 전에 페이지 수준 코드를 실행하려는 경우에는 각 페이지에서 탐색을 처리하면 됩니다.

전체 앱의 뒤로 탐색을 처리하려면 `App.xaml` 코드 숨김 파일에서 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 이벤트에 대한 글로벌 수신기를 등록합니다.

App.xaml 코드 숨김:

```csharp
// App.xaml.cs
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content as Frame;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}

private bool On_BackRequested()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// App.cpp
#include <winrt/Windows.UI.Core.h>
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"

#include "App.h"
#include "MainPage.h"

using namespace winrt;
...

    Windows::UI::Core::SystemNavigationManager::GetForCurrentView().BackRequested({ this, &App::App_BackRequested });
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }
    rootFrame.PointerPressed({ this, &App::On_PointerPressed });
...

void App::App_BackRequested(IInspectable const& /* sender */, Windows::UI::Core::BackRequestedEventArgs const& e)
{
    e.Handled(On_BackRequested());
}

void App::On_PointerPressed(IInspectable const& sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender.as<UIElement>()).Properties().PointerUpdateKind() == Windows::UI::Input::PointerUpdateKind::XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled(On_BackRequested());
    }
}

// Handles system-level BackRequested events.
bool App::On_BackRequested()
{
    if (Frame().CanGoBack())
    {
        Frame().GoBack();
        return true;
    }
    return false;
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>여러 디바이스와 폼 팩터에 맞게 최적화

이 뒤로 탐색 디자인 지침을 모든 디바이스에 적용할 수 있지만, 디바이스와 폼 팩터를 최적화하는 것이 좋습니다. 최적화 역시 여러 셸에서 지원하는 하드웨어 뒤로 단추에 따라 달라집니다.

- **휴대폰/태블릿**: 휴대폰과 태블릿에는 항상 하드웨어 또는 소프트웨어 뒤로 단추가 있지만, 확실하게 알 수 있도록 인-앱 단추를 구현하는 것이 좋습니다.
- **데스크톱/허브**: 앱 UI 왼쪽 위 모서리에 인-앱 단추를 구현합니다.
- **Xbox/TV**: UI가 불필요하게 복잡해지지 않도록 뒤로 단추를 구현하지 않습니다. 대신 뒤로 탐색에 게임패드의 B 버튼을 사용합니다.

앱이 여러 디바이스에서 실행되는 경우 단추 표시 여부를 전환하는 [사용자 지정 트리거를 Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox)를 대상으로 만듭니다. NavigationView 컨트롤은 앱이 Xbox에서 실행되는 경우 자동으로 뒤로 단추를 토글합니다. 

뒤로 탐색에 다음 입력을 지원하는 것이 좋습니다. (일부 입력은 시스템 BackRequested에서 지원하지 않기 때문에 별도의 이벤트로 처리해야 합니다.).

| 입력 | 이벤트 |
| --- | --- |
| Windows-Backspace 키 | BackRequested |
| 하드웨어 뒤로 버튼 | BackRequested |
| 셸 태블릿 모드 뒤로 단추 | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Alt+LeftArrow 키 | KeyboardAccelerator.BackInvoked |

위에 제공된 코드 예제에서는 이러한 입력을 처리하는 방법을 보여줍니다.

## <a name="system-back-behavior-for-backward-compatibilities"></a>이전 버전과의 호환성을 위한 시스템 뒤로 동작

이전에는 UWP 앱이 뒤로 탐색을 지원하기 위해 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)를 사용했습니다. 이전 버전과의 호환성을 위해 API는 계속 지원되지만, [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)를 사용하는 것은 더 이상 권장하지 않습니다. 대신 앱에서 자체적인 인-앱 뒤로 단추를 구현해야 합니다.

앱에서 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)를 계속 사용하는 경우 시스템 UI는 제목 표시줄 내부에 시스템 뒤로 단추를 렌더링합니다. (뒤로 단추의 모양과 사용자 상호 작용은 이전 빌드와 달라진 것이 없습니다.)

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
<td style="vertical-align:top;"><strong>페이지 간, 동일한 피어 그룹, 화면 탐색 요소 없음</strong>
<p>동일한 피어 그룹을 사용하여 페이지 간을 이동합니다. 두 페이지를 직접 탐색하는 화면 탐색 요소(예: <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>)는 없습니다.</p></td>
<td style="vertical-align:top;"><strong>예</strong>
<p>다음 일러스트레이션에서 사용자는 같은 피어 그룹에 있는 두 페이지 사이를 탐색하며, 탐색 기록에 탐색을 추가해야 합니다.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>페이지 간, 동일한 피어 그룹, 화면 탐색 요소 사용</strong>
<p>동일한 피어 그룹에서 페이지 간을 이동합니다. 두 페이지가 동일한 탐색 요소(예: <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>)에 표시됩니다.</p></td>
<td style="vertical-align:top;"><strong>유동적</strong>
<p>예, 탐색 기록에 추가하지만, 두 가지 주목할 만한 예외가 있습니다. 앱 사용자가 피어 그룹의 페이지 간에 자주 전환할 것으로 예상되는 경우 또는 탐색 계층을 유지하려는 경우에는 탐색 기록에 추가하지 마세요. 이 경우 사용자가 뒤로 탐색을 누르면 현재 피어 그룹으로 이동하기 전에 있던 마지막 페이지로 돌아갑니다. </p>
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

## <a name="related-articles"></a>관련된 문서

- [탐색 기본 사항](navigation-basics.md)
