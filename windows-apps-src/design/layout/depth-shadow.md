---
author: knicholasa
description: Z-깊이 또는 상대 깊이 섀도 깊이 자연스럽 고 효율적으로 사용자가 집중 하는 데 앱에 통합 하는 두 가지 방법입니다.
title: Z 깊이 및 UWP 앱에 대 한 섀도
template: detail.hbs
ms.author: nichola
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: ab49f13d3938e55750ce523f9e0d4ae241304763
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817732"
---
# <a name="z-depth-and-shadow"></a>Z-깊이 및 그림자

![네 개의 회색 사각형을 보여 주는 gif 하나 하나 쌓아 대각선으로 누적 합니다. Gif는 그림자를 표시 하거나 숨길 수 있도록 애니메이션 효과가 적용 됩니다.](images/elevation-shadow/shadow.gif)

UI에서 요소의 시각적 계층을 만드는 UI를 보다 쉽게 검색 및 중요 한 것에 집중을 전달 합니다. 권한 상승, act UI 앞의 select 요소를 전환 하는 소프트웨어에서 이러한 계층을 달성 하기 위해 주로 사용 됩니다. 이 문서는 z 깊이 그림자를 사용 하 여 UWP 앱에서 권한 상승을 만드는 방법을 설명 합니다.

Z-깊이 3D 앱 작성자 간에 z 축 따라 두 화면 간의 거리를 나타내는 데 사용 되는 용어입니다. 뷰어에 얼마나 근접 개체를 보여 줍니다. X에 이와 비슷한 개념으로 생각해 / y 좌표 z 방향입니다.

## <a name="why-use-z-depth"></a>Z 깊이 사용 하는 이유는?

실제 세계에서 우리에 게 보다 가까이 있는 개체에 집중 경향이 있습니다. 이러한 공간 속성 디지털 ui도 적용할 수 있습니다. 예를 들어, 사용자에 게 더 가깝게 요소를 가져오는 경우 사용자는 요소에 포커스 옳 됩니다. 이동 UI 요소에서 가깝게 z 축에서 지원 앱에서 자연스럽 고 효율적으로 작업을 완료 하는 사용자 개체 간의 시각적 계층을 설정할 수 있습니다.

## <a name="what-is-shadow"></a>섀도 란?

그림자는 사용자에 게 권한 상승 하는 방법 중 하나입니다. 관리자 권한 개체 위에서 light 아래 화면에 그림자를 만듭니다. 높을수록 개체, 큰 및 부드러운 그림자가 됩니다. UI에서 관리자 권한 개체 그림자가 필요는 없지만 권한 상승의 모양을 만들 하는 데 도움이 됩니다.

UWP 앱에서 그림자 고의적 보다는 시각적인 방식으로 사용할 해야 합니다. 너무 많은 그림자를 사용 하 여을 감소 하거나 사용자 집중 하는 그림자의 기능을 제거 합니다.

표준 컨트롤을 사용 하는 경우 UI에 ThemeShadow 그림자를 자동으로 통합 됩니다. 그러나 포함할 수 있습니다 수동으로 그림자 UI에는 ThemeShadow 또는 DropShadow Api를 사용 하 여. 

## <a name="themeshadow"></a>ThemeShadow

그릴 XAML 요소에 적용할 수 형식 ThemeShadow 적절 하 게 따라 x, y, z 좌표를 섀도 처리 합니다. ThemeShadow 다른 환경 사양에 대 한 자동으로 조정합니다.

- 조명, 사용자 테마, 앱 환경 및 shell의 변경 내용에 맞게 변경합니다.
- 그림자가 z-깊이에 따라 자동으로 요소에 적용 됩니다. 
- 이동 하 고 권한 상승 변경 요소 동기화 유지 합니다.
- 그림자 전체 및 응용 프로그램에서 일관성을 유지합니다.

MenuFlyout에 ThemeShadow를 구현한 방법을 다음과 같습니다. MenuFlyout 기본 제공 된 기본 화면 32px로 승격 됩니다 및 각 추가 계단식 메뉴를 열 수 있는 환경 + 8px 위의 메뉴에서 열립니다.

