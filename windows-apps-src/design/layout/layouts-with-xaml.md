---
description: 자동 크기 조정, 레이아웃 패널, 시각적 상태 및 별도의 UI 정의를 지원하는 XAML의 유연한 레이아웃 시스템을 사용하여 반응형 UI를 만드는 방법을 알아봅니다.
title: XAML을 사용한 반응형 레이아웃
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: contperf-fy21q2
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: e2bc29acc63a5363891ad78b873db6c8311ac970
ms.sourcegitcommit: 06d59b59a95aad009acb947a0dac7432116bdb60
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100544653"
---
# <a name="responsive-layouts-with-xaml"></a>XAML을 사용한 반응형 레이아웃

XAML 레이아웃 시스템은 반응형 UI를 만드는 데 도움이 되도록 요소, 레이아웃 패널 및 시각적 상태의 자동 크기 조정을 제공합니다. 반응형 레이아웃을 사용하면 다양한 앱 창 크기, 해상도, 픽셀 밀도 및 방향의 화면에서 앱의 모양을 멋지게 표현할 수 있습니다. 또한 XAML을 사용하면 [반응형 디자인 기술](responsive-design.md)에서 설명한 것처럼 앱 UI의 위치 및 크기 조정, 재배치, 표시/숨기기, 변경 또는 재설계가 가능합니다. 여기에서는 XAML을 사용하여 반응형 레이아웃을 구현하는 방법에 대해서 설명하겠습니다.

## <a name="fluid-layouts-with-properties-and-panels"></a>속성 및 패널을 통한 유동 레이아웃

반응형 레이아웃의 기초는 콘텐츠의 유동적인 위치 변경, 크기 조정 및 재배치를 위해 XAML 레이아웃 속성 및 패널을 적절하게 사용하는 것입니다.

XAML 레이아웃 시스템은 고정 레이아웃과 가변 레이아웃을 모두 지원합니다. 고정 레이아웃에서는 컨트롤에 정확한 픽셀 크기와 위치를 제공합니다. 사용자가 디바이스의 해상도나 방향을 변경해도 UI가 바뀌지 않습니다. 고정 레이아웃은 다양한 폼 요소 및 디스플레이 크기에서 잘릴 수 있습니다. 반면에 유동 레이아웃은 디바이스에서 사용할 수 있는 가시 공간에 맞게 축소, 확대 또는 재배치됩니다.

실제로는 고정 및 가변 요소의 조합을 사용하여 UI를 만듭니다. 일부 위치에서 여전히 고정 요소와 값을 사용할 수 있지만, 전체 UI는 해상도, 화면 크기 및 보기에 따라 반응할 수 있도록 해야 합니다.

여기서는 XAML 속성 및 레이아웃 패널을 사용하여 유동 레이아웃을 만드는 방법을 살펴봅니다.

### <a name="layout-properties"></a>레이아웃 속성
레이아웃 속성은 요소의 크기 및 위치를 제어합니다. 유동 레이아웃을 만들려면 요소에 대해 자동 또는 가변 크기 조정을 사용하고 레이아웃 패널에서 필요에 따라 자식의 위치를 지정하도록 허용합니다.

다음은 몇 가지 공통 레이아웃 속성과 이러한 속성을 사용하여 유동 레이아웃을 만드는 방법입니다.

**Height 및 Width**

[**Height**](/uwp/api/windows.ui.xaml.frameworkelement.height) 및 [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.width) 속성은 요소의 크기를 지정합니다. 유효 픽셀로 측정된 고정 값을 사용하거나 자동 또는 가변 크기 조정을 사용할 수 있습니다.

자동 크기 조정은 해당 콘텐츠 또는 부모 컨테이너에 맞게 UI 요소의 크기를 조정합니다. 그리드의 행과 열에서 자동 크기 조정을 사용할 수도 있습니다. 자동 크기 조정을 사용하려면 UI 요소의 Height 및/또는 Width를 **Auto** 로 설정합니다.

