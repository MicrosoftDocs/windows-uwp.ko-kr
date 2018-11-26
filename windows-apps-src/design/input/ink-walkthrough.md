---
ms.assetid: ''
title: UWP 앱에서 잉크 지원
description: UWP 앱에 잉크 지원을 추가하는 단계별 자습서입니다.
keywords: 잉크, 잉크 입력, 자습서
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cc650c1f81fbcac5b62b090a6dc58b5f8709cd7a
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7720409"
---
# <a name="tutorial-support-ink-in-your-uwp-app"></a>자습서: UWP 앱에서 잉크 지원

![Surface 펜](images/ink/ink-hero-small.png)  
*Surface 펜*([Microsoft 스토어](https://aka.ms/purchasesurfacepen)에서 구매 가능)

이 자습서는 Windows Ink를 사용한 쓰기와 그리기를 지원하는 기본적인 UWP(유니버설 Windows 플랫폼) 앱을 만드는 방법을 단계별로 설명합니다. GitHub에서 다운로드할 수 있는 샘플 앱의 코드 조각을 사용할 것입니다. Windows Ink API에 관련된 다양한 기능을 설명하는 [샘플 코드](#sample-code))와 각 단계에서 설명하는 [Windows Ink 플랫폼의 구성 요소](#components-of-the-windows-ink-platform)를 참조하세요.

중점을 두고 살펴볼 내용은 다음과 같습니다.
* 기본 잉크 지원 추가
* 잉크 도구 모음 추가
* 필기 인식 지원
* 기본 모양 인식 지원
* 잉크 저장 및 로드

이러한 기능을 구현하는 것에 대한 자세한 내용은 [UWP 앱에서 펜 상호 작용 및 Windows Ink](https://docs.microsoft.com/windows/uwp/input/pen-and-stylus-interactions)를 참조하세요.

## <a name="introduction"></a>소개

Windows Ink를 사용하면 상상할 수 있는 거의 모든 펜과 종이 환경과 동일한 디지털 환경을 고객에게 제공할 수 있습니다. 이를 통해 빠른 필기 노트, 화이트보드 데모에 주석 추가, 건축 및 엔지니어링 도면에서 개인 작품에 이르기까지 모든 것을 적고 그릴 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

* Windows 10 최신 버전을 실행하는 Windows 컴퓨터(또는 가상 컴퓨터)
* [Visual Studio 2017 및 RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [Windows10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 구성에 따라 [Microsoft.NETCore.UniversalWindowsPlatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/6.1.9) NuGet 패키지를 설치 하 고 시스템 설정에서 **개발자 모드** 를 사용 해야 할 수 있습니다 (설정->는 업데이트 및 보안 개발자->에 대 한-> 개발자 기능을 사용).
* Visual Studio를 사용하는 UWP(유니버설 Windows 플랫폼) 앱 개발을 처음 하는 경우, 이 자습서를 시작하기 전에 이러한 항목을 살펴보십시오.  
    * [설정하기](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * ["Hello, World" 앱 만들기(XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* **[선택 사항] ** 디지털 펜의 입력을 지원하는 디스플레이가 있는 컴퓨터와 디지털 펜.

> [!NOTE] 
> Windows Ink는 최적의 Windows Ink 환경을 위해 마우스와 터치를 사용한 그리기(이 자습서의 3단계에서 방법 설명)를 지원하지만, 디지털 펜과 디지털 펜의 입력을 지원하는 디스플레이가 있는 컴퓨터를 사용하는 것이 좋습니다.

## <a name="sample-code"></a>샘플 코드
이 자습서에서는 샘플 잉크 앱을 사용하여 개념과 기능을 설명할 것입니다.

[windows-appsample-get-started-ink 샘플](https://aka.ms/appsample-ink)의 [GitHub](https://github.com/)에서 Visual Studio 샘플과 소스 코드를 다운로드하세요.

1. 녹색 **복제 또는 다운로드** 단추를 선택합니다.  
![리포지토리 복제](images/ink/ink-clone.png)
2. GitHub 계정이 있는 경우 **Visual Studio에서 열기**를 선택하여 리포지토리를 로컬 컴퓨터로 복제할 수 있습니다. 
3. GitHub 계정이 없거나 프로젝트의 로컬 복사본만 원한다면 **ZIP 다운로드**를 선택합니다. 최신 업데이트를 다운로드하기 위해 주기적으로 확인해야 합니다.

> [!IMPORTANT]
> 샘플의 대부분 코드는 주석으로 처리됩니다. 각 단계를 진행하면서 코드의 여러 섹션에 대한 주석 처리를 제거하라는 지침이 있을 것입니다. Visual Studio에서 코드 줄을 강조 표시하고 CTRL-K를 누른 다음 CTRL-U를 누릅니다.

## <a name="components-of-the-windows-ink-platform"></a>Windows Ink 플랫폼의 구성 요소

이러한 개체는 UWP 앱을 위한 많은 잉크 환경을 제공합니다.

| 구성 요소 | 설명 |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) | 기본적으로 받아 펜의 모든 입력을 잉크 스트로크 또는 지우기 스트로크로 표시 XAMLUI 플랫폼 컨트롤입니다. |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤([**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) 속성을 통해 노출)과 함께 인스턴스화되는 코드 숨김 개체입니다. 이 개체는 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)에서 노출하는 모든 기본 수동 입력 기능과 추가 사용자 지정 및 개인 설정을 위한 포괄적인 API 집합을 제공합니다. |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | 사용자 지정 및 확장이 가능한 관련된 된 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)에서 잉크 관련 기능을 활성화 하는 단추 컬렉션이 포함 된 XAMLUI 플랫폼 컨트롤입니다. |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263)<br/>여기서는 이 기능을 다루지 않습니다. 자세한 내용은 [복잡한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620314)을 참조하세요. | 기본 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤 대신 유니버설 Windows 앱의 지정된 Direct2D 장치 컨텍스트 위에 잉크 스트로크를 렌더링할 수 있도록 합니다. |

## <a name="step-1-run-the-sample"></a>1단계: 샘플 실행

RadialController 샘플 앱을 다운로드한 후 실행되는지 확인합니다.
1. Visual Studio에서 샘플 프로젝트를 엽니다.
2. **솔루션 플랫폼** 드롭다운을 비-ARM 선택 항목으로 설정합니다.
3. F5를 눌러 컴파일하고, 배포하고, 실행합니다.  

   > [!NOTE]
   > 또는 **디버그** > **디버깅 시작** 메뉴 항목을 선택하거나, 여기에 표시된 **로컬 컴퓨터** 실행 단추를 선택합니다.
   > ![Visual Studio 프로젝트 빌드 단추](images/ink/ink-vsrun-small.png)

앱 창이 열리고 몇 초 동안 시작 화면이 나타난 후 이 초기 화면이 표시됩니다.

![빈 앱](images/ink/ink-app-step1-empty-small.png)

이제 이 자습서의 나머지 부분에서 사용할 기본 UWP 앱이 준비되었습니다. 다음 단계에서 잉크 기능을 추가할 것입니다.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>2단계: 기본 수동 입력 지원을 위해 InkCanvas 사용

초기 형태의 앱에서는 표준 포인터 디바이스로 펜을 사용하여 앱과 상호 작용할 수는 있지만, 펜으로 그릴 수는 없습니다. 

이 단계에서 이 작은 단점을 해결해 보겠습니다.

기본 수동 입력 기능을 추가하려면 앱의 적절한 페이지에 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) UWP 플랫폼 컨트롤을 배치합니다.

> [!NOTE]
> InkCanvas의 기본 [**높이**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height)와 [**너비**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width)는 자녀 요소의 크기를 자동으로 설정하는 요소의 자식이 아닌 경우 0입니다. 

### <a name="in-the-sample"></a>이 샘플에서:
1. MainPage.xaml.cs 파일을 엽니다.
2. 이 단계의 제목("// Step 2: Use InkCanvas to support basic inking")으로 표시된 코드를 찾습니다.
3. 다음 줄의 주석 처리를 제거합니다. (이 참조는 다음 단계에 사용되는 기능에 필요합니다.)  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. MainPage.xaml 파일을 엽니다.
5. 이 단계의 제목("\<!-- Step 2: Basic inking with InkCanvas -->")으로 표시된 코드를 찾습니다.
6. 다음 줄의 주석 처리를 제거합니다.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

이제 되었습니다. 

이제 앱을 다시 실행합니다. 낙서하거나, 이름을 쓰거나, 거울이 있거나 기억력이 좋다면 자신의 얼굴을 그려 보십시오.

![기본 수동 입력](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>3단계: 터치와 마우스를 사용한 수동 입력 지원

기본적으로 잉크는 펜 입력에 대해서만 지원됩니다. 손가락, 마우스나 터치 패드를 사용하여 쓰거나 그려보면 실망할 것입니다.

실망을 해소하려면, 코드의 두 번째 줄을 추가해야 합니다. 이번에는 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)에서 선언한 XAML 파일에 대한 코드 숨김입니다. 

이 단계에서는 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)에서 잉크 입력(표준 및 수정)의 입력, 처리, 렌더링을 더 정밀하게 관리하는 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) 개체를 사용합니다.

> [!NOTE]
> 표준 잉크 입력(펜 팁 또는 지우개 팁/단추)은 펜 단추, 마우스 오른쪽 단추 또는 유사한 메커니즘과 같은 보조 하드웨어 어포던스를 사용하여 수정되지 않습니다. 

마우스와 터치 수동 입력을 사용하려면, [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter)의 [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) 속성을 원하는 [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) 값의 조합으로 설정합니다.

### <a name="in-the-sample"></a>이 샘플에서:
1. MainPage.xaml.cs 파일을 엽니다.
2. 이 단계의 제목("// Step 3: Support inking with touch and mouse")으로 표시된 코드를 찾습니다.
3. 다음 줄의 주석 처리를 제거합니다.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

앱을 다시 실행하면 컴퓨터 화면에 손가락으로 그리는 꿈이 현실화되었음을 알 수 있습니다.

> [!NOTE]
> 입력 디바이스 유형을 지정할 때는 이 속성이 기본 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 설정을 덮어쓰기 때문에 특정한 각 입력 유형(펜 포함)에 대한 지원을 설정해야 합니다.

## <a name="step-4-add-an-ink-toolbar"></a>4단계: 잉크 도구 모음 추가

[**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)는 잉크 관련 기능을 활성화하는 사용자 지정 및 확장 가능한 단추 모음을 제공하는 UWP 플랫폼 컨트롤입니다. 

기본적으로 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)에는 사용자가 펜, 연필, 형광펜, 지우개, 스텐실과 함께 사용할 수 있는 모든 도구(눈금자 또는 각도기) 사이에서 빠르게 선택할 수 있는 기본 단추 세트를 포함하고 있습니다. 펜, 연필, 형광펜 단추는 또한 각각 잉크 색과 스트로크 크기를 선택하기 위한 플라이아웃을 제공합니다.

기본 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)를 수동 입력 앱에 추가하려면, [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)와 동일한 페이지에 배치하고 두 개의 컨트롤을 연결합니다.

### <a name="in-the-sample"></a>이 샘플에서:
1. MainPage.xaml 파일을 엽니다.
2. 이 단계의 제목("\<!-- Step 4: Add an ink toolbar -->")으로 표시된 코드를 찾습니다.
3. 다음 줄의 주석 처리를 제거합니다.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> UI와 코드를 최대한 간결하고 간단하게 유지하기 위해, 기본 그리드 레이아웃을 사용하고 그리드 행에서 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 뒤에 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)를 선언합니다. [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 전에 선언할 경우 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)가 사용자가 접근할 수 없는 캔버스 아래쪽에 먼저 렌더링됩니다.  

