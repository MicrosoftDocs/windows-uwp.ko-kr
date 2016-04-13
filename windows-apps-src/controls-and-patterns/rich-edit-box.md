---
Description: RichEditBox 컨트롤을 사용하면 서식 있는 텍스트, 하이퍼링크 및 이미지를 포함하는 서식 있는 텍스트 문서를 입력하고 편집할 수 있습니다. IsReadOnly 속성을 true로 설정하여 RichEditBox를 읽기 전용으로 만들 수 있습니다.
title: RichEditBox
ms.assetid: 4AFC0DFA-3B89-434D-9F86-4309CCFF7839
label: 서식 있는 편집 상자
template: detail.hbs
---
# 서식 있는 편집 상자
RichEditBox 컨트롤을 사용하면 서식 있는 텍스트, 하이퍼링크 및 이미지를 포함하는 서식 있는 텍스트 문서를 입력하고 편집할 수 있습니다. IsReadOnly 속성을 **true**로 설정하여 RichEditBox를 읽기 전용으로 만들 수 있습니다.

<span class="sidebar_heading" style="font-weight: bold;">중요 API</span>

-   [**RichEditBox 클래스**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)
-   [**Document 속성**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx)
-   [**IsReadOnly 속성**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isreadonly.aspx)
-   [**IsSpellCheckEnabled 속성**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx)

## 올바른 컨트롤인가요?

텍스트 파일을 표시하고 편집하려면 **RichEditBox**를 사용합니다. 다른 표준 텍스트 입력란을 사용하는 방식으로 사용자 입력을 앱으로 가져오기 위해 RichEditBox를 사용하지 마세요. 대신, 앱과 분리된 텍스트 파일 작업에 사용합니다. 일반적으로 RichEditBox에 입력된 텍스트는 .rtf 파일에 저장합니다.
-   여러 줄 입력란의 주된 목적이 문서(예, 블로그 항목 또는 메일 메시지의 본문)를 만드는 것이며 그 문서에 서식을 지정해야 하는 경우에는 서식 있는 입력란을 사용합니다.
-   사용자가 텍스트에 서식을 지정할 수 있도록 하려면 서식 있는 입력란을 사용합니다.
-   데이터 처리에 사용 후 사용자에게 다시 표시하지는 않을 텍스트를 캡처할 때는 일반 텍스트 입력 컨트롤을 사용합니다.
-   기타 모든 시나리오에서는 일반 텍스트 입력 컨트롤을 사용합니다.

올바른 텍스트 컨트롤을 선택하는 방법에 대한 자세한 내용은 [텍스트 컨트롤](text-controls.md) 문서를 참조하세요.

## 예제

이 서식 있는 편집 상자에는 서식 있는 텍스트 문서가 열려 있습니다. 서식 및 파일 단추는 서식 있는 편집 상자에 없으므로 최소한의 스타일 지정 단추 집합을 제공하고 해당 작업을 구현해야 합니다.

![문서가 열려 있는 서식 있는 입력란](images/rich-edit-box.png)

## 서식 있는 편집 상자 만들기

기본적으로 RichEditBox는 맞춤법 검사를 지원합니다. 맞춤법 검사기를 사용하지 않도록 설정하려면 [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx) 속성을 **false**로 설정합니다. 자세한 내용은 맞춤법 검사에 대한 지침 및 검사 목록을 참조하세요.

