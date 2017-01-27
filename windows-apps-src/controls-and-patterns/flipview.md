---
author: Jwmsft
Description: "앨범의 사진이나 제품 세부 정보 페이지의 품목과 같이 한 번에 하나씩 컬렉션의 이미지를 표시합니다."
title: "대칭 이동 보기 컨트롤에 대한 지침"
ms.assetid: A4E05D92-1A0E-4CDD-84B9-92199FF8A8A3
label: Flip view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 92c523c100a021808e01dffe4cd9b5c47c21b58a
ms.openlocfilehash: 3ad89682248462efa5022467ceb330da03843de4

---
# <a name="flip-view"></a>대칭 이동 뷰

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

대칭 이동 보기를 사용하여 앨범의 사진이나 제품 세부 정보 페이지의 품목과 같이 한 번에 하나씩 컬렉션의 이미지나 다른 항목을 탐색할 수 있습니다. 터치 디바이스의 경우 항목을 살짝 밀면 컬렉션 내에서 이동됩니다. 마우스를 사용할 경우에는 항목 위로 마우스를 가져가면 탐색 단추가 나타납니다. 키보드를 사용할 경우에는 화살표 키를 사용하여 컬렉션 내에서 이동합니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**FlipView 클래스**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx)</li>
<li> [**ItemsSource 속성**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx)</li>
<li>[**ItemTemplate 속성**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)</li>

</ul>
</div>

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

대칭 이동 보기는 작은 크기부터 중간 크기의 컬렉션에 있는 이미지를 읽는 데 가장 적합합니다(최대 25개 항목). 이러한 컬렉션의 예에는 제품 세부 정보 페이지의 품목 또는 사진 앨범의 사진이 포함됩니다. 대부분의 대형 컬렉션의 경우에는 대칭 이동 보기가 권장되지 않지만 사진 앨범의 개별 이미지를 보는 데는 보통 괜찮습니다.

## <a name="examples"></a>예제

가장 왼쪽 항목에서 시작하고 오른쪽에서 대칭 이동하는 가로 검색은 보통 대칭 이동 보기에 대한 일반적인 레이아웃입니다. 이 레이아웃은 모든 디바이스에서 세로 또는 가로 방향으로 원활하게 작동합니다.

![가로 대칭 이동 보기 레이아웃 예제](images/controls_flipview_horizonal.jpg)

대칭 이동 보기는 세로로 검색할 수도 있습니다.

![세로 대칭 이동 보기 예제](images/controls_flipview_vertical.jpg)

## <a name="create-a-flip-view"></a>대칭 이동 뷰 만들기

FlipView는 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx)이므로 모든 유형의 항목 컬렉션을 포함할 수 있습니다. 뷰를 채우려면 [**Items**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 컬렉션에 항목을 추가하거나 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 속성을 데이터 원본에 설정합니다.

기본적으로, 데이터 항목은 바운딩된 데이터 개체의 문자열 표현으로 대칭 이동 보기에 표시됩니다. 대칭 이동 뷰에서 항목 표시 방법을 정확히 지정하려면 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.datatemplate.aspx)을 만들어 개별 항목을 표시하는 데 사용되는 컨트롤의 레이아웃을 정의합니다. 레이아웃의 컨트롤은 데이터 개체의 속성에 바운딩되거나 콘텐츠가 정의된 인라인을 가질 수 있습니다. FlipView의 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 속성에 DataTemplate를 할당합니다.

### <a name="add-items-to-the-items-collection"></a>항목 컬렉션에 항목 추가

