---
Description: 앱의 각 부분을 별도의 창으로 표시합니다.
title: 앱에 대한 여러 보기 표시
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729514"
---
# <a name="show-multiple-views-for-an-app"></a>앱에 대한 여러 보기 표시

![여러 창으로 앱을 표시하는 와이어프레임](images/multi-view.gif)

앱의 독립적인 부분을 개별 창에서 볼 수 있도록 하면 사용자의 생산성을 높이는 데 도움이 됩니다. 여러 개의 앱 창을 만드는 경우 작업 표시줄에 각 창이 별도로 표시됩니다. 사용자는 앱 창을 독립적으로 이동, 크기 조정, 표시 및 숨길 수 있으며, 별도의 앱인 것처럼 앱 창 간에 전환할 수 있습니다.

> **중요 API**: [Windows.UI.ViewManagement 네임스페이스](/uwp/api/windows.ui.viewmanagement), [Windows.UI.WindowManagement 네임스페이스](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>앱에서 여러 뷰를 사용해야 하는 경우

여러 뷰를 활용할 수 있는 다양한 시나리오가 있습니다. 다음은 몇 가지 예입니다.

- 사용자가 받은 메시지 목록을 보면서 새 메일을 작성할 수 있는 메일 앱
- 사용자가 여러 사람의 연락처 정보를 나란히 비교할 수 있는 주소록 앱
- 사용자가 현재 재생 중인 음악과 재생 가능한 음악 목록을 동시에 볼 수 있는 음악 플레이어 앱
- 사용자가 노트의 한 페이지에서 다른 페이지로 정보를 복사할 수 있는 메모 작성 앱
- 사용자가 모든 상위 헤드라인을 정독한 후 나중에 읽을 수 있도록 여러 기사를 열어 둘 수 있는 읽기 앱

각 앱 레이아웃은 고유하지만, 콘텐츠의 오른쪽 위와 같은 예측 가능한 위치에 “새 창” 단추를 배치하여 새 창으로 열 수 있도록 하는 것이 좋습니다. [상황에 맞는 메뉴](../controls-and-patterns/menus.md)에 “새 창에서 열기” 옵션을 넣는 것도 좋습니다.

동일한 인스턴스에 대해 별도의 창을 만드는 대신, 앱의 개별 인스턴스를 만들려면 [다중 인스턴스 UWP 앱 만들기](../../launch-resume/multi-instance-uwp.md)를 참조하세요.

## <a name="windowing-hosts"></a>창 작업 호스트

앱 내에서 UWP 콘텐츠를 호스트할 수 있는 방법에는 여러 가지가 있습니다.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     앱 보기는 앱이 콘텐츠를 표시하는 데 사용하는 창과 스레드의 1:1 연결입니다. 앱을 시작할 때 생성되는 첫 번째 보기를 *기본 보기*라고 합니다. 각 CoreWindow/ApplicationView가 해당 스레드에서 작동합니다. 여러 UI 스레드에서 작업해야 하는 경우 다중 창 앱이 복잡해질 수 있습니다.

    앱의 기본 보기는 항상 ApplicationView에서 호스트됩니다. 보조 창의 콘텐츠는 ApplicationView 또는 AppWindow에서 호스트할 수 있습니다.

    ApplicationView를 사용하여 앱에 보조 창을 표시하는 방법에 대한 자세한 내용은 [ApplicationView 사용](application-view.md)을 참조하세요.
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow는 생성 시 사용된 것과 동일한 UI 스레드에서 작동하기 때문에 간편하게 다중 창 UWP 앱을 만들 수 있게 해줍니다.

    AppWindow 클래스와 [WindowManagement](/uwp/api/windows.ui.windowmanagement) 네임스페이스의 기타 API는 Windows 10 버전 1903(SDK 18362)부터 사용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우에는 ApplicationView를 사용하여 보조 창을 만들어야 합니다.

    AppWindow를 사용하여 앱에 보조 창을 표시하는 방법에 대한 자세한 내용은 [AppWindow 사용](app-window.md)을 참조하세요.

    > [!NOTE]
    > AppWindow는 현재 미리 보기로 제공됩니다. 따라서 AppWindow를 사용하는 앱을 Microsoft Store에 제출할 수 있지만, 일부 플랫폼 및 프레임워크 구성 요소는 AppWindow에서 작동하지 않는 것으로 알려져 있습니다([제한 사항](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) 참조).
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)(XAML Islands)

     XAML Islands라고도 하는 Win32 앱(HWND 사용)의 UWP XAML 콘텐츠는 DesktopWindowXamlSource에서 호스트됩니다.

    XAML Islands에 대한 자세한 내용은 [데스크톱 애플리케이션에서 UWP XAML 호스팅 API 사용](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)을 참조하세요.