RichEditBox의 [Document](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx) 속성을 사용하여 콘텐츠를 가져옵니다. [Windows.UI.Xaml.Documents.Block](https://msdn.microsoft.com/library/windows/apps/xaml/bb774052.aspx) 개체를 해당 콘텐츠로 사용하는 RichTextBlock 컨트롤과 달리 RichEditBox의 콘텐츠는 [Windows.UI.Text.ITextDocument](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.block.aspx) 개체입니다. ITextDocument 인터페이스는 문서를 로드하여 스트림에 저장, 텍스트 범위 검색, 활성 선택 영역 가져오기, 변경 내용 실행 취소 및 다시 실행, 기본 서식 특성 설정 등을 수행하는 방법을 제공합니다.

다음 예제에서는 RichEditBox에서 서식 있는 텍스트 형식(.rtf) 파일을 편집, 로드 및 저장하는 방법을 보여 줍니다.

```xaml
<RelativePanel Margin="20" HorizontalAlignment="Stretch">
    <RelativePanel.Resources>
        <Style TargetType="AppBarButton">
            <Setter Property="IsCompact" Value="True"/>
        </Style>
    </RelativePanel.Resources>
    <AppBarButton x:Name="openFileButton" Icon="OpenFile" 
                  Click="OpenButton_Click" ToolTipService.ToolTip="Open file"/>
    <AppBarButton Icon="Save" Click="SaveButton_Click" 
                  ToolTipService.ToolTip="Save file" 
                  RelativePanel.RightOf="openFileButton" Margin="8,0,0,0"/>

    <AppBarButton Icon="Bold" Click="BoldButton_Click" ToolTipService.ToolTip="Bold" 
                  RelativePanel.LeftOf="italicButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="italicButton" Icon="Italic" Click="ItalicButton_Click" 
                  ToolTipService.ToolTip="Italic" RelativePanel.LeftOf="underlineButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="underlineButton" Icon="Underline" Click="UnderlineButton_Click" 
                  ToolTipService.ToolTip="Underline" RelativePanel.AlignRightWithPanel="True"/>

    <RichEditBox x:Name="editor" Height="200" RelativePanel.Below="openFileButton" 
                 RelativePanel.AlignLeftWithPanel="True" RelativePanel.AlignRightWithPanel="True"/>
</RelativePanel>
```


```csharp
private async void OpenButton_Click(object sender, RoutedEventArgs e)
{
    // Open a text file.
    Windows.Storage.Pickers.FileOpenPicker open =
        new Windows.Storage.Pickers.FileOpenPicker();
    open.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    open.FileTypeFilter.Add(".rtf");

    Windows.Storage.StorageFile file = await open.PickSingleFileAsync();

    if (file != null)
    {
        try
        {
            Windows.Storage.Streams.IRandomAccessStream randAccStream =
        await file.OpenAsync(Windows.Storage.FileAccessMode.Read);

            // Load the file into the Document property of the RichEditBox.
            editor.Document.LoadFromStream(Windows.UI.Text.TextSetOptions.FormatRtf, randAccStream);
        }
        catch (Exception)
        {
            ContentDialog errorDialog = new ContentDialog()
            {
                Title = "File open error",
                Content = "Sorry, I couldn't open the file.",
                PrimaryButtonText = "Ok"
            };

            await errorDialog.ShowAsync();
        }
    }
}

private async void SaveButton_Click(object sender, RoutedEventArgs e)
{
    Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;

    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Rich Text", new List<string>() { ".rtf" });

    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";

    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
    if (file != null)
    {
        // Prevent updates to the remote version of the file until we 
        // finish making changes and call CompleteUpdatesAsync.
        Windows.Storage.CachedFileManager.DeferUpdates(file);
        // write to file
        Windows.Storage.Streams.IRandomAccessStream randAccStream =
            await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

        editor.Document.SaveToStream(Windows.UI.Text.TextGetOptions.FormatRtf, randAccStream);

        // Let Windows know that we're finished changing the file so the 
        // other app can update the remote version of the file.
        Windows.Storage.Provider.FileUpdateStatus status = await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
        if (status != Windows.Storage.Provider.FileUpdateStatus.Complete)
        {
            Windows.UI.Popups.MessageDialog errorBox =
                new Windows.UI.Popups.MessageDialog("File " + file.Name + " couldn't be saved.");
            await errorBox.ShowAsync();
        }
    }
}

private void BoldButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Bold = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void ItalicButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Italic = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void UnderlineButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        if (charFormatting.Underline == Windows.UI.Text.UnderlineType.None)
        {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.Single;
        }
        else {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.None;
        }
        selectedText.CharacterFormat = charFormatting;
    }
}
```

## 텍스트 컨트롤에 맞는 키보드를 선택합니다.

사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 터치 키보드나 SIP(Soft Input Panel)를 사용한 데이터 입력을 도울 수 있습니다. 일반적으로 기본 자판 배열은 서식 있는 텍스트 문서 작업에 적합합니다.

입력 범위를 사용하는 방법에 대한 자세한 내용은 [입력 범위를 사용하여 터치 키보드를 변경]()을 참조하세요.

## 권장 사항

-   서식 있는 입력란을 만들 때 스타일 단추를 제공하고 해당 작업을 구현합니다.
-   앱의 스타일과 일치하는 글꼴을 사용합니다.
-   텍스트 컨트롤의 높이는 기본 입력을 수용하기에 충분하게 만듭니다.
-   사용자가 입력하는 동안 텍스트 입력 컨트롤의 높이가 늘어나도록 만들지 마세요.
-   한 줄만 필요한 경우에는 여러 줄 입력란을 사용하지 마세요.
-   일반 텍스트 컨트롤이 적절한 경우 서식 있는 텍스트 컨트롤을 사용하지 마세요.



\[이 문서에는 UWP(유니버설 Windows 플랫폼) 앱 및 Windows 10과 관련된 정보가 있습니다. Windows 8.1 참고 자료는 [Windows 8.1 지침 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)를 다운로드하세요.\]

## 관련 문서

[텍스트 컨트롤](text-controls.md)

**디자이너용**
- [맞춤법 검사에 대한 지침](spell-checking-and-prediction.md)
- [검색 추가](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [텍스트 입력에 대한 지침](text-controls.md)

**개발자용(XAML)**
- [**TextBox 클래스**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox 클래스**](https://msdn.microsoft.com/library/windows/apps/br227519)


**개발자용(기타)**
- [String.Length 속성](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)


<!--HONumber=Mar16_HO1-->


