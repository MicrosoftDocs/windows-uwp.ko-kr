---
author: Jwmsft
Description: A text entry box that provides suggestions as the user types.
title: 콤보 상자 (드롭다운 목록)
label: Combo box
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 518ce49ddb631e3e914a6c7662b4e74de247c29c
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4539118"
---
# <a name="combo-box"></a>콤보 상자

콤보 상자 (드롭 다운 목록)를 사용 하 여 사용자가 선택할 수 있는 항목의 목록을 표시 합니다. 콤보 상자 압축 상태로 시작 하 고 선택할 수 있는 항목의 목록을 표시 하도록 확장 됩니다.

콤보 상자를 닫을 때 현재 선택 항목을 표시 하거나 선택한 항목이 없는 경우 비어 있습니다. 콤보 상자를 확장 하는 사용자를 선택 가능한 항목 목록이 표시 됩니다.

> **중요 Api**: [ComboBox 클래스](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [IsEditable 속성](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [Text 속성](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted 이벤트](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

헤더를 사용 하 여 압축 상태의 콤보 상자입니다.

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
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 된 경우 여기를 클릭 <a href="xamlcontrolsgallery:/item/ComboBox">앱을 열고 중인 콤보 상자를 참조 하세요</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
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

[Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션에 직접 개체를 추가 하거나 [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 데이터 원본에 바인딩하여 콤보 상자를 채웁니다. 콤보 상자에 추가 된 항목 [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) 컨테이너에서 줄 바꿈 합니다.

XAML에서 추가 된 간단한 콤보 상자는 다음과 같습니다.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue<x:String>
    <x:String>Green<x:String>
    <x:String>Red<x:String>
    <x:String>Yellow<x:String>
</ComboBox>
```

다음 예제에서는 콤보 상자 FontFamily 개체의 컬렉션에 바인딩

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

ListView 및 GridView 같은 ComboBox 같은 표준 방식으로 사용자의 선택 항목을 가져올 수 있는 [선택기](/uwp/api/windows.ui.xaml.controls.primitives.selector)파생 됩니다.

있습니다 수 가져오기 또는 설정 콤보 상자 항목 사용 하 여 [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 속성을 선택 하 고 가져오거나 [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 속성을 사용 하 여 선택한 항목의 인덱스를 설정 합니다.

선택한 데이터 항목에는 특정 속성의 값을 얻기 위해 [속성을](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) 사용할 수 있습니다. 이 경우 속성에서 값을 가져오려면 선택한 항목에 지정 [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) 를 설정 합니다.

> [!TIP]
> 기본 선택 했음을 SelectedItem 또는 SelectedIndex를 설정 하는 경우 콤보 상자 항목 컬렉션 채워지면 전에 속성을 설정 하는 경우 예외가 발생 합니다. XAML에서 항목을 정의 하지 않는 한 콤보 상자 로드 된 이벤트를 처리 하 고 SelectedItem 또는 SelectedIndex Loaded 이벤트 처리기에서 설정 하는 것이 좋습니다.

XAML에서 이러한 속성에 바인딩할 수도 있고 선택이 변경에 응답할 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트를 처리할 수 있습니다.

이벤트 처리기 코드에서는 [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 속성에서 선택한 항목을 가져올 수 있습니다. [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 속성에서 이전에 선택한 항목 (있는 경우)를 가져올 수 있습니다. 콤보 상자 다중 선택을 지원 하지 않으므로 AddedItems 및 RemovedItems 컬렉션 항목을 하나만 포함 합니다.

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

기본적으로 SelectionChanged 이벤트는 사용자가 클릭 하거나 탭, 자신의 선택을 하려면 목록에서 항목에서 Enter 키를 누르면 콤보 상자 닫힐 때 발생 합니다. 키보드 화살표 키를 사용 하 여 열려 있는 콤보 상자 목록을 탐색할 때 선택 변경 되지 않습니다.

한 업데이트는 콤보 상자는 "라이브" 사용자 열기 목록 (예: 글꼴 선택 드롭다운) 화살표 키를 사용 하 여 탐색 하는 동안 [항상](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger) [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) 를 설정 합니다. 그러면 SelectionChanged 이벤트를 열기 목록의 다른 항목으로 포커스가 변경 될 때 발생 합니다.

#### <a name="selected-item-behavior-change"></a>선택한 항목 동작 변경

RS5에서 (Windows SDK 버전 (Windows 10, 버전 YYMM) 10.0.NNNNN.0 선택한 항목의 동작 편집 가능한 콤보 상자를 지원 하도록 업데이트 됩니다.

이전 RS5 SelectedItem 속성의 값 (및 따라서 SelectedValue 및 SelectedIndex) Items 컬렉션에 있어야 할 필요 했습니다. 설정 이전 예제를 사용 하 여 `colorComboBox.SelectedItem = "Pink"` 결과:

- SelectedItem = null
- SelectedValue = null
- SelectedIndex-1 =

RS5 이상, SelectedItem 속성의 값 (및 따라서 SelectedValue 및 SelectedIndex) Items 컬렉션에 있이 필요가 없습니다. 설정 이전 예제를 사용 하 여 `colorComboBox.SelectedItem = "Pink"` 결과:

- SelectedItem 핑크 =
- SelectedValue 분홍색 =
- SelectedIndex-1 =

### <a name="text-search"></a>텍스트 검색

콤보 상자는 자동으로 컬렉션 내의 검색을 지원합니다. 사용자가 열리거나 닫힌 콤보 상자에 포커스가 있는 상태에서 실제 키보드를 통해 문자를 입력하면 사용자 문자열과 일치하는 항목이 표시됩니다. 이 기능은 긴 목록을 탐색할 때 특히 유용합니다. 예를 들어 상태 목록이 포함 된 드롭다운을 조작 하는 경우 사용자가 빠른 선택을 위해 보기로 "워싱턴"를 "w" 키를 눌러 수 있습니다. 텍스트 검색은 대/소문자 구분 하지 않습니다.

[IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) 속성을 **false** 로이 기능을 사용 하지 않도록 설정할 수 있습니다.

## <a name="make-a-combo-box-editable"></a>콤보 상자를 편집할 수 있게

> [!IMPORTANT]
> 이 기능은는 [최신 Windows 10 Insider Preview 빌드 및 SDK가](https://insider.windows.com/for-developers/)필요 합니다.

콤보 상자는 기본적으로 사용자 수 있도록 미리 정의 된 옵션 목록에서 선택 합니다. 그러나 목록에 유효한 값의 하위 집합만 포함 되어 있고 사용자가 나열 되지 않은 다른 값을 입력할 수 있습니다. 이 지원 하기 위해 할 수 콤보 상자 편집할 수 있습니다.

콤보 상자를 편집할 수 있게 [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) 속성을 **true**로 설정 합니다. 그런 다음 사용자가 입력 한 값과 작동 하도록 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 이벤트를 처리 합니다.

기본적으로 사용자 지정 텍스트를 커밋합니다 SelectedItem 값 업데이트 됩니다. **Handled** 를 **true** TextSubmitted 이벤트 인수에서 설정 하 여이 동작을 재정의할 수 있습니다. 이벤트가 처리 된 것으로 표시 되 면 콤보 상자는 이벤트 후 없는 추가 작업을 수행 하 고 편집 상태가 유지 됩니다. SelectedItem 업데이트 되지 않습니다.

이 예제에서는 간단한 편집 가능한 콤보 상자를 보여 줍니다. 목록에 간단한 문자열 및 사용자가 입력 한 모든 값이 입력으로 사용 됩니다.

"최근에 사용한 이름" 선택 사용자 지정 문자열을 입력할 수 있습니다. 'RecentlyUsedNames' 목록에서 선택할 수 있는 몇 가지 값이 포함 되지만 사용자가 새, 사용자 지정 값을 추가할 수도 있습니다. 'CurrentName' 속성에 현재 입력 된 이름을 나타냅니다.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>제출 된 텍스트

사용자가 입력 한 값과 작동 하도록 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 이벤트를 처리할 수 있습니다. 이벤트 처리기를 하면 일반적으로 유효성이 검사는 사용자가 입력 한 값이 유효한 지 앱에 값을 사용 합니다. 상황에 따라 나중에 사용할 수 있는 옵션 목록 콤보 상자에 값을 추가할 수 있습니다.

TextSubmitted 이벤트 이러한 조건이 충족 될 때 발생 합니다.

- IsEditable 속성은 **true** 입니다.
- 사용자가 콤보 상자 목록에서 기존 항목 일치 하지 않는 텍스트 입력
- 사용자가 Enter, 또는 콤보 상자에서 포커스를 이동 합니다.

사용자가 텍스트를 입력 하 고 목록 위쪽 또는 아래쪽으로 이동 하는 경우에 TextSubmitted 이벤트가 발생 하지 않습니다.

### <a name="sample---validate-input-and-use-locally"></a>샘플-입력의 유효성을 검사 하 고 로컬로 사용

이 리터럴의 예 글꼴 크기 선택을 글꼴 크기 램프 해당 값의 집합을 포함 하지만 사용자 목록에 없는 글꼴 크기를 입력할 수 있습니다.

사용자 목록, 글꼴 크기 업데이트 하지만 값에 있지 않은 값을 추가 하는 경우 글꼴 크기의 목록에 추가 되지 않습니다.

새로 입력 한 값은 SelectedValue Text 속성을 마지막 되돌리고를 사용 하 여 유효 하지 않으면 좋은 값을 알 수 있습니다.

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

### <a name="sample---validate-input-and-add-to-list"></a>샘플-입력의 유효성을 검사 하 고 목록에 추가

여기서 "선호 하는 색 선택" (빨강, 파랑, 녹색, Orange) 가장 일반적인 즐겨찾기 색상을 포함 하지만 사용자 목록에 없는 원하는 색상을 입력할 수 있습니다. 유효한 색 (예: 분홍색)를 추가 하는 사용자를 새로 입력된 색상 목록에 추가 되 고 활성 "선호 하는 색"으로 설정 합니다.

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

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [AutoSuggestBox 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>관련 문서

- [텍스트 컨트롤](text-controls.md)
- [맞춤법 검사](text-controls.md)
- [검색](search.md)
- [TextBox 클래스](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 클래스](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 속성](https://msdn.microsoft.com/library/system.string.length.aspx)
