---
author: Jwmsft
Description: Hyperlinks navigate the user to another part of the app, to another app, or launch a specific uniform resource identifier (URI) using a separate browser app.
title: 하이퍼링크
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e6679f789f2530a34a2bf527556c144e7a8e03c3
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6659218"
---
# <a name="hyperlinks"></a>하이퍼링크

 

하이퍼링크는 사용자를 앱의 다른 부분이나 다른 앱으로 이동하거나, 별도의 브라우저 앱을 사용하여 특정 URI(Uniform Resource Identifier)를 실행합니다. XAML 앱에 하이퍼링크를 추가할 수 있는 두 가지 방법(**Hyperlink** 텍스트 요소 및 **HyperlinkButton** 컨트롤)이 있습니다.

> **중요 API**: [Hyperlink 텍스트 요소](https://msdn.microsoft.com/library/windows/apps/dn279356), [HyperlinkButton 컨트롤](https://msdn.microsoft.com/library/windows/apps/br242739)

![하이퍼링크 단추](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

선택하면 반응하고 선택한 텍스트에 대한 추가 정보로 이동되는 텍스트가 필요할 때는 하이퍼링크를 사용합니다.

필요에 따라 적절한 유형의 하이퍼링크를 선택합니다.

-   텍스트 컨트롤 내부에 인라인 **Hyperlink** 텍스트 요소를 사용합니다. Hyperlink 요소는 다른 텍스트 요소와 함께 제공되며 모든 InlineCollection에서 사용할 수 있습니다. 자동 텍스트 배치를 원하며 누르기 대상이 반드시 클 필요가 없는 경우에 텍스트 하이퍼링크를 사용합니다. 하이퍼링크 텍스트가 작아 터치하기 어려울 수도 있습니다.
-   **HyperlinkButton**은 독립 실행형 하이퍼링크에 사용합니다. HyperlinkButton은 특수화된 단추 컨트롤로, 단추를 사용하는 모든 위치에서 사용할 수 있습니다.
-   클릭할 수 있는 이미지를 만들려면 콘텐츠로 [이미지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx)가 있는 **HyperlinkButton**을 사용합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/HyperlinkButton">앱을 열고 작동 중인 HyperlinkButton을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-hyperlink-text-element"></a>Hyperlink 텍스트 요소 만들기

이 예제는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 내에서 Hyperlink 텍스트 요소를 사용하는 방법을 보여 줍니다.

```xml
<StackPanel Width="200">
    <TextBlock Text="Privacy" Style="{StaticResource SubheaderTextBlockStyle}"/>
    <TextBlock TextWrapping="WrapWholeWords">
        <Span xml:space="preserve"><Run>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Read the </Run><Hyperlink NavigateUri="http://www.contoso.com">Contoso Privacy Statement</Hyperlink><Run> in your browser.</Run> Donec pharetra, enim sit amet mattis tincidunt, felis nisi semper lectus, vel porta diam nisi in augue.</Span>
    </TextBlock>
</StackPanel>

```
하이퍼링크는 인라인으로 표시되고 주변 텍스트와 함께 제공됩니다.

![텍스트 요소로 표시되는 하이퍼링크의 예](images/controls_hyperlink-element.png) 

> **팁**&nbsp;&nbsp;XAML에서 텍스트 컨트롤에 다른 텍스트 요소와 함께 Hyperlink를 사용하는 경우에는 콘텐츠를 [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) 컨테이너에 넣고 `xml:space="preserve"` 특성을 Span에 적용하여 Hyperlink와 기타 요소 사이의 공백을 유지합니다.

## <a name="create-a-hyperlinkbutton"></a>HyperlinkButton 만들기

다음은 텍스트와 이미지가 모두 있는 HyperlinkButton을 사용하는 방법입니다.

```xml
<StackPanel>
    <TextBlock Text="About" Style="{StaticResource TitleTextBlockStyle}"/>
    <HyperlinkButton NavigateUri="http://www.contoso.com">
        <Image Source="Assets/ContosoLogo.png"/>
    </HyperlinkButton>
    <TextBlock Text="Version: 1.0.0001" Style="{StaticResource CaptionTextBlockStyle}"/>
    <HyperlinkButton Content="Contoso.com" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Acknowledgments" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Help" NavigateUri="http://www.contoso.com"/>
</StackPanel>

```

텍스트 콘텐츠가 있는 하이퍼링크 단추는 태그 지정 텍스트로 표시됩니다. Contoso 로고 이미지도 클릭 가능한 하이퍼링크입니다.

![단추 컨트롤로 표시되는 하이퍼링크의 예](images/controls_hyperlink-button-image.png)

이 예제에서는 코드에서 HyperlinkButton을 만드는 방법을 보여줍니다.

```csharp
var helpLinkButton = new HyperlinkButton();
helpLinkButton.Content = "Help";
helpLinkButton.NavigateUri = new Uri("http://www.contoso.com");
```

## <a name="handle-navigation"></a>이동 처리

두 종류의 하이퍼링크에 대한 이동 처리 방법은 동일하며 **NavigateUri** 속성을 설정하거나 **Click** 이벤트를 처리할 수 있습니다.

**URI로 이동**

URI로 이동하기 위해 하이퍼링크를 사용하려면 NavigateUri 속성을 설정합니다. 사용자가 하이퍼링크를 클릭하거나 탭하면 지정된 URI가 기본 브라우저에서 열립니다. 기본 브라우저는 앱과는 별도의 프로세스에서 실행됩니다.

> [!NOTE]
> URI는 [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) 클래스에 의해 표시됩니다. .NET으로 프로그래밍할 때 이 클래스는 숨겨지므로 [System.Uri](https://docs.microsoft.com/dotnet/api/system.uri) 클래스를 사용해야 합니다. 자세한 내용은 이들 클래스에 대한 참조 페이지를 확인하세요.

**http:** 또는 **https:** 체계를 사용하지 않아도 됩니다. 브라우저에서 로드하기 적합한 위치에 리소스 콘텐츠가 있으면 **ms-appx:**, **ms-appdata:** 또는 **ms-resources:** 같은 체계를 사용할 수 있습니다. 그러나 **file:** 체계는 특별히 차단됩니다. 자세한 내용은 [URI 체계](https://msdn.microsoft.com/library/windows/apps/jj655406.aspx)를 참조하세요.

사용자가 하이퍼링크를 클릭하면 NavigateUri 속성 값이 URI 형식 및 체계에 대한 시스템 처리기에 전달됩니다. 그런 다음 시스템은 NavigateUri에 제공된 URI의 체계를 등록한 앱을 시작합니다.

하이퍼링크가 기본 웹 브라우저에 콘텐츠를 로드하지 않도록 않고 브라우저를 표시하고 싶지 않은 경우 NavigateUri 값을 설정하지 않습니다. 대신 Click 이벤트를 처리하고 원하는 작업을 수행하는 코드를 작성합니다.


**Click 이벤트 처리**

앱 내 이동과 같이 브라우저에서의 URI 실행 이외의 작업에 대해 Click 이벤트를 사용합니다. 예를 들어 브라우저를 열지 않고 새 앱 페이지를 로드하려는 경우 새 앱 페이지로 이동하기 위해 Click 이벤트 처리기 내에서 [Frame.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.navigate.aspx) 메서드를 호출합니다. 외부, 절대 URI를 앱에 있는 [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) 컨트롤 내에서 로드하려는 경우 클릭 처리기 논리의 일부로 [WebView.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.navigate.aspx)를 호출합니다.

일반적으로 Click 이벤트 처리와 NavigateUri 값 지정을 동시에 수행하지 않는데 이는 이들이 Hyperlink 요소를 사용하는 대표적인 두 가지 다른 방법이기 때문입니다. 기본 브라우저에서 URI를 열려고 하고 NavigateUri에 대한 값을 지정한 경우 Click 이벤트는 처리하지 않습니다. 반대로, Click 이벤트를 처리하는 경우에는 NavigateUri를 지정하지 않습니다.

기본 브라우저에서 NavigateUri에 지정된 모든 유효한 대상이 로드되지 않도록 하는 데 Click 이벤트 처리기 내에서 수행할 수 있는 작업은 없습니다. 이 작업은 하이퍼링크가 활성화되면 자동으로(비동기적으로) 수행되며 Click 이벤트 처리기 내에서 취소할 수 없습니다. 

## <a name="hyperlink-underlines"></a>하이퍼링크 밑줄
기본적으로 하이퍼링크에 밑줄이 표시됩니다. 이 밑줄은 접근성 요구 사항을 충족하는 데 도움이 되므로 중요합니다. 색맹인 사용자는 밑줄을 사용하여 하이퍼링크 및 기타 텍스트를 구분합니다. 밑줄을 사용하지 않도록 설정하려면 하이퍼링크와 다른 텍스트를 구분할 수 있는 다른 유형의 서식(FontWeight 또는 FontStyle)을 지정하는 것이 좋습니다.

**Hyperlink 텍스트 요소**

[UnderlineStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.hyperlink.underlinestyle.aspx) 속성에서 밑줄을 사용하지 않도록 설정할 수 있습니다. 그렇게 하려면 [FontWeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontweight.aspx) 또는 [FontStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontstyle.aspx)을 사용하여 링크 텍스트를 구분하는 것이 좋습니다.

**HyperlinkButton** 

[Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx) 속성에 대한 값으로 문자열을 설정하면 기본적으로 HyperlinkButton이 밑줄이 그어진 텍스트로 나타납니다.

다음과 같은 경우에는 텍스트에 밑줄이 나타나지 않습니다.
- [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 Content 속성에 대한 값으로 설정하고 TextBlock에 [Text](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.text.aspx) 속성을 설정합니다.
- HyperlinkButton 템플릿을 다시 만들고 [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx) 템플릿 요소의 이름을 변경합니다.

밑줄이 없는 텍스트로 표시되는 단추가 필요한 경우 표준 단추 컨트롤을 사용하고 기본 제공 `TextBlockButtonStyle` 시스템 리소스를 해당 Style 속성에 적용합니다.

## <a name="notes-for-hyperlink-text-element"></a>Hyperlink 텍스트 요소에 대한 참고 사항

이 섹션은 Hyperlink 텍스트 요소에만 적용되고 HyperlinkButton 컨트롤에는 적용되지 않습니다.

**입력 이벤트**

Hyperlink는 [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx)가 아니기 때문에 Tapped, PointerPressed 등의 UI 요소 입력 이벤트 집합은 없습니다. 대신 Hyperlink에는 고유 Click 이벤트와 NavigateUri로 지정된 모든 URI를 로드하는 시스템의 암시적 동작이 있습니다. 시스템은 Hyperlink 작업을 호출하여 그 응답으로 Click 이벤트를 발생시키는 모든 입력 작업을 처리합니다.

**콘텐츠**

Hyperlink는 해당 [Inlines](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.inlines.aspx) 컬렉션에 존재할 수 있는 콘텐츠를 제한합니다. 특히 Hyperlink는 다른 Hyperlink가 없는 [Run](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.run.aspx) 및 기타 [Span]() 형식만 허용합니다. [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.inlineuicontainer.aspx)는 Hyperlink의 Inlines 컬렉션에 있을 수 없습니다. 제한된 콘텐츠를 추가하려고 하면 잘못된 인수 예외 또는 XAML 구문 분석 예외가 발생합니다.

**하이퍼링크와 테마/스타일 동작**

Hyperlink는 [Control](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.aspx)에서 상속되지 않으므로 [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.style.aspx) 속성 또는 [Template](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.template.aspx)이 없습니다. 하이퍼링크의 모양을 변경하기 위해 Foreground 또는 FontFamily와 같은 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.aspx)에서 상속되는 속성은 편집할 수 있지만 변경 내용을 적용하는 데 일반적인 스타일 또는 템플릿은 사용할 수 없습니다. 템플릿을 사용하는 대신 Hyperlink 속성 값에 대한 공통 리소스를 사용하여 일관성을 제공하는 것이 좋습니다. Hyperlink의 일부 속성은 시스템에서 제공하는 {ThemeResource} 태그 확장 값의 기본값을 사용합니다. 이렇게 하면 사용자가 런타임 시 시스템 테마를 변경할 때 하이퍼링크 모양을 적절한 방법으로 전환할 수 있습니다.

하이퍼링크의 기본 색상은 시스템의 테마 컬러입니다. [Foreground](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.foreground.aspx) 속성에서 이를 재정의할 수 있습니다.

## <a name="recommendations"></a>권장 사항

-   하이퍼링크는 탐색하는 데만 사용하고 다른 작업에는 사용하지 않도록 합니다.
-   텍스트 기반 하이퍼링크의 유형 램프에서 본문 스타일을 사용합니다. [fonts and the Windows 10 type ramp](../style/typography.md)를 참조하세요.
-   사용자 구별할 수 있고 각각을 쉽게 선택할 수 있게 하이퍼링크 사이에 충분한 공간을 둡니다.
-   사용자가 이동될 위치를 나타내는 도구 설명을 하이퍼링크에 추가합니다. 사용자가 외부 사이트로 이동되면 도구 설명 내에 최상위 도메인 이름을 포함하고 텍스트에 보조 글꼴 색을 지정합니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [텍스트 컨트롤](text-controls.md)
- [도구 설명에 대한 지침](tooltips.md)

**개발자용(XAML)**
- [Windows.UI.Xaml.Documents.Hyperlink 클래스](https://msdn.microsoft.com/library/windows/apps/dn279356)
- [Windows.UI.Xaml.Controls.HyperlinkButton 클래스](https://msdn.microsoft.com/library/windows/apps/br242739)
