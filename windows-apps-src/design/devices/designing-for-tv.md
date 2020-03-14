---
Description: TV에서 멋지게 보이고 제대로 작동하도록 앱을 디자인합니다.
title: Xbox 및 TV용 디자인
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, 3m 환경, 게임 패드, 리모컨, 입력, 조작
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 431b8912e43647bc2678aaab7efc9ec68b866d10
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209999"
---
# <a name="designing-for-xbox-and-tv"></a>Xbox 및 TV용 디자인

Xbox One 및 TV 화면에서 멋지게 보이고 제대로 작동하도록 UWP(유니버설 Windows 플랫폼) 앱을 디자인합니다.

*10 피트* 환경에서 UWP 응용 프로그램의 상호 작용 환경에 대 한 지침은 [게임 패드 및 원격 제어 상호 작용](../input/gamepad-and-remote-interactions.md) 을 참조 하세요.

## <a name="overview"></a>개요

유니버설 Windows 플랫폼을 사용하면 여러 Windows&nbsp;10 디바이스에서 흥미로운 환경을 만들 수 있습니다.
UWP 프레임워크에서 제공하는 대부분의 기능은 추가 작업 없이 앱이 이러한 디바이스에서 동일한 UI(사용자 인터페이스)를 사용할 수 있게 합니다.
그러나 Xbox One 및 TV 화면에서 잘 작동하도록 앱을 조정하고 최적화하려면 특별한 고려 사항이 필요합니다.

실내의 소파에 앉아 TV와 상호 작용 하는 게임 패드 또는 리모컨을 사용하여 TV를 조작하는 환경을 **10피트 환경**이라고 합니다.
일반적으로 사용자가 화면에서 약 3m 떨어진 곳에 앉아 있기 때문에 이렇게 이름이 지정되었습니다.
이 경우 가령 *0.6m* 환경이나 PC 조작 시에는 존재하지 않는 고유한 과제가 발생합니다.
Xbox One 또는 TV 화면에 출력되고 입력에 컨트롤러를 사용하는 다른 모든 디바이스용 앱을 개발하는 경우 항상 이 점에 유의해야 합니다.

이 문서의 모든 단계가 3m 환경에 적합한 앱을 만드는 데 필요한 것은 아니지만 이해하고 앱에 적절한 결정을 하면 앱의 특정 요구에 맞는 더 나은 3m 환경을 만들 수 있습니다.
3m 환경에 맞는 앱을 만들려면 다음 디자인 원칙을 고려합니다.

### <a name="simple"></a>단순

3m 환경에 맞게 디자인하려면 일련의 고유한 과제를 처리해야 합니다. 해상도 및 시청 거리 때문에 사용자가 너무 많은 정보를 처리하기 어려울 수 있습니다.
가능한 가장 간단한 구성 요소로 줄여 디자인을 명확하게 유지합니다. TV에 표시되는 정보량은 데스크톱이 아니라 휴대폰에 표시되는 정보량과 일치해야 합니다.

