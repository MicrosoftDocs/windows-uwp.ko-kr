---
Description: Microsoft UI 자동화에 대 한 자동화 피어의 개념과 고유한 사용자 지정 UI 클래스에 대 한 자동화 지원을 제공 하는 방법을 설명 합니다.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: 사용자 지정 자동화 피어
label: Custom automation peers
template: detail.hbs
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 82d42dcc78dabc6374250b088c0d076c325d1045
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157017"
---
# <a name="custom-automation-peers"></a>사용자 지정 자동화 피어  

Microsoft UI 자동화에 대 한 자동화 피어의 개념과 고유한 사용자 지정 UI 클래스에 대 한 자동화 지원을 제공 하는 방법을 설명 합니다.

UI 자동화는 자동화 클라이언트에서 다양 한 UI 플랫폼과 프레임 워크의 사용자 인터페이스를 검사 하거나 운영 하는 데 사용할 수 있는 프레임 워크를 제공 합니다. Windows 앱을 작성 하는 경우 UI에 사용 하는 클래스는 이미 UI 자동화를 지원 합니다. 봉인 되지 않은 기존 클래스에서 파생 하 여 새로운 종류의 UI 컨트롤이 나 지원 클래스를 정의할 수 있습니다. 이 작업을 수행 하는 동안 클래스는 접근성을 지원 해야 하지만 기본 UI 자동화 지원에서 다루지 않는 동작을 추가할 수 있습니다. 이 경우 기본 구현에서 사용 하는 [**automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 클래스에서 파생 하 여 기존 UI 자동화 지원을 확장 하 고, 필요한 지원을 피어 구현에 추가 하 고, 새 피어를 만들어야 하는 Windows 앱 컨트롤 인프라에 알립니다.

UI 자동화를 사용 하면 화면 판독기와 같은 내게 필요한 옵션 응용 프로그램 및 보조 기술 뿐만 아니라 품질 보증 (테스트) 코드도 사용할 수 있습니다. 두 시나리오에서 UI 자동화 클라이언트는 사용자 인터페이스 요소를 검사 하 고 앱 외부의 다른 코드에서 앱과의 사용자 상호 작용을 시뮬레이션할 수 있습니다. 모든 플랫폼에서 UI 자동화에 대 한 정보 및 더 광범위 한 의미에서 [Ui 자동화 개요](/windows/desktop/WinAuto/uiauto-uiautomationoverview)를 참조 하세요.

UI 자동화 프레임 워크를 사용 하는 두 가지 고유한 대상 그룹이 있습니다.

* **Ui 자동화 *클라이언트* ** 는 ui 자동화 api를 호출 하 여 현재 사용자에 게 표시 되는 모든 ui에 대해 알아봅니다. 예를 들어 화면 판독기와 같은 보조 기술은 UI 자동화 클라이언트 역할을 합니다. UI는 관련 된 자동화 요소 트리로 표시 됩니다. UI 자동화 클라이언트는 한 번에 하나의 앱에만 관심이 나 전체 트리에서 관심이 있을 수 있습니다. UI 자동화 클라이언트는 UI 자동화 Api를 사용 하 여 트리를 탐색 하 고 자동화 요소의 정보를 읽거나 변경할 수 있습니다.
* Ui **자동화 *공급자* ** 는 ui에서 해당 앱의 일부로 도입 된 요소를 노출 하는 api를 구현 하 여 ui 자동화 트리에 정보를 제공 합니다. 새 컨트롤을 만들 때 이제 UI 자동화 공급자 시나리오에서 참가자 역할을 수행 해야 합니다. 공급자는 모든 UI 자동화 클라이언트에서 UI 자동화 프레임 워크를 사용 하 여 내게 필요한 옵션 및 테스트 목적으로 컨트롤과 상호 작용할 수 있는지 확인 해야 합니다.

일반적으로 UI 자동화 프레임워크에는 각각 UI 자동화 클라이언트용 API와 유사한 이름의 UI 자동화용 API 등 병렬 API가 있습니다. 대부분의 경우이 항목에서는 UI 자동화 공급자의 Api와 특히 해당 UI 프레임 워크에서 공급자 확장성을 사용할 수 있도록 하는 클래스 및 인터페이스에 대해 설명 합니다. UI 자동화 클라이언트에서 사용 하는 UI 자동화 Api를 언급 하거나, 일부 큐브 뷰를 제공 하거나, 클라이언트 및 공급자 Api와 상관 관계를 지정 하는 조회 테이블을 제공 하는 경우가 있습니다. 클라이언트 관점에 대 한 자세한 내용은 [UI Automation 클라이언트 프로그래머 가이드](/windows/desktop/WinAuto/uiauto-clientportal)를 참조 하세요.

> [!NOTE]
> UI 자동화 클라이언트는 일반적으로 관리 코드를 사용 하지 않고 UWP 앱으로 구현 되지 않습니다 (일반적으로 데스크톱 앱). UI 자동화는 특정 구현이 나 프레임 워크가 아니라 표준을 기반으로 합니다. 화면 판독기와 같은 보조 기술 제품을 비롯 한 많은 기존 UI 자동화 클라이언트는 COM (구성 요소 개체 모델) 인터페이스를 사용 하 여 UI 자동화, 시스템 및 자식 창에서 실행 되는 응용 프로그램과 상호 작용 합니다. Com 인터페이스에 대 한 자세한 내용 및 COM을 사용 하 여 UI 자동화 클라이언트를 작성 하는 방법에 대 한 자세한 내용은 [Ui 자동화 기본 사항](/windows/desktop/WinAuto/entry-uiautocore-overview)을 참조 하세요.

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>사용자 지정 UI 클래스에 대 한 UI 자동화 지원의 기존 상태 결정  
사용자 지정 컨트롤에 대 한 자동화 피어를 구현 하려면 먼저 기본 클래스와 해당 자동화 피어가 필요한 내게 필요한 옵션 또는 자동화 지원 기능을 제공 하는지 테스트 해야 합니다. 대부분의 경우 [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) 구현, 특정 피어 및이를 구현 하는 패턴의 조합은 기본 이지만 만족 스러운 내게 필요한 옵션 환경을 제공할 수 있습니다. 이가 true 인지 여부는 컨트롤에 대 한 개체 모델의 변경 내용 수와 해당 기본 클래스에 대 한 변경 내용 수에 따라 달라 집니다. 또한 기본 클래스 기능에 대 한 추가 사항이 템플릿 계약의 새 UI 요소 또는 컨트롤의 시각적 모양에 상관 관계가 있는지 여부에 따라 달라 집니다. 일부 경우에는 추가 접근성 지원이 필요한 사용자 환경의 새로운 측면이 변경 될 수 있습니다.

기존 기본 피어 클래스를 사용 하는 경우에도 기본적인 접근성 지원 기능을 제공 하는 것이 좋지만, 자동화 된 테스트 시나리오에 대 한 UI 자동화에 정확한 **ClassName** 정보를 보고할 수 있도록 피어를 정의 하는 것이 좋습니다. 이러한 고려 사항은 타사 사용을 위한 컨트롤을 작성 하는 경우에 특히 중요 합니다.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>자동화 피어 클래스  
UWP는 WPF (Windows Forms, Windows Presentation Foundation) 및 Microsoft Silverlight와 같은 이전 관리 코드 UI 프레임 워크에서 사용 하는 기존 UI 자동화 기술 및 규칙을 기반으로 합니다. 대부분의 컨트롤 클래스와 해당 함수 및 용도는 이전 UI 프레임 워크에도 해당 원본을 포함 합니다.

규칙에 따라 피어 클래스 이름은 컨트롤 클래스 이름으로 시작하고 "AutomationPeer"로 끝납니다. 예를 들어 [**Buttonautomationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.ButtonAutomationPeer) 는 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 컨트롤 클래스의 피어 클래스입니다.

> [!NOTE]
> 이 항목에서는 컨트롤 피어를 구현할 때 접근성과 관련 된 속성을 더 중요 하 게 처리 합니다. 그러나 UI 자동화 지원의 보다 일반적인 개념은 [Ui 자동화 공급자 프로그래머 가이드](/windows/desktop/WinAuto/uiauto-providerportal) 및 [Ui 자동화 기본 사항](/windows/desktop/WinAuto/entry-uiautocore-overview)에 설명 된 권장 사항에 따라 피어를 구현 해야 합니다. 이러한 항목에서는 UI 자동화를 위해 UWP 프레임 워크에서 정보를 제공 하는 데 사용 하는 특정 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) api에 대해 다루지 않지만 클래스를 식별 하거나 다른 정보나 상호 작용을 제공 하는 속성을 설명 합니다.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>피어, 패턴 및 컨트롤 형식  
*컨트롤 패턴* 은 UI 자동화 클라이언트에 컨트롤 기능의 특정 측면을 노출 하는 인터페이스 구현입니다. UI 자동화 클라이언트는 컨트롤 패턴을 통해 노출 되는 속성 및 메서드를 사용 하 여 컨트롤의 기능에 대 한 정보를 검색 하거나 런타임에 컨트롤의 동작을 조작 합니다.

