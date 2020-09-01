---
Description: Windows 잉크를 지 원하는 windows 앱은 잉크 스트로크를 ISF (Ink Serialize 된 형식) 파일로 serialize 및 deserialize 할 수 있습니다. ISF 파일은 모든 잉크 스트로크 속성 및 동작에 대 한 추가 메타 데이터가 포함 된 GIF 이미지입니다. 잉크를 사용할 수 없는 앱은 알파 채널 배경 투명도를 포함 하 여 정적 GIF 이미지를 볼 수 있습니다.
title: Windows Ink 스트로크 데이터 저장 및 검색
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keywords: Windows Ink, Windows Ink, DirectInk, InkPresenter, InkCanvas, ISF, 잉크 직렬화 된 형식, 사용자 조작, 입력
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 72076ffb27046a5c5e804cf30e8cb6c78b88cd69
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173337"
---
# <a name="store-and-retrieve-windows-ink-stroke-data"></a>Windows Ink 스트로크 데이터 저장 및 검색


Windows 잉크를 지 원하는 windows 앱은 잉크 스트로크를 ISF (Ink Serialize 된 형식) 파일로 serialize 및 deserialize 할 수 있습니다. ISF 파일은 모든 잉크 스트로크 속성 및 동작에 대 한 추가 메타 데이터가 포함 된 GIF 이미지입니다. 잉크를 사용할 수 없는 앱은 알파 채널 배경 투명도를 포함 하 여 정적 GIF 이미지를 볼 수 있습니다.

> [!NOTE]
> ISF는 잉크의 가장 간결한 영구 표현입니다. GIF 파일 등의 이진 문서 형식에 포함 하거나 클립보드에 직접 배치할 수 있습니다.

> **중요 한 api**: InkCanvas [**,**](/uwp/api/Windows.UI.Input.Inking) [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)

## <a name="save-ink-strokes-to-a-file"></a>파일에 잉크 스트로크 저장

여기에서는 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤에 그려진 잉크 스트로크를 저장 하는 방법을 보여 줍니다.

**[ISF (Ink serialize 된 형식) 파일에서 잉크 스트로크 저장 및 로드](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip) 에서이 샘플을 다운로드 합니다.**

