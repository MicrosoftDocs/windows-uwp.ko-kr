---
author: Karl-Bridge-Microsoft
Description: Use handwriting recognition and ink analysis to recognize Windows Ink strokes as text and shapes.
title: Windows Ink 스트로크를 텍스트 및 셰이프로 인식
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, Windows 수동 입력, DirectInk, InkPresenter, InkCanvas, 필기 인식, 사용자 조작, 입력
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3f33ec4df6661861bc9ee956393cf25cad1c2cb7
ms.sourcegitcommit: 346b5c9298a6e9e78acf05944bfe13624ea7062e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/05/2018
ms.locfileid: "1707384"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Windows Ink 스트로크를 텍스트 및 셰이프로 인식


Windows Ink에 내장된 인식 기능을 사용하여 잉크 스트로크 및 셰이프를 변환합니다.

> **중요 API**: [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="free-form-recognition-with-ink-analysis"></a>잉크 분석을 통한 자유 형식 인식

여기서는 Windows Ink 분석 엔진([Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis))을 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)에서 일련의 자유 형식 스트로크를 텍스트 또는 셰이프로 분류, 분석 및 인식하는 방법을 보여 줍니다. (텍스트 및 셰이프 인식 외에도 잉크 분석을 사용하여 문서 구조, 글머리 기호 목록, 일반 드로잉을 인식할 수 있습니다.)

