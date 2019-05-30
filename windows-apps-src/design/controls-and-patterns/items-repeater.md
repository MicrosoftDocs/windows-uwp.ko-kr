---
Description: ItemsRepeater는 간단한 컨트롤을 생성 하 고 항목의 컬렉션을 표시 합니다.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 93a81501b524826484111419899675fbb99b86fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364763"
---
# <a name="itemsrepeater"></a>ItemsRepeater

사용 된 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 유연한 레이아웃 시스템, 사용자 지정 뷰 및 가상화를 사용 하 여 환경을 사용자 지정 컬렉션을 만듭니다.

와 달리 [ListView](/uwp/api/windows.ui.xaml.controls.listview), [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 는 포괄적인 최종 사용자 환경을 제공 하지 않습니다 기본값이 없는 UI 고 포커스, 선택 영역 또는 사용자 상호 작용에 관한 정책이 없는 제공 합니다. 대신 자신의 고유한 컬렉션 기반 환경을 만들고 사용자 지정 컨트롤을 사용할 수 있는 구성 요소는 것입니다. 없는 기본 제공 정책에 해당 하는 동안 필요한 환경을 구축 하는 정책을 연결할 수 있습니다. 예를 들어 장애자 정책, 선택 정책 등을 사용 하려면 레이아웃을 정의할 수 있습니다.

생각할 수 있습니다 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 개념적 데이터 기반 패널 아니라 ListView와 같은 컨트롤을 완료 합니다. 표시할 데이터 항목의 컬렉션, 각 데이터 항목에 대 한 UI 요소를 생성 하는 항목 템플릿 및 요소 크기 및 위치를 결정 하는 레이아웃을 지정 합니다. 그런 다음 ItemsRepeater 데이터 원본에 따라 자식 요소를 생성 하 고 항목 템플릿 및 레이아웃에 지정 된 대로 표시 합니다. 표시 된 항목 ItemsRepeater 데이터 템플릿 선택기의 사용자가 지정한 조건에 따라 데이터 항목을 나타내는 콘텐츠를 로드할 수 없기 때문에 형식이 되도록 필요가 없습니다.

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 Windows UI 라이브러리, 새로운 컨트롤 및 UWP 앱 용 UI 기능을 포함 하는 NuGet 패키지의 일부로 포함 합니다. 설치 지침을 비롯 한 자세한 내용은 참조는 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)합니다. |

> **중요 API**: [ItemsRepeater 클래스](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), [ScrollViewer 클래스](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

사용 하 여는 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 데이터 컬렉션에 대 한 사용자 지정 표시를 만들려고 합니다. 항목의 기본 집합을 주도록 사용할 수 있지만, 사용자 지정 컨트롤의 템플릿에 표시 요소로 자주 사용할 수 있습니다.

목록 또는 최소한의 사용자 지정을 사용 하 여 그리드에서 데이터를 표시 하는 기본 제공 컨트롤을 해야 하는 경우는 [ListView](/uwp/api/windows.ui.xaml.controls.listview) 하거나 [GridView](/uwp/api/windows.ui.xaml.controls.gridview)합니다.

ItemsRepeater을 기본 제공 항목 컬렉션에 없습니다. 별도 데이터 원본에 바인딩하는 대신 항목 컬렉션을 직접 제공 해야 할 경우 더 높은 정책 경험을 해야 하 고 사용 해야 [ListView](/uwp/api/windows.ui.xaml.controls.listview) 하거나 [GridView](/uwp/api/windows.ui.xaml.controls.gridview)합니다.

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) ItemsRepeater 두 컬렉션 사용자 지정 가능한 경험을 사용 하도록 설정 하며 ItemsRepeater ItemsControl 않습니다 하지만 가상화 UI 레이아웃을 지원 합니다. 여부 ItemsControl 대신 ItemsRepeater를 사용 하는 것이 좋습니다는 방금 데이터에서 몇 가지 항목을 표시 또는 사용자 지정 컬렉션 컨트롤 작성에 대 한 합니다.

