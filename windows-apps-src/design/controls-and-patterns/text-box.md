---
author: Jwmsft
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
Description: The TextBox control lets a user enter text into an app.
title: 입력란
label: Text box
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 43af446513ebb857858c21f0b2859af6febd82d0
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6845874"
---
# <a name="text-box"></a>입력란

 

TextBox 컨트롤을 사용하면 사용자가 앱에 텍스트를 입력할 수 있습니다. 일반적으로 한 줄의 텍스트를 캡처하는 데 사용되지만 여러 줄의 텍스트를 캡처하도록 구성할 수 있습니다. 텍스트는 단순하고 균일한 일반 텍스트 형식으로 화면에 표시됩니다.

TextBox에는 텍스트 입력을 간소화할 수 있는 다양한 기능이 있습니다. 텍스트 복사 및 붙여넣기를 지원하는 친숙한 기본 제공 상황에 맞는 메뉴와 함께 제공됩니다. "모두 지우기" 단추를 통해 사용자는 입력된 모든 텍스트를 빠르게 삭제할 수 있습니다. 또한 맞춤법 검사 기능이 기본 제공되며 기본적으로 사용됩니다.

> **중요 API**: [TextBox 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx), [Text 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

사용자가 양식 등에서 서식 없는 텍스트를 입력하고 편집할 수 있게 하려면 **TextBox** 컨트롤을 사용합니다. [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) 속성을 사용하여 TextBox에 텍스트를 가져오고 설정할 수 있습니다.

TextBox를 읽기 전용으로 만들 수 있지만 일시적인 조건부 상태여야 합니다. 텍스트를 편집할 수 없는 경우 [TextBlock](text-block.md)을 대신 사용하는 것이 좋습니다.

[PasswordBox](password-box.md) 컨트롤을 사용하여 암호 또는 주민 등록 번호 등의 기타 개인 데이터를 수집합니다. 암호 상자는 텍스트 입력란처럼 보이지만 입력된 텍스트 대신 글머리 기호를 렌더링합니다.

사용자가 검색어를 입력할 수 있게 하거나 입력 시 선택할 제안 목록을 사용자에게 표시하려면 [AutoSuggestBox](auto-suggest-box.md)를 사용합니다.

서식 있는 텍스트 파일을 표시하고 편집하려면 [RichEditBox](rich-edit-box.md)를 사용합니다.

올바른 텍스트 컨트롤을 선택하는 방법에 대한 자세한 내용은 [텍스트 컨트롤](text-controls.md) 문서를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/TextBox">앱을 열고 작동 중인 TextBox를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

![입력란](images/text-box.png)

## <a name="create-a-text-box"></a>입력란 만들기

다음은 헤더 및 개체 틀 텍스트가 있는 간단한 입력란에 대한 XAML입니다.

```xaml
<TextBox Width="500" Header="Notes" PlaceholderText="Type your notes here"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Width = 500;
textBox.Header = "Notes";
textBox.PlaceholderText = "Type your notes here";
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

다음은 이 XAML의 결과로 나온 입력란입니다.

![간단한 입력란](images/text-box-ex1.png)

### <a name="use-a-text-box-for-data-input-in-a-form"></a>양식의 데이터 입력에는 입력란을 사용합니다.

입력란을 사용하여 양식의 데이터 입력을 수락하고 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) 속성을 사용하여 입력란에서 전체 텍스트 문자열을 가져오는 것이 일반적입니다. 일반적으로 제출 단추 클릭과 같은 이벤트를 사용하여 Text 속성에 액세스하지만, 텍스트가 변경될 때 특정 작업을 수행해야 하는 경우 [TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanged.aspx) 또는 [TextChanging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanging.aspx) 이벤트를 처리할 수 있습니다.

[Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.header.aspx)(또는 레이블) 및 [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.placeholdertext.aspx)(또는 워터마크)를 입력란에 추가하여 입력란의 용도를 사용자에게 표시할 수 있습니다. 헤더의 모양을 사용자 지정하려면 Header 대신 [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.headertemplate.aspx) 속성을 설정할 수 있습니다. *디자인 정보는 레이블에 대한 지침을 참조하세요*.

[MaxLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.maxlength.aspx) 속성을 설정하여 사용자가 입력할 수 있는 문자 수를 제한할 수 있습니다. 그러나 MaxLength는 붙여넣은 텍스트의 길이는 제한하지 않습니다. 앱에 중요한 경우 붙여넣은 텍스트를 수정하려면 [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) 이벤트를 사용합니다.

입력란에는 상자에 텍스트를 입력할 때 표시되는 모두 지우기 단추("X")가 포함됩니다. 사용자가 "X"를 클릭하면 입력란의 텍스트가 지워집니다. 모양은 다음과 같습니다.

![모두 지우기 단추가 있는 입력란](images/text-box-clear-all.png)

모두 지우기 단추는 텍스트를 포함하며 포커스가 있는 편집 가능한 한 줄 입력란에만 표시됩니다.

다음과 같은 경우에는 모두 지우기 단추가 표시되지 않습니다.
- **IsReadOnly**가 **true**인 경우
- **AcceptsReturn**이 **true**인 경우
- **TextWrap**에 **NoWrap** 이외의 값이 있는 경우

### <a name="make-a-text-box-read-only"></a>입력란을 읽기 전용으로 만들기

[IsReadOnly](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isreadonly.aspx) 속성을 **true**로 설정하여 입력란을 읽기 전용으로 만들 수 있습니다. 일반적으로 앱의 조건에 따라 앱 코드에서 이 속성을 전환합니다. 항상 읽기 전용인 텍스트가 필요한 경우 TextBlock을 대신 사용하는 것이 좋습니다.

IsReadOnly 속성을 true로 설정하여 TextBox를 읽기 전용으로 만들 수 있습니다. 예를 들어 사용자가 설명을 입력하는 TextBox를 특정 조건에서만 사용할 수 있게 할 수 있습니다. 조건이 충족될 때까지 TextBox를 읽기 전용으로 만들 수 있습니다. 텍스트를 표시만 해야 하는 경우 TextBlock 또는 RichTextBlock을 대신 사용하는 것이 좋습니다.

읽기 전용 입력란은 읽기/쓰기 입력란과 동일하게 표시되므로 사용자에게 혼동을 줄 수 있습니다.
사용자는 텍스트를 선택하여 복사할 수 있습니다.
IsEnabled


### <a name="enable-multi-line-input"></a>여러 줄 입력 사용

입력란에서 여러 줄에 텍스트를 표시할지 여부를 제어하는 데 사용할 수 있는 두 개의 속성이 있습니다. 일반적으로 여러 줄 입력란을 만들려면 두 속성을 모두 설정합니다.
- 입력란에서 줄 바꿈 또는 리턴 문자를 허용하고 표시할 수 있게 하려면 [AcceptsReturn](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.acceptsreturn.aspx) 속성을 **true**로 설정합니다.
- 텍스트 줄 바꿈을 사용하도록 설정하려면 [TextWrapping](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textwrapping.aspx) 속성을 **Wrap**으로 설정합니다. 이렇게 하면 줄 구분 기호에 관계없이 입력란의 가장자리에 도달하면 텍스트가 줄 바꿈됩니다.

> **참고**&nbsp;&nbsp;TextBox와 RichEditBox는 해당 TextWrapping 속성에 대해 **WrapWholeWords** 값을 지원하지 않습니다. TextBox.TextWrapping 또는 RichEditBox.TextWrapping 값으로 WrapWholeWords를 사용하려고 하면 잘못된 인수 예외가 발생합니다.

여러 줄 입력란은 해당 [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 또는 [MaxHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) 속성이나 부모 컨테이너에 의해 제한되지 않을 경우 텍스트를 입력함에 따라 계속 세로로 증가합니다. 여러 줄 입력란이 표시 영역 이상 증가하지 않는지 테스트하고, 증가할 경우 증가를 제한해야 합니다. 항상 여러 줄 입력란에 대해 적절한 높이를 지정하여 사용자가 입력함에 따라 높이가 증가하지 않도록 하는 것이 좋습니다.

스크롤 휠 또는 터치를 사용한 스크롤은 필요에 따라 자동으로 사용됩니다. 그러나 세로 스크롤 막대는 기본적으로 표시되지 않습니다. 다음과 같이 포함된 ScrollViewer에서 [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx)를 **Auto**로 설정하면 세로 스크롤 막대를 표시할 수 있습니다.

```xaml
<TextBox AcceptsReturn="True" TextWrapping="Wrap"
         MaxHeight="172" Width="300" Header="Description"
         ScrollViewer.VerticalScrollBarVisibility="Auto"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.AcceptsReturn = true;
textBox.TextWrapping = TextWrapping.Wrap;
textBox.MaxHeight = 172;
textBox.Width = 300;
textBox.Header = "Description";
ScrollViewer.SetVerticalScrollBarVisibility(textBox, ScrollBarVisibility.Auto);
```

다음은 텍스트를 추가한 후의 입력란 모양입니다.

![여러 줄 입력란](images/text-box-multi-line.png)

### <a name="format-the-text-display"></a>텍스트 표시 서식 지정

입력란 내에서 텍스트를 맞추려면 [TextAlignment]() 속성을 사용합니다. 페이지 레이아웃 내에서 입력란을 맞추려면 [HorizontalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) 및 [VerticalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) 속성을 사용합니다.

입력란은 서식 없는 텍스트만 지원하지만 입력란에서 텍스트가 표시되는 방식을 브랜딩에 맞게 사용자 지정할 수 있습니다. [FontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx), [FontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontfamily.aspx), [FontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontsize.aspx), [Background](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontstyle.aspx), [Foreground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx) 및 [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx)과 같은 표준 [컨트롤](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.characterspacing.aspx) 속성을 설정하여 텍스트의 모양을 변경할 수 있습니다. 이러한 속성은 입력란이 로컬에서 텍스트를 표시하는 방식에만 영향을 줍니다. 예를 들어 서식 있는 텍스트 컨트롤에 텍스트를 복사하여 붙여넣을 경우 서식이 적용되지 않습니다.

이 예제에서는 텍스트 모양을 사용자 지정하기 위해 여러 속성이 설정된 읽기 전용 입력란을 보여 줍니다.

```xaml
<TextBox Text="Sample Text" IsReadOnly="True"
         FontFamily="Verdana" FontSize="24"
         FontWeight="Bold" FontStyle="Italic"
         CharacterSpacing="200" Width="300"
         Foreground="Blue" Background="Beige"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Text = "Sample Text";
textBox.IsReadOnly = true;
textBox.FontFamily = new FontFamily("Verdana");
textBox.FontSize = 24;
textBox.FontWeight = Windows.UI.Text.FontWeights.Bold;
textBox.FontStyle = Windows.UI.Text.FontStyle.Italic;
textBox.CharacterSpacing = 200;
textBox.Width = 300;
textBox.Background = new SolidColorBrush(Windows.UI.Colors.Beige);
textBox.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

이에 따라 표시되는 입력란은 다음과 같습니다.

![서식 있는 텍스트 상자](images/text-box-formatted.png)

### <a name="modify-the-context-menu"></a>상황에 맞는 메뉴 수정

기본적으로 입력란의 상황에 맞는 메뉴에 표시되는 명령은 입력란의 상태에 따라 달라집니다. 예를 들어 입력란을 편집할 수 있는 경우 다음 명령이 표시될 수 있습니다.

명령 | 표시되는 경우
------- | -------------
복사 | 텍스트를 선택한 경우
잘라내기 | 텍스트를 선택한 경우
붙여넣기 | 클립보드에 텍스트가 있는 경우
모두 선택 | TextBox에 텍스트가 있는 경우
실행 취소 | 텍스트가 변경된 경우

상황에 맞는 메뉴에 표시되는 명령을 수정하려면 [ContextMenuOpening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.contextmenuopening.aspx) 이벤트를 처리합니다. 이러한 경우의 예제는 [ContextMenu 샘플](http://go.microsoft.com/fwlink/p/?linkid=234891)의 시나리오 2를 참조하세요. 디자인 정보는 상황에 맞는 메뉴에 대한 지침을 참조하세요.

### <a name="select-copy-and-paste"></a>선택, 복사 및 붙여넣기

[SelectedText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectedtext.aspx) 속성을 사용하여 입력란에서 선택한 텍스트를 가져오거나 설정할 수 있습니다. 텍스트 선택을 조작하려면 [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) 및 [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) 속성과 [Select](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.select.aspx) 및 [SelectAll](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectall.aspx) 메서드를 사용합니다. 사용자가 텍스트를 선택하거나 선택을 취소하면 [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx) 이벤트를 처리하여 작업을 수행합니다. [SelectionHighlightColor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionhighlightcolor.aspx) 속성을 설정하면 선택한 텍스트를 강조 표시하는 데 사용되는 색을 변경할 수 있습니다.

TextBox는 기본적으로 복사 및 붙여넣기를 지원합니다. 앱의 편집 가능한 텍스트 컨트롤에서 [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) 이벤트를 사용자 지정 처리할 수 있습니다. 예를 들어 한 줄 검색 상자에 붙여넣을 때 여러 줄 주소에서 줄 바꿈을 제거할 수 있습니다. 또는 붙여넣은 텍스트의 길이를 확인하고 데이터베이스에 저장할 수 있는 최대 길이를 초과할 경우 사용자에게 경고 메시지를 표시할 수 있습니다. 자세한 내용과 예제는 [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) 이벤트를 참조하세요.

다음은 이러한 속성과 메서드의 사용 예입니다. 첫 번째 입력란에서 텍스트를 선택하면 선택한 텍스트가 읽기 전용인 두 번째 입력란에 표시됩니다. [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) 및 [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) 속성의 값이 두 개의 입력란에 표시됩니다. 이 작업은 [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx) 이벤트를 사용하여 수행됩니다.

```xaml
<StackPanel>
   <TextBox x:Name="textBox1" Height="75" Width="300" Margin="10"
         Text="The text that is selected in this TextBox will show up in the read only TextBox below."
         TextWrapping="Wrap" AcceptsReturn="True"
         SelectionChanged="TextBox1_SelectionChanged" />
   <TextBox x:Name="textBox2" Height="75" Width="300" Margin="5"
         TextWrapping="Wrap" AcceptsReturn="True" IsReadOnly="True"/>
   <TextBlock x:Name="label1" HorizontalAlignment="Center"/>
   <TextBlock x:Name="label2" HorizontalAlignment="Center"/>
</StackPanel>
```

```csharp
private void TextBox1_SelectionChanged(object sender, RoutedEventArgs e)
{
    textBox2.Text = textBox1.SelectedText;
    label1.Text = "Selection length is " + textBox1.SelectionLength.ToString();
    label2.Text = "Selection starts at " + textBox1.SelectionStart.ToString();
}
```

다음은 이 코드의 결과입니다.

![입력란에서 선택된 텍스트](images/text-box-selection.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>텍스트 컨트롤에 맞는 키보드를 선택합니다.

사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 터치 키보드나 SIP(Soft Input Panel)를 사용한 데이터 입력을 도울 수 있습니다.

터치 키보드는 앱이 터치 스크린이 있는 디바이스에서 실행될 때 텍스트 입력에 사용할 수 있습니다. 터치 키보드는 사용자가 TextBox 또는 RichEditBox 같이 편집 가능한 입력 필드를 탭할 때 호출됩니다. 사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 사용자가 앱에서 데이터를 쉽고 빠르게 입력할 수 있도록 지원할 수 있습니다. 입력 범위는 시스템에서 해당 입력 형식에 맞는 특수한 터치 키보드를 제공할 수 있도록 컨트롤에서 예상되는 텍스트 입력 형식에 대한 힌트를 시스템에 제공합니다.

예를 들어 텍스트 상자가 4자리 숫자의 PIN을 입력하는 목적으로만 사용될 경우 [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) 속성을 **Number**로 설정합니다. 이렇게 하면 사용자가 PIN을 쉽게 입력할 수 있도록 시스템에서 숫자 키패드 레이아웃이 표시됩니다.

> **중요**&nbsp;&nbsp;입력 범위는 입력 유효성 검사가 수행되지 않으며 사용자가 하드웨어 키보드 또는 다른 입력 디바이스를 사용해서 입력을 제공하지 못하도록 방지하지 않습니다. 따라서 필요에 따라 입력 코드에 대한 유효성을 검사해야 합니다.

터치 키보드에 영향을 주는 다른 속성은 [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isspellcheckenabled.aspx), [IsTextPredictionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.istextpredictionenabled.aspx) 및 [PreventKeyboardDisplayOnProgrammaticFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus.aspx)입니다. 하드웨어 키보드를 사용하는 경우 IsSpellCheckEnabled는 TextBox에도 영향을 줍니다.

자세한 내용과 예제는 [입력 범위를 사용하여 터치 키보드 변경](https://msdn.microsoft.com/library/windows/apps/mt280229) 및 속성 설명서를 참조하세요.

## <a name="recommendations"></a>권장 사항

-   텍스트 상자의 용도가 명확하지 않은 경우 레이블 또는 개체 틀 텍스트를 사용하세요. 레이블은 텍스트 입력란에 값이 있는지 여부에 상관없이 표시됩니다. 개체 틀 텍스트는 텍스트 입력란 내에 표시되었다가 값을 입력하면 사라집니다.
-   입력할 수 있는 값의 범위에 적절한 너비로 텍스트 상자를 설정하세요. 단어 길이는 언어에 따라 달라지므로 앱을 세계화하려는 경우 지역화를 고려해야 합니다.
-   텍스트 입력란은 일반적으로 한 줄(`TextWrap = "NoWrap"`)입니다. 긴 문자열을 입력하거나 편집해야 하는 경우 텍스트 입력란을 여러 줄(`TextWrap = "Wrap"`)로 설정합니다.
-   일반적으로 텍스트 입력란은 편집 가능한 텍스트에 사용됩니다. 내용을 읽고, 선택하고, 복사할 수 있지만, 편집할 수 없도록 텍스트 입력란을 읽기 전용으로 설정할 수 있습니다.
-   보기를 깔끔하게 정리해야 하는 경우 제어 확인란을 선택한 경우에만 텍스트 입력 상자를 표시하도록 설정하는 것이 좋습니다. 사용 상태인 텍스트 입력 상자를 컨트롤(예: 확인란)에 바인딩할 수도 있습니다.
-   값이 포함된 텍스트 입력란을 탭할 때 텍스트 입력란이 어떻게 동작하는지를 고려하세요. 기본 동작은 값을 바꾸지 않고 편집하는 데 적합합니다. 삽입 지점은 단어 사이에 있고 아무것도 선택되어 있지 않습니다. 주어진 텍스트 입력란의 가장 일반적인 사용 사례가 바꾸기인 경우 컨트롤이 포커스를 받을 때마다 필드에서 모든 텍스트를 선택할 수 있습니다. 입력이 선택을 대체합니다.

**한 줄 입력란**

-   한 줄 입력란을 여러 개 사용하여 다양한 텍스트 정보를 캡처합니다. 입력란이 본질적으로 관련된 경우 함께 그룹화합니다.

-   한 줄 입력란은 예상되는 제일 긴 입력보다 약간 넓게 만듭니다. 이 경우 컨트롤이 너무 넓어지면 두 개의 컨트롤로 분리합니다. 예를 들어 단일 주소 입력을 "주소 1"과 "주소 2"로 분할할 수 있습니다.
-   입력할 수 있는 문자의 최대 길이를 설정합니다. 지원 데이터 원본이 긴 입력 문자열을 허용하지 않는 경우 입력을 제한하고 유효성 검사 팝업을 사용하여 사용자에게 한계에 도달했음을 알립니다.
-   한 줄 텍스트 입력 컨트롤을 사용하여 사용자로부터 텍스트 조각들을 수집할 수 있습니다.

    다음 예는 보안 질문에 대한 답변을 캡처하기 위한 한 줄 입력란을 보여줍니다. 답변은 짧을 것이므로 여기서는 한 줄 입력란이 적합합니다.

    ![기본 데이터 입력](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)

-   짧고 크기가 정해져 있는 한 줄 텍스트 입력 컨트롤 집합을 사용하여 특수한 형식의 데이터를 입력할 수 있습니다.

    ![서식이 지정된 데이터 입력](images/textinput_example_productkey.png)

-   제약이 없는 한 줄 텍스트 입력 컨트롤과 사용자가 유효한 값을 선택하는 데 도움이 되는 명령 단추를 이용하여 문자열을 입력 또는 편집할 수 있습니다.

    ![보조 데이터 입력](images/textinput_example_assisted.png)


**여러 줄 텍스트 입력 컨트롤**

-   서식 있는 텍스트 상자를 만들 때 스타일 단추를 제공하고 해당 작업을 구현합니다.
-   앱의 스타일과 일치하는 글꼴을 사용합니다.
-   텍스트 컨트롤의 높이는 기본 입력을 수용하기에 충분하게 만듭니다.
-   최대 개수의 문자 또는 단어가 포함된 긴 텍스트를 캡처할 때는 일반 입력란을 사용하고 한계에 도달하려면 몇 글자 또는 몇 단어가 남았는지 보여 주는 카운터를 제공합니다. 카운터는 직접 만들어야 합니다. 카운터를 입력란 아래에 놓고 사용자가 문자 또는 단어를 입력할 때 동적으로 업데이트되도록 합니다.

    ![긴 텍스트 스팬](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

-   사용자가 입력하는 동안 텍스트 입력 컨트롤의 높이가 늘어나도록 만들지 마세요.
-   한 줄만 필요한 경우에는 여러 줄 입력란을 사용하지 마세요.
-   일반 텍스트 컨트롤으로도 충분한 경우 서식 있는 텍스트 컨트롤을 사용하지 마세요.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [텍스트 컨트롤](text-controls.md)
- [맞춤법 검사에 대한 지침](text-controls.md)
- [검색 추가](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [텍스트 입력에 대한 지침](text-controls.md)
- [TextBox 클래스](https://msdn.microsoft.com/library/windows/apps/br209683)
- [PasswordBox 클래스](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 속성](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
