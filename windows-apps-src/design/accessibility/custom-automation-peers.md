---
Description: Microsoft UI 자동화의 자동화 피어 개념과 고유한 사용자 지정 UI 클래스에 대해 자동화 지원을 제공하는 방법을 설명합니다.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: 사용자 지정 자동화 피어
label: Custom automation peers
template: detail.hbs
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6607038371bdbf1823eec51cfd7884ebc1956197
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339438"
---
# <a name="custom-automation-peers"></a>사용자 지정 자동화 피어  

Microsoft UI 자동화의 자동화 피어 개념과 고유한 사용자 지정 UI 클래스에 대해 자동화 지원을 제공하는 방법을 설명합니다.

UI 자동화는 자동화 클라이언트에서 다양한 UI 플랫폼 및 프레임워크의 사용자 인터페이스를 검사하거나 조작하는 데 사용할 수 있는 프레임워크를 제공합니다. UWP(유니버설 Windows 플랫폼) 앱을 작성하는 경우 UI에 사용하는 클래스는 이미 UI 자동화 지원을 제공합니다. 기존의 봉인되지 않은 클래스에서 파생시켜 새로운 종류의 UI 컨트롤 또는 지원 클래스를 정의할 수 있습니다. 이 작업을 수행하는 동안 클래스에서 접근성을 지원해야 하지만 기본 UI 자동화 지원에 포함되지 않는 동작을 추가할 수 있습니다. 이 경우 기본 구현에서 사용된 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 클래스에서 파생시키고 피어 구현에 필요한 지원을 추가한 다음 새 피어를 만들도록 UWP(유니버설 Windows 플랫폼) 컨트롤 인프라에 알려 기존 UI 자동화 지원을 확장해야 합니다.

UI 자동화를 통해 화면 읽기 프로그램과 같은 접근성 응용 프로그램 및 보조 기술뿐만 아니라 품질 보증(테스트) 코드도 사용할 수 있습니다. 두 시나리오에서 UI 자동화 클라이언트는 사용자 인터페이스 요소를 검사하고 앱 외부의 다른 코드에서 사용자의 앱 조작을 시뮬레이트할 수 있습니다. 모든 플랫폼에서의 UI 자동화 및 더 넓은 의미의 UI 자동화에 대한 자세한 내용은 [UI 자동화 개요](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-uiautomationoverview)를 참조하세요.

UI 자동화 프레임워크를 사용하는 두 개의 대상 그룹이 있습니다.

* **UI 자동화 *클라이언트*** 는 UI 자동화 API를 호출하여 현재 사용자에게 표시되는 모든 UI에 대해 알아봅니다. 예를 들어 화면 읽기 프로그램 같은 보조 기술은 UI 자동화 클라이언트 역할을 합니다. UI는 관련된 자동화 요소의 트리로 제공됩니다. UI 자동화 클라이언트가 한 번에 하나의 앱에만 관련이 있거나 전체 트리에 관련이 있을 수 있습니다. UI 자동화 클라이언트는 UI 자동화 API를 사용하여 트리를 탐색하고 자동화 요소의 정보를 읽거나 변경할 수 있습니다.
* **UI 자동화 *공급자*** 는 앱의 일부로 도입한 요소를 UI에 표시하는 API를 구현하여 UI 자동화 트리에 정보를 제공합니다. 이제 새 컨트롤을 만들 때 사용자는 UI 자동화 공급자 시나리오에서 참가자 역할을 해야 합니다. 공급자로서 사용자는 모든 UI 자동화 클라이언트가 UI 자동화 프레임워크를 사용하여 접근성 및 테스트 용도로 컨트롤을 조작할 수 있도록 해야 합니다.

일반적으로 UI 자동화 프레임워크에는 각각 UI 자동화 클라이언트용 API와 유사한 이름의 UI 자동화용 API 등 병렬 API가 있습니다. 이 항목의 대부분에서는 UI 자동화 공급자용 API와 특히 이 UI 프레임워크에서 공급자 확장성을 가능하게 하는 클래스 및 인터페이스에 대해 다룹니다. 특정 관점을 제공하거나 클라이언트와 공급자 API를 연결하는 조회 테이블을 제공하기 위해 UI 자동화 클라이언트가 사용하는 UI 자동화 API를 언급하는 경우도 있습니다. 클라이언트 관점에 대한 자세한 내용은 [UI 자동화 클라이언트 프로그래머 가이드](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-clientportal)를 참조하세요.

> [!NOTE]
> UI 자동화 클라이언트는 일반적으로 관리 코드를 사용하지 않으며 대개 UWP 앱으로 구현되지 않습니다(일반적으로 데스크톱 앱). UI 자동화는 표준을 기반으로 하는 것이지 특정 구현이나 프레임워크를 기반으로 하는 것이 아닙니다. 많은 기존 UI 자동화 클라이언트(화면 읽기 프로그램과 같은 보조 기술 제품 포함)는 COM(구성 요소 개체 모델) 인터페이스를 사용하여 UI 자동화, 시스템 및 하위 창에서 실행되는 앱을 조작합니다. COM 인터페이스 및 COM을 사용하여 UI 자동화 클라이언트를 작성하는 방법에 대한 자세한 내용은 [UI 자동화 기본 사항](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview)을 참조하세요.

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>사용자 지정 UI 클래스에 대한 UI 자동화 지원의 기존 상태 확인  
사용자 지정 컨트롤에 대한 자동화 피어를 구현하기 전에 기본 클래스 및 해당 자동화 피어에서 필요한 접근성이나 자동화 지원을 이미 제공하는지 여부를 테스트해야 합니다. 대부분의 경우 [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) 구현, 특정 피어 및 해당 피어에서 구현하는 패턴 조합을 사용하면 기본적이지만 만족스러운 접근성 환경을 제공할 수 있습니다. 이 사항이 참인지 여부는 컨트롤 및 기본 클래스에 대한 개체 모델 노출을 변경한 횟수에 따라 달라집니다. 또한 기본 클래스 기능에 대한 추가 기능이 템플릿 계약의 새 UI 요소 또는 컨트롤의 시각적 모양과 상호 관련되는지 여부에 따라 달라집니다. 경우에 따라 변경 결과로 추가 접근성 지원이 필요한 사용자 환경의 새로운 측면이 도입될 수 있습니다.

기존의 기본 피어 클래스를 사용해도 기본 접근성 지원이 지원되지만 자동화된 테스트 시나리오를 위해 UI 자동화에 정확한 **ClassName** 정보를 보고할 수 있도록 피어를 정의하는 것이 좋습니다. 이 고려 사항은 타사에서 사용할 컨트롤을 작성하는 경우에 특히 중요합니다.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>자동화 피어 클래스  
UWP는 Windows Forms, WPF(Windows Presentation Foundation) 및 Microsoft Silverlight 같은 이전 관리 코드 UI 프레임워크에서 사용한 기존 UI 자동화 기술 및 규칙을 기반으로 합니다. 대부분의 컨트롤 클래스와 해당 기능 및 용도도 이전 UI 프레임워크에서 비롯됩니다.

