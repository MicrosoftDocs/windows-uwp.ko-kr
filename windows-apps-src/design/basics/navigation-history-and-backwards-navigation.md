---
description: Windows 앱 내에서 사용자의 탐색 기록을 살펴볼 수 있도록 뒤로 탐색하는 기능을 구현하는 방법을 알아봅니다.
title: 탐색 기록 및 뒤로 탐색
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 738efe43afaf5c278425d5636bddc72cecadf47a
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988765"
---
# <a name="navigation-history-and-backwards-navigation-for-windows-apps"></a>Windows 앱을 위한 탐색 기록 및 뒤로 탐색

> **중요 API**: [BackRequested 이벤트](/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [SystemNavigationManager 클래스](/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)

Windows 앱은 앱 내에서 그리고 디바이스에 따라 앱 간에 사용자의 탐색 기록을 탐색할 수 있도록 일관적인 뒤로 탐색 시스템을 제공합니다.

앱에 뒤로 탐색 기능을 구현하려면 앱 UI 왼쪽 위 모서리에 뒤로 단추를 배치합니다. 사용자는 뒤로 단추를 누르면 앱의 탐색 기록에서 이전 위치로 이동될 것으로 예상합니다. 탐색 기록에 추가할 탐색 동작과 뒤로 단추 누르기에 응답하는 방식은 사용자가 결정해야 합니다.

여러 페이지가 있는 대부분의 앱에서는 [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤을 사용하여 앱에 대한 탐색 프레임워크를 제공하는 것이 좋습니다. 이 컨트롤은 다양한 화면 크기에 맞게 조정되고 ‘위쪽’ 및 ‘왼쪽’ 탐색 스타일을 둘 다 지원합니다.  앱에서 `NavigationView` 컨트롤을 사용하는 경우 [NavigationView의 기본 뒤로 단추](../controls-and-patterns/navigationview.md#backwards-navigation)를 사용할 수 있습니다.

> [!NOTE]
> `NavigationView` 컨트롤을 사용하지 않고 탐색을 구현하는 경우 이 문서의 지침과 예제를 사용해야 합니다. `NavigationView`를 사용하는 경우 이 정보는 유용한 배경 지식을 제공하지만, [NavigationView](../controls-and-patterns/navigationview.md) 문서에 제공된 구체적인 지침과 예제를 사용해야 합니다.

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

        <Button x:Name="BackButton"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>

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
                <Button x:Name="BackButton"
                        Style="{StaticResource NavigationBackButtonNormalStyle}"
                        IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                        ToolTipService.ToolTip="Back" 
                        VerticalAlignment="Top"/>
            </CommandBar.Content>
        
            <AppBarButton Icon="Delete" Label="Delete"/>
            <AppBarButton Icon="Save" Label="Save"/>
        </CommandBar>
    </Grid>
</Page>
```

UI 요소가 앱 내부를 돌아다니는 일을 최소화하려면 백스택에 아무 것도 없을 때 비활성화된 뒤로 단추를 표시하세요(`IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}"`). 하지만 앱에 백스택이 없는 것이 분명한 경우에는 뒤로 단추를 표시할 필요가 없습니다.

![뒤로 단추 상태](images/back-nav/BackDisabled.png)

## <a name="optimize-for-different-devices-and-inputs"></a>다양한 디바이스 및 입력에 맞게 최적화

이 역방향 탐색 디자인 지침은 모든 디바이스에 적용되지만, 다양한 폼 팩터 및 입력 방법을 최적화할 때 사용자에게 도움이 됩니다.

UI를 최적화하는 방법은 다음과 같습니다.

- **데스크톱/허브**: 앱 UI 왼쪽 위 모서리에 인-앱 단추를 구현합니다.
- **[태블릿 모드](https://support.microsoft.com/windows/use-your-pc-like-a-tablet-4fbfcca5-f058-814a-4f80-a12e703d7c34)** : 태블릿에 하드웨어 또는 소프트웨어 뒤로 단추가 있을 수 있지만, 확실하게 알 수 있도록 인-앱 뒤로 단추를 구현하는 것이 좋습니다.
- **Xbox/TV**: UI가 불필요하게 복잡해지지 않도록 뒤로 단추를 구현하지 않습니다. 대신 뒤로 탐색에 게임패드의 B 버튼을 사용합니다.

앱이 Xbox에서 실행되는 경우 단추 표시 여부를 전환하는 [Xbox의 사용자 지정 시각적 트리거를 만듭니다](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox). [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤을 사용할 경우 앱이 Xbox에서 실행되면 이 컨트롤이 자동으로 뒤로 단추를 토글합니다.

뒤로 탐색을 위한 가장 일반적인 입력을 지원하려면 뒤로 단추 클릭 외에도 다음 이벤트를 처리하는 것이 좋습니다.

| 이벤트 | 입력 |
| --- | --- |
| [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) | Alt+왼쪽 화살표,<br/>VirtualKey.GoBack |
| [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) | Windows + 백스페이스 키,<br/>게임 패드 B 단추,<br/>태블릿 모드 뒤로 단추,<br/>하드웨어 뒤로 버튼 |
| [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) | VirtualKey.XButton1<br/>(예: 일부 마우스의 뒤로 단추) |

## <a name="code-examples"></a>코드 예제

이 섹션에서는 다양한 입력을 사용하여 뒤로 탐색을 처리하는 방법을 보여줍니다.

### <a name="back-button-and-back-navigation"></a>뒤로 단추 및 뒤로 탐색

최소한 뒤로 단추 `Click` 이벤트를 처리하고 뒤로 탐색을 수행하는 코드를 제공해야 합니다. 또한 백스택이 비어 있는 경우 뒤로 단추를 사용하지 않도록 설정해야 합니다.

다음 코드 예제에서는 뒤로 단추를 사용하여 뒤로 탐색 동작을 구현하는 방법을 보여줍니다. 이 코드는 단추 [클릭](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) 이벤트에 응답하여 탐색합니다. 뒤로 단추는 새 페이지를 탐색할 때 호출되는 [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) 메서드에서 사용하거나 사용하지 않도록 설정됩니다.

이 코드는 `MainPage`에 대한 것이지만, 뒤로 탐색을 지원하는 각 페이지에 이 코드를 추가합니다. 중복을 방지하려면 `App.xaml.*` 코드 숨김 페이지의 `App` 클래스에 탐색 관련 코드를 넣으면 됩니다.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
        <Button x:Name="BackButton" Click="BackButton_Click"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>
...
<Page/>
```

코드 숨김:

```csharp
// MainPage.xaml.cs
private void BackButton_Click(object sender, RoutedEventArgs e)
{
    App.TryGoBack();
}

// App.xaml.cs
//
// Add this method to the App class.
public static bool TryGoBack()
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
// MainPage.h
namespace winrt::AppName::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();
 
        void MainPage::BackButton_Click(IInspectable const&, RoutedEventArgs const&)
        {
            App::TryGoBack();
        }
    };
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
};
```

### <a name="support-access-keys"></a>액세스 키 지원

키보드 지원은 기술, 능력, 기대치가 각기 다른 사용자들이 애플리케이션을 원활하게 조작하기 위한 필수 요소입니다. 앞으로 및 뒤로 탐색을 사용하는 사용자는 두 가지 탐색 기능을 기대하므로 앞으로 및 뒤로 탐색을 위한 액셀러레이터 키를 모두 지원하는 것이 좋습니다. 자세한 내용은 [키보드 조작](..\input\keyboard-interactions.md) 및 [키보드 액셀러레이터](..\input\keyboard-accelerators.md)를 참조하세요.

앞으로 및 뒤로 탐색을 위한 일반적인 액셀러레이터 키는 Alt+오른쪽 화살표(앞으로) 및 Alt+왼쪽 화살표(뒤로)입니다. 이러한 키를 탐색에 지원하려면 [CoreDispatcher. AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) 이벤트를 처리합니다. 페이지의 요소가 아니라 창에 직접 있는 이벤트를 처리하므로 포커스가 어느 요소에 있든 상관없이 앱이 액셀러레이터 키에 응답합니다.

다음과 같이 액셀러레이터 키 및 앞으로 탐색을 지원하기 위해 `App` 클래스에 코드를 추가합니다. (뒤로 단추를 지원하는 이전 코드가 이미 추가된 것으로 가정합니다.) 코드 예제 섹션의 끝 부분에서 모든 `App` 코드를 함께 볼 수 있습니다.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // ...

    }
}

// ...

// Add this code after the TryGoBack method added previously.
// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...
    // Add this code after the TryGoBack method added previously.

private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
 
 
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }
};
```

### <a name="handle-system-back-requests"></a>시스템 뒤로 요청 처리

Windows 디바이스는 시스템에서 뒤로 탐색 요청을 앱에 전달할 수 있는 다양한 방법을 제공합니다. 대표적인 방법으로 게임 패드의 B 단추, Windows 키 + 백스페이스 키 바로 가기 또는 태블릿 모드의 시스템 뒤로 단추가 있으며, 사용 가능한 정확한 옵션은 디바이스에 따라 다릅니다.

[SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) 이벤트의 수신기를 등록하면 하드웨어 및 소프트웨어 시스템 뒤로 키의 시스템 제공 뒤로 요청을 지원할 수 있습니다.

다음은 시스템 제공 뒤로 요청을 지원하기 위해 `App` 클래스에 추가되는 코드입니다. (뒤로 단추를 지원하는 이전 코드가 이미 추가된 것으로 가정합니다.) 코드 예제 섹션의 끝 부분에서 모든 `App` 코드를 함께 볼 수 있습니다.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // ...

    }
}

