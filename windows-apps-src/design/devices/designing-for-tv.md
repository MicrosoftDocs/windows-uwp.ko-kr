---
Description: Tv에서 잘 작동 하 고 잘 작동 하도록 앱을 디자인 합니다.
title: Xbox 및 TV용 디자인
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, 10 피트 환경, 게임 패드, 원격 제어, 입력, 상호 작용
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 491b67322c8b328c21446d50951daad61f15ad3d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175577"
---
# <a name="designing-for-xbox-and-tv"></a>Xbox 및 TV용 디자인

Xbox One 및 텔레비전 화면에서 잘 작동 하도록 Windows 앱을 디자인 합니다.

*10 피트* 환경에서 UWP 응용 프로그램의 상호 작용 환경에 대 한 지침은 [게임 패드 및 원격 제어 상호 작용](../input/gamepad-and-remote-interactions.md) 을 참조 하세요.

## <a name="overview"></a>개요

유니버설 Windows 플랫폼를 사용 하 여 여러 Windows 10 장치에서 매력적인 환경을 만들 수 있습니다.
UWP 프레임 워크에서 제공 되는 대부분의 기능을 사용 하면 앱이 추가 작업 없이 이러한 장치에서 동일한 UI (사용자 인터페이스)를 사용할 수 있습니다.
그러나 Xbox One 및 TV 화면에서 잘 작동 하도록 앱을 조정 하 고 최적화 하려면 특별 한 고려 사항이 필요 합니다.

실내의 소파에 앉아 TV와 상호 작용 하는 게임 패드 또는 리모컨을 사용하여 TV를 조작하는 환경을 **10피트 환경**이라고 합니다.
일반적으로 사용자가 화면에서 약 3m 떨어진 곳에 앉아 있기 때문에 이렇게 이름이 지정되었습니다.
이 경우 가령 *0.6m* 환경이나 PC 조작 시에는 존재하지 않는 고유한 과제가 발생합니다.
Xbox One 또는 TV 화면에 출력 되는 다른 장치에 대 한 앱을 개발 하 고 있으며,이를 위해 컨트롤러를 사용 하는 경우에는 항상이 점을 명심 해야 합니다.

앱이 10 피트 환경에서 잘 작동 하도록 하는 데이 문서의 단계를 모두 수행 하는 것은 아니지만 앱을 이해 하 고 앱에 적절 한 결정을 내릴 때 앱의 특정 요구 사항에 맞게 조정 된 10 피트 환경을 구현 하는 것이 좋습니다.
10 피트 환경에서 앱을 개발 하는 경우 다음과 같은 디자인 원칙을 고려해 야 합니다.

### <a name="simple"></a>단순

10 피트 환경용 디자인은 고유한 문제 집합을 제공 합니다. 해상도 및 보기 거리를 통해 사용자가 너무 많은 정보를 처리 하기 어려울 수 있습니다.
디자인을 깔끔하게 유지 하 고 가능한 가장 간단한 구성 요소로 축소 해 보세요. TV에 표시 되는 정보의 양은 데스크톱이 아닌 휴대폰에서 볼 수 있는 것과 비교할 수 있어야 합니다.

