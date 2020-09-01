---
Description: 필기 인식 및 잉크 분석을 사용 하 여 Windows 잉크 스트로크를 텍스트 및 모양으로 인식 합니다.
title: Windows Ink 스트로크를 텍스트 및 셰이프로 인식
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, Windows Ink, DirectInk, InkPresenter, InkCanvas, 필기 인식, 사용자 조작, 입력
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bcbb665b2e5920ae4a85cf374d30c3c6a005b1ab
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160107"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Windows Ink 스트로크를 텍스트 및 셰이프로 인식

Windows Ink에 기본 제공 되는 인식 기능을 사용 하 여 잉크 스트로크를 텍스트 및 도형으로 변환 합니다.

> **중요 한 api**: InkCanvas [**,**](/uwp/api/Windows.UI.Input.Inking) [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)

## <a name="free-form-recognition-with-ink-analysis"></a>잉크 분석을 사용한 자유 형식 인식

여기서는 Windows Ink 분석 엔진 (InkCanvas)을 사용 하 여 텍스트 또는 도형으로 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 의 자유 형식 스트로크 집합을 분류, 분석 및 인식 하는 방법을 보여[줍니다.](/uwp/api/windows.ui.input.inking.analysis) 텍스트 및 모양 인식 외에도 잉크 분석을 사용 하 여 문서 구조, 글머리 기호 목록 및 일반 그리기를 인식할 수 있습니다.