이제 다시 앱을 실행하고 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)가 보이는지 확인하고 도구를 사용해 보십시오.

![작업 영역 스케치북의 InkToolbar](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>과제: 사용자 지정 단추 추가
<table class="wdg-noborder">
<tr>
<td>

![작업 영역 스케치북의 InkToolbar](images/challenge-icon.png)

</td>
<td>

다음은 사용자 지정 **[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)** 의 예(Windows Ink 작업 영역의 스케치북)입니다.

![작업 영역 스케치북의 InkToolbar](images/ink/ink-inktoolbar-sketchpad-small.png)

[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)를 사용자 지정하는 방법은 [UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 InkToolbar 추가](ink-toolbar.md)를 참조하세요.

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>5단계: 필기 인식 지원

이제 앱에서 쓰고 그릴 수 있으므로, 이러한 낙서로 더 유용한 무언가를 해보겠습니다.

이 단계에서는 Windows Ink의 필기 인식을 기능을 사용하여 작성한 문자를 해독할 것입니다.

> [!NOTE]
> 필기 인식은 **펜 및 Windows Ink** 설정을 통해 향상됩니다.
> 1. 시작 메뉴를 열고 **설정**을 선택합니다.
> 2. 설정 화면에서 **디바이스** > **펜 및 Windows Ink**를 선택합니다.
> ![작업 영역 스케치북의 InkToolbar](images/ink/ink-settings-small.png)
> 3. 선택 **내 필기 이해**를 선택하여 **필기 개인 설정** 대화 상자를 엽니다.
> ![작업 영역 스케치북의 InkToolbar](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>이 샘플에서:
1. MainPage.xaml 파일을 엽니다.
2. 이 단계의 제목("\<!-- Step 5: Support handwriting recognition -->")으로 표시된 코드를 찾습니다.
3. 다음 줄의 주석 처리를 제거합니다.  

``` xaml
    <Button x:Name="recognizeText" 
            Content="Recognize text"  
            Grid.Row="0" Grid.Column="0"
            Margin="10,10,10,10"
            Click="recognizeText_ClickAsync"/>
    <TextBlock x:Name="recognitionResult" 
                Text="Recognition results: "
                VerticalAlignment="Center" 
                Grid.Row="0" Grid.Column="1"
                Margin="50,0,0,0" />
```

4. MainPage.xaml.cs 파일을 엽니다.
5. 이 단계의 제목(" Step 5: Support handwriting recognition")으로 표시된 코드를 찾습니다.
6. 다음 줄의 주석 처리를 제거합니다.  

- 이는 이 단계에 필요한 전역 변수입니다.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- 이는 인식 처리를 수행하는 **텍스트 인식** 단추에 대한 처리기입니다.

``` csharp
    private async void recognizeText_ClickAsync(object sender, RoutedEventArgs e)
    {
        strokesText = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (strokesText.Count > 0)
        {
            analyzerText.AddDataForStrokes(strokesText);

            resultText = await analyzerText.AnalyzeAsync();

            if (resultText.Status == InkAnalysisStatus.Updated)
            {
                words = analyzerText.AnalysisRoot.FindNodes(InkAnalysisNodeKind.InkWord);
                foreach (var word in words)
                {
                    InkAnalysisInkWord concreteWord = (InkAnalysisInkWord)word;
                    foreach (string s in concreteWord.TextAlternates)
                    {
                        recognitionResult.Text += s;
                    }
                }
            }
            analyzerText.ClearDataForAllStrokes();
        }
    }
```

7. 앱을 다시 실행하고, 글을 쓴 다음 **텍스트 인식** 단추를 클릭합니다.
8. 인식의 결과가 단추 옆에 표시됩니다.

### <a name="challenge-1-international-recognition"></a>과제 1: 국가별 인식
<table class="wdg-noborder">
<tr>
<td>

![작업 영역 스케치북의 InkToolbar](images/challenge-icon.png)

</td>
<td>

Windows Ink는 Windows에서 지원하는 많은 언어에 대한 문자 인식을 지원합니다. 각 언어 팩에는 언어 팩과 함께 설치되는 필기 인식 엔진이 포함되어 있습니다.

설치된 필기 인식 엔진을 쿼리하여 특정 언어를 대상으로 선택합니다.

국가별 필기 인식에 대한 자세한 내용은 [Windows Ink 스트로크를 문자로 인식](convert-ink-to-text.md)을 참조하세요.

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>과제 2: 동적 인식
<table class="wdg-noborder">
<tr>
<td>

![작업 영역 스케치북의 InkToolbar](images/challenge-icon.png)

</td>
<td>

이 자습서의 초기 인식을 위해 단추를 눌러야 합니다. 또한 기본 타이밍 기능을 사용하여 동적 인식을 수행하고 있습니다.

동적 인식에 대한 자세한 내용은 [Windows Ink 스트로크를 문자로 인식](convert-ink-to-text.md)을 참조하세요.

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>6단계: 모양 인식

이제 필기 노트를 조금 더 명확한 내용으로 변환할 수 있습니다. 하지만 아침 플로우차트 익명 회의에서 커피를 마시며 끄적인 낙서라면 어떨까요?

잉크 분석을 사용하면, 앱이 다음을 포함한 핵심 모양을 인식할 수 있습니다.

- 원
- 다이아몬드
- 그림
- 타원
- 등변 삼각형
- 육각형
- 이등변 삼각형
- 평행 사변형
- 오각형
- 사변형
- 사각형
- 직각 삼각형
- 정사각형
- 사다리꼴
- 삼각형

이 단계에서는 Windows Ink의 모양 인식 기능을 사용하여 낙서를 정리합니다.

이 예를 위해 잉크 스트로크를 다시 그리지는 않습니다(가능하기는 하지만). 대신, InkCanvas 아래 원래 잉크에서 가져온 등가 타원 또는 다각형 개체를 그린 표준 캔버스를 추가합니다. 그런 다음 잉크 스트로크를 삭제합니다.

### <a name="in-the-sample"></a>이 샘플에서:
1. MainPage.xaml 파일을 엽니다.
2. 이 단계의 제목("\<!-- Step 6: Recognize shapes -->")으로 표시된 코드를 찾습니다.
3. 이 줄의 주석 처리를 제거합니다.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. MainPage.xaml.cs 파일을 엽니다.
5. 이 단계의 제목("// Step 6: Recognize shapes")으로 표시된 코드를 찾습니다.
6. 이 줄의 주석 처리를 제거합니다.  

``` csharp
    private async void recognizeShape_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        ...
    }

    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        ...
    }
```

7. 앱을 실행하고, 몇 가지 모양을 그린 다음, **모양 인식** 단추를 클릭합니다.

다음은 디지털 냅킨의 기본적인 순서도의 예입니다.

![원래 잉크 순서도](images/ink/ink-app-step6-shapereco1-small.png)

모양 인식 후 동일한 순서도의 모습은 다음과 같습니다.

![원래 잉크 순서도](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>7단계: 잉크 저장 및 로드

낙서 중에 원하는 내용을 찾았고 나중에 이를 활용하고자 한다면 어떻게 할까요? 잉크 스트로크는 ISF(Ink Serialized Format) 파일로 저장하여 영감이 떠오를 때마다 로드하여 편집할 수 있습니다. 

ISF 파일은 잉크 스트로크 속성과 동작에 대한 추가 메타데이터가 포함된 기본 GIF 이미지입니다. 잉크를 지원하지 않는 앱은 추가 메타데이터를 무시하지만 기본 GIF 이미지(알파 채널 배경 투명도 포함)를 로드할 수는 있습니다.

이 단계에서는 잉크 도구 모음 옆에 있는 **저장** 및 **로드** 단추가 기능을 발휘하도록 해보겠습니다.

### <a name="in-the-sample"></a>이 샘플에서:
1. MainPage.xaml 파일을 엽니다.
2. 이 단계의 제목("\<!-- Step 7: Saving and loading ink -->")으로 표시된 코드를 찾습니다.
3. 다음 줄의 주석 처리를 제거합니다. 

``` xaml
    <Button x:Name="buttonSave" 
            Content="Save" 
            Click="buttonSave_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
    <Button x:Name="buttonLoad" 
            Content="Load"  
            Click="buttonLoad_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
```

4. MainPage.xaml.cs 파일을 엽니다.
5. 이 단계의 제목("// Step 7: Save and load ink")으로 표시된 코드를 찾습니다.
6. 다음 줄의 주석 처리를 제거합니다.  

``` csharp
    private async void buttonSave_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private async void buttonLoad_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }
```

7. 앱을 실행하고 아무것이나 그립니다.
8. **저장** 단추를 눌러 그린 내용을 저장합니다.
9. 잉크를 지우거나 앱을 다시 시작합니다.
10. **로드** 단추를 눌러 저장한 잉크 파일을 엽니다.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>과제: 클립보드를 사용하여 잉크 스트로크를 복사 및 붙여넣기 
<table class="wdg-noborder">
<tr>
<td>

![작업 영역 스케치북의 InkToolbar](images/challenge-icon.png)

</td>

<td>

Windows Ink는 잉크 스트로크를 클립보드에 복사하고 붙여넣는 것도 지원합니다.

잉크와 함께 클립보드를 사용하는 것에 대한 자세한 내용은 [Windows Ink 스트로크 데이터 저장 및 가져오기](save-and-load-ink.md)를 참조하세요.

</td>
</tr>
</table>

## <a name="summary"></a>요약

축하합니다. **입력: UWP 앱에서 잉크 지원** 자습서를 완료했습니다! UWP 앱에서 잉크를 지원하기 위해 필요한 기본 코드를 소개했으며, Windows Ink 플랫폼에서 지원하는 몇 가지 풍부한 사용자 환경을 제공하는 방법을 설명했습니다.

## <a name="related-articles"></a>관련 문서

* [UWP 앱의 펜 조작 및 Windows Ink](pen-and-stylus-interactions.md)

### <a name="samples"></a>샘플

* [잉크 분석 샘플(기본)(C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [잉크 필기 인식 샘플(C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [ISF(Ink Serialized Format) 파일에서 잉크 스트로크 저장 및 로드](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [클립보드에서 잉크 스트로크 저장 및 로드](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [잉크 도구 모음 위치 및 방향 샘플(기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [잉크 도구 모음 위치 및 방향 샘플(동적)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [간단한 잉크 샘플(C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [복잡한 잉크 샘플(C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [잉크 샘플(JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [시작 자습서: UWP 앱에서 잉크 지원](https://aka.ms/appsample-ink)
* [색칠하기 책 샘플](https://aka.ms/cpubsample-coloringbook)
* [가족 메모 샘플](https://aka.ms/cpubsample-familynotessample)
