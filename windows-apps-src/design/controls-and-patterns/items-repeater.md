---
Description: ItemsRepeater는 항목 컬렉션을 생성하고 표시하는 경량 컨트롤입니다.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5782c6e9ba42fed07c2b1382f2d17b1d311d0a13
ms.sourcegitcommit: 1b06c27e7fa4726fd950cbeaf05206c0a070e3c7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80893463"
---
# <a name="itemsrepeater"></a>ItemsRepeater

유연한 레이아웃 시스템, 사용자 지정 보기 및 가상화를 사용하는 사용자 지정 컬렉션 환경을 만들려면 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 사용합니다.

[ListView](/uwp/api/windows.ui.xaml.controls.listview)와는 달리, [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)는 포괄적인 최종 사용자 환경을 제공하지 않습니다. 기본 UI가 없으며 포커스, 선택 또는 사용자 상호 작용과 관련된 정책을 제공하지 않습니다. 대신 이는 고유한 컬렉션 기반 환경 및 사용자 지정 컨트롤을 만드는 데 사용할 수 있는 문서 블록입니다. 기본 정책은 없지만, 정책을 연결하여 필요한 환경을 구축할 수 있습니다. 예를 들어 사용할 레이아웃, 키보드 정책, 선택 정책 등을 정의할 수 있습니다.

개념적으로 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)는 ListView 같은 완전한 컨트롤이라기 보다는 데이터 중심 패널에 가깝습니다. 개발자는 표시할 데이터 항목 컬렉션, 각 데이터 항목의 UI 요소를 생성하는 항목 템플릿 그리고 요소의 크기와 위치를 결정하는 레이아웃을 지정합니다. 그러면 ItemsRepeater가 데이터 원본에 따라 자식 요소를 생성하고, 항목 템플릿과 레이아웃에 지정된 대로 자식 요소를 표시합니다. ItemsRepeater는 개발자가 데이터 템플릿 선택기에서 지정하는 기준에 따라 데이터 항목을 표시할 콘텐츠를 로드할 수 있으므로 표시되는 항목이 같은 형식일 필요는 없습니다

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | **ItemsRepeater** 컨트롤은 UWP 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되었습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

