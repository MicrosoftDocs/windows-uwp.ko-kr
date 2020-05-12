---
author: knicholasa
description: Z-깊이, 즉 상대 깊이와 그림자는 앱에 깊이를 통합하여 자연스럽고 효율적으로 사용자가 집중할 수 있게 하는 두 가지 방법입니다.
title: Windows 앱의 Z-깊이 및 그림자
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 2655abd69f0f02efada9de5bab22e463c86b5d7e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970188"
---
# <a name="z-depth-and-shadow"></a>Z-깊이 및 그림자

![대각선 방향으로 차례로 누적된 네 개의 회색 사각형을 표시하는 gif입니다. 그림자가 표시되고 사라지도록 gif에 애니메이션 효과가 적용되었습니다.](images/elevation-shadow/shadow.gif)

UI 요소의 시각적 계층 구조를 만들면 UI를 쉽게 검사하고 집중해야 하는 항목을 전달할 수 있습니다. 선택된 UI 요소를 앞으로 가져오는 작업인 승격은 소프트웨어에서 이러한 계층 구조를 구현하는 데 사용되는 경우가 많습니다. 이 문서에서는 Z-깊이와 그림자를 사용하여 Windows 앱에서 승격을 만드는 방법을 설명합니다.

Z-깊이는 z축에서 두 표면 사이의 거리를 나타내기 위해 3D 앱 작성자 간에 사용되는 용어입니다. 개체와 뷰어 사이의 거리를 설명합니다. x/y 좌표와 비슷한 개념으로 생각하면 되지만, 단 z 방향입니다.

## <a name="why-use-z-depth"></a>Z-깊이를 사용해야 하는 이유

실제 세계에서는 더 가까운 개체에 집중하는 경향이 있습니다. 이 공간 본능을 디지털 UI에도 적용할 수 있습니다. 예를 들어 요소를 사용자에게 더 가깝게 가져오면 사용자가 본능적으로 요소에 집중하게 됩니다. z축에서 UI 요소를 더 가깝게 이동하면 개체 간에 시각적 계층 구조를 설정하여 사용자가 앱에서 자연스럽고 효율적으로 작업을 완료하게 할 수 있습니다.

## <a name="what-is-shadow"></a>그림자란?

그림자는 사용자가 승격을 인식하는 한 가지 방법입니다. 승격된 개체 위의 빛은 아래 화면에 그림자를 만듭니다. 개체가 높을수록 그림자가 더 크고 부드러워집니다. UI의 승격된 개체에 그림자가 반드시 필요한 것은 아니지만, 그림자는 승격 모양을 만드는 데 도움이 됩니다.

Windows 앱에서는 미적 측면이 아닌 목적 중심으로 그림자를 사용해야 합니다. 그림자를 너무 많이 사용하면 그림자를 통해 사용자가 집중하게 하는 효과가 감소하거나 사라집니다.

표준 컨트롤을 사용하는 경우 ThemeShadow 그림자가 자동으로 UI에 통합됩니다. 그러나 ThemeShadow 또는 DropShadow API를 사용하여 UI에 그림자를 수동으로 포함할 수도 있습니다. 

## <a name="themeshadow"></a>ThemeShadow

[ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) 유형은 모든 XAML 요소에 적용할 수 있으며 x, y, z 좌표에 따라 적절하게 그림자를 그립니다. 또한 ThemeShadow는 다음과 같이 다른 환경 사양에 맞게 자동으로 조정됩니다.

- 조명, 사용자 테마, 앱 환경 및 셸의 변화에 맞게 조정됩니다.
- Z-깊이에 따라 자동으로 요소에 그림자를 적용합니다. 
- 요소가 이동하고 승격을 변경함에 따라 요소를 동기화된 상태로 유지합니다.
- 애플리케이션 전체와 애플리케이션 간에 그림자를 일관성 있게 유지합니다.

MenuFlyout에서 ThemeShadow를 구현하는 방법은 다음과 같습니다. MenuFlyout에는 기본 화면이 32px로 승격되고, 추가 계단식 메뉴가 해당 메뉴를 여는 메뉴보다 각각 +8px 위에 열리는 기본 제공 환경이 있습니다.

