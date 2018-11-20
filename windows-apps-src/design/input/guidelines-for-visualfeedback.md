---
author: Karl-Bridge-Microsoft
Description: Use visual feedback to show users when their interactions with a UWP app are detected, interpreted, and handled.
title: 시각적 피드백
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: 시각적 피드백, 포커스 피드백, 터치 피드백, 접촉 시각화, 입력, 조작
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3440a0c64c0546b12c7f2c1bf1ea30168886d1db
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7306952"
---
# <a name="guidelines-for-visual-feedback"></a>시각적 피드백에 대한 지침

시각적 피드백을 사용하여 조작이 감지, 해석 및 처리될 때 사용자에게 표시할 수 있습니다. 시각적 피드백은 조작 의지를 북돋아 사용자에게 도움이 될 수 있습니다. 시각적 피드백은 조작이 성공했음을 표시하여 사용자의 제어 감각을 향상합니다. 또한 시스템 상태를 전달하고 오류를 줄여 줍니다.

> **중요 API**: [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)

## <a name="recommendations"></a>권장 사항

- 광범위한 수정이 컨트롤과 응용 프로그램의 성능 및 접근성에 영향을 미칠 수 있으므로 컨트롤 템플릿의 수정을 디자인 의도와 직접 관련된 것으로만 제한하도록 합니다. 
    - 시각적 상태 속성을 비롯한 컨트롤 속성 사용자 지정에 대한 추가 정보는 [XAML 스타일](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles)을 참조하세요.
    - 컨트롤 템플릿 변경한 대한 자세한 내용은 [UserControl 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)를 참조하세요.
    - 템플릿 컨트롤에 중요한 변경을 하려는 경우 사용자 지정 템플릿의 컨트롤을 만드는 것이 좋습니다. 사용자 지정 템플릿의 컨트롤의 예는 [사용자 지정 편집 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)을 참조합니다.
- 앱 사용에 방해가 될 수 있는 경우에는 터치 시각화를 사용하지 마세요. 자세한 내용은 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969)를 참조하세요.
- 반드시 필요한 경우가 아니면 피드백을 표시하지 마세요. 다른 곳에서 제공되지 않는 가치를 추가하는 경우가 아니면 시각적 피드백을 표시하지 않고 UI를 깔끔하고 간결하게 유지합니다.
- 기본 제공 Windows 제스처의 시각적 피드백 동작을 과도하게 사용자 지정하지 마세요. 사용자 지정하면 일관되지 않고 혼란을 주는 사용자 환경이 생성될 수 있습니다.

## <a name="additional-usage-guidance"></a>추가 사용법 지침

접촉 시각화는 정확도 및 정밀도를 요구하는 터치 조작에 특히 중요합니다. 예를 들어 앱은 사용자가 대상을 빗나갔는지 여부, 빗나간 간격 및 필요한 조정을 알 수 있도록 탭 위치를 명확하게 표시해야 합니다.

제공된 기본 XAML 플랫폼 컨트롤을 사용하면 모든 디바이스와 모든 입력 상황에서 앱이 올바르게 작동합니다. 앱에서 사용자 지정 피드백이 필요한 사용자 지정 조작을 사용하는 경우 피드백이 적절하고 여러 입력 디바이스를 포괄하며 사용자 주의를 분산시키지 않는지 확인해야 합니다. 이 사항은 시각적 피드백이 중요한 UI와 충돌하거나 가릴 수 있는 게임 또는 그리기 앱에서 특히 문제가 됩니다.

> [!Important]
> 기본 제공 제스처의 조작 동작은 변경하지 않는 것이 좋습니다.

**디바이스 간 피드백**

시각적 피드백은 일반적으로 입력 디바이스(터치, 터치 패드, 마우스, 펜/스타일러스, 키보드 등)에 따라 달라집니다. 예를 들어 마우스에 대한 기본 제공 피드백은 대체로 커서 이동 및 변경인 반면 터치와 펜에는 접촉 시각화가 필요하고 키보드 입력 및 탐색은 포커스 사각형과 강조 표시를 사용합니다.

[**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969)을 사용하여 플랫폼 제스처에 대한 피드백 동작을 설정합니다.

피드백 UI를 사용자 지정하는 경우 모든 입력 모드를 지원하고 모든 입력 모드에 적합한 피드백을 제공해야 합니다.

다음은 Windows에 포함된 몇 가지 기본 제공 접촉 시각화의 예입니다.

