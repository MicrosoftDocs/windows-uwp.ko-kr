---
author: knicholasa
description: Z 수준, 상대 깊이 및 그림자는 응용 프로그램에 깊이를 통합 하 여 사용자가 자연스럽 게 효율적으로 집중할 수 있도록 하는 두 가지 방법입니다.
title: UWP 앱에 대 한 Z 깊이 및 그림자
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081394"
---
# <a name="z-depth-and-shadow"></a>Z-깊이 및 그림자

![대각선 방향으로 누적 되는 네 개의 회색 사각형을 보여 주는 gif로, 다른 사각형은 위에서 하나씩 표시 됩니다. 그림자가 표시 되 고 사라지게 되도록 gif에 애니메이션 효과가 적용 됩니다.](images/elevation-shadow/shadow.gif)

UI의 요소에 대 한 시각적 계층 구조를 만들면 UI를 쉽게 검색 하 고 집중할 수 있는 것을 알 수 있습니다. 권한 상승을 통해 UI의 선택 요소를 앞으로 가져오는 작업은 소프트웨어에서 이러한 계층 구조를 구현 하는 데 종종 사용 됩니다. 이 문서에서는 z 깊이 및 그림자를 사용 하 여 UWP 앱에서 권한 상승을 만드는 방법을 설명 합니다.

Z 깊이는 z 축을 따라 두 표면 사이의 거리를 나타내는 3D 앱 작성자 간에 사용 되는 용어입니다. 개체를 뷰어에 닫는 방법을 보여 줍니다. X/y 좌표에 대 한 비슷한 개념으로 생각 하면 z 방향입니다.

## <a name="why-use-z-depth"></a>Z 깊이를 사용 하는 이유

실제 세계에서는 우리에 게 더 가까운 개체에 집중 하는 경향이 있습니다. 이 공간 이러한을 디지털 UI에도 적용할 수 있습니다. 예를 들어 사용자에 게 더 가깝게 요소를 가져오면 사용자가 요소에 포커스를 옳 됩니다. Z 축에서 UI 요소를 더 가깝게 이동 하면 개체 간에 시각적 계층을 설정 하 여 사용자가 앱에서 자연스럽 고 효율적으로 작업을 완료할 수 있도록 할 수 있습니다.

## <a name="what-is-shadow"></a>그림자 란 무엇 인가요?

섀도은 사용자가 권한 상승을 인식 하는 한 가지 방법입니다. 상승 된 개체 위에 있는 빛은 아래 화면에 그림자를 만듭니다. 개체가 높을수록 그림자가 더 크고 부드러워집니다. UI의 상승 된 개체는 그림자를 가질 필요가 없지만 권한 상승의 형태를 만드는 데 도움이 됩니다.

UWP 앱에서는 미적가 아닌 목적이에서 그림자를 사용 해야 합니다. 너무 많은 그림자를 사용 하면 그림자가 사용자에 게 집중할 수 있는 기능이 감소 하거나 제거 됩니다.

표준 컨트롤을 사용 하는 경우 ThemeShadow shadows가 자동으로 UI에 통합 됩니다. 그러나 ThemeShadow 또는 DropShadow Api를 사용 하 여 UI에 그림자를 수동으로 포함할 수 있습니다. 

## <a name="themeshadow"></a>ThemeShadow

[ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) 형식은 x, y, z 좌표를 기준으로 적절 하 게 그림자를 그리기 위해 모든 XAML 요소에 적용 될 수 있습니다. 또한 ThemeShadow는 다른 환경 사양에 대해 자동으로 조정 합니다.

- 조명, 사용자 테마, 앱 환경 및 셸에서 변화에 맞게 조정 됩니다.
- Z 수준에 따라 자동으로 요소에 그림자를 적용 합니다. 
- 는 요소를 이동 하 고 변경 하는 동안 동기화 된 상태로 유지 합니다.
- 응용 프로그램 전체에서 그림자를 일관 되 게 유지 합니다.

ThemeShadow가 MenuFlyout 아웃에 구현 되는 방법은 다음과 같습니다. MenuFlyout 아웃은 기본 화면이 32px로 상승 하 고 각각의 추가 계단식 메뉴가 열리는 메뉴에서 열리는 메뉴 보다 위쪽에 있는 기본 제공 환경을 제공 합니다.