규칙에 따라 피어 클래스 이름은 컨트롤 클래스 이름으로 시작하고 "AutomationPeer"로 끝납니다. 예를 들어 [**ButtonAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.ButtonAutomationPeer)는 [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 컨트롤 클래스의 피어 클래스입니다.

> [!NOTE]
> 이 항목에서는 컨트롤 피어를 구현할 때 더 중요한 접근성 관련 속성을 처리합니다. 그러나 UI 자동화 지원의 보다 일반적인 개념에서는 [UI 자동화 공급자 프로그래머 가이드](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-providerportal) 및 [UI 자동화 기본 사항](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview)에 문서화된 권장 사항에 따라 피어를 구현해야 합니다. 이러한 항목에서는 UI 자동화용 UWP 프레임워크의 정보를 제공하는 데 사용하는 특정 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) API에 대해 설명하지 않지만 클래스를 식별하거나 다른 정보 또는 조작을 제공하는 속성을 설명합니다.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>피어, 패턴 및 컨트롤 형식  
*컨트롤 패턴*은 컨트롤 기능의 특정 측면을 UI 자동화 클라이언트에 표시하는 인터페이스 구현입니다. UI 자동화 클라이언트는 컨트롤 패턴을 통해 표시된 속성 및 메서드를 사용하여 컨트롤의 기능에 대한 정보를 검색하고 런타임에 컨트롤의 동작을 조작합니다.

컨트롤 패턴은 컨트롤 형식이나 컨트롤 모양에 독립적으로 컨트롤 기능을 분류하고 표시하는 방법을 제공합니다. 예를 들어 표 형식 인터페이스를 제공하는 컨트롤은 **Grid** 컨트롤 패턴을 사용하여 표의 행 및 열 수를 표시하고 UI 자동화 클라이언트를 사용하여 해당 표에서 항목을 검색합니다. 다른 예로, UI 자동화 클라이언트는 단추와 같이 호출할 수 있는 컨트롤에는 **Invoke** 컨트롤 패턴을 사용하고 목록 상자, 목록 보기 또는 콤보 상자와 같이 스크롤 막대가 있는 컨트롤에는 **Scroll** 컨트롤 패턴을 사용할 수 있습니다. 각 컨트롤 패턴은 개별 기능 형식을 나타내며, 컨트롤 패턴을 결합하여 특정 컨트롤이 지원하는 전체 기능 집합을 설명할 수 있습니다.

컨트롤 패턴이 UI에 연결된 방식은 인터페이스가 COM 개체에 연결된 방식과 같습니다. COM에서 개체를 쿼리하여 지원하는 인터페이스를 확인한 다음 해당 인터페이스를 사용하여 기능에 액세스할 수 있습니다. UI 자동화에서 UI 자동화 클라이언트는 UI 자동화 요소를 쿼리하여 지원되는 컨트롤 패턴을 확인한 다음 지원되는 컨트롤 패턴이 표시하는 속성, 메서드, 이벤트 및 구조를 통해 요소 및 피어 컨트롤을 조작할 수 있습니다.

