---
Description: 이 문서는 Windows 앱에 대 한 내게 필요한 옵션 시나리오와 관련 된 개념과 기술에 대 한 개요입니다.
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
title: 접근성 개요
label: Accessibility overview
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 525f8d3170d94b035648841edca5f903450bf871
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216486"
---
# <a name="accessibility-overview"></a>접근성 개요

이 문서는 Windows 앱에 대 한 내게 필요한 옵션 시나리오와 관련 된 개념과 기술에 대 한 개요입니다.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]

<span id="Accessibility_and_your_app"/>
<span id="accessibility_and_your_app"/>
<span id="ACCESSIBILITY_AND_YOUR_APP"/>

## <a name="accessibility-and-your-app"></a>내게 필요한 옵션 및 앱

이동성, 시각, 색 인식, 청각, 음성, cognition 및 literacy에 대 한 제한 사항을 포함 하 여 다양 한 장애가 나 장애가 발생할 수 있습니다. 그러나 여기에 제공 된 지침에 따라 대부분의 요구 사항을 해결할 수 있습니다. 즉, 다음을 제공 합니다.

* 키보드 상호 작용 및 화면 판독기 지원.
* 글꼴, 확대/축소 설정 (확대), 색 및 고대비 설정과 같은 사용자 지정을 지원 합니다.
* UI의 부분에 대 한 대안 또는 보완.

XAML 용 컨트롤은 이미 UWP 앱, HTML 및 기타 UI 기술을 지원 하는 내게 필요한 옵션 프레임 워크를 활용 하는 화면 판독기와 같은 보조 기술에 대 한 지원을 제공 합니다. 이 기본 제공 지원을 사용 하면 몇 가지 속성만 설정 하 여 매우 적은 작업으로 사용자 지정할 수 있는 기본 수준의 액세스 가능성을 사용할 수 있습니다. 사용자 지정 XAML 구성 요소 및 컨트롤을 만드는 경우 *자동화 피어의*개념을 사용 하 여 해당 컨트롤에 비슷한 지원을 추가할 수도 있습니다.

또한 데이터 바인딩, 스타일 및 템플릿 기능을 사용 하면 대체 Ui에 대 한 설정 및 텍스트를 표시 하는 동적 변경에 대 한 지원을 쉽게 구현할 수 있습니다.

<span id="UI_Automation"/>
<span id="ui_automation"/>
<span id="UI_AUTOMATION"/>

## <a name="ui-automation"></a>UI 자동화

내게 필요한 옵션 지원은 주로 Microsoft UI 자동화 프레임 워크에 대 한 통합 지원에서 제공 됩니다. 이 지원은 기본 클래스를 통해 제공 되며, 컨트롤 형식에 대 한 클래스 구현의 기본 제공 동작 및 UI 자동화 공급자 API의 인터페이스 표현으로 제공 됩니다. 각 컨트롤 클래스는 UI 자동화 클라이언트에 컨트롤의 역할 및 콘텐츠를 보고 하는 자동화의 UI 자동화 개념과 자동화 피어의 개념을 사용 합니다. 앱은 UI 자동화를 통해 최상위 창으로 처리 되 고, UI 자동화 프레임 워크를 통해 해당 앱 창 내의 접근성 관련 콘텐츠를 모두 UI 자동화 클라이언트에서 사용할 수 있습니다. UI 자동화에 대 한 자세한 내용은 [Ui Automation 개요](/windows/desktop/WinAuto/uiauto-uiautomationoverview)를 참조 하세요.

<span id="Assistive_technology"/>
<span id="assistive_technology"/>
<span id="ASSISTIVE_TECHNOLOGY"/>

## <a name="assistive-technology"></a>보조 기술

사용자가 설치한 보조 기술 제품이 나 운영 체제에서 제공 하는 도구 및 설정에 의해 많은 사용자의 접근성 요구 사항이 충족 됩니다. 여기에는 화면 판독기, 화면 확대 및 고대비 설정과 같은 기능이 포함 됩니다.

보조 기술 제품에는 다양 한 소프트웨어와 하드웨어가 포함 되어 있습니다. 이러한 제품은 표준 키보드 인터페이스 및 화면 판독기와 기타 보조 기술에 대 한 UI의 내용 및 구조에 대 한 정보를 보고 하는 내게 필요한 옵션 프레임 워크를 통해 작동 합니다. 보조 기술 제품의 예는 다음과 같습니다.