> [!NOTE]
> 요소의 크기가 콘텐츠 또는 컨테이너 중에서 어떤 것에 맞게 조정될지는 부모 컨테이너가 자식의 크기 조정을 처리하는 방식에 따라 결정됩니다. 자세한 내용은 이 문서의 뒷부분에 나오는 [레이아웃 패널](#layout-panels)을 참조하세요.

*배율 크기 조정* 이라고도 하는 가변 크기 조정은 가중 비율에 따라 그리드의 행과 열 사이에서 사용 가능한 공간을 분배합니다. XAML에서는 배율 값이 \*\*(또는 가중 배율 크기 조정의 경우 *n*\*)로 표현됩니다. 예를 들어 2열 레이아웃에서 첫 번째 열을 두 번째 열보다 5배 너비로 지정하려면 [**ColumnDefinition**](/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition) 요소에서 [**Width**](/uwp/api/windows.ui.xaml.controls.columndefinition.width) 속성에 대해 "5\*" 및 "\*"를 사용합니다.

이 예제에서는 4개의 열을 가진 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)에서 고정, 자동, 가변 크기 조정을 조합합니다.

| 열 | 크기 조정 | 설명 |
| ------ | ------ | ----------- |
Column_1 | **자동** | 열의 크기가 내용에 맞게 조정됩니다.
Column_2 | * | 자동 열이 계산된 후에는 해당 열에 나머지 너비의 일부가 지정됩니다. Column_2는 Column_4 너비의 절반이 됩니다.
Column_3 | **44** | 열의 너비는 44픽셀입니다.
Column_4 | **2**\* | 자동 열이 계산된 후에는 해당 열에 나머지 너비의 일부가 지정됩니다. Column_4는 Column_2 너비의 두 배가 됩니다.

