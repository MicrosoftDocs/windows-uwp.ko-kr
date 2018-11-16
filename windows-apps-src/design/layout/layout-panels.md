---
author: QuinnRadich
Description: Use layout panels to arrange and group UI elements in your app.
title: UWP(유니버설 Windows 플랫폼) 앱용 레이아웃 패널
ms.author: quradic
ms.date: 04/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 623ca1484091f9f15b5a6dbedb85ed9d5149154e
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6860151"
---
# <a name="layout-panels"></a>레이아웃 패널

레이아웃 패널은 앱의 UI 요소를 배치하여 그룹화할 수 있는 컨테이너입니다. 기본 제공 XAML 레이아웃 패널은 [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) 및 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)가 포함되어 있습니다. 여기서는 각 패널에 대해 설명하고 XAML UI 요소를 배치하기 위해 패널을 사용하는 방법을 설명합니다.

레이아웃 패널을 선택할 때 다음과 같이 몇 가지 고려할 사항이 있습니다.
- 패널이 자식 요소의 위치를 지정하는 방식
- 패널이 자식 요소의 크기를 조정하는 방식
- 겹치는 자식 요소가 서로 다른 항목 위에 계층화되는 방식(z-순서)
- 원하는 레이아웃을 만드는 데 필요한 중첩된 패널 요소의 수 및 복잡성

## <a name="panel-properties"></a>패널 속성

각 패널에 대해 얘기하기 전에 모든 패널이 공통적으로 가지고 있는 속성에 대해서 살펴보겠습니다. 

### <a name="panel-attached-properties"></a>패널 연결된 속성

대부분의 XAML 레이아웃 패널은 연결된 속성을 사용하여 해당 자식 요소가 UI에 위치가 지정되는 방식을 부모 패널에 알릴 수 있습니다. 연결된 속성은 구문 *AttachedPropertyProvider.PropertyName*을 사용합니다. 다른 패널 안에 중첩된 패널이 있는 경우 부모에 레이아웃 특성을 지정하는 UI 요소의 연결된 속성은 가장 직계 부모 패널에서만 해석됩니다.

다음은 XAML의 Button 컨트롤에서 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) 연결된 속성을 설정할 수 있는 방법의 예제입니다. 여기서 Button이 Canvas의 왼쪽 가장자리에서 50개의 유효 픽셀에 위치가 지정되어야 한다는 것을 부모 Canvas에 알립니다.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

연결된 속성에 대한 자세한 내용은 [연결된 속성 개요](../../xaml-platform/attached-properties-overview.md)를 참조하세요.

### <a name="panel-borders"></a>패널 테두리

RelativePanel, StackPanel 및 Grid 패널은 추가 Border 요소에서 래핑 없이 패널 주위에 테두리를 그릴 수 있는 테두리 속성을 정의합니다. 테두리 속성은 **BorderBrush**, **BorderThickness**, **CornerRadius** 및 **Padding**입니다.

다음은 Grid에서 테두리 속성을 설정하는 방법의 예제입니다.

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![테두리가 포함된 그리드](images/layout-panel-grid-border.png)

기본 제공 테두리 속성을 사용하면 XAML 요소 수를 줄일 수 있으므로 앱의 UI 성능을 향상시킬 수 있습니다. 레이아웃 패널 및 UI 성능에 대한 자세한 내용은 [XAML 레이아웃 최적화](https://msdn.microsoft.com/en-us/library/windows/apps/mt404609.aspx)를 참조하세요.

## <a name="relativepanel"></a>RelativePanel

[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx)을 통해 다른 요소 및 패널을 기준으로 위치를 지정하여 UI 요소를 배치할 수 있습니다. 기본적으로 요소는 패널의 왼쪽 위 모서리에 위치가 지정됩니다. [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) 및 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx)와 함께 RelativePanel을 사용하면 다양한 창 크기에 맞게 UI를 다시 정렬할 수 있습니다.

다음 표는 요소를 패널 또는 다른 요소를 기준으로 정렬할 수 있는 연결된 속성을 보여 줍니다.

패널 맞춤 | 형제 맞춤 | 형제 위치
----------------|-------------------|-----------------
[**AlignTopWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwithpanel.aspx) | [**AlignTopWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwith.aspx) | [**보다 큼**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.above.aspx)  
[**AlignBottomWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwithpanel.aspx) | [**AlignBottomWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwith.aspx) | [**보다 적음**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.below.aspx)  
[**AlignLeftWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwithpanel.aspx) | [**AlignLeftWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwith.aspx) | [**LeftOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.leftof.aspx)  
[**AlignRightWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwithpanel.aspx) | [**AlignRightWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwith.aspx) | [**RightOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.rightof.aspx)  
[**AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) | [**AlignHorizontalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwith.aspx) | &nbsp;   
[**AlignVerticalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanel.aspx) | [**AlignVerticalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwith.aspx) | &nbsp;   

 
다음 XAML은 RelativePanel에서 요소를 정렬하는 방법을 보여 줍니다.

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Orange"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