> [!NOTE]
> 양식 입력과 같은 기본 한 줄의 일반 텍스트 시나리오의 경우이 항목의 뒷부분에 있는 [제한 된 필기 인식](#constrained-handwriting-recognition) 을 참조 하세요.

이 예제에서는 사용자가 그리기 완료를 나타내기 위해 단추를 클릭할 때 인식이 시작 됩니다.

**[잉크 분석 샘플 (기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip) 에서이 샘플 다운로드**

1. 먼저 UI (MainPage)를 설정 합니다. 

   UI는 "인식" 단추, [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)및 표준 [**캔버스**](/uwp/api/windows.ui.xaml.controls.canvas)를 포함 합니다. "인식" 단추를 누르면 잉크 캔버스의 모든 잉크 스트로크가 분석 되 고 (인식 되는 경우) 해당 셰이프 및 텍스트가 표준 캔버스에 그려집니다. 그러면 원본 잉크 스트로크가 잉크 캔버스에서 삭제 됩니다.

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

2. UI 코드 숨김이 파일 (MainPage.xaml.cs)에서 잉크 및 잉크 분석 기능에 필요한 네임 스페이스 형식 참조를 추가 합니다.
    - [Windows.UI.Input.Inking](/uwp/api/windows.ui.input.inking)
    - [Windows..](/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](/uwp/api/windows.ui.xaml.shapes)

3. 그런 다음 전역 변수를 지정 합니다.

   ```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
   ```

4. 다음에는 몇 가지 기본 잉크 입력 동작을 설정 합니다.
    - [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 펜, 마우스 및 터치에서 입력 데이터를 잉크 스트로크 ([**inputdevicetypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))로 해석 하도록 구성 되어 있습니다. 
    - 잉크 스트로크는 지정 된 [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class)를 사용 하 여 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 에서 렌더링 됩니다. 
    - "인식" 단추의 click 이벤트에 대 한 수신기도 선언 됩니다.

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

5. 이 예에서는 "인식" 단추의 click 이벤트 처리기에서 잉크 분석을 수행 합니다.
    - 먼저 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) 의 [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) 에서 [**getstrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) .를 호출 하 여 모든 현재 잉크 스트로크의 컬렉션을 가져옵니다.
    - 잉크 스트로크가 있는 경우 InkAnalyzer의 [**Adddataforstrokes**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) 호출에 전달 합니다.
    - 드로잉과 텍스트를 모두 인식 하려고 하지만, [**SetStrokeDataKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) 메서드를 사용 하 여 텍스트 (문서 구조 및 글머리 기호 목록 포함) 나 드로잉 (셰이프 인식 포함)에만 관심이 있는지 여부를 지정할 수 있습니다.
    - [**AnalyzeAsync**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) 를 호출 하 여 잉크 분석을 시작 하 고 [**InkAnalysisResult**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)를 가져옵니다.
    - [**Status**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) 가 **업데이트 됨**상태를 반환 하는 경우 [**InkAnalysisNodeKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) 와 [**InkAnalysisNodeKind 그리기**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind)모두에 대해 [**findnodes**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) 를 호출 합니다.
    - 두 노드 형식 집합을 반복 하 여 인식 캔버스 (잉크 캔버스 아래)에 해당 텍스트 또는 셰이프를 그립니다.
    - 마지막으로, InkAnalyzer의 인식 된 노드와 잉크 캔버스의 해당 잉크 스트로크를 삭제 합니다.

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

6. 다음은 인식 캔버스에 TextBlock을 그리기 위한 함수입니다. 잉크 캔버스에서 연결 된 잉크 스트로크의 경계 사각형을 사용 하 여 TextBlock의 위치 및 글꼴 크기를 설정 합니다.

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

7. 다음은 인식 캔버스에서 타원과 다각형을 그리기 위한 함수입니다. 잉크 캔버스에서 연결 된 잉크 스트로크의 경계 사각형을 사용 하 여 모양의 위치 및 글꼴 크기를 설정 합니다.

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

이 샘플의 작업은 다음과 같습니다.

| 분석 전 | 분석 후 |
| --- | --- |
| ![분석 전](images/ink/ink-analysis-raw2-small.png) | ![분석 후](images/ink/ink-analysis-analyzed2-small.png) |

## <a name="constrained-handwriting-recognition"></a>제한 된 필기 인식

이전 섹션 ([잉크 분석을 사용한 자유 형식 인식](#free-form-recognition-with-ink-analysis))에서 [잉크 분석 api](/uwp/api/windows.ui.input.inking.analysis) 를 사용 하 여 InkCanvas 영역 내에서 임의 잉크 스트로크를 분석 하 고 인식 하는 방법을 살펴보았습니다.

이 섹션에서는 Windows Ink 필기 인식 엔진 (잉크 분석 아님)을 사용 하 여 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 의 스트로크 집합을 텍스트 (설치 된 기본 언어 팩 기반)로 변환 하는 방법을 보여 줍니다.

> [!NOTE]
> 이 섹션에 표시 된 기본 필기 인식은 양식 입력과 같은 한 줄의 텍스트 입력 시나리오에 가장 적합 합니다. 문서 구조, 목록 항목, 모양 및 그림 (텍스트 인식 외)의 분석과 해석을 포함 하는 보다 풍부한 인식 시나리오는 이전 섹션인 [잉크 분석을 사용한 자유 형식 인식](#free-form-recognition-with-ink-analysis)을 참조 하세요.

이 예제에서는 사용자가 단추를 클릭 하 여 작성이 완료 되었음을 나타내는 경우 인식이 시작 됩니다.

**[잉크 필기 인식 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip) 에서이 샘플 다운로드**

1. 먼저 UI를 설정 합니다.

   UI에는 "인식" 단추, [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)및 인식 결과를 표시 하는 영역이 포함 되어 있습니다.    

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

2. 이 예에서는 먼저 잉크 기능에 필요한 네임 스페이스 형식 참조를 추가 해야 합니다.
    - [Windows.UI.Input](/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](/uwp/api/windows.ui.input.inking)

3. 그런 다음 몇 가지 기본 잉크 입력 동작을 설정 합니다.

    [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크 ([**inputdevicetypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))로 해석 하도록 구성 됩니다. 잉크 스트로크는 지정 된 [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class)를 사용 하 여 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 에서 렌더링 됩니다. "인식" 단추의 click 이벤트에 대 한 수신기도 선언 됩니다.

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

4. 마지막으로, 기본적인 필기 인식을 수행 합니다. 이 예에서는 "인식" 단추의 click 이벤트 처리기를 사용 하 여 필기 인식을 수행 합니다.

   - [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 개체에 모든 잉크 스트로크를 저장 합니다. 스트로크는 **InkPresenter** 의 [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) 속성을 통해 노출 되 고 [**getstrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes) 메서드를 사용 하 여 검색 됩니다. 

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - 필기 인식 프로세스를 관리 하기 위해 [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) 가 만들어집니다.

    ```csharp
    // Create a manager for the InkRecognizer object
        // used in handwriting recognition.
        InkRecognizerContainer inkRecognizerContainer =
            new InkRecognizerContainer();
    ```

    - [**RecognizeAsync**](/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) 는 [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) 개체 집합을 검색 하기 위해 호출 됩니다. [**Inkrecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer)에서 검색 한 각 단어에 대해 인식 결과가 생성 됩니다.

    ```csharp
    // Recognize all ink strokes on the ink canvas.
        IReadOnlyList<InkRecognitionResult> recognitionResults =
            await inkRecognizerContainer.RecognizeAsync(
                inkCanvas.InkPresenter.StrokeContainer,
                InkRecognitionTarget.All);
    ```

    - 각 [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) 개체에는 텍스트 후보 집합이 포함 되어 있습니다. 이 목록의 맨 위에 있는 항목은 인식 엔진이 가장 일치 하는 것으로 간주 되 고, 그 다음에는 신뢰도가 줄어드는 순서로 남은 후보가 표시 됩니다.

       각 [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) 를 반복 하 고 후보 목록을 컴파일합니다. 그러면 후보가 표시 되 고 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 가 지워집니다 ( [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)도 지움).

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

    - 전체에서 클릭 처리기 예제는 다음과 같습니다.

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

## <a name="international-recognition"></a>국제 인식

Windows ink 플랫폼에 기본 제공 되는 필기 인식에는 Windows에서 지 원하는 로캘 및 언어의 광범위 한 하위 집합이 포함 되어 있습니다.

[**Inkrecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer) 에서 지 원하는 언어 목록은 [**InkRecognizer.Name**](/uwp/api/windows.ui.input.inking.inkrecognizer.name) 속성 항목을 참조 하세요.

앱은 설치 된 필기 인식 엔진 집합을 쿼리하고 이러한 엔진 중 하나를 사용 하거나 사용자가 기본 설정 언어를 선택할 수 있도록 합니다.

**참고**    사용자는 **설정- &gt; 시간 & 언어로**이동 하 여 설치 된 언어 목록을 볼 수 있습니다. 설치 된 언어는 **언어**아래에 나열 됩니다.

새 언어 팩을 설치 하 고 해당 언어에 대해 필기 인식을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. **설정 &gt; 시간 & 언어 &gt; 지역 & 언어**로 이동 합니다.
2. **언어 추가를**선택 합니다.
3. 목록에서 언어를 선택한 다음 지역 버전을 선택 합니다. 이제 언어가 **지역 & 언어** 페이지에 나열 됩니다.
4. 언어를 클릭 하 고 **옵션**을 선택 합니다.
5. **언어 옵션** 페이지에서 **필기 인식 엔진** 을 다운로드 합니다. 여기에서 전체 언어 팩, 음성 인식 엔진 및 자판 배열을 다운로드할 수도 있습니다.

여기서는 필기 인식 엔진을 사용 하 여 선택한 인식기에 따라 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 의 스트로크 집합을 해석 하는 방법을 보여 줍니다.

사용자는 작성이 완료 되 면 단추를 클릭 하 여 인식을 시작 합니다.

1. 먼저 UI를 설정 합니다.

   UI는 "인식" 단추, 설치 된 모든 필기 인식기, [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)및 인식 결과를 표시 하는 영역을 나열 하는 콤보 상자를 포함 합니다.

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

2. 그런 다음 몇 가지 기본 잉크 입력 동작을 설정 합니다.

   [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크 ([**inputdevicetypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))로 해석 하도록 구성 됩니다. 잉크 스트로크는 지정 된 [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class)를 사용 하 여 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 에서 렌더링 됩니다.

   함수를 호출 `InitializeRecognizerList` 하 여 설치 된 필기 인식기의 목록으로 인식기 콤보 상자를 채웁니다.

   또한 인식기 콤보 상자의 "인식" 단추 및 선택 변경 이벤트에서 클릭 이벤트에 대 한 수신기를 선언 합니다.

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

3. 설치 된 필기 인식기 목록으로 인식기 콤보 상자를 채웁니다.

   필기 인식 프로세스를 관리 하기 위해 [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) 가 만들어집니다. 이 개체를 사용 하 여 [**Getrecognizers**](/uwp/api/windows.ui.input.inking.inkrecognizercontainer.getrecognizers) 를 호출 하 고 설치 된 인식자 목록을 검색 하 여 인식기 콤보 상자를 채웁니다.

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
    
4. 인식기 콤보 상자 선택이 변경 되 면 필기 인식기를 업데이트 합니다.

   [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) 를 사용 하 여 인식기 콤보 상자에서 선택한 인식기를 기반으로 [**setdefaultrecognizer**](/uwp/api/windows.ui.input.inking.inkrecognizercontainer.setdefaultrecognizer) 를 호출 합니다.

    ```csharp
    // Handle recognizer change.
        private void comboInstalledRecognizers_SelectionChanged(
            object sender, SelectionChangedEventArgs e)
        {
            inkRecognizerContainer.SetDefaultRecognizer(
                (InkRecognizer)comboInstalledRecognizers.SelectedItem);
        }
    ```

5. 마지막으로, 선택 된 필기 인식기에 따라 필기 인식을 수행 합니다. 이 예에서는 "인식" 단추의 click 이벤트 처리기를 사용 하 여 필기 인식을 수행 합니다.

   - [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 개체에 모든 잉크 스트로크를 저장 합니다. 스트로크는 **InkPresenter** 의 [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) 속성을 통해 노출 되 고 [**getstrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes) 메서드를 사용 하 여 검색 됩니다.

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - [**RecognizeAsync**](/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) 는 [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) 개체 집합을 검색 하기 위해 호출 됩니다.

      [**Inkrecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer)에서 검색 한 각 단어에 대해 인식 결과가 생성 됩니다.

    ```csharp
    // Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
    ```

    - 각 [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) 개체에는 텍스트 후보 집합이 포함 되어 있습니다. 이 목록의 맨 위에 있는 항목은 인식 엔진이 가장 일치 하는 것으로 간주 되 고, 그 다음에는 신뢰도가 줄어드는 순서로 남은 후보가 표시 됩니다.

       각 [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) 를 반복 하 고 후보 목록을 컴파일합니다. 그러면 후보가 표시 되 고 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 가 지워집니다 ( [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)도 지움).

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

    - 전체에서 클릭 처리기 예제는 다음과 같습니다.

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

앞의 두 예제에서는 사용자가 단추를 눌러 인식을 시작 해야 하지만 기본 타이밍 함수와 쌍을 이루는 스트로크 입력을 사용 하 여 동적 인식을 수행할 수도 있습니다.

이 예에서는 이전 국제 인식 예제와 동일한 UI 및 스트로크 설정을 사용 합니다.

1. 이러한 전역 개체 ([Inkanalyzer](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [inkanalyzer](/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [dispatchertimer](/uwp/api/windows.ui.xaml.dispatchertimer))는 앱 전체에서 사용 됩니다.

    ```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
    ```

2. 인식을 시작 하는 단추 대신 두 개의 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 스트로크 이벤트 ([**StrokesCollected**](/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected) 및 [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted))에 대 한 수신기를 추가 하 고 1 초 [**눈금**](/uwp/api/windows.ui.xaml.dispatchertimer.tick) 간격을 사용 하 여 기본 타이머 ([**dispatchertimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer))를 설정 합니다.

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

3. 그런 다음 첫 번째 단계에서 선언한 InkPresenter 이벤트에 대 한 처리기를 정의 합니다. [**OnNavigatingFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) page 이벤트를 재정의 하 여 타이머를 관리 합니다.

    - [**StrokesCollected**](/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected)  
    잉크 스트로크 ([**Adddataforstrokes**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes))를 inkanalyzer에 추가 하 고 사용자가 잉크를 중지할 때 (펜 또는 손가락을 늘리거나 마우스 단추를 놓으면) 인식 타이머를 시작 합니다. 잉크 입력 중 1 초 후 인식이 시작 됩니다.  

        [**SetStrokeDataKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) 메서드를 사용 하 여 텍스트에만 관심이 있는지 (문서 구조 amd 글머리 기호 목록 포함) 아니면 드로잉에만 관심이 있는지 여부를 지정 합니다 (inlout.shape 인식).

    - [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)  
    새 스트로크가 다음 타이머 tick 이벤트 앞에서 시작 되는 경우 새 스트로크가 단일 필기 항목의 연속으로 나올 가능성이 있으므로 타이머를 중지 합니다.

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

4. 마지막으로 필기 인식을 수행 합니다. 이 예제에서는 [**Dispatchertimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer) 의 [**Tick**](/uwp/api/windows.ui.xaml.dispatchertimer.tick) 이벤트 처리기를 사용 하 여 필기 인식을 시작 합니다.
    - [**AnalyzeAsync**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) 를 호출 하 여 잉크 분석을 시작 하 고 [**InkAnalysisResult**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)를 가져옵니다.
    - [**Status**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) 가 **업데이트 됨**상태를 반환 하는 경우 [**InkAnalysisNodeKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind)의 노드 형식에 대해 [**findnodes**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) 를 호출 합니다.
    - 노드를 반복 하 고 인식 된 텍스트를 표시 합니다.
    - 마지막으로, InkAnalyzer의 인식 된 노드와 잉크 캔버스의 해당 잉크 스트로크를 삭제 합니다.

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

## <a name="related-articles"></a>관련된 문서

- [펜 및 스타일러스 상호 작용](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>토픽 샘플

- [잉크 분석 샘플 (기본) (c #)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
- [잉크 필기 인식 샘플 (c #)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

### <a name="other-samples"></a>기타 샘플

- [단순 잉크 샘플 (c #/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [복합 잉크 샘플 (c + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink 샘플 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [자습서 시작: Windows 앱에서 잉크 지원](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [서적 샘플 강조](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [제품군 정보 샘플](https://github.com/Microsoft/Windows-appsample-familynotes)