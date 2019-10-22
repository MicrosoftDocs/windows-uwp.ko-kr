---
Description: 템플릿을 사용하여 ListView 또는 GridView 컨트롤의 항목 모양을 수정합니다.
title: 항목 컨테이너 및 템플릿
label: Item containers and templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: d8eb818d-b62e-4314-a612-f29142dbd93f
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 761cd9e6d1fc92b4919f701fdd9f8f62078faedf
ms.sourcegitcommit: b8a4b0d5a65da297290b93d73c641df3c135a086
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531667"
---
# <a name="item-containers-and-templates"></a>항목 컨테이너 및 템플릿

 

**ListView** 및 **GridView** 컨트롤은 항목 정렬 방법(가로, 세로, 줄 바꿈 등) 및 사용자가 항목을 조작하는 방법을 관리하지만 개별 항목이 화면에 표시되는 모양은 관리하지 않습니다. 항목 시각화는 항목 컨테이너에 의해 관리됩니다. 항목을 목록 보기에 추가하는 경우 항목이 자동으로 컨테이너에 추가됩니다. ListView에 대한 기본 항목 컨테이너는 [ListViewItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem)입니다. GridView의 경우 해당 [GridViewItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem)입니다.

> **중요 API**: [ListView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [GridView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [ListViewItem 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.listviewitem), [GridViewItem 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.gridviewitem), [ItemTemplate 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate), [ItemContainerStyle 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)


> [!NOTE]
> ListView 및 GridView 모두 [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase) 클래스에서 파생되므로 동일한 기능을 갖지만 데이터를 다르게 표시합니다. 이 문서에서 목록 보기에 대해 논의할 때 다른 언급이 없는 한, 해당 정보가 ListView 및 GridView 컨트롤 둘 다에 적용됩니다. ListView 또는 ListViewItem과 같은 클래스를 참조할 수 있지만 그리드와 관련된 동일한 항목에 대해 *List* 접두사 *Grid*로 바꿀 수 있습니다(GridView 또는 GridViewItem). 

## <a name="listview-items-and-gridview-items"></a>ListView 항목 및 GridView 항목
위에서 설명한 대로 ListView 항목은 ListViewItem 컨테이너에 자동으로 배치되고, GridView 항목은 GridViewItem 컨테이너에 배치됩니다. 이러한 항목 컨테이너는 자체의 기본 제공 스타일 지정 및 상호 작용을 포함하지만 고도로 사용자 지정할 수 있는 컨트롤입니다. 그러나 사용자 지정하기 전에 ListViewItem 및 GridViewItem에 대한 추천 스타일 지정과 지침을 숙지해야 합니다.

- **ListViewItem** - 주로 텍스트 중심이며 길쭉한 모양의 항목입니다. 아이콘 또는 이미지가 텍스트의 왼쪽에 나타날 수 있습니다.
- **GridViewItem** - 일반적으로 정사각형 모양이거나 적어도 약간 긴 직사각형 모양의 항목입니다. 이미지 중심이며, 텍스트가 이미지 주위에 있거나 이미지와 겹쳐 있을 수 있습니다. 

## <a name="introduction-to-customization"></a>사용자 지정 소개
컨테이너 컨트롤(예: ListViewItem 및 GridViewItem)은 항목에 대해 표시되는 최종 시각적 개체를 만들기 위해 결합되는 두 가지 중요한 부분인 *데이터 템플릿* 및 *컨트롤 템플릿*으로 구성됩니다.

- **데이터 템플릿** - 목록 보기의 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 속성에 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)을 할당하여 개별 데이터 항목이 표시되는 방식을 지정합니다.
- **컨트롤 템플릿** - 컨트롤 템플릿은 시각적 상태와 같이 프레임워크에서 담당하는 항목 시각화의 일부를 제공합니다. [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) 속성을 사용하여 컨트롤 템플릿을 수정할 수 있습니다. 일반적으로 사용자의 브랜드에 맞게 목록 보기 색을 수정하거나 선택한 항목이 표시되는 방법을 변경하기 위해 이 작업을 수행합니다.

이 이미지는 컨트롤 템플릿과 데이터 템플릿을 결합하여 항목의 최종 시각적 개체를 만드는 방법을 보여 줍니다.

![목록 보기 컨트롤 및 데이터 템플릿](images/listview-visual-parts.png)

다음은 이 항목을 만드는 XAML입니다. 템플릿에 대해서는 나중에 설명합니다.

```xaml
<ListView Width="220" SelectionMode="Multiple">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid Background="Yellow">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="54"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="44" Height="44"
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Black"
                           FontSize="14" Grid.Column="1"
                           VerticalAlignment="Center"
                           Padding="0,0,54,0"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Background" Value="LightGreen"/>
        </Style>
    </ListView.ItemContainerStyle>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

> [!IMPORTANT]
> 데이터 템플릿 및 컨트롤 템플릿은 ListView 및 GridView 이외의 많은 컨트롤에 대한 스타일을 사용자 지정하는 데 사용됩니다. 여기에는 자체의 기본 제공 스타일 지정을 사용하는 컨트롤(예: FlipView) 및 사용자 지정하여 만든 컨트롤(예: ItemsRepeater)이 포함됩니다. 아래 예제는 ListView/GridView와 관련이 있지만, 개념은 다른 많은 컨트롤에 적용할 수 있습니다. 
 
## <a name="prerequisites"></a>필수 구성 요소

- 여기서는 목록 보기 컨트롤을 사용하는 방법을 알고 있다고 가정합니다. 자세한 내용은 [ListView 및 GridView](listview-and-gridview.md) 문서를 참조하세요.
- 또한 스타일 인라인 또는 리소스로 사용하는 방법 등 컨트롤 스타일 및 템플릿을 이해하고 있다고 가정합니다. 자세한 내용은 [컨트롤 스타일 지정](xaml-styles.md) 및 [컨트롤 템플릿](control-templates.md)을 참조하세요.

## <a name="the-data"></a>데이터

목록 보기에 데이터 항목을 표시하는 방법을 더 자세히 살펴보기 전에 표시할 데이터를 이해해야 합니다. 이 예제에서는 `NamedColor`라는 데이터 형식을 만듭니다. 3가지 속성 `Name`, `Color` 및 `Brush`로 표현되는 색 이름, 색 값 및 색의 **SolidColorBrush**를 결합합니다.
 
그런 다음, [Colors](https://docs.microsoft.com/uwp/api/windows.ui.colors) 클래스의 각 명명된 색에 대한 `NamedColor` 개체로 **List**를 채웁니다. 목록은 목록 보기에 대한 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)로 설정됩니다.

다음은 클래스를 정의하고 `NamedColors` 목록을 채우는 코드입니다.

**C#**
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

namespace ColorsListApp
{
    public sealed partial class MainPage : Page
    {
        // The list of colors won't change after it's populated, so we use List<T>. 
        // If the data can change, we should use an ObservableCollection<T> intead.
        List<NamedColor> NamedColors = new List<NamedColor>();

        public MainPage()
        {
            this.InitializeComponent();

            // Use reflection to get all the properties of the Colors class.
            IEnumerable<PropertyInfo> propertyInfos = typeof(Colors).GetRuntimeProperties();

            // For each property, create a NamedColor with the property name (color name),
            // and property value (color value). Add it the NamedColors list.
            for (int i = 0; i < propertyInfos.Count(); i++)
            {
                NamedColors.Add(new NamedColor(propertyInfos.ElementAt(i).Name,
                                    (Color)propertyInfos.ElementAt(i).GetValue(null)));
            }

            colorsListView.ItemsSource = NamedColors;
        }
    }

    class NamedColor
    {
        public NamedColor(string colorName, Color colorValue)
        {
            Name = colorName;
            Color = colorValue;
        }

        public string Name { get; set; }

        public Color Color { get; set; }

        public SolidColorBrush Brush
        {
            get { return new SolidColorBrush(Color); }
        }
    }
}
```

## <a name="data-template"></a>데이터 템플릿

목록 보기에 데이터 항목을 표시할 방법을 알리는 데이터 템플릿을 지정합니다. 

기본적으로 데이터 항목은 바운딩된 데이터 개체의 문자열 표현으로 목록 보기에 표시됩니다. 목록 보기에 모양을 알리지 않고 목록 보기에 'NamedColors' 데이터를 표시는 경우 다음과 같이 **ToString** 메서드의 반환 항목을 표시합니다.

**XAML**
```xaml
<ListView x:Name="colorsListView"/>
```

![항목의 문자열 표현을 나타내는 목록 보기](images/listview-no-template.png)

[DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath)를 해당 속성으로 설정하여 데이터 항목의 특정 속성에 대한 문자열 표현을 표시할 수 있습니다. 여기서는 DisplayMemberPath를 `NamedColor` 항목의 `Name` 속성으로 설정합니다.

**XAML**
```xaml
<ListView x:Name="colorsListView" DisplayMemberPath="Name" />
```

목록 보기에서는 이제 여기에 표시된 대로 항목을 이름으로 표시합니다. 이 방법이 더 유용하지만 많은 정보가 표시되지 않습니다.

![항목 속성의 문자열 표현을 나타내는 목록 보기](images/listview-display-member-path.png)

일반적으로 데이터를 보다 다양하게 표시하려는 경우가 많습니다. 목록 보기에서 항목이 표시되는 방법을 정확히 지정하려면 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)을 만듭니다. DataTemplate의 XAML은 개별 항목을 표시하는 데 사용되는 컨트롤의 레이아웃 및 모양을 정의합니다. 레이아웃의 컨트롤은 데이터 개체의 속성에 바운딩되거나 정적 콘텐츠 정의 인라인을 가질 수 있습니다. 목록 컨트롤의 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 속성에 DataTemplate를 할당합니다.