![Xbox One 홈 화면](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>일관성

3m 환경의 UWP 앱은 직관적이고 사용하기 쉬워야 합니다. 포커스를 명확하게 유지합니다.
공간에서의 이동이 일관되고 예측 가능하도록 콘텐츠를 정렬합니다. 사용자에게 수행하려는 작업에 대한 최단 경로를 제공합니다.

![Xbox One 영화 앱](images/designing-for-tv/xbox-movies-app.png)

_**스크린샷에 표시 된 모든 동영상은 Microsoft 영화 & TV에서 사용할 수 있습니다.**_  

### <a name="captivating"></a>흥미 유발

영화 같은 가장 몰입형 환경이 큰 화면에 표시됩니다. 횡단 간 장면, 세련된 동작 및 생생한 색과 입력 체계 사용을 통해 앱을 한 단계 더 발전시킬 수 있습니다. 대담하고 아름답게 구현하세요.

![Xbox One 아바타 앱](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>3m 환경에 최적화

3m 환경에 적합한 UWP 앱 디자인의 원칙을 배웠으므로 이제 앱을 최적화하고 뛰어난 사용자 환경을 만들 수 있는 특정 방법에 대한 다음 개요를 살펴보세요.

| 기능        | 설명           |
| -------------------------------------------------------------- |--------------------------------|
| [UI 요소 크기 조정](#ui-element-sizing)  | 유니버설 Windows 플랫폼은 [크기 조정 및 유효 픽셀](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)을 사용하여 시청 거리에 따라 UI의 크기를 조정합니다. 크기 조정을 이해하고 UI 전체에 적용하면 3m 환경에 앱을 최적화하는 데 도움이 됩니다.  |
|  [TV 안전 영역](#tv-safe-area) | UWP는 기본적으로 안전하지 않은 TV 영역(화면 가장자리에 가까운 영역)에 UI가 표시되는 것을 자동으로 방지합니다. 그러나 이 때문에 UI가 레터박스처럼 보이는 "박스" 효과가 발생합니다. TV에서 진정한 몰입형 앱이 되려면 지원하는 TV에서 화면 가장자리까지 확장되도록 앱을 수정하는 것이 좋습니다. |
| [분판](#colors)  |  UWP는 색 테마를 지원하며, 시스템 테마를 적용하는 앱은 Xbox One에서 기본적으로 **어둡게**로 설정됩니다. 앱에 특정 색 테마가 있는 경우 일부 색은 TV에서 제대로 작동하지 않으며 피해야 한다는 것을 고려해야 합니다. |
| [소리](../style/sound.md)    | 소리는 사용자를 몰두하게 하고 피드백을 제공하는 데 도움이 되어 3m 환경에 중요한 역할을 합니다. UWP는 앱이 Xbox One에서 실행 중일 때 공통 컨트롤에 대한 소리를 자동으로 켜는 기능을 제공합니다. UWP에 기본 제공되는 소리 지원 기능에 대한 자세한 내용을 살펴보고 이를 활용하는 방법을 알아봅니다.    |
| [UI 컨트롤에 대 한 지침](#guidelines-for-ui-controls)  |  여러 UI 컨트롤이 다양한 디바이스에서 잘 작동하지만 TV에서 사용할 경우 특별히 고려해야 할 사항이 있습니다. 3m 환경용으로 디자인할 경우 이러한 컨트롤 사용을 위한 몇 가지 모범 사례에 대해 알아봅니다. |
| [Xbox의 사용자 지정 시각적 상태 트리거](#custom-visual-state-trigger-for-xbox) | 10피트 환경에 맞게 UWP 앱을 조정하려면 사용자 지정 *시각적 상태 트리거*를 사용하여 앱이 Xbox 콘솔에서 시작된 것을 감지할 때 레이아웃을 변경하는 것이 좋습니다. |

이전 디자인 및 레이아웃 고려 사항 외에도 응용 프로그램을 빌드할 때 고려해 야 할 몇 가지 [게임 패드 및 원격 제어 상호 작용](../input/gamepad-and-remote-interactions.md) 최적화가 있습니다.

| 기능        | 설명           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 포커스 탐색 및 상호 작용](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | **XY 포커스 탐색** 을 사용 하면 사용자가 앱의 UI를 탐색할 수 있습니다. 그러나 사용자가 위, 아래, 왼쪽 및 오른쪽으로만 탐색할 수 있도록 제한됩니다. 이 섹션에는 이 문제를 해결하기 위한 권장 사항과 기타 고려 사항이 요약되어 있습니다. |
| [마우스 모드](../input/gamepad-and-remote-interactions.md#mouse-mode)|지도, 그리기 및 그리기와 같은 일부 유형의 응용 프로그램에서는 XY 포커스 탐색이 실용적이 지 않거나 가능 하지 않습니다. 이러한 경우 **마우스 모드** 를 사용 하면 사용자가 PC의 마우스와 마찬가지로 게임 패드 또는 원격 제어로 자유롭게 이동할 수 있습니다.|
| [포커스 시각적 개체](../input/gamepad-and-remote-interactions.md#focus-visual)  | 포커스 시각적 개체는 현재 포커스가 있는 UI 요소를 강조 표시 하는 테두리입니다. 이렇게 하면 사용자가 탐색 하거나 상호 작용 하는 UI를 빠르게 식별할 수 있습니다.  |
| [포커스 참여](../input/gamepad-and-remote-interactions.md#focus-engagement) | 포커스 engagement를 사용 하려면 UI 요소에 포커스가 있을 때 사용자가 게임 패드 또는 원격 제어에서 **A/Select** 단추를 눌러야 합니다. |
| [하드웨어 단추](../input/gamepad-and-remote-interactions.md#hardware-buttons) | 게임 패드 및 원격 제어는 매우 다양 한 단추와 구성을 제공 합니다. |

> [!NOTE]
> 이 항목의 코드 조각은 대부분 XAML/C#으로 작성되었지만 원칙과 개념은 모든 UWP 앱에 적용됩니다. Xbox용 HTML/JavaScript UWP 앱을 개발하는 경우 GitHub 라이브러리에서 뛰어난 [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) 라이브러리를 확인하세요.

## <a name="ui-element-sizing"></a>UI 요소 크기 조정

3m 환경에서 앱의 사용자는 리모컨이나 게임 패드를 사용하고 화면에서 몇 미터 떨어진 곳에 앉아 있으므로 디자인에 고려해야 하는 몇 가지 UI 고려 사항이 있습니다.
UI의 콘텐츠 밀도를 적절하게 유지하고, 사용자가 요소를 쉽게 탐색하고 선택할 수 있도록 UI가 너무 혼잡하지 않도록 합니다. 단순성이 중요하다는 것을 명심하세요.

### <a name="scale-factor-and-adaptive-layout"></a>배율 인수 및 적응형 레이아웃

**배율 인수**는 앱을 실행하는 디바이스에 맞는 크기로 UI 요소가 표시되도록 하는 데 도움이 됩니다.
데스크톱에서 이 설정은 **설정 &gt; 시스템 &gt; 디스플레이**에 슬라이딩 값으로 제공됩니다.
디바이스에서 지원하는 경우 휴대폰에도 이 동일한 설정이 있습니다.

![텍스트, 앱 및 기타 항목의 크기 변경](images/designing-for-tv/ui-scaling.png)

Xbox One에는 이러한 시스템 설정이 없지만 UWP UI 요소가 TV에 적절한 크기로 조정되도록 XAML 앱의 경우 **200%** , HTML 앱의 경우 **150%** 기본값으로 배율이 지정됩니다.
UI 요소가 다른 디바이스에 적절한 크기로 조정되기만 하면 TV에서도 적절한 크기로 조정됩니다.
Xbox One은 앱을 1080p(1920 x 1080픽셀)로 렌더링합니다. 따라서 PC 등의 다른 디바이스에서 앱을 가져오는 경우 [적응형 기술](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) 이용하여 100% 배율의 960 x 540px(또는 HTML 앱의 경우 100% 배율의 1280 x 720px)에서 UI가 멋지게 표시되도록 합니다.

Xbox용 디자인은 1920 x 1080이라는 하나의 해상도만 고려하면 되기 때문에 PC용 디자인과 약간 다릅니다.
사용자에게 더 나은 해상도의 TV가 있는지 여부는 중요하지 않습니다. UWP 앱은 항상 1080p로 크기가 조정됩니다.

TV 해상도에 관계없이 Xbox One에서 실행되는 경우 200%(또는 HTML 앱의 경우 150%) 세트에서 올바른 자산 크기를 앱에 대해서도 가져옵니다.

### <a name="content-density"></a>콘텐츠 밀도

앱을 디자인할 때 사용자가 일정 거리에서 UI를 보고 리모컨이나 게임 컨트롤러를 사용하여 조작하기 때문에 마우스나 터치 입력을 사용할 때보다 이동 시간이 더 오래 걸린다는 것에 유의하세요.

#### <a name="sizes-of-ui-controls"></a>UI 컨트롤의 크기

대화형 UI 요소는 32epx(유효 픽셀)의 최소 높이로 크기를 조정해야 합니다. 이는 일반적인 UWP 컨트롤의 기본값이며, 200% 배율로 사용할 때 일정 거리에서 UI 요소를 볼 수 있게 하며 콘텐츠 밀도를 줄이는 데 도움이 됩니다.

![100% 및 200% 배율의 UWP 단추](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>클릭 수

UI를 간소화하려면 사용자가 TV 화면의 한쪽 가장자리에서 다른 쪽 가장자리로 이동할 때 **6번 클릭**을 넘지 않도록 해야 합니다. 여기서도 다시 **단순성**의 원칙이 적용됩니다. 

![가로로 6개 아이콘](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>텍스트 크기

일정 거리에서 UI를 볼 수 있게 하려면 다음과 같은 경험적 규칙을 따르세요.

* 기본 텍스트 및 읽기용 콘텐츠: 최소 15epx
* 중요하지 않은 텍스트 및 보충 콘텐츠: 최소 12epx

UI에 큰 텍스트를 사용하는 경우 다른 콘텐츠가 채울 수 있는 공간을 사용하여 화면 공간을 너무 제한하지 않는 크기를 선택합니다.

### <a name="opting-out-of-scale-factor"></a>배율 인수 옵트아웃

각 디바이스 유형에 맞게 조정하여 모든 디바이스에서 적절하게 실행되도록 하는 배율 인수 지원을 앱에서 활용하는 것이 좋습니다.
그러나 이 동작에서 옵트아웃하고 모든 UI를 100% 배율로 디자인할 수 있습니다. 100% 이외의 값으로 배율 인수를 변경할 수는 없습니다.

XAML 앱의 경우 다음 코드 조각을 사용하여 배율 인수를 옵트아웃(opt out)할 수 있습니다.

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

옵트아웃 (opt out) 했는지 여부를 알려 주는 `result`입니다.

HTML/JavaScript용 샘플 코드를 포함하여 자세한 내용은 [크기 조정을 끄는 방법](../../xbox-apps/disable-scaling.md)을 참조하세요.

이 항목에 설명된 *유효* 픽셀 값의 2배(또는 HTML 앱의 경우 1.5배)인 *실제* 픽셀 값을 구하여 UI 요소의 적절한 크기를 계산해야 합니다.

## <a name="tv-safe-area"></a>TV 안전 영역

기록 및 기술적 이유로 인해 일부 TV는 화면의 가장자리까지 콘텐츠를 표시하지 않습니다. 기본적으로 UWP는 안전하지 않은 TV 영역에 UI 콘텐츠가 표시되지 않도록 하고 페이지 배경만 그립니다.

안전하지 않은 TV 영역은 다음 이미지에서 파란색 영역으로 표시됩니다.

![안전하지 않은 TV 영역](images/designing-for-tv/tv-unsafe-area.png)

다음 코드 조각과 같이 배경을 정적 또는 테마 색이나 이미지로 설정할 수 있습니다.

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

추가 작업 없이 앱이 표시되는 모양입니다.

![TV 안전 영역](images/designing-for-tv/tv-safe-area.png)

지금은 탐색 창과 그리드 같은 UI 부분이 잘린 것처럼 보이는 "박스" 효과가 앱에 나타나기 때문에 최적 상태가 아닙니다. 그러나 화면 가장자리로 UI 부분을 확장하여 앱에 보다 영화 같은 효과를 주면 최적화할 수 있습니다.

### <a name="drawing-ui-to-the-edge"></a>가장자리까지 UI 그리기

사용자에게 더 몰입감을 제공하기 위해 특정 UI 요소를 사용하여 화면 가장자리까지 확장하는 것이 좋습니다. 여기에는 [ScrollViewers](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [탐색 창](../controls-and-patterns/navigationview.md), [CommandBars](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 등이 포함됩니다.

반면, 대화형 요소와 텍스트의 경우 항상 화면 가장자리를 피하여 일부 TV에서 잘리지 않도록 하는 것이 중요합니다. 화면 가장자리의 5% 내에는 중요하지 않은 시각 효과만 그리는 것이 좋습니다. [UI 요소 크기 지정](#ui-element-sizing)에 설명된 것처럼 Xbox One 콘솔의 기본 배율 인수 200%를 따르는 UWP 앱은 960 x 540 epx 영역을 이용하므로 앱의 UI에서 다음 영역에는 필수 UI를 배치하지 않도록 해야 합니다.

- 위쪽 및 아래쪽에서 27epx
- 왼쪽 및 오른쪽에서 48epx

다음 섹션에서는 UI를 화면 가장자리까지 확장하는 방법을 설명합니다.

#### <a name="core-window-bounds"></a>핵심 창 경계

3m 환경만을 대상으로 하는 UWP 앱의 경우 핵심 창 경계를 사용하는 것이 더 간단한 옵션입니다.

`OnLaunched`의 `App.xaml.cs` 메서드에 다음 코드를 추가합니다.

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

이 코드 줄을 사용하면 앱 창이 화면 가장자리까지 확장되므로 모든 대화형 필수 UI를 앞에서 설명한 TV 안전 영역으로 이동해야 합니다. 상황에 맞는 메뉴, 열린 [ComboBoxes](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 등의 임시 UI는 TV 안전 영역 안에 자동으로 유지됩니다.

![핵심 창 경계](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>창 배경

탐색 창은 일반적으로 화면 가장자리 근처에 그려지므로 어색한 간격이 생기지 않도록 배경이 안전하지 않은 TV 영역까지 확장되어야 합니다. 이렇게 하려면 탐색 창의 배경색을 앱의 배경색으로 변경합니다.

앞에서 설명한 대로 핵심 창 경계를 사용하면 UI를 화면 가장자리까지 그릴 수 있지만 [SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView)의 콘텐츠에서 양수 여백을 사용하여 TV 안전한 영역 내에 유지해야 합니다.

![화면 가장자리까지 확장된 탐색 창](images/designing-for-tv/tv-safe-areas-2.png)

여기서는 탐색 항목을 TV 안전 영역에 유지하는 동시에 탐색 창의 배경이 화면 가장자리까지 확장되었습니다.
그리드 위쪽은 여전히 TV 안전 영역 내에 유지하는 동시에 연속된 것처럼 보이고 잘리지 않도록 `SplitView` 콘텐츠(이 경우 항목 그리드)가 화면 아래쪽까지 확장되었습니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 [목록 및 그리드의 스크롤 끝](#scrolling-ends-of-lists-and-grids)을 참조하세요.

다음 코드 조각을 사용하여 이 효과를 만들 수 있습니다.

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

[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)는 앱의 하나 이상 가장자리 근처에 자주 배치되는 창의 또 다른 예이므로 TV에서는 배경이 화면 가장자리까지 확장되어야 합니다. 또한 일반적으로 TV 안전 영역에 유지되어야 하는 **자세히** 단추(“...”로 표시됨)가 오른쪽에 있습니다. 다음은 원하는 조작 및 시각 효과를 얻기 위한 몇 가지 전략입니다.

**옵션 1**: `CommandBar` 배경색을 투명 또는 페이지 배경과 동일한 색으로 변경합니다.

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

이렇게 하면 `CommandBar`이 페이지의 나머지 부분과 동일한 배경 위에 있는 것처럼 표시되므로 배경이 화면 가장자리까지 매끄럽게 이어집니다.

**옵션 2**: 채우기가 `CommandBar` 배경과 동일한 색인 사각형을 추가하고 페이지의 나머지 부분에 걸쳐 `CommandBar` 아래에 표시되게 합니다.

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
> 이 방식을 사용하는 경우 **자세히** 단추는 필요에 따라 `CommandBar`의 레이블을 아이콘 아래에 표시하기 위해 열린 `AppBarButton`의 높이를 변경한다는 것에 유의하세요. 이러한 크기 조정을 방지하려면 레이블을 아이콘의 *오른쪽*으로 이동하는 것이 좋습니다. 자세한 내용은 [CommandBar 레이블](#commandbar-labels)을 참조하세요.

이러한 방법은 둘 다 이 섹션에 나열된 다른 유형의 컨트롤에도 적용됩니다.

#### <a name="scrolling-ends-of-lists-and-grids"></a>목록 및 그리드의 스크롤 끝

목록 및 그리드에는 동시에 화면에 들어가는 것보다 많은 항목이 포함되어 있는 경우가 많습니다. 이 경우 목록이나 그리드를 화면 가장자리까지 확장하는 것이 좋습니다. 가로로 스크롤되는 목록 및 그리드는 오른쪽 가장자리까지 확장되어야 하고, 세로로 스크롤되는 목록 및 그리드는 아래쪽까지 확장되어야 합니다.

![TV 안전 영역 그리드 잘림](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

목록이나 그리드가 이렇게 확장되는 경우에도 포커스 화면 효과 및 관련 항목은 TV 안전 영역 안에 유지하는 것이 중요합니다.

![스크롤되는 그리드 포커스가 TV 안전 영역에 유지되어야 함](images/designing-for-tv/scrolling-grid-focus.png)

UWP에는 포커스 화면 효과를 [VisibleBounds](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds) 안에 유지하는 기능이 있지만 안쪽 여백을 추가하여 목록/그리드 항목이 안전 영역의 보기로 스크롤될 수 있도록 해야 합니다. 구체적으로, 다음 코드 조각과 같이 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 또는 [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)의 [ItemsPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter)에 양수 여백을 추가합니다.

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

페이지 또는 앱 리소스에 이전 코드 조각을 배치한 후 다음과 같은 방법으로 액세스합니다.

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> 이 코드 조각은 `ListView`용으로 특별히 작성되었습니다. `GridView` 스타일의 경우 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) 및 [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 둘 다의 [TargetType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 특성을 `GridView`로 설정합니다.

항목이 표시 되는 방식을 보다 세부적으로 제어 하기 위해 응용 프로그램이 버전 1803 이상을 대상으로 하는 경우 [BringIntoViewRequested 이벤트](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)를 사용할 수 있습니다. 다음 코드 조각과 같이 내부 **ScrollViewer** 가 실행 되기 전에이를 catch 하기 위해 **ListView**/**GridView** 에 대 한 [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) 에이를 추가할 수 있습니다.

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

기본적으로 유니버설 Windows 플랫폼은 앱의 색상을 TV 안전 범위에 맞게 조정하여(자세한 내용은 [TV에 적합한 색 참조](#tv-safe-colors)) 앱이 어떤 TV에서도 자연스럽게 보이도록 합니다. 또한 TV의 시각적 환경을 개선하기 위해 앱에서 사용하는 색 집합을 향상할 수 있습니다.

### <a name="application-theme"></a>응용 프로그램 테마

앱에 적합한 테마에 따라 **응용 프로그램 테마**(어둡게 또는 밝게)를 선택하거나 테마를 옵트아웃할 수 있습니다. [색 테마](../style/color.md)에서 테마에 대한 일반적인 권장 사항을 읽어보세요.

UWP를 사용하면 앱이 실행되는 디바이스에서 제공하는 시스템 설정에 따라 테마를 동적으로 설정할 수도 있습니다.
UWP는 항상 사용자가 지정한 테마 설정을 따르지만 각 디바이스에서도 적절한 기본 테마를 제공합니다.
*생산성* 경험보다 *미디어* 경험이 더 많을 것으로 예상되는 Xbox One의 특성 때문에 기본적으로 어두운 시스템 테마로 설정됩니다.
앱의 테마가 시스템 설정을 기반으로 하는 경우 Xbox One에서는 기본적으로 어둡게 설정됩니다.

### <a name="accent-color"></a>테마 컬러

UWP는 사용자가 시스템 설정에서 선택한 **테마 컬러**를 표시하는 편리한 방법을 제공합니다.

Xbox One에서는 사용자가 PC에서 테마 컬러를 선택할 수 있는 것처럼 사용자 색을 선택할 수 있습니다.
앱에서 브러시 또는 색 리소스를 통해 이러한 테마 컬러를 호출하기만 하면 사용자가 시스템 설정에서 선택한 색이 사용됩니다. Xbox One의 테마 컬러는 시스템 단위가 아니라 사용자 단위로 설정됩니다.

또한 Xbox One의 사용자 색 집합은 PC, 휴대폰 및 기타 디바이스와 다릅니다.

앱에서 **SystemControlForegroundAccentBrush** 등의 브러시 리소스 또는 색 리소스(**SystemAccentColor**)를 사용하거나 [UIColorType.Accent*](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) API를 통해 직접 테마 컬러를 호출하기만 하면 해당 색이 Xbox One에 적합한 테마 컬러로 바뀝니다. 고대비 브러시 색도 PC 및 휴대폰과 동일한 방식으로 시스템에서 가져옵니다.

일반적인 테마 컬러에 대한 자세한 내용은 [테마 컬러](../style/color.md#accent-color)를 참조하세요.

### <a name="color-variance-among-tvs"></a>TV 간의 색 차이

TV용으로 디자인할 때는 렌더링되는 TV에 따라 색이 완전히 다르게 표시된다는 것에 유의하세요. 색이 모니터와 동일하게 표시될 것이라고 가정하지 마세요. 앱이 색의 미묘한 차이를 사용하여 UI 부분을 구분하는 경우 색이 혼합되어 사용자가 혼동할 수 있습니다. 사용하는 TV에 관계없이 사용자가 명확하게 구분할 수 있을 만큼 다른 색을 사용합니다.

### <a name="tv-safe-colors"></a>TV에 적합한 색

색의 RGB 값은 빨강, 녹색 및 파랑의 농도를 나타냅니다. TV는 극단적인 농도를 제대로 처리하지 못하기 때문에 특정 TV에서 이상한 밴드 효과를 생성하거나 흐리게 표시될 수 있습니다. 또한 고농도 색을 사용할 경우 블루밍(주변 픽셀이 동일한 색을 그리기 시작)이 발생할 수도 있습니다. TV에 적합한 색에 대해서는 의견이 분분하지만, 일반적으로 RGB 값 16-235(또는 16진수에서 10-EB) 내의 색이 TV에 사용하기에 적합합니다.

![TV에 적합한 색 범위](images/designing-for-tv/tv-safe-colors-2.png)

지금까지 Xbox 앱은 색상을 "TV에 적합한" 색 범위에서 벗어나지 않도록 맞추어야 했습니다. 하지만 Fall Creators Update부터는 Xbox One이 전체 범위 콘텐츠를 TV에 적합한 범위에 맞게 자동으로 조정합니다. 이 말은 대부분 앱 개발자들이 더 이상 TV에 적합한 색에 대해서 걱정할 필요가 없다는 것을 의미합니다.

> [!IMPORTANT]
> 이미 TV에 적합한 색 범위에 포함되는 비디오 콘텐츠는 [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)을 사용해 재생해도 이러한 색 조정 효과가 적용되지 않습니다.

DirectX 11 또는 DirectX 12를 사용해 앱을 개발하면서 사용자 고유의 스왑 체인을 만들어 UI 또는 비디오를 렌더링하는 경우에는 [IDXGISwapChain3::SetColorSpace1](https://docs.microsoft.com/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1)을 호출하여 사용할 색 공간을 지정할 수 있습니다. 그러면 시스템이 색 조정 여부를 알 수 있습니다.

## <a name="guidelines-for-ui-controls"></a>UI 컨트롤에 대한 지침

여러 UI 컨트롤이 다양한 디바이스에서 잘 작동하지만 TV에서 사용할 경우 특별히 고려해야 할 사항이 있습니다. 3m 환경용으로 디자인할 경우 이러한 컨트롤 사용을 위한 몇 가지 모범 사례에 대해 알아봅니다.

### <a name="pivot-control"></a>피벗 컨트롤

[피벗](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)을 사용하면 다양한 헤더 또는 탭을 선택하여 앱 내에서 보기를 빠르게 탐색할 수 있습니다. 이 컨트롤은 포커스가 있는 헤더에 밑줄을 표시하여 게임 패드/리모컨을 사용할 때 현재 선택된 헤더를 보다 명확하게 나타냅니다.

![피벗 밑줄](images/designing-for-tv/pivot-underline.png)

선택한 피벗 헤더가 항상 첫 번째 위치로 이동하도록 하는 대신 피벗이 항상 동일한 위치를 유지하도록 [Pivot.IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty) 속성을 `true`로 설정할 수 있습니다. TV와 같은 큰 화면 표시에서는 헤더 래핑이 사용자에게 방해가 될 수 있으므로 환경이 개선됩니다. 모든 피벗 헤더가 한 번에 화면에 들어가지 않는 경우 고객이 다른 헤더를 볼 수 있도록 스크롤 막대가 표시됩니다. 그러나 최상의 환경을 제공하려면 모두 화면에 들어가도록 해야 합니다. 자세한 내용은 [탭 및 피벗](/windows/uwp/design/controls-and-patterns/pivot)을 참조하세요.

### <a name="navigation-pane-a-namenavigation-pane-"></a>탐색 창 <a name="navigation-pane" />

탐색 창(*햄버거 메뉴*라고도 함)은 UWP 앱에서 일반적으로 사용되는 탐색 컨트롤입니다. 주로 목록 스타일 메뉴에서 선택하며 사용자를 다른 페이지로 이동하는 여러 옵션이 있는 창입니다. 일반적으로 이 창은 공간을 절약하기 위해 축소된 형태로 시작되며 사용자가 단추를 클릭하여 열 수 있습니다.

탐색 창은 마우스와 터치를 사용한 접근성이 높은 반면, 사용자가 창을 여는 단추로 이동해야 하기 때문에 게임 패드/리모컨의 접근성은 낮습니다. 따라서 **보기** 단추를 통해 탐색 창을 여는 것뿐 아니라 사용자가 페이지의 왼쪽으로 이동하여 창을 열 수 있도록 하는 것이 좋습니다. 이 디자인 패턴을 구현하는 방법에 대한 코드 샘플은 [프로그래밍 방식 포커스 탐색](../input/focus-navigation-programmatic.md#split-view-code-sample) 문서에서 찾을 수 있습니다. 이렇게 하면 사용자가 창의 내용에 쉽게 액세스할 수 있습니다. 탐색 창이 다양한 화면 크기에서 동작하는 방식 및 게임 패드/리모컨 탐색 모범 사례에 대한 자세한 내용은 [탐색 창](../controls-and-patterns/navigationview.md)을 참조하세요.

### <a name="commandbar-labels"></a>CommandBar 레이블

높이가 최소화되고 일관성이 유지되도록 [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)에서 아이콘의 오른쪽에 레이블을 배치하는 것이 좋습니다. 이렇게 하려면 [CommandBar.DefaultLabelPosition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) 속성을 `CommandBarDefaultLabelPosition.Right`로 설정합니다.

![아이콘의 오른쪽에 레이블이 있는 CommandBar](images/designing-for-tv/commandbar.png)

이 속성을 설정하면 레이블이 항상 표시되므로 사용자의 클릭 횟수를 최소화하기 때문에 3m 환경에도 적합합니다. 다른 디바이스 유형에도 적용할 수 있는 멋진 모델이기도 합니다.

### <a name="tooltip"></a>도구 설명

[Tooltip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip) 컨트롤은 사용자가 요소를 마우스로 가리키거나 요소를 손가락으로 길게 누 때 UI에 자세한 정보를 제공하는 방법으로 도입되었습니다. 게임 패드 및 리모컨의 경우 요소가 포커스를 받을 때 잠시 후에 `Tooltip`이 나타나고 짧은 시간 동안 화면에 있다가 사라집니다. `Tooltip`을 너무 많이 사용할 경우 이 동작이 방해가 될 수 있습니다. TV용으로 디자인할 때는 `Tooltip`을 사용하지 않도록 합니다.

### <a name="button-styles"></a>단추 스타일

표준 UWP 단추는 TV에서 잘 작동하지만 단추의 일부 시각적 스타일은 UI를 더 명확하게 표시하므로 모든 플랫폼, 특히 포커스가 있는 위치를 명확하게 전달하면 도움이 되는 3m 환경에서 고려하는 것이 좋습니다. 이러한 스타일에 대한 자세한 내용은 [단추](../controls-and-patterns/buttons.md)를 참조하세요.

### <a name="nested-ui-elements"></a>중첩된 UI 요소

중첩된 UI는 중첩된 항목과 컨테이너 항목이 각각 독립적인 포커스를 가질 수도 있는 컨테이너 UI 요소 내에 중첩된 실행 가능한 항목을 묶어 표시합니다.

중첩된 UI는 일부 입력 유형과 잘 작동하지만 XY 탐색을 사용하는 게임 패드 및 원격에 대해서는 작동하지 않는 경우도 있습니다. UI가 3m 환경에 최적화되고 조작 가능한 모든 요소에 사용자가 쉽게 액세스할 수 있도록 이 항목의 지침을 수행해야 합니다. 한 가지 일반적인 해결 방법은 중첩 된 UI 요소를 `ContextFlyout`에 넣는 것입니다.

중첩된 UI에 대한 자세한 내용은 [목록 항목의 중첩된 UI](../controls-and-patterns/nested-ui.md)를 참조하세요.

### <a name="mediatransportcontrols"></a>MediaTransportControls

[MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 요소를 사용하면 사용자가 재생, 일시 중지, 선택 자막 켜기 등을 수행할 수 있는 기본 재생 환경을 통해 미디어를 조작할 수 있습니다. 이 컨트롤은 [MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)의 속성으로, 두 가지 레이아웃 옵션 즉, *단일 행* 및 *이중 행*을 지원합니다. 단일 행 레이아웃에서는 슬라이더 및 재생 단추가 모두 한 행에 있으며, 재생/일시 중지 단추가 슬라이더의 왼쪽에 있습니다. 이중 행 레이아웃에서는 슬라이더가 고유한 행을 차지하며, 재생 단추가 별도의 하위 행에 있습니다. 3m 환경용으로 디자인할 경우 이중 행 레이아웃이 게임 패드에 더 나은 탐색을 제공하므로 이 레이아웃을 사용해야 합니다. 이중 행 레이아웃을 사용하려면 `IsCompact="False"`의 `MediaTransportControls`TransportControls[ 속성에서 ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) 요소에 대해 `MediaPlayerElement`를 설정합니다.

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

앱에 미디어를 추가하는 방법에 대한 자세한 내용은 [미디어 재생](../controls-and-patterns/media-playback.md) 항목을 참조하세요.

> ![참고] `MediaPlayerElement`는 Windows&nbsp;10, 1607 이상 버전에서만 사용할 수 있습니다. 이전 버전의 Windows&nbsp;10 앱을 개발하는 경우 [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)를 대신 사용해야 합니다. 위 권장 사항은 `MediaElement`에도 적용되며, `TransportControls` 속성은 동일한 방식으로 액세스됩니다.

### <a name="search-experience"></a>검색 환경

콘텐츠 검색은 3m 환경에서 가장 일반적으로 수행되는 기능 중 하나입니다. 앱에서 검색 환경을 제공하는 경우 사용자는 게임 패드의 **Y** 단추를 바로 가기로 사용하여 빠르게 액세스할 수 있습니다.

대부분의 고객은 이 바로 가기에 대해 이미 잘 알고 있지만, 원하는 경우 고객이 버튼을 사용하여 검색 기능에 액세스할 수 있음을 나타내기 위해 UI에 **Y** 문자 모양을 시각적으로 추가할 수 있습니다. 이 신호를 추가하는 경우 Xbox 셸 및 다른 앱과의 일관성이 제공되도록 **Segoe Xbox MDL2 Symbol** 글꼴(XAML 앱의 경우 `&#xE3CC;`, HTML 앱의 경우 `\E426`)의 기호를 사용해야 합니다.

> [!NOTE]
> **Segoe Xbox MDL2 Symbol** 글꼴은 Xbox에서만 사용할 수 있기 때문에 PC에서는 기호가 제대로 표시되지 않습니다. 그러나 Xbox에 배포하고 나면 TV에서 표시됩니다.

**Y** 버튼은 게임 패드에서만 사용할 수 있기 때문에 UI의 단추와 같은, 검색 기능에 액세스하는 다른 방법을 제공해야 합니다. 그렇지 않으면 일부 고객의 경우 기능을 사용하지 못할 수 있습니다.

3m 환경에서는 디스플레이에 제한된 공간이 있으므로 경우에 따라 고객이 전체 화면 검색 환경을 사용하는 것이 더 쉽습니다. 전체 화면인지, 부분 화면인지에 상관없이 “내부" 검색 시 사용자가 검색 환경을 열 때 화상 키보드가 이미 열려 있는 상태로 나타나 고객이 검색어를 입력할 수 있도록 하는 것이 좋습니다.

## <a name="custom-visual-state-trigger-for-xbox"></a>Xbox에 대한 사용자 지정 시각적 상태 트리거

3m 환경에 맞게 UWP 앱을 조정하려면 앱이 Xbox 콘솔에서 시작된 것을 감지할 때 레이아웃을 변경하는 것이 좋습니다. 이 작업을 수행하는 한 가지 방법은 사용자 지정 *시각적 상태 트리거*를 사용하는 것입니다. 시각적 상태 트리거는 **Blend for Visual Studio**에서 편집하려는 경우에 가장 유용합니다. 다음 코드 조각은 Xbox에 대한 시각적 상태 트리거를 만드는 방법을 보여 줍니다.

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

트리거를 만들려면 다음 클래스를 앱에 추가합니다. 위의 XAML 코드에서 참조한 클래스입니다.

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

사용자 지정 트리거를 추가하면 앱은 Xbox One 콘솔에서 실행되는 것을 감지할 때마다 XAML 코드에 지정된 대로 레이아웃을 자동으로 수정합니다.

앱이 Xbox에서 실행되고 있는지 확인하고 적절하게 조정하는 또 다른 방법은 코드를 사용하는 것입니다. 다음과 같은 간단한 변수를 사용하여 앱이 Xbox에서 실행되고 있는지 확인할 수 있습니다.

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

그런 다음 이 검사 후의 코드 블록에서 UI를 적절하게 조정할 수 있습니다. 

## <a name="summary"></a>요약

3m 환경용으로 디자인할 경우 다른 플랫폼용으로 디자인할 때와 달라야 함을 고려해야 합니다. UWP 앱을 Xbox One에 직접 포팅할 수 있고 실제로 작동하기도 하지만 이 방식은 3m 환경용으로 최적화되지는 않았으며 이로 인해 사용자가 불만을 느낄 수 있습니다. 앱이 TV에서도 정상일 수 있도록 이 문서의 지침을 따르세요.

## <a name="related-articles"></a>관련 문서

- [UWP (유니버설 Windows 플랫폼 용 장치 입문) 앱](index.md)
- [게임 패드 및 리모컨 조작](../input/gamepad-and-remote-interactions.md)
- [UWP 앱의 소리](../style/sound.md)