> **Windows UI 라이브러리 API:** [ItemsRepeater 클래스](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
>
> **플랫폼 API:** [ScrollViewer 클래스](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 사용하여 데이터 컬렉션에 대한 사용자 지정 표시를 만듭니다. 기본 항목 세트를 표시하는 데 사용할 수 있지만, 사용자 지정 컨트롤의 템플릿에 표시 요소로 자주 사용됩니다.

최소한의 사용자 지정으로 목록 또는 그리드에 데이터를 표시할 수 있는 기본 제공 컨트롤이 필요하다면 [ListView](/uwp/api/windows.ui.xaml.controls.listview) 또는 [GridView](/uwp/api/windows.ui.xaml.controls.gridview)를 사용하는 것을 고려해 보세요.

ItemsRepeater는 기본 항목 컬렉션을 제공하지 않습니다. 별도의 데이터 원본에 바인딩하지 않고 항목 컬렉션을 직접 제공해야 하는 경우 보다 강력한 정책 환경이 필요한 것이므로 [ListView](/uwp/api/windows.ui.xaml.controls.listview) 또는 [GridView](/uwp/api/windows.ui.xaml.controls.gridview)를 사용해야 합니다.

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol)과 ItemsRepeater 둘 다 사용자 지정 가능한 컬렉션 환경을 지원하지만, ItemsRepeater는 UI 레이아웃 가상화를 지원하고 ItemsControl은 지원하지 않습니다. 간단하게 데이터의 몇 가지 항목을 표시하든 아니면 사용자 지정 컬렉션 컨트롤을 빌드하든, ItemsControl 대신 ItemsRepeater를 사용하는 것이 좋습니다.

> [!NOTE]
> ItemsControl이 요구 사항을 충족하고 ItemsRepeater는 충족하지 않는다고 생각되는 경우 [Windows UI 라이브러리 GitHub 프로젝트](https://github.com/Microsoft/microsoft-ui-xaml/issues)에 피드백을 남겨주세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 실제로 작동하는 <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>를 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>ItemsRepeater를 사용하여 스크롤

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)는 [**컨트롤**](/uwp/api/windows.ui.xaml.controls.control)에서 파생되지 않으므로 컨트롤 템플릿이 없습니다. 따라서 ListView 또는 다른 컬렉션 컨트롤처럼 기본 스크롤 기능을 제공하지 않습니다.

**ItemsRepeater**를 사용하는 경우 [**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer) 컨트롤에 스크롤 기능을 래핑하는 방법으로 스크롤 기능을 제공해야 합니다.

> [!NOTE]
> 또한 앱이 Windows 10 버전 1809 *이전*에 출시된 Windows에서 실행되는 경우 [ **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost) 내부에 **ScrollViewer**를 호스트해야 합니다. 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> 앱이 Windows 10 버전 1809 이상에서만 실행되는 경우 [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost)를 사용할 필요가 없습니다.
>
> Windows 10 버전 1809보다 낮은 버전에서는 **ScrollViewer**가 **ItemsRepeater**에 필요한 [**IScrollAnchorProvider**](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) 인터페이스를 구현하지 않습니다.  **ItemsRepeaterScrollHost**는 이전 릴리스에서 **ItemsRepeater**가 **ScrollViewer**와 함께 작동하여 사용자가 보고 있는 항목의 시각적 위치를 유지할 수 있게 해줍니다.  그렇지 않으면 목록의 항목이 변경되거나 앱의 크기를 조정할 때 항목이 갑자기 이동하거나 사라지는 것처럼 보일 수 있습니다.

## <a name="create-an-itemsrepeater"></a>ItemsRepeater 만들기

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 사용하려면 **ItemsSource** 속성을 설정하여 표시할 데이터를 제공해야 합니다. 그런 다음, [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) 속성을 설정하여 항목을 표시하는 방법을 알려주어야 합니다.

### <a name="itemssource"></a>ItemsSource

보기를 채우려면 [**ItemsSource**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) 속성을 데이터 항목 컬렉션으로 설정합니다. 여기서는 **ItemsSource**가 코드에서 바로 컬렉션의 인스턴스로 설정됩니다.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

XAML에서 **ItemsSource** 속성을 컬렉션에 바인딩할 수도 있습니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 개요](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)를 참조하세요.


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
데이터 항목을 시각화하는 방법을 지정하려면 [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) 속성을 이전에 정의한 [**DataTemplate**](/uwp/api/windows.ui.xaml.datatemplate) 또는 [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector)로 설정합니다. 데이터 템플릿은 데이터를 시각화하는 방법을 정의합니다. 기본적으로 항목은 데이터 개체의 문자열 표현을 사용하는 **TextBlock**을 통해 보기에 표시됩니다.

그러나 개별 항목을 표시하는 데 사용할 하나 이상의 컨트롤의 모양과 레이아웃을 정의하는 템플릿을 사용하여 데이터를 보다 풍부하게 표시하는 것이 일반적입니다. 템플릿에 사용하는 컨트롤을 데이터 개체의 속성에 바인딩하거나 정적 콘텐츠를 인라인으로 정의할 수 있습니다.

#### <a name="datatemplate"></a>DataTemplate
이 예제의 데이터 개체는 간단한 문자열입니다. **DataTemplate**은 텍스트 왼쪽에 이미지를 포함하고 있으며, 문자열을 청록색으로 표시하도록 **TextBlock**의 스타일을 지정합니다.

> [!NOTE]
> **DataTemplate**에서 [x:Bind 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)을 사용하는 경우 DataTemplate에서 DataType(`x:DataType`)을 지정해야 합니다.

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

이 **DataTemplate**을 사용하여 항목을 표시하면 항목이 다음과 같이 나타납니다.

![데이터 템플릿을 사용하여 표시된 항목](images/listview-itemstemplate.png)

