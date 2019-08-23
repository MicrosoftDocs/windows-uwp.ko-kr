---
Description: 별도의 창에서 앱의 서로 다른 부분을 봅니다.
title: 앱에 대한 여러 보기 표시
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729514"
---
# <a name="show-multiple-views-for-an-app"></a>앱에 대한 여러 보기 표시

![여러 창으로 앱을 보여 주는 와이어프레임](images/multi-view.gif)

개별 창에서 앱의 독립 부분을 볼 수 있도록 하여 사용자의 생산성을 높이는 데 도움을 줍니다. 앱에 대 한 창을 여러 개 만들면 작업 표시줄에 각 창이 별도로 표시 됩니다. 사용자는 앱 창을 독립적으로 이동, 크기 조정, 표시 및 숨길 수 있으며, 별도의 앱인 것처럼 앱 창 간에 전환할 수 있습니다.

> **중요 API**: [Windows. viewmanagement 네임 스페이스](/uwp/api/windows.ui.viewmanagement), [windows.](/uwp/api/windows.ui.windowmanagement) i n i 관리 네임 스페이스

## <a name="when-should-an-app-use-multiple-views"></a>앱에서 언제 여러 보기를 사용해야 하나요?

여러 뷰를 활용할 수 있는 다양 한 시나리오가 있습니다. 다음은 몇 가지 예입니다.

- 사용자가 받은 메시지 목록을 보면서 새 이메일을 작성할 수 있는 이메일 앱
- 사용자가 여러 사람의 연락처 정보를 나란히 비교할 수 있는 주소록 앱
- 사용자가 현재 재생 중인 음악과 재생 가능한 음악의 목록을 동시에 볼 수 있는 음악 플레이어 앱
- 사용자가 노트의 한 페이지에서 다른 페이지로 정보를 복사할 수 있는 메모 작성 앱
- 사용자가 모든 상위 헤드라인을 정독한 후 나중에 읽을 수 있도록 여러 기사를 열어 둘 수 있는 읽기 앱

각 앱 레이아웃은 고유하지만, 콘텐츠의 오른쪽 위 모서리와 같은 예측 가능한 위치에 "새 창" 단추를 배치하여 새 창으로 열 수 있도록 하는 것이 좋습니다. 또한 [상황에 맞는 메뉴](../controls-and-patterns/menus.md) 옵션을 "새 창에서 열기"로 포함 하는 것이 좋습니다.

동일한 인스턴스에 대해 별도의 창이 아니라 앱의 개별 인스턴스를 만들려면 [다중 인스턴스 UWP 앱 만들기](../../launch-resume/multi-instance-uwp.md)를 참조 하세요.

## <a name="windowing-hosts"></a>창 고 호스트

앱 내에서 UWP 콘텐츠를 호스트 하는 방법에는 여러 가지가 있습니다.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)

     앱 보기는 앱이 콘텐츠를 표시하는 데 사용하는 창과 스레드의 1:1 연결입니다. 앱을 시작할 때 생성되는 첫 번째 보기를 *기본 보기*라고 합니다. 각 CoreWindow/ApplicationView는 자체 스레드에서 작동 합니다. 여러 UI 스레드에서 작업 해야 하는 경우 다중 창 앱이 복잡 해질 수 있습니다.

    앱에 대 한 기본 보기는 항상 ApplicationView에서 호스팅됩니다. 보조 창의 콘텐츠는 ApplicationView 또는 AppWindow에서 호스팅될 수 있습니다.

    응용 프로그램에서 ApplicationView를 사용 하 여 보조 창을 표시 하는 방법을 알아보려면 [Applicationview 사용](application-view.md)을 참조 하세요.
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow는 생성 된 것과 동일한 UI 스레드에서 작동 하기 때문에 다중 창 UWP 앱 만들기를 간소화 합니다.

    [Windowmanagement](/uwp/api/windows.ui.windowmanagement) 네임 스페이스의 appwindow 클래스 및 기타 Api는 Windows 10 버전 1903 (SDK 18362)부터 사용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 ApplicationView를 사용 하 여 보조 창을 만들어야 합니다.

    앱에서 AppWindow를 사용 하 여 보조 창을 표시 하는 방법을 알아보려면 [Appwindow 사용](app-window.md)을 참조 하세요.

    > [!NOTE]
    > AppWindow는 현재 미리 보기로 제공 됩니다. 즉, AppWindow를 사용 하는 앱을 저장소에 제출할 수 있지만, 일부 플랫폼 및 프레임 워크 구성 요소는 AppWindow에서 작동 하지 않는 것으로 알려져 있습니다 ( [제한 사항](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)참조).
