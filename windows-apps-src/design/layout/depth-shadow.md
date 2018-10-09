---
author: serenaz
description: Z-깊이 또는 상대 깊이 및 그림자는 다음 두 가지가 자연스럽 고 효율적으로 사용자가 포커스를 위해 앱에 깊이 통합 합니다.
title: Z-깊이 및 UWP 앱에 대 한 그림자
template: detail.hbs
ms.author: sezhen
ms.date: 02/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: chigy
design-contact: balrayit
ms.localizationpriority: medium
ms.openlocfilehash: a1433b131b994ee2b1323909bc7c195e00f43cde
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4470016"
---
# <a name="z-depth-and-shadow"></a>Z-깊이 및 그림자

![true 깊이](images/elevation-shadow/depth.svg)

3D 위치, 조명 등의 물리적 개념을 사용 하 여 흐름 깊이 시스템 및 그림자 선사할 디지털 UI를 더 계층화 된 물리적 환경에서 인식 될 수 있습니다. Z-깊이 또는 상대 깊이 및 그림자는 UWP 앱에 깊이 통합 다음 두 가지가 있습니다.

## <a name="what-is-z-depth"></a>Z-깊이 란 무엇 인가요?

Z-깊이, z 축 따라 두 표면 사이의 거리 이며 뷰어에 얼마나 가까이 개체를 보여 줍니다.

![z-깊이](images/elevation-shadow/elevation.svg)

### <a name="why-use-z-depth"></a>Z-깊이 사용 하는 이유

실제 세계에서 우리에 게 더 가까이 있는 개체에 집중 경향이 있습니다. 이 공간 외 디지털 UI를 적용할 수도 있습니다. 예를 들어, 사용자에 게 더 가깝게 요소를 표시 하는 경우 사용자는 요소에 포커스 instinctively 됩니다. 이동 UI 요소를 더 가깝게 z 축에서 앱에서 자연스럽 게, 그리고 효율적으로 작업을 완료 하는 사용자가 개체 간의 시각적 계층 구조를 설정할 수 있습니다. 

![z-깊이 콘텐츠 메뉴](images/elevation-shadow/whyelevation.svg)

외에 의미 있는 시각적 계층 구조 z 깊이 해줍니다 제공를 만들 수 일관 된 환경을 2D에서 원활 하 게 모든 장치와 폼 팩터에 걸쳐 앱을 확장 하는 3D 환경에 합니다. 

![2d에서 3d의 z-depth](images/elevation-shadow/elevation-2d3d.svg)

### <a name="how-is-z-depth-perceived"></a>Z-깊이 최적이?

실제에서 깊이 떨어지는 하는 방법에 따라, 일부의 기술이 근접 디지털 UI에 표시를 사용할 수 있는 합니다.

- **배율** 멀리 개체 같은 크기의 가까운 개체 보다 더 작게 표시 합니다. 이 방법은 하므로 일반적으로 권장 되지 2D 공간에서 효과적으로 설명 하기 어렵습니다. 그러나 2d에서 사용자에 게 더 가깝게 이동 하는 개체의 유효 시뮬레이션을 만들려면 배율 및 [그림자](#what-is-shadow) 를 사용할 수 있습니다.

    ![배율로 근접 연결](images/elevation-shadow/elevation-scale.svg)

- **분위기** 개체는 멀리 및 "연기가 찬" 오버레이 또는 다른 대기 효과 사용 하 여 초점 밖 나타날 수 있습니다.

    ![분위기를 사용 하 여 근접 연결](images/elevation-shadow/elevation-atmosphere.svg)

- **동작** 근접 연결을 보여 주기 위해 상대 속도 사용할 수: 가까운 개체 배경 멀리 있는 개체 보다 빠르게 이동 합니다. 이 효과 구현 하는 방법을 알아보려면 [시차](../motion/parallax.md)를 참조 하세요.

    ![동작의 근접 연결](images/elevation-shadow/elevation-motion.svg)

### <a name="recommendations-for-z-depth"></a>Z-깊이 위한 권장 사항

명확한 시각적 포커스를 제공 하기 위해 높은 평면의 수를 줄입니다. 대부분의 시나리오에 대 한 두 개의 평면에 충분: 하나는 포그라운드 항목 (높은 근접) 용이고 다른 배경 항목 (낮은 근접). 여러 상승 된 항목을 겹치지 않는 경우 평면의 수를 줄이는 동일 평면 (즉, 전경) 그룹화 합니다.

![앱 내에서 z 깊이](images/elevation-shadow/app-depth.svg)

## <a name="what-is-shadow"></a>그림자 란 무엇 인가요?

![그림자](images/elevation-shadow/shadow.svg)

그림자는 권한 상승 생각 하는 방법입니다. 관리자 권한 개체 위에 빛이 있는 경우 아래 표면에 그림자가 않습니다. 클수록 개체, 더 큰 및 부드러운 그림자가 됩니다. 관리자 권한 개체, 그림자가 필요 없습니다 하지만 그림자 권한 상승 표시 작업을 수행 합니다.

UWP 앱에서 그림자 체계적으로 하지 미학적 있어야 합니다. 그림자 강제로 포커스 및 생산성을 하는 경우 다음 그림자의 사용을 제한 합니다.

ThemeShadow 또는 DropShadow Api를 사용 하 여 그림자를 사용할 수 있습니다.

## <a name="themeshadow"></a>ThemeShadow

형식 그리려는 모든 XAML 요소에 적용할 수 있습니다 ThemeShadow 적절 하 게에 따라 x, y, z 좌표를 숨깁니다. ThemeShadow 다른 환경 사양에 대 한 자동으로 조정합니다.

- 조명, 사용자 테마, 앱 환경 및 셸의 변경에 적응합니다.
- 자신의 권한 상승에 따라 자동으로 요소를 숨깁니다.
- 이동 하 고 권한 상승 변경 요소 동기화 유지 합니다.
- 그림자 전체 및 응용 프로그램 간에 일관성을 유지합니다.

밝고 어두운 테마를 사용 하 여 다른 권한 상승에 ThemeShadow의 예는 다음과 같습니다.

![밝은 테마를 사용 하 여 스마트 그림자](images/elevation-shadow/smartshadow-light.svg)

![어두운 테마를 사용 하 여 스마트 그림자](images/elevation-shadow/smartshadow-dark.svg)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow 공통 컨트롤

다음과 같은 공용 컨트롤 그림자를 캐스팅할 ThemeShadow를 자동으로 사용 됩니다.

- [대화 상자 및 플라이아웃](../controls-and-patterns/dialogs.md)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [미디어 전송 컨트롤](../controls-and-patterns/media-playback.md)
- [상황에 맞는 메뉴](../controls-and-patterns/menus.md)
- [명령 모음](../controls-and-patterns/app-bars.md)
- [자동 제안](../controls-and-patterns/auto-suggest-box.md), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) [일정/날짜/시간 선택기](../controls-and-patterns/date-and-time.md), [도구 설명](../controls-and-patterns/tooltips.md)
- [선택키](../input/access-keys.md)