보기에 대량의 항목이 표시되는 경우 항목의 **DataTemplate**에 사용된 요소의 수가 성능에 큰 영향을 미칠 수 있습니다. **DataTemplate**을 사용하여 목록에 있는 항목의 모양을 정의하는 자세한 방법과 예제는 [항목 컨테이너 및 템플릿](item-containers-templates.md)을 참조하세요.

> [!TIP]
> 템플릿을 정적 리소스로 참조되는 것이 아닌 인라인으로 선언하려는 경우 사용 편의를 위해 **DataTemplate** 또는 **DataTemplateSelector**를 **ItemsRepeater**의 직계 자식으로 지정할 수 있습니다.  ItemsRepeater는 **ItemTemplate** 속성의 값으로 할당됩니다. 예를 들어 다음과 같이 사용하면 유효합니다.
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> **ListView** 및 다른 컬렉션 컨트롤과는 달리, **ItemsRepeater**는 **DataTemplate**의 요소를 여백, 안쪽 여백, 선택 시각적 개체 또는 시각적 상태 위의 포인터 같은 기본 정책을 포함하고 있는 추가 항목 컨테이너로 래핑하지 않습니다. 대신 **ItemsRepeater**는 **DataTemplate**에 정의된 것만 제공합니다. 항목의 모양을 목록 보기 항목과 똑같이 만들고 싶으면 **ListViewItem** 같은 컨테이너를 데이터 템플릿에 명시적으로 포함하면 됩니다. **ItemsRepeater**는 **ListViewItem** 시각적 개체를 표시하지만, 선택 또는 다중 선택 확인란 표시 같은 다른 기능을 자동으로 사용하지는 않습니다.
>
> 마찬가지로, 데이터 컬렉션이 **단추**(`List<Button>`)같은 실제 컨트롤 컬렉션인 경우 **ContentPresenter**를 **DataTemplate**에 배치하여 컨트롤을 표시할 수 있습니다.

#### <a name="datatemplateselector"></a>DataTemplateSelector

보기에 표시하는 항목이 동일한 형식일 필요는 없습니다. 개발자가 지정하는 기준에 따라 다른 **DataTemplate**을 선택할 수 있도록 [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) 속성에 [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector)를 제공할 수 있습니다.

이 예제에서는 큰 항목과 작은 항목을 표현하는 두 **DataTemplate** 중에 무엇을 사용할 것인지 결정하는 **DataTemplateSelector**가 이미 정의된 것으로 가정합니다.

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

**ItemsRepeater**에 사용할 **DataTemplateSelector**를 정의할 때 [**SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_) 메서드의 재정의만 구현하면 됩니다. 자세한 내용과 예제는 [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector)를 참조하세요.

> [!NOTE]
> 고급 시나리오에서 요소를 만드는 방법을 관리하는 데 사용되는 **DataTemplate** 대신, **ItemTemplate**으로 사용할 [**Windows.UI.Xaml.Controls.IElementFactory**](/uwp/api/windows.ui.xaml.controls.ielementfactory)를 구현할 수 있습니다.  이 대안을 사용할 경우 요청을 받으면 콘텐츠를 생성해야 합니다.

## <a name="configure-the-data-source"></a>데이터 원본 구성

항목의 콘텐츠를 생성하는 데 사용할 컬렉션을 지정하려면 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) 속성을 사용합니다. **IEnumerable**을 구현하는 모든 형식으로 ItemsSource를 설정할 수 있습니다. 데이터 원본이 구현하는 추가 컬렉션 인터페이스에 따라 ItemsRepeater가 데이터와 상호 작용하는 데 사용할 수 있는 기능이 결정됩니다.

