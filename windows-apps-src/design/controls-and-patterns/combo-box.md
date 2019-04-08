---
Description: 사용자가 입력할 때 제안을 제공하는 텍스트 입력 상자입니다.
title: 콤보 상자 (드롭다운 목록)
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 21a6c698fa0e07587e2c25ae827dc6654a8ced9d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618628"
---
# <a name="combo-box"></a>콤보 상자

콤보 상자 (드롭다운 목록이 라고도 함)를 사용 하 여 사용자에서 선택할 수 있는 항목의 목록을 표시 합니다. 콤보 상자 compact 상태에서 시작 하 고 선택 가능한 항목의 목록을 표시 하려면 확장 합니다.

콤보 상자를 닫을 때 현재 선택 영역을 표시 하거나 선택된 된 항목이 없는 경우 비어 있습니다. 콤보 상자를 확장 하는 사용자를 선택할 수 있는 항목 목록이 표시 됩니다.

> **중요 API**: [ComboBox 클래스](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)하십시오 [IsEditable 속성](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)를 [Text 속성](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted 이벤트](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

헤더를 사용 하 여 compact 상태의 콤보 상자입니다.

![압축 상태의 드롭다운 목록의 예](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

- 드롭다운 목록을 사용하여 사용자가 한 줄의 텍스트로 적절하게 나타낼 수 있는 일련의 항목에서 단일 값을 선택할 수 있도록 합니다.
- 여러 줄의 텍스트나 이미지가 포함된 항목을 표시하려면 콤보 상자 대신 목록 또는 그리드 보기를 사용합니다.
- 항목이 4개 이하이면 [라디오 단추](radio-button.md)(한 항목만 선택할 수 있는 경우) 또는 [확인란](checkbox.md)(여러 항목을 선택할 수 있는 경우)을 사용합니다.
- 선택 항목이 앱 흐름에서 두 번째로 중요한 경우 콤보 상자를 사용합니다. 대부분의 상황에서 대부분의 사용자에게 기본 옵션이 권장되는 경우 목록 보기를 사용하여 모든 항목을 표시하면 필요 이상으로 옵션에 주의를 집중시킬 수 있습니다. 콤보 상자를 사용하여 공간을 절약하고 주의 분산을 최소화할 수 있습니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 <a href="xamlcontrolsgallery:/item/ComboBox">앱을 열고 작업에서 콤보 상자를 참조 하세요.</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
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

개체를 직접 추가 하 여 콤보 상자를 채우는 합니다 [항목](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션 또는 바인딩를 [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 데이터 소스입니다. 콤보 상자에 추가 된 항목에 래핑됩니다 [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) 컨테이너입니다.

XAML에 추가 된 항목을 사용 하 여 간단한 콤보 상자는 다음과 같습니다.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

다음 예제에서는 콤보 상자 FontFamily 개체의 컬렉션에 바인딩하는 방법을 보여 줍니다.

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

콤보 상자에서 파생 된 ListView 및 GridView와 마찬가지로 [선택기](/uwp/api/windows.ui.xaml.controls.primitives.selector)이므로 동일한 표준 방식으로 사용자의 선택 항목을 가져올 수 있습니다.

가져오거나 사용 하 여 콤보 상자의 선택 되어 있는 항목을 설정할 수 있습니다 합니다 [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 속성 및 get 또는 set를 사용 하 여 선택한 항목의 인덱스를 [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 속성입니다.

선택한 데이터 항목에는 특정 속성의 값을 검색할 사용할 수 있습니다 합니다 [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) 속성입니다. 이 경우에 설정 합니다 [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) 에서 값을 가져오려면 선택된 항목에는 속성을 지정 합니다.

> [!TIP]
> 기본 선택 항목을 나타내기 위해 SelectedItem 또는 SelectedIndex 설정 하면 콤보 상자 항목 컬렉션이 채워집니다 전에 속성이 설정 된 경우 예외가 발생 합니다. XAML에서 항목을 정의 하지 않으면 콤보 상자 Loaded 이벤트를 처리 하 고 Loaded 이벤트 처리기의 SelectedItem 또는 SelectedIndex 설정 하는 것이 좋습니다.

처리 하거나, XAML에서 이러한 속성에 바인딩할 수 있습니다 합니다 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 선택 변경에 대응 하는 이벤트입니다.

이벤트 처리기 코드를 가져올 수 있습니다에서 선택한 항목의 [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 속성입니다. (해당 되는 경우)에서 이전에 선택한 항목을 가져올 수는 [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 속성입니다. AddedItems 및 RemovedItems 컬렉션 콤보 상자에서 다중 선택을 지원 하지 않으므로 항목을 하나만 포함 합니다.

이 예제에서는 SelectionChanged 이벤트를 처리 하는 방법 및 선택한 항목에 바인딩하는 방법을 보여 줍니다.

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

기본적으로 SelectionChanged 이벤트에는 사용자가 탭 하거나 선택, 커밋 목록에서 항목을 누르면 콤보 상자가 닫힐 때 발생 합니다. 사용자가 키보드의 화살표 키를 사용 하 여 열고 콤보 상자 목록 탐색 선택 변경 되지 않습니다.

콤보의 사용자가 (예: 글꼴 선택 드롭다운) 화살표 키를 사용 하 여 목록 열기를 탐색 하는 동안 해당 "live updates" 상자를 설정 [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) 하 [항상](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)합니다. 그러면 SelectionChanged 이벤트를 열고 목록에서 다른 항목에 포커스가 변경 될 때 발생 합니다.

#### <a name="selected-item-behavior-change"></a>선택한 항목의 동작 변경

Windows 10, 버전 1809 ([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk))은 편집할 수 있는 콤보 상자를 지원 하도록 선택한 항목의 동작을 업데이트 하는 나중에 또는 합니다.

이전 SDK 17763, SelectedItem 속성의 값 (및 따라서 SelectedValue 및 SelectedIndex) 콤보 상자의 항목 컬렉션에 포함 되도록 필요 했습니다. 설정, 이전 예제를 사용 하 여 `colorComboBox.SelectedItem = "Pink"` 결과:

- SelectedItem = null
- SelectedValue = null
- SelectedIndex =-1

Sdk 17763 이상 SelectedItem 속성의 값 (및 따라서 SelectedValue 및 SelectedIndex) 콤보 상자의 항목 컬렉션에 있이 필요가 없습니다. 설정, 이전 예제를 사용 하 여 `colorComboBox.SelectedItem = "Pink"` 결과:

- SelectedItem Pink =
- SelectedValue Pink =
- SelectedIndex =-1

### <a name="text-search"></a>텍스트 검색

콤보 상자는 자동으로 컬렉션 내의 검색을 지원합니다. 사용자가 열리거나 닫힌 콤보 상자에 포커스가 있는 상태에서 실제 키보드를 통해 문자를 입력하면 사용자 문자열과 일치하는 항목이 표시됩니다. 이 기능은 긴 목록을 탐색할 때 특히 유용합니다. 예를 들어 상태 목록이 포함 된 드롭다운을 사용 하 여 상호 작용을 하는 경우 사용자 수 빠른 선택에 대 한 보기를 "Washington" 가져오는 "w" 키를 누릅니다. 텍스트 검색은 대/소문자 구분 하지 않습니다.

설정할 수 있습니다 합니다 [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) 속성을 **false** 이 기능을 사용 하지 않도록 설정 합니다.

## <a name="make-a-combo-box-editable"></a>콤보 상자의 편집 가능 하 게

> [!IMPORTANT]
> 이 기능을 사용 하려면 Windows 10 버전 1809 ([17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상.

콤보 상자에는 사용자 수 있도록 기본적으로 미리 정의 된 옵션 목록에서 선택 합니다. 그러나 목록에는 유효한 값의 하위 집합만 포함 하 고 사용자는 나열 되지 않은 다른 값을 입력할 수 있어야 합니다. 있는 경우가 있습니다. 이 지원 하려면 할 수 있습니다 콤보 상자의 편집 가능.

콤보 상자를 편집할 수 있도록 설정 합니다 [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) 속성을 **true**합니다. 그런 다음 처리 합니다 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 사용자가 입력 한 값을 사용 하는 이벤트입니다.

기본적으로 사용자는 사용자 지정 텍스트를 커밋할 때 SelectedItem 값 업데이트 됩니다. 설정 하 여이 동작을 재정의할 수 있습니다 **Handled** 하 **true** TextSubmitted 이벤트 인수에서. 이벤트에는 처리 된 것으로 표시 되 면 콤보 상자 이벤트 후 추가 작업이 없으므로 걸립니다 및 편집 상태로 유지 됩니다. SelectedItem 업데이트 되지 않습니다.

이 예제에서는 간단한 편집할 수 있는 콤보 상자를 보여 줍니다. 목록에는 간단한 문자열을 포함 하 고 사용자가 입력 한 모든 값이 입력 한 대로 사용 됩니다.

"최근 사용 된 이름을" 선택기는 사용자 지정 문자열을 입력할 수가 있습니다. 'RecentlyUsedNames' 목록에서 선택할 수는 몇 가지 값을 포함 하지만 사용자 새 사용자 지정 값을 추가할 수도 있습니다. 'CurrentName' 속성은 현재 입력 한 이름을 나타냅니다.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>전송 되는 텍스트

처리할 수 있습니다 합니다 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 사용자가 입력 한 값을 사용 하는 이벤트입니다. 이벤트 처리기를 일반적으로 확인 된 사용자가 입력 한 값이 유효한 지, 선택한 앱의 값을 사용 합니다. 상황에 따라 나중에 사용할 옵션 콤보 상자의 목록에 값을 추가할 수 있습니다.

TextSubmitted 이벤트에는 이러한 조건이 충족 될 때 발생 합니다.

- IsEditable 속성이 **true**
- 콤보 상자 목록의 기존 항목과 일치 하지 않는 텍스트 입력
- Enter 키를 누르거나 콤보 상자에서 포커스를 이동 합니다.

사용자가 텍스트를 입력 하 고 다음 목록 위로 또는 아래로 탐색 하는 경우에 TextSubmitted 이벤트가 발생 하지 않습니다.

### <a name="sample---validate-input-and-use-locally"></a>샘플-입력의 유효성을 검사 하 고 로컬로 사용

이 예제에서는 글꼴 크기 선택에는 해당 글꼴 크기 진입 하는 값 집합을 포함 하지만 사용자 목록에 있지 않은 글꼴 크기를 입력할 수 있습니다.

사용자 목록, 글꼴 크기 업데이트는 없지만 값에 있지 않은 값을 추가 하는 경우 글꼴 크기 목록에 추가 되지 않습니다.

새로 입력 한 값을 유효 하지 않은 경우에 SelectedValue를 사용 하 여 Text 속성을 마지막으로 되돌리려면 알려진 좋은 값입니다.

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

### <a name="sample---validate-input-and-add-to-list"></a>샘플-입력의 유효성을 검사 하 고 목록에 추가 합니다.

여기에서 "좋아하는 색 선택기"는 가장 일반적인 좋아하는 색 (빨강, 파랑, 녹색, 주황색)를 포함 하지만 사용자 목록에 없는 좋아하는 색을 입력할 수 있습니다. 사용자가 유효한 색 (예: Pink)에 추가 하는 경우 새로 입력 한 색 목록에 추가 되 고 "즐겨 찾는 color" 활성으로 설정 합니다.

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

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [AutoSuggestBox 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>관련 문서

- [텍스트 컨트롤](text-controls.md)
- [맞춤법 검사](text-controls.md)
- [검색](search.md)
- [TextBox 클래스](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 클래스](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 속성이](https://msdn.microsoft.com/library/system.string.length.aspx)
