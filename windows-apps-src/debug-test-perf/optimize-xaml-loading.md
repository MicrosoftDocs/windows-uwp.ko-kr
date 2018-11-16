---
author: jwmsft
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: XAML 태그 최적화
description: 메모리에서 개체를 생성하기 위해 XAML 태그를 구문 분석하는 작업은 복잡한 UI의 경우 시간이 많이 걸립니다. 다음은 XAML 태그 구문 분석 및 로드 시간과 앱의 메모리 효율성을 개선하기 위해 수행할 수 있는 몇 가지 작업입니다.
ms.author: jimwalk
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 884825f2e9639f620d8db4e6110791fddf2d7e77
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6853066"
---
# <a name="optimize-your-xaml-markup"></a>XAML 태그 최적화


메모리에서 개체를 생성하기 위해 XAML 태그를 구문 분석하는 작업은 복잡한 UI의 경우 시간이 많이 걸립니다. 다음은 XAML 태그 구문 분석 및 로드 시간과 앱의 메모리 효율성을 개선하기 위해 수행할 수 있는 몇 가지 작업입니다.

앱 시작 시 로드되는 XAML 태그를 초기 UI에 필요한 태그로만 제한합니다. 초기 페이지(페이지 리소스 포함)의 태그를 검사하고 지금 필요하지 않은 기타 요소를 로드하지 않도록 합니다. 이러한 요소는 리소스 사전, 초기 축소된 요소, 다른 요소 위에 그려지는 요소와 같은 다양한 출처에서 옵니다.

XAML 효율성을 위한 최적화에는 장단점이 있습니다. 모든 상황에 대한 하나의 해결책은 없습니다. 여기에서는 몇 가지 일반적인 문제와 앱을 위한 올바른 장단점 선택을 할 수 있는 지침을 살펴봅니다.

## <a name="minimize-element-count"></a>요소 수 최소화

XAML 플랫폼은 많은 요소를 표시할 수 있지만 원하는 시각 효과를 달성하려면 최소한의 요소를 사용하여 앱 배치 및 렌더링을 보다 빠르게 만들어야 합니다.

UI 컨트롤의 레이아웃을 만들기 위해 하는 선택은 앱이 시작될 때 생성되는 UI 요소의 수에 영향을 미칩니다. 레이아웃 최적화에 대한 자세한 내용은 [XAML 레이아웃 최적화](optimize-your-xaml-layout.md)를 참조하세요.

각 요소는 각 데이터 항목에 대해 다시 생성되기 때문에 요소 수는 데이터 템플릿에 매우 중요합니다. 목록 또는 그리드에서 요소 수를 줄이는 데 대한 자세한 내용은 [ListView 및 GridView UI 최적화](optimize-gridview-and-listview.md) 문서의 *항목당 요소 감소*를 참조하세요.

앱이 시작 시 로드해야 하는 요소의 수를 줄이는 몇 가지 다른 방법이 있습니다.

### <a name="defer-item-creation"></a>항목 만들기 연기

XAML 태그에 지금 바로 표시되지 않는 요소가 포함되어 있다면, 표시할 때까지 이러한 요소의 로드를 연기할 수 있습니다. 예를 들어, 탭 형식 UI에서 보조 탭과 같은 표시되지 않는 콘텐츠를 연기할 수 있습니다. 또는 기본적으로 그리드 보기로 항목을 표시하고, 사용자가 목록으로 볼 수 있는 옵션을 제공할 수 있습니다. 필요할 때까지 목록의 로드를 연기할 수 있습니다.

요소의 표시를 제어하는 데 [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.Visibility) 대신 [x:Load 특성](../xaml-platform/x-load-attribute.md)을 사용합니다. 요소의 가시성이 **Collapsed**일 경우, 렌더링 전달 동안 건너뛸 수 있지만 여전히 메모리의 개체 인스턴스 비용을 지불해야 합니다. 대신 x:Load를 사용하면 프레임워크가 필요할 때까지 개체를 생성하지 않으므로 메모리 비용이 더욱 저렴합니다. 단점은 UI가 로드되지 않을 때 작은 메모리 오버헤드(약 600바이트)를 지불하는 것입니다.

