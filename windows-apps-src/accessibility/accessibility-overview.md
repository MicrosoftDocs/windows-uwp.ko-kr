---
author: Xansky
Description: "이 문서는 UWP(유니버설 Windows 플랫폼) 앱의 접근성 시나리오와 관련된 개념 및 기술에 대한 개요입니다."
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
title: "접근성 개요"
label: Accessibility overview
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a03953885179cf8e969e3b35a426aa958c528f54
ms.lasthandoff: 02/07/2017

---

# <a name="accessibility-overview"></a>접근성 개요  




이 문서는 UWP(유니버설 Windows 플랫폼) 앱의 접근성 시나리오와 관련된 개념 및 기술에 대한 개요입니다.

<span id="Accessibility_and_your_app"/>
<span id="accessibility_and_your_app"/>
<span id="ACCESSIBILITY_AND_YOUR_APP"/>
## <a name="accessibility-and-your-app"></a>접근성 및 앱  
장애 형식은 신체의 움직임, 시각, 색 지각, 청각, 말하기, 인지 능력, 읽고 쓰는 능력 제약 등을 비롯하여 매우 다양합니다. 그러나 여기에 제공된 지침을 따르면 대부분의 요구 사항을 충족할 수 있습니다. 이는 다음과 같은 경우를 의미합니다.

* 키보드 조작 및 화면 읽기 프로그램 지원
* 글꼴, 확대/축소 설정(확대), 색 및 고대비 설정 지원
* UI의 부분에 대한 대체 또는 보완

XAML용 컨트롤은 기본적으로 키보드를 지원할 뿐만 아니라 및 이미 UWP 앱, HTML 및 기타 UI 기술을 지원하는 접근성 프레임워크를 이용하는 화면 읽기 프로그램과 같은 보조 기술을 지원합니다. 이 기본 제공 지원을 통해 몇몇 속성만 설정하는 약간의 작업으로 사용자 지정할 수 있는 기본적인 수준의 접근성이 구현됩니다. 사용자 지정 XAML 구성 요소 및 컨트롤을 직접 만들려는 경우 *자동화 피어*의 개념을 사용하여 해당 컨트롤에 유사한 지원을 추가할 수도 있습니다.

또한 데이터 바인딩, 스타일 및 템플릿 기능을 사용하면 디스플레이 설정 및 대체 UI용 텍스트에 대한 동적 변경 지원을 쉽게 구현할 수 있습니다.