컨트롤 패턴은 컨트롤 형식 및 컨트롤의 모양에 관계없이 컨트롤의 기능을 분류하고 노출하는 방법을 제공합니다. 예를 들어 테이블 형식 인터페이스를 표시 하는 컨트롤은 **Grid** 컨트롤 패턴을 사용 하 여 테이블의 행 및 열 수를 표시 하 고 UI 자동화 클라이언트가 테이블에서 항목을 검색할 수 있도록 합니다. 다른 예로, UI 자동화 클라이언트는 호출할 수 있는 컨트롤 (예: 단추)에 대해 **Invoke** 컨트롤 패턴을 사용 하 고, 목록 상자, 목록 뷰 또는 콤보 상자와 같이 스크롤 막대가 있는 컨트롤에 대해 **scroll** 컨트롤 패턴을 사용할 수 있습니다. 각 컨트롤 패턴은 별도의 기능 형식을 나타내며 컨트롤 패턴을 조합 하 여 특정 컨트롤에서 지 원하는 전체 기능 집합을 설명할 수 있습니다.

인터페이스는 COM 개체와 관련이 있으므로 컨트롤 패턴은 UI와 관련이 있습니다. COM에서 개체를 쿼리하여 개체에서 지 원하는 인터페이스를 확인 한 다음 이러한 인터페이스를 사용 하 여 기능에 액세스할 수 있습니다. Ui 자동화에서 ui 자동화 클라이언트는 UI 자동화 요소를 쿼리하여 지원 되는 컨트롤 패턴을 확인 한 다음, 지원 되는 컨트롤 패턴에 의해 노출 되는 속성, 메서드, 이벤트 및 구조를 통해 요소와 해당 피어 링 컨트롤을 조작할 수 있습니다.