| ![터치 피드백](images/TouchFeedback.png) | ![마우스 피드백](images/MouseFeedback.png) | ![펜 피드백](images/PenFeedback.png) | ![키보드 피드백](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| 터치 시각화 | 마우스/터치 패드 시각화 | 펜 시각화 | 키보드 시각화 |

## <a name="high-visibility-focus-visuals"></a>높은 가시성 포커스 화면 효과

모든 Windows 앱은 응용 프로그램 내의 조작 가능한 컨트롤 주위에 보다 정의된 포커스 화면 효과를 표시합니다. 이러한 새 포커스 화면 효과는 완전히 사용자 지정할 수 있으며 필요에 따라 사용하지 않도록 설정할 수도 있습니다.

Xbox 및 TV 사용에 일반적인 **3m 환경**의 경우 Windows는 단추와 같이 게임 패드 또는 키보드 입력을 통해 포커스할 수 있는 요소의 경계를 애니메이션화하는 조명 효과인 **포커스 표시**를 지원합니다. 자세한 내용은 [Xbox 및 TV용 디자인](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus)을 참조하세요.

## <a name="color-branding--customizing"></a>색 브랜딩 및 사용자 지정

**테두리 속성**

높은 가시성 포커스 화면 효과는 기본 테두리와 보조 테두리의 두 부분으로 이루어져 있습니다. 기본 테두리는 **2px** 두께이고 보조 테두리 *외부*에서 실행됩니다. 보조 테두리는 **1px** 두께이고 기본 테두리 *내부*에서 실행됩니다.
![높은 가시성 포커스 화면 효과 redline](images/FocusRectRedlines.png)

테두리 유형(기본 또는 보조)의 두께를 변경하려면 각각 **FocusVisualPrimaryThickness** 또는 **FocusVisualSecondaryThickness**를 사용합니다.
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![높은 가시성 포커스 화면 효과 여백 두께](images/FocusMargin.png)

여백은 [**Thickness**](https://msdn.microsoft.com/library/system.windows.thickness) 형식의 속성이므로 컨트롤의 특정 측면에만 표시되도록 사용자 지정할 수 있습니다. 아래를 참조하세요. ![맨 아래에만 높은 가시성 포커스 화면 효과 여백 두께 적용](images/FocusThicknessSide.png)

여백은 컨트롤의 시각적 범위와 포커스 화면 효과 *보조 테두리* 시작 부분 사이의 간격입니다. 기본 여백은 컨트롤 범위에서 **1px**입니다. **FocusVisualMargin** 속성을 변경하여 컨트롤별로 이 여백을 편집할 수 있습니다.
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![높은 가시성 포커스 화면 효과 여백 차이](images/FocusPlusMinusMargin.png)

*음수 여백은 컨트롤의 중심에서 테두리를 멀리 이동하고 양수 여백은 컨트롤의 중심으로 테두리를 더 가깝게 이동합니다.*

컨트롤의 포커스 화면 효과를 완전히 끄려면 **UseSystemFocusVisuals**를 사용하지 않도록 설정하면 됩니다.
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

두께, 여백 또는 앱 개발자가 포커스 화면 효과를 사용할지 여부는 컨트롤별로 결정됩니다.

**색 속성**

포커스 화면 효과에는 기본 테두리 색과 보조 테두리 색의 두 가지 색 속성만 있습니다. 이러한 포커스 화면 효과 테두리 색은 페이지 수준에서 컨트롤별로 변경하거나 앱 수준에서 전체적으로 변경할 수 있습니다.

앱 수준에서 포커스 화면 효과를 브랜딩하려면 시스템 브러시를 재정의합니다.
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![높은 가시성 포커스 화면 효과 색 변경](images/FocusRectColorChanges.png)

컨트롤별로 색을 변경하려면 원하는 컨트롤의 포커스 화면 효과 속성만 편집합니다.
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>관련 문서

**디자이너용**
* [이동에 대한 지침](guidelines-for-panning.md)

**개발자용**
* [사용자 지정 사용자 조작](https://msdn.microsoft.com/library/windows/apps/mt185599)

**샘플**
* [기본 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [사용자 조작 모드 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [포커스 화면 효과 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**보관 샘플**
* [입력: XAML 사용자 입력 이벤트 샘플](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [입력: 디바이스 기능 샘플](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: 터치 적중 횟수 테스트 샘플](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 스크롤, 이동 및 확대/축소 샘플](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [입력: 간단한 잉크 샘플](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [입력: Windows 8 제스처 샘플](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [입력: 조작 및 제스처(C++) 샘플](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 터치 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 