1.  먼저 UI를 설정 합니다.

    UI에는 "저장", "로드" 및 "지우기" 단추와 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)포함 되어 있습니다.
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

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정 합니다.

    [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크 ([**inputdevicetypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))로 해석 하 고 단추에서 클릭 이벤트에 대 한 수신기가 선언 되도록 구성 되어 있습니다.
```csharp
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

3.  마지막으로, **저장** 단추의 click 이벤트 처리기에 잉크를 저장 합니다.

    [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 를 사용 하면 사용자가 파일 및 잉크 데이터가 저장 되는 위치를 모두 선택할 수 있습니다.

    파일이 선택 되 면 [**ReadWrite**](/uwp/api/Windows.Storage.FileAccessMode)로 설정 된 [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 스트림이 열립니다.

    그런 다음 [**SaveAsync**](/uwp/api/windows.ui.input.inking.iinkstrokecontainer.saveasync) 를 호출 하 여 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 에서 관리 하는 잉크 스트로크를 스트림으로 직렬화 합니다.

```csharp
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
                    // File couldn't be saved.
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

> [!NOTE]
> GIF는 잉크 데이터를 저장 하는 데 지원 되는 유일한 파일 형식입니다. 그러나 [**LoadAsync**](/uwp/api/windows.ui.input.inking.inkmanager.loadasync) 메서드 (다음 섹션에서 설명)는 이전 버전과의 호환성을 위해 추가 형식을 지원 합니다.

## <a name="load-ink-strokes-from-a-file"></a>파일에서 잉크 스트로크 로드

여기서는 파일에서 잉크 스트로크를 로드 하 고 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤에서 렌더링 하는 방법을 보여 줍니다.

**[ISF (Ink serialize 된 형식) 파일에서 잉크 스트로크 저장 및 로드](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip) 에서이 샘플을 다운로드 합니다.**

1.  먼저 UI를 설정 합니다.

    UI에는 "저장", "로드" 및 "지우기" 단추와 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)포함 되어 있습니다.
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

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정 합니다.

    [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크 ([**inputdevicetypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))로 해석 하 고 단추에서 클릭 이벤트에 대 한 수신기가 선언 되도록 구성 되어 있습니다.
```csharp
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

3.  마지막으로, **로드** 단추의 click 이벤트 처리기에서 잉크를 로드 합니다.

    [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 를 통해 사용자는 저장 된 잉크 데이터를 검색할 위치에서 파일 및 위치를 선택할 수 있습니다.

    파일이 선택 되 면 [**읽기**](/uwp/api/Windows.Storage.FileAccessMode)로 설정 된 [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 스트림이 열립니다.

    그런 다음 [**LoadAsync**](/uwp/api/windows.ui.input.inking.inkmanager.loadasync) 를 호출 하 여 저장 된 잉크 스트로크를 읽고 역직렬화 하 고 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer)로 로드 합니다. 스트로크를 **InkStrokeContainer** 에 로드 하면 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 가 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)에 즉시 렌더링 합니다.

    > [!NOTE]
    > 새 스트로크를 로드 하기 전에 InkStrokeContainer의 모든 기존 스트로크가 지워집니다.

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
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(inputStream);
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

> [!NOTE]
> GIF는 잉크 데이터를 저장 하는 데 지원 되는 유일한 파일 형식입니다. 그러나 [**LoadAsync**](/uwp/api/windows.ui.input.inking.inkmanager.loadasync) 메서드는 이전 버전과의 호환성을 위해 다음 형식을 지원 합니다.

| 서식                    | Description |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | ISF를 사용 하 여 유지 되는 잉크를 지정 합니다. 이는 잉크의 가장 간결한 영구 표현입니다. 이진 문서 형식에 포함 하거나 클립보드에 직접 배치할 수 있습니다.                                                                                                                                                                                                         |
| Base64InkSerializedFormat | ISF를 base64 스트림으로 인코딩하여 유지 되는 잉크를 지정 합니다. 이 형식은 잉크를 XML 또는 HTML 파일에서 직접 인코딩할 수 있도록 제공 됩니다.                                                                                                                                                                                                                                                |
| Gif                       | ISF를 파일 내에 포함 된 메타 데이터로 포함 하는 GIF 파일을 사용 하 여 유지 되는 잉크를 지정 합니다. 이렇게 하면 잉크를 사용할 수 없는 응용 프로그램에서 잉크를 볼 수 있으며, 잉크 사용 응용 프로그램으로 반환 될 때 전체 잉크 충실도를 유지할 수 있습니다. 이 형식은 HTML 파일 내에서 잉크 콘텐츠를 전송 하 고 잉크 및 비 잉크 응용 프로그램에서 사용할 수 있도록 하는 경우에 적합 합니다. |
| Base64Gif                 | Base64 인코딩 강화 GIF를 사용 하 여 유지 되는 잉크를 지정 합니다. 이 형식은 나중에 이미지로 변환 하기 위해 잉크를 XML 또는 HTML 파일에서 직접 인코딩할 때 제공 됩니다. 이는 모든 잉크 정보를 포함 하 고 XSLT (Extensible Stylesheet Language 변환)를 통해 HTML을 생성 하는 데 사용 되는 XML 형식으로 사용 될 수 있습니다. 

## <a name="copy-and-paste-ink-strokes-with-the-clipboard"></a>클립보드를 사용 하 여 잉크 스트로크 복사 및 붙여넣기

여기서는 클립보드를 사용 하 여 앱 간에 잉크 스트로크를 전송 하는 방법을 보여 줍니다.

클립보드 기능을 지원 하기 위해 기본 제공 되는 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 잘라내기 및 복사 명령에 하나 이상의 잉크 스트로크를 선택 해야 합니다.

이 예에서는 펜 배럴 단추 (또는 마우스 오른쪽 단추)를 사용 하 여 입력을 수정할 때 스트로크 선택을 사용 하도록 설정 합니다. 스트로크 선택을 구현 하는 방법에 대 한 전체 예제는 [펜 및 스타일러스 상호 작용](pen-and-stylus-interactions.md)에서 고급 처리를 위한 통과 입력을 참조 하세요.

**[클립보드에서 잉크 스트로크 저장 및 로드](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip) 에서이 샘플 다운로드**

1.  먼저 UI를 설정 합니다.

    UI는 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 및 선택 canvas와 함께 "잘라내기", "복사", "붙여넣기" 및 "지우기" 단추를 포함 합니다.
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

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정 합니다.

    [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크 ([**inputdevicetypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))로 해석 하도록 구성 됩니다. 선택 기능에 대 한 포인터 및 스트로크 이벤트 뿐만 아니라 단추에서 클릭 이벤트에 대 한 수신기도 여기에서 선언 됩니다.

    스트로크 선택을 구현 하는 방법에 대 한 전체 예제는 [펜 및 스타일러스 상호 작용](pen-and-stylus-interactions.md)에서 고급 처리를 위한 통과 입력을 참조 하세요.
```csharp
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

3.  마지막으로, 스트로크 선택 지원을 추가한 후 **잘라내기**, **복사**및 **붙여넣기** 단추의 click 이벤트 처리기에서 클립보드 기능을 구현 합니다.

    잘라내기의 경우 먼저 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)의 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 에서 [**copyselectedtoclipboard**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) 를 호출 합니다.

    그런 다음 [**선택**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.deleteselected) 된 삭제를 호출 하 여 잉크 캔버스에서 스트로크를 제거 합니다.

    마지막으로 선택 캔버스에서 모든 선택 스트로크를 삭제 합니다.
    
```csharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
```csharp
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

복사를 위해 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 의 경우에만 [**Copyselectedtoclipboard**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) [**를 호출 합니다.**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)


```csharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

붙여넣기의 경우 [**CanPasteFromClipboard**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.canpastefromclipboard) 를 호출 하 여 클립보드의 내용을 잉크 캔버스에 붙여 넣을 수 있는지 확인 합니다.

이 경우 [**PasteFromClipboard**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.pastefromclipboard) 를 호출 하 여 클립보드 잉크 스트로크를 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)의 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 에 삽입 하면 스트로크가 잉크 캔버스로 렌더링 됩니다.

```csharp
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

## <a name="related-articles"></a>관련된 문서

* [펜 및 스타일러스 상호 작용](pen-and-stylus-interactions.md)

**토픽 샘플**
* [ISF (Ink Serialize 된 형식) 파일에서 잉크 스트로크를 저장 하 고 로드 합니다.](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [클립보드에서 잉크 스트로크 저장 및 로드](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)

**기타 샘플**
* [단순 잉크 샘플 (c #/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [복합 잉크 샘플 (c + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ink 샘플 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [자습서 시작: Windows 앱에서 잉크 지원](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [서적 샘플 강조](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [제품군 정보 샘플](https://github.com/Microsoft/Windows-appsample-familynotes)