![세 개 열려 있고 중첩 메뉴를 사용 하 여 MenuFlyout 적용할 ThemeShadow의 스크린샷. 첫 번째 메뉴 높은 32px 이며 이전 메뉴에서 열리는 각 후속 메뉴는 높은 8px 자세한 배경을 고유 그림자를 그대로입니다.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow 공통 제어합니다.

다음과 같은 일반적인 컨트롤 ThemeShadow 달리 지정 하지 않으면 32px 깊이에서 그림자를 사용 하 여 자동으로 됩니다.

- [상황에 맞는 메뉴](../controls-and-patterns/menus.md), [명령 모음](../controls-and-patterns/app-bars.md)합니다 [명령 모음 플라이 아웃](../controls-and-patterns/command-bar-flyout.md), [메뉴 모음](../controls-and-patterns/menus.md#create-a-menu-bar)
- [대화 상자 및 플라이 아웃](../controls-and-patterns/dialogs.md) (64px에서 대화 상자)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [달력/날짜/시간 선택기](../controls-and-patterns/date-and-time.md)
- [도구 설명](../controls-and-patterns/tooltips.md) (16px)
- [미디어 전송 컨트롤](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [연결 된 애니메이션](../motion/connected-animation.md)

참고: 플라이 아웃에 ThemeShadow Windows 10 버전이 1903 또는 최신 SDK에 대해 컴파일할 때만 적용 됩니다.

### <a name="themeshadow-in-popups"></a>팝업에서 ThemeShadow

앱의 UI는 팝업 시나리오에 대 한 사용자의 주 및 빠른 작업 필요한 경우 경우가 있습니다. 이들은 섀도 앱의 ui 계층 구조를 만드는 데 사용 해야 하는 경우 좋은 예입니다.

ThemeShadow 그림자에 있는 모든 XAML 요소에 적용 될 때를 자동으로 캐스팅 된 [팝업](/uwp/api/windows.ui.xaml.controls.primitives.popup)합니다. 이 앱 백그라운드 콘텐츠 및 그 아래의 다른 열린 팝업 뒤에 그림자 캐스트 합니다.

ThemeShadow 팝업을 사용 하려면 사용는 `Shadow` 는 ThemeShadow XAML 요소에 적용할 속성입니다. 그런 다음 elevate 요소 뒤에 다른 요소에서 예를 들어의 z 구성 요소를 사용 하 여는 `Translation` 속성입니다.
대부분의 팝업 UI에 대 한 앱 백그라운드 콘텐츠를 기준으로 권장 되는 기본 권한 상승은 32 효과적인 픽셀입니다.

이 예제에 표시 팝업에서 앱 백그라운드 콘텐츠 및 모든 다른 팝업 그 뒤에 그림자가 생기 사각형:

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

![그림자가 적용 된 단일 사각형 팝업 합니다.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>사용자 지정 플라이 아웃에서 ThemeShadow 제어 기본값을 사용 하지 않도록 설정

에 따라 컨트롤 [플라이 아웃](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) 하거나 [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) 를 자동으로 사용 하 여 ThemeShadow를 그림자입니다.

설정 하 여 비활성화할 수 있습니다 다음 컨트롤의 콘텐츠 기본 그림자에 올바르게 표시 되지 않습니다 경우는 [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) 속성을 `false` 연결된 FlyoutPresenter에서:

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>다른 요소에서 ThemeShadow

일반적 좋습니다 그림자를 사용 하는 방법에 대 한 신중 하 게 생각 하 고 의미 있는 시각적 개체 계층 구조를 도입 하는 경우에 해당 사용을 제한할 수 있습니다. 그러나 것을 볼 수 있는 시나리오 발전 하는 경우 모든 UI 요소에서 섀도 캐스팅 하는 방법을 제공지 않습니다.

팝업에 없는 XAML 요소에서 그림자를 명시적으로 지정 해야 섀도를 받을 수 있는 다른 요소는 `ThemeShadow.Receivers` 컬렉션입니다. 수신기는 시각적 트리에서 자의 상위 항목 일 수 없습니다.

이 예제는 표 뒤에 그림자를 캐스팅 하는 두 사각형을 보여줍니다.

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

![서로 그림자를 사용 하 여 모두 옆에 있는 두 개의 옥색 사각형입니다.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow에 대 한 성능 모범 사례

1. 시스템은 최대 5 자를-수신자 쌍을 설정 하 고이 값을 초과 하는 경우 섀도 해제 됩니다. 5 자를-수신자 쌍 시스템 적용 제한에 집중 합니다.

2. 필요한 최소 사용자 지정 수신기 요소 수를 제한 합니다.

3. 여러 받는 사람 요소가 동일한 권한 상승의 경우 대신 단일 부모 요소를 대상으로 하 여 결합 하려고 합니다.

4. 여러 요소는 동일한 유형의 동일한 받는 사람 요소에 섀도 캐스팅 하는 경우 다음 공유 리소스로 그림자를 추가 하 고 다시 사용 합니다.

## <a name="drop-shadow"></a>그림자

DropShadow 해당 환경에 자동으로 응답 하지 않는 및 광원을 사용 하지 않습니다. 예를 들어 구현을 참조 합니다 [DropShadow 클래스](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)합니다.

## <a name="which-shadow-should-i-use"></a>그림자를 사용 해야 합니까?

| 속성 | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | Windows 10 버전이 1903 | 14393 |
| **적응성** | 예 | 아니오 |
| **사용자 지정** | 아니오 | 예 |
| **광원** | 자동 (기본적으로 전역 앱 별로 재정의할 수 있지만) | 없음 |
| **지원 되는 3D 환경** | 예 | 아니오 |

- 그림자의 용도 간단한 시각적 처리 아니라 의미 있는 계층을 제공 하는 점을 염두에 두십시오.
- 일반적으로 해당 환경에 자동으로 조정 되는 ThemeShadow를 사용 하는 것이 좋습니다.
- 성능에 대 한 문제를 그림자의 수를 제한, 다른 시각적 처리를 사용 하거나 DropShadow를 사용 합니다.
- 시각적 계층을 달성 하는 시나리오를 더 발전 하는 경우에 다른 시각적 처리 (예: 색)를 사용 하 여 하는 것이 좋습니다. 섀도 필요한 경우에 DropShadow를 사용 합니다.
