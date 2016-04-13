---
잉크 스트로크를 필기 인식을 사용하여 텍스트로 변환하거나 사용자 지정 인식을 사용하여 모양으로 변환합니다.
잉크 스트로크 인식
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
잉크 스트로크 인식
template: detail.hbs
---

# 잉크 스트로크 인식


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)

잉크 스트로크를 필기 인식을 사용하여 텍스트로 변환하거나 사용자 지정 인식을 사용하여 모양으로 변환합니다.

필기 인식은 Windows 잉크 플랫폼에 기본 제공되며 다양한 로캘 및 언어를 지원합니다.

여기의 모든 예제에서 잉크 기능에 필요한 네임스페이스 참조를 추가하세요. 여기에는 "Windows.UI.Input.Inking"이 포함됩니다.

## <span id="Basic_handwriting_recognition"> </span> <span id="basic_handwriting_recognition"> </span> <span id="BASIC_HANDWRITING_RECOGNITION"> </span>기본 필기 인식


여기에서는 설치된 기본 언어 팩과 연결된 필기 인식 엔진을 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)에서 일련의 스트로크를 해석하는 방법을 보여 줍니다.

사용자가 쓰기를 마치고 단추를 클릭하면 인식이 시작됩니다.

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

2.  그런 다음 몇 가지 기본 잉크 입력 동작을 설정합니다.

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 잉크 스트로크는 지정된 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)를 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)에 렌더링됩니다. "인식" 단추의 Click 이벤트에 대한 수신기도 선언합니다.

```    CSharp
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

3.  마지막으로 기본 필기 인식을 수행합니다. 이 예제에서는 "인식" 단추의 Click 이벤트 처리기를 사용하여 필기 인식을 수행합니다.

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 모든 잉크 스트로크를 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 개체에 저장합니다. 스트로크는 **InkPresenter**의 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 속성을 통해 노출되고 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 메서드를 사용하여 검색합니다.

```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.

```    CSharp
// Create a manager for the InkRecognizer object 
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer = 
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults = 
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer, 
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```    CSharp
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

```    CSharp
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

## <span id="International_recognition"> </span> <span id="international_recognition"> </span> <span id="INTERNATIONAL_RECOGNITION"> </span>국가별 인식


Windows에서 지원되는 언어의 포괄적인 하위 집합을 필기 인식에 사용할 수 있습니다.

다음 표에는 [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478)에서 지원되는 언어가 나와 있습니다. 첫 번째 열에는 가능한 [**Name**](https://msdn.microsoft.com/library/windows/apps/br208484) 값이 포함되어 있습니다.

| 이름                                                            | IETF 언어 태그 | 적용 범위                                                                      |
|-----------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------|
| Microsoft 영어(미국) 필기 인식기                   | en-US             | 미국 및 필리핀의 영어                                       |
| Microsoft 영어(영국) 필기 인식기                   | en-GB             | 영국 및 이 표에 지정되지 않은 다른 모든 위치의 영어 |
| Microsoft English (Canada) Handwriting Recognizer               | en-CA             | 캐나다의 영어                                                             |
| Microsoft English (Australia) Handwriting Recognizer            | en-AU             | 오스트레일리아의 영어                                                          |
| Microsoft-Handschrifterkennung - Deutsch                        | de-DE             | 독일, 오스트리아, 룩셈부르크 및 나미비아의 독일어                           |
| Microsoft-Handschrifterkennung - Deutsch (Schweiz)              | de-CH             | 스위스 및 리히텐슈타인의 독일어                                       |
| Reconocimiento de escritura a mano en español de Microsoft      | es-ES             | 스페인 및 이 표에 지정되지 않은 다른 모든 위치의 스페인어         |
| Reconocedor de escritura en Español (México) de Microsoft       | es-MX             | 멕시코 및 미국의 스페인어                                       |
| Reconocedor de escritura en Español (Argentina) de Microsoft    | es-AR             | 아르헨티나, 파라과이 및 우루과이의 스페인어                                   |
| Reconnaissance d'écriture Microsoft - Français                  | fr                | 프랑스, 캐나다, 벨기에, 스위스 및 다른 모든 위치의 프랑스어       |
| Microsoft 日本語手書き認識エンジン                              | ja                | 모든 위치의 일본어                                                     |
| Riconoscimento grafia italiana Microsoft                        | it                | 이탈리아, 스위스 및 다른 모든 위치의 이탈리아어                        |
| Microsoft Nederlandstalige handschriftherkenning                | nl-NL             | 네덜란드, 수리남 및 앤틸리스의 네덜란드어                           |
| Microsoft Nederlandstalige (België) handschriftherkenning       | nl-BE             | 벨기에의 네덜란드어(플라망어)                                                    |
| Microsoft 中文(简体)手写识别器                                  | zh                | 간체 스크립트의 중국어                                                  |
| Microsoft 中文(繁體)手寫辨識器                                  | zh-Hant           | 번체 스크립트의 중국어                                                 |
| Microsoft система распознавания русского рукописного ввода      | ru                | 모든 위치의 러시아어                                                      |
| Reconhecedor de Manuscrito da Microsoft para Português (Brasil) | pt-BR             | 브라질의 포르투갈어                                                          |
| Reconhecedor de escrita manual da Microsoft para português      | pt-PT             | 포르투갈 및 다른 모든 위치의 포르투갈어                               |
| Microsoft 한글 필기 인식기                                      | ko                | 모든 위치의 한국어                                                       |
| System rozpoznawania polskiego pisma odręcznego firmy Microsoft | pl                | 모든 위치의 폴란드어                                                       |
| Microsoft Handskriftstolk för svenska                           | sv                | 모든 위치의 스웨덴어                                                      |
| Microsoft rozpoznávač rukopisu pro český jazyk                  | cs                | 모든 위치의 체코어                                                        |
| Microsoft Genkendelse af dansk håndskrift                       | da                | 모든 위치의 덴마크어                                                       |
| Microsoft Håndskriftsgjenkjenner for norsk                      | nb                | 모든 위치의 노르웨이어(복말)                                           |
| Microsoft Håndskriftsgjenkjenner for nynorsk                    | nn                | 모든 위치의 노르웨이어(니노르스크)                                          |
| Microsoftin suomenkielinen käsinkirjoituksen tunnistus          | fi                | 모든 위치의 핀란드어                                                      |
| Microsoft recunoaştere grafie - Română                          | ro                | 모든 위치의 루마니아어                                                     |
| Microsoftov hrvatski rukopisni prepoznavač                      | hr                | 모든 위치의 크로아티아어                                                     |
| Microsoft prepoznavač rukopisa za srpski (latinica)             | sr-Latn           | 모든 위치의 세르비아어(라틴 문자)                                           |
| Microsoft препознавач рукописа за српски (ћирилица)             | sr                | 모든 위치의 세르비아어(키릴 자모)                                        |
| Reconeixedor d'escriptura manual en català de Microsoft         | ca                | 모든 위치의 카탈로니아어                                                      |

 

앱에서 설치된 필기 인식 엔진 집합을 쿼리하고 이 중 하나를 선택하거나 사용자가 기본 언어를 선택할 수 있습니다.

**참고**  
사용자는 설정 -&gt; 시간 및 언어로 이동하여 설치된 언어 목록을 볼 수 있습니다. 설치된 언어는 언어 아래에 나열됩니다.

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

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되어 있습니다([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 잉크 스트로크는 지정된 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)를 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)에 렌더링됩니다.

    `InitializeRecognizerList` 함수를 호출하여 인식기 콤보 상자에 설치된 필기 인식기 목록을 채웁니다.

    또한 "인식" 단추의 Click 이벤트 및 인식기 콤보 상자의 선택 항목 변경 이벤트에 대한 수신기도 선언합니다.

```    CSharp
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