기본 열 너비가 "*"이므로 두 번째 열에 대해서는 이 값을 명시적으로 설정할 필요가 없습니다.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its content." FontSize="24"/>
</Grid>
```

Visual Studio XAML 디자이너에서 결과는 다음과 같습니다.

![Visual Studio 디자이너에서 4개의 열 그리드](images/xaml-layout-grid-in-designer.png)

런타임 시 요소의 크기를 가져오려면 Height와 Width 대신 읽기 전용 [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight)와 [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 속성을 사용합니다.

**크기 제약 조건**

UI에서 자동 크기 조정을 사용하는 경우 여전히 요소 크기에 제약 조건을 적용해야 할 수 있습니다. [  **MinWidth**](/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) 및 [**MinHeight**](/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](/uwp/api/windows.ui.xaml.frameworkelement.maxheight) 속성을 설정하면 유동 크기 조정을 허용하면서 요소의 크기를 제한하는 값을 지정할 수 있습니다.

Grid에서 MinWidth/MaxWidth는 열 정의에 사용할 수도 있으며 MinHeight/MaxHeight는 행 정의에 사용할 수도 있습니다.

**맞춤**

[  **HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 및 [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 속성을 사용하여 해당 부모 컨테이너 내에서 요소의 위치가 지정되는 방법을 지정합니다.
- **HorizontalAlignment** 에 대한 값은 **Left**, **Center**, **Right** 및 **Stretch** 입니다.
- **VerticalAlignment** 에 대한 값은 **Top**, **Center**, **Bottom** 및 **Stretch** 입니다.

**Stretch** 맞춤을 사용하면 요소가 부모 컨테이너에 제공된 모든 공간을 채웁니다. 두 맞춤 속성의 기본값은 모두 Stretch입니다. 그러나 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)과 같은 일부 컨트롤은 이 값을 해당하는 기본 스타일로 재정의합니다.
자식 요소를 가질 수 있는 요소는 HorizontalAlignment 및 VerticalAlignment 속성에 대한 Stretch 값을 고유하게 처리할 수 있습니다. 예를 들어 Grid에 배치된 기본 Stretch 값을 사용하는 요소는 이 요소를 포함하는 셀을 채울 수 있도록 늘어납니다. Canvas에 배치된 동일한 요소는 해당 콘텐츠에 맞게 크기가 조정됩니다. 각 패널이 Stretch 값을 처리하는 방법에 대한 자세한 내용은 [레이아웃 패널](layout-panels.md) 문서를 참조하세요.

자세한 내용은 [맞춤, 여백 및 안쪽 여백](alignment-margin-padding.md) 문서, [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 및 [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 참조 페이지를 참조하세요.

**표시 여부**

해당 [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) 속성을 [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility) 열거형 값인 **Visible** 또는 **Collapsed** 로 설정하여 요소를 표시하거나 숨길 수 있습니다. 요소가 Collapsed인 경우 UI 레이아웃에서 공간을 차지하지 않습니다.

코드 또는 시각적 상태에서 요소의 Visibility 속성을 변경할 수 있습니다. 요소의 Visibility가 변경되는 경우 해당하는 모든 자식 요소도 변경됩니다. 한 패널을 표시하고 다른 패널은 축소하여 UI의 섹션을 바꿀 수 있습니다.

> [!Tip]
> 기본적으로 **Collapsed** 인 요소가 UI에 있는 경우 비록 보이지 않아도 시작할 때 해당 개체는 여전히 만들어집니다. **x:DeferLoadStrategy 특성** 을 "Lazy"로 설정하면 이러한 요소가 표시될 때까지 로드하는 것을 연기할 수 있습니다. 그러면 시작 성능이 향상될 수 있습니다. 자세한 내용은 [x:DeferLoadStrategy 특성](../../xaml-platform/x-deferloadstrategy-attribute.md)을 참조하세요.

### <a name="style-resources"></a>스타일 리소스

하나의 컨트롤에서 각 속성 값을 개별적으로 설정할 필요가 없습니다. 일반적으로 속성 값을 [**Style**](/uwp/api/Windows.UI.Xaml.Style) 리소스로 그룹화하고 Style을 컨트롤에 적용하는 것이 더 효율적입니다. 이는 동일한 속성 값을 많은 컨트롤에 적용해야 하는 경우 특히 그렇습니다. 스타일을 사용하는 방법에 대한 자세한 내용은 [컨트롤의 스타일 지정](../controls-and-patterns/xaml-styles.md)을 참조하세요.

### <a name="layout-panels"></a>레이아웃 패널

시각적 개체의 위치를 지정하려면 패널 또는 다른 컨테이너 개체에 배치해야 합니다. XAML 프레임워크는 컨테이너로 사용되는 다양한 패널 클래스(예: [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas), [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) 및 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel))를 제공합니다. 이러한 클래스를 사용하면 클래스 내에 UI 요소의 위치를 지정하고 정렬할 수 있습니다.

레이아웃 패널을 선택할 때 우선적으로 고려해야 할 사항은 패널에서 자식 요소의 위치를 지정하고 크기를 조정하는 방식입니다. 또한 겹치는 자식 요소가 서로 다른 항목 위에 계층화되는 방식을 고려해야 할 수도 있습니다.

다음은 XAML 프레임워크에 제공되는 패널 컨트롤의 주요 기능을 비교한 것입니다.

패널 컨트롤 | 설명
--------------|------------
[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) | **Canvas** 는 유동 UI를 지원하지 않으며 개발자가 자식 요소의 위치 지정 및 크기 조정과 관련한 모든 측면을 제어합니다. 일반적으로 이 컨트롤은 그래픽 만들기와 같은 특수한 경우에 사용하거나 더 큰 적응형 UI의 작은 고정 영역을 정의하는 데 사용합니다. 코드나 시각적 상태를 사용하여 런타임 시 요소의 위치를 변경할 수 있습니다.<li>요소가 Canvas.Top 및 Canvas.Left 연결된 속성을 사용하여 절대 위치의 위치가 지정됩니다.</li><li>계층화는 Canvas.ZIndex 연결된 속성을 사용하여 명시적으로 지정할 수 있습니다.</li><li>HorizontalAlignment/VerticalAlignment에 대한 Stretch 값은 무시됩니다. 요소의 크기가 명시적으로 설정되지 않은 경우 콘텐츠에 맞게 조정됩니다.</li><li>자식 콘텐츠가 패널보다 더 크면 시각적으로 잘리지 않습니다. </li><li>자식 콘텐츠가 패널 범위에 의해 제한되지 않습니다.</li>
[**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) | **Grid** 는 자식 요소의 유동 크기 조정을 지원합니다. 코드나 시각적 상태를 사용하여 요소의 위치를 변경하고 재배치할 수 있습니다.<li>요소가 Grid.Row 및 Grid.Column 연결된 속성을 사용하여 행과 열에 배열됩니다.</li><li>연결 속성인 Grid.RowSpan 및 Grid.ColumnSpan을 사용하여 요소를 여러 행과 열에 걸쳐 표시할 수 있습니다.</li><li>HorizontalAlignment/VerticalAlignment에 대한 Stretch 값은 유지됩니다. 요소의 크기가 명시적으로 설정되지 않은 경우 늘어나서 그리드 셀의 사용 가능한 공간을 채웁니다.</li><li>패널보다 더 큰 자식 콘텐츠는 시각적으로 잘립니다.</li><li>콘텐츠 크기가 패널 범위로 제한되므로 필요한 경우 스크롤 가능한 콘텐츠에 스크롤 막대가 표시됩니다.</li>
[**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | <li>요소가 패널 가장자리 또는 중심과 서로를 기준으로 정렬됩니다.</li><li>패널 맞춤, 형제 맞춤 및 형제 위치를 제어하는 다양한 연결된 속성을 사용하여 요소의 위치가 지정됩니다. </li><li>맞춤에 대한 RelativePanel 연결된 속성으로 늘이기가 발생하지 않는 경우(예를 들어 요소가 패널의 오른쪽과 왼쪽 가장자리에 배열되는 경우) HorizontalAlignment/VerticalAlignment에 대한 Stretch 값은 무시됩니다. 요소의 크기가 명시적으로 설정되지 않아 늘어나지 않는 경우에는 콘텐츠에 맞게 조정됩니다.</li><li>패널보다 더 큰 자식 콘텐츠는 시각적으로 잘립니다.</li><li>콘텐츠 크기가 패널 범위로 제한되므로 필요한 경우 스크롤 가능한 콘텐츠에 스크롤 막대가 표시됩니다.</li>
[**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) |<li>요소가 세로 또는 가로로 한 줄에 스택 형태로 쌓입니다.</li><li>HorizontalAlignment/VerticalAlignment에 대한 Stretch 값은 Orientation 속성의 반대 방향으로 유지됩니다. 요소의 크기가 명시적으로 설정되지 않은 경우 늘려서 사용 가능한 너비(또는 Orientation이 Horizontal인 경우에는 높이)를 채웁니다. Orientation 속성에서 지정된 방향으로 요소가 해당 콘텐츠에 맞게 크기가 조정됩니다.</li><li>패널보다 더 큰 자식 콘텐츠는 시각적으로 잘립니다.</li><li>콘텐츠 크기가 Orientation 속성에서 지정된 방향의 패널 범위에 의해 제한되지 않으므로 스크롤 가능한 콘텐츠가 패널 범위 이상으로 늘어나고 스크롤 막대는 표시하지 않습니다. 스크롤 막대를 표시하려면 자식 콘텐츠의 높이나 너비를 명시적으로 제한해야 합니다.</li>
[**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) |<li>요소가 행이나 열 형태로 배열되며 MaximumRowsOrColumns 값에 도달할 경우 새 행이나 열로 자동 줄 바꿈됩니다.</li><li>요소를 행으로 배치할지 열로 배치할지 여부는 Orientation 속성을 통해 지정됩니다.</li><li>연결 속성인 VariableSizedWrapGrid.RowSpan 및 VariableSizedWrapGrid.ColumnSpan을 사용하여 요소를 여러 행과 열에 걸쳐 표시할 수 있습니다.</li><li>HorizontalAlignment/VerticalAlignment에 대한 Stretch 값은 무시됩니다. 요소의 크기는 ItemHeight 및 ItemWidth 속성에 지정된 대로 조정됩니다. 이러한 속성을 설정하지 않으면 첫 번째 셀 크기의 값이 사용됩니다.</li><li>패널보다 더 큰 자식 콘텐츠는 시각적으로 잘립니다.</li><li>콘텐츠 크기가 패널 범위로 제한되므로 필요한 경우 스크롤 가능한 콘텐츠에 스크롤 막대가 표시됩니다.</li>

이러한 패널에 대한 자세한 정보 및 예제는 [레이아웃 패널](layout-panels.md)과 [반응형 기술 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)을 참조하세요.

레이아웃 패널을 통해 UI를 논리적 컨트롤 그룹으로 구성할 수 있습니다. 적절한 속성 설정과 함께 레이아웃 패널을 사용하면 UI 요소의 자동 크기 조정, 위치 변경 및 재배치에 대한 일부 지원을 받을 수 있습니다. 그러나 창 크기가 크게 변경되는 경우 대부분의 UI 레이아웃을 추가로 수정해야 합니다. 이 경우 시각적 상태를 사용하면 됩니다.

## <a name="adaptive-layouts-with-visual-states-and-state-triggers"></a>시각적 상태 및 상태 트리거를 사용한 적응형 레이아웃
시각적 상태를 사용하여 창 크기 또는 기타 변경 사항에 따라 UI를 크게 변경할 수 있습니다.

앱 창이 일정 수준 이상으로 늘어나거나 줄어드는 경우 레이아웃 속성을 변경하여 UI 섹션에 대해 위치 변경, 크기 조정, 재배치, 표시 또는 바꾸기 작업을 수행할 수 있습니다. UI에 다양한 시각적 상태를 정의한 후 창 너비 또는 창 높이가 지정한 임계값을 넘어가면 적용할 수 있습니다.

[  **VisualState**](/uwp/api/Windows.UI.Xaml.VisualState)는 특정 상태에 있을 때 요소에 적용되는 속성 값을 정의합니다. 지정된 조건이 충족되는 경우 적절한 VisualState를 적용하는 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager)에서 시각적 상태를 그룹화합니다. [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)는 XAML에서 상태가 적용되는 임계값('중단점'이라고도 함)을 설정하는 쉬운 방법을 제공합니다. 또는 코드에서 [**VisualStateManager.GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate) 메서드를 호출하여 시각적 상태를 적용할 수 있습니다. 두 가지 방법의 예제는 다음 섹션에 나와 있습니다.

### <a name="set-visual-states-in-code"></a>코드에서 시각적 상태 설정

코드에서 시각적 상태를 적용하려면 [**VisualStateManager.GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate) 메서드를 호출합니다. 예를 들어 앱 창이 특정한 크기인 상태를 적용하려면 [**SizeChanged**](/uwp/api/windows.ui.xaml.window.sizechanged) 이벤트를 처리하고 **GoToState** 를 호출하여 적절한 상태를 적용합니다.

여기서는 [**VisualStateGroup**](/uwp/api/Windows.UI.Xaml.VisualStateGroup)에 2개의 VisualState 정의가 포함되어 있습니다. 첫 번째 정의인 `DefaultState`는 비어 있습니다. 적용되면 XAML 페이지에서 정의된 값이 적용됩니다. 두 번째 정의인 `WideState`는 [**SplitView**](/uwp/api/Windows.UI.Xaml.Controls.SplitView)의 [**DisplayMode**](/uwp/api/windows.ui.xaml.controls.splitview.displaymode) 속성을 **Inline** 으로 변경하고 창을 엽니다. 이 상태는 창 너비가 640 유효 픽셀 이상인 경우 SizeChanged 이벤트 처리기에서 적용됩니다.

> [!NOTE]
> Windows에서는 앱이 실행되는 특정 디바이스를 앱 내에서 검색하는 방법을 제공하지 않습니다. 물론 앱이 실행 중인 디바이스 패밀리(모바일, 데스크톱, 등), 유효 해상도 및 앱에서 사용할 수 있는 화면 공간 크기(앱의 창 크기)는 알 수 있습니다. 따라서 [화면 크기 및 중단점](screen-sizes-and-breakpoints-for-responsive-design.md)에 대해 시각적 상태를 정의하는 것이 좋습니다.

```xaml
<Page ...
    SizeChanged="CurrentWindow_SizeChanged">
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width > 640)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