// ...
// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }
};
```

#### <a name="system-back-behavior-for-backward-compatibility"></a>이전 버전과의 호환성을 위한 시스템 뒤로 동작

이전에는 UWP 앱에서 뒤로 탐색을 위한 시스템 뒤로 단추를 표시하거나 숨기기 위해 [SystemNavigationManager.AppViewBackButtonVisibility](/uwp/api/windows.ui.core.systemnavigationmanager.appviewbackbuttonvisibility)를 사용했습니다. (이 단추는 [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) 이벤트를 발생시킵니다.) 이전 버전과의 호환성을 위해 API는 계속 지원되지만, `AppViewBackButtonVisibility`에서 표시하는 단추를 사용하는 것은 더 이상 권장하지 않습니다. 그 대신, 이 문서에 설명된 대로 개발자 고유의 인-앱 뒤로 단추를 제공해야 합니다.

[AppViewBackButtonVisibility](/uwp/api/windows.ui.core.appviewbackbuttonvisibility)를 계속 사용하는 경우 시스템 UI는 제목 표시줄 내부에 시스템 뒤로 단추를 렌더링합니다. (뒤로 단추의 모양과 사용자 상호 작용은 이전 빌드와 달라진 것이 없습니다.)

![제목 표시줄 뒤로 단추](images/nav-back-pc.png)

### <a name="handle-mouse-navigation-buttons"></a>마우스 탐색 단추 처리

일부 마우스는 앞으로 및 뒤로 탐색을 위한 하드웨어 탐색 단추를 제공합니다. [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) 이벤트를 처리하고 [IsXButton1Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton1pressed)(뒤로) 또는 [IsXButton2Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton2pressed)(앞으로)를 확인하여 이러한 마우스 단추를 지원할 수 있습니다.

다음은 마우스 단추 탐색을 지원하기 위해 `App` 클래스에 추가되는 코드입니다. (뒤로 단추를 지원하는 이전 코드가 이미 추가된 것으로 가정합니다.) 코드 예제 섹션의 끝 부분에서 모든 `App` 코드를 함께 볼 수 있습니다.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

### <a name="all-code-added-to-app-class"></a>App 클래스에 추가된 모든 코드
 
```csharp
// App.xaml.cs
//
// (Add event handlers in OnLaunched override.)
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// (Add these methods to the App class.)
public static bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}

// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}

// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}


```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
  
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

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
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two and the back to group one." /></p>
<p>다음 그림에서 사용자는 동일한 수준의 두 피어 그룹 간을 이동하고 피어 그룹을 교차하므로 탐색이 탐색 기록에 추가됩니다.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two then on to group three and back to group two." /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>페이지 간, 동일한 피어 그룹, 화면 탐색 요소 없음</strong>
<p>동일한 피어 그룹을 사용하여 페이지 간을 이동합니다. 두 페이지를 직접 탐색하는 화면 탐색 요소(예: <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>)는 없습니다.</p></td>
<td style="vertical-align:top;"><strong>예</strong>
<p>다음 일러스트레이션에서 사용자는 같은 피어 그룹에 있는 두 페이지 사이를 탐색하며, 탐색 기록에 탐색을 추가해야 합니다.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>페이지 간, 동일한 피어 그룹, 화면 탐색 요소 사용</strong>
<p>동일한 피어 그룹에서 페이지 간을 이동합니다. 두 페이지가 동일한 탐색 요소(예: <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>)에 표시됩니다.</p></td>
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