결과는 다음과 같습니다. 

![상대 패널](images/layout-panel-relative-panel.png)

사각형의 크기를 조정하는 방법에 대해 참고할 몇 가지 사항이 있습니다.
- 빨간색 사각형은 명시적으로 크기가 44x44로 지정되어 패널의 왼쪽 위 모서리에 위치가 지정되며 이는 기본 위치입니다.
- 녹색 사각형은 명시적으로 높이가 44로 지정되며 왼쪽 면은 빨간색 사각형에 맞춰 정렬되고 오른쪽 면은 파란색 사각형에 맞춰 정렬됨으로써 너비가 결정됩니다.
- 주황색 사각형은 크기가 명시적으로 지정되지 않고 왼쪽 면이 파란색 사각형에 맞춰 정렬됩니다. 오른쪽 및 아래쪽 가장자리는 패널의 가장자리에 맞춰 정렬됩니다. 크기가 이러한 맞춤에 따라 결정되므로 패널 크기가 조정되면 같이 조정됩니다.

## <a name="stackpanel"></a>StackPanel

[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)은 가로 또는 세로 방향으로 지정된 단일 행에 자식 요소를 배열하는 레이아웃 패널입니다. StackPanel은 일반적으로 페이지에서 작은 하위 섹션 UI를 정렬하는 데 사용됩니다.

[**Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.orientation.aspx) 속성을 사용하여 자식 요소의 방향을 지정할 수 있습니다. 기본 방향은 [**Vertical**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.orientation.aspx)입니다.

