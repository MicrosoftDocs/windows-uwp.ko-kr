---
Description: 이 항목에서는 색 및 배경이 필요한 대비 비율을 충족 하도록 하 여 앱에서 텍스트의 접근성에 대 한 모범 사례를 설명 합니다.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: 접근성 있는 텍스트 요구 사항
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3294daa57cc7d1eb585e41910f72f574d9ffb600
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163387"
---
# <a name="accessible-text-requirements"></a>접근성 있는 텍스트 요구 사항  




이 항목에서는 색 및 배경이 필요한 대비 비율을 충족 하도록 하 여 앱에서 텍스트의 접근성에 대 한 모범 사례를 설명 합니다. 이 항목에서는 UWP (유니버설 Windows 플랫폼) 앱의 텍스트 요소에 포함 될 수 있는 Microsoft UI Automation 역할과 그래픽의 텍스트에 대 한 모범 사례에 대해서도 설명 합니다.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>대비 비율  
사용자는 항상 고대비 모드로 전환 하는 옵션을 사용할 수 있지만 텍스트에 대 한 앱 디자인은이 옵션을 마지막 수단으로 간주 해야 합니다. 앱 텍스트가 텍스트와 배경 간의 대비 수준에 대해 설정 된 특정 지침을 충족 하는지 확인 하는 것이 훨씬 더 좋습니다. 대비 수준을 평가 하는 것은 색 색상을 고려 하지 않는 결정적 기술을 기반으로 합니다. 예를 들어 녹색 배경에 빨간색 텍스트가 있는 경우이 텍스트를 색 시각 장애가 있는 사용자가 읽을 수 없습니다. 대비 비율을 확인 하 고 수정 하면 이러한 유형의 접근성 문제를 방지할 수 있습니다.