![Xbox One 홈 화면](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>일관성이

10 피트 환경의 UWP 앱은 직관적이 고 사용 하기 쉽습니다. 포커스를 명확 하 게 하 고 unmistakable 합니다.
공간 전체의 이동이 일관적이 고 예측 가능 하도록 콘텐츠를 정렬 합니다. 원하는 작업에 가장 짧은 경로를 제공 합니다.

![Xbox One 영화 앱](images/designing-for-tv/xbox-movies-app.png)

_**스크린샷에 표시 된 모든 동영상은 Microsoft 영화 & TV에서 사용할 수 있습니다.**_  

### <a name="captivating"></a>Captivating

영화 같은 가장 몰입형 환경이 큰 화면에 표시됩니다. 가장자리에서 가장자리로의 장면, 세련 된 움직임, 색 및 입력 체계의 생생한 사용은 앱을 다음 수준으로 사용 합니다. 굵게 표시 됩니다.

![Xbox One 아바타 앱](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>10 피트 환경에 대 한 최적화

이제 10 피트 환경에 적합 한 UWP 앱 디자인의 원리를 배웠으므로 앱을 최적화 하 고 뛰어난 사용자 환경을 만들 수 있는 특정 방법에 대 한 다음 개요를 참조 하세요.

| 기능        | Description           |
| -------------------------------------------------------------- |--------------------------------|
| [UI 요소 크기 조정](#ui-element-sizing)  | 유니버설 Windows 플랫폼는 [크기 조정 및 유효 픽셀](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) 을 사용 하 여 보기 거리에 따라 UI의 크기를 조정 합니다. 크기를 이해 하 고 UI를 통해 적용 하면 10 피트 환경에 맞게 앱을 최적화 하는 데 도움이 됩니다.  |
|  [TV 안전 영역](#tv-safe-area) | UWP는 기본적으로 TV 안전 하지 않은 영역 (화면 가장자리에 가까운 영역)의 UI를 자동으로 표시 하지 않습니다. 그러나이는 UI가 letterboxed 보이는 "boxed" 효과를 만듭니다. TV에서 진정한 몰입형 앱이 되려면 지원하는 TV에서 화면 가장자리까지 확장되도록 앱을 수정하는 것이 좋습니다. |
| [색](#colors)  |  UWP는 색 테마를 지원 하 고, 시스템 테마 **를 기반으로 하는** 앱은 Xbox one에서 기본 설정으로 설정 됩니다. 앱에 특정 색 테마가 있는 경우 일부 색은 TV에서 제대로 작동하지 않으며 피해야 한다는 것을 고려해야 합니다. |
| [소리](../style/sound.md)    | 소리는 컨퍼런스에서 사용자에 게 피드백을 제공 하 고 사용자에 게 피드백을 제공 하는 데 도움이 되는 10 피트 환경의 주요 역할을 합니다. UWP는 Xbox One에서 앱이 실행 중일 때 공용 컨트롤의 소리를 자동으로 설정 하는 기능을 제공 합니다. UWP에 기본 제공 되는 소리 지원에 대해 자세히 알아보고이를 활용 하는 방법을 알아보세요.    |
| [UI 컨트롤에 대 한 지침](#guidelines-for-ui-controls)  |  여러 장치에서 잘 작동 하는 몇 가지 UI 컨트롤이 있지만 TV에서 사용 하는 경우 몇 가지 사항을 고려해 야 합니다. 10 피트 환경을 설계할 때 이러한 컨트롤을 사용 하기 위한 몇 가지 모범 사례에 대해 알아보세요. |
| [Xbox의 사용자 지정 시각적 상태 트리거](#custom-visual-state-trigger-for-xbox) | UWP 앱에 대해 10 피트 환경에 맞게 조정 하려면 사용자 지정 *시각적 상태 트리거* 를 사용 하 여 앱이 Xbox 콘솔에서 시작 된 것을 감지할 때 레이아웃 변경을 수행 하는 것이 좋습니다. |

이전 디자인 및 레이아웃 고려 사항 외에도 응용 프로그램을 빌드할 때 고려해 야 할 몇 가지 [게임 패드 및 원격 제어 상호 작용](../input/gamepad-and-remote-interactions.md) 최적화가 있습니다.

| 기능        | Description           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 포커스 탐색 및 상호 작용](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | **XY 포커스 탐색** 을 사용 하면 사용자가 앱의 UI를 탐색할 수 있습니다. 그러나이 경우 사용자가 위로, 아래로, 왼쪽 및 오른쪽으로 이동 하도록 제한 됩니다. 이에 대 한 권장 사항 및 기타 고려 사항은이 섹션에 설명 되어 있습니다. |
| [마우스 모드.](../input/gamepad-and-remote-interactions.md#mouse-mode)|지도, 그리기 및 그리기와 같은 일부 유형의 응용 프로그램에서는 XY 포커스 탐색이 실용적이 지 않거나 가능 하지 않습니다. 이러한 경우 **마우스 모드** 를 사용 하면 사용자가 PC의 마우스와 마찬가지로 게임 패드 또는 원격 제어로 자유롭게 이동할 수 있습니다.|
| [포커스 시각적 개체](../input/gamepad-and-remote-interactions.md#focus-visual)  | 포커스 시각적 개체는 현재 포커스가 있는 UI 요소를 강조 표시 하는 테두리입니다. 이렇게 하면 사용자가 탐색 하거나 상호 작용 하는 UI를 빠르게 식별할 수 있습니다.  |
| [포커스 연결](../input/gamepad-and-remote-interactions.md#focus-engagement) | 포커스 engagement를 사용 하려면 UI 요소에 포커스가 있을 때 사용자가 게임 패드 또는 원격 제어에서 **A/Select** 단추를 눌러야 합니다. |
| [하드웨어 단추](../input/gamepad-and-remote-interactions.md#hardware-buttons) | 게임 패드 및 원격 제어는 매우 다양 한 단추와 구성을 제공 합니다. |

> [!NOTE]
> 이 항목의 코드 조각은 대부분 XAML/c #에 있습니다. 그러나 원칙과 개념은 모든 UWP 앱에 적용 됩니다. Xbox 용 HTML/JavaScript UWP 앱을 개발 하는 경우 GitHub의 뛰어난 [Tvhelpers](https://github.com/Microsoft/TVHelpers/wiki) 라이브러리를 확인 하세요.

## <a name="ui-element-sizing"></a>UI 요소 크기 조정

10 피트 환경의 앱 사용자는 원격 제어 또는 게임 기를 사용 하 고 있으며 화면에서 몇 피트 떨어진 곳에 있기 때문에 디자인에 팩터링 해야 할 몇 가지 UI 고려 사항이 있습니다.
UI에 적절 한 콘텐츠 밀도가 있는지 확인 하 고, 사용자가 쉽게 요소를 탐색 하 고 선택할 수 있도록 너무 복잡해 지지 않도록 합니다. 주의: 단순성은 키입니다.

### <a name="scale-factor-and-adaptive-layout"></a>배율 인수 및 적응 레이아웃

**크기 조정 요소** 를 사용 하면 앱이 실행 되는 장치에 적합 한 크기 조정으로 UI 요소가 표시 되도록 할 수 있습니다.
바탕 화면에서이 설정은 **설정 > 시스템 >** 슬라이딩 값으로 표시 될 수 있습니다.
장치에서 지 원하는 경우 동일한 설정이 휴대폰에도 있습니다.

![텍스트, 앱 및 기타 항목 크기 변경](images/designing-for-tv/ui-scaling.png)

Xbox One에는 이러한 시스템 설정이 없지만 UWP UI 요소가 TV에 적절한 크기로 조정되도록 XAML 앱의 경우 **200%**, HTML 앱의 경우 **150%** 기본값으로 배율이 지정됩니다.
다른 장치에 대해 UI 요소의 크기가 적절 하 게 조정 되는 동안에는 TV에 대해 적절 하 게 크기가 조정 됩니다.
Xbox One은 1080p (1920 x 1080 픽셀)에서 앱을 렌더링 합니다. 따라서 PC와 같은 다른 장치에서 앱을 가져올 때 [적응 기법](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)을 활용 하 여 UI가 960 x 540 px 100% scale (또는 HTML 앱에 대 한 720% scale의 1280 x 100 px)에서 잘 보이는지 확인 합니다.

한 가지 해상도, 1920 x 1080에 대해 걱정 하기만 하면 되므로 Xbox를 위한 디자인은 PC 디자인과 약간 다릅니다.
사용자에 게 더 나은 해상도를 가진 TV가 있는지 여부는 중요 하지 않습니다 &mdash; . UWP 앱은 항상 1080p로 규모를 조정 합니다.

TV 해상도에 관계 없이 Xbox One에서 실행 하는 경우 앱에 대해 200% (또는 HTML 앱의 경우 150%)의 올바른 자산 크기를 가져올 수도 있습니다.

### <a name="content-density"></a>콘텐츠 밀도

앱을 디자인 하는 경우 사용자는 마우스 또는 터치식 입력을 사용 하는 것 보다 탐색 하는 데 시간이 오래 걸리는 원격 또는 게임 컨트롤러를 사용 하 여 UI를 표시 하 고 상호 작용 하 게 됩니다.

#### <a name="sizes-of-ui-controls"></a>UI 컨트롤의 크기

대화형 UI 요소는 최소 높이 32 window.epx.codesnippet (유효 픽셀)로 크기를 조정 해야 합니다. 이는 일반적인 UWP 컨트롤에 대 한 기본값이 며, 200% 배율로 사용 될 경우 UI 요소가 거리에서 표시 되 고 콘텐츠 밀도를 줄일 수 있도록 합니다.

![100% 및 200% 규모의 UWP 단추](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>클릭 횟수

사용자가 TV 화면의 한 가장자리에서 다른 가장자리로 이동 하는 경우 UI를 간소화 하기 위해 6 개 이하의 **클릭** 이 필요 합니다. 여기서는 **단순함** 의 원칙이 여기에 적용 됩니다. 

![6 개 아이콘](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>텍스트 크기

UI가 거리에서 표시 되도록 하려면 다음의 thumb 규칙을 사용 합니다.

* 주 텍스트 및 콘텐츠 읽기: 15 window.epx.codesnippet 최소값
* 중요 하지 않은 텍스트 및 보충 콘텐츠: 12 window.epx.codesnippet 최소값

UI에서 더 큰 텍스트를 사용 하는 경우 화면 부동산을 너무 많이 제한 하지 않는 크기를 선택 하 여 다른 콘텐츠가 채워질 수 있는 공간을 확보 합니다.

### <a name="opting-out-of-scale-factor"></a>배율 인수 옵트아웃

앱이 확장 비율 지원을 활용 하는 것이 좋습니다 .이를 통해 각 장치 유형에 대해 크기를 조정 하 여 모든 장치에서 적절 하 게 실행 하는 데 도움이 됩니다.
그러나이 동작을 옵트아웃 (opt out) 하 고 모든 UI를 100% 배율로 디자인할 수 있습니다. 배율 인수를 100% 이외의 값으로 변경할 수 없습니다.

XAML 앱의 경우 다음 코드 조각을 사용 하 여 배율 인수를 옵트아웃 (opt out) 할 수 있습니다.

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` 에서 옵트아웃 (opt out) 했는지 여부를 알려 줍니다.

HTML/JavaScript에 대 한 샘플 코드를 비롯 한 자세한 내용은 [크기 조정을 해제 하는 방법](../../xbox-apps/disable-scaling.md)을 참조 하세요.

이 항목에서 설명 하는 *유효* 픽셀 값을 *실제* 픽셀 값 (또는 HTML 앱의 경우 1.5을 곱하여)으로 두 배로 조정 하 여 적절 한 크기의 UI 요소를 계산 해야 합니다.

## <a name="tv-safe-area"></a>TV 안전 영역

모든 Tv에서 콘텐츠를 표시 하는 것은 과거 및 기술적인 이유로 인해 화면 가장자리까지 모두 표시 됩니다. 기본적으로 UWP는 TV 안전 하지 않은 영역에 UI 콘텐츠를 표시 하는 것을 방지 하 고 대신 페이지 배경만 그립니다.

TV 안전 하지 않은 영역은 다음 이미지의 파란색 영역으로 표시 됩니다.

![TV 안전 하지 않은 영역](images/designing-for-tv/tv-unsafe-area.png)

다음 코드 조각에서 보여 주는 것 처럼 배경을 정적 또는 테마가 적용 된 색 또는 이미지로 설정할 수 있습니다.

### <a name="theme-color"></a>테마 색

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>이미지

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

이는 추가 작업 없이 앱이 표시 되는 모양입니다.

![TV 안전 영역](images/designing-for-tv/tv-safe-area.png)

지금은 탐색 창과 그리드 같은 UI 부분이 잘린 것처럼 보이는 "박스" 효과가 앱에 나타나기 때문에 최적 상태가 아닙니다. 그러나 UI의 일부를 화면 가장자리로 확장 하 여 앱에 더 많은 효과를 줄 수 있도록 최적화를 수행할 수 있습니다.

### <a name="drawing-ui-to-the-edge"></a>가장자리에 UI 그리기

사용자에 게 더 많은 집중 교육을 제공 하기 위해 특정 UI 요소를 사용 하 여 화면 가장자리로 확장 하는 것이 좋습니다. 여기에는 [ScrollViewers](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [Nav 창](../controls-and-patterns/navigationview.md)및 [CommandBars](/uwp/api/Windows.UI.Xaml.Controls.CommandBar)가 포함 됩니다.

반면에 대화형 요소와 텍스트는 일부 Tv에서 잘리지 않도록 하기 위해 항상 화면 가장자리를 피하는 것도 중요 합니다. 화면 가장자리의 5% 내에서 필수적이 지 않은 시각적 개체만 그리는 것이 좋습니다. [UI 요소 크기 조정](#ui-element-sizing)에 설명 된 대로, Xbox one 콘솔의 기본 배율 비율 200%에 해당 하는 UWP 앱은 960 x 540 epx의 영역을 사용 하므로 앱의 UI에서 다음과 같은 영역에 필수 UI를 배치 하지 않아야 합니다.

- 위쪽 및 아래쪽에서 27 window.epx.codesnippet
- 48 왼쪽 및 오른쪽 가장자리에서의 window.epx.codesnippet

다음 섹션에서는 UI를 화면 가장자리로 확장 하는 방법을 설명 합니다.

#### <a name="core-window-bounds"></a>핵심 창 경계

10 피트 환경을 대상으로 하는 UWP 앱의 경우에는 핵심 창 경계를 사용 하는 것이 더 간단 합니다.

`OnLaunched`의 메서드에서 `App.xaml.cs` 다음 코드를 추가 합니다.

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

이 코드 줄을 사용 하면 앱 창이 화면 가장자리로 확장 되므로 모든 대화형 및 필수 UI를 앞에서 설명한 TV 안전 영역으로 이동 해야 합니다. 상황에 맞는 메뉴 및 열린 [ComboBoxes](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)같은 임시 UI는 TV 안전 영역 내에 자동으로 남아 있습니다.

![핵심 창 경계](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>창 배경

탐색 창은 일반적으로 화면 가장자리 근처에 그려져 있으므로 배경을 TV 안전 하지 않은 영역으로 확장 하 여 잘못 된 간격을 도입 하지 않도록 합니다. 이렇게 하려면 탐색 창의 배경색을 앱 배경의 색으로 변경 하면 됩니다.

앞에서 설명한 대로 핵심 창 경계를 사용 하면 UI를 화면 가장자리에 그릴 수 있지만 [SplitView](/uwp/api/Windows.UI.Xaml.Controls.SplitView)의 콘텐츠에 양의 여백을 사용 하 여 TV 안전 영역 내에 유지 해야 합니다.

![화면 가장자리로 확장 된 탐색 창](images/designing-for-tv/tv-safe-areas-2.png)

여기서 탐색 창의 배경은 화면 가장자리로 확장 되었으며 해당 탐색 항목은 TV 안전 영역에 유지 됩니다.
`SplitView`(이 경우에는 항목의 그리드) 콘텐츠가 화면 아래쪽으로 확장 되어 계속 표시 되는 것 처럼 표시 되 고, 그리드 위쪽은 TV 안전 영역 내에 있습니다. [목록 및 표의 스크롤 끝](#scrolling-ends-of-lists-and-grids)에서이 작업을 수행 하는 방법에 대해 자세히 알아보세요.

다음 코드 조각에서는이 효과를 적용 합니다.

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[명령 모음](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 은 일반적으로 앱의 하나 이상 가장자리 근처에 배치 되는 창의 또 다른 예입니다. TV에서는 배경이 화면 가장자리로 확장 되어야 합니다. 또한 일반적으로 "..."로 표시 되는 **더 많은** 단추를 포함 합니다. 오른쪽에는 TV 안전 영역에 남아 있어야 합니다. 다음은 원하는 상호 작용 및 시각적 효과를 얻기 위한 몇 가지 다른 전략입니다.

**옵션 1**: 배경색을 `CommandBar` 페이지 배경과 동일한 색 또는 투명 한 색으로 변경 합니다.

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

이렇게 하면 `CommandBar` 페이지의 나머지 부분과 동일한 배경 위에 있는 것 처럼 보일 수 있으므로 배경은 화면 가장자리에 원활 하 게 흐릅니다.

**옵션 2**: 채우기가 배경과 동일한 색 인 배경 사각형을 추가 하 `CommandBar` 고 `CommandBar` 페이지의 나머지 부분에서 및 아래에 배치 합니다.

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> 이 방법을 사용 하는 경우 **추가** 단추를 클릭 하면 필요한 경우 열린의 높이가 변경 되어 `CommandBar` 아이콘 아래에의 레이블이 표시 됩니다 `AppBarButton` . 이러한 크기 조정을 방지 하려면 레이블을 아이콘의 *오른쪽* 으로 이동 하는 것이 좋습니다. 자세한 내용은 [CommandBar 레이블](#commandbar-labels)을 참조 하세요.

이 두 가지 방법 모두이 섹션에 나열 된 다른 형식의 컨트롤에도 적용 됩니다.

#### <a name="scrolling-ends-of-lists-and-grids"></a>목록과 모눈의 스크롤 끝

목록과 그리드는 동시에 화면에 맞출 수 있는 것 보다 더 많은 항목을 포함 하는 것이 일반적입니다. 이 경우 목록 또는 그리드를 화면 가장자리로 확장 하는 것이 좋습니다. 가로 스크롤 목록과 모눈은 오른쪽 가장자리로 확장 하 고 세로 스크롤은 아래쪽으로 확장 해야 합니다.

![TV 안전 영역 그리드 구분](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

목록 또는 눈금이 다음과 같이 확장 되는 동안 포커스 시각적 개체와 관련 항목을 TV 안전 영역 내에 유지 하는 것이 중요 합니다.

![스크롤 그리드 포커스는 TV 안전 영역에 유지 되어야 합니다.](images/designing-for-tv/scrolling-grid-focus.png)

UWP에는 포커스를 [VisibleBounds](/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds)내부에 유지 하는 기능이 있지만 목록/그리드 항목을 안전 영역 보기로 스크롤할 수 있도록 안쪽 여백을 추가 해야 합니다. 특히 다음 코드 조각과 같이 [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) 또는 [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView)의 [ItemsPresenter](/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter)에 양의 여백을 추가 합니다.

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

페이지 또는 앱 리소스에 이전 코드 조각을 추가한 후 다음과 같은 방법으로 액세스할 수 있습니다.

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> 이 코드 조각은 특히에 해당 합니다. `ListView` 스타일의 경우 `GridView` [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 와 [스타일](/uwp/api/Windows.UI.Xaml.Style) 모두에 대해 [TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) 특성을로 설정 `GridView` 합니다.

항목이 표시 되는 방식을 보다 세부적으로 제어 하기 위해 응용 프로그램이 버전 1803 이상을 대상으로 하는 경우 [BringIntoViewRequested 이벤트](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)를 사용할 수 있습니다. [ItemsPanel](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) 다음 코드 조각과 **ListView** / 같이 내부 **ScrollViewer** 가 실행 되기 전에이를 catch 하기 위해 ListView**GridView** 의 ItemsPanel에 넣을 수 있습니다.

```xaml
<GridView x:Name="gridView">
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid Orientation="Horizontal"
                           BringIntoViewRequested="ItemsWrapGrid_BringIntoViewRequested"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

```cs
// The BringIntoViewRequested event is raised by the framework when items receive keyboard (or Narrator) focus or 
// someone triggers it with a call to UIElement.StartBringIntoView.
private void ItemsWrapGrid_BringIntoViewRequested(UIElement sender, BringIntoViewRequestedEventArgs args)
{
    if (args.VerticalAlignmentRatio != 0.5)  // Guard against our own request
    {
        args.Handled = true;
        // Swallow this request and restart it with a request to center the item.  We could instead have chosen
        // to adjust the TargetRect’s Y and Height values to add a specific amount of padding as it bubbles up, 
        // but if we just want to center it then this is easier.

        // (Optional) Account for sticky headers if they exist
        var headerOffset = 0.0;
        var itemsWrapGrid = sender as ItemsWrapGrid;
        if (gridView.IsGrouping && itemsWrapGrid.AreStickyGroupHeadersEnabled)
        {
            var header = gridView.GroupHeaderContainerFromItemContainer(args.TargetElement as GridViewItem);
            if (header != null)
            {
                headerOffset = ((FrameworkElement)header).ActualHeight;
            }
        }

        // Issue a new request
        args.TargetElement.StartBringIntoView(new BringIntoViewOptions()
        {
            AnimationDesired = true,
            VerticalAlignmentRatio = 0.5, // a normalized alignment position (0 for the top, 1 for the bottom)
            VerticalOffset = headerOffset, // applied after meeting the alignment ratio request
        });
    }
}
```

## <a name="colors"></a>색

기본적으로 유니버설 Windows 플랫폼는 tv 안전 범위로 앱 색의 크기를 조정 합니다 (자세한 내용은 [tv 안전 색](#tv-safe-colors) 참조). 앱이 tv에서 잘 보이도록 합니다. 또한 TV에서 시각적 환경을 개선 하기 위해 앱에서 사용 하는 색 집합에 대해 향상 된 기능이 있습니다.

### <a name="application-theme"></a>응용 프로그램 테마

앱에 적합 한 항목에 따라 **응용 프로그램 테마** (어둡게 또는 밝게)를 선택 하거나 테마를 옵트아웃 (opt out) 할 수 있습니다. [색 테마](../style/color.md)의 테마에 대 한 일반적인 권장 사항에 대해 자세히 알아보세요.

또한 UWP를 사용 하면 앱이 실행 되는 장치에서 제공 하는 시스템 설정에 따라 테마를 동적으로 설정할 수 있습니다.
UWP는 항상 사용자가 지정 하는 테마 설정을 고려 하지만 각 장치는 적절 한 기본 테마를 제공 합니다.
Xbox One의 특성으로 인해 *생산성* 경험 보다 많은 *미디어* 환경을 사용할 것으로 예상 되는 경우 기본적으로 어두운 시스템 테마를 사용할 수 있습니다.
앱의 테마가 시스템 설정을 기반으로 하는 경우 Xbox One에서는 기본적으로 어둡게 설정됩니다.

### <a name="accent-color"></a>강조 색

UWP는 사용자가 시스템 설정에서 선택한 **강조 색** 을 표시 하는 편리한 방법을 제공 합니다.

Xbox One에서는 사용자가 PC에서 강조 색을 선택할 수 있는 것 처럼 사용자 색을 선택할 수 있습니다.
앱이 브러시 또는 색 리소스를 통해 이러한 강조 색을 호출 하는 한 시스템 설정에서 사용자가 선택한 색이 사용 됩니다. Xbox One의 악센트 색은 시스템이 아닌 사용자 당입니다.

또한 Xbox One의 사용자 색 집합은 Pc, 휴대폰 및 기타 장치의 사용자 색 집합과 동일 하지 않습니다.

앱에서 **SystemControlForegroundAccentBrush**또는 색 리소스 (**SystemAccentColor**)와 같은 브러시 리소스를 사용 하거나 대신 [UIColorType *](/uwp/api/Windows.UI.ViewManagement.UIColorType) API를 통해 악센트 색을 직접 호출 하는 경우 해당 색은 Xbox one에서 사용할 수 있는 악센트 색으로 바뀝니다. 고대비 브러시 색은 PC와 휴대폰에서와 동일한 방법으로 시스템에서 가져옵니다.

일반적으로 강조 색에 대 한 자세한 내용은 [강조 색](../style/color.md#accent-color)을 참조 하세요.

### <a name="color-variance-among-tvs"></a>Tv 간의 색 차이

TV를 설계할 때 색은 렌더링 되는 TV에 따라 크게 다르게 표시 됩니다. 색이 모니터에서와 똑같이 표시 된다고 가정 하지 마십시오. 앱이 UI의 일부를 구분 하기 위해 색의 미묘한 차이를 사용 하는 경우 색은 함께 혼합할 수 있으며 사용자는 혼동을 받을 수 있습니다. 사용자가 사용 하는 TV에 관계 없이 사용자가 명확 하 게 구분할 수 있는 색을 사용 하려고 합니다.

### <a name="tv-safe-colors"></a>TV 안전 색

색의 RGB 값은 빨강, 녹색 및 파랑의 강도를 나타냅니다. Tv는 극단적인 강도를 잘 처리 하지 않습니다 &mdash; . 이러한 효과는 홀수 줄무늬 효과를 생성 하거나 특정 tv에서 희미하게 표시 될 수 있습니다. 또한 높은 강도 색으로 인해 blooming (인접 한 픽셀은 동일한 색 그리기 시작) 될 수 있습니다. TV를 안전 하 게 보호 하는 것으로 간주 되는 항목에는 다른 학교가 있지만 RGB 값 16-235 (또는 16 진수의 경우 10-EB)에 있는 색은 일반적으로 TV에 안전 하 게 사용할 수 있습니다.

![TV 안전 색 범위](images/designing-for-tv/tv-safe-colors-2.png)

지금까지 Xbox의 앱은이 "TV 안전" 색 범위 내에서 색을 조정 해야 했습니다. 그러나 적합 한 작성자 업데이트로 시작 하는 Xbox One은 전체 범위 콘텐츠를 TV 안전 범위로 자동으로 확장 합니다. 즉, 대부분의 앱 개발자는 TV 안전 색에 대해 더 이상 걱정할 필요가 없습니다.

> [!IMPORTANT]
> TV 안전 색 범위에 이미 있는 비디오 콘텐츠에는 [미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk)을 사용 하 여 재생할 때이 색 크기 조정 효과가 적용 되지 않습니다.

DirectX 11 또는 DirectX 12를 사용 하 여 앱을 개발 하 고 UI 또는 비디오를 렌더링 하기 위해 고유한 스왑 체인을 만드는 경우 [IDXGISwapChain3:: SetColorSpace1](/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1)를 호출 하 여 사용 하는 색 공간을 지정할 수 있습니다. 그러면 시스템에서 색의 크기를 조정 해야 하는지 여부를 알 수 있습니다.

## <a name="guidelines-for-ui-controls"></a>UI 컨트롤에 대 한 지침

여러 장치에서 잘 작동 하는 몇 가지 UI 컨트롤이 있지만 TV에서 사용 하는 경우 몇 가지 사항을 고려해 야 합니다. 10 피트 환경을 설계할 때 이러한 컨트롤을 사용 하기 위한 몇 가지 모범 사례에 대해 알아보세요.

### <a name="pivot-control"></a>피벗 컨트롤

[피벗](/uwp/api/Windows.UI.Xaml.Controls.Pivot) 은 다른 머리글이 나 탭을 선택 하 여 앱 내에서 보기를 빠르게 탐색 합니다. 컨트롤은 어떤 머리글에 포커스가 있는지 밑줄을 사용 하 여 게임 패드/원격을 사용할 때 현재 선택 된 헤더를 보다 명확 하 게 합니다.

![피벗 밑줄](images/designing-for-tv/pivot-underline.png)

[IsHeaderItemsCarouselEnabled](/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty) 속성을로 설정 하면 `true` 선택 된 피벗 머리글이 항상 첫 번째 위치로 이동 하는 것이 아니라 피벗이 항상 동일한 위치를 유지할 수 있습니다. TV와 같은 큰 화면 표시에서는 헤더 래핑이 사용자에게 방해가 될 수 있으므로 환경이 개선됩니다. 모든 피벗 머리글이 한 번에 화면에 맞지 않는 경우 고객이 다른 헤더를 볼 수 있도록 스크롤 막대가 있습니다. 그러나 최상의 환경을 제공 하기 위해 모든 것이 화면에 적합 한지 확인 해야 합니다. 자세한 내용은 [탭 및 피벗](../controls-and-patterns/pivot.md)을 참조 하세요.

### <a name="navigation-pane"></a>탐색 창 <a name="navigation-pane" />

탐색 창 ( *햄버거 메뉴*라고도 함)은 UWP 앱에서 일반적으로 사용 되는 탐색 컨트롤입니다. 일반적으로 사용자가 다른 페이지로 이동 하는 목록 스타일 메뉴에서 선택할 수 있는 몇 가지 옵션이 포함 된 창입니다. 일반적으로이 창은 공간 절약을 위해 축소 된 상태로 시작 되며, 사용자는 단추를 클릭 하 여 열 수 있습니다.

마우스 및 터치를 사용 하 여 탐색 창에 액세스할 수 있지만, 게임 패드/원격을 사용 하면 사용자가 창을 열기 위해 단추를 탐색 해야 하므로 액세스 가능성이 줄어듭니다. 따라서 **보기** 단추를 사용 하 여 탐색 창을 열고 사용자가 페이지의 왼쪽에 있는 모든 방법을 탐색 하 여 열 수 있도록 하는 것이 좋습니다. 이 디자인 패턴을 구현 하는 방법에 대 한 코드 샘플은 [프로그래밍 방식 포커스 탐색](../input/focus-navigation-programmatic.md#split-view-code-sample) 문서에서 찾을 수 있습니다. 이를 통해 사용자는 창의 내용에 매우 쉽게 액세스할 수 있습니다. 탐색 창이 다른 화면 크기에서 작동 하는 방법 및 게임 패드/원격 탐색의 모범 사례에 대 한 자세한 내용은 [nav pane](../controls-and-patterns/navigationview.md)을 참조 하세요.

### <a name="commandbar-labels"></a>CommandBar 레이블

해당 높이가 최소화 되 고 일관 되 게 유지 되도록 [CommandBar](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 의 아이콘 오른쪽에 레이블을 배치 하는 것이 좋습니다. 이 작업은 [CommandBar. DefaultLabelPosition](/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) 속성을로 설정 하 여 수행할 수 `CommandBarDefaultLabelPosition.Right` 있습니다.

![아이콘 오른쪽에 레이블이 있는 명령 모음](images/designing-for-tv/commandbar.png)

이 속성을 설정 하면 레이블이 항상 표시 됩니다 .이는 사용자에 대 한 클릭 횟수를 최소화 하기 때문에 10 피트 환경에 적합 합니다. 이는 다른 장치 유형을 따르는 좋은 모델 이기도 합니다.

### <a name="tooltip"></a>Tooltip

[도구 설명](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) 컨트롤은 사용자가 마우스를 위로 가져갈 때 UI에 추가 정보를 제공 하는 방법으로 도입 되었습니다. 게임 패드 및 원격의 경우 `Tooltip` 요소가 포커스를 가질 때 잠깐 후에 표시 되 고, 잠시 동안 화면을 유지 한 후 사라집니다. 너무 많은를 사용 하는 경우이 동작이 혼란을 수 있습니다 `Tooltip` . `Tooltip`TV를 설계할 때를 사용 하지 않도록 합니다.

### <a name="button-styles"></a>단추 스타일

표준 UWP 단추가 TV에서 잘 작동 하는 반면, 단추의 일부 비주얼 스타일은 UI에 대 한 주의를 표시 합니다. 특히,이는 특히 10 피트 환경에서 포커스가 있는 위치를 명확 하 게 전달 하는 데 도움이 되는 모든 플랫폼에 대해 고려해 야 할 수 있습니다. 이러한 스타일에 대 한 자세한 내용은 [단추](../controls-and-patterns/buttons.md)를 참조 하세요.

### <a name="nested-ui-elements"></a>중첩 된 UI 요소

중첩 된 UI는 컨테이너 UI 요소 내에 중첩 된 실행 가능한 항목을 노출 합니다. 중첩 된 항목 뿐만 아니라 컨테이너 항목도 서로 독립적인 포커스를 가질 수 있습니다.

중첩 된 UI는 일부 입력 형식에 대해서는 잘 작동 하지만 항상 사용 하는 것은 아닙니다. 이 항목의 지침에 따라 UI가 10 피트 환경에 맞게 최적화 되어 있고 사용자가 모든 interactable 요소에 쉽게 액세스할 수 있는지 확인 해야 합니다. 한 가지 일반적인 해결 방법은 중첩 된 UI 요소를에 넣는 것입니다 `ContextFlyout` .

중첩 된 UI에 대 한 자세한 내용은 [목록 항목의 중첩 된 ui](../controls-and-patterns/nested-ui.md)를 참조 하세요.

### <a name="mediatransportcontrols"></a>MediaTransportControls

[MediaTransportControls](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 요소를 사용 하면 사용자가 재생, 일시 중지, 폐쇄 캡션 등을 수행할 수 있는 기본 재생 환경을 제공 하 여 미디어와 상호 작용할 수 있습니다. 이 컨트롤은 [MediaPlayerElement](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 의 속성 이며 두 개의 레이아웃 옵션인 *단일 행* 및 *두 행*을 지원 합니다. 단일 행 레이아웃에서 슬라이더와 재생 단추는 모두 한 행에 있으며 슬라이더 왼쪽에 있는 재생/일시 중지 단추가 있습니다. 이중 행 레이아웃에서 슬라이더는 자체 행을 차지 하며 재생 단추는 별도의 아래쪽 행에 있습니다. 10 피트 환경을 디자인 하는 경우 더 나은 게임 패드 탐색을 제공 하므로 이중 행 레이아웃을 사용 해야 합니다. 이중 행 레이아웃을 사용 하도록 설정 하려면 `IsCompact="False"` 의 요소에서를 설정 `MediaTransportControls` [TransportControls](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) `MediaPlayerElement` 합니다.

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

미디어를 앱에 추가 하는 방법에 대 한 자세한 내용을 보려면 [미디어 재생](../controls-and-patterns/media-playback.md) 을 방문 하세요.

> ! [참고] `MediaPlayerElement` 는 Windows 10 버전 1607 이상 에서만 사용할 수 있습니다. 이전 버전의 Windows 10 용 응용 프로그램을 개발 하는 경우에는 [MediaElement](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 를 대신 사용 해야 합니다. 위의 권장 사항은에 `MediaElement` 도 적용 되며, 속성은 `TransportControls` 동일한 방식으로 액세스 됩니다.

### <a name="search-experience"></a>검색 환경

콘텐츠 검색은 10 피트 환경에서 가장 일반적으로 수행 되는 기능 중 하나입니다. 앱이 검색 환경을 제공 하는 경우 사용자는 게임 패드의 **Y** 단추를 액셀러레이터 키로 사용 하 여 빠르게 액세스할 수 있도록 하는 것이 도움이 됩니다.

대부분의 고객은이 액셀러레이터를 이미 잘 알고 있어야 하지만, 원하는 경우 시각적 **Y** 문자 모양을 UI에 추가 하 여 고객이 단추를 사용 하 여 검색 기능에 액세스할 수 있음을 나타낼 수 있습니다. 이 큐를 추가 하는 경우에는 **Segoe XBOX MDL2 symbol** 글꼴 (HTML 앱의 경우)에서 기호를 사용 하 여 `&#xE3CC;` `\E426` Xbox shell 및 기타 앱과의 일관성을 제공 해야 합니다.

> [!NOTE]
> **Segoe Xbox MDL2 Symbol** 글꼴은 Xbox에서만 사용할 수 있기 때문에 PC에서는 기호가 제대로 표시되지 않습니다. 그러나 Xbox에 배포한 후에는 TV에 표시 됩니다.

**Y** 단추는 게임 패드 에서만 사용할 수 있으므로 UI의 단추와 같은 검색에 대 한 다른 액세스 방법을 제공 해야 합니다. 그렇지 않으면 일부 고객이 기능에 액세스 하지 못할 수 있습니다.

10 피트 환경에서는 고객이 전체 화면 검색 환경을 사용 하는 것이 더 쉬울 수도 있습니다. 전체 화면 또는 부분 화면, "내부" 검색을 사용 하는 경우 사용자가 검색 환경을 열 때 사용자가 검색 환경을 열 때 사용자가 검색 용어를 입력할 준비가 되 면 화면 키보드가 이미 열려 있는 것이 좋습니다.

## <a name="custom-visual-state-trigger-for-xbox"></a>Xbox의 사용자 지정 시각적 상태 트리거

UWP 앱을 10 피트 환경에 맞게 조정 하려면 앱이 Xbox 콘솔에서 시작 된 것을 감지할 때 레이아웃을 변경 하는 것이 좋습니다. 이 작업을 수행 하는 한 가지 방법은 사용자 지정 *시각적 상태 트리거*를 사용 하는 것입니다. 시각적 상태 트리거는 **Blend for Visual Studio**에서 편집 하려는 경우에 가장 유용 합니다. 다음 코드 조각에서는 Xbox의 시각적 상태 트리거를 만드는 방법을 보여 줍니다.

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

트리거를 만들려면 앱에 다음 클래스를 추가 합니다. 이 클래스는 위의 XAML 코드에서 참조 하는 클래스입니다.

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

사용자 지정 트리거를 추가한 후에는 앱이 Xbox One 콘솔에서 실행 중임을 검색할 때마다 XAML 코드에서 지정 된 레이아웃을 자동으로 수정 합니다.

앱이 Xbox에서 실행 중인지 확인 하 고 코드를 통해 적절 한 조정을 수행할 수 있습니다. 다음 간단한 변수를 사용 하 여 앱이 Xbox에서 실행 중인지 확인할 수 있습니다.

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

그런 다음이 검사에 따라 코드 블록에서 UI를 적절 하 게 조정할 수 있습니다. 

## <a name="summary"></a>요약

10 피트 환경을 디자인 하는 경우 다른 플랫폼에 대 한 디자인과 다르게 고려 하기 위해 고려해 야 할 특별 한 고려 사항이 있습니다. UWP 앱의 직선 포트를 Xbox One으로 사용할 수는 있지만, 작동 하는 것은 아니지만, 사용자의 불편을 일으킬 수 있는 것은 아닙니다. 이 문서의 지침에 따라 앱이 TV에 있을 수 있는 것과 동일한 지 확인 합니다.

## <a name="related-articles"></a>관련된 문서

- [Windows 앱 용 장치 입문](index.md)
- [게임 패드 및 리모컨 조작](../input/gamepad-and-remote-interactions.md)
- [UWP 앱의 소리](../style/sound.md)