> [!IMPORTANT]
> **ItemTemplate** 및 **DisplayMemberPath**를 동시에 사용할 수 없습니다. 두 속성을 모두 설정한 경우 예외가 발생합니다.

여기서는 항목 색의 [Rectangle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.rectangle) 및 색 이름과 RGB 값을 표시하는 DataTemplate을 정의합니다. 

> [!NOTE]
> DataTemplate에서 [x:Bind 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)을 사용하는 경우 DataTemplate에서 DataType(`x:DataType`)을 지정해야 합니다.

**XAML**
```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:NamedColor">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition MinWidth="54"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Rectangle Width="44" Height="44" Fill="{x:Bind Brush}" Grid.RowSpan="2"/>
                <TextBlock Text="{x:Bind Name}" Grid.Column="1" Grid.ColumnSpan="4"/>
                <TextBlock Text="{x:Bind Color.R}" Grid.Column="1" Grid.Row="1" Foreground="Red"/>
                <TextBlock Text="{x:Bind Color.G}" Grid.Column="2" Grid.Row="1" Foreground="Green"/>
                <TextBlock Text="{x:Bind Color.B}" Grid.Column="3" Grid.Row="1" Foreground="Blue"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

이 데이터 템플릿을 사용하여 표시할 경우 데이터 항목의 모양은 다음과 같습니다.

![데이터 템플릿을 사용한 목록 보기 항목](images/listview-data-template-0.png)

> [!IMPORTANT]
> ListViewItem의 콘텐츠는 기본적으로 왼쪽에 맞추어집니다. 즉 [HorizontalContentAlignmentProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment#Windows_UI_Xaml_Controls_Control_HorizontalContentAlignment)가 Left로 설정되어 있습니다. 가로로 쌓은 요소 또는 동일한 Grid 행에 배치된 요소와 같이 ListViewItem 내에 가로로 인접한 여러 요소가 있는 경우 모두 왼쪽에 맞추어지고 정의된 여백으로만 구분됩니다. 
<br/><br/> 요소를 분산하여 ListItem의 전체 본문을 채우려면 ListView 내의 [Setter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.setter)를 사용하여 HorizontalContentAlignmentProperty를 [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.horizontalalignment)로 설정해야 합니다.

```xaml
<ListView.ItemContainerStyle>
    <Style TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>