이 목록은 사용 가능한 인터페이스과 각 인터페이스를 언제 사용하는지 보여줍니다.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET)/[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - 작은 정적 데이터 세트에 사용할 수 있습니다.

    적어도 데이터 원본이 IEnumerable/IIterable 인터페이스를 구현해야 합니다. 이 외에는 지원되지 않는 경우 컨트롤은 모든 항목을 한 번씩 반복하여 인덱스 값을 통해 항목에 액세스할 때 사용할 수 있는 복사본을 만듭니다.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET)/[IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - 정적 읽기 전용 데이터 세트에 사용할 수 있습니다.

    컨트롤이 인덱스를 통해 항목에 액세스할 수 있게 해주고 중복 내부 복사를 방지합니다.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET)/[IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - 정적 데이터 세트에 사용할 수 있습니다.

    컨트롤이 인덱스를 통해 항목에 액세스할 수 있게 해주고 중복 내부 복사를 방지합니다.

    **경고**: [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)를 구현하지 않고 목록/벡터를 변경하면 UI에 반영되지 않습니다.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - 변경 알림을 지원하려면 이 인터페이스를 사용하는 것이 좋습니다.

    컨트롤이 데이터 원본의 변경 내용을 관찰하고 그에 따라 대응할 수 있게 해주며 변경 내용을 UI에 반영합니다.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - 변경 알림을 지원합니다.

    **INotifyCollectionChanged** 인터페이스와 마찬가지로, 컨트롤이 데이터 원본의 변경 내용을 관찰하고 그에 따라 대응할 수 있게 해줍니다.

    **경고**: Windows.Foundation.IObservableVector\<T>는 '이동' 작업을 지원하지 않습니다. 이로 인해 항목의 UI가 시각적 상태를 잃게 될 수 있습니다.  예를 들어 현재 선택되고/되었거나 'Remove' 뒤에 'Add'를 사용하여 이동이 수행되는 곳에 포커스가 있는 항목은 포커스를 잃게 되고 더 이상 선택되지 않습니다.

    Platform.Collections.Vector\<T>는 IObservableVector\<T>를 사용하며 동일한 제한이 적용됩니다. '이동' 작업을 반드시 지원해야 하는 경우 **INotifyCollectionChanged** 인터페이스를 사용합니다.  .NET ObservableCollection\<T> 클래스는 **INotifyCollectionChanged**를 사용합니다.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - 고유 식별자를 각 항목에 연결할 수 있는 경우.  컬렉션 변경 작업으로 '다시 설정'을 사용하는 경우에 권장합니다.

    **INotifyCollectionChanged** 또는 **IObservableVector** 이벤트의 일부로 '다시 설정' 작업을 수신한 후 컨트롤이 매우 효율적으로 기존 UI를 복구할 수 있게 해줍니다. 다시 설정을 수신한 후 컨트롤은 제공된 고유 ID를 사용하여 현재 데이터를 이미 만들어진 요소와 연결합니다. 인덱싱할 키 없이 컨트롤을 매핑하는 경우 데이터의 UI를 만들 때 처음부터 다시 시작해야 하는 것으로 가정합니다.

IKeyIndexMapping을 제외하고 위에 나열된 인터페이스는 ListView 및 GridView와 똑같은 동작을 ItemsRepeater에서도 제공합니다.


ItemsSource의 다음 인터페이스는 ListView 및 GridView 컨트롤의 특수 기능을 사용할 수 있게 해주지만, 현재는 ItemsRepeater에 아무 영향도 미치지 않습니다.

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> 의견이 있으시면 알려주세요. [Windows UI 라이브러리 GitHub 프로젝트](https://github.com/Microsoft/microsoft-ui-xaml/issues)에 대한 의견을 남겨주세요. 다음과 같이 기존 제안에 대한 생각을 추가해 주세요. [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): ItemsRepeater에 대한 증분 로딩 지원 추가.

사용자가 위로 또는 아래로 스크롤할 때 데이터를 증분 로드하는 방법의 대안은 ScrollViewer의 뷰포트 위치를 관찰하고 뷰포트가 한계에 근접하면 더 많은 데이터를 로드하는 것입니다.

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

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 통해 표시되는 항목은 자식 요소의 크기와 배치를 관리하는 [레이아웃](/uwp/api/microsoft.ui.xaml.controls.layout) 개체를 통해 정렬됩니다. 레이아웃 개체는 ItemsRepeater와 함께 사용할 경우 UI 가상화를 지원합니다. 제공된 레이아웃은 [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) 및 [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout)입니다. 기본적으로 ItemsRepeater는 StackLayout을 세로 방향으로 사용합니다.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout)은 요소를 가로 또는 세로 방향으로 설정할 수 있는 한 줄로 정렬합니다.

[Spacing](/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) 속성을 설정하여 항목 사이의 공간 크기를 조정할 수 있습니다. 간격은 레이아웃의 [방향](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation)으로 적용됩니다.

![스택 레이아웃 간격](images/stack-layout.png)

이 예제에서는 가로 방향 및 8픽셀 간격을 사용하여 ItemsRepeater.Layout 속성을 StackLayout으로 설정하는 방법을 보여줍니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

[UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout)은 요소를 래핑 레이아웃에 순차적으로 배치합니다. [방향](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation)이 **가로**이면 항목이 왼쪽에서 오른쪽 순서로 배치되고, 방향이 **세로**이면 위에서 아래 순서로 배치됩니다. 모든 항목의 크기가 동일하게 조정됩니다.

![균일한 그리드 레이아웃 간격](images/uniform-grid-layout.png)

가로 레이아웃의 각 행에 있는 항목 수는 최소 항목 너비의 영향을 받습니다. 세로 레이아웃의 각 열에 있는 항목 수는 최소 항목 높이의 영향을 받습니다.

- [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) 및 [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) 속성을 설정하여 사용할 최소 크기를 명시적으로 제공할 수 있습니다.
- 최소 크기를 지정하지 않으면 첫 번째 항목을 측정한 크기가 항목의 최소 크기로 간주됩니다.

[MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) 및 [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) 속성을 설정하여 레이아웃의 행과 열 사이에 포함할 최소 간격을 설정할 수도 있습니다.

![균일한 그리드 크기 및 간격](images/uniform-grid-sizing-spacing.png)

항목의 최소 크기 및 간격에 따라 행 또는 열의 항목이 결정된 경우 이전 이미지처럼 행 또는 열의 마지막 항목 뒤에 사용되지 않는 공간이 남아 있을 수 있습니다. 모든 추가 공간을 무시할 것인지, 각 항목의 크기를 늘리는 데 사용할 것인지 아니면 항목 사이에 추가 공간을 만드는 데 사용할 것인지 지정할 수 있습니다. 이는 [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) 및 [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) 속성을 사용하여 제어합니다.

[ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) 속성을 설정하여 사용하지 않는 공간을 채우도록 항목 크기를 늘리는 방법을 지정할 수 있습니다.

다음 목록은 사용 가능한 값을 보여줍니다. 정의에서는 기본 **방향**을 **가로**로 가정합니다.

- **없음**: 추가 공간이 행의 끝에 사용되지 않은 상태로 남아 있습니다. 이것이 기본값입니다.
- **Fill**: 사용 가능한 공간(세로 방향인 경우 높이)을 사용하도록 항목에 추가 너비가 제공됩니다.
- **Uniform**: 사용 가능한 공간을 사용하도록 항목에 추가 너비가 제공되며, 가로 세로 비율을 유지하기 위해 추가 높이가 제공됩니다(세로 방향인 경우 높이와 너비가 전환됨).

이 이미지는 가로 레이아웃에서 **ItemsStretch** 값의 효과를 보여줍니다.

![균일한 그리드 항목 확대](images/uniform-grid-item-stretch.png)

**ItemsStretch**가 **없음**이면 [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) 속성을 설정하여 추가 공간을 이용해 항목에 맞추는 방법을 지정할 수 있습니다.

다음 목록은 사용 가능한 값을 보여줍니다. 정의에서는 기본 **방향**을 **가로**로 가정합니다.

- **Start**: 항목을 행의 시작 부분에 맞춥니다. 추가 공간이 행의 끝에 사용되지 않은 상태로 남아 있습니다. 이것이 기본값입니다.
- **Center**: 항목을 행의 중심에 맞춥니다. 추가 공간이 행의 시작 부분과 끝부분에 균등하게 분배됩니다.
- **End**: 항목을 행의 끝에 맞춥니다. 추가 공간은 행의 시작 부분에 사용되지 않은 상태로 남아 있습니다.
- **SpaceAround**: 항목을 균일하게 분산합니다. 각 항목 앞뒤에 동일한 양의 공간이 추가됩니다.
- **SpaceBetween**: 항목을 균일하게 분산합니다. 각 항목 사이에 동일한 양의 공간이 추가됩니다. 행의 시작과 끝에 공간이 추가되지 않습니다.
- **SpaceEvenly**: 각 항목의 사이와 행의 시작 및 끝에 동일한 양의 공간을 두어 항목을 균일하게 분산합니다.

이 이미지는 세로 레이아웃에서 **ItemsStretch** 값의 효과를 보여줍니다(행이 아닌 열에 적용됨).

![균일한 그리드 항목 맞춤](images/uniform-grid-item-justification.png)

> [!TIP]
> **ItemsStretch** 속성은 레이아웃의 _측정값_ 전달에 영향을 줍니다. **ItemsJustification** 속성은 레이아웃의 _정렬_ 전달에 영향을 줍니다.

다음 예제에서는 **ItemsRepeater.Layout** 속성을 **UniformGridLayout**으로 설정하는 방법을 보여줍니다.

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

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)에 항목을 호스트하는 경우 콘텐츠의 비동기 다운로드를 시작하거나, 요소를 선택 항목을 추적하는 메커니즘에 연결하거나, 백그라운드 작업을 중지하는 경우처럼 항목이 표시되거나 더 이상 표시되지 않을 때 조치를 취해야 할 수도 있습니다.