> [!NOTE]
> ItemsControl 요구 사항을 충족 하 고 ItemsRepeater 하지 생각 되는 상황에 있는 경우 의견을 남겨 주세요에 [Windows UI 라이브러리 GitHub 프로젝트](https://github.com/Microsoft/microsoft-ui-xaml/issues) 하 여 알려주세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 앱을 열고 참조를 <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> 작업에서.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>ItemsRepeater를 사용 하 여 스크롤

[**ItemsRepeater** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 에서 파생 되지 않습니다 [ **컨트롤**](/uwp/api/windows.ui.xaml.controls.control)이므로 컨트롤 템플릿에 것은 아닙니다. 따라서 스크롤 하는 ListView와 같은 기본 제공 포함 되지 않습니다 또는 다른 컬렉션 컨트롤 작업을 수행 합니다.

사용 하는 경우는 **ItemsRepeater**에 래핑하여 스크롤 기능을 제공 해야 하는 [ **ScrollViewer** ](/uwp/api/windows.ui.xaml.controls.scrollviewer) 컨트롤입니다.

> [!NOTE]
> 출시 된 앱-Windows의 이전 버전에서 실행 되는 경우 *하기 전에* Windows 10, 버전 1809-을 호스트 해야 합니다 **ScrollViewer** 안에 [  **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost)합니다. 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> 사용할 필요는 경우 앱 버전 1809 이상-Windows 10의 최신 버전에만 실행 됩니다 합니다 [ **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost)합니다.
>
> Windows 10 버전 1809, 이전 **ScrollViewer** 를 구현 하지 않았습니다 합니다 [ **IScrollAnchorProvider** ](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 는 인터페이스를 **ItemsRepeater**해야 합니다.  합니다 **ItemsRepeaterScrollHost** 사용 하도록 설정 합니다 **ItemsRepeater** 협력 하 여 **ScrollViewer** 올바르게 항목의 표시 위치를 유지 하기 위해 이전 버전에서 사용자가 볼 수 있습니다.  그렇지 않으면 항목을 이동 하거나 목록에서 항목을 변경한 경우 또는 앱의 크기를 조정할 때 갑자기 사라지는 나타날 수 있습니다.

## <a name="create-an-itemsrepeater"></a>ItemsRepeater 만들기

사용 하는 [ **ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 설정 하 여 표시할 데이터를 지정 해야 합니다 **ItemsSource** 속성입니다. 그런 다음 설정 하 여 항목을 표시 하는 방법을 알려주는 합니다 [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) 속성입니다.

### <a name="itemssource"></a>ItemsSource

뷰를 채우는 설정 합니다 [ **ItemsSource** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) 속성 데이터 항목의 컬렉션을 합니다. 여기에 **ItemsSource** 컬렉션 인스턴스에 직접 코드에 설정 됩니다.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

바인딩할 수도 있습니다는 **ItemsSource** XAML 컬렉션 속성입니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 개요](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)를 참조하세요.


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
데이터 항목은 시각화 하는 방법을 지정 하려면 설정 합니다 [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) 속성을 [ **DataTemplate** ](/uwp/api/windows.ui.xaml.datatemplate) 또는 [  **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 정의한 합니다. 데이터 템플릿 데이터는 시각화 하는 방법을 정의 합니다. 기본적으로 항목을 사용 하 여 보기에 표시 됩니다는 **TextBlock** 를 사용 하 여 데이터 개체의 문자열 표현입니다.

그러나 일반적으로 표시 하려는 데이터를 더 풍부한 기능의 프레젠테이션 개별 항목을 표시 하는 데 사용할 수 있는 하나 이상의 컨트롤의 모양과 레이아웃을 정의 하는 템플릿을 사용 하 여 합니다. 템플릿에서 사용할 컨트롤 데이터 개체의 속성에 바인딩할 수 있고 정적 콘텐츠 인라인으로 정의.

#### <a name="datatemplate"></a>DataTemplate
이 예제에서는 데이터 개체에는 간단한 문자열입니다. **DataTemplate** 텍스트 및 스타일의 왼쪽에 이미지를 포함 합니다 **TextBlock** 청록 색에서 문자열을 표시 하 합니다.

> [!NOTE]
> 사용 하는 경우는 [X:bind 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 에 **DataTemplate**, 데이터 형식을 지정 해야 (`x:DataType`) DataTemplate에 합니다.

```xaml
<DataTemplate x:DataType="x:String">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="47"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/placeholder.png" Width="32" Height="32"
               HorizontalAlignment="Left"/>
        <TextBlock Text="{x:Bind}" Foreground="Teal"
                   FontSize="15" Grid.Column="1"/>
    </Grid>
</DataTemplate>
```

이 사용 하 여 표시할 때 항목 형태가 나타나는 방법을 다음과 같습니다 **DataTemplate**합니다.

![데이터 템플릿을 사용 하 여 표시 되는 항목](images/listview-itemstemplate.png)

사용 되는 요소의 수를 **DataTemplate** 보기는 많은 수의 항목을 표시 하는 경우 항목 성능에 큰 영향을 줄 수에 대 한 합니다. 자세한 내용 및 예제를 사용 하는 방법에 대 한 **DataTemplate**목록에서 항목의 모양을 정의 참조 하십시오 [항목 컨테이너 및 템플릿](item-containers-templates.md)합니다.

> [!TIP]
> 정적 리소스 참조를 지정할 수 있습니다 보다는 인라인으로 선언할 때 편의 위해 합니다 **DataTemplate** 또는 **DataTemplateSelector** 합니다 의직접적인자식으로**ItemsRepeater**합니다.  값으로 할당 됩니다 합니다 **ItemTemplate** 속성입니다. 예를 들어이 유효 합니다.
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> 와 달리 **ListView** 및 기타 컬렉션 컨트롤을 **ItemsRepeater** 의 요소를 둘러싼를 **DataTemplate** 포함 하는 추가 항목 컨테이너를 사용 하 여 여백, 안쪽 여백, 시각적 개체 선택 또는 시각적 상태 위로 포인터와 같은 기본 정책입니다. 대신 **ItemsRepeater** 에 정의 된 제공 된 **DataTemplate**합니다. 목록 보기 항목으로 동일한 모양을 할 항목을 하려는 경우 같은 컨테이너를 명시적으로 포함할 수 있습니다 **ListViewItem**, 데이터 서식 파일에 있습니다. **ItemsRepeater** 표시 됩니다는 **ListViewItem** 시각적 개체를 자동으로 확인 하지 않습니다 하지만 선택 영역 또는 다중 선택 확인란을 보여 주는 같은 다른 기능을 사용 합니다.
>
> 데이터를 수집 하면 실제 컨트롤의 컬렉션인 경우 같은 마찬가지로 **단추** (`List<Button>`)를 넣을 수 있습니다를 **ContentPresenter** 에서 프로그램 **DataTemplate** 를 컨트롤을 표시 합니다.

#### <a name="datatemplateselector"></a>DataTemplateSelector

보기에 표시할 항목 동일한 형식일 필요가 없습니다. 제공할 수 있습니다 합니다 [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) 사용 하 여 속성을 [ **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 다른 선택  **DataTemplate**사용자가 지정한 조건에 따라 합니다.

이 예에서는 가정를 **DataTemplateSelector** 정의 된 다른 두 결정 하는 **DataTemplate**대형 및 소형 항목을 나타내는 s입니다.

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

정의 하는 경우는 **DataTemplateSelector** 사용 하 **ItemsRepeater** 에 대 한 재정의 구현 해야 합니다 [ **SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_) 메서드. 자세한 정보 및 예제를 참조 하세요 [ **DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector)합니다.

> [!NOTE]
> 대 안으로 **DataTemplate**관리 보다 고급 시나리오에서는 요소를 만드는 방법에 직접 구현 하는 [ **Windows.UI.Xaml.Controls.IElementFactory** ](/uwp/api/windows.ui.xaml.controls.ielementfactory)를 사용 하 여 **ItemTemplate**합니다.  요청 하는 경우 콘텐츠를 생성 해야 합니다.

## <a name="configure-the-data-source"></a>데이터 원본 구성

사용 된 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) 항목의 콘텐츠를 생성 하는 데 컬렉션을 지정 하는 속성입니다. 구현 하는 형식으로의 ItemsSource를 설정할 수 있습니다 **IEnumerable**합니다. 데이터 원본에 의해 구현 되는 추가 컬렉션 인터페이스 기능 사용자 데이터와 상호 작용 ItemsRepeater 수를 결정 합니다.

이 목록에는 사용할 수 있는 인터페이스 및 각각을 사용 하 여 고려 하는 경우 표시 됩니다.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - 작은, 정적 데이터 집합에 대해 사용할 수 있습니다.

    데이터 원본에는 최소한 IEnumerable을 구현 해야 합니다 / IIterable 인터페이스입니다. 이 모든 경우 지원 되는 컨트롤 인덱스 값을 통해 항목에 액세스 하는 데 사용할 수 있는 복사본을 만드는 모든 한 번을 통해 반복 됩니다.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - 데이터 집합의 정적, 읽기 전용으로 사용할 수 있습니다.

    인덱스에서 항목에 액세스 하는 컨트롤을 사용 하도록 설정 하 고 중복 내부 복사를 방지 됩니다.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - 정적 데이터 집합에 대해 사용할 수 있습니다.

    인덱스에서 항목에 액세스 하는 컨트롤을 사용 하도록 설정 하 고 중복 내부 복사를 방지 됩니다.

    **경고**: 구현 하지 않고 변경 하는 목록/벡터 [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) UI에 반영 되지 않습니다.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - 변경 알림을 지원 하려면 것이 좋습니다.

    관찰 및 데이터 원본에 변경 내용에 대응 하 고 UI의 변경 내용을 반영 제어할 수 있습니다.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - 변경 알림을 지원

    같은 합니다 **INotifyCollectionChanged** 인터페이스,이 통해 컨트롤을 관찰 하 고 데이터 소스의 변경에 대응 합니다.

    **경고**: Windows.Foundation.IObservableVector\<T > '이동' 작업을 지원 하지 않습니다. 해당 시각적 상태를 손실 하는 항목에 대 한 UI를 발생할 수 있습니다.  예를 들어, 현재 선택 및/또는 '추가' 뒤에 'Remove' 이동이 인스턴스에서 포커스가 있는 항목에서 포커스를 잃지 및 더 이상 선택할 수 없습니다.

    Platform.Collections.Vector\<T > IObservableVector를 사용 하 여\<T > 있고이 동일한 제한 사항이 있습니다. 지원 필수 '이동' 작업에 사용 합니다 **INotifyCollectionChanged** 인터페이스입니다.  .NET ObservableCollection\<T > 클래스는 **INotifyCollectionChanged**합니다.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - 경우 고유 식별자는 각 항목을 사용 하 여 연결할 수 있습니다.  컬렉션 변경 작업으로 '재설정'를 사용 하는 경우 것이 좋습니다.

    하드 '재설정' 작업의 일부로 받은 후 기존 UI를 매우 효율적으로 복구 하는 컨트롤을 사용 하도록 설정 된 **INotifyCollectionChanged** 또는 **IObservableVector** 이벤트입니다. 재설정을 받은 후 컨트롤이 이미 만들어 둔 요소를 사용 하 여 현재 데이터를 연결 하려면 제공 된 고유 ID를 사용 합니다. 매핑 인덱스에 키가 없는 데이터에 대 한 UI를 만들기에서 처음부터 다시 시작 해야 하는 것으로 가정 컨트롤 것입니다.

ListView 및 GridView와 마찬가지로, IKeyIndexMapping 이외의 위에 나열 된 인터페이스 ItemsRepeater에서 동일한 동작을 제공 합니다.


ItemsSource에 다음과 같은 인터페이스, ListView 및 GridView 컨트롤의 특별 한 기능을 사용 하도록 설정 했지만 현재는 ItemsRepeater에 영향을 주지:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> 의견이 있으시면 알려주세요. 의견을 남겨 주세요 합니다 [Windows UI 라이브러리 GitHub 프로젝트](https://github.com/Microsoft/microsoft-ui-xaml/issues)합니다. 와 같은 기존 제안에 의견을 추가 하는 것이 좋습니다 [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): 증분 로드 ItemsRepeater 지원을 추가 합니다.

등록 사용자가 스크롤하면 또는 아래로 데이터를 증분 로드 하는 다른 방법은 ScrollViewer의 뷰포트의 위치를 확인 하 고 뷰포트 범위 다가오면 더 많은 데이터를 로드 하는 것입니다.

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>항목의 레이아웃 변경

항목으로 표시 합니다 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 별로 정렬 되어를 [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layout) 해당 자식 요소 배치 및 크기 조정을 관리 하는 개체입니다. 레이아웃 개체는 ItemsRepeater를 사용 하면 UI 가상화 됩니다. 제공 된 레이아웃이 [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) 하 고 [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout)합니다. 기본적으로 ItemsRepeater 세로 방향으로는 StackLayout를 사용합니다.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) 가로나 세로 방향으로 방향을 설정할 수 있습니다 하는 한 줄으로 요소를 정렬 합니다.

설정할 수 있습니다 합니다 [간격](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) 항목 사이의 공간 크기를 조정 하는 속성입니다. 간격을 적용 하는 레이아웃의 방향을 [방향을](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation)합니다.

![스택 레이아웃 간격](images/stack-layout.png)

이 예제에서는 가로 방향 및 8 픽셀의 간격을 사용 하 여 StackLayout ItemsRepeater.Layout 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

합니다 [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) 래핑 레이아웃에서 요소를 순차적으로 배치 합니다. 항목 왼쪽에서 오른쪽 순서로 배치 됩니다 때를 [방향](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) 는 **가로**, 및 위쪽-아래쪽 방향 때 레이아웃 **세로**. 모든 항목 동일 하 게 크기가 조정 됩니다.

![균일 한 모눈 레이아웃 간격](images/uniform-grid-layout.png)

가로 레이아웃의 각 행의 항목 수가 최소 항목 너비에 영향을 받습니다. 세로 레이아웃의 각 열에는 항목 수가 최소 항목 높이 영향을 받습니다.

- 설정 하 여 사용할 최소 크기를 명시적으로 제공할 수는 [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) 하 고 [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) 속성입니다.
- 최소 크기를 지정 하지 않으면 첫 번째 항목의 측정된 크기 항목당 최소 크기를 간주 됩니다.

레이아웃을 설정 하 여 행 및 열 포함에 대 한 최소 간격을 설정할 수도 있습니다는 [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) 하 고 [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) 속성입니다.

![균일 한 모눈 단위의 크기 조정 및 간격](images/uniform-grid-sizing-spacing.png)

행 또는 열에 있는 항목 결정 한 경우 숫자 항목의 최소 크기 및 간격에 따라, 후 사용 하지 않은 공간 (같이 이전 이미지) 행 또는 열에 있는 마지막 항목 후에도 남아 있을 수 있습니다. 모든 공백 무시, 각 항목의 크기를 늘리는 데 또는 항목 사이의 추가 공백을 만드는 데 지정할 수 있습니다. 으로 제어 합니다 [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) 하 고 [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) 속성입니다.

설정할 수 있습니다 합니다 [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) 속성을 사용 하지 않는 공간을 채울 항목 크기가 증가 하는 방법을 지정 합니다.

이 목록은 사용 가능한 값을 보여줍니다. 정의 기본값 가정 **방향을** 의 **가로**합니다.

- **없음**: 추가 공간이 행의 끝에 사용 되지 않는 그대로 유지 됩니다. 기본값입니다.
- **입력**: 항목에는 사용 가능한 공간 (높이 세로 경우)을 사용 하 여 추가 너비가 제공 됩니다.
- **Uniform**: 사용 가능한 공간을 사용 하는 추가 너비를 지정 된 항목과 가로 세로 비율을 유지 하기 위해 추가 높이 지정 (높이 너비는 전환 세로 경우).

이 이미지의 결과 보여 줍니다.는 **ItemsStretch** 값 가로 레이아웃.

![확장 하 여 균일 한 모눈 항목](images/uniform-grid-item-stretch.png)

때 **ItemsStretch** 됩니다 **None**를 설정할 수 있습니다는 [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) 공간 항목 정렬에 사용 되는 추가 하는 방법을 지정 하려면 속성.

이 목록은 사용 가능한 값을 보여줍니다. 정의 기본값 가정 **방향을** 의 **가로**합니다.

- **시작**: 항목은 행의 시작을 사용 하 여 정렬 됩니다. 추가 공간이 행의 끝에 사용 되지 않는 그대로 유지 됩니다. 기본값입니다.
- **Center**: 항목은 행의 가운데에 정렬 됩니다. 시작 및 행의 끝에 추가 공간이 균등 하 게 구분 됩니다.
- **최종**: 항목 행의 끝에 맞춥니다. 행의 시작 부분에 공백은 사용 하지 않는 그대로 사용 합니다.
- **SpaceAround**: 항목은 균일 하 게 분산 됩니다. 같은 공간 크기는 각 항목 앞뒤에 추가 됩니다.
- **SpaceBetween**: 항목은 균일 하 게 분산 됩니다. 같은 공간 크기는 각 항목 사이 추가 됩니다. 시작 및 행의 끝에 공백이 추가 됩니다.
- **SpaceEvenly**: 항목은 같은 크기의 각 항목 사이 및 시작 및 끝 행의 공간을 사용 하 여 균등 하 게 분산 됩니다.

이 이미지의 결과 보여 줍니다.는 **ItemsStretch** 세로 레이아웃 (행이 아니라 열에 적용) 값입니다.

![균일 한 그리드 항목 맞춤](images/uniform-grid-item-justification.png)

> [!TIP]
> 합니다 **ItemsStretch** 속성에 영향을 줍니다 합니다 _측정값_ 레이아웃 전달 합니다. 합니다 **ItemsJustification** 속성에 영향을 줍니다 합니다 _정렬_ 레이아웃 전달 합니다.

설정 하는 방법을 보여 주는이 예제는 **ItemsRepeater.Layout** 속성을 **UniformGridLayout**합니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>수명 주기 이벤트

항목을 호스팅하는 경우는 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)작업을 수행 하도록 일부 항목이 표시 됩니다 또는 표시를 중지 해야 할 수 있습니다, 등의 일부 콘텐츠를 비동기 다운로드를 시작 하는 대로 연결 요소 선택 항목을 추적 하는 메커니즘을 사용 하 여 또는 일부 백그라운드 작업을 중지 합니다.

가상화 컨트롤에서 재활용 된 경우 요소가 라이브 시각적 트리에서 제거 되지 않을 수 있습니다 때문에 로드/언로드 이벤트에 사용할 수 없게 있습니다. 대신, 요소의 수명 주기를 관리 하려면 다른 이벤트가 제공 됩니다. 이 다이어그램에서는 관련된 이벤트가 발생 하는 시점과 ItemsRepeater 요소의 수명 주기를 보여 줍니다.

![수명 주기 이벤트 다이어그램](images/items-repeater-lifecycle.png)

- [**ElementPrepared** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) 요소를 사용할 수 있도록 수행 될 때마다 발생 합니다. 이 이미 있고이 재생 큐에서 다시 사용 되는 요소와 새로 만든된 요소를 모두 발생 합니다.
- [**ElementClearing** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) 요소 범위를 벗어난 경우 같은 재활용 큐에 전송 된 때마다 항목을 실현 하는 즉시 발생 합니다.
- [**ElementIndexChanged** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) 나타내는 항목의 인덱스에 변경 된 UIElement를 실현 하는 각각에 대해 발생 합니다. 예를 들어, 다른 항목을 추가 하거나 데이터 소스에서 제거 순서에서 다음에 오는 항목의 인덱스입니다이 이벤트를 수신 합니다.

이 예제에서는 이러한 이벤트를 사용 하 수 ItemsRepeater를 사용 하 여 항목을 표시 하는 사용자 지정 컨트롤의 선택 항목을 추적 하는 사용자 지정 선택 서비스를 연결 하는 방법을 보여 줍니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>정렬, 필터링 및 데이터를 다시 설정

필터링 하거나 데이터 집합을 정렬 하는 등의 작업을 수행 하면 있습니다 일반적으로 수는 이전 데이터 집합에 새 데이터 비교을 통해 세부적인 변경 알림을 발급 [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged)합니다. 그러나 하기가 종종 완전히 이전 데이터를 새 데이터로 대체 하 고 사용 하 여 컬렉션 변경 알림을 트리거하는 [재설정](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction) 작업 대신 합니다.

일반적으로 다시 설정 하는 컨트롤을 기존 자식 요소를 해제 하 고 데이터가 다시 설정 하는 동안 변경 된 방법을 정확 하 게 인식 하지가 스크롤 위치 0의 시작 부분에서 UI를 구축, 다시 시작 하면 됩니다.

그러나 컬렉션으로 할당 하는 경우의 ItemsSource 지원 고유 식별자를 구현 하 여 합니다 [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping) 인터페이스를 ItemsRepeater 신속 하 게 식별할 수 있습니다.

- 재설정 앞뒤 모두 존재 하는 데이터에 대 한 재사용 가능한 UIElements
- 표시 된 항목 중 제거 된 이전에
- 새 항목 추가 볼 수 있는

이렇게 하면 스크롤 위치 0에서에서 다시 시작 방지 ItemsRepeater가 있습니다. 또한 신속 하 게 더 나은 성능이 재설정을 변경 하지 않은 데이터에 대 한 Uielement를 복원 하 고 있습니다.

세로 스택 항목의 목록을 표시 하는 방법을 보여 주는이 예제는 _MyItemsSource_ 되는 기본 목록 항목을 래핑하는 사용자 지정 데이터 원본. 노출 된 _데이터_ 재설정을 트리거합니다 항목 원본으로 사용할 새 목록을 다시 할당할 수 있는 속성입니다.

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>사용자 지정 컬렉션 컨트롤 만들기

사용할 수는 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 컨트롤을 만드는 사용자 지정 컬렉션의 각 항목을 제공 하는 컨트롤 형식을 사용 하 여 완료 합니다.

> [!NOTE]
> 사용 하 여 비슷합니다 **ItemsControl**에서 파생 하는 대신 **ItemsControl** 배치 하 고는 **ItemsPresenter** 에서파생하는컨트롤템플릿에서 **컨트롤** 삽입 하 고는 **ItemsRepeater** 컨트롤 템플릿에서 합니다. 사용자 지정 컬렉션 컨트롤 "에" **ItemsRepeater** 와 "되는" **ItemsControl**합니다. 즉,는 노출 하려면 속성을 명시적으로 선택 해야 하는 상속 받지 말고 속성을 지원 하지 않습니다.

배치 하는 방법을 보여 주는이 예제는 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 라는 사용자 지정 컨트롤의 템플릿에 _MediaCollectionView_ 및 해당 속성을 노출 합니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>그룹화 된 항목 표시

중첩할 수 있습니다는 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 에 [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) 중첩 만들려면 다른 ItemsRepeater의 레이아웃을 가상화 합니다. 프레임 워크는 불필요 한 인식 또는 현재 뷰포트 거의 표시 되지 않는 요소를 최소화 하 여 리소스의 효율적인 사용을 확인 합니다.

이 예제에서는 세로로 그룹화 된 항목의 목록을 표시할 수는 방법을 보여 줍니다. 외부 ItemsRepeater 각 그룹을 생성합니다. 각 그룹에 대 한 템플릿에서 다른 ItemsRepeater 항목을 생성합니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

이 예제에서는 사용자 기본 설정으로 변경할 수 있으며 여기에 나와 있는 것 처럼 가로 방향으로 스크롤 하는 목록으로 제공 되는 다양 한 범주에는 앱에 대 한 레이아웃을 보여 줍니다.

![반복기 항목을 사용 하 여 중첩 된 레이아웃](images/items-repeater-nested-layout.png)

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>보기에는 요소 가져오기

XAML 프레임 워크는 이미 키보드 포커스를 받을 하는 1) 또는 2) 내레이터 포커스를 받을 때 뷰로 FrameworkElement를 가져오는 처리 합니다. 명시적으로 요소를 뷰로 가져옵니다 해야 하는 경우도 있을 수 있습니다. 예를 들어, 페이지 탐색 후 UI의 상태를 복원 하거나 사용자 동작에 응답에서 합니다.

보기에는 가상화 된 요소를 가져오는 하는 과정은 다음과 같습니다.
1. 항목에 대 한 UIElement 실현
2. 레이아웃 요소에 올바른 위치를 확인을 실행 합니다.
3. 뷰를 표시 요소를 가져오는 요청을 시작

아래 예제에서는 페이지 탐색 한 후 플랫, 세로 목록에서 항목의 스크롤 위치를 복원 하는 일환으로 이러한 단계를 보여 줍니다. 중첩 된 ItemsRepeaters를 사용 하 여 계층적 데이터의 경우 접근 방식은 기본적으로 동일 하지만 계층 구조의 각 수준에서 수행 해야 합니다.

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

            // retrieve saved offset + index(es) of the tracked element and then bring it into view.
            // ... 

            var element = repeater.GetOrCreateElement(index);

            // ensure the item is given a valid position
            element.UpdateLayout();

            element.StartBringIntoView(new BringIntoViewOptions()
            {
                VerticalOffset = relativeVerticalOffset
            });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualWidth, anchor.ActualHeight));
        relativeVerticalOffset = this.sv.VerticalOffset – anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>내게 필요한 옵션을 사용 하도록 설정

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) 기본 접근성 경험을 제공 하지 않습니다. 에 대 한 설명서 [UWP 앱에 대 한 유용성](/windows/uwp/design/usability) 제공 다양 한 앱을 확인 하는 데 유용한 정보를 포함 하는 사용자 환경을 제공 합니다. 사용자 지정 컨트롤을 만드는 ItemsRepeater를 사용 하는 경우 다음 해야 설명서를 참조 [사용자 지정 자동화 피어는](/windows/uwp/design/accessibility/custom-automation-peers)합니다.

