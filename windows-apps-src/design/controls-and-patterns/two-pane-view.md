---
Description: TwoPaneView는 서로 다른 2개의 콘텐츠 영역이 있는 앱의 디스플레이를 관리하는 데 도움이 되는 레이아웃 컨트롤입니다.
title: 2 창 보기
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67b97aec970cc655700729743f10c63c666ab0a6
ms.sourcegitcommit: 09571e1c6a01fabed773330aa7ead459a47d94f7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929257"
---
# <a name="two-pane-view"></a>2 창 보기

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview)는 마스터/세부 정보 보기처럼 서로 다른 2개의 콘텐츠 영역이 있는 앱의 디스플레이를 관리하는 데 도움이 되는 레이아웃 컨트롤입니다.

> [!IMPORTANT]
> 이 문서에서 설명하는 기능 및 지침은 공개 미리 보기 상태이며 일반적으로 공급되기 전에 대대적으로 수정될 수 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

TwoPaneView 컨트롤은 모든 Windows 디바이스에서 작동하는 동안 특수한 코딩 없이도 이중 화면 디바이스를 자동으로 활용할 수 있도록 설계되었습니다. 이중 화면 디바이스에서 나란히 보기는 UI(사용자 인터페이스)가 두 화면 사이의 간격을 스패닝할 때 콘텐츠가 각 화면에 표시되도록 깔끔하게 분할됩니다.

> **참고:** _이중 화면 디바이스_는 고유한 기능을 제공하는 특수한 종류의 디바이스입니다. 여러 모니터가 있는 데스크톱 디바이스와는 다릅니다. 이중 화면 디바이스에 대한 자세한 내용은 [이중 화면 디바이스 소개](/dual-screen/introduction)를 참조하세요.
>
>이 문서에서 사용된 _단일 화면 디바이스_ 또는 _단일 화면 디스플레이_라는 용어는 단일 모니터이든, 다중 모니터로 구성된 모니터이든 관계없이 이중 화면 디바이스가 아닌 모든 디바이스를 의미합니다. TwoPaneView 컨트롤은 다른 XAML 컨트롤과 동일한 방식으로 단일 화면에서 동작합니다. 여러 모니터에 맞게 앱을 최적화하는 방법에 대한 자세한 내용은 [여러 보기 표시](/windows/uwp/design/layout/show-multiple-views)를 참조하세요.

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 UWP 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되었습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리 개요](/uwp/toolkits/winui/)를 참조하세요. |