여기서 설명 하는 텍스트 대비 권장 사항은 웹 접근성 표준 (G18)을 기반으로 합니다. 즉, 텍스트 [(및 텍스트 이미지)와 텍스트의 배경 사이에 최소 4.5:1의 명암비가 존재 하는지](https://www.w3.org/TR/WCAG20-TECHS/G18.html)확인 합니다. 이 지침은 *WCAG 2.0 사양에 대 한 W3C 기술* 에 있습니다.

액세스할 수 있는 것으로 간주 되려면 배경에 대해 표시 되는 텍스트의 최소 광도 명암비 4.5:1 이어야 합니다. 예외에는 비활성 UI 구성 요소의 일부인 텍스트와 같은 로고 및 부수적 텍스트가 포함 됩니다.

장식용 이며 정보를 전달 하지 않는 텍스트는 제외 됩니다. 예를 들어, 임의의 단어를 사용하여 배경을 만드는 경우 이들 단어는 의미 변경 없이 다시 정렬 또는 대체가 가능하며, 장식 텍스트로 간주되어 이 조건을 충족하지 않아도 됩니다.

색 대비 도구를 사용 하 여 표시 되는 텍스트 대비 비율이 허용 되는지 확인 합니다. 대비 비율을 테스트할 수 있는 도구는 [WCAG 2.0 G18 (리소스 섹션) 기술](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) 을 참조 하세요.

> [!NOTE]
> WCAG 2.0 G18에 대 한 기술 별로 나열 된 도구 중 일부는 UWP 앱에서 대화형으로 사용할 수 없습니다. 도구에서 전경색 및 배경색 값을 수동으로 입력 하거나, 앱 UI의 화면 캡처를 만든 후 화면 캡처 이미지에서 대비 비율 도구를 실행 해야 할 수 있습니다.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>텍스트 요소 역할  
UWP 앱은 다음과 같은 기본 요소 (일반적으로 *텍스트 요소나* *textedit과 컨트롤*이라고 함)를 사용할 수 있습니다.

* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock): Role은 [ **Text** 입니다.](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox): 역할 [ **편집**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) (및 오버플로 클래스 [**RichTextBlockOverflow**](/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)): role은 [**Text**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 입니다.
* [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox): 역할 [ **편집**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)

컨트롤에 [**편집**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)역할이 있는 것으로 보고 되는 경우 보조 기술에서는 사용자가 값을 변경할 수 있는 방법이 있다고 가정 합니다. 따라서 [**텍스트 상자**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)에 정적 텍스트를 입력 하는 경우 역할을 misreporting 응용 프로그램의 구조를 내게 필요한 옵션 지원 사용자에 게 misreporting 합니다.

XAML에 대 한 텍스트 모델에는 정적 텍스트, [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 및 [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)에 주로 사용 되는 두 개의 요소가 있습니다. 둘 다 [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control) 하위 클래스는 아니므로 둘 다 키보드 포커스를 받을 수 없거나 탭 순서에 표시 될 수 있습니다. 하지만이는 보조 기술이이를 읽을 수 없거나 읽을 수 없음을 의미 하는 것은 아닙니다. 화면 읽기 프로그램은 일반적으로 응용 프로그램에서 콘텐츠를 읽을 수 있는 여러 모드를 지원 하도록 설계 되었습니다. 즉, 전용 읽기 모드 또는 포커스를 벗어난 탐색 패턴과 "가상 커서"와 같은 탭 순서를 포함 합니다. 따라서 탭 순서가 사용자를 가져오기 위해서만 정적 텍스트를 포커스 가능한 컨테이너에 넣지 마세요. 보조 기술 사용자는 탭 순서에서 모든 항목을 대화형으로 간주 하 고 정적 텍스트를 발견 하면 도움이 되는 것 보다 혼란 스 러 울 것으로 간주 합니다. 화면 판독기를 사용 하 여 앱의 정적 텍스트를 검사할 때 앱에 대 한 사용자 경험을 이해 하려면 내레이터에서 직접 테스트 해야 합니다.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>접근성 자동 제안  
사용자가 입력 필드에 입력 하 고 잠재적 제안 목록이 표시 되는 경우이 유형의 시나리오를 자동 제안 이라고 합니다. 이는 메일 필드의 끝 **:** 줄, Windows의 Cortana 검색 상자, Microsoft EDGE의 URL 입력 필드, 날씨 앱의 위치 입력 필드 등에서 일반적입니다. XAML [**AutosuggestBox**](/uwp/api/windows.ui.xaml.controls.autosuggestbox) 또는 HTML 내장 컨트롤을 사용 하는 경우 기본적으로이 환경이 이미 후크 되어 있습니다. 이 환경을 사용 하려면 입력 필드에 액세스 하 고 목록을 연결 해야 합니다. 이에 대 한 설명은 [자동 제안 구현](#implementing_auto-suggest) 섹션에 설명 되어 있습니다.

내레이터는 특별 제안 모드를 통해 이러한 유형의 액세스를 액세스할 수 있도록 업데이트 되었습니다. 상위 수준에서 편집 필드와 목록이 제대로 연결 되 면 최종 사용자가 다음을 수행 합니다.

* 목록이 있으며 목록이 닫힐 때를 확인 합니다.
* 사용할 수 있는 제안의 수를 확인 합니다.
* 선택한 항목을 확인 합니다 (있는 경우).
* 내레이터 포커스를 목록으로 이동할 수 있습니다.
* 다른 모든 읽기 모드를 사용 하 여 제안을 탐색할 수 있습니다.

![제안 목록](images/autosuggest-list.png)<br/>
_제안 목록의 예_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>자동 제안 구현  
이 환경을이 환경에 액세스할 수 있게 하려면 UIA 트리에서 해당 목록을 연결 해야 합니다. 이 연결은 데스크톱 응용 프로그램의 [UIA_ControllerForPropertyId](/windows/win32/winauto/uiauto-automation-element-propids) 속성 또는 UWP 앱의 [ControlledPeers](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 속성을 사용 하 여 수행 됩니다.

높은 수준에서 자동 제안 환경의 두 가지 유형이 있습니다.

**기본 선택**  
목록에서 기본 선택을 수행 하는 경우 내레이터는 데스크톱 앱에서  [**UIA_SelectionItem_ElementSelectedEventId**](/windows/desktop/WinAuto/uiauto-event-ids) 이벤트를 검색 하거나 UWP 앱에서 발생 하는 [**automationevents**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트를 찾습니다. 선택이 변경 될 때마다 사용자가 다른 문자를 입력 하 고 제안이 업데이트 되거나 사용자가 목록을 탐색할 때 **Elementselected** 이벤트가 발생 해야 합니다.

![기본 선택 항목이 포함 된 목록](images/autosuggest-default-selection.png)<br/>
_기본 선택이 있는 예제_

**기본 선택 없음**  
날씨 응용 프로그램의 위치 상자에와 같이 기본 선택이 없는 경우 내레이터가 목록이 업데이트 될 때마다 목록에서 발생 하는 데스크톱 [**UIA_LayoutInvalidatedEventId**](/windows/desktop/WinAuto/uiauto-event-ids) 이벤트 또는 UWP [**LayoutInvalidated**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트를 찾습니다.

![기본 선택 항목이 없는 목록](images/autosuggest-no-default-selection.png)<br/>
_기본 선택이 없는 예_

### <a name="xaml-implementation"></a>XAML 구현  
기본 XAML [**AutosuggestBox**](/uwp/api/windows.ui.xaml.controls.autosuggestbox)를 사용 하는 경우 모든 것이 이미 후크 되어 있습니다. [**텍스트 상자**](/uwp/api/windows.ui.xaml.controls.textbox) 와 목록을 사용 하 여 자동 제안 환경을 자동으로 만들려면 **텍스트 상자**에 목록을 [**ControlledPeers**](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 로 설정 해야 합니다. 이 속성을 추가 또는 제거할 때마다 [**ControlledPeers**](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 속성에 대 한 **AutomationPropertyChanged** 이벤트를 실행 해야 하며, 시나리오의 유형에 따라 사용자 고유의 [**SelectionItemPatternOnElementSelected**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트 또는 [**LayoutInvalidated**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) 이벤트를 발생 시켜야 합니다 .이 문서에서는이 문서의 앞부분에서 설명한 것입니다.

### <a name="html-implementation"></a>HTML 구현  
HTML에서 내장 컨트롤을 사용 하는 경우 UIA 구현은 이미 매핑 되었습니다. 다음은 이미 후크 된 구현의 예입니다.

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 사용자 고유의 컨트롤을 만드는 경우 W3C 표준에서 설명 하는 사용자 고유의 ARIA 컨트롤을 설정 해야 합니다.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>그래픽의 텍스트

가능 하면 그래픽에 텍스트를 포함 하지 마십시오. 예를 들어, 응용 프로그램에 [**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image) 요소로 표시 되는 이미지 소스 파일에 포함 된 텍스트는 보조 기술로 자동으로 액세스 하거나 읽을 수 없습니다. 그래픽에 텍스트를 사용해야 하는 경우 "alt 텍스트"의 값으로 제공하는 [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) 값에 해당 텍스트나 해당 텍스트의 의미에 대한 요약이 포함되도록 합니다. 벡터에서 [**경로의**](/uwp/api/Windows.UI.Xaml.Shapes.Path)일부로 텍스트 문자를 만들거나 [**문자 모양을**](/uwp/api/Windows.UI.Xaml.Documents.Glyphs)사용 하는 경우에도 비슷한 고려 사항이 적용 됩니다.

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>텍스트 글꼴 크기 및 배율

사용자는 글꼴이 매우 작은 경우에만 앱에서 텍스트를 읽는 데 어려움이 있으므로 응용 프로그램의 텍스트가 첫 번째 위치의 적당 한 크기 인지 확인 합니다.

명확 하 게 완료 되 면 Windows에는 사용자가 활용할 수 있는 다양 한 내게 필요한 옵션 도구 및 설정이 포함 되며,이를 통해 사용자는 자신의 요구와 텍스트 읽기를 위한 기본 설정을 변경할 수 있습니다. 여기에는 다음이 포함됩니다.

* UI의 선택한 영역을 확대 하는 돋보기 도구입니다. 앱에서 텍스트의 레이아웃을 사용 하 여 편집용으로 돋보기를 사용 하는 것이 어려울 수 있도록 해야 합니다.
* **설정->시스템->디스플레이 >크기 조정 및 레이아웃에 대**한 전역 크기 조정 및 해상도 설정 사용 가능한 크기 옵션은 표시 장치의 기능에 따라 달라질 수 있습니다.
* 설정의 텍스트 크기 설정 **-접근성 >표시를 >** 합니다. **텍스트 크게 만들기** 설정을 조정 하 여 모든 응용 프로그램 및 화면에서 지원 컨트롤의 텍스트 크기만 지정 합니다. 모든 UWP 텍스트 컨트롤은 사용자 지정 또는 템플릿 없이 텍스트 크기 조정 환경을 지원 합니다. 
> [!NOTE]
> **모든 항목을 크게** 설정 하면 사용자가 기본 화면 에서만 일반 텍스트 및 앱에 대 한 기본 설정 크기를 지정할 수 있습니다.

다양 한 텍스트 요소 및 컨트롤에는 [**IsTextScaleFactorEnabled**](/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled) 속성이 있습니다. 이 속성의 기본값은 **true** 입니다. **True 이면**해당 요소의 텍스트 크기를 조정할 수 있습니다. 크기 **조정은 크기가 작은**텍스트에 영향을 주는 것 보다 작은 **fontsize** 의 텍스트에 영향을 줍니다. 요소의 **IsTextScaleFactorEnabled** 속성을 **false**로 설정 하 여 자동 크기 조정을 사용 하지 않도록 설정할 수 있습니다. 

자세한 내용은 [텍스트 크기 조정](../input/text-scaling.md) 을 참조 하세요.

앱에 다음 태그를 추가 하 고 실행 합니다. **텍스트 크기** 설정을 조정 하 고 각 **TextBlock**에 발생 하는 결과를 확인 합니다.

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

모든 앱에서 전체적으로 UI 텍스트 크기를 조정 하는 것은 사용자에 게 중요 한 액세스 가능성 환경 이므로 텍스트 크기 조정을 사용 하지 않는 것이 좋습니다.

[**TextScaleFactorChanged**](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) 이벤트와 [**TextScaleFactor**](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) 속성을 사용 하 여 휴대폰에서 **텍스트 크기** 설정에 대 한 변경 내용을 확인할 수도 있습니다. 방법을 알아보겠습니다.

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

**TextScaleFactor** 의 값은 1, 2.25 범위의 double입니다 \[ \] . 가장 작은 텍스트는이 양만큼 확장 됩니다. 텍스트에 맞게 그래픽 크기를 조정 하는 값을 사용할 수 있습니다. 그러나 모든 텍스트는 동일한 배율로 크기가 조정 되는 것은 아닙니다. 일반적으로 더 큰 텍스트는부터 시작 하 여 크기 조정의 영향을 받지 않습니다.

이러한 형식에는 **IsTextScaleFactorEnabled** 속성이 있습니다.  
* [**ContentPresenter**](/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter)
* [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control) 및 파생 클래스
* [**FontIcon**](/uwp/api/Windows.UI.Xaml.Controls.FontIcon)
* [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)
* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)
* [**Textelement**](/uwp/api/Windows.UI.Xaml.Documents.TextElement) 및 파생 클래스

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  

* [텍스트 크기 조정](../input/text-scaling.md)
* [접근성](accessibility.md)
* [기본 접근성 정보](basic-accessibility-information.md)
* [XAML 텍스트 표시 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))
* [XAML 텍스트 편집 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
* [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)