가상화 컨트롤에서는 요소가 재활용되는 경우 라이브 시각적 트리에서 요소를 제거할 수 없는 상황도 있기 때문에 Loaded/Unloaded 이벤트를 사용할 수 없습니다. 그 대신, 요소의 수명 주기를 관리하는 다른 이벤트가 제공됩니다. 이 다이어그램은 ItemsRepeater에 있는 요소의 수명 주기와 관련 이벤트가 발생하는 시점을 보여줍니다.

![수명 주기 이벤트 다이어그램](images/items-repeater-lifecycle.png)

- [**ElementPrepared**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) 이벤트는 요소를 사용할 수 있게 될 때마다 발생합니다. 새로 만들어진 요소와 이미 있고 재생 큐에서 다시 사용되는 요소에 대해 모두 발생합니다.
- [**ElementClearing**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) 이벤트는 실현된 항목의 범위를 벗어나는 경우처럼 요소가 재활용 큐에 전송될 때마다 즉시 발생합니다.
- [**ElementIndexChanged**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) 이벤트는 UIElement 요소가 나타내는 항목의 인덱스가 변경될 때마다 발생합니다. 예를 들어 데이터 원본에서 다른 항목이 추가 또는 제거되면 순서가 늦은 항목의 인덱스가 이 이벤트를 수신합니다.