### <a name="make-code-portable-across-windowing-hosts"></a>창 작업 호스트 간에 이식 가능한 코드 만들기

XAML 콘텐츠가 [CoreWindow](/uwp/api/windows.ui.core.corewindow)에 표시되는 경우 항상 연결된 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 및 XAML [Window](/uwp/api/windows.ui.xaml.window)가 있습니다. 해당 클래스의 API를 사용하여 창 범위와 같은 정보를 가져올 수 있습니다. 이 클래스의 인스턴스를 검색하려면 정적 [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 메서드, [ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 메서드 또는 [Window.Current](/uwp/api/windows.ui.xaml.window.current) 속성을 사용합니다. 또한 `GetForCurrentView` 패턴을 사용하여 클래스의 인스턴스를 검색하는 여러 클래스가 있습니다(예: [DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview)).

CoreWindow/ApplicationView의 XAML 콘텐츠 트리는 하나뿐이므로 XAML에서 호스트된 컨텍스트가 CoreWindow/ApplicationView인 것을 알기 때문에 해당 API는 작동합니다.

XAML 콘텐츠가 AppWindow 또는 DesktopWindowXamlSource 내에서 실행되는 경우 여러 XAML 콘텐츠 트리가 동시에 같은 스레드에서 실행될 수 있습니다. 이 경우에는 콘텐츠가 더 이상 현재 CoreWindow/ApplicationView(및 XAML Window)에서 실행되지 않으므로 해당 API가 올바른 정보를 제공하지 않습니다.

모든 창 작업 호스트에서 코드가 올바르게 작동하는지 확인하려면 [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 및 [Window](/uwp/api/windows.ui.xaml.window)를 사용하는 API를 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 클래스에서 컨텍스트를 가져오는 새 API로 대체해야 합니다.
XamlRoot 클래스는 CoreWindow, AppWindow 또는 DesktopWindowXamlSource인지에 관계없이 XAML 콘텐츠 트리와 호스트된 컨텍스트에 대한 정보를 나타냅니다. 이 추상화 계층을 사용하면 XAML이 실행되는 창 작업 호스트에 관계없이 동일한 코드를 작성할 수 있습니다.

다음 표에서는 창 작업 호스트에서 제대로 작동하지 않는 코드 및 대체할 수 있는 새 이식 가능한 코드와 변경할 필요가 없는 몇 가지 API를 보여 줍니다.

| 기존 코드... | 새 코드... |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | 변경하지 않습니다. AppWindow 및 DesktopWindowXamlSource에서 지원됩니다. |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | 변경하지 않습니다. AppWindow 및 DesktopWindowXamlSource에서 지원됩니다. |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | 현재 CoreWindow에 긴밀하게 바인딩된 주 XAML Window 개체를 반환합니다. 이 표 다음에 나오는 참고를 참조하세요. |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | 변경하지 않습니다. AppWindow 및 DesktopWindowXamlSource에서 지원됩니다. |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>XAML Islands 앱에서는 오류가 발생합니다. AppWindow 앱에서는 주 창에 열린 팝업이 반환됩니다. | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> DesktopWindowXamlSource에 있는 XAML 콘텐츠의 경우 스레드에 CoreWindow/Window가 있지만 항상 표시되지 않으며 크기가 1x1로 지정됩니다. 앱에서 액세스할 수는 있지만, 의미 있는 범위나 표시 유형을 반환하지 않습니다.
>
>AppWindow에 있는 XAML 콘텐츠의 경우 항상 동일한 스레드에 정확히 하나의 CoreWindow가 있습니다. `GetForCurrentView` 또는 `GetForCurrentThread` API를 호출하면, API는 스레드에서 실행될 수 있는 AppWindows가 아니라 스레드에 있는 CoreWindow의 상태를 반영하는 개체를 반환합니다.


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- “새 창 열기” 기호를 활용하여 보조 뷰로의 명확한 진입점을 제공합니다.
- 사용자에게 보조 뷰의 목적을 알립니다.
- 앱이 단일 뷰에서 완벽하게 작동하고, 사용자가 편의상 필요한 경우에만 보조 뷰를 열도록 합니다.
- 보조 뷰를 통해 알림이나 다른 임시 시각적 개체를 제공하지 않습니다.

## <a name="related-topics"></a>관련 항목

- [AppWindow 사용](app-window.md)
- [ApplicationView 사용](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
