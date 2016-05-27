---
author: Karl-Bridge-Microsoft
Description: Windows 잉크를 지원하는 UWP 앱은 ISF(Ink Serialized Format) 파일에 잉크 스트로크를 직렬화하고 역직렬화할 수 있습니다. ISF 파일은 모든 잉크 스트로크 속성과 동작에 대한 추가 메타 데이터가 포함된 GIF 이미지입니다. 잉크 불가능 앱에서는 알파 채널 배경 투명도를 포함하여 정적 GIF 이미지를 볼 수 있습니다.
title: Windows 잉크 스트로크 데이터 저장 및 검색
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, ISF, Ink Serialized Format
---

# Windows 잉크 스트로크 데이터 저장 및 검색


Windows 잉크를 지원하는 UWP 앱은 ISF(Ink Serialized Format) 파일에 잉크 스트로크를 직렬화하고 역직렬화할 수 있습니다. ISF 파일은 모든 잉크 스트로크 속성과 동작에 대한 추가 메타 데이터가 포함된 GIF 이미지입니다. 잉크 불가능 앱에서는 알파 채널 배경 투명도를 포함하여 정적 GIF 이미지를 볼 수 있습니다.


**중요 API**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)



**참고**  
ISF는 잉크 데이터를 가장 많이 압축한 영구적 표시입니다. GIF 파일과 같은 이진 문서 형식에 포함하거나 클립보드에 직접 배치할 수 있습니다.

 

## <span id="Save_ink_strokes_to_a_file"></span><span id="save_ink_strokes_to_a_file"></span><span id="SAVE_INK_STROKES_TO_A_FILE"></span>파일에 잉크 스트로크 저장


여기서는 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤에 그려진 잉크 스트로크를 저장하는 방법을 설명합니다.

1.  먼저 UI를 설정합니다.

    UI에는 "저장", "로드" 및 "지우기" 단추와 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)가 포함됩니다.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정합니다.

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 단추의 클릭 이벤트에 대한 수신기가 선언됩니다.
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  마지막으로, **저장** 단추의 클릭 이벤트 처리기에 잉크를 저장합니다.

    [
            **FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용하면 잉크 데이터가 저장되는 파일과 위치를 모두 사용자가 선택할 수 있습니다.

    파일을 선택한 후 [**ReadWrite**](https://msdn.microsoft.com/library/windows/apps/br241635)에 대한 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) 스트림 설정을 엽니다.

    그런 다음, [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/br242114)를 호출하여 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)로 관리되는 잉크 스트로크를 스트림으로 직렬화합니다.
```    CSharp
// Save ink data to a file.
    private async void btnSave_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Strokes present on ink canvas.
        if (currentStrokes.Count > 0)
        {
            // Let users choose their ink file using a file picker.
            // Initialize the picker.
            Windows.Storage.Pickers.FileSavePicker savePicker = 
                new Windows.Storage.Pickers.FileSavePicker();
            savePicker.SuggestedStartLocation = 
                Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
            savePicker.FileTypeChoices.Add(
                "GIF with embedded ISF", 
                new List<string>() { ".gif" });
            savePicker.DefaultFileExtension = ".gif";
            savePicker.SuggestedFileName = "InkSample";

            // Show the file picker.
            Windows.Storage.StorageFile file = 
                await savePicker.PickSaveFileAsync();
            // When chosen, picker returns a reference to the selected file.
            if (file != null)
            {
                // Prevent updates to the file until updates are 
                // finalized with call to CompleteUpdatesAsync.
                Windows.Storage.CachedFileManager.DeferUpdates(file);
                // Open a file stream for writing.
                IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
                // Write the ink strokes to the output stream.
                using (IOutputStream outputStream = stream.GetOutputStreamAt(0))
                {
                    await inkCanvas.InkPresenter.StrokeContainer.SaveAsync(outputStream);
                    await outputStream.FlushAsync();
                }
                stream.Dispose();

                // Finalize write so other apps can update file.
                Windows.Storage.Provider.FileUpdateStatus status =
                    await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);

                if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
                {
                    // File saved.
                }
                else
                {
                    // File couldn&#39;t be saved.
                }
            }
            // User selects Cancel and picker returns null.
            else
            {
                // Operation cancelled.
            }
        }
    }
```

[!NOTE]  
GIF는 잉크 데이터를 저장할 수 있는 유일한 형식입니다. 그러나 [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) 메서드(다음 섹션에서 설명)는 이전 버전과의 호환성을 위해 추가 형식을 지원합니다.

 

## <span id="Load_ink_strokes_from_a_file"></span><span id="load_ink_strokes_from_a_file"></span><span id="LOAD_INK_STROKES_FROM_A_FILE"></span>파일에서 잉크 스트로크 로드


다음은 파일에서 잉크 스트로크를 로드하고 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤에서 해당 잉크 스트로크를 렌더링하는 방법을 설명합니다.

1.  먼저 UI를 설정합니다.

    UI에는 "저장", "로드" 및 "지우기" 단추와 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)가 포함됩니다.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정합니다.

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 단추의 클릭 이벤트에 대한 수신기가 선언됩니다.
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  마지막으로, **로드** 단추의 클릭 이벤트 처리기에서 잉크를 로드합니다.

    [
            **FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)를 사용하면 저장된 잉크 데이터를 검색할 파일과 위치를 모두 사용자가 선택할 수 있습니다.

    파일을 선택한 후 [**Read**](https://msdn.microsoft.com/library/windows/apps/br241635)에 대한 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) 스트림 설정을 엽니다.

    그런 다음, [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607)를 호출하여 저장된 잉크 스트로크를 읽고, 역직렬화하고, [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)에 로드합니다. 스트로크를 **InkStrokeContainer**에 로드하면 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)가 해당 스트로크를 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)로 즉시 렌더링합니다.
