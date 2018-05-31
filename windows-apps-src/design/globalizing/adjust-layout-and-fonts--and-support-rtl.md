---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: 레이아웃 및 글꼴 조정, RTL 지원
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 지역화 가능, 지역화, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: cf3a2d781dc916fbda9a9d6386dee4e2e6144873
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673980"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>레이아웃 및 글꼴 조정, RTL 지원

RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 설계합니다. 텍스트 흐름 방향은 스크립트가 작성되고 표시되는 방향이며 페이지의 UI 요소는 눈으로 스캔됩니다.

## <a name="layout-guidelines"></a>레이아웃 지침

독일어 및 핀란드어와 같은 언어는 일반적으로 영어보다 글자 수가 더 많습니다. 극동 지역의 글꼴 높이는 일반적으로 더 깁니다. 아랍어 및 히브리어와 같은 언어의 경우 레이아웃 패널 및 텍스트 요소의 읽는 순서가 RTL(오른쪽에서 왼쪽)로 배치됩니다.

번역된 텍스트의 변수 길이 때문에 절대 위치, 고정 너비 또는 고정 높이 대신 동적 UI 레이아웃 메커니즘을 사용해야 합니다. 실제 제대로 지역화되지 않은 앱은 UI 요소의 크기가 콘텐츠에 맞게 적절히 조정되지 않는 한 가장자리에 문제가 있을 수 있습니다.

RTL 언어의 경우 [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 사용하고 대칭되는 안쪽 여백 및 여백을 설정하십시오. [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) 배율 및 대칭 이동과 같은 레이아웃 패널은 설정한 **FlowDirection** 값을 자동으로 따릅니다.

다음은 로컬라이저가 적절하게 설정할 수 있는 리소스로 앱에서 FlowDirection 속성을 노출할 수 있는 방법의 예입니다.

먼저 앱의 리소스 파일(.resw)에서 "MainPage.FlowDirection"라는 이름의 속성 ID를 추가합니다("MainPage"를 제외하고 원하는 아무 이름이나 사용할 수 있음).

그런 다음 **x:Uid**를 사용하여 주 **Page** 요소를 이 속성 ID와 연결합니다.

```xaml
<Page x:Uid="MainPage">
```

리소스 파일(.resw), 속성 ID 및 **x:Uid**에 대한 자세한 내용은 [UI 및 앱 패키지 매니페스트의 문자열 지역화하기](../../app-resources/localize-strings-ui-manifest.md)을(를) 참조하세요.

언어 기반의 UI 요소에서 절대적 레이아웃 값 설정을 피해야 합니다. 그러나 어쩔 수 없이 사용해야 하는 경우 "TitleText.Width" 형식의 속성 ID를 생성할 수 있습니다.

```xaml
<TextBlock x:Uid="TitleText">
```

## <a name="fonts"></a>글꼴

특정 언어의 권장 글꼴 패밀리, 크기, 두께 및 스타일에 대한 프로그래밍 방식 액세스는 [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) 글꼴 매핑을 사용하십시오. **LanguageFont** 클래스를 통해 UI 헤더, 알림, 본문 텍스트 및 사용자 수정 가능 문서 본문 글꼴을 포함한 다양한 콘텐츠 양식에 대한 정확한 글꼴 정보에 액세스할 수 있습니다.

## <a name="mirroring-images"></a>미러링 이미지

RTL에 대해 미러링해야 하는 이미지(동일한 이미지가 대칭될 수 있음)가 앱에 있는 경우 **FlowDirection**을 사용할 수 있습니다.

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

이미지를 올바르게 전환하기 위해 앱에 다른 이미지가 필요한 경우 한정자가 `LayoutDirection`인 리소스 관리 시스템을 사용할 수 있습니다([언어, 크기 및 다른 한정자의 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)의 LayoutDirection 섹션 참조). 앱 런타임 언어([사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md) 참조)가 RTL 언어로 설정된 경우 시스템에서 `file.layoutdir-rtl.png`이라는 이미지를 선택합니다. 이미지의 일부가 전환된 경우 이러한 접근이 필요할 수 있지만 그외의 경우는 그렇지 않습니다.

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>RTL 언어 처리를 위한 모범 사례

앱이 RTL 언어에 대해 지역화된 경우 API를 사용하여 페이지의 루트 레이아웃 패널의 기본 텍스트 방향을 설정합니다. 이렇게 하면 루트 패널에 포함된 모든 컨트롤이 기본 텍스트 방향에 적절히 대응하게 됩니다. 둘 이상의 언어가 지원되는 경우 상위 앱 런타임 언어에 대한 `LayoutDirection`을 사용하여 [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 설정하십시오(아래 코드 예 참조). Windows에 포함된 대부분의 컨트롤은 **FlowDirection**을 이미 사용합니다. 사용자 지정 컨트롤을 구현 중인 경우 RTL 및 LTR 언어에 올바른 레이아웃 변경을 위해 **FlowDirection**을 사용해야 합니다.

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

### <a name="rtl-faq"></a>RTL FAQ 

**Q:** **FlowDirection**은 현재 언어 선택에 따라 자동으로 설정되나요? 예를 들어 영어를 선택하는 경우 왼쪽에서 오른쪽으로 표시되고 아랍어를 선택하는 경우 오른쪽에서 왼쪽으로 표시되나요?

> **A:** **FlowDirection**은 언어를 고려하지 않습니다. 현재 표시되고 있는 언어에 대한 **FlowDirection**을 올바르게 설정합니다. 위 샘플 코드를 참조하세요.

**Q:** 지역화에 대해 잘 알지 못합니다. 리소스에 텍스트 방향에 대해 이미 나와 있습니까? 현재 언어에서 텍스트 방향을 결정할 수 있습니까?

> **A:** 현재 모범 사례를 사용하고 있는 경우 리소스에 텍스트 방향이 직접적으로 나와 있지 않습니다. 현재 언어에 대한 텍스트 방향을 지정해야 합니다. 이를 위한 두 가지 방법은 다음과 같습니다.
> 
> 첫 번째 방법은 기본 설정 첫 번째 언어의 **LayoutDirection**을 사용하여 RootFrame의 **FlowDirection** 속성을 설정하는 것입니다. RootFrame의 모든 컨트롤은 RootFrame의 FlowDirection을 상속합니다.
> 
> 다른 방법으로는 지역화 중인 RTL 언어의 .resw 파일에서 FlowDirection을 설정하는 것입니다. 예를 들어 아랍어 resw 파일과 히브리어 resw 파일이 있을 수 있습니다. 이러한 파일에서 x:UID를 사용하여 FlowDirection을 설정할 수 있습니다. 그러나 이 방식은 프로그래밍 방식에 비해 오류가 발생하기 쉽습니다.

## <a name="important-apis"></a>중요 API

* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>관련 주제

* [UI 및 앱 패키지 매니페스트의 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)
* [언어, 크기 및 다른 지정자에 대해 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)