자동화 피어의 주요 목적 중 하나는 ui 요소가 피어를 통해 지원할 수 있는 패턴을 제어 하는 UI 자동화 클라이언트에 보고 하는 것입니다. 이렇게 하기 위해 UI 자동화 공급자는 [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 메서드를 재정의 하 여 [**getpattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 메서드 동작을 변경 하는 새 피어를 구현 합니다. UI 자동화 클라이언트는 UI 자동화 공급자가 호출 하는 **Getpattern**에 매핑되는 호출을 수행 합니다. UI 자동화 클라이언트는 상호 작용 하려는 각 특정 패턴에 대해 쿼리 합니다. 피어가 패턴을 지 원하는 경우 자체에 대 한 개체 참조를 반환 합니다. 그렇지 않으면 **null**을 반환 합니다. 반환이 **null**이 아닌 경우 UI 자동화 클라이언트는 해당 컨트롤 패턴과 상호 작용 하기 위해 패턴 인터페이스의 api를 클라이언트로 호출할 수 있다고 예상 합니다.

*컨트롤 형식은* 피어에서 나타내는 컨트롤의 기능을 광범위 하 게 정의 하는 방법입니다. 패턴은 UI 자동화에서 가져올 수 있는 정보 또는 특정 인터페이스를 통해 수행할 수 있는 작업을 알려 주는 반면, 컨트롤 형식은 한 수준 위에 있습니다. 각 컨트롤 형식에는 UI 자동화의 이러한 측면에 대 한 지침이 있습니다.

* UI 자동화 컨트롤 패턴: 컨트롤 형식에서 두 개 이상의 패턴을 지원할 수 있으며 각 패턴은 서로 다른 정보 또는 상호 작용 분류를 나타냅니다. 각 컨트롤 형식에는 컨트롤에서 지원 해야 하는 컨트롤 패턴 집합, 선택 사항인 집합 및 컨트롤이 지원 하지 않아야 하는 집합이 있습니다.
* UI 자동화 속성 값: 각 컨트롤 형식에는 컨트롤에서 지원 해야 하는 속성 집합이 있습니다. 이러한 속성은 패턴에 따라 달라 지는 것이 아니라 [UI 자동화 속성 개요](/windows/desktop/WinAuto/uiauto-propertiesoverview)에 설명 된 일반 속성입니다.
* UI 자동화 이벤트: 각 컨트롤 형식에는 컨트롤에서 지원 해야 하는 이벤트 집합이 있습니다. [UI 자동화 이벤트 개요](/windows/desktop/WinAuto/uiauto-eventsoverview)에 설명 된 대로이는 패턴에 한정 되지 않는 일반적인 기능입니다.
* UI 자동화 트리 구조: 각 컨트롤 형식은 UI 자동화 트리 구조에서 컨트롤이 표시 되는 방법을 정의 합니다.

프레임 워크에 대 한 자동화 피어가 구현 되는 방법에 관계 없이 UI 자동화 클라이언트 기능은 UWP에 연결 되지 않으므로 보조 기술과 같은 기존 UI 자동화 클라이언트는 COM과 같은 다른 프로그래밍 모델을 사용 하 게 될 가능성이 높습니다. COM에서 클라이언트는 속성, 이벤트 또는 트리 검사를 위해 요청 된 패턴 또는 일반 UI 자동화 프레임 워크를 구현 하는 COM 컨트롤 패턴 인터페이스에 대해 **QueryInterface** 를 수행할 수 있습니다. 패턴의 경우 UI 자동화 프레임 워크는 해당 인터페이스 코드를 앱 UI 자동화 공급자 및 관련 피어에 대해 실행 되는 UWP 코드로 마샬링합니다.

C 또는 Microsoft Visual Basic를 사용 하 여 UWP 앱과 같은 관리 코드 프레임 워크에 대 한 컨트롤 패턴을 구현 하는 경우 \# COM 인터페이스 표현을 사용 하는 대신 .NET Framework 인터페이스를 사용 하 여 이러한 패턴을 나타낼 수 있습니다. 예를 들어 **호출** 패턴의 Microsoft .NET 공급자 구현에 대 한 UI 자동화 패턴 인터페이스는 [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider)입니다.

컨트롤 패턴, 공급자 인터페이스 및 해당 용도에 대 한 목록은 [컨트롤 패턴 및 인터페이스](control-patterns-and-interfaces.md)를 참조 하세요. 컨트롤 형식 목록에 대해서는 [UI 자동화 컨트롤 형식 개요](/windows/desktop/WinAuto/uiauto-controltypesoverview)를 참조 하세요.

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>컨트롤 패턴을 구현 하는 방법에 대 한 지침  
컨트롤 패턴 및 컨트롤 패턴은 UI 자동화 프레임 워크의 더 큰 정의의 일부 이며, UWP 앱에 대 한 액세스 가능성 지원에만 적용 되지 않습니다. 컨트롤 패턴을 구현 하는 경우 이러한 문서와 UI 자동화 사양에 설명 된 지침과 일치 하는 방식으로 구현 하 고 있는지 확인 해야 합니다. 지침을 찾고 있다면 일반적으로 Microsoft 설명서를 사용할 수 있으며 사양을 참조할 필요가 없습니다. 각 패턴에 대 한 지침은 [UI 자동화 컨트롤 패턴 구현](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)에 설명 되어 있습니다. 이 영역에 있는 각 항목에는 "구현 지침 및 규칙" 섹션과 "필수 멤버" 섹션이 있습니다. 이 지침은 일반적으로 [공급자 참조를 위한 컨트롤 패턴 인터페이스](/windows/desktop/WinAuto/uiauto-cpinterfaces) 에 있는 관련 컨트롤 패턴 인터페이스의 특정 api를 나타냅니다. 이러한 인터페이스는 네이티브/COM 인터페이스 이며 해당 Api는 COM 스타일 구문을 사용 합니다. 그러나 표시 되는 모든 항목에는 해당 하는 것이 [**Windows. .xaml. 공급자**](/uwp/api/Windows.UI.Xaml.Automation.Provider) 네임 스페이스에 있습니다.

기본 자동화 피어를 사용 하 고 해당 동작을 확장 하는 경우 해당 피어는 이미 UI 자동화 지침에 따라 작성 되었습니다. 컨트롤 패턴을 지 원하는 경우 [UI 자동화 컨트롤 패턴 구현](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)의 지침을 준수 하는 패턴 지원을 사용할 수 있습니다. 컨트롤 피어에서 UI 자동화로 정의 된 컨트롤 형식에 대 한 대표를 보고 하는 경우 [Ui 자동화 컨트롤 형식 지원](/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) 에 설명 된 지침에 해당 피어가 나옵니다.

그럼에도 불구 하 고 피어 구현에서 UI 자동화 권장 사항을 따르기 위해 컨트롤 패턴 또는 컨트롤 형식에 대 한 추가 지침이 필요할 수 있습니다. 이는 UWP 컨트롤에서 아직 기본 구현으로 존재 하지 않는 패턴 또는 컨트롤 형식 지원을 구현 하는 경우에 특히 그렇습니다. 예를 들어 주석의 패턴은 기본 XAML 컨트롤에서 구현 되지 않습니다. 하지만 주석을 광범위 하 게 사용 하는 앱이 있을 수 있으므로 해당 기능을 액세스할 수 있도록 노출 하려고 합니다. 이 시나리오의 경우 피어는 [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) 을 구현 해야 하며, 문서가 주석을 지원 한다는 것을 나타내는 적절 한 속성을 가진 **문서** 컨트롤 형식으로 자신을 보고 해야 합니다.

Ui 자동화 컨트롤 [패턴을 구현](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) 하는 패턴 또는 [Ui 자동화 컨트롤 형식 지원](/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) 아래에 표시 되는 패턴에 대 한 지침을 방향 및 일반 지침으로 사용 하는 것이 좋습니다. Api의 용도에 대 한 설명 및 설명에 대 한 몇 가지 API 링크를 수행해 볼 수도 있습니다. 그러나 UWP 앱 프로그래밍에 필요한 구문에 대 한 자세한 내용은 해당 API를 [**Windows.**](/uwp/api/Windows.UI.Xaml.Automation.Provider) c o n. c o n. c o n. c o n 네임 스페이스 내에서 확인 하 고 자세한 내용을 보려면 해당 참조 페이지를 사용 하세요.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>기본 제공 자동화 피어 클래스  
일반적으로 요소는 사용자의 UI 작업을 수락 하거나 응용 프로그램의 대화형 또는 의미 있는 UI를 나타내는 보조 기술의 사용자에 게 필요한 정보를 포함 하는 경우 자동화 피어 클래스를 구현 합니다. 일부 UWP 시각적 요소에는 자동화 피어가 없습니다. 자동화 피어를 구현 하는 클래스의 예로는 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 와 [**텍스트 상자가**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)있습니다. 자동화 피어를 구현 하지 않는 클래스의 예로는 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) 및 [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas)와 같은 [**패널**](/uwp/api/Windows.UI.Xaml.Controls.Panel)기반 [**테두리**](/uwp/api/Windows.UI.Xaml.Controls.Border) 및 클래스가 있습니다. **패널** 은 시각적 으로만 표시 되는 레이아웃 동작을 제공 하기 때문에 피어가 없습니다. 사용자가 **패널과**상호 작용할 수 있는 접근성 관련 방법이 없습니다. **패널** 에 포함 된 자식 요소는 모두 UI 자동화 트리에 피어 또는 요소 표현이 있는 트리에서 사용 가능한 다음 부모의 자식 요소로 보고 됩니다.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>UI 자동화 및 UWP 프로세스 경계  
일반적으로 UWP 앱에 액세스 하는 UI 자동화 클라이언트 코드는 out-of-process를 실행 합니다. UI 자동화 프레임 워크 인프라를 사용 하면 프로세스 경계를 넘어 정보를 가져올 수 있습니다. 이 개념에 대해서는 [UI 자동화 기본 사항](/windows/desktop/WinAuto/entry-uiautocore-overview)에 자세히 설명 되어 있습니다.

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>System.windows.uielement.oncreateautomationpeer  
[**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 에서 파생 되는 모든 클래스에는 protected 가상 메서드 [**Oncreateautomationpeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)가 포함 됩니다. Automation 피어의 개체 초기화 시퀀스는 **Oncreateautomationpeer** 를 호출 하 여 각 컨트롤에 대 한 자동화 피어 개체를 가져온 다음 런타임 사용을 위한 UI 자동화 트리를 생성 합니다. UI 자동화 코드는 피어를 사용 하 여 컨트롤의 특징과 기능에 대 한 정보를 가져오고 컨트롤 패턴을 통해 대화형 사용을 시뮬레이션할 수 있습니다. Automation을 지 원하는 사용자 지정 컨트롤은 **Oncreateautomationpeer** 를 재정의 하 고 [**automation피어에서**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)파생 된 클래스의 인스턴스를 반환 해야 합니다. 예를 들어 사용자 지정 컨트롤이 [**Buttonbase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) 클래스에서 파생 되는 경우 **oncreateautomationpeer** 반환 되는 개체는 [**buttonbaseautomation피어에서**](/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer)파생 되어야 합니다.

사용자 지정 컨트롤 클래스를 작성 하 고 새 자동화 피어를 제공 하려는 경우 사용자 지정 컨트롤에 대 한 [**Oncreateautomationpeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 메서드를 재정의 하 여 피어의 새 인스턴스를 반환 하도록 해야 합니다. 피어 클래스는 [**automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)에서 직접 또는 간접적으로 파생 되어야 합니다.

예를 들어 다음 코드는 사용자 지정 컨트롤이 `NumericUpDown` UI 자동화를 위해 피어를 사용 해야 함을 선언 합니다 `NumericUpDownPeer` .

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
> [**Oncreateautomationpeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 구현에서는 사용자 지정 자동화 피어의 새 인스턴스를 초기화 하 고 호출 하는 컨트롤을 소유자로 전달 하 고 해당 인스턴스를 반환 하는 것 외에는 아무 작업도 수행 하지 않아야 합니다. 이 메서드에서 추가 논리를 시도 하지 마십시오. 특히 동일한 호출 내에서 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 를 소멸 시킬 수 있는 모든 논리에서 예기치 않은 런타임 동작이 발생할 수 있습니다.

[**Oncreateautomationpeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)일반적인 구현에서는 메서드 재정의가 컨트롤 클래스 정의의 나머지 부분과 동일한 범위에 있기 때문에 *소유자* 가 **this** 또는 **Me** 로 지정 됩니다.

실제 피어 클래스 정의는 컨트롤과 같은 코드 파일이 나 별도의 코드 파일에서 수행할 수 있습니다. 피어 정의는 피어를 제공 하는 컨트롤과는 별도의 네임 스페이스인 [**Windows. .xaml**](/uwp/api/Windows.UI.Xaml.Automation.Peers) 네임 스페이스에 있습니다. [**Oncreateautomationpeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 메서드 호출에 필요한 네임 스페이스를 참조 하는 한 별도의 네임 스페이스에도 피어를 선언 하도록 선택할 수 있습니다.

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>올바른 피어 기본 클래스 선택  
사용자가 파생 하는 컨트롤 클래스의 기존 피어 논리와 가장 일치 하는 항목을 제공 하는 기본 클래스에서 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 가 파생 되는지 확인 합니다. 이전 예제의 경우가 범위 `NumericUpDown` [**기반**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase)에서 파생 되기 때문에 피어를 기반으로 하는 범위 지정 클래스 [**를**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) 사용할 수 있습니다. 가장 근접 하 게 일치 하는 피어 클래스를 파생 하는 방법에 대해 병렬로 사용 하 여 기본 피어 클래스가 이미 [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) 기능을 구현 하기 때문에 최소한의 기능을 재정의 하지 않을 수 있습니다.

기본 [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control) 클래스에 해당 하는 피어 클래스가 없습니다. **컨트롤**에서 파생 되는 사용자 지정 컨트롤에 해당 하는 피어 클래스가 필요한 경우 [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)에서 사용자 지정 피어 클래스를 파생 시킵니다.

[**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) 에서 직접 파생 하는 경우 피어 클래스를 참조 하는 [**oncreateautomationpeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) 구현이 없으므로 해당 클래스에 기본 자동화 피어 동작이 없습니다. 따라서 사용자 고유의 피어를 사용 하려면 **Oncreateautomationpeer** 를 구현 하거나, 내게 필요한 옵션 지원 수준이 컨트롤에 적절 한 경우 [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) 을 피어로 사용 해야 합니다.

> [!NOTE]
> 일반적으로 [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)가 아니라 [**automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 파생 되지 않습니다. **Automationpeer** 에서 직접 파생 한 경우 **FrameworkElementAutomationPeer**에서 제공 되는 많은 기본 접근성 지원 기능을 복제 해야 합니다.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>사용자 지정 피어 클래스 초기화  
자동화 피어는 기본 초기화에 대 한 소유자 컨트롤의 인스턴스를 사용 하는 형식이 안전한 생성자를 정의 해야 합니다. 다음 예제에서 구현은 *소유자* 값을 범위 [**baseautomation피어**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) 기반에 전달 하 고 궁극적으로 [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) *를 사용 하* 여 [**FrameworkElementAutomationPeer**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner)를 설정 하는 것입니다.

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

## <a name="core-methods-of-automationpeer"></a>AutomationPeer의 핵심 메서드  
UWP 인프라의 경우, 자동화 피어의 재정의 가능 메서드는 UI 자동화 공급자가 UI 자동화 클라이언트에 대 한 전달 지점으로 사용 하는 공용 액세스 메서드와, UWP 클래스가 동작에 영향을 주도록 재정의 될 수 있는 보호 된 "코어" 사용자 지정 메서드를 사용 하는 메서드 쌍의 일부입니다. 액세스 메서드를 호출할 때 항상 공급자 구현이 있는 병렬 "Core" 메서드를 호출 하거나 대체 (fallback)를 사용 하 여 기본 클래스에서 기본 구현을 호출 하는 방식으로 메서드 쌍은 기본적으로 함께 사용 됩니다.

사용자 지정 컨트롤에 대 한 피어를 구현할 때 사용자 지정 컨트롤에 고유한 동작을 노출 하려는 기본 자동화 피어 클래스의 "핵심" 메서드를 재정의 합니다. UI 자동화 코드는 피어 클래스의 공용 메서드를 호출 하 여 컨트롤에 대 한 정보를 가져옵니다. 컨트롤에 대 한 정보를 제공 하려면 컨트롤 구현 및 디자인에서 내게 필요한 옵션 시나리오 또는 기본 자동화 피어 클래스에서 지원 되는 것과 다른 UI 자동화 시나리오를 만들 때 "Core"로 끝나는 이름으로 각 메서드를 재정의 합니다.

최소한 새 피어 클래스를 정의할 때마다 다음 예제와 같이 [**Getclassnamecore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) 메서드를 구현 합니다.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> 문자열을 메서드 본문에 직접 저장 하는 것이 아니라 상수로 저장할 수도 있습니다. [**Getclassnamecore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore)의 경우이 문자열을 지역화할 필요가 없습니다. **LocalizedControlType** 속성은 **CLASSNAME**이 아닌 UI 자동화 클라이언트에서 지역화 된 문자열이 필요할 때마다 사용 됩니다.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

일부 보조 기술에서는 ui 자동화 트리에서 항목의 특성을 보고 하는 경우 UI 자동화 **이름을**벗어나는 추가 정보로 [**GetAutomationControlType**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) 값을 직접 사용 합니다. 컨트롤이 파생 되는 컨트롤과 크게 다른 경우 컨트롤에서 사용 하는 기본 피어 클래스가 보고 하는 것과 다른 컨트롤 형식을 보고 하려면 피어를 구현 하 고 피어 구현에서 [**system.windows.automation.peers.automationpeer.getautomationcontroltypecore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) 를 재정의 해야 합니다. [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 또는 [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl)와 같은 일반화 된 기본 클래스에서 파생 하는 경우 기본 피어에서 컨트롤 형식에 대 한 정확한 정보를 제공 하지 않는 경우에 특히 중요 합니다.

[**System.windows.automation.peers.automationpeer.getautomationcontroltypecore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) 구현에서는 [**AutomationControlType**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 값을 반환 하 여 컨트롤을 설명 합니다. AutomationControlType를 반환할 수 있지만 컨트롤의 주 시나리오를 정확 하 게 설명 하는 경우에는 보다 구체적인 컨트롤 형식 중 하나를 반환 해야 합니다 **.** 예를 들면 다음과 같습니다.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> [**AutomationControlType**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)를 지정 하지 않으면 [**GetLocalizedControlTypeCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) 를 구현 하 여 클라이언트에 **LocalizedControlType** 속성 값을 제공할 필요가 없습니다. UI 자동화 common infrastructure는 **AutomationControlType**이외의 모든 가능한 **AutomationControlType** 값에 대해 번역 된 문자열을 제공 합니다.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern 및 GetPatternCore  
피어의 [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 구현은 입력 매개 변수에서 요청 된 패턴을 지 원하는 개체를 반환 합니다. 특히, UI 자동화 클라이언트는 공급자의 [**Getpattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 메서드에 전달 되는 메서드를 호출 하 고, 요청 된 패턴의 이름을 지정 하는 [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 열거형 값을 지정 합니다. **GetPatternCore** 의 재정의는 지정 된 패턴을 구현 하는 개체를 반환 해야 합니다. 해당 개체는 피어 자체 이며, 피어는 패턴을 지원 한다는 것을 보고할 때마다 해당 패턴 인터페이스를 구현 해야 합니다. 피어에 패턴의 사용자 지정 구현이 없지만 피어의 기반이 패턴을 구현 하는 것을 알고 있는 경우 **GetPatternCore**에서 기본 형식의 **GetPatternCore** 구현을 호출할 수 있습니다. 피어에서 패턴이 지원 되지 않는 경우 피어의 **GetPatternCore** 는 **null** 을 반환 해야 합니다. 그러나 구현에서 직접 **null** 을 반환 하는 대신 일반적으로 기본 구현에 대 한 호출을 사용 하 여 지원 되지 않는 모든 패턴에 대해 **null** 을 반환 합니다.

패턴을 지 원하는 경우 [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 구현에서 **이** 또는 **Me**를 반환할 수 있습니다. UI 자동화 클라이언트는 **null**이 아닌 경우 요청 된 패턴 인터페이스로 [**getpattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 반환 값을 캐스팅 하는 것이 예상 됩니다.

피어 클래스가 다른 피어에서 상속 하 고 필요한 모든 지원 및 패턴 보고가 기본 클래스에서 이미 처리 된 경우 [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 구현은 필요 하지 않습니다. 예를 들어 범위 컨트롤을 구현 하는 경우 범위 [**기반**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase)에서 파생 되 고, 피어가 범위 [**baseautomation피어**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer)에서 파생 되는 경우 해당 피어는 [**PatternInterface 값**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 을 반환 하 고 패턴을 지 원하는 [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) 인터페이스의 작업 구현을 포함 합니다.

이 예제는 리터럴 코드가 아니지만,이 예제에서는 [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 의 구현이 현재 [**범위를 계산**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer)하는 것과 비슷합니다.


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

기본 피어 클래스에서 필요한 모든 지원이 없는 피어를 구현 하거나 피어가 지원할 수 있는 기본 상속 패턴 집합을 변경 하거나 추가 하려는 경우 [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 를 재정의 하 여 UI 자동화 클라이언트가 패턴을 사용할 수 있도록 해야 합니다.

UI 자동화 지원의 UWP 구현에서 사용할 수 있는 공급자 패턴의 목록은 [**Windows. .xaml**](/uwp/api/Windows.UI.Xaml.Automation.Provider). 이러한 각 패턴에는 [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 열거형의 해당 값이 있습니다 .이 값은 UI 자동화 클라이언트가 [**getpattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 호출에서 패턴을 요청 하는 방법입니다.

피어는 둘 이상의 패턴을 지원함을 보고할 수 있습니다. 그렇다면 재정의에는 지원 되는 각 [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 값에 대 한 반환 경로 논리가 포함 되 고 각 일치 하는 대/소문자에 피어가 반환 되어야 합니다. 호출자는 한 번에 하나의 인터페이스만 요청 하 고 호출자는 예상 되는 인터페이스로 캐스팅 해야 합니다.

다음은 사용자 지정 피어에 대 한 [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 재정의의 예입니다. [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) 및 [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider)의 두 가지 패턴에 대 한 지원을 보고 합니다. 이 컨트롤은 전체 화면 (설정/해제 모드)으로 표시 되 고 사용자가 위치 (범위 컨트롤)를 선택할 수 있는 진행률 표시줄이 있는 미디어 표시 컨트롤입니다. 이 코드는 [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)에서 가져온 것입니다.


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

### <a name="forwarding-patterns-from-sub-elements"></a>하위 요소의 전달 패턴  
[**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 메서드 구현에서는 하위 요소 또는 파트를 해당 호스트의 패턴 공급자로 지정할 수도 있습니다. 이 예제에서는 [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 가 내부 [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 컨트롤의 피어에 스크롤 패턴 처리를 전송 하는 방법을 모방 합니다. 패턴 처리에 대 한 하위 요소를 지정 하기 위해이 코드는 하위 요소 개체를 가져오고 [**FrameworkElementAutomationPeer**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) 를 사용 하 여 하위 요소에 대 한 피어를 만들고 새 피어를 반환 합니다.


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

### <a name="other-core-methods"></a>기타 핵심 방법  
컨트롤에서 기본 시나리오에 대 한 키보드 관련 기능을 지원 해야 할 수도 있습니다. 이 작업이 필요한 이유에 대 한 자세한 내용은 [키보드 접근성](keyboard-accessibility.md)을 참조 하세요. 키 지원을 구현 하는 것은 컨트롤 코드의 일부 이며, 컨트롤의 논리의 일부 이기 때문에 피어 코드가 아니라, 피어 클래스가 [**GetAcceleratorKeyCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) 및 [**Getaccesskeycore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) 메서드를 재정의 하 여 사용 되는 키를 UI 자동화 클라이언트에 보고 해야 합니다. 키 정보를 보고 하는 문자열은 지역화 해야 할 수 있으므로 하드 코드 된 문자열이 아닌 리소스에서 가져와야 합니다.

컬렉션을 지 원하는 클래스에 대 한 피어를 제공 하는 경우 함수 클래스와 이미 해당 종류의 컬렉션을 지 원하는 피어 클래스에서 파생 하는 것이 가장 좋습니다. 이렇게 할 수 없는 경우 자식 컬렉션을 유지 관리 하는 컨트롤의 피어는 부모-자식 관계를 UI 자동화 트리에 올바르게 보고 하기 위해 컬렉션 관련 피어 메서드 [**GetChildrenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) 를 재정의 해야 할 수 있습니다.

사용자 인터페이스 (또는 둘 다)에서 컨트롤이 데이터 콘텐츠를 포함 하거나 대화형 역할을 수행할지 여부를 나타내는 [**Iscontentelementcore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) 및 [**iscontentelementcore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) 메서드를 구현 합니다. 기본적으로 두 메서드는 모두 **true**를 반환 합니다. 이러한 설정은 화면 판독기와 같은 보조 기술의 유용성을 향상 시킵니다. 이러한 메서드를 사용 하 여 자동화 트리를 필터링 할 수 있습니다. [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 메서드가 하위 요소 피어로 패턴 처리를 전송 하는 경우 하위 요소 피어의 **Iscontrolelementcore** 메서드에서 **false** 를 반환 하 여 자동화 트리에서 하위 요소 피어를 숨길 수 있습니다.

일부 컨트롤은 텍스트 레이블 파트에서 텍스트가 아닌 부분에 대 한 정보를 제공 하거나 컨트롤을 UI의 다른 컨트롤과 알려진 레이블 지정 관계로 사용 하기 위해 레이블 지정 시나리오를 지원할 수 있습니다. 유용한 클래스 기반 동작을 제공할 수 있는 경우 [**Getlabeledbycore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) 를 재정의 하 여이 동작을 제공할 수 있습니다.

[**GetBoundingRectangleCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) 및 [**GetClickablePointCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) 는 주로 자동화 된 테스트 시나리오에 사용 됩니다. 컨트롤에 대 한 자동화 된 테스트를 지원 하려는 경우 이러한 메서드를 재정의할 수 있습니다. 사용자가 좌표 공간에서 클릭 한 범위가 범위에 다른 영향을 주므로 단일 지점만 제안할 수 없는 범위 형식 컨트롤의 경우이 작업이 필요할 수 있습니다. 예를 들어 기본 [**스크롤 막대**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar) 자동화 피어는 **GetClickablePointCore** 를 재정의 하 여 "Not a number" [**Point**](/uwp/api/Windows.Foundation.Point) 값을 반환 합니다.

[**GetLiveSettingCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) 는 UI 자동화에 대 한 **LiveSetting** 값의 컨트롤 기본값에 영향을 미칩니다. 컨트롤이 [**AutomationLiveSetting**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationLiveSetting)이외의 값을 반환 하도록 하려면이 재정의를 재정의 하는 것이 좋습니다. **LiveSetting** 가 나타내는 항목에 대 한 자세한 내용은 [**LiveSetting**](/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty)를 참조 하세요.

컨트롤에 [**automationorientation**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationOrientation)에 매핑할 수 있는 설정 가능한 방향 속성이 있는 경우 [**GetOrientationCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) 을 재정의할 수 있습니다. [**ScrollBarAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.ScrollBarAutomationPeer) 및 [**SliderAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.SliderAutomationPeer) 클래스에서이 작업을 수행 합니다.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>FrameworkElementAutomationPeer의 기본 구현  
[**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) 의 기본 구현에서는 프레임 워크 수준에서 정의 된 다양 한 레이아웃 및 동작 속성으로 해석할 수 있는 몇 가지 UI 자동화 정보를 제공 합니다.

* [**GetBoundingRectangleCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): 알려진 레이아웃 특성을 기반으로 하는 [**Rect**](/uwp/api/Windows.Foundation.Rect) 구조체를 반환 합니다. [**IsOffscreen**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) 가 **true**인 경우 0 값 **Rect** 를 반환 합니다.
* [**GetClickablePointCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore): 알 수 없는 **BoundingRectangle**이 있는 한 알려진 레이아웃 특성을 기반으로 하는 [**점**](/uwp/api/Windows.Foundation.Point) 구조를 반환 합니다.
* [**Getnamecore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore): 여기에 요약 될 수 있는 것 보다 더 광범위 한 동작입니다. [**Getnamecore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore)를 참조 하십시오. 기본적으로 콘텐츠를 포함 하는 [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) 또는 관련 클래스의 알려진 콘텐츠에서 문자열 변환을 시도 합니다. 또한 [**있으면 labeledby**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95))에 대 한 값이 있는 경우 해당 항목의 **이름** 값이 **이름**으로 사용 됩니다.
* [**HasKeyboardFocusCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): 소유자의 [**FocusState**](/uwp/api/windows.ui.xaml.controls.control.focusstate) 및 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) 속성을 기반으로 평가 됩니다. 컨트롤이 아닌 요소는 항상 **false**를 반환 합니다.
* [**IsEnabledCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): 소유자의 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) 속성 ( [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control)인 경우)을 기반으로 평가 됩니다. 컨트롤이 아닌 요소는 항상 **true**를 반환 합니다. 이는 소유자가 기존 상호 작용 센스에서 사용 하도록 설정 된 것을 의미 하지 않습니다. 이는 소유자가 **IsEnabled** 속성을 갖지 않더라도 피어를 사용할 수 있음을 의미 합니다.
* [**IsKeyboardFocusableCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): Owner가 [**컨트롤인**](/uwp/api/Windows.UI.Xaml.Controls.Control)경우 **true** 를 반환 합니다. 그렇지 않으면 **false**입니다.
* [**IsOffscreenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): owner 요소 또는 해당 부모 항목에서 [**축소**](/uwp/api/windows.ui.xaml.visibility) 된 [**표시 유형은**](/uwp/api/windows.ui.xaml.uielement.visibility) [**IsOffscreen**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen)에 대 한 **true** 값과 같습니다. 예외: 해당 소유자의 부모가이 아닌 경우에도 [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) 개체를 표시할 수 있습니다.
* [**SetFocusCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): [**포커스**](/uwp/api/windows.ui.xaml.controls.control.focus)를 호출 합니다.
* [**Getparent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent): 소유자에서 [**FrameworkElement. 부모**](/uwp/api/windows.ui.xaml.frameworkelement.parent) 를 호출 하 고 적절 한 피어를 조회 합니다. 이는 "Core" 메서드를 사용 하는 재정의 쌍이 아니므로이 동작을 변경할 수 없습니다.

> [!NOTE]
> 기본 UWP 피어는 실제 UWP 코드를 사용할 필요는 없지만 UWP를 구현 하는 내부 네이티브 코드를 사용 하 여 동작을 구현 합니다. CLR (공용 언어 런타임) 리플렉션 또는 기타 기술을 통해 구현에 대 한 코드 또는 논리를 볼 수 없습니다. 기본 피어 동작의 서브 클래스 관련 재정의에 대 한 별도의 참조 페이지도 표시 되지 않습니다. 예를 들어 [**TextBoxAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer)에 대 한 추가 동작이 있을 수 있으며,이는 **automationpeer** 참조 [**페이지에 설명**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore) 되지 않으며, **TextBoxAutomationPeer.GetNameCore**에 대 한 참조 페이지는 없습니다. **TextBoxAutomationPeer** 참조 페이지도 없습니다. 대신, 가장 직접적인 피어 클래스에 대 한 참조 항목을 읽고 설명 섹션에서 구현 메모를 찾습니다.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>피어 및 AutomationProperties  
자동화 피어는 컨트롤의 접근성 관련 정보에 대 한 적절 한 기본값을 제공 해야 합니다. 컨트롤을 사용 하는 모든 앱 코드는 컨트롤 인스턴스에 [**Automationproperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 연결 된 속성 값을 포함 하 여 해당 동작 중 일부를 재정의할 수 있습니다. 호출자는 기본 컨트롤이 나 사용자 지정 컨트롤에 대해이 작업을 수행할 수 있습니다. 예를 들어 다음 XAML은 두 개의 사용자 지정 된 UI 자동화 속성이 있는 단추를 만듭니다. `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

[**Automationproperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 연결 된 속성에 대 한 자세한 내용은 [기본 접근성 정보](basic-accessibility-information.md)를 참조 하세요.

일부 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 메서드는 UI 자동화 공급자가 정보를 보고 하는 방법의 일반적인 계약 때문에 존재 하지만 이러한 메서드는 일반적으로 컨트롤 피어에서 구현 되지 않습니다. 이러한 정보는 특정 UI에서 컨트롤을 사용 하는 응용 프로그램 코드에 적용 된 [**Automationproperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 값에서 제공 될 것으로 예상 되기 때문입니다. 예를 들어 대부분의 앱은 [**있으면 labeledby**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) 값을 적용 하 여 UI에서 서로 다른 두 컨트롤 간의 레이블 지정 관계를 정의 합니다. 그러나 [**Labeledbycore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) 는 헤더 파트를 사용 하 여 데이터 필드 파트에 레이블을 지정 하거나, 컨테이너를 사용 하 여 항목을 레이블 지정 하거나, 유사한 시나리오를 비롯 하 여 컨트롤의 데이터 또는 항목 관계를 나타내는 특정 피어에서 구현 됩니다.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>구현 패턴  
확장-축소를 위해 컨트롤 패턴 인터페이스를 구현 하 여 확장/축소 동작을 구현 하는 컨트롤에 대 한 피어를 작성 하는 방법을 살펴보겠습니다. [**PatternInterface ExpandCollapse**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)값을 사용 하 여 [**getpattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) 이 호출 될 때마다 자체를 반환 하 여 확장-축소 동작에 대 한 액세스 가능성을 피어가 사용 하도록 설정 해야 합니다. 그런 다음 피어는 해당 패턴 ([**IExpandCollapseProvider**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider))에 대 한 공급자 인터페이스를 상속 하 고 해당 공급자 인터페이스의 각 멤버에 대 한 구현을 제공 해야 합니다. 이 경우 인터페이스에는 재정의 하는 세 가지 멤버 ( [**확장**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**축소**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse), [**ExpandCollapseState**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate))가 있습니다.

클래스 자체의 API 디자인에서 접근성에 대해 미리 계획 하는 것이 좋습니다. UI에서 작업하는 사용자의 일반적인 조작 결과로 또는 자동화 공급자 패턴을 통해 잠재적으로 요청되는 동작이 있을 때마다 UI 응답이나 자동화 패턴에서 호출할 수 있는 단일 메서드를 제공합니다. 예를 들어 컨트롤에 컨트롤을 확장 하거나 축소할 수 있는 유선 이벤트 처리기를 포함 하는 단추 파트가 있고 이러한 작업에 대해 키보드를 포함 하는 경우 이러한 이벤트 처리기는 피어의 [**IExpandCollapseProvider**](/windows/desktop/api/uiautomationcore/nn-uiautomationcore-iexpandcollapseprovider) 에 대 한 [**확장**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand) 또는 [**축소**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) 구현의 본문 내에서 호출 하는 것과 동일한 메서드를 호출 합니다. 또한 공통 논리 메서드를 사용 하면 컨트롤의 시각적 상태가 동작의 호출 방식에 관계 없이 일관 된 방식으로 논리적 상태를 표시 하도록 업데이트 되도록 하는 데 유용할 수 있습니다.

일반적인 구현에서는 공급자 Api가 런타임에 컨트롤 인스턴스에 액세스 하기 위해 [**소유자**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) 를 먼저 호출 합니다. 그런 다음 해당 개체에 대해 필요한 동작 메서드를 호출할 수 있습니다.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

대체 구현에서는 컨트롤 자체에서 피어를 참조할 수 있습니다. 이는 [**RaiseAutomationEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 메서드가 피어 메서드 이기 때문에 컨트롤에서 자동화 이벤트를 발생 시키는 경우 일반적인 패턴입니다.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>UI 자동화 이벤트  

UI 자동화 이벤트는 다음 범주로 분류 됩니다.

| 이벤트 | Description |
|-------|-------------|
| 속성 변경 | UI 자동화 요소 또는 컨트롤 패턴의 속성이 변경 될 때 발생 합니다. 예를 들어 클라이언트가 응용 프로그램의 확인란 컨트롤을 모니터링 해야 하는 경우 [**ToggleState**](/uwp/api/windows.ui.xaml.automation.provider.itoggleprovider.togglestate) 속성의 속성 변경 이벤트를 수신 하도록 등록할 수 있습니다. 확인란 컨트롤을 선택 하거나 선택 취소 하면 공급자가 이벤트를 발생 시키고 클라이언트가 필요에 따라 작동할 수 있습니다. |
| 요소 작업 | 사용자 또는 프로그래밍 방식 작업에서 UI가 변경 되 면 발생 합니다. 예를 들어 **호출** 패턴을 통해 단추를 클릭 하거나 호출할 때입니다. |
| 구조 변경 | UI 자동화 트리의 구조가 변경 되 면 발생 합니다. 새 UI 항목이 데스크톱에서 표시 되거나 숨겨지거나 제거 되 면 구조가 변경 됩니다. |
| 전역 변경 | 클라이언트에 대해 전역적으로 관심이 있는 작업 (예: 포커스가 한 요소에서 다른 요소로 이동 하거나 자식 창이 닫히면)이 발생 하는 경우에 발생 합니다. 일부 이벤트는 UI 상태가 변경된 것을 의미하지 않을 수도 있습니다. 예를 들어 사용자가 텍스트 입력 필드를 탭 한 다음 단추를 클릭 하 여 필드를 업데이트 하면 사용자가 실제로 텍스트를 변경 하지 않은 경우에도 [**TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 이벤트가 발생 합니다. 이벤트를 처리할 때 클라이언트 애플리케이션이 작업을 수행하기 전에 실제로 변경된 사항이 있는지 여부를 확인해야 할 수도 있습니다. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>AutomationEvents 식별자  
UI 자동화 이벤트는 [**automationevents**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationEvents) 값으로 식별 됩니다. 열거형의 값은 이벤트의 종류를 고유 하 게 식별 합니다.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>이벤트 발생  
UI 자동화 클라이언트에서 자동화 이벤트를 구독할 수 있습니다. 자동화 피어 모델에서 사용자 지정 컨트롤에 대 한 피어는 [**RaiseAutomationEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 메서드를 호출 하 여 접근성과 관련 된 컨트롤 상태의 변경 내용을 보고 해야 합니다. 마찬가지로, 키 UI 자동화 속성 값이 변경 되 면 사용자 지정 컨트롤 피어는 [**RaisePropertyChangedEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent) 메서드를 호출 해야 합니다.

다음 코드 예제에서는 컨트롤 정의 코드 내에서 피어 개체를 가져오고 메서드를 호출 하 여 해당 피어에서 이벤트를 발생 시키는 방법을 보여 줍니다. 최적화를 위해이 이벤트 유형에 대 한 수신기가 있는지 여부를 코드에서 결정 합니다. 수신기가 있는 경우에만 이벤트를 실행 하 고 피어 개체를 만들어 불필요 한 오버 헤드를 방지 하 고 컨트롤의 응답성을 유지할 수 있습니다.


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
자동화 피어를 찾은 후에는 UI 자동화 클라이언트에서 피어 개체의 [**Getchildren**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildren) 및 [**getchildren**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent) 메서드를 호출 하 여 앱의 피어 구조를 탐색할 수 있습니다. 컨트롤 내의 UI 요소 간 탐색은 피어의 [**GetChildrenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) 메서드 구현에서 지원 됩니다. UI 자동화 시스템은이 메서드를 호출 하 여 컨트롤 내에 포함 된 하위 요소의 트리를 빌드합니다. 예를 들어 목록 상자에 항목을 나열 합니다. [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) 의 기본 **GetChildrenCore** 메서드는 요소의 시각적 트리를 순회 하 여 자동화 피어의 트리를 빌드합니다. 사용자 지정 컨트롤은이 메서드를 재정의 하 여 자식 요소의 다른 표현을 automation 클라이언트에 노출 하 고, 정보를 전달 하거나 사용자 상호 작용을 허용 하는 요소의 자동화 피어를 반환할 수 있습니다.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>텍스트 패턴에 대 한 네이티브 자동화 지원  
일부 기본 UWP 앱 자동화 피어는 텍스트 패턴 ([**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface))에 대 한 컨트롤 패턴 지원을 제공 합니다. 그러나 이러한 메서드는 네이티브 메서드를 통해 이러한 지원을 제공 하며, 관련 된 피어는 관리 되는 상속의 [**Itextprovider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) 인터페이스를 확인 하지 않습니다. 그러나 관리 되거나 관리 되지 않는 UI 자동화 클라이언트에서 패턴에 대 한 피어를 쿼리하면 텍스트 패턴에 대 한 지원을 보고 하 고 클라이언트 Api가 호출 될 때 패턴의 일부에 대 한 동작을 제공 합니다.

UWP 앱 텍스트 컨트롤 중 하나에서 파생 시키고 텍스트 관련 피어 중 하나에서 파생 되는 사용자 지정 피어를 만들려면 피어에 대 한 설명 섹션을 확인 하 여 패턴에 대 한 기본 수준 지원에 대해 자세히 알아보세요. 관리 되는 공급자 인터페이스 구현에서 기본 구현을 호출 하는 경우 사용자 지정 피어의 기본 기본 동작에 액세스할 수 있지만, 피어와 해당 소유자 컨트롤의 네이티브 인터페이스가 노출 되지 않기 때문에 기본 구현에서 수행 하는 작업을 수정 하는 것은 어렵습니다. 일반적으로 기본 구현을 그대로 사용 하거나 (call base 전용) 기능을 자체 관리 코드로 완전히 바꾸고 기본 구현을 호출 하지 않아야 합니다. 후자는 고급 시나리오 이므로 해당 프레임 워크를 사용할 때 내게 필요한 옵션 요구 사항을 지원 하기 위해 컨트롤에서 사용 하는 텍스트 서비스 프레임 워크에 대해 잘 알고 있어야 합니다.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties. AccessibilityView  
사용자 지정 피어를 제공 하는 것 외에도 XAML에서 [**Automationproperties**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview) 를 설정 하 여 모든 컨트롤 인스턴스에 대 한 트리 뷰 표현을 조정할 수 있습니다. 이는 피어 클래스의 일부로 구현 되지 않지만 사용자 지정 컨트롤 또는 사용자 지정 템플릿에 대 한 전반적인 접근성 지원에 germane 때문에 여기에 언급 합니다.

AccessibilityView를 사용 하는 주요 시나리오는 UI 자동화 뷰에서 템플릿의 특정 컨트롤을 의도적으로 생략 하는 것입니다 .이는 전체 컨트롤의 접근성 보기에 영향을 주지 않기 때문입니다 **.** 이를 방지 하려면 **Automationproperties** 을 "Raw"로 설정 합니다.

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>자동화 피어에서 예외 throw  
자동화 피어 지원에 대해 구현 하는 Api는 예외를 throw 할 수 있습니다. 대부분의 예외가 throw 된 후에도 계속 사용할 수 있는 모든 UI 자동화 클라이언트는 정상적으로 작동 합니다. 수신기가 자체 이외의 앱을 포함 하는 모든 자동화 트리를 살펴볼 가능성이 있으므로 클라이언트에서 Api를 호출할 때 트리의 한 영역에서 피어 기반 예외가 발생 했기 때문에 전체 클라이언트를 중단 하는 것은 불가능 한 클라이언트 디자인입니다.

피어로 전달 되는 매개 변수의 경우 입력의 유효성을 검사할 수 있으며, **null** 이 전달 되었고 구현에 유효한 값이 아닌 경우 [**argumentnullexception**](/dotnet/api/system.argumentnullexception) 을 throw 할 수 있습니다. 그러나 피어에서 후속 작업을 수행 하는 경우 호스트 컨트롤과의 피어 상호 작용에는 비동기 문자가 포함 됩니다. 피어가 컨트롤의 UI 스레드를 차단 하지 않아도 되는 것은 아닙니다. 따라서 개체를 사용할 수 있거나, 피어가 만들어지거나 자동화 피어 메서드가 처음 호출 될 때 또는 컨트롤 상태가 변경 된 경우에는 특정 속성을 가진 상황이 발생할 수 있습니다. 이러한 경우에는 공급자가 throw 할 수 있는 두 가지 전용 예외가 있습니다.

* API가 전달 된 원래 정보에 따라 피어의 소유자 또는 관련 피어 요소에 액세스할 수 없는 경우 [**Elementnot사용할**](/dotnet/api/system.windows.automation.elementnotavailableexception) 수 있는 예외를 Throw 합니다. 예를 들어 메서드를 실행 하려고 하지만 닫힌 모달 대화 상자와 같이 소유자가 UI에서 제거 된 후에는 피어가 있을 수 있습니다. Non-.NET client의 경우 [**UIA \_ E \_ elementnotavailable**](/windows/desktop/WinAuto/uiauto-error-codes)에 매핑됩니다.
* 여전히 소유자 이지만 해당 소유자가 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled)false와 같은 모드에 있는 경우에는 피어에서 [**system.windows.automation.elementnotenabledexception**](/dotnet/api/system.windows.automation.elementnotenabledexception) 에서 `=` **false** 수행 하려고 하는 특정 프로그래밍 변경 내용의 일부를 차단 하는 경우에 Throw 됩니다. Non-.NET client의 경우 [**UIA \_ E \_ elementnotenabled**](/windows/desktop/WinAuto/uiauto-error-codes)에 매핑됩니다.

이 외에 피어는 피어 지원에서 throw 되는 예외와 관련 하 여 상대적으로는 안 됩니다. 대부분의 클라이언트는 피어의 예외를 처리 하 고 클라이언트와 상호 작용할 때 사용자가 수행할 수 있는 실행 가능한 선택 항목으로 전환할 수 없습니다. 따라서 피어 구현 내에서 다시 throw 없는 경우에도 작동 하지 않고 예외를 catch 하는 것은 피어가 작동 하지 않는 항목이 발생할 때마다 예외를 throw 하는 것 보다 더 나은 전략입니다. 또한 대부분의 UI 자동화 클라이언트는 관리 코드로 작성 되지 않습니다. 대부분은 COM으로 작성 되며, 피어에 대 한 액세스를 종료 하는 UI 자동화 클라이언트 메서드를 호출할 때마다 **HRESULT** 에서 ** \_ OK** 를 확인 하는 것입니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)
* [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
* [**System.windows.uielement.oncreateautomationpeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)
* [컨트롤 패턴 및 인터페이스](control-patterns-and-interfaces.md)