``` csharp
// Load ink data from a file.
private async void btnLoad_Click(object sender, RoutedEventArgs e)
{
    // Let users choose their ink file using a file picker.
    // Initialize the picker.
    Windows.Storage.Pickers.FileOpenPicker openPicker =
        new Windows.Storage.Pickers.FileOpenPicker();
    openPicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    openPicker.FileTypeFilter.Add(".gif");
    // Show the file picker.
    Windows.Storage.StorageFile file = await openPicker.PickSingleFileAsync();
    // User selects a file and picker returns a reference to the selected file.
    if (file != null)
    {
        // Open a file stream for reading.
        IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        // Read from file.
        using (var inputStream = stream.GetInputStreamAt(0))
        {
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(stream);
        }
        stream.Dispose();
    }
    // User selects Cancel and picker returns null.
    else
    {
        // Operation cancelled.
    }
}
```

**참고**  
GIF는 잉크 데이터를 저장할 수 있는 유일한 형식입니다. 그러나 [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) 메서드는 이전 버전과의 호환성을 위해 다음 형식을 지원합니다.

| 형식                    | 설명 |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | ISF를 사용하여 유지되는 잉크를 지정합니다. 이 형식은 잉크에 대한 최대로 압축된 영구적 표현입니다. 이진 문서 형식에 포함되거나 클립보드에 직접 배치될 수 있습니다.                                                                                                                                                                                                         |
| Base64InkSerializedFormat | ISF를 base64 스트림으로 인코딩하여 유지되는 잉크를 지정합니다. 이 형식은 XML 또는 HTML 파일에서 직접 잉크를 인코딩할 수 있도록 제공됩니다.                                                                                                                                                                                                                                                |
| Gif                       | ISF를 파일에 포함된 메타데이터로 포함하는 GIF 파일을 사용하여 유지되는 잉크를 지정합니다. 이 형식을 사용하면 잉크 불가능 응용 프로그램에서 잉크를 볼 수 있고 잉크 가능 응용 프로그램으로 돌아갈 때 최대 잉크 화질을 유지할 수 있습니다. 이 형식은 HTML 파일 내에서 잉크 콘텐츠를 전송하고 잉크 가능 및 잉크 불가능 응용 프로그램에서 사용할 수 있도록 하는 경우에 적합합니다. |
| Base64Gif                 | base64 인코딩된 강화된 GIF를 사용하여 유지되는 잉크를 지정합니다. 이 형식은 XML 또는 HTML 파일에서 직접 잉크를 인코딩하고 나중에 이미지로 변환하는 경우에 제공됩니다. 이 형식은 모든 잉크 정보를 포함하기 위해 생성되며 XSLT(Extensible Stylesheet Language Transformations)를 통해 HTML을 생성하는 데 사용되는 XML 형식에 사용할 수 있습니다. 

## <span id="Copy_and_paste_ink_strokes_with_the_clipboard"></span><span id="copy_and_paste_ink_strokes_with_the_clipboard"></span><span id="COPY_AND_PASTE_INK_STROKES_WITH_THE_CLIPBOARD"></span>클립보드를 사용한 잉크 스트로크 복사 및 붙여넣기