* 사용자가 키보드 대신 포인터를 사용 하 여 텍스트를 입력할 수 있는 화상 키보드
* 음성 인식 소프트웨어로, 음성 단어를 입력 된 텍스트로 변환 합니다.
* 텍스트를 음성 또는 브라유 점자와 같은 다른 형식으로 변환 하는 화면 판독기입니다.
* Windows의 특정 부분인 내레이터 화면 판독기입니다. 내레이터에는 터치 제스처를 사용 하 여 키보드를 사용할 수 없는 경우 터치 제스처를 처리 하 여 화면 읽기 작업을 수행할 수 있는 터치 모드가 있습니다.
* 화면 표시를 조정 하는 프로그램 또는 설정 (예: 고대비 테마, dpi (인치당 도트 수) 설정 또는 돋보기 도구)

적절 한 키보드 및 화면 판독기를 지 원하는 앱은 일반적으로 다양 한 보조 기술 제품에서 잘 작동 합니다. 대부분의 경우 UWP 앱은 정보나 구조를 추가로 수정 하지 않고도 이러한 제품과 함께 작동 합니다. 그러나 최적의 액세스 가능성 경험을 위해 일부 설정을 수정 하거나 추가 지원을 구현 하는 것이 좋습니다.

보조 기술을 사용 하 여 기본적인 접근성 시나리오를 테스트 하는 데 사용할 수 있는 옵션 중 일부는 [접근성 테스트](accessibility-testing.md)에 나와 있습니다.

<span id="Screen_reader_support_and_basic_accessibility_information"/>
<span id="screen_reader_support_and_basic_accessibility_information"/>
<span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"/>

## <a name="screen-reader-support-and-basic-accessibility-information"></a>화면 읽기 프로그램 지원 및 기본 접근성 정보

화면 판독기는 음성 언어 또는 브라유 점자 출력과 같은 다른 형식으로 렌더링 하 여 앱의 텍스트에 대 한 액세스를 제공 합니다. 화면 판독기의 정확한 동작은 소프트웨어 및 사용자의 구성에 따라 달라 집니다.

예를 들어 일부 화면 판독기는 사용자가를 시작 하거나 보려는 앱으로 전환할 때 전체 앱 UI를 읽고 사용자가 탐색을 시도 하기 전에 사용 가능한 정보 콘텐츠를 모두 받을 수 있도록 합니다. 또한 일부 화면 판독기는 탭 탐색 중에 포커스를 받을 때 개별 컨트롤과 연결 된 텍스트를 읽습니다. 이렇게 하면 사용자가 응용 프로그램의 입력 컨트롤 사이에서 이동할 때 스스로 방향을 지정할 수 있습니다. 내레이터는 사용자의 선택에 따라 두 동작을 모두 제공 하는 화면 판독기의 한 예입니다.

사용자가 앱을 이해 하거나 탐색 하는 데 도움이 되도록 화면 판독기나 기타 보조 기술에 필요한 가장 중요 한 정보는 앱의 요소 부분에 대 한 *액세스 가능한 이름* 입니다. 대부분의 경우 컨트롤이 나 요소에는 다른 방법으로 제공 된 다른 속성 값에서 계산 되는 액세스 가능 이름이 이미 있습니다. 이미 계산 된 이름을 사용할 수 있는 가장 일반적인 경우는를 지원 하 고 내부 텍스트를 표시 하는 요소를 사용 하는 것입니다. 다른 요소의 경우 요소 구조에 대 한 모범 사례를 따라 액세스 가능한 이름을 제공 하는 다른 방법을 고려해 야 합니다. 경우에 따라 앱 접근성을 위한 접근성 있는 이름으로 명시적으로 사용되는 이름을 제공해야 합니다. 일반적인 UI 요소에서 작업 하는 계산 된 값의 수와 일반적인 액세스 가능한 이름에 대 한 자세한 내용은 [기본 접근성 정보](basic-accessibility-information.md)를 참조 하세요.

사용할 수 있는 다른 여러 가지 자동화 속성 (다음 섹션에서 설명 하는 키보드 속성 포함)이 있습니다. 그러나 모든 화면 판독기에서 모든 automation 속성을 지 원하는 것은 아닙니다. 일반적으로 적절 한 모든 자동화 속성을 설정 하 고 테스트 하 여 화면 판독기에 대 한 최대한 광범위 한 지원을 제공 해야 합니다.

<span id="Keyboard_support"/>
<span id="keyboard_support"/>
<span id="KEYBOARD_SUPPORT"/>

## <a name="keyboard-support"></a>키보드 지원

