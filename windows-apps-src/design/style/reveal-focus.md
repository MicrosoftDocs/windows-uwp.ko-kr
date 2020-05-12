---
description: 포커스 표시는 사용자가 게임 패드 또는 키보드 포커스를 포커스 가능 요소로 이동하면 이러한 요소의 테두리에 애니메이션 효과를 주는 조명 효과입니다.
title: 포커스 표시
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 098c46499e65c34e3699b09e137ea94c40590ef7
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968009"
---
# <a name="reveal-focus"></a>포커스 표시

![영웅 이미지](images/header-reveal-focus.svg)

포커스 표시는 Xbox One, 텔레비전 화면 등의 [3m 환경](/windows/uwp/design/devices/designing-for-tv)에 조명 효과를 제공합니다. 사용자가 게임 패드 또는 키보드 포커스를 포커스 가능 요소(예: 단추)로 이동하면 이러한 요소의 테두리에 애니메이션 효과를 줍니다. 기본적으로 꺼져 있지만 사용하는 방법은 간단합니다. 

대화형 요소를 강조 표시하는 조명 효과인 강조 표시 효과의 경우 [강조 표시 문서](/windows/uwp/design/style/reveal)를 참조하세요.


> **중요 API**: [Application.FocusVisualKind 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [Control.UseSystemFocusVisuals 속성](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>작동 방식
포커스 표시는 요소의 테두리에 애니메이션 효과를 준 후광을 추가하여 포커스가 있는 요소를 강조 표시합니다.

![표시 화면 효과](images/traveling-focus-fullscreen-light-rf.gif)

이 기능은 사용자가 TV 화면 전체에 완전히 집중을 기울이지 못할 수 있는 3m 시나리오에서 특히 유용합니다. 

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/RevealFocus">앱을 열고 포커스 표시의 기능을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>사용 방법

포커스 표시는 기본적으로 꺼져 있습니다. 포커스 표시를 사용하도록 설정하려면
1. 앱의 생성자에서 [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) 속성을 호출하여 현재 디바이스 패밀리가 `Windows.Xbox`인지 확인합니다.
2. 디바이스 패밀리가 `Windows.Xbox`이면, [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) 속성을 `FocusVisualKind.Reveal`로 설정합니다. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

**FocusVisualKind** 속성을 설정하면, [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) 속성이 **True**(대부분의 컨트롤에서 기본값)로 설정된 모든 컨트롤에 표시 포커스 효과가 시스템에서 자동으로 적용됩니다. 

## <a name="why-isnt-reveal-focus-on-by-default"></a>표시 포커스가 기본적으로 켜져 있지 이유는 무엇인가요? 
앱이 Xbox에서 실행되고 있음을 감지하면 쉽게 포커스 표시를 켤 수 있습니다. 그렇다면 시스템에서 자동으로 켜지 않는 이유는 무엇일까요? 포커스 표시는 포커스 화면 효과의 크기를 늘리기 때문에 UI 레이아웃에서 문제가 발생할 수 있습니다. 경우에 따라 앱에 최적화하기 위해 포커스 표시 효과를 사용자 지정해야 합니다.

## <a name="customizing-reveal-focus"></a>포커스 표시 사용자 지정

각 컨트롤의 포커스 화면 효과 속성을 수정하여 포커스 표시 효과를 사용자 지정할 수 있습니다. [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush), [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) 등의 시각적 속성이 있습니다. 이러한 속성을 사용하여 포커스 영역의 색과 두께를 사용자 지정할 수 있습니다. [높은 가시성 포커스 화면 효과](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)를 만드는 데 사용하는 속성과 같습니다. 

하지만 사용자 지정을 시작하기 전에 포커스 표시를 구성하는 요소를 좀 더 자세히 알아두는 것이 도움이 됩니다.

기본 포커스 표시 화면 효과는 기본 테두리, 보조 테두리, 표시 후광의 세 부분으로 이루어져 있습니다. 기본 테두리는 **2px** 두께이고 보조 테두리 *외부*에서 실행됩니다. 보조 테두리는 **1px** 두께이고 기본 테두리 *내부*에서 실행됩니다. 포커스 표시 후광의 두께는 기본 테두리의 두께와 비례하며, 기본 테두리 ‘외부’에서 실행됩니다. 

정적 요소뿐 아니라, 포커스 표시 화면 효과는 사용하지 않을 때 진동하고 포커스를 이동할 때 포커스 방향으로 움직이는 애니메이션 효과를 준 조명을 제공합니다.

![포커스 표시 계층](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>테두리 두께 사용자 지정

컨트롤의 테두리 유형 두께를 변경하려면 다음 속성을 사용합니다.

| 테두리 유형 | 속성 |
| --- | --- |
| 기본, 후광   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> 기본 테두리를 변경하면 후광의 두께도 비례해서 늘어납니다.   |
| 보조   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


이 예제에서는 단추 포커스 화면 효과의 테두리 두께를 변경합니다.

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>여백 사용자 지정

여백은 컨트롤의 시각적 범위와 포커스 화면 효과 보조 테두리 시작 부분 사이의 간격입니다. 기본 여백은 컨트롤 범위에서 1px입니다. [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) 속성을 변경하여 컨트롤별로 이 여백을 편집할 수 있습니다.

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

음수 여백은 컨트롤의 중심에서 테두리를 멀리 이동하고, 양수 여백은 컨트롤의 중심으로 테두리를 가깝게 이동합니다.

## <a name="customize-the-color"></a>색 사용자 지정

포커스 표시 화면 효과의 색을 변경하려면 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) 및 [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) 속성을 사용합니다.

| 속성 | 기본 리소스 | 기본 리소스 값 |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

FocusPrimaryBrush 속성은 **FocusVisualKind**가 **Reveal**로 설정되어 있는 경우에만 기본적으로 **SystemControlRevealFocusVisualBrush** 리소스로 설정됩니다. 그러지 않으면 **SystemControlFocusVisualPrimaryBrush**를 사용합니다.


개별 컨트롤의 포커스 화면 효과 색을 변경하려면 컨트롤에서 직접 속성을 설정합니다. 이 예제에서는 단추의 포커스 화면 효과 색을 재정의합니다.

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

전체 앱에서 모든 포커스 화면 효과 색을 변경하려면 **SystemControlRevealFocusVisualBrush** 및 **SystemControlFocusVisualSecondaryBrush** 리소스를 직접 정의하여 재정의합니다.

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

포커스 화면 효과 색을 수정하는 방법에 대한 자세한 내용은 [색 브랜딩 및 사용자 지정](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing)을 참조하세요.


## <a name="show-just-the-glow"></a>후광만 표시

기본 또는 보조 포커스 화면 효과 없이 후광만 사용하려는 경우 컨트롤의 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) 속성을 `Transparent`로, [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)를 `0`으로 설정합니다. 이 경우 후광은 테두리가 없는 느낌을 주기 위해 컨트롤 배경색을 사용합니다. [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)를 사용하여 후광의 두께를 수정할 수 있습니다.

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>고유한 포커스 화면 효과 사용

포커스 표시를 사용자 지정하는 또 다른 방법은 시각적 상태로 고유한 화면 효과를 그려 시스템 제공 포커스 화면 효과를 옵트아웃하는 것입니다. 자세한 내용은 [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)을 참조하세요.


## <a name="reveal-focus-and-the-fluent-design-system"></a>포커스 표시 및 흐름 디자인 시스템

포커스 표시는 앱에 조명을 추가하는 흐름 디자인 시스템의 구성 요소입니다. Fluent Design 시스템 및 기타 구성 요소에 대한 자세한 내용은 [Fluent Design 개요](/windows/apps/fluent-design-system)를 참조하세요.

## <a name="related-articles"></a>관련된 문서

- [강조 표시](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Xbox 및 TV용 디자인](/windows/uwp/design/devices/designing-for-tv)
- [게임 패드 및 리모컨 조작](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
- [컴퍼지션 효과](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [시스템의 과학: 흐름 디자인 및 깊이](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [시스템의 과학: 흐름 디자인 및 조명](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
