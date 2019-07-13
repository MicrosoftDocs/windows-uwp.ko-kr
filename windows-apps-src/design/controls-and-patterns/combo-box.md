---
Description: 사용자가 입력할 때 제안을 제공하는 텍스트 입력 상자입니다.
title: 콤보 상자(드롭다운 목록)
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 4ea8c4476a47c8bd00ce74e893e2f0a730615a99
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319158"
---
# <a name="combo-box"></a>콤보 상자

콤보 상자(드롭다운 목록이라고도 함)를 사용하여 사용자가 선택할 수 있는 항목 목록을 표시할 수 있습니다. 콤보 상자는 압축 상태에서 시작하여 선택 가능한 항목 목록을 표시하도록 확장됩니다.

콤보 상자를 닫으면 콤보 상자는 현재 선택한 항목을 표시하거나 선택된 항목이 없는 경우 비어 있습니다. 사용자가 콤보 상자를 확장하면 콤보 상자는 선택 가능한 항목 목록을 표시합니다.

> **중요 API**: [ComboBox 클래스](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [IsEditable 속성](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [Text 속성](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted 이벤트](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

헤더를 표시하는 압축 상태의 콤보 상자.

![압축 상태의 드롭다운 목록의 예](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

- 드롭다운 목록을 사용하여 사용자가 한 줄의 텍스트로 적절하게 나타낼 수 있는 일련의 항목에서 단일 값을 선택할 수 있도록 합니다.
- 여러 줄의 텍스트나 이미지가 포함된 항목을 표시하려면 콤보 상자 대신 목록 또는 그리드 보기를 사용합니다.
- 항목이 4개 이하이면 [라디오 단추](radio-button.md)(한 항목만 선택할 수 있는 경우) 또는 [확인란](checkbox.md)(여러 항목을 선택할 수 있는 경우)을 사용합니다.
- 선택 항목이 앱 흐름에서 두 번째로 중요한 경우 콤보 상자를 사용합니다. 대부분의 상황에서 대부분의 사용자에게 기본 옵션이 권장되는 경우 목록 보기를 사용하여 모든 항목을 표시하면 필요 이상으로 옵션에 주의를 집중시킬 수 있습니다. 콤보 상자를 사용하여 공간을 절약하고 주의 분산을 최소화할 수 있습니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/ComboBox">앱을 열고 ComboBox가 실제로 작동하는 모습을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

압축 상태의 콤보 상자는 헤더를 표시할 수 있습니다.

![압축 상태의 드롭다운 목록의 예](images/combo_box_collapsed.png)

콤보 상자는 더 긴 문자열 길이를 지원하도록 확장되지만 읽기 어려운 너무 긴 문자열은 피해야 합니다.

![텍스트 문자열이 긴 드롭다운 목록의 예](images/combo_box_listitemstate.png)

콤보 상자에 있는 컬렉션이 긴 경우 이를 수용할 수 있는 스크롤 막대가 표시됩니다. 목록의 항목을 논리적으로 그룹화합니다.

![드롭다운 목록에 있는 스크롤 막대의 예](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>콤보 상자 만들기

[Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션에 직접 항목을 추가하거나 [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 데이터 원본에 바인딩하여 콤보 상자를 채울 수 있습니다. ComboBox에 추가된 항목은 [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) 컨테이너에 래핑됩니다.

다음은 XAML로 항목이 추가된 간단한 콤보 상자입니다.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

다음 예제에서는 FontFamily 개체 컬렉션에 콤보 상자를 바인딩하는 방법을 보여줍니다.

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>항목 선택

ListView 및 GridView와 마찬가지로, ComboBox는 [선택기](/uwp/api/windows.ui.xaml.controls.primitives.selector)에서 파생되므로 동일한 표준 방식으로 사용자의 선택 항목을 가져올 수 있습니다.

[SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 속성을 사용하여 콤보 상자의 선택 항목을 가져오거나 설정하고, [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 속성을 사용하여 선택된 항목의 인덱스를 가져오거나 설정할 수 있습니다.

선택한 데이터 항목의 특정 속성 값을 가져오려면 [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) 속성을 사용합니다. 이 예제에서는 [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath)를 설정하여 선택한 항목에서 값을 가져올 속성을 지정합니다.

> [!TIP]
> 기본 선택 항목을 나타내기 위해 SelectedItem 또는 SelectedIndex를 설정하는 경우 콤보 상자 항목 컬렉션이 채워지기 전에 속성이 설정되면 예외가 발생합니다. XAML로 항목을 정의하지 않는 이상, 콤보 상자 Loaded 이벤트를 처리하고 Loaded 이벤트 처리기의 SelectedItem 또는 SelectedIndex를 설정하는 것이 좋습니다.

이러한 속성을 XAML로 바인딩할 수도 있고, 선택 변경에 대응하도록 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트를 처리할 수도 있습니다.

이벤트 처리기 코드에서는 [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 속성에서 선택 항목을 가져올 수 있습니다. [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 속성에서 이전에 선택한 항목(있는 경우)을 가져올 수 있습니다. 콤보 상자는 다중 선택을 지원하지 않기 때문에 AddedItems 및 RemovedItems 컬렉션은 각각 1개 항목만 포함합니다.

이 예제에서는 SelectionChanged 이벤트를 처리하는 방법과 선택한 항목에 바인딩하는 방법을 보여줍니다.

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged 및 키보드 탐색

기본적으로 사용자가 목록의 항목을 클릭하거나 탭하거나 Enter 키를 누르면 SelectionChanged 이벤트가 발생하고, 콤보 상자가 닫힙니다. 사용자가 키보드의 화살표 키를 사용하여 열려 있는 콤보 상자 목록을 탐색할 때 선택 항목이 변경되지 않습니다.

사용자가 화살표 키를 사용하여 열려 있는 목록(예: 글꼴 선택 드롭다운)을 탐색하는 동안 "실시간으로 업데이트되는" 콤보 상자를 만들려면 [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger)를 [Always](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)로 설정합니다. 그러면 열려 있는 목록의 다른 항목으로 포커스가 변경될 때 SelectionChanged 이벤트가 발생합니다.

#### <a name="selected-item-behavior-change"></a>선택한 항목의 동작 변경

Windows 10 버전 1809([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상에서는 편집 가능한 콤보 상자를 지원하도록 선택한 항목의 동작이 업데이트됩니다.

SDK 17763 이전 버전에서는 SelectedItem 속성(및 따라서 SelectedValue 및 SelectedIndex)의 값이 반드시 콤보 상자의 항목 컬렉션에 있어야 했습니다. 이전 예제를 사용하여 `colorComboBox.SelectedItem = "Pink"`로 설정하면 다음과 같은 결과를 얻게 됩니다.

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

SDK 17763 이상 버전에서는 SelectedItem 속성(및 따라서 SelectedValue 및 SelectedIndex)의 값이 콤보 상자의 항목 컬렉션에 있지 않아도 됩니다. 이전 예제를 사용하여 `colorComboBox.SelectedItem = "Pink"`로 설정하면 다음과 같은 결과를 얻게 됩니다.

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>텍스트 검색

콤보 상자는 자동으로 컬렉션 내의 검색을 지원합니다. 사용자가 열리거나 닫힌 콤보 상자에 포커스가 있는 상태에서 실제 키보드를 통해 문자를 입력하면 사용자 문자열과 일치하는 항목이 표시됩니다. 이 기능은 긴 목록을 탐색할 때 특히 유용합니다. 예를 들어 상태 목록이 포함된 드롭다운을 조작할 때 사용자는 빠른 선택을 위해 "w" 키를 눌러 "워싱턴"을 표시할 수 있습니다. 텍스트 검색은 대/소문자를 구분하지 않습니다.

[IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) 속성을 **false**로 설정하여 이 기능을 해제할 수 있습니다.

## <a name="make-a-combo-box-editable"></a>콤보 상자를 편집 가능하게 만들기

> [!IMPORTANT]
> 이 기능을 사용하려면 Windows 10 버전 1809([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상이 필요합니다.

기본적으로 콤보 상자는 사용자가 미리 정의된 옵션 목록에서 선택할 수 있습니다. 그러나 목록에 유효한 값이 몇 개 없어서 사용자가 목록에 없는 다른 값을 입력할 수 있어야 하는 경우가 있습니다. 이 상황을 지원하려면 콤보 상자를 편집 가능하게 만들면 됩니다.

콤보 상자를 편집 가능하게 만들려면 [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) 속성을 **true**로 설정합니다. 그런 다음, 사용자가 입력한 값을 작업하는 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 이벤트를 처리합니다.

기본적으로 SelectedItem 값은 사용자가 사용자 지정 텍스트를 커밋할 때 업데이트됩니다. TextSubmitted 이벤트 인수에서 **Handled**를 **true**로 설정하여 이 동작을 재정의할 수 있습니다. 이벤트가 처리된 것으로 표시되면 콤보 상자는 이벤트 이후에 더 이상 작업을 수행하지 않으며 편집 중 상태를 유지합니다. SelectedItem은 업데이트 되지 않습니다.

이 예제에서는 간단한 편집 가능한 콤보 상자를 보여줍니다. 목록에는 간단한 문자열이 포함되어 있고, 사용자가 입력한 값은 입력된 그대로 사용됩니다.

사용자는 "최근에 사용된 이름" 선택기를 통해 사용자 지정 문자열을 입력할 수 있습니다. 'RecentlyUsedNames' 목록에는 사용자가 선택할 수 있는 값이 포함되어 있지만, 사용자는 새로운 사용자 지정 값을 추가할 수도 있습니다. 'CurrentName' 속성은 현재 입력된 이름을 나타냅니다.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>제출된 텍스트

사용자가 입력한 값을 작업하는 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 이벤트를 처리할 수 있습니다. 일반적으로 이벤트 처리기에서는 사용자가 입력한 값이 유효한지 확인한 다음, 해당 값을 앱에 사용합니다. 상황에 따라 나중에 사용할 수 있도록 값을 콤보 상자의 옵션 목록에 추가할 수도 있습니다.

TextSubmitted 이벤트는 다음 조건이 충족될 때 발생합니다.

- IsEditable 속성이 **true**
- 사용자가 콤보 상자 목록의 기존 항목과 일치하지 않는 텍스트를 입력
- 사용자가 Enter 키를 누르거나 콤보 상자에서 포커스를 이동

사용자가 텍스트를 입력하고 목록을 위 또는 아래로 탐색하는 경우에는 TextSubmitted 이벤트가 발생하지 않습니다.

### <a name="sample---validate-input-and-use-locally"></a>샘플 - 입력의 유효성을 검사하고 로컬로 사용

이 예제의 글꼴 크기 선택기는 글꼴 크기 램프에 해당하는 값 세트를 포함하고 있지만, 사용자가 목록에 없는 글꼴 크기를 입력할 수도 있습니다.

사용자가 목록에 없는 값을 추가하면 글꼴 크기가 업데이트되지만, 값은 글꼴 크기 목록에 추가되지 않습니다.

새로 입력한 값이 유효하지 않은 경우 SelectedValue를 사용하여 Text 속성을 마지막으로 알려진 유효한 값으로 되돌립니다.

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>샘플 - 입력의 유효성을 검사하고 목록에 추가

여기서 "좋아하는 색 선택기"는 대중적으로 가장 인기가 많은 색(빨간색, 파란색, 녹색, 주황색)을 포함하고 있지만, 사용자가 목록에 없는 색을 입력할 수도 있습니다. 사용자가 유효한 색(예: 분홍색)을 추가하면 새로 입력된 색이 목록에 추가되고 활성 "좋아하는 색"으로 설정됩니다.

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 콤보 상자 항목의 텍스트 콘텐츠를 한 줄로 제한합니다.
- 가장 논리적인 순서로 콤보 상자의 항목을 정렬합니다. 관련 옵션을 그룹화하고 가장 일반적인 옵션을 맨 위에 배치합니다. 이름은 사전순으로 정렬하고, 숫자는 숫자순으로 정렬하고, 날짜는 시간순으로 정렬합니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.
- [AutoSuggestBox 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>관련 문서

- [텍스트 컨트롤](text-controls.md)
- [맞춤법 검사](text-controls.md)
- [검색](search.md)
- [TextBox 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length 속성](https://docs.microsoft.com/dotnet/api/system.string.length?redirectedfrom=MSDN#System_String_Length)