</ListView.ItemContainerStyle>
```


GridView에 데이터를 표시할 수 있습니다. 다음은 그리드 레이아웃에 더 적합한 방식으로 데이터를 표시하는 데이터 템플릿입니다. 이때 데이터 템플릿은 GridView에 대한 XAML을 사용하여 인라인이 아닌 리소스로 정의됩니다.


**XAML**
```xaml
<Page.Resources>
    <DataTemplate x:Key="namedColorItemGridTemplate" x:DataType="local:NamedColor">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="96"/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
    
            <Rectangle Width="96" Height="96" Fill="{x:Bind Brush}" Grid.ColumnSpan="3" />
            <!-- Name -->
            <Border Background="#AAFFFFFF" Grid.ColumnSpan="3" Height="40" VerticalAlignment="Top">
                <TextBlock Text="{x:Bind Name}" TextWrapping="Wrap" Margin="4,0,0,0"/>
            </Border>
            <!-- RGB -->
            <Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
            <TextBlock Text="{x:Bind Color.R}" Foreground="Red"
                   Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.G}" Foreground="Green"
                   Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
                   Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
            <!-- HEX -->
            <Border Background="Gray" Grid.Row="2" Grid.ColumnSpan="3">
                <TextBlock Text="{x:Bind Color}" Foreground="White" Margin="4,0,0,0"/>
            </Border>
        </Grid>
    </DataTemplate>