### <a name="themeshadow-in-popups"></a>팝업에 ThemeShadow

ThemeShadow [팝업](/uwp/api/windows.ui.xaml.controls.primitives.popup)모든 XAML 요소에 적용 될 때 그림자를 자동으로 캐스팅 합니다. 앱 백그라운드 콘텐츠 및 그 아래 다른 열려 팝업 뒤에 그림자를 캐스팅할는 것입니다.

ThemeShadow 팝업을 사용 하려면 사용 합니다 `Shadow` 속성을 XAML 요소는 ThemeShadow 적용 합니다. 그런 다음 권한 상승 요소에서 다른 요소 뒤, 예를 들어의 z 구성 요소를 사용 하 여는 `Translation` 속성입니다.
대부분의 팝업 UI 앱 백그라운드 콘텐츠를 기준으로 권장 되는 기본 권한 상승 32 유효 픽셀입니다.

이 예제에서는 사각형이 표시 팝업에서 앱 백그라운드 콘텐츠 및 그 뒤 다른 팝업 그림자:

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="White" Height="48" Width="96">
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

![코드 예제에서 그림자](images/elevation-shadow/smartshadow-example.svg)

### <a name="themeshadow-in-other-elements"></a>다른 요소에서 ThemeShadow

팝업에 없는 XAML 요소에서 그림자를 명시적으로 지정 해야에서 그림자를 받을 수 있는 다른 요소는 `ThemeShadow.Receivers` 컬렉션입니다.

이 예에서는 그림자 그리드 뒤에 두 개의 단추를 보여줍니다.

```xaml
<Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Button x:Name="Button1" Content="Button 1" Shadow="{StaticResource SharedShadow}" Margin="10" />

    <Button x:Name="Button2" Content="Button 2" Shadow="{StaticResource SharedShadow}" Margin="120" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Button1.Translation += new Vector3(0, 0, 16);
Button2.Translation += new Vector3(0, 0, 32);
```

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow에 대 한 성능 모범 사례

1. 최소 필요한 사용자 지정 수신기 요소의 수를 제한 합니다. 

2. 동일한 권한 상승에 여러 수신기 요소는 경우 대신 단일 부모 요소를 대상으로 하 여 결합할 수 하십시오.

3. 여러 요소는 동일한 유형의 동일한 수신기 요소에는 그림자를 캐스팅 하는 경우 공유 리소스로 그림자를 추가 하 고 다시 사용 합니다.

## <a name="drop-shadow"></a>그림자

DropShadow 환경에 자동으로 응답 이며 광원을 사용 하지 않습니다. 예를 들어 구현 [DropShadow 클래스](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)를 참조 하세요.

## <a name="which-shadow-should-i-use"></a>어떤 그림자를 사용 해야 하나요?

| 속성 | ThemeShadow | DropShadow |
| - | - | - | - |
| **최소 SDK** | RS5 | 14393 |
| **적응성** | 예 | 아니요 |
| **사용자 지정** | 아니요 | 예 |
| **광원** | 자동 (기본적으로 전역 앱 당 재정의할 수 있지만) | 없음 |
| **3D 환경에서 지원** | 예 | 아니요 |

- 일반적으로 자동으로 해당 환경에 맞게 조정 되는 ThemeShadow를 사용 하는 것이 좋습니다.
- 사용자 지정 그림자에 대 한 시나리오를 보다 고급 큰 사용자 지정할 수 있는 DropShadow를 사용 합니다.
- 에 대 한 이전 버전과 호환성을 DropShadow 사용 합니다.
- 성능에 대 한 우려, 그림자의 수를 제한 하거나 DropShadow 사용 합니다.
- True 3d에서 HMDs, ThemeShadow를 사용 합니다. DropShadow 측에서, 부모가 시각적 개체에서 지정 된 오프셋 그립니다 이후 공간에서 부동 처럼 보일 합니다. 반면에 ThemeShadow 수신기로 정의 된 시각적 개체 위에 렌더링 됩니다.
