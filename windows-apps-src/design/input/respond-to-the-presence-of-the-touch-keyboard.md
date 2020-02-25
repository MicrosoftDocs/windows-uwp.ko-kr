---
Description: 터치 키보드를 표시하거나 숨길 때 앱의 UI를 세부 조정하는 방법을 알아봅니다.
title: 터치 키보드의 현재 상태에 응답
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: 키보드, 손쉬운 사용, 탐색, 포커스, 텍스트, 입력, 사용자 조작
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: e26bbe00bba8b3d91d7ee842cb4d9c984a941f2b
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521364"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>터치 키보드의 현재 상태에 응답

터치 키보드를 표시하거나 숨길 때 앱의 UI를 세부 조정하는 방법을 알아봅니다.

### <a name="important-apis"></a>중요 API

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![기본 레이아웃 모드의 터치 키보드](images/keyboard/default.png)

<sup>기본 레이아웃 모드의 터치 키보드</sup>

터치 키보드는 터치 지원 디바이스의 텍스트 입력을 가능하게 합니다. 사용자가 편집 가능한 입력 필드를 탭하면 UWP(유니버설 Windows 플랫폼) 텍스트 입력 컨트롤은 기본적으로 터치 키보드를 호출합니다. 터치 키보드는 일반적으로 사용자가 양식에서 컨트롤 사이를 이동하는 동안 계속 표시되지만, 이 동작은 양식 내의 다른 컨트롤 유형에 따라 다를 수 있습니다.

표준 텍스트 입력 컨트롤에서 파생되지 않은 사용자 지정 텍스트 입력 컨트롤에서 해당 터치 키보드 동작을 지원하려면 <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> 클래스를 사용하여 컨트롤을 Microsoft UI 자동화에 노출하고 올바른 UI 자동화 컨트롤 패턴을 구현해야 합니다. [키보드 접근성](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility) 및 [사용자 지정 자동화 피어](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers)를 참조하세요.

이 지원이 사용자 지정 컨트롤에 추가되고 나면 터치 키보드의 존재에 적절하게 응답할 수 있습니다.

**사전**

이 항목은 [키보드 조작](keyboard-interactions.md)을 기반으로 합니다.

표준 키보드 조작, 키보드 입력 처리, UI 자동화에 대한 기본적인 지식이 있어야 합니다.

UWP(유니버설 Windows 플랫폼) 앱을 처음 개발하는 경우 다음 항목을 검토하여 여기서 설명하는 기술에 대해 알아보세요.

- [첫 번째 앱 만들기](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- 이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)를 참조하세요.

**사용자 환경 지침:**

키보드 입력에 최적화 된 유용한 앱 디자인에 대 한 유용한 팁은 [키보드 상호 작용](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions) 을 참조 하세요.

## <a name="touch-keyboard-and-a-custom-ui"></a>터치 키보드 및 사용자 지정 UI

사용자 지정 텍스트 입력 컨트롤에 대한 몇 가지 기본 권장 사항은 다음과 같습니다.

- 전체 양식 조직에서 터치 키보드를 표시합니다.

- 텍스트 입력의 컨텍스트에서 포커스가 텍스트 입력 필드에서 이동할 때 키보드를 유지 하기 위해 사용자 지정 컨트롤에 적절 한 UI 자동화 [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 있는지 확인 합니다. 예를 들어 텍스트 입력 시나리오 중간에 열린 메뉴가 있으며 키보드를 지속하려는 경우 메뉴에 **AutomationControlType** 메뉴가 있어야 합니다.

- UI 자동화 속성을 조작하여 터치 키보드를 제어하지는 않도록 합니다. 다른 접근성 도구는 UI 자동화 속성의 정확도에 의존합니다.

- 사용자가 조작 중인 입력 필드를 항상 볼 수 있도록 합니다.

    터치 키보드는 화면의 많은 부분을 가리기 때문에 UWP에서는 사용자가 현재 보기에 없는 컨트롤을 포함하여 양식의 컨트롤을 탐색할 때 포커스가 있는 입력 필드가 보기로 스크롤되도록 합니다.

    UI를 사용자 지정 하는 경우 [**Inputpane**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane) 개체에서 표시 하는 이벤트 [표시](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) 및 [숨기기](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) 를 처리 하 여 터치 키보드의 모양에 비슷한 동작을 제공 합니다.

    ![터치 키보드가 표시된 양식 및 표시되지 않은 양식](images/touch-keyboard-pan1.png)

    경우에 따라 전체 시간 동안 화면에 유지해야 하는 UI 요소가 있습니다. 양식 컨트롤은 이동 영역에 포함되고 중요한 UI 요소는 고정되도록 UI를 디자인해 보세요. 예를 들면 다음과 같습니다.

    ![뷰에 항상 유지되어야 하는 영역이 있는 양식](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Showing 이벤트 및 Hiding 이벤트 처리

터치 키보드의 [표시](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) 및 [숨기기](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) 이벤트에 대 한 이벤트 처리기를 연결 하는 예제는 다음과 같습니다.

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>관련 문서

- [키보드 상호 작용](keyboard-interactions.md)
- [키보드 접근성](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)
- [사용자 지정 자동화 피어](https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers)

**샘플**

- [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

**보관 샘플**

- [입력: 터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [화상 키보드 샘플의 모양에 대 한 응답](https://code.msdn.microsoft.com/windowsapps/keyboard-events-sample-866ba41c)
- [XAML 텍스트 편집 샘플](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)
- [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