다음 예제에서는 이러한 이벤트를 사용하여 사용자 지정 선택 서비스를 연결하고, ItemsRepeater를 사용하는 사용자 지정 컨트롤의 항목 선택을 추적하여 항목을 표시하는 방법을 보여줍니다.

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

## <a name="sorting-filtering-and-resetting-the-data"></a>데이터 정렬, 필터링 및 다시 설정

데이터 세트를 필터링하거나 정렬하는 등의 작업을 수행할 때 기존에는 이전 데이터 세트를 새 데이터와 비교한 다음, [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged)를 통해 세밀한 변경 알림을 제공했습니다. 하지만 이전 데이터를 새 데이터로 완전히 대체하고 [다시 설정](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction) 작업을 대신 사용하여 컬렉션 변경 알림을 트리거하는 것이 더 쉬운 경우가 자주 있습니다.

일반적으로 다시 설정하면 컨트롤은 다시 설정하는 동안 데이터가 어떻게 변경되었는지 정확하게 알지 못하기 때문에 기존 자식 요소를 해제한 후 다시 시작하고, 스크롤 위치 0에서 UI를 처음부터 새로 빌드합니다.

그러나 ItemsSource로 할당된 컬렉션이 [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping) 인터페이스를 구현하여 고유 식별자를 지원하는 경우에는 ItemsRepeater가 다음 항목을 신속하게 식별할 수 있습니다.