<span id="UI_Automation"/>
<span id="ui_automation"/>
<span id="UI_AUTOMATION"/>
## <a name="ui-automation"></a>UI 자동화  
접근성 지원은 주로 Microsoft UI 자동화 프레임워크에 대한 통합 지원에서 제공됩니다. 이 지원은 기본 클래스, 컨트롤 형식에 대한 클래스 구현의 기본 제공 동작, UI 자동화 공급자 API의 인터페이스 표현 등을 통해 제공됩니다. 각 컨트롤 클래스에서는 컨트롤의 역할 및 콘텐츠를 UI 자동화 클라이언트에 보고하는 자동화 피어 및 자동화 패턴이라는 UI 자동화 개념을 사용합니다. 이 앱은 UI 자동화에서 최상위 창으로 처리되며, UI 자동화 프레임워크를 통해 해당 앱 창에 포함된 모든 접근성 관련 콘텐츠를 UI 자동화 클라이언트에서 사용할 수 있습니다. UI 자동화에 대한 자세한 내용은 [UI 자동화 개요](https://msdn.microsoft.com/library/windows/desktop/Ee684076)를 참조하세요.

<span id="Assistive_technology"/>
<span id="assistive_technology"/>
<span id="ASSISTIVE_TECHNOLOGY"/>
## <a name="assistive-technology"></a>보조 기술  
대부분의 사용자 접근성 요구 사항은 사용자가 설치하는 보조 기술 제품 또는 운영 체제에서 제공하는 도구 및 설정으로 충족됩니다. 여기에는 화면 읽기 프로그램, 화면 돋보기, 고대비 설정 등의 기능이 포함됩니다.

보조 기술 제품에는 광범위한 소프트웨어 및 하드웨어가 포함됩니다. 이러한 제품은 UI의 콘텐츠 및 구조에 대한 정보를 화면 읽기 프로그램 및 기타 보조 기술에 보고하는 접근성 프레임워크 및 표준 키보드 인터페이스를 통해 작동합니다. 보조 기술 제품의 예는 다음과 같습니다.

* 화상 키보드 - 사용자가 텍스트를 입력할 때 키보드 대신 포인터를 사용하도록 합니다.
* 음성 인식 소프트웨어 - 음성을 입력 텍스트로 변환합니다.
* 화면 읽기 프로그램 - 텍스트를 음성이나 브라유 점자 같은 다른 형식으로 변환합니다.
* 내레이터 화면 읽기 프로그램 - 특히 Windows의 일부입니다. 내레이터에는 사용 가능한 키보드가 없을 때 터치 제스처를 처리하여 화면 판독 작업을 수행할 수 있는 터치 모드가 있습니다.
* 디스플레이 또는 디스플레이 영역을 조정하는 프로그램 또는 설정(예제: 고대비 테마, 디스플레이의 dpi(인치당 도트 수) 설정 또는 돋보기 도구).

성능이 좋은 키보드와 화면 읽기 프로그램이 지원되는 앱은 일반적으로 다양한 보조 기술 제품에서 잘 작동합니다. 대부분의 경우 UWP 앱은 정보나 구조를 추가로 수정하지 않고도 다양한 보조 기술 제품과 호환됩니다. 그러나 최적의 접근성 환경을 위해 일부 설정을 수정하거나 추가 지원을 구현해야 할 수 있습니다.

보조 기술이 포함된 기본 접근성 시나리오를 테스트하는 데 사용할 수 있는 일부 옵션은 [접근성 테스트](accessibility-testing.md)에 나와 있습니다.

<span id="Screen_reader_support_and_basic_accessibility_information"/>
<span id="screen_reader_support_and_basic_accessibility_information"/>
<span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"/>
## <a name="screen-reader-support-and-basic-accessibility-information"></a>화면 읽기 프로그램 지원 및 기본 접근성 정보  
화면 읽기 프로그램은 앱의 텍스트를 음성 언어나 점자 출력 등, 다른 형식으로 렌더링하여 그에 대한 액세스를 제공합니다. 화면 읽기 프로그램의 정확한 동작은 소프트웨어 및 소프트웨어의 사용자 구성에 따라 다릅니다.

예를 들어 어떤 화면 읽기 프로그램은 사용자가 앱을 시작하거나 표시 중인 앱을 전환하면 탐색하기 전 사용 가능한 모든 정보 콘텐츠를 사용자가 받을 수 있도록 전체 앱 UI를 읽습니다. 또 어떤 화면 읽기 프로그램은 탭 탐색 중 포커스를 받으면 개별 컨트롤과 연결된 텍스트도 읽어 줍니다. 이렇게 하면 사용자가 응용 프로그램의 입력 컨트롤을 탐색할 때 자신의 위치를 알 수 있습니다. 내레이터는 사용자의 선택에 따라 두 가지 동작을 모두 제공하는 화면 읽기 프로그램의 예입니다.

사용자가 앱을 이해하거나 탐색하는 데 도움을 주기 위해 화면 읽기 프로그램이나 다른 모든 보조 기술에 필요한 가장 중요한 정보는 앱의 요소 부분의 *접근성 있는 이름*입니다. 많은 경우 컨트롤이나 요소에는 제공한 다른 속성 값에서 계산되는 접근성 있는 이름이 이미 있습니다. 이미 계산된 이름을 사용할 수 있는 가장 일반적인 경우는 내부 텍스트를 지원하고 표시하는 요소를 사용할 때입니다. 다른 요소의 경우 요소 구조에 대한 모범 사례에 따라 접근성 있는 이름을 제공할 다른 방법을 고려해야 할 수 있습니다. 경우에 따라 앱 접근성을 위한 접근성 있는 이름으로 명시적으로 사용되는 이름을 제공해야 합니다. 공용 UI 요소에서 사용되는 계산된 값 목록 및 일반적인 접근성 있는 이름에 대한 자세한 내용은 [기본 접근성 정보](basic-accessibility-information.md)를 참조하세요.

이 외에도 다음 섹션에서 설명하는 키보드 속성을 비롯하여 사용 가능한 몇 가지 다른 자동화 속성이 있습니다. 단, 모든 화면 읽기 프로그램이 모든 자동화 속성을 지원하지는 않습니다. 화면 읽기 프로그램에 대해 가능한 한 최대 지원을 제공하려면 적절한 자동화 속성을 설정하고 테스트해야 하는 것이 일반적입니다.

<span id="Keyboard_support"/>
<span id="keyboard_support"/>
<span id="KEYBOARD_SUPPORT"/>
## <a name="keyboard-support"></a>키보드 지원  
우수한 키보드 지원을 구현하려면 응용 프로그램의 각 부분을 키보드로 사용할 수 있어야 합니다. 앱에서 대부분 표준 컨트롤을 사용하고 사용자 지정 컨트롤을 사용하지 않는 경우 이미 최적의 접근성 환경을 구현하고 있는 것입니다. 기본 XAML 컨트롤 모델은 탭 탐색, 텍스트 입력 및 컨트롤별 지원을 비롯한 키보드 지원을 기본적으로 제공합니다. 레이아웃 컨테이너(예: 패널)로 사용되는 요소는 레이아웃 순서를 사용하여 기본 탭 순서를 설정합니다. 종종 해당 순서는 UI의 접근성 있는 표현에 사용할 올바른 탭 순서가 됩니다. 데이터를 표시하는 데 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 및 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 컨트롤을 사용하는 경우 화살표 키 탐색이 기본 제공됩니다. 또는 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 컨트롤을 사용하는 경우 이 컨트롤에서 단추 활성화를 위해 스페이스바나 Enter 키를 이미 처리합니다.

탭 순서 및 키 기반 활성화 또는 탐색을 포함하여 키보드 지원의 모든 측면에 대한 자세한 내용은 [키보드 접근성](keyboard-accessibility.md)을 참조하세요.

<span id="Media_and_captioning"/>
<span id="media_and_captioning"/>
<span id="MEDIA_AND_CAPTIONING"/>
## <a name="media-and-captioning"></a>미디어 및 캡션  
일반적으로 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) 개체를 통해 시청각 미디어를 표시합니다. **MediaElement** API를 사용하여 미디어 재생을 제어할 수 있습니다. 접근성을 목적으로 하는 경우 사용자가 필요에 맞게 미디어를 재생, 일시 중지, 중지할 수 있도록 하는 컨트롤을 제공합니다. 경우에 따라 미디어에는 자막이나 내레이터 설명을 포함하는 대체 오디오 트랙과 같이 접근성을 위한 추가 구성 요소가 포함됩니다.