| **플랫폼 API** | **Windows UI 라이브러리 API** |
| - | - |
| [TwoPaneView 클래스](/uwp/api/windows.ui.xaml.controls.twopaneview) | [TwoPaneView 클래스](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

이 문서 전체에서 XAML의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 다음을 [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 요소에 추가했습니다.

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

코드 숨김에서는 C#의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 다음 **using** 문을 파일 맨 위에 추가했습니다.

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

2개의 개별 콘텐츠 영역이 있는 경우 나란히 보기를 사용합니다. 그리고

- 콘텐츠가 자동으로 다시 정렬되고 창에 잘 맞게 크기가 조정되어야 합니다.
- 여유 공간에 따라 콘텐츠의 보조 영역이 표시되거나 숨겨져야 합니다.
- 콘텐츠가 이중 화면 디바이스의 2개 화면에 깔끔하게 분할되어야 합니다.

## <a name="examples"></a>예

이러한 이미지는 단일 화면 디바이스 및 이중 화면 디바이스에서 실행되는 앱을 보여줍니다. 나란히 보기는 앱 UI를 각 디바이스의 다양한 영역 구성에 맞게 조정합니다.

![tpv-single.png](images/two-pane-view/tpv-single.png)

_단일 화면 디바이스의 앱_

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

_와이드 모드에서 이중 화면 디바이스를 스패닝하는 앱_

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

_톨 모드에서 이중 화면 디바이스를 스패닝하는 앱_

## <a name="how-it-works"></a>작동 방식

나란히 보기에는 콘텐츠가 표시되는 2개의 영역이 있습니다. 창의 여유 공간에 따라 영역의 크기와 배열이 조정됩니다. 가능한 영역 레이아웃은 [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) 열거에 의해 정의됩니다.

- **SinglePane** - [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 속성에 지정된 대로 1개 영역만 표시됩니다.
- **Wide** - [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) 속성에 지정된 대로 영역이 가로로 표시되거나 단일 영역이 표시됩니다.
- **Tall** - [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) 속성에 지정된 대로 영역이 세로로 표시되거나 단일 영역이 표시됩니다.

1개 영역만 사용할 수 있는 공간이 있을 때 표시되는 영역을 지정하려면 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority)를 설정하여 나란히 보기를 구성합니다. 그런 다음, Pane1을 톨 창의 위쪽 또는 아래쪽에 표시할지, 와이드 창의 왼쪽 또는 아래쪽에 표시할지 지정합니다.

영역의 크기와 배열은 나란히 보기에서 자동으로 처리하지만 영역 내부의 콘텐츠는 크기 및 방향 변경에 맞게 사용자가 직접 조정해야 합니다. 적응형 UI를 만드는 방법에 대한 자세한 내용은 [XAML을 사용하는 응답성이 뛰어난 레이아웃](/windows/uwp/design/layout/layouts-with-xaml) 및 [레이아웃 패널](/windows/uwp/design/layout/layout-panels)을 참조하세요.

나란히 보기는 앱이 실행되는 디바이스 유형에 따라 영역의 표시를 관리합니다.

- 이중 화면 디바이스

    나란히 보기는 UI를 이중 화면 디바이스에 맞게 손쉽게 최적화할 수 있도록 설계되었습니다. 창 크기는 화면의 여유 공간을 모두 사용하도록 조정됩니다. 앱이 디바이스 화면 중 하나에만 표시될 경우 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 속성에 지정된 대로 1개 영역이 표시됩니다.

    앱이 이중 화면 디바이스의 두 화면을 스패닝하는 경우 각 화면이 두 영역 중 하나의 콘텐츠를 표시하고 두 화면 사이의 간격에 콘텐츠를 적절히 스패닝합니다. 나란히 보기를 사용하는 경우 스패닝 인식 기능이 기본 제공됩니다. 톨/와이드 구성을 설정하여 어느 영역이 어느 화면에 표시되는지만 지정하면 됩니다. 나머지는 나란히 보기에서 자동으로 처리합니다.


- 단일 화면 디바이스

    앱이 노트북 또는 데스크톱 PC와 같은 단일 화면 디바이스에서 실행되는 경우 나란히 보기가 XAML 컨트롤에서 올바른 동작을 지원합니다. 창의 크기가 조정되면 나란히 보기가 창 크기에 따라 영역 크기와 위치를 조정합니다. 앱이 크기 조정이 가능한 창에서 단일 화면에 표시될 때 컨트롤의 동작을 정의하기 위해 설정하는 추가 속성이 있습니다. 이러한 속성은 다음 섹션에서 자세히 설명합니다.

## <a name="how-to-use-the-two-pane-view-control"></a>나란히 보기 컨트롤을 사용하는 방법

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview)는 페이지 레이아웃의 루트 요소가 아니어도 됩니다. 실제로 이는 앱을 전체적으로 탐색하는 데 사용되는 [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) 컨트롤 내에서 사용하는 경우가 많습니다. TwoPaneView는 XAML 트리의 어디에 있든 관계없이 적절히 조정되지만 TwoPaneView를 다른 TwoPaneView 안에 중첩하지 않는 것이 좋습니다.

### <a name="add-content-to-the-panes"></a>영역에 콘텐츠 추가

나란히 보기의 각 영역에는 단일 XAML UIElement가 포함될 수 있습니다. 콘텐츠를 추가하려면 일반적으로 각 영역에 XAML 레이아웃 패널을 배치한 다음, 다른 컨트롤과 콘텐츠를 패널에 추가합니다. 영역은 크기가 변경될 수 있고 와이드 모드와 톨 모드 간에 전환될 수 있으므로 각 영역의 콘텐츠가 이러한 변경에 맞게 조정될 수 있는지 확인해야 합니다. 적응형 UI를 만드는 방법에 대한 자세한 내용은 [XAML을 사용하는 응답성이 뛰어난 레이아웃](/windows/uwp/design/layout/layouts-with-xaml) 및 [레이아웃 패널](/windows/uwp/design/layout/layout-panels)을 참조하세요.

이 예시에서는 여기에 표시된 간단한 그림/정보 앱 UI를 만듭니다. 2개 영역에 맞는 공간이 있으면 그림 및 정보가 별도의 영역에 표시됩니다. (한 영역에 맞는 공간만 있다면 Pane2의 콘텐츠를 Pane1로 이동하고 사용자가 화면을 스크롤하여 숨겨진 콘텐츠를 보도록 합니다. 이에 대한 코드는 나중에 _모드 변경에 응답_ 섹션에서 볼 수 있습니다.

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
```

### <a name="specify-which-pane-to-display"></a>표시할 영역 지정

나란히 보기가 단일 영역만 표시할 수 있는 경우 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 속성을 사용하여 표시할 영역을 결정합니다. 기본적으로 PanePriority는 **Pane1**로 설정됩니다. XAML 또는 코드에서 이 속성을 설정하는 방법은 다음과 같습니다.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>영역 크기 조정

단일 화면 디바이스의 영역 크기는 [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) 및 [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) 속성에 따라 결정됩니다. 이는 _자동_ 및 _배율_(\*) 크기 조정을 지원하는 [GridLength](/uwp/api/windows.ui.xaml.gridlength) 값을 사용합니다. 자동 및 배율 크기 조정에 대한 설명은 [XAML을 사용하는 응답성이 뛰어난 레이아웃](/windows/uwp/design/layout/layouts-with-xaml#layout-properties)의 _레이아웃 속성_ 섹션을 참조하세요.

기본적으로 Pane1Length는 **Auto**로 설정되고 해당 콘텐츠에 맞게 크기가 조정됩니다. Pane2Length는 *로 설정되며 나머지 공간을 모두 사용합니다.

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

_기본 크기 조정을 사용하는 영역_

기본값은 Pane1에 항목 목록이 있고 Pane2에 많은 세부 정보가 있는 일반적인 마스터/세부 정보 레이아웃에 유용합니다. 그러나 콘텐츠에 따라 공간을 다르게 나눌 수 있습니다. 여기서 Pane1Length는 2*로 설정되어 있으므로 Pane2보다 2배 더 큰 공간을 차지합니다.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

_크기가 2* 및 *인 영역_

> **참고:** 앞서 언급했듯이 이중 화면 디바이스에서는 이러한 속성이 무시되고 디바이스 _상태_에 따라 자동으로 영역의 크기가 조정되고 정렬됩니다.

자동 크기 조정을 사용하도록 영역을 설정하면 영역의 콘텐츠가 포함되는 패널의 높이와 너비를 설정하여 크기를 제어할 수 있습니다. 이 경우 ModeChanged 이벤트를 처리하고 콘텐츠의 높이 및 너비 제약 조건을 현재 모드에 맞게 설정해야 할 수 있습니다.

### <a name="display-in-wide-or-tall-mode"></a>와이드 또는 톨 모드로 표시

데스크톱 디스플레이에서 나란히 보기의 표시 [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode)는 [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) 및 [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) 속성에 따라 결정됩니다. 두 속성의 기본값은 641px이며 [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth)와 동일합니다.

창이

- MinWideModeWidth보다 넓다면 **Wide** 모드가 사용됩니다.
- MinWideModeWidth보다 좁고 MinTallModeHeight보다 길다면 **Tall** 모드가 사용됩니다.
- MinWideModeWidth보다 좁고 MinTallModeHeight보다 짧다면 **SinglePane** 모드가 사용됩니다.

> **참고:** 앞서 언급했듯이 이중 화면 디바이스에서는 이러한 속성이 무시되고 디바이스 _상태_에 따라 자동으로 영역의 크기가 조정되고 정렬됩니다.

#### <a name="wide-configuration-options"></a>와이드 구성 옵션

MinWideModeWidth 속성보다 넓은 단일 디스플레이가 있는 경우 나란히 보기가 와이드 모드로 전환됩니다. MinWideModeWidth는 나란히 보기가 와이드 모드로 전환되는 시점을 제어합니다. 기본값은 641px이지만 원하는 대로 변경할 수 있습니다. 일반적으로 이 속성은 원하는 영역의 최소 너비로 설정해야 합니다.

나란히 보기가 와이드 모드인 경우 WideModeConfiguration 속성에 따라 표시되는 영역이 결정됩니다.

- **SinglePane** - 단일 영역(PanePriority에 지정된 값). 영역이 TwoPaneView의 전체 크기로 표시됩니다(즉, 양방향으로 배율에 따라 조정됨).
- **LeftRight** - Pane1은 왼쪽/Pane2는 오른쪽. 두 영역이 세로로 배율에 따라 조정되고 Pane1의 너비는 자동으로 조정되며 Pane2의 너비는 배율에 따라 조정됩니다.
- **RightLeft** - Pane1은 오른쪽/Pane2는 왼쪽. 두 영역이 세로로 배율에 따라 조정되고 Pane2의 너비는 자동으로 조정되며 Pane1의 너비는 배율에 따라 조정됩니다.

기본 설정은 **LeftRight**입니다.

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **팁:** 디바이스가 오른쪽에서 왼쪽(RTL) 언어를 사용하는 경우 나란히 보기가 순서를 자동으로 바꿉니다. 즉, RightLeft는 LeftRight로 렌더링되고 LeftRight는 RightLeft로 렌더링됩니다.

#### <a name="tall-configuration-options"></a>톨 구성 옵션

MinWideModeWidth보다 좁고 MinTallModeHeight보다 긴 단일 디스플레이가 있는 경우 나란히 보기가 톨 모드로 전환됩니다. 기본값은 641px이지만 원하는 대로 변경할 수 있습니다. 일반적으로 이 속성은 원하는 영역의 최소 높이로 설정해야 합니다.

나란히 보기가 와이드 모드인 경우 TallLayout 속성에 따라 표시되는 영역이 결정됩니다.

- **SinglePane** - 단일 영역(PanePriority에 지정된 값). 영역이 TwoPaneView의 전체 크기로 표시됩니다(즉, 양방향으로 배율에 따라 조정됨).
- **TopBottom** - 위쪽의 Pane1/아래쪽의 Pane2. 두 영역이 가로로 배율에 따라 조정되고 Pane1의 높이는 자동으로 조정되며 Pane2의 높이는 배율에 따라 조정됩니다.
- **BottomTop** - 아래쪽의 Pane1/위쪽의 Pane2. 두 영역이 가로로 배율에 따라 조정되고 Pane2의 높이는 자동으로 조정되며 Pane1의 높이는 배율에 따라 조정됩니다.

기본값은 **TopBottom**입니다.

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>MinWideModeWidth 및 MinTallModeHeight의 특수 값

MinWideModeWidth 속성을 사용하여 나란히 보기가 와이드 모드로 전환되지 않도록 할 수 있습니다. MinWideModeWidth를 [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0)로 설정하기만 하면 됩니다.

MinTallModeHeight를 [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0)로 설정하면 나란히 보기가 톨 모드로 전환되지 않습니다.

MinTallModeHeight를 0으로 설정하면 나란히 보기가 SinglePane 모드로 전환되지 않습니다.

#### <a name="responding-to-mode-changes"></a>모드 변경에 대한 응답

읽기 전용 [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) 속성을 사용하여 현재 표시 모드를 가져올 수 있습니다. 나란히 보기가 표시하는 영역이 변경될 때마다 업데이트된 콘텐츠를 렌더링하기 전에 [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) 이벤트가 발생합니다. 이벤트를 처리하여 표시 모드 변경에 응답할 수 있습니다.

이 이벤트를 사용할 수 있는 한 가지 방법은 사용자가 SinglePane 모드에서 모든 콘텐츠를 볼 수 있도록 앱의 UI를 업데이트하는 것입니다. 예를 들어 예시 앱에는 기본 영역(이미지) 및 정보 영역이 있습니다.

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

_와이드 모드_

하나의 영역을 표시할 공간만 있는 경우 사용자가 화면을 스크롤하면 모든 콘텐츠를 볼 수 있도록 Pane2의 콘텐츠를 Pane1로 이동합니다. 모양은 다음과 같습니다.

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

_SinglePane 모드_

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 앱이 이중 디스플레이 및 대형 화면을 활용할 수 있도록 항상 나란히 보기를 사용합니다.
- 나란히 보기를 다른 나란히 보기 안에 배치하지 않습니다.

## <a name="related-articles"></a>관련된 문서

- [레이아웃 개요](../layout/index.md)
- [이중 화면 개발](/dual-screen)
- [이중 화면 디바이스 소개](/dual-screen/introduction)