> [!NOTE]
> 양식 입력 같은 기본적인 한 줄 일반 텍스트 시나리오에 대한 내용은 이 항목 후반부의 [제한된 필기 인식](#constrained-handwriting-recognition)을 참조하세요.

이 예에서는 사용자가 그리기를 마쳤음을 알리는 단추를 클릭하면 인식이 시작됩니다.

**[잉크 분석 샘플(기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)에서 이 샘플 다운로드**

1.  먼저 UI(MainPage.xaml)를 설정합니다. 

    UI에는 "인식" 단추, [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 및 표준 [**캔버스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)가 포함되어 있습니다. "인식" 단추를 누르면 잉크 캔버스의 모든 잉크 스트로크를 분석하고, (분석된 경우) 해당 셰이프 및 텍스트를 표준 캔버스에 그립니다. 그런 다음 원래 잉크 스트로크는 잉크 캔버스에서 삭제됩니다.
```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
    </Grid>
```
2. UI 코드 숨김 파일(MainPage.xaml.cs)에서, 잉크 및 잉크 분석 기능에 필요한 네임스페이스 유형 참조를 추가합니다.
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. 그런 다음 전역 변수를 지정합니다.
```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
```
4.  다음으로 몇 가지 기본 잉크 입력 동작을 설정합니다.
    - [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜, 마우스 및 터치의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 
    - 잉크 스트로크는 지정된 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/apps/dn858535)를 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/desktop/ms695050)에 렌더링됩니다. 
    - "인식" 단추의 Click 이벤트에 대한 수신기도 선언합니다.
```csharp
/// <summary>
/// Initialize the UI page.
/// </summary>
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
```
5.  이 예에서는 "인식" 단추의 Click 이벤트 처리기에서 잉크 분석을 수행합니다.
    - 먼저 [**InkCanvas.InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter)의 [**StrokeContainer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer)에서 [**GetStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes)를 호출하여 모든 현재 잉크 스트로크 컬렉션을 가져옵니다.
    - 잉크 스트로크가 있는 경우 InkAnalyzer의 [**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) 호출에 전달합니다.
    - 여기서는 드로잉과 텍스트를 모두 인식하려고 시도할 예정이지만, [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) 메서드를 사용하여 텍스트(문서 구조 및 글머리 기호 목록 포함)만 또는 드로잉(셰이프 인식 포함)만 인식하도록 지정할 수 있습니다.
    - [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync)를 호출하여 잉크 분석을 시작하고 [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)를 가져옵니다.
    - [**상태**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status)가 **업데이트됨**을 반환하면 [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) 및 [**InkAnalysisNodeKind.InkDrawing**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) 모두에 대해 [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes)를 호출합니다.
    - 두 노드 유형 집합을 반복하고 인식 캔버스(잉크 캔버스 아래에 있음)에서 각각의 텍스트 또는 셰이프를 그립니다.
    - 마지막으로 InkAnalyzer에서 인식된 노드를 삭제하고 잉크 캔버스에서 해당 잉크 스트로크를 삭제합니다.
```csharp
/// <summary>
/// The "Analyze" button click handler.
/// Ink recognition is performed here.
/// </summary>
/// <param name="sender">Source of the click event</param>
/// <param name="e">Event args for the button click routed event</param>
private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
```
6. 다음은 인식 캔버스에 TextBlock을 그리는 함수입니다. 연결된 잉크 스트로크의 경계 사각형을 잉크 캔버스에 사용하여 TextBlock의 위치 및 글꼴 크기를 설정합니다.
```csharp
/// <summary>
/// Draw ink recognition text string on the recognitionCanvas.
/// </summary>
/// <param name="recognizedText">The string returned by text recognition.</param>
/// <param name="boundingRect">The bounding rect of the original ink writing.</param>
private void DrawText(string recognizedText, Rect boundingRect)
{
    TextBlock text = new TextBlock();
    Canvas.SetTop(text, boundingRect.Top);
    Canvas.SetLeft(text, boundingRect.Left);

    text.Text = recognizedText;
    text.FontSize = boundingRect.Height;

    recognitionCanvas.Children.Add(text);
}
```
7. 다음은 인식 캔버스에 줄임표와 다각형을 그리는 함수입니다. 연결된 잉크 스트로크의 경계 사각형을 잉크 캔버스에 사용하여 셰이프의 위치 및 글꼴 크기를 설정합니다.
```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
```

다음은 실제 작동하는 샘플입니다.

| 분석 전 | 분석 후 |
| --- | --- |
| ![분석 전](images/ink/ink-analysis-raw2-small.png) | ![분석 후](images/ink/ink-analysis-analyzed2-small.png) |

---

## <a name="constrained-handwriting-recognition"></a>제한된 필기 인식

이전 섹션 ([잉크 분석을 통한 자유 형식 인식](#free-form-recognition-with-ink-analysis))에서는 [잉크 분석 API](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis)를 사용하여 InkCanvas 영역 내에서 임의의 잉크 스트로크를 인식하는 방법을 살펴보았습니다.

이번 섹션에서는 Windows Ink 필기 인식 엔진을 사용하여(잉크 분석 아님) [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)에 필기된 일련의 스트로크를 텍스트로 변환(설치된 기본 언어 팩을 기반으로)하는 방법을 보여 줍니다.

> [!NOTE]
> 이 섹션에서 보여 주는 기본 필기 인식은 양식 입력처럼 한 줄로 된 텍스트 입력 시나리오에 가장 적합합니다. 텍스트 인식뿐 아니라 문서 구조, 목록 항목, 셰이프 및 드로잉이 포함된 풍부한 인식 시나리오에 대한 내용은 이전 섹션 [잉크 분석을 통한 풍부한 인식](#free-form-recognition-with-ink-analysis)을 참조하세요.

이 예에서는 사용자가 필기를 마쳤음을 알리는 단추를 클릭하면 인식이 시작됩니다.

**[잉크 필기 인식 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)에서 이 샘플 다운로드**

1.  먼저 UI를 설정합니다.

    UI에는 "인식" 단추, [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 및 인식 결과를 표시할 영역이 포함됩니다.    
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Basic ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <Button x:Name="recognize"
                    Content="Recognize"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2. 이 예에서는 먼저 잉크 기능에 필요한 네임스페이스 유형 참조를 추가해야 합니다.
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)


3.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정합니다.

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 잉크 스트로크는 지정된 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/apps/dn858535)를 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/desktop/ms695050)에 렌더링됩니다. "인식" 단추의 Click 이벤트에 대한 수신기도 선언합니다.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += Recognize_Click;
    }
```

4.  마지막으로 기본 필기 인식을 수행합니다. 이 예제에서는 "인식" 단추의 Click 이벤트 처리기를 사용하여 필기 인식을 수행합니다.

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 모든 잉크 스트로크를 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 개체에 저장합니다. 스트로크는 **InkPresenter**의 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 속성을 통해 노출되고 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 메서드를 사용하여 검색합니다.
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.

```csharp
// Create a manager for the InkRecognizer object
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer =
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (var result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.

```csharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // Create a manager for the InkRecognizer object
            // used in handwriting recognition.
            InkRecognizerContainer inkRecognizerContainer =
                new InkRecognizerContainer();

            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (var result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <a name="international-recognition"></a>국가별 인식

Windows 잉크 플랫폼에 내장된 필기 인식은 Windows에서 지원되는 방대한 로캘 및 언어 하위 집합을 포함하고 있습니다.

[**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478)에서 지원하는 언어 목록은 [**InkRecognizer.Name**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx) 속성 항목을 참조하세요.

앱이 설치된 필기 인식 엔진 집합을 쿼리하고 그 중 하나를 사용할 수도 있고, 사용자가 기본 언어를 선택할 수도 있습니다.

**참고**  
사용자는 **설정 -&gt; 시간 및 언어**로 이동하여 설치된 언어 목록을 볼 수 있습니다. 설치된 언어는 **언어** 아래에 나열됩니다.

새 언어 팩을 설치하고 해당 언어에 대해 필기 인식을 사용하도록 설정하려면 다음과 같이 하세요.

1.  **설정 &gt; 시간 및 언어 &gt; 국가 및 언어**로 이동합니다.
2.  **언어 추가**를 선택합니다.
3.  목록에서 언어를 선택한 다음 국가 버전을 선택합니다. 이제 선택한 언어가 **국가 및 언어** 페이지에 나열됩니다.
4.  언어를 클릭하고 **옵션**을 선택합니다.
5.  **언어 옵션** 페이지에서 **필기 인식 엔진**을 다운로드합니다. 여기서 전체 언어 팩, 음성 인식 엔진 및 키보드 레이아웃을 다운로드할 수도 있습니다.

 

여기서는 필기 인식 엔진을 사용하여 선택한 인식기를 기반으로 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)의 스트로크 집합을 해석하는 방법을 보여 줍니다.

사용자가 쓰기를 마치고 단추를 클릭하면 인식이 시작됩니다.

1.  먼저 UI를 설정합니다.

    UI에는 "인식" 단추, 설치된 필기 인식기를 모두 나열하는 콤보 상자, [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 및 인식 결과를 표시할 영역이 포함됩니다.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Advanced international ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <ComboBox x:Name="comboInstalledRecognizers"
                     Margin="50,0,10,0">
                <ComboBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{Binding Name}" />
                        </StackPanel>
                    </DataTemplate>
                </ComboBox.ItemTemplate>
            </ComboBox>
            <Button x:Name="buttonRecognize"
                    Content="Recognize"
                    IsEnabled="False"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정합니다.

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 잉크 스트로크는 지정된 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/apps/dn858535)를 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/desktop/ms695050)에 렌더링됩니다.

    `InitializeRecognizerList` 함수를 호출하여 인식기 콤보 상자에 설치된 필기 인식기 목록을 채웁니다.

    또한 "인식" 단추의 Click 이벤트 및 인식기 콤보 상자의 선택 항목 변경 이벤트에 대한 수신기도 선언합니다.
```csharp
 public MainPage()
     {
         this.InitializeComponent();

         // Set supported inking device types.
         inkCanvas.InkPresenter.InputDeviceTypes =
             Windows.UI.Core.CoreInputDeviceTypes.Mouse |
             Windows.UI.Core.CoreInputDeviceTypes.Pen;

         // Set initial ink stroke attributes.
         InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
         drawingAttributes.Color = Windows.UI.Colors.Black;
         drawingAttributes.IgnorePressure = false;
         drawingAttributes.FitToCurve = true;
         inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

         // Populate the recognizer combo box with installed recognizers.
         InitializeRecognizerList();

         // Listen for combo box selection.
         comboInstalledRecognizers.SelectionChanged +=
             comboInstalledRecognizers_SelectionChanged;

         // Listen for button click to initiate recognition.
         buttonRecognize.Click += Recognize_Click;
     }
```

3.  인식기 콤보 상자에 설치된 필기 인식기의 목록을 채웁니다.

    필기 인식 프로세스를 관리하기 위해 [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479)를 만듭니다. 이 개체를 사용하여 [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480)를 호출하고 설치된 인식기 목록을 검색하여 인식기 콤보 상자를 채웁니다.
```csharp
// Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers =
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
```

4.  인식기 콤보 상자 선택이 변경되는 경우 필기 인식기를 업데이트합니다.

    인식기 콤보 상자에서 선택한 인식기를 기반으로 [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479)를 사용하여 [**SetDefaultRecognizer**](https://msdn.microsoft.com/library/windows/apps/hh920328)를 호출합니다.
```csharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  마지막으로 선택한 필기 인식기를 기반으로 필기 인식을 수행합니다. 이 예제에서는 "인식" 단추의 Click 이벤트 처리기를 사용하여 필기 인식을 수행합니다.

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 모든 잉크 스트로크를 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 개체에 저장합니다. 스트로크는 **InkPresenter**의 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 속성을 통해 노출되고 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 메서드를 사용하여 검색합니다.
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates =
            result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
    
```csharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates =
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog =
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <a name="dynamic-recognition"></a>동적 인식

앞의 두 예제에서는 사용자가 인식 시작 단추를 눌러야 하지만, 기본 타이밍 기능과 페어링된 스트로크 입력을 사용하여 동적 인식을 수행할 수도 있습니다.

이 예제에서는 이전 국가별 인식 예제와 같은 UI 및 스트로크 설정을 사용합니다.

1. 이러한 전역 개체([InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer))는 앱 전체에서 사용됩니다.    
```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
```

2.  인식을 시작하는 단추 대신 두 가지 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 이벤트 스트로크([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) 및 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702))에 대한 수신기를 추가하고 기본 타이머([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250))를 1초 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) 간격으로 설정합니다.    
```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
```

3.  그런 다음 첫 번째 단계에서 선언한 InkPresenter 이벤트에 대한 처리기를 정의합니다(타이머를 관리할 [**OnNavigatingFrom**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) 페이지 이벤트도 재정의).

    - [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    사용자가 펜 또는 손가락을 떼거나 마우스 단추를 놓아 수동 입력을 중지하면 InkAnalyzer에 잉크 스트로크를 추가하고([**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) 인식 타이머를 시작합니다. 1초 동안 수동 입력이 없으면 인식이 시작됩니다.  

        [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) 메서드를 사용하여 텍스트(문서 구조 및 글머리 기호 목록 포함)만 또는 드로잉(셰이프 인식 포함)만 인식할 것인지 여부를 지정합니다.

    - [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    다음 타이머 틱 이벤트 전에 새 스트로크가 시작되면 새 스트로크는 단일 필기 항목의 연속이 될 가능성이 있으므로 타이머를 중지합니다.
```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }    
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }    
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    } 
```

4.  마지막으로 필기 인식을 수행합니다. 이 예제에서는 [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244256)의 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244250) 이벤트 처리기를 사용하여 필기 인식을 시작합니다.
    - [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync)를 호출하여 잉크 분석을 시작하고 [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)를 가져옵니다.
    - [**Status**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status)에서 **업데이트됨** 상태를 반환하면 [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) 노드 유형에 대해 [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes)를 호출합니다.
    - 노드를 반복하고 인식된 텍스트를 표시합니다.
    - 마지막으로 InkAnalyzer에서 인식된 노드를 삭제하고 잉크 캔버스에서 해당 잉크 스트로크를 삭제합니다.
```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
```

## <a name="related-articles"></a>관련 문서

* [펜 및 스타일러스 조작](pen-and-stylus-interactions.md)

**항목 샘플**
* [잉크 분석 샘플(기본)(C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [잉크 필기 인식 샘플(C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

**기타 샘플**
* [간단한 잉크 샘플(C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [복잡한 잉크 샘플(C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [잉크 샘플(JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [시작 자습서: UWP 앱에서 잉크 지원](https://aka.ms/appsample-ink)
* [색칠하기 책 샘플](https://aka.ms/cpubsample-coloringbook)
* [가족 메모 샘플](https://aka.ms/cpubsample-familynotessample)


 
