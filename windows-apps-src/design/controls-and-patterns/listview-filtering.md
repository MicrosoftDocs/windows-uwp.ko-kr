---
description: 사용자 입력을 통해 컬렉션의 항목을 필터링합니다.
title: 컬렉션 필터링
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: cd78d46abacd57be5b08d6caf057e9c703b32560
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860376"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>사용자 입력을 통해 컬렉션 및 목록 필터링
컬렉션에 많은 항목이 표시되거나 사용자 상호 작용에 크게 얽매이는 경우 필터링은 구현에 유용한 기능입니다. 이 문서에서 설명하는 메서드를 사용하는 필터링은 [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView](/uwp/api/windows.ui.xaml.controls.gridview) 및 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2&preserve-view=true)를 비롯한 대부분의 컬렉션 컨트롤에 구현될 수 있습니다. 확인란, 라디오 단추 및 슬라이더와 같은 컬렉션을 필터링하는 데 많은 유형의 사용자 입력을 사용할 수 있습니다. 하지만 이 문서에서는 텍스트 기반 사용자 입력을 수집하여 사용자의 검색에 따라 실시간으로 ListView를 업데이트하는 데 초점을 맞춥니다. 

> [!NOTE]
> 이 문서에서는 ListView로 필터링하는 방법을 집중적으로 설명합니다. 또한 필터링 메서드는 GridView, ItemsRepeater 또는 TreeView와 같은 다른 컬렉션 컨트롤에도 적용할 수 있습니다.

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>필터링을 위한 UI 및 XAML 설정
필터링을 구현하려면 사용자 입력을 허용하는 TextBox 또는 다른 컨트롤과 함께 ListView가 앱에 표시되어야 합니다. 사용자가 TextBox에 입력하는 텍스트는 필터로 사용됩니다. 즉, 텍스트 입력/검색 쿼리를 포함하는 결과만 표시됩니다. 사용자가 TextBox에 입력하면 ListView는 필터링된 결과를 지속적으로 업데이트합니다. 특히 TextBox의 텍스트가 변경될 때마다, 한 문자만 변경되어도, ListView는 해당 항목을 살펴보고 해당 용어로 필터링합니다.

아래 코드는 제공된 TextBox와 함께 간단한 ListView 및 해당 DataTemplate이 있는 UI를 보여 줍니다. 이 예제에서 ListView는 Person 개체의 컬렉션을 표시합니다. Person은 코드 숨김으로 정의된 클래스이며 (아래 코드 샘플에는 표시되지 않음) 각 Person 개체에는 FirstName, LastName 및 Company라는 속성이 있습니다.

사용자는 TextBox를 사용하여 Person 개체 목록을 성 기준으로 필터링하는 검색/필터링 용어를 입력할 수 있습니다. TextBox는 특정 이름(`FilterByLName`)에 바인딩되고 자체 TextChanged 이벤트(`FilteredLV_LNameChanged`)를 포함합니다. 바인딩 이름을 사용하면 코드 숨김으로 TextBox의 콘텐츠/텍스트에 액세스할 수 있으며, 사용자가 TextBox에 입력할 때마다 TextChanged 이벤트가 발생하여 사용자 입력을 수신할 때 필터링 작업을 수행할 수 있습니다. 

필터링이 작동하려면 ListView에 `ObservableCollection<>`과 같은 코드 숨김으로 조작할 수 있는 데이터 원본이 있어야 합니다. 이 경우 ListView의 ItemsSource 속성은 코드 숨김으로 `ObservableCollection<Person>`에 할당됩니다. 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>데이터 필터링
[Linq](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) 쿼리를 사용하면 컬렉션에서 특정 항목을 그룹화, 정렬 및 선택할 수 있습니다. 목록을 필터링하기 위해 `FilterByLName` TextBox에 입력한 사용자-입력 검색 쿼리/필터링 용어와 일치하는 용어를 선택하는 Linq 쿼리를 생성하겠습니다. 쿼리 결과는 [IEnumerable<T>](/dotnet/api/system.collections.generic.ienumerable-1) 컬렉션 개체에 할당할 수 있습니다. 이 컬렉션을 만든 후, 이를 사용하여 원래 목록과 비교하여 일치하지 않는 항목을 제거하고 백스페이스의 경우 일치하는 항목을 다시 추가할 수 있습니다.

> [!NOTE]
> 항목을 더하거나 뺄 때 가장 직관적인 방법으로 ListView에 애니메이션 효과를 주려면 필터링된 개체의 새로운 컬렉션을 만들어서 ListView의 ItemsSource 속성에 할당하는 것이 아니라, ListView의 ItemsSource 컬렉션 자체에서 항목을 제거하거나 추가하는 것이 중요합니다.

시작하려면 `List<T>` 또는 `ObservableCollection<T>`과 같은 별도의 컬렉션에서 원래 데이터 원본을 초기화해야 합니다. 이 예제에는 ListView에 표시된 모든 Person 개체를 포함하는 `People`이라는 `List<Person>`이 있습니다(이 목록의 채우기/초기화는 아래 코드 조각에 표시되지 않음). 필터링된 데이터를 포함하는 목록도 필요하며, 이는 필터가 적용될 때마다 지속적으로 변경됩니다. 이는 `PeopleFiltered`라는 `ObservableCollection<Person>`이 되며 초기화 시 `People`과 동일한 콘텐츠를 가집니다.
 
아래 코드에서는 아래 코드에 나와 있는 것처럼 다음 단계를 통해 필터링 작업을 수행합니다.
 - ListView의 ItemsSource 속성을 `PeopledFiltered`로 설정합니다. 
 - `FilterByLName` TextBox에 대해 TextChanged 이벤트 `FilteredLV_LNameChanged()`를 정의합니다. 이 함수에서 데이터를 필터링합니다.
 - 데이터를 필터링하려면 `FilterByLName.Text`를 통해 사용자 입력 검색 쿼리/필터링 용어에 액세스합니다. Linq 쿼리를 사용하여 성에 `FilterByLName.Text`라는 용어가 포함된 `People`의 항목을 선택하고 일치하는 항목을 `TempFiltered`라는 컬렉션에 추가합니다.
 - 현재 `PeopleFiltered` 컬렉션을 새로 필터링한 `TempFiltered`의 항목과 비교하여 필요한 `PeopleFiltered`에서 항목을 제거하고 추가합니다.
 - `PeopleFiltered`에서 항목이 제거되고 추가되면 ListView가 그에 따라 업데이트되고 애니메이션 효과가 적용됩니다.

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

이제 사용자가 `FilterByLName` TextBox의 필터링 용어에 입력하면 ListView가 즉시 업데이트되어 성에 필터링 용어가 포함된 사람만 표시합니다.

## <a name="next-steps"></a>다음 단계

### <a name="get-the-sample-code"></a>샘플 코드 가져오기
- XAML 컨트롤 갤러리</strong> 앱이 설치되어 있는 경우 [여기](xamlcontrolsgallery:/item/ListView)를 클릭하여 앱을 열고 ListView 페이지에서 목록 필터링에 관한 더 강력하고 심층적인 예제를 확인하세요.
- [XAML 컨트롤 갤러리 앱(Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT) 가져오기

### <a name="related-articles"></a>관련된 문서
- [목록](lists.md)
- [목록 보기 및 그리드 보기](listview-and-gridview.md)
- [컬렉션 명령](collection-commanding.md)