<span id="Accessible_text"/>
<span id="accessible_text"/>
<span id="ACCESSIBLE_TEXT"/>
## <a name="accessible-text"></a>접근성 있는 텍스트  
다음과 같은 텍스트의 세 가지 주요 측면이 접근성과 관련이 있습니다.

* 도구에서 텍스트를 탭 시퀀스 통과의 일부로 읽을 수 있는지 전체 문서 표현의 일부로만 읽을 수 있는지를 결정해야 합니다. 텍스트를 표시하는 데 적절한 요소를 선택하거나 해당 텍스트 요소의 속성을 조정하여 이 결정을 제어할 수 있습니다. 각 텍스트 요소에는 특정 용도가 있으며 이 용도에는 대개 해당하는 UI 자동화 역할이 있습니다. 잘못된 요소를 사용하면 잘못된 역할을 UI 자동화에 보고하여 보조 기술 사용자에게 혼란을 줄 수 있습니다.
* 텍스트가 배경과 적절한 대비를 이루지 못할 경우 많은 사용자가 텍스트를 읽기 어렵게 만드는 약간의 제한이 있습니다. 이 제한이 사용자에게 영향을 주는 방식은 해당 제한이 없는 앱 디자이너에게 직관적이지 않을 수 있습니다. 예를 들어 색맹인 사용자의 경우 디자인 단계에서 색을 잘못 선택하면 일부 사용자가 텍스트를 읽을 수 없습니다. 처음에 웹 콘텐츠용으로 만들어진 접근성 요구 사항은 앱에서도 이러한 문제가 발생하지 않도록 하는 대비 표준을 정의합니다. 자세한 내용은 [접근성 있는 텍스트 요구 사항](accessible-text-requirements.md)을 참조하세요.
* 많은 사용자는 매우 작은 텍스트를 읽는 데 어려움을 겪습니다. 앱 UI의 텍스트를 첫 번째 위치에서 적절히 크게 표시하면 이 문제를 방지할 수 있습니다. 그러나 많은 양의 텍스트를 표시하거나 다른 시각적 요소로 텍스트가 분산되는 앱에서는 그리 쉽지 않습니다. 이런 경우 앱이 디스플레이를 확대하여 앱의 텍스트가 함께 확대되도록 하는 시스템 기능을 올바르게 조작하도록 해야 합니다. 일부 사용자는 접근성 옵션으로 dpi 값을 변경합니다. 이 옵션은 **접근성**의 **화면의 항목을 더 크게 표시**에서 사용할 수 있으며 **모양 및 개인 설정** / **디스플레이**를 위한 **제어판** UI로 리디렉션됩니다.

