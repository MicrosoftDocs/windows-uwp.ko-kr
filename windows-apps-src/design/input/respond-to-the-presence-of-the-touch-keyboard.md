---
Description: 터치 키보드를 표시 하거나 숨길 때 응용 프로그램의 UI를 조정 하는 방법에 대해 알아봅니다.
title: 터치 키보드의 현재 상태에 응답
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: 키보드, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 상호 작용
ms.date: 09/24/2020
ms.topic: article
ms.openlocfilehash: 4af7e7533ebd985a22eedd2e11f35d8bf5f5dc8a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216906"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>터치 키보드의 현재 상태에 응답

터치 키보드를 표시 하거나 숨길 때 응용 프로그램의 UI를 조정 하는 방법에 대해 알아봅니다.

### <a name="important-apis"></a>중요 API

- [AutomationPeer](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](/uwp/api/Windows.UI.ViewManagement.InputPane)

![기본 레이아웃 모드의 터치 키보드](images/keyboard/default.png)

<sup>기본 레이아웃 모드의 터치 키보드</sup>

터치 키보드는 터치를 지 원하는 장치에 대 한 텍스트 입력을 사용 하도록 설정 합니다. Windows 앱 텍스트 입력 컨트롤은 사용자가 편집 가능한 입력 필드를 탭 하면 기본적으로 터치 키보드를 호출 합니다. 터치 키보드는 일반적으로 사용자가 폼의 컨트롤 간을 탐색 하는 동안 계속 표시 되지만이 동작은 폼 내의 다른 컨트롤 형식에 따라 달라질 수 있습니다.

표준 텍스트 입력 컨트롤에서 파생 되지 않은 사용자 지정 텍스트 입력 컨트롤에서 해당 터치 키보드 동작을 지원 하려면 <a href="/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">Automationpeer</a> 클래스를 사용 하 여 컨트롤을 Microsoft ui 자동화에 노출 하 고 올바른 UI 자동화 컨트롤 패턴을 구현 해야 합니다. [키보드 접근성](../accessibility/keyboard-accessibility.md) 및 [사용자 지정 자동화 피어](../accessibility/custom-automation-peers.md)를 참조 하세요.

이 지원이 사용자 지정 컨트롤에 추가 되 면 터치 키보드의 유무에 따라 적절 하 게 응답할 수 있습니다.

**사전 요구 사항:**

이 항목은 [키보드 상호 작용](keyboard-interactions.md)을 기반으로 합니다.

표준 키보드 상호 작용, 키보드 입력 및 이벤트 처리 및 UI 자동화를 기본적으로 이해 해야 합니다.

Windows 앱 개발에 익숙하지 않은 경우이 항목을 살펴보고 여기에 설명 된 기술에 대해 알아보세요.

- [첫 번째 앱 만들기](../../get-started/your-first-app.md)
- [이벤트 및 라우트된 이벤트](../../xaml-platform/events-and-routed-events-overview.md) 를 사용 하 여 이벤트에 대해 알아보기 개요

**사용자 환경 지침:**

키보드 입력에 최적화 된 유용한 앱 디자인에 대 한 유용한 팁은 [키보드 상호 작용](./keyboard-interactions.md) 을 참조 하세요.

## <a name="touch-keyboard-and-a-custom-ui"></a>터치 키보드 및 사용자 지정 UI

다음은 사용자 지정 텍스트 입력 컨트롤에 대 한 몇 가지 기본적인 권장 사항입니다.

- 양식과의 전체 상호 작용 전체에서 터치 키보드를 표시 합니다.

- 텍스트 입력의 컨텍스트에서 포커스가 텍스트 입력 필드에서 이동할 때 키보드를 유지 하기 위해 사용자 지정 컨트롤에 적절 한 UI 자동화 [AutomationControlType](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 있는지 확인 합니다. 예를 들어 텍스트 입력 시나리오의 중간에 열리는 메뉴가 있고 키보드를 유지 하려는 경우 메뉴에 **AutomationControlType** of 메뉴가 있어야 합니다.

- 터치 키보드를 제어 하기 위해 UI 자동화 속성을 조작 하지 마세요. 다른 내게 필요한 옵션 도구는 UI 자동화 속성의 정확도를 사용 합니다.

- 사용자가 상호 작용 하는 입력 필드를 항상 볼 수 있는지 확인 합니다.

    터치 키보드는 화면에서 많은 부분을 occludes 때문에, 사용자가 현재 표시 되지 않는 컨트롤을 비롯 하 여 폼의 컨트롤을 탐색할 때 포커스가 있는 입력 필드가 뷰로 스크롤되는 지 확인 합니다.

    UI를 사용자 지정 하는 경우 [**Inputpane**](/uwp/api/Windows.UI.ViewManagement.InputPane) 개체에서 표시 하는 이벤트 [표시](/uwp/api/windows.ui.viewmanagement.inputpane.showing) 및 [숨기기](/uwp/api/windows.ui.viewmanagement.inputpane.hiding) 를 처리 하 여 터치 키보드의 모양에 비슷한 동작을 제공 합니다.

    ![터치 키보드를 표시 하거나 표시 하지 않는 폼](images/touch-keyboard-pan1.png)

    경우에 따라 전체 시간에 화면에 유지 해야 하는 UI 요소가 있습니다. 폼 컨트롤이 패닝 영역에 포함 되 고 중요 한 UI 요소가 고정 되도록 UI를 디자인 합니다. 예를 들어:

    ![항상 보기 상태를 유지 해야 하는 영역을 포함 하는 폼입니다.](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>표시 및 숨기기 이벤트 처리

터치 키보드의 [표시](/uwp/api/windows.ui.viewmanagement.inputpane.showing) 및 [숨기기](/uwp/api/windows.ui.viewmanagement.inputpane.hiding) 이벤트에 대 한 이벤트 처리기를 연결 하는 예제는 다음과 같습니다.

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

## <a name="related-articles"></a>관련된 문서

- [키보드 조작](keyboard-interactions.md)
- [키보드 접근성](../accessibility/keyboard-accessibility.md)
- [사용자 지정 자동화 피어](../accessibility/custom-automation-peers.md)

### <a name="samples"></a>샘플

- [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

### <a name="archive-samples"></a>보관 샘플

- [입력: 터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [화상 키보드 샘플의 모양에 대 한 응답](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [XAML 텍스트 편집 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
- [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
