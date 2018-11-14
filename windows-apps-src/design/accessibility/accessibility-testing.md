---
author: Xansky
Description: Testing procedures to follow to ensure that your Universal Windows Platform (UWP) app is accessible.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: 접근성 테스트
label: Accessibility testing
template: detail.hbs
ms.author: mhopkins
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b23d06f84a0d798a1e8b9c45a41d33751037402
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6465967"
---
# <a name="accessibility-testing"></a>접근성 테스트  

UWP(유니버설 Windows 플랫폼) 앱에 접근성을 구현하기 위해 따라야 할 테스트 절차입니다.

<span id="run_accessibility_testing_tools"/>
<span id="RUN_ACCESSIBILITY_TESTING_TOOLS"/>

## <a name="run-accessibility-testing-tools"></a>접근성 테스트 도구 실행  
Windows SDK(소프트웨어 개발 키트)에는 [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239), [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 및 [**UI Accessibility Checker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) 같은 여러 가지 접근성 테스트 도구가 포함되어 있습니다. 이 도구는 앱의 접근성을 검증하는 데 도움이 될 수 있습니다. 모든 앱 시나리오와 UI 요소를 확인하세요.

Microsoft Visual Studio 명령 프롬프트 또는 Windows SDK 도구 폴더(개발 컴퓨터에서 Windows SDK가 설치되어 있는 위치의 bin 하위 디렉터리)에서 접근성 테스트 도구를 시작할 수 있습니다.
  
> [!VIDEO https://www.youtube.com/embed/ce0hKQfY9B8]
  
<span id="AccScope"/>
<span id="accscope"/>
<span id="ACCSCOPE"/>

### **<a name="accscope"></a>AccScope**  

[**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 도구를 사용하면 개발자와 테스터가 앱 개발 및 디자인 중 앱 개발 주기의 이후 테스트 단계가 아니라 초기 프로토타입 단계에서 앱의 접근성을 평가할 수 있습니다. 특히 앱에서 내레이터 접근성 시나리오를 테스트하는 데 사용됩니다.

<span id="inspect"/>
<span id="INSPECT"/>

### **<a name="inspect"></a>검사**  

[**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521)를 사용하면 원하는 UI 요소를 선택하고 그 접근성 데이터를 볼 수 있습니다. 사용자는 Microsoft UI 자동화 속성 및 컨트롤 패턴을 보고 UI 자동화 트리에서 자동화 요소의 탐색 구조를 테스트할 수 있습니다. UI를 개발할 때 **Inspect**를 사용하여 접근성 특성이 UI 자동화에 어떻게 표시되는지 확인하세요. 경우에 따라 특성은 기본 XAML 컨트롤에 대해 이미 구현된 UI 자동화 지원에서 제공됩니다. 다른 경우 특성은 XAML 태그에서 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties) 연결된 속성으로 설정한 특정 값에서 제공됩니다.

다음 이미지는 메모장에서 **편집** 메뉴 요소의 UI 자동화 속성을 쿼리하는 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 도구를 보여 줍니다.

![검사 도구의 스크린샷](./images/inspect.png)

<span id="ui_accessibility_checker"/>
<span id="UI_ACCESSIBILITY_CHECKER"/>

### **<a name="ui-accessibility-checker"></a>UI 접근성 검사기**  
**UI Accessibility Checker (AccChecker)** 를 사용하면 런타임에 접근성 문제를 알아내는 데 도움이 됩니다. 완전하고 기능적인 UI인 경우 **AccChecker**를 사용하여 다양한 시나리오를 테스트하고, 런타임 접근성 정보의 정확도를 검증하고, 런타임 문제를 찾을 수 있습니다. UI 또는 명령줄 모드에서 **AccChecker**를 실행할 수 있습니다. UI 모드 도구를 실행하려면 Windows SDK bin 디렉터리의 **AccChecker** 디렉터리를 열고 acccheckui.exe를 실행한 다음 **도움말** 메뉴를 클릭합니다.

<span id="ui_automation_verify"/>
<span id="UI_AUTOMATION_VERIFY"/>

### **<a name="ui-automation-verify"></a>UI 자동화 검증**  
**UI Automation Verify (UIA Verify)** 은 UI 자동화 구현을 위해 자동화된 테스트 및 검증 프레임워크입니다. **UIA 검증**은 테스트 코드에 통합되어 UI 자동화 시나리오의 정기적인 자동화된 테스트 또는 스팟 체킹을 수행할 수 있습니다. **UIA 검증**을 실행하려면 UIAVerify 하위 디렉터리에서 VisualUIAVerifyNative.exe를 실행합니다.

<span id="accessible_event_watcher"/>
<span id="ACCESSIBLE_EVENT_WATCHER"/>

### **<a name="accessible-event-watcher"></a>접근성 있는 이벤트 감시자**  
[**Accessible Event Watcher (AccEvent)**](https://msdn.microsoft.com/library/windows/desktop/Dd317979)에서는 UI가 변경될 때 앱의 UI 요소가 적절한 UI 자동화 및 Microsoft Active Accessibility 이벤트를 발생시키는지 테스트합니다. UI는 포커스가 변경되거나 UI 요소가 호출, 선택 또는 상태나 속성이 변경될 때 변경될 수 있습니다.

> [!NOTE]
> 설명서에 나오는 대부분의 접근성 테스트 도구는 PC에서 실행되며 휴대폰에서는 실행되지 않습니다. 개발하고 에뮬레이터를 사용하는 동안 일부 도구를 실행할 수 있지만, 이 도구의 대부분은 에뮬레이터 내에서 UI 자동화 트리를 노출할 수 없습니다.

<span id="test_keyboard_accessibility"/>
<span id="TEST_KEYBOARD_ACCESSIBILITY"/>

## <a name="test-keyboard-accessibility"></a>키보드 접근성 테스트  
키보드 접근성을 테스트하는 가장 좋은 방법은 마우스를 사용하지 않거나, 태블릿 장치를 사용하는 경우 화상 키보드를 사용하는 것입니다. _Tab_ 키를 사용하여 키보드 접근성 탐색을 테스트합니다. _Tab_ 키로 모든 대화형 UI를 순환할 수 있어야 합니다. 복합 UI 요소의 경우 화살표 키로 요소의 여러 부분을 탐색할 수 있는지 확인합니다. 예를 들어 키보드 키를 사용하여 항목 목록을 탐색할 수 있어야 합니다. 마지막으로 해당 요 소에 포커스가 있는 경우 키보드(주로 Enter 키 또는 스페이스바)로 모든 대화형 UI 요소를 호출할 수 있는지 확인합니다.

<span id="verify_the_contrast_ratio_of_visible_text"/>
<span id="VERIFY_THE_CONTRAST_RATIO_OF_VISIBLE_TEXT"/>

## <a name="verify-the-contrast-ratio-of-visible-text"></a>표시되는 텍스트의 대비 확인  
색상 대비 도구를 사용하여 표시되는 텍스트 명암비가 허용되는지 검증합니다. 여기서 비활성 UI 요소 및 로고, 또는 정보를 전달하지 않으며 그 의미를 변경하지 않고 다시 정렬할 수 있는 장식 텍스트는 예외입니다. 명암비 및 예외에 대한 자세한 내용은 [접근성 있는 텍스트 요구 사항](accessible-text-requirements.md)을 참조하세요. 명암비를 테스트할 수 있는 도구는 [Techniques for WCAG 2.0 G18(리소스 섹션)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)을 참조하세요.

> [!NOTE]
> Techniques for WCAG 2.0 G18에 나열된 도구 중 일부는 UWP 앱에서 대화형으로 사용할 수 없습니다. 도구에서 수동으로 전경색 및 배경색 값을 입력하고, 앱 UI의 화면 캡처를 만든 다음 화면 캡처 이미지에서 대비 비율 도구를 실행하거나, 앱에서 해당 이미지를 로드할 때가 아니라 이미지 편집 프로그램에서 원본 비트맵 파일을 열 때 도구를 실행해야 할 수도 있습니다.

<span id="verify_your_app_in_high_contrast"/>
<span id="VERIFY_YOUR_APP_IN_HIGH_CONTRAST"/>

## <a name="verify-your-app-in-high-contrast"></a>고대비에서 앱 확인  
고대비 테마가 활성 상태인 동안 앱의 모든 UI 요소가 올바르게 표시되는지 확인합니다. 모든 텍스트는 읽을 수 있어야 하고 모든 이미지는 또렷해야 합니다. XAML 테마 사전 리소스 도는 컨트롤 템플릿을 조정하여 컨트롤에서 발생되는 테마 문제를 해결합니다. 주요 고대비 문제가 테마 또는 컨트롤(예제: 이미지 파일의 컨트롤)에서 발생되지 않는 경우 고대비 테마가 활성화된 경우 사용할 별도의 버전을 제공합니다.

<span id="verify_your_app_with_make_everything_on_your_screen_bigger"/>
<span id="VERIFY_YOUR_APP_WITH_MAKE_EVERYTHING_ON_YOUR_SCREEN_BIGGER"/>

## <a name="verify-your-app-with-display-settings"></a>디스플레이 설정으로 앱 확인  

디스플레이의 dpi(인치당 도트 수) 값을 조정하는 시스템 디스플레이 옵션을 사용하고 dpi 값이 변경되면 앱 UI 크기가 올바르게 조정되도록 합니다. 일부 사용자는 접근성 옵션으로 DPI 값을 변경하며 이 옵션은 **접근성** 또는 디스플레이 속성에서 사용할 수 있습니다. 문제가 있으면 [레이아웃 크기 조정 지침](https://msdn.microsoft.com/library/windows/apps/Dn611863)에 따라 다른 크기 조정 인수에 대한 추가 리소스를 제공합니다.

<span id="verify_main_app_scenarios_by_using_narrator"/>
<span id="VERIFY_MAIN_APP_SCENARIOS_BY_USING_NARRATOR"/>

## <a name="verify-main-app-scenarios-by-using-narrator"></a>내레이터를 사용하여 메인 앱 시나리오 확인  
내레이터를 사용해 앱의 화면 읽기 환경을 테스트하세요.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**마우스와 키보드로 내레이터를 사용하여 앱을 테스트하려면 다음 단계를 따르세요.**
1.  _Windows 로고 키 + Ctrl + Enter_를 눌러 내레이터를 시작합니다. Windows 10 버전 1607 이전 버전에서는 _Windows 로고 키 + Enter_를 눌러 내레이터를 시작합니다.
2.  키보드의 _Tab_ 키, 화살표 키 및 _Caps Lock + 화살표 키_를 사용하여 앱을 탐색합니다.
3.  앱을 탐색할 때 내레이터가 UI의 요소를 읽는 내용을 듣고 다음을 확인합니다.
    * 각 컨트롤에 대해 내레이터가 표시되는 모든 콘텐츠를 읽는지 확인합니다. 또한 내레이터가 각 컨트롤의 이름, 해당 상태(확인됨, 선택됨 등) 및 컨트롤 형식(단추, 확인란, 목록 항목 등)을 읽는지 확인합니다.
    * 요소가 대화형인 경우 내레이터를 사용하여 _Caps Lock + Enter_를 누르면 해당 작업을 호출할 수 있는지 확인합니다.
    * 각 테이블에 대해 내레이터가 테이블 이름, 테이블 설명(있는 경우), 행 및 열 제목을 올바로 읽는지 확인합니다.

4.  _Caps Lock + Shift + Enter_를 눌러 앱을 검색하고 모든 컨트롤이 검색 목록에 표시되고 컨트롤 이름이 지역화되어 읽을 수 있는 상태인지 확인합니다.
5.  모니터를 끄고 키보드와 내레이터만 사용하여 메인 앱 시나리오를 수행해 봅니다. 내레이터 명령과 바로 가기의 전체 목록을 표시하려면 _Caps Lock + F1_을 누릅니다.

Windows 10 버전 1607부터 내레이터에 새 개발자 모드가 도입되었습니다. 내레이터가 이미 실행 중인 경우 _Caps Lock + Shift + F12_를 눌러 개발자 모드를 켭니다. 개발자 모드를 사용하면 화면이 마스크되고 액세스할 수 있는 개체와 내레이터에 프로그래밍 방식으로 노출되는 관련 텍스트만 강조 표시됩니다. 이렇게 하면 내레이터에 노출되는 정보를 한눈에 확인할 수 있습니다.

**다음 단계에 따라 내레이터의 터치 모드를 사용하여 앱 테스트**

> [!NOTE]
> 4개 이상의 접점을 지원하는 장치에서는 내레이터가 터치 모드를 자동으로 시작합니다. 내레이터는 주 화면에서 다중 모니터 시나리오나 멀티 터치 디지타이저를 지원하지 않습니다.

1.  UI에 익숙해지고 레이아웃을 탐색합니다.

    * **한 손가락으로 살짝 밀기 제스처를 사용하여 UI 탐색.** 왼쪽 또는 오른쪽 살짝 밀기를 사용하여 항목 간을 이동하고 위쪽 또는 아래쪽 살짝 밀기를 사용하여 탐색하는 항목의 범주를 변경합니다. 범주에는 모든 항목, 링크, 테이블, 머리글 등이 포함됩니다. 한 손가락으로 살짝 밀기 제스처를 사용한 탐색은 _Caps Lock + Arrow_를 사용한 탐색과 유사합니다.
    * **탭 제스처를 사용하여 포커스 가능 요소 탐색.** 오른쪽 또는 왼쪽으로 세 손가락 살짝 밀기는 키보드의 _Tab_ 키 및 _Shift + Tab_을 사용한 탐색과 유사합니다.
    * **한 손가락으로 UI 공간 탐색.** 한 손가락을 위쪽과 아래쪽 또는 왼쪽과 오른쪽으로 끌어 내레이터가 손가락 아래의 항목을 읽게 합니다. 마우스도 한 손가락 끌기와 동일한 적중 테스트 논리를 사용하므로 마우스를 대신 사용할 수 있습니다.
    * **세 손가락 위로 살짝 밀기로 전체 창과 모든 콘텐츠 읽기**. 이 동작은 _Caps Lock + W_를 사용하는 것과 동일합니다.

    중요 UI에 연결할 수 없는 경우 접근성 문제가 발생할 수 있습니다.

2.  컨트롤을 조작하여 컨트롤의 주 및 보조 작업과 스크롤 동작을 테스트합니다.

    주 작업에는 단추 활성화, 텍스트 캐럿 배치, 컨트롤에 포커스 설정 등과 같은 작업이 포함되고, 보조 작업에는 목록 항목 선택, 여러 옵션을 제공하는 단추 확장 등과 같은 작업이 포함됩니다.

    * 주 작업 테스트: 두 번 탭하거나 한 손가락으로 누른 상태에서 다른 손가락으로 탭합니다.
    * 보조 작업 테스트: 세 번 탭하거나 한 손가락으로 누른 상태에서 다른 손가락으로 두 번 탭합니다.
    * 스크롤 동작 테스트: 두 손가락 살짝 밀기를 사용하여 원하는 방향으로 스크롤합니다.

    일부 컨트롤은 추가 작업을 제공합니다. 전체 목록을 표시하려면 네 손가락으로 한 번 탭합니다.

    컨트롤이 마우스 또는 키보드에는 응답하지만 주 또는 보조 터치 조작에는 응답하지 않는 경우에는 컨트롤에서 추가 [UI 자동화](https://msdn.microsoft.com/library/windows/desktop/Ee684009) 컨트롤 패턴을 구현해야 합니다.

[**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 도구를 사용하여 앱에서 내레이터 접근성 시나리오를 테스트하는 것도 고려해야 합니다. [**AccScope tool topic**](https://msdn.microsoft.com/library/windows/desktop/Dn433239)에서는 내레이터 시나리오를 테스트하도록 **AccScope**을 구성하는 방법에 대해 설명합니다.

<span id="Examine_the_UI_Automation_representation_for_your_app"/>
<span id="examine_the_ui_automation_representation_for_your_app"/>
<span id="EXAMINE_THE_UI_AUTOMATION_REPRESENTATION_FOR_YOUR_APP"/>

## <a name="examine-the-ui-automation-representation-for-your-app"></a>앱의 UI 자동화 표현 검사  
앞에서 언급한 다수의 UI 자동화 테스트 도구를 사용하면 의도적으로 앱의 모양을 고려하지 않고 대신 앱을 UI 자동화 요소 구조로 표현하는 방법으로 앱을 볼 수 있습니다. 이는 UI 자동화 클라이언트, 주로 보조 기술이 접근성 시나리오에서 앱과 상호 작용하는 방법입니다.

[**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 도구는 UI 자동화 요소를 시각적 표현이나 목록으로 표시할 수 있어 특히 흥미로운 앱 보기를 제공합니다. 시각화를 사용하면 앱 UI의 시각적 모양과 서로 연결할 수 있는 방법을 통해 부분으로 드릴다운할 수 있습니다. UI에 논리를 할당하기 전에 초기 UI 프로토타입의 접근성을 테스트할 수도 있어, 이를 통해 시각적 상호 작용과 앱의 접근성 시나리오 탐색이 모두 균형을 유지하는지 확인할 수 있습니다.

테스트할 수 있는 한 가지 측면은 UI 자동화 요소 보기에 표시하지 않으려는 요소가 나타나는지 여부입니다. 보기에 표시하지 않으려는 요소가 발견되는 경우나 이와 반대로 누락된 요소가 있는 경우 XAML 연결된 속성 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview)를 사용하여 접근성 보기에 XAML 컨트롤이 나타나는 방식을 조정할 수 있습니다. 기본 접근성 보기를 살펴본 후에는 화살표 키를 통해 활성화되는 탭 시퀀스나 공간 탐색을 다시 점검하여 사용자가 컨트롤 보기에 표시되는 각 대화형 부분에 도달할 수 있는지 확인하는 것도 좋습니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [피해야 할 사례](practices-to-avoid.md)
* [UI 자동화](https://msdn.microsoft.com/library/windows/desktop/Ee684009)
* [Windows의 접근성](http://go.microsoft.com/fwlink/p/?LinkId=320802)
* [내레이터 시작](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