<span id="Supporting_high-contrast_themes"/>
<span id="supporting_high-contrast_themes"/>
<span id="SUPPORTING_HIGH-CONTRAST_THEMES"/>
## <a name="supporting-high-contrast-themes"></a>고대비 테마 지원  
UI 컨트롤에서는 테마의 XAML 리소스 사전 일부로 정의되는 시각적 표현을 사용합니다. 이러한 테마 중 하나 이상은 특히 시스템이 고대비로 설정된 경우에 사용됩니다. 사용자가 리소스 사전에서 적절한 테마를 동적으로 조회하여 고대비로 전환하면 모든 UI 컨트롤도 적절한 고대비 테마를 사용합니다. 명시적 스타일을 지정하거나 고대비 테마가 로드되어 스타일 변경을 재정의할 수 없도록 하는 스타일 지정 기술을 사용하여 테마를 사용하지 않도록 설정하지 않았는지 확인하세요. 자세한 내용은 [고대비 테마](high-contrast-themes.md)를 참조하세요.

<span id="Design_for_alternative_UI"/>
<span id="design_for_alternative_ui"/>
<span id="DESIGN_FOR_ALTERNATIVE_UI"/>
## <a name="design-for-alternative-ui"></a>대체 UI 디자인  
앱을 디자인할 때는 움직임이 불편하거나, 시각 또는 청각 장애를 가진 사용자가 사용하는 방법에 대해 고려해야 합니다. 보조 기술 제품은 접근성에 대한 다른 조정은 필요하지 않더라도 표준 UI를 집중 사용하므로 성능이 우수한 키보드 및 화면 읽기 프로그램 지원을 제공하는 것이 중요합니다.

대부분의 경우 사용자층의 폭을 넓히기 위해 여러 가지 기술을 사용하여 필요한 정보를 전달할 수 있습니다. 예를 들어 색맹인 사용자에게는 아이콘과 색 정보를 함께 사용하여 정보를 강조 표시할 수 있고, 청각 장애가 있는 사용자에게는 소리 효과와 함께 시각 경고를 표시할 수 있습니다.