![열린 메뉴 3개가 중첩되어 있는 MenuFlyout에 적용된 ThemeShadow의 스크린샷 첫 번째 메뉴는 32px로 승격되었으며, 이전 메뉴에서 열리는 각 후속 메뉴는 8px 더 승격되어 배경에 개별 그림자를 남깁니다.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>공용 컨트롤의 ThemeShadow

별도로 지정하지 않는 한, 다음 공용 컨트롤은 자동으로 ThemeShadow를 사용하여 32px 깊이에서 그림자를 적용합니다.

- [상황에 맞는 메뉴](../controls-and-patterns/menus.md), [명령 모음](../controls-and-patterns/app-bars.md), [명령 모음 플라이아웃](../controls-and-patterns/command-bar-flyout.md), [메뉴 모음](../controls-and-patterns/menus.md#create-a-menu-bar)
- [대화 상자 및 플라이아웃](../controls-and-patterns/dialogs.md)(64px 대화 상자)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [달력/날짜/시간 선택](../controls-and-patterns/date-and-time.md)
- [도구 설명](../controls-and-patterns/tooltips.md)(16px)
- [미디어 전송 컨트롤](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [연결된 애니메이션](../motion/connected-animation.md)

참고: 플라이아웃은 Windows 10 버전 1903 또는 최신 SDK에 대해 컴파일된 경우에만 ThemeShadow를 적용합니다.

### <a name="themeshadow-in-popups"></a>팝업의 ThemeShadow

사용자의 주의와 빠른 작업이 필요한 시나리오에서 앱 UI가 팝업을 사용하는 경우가 많습니다. 해당 시나리오는 앱 UI에서 계층 구조를 만드는 데 그림자를 사용해야 하는 경우의 좋은 예입니다.

[Popup](/uwp/api/windows.ui.xaml.controls.primitives.popup)의 XAML 요소에 ThemeShadow를 적용하면 자동으로 그림자가 생깁니다. 뒤에 있는 앱 배경 콘텐츠와 그 아래에서 열린 다른 모든 팝업에 그림자를 적용합니다.

팝업과 함께 ThemeShadow를 사용하려면 `Shadow` 속성을 사용하여 XAML 요소에 ThemeShadow를 적용합니다. 그런 다음, `Translation` 속성의 z 구성 요소를 사용하는 등의 방법으로 해당 요소를 뒤에 있는 다른 요소보다 승격합니다.
대부분의 팝업 UI에는 앱 배경 콘텐츠를 기준으로 기본 승격인 32 유효 픽셀을 사용하는 것이 좋습니다.

이 예제에서는 앱 배경 콘텐츠와 그 뒤의 다른 모든 팝업에 그림자를 적용하는 팝업의 사각형을 보여 줍니다.

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![그림자가 있는 단일 사각형 팝업](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>사용자 지정 플라이아웃 컨트롤에서 기본 ThemeShadow 사용 안 함

[Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) 또는 [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) 기반의 컨트롤은 자동으로 ThemeShadow를 사용하여 그림자를 적용합니다.

컨트롤 콘텐츠에 기본 그림자가 올바르게 표시되지 않는 경우, 연결된 FlyoutPresenter의 [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) 속성을 `false`로 설정하여 그림자를 사용하지 않도록 설정할 수 있습니다.

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>다른 요소의 ThemeShadow

일반적으로 그림자를 사용할 때는 주의해야 하며, 의미 있는 시각적 계층 구조를 도입하는 경우에만 제한적으로 사용하는 것이 좋습니다. 그러나 그림자가 필요한 고급 시나리오가 있는 경우를 위해 모든 UI 요소에서 그림자를 적용할 수 있는 방법을 제공합니다.

팝업에 없는 XAML 요소에서 그림자를 적용하려면 `ThemeShadow.Receivers` 컬렉션의 그림자를 받을 수 있는 다른 요소를 명시적으로 지정해야 합니다. 그림자를 받는 항목은 시각적 트리에서 그림자를 적용하는 항목의 상위 항목이 될 수 없습니다.

이 예제에서는 그 뒤에 있는 그리드에 그림자를 적용하는 두 개의 사각형을 보여 줍니다.

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![나란히 배치되고 둘 다 그림자가 있는 두 개의 옥색 사각형](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow의 성능 모범 사례

1. 시스템은 그림자를 적용하는 항목-그림자를 받는 항목 쌍을 5개로 제한하며, 한도를 초과할 경우 그림자를 해제합니다. 그림자를 적용하는 항목-그림자를 받는 항목 쌍을 5개로 제한하는 시스템 적용 한도를 준수합니다.

2. 사용자 지정 그림자를 받는 항목 요소 수를 필요한 최솟값으로 제한합니다.

3. 그림자를 받는 항목 요소 중 승격이 같은 요소가 여러 개 있을 경우, 대신 단일 부모 요소를 대상으로 지정하여 결합합니다.

4. 여러 요소가 동일한 그림자를 받는 항목 요소에 같은 유형의 그림자를 적용하는 경우, 그림자를 공유 리소스로 추가하고 재사용합니다.

## <a name="drop-shadow"></a>그림자

DropShadow는 환경에 자동으로 반응하지 않으며 광원을 사용하지 않습니다. 예제 구현은 [DropShadow 클래스](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)를 참조하세요.

## <a name="which-shadow-should-i-use"></a>사용해야 하는 그림자

| 속성 | ThemeShadow | DropShadow |
| - | - | - |
| **최소 SDK** | Windows 10 버전 1903 | 14393 |
| **적응성** | 예 | 아니요 |
| **사용자 지정** | 아니요 | 예 |
| **광원** | 자동(기본적으로 전역 속성이지만 앱별로 재정의할 수 있음) | 없음 |
| **지원되는 3D 환경** | 예 | 아니요 |

- 그림자의 목적은 간단한 시각적 처리뿐만 아니라 의미 있는 계층 구조를 제공하는 것입니다.
- 일반적으로 환경에 맞게 자동으로 조정되는 ThemeShadow를 사용하는 것이 좋습니다.
- 성능이 중요한 경우 그림자 수를 제한하거나, 다른 시각적 처리를 사용하거나, DropShadow를 사용합니다.
- 시각적 계층 구조를 구현하는 고급 시나리오가 있는 경우 다른 시각적 처리(예: 색)를 사용하는 것이 좋습니다. 그림자가 필요한 경우 DropShadow를 사용합니다.