### <a name="keyboarding"></a>키보드 사용
ItemsRepeater 제공 하는 포커스 이동에 대 한 최소한의 장애자 지원 XAML의 기반인 [Keyboarding에 대 한 2D 방향 탐색](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard)합니다.

![방향 탐색](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

ItemsRepeater [XYFocusKeyboardNavigation 모드](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) 됩니다 _Enabled_ 기본적으로. 의도 된 환경을 따라 일반적인에 대 한 지원 추가 고려할 [키보드 조작을](/windows/uwp/design/input/keyboard-interactions) Home, End, pageup 키, PageDown 등입니다.

ItemsRepeater 않습니다는 해당 항목에 대 한 기본 탭 순서 (뒤 추가할지 여부를 여부 가상화) 항목은 데이터에서 제공 되는 순서를 자동으로 확인 합니다. 기본적으로는 ItemsRepeater에 해당 [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) 속성으로 설정 [한 번](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) 일반적인 기본값 대신 _로컬_합니다.

> [!NOTE]
> ItemsRepeater에서 포커스가 있는 마지막 항목을 자동으로 유지 하지 않는 합니다.  이 사용자가 사용 하는 경우 Shift + Tab 마지막 수행할 수 있습니다 실현 항목을 의미 합니다.

### <a name="announcing-item-x-of-y-in-screen-readers"></a>발표 "항목 _X_ 의 _Y_" 화면 판독기에서

관리에 대 한 값과 같은 적절 한 자동화 속성을 설정 해야 할 **PositionInSet** 하 고 **SizeOfSet**, 항목이 추가 될 때 최신 상태로 유지 하 고 이동, 제거 등입니다.

일부 사용자 지정 레이아웃의 없을 시각적 개체 순서 하는 확실 한 순서입니다.  사용자는 최소 화면 판독기에서 사용 된 PositionInSet 및 SizeOfSet 속성에 대 한 값 (0부터 시작 되 고 및 계산 하는 자연 스러운 맞게 1만 오프셋) 데이터에서 항목의 순서는 일치 기대 합니다.

항목 컨트롤 구현에 대 한 자동화 피어를 함으로써이 작업을 수행 하는 가장 좋은 방법은는 [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) 하 고 [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) 메서드 및 보고서 데이터 집합에서 항목의 위치 컨트롤에서 표시 합니다. 값만 하는 보조 기술에 액세스할 경우 런타임 시 계산 되 고 문제를 비 됩니다 지속적으로 업데이트. 값은 데이터 순서와 일치 합니다.

이 예제에서는 어떻게 수행할 수 있습니다 라는 사용자 지정 컨트롤을 제공 하는 경우를 보여 줍니다 _CardControl_합니다.

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>관련 문서

- [목록](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