![열려 있는 세 개의 중첩 된 메뉴가 있는 MenuFlyout 아웃에 적용 되는 ThemeShadow의 스크린샷 첫 번째 메뉴는 상승 된 32px 이며 이전 메뉴에서 열리는 각 후속 메뉴는 배경에 별도의 그림자를 유지 하기 위해 상승 된 8px입니다.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>공용 컨트롤의 ThemeShadow

다음 공용 컨트롤은 달리 지정 하지 않는 한 자동으로 ThemeShadow를 사용 하 여 32px 깊이에서 그림자를 캐스팅 합니다.

- [상황에 맞는 메뉴](../controls-and-patterns/menus.md), [명령 모음](../controls-and-patterns/app-bars.md), [명령 모음 플라이 아웃](../controls-and-patterns/command-bar-flyout.md) [, 메뉴 모음](../controls-and-patterns/menus.md#create-a-menu-bar)
- 대화 [상자 및 flyouts](../controls-and-patterns/dialogs.md) (64px의 대화 상자)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [달력/날짜/시간 선택기](../controls-and-patterns/date-and-time.md)
- [도구 설명](../controls-and-patterns/tooltips.md) (16px)
- [미디어 전송 제어](../controls-and-patterns/media-playback.md#media-transport-controls), [inktoolbar](../controls-and-patterns/inking-controls.md)
- [연결된 애니메이션](../motion/connected-animation.md)

참고: Flyouts는 Windows 10 버전 1903 또는 최신 SDK에 대해 컴파일된 경우에만 ThemeShadow를 적용 합니다.

### <a name="themeshadow-in-popups"></a>팝업의 ThemeShadow

사용자의 주의가 필요한 시나리오에서 앱 UI가 팝업을 사용 하는 경우가 종종 있습니다. 응용 프로그램의 UI에서 계층 구조를 만드는 데 도움이 되는 섀도를 사용 해야 하는 경우에는 이러한 예가 좋습니다.

ThemeShadow는 [팝업](/uwp/api/windows.ui.xaml.controls.primitives.popup)의 모든 XAML 요소에 적용 될 때 그림자를 자동으로 캐스팅 합니다. 앱 배경 콘텐츠와 그 아래의 다른 열려 있는 팝업에 대 한 그림자를 캐스팅 합니다.

팝업에서 ThemeShadow를 사용 하려면 `Shadow` 속성을 사용 하 여 ThemeShadow를 XAML 요소에 적용 합니다. 그런 다음 `Translation` 속성의 z 구성 요소를 사용 하는 등의 다른 요소에 있는 요소를 승격 합니다.
대부분의 팝업 UI의 경우 앱 배경 콘텐츠에 상대적인 권장 기본 권한 상승이 32 유효 픽셀입니다.

이 예제에서는 그림자를 앱 배경 콘텐츠와 그 뒤의 다른 팝업에 캐스팅 하는 팝업의 사각형을 보여 줍니다.

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

![그림자가 있는 단일 사각형 popup입니다.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>사용자 지정 플라이 아웃 컨트롤에서 기본 ThemeShadow 사용 안 함

[플라이](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)아웃, [datepickerflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)아웃, [Menuflyout 아웃](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) 또는 [timepickerflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) 아웃 기반 컨트롤은 자동으로 ThemeShadow를 사용 하 여 그림자를 캐스팅 합니다.

컨트롤 콘텐츠에서 기본 그림자가 올바르지 않은 경우 [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) 속성을 연결 된 FlyoutPresenter에서 `false`로 설정 하 여 해제할 수 있습니다.

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

일반적으로 섀도 사용에 대해 신중 하 게 생각 하 고 의미 있는 시각적 계층을 도입 하는 경우 사용을 제한 하는 것이 좋습니다. 그러나 필요한 고급 시나리오가 있는 경우 UI 요소에서 그림자를 캐스팅 하는 방법을 제공 합니다.

Popup이 아닌 XAML 요소에서 그림자를 캐스팅 하려면 `ThemeShadow.Receivers` 컬렉션에서 그림자를 받을 수 있는 다른 요소를 명시적으로 지정 해야 합니다. 수신기는 시각적 트리에서 주술의 상위 항목이 될 수 없습니다.

이 예제에서는 그림자를 모눈으로 캐스팅 하는 두 개의 사각형을 보여 줍니다.

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

![각각 그림자를 사용 하 여 두 개의 옥색 사각형 옆에 있습니다.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow에 대 한 성능 모범 사례

1. 시스템은 수신자 쌍을 5 개로 제한 하며,이를 초과 하면 섀도를 해제 합니다. 시스템에서 받는 수신자 쌍의 한도를 적용 합니다.

2. 사용자 지정 받는 사람 요소의 수를 필요한 최소값으로 제한 합니다.

3. 여러 받는 사람 요소가 동일한 권한 상승을 수행 하는 경우에는 대신 단일 부모 요소를 대상으로 하 여 결합 합니다.

4. 여러 요소가 같은 형식의 섀도를 동일한 받는 사람 요소로 캐스팅 하는 경우 섀도를 공유 리소스로 추가 하 고 다시 사용 합니다.

## <a name="drop-shadow"></a>그림자

DropShadow는 환경에 자동으로 반응 하지 않으며 광원을 사용 하지 않습니다. 예제 구현은 [Dropshadow 클래스](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)를 참조 하세요.

## <a name="which-shadow-should-i-use"></a>어떤 그림자를 사용 해야 하나요?

| 속성 | ThemeShadow | DropShadow |
| - | - | - |
| **최소 SDK** | Windows 10 버전 1903 | 14393 |
| **적응성** | 예 | 아니요 |
| **스키마별** | 아니요 | 예 |
| **광원** | 자동 (기본적으로 전역 이지만 앱 단위로 재정의할 수 있음) | 없음 |
| **3D 환경에서 지원** | 예 | 아니요 |

- 그림자의 목적은 간단한 시각적 처리 뿐만 아니라 의미 있는 계층을 제공 하는 것입니다.
- 일반적으로 ThemeShadow를 사용 하는 것이 좋습니다 .이는 환경에 맞게 자동으로 조정 됩니다.
- 성능에 대 한 문제를 해결 하려면 그림자 수를 제한 하거나 다른 시각적 처리를 사용 하거나 DropShadow를 사용 합니다.
- 시각적 계층 구조를 달성할 수 있는 고급 시나리오가 있는 경우 다른 시각적 처리를 사용 하는 것이 좋습니다 (예: 색). 섀도가 필요한 경우 DropShadow를 사용 합니다.
