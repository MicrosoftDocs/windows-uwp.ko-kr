---
Description: 이 항목에서는 색 및 배경이 필요한 명암비를 충족하도록 하여 앱 텍스트의 접근성에 대한 모범 사례를 설명합니다.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: 접근성 있는 텍스트 요구 사항
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f5b87590736c4875214819f5c60a05edd47b1476
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339516"
---
# <a name="accessible-text-requirements"></a>접근성 있는 텍스트 요구 사항  




이 항목에서는 색 및 배경이 필요한 명암비를 충족하도록 하여 앱 텍스트의 접근성에 대한 모범 사례를 설명합니다. 또한 이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱의 텍스트 요소에 부여될 수 있는 Microsoft UI 자동화 역할 및 그래픽의 텍스트에 대한 모범 사례를 설명합니다.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>명암비  
사용자는 항상 고대비 모드로 전환할 수 있지만 텍스트의 앱 디자인에서는 해당 옵션을 마지막 방법으로 간주해야 합니다. 그러나 앱 텍스트에서 텍스트와 배경 간 대비 수준에 대해 설정된 특정 지침을 충족하도록 하는 것이 훨씬 더 바람직합니다. 대비 수준의 평가는 색상을 고려하지 않는 결정적 기술을 기반으로 합니다. 예를 들어 녹색 배경에 빨간색 텍스트가 있는 경우 색맹 장애가 있는 사용자는 해당 텍스트를 읽을 수 없습니다. 명암비를 확인하고 수정하면 이러한 형식의 접근성 문제를 방지할 수 있습니다.