```cppwinrt
// YourPage.h
void CurrentWindow_SizeChanged(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::SizeChangedEventArgs const& e);

// YourPage.cpp
void YourPage::CurrentWindow_SizeChanged(IInspectable const& sender, SizeChangedEventArgs const& e)
{
    if (e.NewSize.Width > 640)
        VisualStateManager::GoToState(*this, "WideState", false);
    else
        VisualStateManager::GoToState(*this, "DefaultState", false);
}

```

### <a name="set-visual-states-in-xaml-markup"></a>XAML 태그에서 시각적 상태 설정

Windows 10 이전에는 속성 변경을 위해 [**VisualState**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) 정의에 Storyboard 개체가 필요했기 때문에 상태를 적용하려면 코드에서 **GoToState** 를 호출해야 했습니다. 이 구문은 이전 예제에 나와 있습니다. 아직도 이 구문을 사용하는 많은 예제를 볼 수 있으며 기존 코드에서 이 구문을 사용했을 수도 있습니다.

Windows 10부터는 여기 나와 있는 것처럼 간소화된 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 구문을 사용할 수 있기 때문에 상태를 적용하려면 XAML 태그에서 [**StateTrigger**](/uwp/api/Windows.UI.Xaml.StateTrigger)를 사용하면 됩니다. 상태 트리거를 사용하여 앱 이벤트에 응답해서 자동으로 시각적 상태 변경을 트리거하는 간단한 규칙을 만들 수 있습니다.