XAML 또는 코드를 사용하여 [**Items**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 컬렉션에 항목을 추가할 수 있습니다. 일반적으로 XAML로 쉽게 정의되며 변경되지 않는 항목 수가 적은 경우 또는 런타임 시 코드에서 항목을 생성하는 경우 이 방식으로 항목을 추가합니다. 다음은 인라인으로 정의된 항목이 있는 대칭 이동 뷰입니다.

```xaml
<FlipView x:Name="flipView1">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

```csharp
// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.Items.Add("Item 1");
flipView1.Items.Add("Item 2");

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

항목을 대칭 이동 보기에 추가하는 경우 항목이 자동으로 [**FlipViewItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipviewitem.aspx) 컨테이너에 추가됩니다. 항목이 표시되는 방법을 변경하려면 [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) 속성을 설정하여 항목 컨테이너에 스타일을 적용할 수 있습니다. 

항목이 XAML로 정의된 경우에는 Items 컬렉션에도 자동으로 추가됩니다.

### <a name="set-the-items-source"></a>항목 원본 설정

일반적으로 대칭 이동 보기를 사용하여 데이터베이스나 인터넷과 같은 원본의 데이터를 표시합니다. 데이터 원본에서 대칭 이동 보기를 채우려면 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 속성을 데이터 항목의 컬렉션으로 설정합니다.

여기서 대칭 이동 보기의 ItemsSource는 코드에서 컬렉션의 인스턴스로 직접 설정됩니다.

```csharp
// Data source.
List<String> itemsList = new List<string>();
itemsList.Add("Item 1");
itemsList.Add("Item 2");

// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.ItemsSource = itemsList;
flipView1.SelectionChanged += FlipView_SelectionChanged;

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

또한 XAML에서 ItemsSource 속성을 컬렉션에 바인딩할 수도 있습니다. 자세한 내용은 [XAML을 사용하는 데이터 바인딩](../data-binding/data-binding-quickstart.md)을 참조하세요.

여기서는 ItemsSource가 `itemsViewSource`로 명명된 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)에 바인딩됩니다. 

```xaml
<Page.Resources>
    <!-- Collection of items displayed by this page -->
    <CollectionViewSource x:Name="itemsViewSource" Source="{Binding Items}"/>
</Page.Resources>

...

<FlipView x:Name="itemFlipView" 
          ItemsSource="{Binding Source={StaticResource itemsViewSource}}"/>
```

>**참고**&nbsp;&nbsp;항목을 해당 Items 컬렉션에 추가하거나 ItemsSource 속성을 설정하여 FlipView를 채울 수 있지만, 두 방법을 동시에 사용할 수는 없습니다. ItemsSource 속성을 설정하고 항목을 XAML에 추가하는 경우 추가된 항목이 무시됩니다. ItemsSource 속성을 설정하고 코드에서 항목을 Items 컬렉션에 추가하는 경우 예외가 발생합니다.

### <a name="specify-the-look-of-the-items"></a>항목의 모양 지정

기본적으로, 데이터 항목은 바운딩된 데이터 개체의 문자열 표현으로 대칭 이동 보기에 표시됩니다. 일반적으로 데이터를 보다 다양하게 표시하려는 경우가 많습니다. 대칭 이동 보기에서 항목이 표시되는 방법을 정확히 지정하려면 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx)을 만듭니다. DataTemplate의 XAML은 개별 항목을 표시하는 데 사용되는 컨트롤의 레이아웃 및 모양을 정의합니다. 레이아웃의 컨트롤은 데이터 개체의 속성에 바운딩되거나 콘텐츠가 정의된 인라인을 가질 수 있습니다. DataTemplate은 FlipView 컨트롤의 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 속성에 할당됩니다.

다음 예제에서는 FlipView의 ItemTemplate이 인라인으로 정의됩니다. 이미지 이름을 표시하는 오버레이가 이미지에 추가됩니다. 

```XAML
<FlipView x:Name="flipView1" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

데이터 템플릿에 의해 정의된 레이아웃의 모양은 다음과 같습니다.

전환 보기 데이터 템플릿

### <a name="set-the-orientation-of-the-flip-view"></a>대칭 이동 뷰 방향 설정

기본적으로 대칭 이동 보기는 가로로 전환됩니다. 세로로 전환되도록 하려면 세로 방향의 스택 패널을 대칭 이동 보기의 [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)로 사용합니다.

이 예제에서는 세로 방향의 스택 패널을 FlipView의 ItemsPanel로 지정하는 방법을 보여 줍니다.

```XAML
<FlipView x:Name="flipViewVertical" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    
    <!-- Use a vertical stack panel for vertical flipping. -->
    <FlipView.ItemsPanel>
        <ItemsPanelTemplate>
            <VirtualizingStackPanel Orientation="Vertical"/>
        </ItemsPanelTemplate>
    </FlipView.ItemsPanel>
    
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

세로 방향의 대칭 이동 보기 모양은 다음과 같습니다.

![세로 대칭 이동 보기 예제](images/controls_flipview_vertical.jpg)

## <a name="adding-a-context-indicator"></a>상황 표시 추가

대칭 이동 보기의 상황 표시를 사용하면 참조하기 쉽습니다. 표준 상황의 점은 대화형으로 작동하지 않습니다. 이 예제와 같이 가장 좋은 위치는 일반적으로 갤러리 아래의 가운데입니다.

![페이지 표시기 예제](images/controls_pageindicator.png)

대형 컬렉션(10-25개 항목 포함)의 경우 미리 보기 표시줄과 같이 더 많은 상황별 정보를 제공하는 표시기를 사용해 보세요. 간단한 점을 사용하는 상황 표시기와 달리 표시줄의 각 미리 보기는 해당 이미지의 작은 버전을 표시하며 선택 가능합니다.

![상황 표시기 예제](images/controls_contextindicator.jpg)

FlipView에 상황 표시기를 추가하는 방법을 보여 주는 예제 코드는 [XAML FlipView 샘플](http://go.microsoft.com/fwlink/p/?LinkID=311760)을 참조하세요.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

-   대칭 이동 보기는 최대 25개 항목으로 구성된 컬렉션에 가장 적합합니다.
-   각 항목을 전환하는 반복적인 동작이 번거로울 수 있으므로 대형 컬렉션의 경우에는 대칭 이동 보기 컨트롤을 사용하지 않도록 합니다. 수백 또는 수천 개의 이미지가 있는 사진 앨범의 경우는 예외입니다. 그리드 보기 레이아웃에서 사진을 선택하면 사진 앨범은 거의 항상 대칭 이동 보기로 바뀝니다. 기타 큰 컬렉션의 경우는 [목록 보기 또는 그리드 보기](lists.md)를 고려하세요.
-   상황 표시기의 경우
    -   점의 순서(또는 선택하는 시각적 마커)는 가로로 걸쳐 있는 갤러리 아래의 가운데에 배치될 경우 가장 원활하게 작동합니다.
    -   세로로 걸쳐 있는 갤러리에서 상황 표시를 원하는 경우 이미지 오른쪽의 가운데에서 가장 원활하게 작동합니다.
    -   강조된 점은 현재 항목을 나타냅니다. 일반적으로 강조된 점은 흰색이며 다른 점은 회색입니다.
    -   점의 수는 다를 수 있지만 사용자가 자신을 찾는 데 어려움을 겪을 정도로 너무 많은 수를 포함하지는 않습니다. 10개 점이 일반적으로 표시하는 최대 수입니다.

## <a name="globalization-and-localization-checklist"></a>세계화 및 지역화 검사 목록

<table>
<tr>
<th>양방향 고려 사항</th><td>RTL 언어용 표준 미러링을 사용합니다. 뒤로 및 앞으로 컨트롤은 언어 방향을 기반으로 하므로 RTL 언어의 경우 오른쪽 단추는 뒤로 탐색해야 하고 왼쪽 단추는 앞으로 탐색해야 합니다.</td>
</tr>

</table>

## <a name="get-the-sample-code"></a>샘플 코드 다운로드
* [XAML UI 기본 사항 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-articles"></a>관련 문서

- [목록에 대한 지침](lists.md)
- [**FlipView 클래스**](https://msdn.microsoft.com/library/windows/apps/br242678)



<!--HONumber=Dec16_HO1-->