> [!NOTE]
> [x:Load](../xaml-platform/x-load-attribute.md) 또는 [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md) 특성을 사용하여 요소 로드를 지연시킬 수 있습니다. x:Load 특성은 Windows 10 크리에이터스 업데이트(버전 1703, SDK 빌드 15063)부터 사용할 수 있습니다. Visual Studio 프로젝트가 대상으로 하는 최소 버전이 *Windows 10 크리에이터스 업데이트(10.0, Build 15063)* 는 되어야 x:Load를 사용할 수 있습니다. 이전 버전을 대상으로 하는 경우 x:DeferLoadStrategy를 사용합니다.

다음 예에서는 UI 요소를 숨기는 데 다른 기술을 사용했을 때 요소의 수와 메모리 사용의 차이를 보여 줍니다. 동일한 항목을 포함한 ListView와 GridView가 페이지의 루트 그리드에 배치됩니다. ListView는 보이지 않지만 GridView는 표시됩니다. 이러한 예의 각 XAML은 화면에 동일한 UI를 생성합니다. Visual Studio의 [프로파일링 및 성능 도구](tools-for-profiling-and-performance.md)를 사용하여 요소 수와 메모리 사용을 확인합니다.

#### <a name="option-1---inefficient"></a>옵션 1 - 비효율적

ListView가 로드되지만 너비가 0이기 때문에 표시되지 않습니다. ListView와 각 하위 요소가 시각적 트리에서 생성되고 메모리에 로드됩니다.

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <ListView x:Name="List1" Width="0">
        <ListViewItem>Item 1</ListViewItem>
        <ListViewItem>Item 2</ListViewItem>
        <ListViewItem>Item 3</ListViewItem>
        <ListViewItem>Item 4</ListViewItem>
        <ListViewItem>Item 5</ListViewItem>
        <ListViewItem>Item 6</ListViewItem>
        <ListViewItem>Item 7</ListViewItem>
        <ListViewItem>Item 8</ListViewItem>
        <ListViewItem>Item 9</ListViewItem>
        <ListViewItem>Item 10</ListViewItem>
    </ListView>

    <GridView x:Name="Grid1">
        <GridViewItem>Item 1</GridViewItem>
        <GridViewItem>Item 2</GridViewItem>
        <GridViewItem>Item 3</GridViewItem>
        <GridViewItem>Item 4</GridViewItem>
        <GridViewItem>Item 5</GridViewItem>
        <GridViewItem>Item 6</GridViewItem>
        <GridViewItem>Item 7</GridViewItem>
        <GridViewItem>Item 8</GridViewItem>
        <GridViewItem>Item 9</GridViewItem>
        <GridViewItem>Item 10</GridViewItem>
    </GridView>
</Grid>
```

ListView로 실제 시각적 트리가 로드됩니다. 페이지의 총 요소 수는 89개입니다.

![목록 보기를 사용한 시각적 트리](images/visual-tree-1.png)

ListView와 하위 요소가 메모리에 로드됩니다.

![목록 보기를 사용한 시각적 트리](images/memory-use-1.png)

#### <a name="option-2---better"></a>옵션 2 - 더 나음

이번에는 ListView의 Visibility가 Collapsed로 설정되었습니다(다른 XAML은 원래와 동일). ListView가 시각적 트리에서 생성되지만 하위 요소는 그렇지 않습니다. 그러나 메모리에는 로드되므로, 메모리 사용은 이전 예와 동일합니다.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

ListView가 축소된 실제 시각적 트리입니다. 페이지의 총 요소 수는 46개입니다.

![축소된 목록 보기를 사용한 시각적 트리](images/visual-tree-2.png)

ListView와 하위 요소가 메모리에 로드됩니다.

![목록 보기를 사용한 시각적 트리](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>옵션 3 - 가장 효율적

여기서는 ListView의 x:Load 특성이 **False**로 설정되었습니다(다른 XAML은 원래와 동일). ListView는 시작 시 시각적 트리에 생성되거나 메모리에 로드되지 않습니다.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

ListView로 실제 시각적 트리가 로드되지 않습니다. 페이지의 총 요소 수는 45개입니다.

![목록 보기가 로드되지 않은 시각적 트리](images/visual-tree-3.png)

ListView와 하위 요소가 메모리에 로드되지 않습니다.

![목록 보기를 사용한 시각적 트리](images/memory-use-3.png)

> [!NOTE]
> 이러한 예에서 사용한 요소 수와 메모리 사용은 아주 적은 것으로, 개념 설명을 위한 것입니다. 이러한 예에서는 x:Load를 사용하는 오버헤드가 메모리 절약 효과보다 크므로 앱에 장점이 되지 못합니다. 앱에서 프로파일링 도구를 사용하여 지연 로딩으로 앱에 효과가 있는지 파악해야 합니다.

### <a name="use-layout-panel-properties"></a>레이아웃 패널 속성 사용

레이아웃 패널에는 [Background](https://msdn.microsoft.com/library/windows/apps/BR227512) 속성이 있으므로 색을 지정하기 위해 패널 앞에 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 둘 필요가 없습니다.

**비효율적인 경우**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid>
    <Rectangle Fill="Black"/>
</Grid>
```