다음은 클립보드를 사용하여 앱 간에 잉크 스트로크를 전송하는 방법을 설명합니다.

기본 제공 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 잘라내기 및 복사 명령에서 클립보드 기능을 지원하려면 먼저 잉크 스트로크를 하나 이상 선택해야 합니다.

이 예제의 경우 펜 단추(또는 마우스 오른쪽 단추)를 사용하여 입력을 수정할 때 스트로크 선택을 사용합니다. 스트로크 선택을 구현하는 방법의 전체 예제는 [펜 및 스타일러스 조작](pen-and-stylus-interactions.md)에서 [고급 처리에 대한 통과 입력](pen-and-stylus-interactions.md#passthrough)을 참조하세요.

1.  먼저 UI를 설정합니다.

    UI에는 "잘라내기", "복사", "붙여넣기" 및 "지우기" 단추와 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 및 선택 캔버스가 포함됩니다.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="tbHeader" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnCut" 
                    Content="Cut" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnCopy" 
                    Content="Copy" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnPaste" 
                    Content="Paste" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="20,0,10,0"/>
        </StackPanel>
        <Grid x:Name="gridCanvas" Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정합니다.

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 선택 기능에 대한 포인터 및 스트로크 이벤트뿐만 아니라 단추의 클릭 이벤트에 대한 수신기도 여기에서 선언합니다.

    스트로크 선택을 구현하는 방법의 전체 예제는 [펜 및 스타일러스 조작](pen-and-stylus-interactions.md)에서 [고급 처리에 대한 통과 입력](pen-and-stylus-interactions.md#passthrough)을 참조하세요.
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to cut ink strokes.
        btnCut.Click += btnCut_Click;
        // Listen for button click to copy ink strokes.
        btnCopy.Click += btnCopy_Click;
        // Listen for button click to paste ink strokes.
        btnPaste.Click += btnPaste_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;

        // By default, the InkPresenter processes input modified by 
        // a secondary affordance (pen barrel button, right mouse 
        // button, or similar) as ink.
        // To pass through modified input to the app for custom processing 
        // on the app UI thread instead of the background ink thread, set 
        // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
        inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
            InkInputRightDragAction.LeaveUnprocessed;

        // Listen for unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
            UnprocessedInput_PointerPressed;
        inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
            UnprocessedInput_PointerMoved;
        inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
            UnprocessedInput_PointerReleased;

        // Listen for new ink or erase strokes to clean up selection UI.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            StrokeInput_StrokeStarted;
        inkCanvas.InkPresenter.StrokesErased +=
            InkPresenter_StrokesErased;
    }
```

3.  마지막으로, 스트로크 선택 지원을 추가한 후 **잘라내기**, **복사** 및 **붙여넣기** 단추의 클릭 이벤트 처리기에서 클립보드 기능을 구현합니다.

    잘라내기의 경우 먼저, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)의 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)에서 [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232)를 호출합니다.

    그런 다음, [**DeleteSelected**](https://msdn.microsoft.com/library/windows/apps/br244233)를 호출하여 잉크 캔버스에서 스트로크를 제거합니다.

    마지막으로, 선택 캔버스에서 선택 스트로크를 모두 삭제합니다.
```    CSharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
```    CSharp
// Clean up selection UI.
    private void ClearSelection()
    {
        var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        foreach (var stroke in strokes)
        {
            stroke.Selected = false;
        }
        ClearDrawnBoundingRect();
    }

    private void ClearDrawnBoundingRect()
    {
        if (selectionCanvas.Children.Any())
        {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
        }
    }
```

    For copy, we simply call [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232) on the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) of the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).
```    CSharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

    For paste, we call [**CanPasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208495) to ensure that the content on the clipboard can be pasted to the ink canvas.

    If so, we call [**PasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208503) to insert the clipboard ink strokes into the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) of the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011), which then renders the strokes to the ink canvas.
```    CSharp
private void btnPaste_Click(object sender, RoutedEventArgs e)
    {
        if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
        {
            inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                new Point(0, 0));
        }
        else
        {
            // Cannot paste from clipboard.
        }
    }
```

## <span id="related_topics"></span>관련 문서

* [펜 및 스타일러스 조작](pen-and-stylus-interactions.md)

**샘플**
* [잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [간단한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [복잡한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 






<!--HONumber=May16_HO2-->