이 예제에서는 이전 예제와 동일한 작업을 수행하지만 Storyboard 대신 간소화된 **Setter** 구문을 사용하여 속성 변경을 정의합니다. GoToState를 호출하는 대신 기본 제공 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 상태 트리거를 사용하여 상태를 적용합니다. 상태 트리거를 사용하게 되면 빈 `DefaultState`를 정의할 필요가 없습니다. 상태 트리거 조건이 더 이상 충족되지 않는 경우 기본 설정이 자동으로 다시 적용됩니다.

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=640 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="640" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> [!Important]
> 이전 예제에서는 VisualStateManager.VisualStateGroups 연결된 속성이 **Grid** 요소에 설정되었습니다. StateTriggers를 사용하는 경우 트리거가 자동으로 적용되도록 VisualStateGroups가 루트의 첫 번째 자식에 연결되어 있는지 항상 확인합니다. (여기서 **Grid** 는 루트 **Page** 요소의 첫 번째 자식입니다.)

### <a name="attached-property-syntax"></a>연결된 속성 구문

VisualState에서, 일반적으로 컨트롤 속성 또는 컨트롤을 포함하는 패널의 연결된 속성 중 하나에 대한 값을 설정합니다. 연결된 속성을 설정할 때 연결된 속성 이름 주위에 괄호를 사용합니다.