**효율적인 경우**

```xaml
<Grid Background="Black"/>
```

레이아웃 패널에는 기본 제공 테두리 속성도 있으므로, 레이아웃 패널 주변에 [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border)를 배치할 필요가 없습니다. 더 많은 정보와 예는 [XAML 레이아웃 최적화](optimize-your-xaml-layout.md)를 참조하세요.

### <a name="use-images-in-place-of-vector-based-elements"></a>벡터 기반 요소 대신 이미지 사용

동일한 벡터 기반 요소를 많이 재사용할 경우, [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 요소를 대신 사용하는 것이 보다 효율적입니다. 벡터 기반 요소는 CPU에서 각 개별 요소를 별도로 만들어야 하므로 비용이 더 많이 소요될 수 있습니다. 이미지 파일은 한 번만 디코딩하면 됩니다.

## <a name="optimize-resources-and-resource-dictionaries"></a>리소스와 리소스 사전 최적화

일반적으로 [리소스 사전](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)을 사용하여 앱의 여러 곳에서 참조하려는 리소스를 전체 수준 위치에 저장합니다. 예를 들어 스타일, 브러시, 템플릿 등이 포함됩니다.

일반적으로 요청되지 않은 경우 리소스를 인스턴스화하지 않도록 [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)를 최적화했습니다. 하지만 리소스가 불필요하게 인스턴스화되지 않도록 방지해야 하는 경우가 있습니다.

### <a name="resources-with-xname"></a>x:Name이 포함된 리소스

[x:Key 특성](../xaml-platform/x-key-attribute.md)을 사용하여 리소스를 참조하십시오. [x:Name 특성](../xaml-platform/x-name-attribute.md)이 포함된 리소스는 플랫폼 최적화의 이점이 적용되지 않는 대신 ResourceDictionary가 만들어지면 곧바로 인스턴스화됩니다. 이렇게 되는 이유는 x:Name에서 앱에 이 리소스에 대한 필드 액세스가 필요함을 플랫폼에 알리므로 플랫폼이 참조를 생성할 관련 항목을 만들어야 하기 때문입니다.

### <a name="resourcedictionary-in-a-usercontrol"></a>UserControl의 ResourceDictionary

[UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) 내부에서 정의된 ResourceDictionary는 성능 저하를 동반합니다. 플랫폼이 UserControl의 모든 인스턴스에 대해 이러한 ResourceDictionary의 복사본을 만듭니다. 많이 사용되는 UserControl이 있는 경우에는 ResourceDictionary를 UserControl 외부로 이동하여 페이지 수준에 배치합니다.

### <a name="resource-and-resourcedictionary-scope"></a>리소스 및 ResourceDictionary 범위

페이지가 다른 파일에 정의된 사용자 컨트롤이나 리소스를 참조하는 경우 프레임워크는 해당 파일도 구문 분석합니다.

아래 _InitialPage.xaml_에서는 _ExampleResourceDictionary.xaml_의 리소스 하나를 사용하므로 시작 시 전체 _ExampleResourceDictionary.xaml_을 구문 분석해야 합니다.

**InitialPage.xaml.**

```xaml
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
        are used in the app, but are not needed during startup.-->
</ResourceDictionary>
```

앱 전반에 걸쳐 여러 페이지의 리소스를 사용하는 경우 _App.xaml_에 저장하는 것이 가장 좋으며, 중복을 방지합니다. 그러나 _App.xaml_은 앱 시작 시 구문 분석되므로 하나의 페이지(초기 페이지가 아닌 경우)에서만 사용되는 리소스는 해당 페이지의 로컬 리소스에 두어야 합니다. 반대되는 이 예에서는 하나의 페이지(초기 페이지가 아닌)에서 사용되는 리소스가 포함된 _App.xaml_을 보여 줍니다. 이 경우 앱 시작 시간이 불필요하게 증가합니다.

**App.xaml**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Application ...>
     <Application.Resources>
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/>
    </Application.Resources>
</Application>
```

**InitialPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <StackPanel>
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/>
    </StackPanel>
</Page>
```

**SecondPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.SecondPage" ...>
    <StackPanel>
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" />
    </StackPanel>
</Page>
```

이 예를 더 효율적으로 만들려면 `SecondPageTextBrush`를 _SecondPage.xaml_로 이동하고 `ThirdPageTextBrush`를 _ThirdPage.xaml_로 이동하면 됩니다. `InitialPageTextBrush` 는 항상 앱 시작 시 응용 프로그램 리소스를 구문 분석해야 하므로 _App.xaml_에서 유지할 수 있습니다.

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>같은 모양의 여러 브러시를 하나의 리소스에 통합

XAML 플랫폼은 공통적으로 사용되는 개체를 가능한 자주 다시 사용할 수 있도록 캐시하려고 합니다. 그러나 XAML은 하나의 태그 조각에 선언된 브러시가 다른 태그 조각에 선언된 브러시와 동일한지 쉽게 구별할 수 없습니다. 다음 예제에서는 보여 주기 위해 [SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)를 사용하지만 [GradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210068)를 사용하는 것이 더 중요하고 더 일반적입니다. 미리 정의된 색을 사용하는 브러시를 확인합니다. 예를 들어 `"Orange"`와 `"#FFFFA500"`은 같은 색입니다.

**비효율적인 경우**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBox>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

중복을 해결하려면 브러시를 리소스로 정의합니다. 다른 페이지의 컨트롤에서 동일한 브러시를 사용하는 경우 브러시를 _App.xaml_로 이동합니다.

**효율적인 경우**

```xaml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>

    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## <a name="minimize-overdrawing"></a>과도한 그리기 최소화

과도한 그리기는 둘 이상의 개체가 동일한 화면 픽셀에 그려지는 경우에 발생합니다. 이 지침과 요소 수 최소화 간에는 간혹 상충 관계가 있습니다.

[**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823)를 시각적 진단으로 사용하십시오. 장면에서 있는지 모르는 개체가 그려지는 경우가 있을 수 있습니다.

### <a name="transparent-or-hidden-elements"></a>투명 또는 숨겨진 요소

요소가 투명하거나 다른 요소 뒤에 숨겨져 있어 보이지 않고 레이아웃에 참여하지 않는 경우 해당 요소를 삭제합니다. 요소가 초기 시각적 상태에서는 보이지 않지만 다른 시각적 상태에서는 보이는 경우 x:Load를 사용하여 상태를 제어하거나 요소 자체에 대한 [Visibility](https://msdn.microsoft.com/library/windows/apps/BR208992)를 **Collapsed**로 설정하고 적절한 상태에서 값을 **Visible**로 변경합니다. 이 추론에 대한 예외가 있습니다. 일반적으로 대부분의 시각적 상태에서는 속성 값을 요소에서 로컬로 설정하는 것이 가장 좋습니다.

### <a name="composite-elements"></a>복합 요소

여러 요소를 계층화하는 대신 복합 요소를 사용하여 효과를 만듭니다. 이 예제에서는 위쪽 절반은 검은색([Grid](https://msdn.microsoft.com/library/windows/apps/BR242704)의 배경에서)이고 아래쪽 절반은 회색(**Grid**의 검은색 배경 위에 알파 혼합된 반투명 흰색의 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)에서)인 두 가지 색조의 모양이 만들어집니다. 여기에서는 결과를 달성하는 데 필요한 픽셀의 150%가 채워집니다.

**비효율적인 경우**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Black">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/>
</Grid>
```

**효율적인 경우**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Fill="Black"/>
    <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
</Grid>
```

### <a name="layout-panels"></a>레이아웃 패널

레이아웃 패널은 영역의 색을 지정하고 자식 요소를 배치하는 두 가지 용도로 사용됩니다. 다시 z 순서로 지정된 요소가 영역의 색을 이미 지정하는 경우 앞쪽의 레이아웃 패널에서는 해당 영역을 다시 그릴 필요가 없으며 대신 해당 자식을 배치하는 데 중점을 둘 수 있습니다. 예를 들면 다음과 같습니다.

**비효율적인 경우**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid Background="Blue"/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

**효율적인 경우**

```xaml
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

[Grid](https://msdn.microsoft.com/library/windows/apps/BR242704)가 적중 횟수를 테스트할 수 있어야 하는 경우 그 위에 투명한 배경색을 설정합니다.

### <a name="borders"></a>테두리

[Border](https://msdn.microsoft.com/library/windows/apps/BR209253) 요소를 사용하여 개체 주위의 테두리를 그립니다. 이 예제에서는 [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704)를 [TextBox](https://msdn.microsoft.com/library/windows/apps/BR209683) 주위의 임시 테두리로 사용합니다. 그러나 가운데 셀에 있는 모든 픽셀은 과도하게 그려집니다.

**비효율적인 경우**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Blue" Width="300" Height="45">
    <Grid.RowDefinitions>
        <RowDefinition Height="5"/>
        <RowDefinition/>
        <RowDefinition Height="5"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="5"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="5"/>
    </Grid.ColumnDefinitions>
    <TextBox Grid.Row="1" Grid.Column="1"></TextBox>
</Grid>
```

**효율적인 경우**

```xaml
 <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
     <TextBox/>
</Border>
```

### <a name="margins"></a>여백

여백에 주의합니다. 음수 여백이 다른 요소의 렌더링 범위로 확장되어 과도한 그리기가 발생하는 경우 인접한 두 요소가 실수로 겹쳐질 수 있습니다.

### <a name="cache-static-content"></a>정적 콘텐츠 캐시

과도한 그리기의 또 다른 원인은 하나의 모양이 겹쳐진 여러 요소에서 만들어지는 경우입니다. 복합 모양이 포함된 [UIElement](https://msdn.microsoft.com/library/windows/apps/BR208911)에서 [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084)를 **BitmapCache**로 설정한 경우 플랫폼은 요소를 비트맵으로 렌더링한 다음 각 프레임에서 과도한 그리기 대신 해당 비트맵을 사용합니다.

**비효율적인 경우**

```xaml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![단색 원 세 개가 있는 벤 다이어그램](images/solidvenn.png)

위 이미지가 결과이지만 과도하게 그려진 영역의 맵은 다음과 같습니다. 빨간색이 진할수록 과도하게 그려진 정도가 높습니다.

![겹친 영역을 보여 주는 벤 다이어그램](images/translucentvenn.png)

**효율적인 경우**

```xaml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

[CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084)의 사용에 주의합니다. 하위 모양을 애니메이션하는 경우 모든 프레임에서 비트맵 캐시를 다시 생성해야 할 수 있으므로 이 기술을 사용해서는 안 됩니다.

## <a name="use-xbf2"></a>XBF2 사용

XBF2는 런타임 시 모든 텍스트 구문 분석을 방지하는 XAML 태그의 이진 표현입니다. 또한 부하 및 트리 생성을 위한 이진 파일을 최적화하고 XAML 유형에 대해 "빠른 경로"를 허용하여 VSM, ResourceDictionary, 스타일 등과 같은 힙 및 개체 생성 비용을 개선합니다. 메모리가 완전히 매핑되었으므로 XAML 페이지 로드 및 읽기를 위한 힙 공간이 없습니다. 뿐만 아니라 appx에서 저장된 XAML 페이지의 디스크 공간이 줄어듭니다. XBF2는 보다 압축된 표현이며 비교되는 XAML/XBF1 파일의 디스크 공간을 최대 50%까지 줄일 수 있습니다. 예를 들어 ~1mb 정도의 XBF1 자산에서 ~400kb의 XBF2 자산으로 XBF2 삭제 변환 이후 기본 제공 사진 앱에서 디스크 공간이 60% 줄었습니다. 앱의 CPU 공간이 15~20%, Win32 힙이 10~15% 정도 개선되었습니다.

XAML 기본 제공 컨트롤 및 프레임워크에서 제공되는 사전은 이미 XBF2가 전적으로 지원됩니다. 고유한 앱의 경우 프로젝트 파일에서 TargetPlatformVersion 8.2 이상을 선언해야 합니다.

XBF2가 있는지를 확인하려면 바이너리 편집기에서 앱을 엽니다. XBF2가 있는 경우 12번째와 13번째 바이트가 00 02입니다.

## <a name="related-articles"></a>관련 문서

- [앱 시작 성능 모범 사례](best-practices-for-your-app-s-startup-performance.md)
- [XAML 레이아웃 최적화](optimize-your-xaml-layout.md)
- [ListView 및 GridView UI 최적화](optimize-gridview-and-listview.md)
- [프로파일링 및 성능 도구](tools-for-profiling-and-performance.md)