좋은 키보드 지원을 제공 하려면 응용 프로그램의 모든 부분을 키보드와 함께 사용할 수 있는지 확인 해야 합니다. 앱에서 대부분 표준 컨트롤을 사용하고 사용자 지정 컨트롤을 사용하지 않는 경우 이미 최적의 접근성 환경을 구현하고 있는 것입니다. 기본 XAML 제어 모델은 탭 탐색, 텍스트 입력 및 컨트롤별 지원을 포함 하 여 기본 제공 키보드 지원 기능을 제공 합니다. 레이아웃 컨테이너 역할을 하는 요소 (예: 패널)는 레이아웃 순서를 사용 하 여 기본 탭 순서를 설정 합니다. 이러한 순서는 액세스 가능한 UI 표현에 사용할 수 있는 올바른 탭 순서입니다. [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 및 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 컨트롤을 사용 하 여 데이터를 표시 하는 경우 기본 제공 화살표 키 탐색을 제공 합니다. 또는 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 컨트롤을 사용 하는 경우 단추 활성화를 위해 이미 스페이스바 또는 Enter 키를 처리 합니다.

탭 순서 및 키 기반 활성화 나 탐색을 포함 하 여 키보드 지원의 모든 측면에 대 한 자세한 내용은 [키보드 접근성](keyboard-accessibility.md)을 참조 하세요.

<span id="Media_and_captioning"/>
<span id="media_and_captioning"/>
<span id="MEDIA_AND_CAPTIONING"/>

## <a name="media-and-captioning"></a>미디어 및 캡션

일반적으로 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 개체를 통해 오디오 시각적 미디어를 표시 합니다. **MediaElement** api를 사용 하 여 미디어 재생을 제어할 수 있습니다. 내게 필요한 옵션을 위해 사용자가 필요에 따라 미디어를 재생, 일시 중지 및 중지할 수 있는 컨트롤을 제공 합니다. 경우에 따라 미디어에는 설명 설명이 포함 된 캡션 또는 대체 오디오 트랙과 같이 접근성을 위한 추가 구성 요소가 포함 됩니다.

<span id="Accessible_text"/>
<span id="accessible_text"/>
<span id="ACCESSIBLE_TEXT"/>

## <a name="accessible-text"></a>액세스 가능한 텍스트

텍스트의 세 가지 주요 측면은 접근성과 관련이 있습니다.

* 도구는 탭 시퀀스 순회의 일부로 또는 전체 문서 표현의 일부로만 텍스트를 읽을 지 여부를 결정 해야 합니다. 텍스트를 표시 하는 데 적절 한 요소를 선택 하거나 해당 텍스트 요소의 속성을 조정 하 여 이러한 결정을 제어할 수 있습니다. 각 텍스트 요소에는 특정 목적이 있으며 해당 용도에는 해당 하는 UI 자동화 역할이 있습니다. 잘못 된 요소를 사용 하면 UI 자동화에 잘못 된 역할을 보고 하 고 보조 기술 사용자에 게 혼란 스러운 환경을 만들 수 있습니다.
* 대부분의 사용자는 배경에 적절 한 대비를 사용 하지 않는 한 텍스트를 읽기 어려울 수 있는 시각 제한 사항을가지고 있습니다. 이러한 제한 사항이 없는 앱 디자이너에 게는이에 영향을 미치는 방법이 직관적이 지 않습니다. 예를 들어 색 블라인드 사용자의 경우 디자인에서 색이 잘못 선택 되 면 일부 사용자가 텍스트를 읽을 수 없게 됩니다. 웹 콘텐츠에 대해 원래 제공 된 내게 필요한 옵션 권장 사항은 응용 프로그램 에서도 이러한 문제를 방지할 수 있는 대비 표준을 정의 합니다. 자세한 내용은 [액세스 가능한 텍스트 요구 사항](accessible-text-requirements.md)을 참조 하세요.
* 대부분의 사용자는 단순히 너무 작은 텍스트를 읽는 데 어려움이 있습니다. 앱의 UI에 있는 텍스트를 처음부터 크게 크게 설정 하 여이 문제를 방지할 수 있습니다. 그러나 많은 양의 텍스트를 표시 하는 앱 또는 다른 시각적 요소와 함께 표시 되는 텍스트입니다. 이러한 경우 앱의 모든 텍스트가 확장 되도록 앱이 디스플레이를 확장할 수 있는 시스템 기능과 올바르게 상호 작용 하는지 확인 합니다. 일부 사용자는 dpi 값을 내게 필요한 옵션으로 변경 합니다. 이 옵션은 **쉽게 액세스할**수 있는 **화면에** 표시 하는 데 사용할 수 있으며, **모양 및 개인 설정**표시를 위해 **컨트롤 패널** UI로 리디렉션됩니다  /  **Display**.

<span id="Supporting_high-contrast_themes"/>
<span id="supporting_high-contrast_themes"/>
<span id="SUPPORTING_HIGH-CONTRAST_THEMES"/>

## <a name="supporting-high-contrast-themes"></a>고대비 테마 지원

UI 컨트롤은 테마의 XAML 리소스 사전에 포함 된 시각적 표현을 사용 합니다. 이러한 테마 중 하나 이상이 시스템에 고대비를 설정 하는 경우에만 사용 됩니다. 사용자가 리소스 사전에서 적절 한 테마를 동적으로 조회 하 여 고대비로 전환 하는 경우 모든 UI 컨트롤은 적절 한 고대비 테마를 사용 합니다. 명시적 스타일을 지정 하거나 고대비 테마에서 스타일 변경 내용을 로드 및 재정의 하지 못하도록 하는 다른 스타일 지정 기술을 사용 하 여 테마를 사용 하지 않도록 설정 했는지 확인 합니다. 자세한 내용은 고대비 [테마](high-contrast-themes.md)를 참조 하세요.

<span id="Design_for_alternative_UI"/>
<span id="design_for_alternative_ui"/>
<span id="DESIGN_FOR_ALTERNATIVE_UI"/>

## <a name="design-for-alternative-ui"></a>대체 UI 디자인

앱을 디자인 하는 경우 모바일, 시각 및 청각 기능이 제한 된 사용자가 앱을 사용할 수 있는 방법을 고려 합니다. 보조 기술 제품은 표준 UI를 광범위 하 게 사용 하므로 접근성에 대 한 다른 조정 작업을 수행할 필요가 없는 경우에도 적절 한 키보드 및 화면 판독기 지원을 제공 하는 것이 특히 중요 합니다.

대부분의 경우에는 여러 기술을 사용 하 여 대상 그룹을 확장 함으로써 중요 한 정보를 전달할 수 있습니다. 예를 들어 아이콘 및 색 정보를 모두 사용 하 여 정보를 강조 표시 하 여 색맹인 사용자를 도울 수 있으며, 소리 효과와 함께 시각적 경고를 표시 하 여 청각 장애가 있는 사용자에 게 도움을 줍니다.

필요한 경우 불필요 한 요소와 애니메이션을 완전히 제거 하는 액세스 가능한 대체 사용자 인터페이스 요소를 제공 하 고, 다른 단순화를 제공 하 여 사용자 환경을 간소화할 수 있습니다. 다음 코드 예제에서는 사용자 설정에 따라 다른 [**UserControl**](/uwp/api/Windows.UI.Xaml.Controls.UserControl) 인스턴스를 표시 하는 방법을 보여 줍니다.

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

## <a name="verification-and-publishing"></a>확인 및 게시

액세스 가능성 선언 및 앱 게시에 대 한 자세한 내용은 [스토어의 접근성](accessibility-in-the-store.md)을 참조 하세요.

> [!NOTE]
> 응용 프로그램을 액세스할 수 있는 것으로 선언 하는 것은 Microsoft Store에만 해당 됩니다.

<span id="Assistive_technology_support_in_custom_controls"/>
<span id="assistive_technology_support_in_custom_controls"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"/>

## <a name="assistive-technology-support-in-custom-controls"></a>사용자 지정 컨트롤의 보조 기술 지원

사용자 지정 컨트롤을 만들 때 내게 필요한 옵션 지원 기능을 제공 하기 위해 하나 이상의 [**Automationpeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 서브 클래스도 구현 하거나 확장 하는 것이 좋습니다. 기본 컨트롤 클래스에서 사용 된 것과 동일한 피어 클래스를 사용 하는 경우에는 기본 수준에서 파생 클래스에 대 한 자동화 지원이 적합 합니다. 그러나이를 테스트 해야 하며, 피어가 새 컨트롤 클래스의 클래스 이름을 올바르게 보고할 수 있도록 피어를 구현 하는 것이 모범 사례로 권장 됩니다. 사용자 지정 자동화 피어를 구현 하려면 몇 가지 단계를 거쳐야 합니다. 자세한 내용은 [사용자 지정 자동화 피어](custom-automation-peers.md)를 참조하세요.

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"/>
<span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"/>

## <a name="assistive-technology-support-in-apps-that-support-xaml--microsoft-directx-interop"></a>XAML/Microsoft DirectX interop를 지 원하는 앱의 보조 기술 지원

[**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 또는 [**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)를 사용 하 여 XAML UI에서 호스트 되는 Microsoft DirectX 콘텐츠는 기본적으로 액세스할 수 없습니다. [XAML SwapChainPanel directx interop 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/%5BC%23%5D-Windows%208.1%20Store%20app%20samples/XAML%20SwapChainPanel%20DirectX%20interop%20sample) 은 호스트 된 directx 콘텐츠에 대 한 UI 자동화 피어를 만드는 방법을 보여 줍니다. 이 방법을 사용 하면 UI 자동화를 통해 호스트 된 콘텐츠를 액세스할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [**Windows. .Xaml.**](/uwp/api/Windows.UI.Xaml.Automation)
* [접근성을 위한 디자인]()
* [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [접근성](accessibility.md)
* [내레이터 시작](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