- [Desktopwindowxamlsource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (XAML 제도)

     XAML 아일랜드 라고도 하는 Win32 응용 프로그램의 UWP XAML 콘텐츠는 DesktopWindowXamlSource에서 호스팅됩니다.

    XAML 아일랜드에 대 한 자세한 내용은 [데스크톱 응용 프로그램에서 UWP XAML 호스팅 API 사용](/windows/apps/desktop/modernize/using-the-xaml-hosting-api) 을 참조 하세요.

### <a name="make-code-portable-across-windowing-hosts"></a>창 고 호스트에서 코드를 이식 가능 하 게 만들기

XAML 콘텐츠가 [CoreWindow](/uwp/api/windows.ui.core.corewindow)에 표시 되는 경우 항상 연결 된 [APPLICATIONVIEW](/uwp/api/windows.ui.viewmanagement.applicationview) 및 XAML [창이](/uwp/api/windows.ui.xaml.window)있습니다. 이러한 클래스에서 Api를 사용 하 여 창 범위와 같은 정보를 가져올 수 있습니다. 이러한 클래스의 인스턴스를 검색 하려면 정적 [CoreWindow](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 또는 [GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 메서드를 사용 하거나, [Current](/uwp/api/windows.ui.xaml.window.current) 속성을 사용 합니다. 또한 `GetForCurrentView` 패턴을 사용 하 여 클래스의 인스턴스를 검색 하는 클래스 (예: [DisplayInformation. GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview))가 많습니다.

이러한 Api는 CoreWindow/ApplicationView에 대해 XAML 콘텐츠 트리가 하나 뿐 이므로 XAML에서 호스팅된 컨텍스트가 CoreWindow/ApplicationView 이기 때문에 작동 합니다.

XAML 콘텐츠가 AppWindow 또는 DesktopWindowXamlSource 내에서 실행 되는 경우 여러 XAML 콘텐츠 트리가 동시에 같은 스레드에서 실행 될 수 있습니다. 이 경우 현재 CoreWindow/ApplicationView (및 XAML 창) 내에서 콘텐츠가 더 이상 실행 되지 않으므로 이러한 Api는 올바른 정보를 제공 하지 않습니다.

모든 창 작업 호스트에서 코드가 올바르게 작동 하는지 확인 하려면 [CoreWindow](/uwp/api/windows.ui.core.corewindow), [Applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)및 [Window](/uwp/api/windows.ui.xaml.window) 를 사용 하는 api를 [xamlroot](/uwp/api/windows.ui.xaml.xamlroot) 클래스에서 컨텍스트를 가져오는 새 api로 바꾸어야 합니다.
XamlRoot 클래스는 CoreWindow, AppWindow 또는 DesktopWindowXamlSource 인지 여부에 관계 없이 XAML 콘텐츠의 트리와 호스팅되는 컨텍스트에 대 한 정보를 나타냅니다. 이 추상화 계층을 사용 하면 XAML이 실행 되는 창 고 호스트에 관계 없이 동일한 코드를 작성할 수 있습니다.

다음 표에서는 창 작업 호스트에서 제대로 작동 하지 않는 코드 및이를 대체할 수 있는 새로운 이식 가능한 코드와 변경할 필요가 없는 일부 Api를 보여 줍니다.

| 다음을 사용 하는 경우 ... | 바꿀 내용 ... |
| - | - |
| CoreWindow. GetForCurrentThread (). [경계](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_. XamlRoot. [크기](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow. GetForCurrentThread (). [SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_. XamlRoot. [변경 됨](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow. [표시](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_. XamlRoot. [Ishostvisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow. [VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_. XamlRoot. [변경 됨](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow. GetForCurrentThread (). [Getkeystate](/uwp/api/windows.ui.core.corewindow.getkeystate) | 없이. AppWindow 및 DesktopWindowXamlSource에서 지원 됩니다. |
| CoreWindow. GetForCurrentThread (). [Getasynckeystate](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | 없이. AppWindow 및 DesktopWindowXamlSource에서 지원 됩니다. |
| 창인. [현재](/uwp/api/windows.ui.xaml.window.current) | 현재 CoreWindow에 긴밀 하 게 바인딩되는 주 XAML 창 개체를 반환 합니다. 이 표 다음에 나오는 참고를 참조 하세요. |
| 창. 현재. [경계](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_. XamlRoot. [크기](/uwp/api/windows.ui.xaml.xamlroot.size) |
| 창. 현재. [콘텐츠](/uwp/api/windows.ui.xaml.window.content) | UIElement root = _uielement_ XamlRoot. [콘텐츠](/uwp/api/windows.ui.xaml.xamlroot.content) |
| 창. 현재. [Compositor](/uwp/api/windows.ui.xaml.window.compositor) | 없이. AppWindow 및 DesktopWindowXamlSource에서 지원 됩니다. |
| VisualTreeHelper. [Getopenpopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>XAML 아일랜드 앱에서 오류가 발생 합니다. AppWindow 앱에서 주 창에 열린 팝업이 반환 됩니다. | VisualTreeHelper. [GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot) (_uiElement_. XamlRoot) |
| System.windows.input.focusmanager>. [System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | System.windows.input.focusmanager>. [System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_) (_uiElement_. XamlRoot) |
| contentDialog. ShowAsync () | contentDialog. [Xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot)  =  _uiElement_. XamlRoot;<br/>contentDialog. ShowAsync (); |
| menuFlyout 아웃. ShowAt (null, new Point (10, 10)); | menuFlyout 아웃입니다. [Xamlroot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot)  =  _uiElement_. XamlRoot;<br/>menuFlyout 아웃. ShowAt (null, new Point (10, 10)); |

> [!NOTE]
> DesktopWindowXamlSource의 XAML 콘텐츠에 대해 스레드에 CoreWindow/Window가 있지만 항상 표시 되지 않으며 크기는 1x1입니다. 여전히 앱에 액세스할 수 있지만 의미 있는 범위 또는 표시 유형을 반환 하지 않습니다.
>
>AppWindow의 XAML 콘텐츠에 대해서는 항상 동일한 스레드에 정확히 하나의 CoreWindow 있습니다. `GetForCurrentView` 또는`GetForCurrentThread` api를 호출 하는 경우 해당 api는 해당 스레드에서 실행 될 수 있는 appwindows가 아니라 스레드의 CoreWindow 상태를 반영 하는 개체를 반환 합니다.


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- "새 창 열기" 기호를 활용하여 보조 보기로의 깔끔한 진입점을 제공합니다.
- 사용자에게 보조 보기 목적을 알립니다.
- 앱이 단일 뷰에서 완전히 작동 하는지 확인 하 고 사용자가 편의를 위해서만 보조 뷰를 엽니다.
- 알림이나 기타 일시적인 시각 효과를 제공하기 위해 보조 보기에 의존하지 마십시오.

## <a name="related-topics"></a>관련 항목

- [AppWindow 사용](app-window.md)
- [ApplicationView 사용](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