- 다시 설정하기 전과 후에 모두 존재하는 데이터의 다시 사용 가능한 UIElements
- 이전에 표시되다가 제거된 항목
- 표시할 새로 추가된 항목

이렇게 하면 ItemsRepeater가 스크롤 위치 0에서 다시 시작하지 않습니다. 또한 다시 설정할 때 변경되지 않은 데이터의 UIElement를 신속하게 복원할 수 있으므로 성능이 향상됩니다.

다음 예제에서는 세로 스택의 항목 목록을 표시하는 방법을 보여줍니다. 여기서 _MyItemsSource_는 기본 항목 목록을 래핑하는 사용자 지정 데이터 원본입니다. 항목 원본으로 사용할 새 목록을 다시 할당할 수 있는 _Data_ 속성을 노출한 다음, 다시 설정을 트리거합니다.

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

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 사용하여 각 항목을 제공하는 자체 컨트롤 형식을 모두 갖춘 사용자 지정 컬렉션 컨트롤을 만들 수 있습니다.

> [!NOTE]
> **ItemsControl**을 사용하는 것과 비슷하지만, **ItemsControl**에서 파생되고 **ItemsPresenter**를 컨트롤 템플릿에 배치하는 대신  **컨트롤**에서 파생되고 **ItemsRepeater**를 컨트롤 템플릿에 삽입합니다. 사용자 지정 컬렉션 컨트롤에 **ItemsRepeater**가 "있는" 경우도 있고 사용자 지정 컬렉션 컨트롤이 **ItemsControl**"인" 경우도 있습니다. 즉, 상속한 속성 중 지원하지 않을 속성보다는 노출할 속성을 명시적으로 선택해야 합니다.

이 예제에서는 _MediaCollectionView_라는 사용자 지정 컨트롤에 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 배치하고 속성을 노출하는 방법을 보여줍니다.

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

## <a name="display-grouped-items"></a>그룹화된 항목 표시

또 다른 ItemsRepeater의 [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)에 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)를 중첩하여 중첩된 가상화 레이아웃을 만들 수 있습니다. 이 프레임워크는 표시되지 않거나 현재 뷰포트 가까이에 있는 요소를 불필요하게 구현하는 일을 최소화하여 리소스를 효율적으로 사용합니다.

이 예제에서는 그룹화된 항목의 목록을 세로 스택에 표시하는 방법을 보여줍니다. 외부 ItemsRepeater는 각 그룹을 생성합니다. 각 그룹의 템플릿에서 또 다른 ItemsRepeater가 항목을 생성합니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->

<Page.Resources>
    <muxc:StackLayout x:Key="MyGroupLayout"/>
    <muxc:StackLayout x:Key="MyItemLayout" Orientation="Horizontal"/>
</Page.Resources>

<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          <TextBlock Text="{x:Bind AppTitle}"/>
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
아래 이미지는 위의 샘플을 지침으로 사용하여 만든 기본 레이아웃을 보여줍니다.

![항목 반복기가 있는 중첩된 레이아웃](images/items-repeater-nested-layout.png)

다음 예제에서는 사용자 기본 설정을 통해 변경할 수 있고 가로 방향으로 스크롤하는 목록으로 제공되는 다양한 범주가 있는 앱의 레이아웃을 보여줍니다. 이 예제의 레이아웃은 위의 이미지로도 표시됩니다.

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

## <a name="bringing-an-element-into-view"></a>보기로 요소 가져오기

XAML 프레임워크는 1) 키보드 포커스를 받을 때 또는 2) 내레이터 포커스를 받을 때 보기로 FrameworkElement를 가져오는 작업을 이미 처리하고 있습니다. 보기로 요소를 명시적으로 가져와야 하는 경우가 더 있을 수 있습니다. 사용자 작업에 응답할 때 또는 페이지 탐색 후 UI 상태를 복원하는 경우를 예로 들 수 있습니다.

