---
description: RTL (오른쪽에서 왼쪽) 흐름 방향을 포함 하 여 여러 언어의 레이아웃 및 글꼴을 지원 하도록 앱을 디자인 합니다.
title: 레이아웃 및 글꼴 조정, RTL 지원
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 지역화 가능성, 지역화, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: 0e4725f6d26cf1abf42effddd813c31e87926e89
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033796"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>레이아웃 및 글꼴 조정, RTL 지원
RTL (오른쪽에서 왼쪽) 흐름 방향을 포함 하 여 여러 언어의 레이아웃 및 글꼴을 지원 하도록 앱을 디자인 합니다. 흐름 방향은 스크립트가 작성 되 고 표시 되는 방향이 며 페이지의 UI 요소는 눈에 따라 검색 됩니다.

## <a name="layout-guidelines"></a>레이아웃 지침
독일어 및 핀란드어와 같은 언어에서는 일반적으로 영어 보다 더 많은 문자를 사용 합니다. 극동 글꼴은 일반적으로 더 많은 높이가 필요 합니다. 아랍어 및 히브리어와 같은 언어의 경우에는 레이아웃 패널과 텍스트 요소를 RTL (오른쪽에서 왼쪽) 읽기 순서로 레이아웃 해야 합니다.

번역 된 텍스트 메트릭의 이러한 변형으로 인해 절대 위치 지정, 고정 폭 또는 고정 높이를 UI (사용자 인터페이스)에 굽기 하지 않는 것이 좋습니다. 대신 Windows UI 요소에 기본 제공 되는 동적 레이아웃 메커니즘을 활용 합니다. 예를 들어 콘텐츠 컨트롤 (예: 단추), 항목 컨트롤 (예: 그리드 뷰 및 목록 뷰), 레이아웃 패널 (예: 그리드 및 stackpanels)은 내용에 맞게 자동으로 크기를 조정 하 고 자동으로 리플로우 합니다. 응용 프로그램을 의사 지역화 하 여 UI 요소가 콘텐츠로 적절 하 게 조정 되지 않는 문제가 있는 경우를 파악 합니다.

동적 레이아웃은 권장 되는 기술 이며 대부분의 경우 사용할 수 있습니다. UI 태그에 크기를 굽는 하는 것 보다 더 명확 하지만 레이아웃 값을 언어의 함수로 설정 합니다. 다음은 지역화 담당자가 언어별로 적절 하 게 설정할 수 있는 리소스로 앱에서 Width 속성을 노출 하는 방법의 예입니다. 먼저, 앱의 리소스 파일 (. resw)에서 이름이 "TitleText"가 아닌 속성 식별자를 추가 합니다 ("TitleText" 대신 원하는 이름을 사용할 수 있음). 그런 다음, **x:Uid** 를 사용 하 여 하나 이상의 UI 요소를이 속성 식별자와 연결 합니다.

```xaml
<TextBlock x:Uid="TitleText">
```

리소스 파일 (. resw), 속성 식별자 및 **x:Uid** 에 대 한 자세한 내용은 [UI 및 앱 패키지 매니페스트에서 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)를 참조 하세요.

## <a name="fonts"></a>Fonts
[**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) 글꼴 매핑 클래스를 사용 하 여 특정 언어의 권장 글꼴 패밀리, 크기, 두께 및 스타일에 대해 프로그래밍 방식으로 액세스할 수 있습니다. **LanguageFont** 클래스는 UI 헤더, 알림, 본문 텍스트 및 사용자가 편집할 수 있는 문서 본문 글꼴을 비롯 한 다양 한 범주의 콘텐츠를 위한 올바른 글꼴 정보에 대 한 액세스를 제공 합니다.

## <a name="mirroring-images"></a>미러링 이미지
앱이 RTL에 대해 미러링 되어야 하는 이미지 (즉, 동일한 이미지를 대칭 이동할 수 있음)가 있는 경우 **Flowdirection** 을 사용할 수 있습니다.

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

