---
author: cphilippona
description: 포커스 표시는 사용자가 게임 패드 또는 키보드 포커스를 이동하면 포커스를 맞출 수 있는 요소의 테두리를 애니메이션화하는 조명 효과입니다.
title: 포커스 표시
template: detail.hbs
ms.author: mijacobs
ms.date: 03/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: high
ms.openlocfilehash: f545cf38897e44dc2b3da9fac139f37bf10fc50a
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="reveal-focus"></a>포커스 표시

포커스 표시는 Xbox One 및 텔레비전 화면 등의 [3m 환경](/windows/uwp/design/devices/designing-for-tv)에 조명 효과를 제공합니다. 사용자가 게임 패드 또는 키보드 포커스를 이동하면 버튼과 같이 포커스 맞출 수 있는 요소의 테두리를 애니메이션화합니다. 기본적으로 꺼져 있지만 설정하는 방법은 간단합니다. 

(강조 표시 효과의 경우 대화형 요소를 강조 표시하는 조명 효과는 [문서 강조 표시](/windows/uwp/design/style/reveal)를 참조하세요.)


> **중요한 API**: [Application.FocusVisualKind 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [Control.UseSystemFocusVisuals 속성](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>작동 방식
포커스 표시는 요소의 테두리에 애니메이션 빛을 추가하여 포커스된 요소를 강조 표시합니다.

![Visual 표시](images/traveling-focus-fullscreen-light-rf.gif)

이 기능은 사용자가 TV 화면 전체에 완전히 집중을 기울이지 못할 수 있는 3m 시나리오에서 특히 유용합니다. 

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/RevealFocus">앱을 열고 작동 중인 포커스 표시를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>사용 방법

기본적으로 포커스 표시는 꺼져 있습니다. 이를 사용하도록 설정하려면:
1. 앱의 생성자에서 [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) 속성을 호출하여 현재 장치 제품군이 `Windows.Xbox`인지 확인합니다.
2. 장치 제품군이 `Windows.Xbox`인 경우 [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) 속성을 `FocusVisualKind.Reveal`로 설정합니다. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

**FocusVisualKind** 속성을 설정하면 시스템은 표시 포커스 효과를 자동으로 [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) 속성이 **True**(대부분의 컨트롤에서 기본값)로 설정된 모든 컨트롤에 적용합니다. 

## <a name="why-isnt-reveal-focus-on-by-default"></a>표시 포커스가 기본적으로 설정되지 않는 이유는 무엇인가요? 
알 수 있듯이 앱이 Xbox에서 실행 중인 경우 이를 감지하여 포커스 표시를 켜는 것은 상당히 쉽습니다. 그렇다면 왜 시스템에서 자동으로 켜지지 않는 것일까요? 포커스 표시는 포커스 화면 효과의 크기를 늘리기 때문에 UI 레이아웃에서 문제가 발생할 수 있습니다. 경우에 따라 앱에 대해 최적화하기 위해 포커스 표시 효과를 사용자 지정해야 합니다.

## <a name="customizing-reveal-focus"></a>포커스 표시 사용자 지정

[FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush), [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) 등의 각 컨트롤에 대한 포커스 표시 효과 속성을 수정하여 포커스 표시 효과를 사용자 지정할 수 있습니다. 이러한 속성을 사용하면 포커스 영역의 색과 두께를 사용자 지정할 수 있습니다. (이들은 [높은 가시성 포커스 화면 효과](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)를 만드는 데 사용하는 속성과 동일합니다.) 

하지만 사용자 지정을 시작하기 전에 포커스 표시를 구성하는 요소에 대해 좀 더 자세히 알아두는 것이 도움이 됩니다.

기본 포커스 표시 화면 효과는 기본 테두리, 보조 테두리, 그리고 표시 빛의 세 부분으로 이루어져 있습니다. 기본 테두리는 **2px** 두께이고 보조 테두리 *외부*에서 실행됩니다. 보조 테두리는 **1px** 두께이고 기본 테두리 *내부*에서 실행됩니다. 포커스 표시 빛의 두께는 기본 테두리의 두께와 비례하며 기본 테두리 *외부*에서 실행됩니다.

고정 요소 외에 포커스 표시 화면 효과는 사용되지 않을 때 진동하고 포커스를 이동할 때 포커스 방향으로 움직이는 애니메이션 빛을 강조 표시합니다.

![포커스 표시 계층](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>테두리 두께 사용자 지정

컨트롤의 테두리 유형의 두께를 변경하려면 다음 속성을 사용합니다.

| 테두리 유형 | 속성 |
| --- | --- |
| 기본, 빛   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (기본 테두리를 변경하면 빛의 두께도 비례해서 늘어납니다.)   |
| 보조   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


이 예에서는 버튼의 포커스 화면 효과의 테두리 두께를 변경합니다.

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>여백 사용자 지정

여백은 컨트롤의 시각적 범위와 포커스 화면 효과 보조 테두리 시작 부분 사이의 간격입니다. 기본 여백은 컨트롤 범위에서 1px입니다. [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) 속성을 변경하여 컨트롤별로 이 여백을 편집할 수 있습니다.

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

음수 여백은 컨트롤의 중심에서 테두리를 멀리 이동하고 양수 여백은 컨트롤의 중심으로 테두리를 더 가깝게 이동합니다.

## <a name="customize-the-color"></a>색상 사용자 지정

포커스 표시 화면 효과의의 색을 변경하려면 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) 및 [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) 속성을 사용합니다.

| 속성 | 기본 리소스 | 기본 리소스 값 |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(FocusPrimaryBrush 속성은 **FocusVisualKind**가 **표시**로 설정되어 있는 경우 **SystemControlRevealFocusVisualBrush** 리소스에 대해서만 기본값입니다. 그렇지 않으면 **SystemControlFocusVisualPrimaryBrush**를 사용합니다.)


개별 컨트롤의 포커스 화면 효과 색상을 변경하려면 컨트롤에서 직접 속성을 설정합니다. 이 예에서는 버튼의 포커스 화면 효과 색을 재정의합니다.

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

    <!-- Override Reveal focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

포커스 화면 효과의 색 수정에 대한 자세한 내용은 [색 브랜딩 및 사용자 지정](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing)을 참조합니다.


## <a name="show-just-the-glow"></a>빛만 표시

기본 또는 보조 포커스 화면 효과 없이 빛만 사용하고자 하는 경우 컨트롤의 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) 속성을 `Transparent`로, [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)를 `0`으로 설정합니다. 이 경우 빛은 테두리가 없는 느낌을 제공하기 위해 컨트롤의 배경색을 채택됩니다. [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)를 사용하여 빛의 두께를 수정할 수 있습니다.

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>차제 포커스 화면 효과 사용

포커스 표시를 사용자 지정하는 다른 방법은 시각적 상태를 사용하여 고유한 화면 효과를 그려 시스템 제공 포커스 화면 효과를 옵트아웃하는 것입니다. 자세히 알아보려면 [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)을 참조합니다.


## <a name="reveal-focus-and-the-fluent-design-system"></a>포커스 표시 및 Fluent 디자인 시스템

포커스 표시는 앱에 조명을 추가하는 Fluent 디자인 시스템 구성 요소입니다. Fluent 디자인 시스템 및 기타 구성 요소에 대한 자세한 내용은 [UWP용 Fluent 디자인 개요](../fluent-design-system/index.md)를 참조하세요.

## <a name="related-articles"></a>관련 문서

- [강조 표시](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Xbox 및 TV용 디자인](/windows/uwp/design/devices/designing-for-tv)
- [게임 패드 및 리모컨 조작](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [컴퍼지션 효과](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [시스템의 과학: Fluent 디자인 및 깊이](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [시스템의 과학: Fluent 디자인 및 빛](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