</Page.Resources>

...

<GridView x:Name="colorsGridView" 
          ItemTemplate="{StaticResource namedColorItemGridTemplate}"/>
```

데이터가 이 데이터 템플릿을 사용하여 그리드에 표시되면 다음과 같은 모양이 됩니다.

![데이터 템플릿을 사용한 그리드 보기 항목](images/gridview-data-template.png)

### <a name="performance-considerations"></a>성능 고려 사항

데이터 템플릿은 목록 보기의 모양을 정의하는 기본적인 방법입니다. 목록에 많은 수의 항목이 표시되는 경우 데이터 템플릿이 성능에 큰 영향을 미칠 수도 있습니다. 

목록 보기에서 각 항목에 대해 데이터 템플릿의 모든 XAML 요소 인스턴스가 만들어집니다. 예를 들어 이전 예제의 그리드 템플릿에는 10개의 XAML 요소(Grid 1개, Rectangle 1개, Border 3개, TextBlock 5개)가 있습니다. 이 데이터 템플릿을 사용하여 화면에 20개의 항목을 표시하는 GridView는 최소 200개의 요소(20*10=200)를 만듭니다. 데이터 템플릿의 요소 수를 줄이면 목록 보기에 대해 만든 요소의 총 개수를 크게 줄일 수 있습니다. 자세한 내용은 [ListView 및 GridView UI 최적화: 항목당 요소 개수 감소](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)를 참조하세요.

 이 섹션에서 그리드 데이터 템플릿을 참조하세요. 요소 수를 줄일 수 있는 몇 가지 방법을 살펴보겠습니다.

**XAML**
```xaml
<!-- RGB -->
<Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
<TextBlock Text="{x:Bind Color.R}" Foreground="Red"
           Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.G}" Foreground="Green"
           Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
           Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
