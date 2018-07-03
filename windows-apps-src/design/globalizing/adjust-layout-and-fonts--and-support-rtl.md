---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: 레이아웃 및 글꼴 조정, RTL 지원
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 지역화 가능, 지역화, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: 52495740f76f59d5976e98d28147d05fca1e7a13
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1880814"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>레이아웃 및 글꼴 조정, RTL 지원
RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 설계합니다. 텍스트 흐름 방향은 스크립트가 작성되고 표시되는 방향이며 페이지의 UI 요소는 눈으로 스캔됩니다.

## <a name="layout-guidelines"></a>레이아웃 지침
독일어 및 핀란드어와 같은 언어는 일반적으로 영어보다 글자 수가 더 많습니다. 극동 지역의 글꼴 높이는 일반적으로 더 깁니다. 아랍어 및 히브리어와 같은 언어의 경우 레이아웃 패널 및 텍스트 요소의 읽는 순서가 RTL(오른쪽에서 왼쪽)로 배치됩니다.

번역된 텍스트의 메트릭에서 이러한 변형 때문에 절대 위치, 고정된 폭, 또는 고정된 높이를 UI(사용자 인터페이스)에 고정하는 것이 좋습니다. 대신 Windows UI 요소에 내장된 동적 레이아웃 메커니즘을 활용합니다. 예를 들어 콘텐츠 컨트롤(예: 단추), 항목 컨트롤(예: 그리드 보기 및 목록 보기) 및 콘텐츠 패널(예: 표 및 스택 패널)은 자동으로 콘텐츠에 맞춰 자동으로 크기를 조정하고 재배치합니다. UI 요소의 크기가 콘텐츠에 맞게 적절히 조정되지 않는 가장자리에 문제가 있는 경우를 발견하기 위해 앱을 의사 지역화합니다.

동적 레이아웃이 권장된 기술이며 대부분의 경우에서 사용할 수 있습니다. 덜 더 적합하지만 여전히 UI 태그에 크기를 맞추는 것보다 더 나은 방법은 언어 함수로 레이아웃 값을 설정하는 것입니다. 다음은 로컬라이저가 언어별로 적절하게 설정할 수 있는 리소스로 앱에서 Width 속성을 노출할 수 있는 방법의 예입니다. 먼저 앱의 리소스 파일(.resw)에서 "TitleText.Width"라는 이름의 속성 식별자를 추가합니다("TitleText"를 제외하고 원하는 아무 이름이나 사용할 수 있음). 그런 다음 **x:Uid**를 사용하여 하나 이상의 UI 요소를 이 속성 식별자와 연결합니다.

```xaml
<TextBlock x:Uid="TitleText">
```

리소스 파일(.resw), 속성 식별자 및 **x:Uid**에 대한 자세한 내용은 [UI 및 앱 패키지 매니페스트의 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)를 참조하세요.

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

이미지를 올바르게 전환하기 위해 앱에 다른 이미지가 필요한 경우 한정자가 `LayoutDirection`인 리소스 관리 시스템을 사용할 수 있습니다([언어, 크기 및 다른 한정자의 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)의 LayoutDirection 섹션 참조). 앱 런타임 언어([사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md) 참조)가 RTL 언어로 설정된 경우 시스템에서 `file.layoutdir-rtl.png`이라는 이미지를 선택합니다. 이미지의 일부가 전환된 경우 이러한 접근이 필요할 수 있지만 그 외의 경우는 그렇지 않습니다.

## <a name="handling-right-to-left-rtl-languages"></a>RTL(오른쪽에서 왼쪽) 언어 처리
앱이 RTL(오른쪽에서 왼쪽) 언어에 대해 현지화된 경우 [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 사용하고 대칭되는 안쪽 여백 및 여백을 설정하십시오. [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) 배율 및 대칭 이동과 같은 레이아웃 패널은 설정한 **FlowDirection** 값을 자동으로 따릅니다.

페이지의 루트 레이아웃 패널(또는 프레임)에서, 또는 페이지 자체에서 **FlowDirection**을 설정합니다. 그러면 포함된 모든 컨트롤이 해당 속성을 상속합니다.

> [!IMPORTANT]
> 그러나 **FlowDirection**은 Windows 설정에서 사용자가 선택한 표시 언어에 따라 자동으로 설정되지 *않습니다*. 사용자가 표시 언어를 전환해도 이에 대한 응답으로 동적으로 바뀌지 않습니다. 예를 들어 사용자가 Windows 설정을 영어에서 아랍어로 전환하는 경우 **FlowDirection** 속성은 자동으로 왼쪽에서 오른쪽에서 오른쪽에서 왼쪽으로 바뀌지 *않습니다*. 앱 개발자는 현재 표시되고 있는 언어에 대한 **FlowDirection**을 올바르게 설정해야 합니다.

프로그래밍 기술은 사용자 기본 표시 언어의 `LayoutDirection` 속성을 사용하여 [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 설정하는 것입니다(아래의 코드 예제 참조). Windows에 포함된 대부분의 컨트롤은 **FlowDirection**을 이미 사용합니다. 사용자 지정 컨트롤을 구현 중인 경우 RTL 및 LTR 언어에 올바른 레이아웃 변경을 위해 **FlowDirection**을 사용해야 합니다.

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

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
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

위의 기술은 **FlowDirection** 함수를 사용자 기본 표시 언어의 `LayoutDirection` 속성으로 만듭니다. 어떤 이유로 논리를 원하지 않는 경우, 앱에서 FlowDirection 속성을 지역화 도구가 번역할 각 언어에 대해 적절히 설정할 수 있는 리소스로 노출할 수 있습니다.

먼저 앱의 리소스 파일(.resw)에서 "MainPage.FlowDirection"라는 이름의 속성 식별자를 추가합니다("MainPage"를 제외하고 원하는 아무 이름이나 사용할 수 있음). 그런 다음 **x:Uid**를 사용하여 주 **Page** 요소(또는 루트 레이아웃 패널 또는 프레임)를 이 속성 식별자와 연결합니다.

```xaml
<Page x:Uid="MainPage">
```

이는 모든 언어에 한 줄 코드를 사용하는 것이 아니라 번역되는 각 언어에 대해 이 속성 리소스를 올바르게 "번역하는" 번역가에 달려 있습니다. 따라서 이 기술을 사용할 경우 사람의 오류가 추가적으로 있을 수 있음을 주의하십시오.

## <a name="important-apis"></a>중요 API
* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>관련 주제
* [UI 및 앱 패키지 매니페스트의 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)
* [언어, 크기 및 다른 한정자에 대해 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)