이 예제는 `myTextBox`라는 TextBox에서 [**RelativePanel.AlignHorizontalCenterWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty) 연결된 속성을 설정하는 방법을 보여 줍니다. 첫 번째 XAML은 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 구문을 사용하고 두 번째 XAML은 **Setter** 구문을 사용합니다.

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### <a name="custom-state-triggers"></a>상태 트리거 사용자 지정

[  **StateTrigger**](/uwp/api/Windows.UI.Xaml.StateTrigger) 클래스를 확장하여 광범위한 시나리오에 대해 사용자 지정 트리거를 만들 수 있습니다. 예를 들어 StateTrigger를 만들어 입력 형식에 따라 서로 다른 상태를 트리거한 다음 입력 형식을 터치할 때 컨트롤 주위의 여백을 늘릴 수 있습니다. 또는 StateTrigger를 만들어 앱이 실행되는 디바이스 패밀리에 따라 서로 다른 상태를 적용할 수 있습니다. 사용자 지정 트리거를 구성하고 이를 사용하여 단일 XAML 보기에서 최적화된 UI 환경을 만드는 방법에 대한 예제는 [상태 트리거 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)을 참조하세요.

### <a name="visual-states-and-styles"></a>시각적 상태 및 스타일

시각적 상태에서 Style 리소스를 사용하면 속성 변경 집합을 여러 컨트롤에 적용할 수 있습니다. 스타일을 사용하는 방법에 대한 자세한 내용은 [컨트롤의 스타일 지정](../controls-and-patterns/xaml-styles.md)을 참조하세요.

상태 트리거 샘플의 이 간소화된 XAML에서 마우스 또는 터치식 입력에 대한 크기 및 여백을 조정하기 위해 Style 리소스가 Button에 적용되었습니다. 사용자 지정 상태 트리거에 대한 전체 코드 및 정의는 [상태 트리거 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)을 참조하세요.

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## <a name="related-topics"></a>관련 항목

- [자습서: 적응형 레이아웃 만들기](../basics/xaml-basics-adaptive-layout.md)
- [반응 기술 샘플(GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)
- [상태 트리거 샘플(GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)
- [맞춤형 다중 보기 샘플(GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlTailoredMultipleViews)
