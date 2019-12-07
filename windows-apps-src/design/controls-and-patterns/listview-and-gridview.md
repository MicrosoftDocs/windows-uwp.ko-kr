---
Description: ListView 및 GridView 컨트롤을 사용하여 이미지 갤러리 또는 메일 메시지 세트와 같은 데이터 세트를 표시하고 조작합니다.
title: 목록 보기 및 그리드 보기
label: List view and grid view
template: detail.hbs
ms.date: 11/04/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e95a9a1f6a0d34e377f48c5b19497eb638fb186e
ms.sourcegitcommit: 27cb7c4539bb6417d32883824ccea160bb948c15
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74830825"
---
# <a name="list-view-and-grid-view"></a>목록 보기 및 그리드 보기

대부분의 응용 프로그램은 이미지 갤러리, 메일 메시지 집합 등과 같은 데이터 집합을 조작하고 표시합니다. XAML UI 프레임워크는 앱에 데이터를 쉽게 표시하고 조작할 수 있도록 하는 ListView 및 GridView 컨트롤을 제공합니다.  

> **중요 API**: [ListView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [GridView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [ItemsSource 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), [Items 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items)

> [!NOTE]
> ListView 및 GridView 모두 [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase) 클래스에서 파생되므로 동일한 기능을 갖지만 데이터를 다르게 표시합니다. 이 문서에서 목록 보기에 대해 논의할 때 다른 언급이 없는 한, 해당 정보가 ListView 및 GridView 컨트롤 둘 다에 적용됩니다. ListView 또는 ListViewItem과 같은 클래스를 참조할 수 있지만 그리드와 관련된 동일한 항목에 대해 *List* 접두사 *Grid*로 바꿀 수 있습니다(GridView 또는 GridViewItem). 

ListView 및 GridView는 컬렉션 작업에서 많은 이점을 누릴 수 있습니다. 둘 다 구현하기 쉽고 기본 UI, 상호 작용 및 스크롤 기능을 제공하며 여전히 쉽게 사용자 지정할 수 있습니다. ListView 및 GridView는 기존 동적 데이터 원본 또는 XAML 자체에 제공되는 하드 코드된 데이터/코드 숨김으로 바인딩할 수 있습니다. 

두 컨트롤은 유연하기 때문에 여러 사용 사례에 사용할 수 있지만, 모든 항목의 기본 구조와 모양이 동일하고 상호 작용 동작이 동일한 컬렉션에 가장 적합합니다. 즉, 두 컨트롤은 클릭 시 동일한 작업을 수행해야 합니다(링크 열기, 탐색 등).


## <a name="differences-between-listview-and-gridview"></a>ListView와 GridView의 차이점

### <a name="listview"></a>ListView
ListView는 데이터를 세로로 쌓으면서 단일 막대에 표시합니다. ListView는 텍스트를 초점으로 포함하고 있는 항목과 위에서 아래로 읽어야 하는(즉, 사전순으로 정렬되는) 컬렉션에 더 적합합니다. ListView의 일반적인 사용 사례로 메시지 목록과 검색 결과를 들 수 있습니다.

![그룹화된 데이터를 사용한 목록 보기](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
GridView는 세로로 스크롤할 수 있는 행과 열로 항목 컬렉션을 제공합니다. 열을 채울 때까지 데이터를 가로로 쌓은 후 다음 행을 계속 채웁니다. GridView는 이미지를 초점으로 포함하고 있는 항목과 옆에서 옆으로 읽을 수 있거나 특정 순서로 정렬되지 않는 컬렉션에 더 적합합니다. GridView의 일반적인 사용 사례로 사진 또는 제품 갤러리를 들 수 있습니다.

![콘텐츠 라이브러리의 예](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>어떤 컬렉션 컨트롤을 사용해야 하나요? ItemsRepeater와 비교

ListView 및 GridView는 별도의 작업 없이도 기본 제공 UI 및 UX를 사용하여 모든 컬렉션을 표시할 수 있는 컨트롤입니다. [ItemsRepeater](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) 컨트롤 역시 컬렉션을 표시하는 데 사용되지만, UI 요구 사항과 정확하게 일치하는 사용자 지정 컨트롤을 만들기 위한 빌딩 블록으로 생성되었습니다. 최종적으로 사용할 컨트롤에 영향을 미치는 가장 결정적인 차이점은 아래와 같습니다.

-   ListView 및 GridView는 사용자 지정이 거의 필요 없으면서도 수많은 기능을 제공하는 컨트롤입니다. ItemsRepeater는 사용자 고유의 레이아웃 컨트롤을 만들기 위한 빌딩 블록이며, 동일한 기본 기능을 갖고 있지 않습니다. 사용자가 필요한 기능 또는 상호 작용을 구현해야 합니다.
-   ListView 또는 GridView로는 만들 수 없는 사용자 지정 UI가 있거나 각 항목에 전혀 다른 동작을 요구하는 데이터 원본이 있는 경우에는 ItemsRepeater를 사용해야 합니다.


ItemsRepeater에 대한 자세한 내용은 해당 [지침](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) 및 [API 설명서](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2) 페이지를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 <a href="xamlcontrolsgallery:/item/ListView">ListView</a> 또는 <a href="xamlcontrolsgallery:/item/GridView">GridView</a>의 기능을 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>ListView 또는 GridView 만들기

ListView 및 GridView는 모두 [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol) 형식이므로 모든 형식의 항목 컬렉션을 포함할 수 있습니다. ListView 또는 GridView는 화면에 무언가를 표시하려면 해당 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션에 항목이 있어야 합니다. 보기를 채우려면 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션에 직접 항목을 추가하거나 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 데이터 원본에 설정할 수 있습니다. 

> [!IMPORTANT]
> Items 또는 ItemsSource를 사용하여 목록을 채울 수 있지만 두 속성을 동시에 사용할 수는 없습니다. ItemsSource 속성을 설정하고 항목을 XAML에 추가하는 경우 추가된 항목이 무시됩니다. ItemsSource 속성을 설정하고 코드에서 항목을 Items 컬렉션에 추가하는 경우 예외가 발생합니다.

이 문서의 많은 예제는 간편성을 위해 `Items` 컬렉션을 직접 채웁니다. 그러나 온라인 데이터베이스의 책 목록처럼 목록의 항목을 동적 원본에서 가져오는 것이 더 일반적입니다. 이를 위해 `ItemsSource` 속성을 사용합니다. 

### <a name="add-items-to-a-listview-or-gridview"></a>ListView 또는 GridView에 항목 추가

XAML 또는 코드를 통해 ListView 또는 GridView의 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션에 항목을 추가하여 동일한 결과를 얻을 수 있습니다. 변경되지 않고 쉽게 정의되는 항목 수가 적은 경우 또는 런타임 시 코드에서 항목을 생성하는 경우 일반적으로 XAML을 통해 항목을 추가합니다. 

<u>방법 1: Items 컬렉션에 항목 추가</u>
#### <a name="option-1-add-items-through-xaml"></a>옵션 1: XAML을 통해 항목 추가
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>옵션 2: C#을 통해 항목 추가

##### <a name="c-code"></a>C# 코드:
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>해당 XAML 코드:
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
위의 두 옵션 모두 아래와 같이 동일한 ListView를 생성합니다.

![간단한 목록 보기](images/listview-basic-code-example2.png)
<br/>
<u> 방법 2: ItemsSource를 설정하여 항목 추가</u>

일반적으로 ListView 또는 GridView를 사용하여 데이터베이스나 인터넷과 같은 원본의 데이터를 표시합니다. 데이터 원본에서 ListView/GridView를 채우려면 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 데이터 항목의 컬렉션으로 설정합니다. 아래 예제에 표시된 것처럼 ListView 또는 GridView가 사용자 지정 클래스 개체를 보유하게 되는 경우 이 방법이 더 적합합니다.

#### <a name="option-1-set-itemssource-in-c"></a>옵션 1: C#에서 ItemsSource 설정
여기서 목록 보기의 ItemsSource는 코드에서 컬렉션의 인스턴스로 직접 설정됩니다. 

##### <a name="c-code"></a>C# 코드:
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>XAML 코드:
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>옵션 2: XAML에서 ItemsSource 설정
XAML에서 ItemsSource 속성을 컬렉션에 바인딩할 수도 있습니다. 여기서 ItemsSource는 Page의 프라이빗 데이터 컬렉션 `_contacts`를 노출하는 `Contacts`라는 public 속성에 바인딩됩니다.

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

위의 두 옵션 모두 아래와 같이 동일한 ListView를 생성합니다. 데이터 템플릿을 제공하지 않았기 때문에 ListView는 각 항목의 문자열 표현만 표시합니다.

![ItemsSource가 설정된 간단한 목록 보기](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> 데이터 템플릿이 정의되지 않은 경우 사용자 지정 클래스 개체는 [ToString()](https://docs.microsoft.com/uwp/api/windows.foundation.istringable.tostring) 메서드가 정의된 경우 해당 문자열 값을 사용하여 ListView에만 표시됩니다.

 다음 섹션에서는 ListView 또는 GridView에서 간단한 사용자 지정 클래스 항목을 시각적으로 표시하는 방법을 자세하 살펴봅니다.

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 개요](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)를 참조하세요.

> [!NOTE]
> 그룹화된 데이터를 ListView에 표시해야 하는 경우 [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)에 바인딩해야 합니다. CollectionViewSource는 XAML의 컬렉션 클래스에 대해 프록시로 작용하고 그룹화 지원을 사용 가능하게 설정합니다. 자세한 내용은 [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)를 참조하세요.

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>DataTemplate을 사용하여 항목의 모양을 사용자 지정

ListView 또는 GridView의 데이터 템플릿은 항목/데이터를 시각화하는 방법을 정의합니다. 기본적으로 데이터 항목은 바인딩된 데이터 개체의 문자열 표현으로 ListView에 표시됩니다. [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath)를 해당 속성으로 설정하여 데이터 항목의 특정 속성에 대한 문자열 표현을 표시할 수 있습니다.

그러나 일반적으로 데이터를 보다 다양하게 표시하려는 경우가 많습니다. ListView/GridView에서 항목이 표시되는 방법을 정확히 지정하려면 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)을 만듭니다. DataTemplate의 XAML은 개별 항목을 표시하는 데 사용되는 컨트롤의 레이아웃 및 모양을 정의합니다. 레이아웃의 컨트롤은 데이터 개체의 속성에 바운딩되거나 정적 콘텐츠 정의 인라인을 가질 수 있습니다. 

> [!NOTE]
> DataTemplate에서 [x:Bind 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)을 사용하는 경우 DataTemplate에서 DataType(`x:DataType`)을 지정해야 합니다.

#### <a name="simple-listview-data-template"></a>간단한 ListView 데이터 템플릿
이 예제에서는 데이터 항목이 간단한 문자열입니다. DataTemplate은 ListView 정의 내에 인라인으로 정의되어 문자열 왼쪽에 이미지를 추가하고 문자열을 청록색으로 표시합니다. 이것은 위에서 방법 1과 옵션 1을 사용하여 만든 것과 동일한 ListView입니다.

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

ListView에서 이 데이터 템플릿을 사용하여 표시할 경우 데이터 항목의 모양은 다음과 같습니다.

![데이터 템플릿을 사용한 ListView 항목](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>사용자 지정 클래스 개체에 대한 ListView 데이터 템플릿
이 예제에서는 데이터 항목이 Contact 개체입니다. DataTemplate은 ListView 정의 내에 인라인으로 정의되어 연락처 이름 및 회사 왼쪽에 연락처 이미지를 추가합니다. 이 ListView는 위에서 언급한 방법 2와 옵션 2를 사용하여 만들었습니다.
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

ListView에서 이 데이터 템플릿을 사용하여 표시할 경우 데이터 항목의 모양은 다음과 같습니다.

![데이터 템플릿을 사용한 ListView 사용자 지정 클래스 항목](images/listview-customclass-datatemplate-final.png)

데이터 템플릿은 ListView의 모양을 정의하는 기본적인 방법입니다. 또한 목록에 많은 수의 항목이 있는 경우 데이터 템플릿이 성능에 큰 영향을 미칠 수 있습니다.  

데이터 템플릿은 위에서 보여드린 것처럼 ListView/GridView 정의 내에서 인라인으로 정의할 수도 있고, 리소스 섹션에서 별도로 정의할 수도 있습니다. ListView/GridView 외부에서 정의한 경우 DataTemplate에 [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) 특성을 부여하고 해당 키를 사용하여 DataTemplate을 ListView 또는 GridView의 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 속성에 할당해야 합니다.

데이터 템플릿 및 항목 컨테이너를 사용하여 목록이나 그리드의 항목 모양을 정의하는 방법에 대한 자세한 내용과 예제는 [항목 컨테이너 및 템플릿](item-containers-templates.md)을 참조하세요. 

## <a name="change-the-layout-of-items"></a>항목의 레이아웃 변경

ListView 또는 GridView에 항목을 추가하면 컨트롤이 항목 컨테이너의 각 항목을 자동으로 줄 바꿈한 후 모든 항목 컨테이너를 배치합니다. 이러한 항목 컨테이너가 배치되는 방식은 컨트롤의 [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)에 따라 좌우됩니다.  
- 기본적으로 **ListView**는 다음과 같이 세로 목록을 생성하는 [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel)을 사용합니다.

![간단한 목록 보기](images/listview-simple.png)

- **GridView**는 다음과 같이 항목을 가로로 추가하고 세로로 줄 바꿈 및 스크롤하는 [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid)를 사용합니다.

![간단한 그리드 보기](images/gridview-simple.png)

항목 패널에서 속성을 조정하여 항목의 레이아웃을 수정하거나 기본 패널을 다른 패널로 바꿀 수 있습니다.

> [!NOTE]
> ItemsPanel을 변경하는 경우 가상화를 해제하지 않도록 주의해야 합니다. **ItemsStackPanel** 및 **ItemsWrapGrid** 둘 다 가상화를 지원하므로 안전하게 사용할 수 있습니다. 다른 패널을 사용하는 경우 목록 보기의 성능이 느려질 수 있으므로 가상화를 사용하지 않도록 설정할 수 있습니다. 자세한 내용은 [성능](https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui) 아래의 목록 보기 문서를 참조하세요. 

이 예제에서는 **ItemsStackPanel**의 [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation) 속성을 변경하여 **ListView**의 항목 컨테이너를 가로 목록으로 레이아웃하는 방법을 보여 줍니다.
목록 보기는 기본적으로 세로로 스크롤되므로 목록 보기의 내부 [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer)에 대한 일부 속성을 조정하여 가로로 스크롤되도록 해야 합니다.
- [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode)를 **Enabled** 또는 **Auto**로
- [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility)를 **Auto**로 
- [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode)를 **Disabled**로 
- [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility)를 **Hidden**으로 

> [!IMPORTANT]
> 이러한 예제는 목록 보기 너비에 대한 제한 없이 표시되므로 가로 스크롤 막대가 표시되지 않습니다. 이 코드를 실행하는 경우 ListView에 대해 `Width="180"`을 설정하여 스크롤 막대가 표시되도록 할 수 있습니다.

**XAML**
```xml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

결과 목록은 다음과 같습니다.

![가로 목록 보기](images/listview-horizontal2-final.png)

 다음 예제에서 **ListView**는 **ItemsStackPanel** 대신 **ItemsWrapGrid**를 사용하여 항목을 세로 줄 바꿈 목록으로 배치합니다. 
 
> [!IMPORTANT]
> 컨트롤이 강제로 컨테이너를 줄 바꿈하도록 목록 보기의 높이를 제한해야 합니다.

**XAML**
```xml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

결과 목록은 다음과 같습니다.

![그리드 레이아웃을 사용한 목록 보기](images/listview-itemswrapgrid2-final.png)

그룹화된 데이터를 목록 보기로 표시하는 경우 ItemsPanel은 개별 항목이 배치되는 방식이 아니라 항목 그룹이 배치되는 방식을 결정합니다. 예를 들어 이전에 표시되던 가로 ItemsStackPanel이 그룹화된 데이터를 표시하는 데 사용될 경우 그룹은 가로로 정렬되지만 각 그룹의 항목은 아래와 같이 세로로 쌓입니다.

![그룹화된 가로 목록 보기](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>항목 선택 및 조작

다양한 방법 중에서 목록 보기를 조작하는 방법을 선택할 수 있습니다. 기본적으로 사용자는 단일 항목을 선택할 수 있습니다. [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) 속성을 변경하여 다중 선택을 사용 가능하게 하거나 선택을 사용 불가능하게 할 수 있습니다. 항목을 선택하는 대신 항목을 클릭하여 작업(예: 단추)을 호출하도록 [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 속성을 설정할 수 있습니다.

> **참고**&nbsp;&nbsp;ListView와 GridView는 둘 다 SelectionMode 속성에 대해 [ListViewSelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewselectionmode) 열거형을 사용합니다. IsItemClickEnabled는 기본적으로 **False**이므로 클릭 모드를 사용 가능하게 설정하려는 경우에만 설정하면 됩니다.

다음 표에는 사용자가 목록 보기를 조작할 수 있는 방법과 이러한 사용자 조작에 반응할 수 있는 방법이 나와 있습니다.

활성화할 조작 | 사용할 설정 | 처리할 이벤트 | 선택한 항목을 가져오기 위해 사용할 속성
----------------------------|---------------------|--------------------|--------------------------------------------
조작 없음 | [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **None**, [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **False** | 해당 없음 | 해당 없음 
단일 선택 | SelectionMode = **Single**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem), [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
다중 선택 | SelectionMode = **Multiple**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
확장 선택 | SelectionMode = **Extended**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
그룹에 타일을 추가하려면 | SelectionMode = **None**, IsItemClickEnabled = **True** | [ItemClick](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) | 해당 없음 

> **참고**&nbsp;&nbsp;Windows 10부터 SelectionMode도 Single, Multiple 또는 Extended로 설정된 상태에서 IsItemClickEnabled를 통해 ItemClick 이벤트를 발생시킬 수 있습니다. 이렇게 하면 먼저 ItemClick 이벤트가 발생한 다음 SelectionChanged 이벤트가 발생합니다. ItemClick 이벤트 처리기에서 다른 페이지로 이동하는 경우와 같은 상황에서는 SelectionChanged 이벤트가 발생하지 않고 항목이 선택되지 않습니다.

아래와 같이 XAML 또는 코드에서 이러한 속성을 설정할 수 있습니다.

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>읽기 전용

SelectionMode 속성을 **ListViewSelectionMode.None**으로 설정하여 항목 선택을 사용하지 않도록 설정할 수 있습니다. 그러면 컨트롤이 읽기 전용 모드가 되어 데이터를 표시하는 데만 사용되고 조작할 수 없게 됩니다. 컨트롤 자체를 사용할 수 없게 설정할 수는 없으며 항목 선택만 사용할 수 없게 됩니다.

### <a name="single-selection"></a>단일 선택

다음 표에서는 SelectionMode가 **Single**일 때의 키보드, 마우스 및 터치 조작에 대해 설명합니다.

보조 키 | 조작
-------------|------------
없음 | <li>사용자가 스페이스바, 마우스 클릭 또는 터치 탭을 사용하여 단일 항목을 선택할 수 있습니다.</li>
Ctrl | <li>사용자가 스페이스바, 마우스 클릭 또는 터치 탭을 사용하여 단일 항목을 선택 취소할 수 있습니다.</li><li>화살표 키를 사용하여 선택과는 별도로 포커스를 이동할 수 있습니다.</li>

SelectionMode가 **Single**이면 [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 속성에서 선택한 데이터 항목을 가져올 수 있습니다. [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 속성을 사용하여 선택한 항목 컬렉션의 인덱스를 가져올 수 있습니다. 선택한 항목이 없으면 SelectedItem은 **null**이고 SelectedIndex는 -1입니다. 
 
**Items** 컬렉션에 없는 항목을 **SelectedItem**으로 설정하려고 하면 작업은 무시되고 SelectedItem은 **null**이 됩니다. 그러나 **SelectedIndex**를 목록의 **Items** 범위를 벗어나는 인덱스로 설정하려고 하면 **System.ArgumentException** 예외가 발생합니다. 

### <a name="multiple-selection"></a>다중 선택

다음 표에서는 SelectionMode가 **Multiple**일 때의 키보드, 마우스 및 터치 조작에 대해 설명합니다.

보조 키 | 조작
-------------|------------
없음 | <li>사용자는 스페이스바, 마우스 클릭 또는 터치 탭으로 여러 항목을 선택하여 포커스가 있는 항목에 대한 선택을 전환할 수 있습니다.</li><li>화살표 키를 사용하여 선택과는 별도로 포커스를 이동할 수 있습니다.</li>
Shift | <li>선택 영역의 첫 번째 항목을 클릭하거나 탭한 다음 선택 영역의 마지막 항목을 클릭하거나 탭하여 인접한 여러 항목을 선택할 수 있습니다.</li><li>Shift 키를 누른 채로 화살표 키를 사용하여 선택한 항목부터 연속되는 선택 영역을 지정할 수 있습니다.</li>

### <a name="extended-selection"></a>확장 선택

다음 표에서는 SelectionMode가 **Extended**일 때의 키보드, 마우스 및 터치 조작에 대해 설명합니다.

보조 키 | 조작
-------------|------------
없음 | <li>동작은 **Single**을 선택할 때와 같습니다.</li>
Ctrl | <li>사용자는 스페이스바, 마우스 클릭 또는 터치 탭으로 여러 항목을 선택하여 포커스가 있는 항목에 대한 선택을 전환할 수 있습니다.</li><li>화살표 키를 사용하여 선택과는 별도로 포커스를 이동할 수 있습니다.</li>
Shift | <li>선택 영역의 첫 번째 항목을 클릭하거나 탭한 다음 선택 영역의 마지막 항목을 클릭하거나 탭하여 인접한 여러 항목을 선택할 수 있습니다.</li><li>Shift 키를 누른 채로 화살표 키를 사용하여 선택한 항목부터 연속되는 선택 영역을 지정할 수 있습니다.</li>

SelectionMode가 **Multiple** 또는 **Extended**이면 [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems) 속성에서 선택한 데이터 항목을 가져올 수 있습니다. 

**SelectedIndex**, **SelectedItem** 및 **SelectedItems** 속성은 동기화됩니다. 예를 들어 SelectedIndex를 -1로 설정하면 SelectedItem은 **null**로 설정되고 SelectedItems는 빈 상태가 됩니다. SelectedItem을 **null**로 설정하면 SelectedIndex는 -1로 설정되고 SelectedItems는 빈 상태가 됩니다.

다중 선택 모드에서 **SelectedItem**에는 처음에 선택된 항목이 포함되고 **Selectedindex**에는 처음에 선택한 항목의 인덱스가 포함됩니다. 

### <a name="respond-to-selection-changes"></a>선택 변경에 응답

목록 보기에서 선택 항목 변경에 응답하려면 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트를 처리합니다. 이벤트 처리기 코드에서는 [SelectionChangedEventArgs.AddedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 속성에서 선택한 항목 목록을 가져올 수 있습니다. [SelectionChangedEventArgs.RemovedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 속성에서 선택 취소된 모든 항목을 가져올 수 있습니다. 사용자가 Shift 키를 눌러 항목의 범위를 선택하지 않으면 AddedItems 및 RemovedItems 컬렉션에는 1개의 항목만 포함됩니다.

이 예제에서는 **SelectionChanged** 이벤트를 처리하고 다양한 항목 컬렉션에 액세스하는 방법을 보여 줍니다.

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>클릭 모드

사용자가 항목을 선택하는 대신 단추와 같은 항목을 클릭하도록 목록 보기를 변경할 수 있습니다. 예를 들어, 이 기능은 사용자가 목록 또는 그리드의 항목을 클릭하면 앱이 새로운 페이지로 이동하는 경우에 유용합니다. 이 동작을 사용하려면
- **SelectionMode**를 **None**으로 설정합니다.
- **IsItemClickEnabled**를 **true**로 설정합니다.
- 항목을 클릭할 때 작업을 수행하도록 **ItemClick** 이벤트를 처리합니다.

다음은 클릭 가능한 항목이 있는 목록 보기입니다. ItemClick 이벤트 처리기의 코드는 새로운 페이지로 이동합니다.

**XAML**
```xml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>항목의 범위를 프로그래밍 방식으로 선택

경우에 따라 목록 보기의 항목 선택을 프로그래밍 방식으로 조작해야 할 수도 있습니다. 예를 들어 사용자가 목록의 모든 항목을 선택할 수 있도록 하는 **모두 선택** 단추가 있을 수 있습니다. 이 경우 SelectedItems 컬렉션에서 항목을 하나씩 추가 및 제거하는 것이 일반적으로 매우 비효율적입니다. 각 항목이 변경될 때마다 SelectionChanged 이벤트가 발생되며, 인덱스 값을 사용하지 않고 항목을 직접 사용하면 항목의 가상화가 취소됩니다.

[SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectall), [SelectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange) 및 [DeselectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) 메서드는 SelectedItems 속성을 사용하는 것보다 선택을 수정하는 보다 효율적인 방법을 제공합니다. 이러한 메서드를 항목 인덱스의 범위를 사용하여 선택하거나 선택을 취소합니다. 인덱스만 사용되기 때문에 가상화된 항목이 가상화 상태를 유지합니다. 지정된 범위의 모든 항목은 원래의 선택 상태에 관계없이 선택(또는 선택 취소)됩니다. SelectionChanged 이벤트는 이러한 메서드를 호출할 때마다 한 번만 발생합니다.

> [!IMPORTANT]
> SelectionMode 속성이 Multiple 또는 Extended로 설정된 경우에만 이러한 메서드를 호출해야 합니다. SelectionMode가 Single 또는 None일 때 SelectRange를 호출하면 예외가 발생합니다.

인덱스 범위를 사용하여 항목을 선택할 경우 [SelectedRanges](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges) 속성을 사용하여 목록의 선택된 모든 범위를 가져옵니다.

ItemsSource가 [IItemsRangeInfo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)를 구현하고 이러한 메서드를 사용하여 선택 항목을 수정하는 경우 **AddedItems** 및 **RemovedItems** 속성은 SelectionChangedEventArgs에서 설정되지 않습니다. 이러한 속성을 설정하려면 항목 개체의 가상화를 해제해야 합니다. 대신 **SelectedRanges** 속성을 사용하여 항목을 가져옵니다.

SelectAll 메서드를 호출하여 컬렉션의 모든 항목을 선택할 수 있습니다. 그러나 모든 항목을 선택 취소하기 위한 메서드는 없습니다. DeselectRange를 호출하고 [FirstIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) 값이 0이고 [Length](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.length) 값이 컬렉션의 항목 수와 같은 [ItemIndexRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange)를 전달하여 모든 항목을 선택 취소할 수 있습니다. 아래 예제에 모든 항목을 선택하는 옵션과 함께 이 내용이 나옵니다.

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

선택한 항목의 모양을 변경하는 방법에 대한 자세한 내용은 [항목 컨테이너 및 템플릿](item-containers-templates.md)을 참조하세요.

### <a name="drag-and-drop"></a>끌어서 놓기

ListView 및 GridView 컨트롤은 컨트롤 내 항목의 끌어서 놓기를 지원하고 서로 다른 ListView 및 GridView 컨트롤 간의 끌어서 놓기를 지원합니다. 끌어서 놓기 패턴을 구현하는 방법은 [끌어서 놓기](../input/drag-and-drop.md)를 참조하세요.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML ListView 및 GridView 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) - ListView 및 GridView 컨트롤을 보여 줍니다.
- [XAML 끌어서 놓기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) - ListView 컨트롤을 통한 끌어서 놓기를 보여 줍니다.
- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여 줍니다.

## <a name="related-articles"></a>관련 문서

- [목록](lists.md)
- [항목 컨테이너 및 템플릿](item-containers-templates.md)
- [끌어서 놓기](../input/drag-and-drop.md)