자동화 피어의 주요 용도 중 하나는 피어를 통해 UI 요소에서 지원할 수 있는 패턴을 제어하는 UI 자동화 클라이언트에 보고하는 것입니다. 이 작업을 하기 위해 UI 자동화 공급자는 [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 메서드를 재정의하여 [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 메서드 동작을 변경하는 새 피어를 구현합니다. UI 자동화 클라이언트는 UI 자동화 공급자가 **GetPattern** 호출에 매핑하는 호출을 수행합니다. UI 자동화 클라이언트는 조작할 각 특정 패턴을 쿼리합니다. 피어에서 패턴을 지원하면 패턴 자체에 대한 개체 참조를 반환하고, 그렇지 않으면 **null**을 반환합니다. 반환 값이 **null**이 아닌 경우 UI 자동화 클라이언트는 컨트롤 패턴을 조작하기 위해 패턴 인터페이스의 API를 클라이언트로 호출할 수 있는 것으로 예상합니다.

*컨트롤 형식*은 피어가 나타내는 컨트롤의 기능을 광범위하게 정의하는 방법입니다. 컨트롤 패턴과는 다른 개념입니다. 패턴은 특정 인터페이스를 통해 가져올 수 있는 정보나 수행할 수 있는 작업을 UI 자동화에 알리고 컨트롤 형식은 그보다 한 수준 위에 존재합니다. 각 컨트롤 형식에는 UI 자동화의 다음과 같은 측면에 대한 지침이 있습니다.

* UI 자동화 컨트롤 패턴: 컨트롤 형식은 각각 서로 다른 정보 분류 또는 상호 작용을 나타내는 둘 이상의 패턴을 지원할 수 있습니다. 각 컨트롤 형식에는 컨트롤이 지원해야 하는 컨트롤 패턴 집합, 선택적인 패턴 집합, 컨트롤이 지원해서는 안 되는 패턴 집합이 있습니다.
* UI 자동화 속성 값: 각 컨트롤 형식에는 컨트롤에서 지원 해야 하는 속성 집합이 있습니다. 이는 패턴 특정 속성이 아니라 [UI 자동화 속성 개요](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-propertiesoverview)에 설명된 일반 속성입니다.
* UI 자동화 이벤트: 각 컨트롤 형식에는 컨트롤에서 지원 해야 하는 이벤트 집합이 있습니다. 이 또한 패턴 특정 이벤트가 아니라 [UI 자동화 이벤트 개요](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-eventsoverview)에 설명된 일반 이벤트입니다.
* UI 자동화 트리 구조: 각 컨트롤 형식은 UI 자동화 트리 구조에서 컨트롤이 표시 되는 방법을 정의 합니다.

프레임워크에 대한 자동화 피어 구현 방법과 관계없이 UI 자동화 클라이언트 기능은 UWP에 연결되지 않으며, 실제로 보조 기술 등의 기존 UI 자동화 클라이언트는 COM과 같은 다른 프로그래밍 모델을 사용하는 경우가 많습니다. COM에서 클라이언트는 요청된 패턴을 구현하는 COM 컨트롤 패턴 인터페이스나 속성, 이벤트 또는 트리 검사용 일반 UI 자동화 프레임워크에 대해 **QueryInterface**를 실행할 수 있습니다. 패턴에 대해 UI 자동화 프레임워크에서 앱의 UI 자동화 공급자 및 관련 피어에 대해 실행 중인 UWP 코드로 해당 인터페이스 코드를 마샬링합니다.

C @ no__t-0 또는 Microsoft Visual Basic를 사용 하 여 UWP 앱과 같은 관리 코드 프레임 워크에 대 한 컨트롤 패턴을 구현 하는 경우 COM 인터페이스 표현을 사용 하는 대신 .NET Framework 인터페이스를 사용 하 여 이러한 패턴을 나타낼 수 있습니다. 예를 들어 **Invoke** 패턴의 Microsoft .NET 공급자 구현을 위한 UI 자동화 패턴 인터페이스는 [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider)입니다.

컨트롤 패턴, 공급자 인터페이스 및 해당 용도 목록은 [컨트롤 패턴 및 인터페이스](control-patterns-and-interfaces.md)를 참조하세요. 컨트롤 형식 목록을 보려면 [UI 자동화 컨트롤 형식 개요](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-controltypesoverview)를 참조하세요.

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>컨트롤 패턴 구현 방법에 대한 지침  
컨트롤 패턴 및 컨트롤 패턴의 용도는 UI 자동화 프레임워크의 광범위한 정의에 포함되며 UWP 앱에 대한 접근성 지원에만 적용되지는 않습니다. 컨트롤 패턴을 구현할 때는 MSDN 및 UI 자동화 사양에서 설명하는 지침과 일치하는 방식으로 구현해야 합니다. 지침을 찾으려는 경우 일반적으로 MSDN 항목을 사용하면 되며 사양을 참조할 필요는 없습니다. 각 패턴에 대 한 지침은 여기에 설명 되어 있습니다. [UI 자동화 컨트롤 패턴 구현](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) 이 영역의 각 항목에는 "구현 지침 및 규칙" 섹션과 "필수 구성원" 섹션이 있습니다. 이 지침은 일반적으로 [공급자용 컨트롤 패턴 인터페이스](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-cpinterfaces) 참조에서 관련 컨트롤 패턴 인터페이스의 특정 API를 참조합니다. 이 인터페이스는 기본/COM 인터페이스이고, 해당 API는 COM-스타일 구문을 사용합니다. 하지만 여기 표시되는 모든 항목은 [**Windows.UI.Xaml.Automation.Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider) 네임스페이스에 동등한 항목이 있습니다.

기본 자동화 피어를 사용 중이며 해당 동작을 확장하는 경우 피어가 이미 UI 자동화 지침에 따라 작성되어 있습니다. 피어가 컨트롤 패턴을 지원하는 경우 [UI 자동화 컨트롤 패턴 구현](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)의 지침에 따른 패턴 지원에 의존할 수 있습니다. 컨트롤 피어가 UI 자동화에 정의된 컨트롤 형식을 나타낸다고 보고하는 경우 이 피어는 [UI 자동화 컨트롤 형식 지원](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes)에 나온 지침을 따른 것입니다.

그렇지만 피어 구현의 UI 자동화 권장 사항을 따르기 위해서는 컨트롤 패턴이나 컨트롤 형식에 대한 추가 지침이 필요할 수도 있습니다. UWP 컨트롤에서 아직 기본 구현으로 존재하지 않는 패턴 또는 컨트롤 유형 지원을 구현하는 경우 특히 그럴 수 있습니다. 예를 들어 주석용 패턴이 기본 XAML 컨트롤에서 구현되어 있지 않습니다. 그런데 주석을 광범위하게 사용하는 앱이 있으며 이에 따라 해당 기능을 액세스 가능하게 만들어야 할 수 있습니다. 이 시나리오의 경우 피어는 [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider)를 구현해야 하며 문서에서 주석을 지원한다는 것을 나타내는 적합한 속성을 사용하여 자신을 **Document** 컨트롤 형식으로 보고해야 합니다.

[UI 자동화 컨트롤 패턴 구현](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)의 패턴 또는 [UI 자동화 컨트롤 형식 지원](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes)의 컨트롤 형식에서 나오는 지침을 안내 및 일반 지침으로 사용하는 것이 좋습니다. API의 목적에 대한 설명을 보기 위해 일부 API 링크를 따를 수도 있습니다. 하지만 UWP 앱 프로그래밍에 필요한 구문 세부 사항을 보려면 [**Windows.UI.Xaml.Automation.Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider) 네임스페이스에서 동등한 API를 찾아 해당 참조 페이지에서 자세한 내용을 참조하세요.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>기본 제공 자동화 피어 클래스  
일반적으로 요소는 사용자의 UI 작업을 허용하거나 앱의 대화형 UI 또는 의미 있는 UI를 나타내는 지원 기술의 사용자에게 필요한 정보를 포함하는 경우에 자동화 피어 클래스를 구현합니다. 일부 UWP 시각적 요소에는 자동화 피어가 없습니다. 자동화 피어를 구현하는 클래스의 예로는 [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 및 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)가 있습니다. 자동화 피어를 구현하지 않는 클래스의 예로는 [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)와 [**Panel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel)을 기반으로 하는 클래스(예제: [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) 및 [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas))가 있습니다. **Panel**은 시각적인 레이아웃 동작만 제공하기 때문에 피어를 포함하지 않습니다. 사용자가 **Panel**을 조작할 수 있는 접근성 관련 방법은 없습니다. 대신 **Panel**에 포함된 모든 자식 요소는 피어 또는 요소 표현이 있는 트리에서 다음으로 사용 가능한 부모의 자식 요소로 UI 자동화 트리에 보고됩니다.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>UI 자동화 및 UWP 프로세스 경계  
일반적으로 UWP 앱에 액세스하는 UI 자동화 클라이언트 코드는 Out of Process로 실행됩니다. UI 자동화 프레임워크 인프라를 사용하면 정보가 프로세스 경계를 넘어갈 수 있습니다. 이 개념은 [UI 자동화 기본 사항](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview)에서 자세히 설명합니다.

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
[  **UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)에서 파생되는 모든 클래스에는 보호된 가상 메서드 [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)가 포함됩니다. 자동화 피어의 개체 초기화 시퀀스에서는 **OnCreateAutomationPeer**를 호출하여 각 컨트롤에 대한 자동화 피어 개체를 가져와 런타임에 사용할 UI 자동화 트리를 구성합니다. UI 자동화 코드는 피어를 사용하여 컨트롤의 특징 및 기능에 대한 정보를 가져오고 해당 컨트롤 패턴을 통해 대화형 사용을 시뮬레이트합니다. 자동화를 지원하는 사용자 지정 컨트롤은 **OnCreateAutomationPeer**를 재정의하고 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)에서 파생되는 클래스의 인스턴스를 반환해야 합니다. 예를 들어 사용자 지정 컨트롤이 [**ButtonBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) 클래스에서 파생되는 경우 **OnCreateAutomationPeer**에서 반환하는 개체는 [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer)에서 파생되어야 합니다.

사용자 지정 컨트롤 클래스를 작성 중이며 새 자동화 피어도 제공하려는 경우 피어의 새 인스턴스를 반환하도록 사용자 지정 컨트롤에 대한 [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 메서드를 재정의해야 합니다. 피어 클래스는 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)에서 직접 또는 간접적으로 파생되어야 합니다.

예를 들어 다음 코드는 사용자 지정 컨트롤 `NumericUpDown`이 UI 자동화를 위해 `NumericUpDownPeer` 피어를 사용하도록 선언합니다.

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> [  **OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 구현에서는 사용자 지정 자동화 피어의 새 인스턴스를 초기화하고, 호출하는 컨트롤을 소유자로서 전달하고, 해당 인스턴스를 반환하는 것 외의 다른 작업은 수행하지 않아야 합니다. 이 메서드에 다른 논리를 추가하지 마세요. 특히, 잠재적으로 동일한 호출 내에서 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)를 소멸시킬 수 있는 모든 논리는 예기치 않은 런타임 동작을 발생시킬 수 있습니다.

[  **OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)의 일반적인 구현에서는 메서드 재정의 범위가 나머지 컨트롤 클래스 정의와 동일하기 때문에 *owner*가 **this** 또는 **Me**로 지정됩니다.

실제 피어 클래스 정의는 컨트롤과 동일한 코드 파일이나 별도의 코드 파일에서 수행할 수 있습니다. 피어 정의는 모두 피어를 제공하는 컨트롤과 다른 별도의 네임스페이스인 [**Windows.UI.Xaml.Automation.Peers**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers) 네임스페이스에 있습니다. [  **OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 메서드 호출에 필요한 네임스페이스를 참조하는 한 피어를 별도의 네임스페이스에 선언하도록 선택할 수도 있습니다.

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>올바른 피어 기본 클래스 선택  
파생하려는 컨트롤 클래스의 기존 피어 논리에 가장 일치하는 클래스를 제공하는 기본 클래스에서 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)가 파생되는지 확인합니다. 이전 예제의 경우 `NumericUpDown`은 [**RangeBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase)에서 파생되기 때문에 피어의 기반으로 사용할 수 있는 [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) 클래스가 있습니다. 컨트롤 자체를 파생시키는 방법에 가장 일치하는 피어 클래스를 사용하면 기본 피어 클래스에서 이미 구현하기 때문에 적어도 [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) 기능의 일부는 재정의할 필요가 없습니다.

기본 [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) 클래스에는 해당 피어 클래스가 없습니다. **Control**에서 파생되는 사용자 지정 컨트롤에 해당하는 피어 클래스가 필요한 경우 [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)에서 사용자 지정 피어 클래스를 파생시킵니다.

[  **ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl)에서 직접 파생시키는 경우 피어 클래스를 참조하는 [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 구현이 없으므로 해당 클래스에 기본 자동화 피어 동작이 없습니다. 따라서 **OnCreateAutomationPeer**를 구현하여 고유한 피어를 사용하거나 접근성 지원의 해당 수준이 컨트롤에 적합한 경우 [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)를 피어로 사용해야 합니다.

> [!NOTE]
> 일반적으로 [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)가 아닌 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)에서 파생하지 않습니다. **AutomationPeer**에서 직접 파생하는 경우 **FrameworkElementAutomationPeer**에서 제공되는 기본적인 접근성 지원을 다량으로 복제해야 합니다.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>사용자 지정 피어 클래스의 초기화  
자동화 피어는 기본 초기화에 소유자 컨트롤의 인스턴스를 사용하는 형식이 안전한 생성자를 정의해야 합니다. 다음 예제의 구현에서는 *owner* 값을 [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) 기본으로 전달하며 최종적으로는 실제로 *owner*를 사용하여 [**FrameworkElementAutomationPeer.Owner**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner)를 설정하는 [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)가 됩니다.

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>AutomationPeer의 Core 메서드  
UWP 인프라를 위해 자동화 피어의 재정의 가능한 메서드는 UI 자동화 공급자에서 UI 자동화 클라이언트에 대한 전달 지점으로 사용하는 공용 액세스 메서드와 UWP 클래스에서 동작에 영향을 주기 위해 재정의할 수 있는 보호된 "Core" 사용자 지정 메서드로 이루어진 쌍의 일부입니다. 이 메서드 쌍은 기본적으로 함께 연결되어 액세스 메서드를 호출하면 공급자 구현이 있는 병렬 "Core" 메서드가 항상 호출되거나 대체 방법으로 기본 클래스의 기본 구현이 호출되도록 합니다.

사용자 지정 컨트롤에 대해 피어를 구현하는 경우 사용자 지정 컨트롤에 고유한 동작을 표시하려는 기본 자동화 피어 클래스에서 "Core" 메서드를 재정의합니다. UI 자동화 코드는 피어 클래스의 공용 메서드를 호출하여 컨트롤에 대한 정보를 가져옵니다. 컨트롤에 대한 정보를 제공하려면 컨트롤 구현과 디자인이 기본 자동화 피어 클래스에서 지원되는 것과 다른 접근성 시나리오 또는 기타 UI 자동화 시나리오를 만들 때 "Core"로 끝나는 이름으로 각 메서드를 재정의합니다.

최소한 새 피어 클래스를 정의할 때마다 다음 예제와 같이 [**GetClassNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) 메서드를 구현합니다.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> 사용자는 이 문자열을 메서드 본문에서 직접 저장하는 것보다 상수로 저장하기를 원할 수도 있지만 이는 사용자가 결정할 문제입니다. [  **GetClassNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore)의 경우 이 문자열을 지역화할 필요가 없습니다. **LocalizedControlType** 속성은 UI 자동화 클라이언트에서 지역화된 문자열을 필요로 할 때마다 사용되지만 **ClassName**은 아닙니다.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/> @ no__t-1 @ no__t-2GetAutomationControlType

일부 보조 기술에서는 UI 자동화 **Name** 외에 추가 정보로 UI 자동화 트리에 있는 항목의 특징을 보고할 때 [**GetAutomationControlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) 값을 직접 사용합니다. 컨트롤이 파생 시 사용된 원본 컨트롤과 상당히 다르고 컨트롤에 사용되는 기본 피어 클래스에서 보고하는 것과 다른 컨트롤 형식을 보고하려는 경우 피어를 구현하고 피어 구현에서 [**GetAutomationControlTypeCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore)를 재정의해야 합니다. 이 작업은 기본 피어가 컨트롤 형식에 대한 정확한 정보를 제공하지 않는 [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 또는 [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl) 같은 일반화된 기본 클래스에서 파생시키는 경우 특히 중요합니다.

[  **GetAutomationControlTypeCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) 구현에서는 [**AutomationControlType**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 값을 반환하여 컨트롤을 설명합니다. **AutomationControlType.Custom**을 반환할 수 있지만 컨트롤의 주요 시나리오를 정확하게 설명하는 경우 보다 구체적인 컨트롤 형식을 반환해야 합니다. 예를 들면 다음과 같습니다.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> [  **AutomationControlType.Custom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)을 지정하지 않는 한 [**GetLocalizedControlTypeCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore)를 구현하여 **LocalizedControlType** 속성 값을 클라이언트에 제공할 필요는 없습니다. UI 자동화 일반 인프라는 **AutomationControlType.Custom** 이외의 가능한 모든 **AutomationControlType** 값에 대해 변환된 문자열을 제공합니다.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern 및 GetPatternCore  
피어의 [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 구현에서는 입력 매개 변수에서 요청되는 패턴을 지원하는 개체를 반환합니다. 특히, UI 자동화 클라이언트에서는 공급자의 [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 메서드로 전달된 메서드를 호출하고, 요청된 패턴의 이름을 지정하는 [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 열거 값을 지정합니다. **GetPatternCore**의 재정의에서는 지정한 패턴을 구현하는 개체를 반환해야 합니다. 피어는 패턴을 지원한다고 보고할 때마다 해당 패턴 인터페이스를 구현해야 하므로 해당 개체는 피어 자체입니다. 피어에 사용자 지정 패턴 구현이 없지만 피어 기본에서 패턴을 구현하는 경우 **GetPatternCore**에서 기본 형식의 **GetPatternCore** 구현을 호출할 수 있습니다. 피어가 패턴을 지원하지 않는 경우 피어의 **GetPatternCore**에서 **null**을 반환해야 합니다. 하지만 구현에서 직접 **null**을 반환하는 대신 기본 구현에 대한 호출을 사용하여 지원되지 않는 모든 패턴에 대해 **null**이 반환되도록 하는 것이 일반적입니다.

패턴이 지원되는 경우 [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 구현에서 **this** 또는 **Me**를 반환할 수 있습니다. UI 자동화 클라이언트는 반환 값이 **null**이 아닐 때마다 [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 반환 값을 요청된 패턴 인터페이스로 캐스팅하도록 되어 있습니다.

피어 클래스가 다른 피어에서 상속된 경우 필요한 모든 지원 및 패턴 보고가 기본 클래스에서 이미 처리되므로 [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)를 구현할 필요가 없습니다. 예를 들어 [**RangeBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase)에서 파생되는 범위 컨트롤을 구현하고 피어가 [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer)에서 파생되는 경우 해당 피어는 [**PatternInterface.RangeValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)에 대해 그 자체를 반환하고 패턴을 지원하는 [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) 인터페이스의 작동을 구현합니다.

리터럴 코드는 아니지만 이 예제에서는 [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer)에 이미 있는 [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)의 구현을 근사화합니다.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

기본 피어 클래스에서 필요한 일부 지원이 없는 위치에 피어를 구현하거나 피어에서 지원할 수 있는 기본 상속 패턴 집합을 변경 또는 추가하려면 [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)를 재정의하여 UI 자동화 클라이언트에서 패턴을 사용하도록 해야 합니다.

UI 자동화 지원의 UWP 구현에서 사용할 수 있는 공급자 패턴 목록은 [**Windows.UI.Xaml.Automation.Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider)를 참조하세요. 이러한 각 패턴에는 UI 자동화 클라이언트가 [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 호출에서 패턴을 요청하는 방식인 [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 열거의 해당 값이 있습니다.

피어는 둘 이상의 패턴을 지원한다고 보고할 수 있습니다. 그럴 경우 재정의는 지원되는 각 [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 값에 대한 반환 경로 논리를 포함해야 하며 일치하는 각 경우에 피어를 반환해야 합니다. 호출자는 한 번에 하나의 인터페이스만 요청해야 하며 예상 인터페이스로 캐스팅하는 것은 호출자가 결정합니다.

다음은 사용자 지정 피어에 대한 [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 재정의의 예입니다. 이 예에서는 [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) 및 [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider)의 두 패턴에 대한 지원을 보고합니다. 다음 컨트롤은 전체 화면(토글 모드)으로 표시될 수 있는 미디어 표시 컨트롤이며 사용자가 위치(범위 컨트롤)를 선택할 수 있는 진행률 표시줄을 포함합니다. 이 코드는 [XAML 접근성 샘플](https://go.microsoft.com/fwlink/p/?linkid=238570)에서 제공됩니다.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>하위 요소에서 패턴 전달  
[  **GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 메서드 구현에서는 하위 요소 또는 부분을 해당 호스트의 패턴 공급자로 지정할 수도 있습니다. 이 예제에서는 [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)이 스크롤 패턴 처리를 내부 [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 컨트롤의 피어로 전송하는 방법을 모방합니다. 패턴 처리를 위한 하위 요소를 지정하기 위해 이 코드는 하위 요소 개체를 가져오고 [**FrameworkElementAutomationPeer.CreatePeerForElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) 메서드를 사용하여 하위 요소에 대한 피어를 만든 다음 새 피어를 반환합니다.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>기타 Core 메서드  
컨트롤은 주요 시나리오에 대해 키보드 시나리오를 지원해야 할 수 있습니다. 이 지원이 필요한 이유에 대한 자세한 내용은 [키보드 접근성](keyboard-accessibility.md)을 참조하세요. 키 지원 구현은 컨트롤 논리의 일부이기 때문에 피어 코드가 아니라 컨트롤 코드에 포함되어야 하지만 피어 클래스에서 [**GetAcceleratorKeyCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) 및 [**GetAccessKeyCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) 메서드를 재정의하여 사용되는 키를 UI 자동화 클라이언트에 보고해야 합니다. 키 정보를 보고하는 문자열이 지역화되어야 할 수 있으므로 하드 코드된 문자열이 아니라 리소스에서 제공되어야 한다고 가정해 보세요.

컬렉션을 지원하는 클래스에 대해 피어를 제공하려면 해당 종류의 컬렉션을 이미 지원하는 피어 클래스와 기능 클래스에서 파생시키는 것이 가장 좋습니다. 그럴 수 없는 경우 자식 컬렉션을 유지 관리하는 컨트롤의 피어에서 컬렉션 관련 피어 메서드인 [**GetChildrenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore)를 재정의하여 UI 자동화 트리에 부모 자식 관계를 보고해야 할 수 있습니다.

[  **IsContentElementCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) 및 [**IsControlElementCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) 메서드를 구현하여 컨트롤이 데이터 콘텐츠를 포함하는지, 용자 인터페이스에서 대화형 역할을 수행하는지 또는 둘 다인지를 나타냅니다. 기본적으로 두 메서드는 **true**를 반환합니다. 이러한 설정을 사용하면 화면 읽기 프로그램 같은 보조 기술의 유용성이 향상되므로 이러한 메서드를 사용하여 자동화 트리를 필터링할 수 있습니다. [  **GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 메서드에서 패턴 처리를 하위 요소 피어로 전송하는 경우 하위 요소 피어의 **IsControlElementCore** 메서드에서 **false**를 반환하여 자동화 트리에서 하위 요소 피어를 숨길 수 있습니다.

일부 컨트롤은 텍스트 레이블 부분이 비텍스트 부분에 대한 정보를 제공하거나 컨트롤이 UI의 다른 컨트롤과 알려진 레이블 지정 관계에 있어야 하는 레이블 지정 시나리오를 지원할 수 있습니다. 유용한 클래스 기반 동작을 제공할 수 있는 경우 [**GetLabeledByCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore)를 재정의하여 이 동작을 제공할 수 있습니다.

[**GetBoundingRectangleCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) 및 [**GetClickablePointCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) 는 주로 자동화 된 테스트 시나리오에 사용 됩니다. 컨트롤에 대해 자동화된 테스트를 지원하려는 경우 이러한 메서드를 재정의할 수 있습니다. 이는 사용자가 좌표 공간에서 클릭하는 위치가 범위에 다른 효과를 주기 때문에 단일 지점을 제안할 수 없는 경우 범위 형식 컨트롤에 적합할 수 있습니다. 예를 들어 기본 [**ScrollBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar) 자동화 피어가 **GetClickablePointCore**를 재정의하여 "숫자가 아닌" [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) 값을 반환하도록 합니다.

[**GetLiveSettingCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) 는 UI 자동화에 대 한 **LiveSetting** 값의 컨트롤 기본값에 영향을 미칩니다. 컨트롤에서 [**AutomationLiveSetting.Off**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationLiveSetting)가 아닌 값을 반환하도록 하려는 경우 이를 재정의할 수 있습니다. **LiveSetting**이 나타내는 내용에 대한 자세한 내용은 [**AutomationProperties.LiveSetting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty)을 참조하세요.

컨트롤에 [**AutomationOrientation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationOrientation)으로 매핑할 수 있는, 설정 가능한 방향 속성이 있는 경우 [**GetOrientationCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getorientationcore)를 재정의할 수 있습니다. [  **ScrollBarAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.ScrollBarAutomationPeer) 및 [**SliderAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.SliderAutomationPeer) 클래스가 이 역할을 수행합니다.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>FrameworkElementAutomationPeer의 기본 구현  
[  **FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)의 기본 구현에서는 프레임워크 수준에서 정의되는 다양한 레이아웃 및 동작 속성에서 해석될 수 있는 UI 자동화 정보를 제공합니다.

* [**GetBoundingRectangleCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): 알려진 레이아웃 특성을 기반으로 하는 [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) 구조체를 반환 합니다. [  **IsOffscreen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen)이 **true**이면 0 값 **Rect**를 반환합니다.
* [**GetClickablePointCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore): 0이 아닌 **BoundingRectangle**있는 경우 알려진 레이아웃 특성을 기반으로 하는 [**점**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) 구조를 반환 합니다.
* [**Getnamecore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore): 여기에 요약 될 수 있는 것 보다 더 광범위 한 동작이 있습니다. [**Getnamecore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore)를 참조 하십시오. 기본적으로 [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl)의 알려진 콘텐츠나 콘텐츠가 있는 관련 클래스에서 문자열 변환을 시도합니다. 또한 [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 값이 있는 경우 해당 항목의 **Name** 값이 **Name**으로 사용됩니다.
* [**HasKeyboardFocusCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): 소유자의 [**FocusState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focusstate) 및 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) 속성을 기반으로 평가 됩니다. 컨트롤이 아닌 요소는 항상 **false**를 반환합니다.
* [**IsEnabledCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): [**컨트롤**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)인 경우 소유자의 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) 속성을 기준으로 평가 됩니다. 컨트롤이 아닌 요소는 항상 **true**를 반환합니다. 이는 기존 조작의 측면에서 소유자를 사용할 수 있다는 것이 아니라 소유자에게 **IsEnabled** 속성이 없어도 피어를 사용할 수 있다는 것을 의미합니다.
* [**IsKeyboardFocusableCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): Owner가 [**컨트롤인**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)경우 **true** 를 반환 합니다. 그렇지 않으면 **false**입니다.
* [**IsOffscreenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): Owner 요소 또는 해당 부모 항목에서 [**축소**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visibility) 된 [**표시 유형은**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) [**IsOffscreen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen)의 **true** 값과 같습니다. 예외: 소유자의 부모가 표시되지 않아도 [**Popup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) 개체가 표시될 수 있습니다.
* [**SetFocusCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): [**포커스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focus)를 호출 합니다.
* [**Getparent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent): 소유자에서 [**FrameworkElement. 부모**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.parent) 를 호출 하 고 적절 한 피어를 조회 합니다. "Core" 메서드와의 재정의 쌍이 아니므로 이 동작은 변경할 수 없습니다.

> [!NOTE]
> 기본 UWP 피어는 UWP를 구현하는 네이티브 코드를 사용하여 동작을 구현합니다. 실제 UWP 코드를 사용하지 않아도 됩니다. CLR(공용 언어 런타임) 리플렉션 또는 다른 기술을 통해 구현 코드나 논리를 확인할 수 없습니다. 또한 기본 피어 동작의 서브클래스별 재정의를 위한 개별 참조 페이지가 표시되지 않습니다. 예를 들어 [**TextBoxAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer)의 [**GetNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore)에 대해 추가 동작이 있을 수 있으며, 이러한 추가 동작에 대해서는 **AutomationPeer.GetNameCore** 참조 페이지에서 설명되지 않고 **TextBoxAutomationPeer.GetNameCore**에 대한 참조 페이지가 없습니다. **TextBoxAutomationPeer.GetNameCore** 참조 페이지도 없습니다. 대신 직계 피어 클래스에 대한 참조 항목을 읽고 설명 섹션에서 구현 참고 사항을 찾습니다.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>피어 및 AutomationProperties  
자동화 피어는 컨트롤의 접근성 관련 정보에 적절한 기본값을 제공해야 합니다. 컨트롤을 사용하는 앱 코드에서 [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 연결된 속성 값을 컨트롤 인스턴스에 포함하여 해당 동작의 일부를 재정의할 수 있습니다. 호출자는 기본 컨트롤이나 사용자 지정 컨트롤에 대해 이 작업 중 하나를 수행할 수 있습니다. 예를 들어 다음 XAML에서는 두 개의 사용자 지정 된 UI 자동화 속성이 있는 단추를 만듭니다. `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

[  **AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 연결된 속성에 대한 자세한 내용은 [기본적인 접근성 정보](basic-accessibility-information.md)를 참조하세요.

일부 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 메서드는 UI 자동화 공급자가 정보를 보고하는 방식의 일반적인 계약으로 인해 존재하지만 일반적으로 이러한 메서드는 컨트롤 피어에 구현되지 않습니다. 이는 특정 UI에서 컨트롤을 사용하는 앱 코드에 적용된 [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 값에서 해당 정보를 제공하도록 되어 있기 때문입니다. 예를 들어 대부분의 앱은 [**AutomationProperties.LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 값을 적용하여 UI에 있는 서로 다른 두 컨트롤 간의 레이블 지정 관계를 정의합니다. 그러나 [**LabeledByCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore)는 헤더 부분을 사용하여 데이터 필드 부분의 레이블 지정, 해당 컨테이너로 항목의 레이블 지정 또는 유사한 시나리오 같이 컨트롤에 데이터 또는 항목 관계를 나타내는 특정 피어에서 구현됩니다.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>패턴 구현  
확장-축소에 대한 컨트롤 패턴 인터페이스를 구현하여 확장-축소 동작을 구현하는 컨트롤에 대해 피어를 작성하는 방법을 살펴보겠습니다. 피어는 [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern)이 [**PatternInterface.ExpandCollapse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 값으로 호출될 때마다 피어 자체를 반환하여 확장-축소 동작에 접근성을 사용하도록 설정해야 합니다. 그런 다음 피어는 해당 패턴([**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider))에 대한 공급자 인터페이스를 상속하고 해당 공급자 인터페이스의 각 구성원에 대해 구현을 제공해야 합니다. 이 경우 인터페이스에 재정의할 세 개의 멤버가 있습니다. [**확장**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**축소**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse), [**ExpandCollapseState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

따라서 클래스 자체의 API 설계에서 접근성을 미리 계획하면 도움이 됩니다. UI에서 작업하는 사용자의 일반적인 조작 결과로 또는 자동화 공급자 패턴을 통해 잠재적으로 요청되는 동작이 있을 때마다 UI 응답이나 자동화 패턴에서 호출할 수 있는 단일 메서드를 제공합니다. 예를 들어 컨트롤을 확장하거나 축소할 수 있는 유선 이벤트 처리기가 있는 단추 부분이 컨트롤에 있을 경우 해당 동작에 대한 키보드 동작이 있으면 이러한 이벤트 처리기에서 피어의 [**IExpandCollapseProvider**](https://docs.microsoft.com/windows/desktop/api/uiautomationcore/nn-uiautomationcore-iexpandcollapseprovider)에 대해 [**Expand**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand) 또는 [**Collapse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) 구현의 본문 내에서 호출하는 것과 동일한 메서드를 호출하도록 합니다. 컨트롤의 시각적 상태가 동작이 호출된 방식에 관계없이 일관된 방식으로 논리적 상태를 표시하도록 업데이트되는지 확인하려는 경우 일반적인 논리 메서드를 사용하는 것도 유용할 수 있습니다.

일반적인 구현에서는 공급자 API가 런타임에 컨트롤 인스턴스에 액세스하기 위해 먼저 [**Owner**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner)를 호출합니다. 그런 다음 해당 개체에서 필요한 동작 메서드를 호출할 수 있습니다.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

대체 구현은 컨트롤 자체가 해당 피어를 참조할 수 있도록 하는 것입니다. [  **RaiseAutomationEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 메서드는 피어 메서드이므로 컨트롤에서 자동화 이벤트를 발생시키는 경우 이는 일반적인 패턴입니다.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>UI 자동화 이벤트  

UI 자동화 이벤트는 다음 범주로 나뉩니다.

| 이벤트 | 설명 |
|-------|-------------|
| 속성 변경 | UI 자동화 요소나 컨트롤 패턴의 속성이 변경될 때 발생합니다. 예를 들어 클라이언트가 앱의 확인란 컨트롤을 모니터링해야 하는 경우 [**ToggleState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itoggleprovider.togglestate) 속성의 속성 변경 이벤트를 수신 대기하기 위해 등록할 수 있습니다. 확인란 컨트롤을 선택하거나 선택 취소하면 공급자가 이벤트를 발생시키며 클라이언트가 필요에 따라 동작할 수 있습니다. |
| 요소 작업 | 사용자 또는 프로그래밍 방식의 활동에 의해 UI가 변경될 때(예제: 단추를 클릭하거나 **Invoke** 패턴을 통해 단추를 호출할 때) 발생합니다. |
| 구조 변경 | UI 자동화 트리의 구조가 변경될 때 발생합니다. 데스크톱에서 새 UI 항목을 표시하거나, 숨기거나, 제거할 때 구조가 변경됩니다. |
| 전체 변경 | 포커스가 한 요소에서 다른 요소로 전환되거나 자식 창이 닫히는 경우 등 클라이언트에 대한 전체 작업이 수행될 때 발생합니다. 일부 이벤트는 UI 상태가 변경된 것을 의미하지 않을 수도 있습니다. 예를 들어 사용자가 텍스트 입력 필드를 탭한 다음 단추를 클릭하여 필드를 업데이트하면 사용자가 실제로 텍스트를 변경하지 않았어도 [**TextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 이벤트가 발생합니다. 이벤트를 처리할 때 클라이언트 응용 프로그램이 작업을 수행하기 전에 실제로 변경된 사항이 있는지 여부를 확인해야 할 수도 있습니다. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>AutomationEvents 식별자  
UI 자동화 이벤트는 [**AutomationEvents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationEvents) 값으로 식별됩니다. 열거 값은 이벤트 종류를 고유하게 식별합니다.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>이벤트 발생  
UI 자동화 클라이언트에서 자동화 이벤트를 구독할 수 있습니다. 자동화 피어 모델에서 사용자 지정 컨트롤에 대한 피어는 [**RaiseAutomationEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 메서드를 호출하여 접근성과 관련된 컨트롤 상태의 변경 내용을 보고해야 합니다. 마찬가지로 키 UI 자동화 속성 값이 변경되면 사용자 지정 컨트롤 피어에서 [**RaisePropertyChangedEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent) 메서드를 호출해야 합니다.

다음 코드 예제에서는 컨트롤 정의 코드 내에서 피어 개체를 가져오고 메서드를 호출하여 해당 피어에서 이벤트를 발생시키는 방법을 보여 줍니다. 최적화 방법으로 코드에서 이 이벤트 형식에 대한 수신기가 있는지 여부를 확인합니다. 수신기가 있는 경우에만 이벤트를 발생시키고 피어 개체를 만들어 불필요한 오버헤드를 방지하고 컨트롤이 계속 응답하도록 합니다.


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>피어 탐색  
자동화 피어를 찾은 후 UI 자동화 클라이언트에서 피어 개체의 [**GetChildren**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildren) 및 [**GetParent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent) 메서드를 호출하여 앱의 피어 구조를 탐색할 수 있습니다. 컨트롤 내 UI 요소 탐색은 피어의 [**GetChildrenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) 메서드 구현으로 지원됩니다. UI 자동화 시스템에서 이 메서드를 호출하여 컨트롤 내에 포함된 하위 요소(예제: 목록 상자의 목록 항목)의 트리를 구축합니다. [  **FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)의 기본 **GetChildrenCore** 메서드는 요소의 시각적 트리를 트래버스하여 자동화 피어의 트리를 만듭니다. 사용자 지정 컨트롤은 이 메서드를 재정의하여 하위 요소의 다양한 표현을 자동화 클라이언트에 표시하고 정보를 전달하거나 사용자 조작을 허용하는 요소의 자동화 피어를 반환할 수 있습니다.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>텍스트 패턴 기본 자동화 지원  
일부 기본 UWP 앱 자동화 피어에서는 텍스트 패턴에 대한 컨트롤 패턴([**PatternInterface.Text**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)) 지원을 제공합니다. 그러나 이 지원은 네이티브 메서드를 통해 제공되고 포함된 피어에서는 관리되는 상속에 [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) 인터페이스를 기록하지 않습니다. 하지만 관리되거나 관리되지 않는 UI 자동화 클라이언트에서 패턴에 대한 피어를 쿼리하는 경우에는 해당 클라이언트에서 텍스트 패턴 지원을 보고하고 클라이언트 API가 호출될 때 패턴 일부에 대한 동작을 제공합니다.

UWP 앱 텍스트 컨트롤 중 하나에서 파생하고 텍스트 관련 피어 중 하나에서 파생되는 사용자 지정 피어를 만들려면 피어에 대한 설명 섹션을 확인하여 패턴에 대한 기본 수준 지원에 대해 알아보세요. 관리되는 공급자 인터페이스 구현에서 기본 구현을 호출하면 사용자 지정 피어의 기본 동작에 액세스할 수 있지만, 피어와 피어의 소유자 컨트롤에 대한 기본 인터페이스가 노출되지 않으므로 기본 구현에서 수행하는 작업을 수정하기가 어렵습니다. 일반적으로 기본 구현을 있는 그대로 사용하거나(기본 호출에만 해당) 해당 기능을 고유한 관리 코드로 완전히 바꾸고 기본 구현을 호출하지 않아야 합니다. 후자는 고급 시나리오이며, 해당 프레임워크를 사용할 때 접근성 요구 사항을 지원하기 위해 컨트롤에서 사용되는 텍스트 서비스 프레임워크를 잘 알고 있어야 합니다.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
사용자 지정 피어를 제공하는 것 외에, XAML에서 [**AutomationProperties.AccessibilityView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview)를 설정하여 컨트롤 인스턴스에 대한 트리 보기 표현을 조정할 수도 있습니다. 이 작업은 피어 클래스의 일부로 구현되지는 않지만 사용자 지정 컨트롤 또는 사용자 지정 템플릿에 대한 전체 접근성 지원과 관련이 있기 때문에 언급하겠습니다.

**AutomationProperties.AccessibilityView** 사용의 주요 시나리오는 템플릿의 특정 컨트롤을 UI 자동화 보기에서 의도적으로 생략하는 것입니다. 전체 컨트롤에 대한 접근성 보기에 별 도움이 되지 않기 때문입니다. 이를 방지하려면 **AutomationProperties.AccessibilityView**를 "Raw"로 설정합니다.

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>자동화 피어에서 예외 발생  
자동화 피어 지원을 위해 구현하는 API는 예외를 발생시킬 수 있습니다. 수신 대기 중인 모든 UI 자동화 클라이언트는 대부분의 예외가 발생한 후에도 지속될 수 있을 정도로 강력해야 합니다. 모든 가능한 상황에서 수신기는 개발자 고유 앱 이외의 앱이 포함된 전체 자동화 트리를 찾습니다. 클라이언트가 API를 호출할 때 트리의 한 영역에서 피어 기반 예외를 발생시켰다는 이유만으로 전체 클라이언트를 중단하는 것은 허용되지 않는 클라이언트 설계입니다.

피어에 전달된 매개 변수의 경우 입력의 유효성을 검사하고 예를 들어 **null**이 전달되었으며 이 값이 구현에 유효한 값이 아닌 경우 [**ArgumentNullException**](https://docs.microsoft.com/dotnet/api/system.argumentnullexception)을 발생시키는 것 등은 허용할 수 있습니다. 하지만 피어에서 수행하는 후속 작업이 있는 경우에는 피어가 호스팅 컨트롤을 상호 작용하는 데 있어서 비동기적인 특성이 있어야 합니다. 피어가 수행하는 작업이 컨트롤의 UI 스레드를 반드시 차단하지는 않습니다(차단해서도 안 됨). 따라서 피어 생성 시 또는 자동화 피어 메서드가 처음 호출될 때 개체를 사용할 수 있거나 개체에 특정 속성이 있었지만 컨트롤 상태가 변경된 상황이 발생할 수 있습니다. 이런 경우 공급자가 발생시킬 수 있는 두 개의 전용 예외가 있습니다.

* API가 전달된 원래 정보를 기반으로 피어의 소유자 또는 관련 피어 요소에 액세스할 수 없는 경우 [**ElementNotAvailableException**](https://docs.microsoft.com/dotnet/api/system.windows.automation.elementnotavailableexception)을 발생시킵니다. 예를 들어 메서드를 실행하려고 하지만 소유자가 닫힌 모달 대화 상자 등과 같은 UI에서 제거된 피어가 있을 수 있습니다. Non-.NET client의 경우 [**UIA @ no__t-2e @ no__t-3ELEMENTNOTAVAILABLE**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-error-codes)에 매핑됩니다.
* 소유자가 여전히 있지만 이 소유자가 피어가 수행하려는 특정 프로그래밍 변경의 일부를 차단하는 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled)`=`**false** 등과 같은 모드에 있는 경우 [**ElementNotEnabledException**](https://docs.microsoft.com/dotnet/api/system.windows.automation.elementnotenabledexception)을 발생시킵니다. Non-.NET client의 경우 [**UIA @ no__t-2e @ no__t-3ELEMENTNOTENABLED**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-error-codes)에 매핑됩니다.

이외에도, 피어는 피어 지원에서 발생시키는 예외와 관련하여 비교적 보수적이어야 합니다. 대부분의 클라이언트는 피어에서 발생한 예외를 처리하여 이를 사용자가 클라이언트 조작 시 결정할 수 있는 작동 가능한 선택으로 전환할 수 없습니다. 따라서 때로는 피어 구현 내에서 다시 발생시키지 않고 예외를 catch하는 No-op이 피어가 시도하는 일이 작동하지 않을 때마다 예외를 발생시키는 것보다 나은 전략입니다. 대부분의 UI 자동화 클라이언트는 관리 코드로 작성되지 않았다는 점도 고려하세요. 대부분은 COM으로 작성 되며, 피어에 대 한 액세스를 종료 하는 UI 자동화 클라이언트 메서드를 호출할 때마다 **HRESULT** 에서 **S @ NO__T-1ok** 를 확인 하는 것입니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [액세스 가능성](accessibility.md)
* [XAML 접근성 샘플](https://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)
* [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
* [**System.windows.uielement.oncreateautomationpeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)
* [컨트롤 패턴 및 인터페이스](control-patterns-and-interfaces.md)