```    CSharp
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

```    CSharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  마지막으로 선택한 필기 인식기를 기반으로 필기 인식을 수행합니다. 이 예제에서는 "인식" 단추의 Click 이벤트 처리기를 사용하여 필기 인식을 수행합니다.

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 모든 잉크 스트로크를 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 개체에 저장합니다. 스트로크는 **InkPresenter**의 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 속성을 통해 노출되고 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 메서드를 사용하여 검색합니다.

```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = 
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```    CSharp
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

```    CSharp
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

## <span id="Dynamic_handwriting_recognition"> </span> <span id="dynamic_handwriting_recognition"> </span> <span id="DYNAMIC_HANDWRITING_RECOGNITION"> </span>동적 필기 인식


앞의 두 예제에서는 사용자가 버튼을 눌러 인식을 시작해야 합니다. 또한 스트로크 입력과 함께 기본 타이밍 함수를 사용하여 동적 인식을 수행할 수도 있습니다.

이 예제에서는 이전 국가별 인식 예제와 같은 UI 및 스트로크 설정을 사용합니다.

1.  이전 예제와 마찬가지로 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석하도록 구성되고([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) 잉크 스트로크는 지정된 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)를 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)에 렌더링됩니다.

    인식을 시작하는 단추 대신 두 가지 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 이벤트 스트로크([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) 및 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702))에 대한 수신기를 추가하고 기본 타이머([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250))를 1초 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) 간격으로 설정합니다.

```    CSharp
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

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += 
            inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted += 
            inkCanvas_StrokeStarted;

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = new TimeSpan(0, 0, 1);
        recoTimer.Tick += recoTimer_Tick;
    }

    // Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
    
```

2.  첫 단계에서 추가한 세 가지 이벤트에 대한 처리기는 다음과 같습니다.

    <span id="StrokesCollected"> </span> <span id="strokescollected"> </span> <span id="STROKESCOLLECTED"> </span> [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    사용자가 펜 또는 손가락을 떼거나 마우스 단추를 놓아 수동 입력을 중지하면 인식 타이머를 시작합니다. 1초 동안 수동 입력이 없으면 인식이 시작됩니다.

    <span id="StrokeStarted"> </span> <span id="strokestarted"> </span> <span id="STROKESTARTED"> </span> [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    다음 타이머 틱 이벤트 전에 새 스트로크가 시작되면 새 스트로크는 단일 필기 항목의 연속이 될 가능성이 있으므로 타이머를 중지합니다.

    <span id="Tick"> </span> <span id="tick"> </span> <span id="TICK"> </span> [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)  
    1초 동안 수동 입력이 없으면 인식 함수를 호출합니다.

```    CSharp
// Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
```

3.  마지막으로 선택한 필기 인식기를 기반으로 필기 인식을 수행합니다. 이 예제에서는 [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)의 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) 이벤트 처리기를 사용하여 필기 인식을 시작합니다.

    [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 모든 잉크 스트로크를 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 개체에 저장합니다. 스트로크는 **InkPresenter**의 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 속성을 통해 노출되고 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 메서드를 사용하여 검색합니다.

```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
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

    Here's the recognition function, in full.

```    CSharp
// Respond to timer Tick and initiate recognition.
    private async void Recognize_Tick()
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

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

        // Stop the dynamic recognition timer.
        recoTimer.Stop();
    }
```

## <span id="related_topics"> </span>관련 문서


* [펜 및 스타일러스 조작](pen-and-stylus-interactions.md)
**샘플**
* [잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [간단한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [복잡한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 






<!--HONumber=Mar16_HO1-->