다음 XAML은 항목의 수직 StackPanel을 만드는 방법을 보여줍니다.

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Orange" Height="44"/>
</StackPanel>
```


결과는 다음과 같습니다.

![스택 패널](images/layout-panel-stack-panel.png)

StackPanel에서 자식 요소의 크기가 명시적으로 설정되지 않은 경우 늘려서 사용 가능한 너비(또는 Orientation이 **Horizontal**인 경우에는 높이)를 채웁니다. 이 예제에서는 직사각형의 너비는 설정되지 않습니다. 직사각형은 StackPanel의 전체 너비에 맞게 확장됩니다.

## <a name="grid"></a>그리드

[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) 패널은 유동 레이아웃을 지원하여 컨트롤을 다중 행/열 레이아웃으로 배열할 수 있습니다. [**RowDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowdefinitions.aspx) 및 [**ColumnDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columndefinitions.aspx) 속성을 사용하여 Grid의의 행과 열을 지정합니다.

Grid의 특정 셀에 개체의 위치를 지정하려면 [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.column.aspx) 및 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.row.aspx) 연결된 속성을 사용하세요.

여러 행과 열에 걸쳐 콘텐츠를 표시하려면 [**Grid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowspan.aspx) 및 [**Grid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columnspan.aspx) 연결된 속성을 사용하세요.

다음 XAML 예제는 행 2개와 열 2개로 이루어진 그리드를 만드는 방법을 보여 줍니다.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


결과는 다음과 같습니다.

![그리드](images/layout-panel-grid.png)

이 예제에서 크기 조정은 다음과 같이 작동합니다. 
- 두 번째 행의 명시적인 높이는 44 유효 픽셀입니다. 기본적으로 첫 번째 행의 높이가 남아 있는 모든 공간을 채웁니다.
- 첫 번째 열의 너비가 **Auto**으로 설정되므로 해당 자식에 필요한 만큼 넓어집니다. 이 경우 너비는 빨간색 사각형의 너비를 수용하는 44 유효 픽셀입니다.
- 사각형에 다른 크기 제한이 없으므로 각각 늘어나서 자기가 속한 그리드 셀을 채웁니다.

**Auto** 또는 배율 크기 조정을 사용하면 열이나 행 안에서 공간을 분배할 수 있습니다. UI 요소가 해당 콘텐츠 또는 부모 컨테이너에 맞게 크기를 조정하려면 자동 크기 조정을 사용합니다. 그리드의 행과 열에서 자동 크기 조정을 사용할 수도 있습니다. 자동 크기 조정을 사용하려면 UI 요소의 Height 및/또는 Width를 **Auto**로 설정합니다.

가중 비율에 따라 그리드의 행과 열 사이에서 사용 가능한 공간을 분배하려면 *배율 크기 조정*이라고도 하는 가변 크기 조정을 사용합니다. XAML에서는 배율 값이 \*(또는 가중 배율 크기 조정의 경우 *n*\*)로 표현됩니다. 예를 들어 2열 레이아웃에서 첫 번째 열을 두 번째 열보다 5배 너비로 지정하려면 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx) 요소에서 [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx) 속성에 대해 "5\*" 및 "\*"를 사용합니다.

이 예제에서는 4개의 열을 가진 [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)에서 고정, 자동, 가변 크기 조정을 조합합니다.

&nbsp;|&nbsp;|&nbsp;
------|------|------
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
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

Visual Studio XAML 디자이너에서 결과는 다음과 같습니다.

![Visual Studio 디자이너에서 4개의 열 그리드](images/xaml-layout-grid-in-designer.png)

## <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid

[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx)는 [**MaximumRowsOrColumns**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns.aspx) 값에 도달할 경우 행 또는 열이 새 행이나 열로 자동 래핑되는 그리드 스타일 레이아웃 패널입니다. 

[**Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.orientation.aspx) 속성은 그리드에서 줄 바꿈 전에 행 또는 열에 해당 항목을 추가할지 여부를 지정합니다. 기본 방향은 **Vertical**이며 이는 하나의 열이 가득 찰 때까지 위에서 아래로 항목을 추가한 다음 새 열로 래핑된다는 의미입니다. 값이 **Horizontal**인 경우 그리드는 왼쪽에서 오른쪽으로 항목을 추가한 다음 새 행으로 래핑됩니다.

셀 치수는 [**ItemHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight.aspx)와 [**ItemWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth.aspx)에 의해 지정됩니다. 각 셀 크기는 동일합니다. ItemHeight 또는 ItemWidth가 지정되지 않은 경우 첫 번째 셀 크기가 해당 콘텐츠에 맞게 조정된 다음 다른 모든 셀은 첫 번째 셀의 크기로 지정됩니다.

[**VariableSizedWrapGrid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.columnspan.aspx) 및 [**VariableSizedWrapGrid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.rowspan.aspx) 연결된 속성을 사용하여 자식 요소를 채울 인접한 셀 수를 지정할 수 있습니다.

다음은 XAML에서 VariableSizedWrapGrid를 사용하는 방법입니다.

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


결과는 다음과 같습니다.

![가변 크기 래핑 그리드](images/layout-panel-variable-size-wrap-grid.png)

이 예제에서 각 열의 최대 행 수는 3입니다. 첫 번째 열은 파란색 사각형이 2행에 걸쳐 있기 때문에 2개 항목만(빨간색 및 파란색 사각형) 포함합니다. 그러면 녹색 사각형은 다음 열의 맨 위로 래핑됩니다.

## <a name="canvas"></a>캔버스

[**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) 패널에서는 고정된 좌표 점을 사용하여 자식 요소를 배치하기 때문에 유동 레이아웃을 지원하지 않습니다. 각 요소의 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) 및 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.top.aspx) 연결된 속성을 설정하여 개별 자식 요소의 점을 지정합니다. 부모 Canvas는 레이아웃의 [Arrange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.arrange.aspx) 단계 중에 자식에서 이러한 연결된 속성 값을 읽습니다.

Canvas의 개체는 겹칠 수 있으므로 한 개체를 다른 개체 위에 그릴 수 있습니다. 기본적으로 Canvas는 선언된 순서로 자식 개체를 렌더링하므로 마지막 자식이 맨 위에 렌더링됩니다(각 요소는 Z-인덱스의 기본값 0을 가짐). 이는 다른 기본 제공 패널과 같습니다. 그러나 Canvas는 각 자식 요소에 설정할 수 있는 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) 연결된 속성도 지원합니다. 코드에서 이 속성을 설정하면 런타임 중 요소의 그리기 순서를 변경할 수 있습니다. 가장 큰 Canvas.ZIndex 값을 가진 요소가 마지막에 그려지므로 동일한 공간을 공유하거나 어떤 방식으로든 겹치는 다른 요소 위에 그려집니다. 알파 값(투명도)이 적용되므로 요소가 겹치는 경우에도 위쪽 요소의 알파 값이 최대값이 아니면 겹치는 영역에 표시되는 콘텐츠가 혼합될 수 있습니다.

Canvas는 자식의 크기를 조정하지 않습니다. 각 요소는 해당 크기를 지정해야 합니다.

다음은 XAML의 Canvas 예제입니다.

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

결과는 다음과 같습니다.

![캔버스](images/layout-panel-canvas.png)

원하는 대로 Canvas 패널을 사용하세요. UI의 요소 위치를 정확하게 제어하는 것이 편리한 시나리오도 있지만 고정 위치가 지정된 레이아웃 패널에서 UI의 해당 영역은 전체 앱 창 크기 변경에 맞게 제대로 적응되지 않습니다. 앱 창 크기 조정은 장치 방향 변경, 분할된 앱 창, 모니터 변경 및 기타 여러 사용자 시나리오에서 발생할 수 있습니다.

## <a name="panels-for-itemscontrol"></a>ItemsControl용 패널

[**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)에서 항목을 표시하기 위한 [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)로만 사용할 수 있는 몇 가지 특별한 용도의 패널이 있습니다. [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemsstackpanel.aspx), [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemswrapgrid.aspx), [**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.virtualizingstackpanel.aspx) 및 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.wrapgrid.aspx)가 이러한 패널입니다. 일반 UI 레이아웃에는 이러한 패널을 사용할 수 없습니다.