여기에서 설명 하는 텍스트 대비 권장 사항은 웹 접근성 표준, [G18을 기반으로 합니다. 최소 4.5:1의 명암비가 텍스트 (및 텍스트 이미지)와 텍스트 (@ no__t-0)의 배경에 있는지 확인 합니다. 이 지침은 *WCAG 2.0에 대한 W3C 기술* 사양에 있습니다.

접근성 있는 텍스트로 간주되려면 표시되는 텍스트의 광도 대비율이 배경과 비교하여 4.5:1 이상이어야 합니다. 예외로는 로고 및 부수적 텍스트(예제: 비활성 UI 구성 요소의 일부인 텍스트)가 있습니다.

장식 텍스트 및 정보를 전달하지 않는 텍스트는 제외됩니다. 예를 들어, 임의의 단어를 사용하여 배경을 만드는 경우 이들 단어는 의미 변경 없이 다시 정렬 또는 대체가 가능하며, 장식 텍스트로 간주되어 이 조건을 충족하지 않아도 됩니다.

색상 대비 도구를 사용하여 표시되는 텍스트 명암비가 허용되는지 검증합니다. 명암비를 테스트할 수 있는 도구는 [Techniques for WCAG 2.0 G18(리소스 섹션)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)을 참조하세요.

> [!NOTE]
> Techniques for WCAG 2.0 G18에 나열된 도구 중 일부는 UWP 앱에서 대화형으로 사용할 수 없습니다. 도구에 전경색 및 배경색 값을 수동으로 입력하거나 앱 UI의 화면 캡처를 만든 다음 화면 캡처 이미지에서 명암비 도구를 실행해야 할 수 있습니다.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>텍스트 요소 역할  
UWP 앱은 다음과 같은 기본 요소(일반적으로 *텍스트 요소* 또는 *textedit 컨트롤*이라고 함)를 사용할 수 있습니다.

* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock): Role은 [ **Text** 입니다.](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox): 역할 [ **편집**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) (및 오버플로 클래스 [**RichTextBlockOverflow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)): role은 [**Text**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 입니다.
* [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox): 역할 [ **편집**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)

컨트롤에 [**Edit**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 역할이 있는 것으로 보고되면 보조 기술에서는 사용자가 값을 변경할 방법이 있는 것으로 가정합니다. 따라서 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)에 정적 텍스트를 입력하면 역할을 잘못 보고하여 앱의 구조를 접근성 사용자에게 잘못 보고할 수 있습니다.

XAML의 텍스트 모델에는 정적 텍스트에 주로 사용되는 두 요소, 즉 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 및 [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)이 있습니다. 이 요소는 [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) 하위 클래스가 아니므로 키보드 포커스가 불가능하고 탭 순서로 표시할 수 없습니다. 하지만 보조 기술에서 이를 읽을 수 없거나 읽지 않는다는 의미는 아닙니다. 화면 읽기 프로그램은 일반적으로 "가상 커서"처럼 포커스 및 탭 순서를 벗어난 탐색 패턴이나 읽기 전용 모드를 포함하여 앱의 콘텐츠를 읽는 다양한 모드를 지원하도록 설계되었습니다. 따라서 탭 순서에 따라 사용자가 도달하도록 정적 텍스트를 포커스 가능 컨테이너에 배치하지 마세요. 보조 기술 사용자는 탭 순서 내의 항목이 대화형이기를 기대하므로 정적 텍스트를 발견할 경우 도움이 되기보다는 오히려 혼동을 줍니다. 내레이터로 직접 테스트하여 화면 읽기 프로그램을 사용해 앱의 정적 텍스트를 검사할 때 앱의 사용자 환경이 어떤지 확인해야 합니다.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>자동 제안 접근성  
사용자가 입력 필드에 입력하고 잠재적인 제안 목록이 나타날 경우 이러한 유형의 시나리오를 자동 제안이라고 합니다. 이 시나리오는 메일 필드의 **받는 사람:** 줄, Windows의 Cortana 검색 상자, Microsoft Edge의 URL 입력 필드, 날씨 앱의 위치 입력 필드 등에서 일반적으로 사용됩니다. XAML [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 또는 HTML 내장 컨트롤을 사용하는 경우 이 환경이 기본적으로 이미 연결되어 있습니다. 이 환경에 액세스할 수 있게 하려면 입력 필드와 목록을 연결해야 합니다. 이 내용은 [자동 제안 구현](#implementing_auto-suggest) 섹션에서 설명합니다.

특수 제안 모드로 이러한 유형의 환경에 액세스할 수 있도록 내레이터가 업데이트되었습니다. 상위 수준에서 편집 필드와 목록이 제대로 연결된 경우 최종 사용자에게 다음과 같은 이점이 있습니다.

* 목록이 있다는 것과 목록이 닫히는 시기를 알 수 있습니다.
* 사용 가능한 제안 수를 알 수 있습니다.
* 선택한 항목을 알 수 있습니다(있는 경우).
* 내레이터 포커스를 목록으로 이동할 수 있습니다.
* 다른 모든 읽기 모드에서 제안을 탐색할 수 있습니다.

![ 제안 목록 @ no__t-1<br/>
_제안 목록의 예_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>자동 제안 구현  
이 환경에 액세스할 수 있게 하려면 UIA 트리에서 입력 필드와 목록을 연결해야 합니다. 이 연결은 데스크톱 앱의 [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) 속성 또는 UWP 앱의 [ControlledPeers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 속성을 통해 수행됩니다.

상위 수준에는 두 가지 유형의 자동 제안 환경이 있습니다.

**기본 선택**  
목록에서 기본 선택을 수행하는 경우 내레이터가 데스크톱 앱에서 [**UIA_SelectionItem_ElementSelectedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids) 이벤트를 찾거나, UWP 앱에서 [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트가 발생합니다. 선택이 변경될 때마다, 사용자가 다른 문자를 입력하고 제안이 업데이트될 때 또는 사용자가 목록을 통해 탐색할 때 **ElementSelected** 이벤트가 발생해야 합니다.

기본 선택이 있는 ![ 목록 @ no__t-1<br/>
_기본 선택이 있는 예제_

**기본 선택 없음**  
날씨 앱의 위치 상자와 같이 기본 선택이 없는 경우 내레이터가 데스크톱 [**UIA_LayoutInvalidatedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids) 이벤트를 찾거나, 목록이 업데이트될 때마다 목록에서 UWP [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트가 발생합니다.

@no__t-기본 선택 없는 목록 @ no__t-1<br/>
_기본 선택이 없는 예_

### <a name="xaml-implementation"></a>XAML 구현  
기본 XAML [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)를 사용하는 경우 모든 항목이 이미 연결되어 있습니다. [  **TextBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox)와 목록을 사용하여 고유한 자동 제안 환경을 만드는 경우 **TextBox**에서 목록을 [**AutomationProperties.ControlledPeers**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers)로 설정해야 합니다. 이 속성을 추가하거나 제거할 때마다 [**ControlledPeers**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 속성에 대한 **AutomationPropertyChanged** 이벤트를 발생하거나, 이 문서의 앞부분에서 설명한 시나리오 유형에 따라 고유한 [**SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트 또는 [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트를 발생해야 합니다.

### <a name="html-implementation"></a>HTML 구현  
HTML에서 내장 컨트롤을 사용하는 경우 UIA 구현이 이미 매핑되어 있습니다. 다음은 이미 연결되어 있는 구현의 예입니다.

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 고유한 컨트롤을 만드는 경우 W3C 표준에서 설명하는 고유한 ARIA 컨트롤을 설정해야 합니다.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>그래픽의 텍스트

가능하면 그래픽에 텍스트를 포함하지 마세요. 예를 들어 앱에서 [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 요소로 표시되는 이미지 원본 파일에 포함하는 텍스트는 보조 기술에서 자동으로 접근하거나 읽을 수 없습니다. 그래픽에 텍스트를 사용해야 하는 경우 "alt 텍스트"의 값으로 제공하는 [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) 값에 해당 텍스트나 해당 텍스트의 의미에 대한 요약이 포함되도록 합니다. 텍스트 문자를 벡터에서 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)의 일부로 만들거나 [**Glyphs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Glyphs)를 사용하여 만드는 경우에도 유사한 고려 사항이 적용됩니다.

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>텍스트 글꼴 크기 및 배율

사용자는 글꼴이 매우 작은 경우에만 앱에서 텍스트를 읽는 데 어려움이 있으므로 응용 프로그램의 텍스트가 첫 번째 위치의 적당 한 크기 인지 확인 합니다.

명확 하 게 완료 되 면 Windows에는 사용자가 활용할 수 있는 다양 한 내게 필요한 옵션 도구 및 설정이 포함 되며,이를 통해 사용자는 자신의 요구와 텍스트 읽기를 위한 기본 설정을 변경할 수 있습니다. 이러한 개체는 다음과 같습니다.

* UI의 선택한 영역을 확대 하는 돋보기 도구입니다. 앱에서 텍스트의 레이아웃을 사용 하 여 편집용으로 돋보기를 사용 하는 것이 어려울 수 있도록 해야 합니다.
* **설정-> 시스템-> 디스플레이 > 크기 조정 및 레이아웃에 대**한 전역 크기 조정 및 해상도 설정 사용 가능한 크기 옵션은 표시 장치의 기능에 따라 달라질 수 있습니다.
* 설정의 텍스트 크기 설정 **-접근성 > 표시를 >** 합니다. **텍스트 크게 만들기** 설정을 조정 하 여 모든 응용 프로그램 및 화면에서 지원 컨트롤의 텍스트 크기만 지정 합니다. 모든 UWP 텍스트 컨트롤은 사용자 지정 또는 템플릿 없이 텍스트 크기 조정 환경을 지원 합니다. 
> [!NOTE]
> **모든 항목을 크게** 설정 하면 사용자가 기본 화면 에서만 일반 텍스트 및 앱에 대 한 기본 설정 크기를 지정할 수 있습니다.

다양한 텍스트 요소와 컨트롤에 [**IsTextScaleFactorEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled) 속성이 있습니다. 이 속성은 기본적으로 **true** 값으로 설정됩니다. **True 이면**해당 요소의 텍스트 크기를 조정할 수 있습니다. 크기 **조정은 크기가 작은**텍스트에 영향을 주는 것 보다 작은 **fontsize** 의 텍스트에 영향을 줍니다. 요소의 **IsTextScaleFactorEnabled** 속성을 **false**로 설정 하 여 자동 크기 조정을 사용 하지 않도록 설정할 수 있습니다. 

자세한 내용은 [텍스트 크기 조정](https://docs.microsoft.com/windows/uwp/design/input/text-scaling) 을 참조 하세요.

앱에 다음 태그를 추가 하 고 실행 합니다. **텍스트 크기** 설정을 조정 하 고 각 **TextBlock**에 발생 하는 결과를 확인 합니다.

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

모든 앱에서 전체적으로 UI 텍스트 크기를 조정 하는 것은 사용자에 게 중요 한 액세스 가능성 환경 이므로 텍스트 크기 조정을 사용 하지 않는 것이 좋습니다.

[  **TextScaleFactorChanged**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) 이벤트와 [**TextScaleFactor**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) 속성을 사용하여 휴대폰의 **텍스트 크기** 설정에 대한 변경 사항을 확인할 수도 있습니다. 방법은 다음과 같습니다.

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

**TextScaleFactor** 값은-11, 2.25 @ no__t-2 @no__t 범위에서 double입니다. 가장 작은 텍스트는 이 값만큼 확대됩니다. 값을 사용하여 텍스트에 맞게 그래픽의 크기를 조정할 수 있습니다. 하지만 모든 텍스트가 같은 배율로 크기가 조정되지는 않습니다. 일반적으로 텍스트 크기가 클수록 크기 조정의 영향을 덜 받습니다.

다음 형식에는 **IsTextScaleFactorEnabled** 속성이 있습니다.  
* [**ContentPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter)
* [**컨트롤**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) 및 파생 클래스
* [**FontIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FontIcon)
* [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)
* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)
* [**Textelement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.TextElement) 및 파생 클래스

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  

* [텍스트 크기 조정](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)
* [액세스 가능성](accessibility.md)
* [기본 접근성 정보](basic-accessibility-information.md)
* [XAML 텍스트 표시 샘플](https://go.microsoft.com/fwlink/p/?linkid=238579)
* [XAML 텍스트 편집 샘플](https://go.microsoft.com/fwlink/p/?linkid=251417)
* [XAML 접근성 샘플](https://go.microsoft.com/fwlink/p/?linkid=238570) 
