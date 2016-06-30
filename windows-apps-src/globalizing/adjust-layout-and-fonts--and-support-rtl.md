---
author: DelfCo
Description: "RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 개발합니다."
title: "레이아웃 및 글꼴 조정, RTL 지원"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 989d810724c925a5bcbebf5f7fb301636905fff9

---

# 레이아웃 및 글꼴 조정, RTL 지원





RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 개발합니다.

## <span id="Layout_guidelines"></span><span id="layout_guidelines"></span><span id="LAYOUT_GUIDELINES"></span>레이아웃 지침


일부 언어(예: 독일어 및 핀란드어)에는 영어보다 더 많은 텍스트 공간이 필요합니다. 일본어 같은 일부 언어의 글꼴은 높이가 더 높아야 합니다. 그리고 아랍어 및 히브리어와 같은 일부 언어에서는 텍스트 레이아웃과 앱 레이아웃이 RTL(오른쪽에서 왼쪽) 읽는 순서를 따라야 합니다.

절대 위치, 고정 너비 또는 고정 높이 대신 유연한 레이아웃 메커니즘을 사용하세요. 필요한 경우 언어에 따라 특정 UI 요소를 조정할 수 있습니다.

### <span id="XAML"></span><span id="xaml"></span>XAML

다음과 같이 요소에 대한 **Uid**를 지정합니다.

```XML
<TextBlock x:Uid="Block1">
```

앱의 ResW 파일에 Block1.Width에 대한 리소스가 있는지 확인합니다. 이 리소스는 지역화하는 언어별로 설정할 수 있습니다.

C++, C\# 또는 Visual Basic으로 작성한 Windows 스토어 앱의 경우 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 속성을 대칭 안쪽 여백 및 여백과 함께 사용하여 다른 레이아웃 방향에 대한 지역화를 사용하도록 설정합니다.

XAML 레이아웃 컨트롤(예: [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704))은 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 속성을 사용하여 자동으로 배율 조정 및 대칭 이동합니다. 앱의 고유한 **FlowDirection** 속성을 지역화 도구를 위한 리소스로 공개합니다.

앱의 기본 페이지에 대한 **Uid**를 지정합니다.

```XML
<Page x:Uid="MainPage">
```

앱의 **ResW** 파일에 MainPage.FlowDirection에 대한 리소스가 있는지 확인합니다. 이 리소스는 지역화하는 언어별로 설정할 수 있습니다.

### <span id="HTML"></span><span id="html"></span>HTML

JavaScript를 사용하는 Windows 스토어 앱의 경우 [CSS 스타일시트](https://msdn.microsoft.com/library/ms531209) 레이아웃 메커니즘(예: [-ms-grid](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#g_section) 및 [–ms-box](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#f_section))을 사용합니다. 다양한 레이아웃 방향에 대해 지역화를 사용하려면 대칭 안쪽 여백과 여백을 사용합니다.

앱에서 [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 의사 클래스 선택기를 사용하여 CSS 속성(예: 앱 언어 기반의 특정 요소에 대한 너비)을 조정할 수도 있습니다. 이를 위해 앱 호스트는 루트 요소의 **lang** 특성을 앱 언어로 설정합니다.

**CSS**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

ui-light.css 또는 ui-dark.css 스타일시트를 사용하는 JavaScript를 사용하는 Windows 스토어 앱은 앱 언어를 기반으로 본문 레이아웃 방향을 자동으로 설정합니다. 다음 CSS는 ui-light 및 ui-dark.css에 있으므로 개발자가 직접 작성할 필요가 없습니다.

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

즉, 시스템에서 오른쪽에서 왼쪽으로 읽는 언어를 사용하는 경우 대부분의 앱 레이아웃이 올바르게 설정됩니다.

[WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782) 컨트롤과 마찬가지로 앱에서 [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 의사 클래스 선택기를 사용하여 물리적 CSS 속성(예: **margin** 및 **padding**)을 조정할 수 있습니다. **after** 및 **before** 같은 키워드를 사용하는 논리적 CSS 속성은 조정할 필요가 없습니다.

HTML에서는 **align** 속성 또는 특성을 사용하지 마세요. 대신 **direction** 속성을 사용하여 특정 구성 요소의 맞춤을 제어합니다.

CSS에서 가로 방향 텍스트 레이아웃을 지원하려면 [**writing-mode**](https://msdn.microsoft.com/library/ms531187) 속성을 사용합니다.

## <span id="Mirroring_images"></span><span id="mirroring_images"></span><span id="MIRRORING_IMAGES"></span>이미지 미러링


### <span id="XAML"></span><span id="xaml"></span>XAML

앱에 RTL을 위해 미러링해야 하는 이미지가 있는 경우(즉, 동일한 이미지가 대칭될 수 있음) [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 속성을 적용할 수 있습니다.

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### <span id="HTML"></span><span id="html"></span>HTML

앱에 RTL에 대해 미러링해야 하는 이미지가 있는 경우(즉, 동일한 이미지가 대칭될 수 있음) .mirrorable 클래스를 요소에 추가하고 다음 CSS 클래스를 추가하여 렌더링할 때 CSS 변환을 사용하여 이미지를 미러링할 수 있습니다.

```CSS
.mirrorable { transform: scaleX(-1); }
```

**XAML 및 HTML:** 앱에서 이미지를 올바르게 대칭 이동하기 위해 다른 이미지가 필요한 경우 리소스 관리 시스템을 [layoutdir 한정자](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)와 함께 사용할 수 있습니다. [응용 프로그램 언어](manage-language-and-region.md)가 RTL 언어로 설정되어 있는 경우 file.layoutdir-rtl.png 이미지가 선택됩니다. 이 방법은 이미지의 일부만 대칭 이동되는 경우에 사용할 수 있습니다.

## <span id="Fonts"></span><span id="fonts"></span><span id="FONTS"></span>글꼴


**XAML 및 HTML:** 특정 언어에 대한 권장 글꼴 패밀리, 크기, 두께 및 스타일에 프로그래밍 방식으로 액세스하려면 [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) 글꼴 매핑 API를 사용합니다. **LanguageFont** 개체는 UI 헤더, 알림, 본문, 사용자 편집 가능한 문서 본문 글꼴을 비롯하여 콘텐츠의 다양한 범주에 대한 올바른 글꼴 정보에 액세스합니다.

### <span id="HTML"></span><span id="html"></span>HTML

ui-light.css 또는 ui-dark.css 스타일시트를 사용하는 JavaScript를 사용하는 Windows 스토어 앱은 앱 언어를 기반으로 가장 적절한 글꼴을 자동으로 설정합니다. 앱 호스트는 루트 요소의 **lang** 특성을 앱 언어로 설정합니다.

한 페이지에 여러 언어를 표시하는 앱은 각 언어의 섹션에 대해 **lang** 특성을 설정해야 합니다. [
            **:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 의사 클래스 선택기는 페이지의 각 섹션에 대해 올바른 글꼴을 선택합니다.

 

 






<!--HONumber=Jun16_HO4-->