가상화된 요소를 보기로 가져오려면 다음을 수행해야 합니다.
1. 항목의 UIElement 실현
2. 레이아웃을 실행하여 요소에 올바른 위치 확인
3. 실현된 요소를 보기로 가져오는 요청 시작

아래 예제에서는 페이지 탐색 후 평평한 세로 목록에 항목의 스크롤 위치를 복원하는 과정의 일부로 이러한 단계를 보여줍니다. 중첩된 ItemsRepeaters를 사용하는 계층적 데이터의 경우 접근 방식은 근본적으로 동일하지만, 계층 구조의 각 수준에서 수행해야 합니다.

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
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualSize.X, anchor.ActualSize.Y));
        relativeVerticalOffset = this.scrollviewer.VerticalOffset - anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>접근성 사용

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)는 기본 접근성 환경을 제공하지 않습니다. [UWP 앱의 유용성](/windows/uwp/design/usability)에 대한 설명서는 앱에서 포괄적인 사용자 환경을 제공할 수 있도록 풍부한 정보를 제공합니다. ItemsRepeater를 사용하여 사용자 지정 컨트롤을 만드는 경우 [사용자 지정 자동화 피어](/windows/uwp/design/accessibility/custom-automation-peers)에 대한 설명서를 꼭 참조하세요.

### <a name="keyboarding"></a>키보드
ItemsRepeater가 제공하는 포커스 이동의 최소 키보드 지원은 XAML의 [키보드용 2D 방향 탐색](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard)을 기반으로 합니다.

![방향 탐색](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

ItemsRepeater의 [XYFocusKeyboardNavigation 모드](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)가 기본적으로 _사용_됩니다. 의도한 환경에 따라 Home, End, PageUp, PageDown 등의 일반적인 [키보드 조작](/windows/uwp/design/input/keyboard-interactions) 지원을 추가하는 방안을 고려해 보세요.

ItemsRepeater는 항목(가상화 여부에 관계 없이)의 기본 탭 순서가 데이터에서 항목에 제공되는 순서와 동일하도록 자동으로 확인합니다. 기본적으로는 ItemsRepeater는 [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) 속성이 일반적인 기본값인 _로컬_ 대신 [한 번](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode)으로 설정됩니다.

> [!NOTE]
> ItemsRepeater는 마지막으로 포커스가 있었던 항목을 자동으로 기억하지 않습니다.  즉, 사용자가 Shift+Tab을 사용할 때 마지막으로 실현된 항목으로 이동될 수 있습니다.

### <a name="announcing-item-_x_-of-_y_-in-screen-readers"></a>화면 읽기 프로그램에서 "_Y_의 _X_ 항목" 발표

**PositionInSet** 및 **SizeOfSet**의 값처럼 적절한 자동화 속성을 설정해야 하며, 항목이 추가, 이동, 제거될 때 최신 상태를 유지해야 합니다.

명확한 시각적 순서가 없는 사용자 지정 레이아웃도 있습니다.  사용자는 적어도 화면 읽기 프로그램에 사용되는 PositionInSet 및 SizeOfSet 속성의 값이 데이터에 항목이 나타나는 순서와 일치할 것으로 예상합니다(자연 집계와 일치하도록 1로 오프셋하거나 0부터 시작).

이렇게 하는 가장 좋은 방법은 항목 컨트롤의 자동화 피어가 [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) 및 [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) 메서드를 구현하고 컨트롤이 표현한 데이터 세트의 항목 위치를 보고하도록 만드는 것입니다. 보조 기술이 액세스할 때 런타임에만 값이 컴퓨팅되므로 최신 상태를 유지하는 것은 문제가 되지 않습니다. 이 값은 데이터 순서와 일치합니다.

이 예제에서는 _CardControl_이라는 사용자 지정 컨트롤을 표시할 때 이렇게 하는 방법을 보여줍니다.

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

## <a name="related-articles"></a>관련된 문서

- [목록](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