```

 - 먼저 레이아웃에서 단일 그리드를 사용합니다. 단일 열 그리드에서는 이러한 3개의 TextBlock을 StackPanel에 배치할 수 있지만 여러 번 생성되는 데이터 템플릿에서는 다른 레이아웃 패널 내에 레이아웃 패널이 포함되지 않도록 방지하는 방법을 찾아야 합니다.
 - 둘째, Border 컨트롤을 사용하여 실제로 Border 요소 내에 항목을 배치하지 않고 배경을 렌더링할 수 있습니다. Border 요소에는 자식 요소가 하나만 포함될 수 있으므로 XAML의 Border 요소 내에서 3개의 TextBlock 요소를 호스트하는 레이아웃 패널을 더 추가해야 합니다. TextBlock을 Border의 자식으로 만들지 않으면 TextBlock을 저장하는 패널이 필요하지 않습니다.
 - 마지막으로 StackPanel 내에 TextBlock을 배치하고 명시적 Border 요소를 사용하는 대신 StackPanel의 테두리 속성을 설정할 수 있습니다. 그러나 Border 요소는 StackPanel보다 훨씬 간단한 컨트롤이므로 여러 번 렌더링해도 성능에 대한 영향이 적습니다.

### <a name="using-different-layouts-for-different-items"></a>항목마다 다른 레이아웃 사용


## <a name="control-template"></a>컨트롤 템플릿
항목의 컨트롤 템플릿에는 선택 항목, 포인터 가리키기 및 포커스와 같은 상태를 표시하는 시각적 개체가 포함됩니다. 이러한 시각적 개체는 데이터 템플릿의 위 또는 아래에 렌더링됩니다. 다음은 ListView 컨트롤 템플릿에서 그리는 일부 일반적인 기본 시각적 개체입니다.

- 가리키기 - 데이터 템플릿 아래에 그려지는 밝은 회색 사각형입니다.  
- 선택 – 데이터 템플릿 아래에 그려지는 밝은 파란색 사각형입니다. 
- 키보드 포커스 - 항목 템플릿 위에 표시되는 흑백 점선입니다. 

![목록 보기 상태 시각적 개체](images/listview-state-visuals.png)

목록 보기는 데이터 템플릿의 요소와 컨트롤 템플릿을 결합하여 화면에 렌더링되는 최종 시각적 개체를 만듭니다. 여기에서 상태 시각적 개체는 목록 보기의 컨텍스트에서 표시됩니다.

![다양한 상태의 항목이 있는 목록 보기](images/listview-states.png)

### <a name="listviewitempresenter"></a>ListViewItemPresenter

앞에서 데이터 템플릿에 대해 설명한 것처럼 각 항목에 대해 만들어지는 XAML 요소의 수는 목록 보기의 성능에 큰 영향을 미칠 수 있습니다. 데이터 템플릿과 컨트롤 템플릿을 결합하여 각 항목을 표시하므로 항목을 표시하는 데 필요한 실제 요소 수에는 두 템플릿의 요소가 모두 포함됩니다.

ListView 및 GridView 컨트롤은 항목당 만들어지는 XAML 요소의 수를 줄이도록 최적화됩니다. **ListViewItem** 화면 효과는 많은 UIElement 오버헤드 없이 포커스, 선택 항목 및 다른 시각적 상태에 대한 복잡한 화면 효과를 표시하는 특수 XAML 요소인 [ListViewItemPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter)를 통해 생성됩니다.
 
> [!NOTE]
> Windows 10용 UWP 앱에서 **ListViewItem** 및 **GridViewItem**은 모두 **ListViewItemPresenter**를 사용합니다. GridViewItemPresenter는 더 이상 사용되지 않습니다. ListViewItem 및 GridViewItem은 ListViewItemPresenter에 다양한 속성 값을 설정하여 다양한 기본 모양을 표시합니다.

항목 컨테이너의 모양을 수정하려면 [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) 속성을 사용하고, [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.targettype)이 **ListViewItem** 또는 **GridViewItem**으로 설정된 [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style)을 제공합니다.

이 예제에서는 ListViewItem에 안쪽 여백을 추가하여 목록의 항목 사이에 일부 간격을 만듭니다.

```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <!-- DataTemplate XAML shown in previous ListView example -->
    </ListView.ItemTemplate>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Padding" Value="0,4"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

이제 목록 보기에서 항목 사이에 간격이 생깁니다.

![안쪽 여백이 적용된 목록 보기 항목](images/listview-data-template-1.png)