원할 경우 필요하지 않은 요소와 애니메이션이 완전히 제거된 접근성 있는 대체 사용자 인터페이스 요소를 제공할 수 있고, 사용자 환경을 간소화할 수 있도록 다른 단순 기능을 제공할 수 있습니다. 다음 코드 예제에서는 사용자 설정에 따라 한 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/BR227647) 인스턴스를 다른 인스턴스를 대신하여 표시하는 방법을 보여 줍니다.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">

  <CheckBox x:Name="ShowAccessibleUICheckBox" Click="ShowAccessibleUICheckBox_Click">
    Show Accessible UI
  </CheckBox>

  <UserControl x:Name="ContentBlock">
    <local:ContentPage/>
  </UserControl>

</StackPanel>
```

Visual Basic
```vb
Private Sub ShowAccessibleUICheckBox_Click(ByVal sender As Object,
    ByVal e As RoutedEventArgs)

    If (ShowAccessibleUICheckBox.IsChecked.Value) Then
        ContentBlock.Content = New AccessibleContentPage()
    Else
        ContentBlock.Content = New ContentPage()
    End If
End Sub
```

C#
```csharp
private void ShowAccessibleUICheckBox_Click(object sender, RoutedEventArgs e)
{
    if ((sender as CheckBox).IsChecked.Value)
    {
        ContentBlock.Content = new AccessibleContentPage();
    }
    else
    {
        ContentBlock.Content = new ContentPage();
    }
}
```

<span id="Verification_and_publishing"/>
<span id="verification_and_publishing"/>
<span id="VERIFICATION_AND_PUBLISHING"/>
## <a name="verification-and-publishing"></a>검증 및 게시  
앱의 접근성 등록 및 게시에 대한 자세한 내용은 [스토어에서의 접근성](accessibility-in-the-store.md)을 참조하세요.

> [!NOTE]
> 앱을 접근성 있는 앱으로 등록하는 것은 Windows 스토어에만 적합합니다.

<span id="Assistive_technology_support_in_custom_controls"/>
<span id="assistive_technology_support_in_custom_controls"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"/>
## <a name="assistive-technology-support-in-custom-controls"></a>사용자 지정 컨트롤의 보조 기술 지원  
사용자 지정 컨트롤을 만들 때 접근성을 지원하기 위해서는 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 하위 클래스도 하나 이상 구현하거나 확장하는 것이 좋습니다. 이 경우 기본 컨트롤 클래스에서 사용된 것과 동일한 피어 클래스를 사용하기만 하면 파생 클래스에 대한 기본 수준의 자동화 지원이 적절합니다. 그러나 실제로 테스트해야 하며, 피어가 새 컨트롤 클래스의 클래스 이름을 올바르게 보고할 수 있도록 피어를 구현하는 것이 좋습니다. 사용자 지정 자동화 피어 구현은 몇 단계로 이루어집니다. 자세한 내용은 [사용자 지정 자동화 피어](custom-automation-peers.md)를 참조하세요.

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"/>
<span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"/>
## <a name="assistive-technology-support-in-apps-that-support-xaml--microsoft-directx-interop"></a>XAML/Microsoft DirectX interop를 지원하는 앱의 보조 기술 지원  
XAML UI([**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/Dn252834) 또는 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/Hh702041) 사용)에서 호스트되는 Microsoft DirectX 콘텐츠에는 기본적으로 액세스할 수 없습니다. [XAML SwapChainPanel DirectX interop 샘플](http://go.microsoft.com/fwlink/p/?LinkID=309155)에서는 호스트된 DirectX 콘텐츠에 대한 UI 자동화 피어를 만드는 방법을 보여 줍니다. 이 방법을 사용하면 UI 자동화를 통해 호스트된 콘텐츠에 액세스할 수 있습니다.

<span id="related_topics"/>
## <a name="related-topics"></a>관련 항목  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179)
* [접근성을 위한 디자인](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [XAML 접근성 샘플](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [접근성](accessibility.md)