앱에서 이미지를 올바르게 대칭 이동 하는 데 다른 이미지가 필요한 경우에는 한정자를 사용 하 여 리소스 관리 시스템을 사용할 수 있습니다 `LayoutDirection` ( [언어, 크기 조정 및 기타 한정자에 대 한 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)의 layoutdirection 섹션 참조). 시스템은 `file.layoutdir-rtl.png` 앱 런타임 언어 ( [사용자 프로필 언어 및 응용 프로그램 매니페스트 언어 이해](manage-language-and-region.md)참조)가 RTL 언어로 설정 된 경우 이름이 지정 된 이미지를 선택 합니다. 이 방법은 이미지의 일부가 대칭 이동 했지만 다른 부분은 그렇지 않은 경우에 필요할 수 있습니다.

## <a name="handling-right-to-left-rtl-languages"></a>RTL (오른쪽에서 왼쪽) 언어 처리
앱이 RTL (오른쪽에서 왼쪽) 언어에 대해 지역화 된 경우에는 [**FrameworkElement. FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 사용 하 고 대칭 안쪽 여백 및 여백을 설정 합니다. [**표**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) 눈금 등의 레이아웃 패널은 설정 하는 **flowdirection** 의 값을 사용 하 여 자동으로 대칭 이동 합니다.

페이지의 루트 레이아웃 패널 (또는 프레임) 또는 페이지 자체에서 **Flowdirection** 을 설정 합니다. 이렇게 하면 내에 포함 된 모든 컨트롤이 해당 속성을 상속 합니다.

> [!IMPORTANT]
> 그러나 **Flowdirection** 은 Windows 설정에서 사용자가 선택한 표시 언어에 따라 자동으로 설정 *되지* 않습니다. 또한 사용자 전환 표시 언어에 대 한 응답으로 동적으로 변경 되지 않습니다. 예를 들어 사용자가 Windows 설정을 영어에서 아랍어로 전환 하면 **Flowdirection** 속성이 왼쪽에서 오른쪽으로 왼쪽에서 오른쪽으로 자동으로 변경 *되지* 않습니다. 앱 개발자는 현재 표시 되는 언어에 맞게 **Flowdirection** 을 적절 하 게 설정 해야 합니다.

프로그래밍 방식의 방법은 `LayoutDirection` 기본 사용자 표시 언어의 속성을 사용 하 여 [**flowdirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 설정 하는 것입니다 (아래 코드 예제 참조). Windows에 포함 된 대부분의 컨트롤은 **Flowdirection** 을 이미 사용 합니다. 사용자 지정 컨트롤을 구현 하는 경우에는 **Flowdirection** 을 사용 하 여 RTL 및 LTR 언어의 레이아웃을 적절 하 게 변경 해야 합니다.

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

위의 기법은 기본 사용자 표시 언어의 속성 함수를 **Flowdirection** 으로 만듭니다 `LayoutDirection` . 이러한 논리를 원하지 않는 경우에는 지역화가 번역 하는 각 언어에 대해 적절 하 게 설정할 수 있는 리소스로 앱에서 FlowDirection 속성을 노출할 수 있습니다.

먼저, 앱의 리소스 파일 (. resw)에서 이름이 "mainpage. FlowDirection" 인 속성 식별자를 추가 합니다 ("MainPage" 대신 원하는 이름을 사용할 수 있음). 그런 다음 **x:Uid** 를 사용 하 여 기본 **페이지** 요소 (또는 해당 루트 레이아웃 패널 또는 프레임)를이 속성 식별자와 연결 합니다.

```xaml
<Page x:Uid="MainPage">
```

모든 언어에 대해 단일 코드 줄 대신 번역 된 각 언어에 대해이 속성 리소스를 "변환" 하는 변환기에 따라 다릅니다. 이 기법을 사용 하는 경우 인간 오류에 대 한 추가 기회가 있습니다.

## <a name="important-apis"></a>중요 API
* [FrameworkElement. FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>관련 항목
* [UI 및 앱 패키지 매니페스트의 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)
* [언어, 크기 조정 및 기타 한정자에 맞게 리소스를 조정 합니다.](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)