ListViewItem 기본 스타일에서 ListViewItemPresenter **ContentMargin** 속성의 [TemplateBinding](https://docs.microsoft.com/windows/uwp/xaml-platform/templatebinding-markup-extension)은 ListViewItem **Padding** 속성(`<ListViewItemPresenter ContentMargin="{TemplateBinding Padding}"/>`)으로 설정되어 있습니다. Padding 속성을 설정할 때 해당 값은 실제로 ListViewItemPresenter ContentMargin 속성에 전달됩니다.

템플릿에 바인딩되지 않는 다른 ListViewItemPresenter 속성을 ListViewItems 속성으로 수정하려면 속성을 수정할 수 있는 새 ListViewItemPresenter를 사용하여 ListViewItem의 템플릿을 다시 작성해야 합니다. 

> [!NOTE]
> ListViewItem 및 GridViewItem 기본 스타일은 ListViewItemPresenter에 대해 많은 속성을 설정합니다. 항상 기본 스타일의 복사본을 사용하여 시작한 다음 필요한 속성만 수정해야 합니다. 그러지 않으면 일부 속성이 올바르게 설정되지 않으므로 시각적 개체가 예상대로 표시되지 않습니다.

**Visual Studio에서 기본 템플릿의 복사본을 만들려면**
 
1. 문서 개요 창(**보기 &gt; 다른 창 &gt; 문서 개요**)을 엽니다.
2. 수정할 목록 또는 그리드 요소를 선택합니다. 이 예제에서는 `colorsGridView` 요소를 선택합니다.
3. 마우스 오른쪽 단추를 클릭하고 **추가 템플릿 편집 &gt; 생성된 항목 컨테이너 편집(ItemContainerStyle) &gt; 복사본 편집**을 선택합니다.
    ![Visual Studio 편집기](images/listview-itemcontainerstyle-vs.png)
4. 스타일 리소스 만들기 대화 상자에서 스타일의 이름을 입력합니다. 이 예제에서는 `colorsGridViewItemStyle`을 사용합니다.
    ![Visual Studio Create Style Resource dialog(images/listview-style-resource-vs.png)

이 XAML에 표시된 대로 기본 스타일의 복사본이 앱에 리소스로 추가되고 **GridView.ItemContainerStyle** 속성이 해당 리소스로 설정됩니다. 

```xaml
<Style x:Key="colorsGridViewItemStyle" TargetType="GridViewItem">
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="Foreground" Value="{ThemeResource SystemControlForegroundBaseHighBrush}"/>
    <Setter Property="TabNavigation" Value="Local"/>
    <Setter Property="IsHoldingEnabled" Value="True"/>
    <Setter Property="HorizontalContentAlignment" Value="Center"/>
    <Setter Property="VerticalContentAlignment" Value="Center"/>
    <Setter Property="Margin" Value="0,0,4,4"/>
    <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
    <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="GridViewItem">
                <ListViewItemPresenter 
                    CheckBrush="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                    ContentMargin="{TemplateBinding Padding}" 
                    CheckMode="Overlay" 
                    ContentTransitions="{TemplateBinding ContentTransitions}" 
                    CheckBoxBrush="{ThemeResource SystemControlBackgroundChromeMediumBrush}" 
                    DragForeground="{ThemeResource ListViewItemDragForegroundThemeBrush}" 
                    DragOpacity="{ThemeResource ListViewItemDragThemeOpacity}" 
                    DragBackground="{ThemeResource ListViewItemDragBackgroundThemeBrush}" 
                    DisabledOpacity="{ThemeResource ListViewItemDisabledThemeOpacity}" 
                    FocusBorderBrush="{ThemeResource SystemControlForegroundAltHighBrush}" 
                    FocusSecondaryBorderBrush="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}" 
                    PointerOverForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    PressedBackground="{ThemeResource SystemControlHighlightListMediumBrush}" 
                    PlaceholderBackground="{ThemeResource ListViewItemPlaceholderBackgroundThemeBrush}" 
                    PointerOverBackground="{ThemeResource SystemControlHighlightListLowBrush}" 
                    ReorderHintOffset="{ThemeResource GridViewItemReorderHintThemeOffset}" 
                    SelectedPressedBackground="{ThemeResource SystemControlHighlightListAccentHighBrush}" 
                    SelectionCheckMarkVisualEnabled="True" 
                    SelectedForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    SelectedPointerOverBackground="{ThemeResource SystemControlHighlightListAccentMediumBrush}" 
                    SelectedBackground="{ThemeResource SystemControlHighlightAccentBrush}" 
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>

...

<GridView x:Name="colorsGridView" ItemContainerStyle="{StaticResource colorsGridViewItemStyle}"/>
```

이제 ListViewItemPresenter의 속성을 수정하여 선택 확인란, 항목 위치 및 브러시 색 등 시각적 상태를 제어할 수 있습니다. 

#### <a name="inline-and-overlay-selection-visuals"></a>인라인 및 오버레이 선택 시각적 개체

ListView 및 GridView는 컨트롤 및 [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode)에 따라 다른 방법으로 선택된 항목을 나타냅니다. 목록 보기 선택에 대한 자세한 내용은 [ListView 및 GridView](listview-and-gridview.md)를 참조하세요. 

**SelectionMode**가 **Multiple**로 설정된 경우 선택 확인란이 항목에 대한 컨트롤 템플릿의 일부로 표시됩니다. [SelectionCheckMarkVisualEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) 속성을 사용하여 Multiple 선택 모드의 선택 확인란을 끌 수 있습니다. 그러나 다른 선택 모드에서는 이 속성이 무시되므로 Extended 또는 Single 선택 모드에서 확인란을 켤 수 없습니다.

[CheckMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode) 속성을 설정하여 확인란을 인라인 스타일 또는 오버레이 스타일을 사용하여 표시할지 여부를 지정할 수 있습니다.

- **Inline**: 이 스타일은 콘텐츠 왼쪽에 확인란을 표시하고 항목 컨테이너의 배경색을 통해 선택을 나타냅니다. ListView의 기본 스타일입니다.
- **Overlay**: 이 스타일은 콘텐츠 위에 확인란을 표시하고 항목 컨테이너의 테두리 색을 통해 선택을 나타냅니다. GridView의 기본 스타일입니다.

이 표에서는 선택을 나타내는 데 사용되는 기본 시각적 개체를 보여 줍니다.

SelectionMode:&nbsp;&nbsp; | Single/Extended | Multiple
---------------|-----------------|---------
인라인 | ![인라인 단일 또는 확장 선택](images/listview-single-selection.png) | ![인라인 다중 선택](images/listview-multi-selection.png)
오버레이 | ![오버레이 단일 또는 확장 선택](images/gridview-single-selection.png) | ![오버레이 다중 선택](images/gridview-multi-selection.png)

> [!NOTE]
> 이 예제와 다음 예제에서는 간단한 문자열 데이터 항목이 데이터 템플릿 없이 표시되어 컨트롤 템플릿에서 제공하는 시각적 개체를 강조합니다.

또한 여러 가지 브러시 속성을 사용하여 확인란의 색을 변경할 수도 있습니다. 이러한 기능에 대해서는 다음에 다른 브러시 속성과 함께 살펴보겠습니다.

#### <a name="brushes"></a>브러시 

다양한 속성을 사용하여 여러 시각적 상태에 사용되는 브러시를 지정합니다. 해당 브랜드의 색에 맞게 수정할 수도 있습니다. 

이 표에서는 ListViewItem에 대한 공통 및 선택 시각적 상태와 각 상태에 대한 시각적 개체를 렌더링하는 데 사용되는 브러시를 보여 줍니다. 이 이미지는 인라인 및 오버레이 선택 시각적 스타일에 적용한 브러시 효과를 보여 줍니다.

> [!NOTE]
> 이 표에서 브러시에 대해 수정된 색 값은 하드코드된 명명된 색이며 색은 템플릿에 적용되는 위치를 더 명확하게 표시하기 위해 선택됩니다. 시각적 상태의 기본 색은 아닙니다. 앱에서 기본 색을 수정하는 경우 기본 템플릿에서 수행한 것처럼 브러시 리소스를 사용하여 색 값을 수정해야 합니다.

상태/브러시 이름 | 인라인 스타일 | 오버레이 스타일
------------|--------------|--------------
<b>Normal</b><ul><li><b>CheckBoxBrush=“Red”</b></li></ul> | ![인라인 항목 선택 일반](images/listview-item-normal.png) | ![오버레이 항목 선택 일반](images/gridview-item-normal.png)
<b>PointerOver</b><ul><li><b>PointerOverForeground=“DarkOrange”</b></li><li><b>PointerOverBackground=“MistyRose”</b></li><li>CheckBoxBrush="Red"</li></ul> | ![인라인 항목 선택 포인터 가리키기](images/listview-item-pointerover.png) | ![오버레이 항목 선택 포인터 가리키기](images/gridview-item-pointerover.png)
<b>Pressed</b><ul><li><b>PressedBackground=“LightCyan”</b></li><li>PointerOverForeground="DarkOrange"</li><li>CheckBoxBrush="Red"</li></ul> | ![인라인 항목 선택 누름](images/listview-item-pressed.png) | ![오버레이 항목 선택 누름](images/gridview-item-pressed.png)
<b>Selected</b><ul><li><b>SelectedForeground=“Navy”</b></li><li><b>SelectedBackground=“Khaki”</b></li><li><b>CheckBrush=“Green”</b></li><li>CheckBoxBrush="Red"(인라인만 해당)</li></ul> | ![인라인 항목 선택 선택됨](images/listview-item-selected.png) | ![오버레이 항목 선택 선택됨](images/gridview-item-selected.png)
<b>PointerOverSelected</b><ul><li><b>SelectedPointerOverBackground=“Lavender”</b></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki"(오버레이만 해당)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red"(인라인만 해당)</li></ul> | ![인라인 항목 선택 포인터 가리키기 선택됨](images/listview-item-pointeroverselected.png) | ![오버레이 항목 선택 포인터 가리키기 선택됨](images/gridview-item-pointeroverselected.png)
<b>PressedSelected</b><ul><li><b>SelectedPressedBackground=“MediumTurquoise”</b></li></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki"(오버레이만 해당)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red"(인라인만 해당)</li></ul> | ![인라인 항목 선택 누름 선택됨](images/listview-item-pressedselected.png) | ![오버레이 항목 선택 누름 선택됨](images/gridview-item-pressedselected.png)
<b>Focused</b><ul><li><b>FocusBorderBrush=“Crimson”</b></li><li><b>FocusSecondaryBorderBrush=“Gold”</b></li><li>CheckBoxBrush="Red"</li></ul> | ![인라인 항목 선택 포커스 있음](images/listview-item-focused.png) | ![오버레이 항목 선택 포커스 있음](images/gridview-item-focused.png)

ListViewItemPresenter에는 데이터 자리 표시자 및 끌기 상태에 대한 다른 브러시 속성이 있습니다. 목록 보기에서 증분 로드 또는 끌어서 놓기를 사용하는 경우 이러한 추가 브러시 속성도 수정해야 하는지 고려해야 합니다. 수정할 수 있는 전체 속성 목록은 ListViewItemPresenter 클래스를 참조하세요. 

### <a name="expanded-xaml-item-templates"></a>확장된 XAML 항목 템플릿

예를 들어 확인란의 위치를 변경해야 하는 등 **ListViewItemPresenter** 속성에서 허용된 것보다 더 많이 수정해야 하는 경우 *ListViewItemExpanded* 또는 *GridViewItemExpanded* 템플릿을 사용할 수 있습니다. 이러한 템플릿은 기본 스타일과 함께 generic.xaml에 포함되어 있습니다. 개별 UIElement를 사용하여 모든 시각적 개체를 빌드하는 표준 XAML 패턴을 따릅니다.

앞서 언급했듯이 항목 템플릿의 UIElements 수는 목록 보기의 성능에 큰 영향을 줍니다. ListViewItemPresenter를 확장된 XAML 템플릿으로 바꾸면 요소 수가 크게 증가하므로 목록 보기에 표시되는 항목 수가 많거나 성능이 중요한 경우에는 권장되지 않습니다.

> [!NOTE]
> **ListViewItemPresenter**는 목록 보기의 [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)이 [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid) 또는 [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel)인 경우에만 지원됩니다. ItemsPanel을 [VariableSizedWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid), [WrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.wrapgrid) 또는 [StackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel)로 변경하면 항목 템플릿이 자동으로 확장된 XAML 템플릿으로 전환됩니다. 자세한 내용은 [ListView 및 GridView UI 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)를 참조하세요.

확장된 XAML 템플릿을 사용자 지정하려면 앱에서 해당 템플릿의 복사본을 만든 다음 **ItemContainerStyle** 속성을 복사본으로 설정해야 합니다.

**확장된 템플릿을 복사하려면**
1. ListView 또는 GridView에 대해 다음과 같이 ItemContainerStyle 속성을 설정합니다.
    ```xaml
    <ListView ItemContainerStyle="{StaticResource ListViewItemExpanded}"/>
    <GridView ItemContainerStyle="{StaticResource GridViewItemExpanded}"/>
    ```
2. Visual Studio 속성 창에서 기타 섹션을 확장하고 ItemContainerStyle 속성을 찾습니다. ListView 또는 GridView가 선택되어 있는지 확인합니다.
3. ItemContainerStyle 속성에 대한 속성 마커를 클릭합니다. 속성 마커는 텍스트 상자 옆에 있는 작은 상자입니다. StaticResource로 설정된 경우 녹색으로 표시됩니다. 속성 메뉴가 열립니다.
4. 속성 메뉴에서 **새 리소스로 변환**을 클릭합니다. 
    
    ![Visual Studio 속성 메뉴](images/listview-convert-resource-vs.png)
5. 스타일 리소스 만들기 대화 상자에서 리소스의 이름을 입력하고 확인을 클릭합니다.

앱에서 generic.xaml을 사용하여 확장된 템플릿의 복사본이 만들어지면 필요에 따라 수정할 수 있습니다.


## <a name="related-articles"></a>관련 문서

- [목록](lists.md)
- [ListView 및 GridView](listview-and-gridview.md)

