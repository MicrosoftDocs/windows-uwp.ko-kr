---
ms.assetid: ''
title: Windows 앱에서 잉크 지원
description: Windows 앱에 잉크 지원을 추가 하는 방법에 대 한 단계별 자습서입니다.
keywords: 잉크, 잉크, tuorial
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d0df2b531510d86591c44bc69f6ed5c6ad9f200f
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234620"
---
# <a name="tutorial-support-ink-in-your-windows-app"></a>자습서: Windows 앱에서 잉크 지원

![화면 펜](images/ink/ink-hero-small.png)  
*Surface Pen* ( [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)에서 구매할 수 있음)

이 자습서에서는 Windows Ink를 사용 하 여 작성 및 그리기를 지 원하는 기본 Windows 앱을 만드는 방법을 단계별로 설명 합니다. 각 단계에서 설명한 다양 한 기능 및 관련 Windows Ink Api ( [Windows ink 플랫폼의 구성 요소](#components-of-the-windows-ink-platform)참조)를 보여 주기 위해 GitHub에서 다운로드할 수 있는 샘플 앱에서 코드 조각을 사용 합니다 ( [샘플 코드](#sample-code)참조).

다음에 중점을 둡니다.
* 기본 잉크 지원 추가
* 잉크 도구 모음 추가
* 필기 인식 지원
* 기본 도형 인식 지원
* 잉크 저장 및 로드

이러한 기능을 구현 하는 방법에 대 한 자세한 내용은 [windows 앱의 펜 상호 작용 및 Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions)를 참조 하세요.

## <a name="introduction"></a>소개

Windows 잉크를 사용 하면 신속 하 고 필기 한 노트와 주석에서 화이트 보드 데모로, 아키텍처 및 엔지니어링 드로잉부터 개인 masterpieces에 이르기까지 거의 모든 펜 및 종이 환경 에서도을 고객에 게 제공할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

* 현재 버전의 Windows 10을 실행 하는 컴퓨터 (또는 가상 컴퓨터)
* [Visual Studio 2019 및 RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK(10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 구성에 따라 [Microsoft.netcore.universalwindowsplatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform) NuGet 패키지를 설치 하 고 시스템 설정에서 **개발자 모드** 를 사용 하도록 설정 해야 할 수 있습니다. 개발자는 개발자 기능을 사용 하 여 개발자 기능을 사용 하 여 > & 보안-> > 업데이트 합니다.
* Visual Studio를 사용 하 여 Windows 앱을 개발 하는 경우이 자습서를 시작 하기 전에 다음 항목을 참조 하세요.  
    * [설정](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * ["Hello, 세계" 앱 만들기 (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* **[선택 사항]** 디지털 펜 및 해당 디지털 펜의 입력을 지 원하는 디스플레이를 포함 하는 컴퓨터입니다.

> [!NOTE] 
> Windows Ink는 마우스 및 터치를 사용 하 여 그리기를 지원할 수 있지만 (이 자습서의 3 단계에서이 작업을 수행 하는 방법을 보여 줍니다) 최적의 Windows Ink 환경을 위해 디지털 펜과 디지털 펜의 입력을 지 원하는 디스플레이가 있는 컴퓨터를 사용 하는 것이 좋습니다.

## <a name="sample-code"></a>샘플 코드
이 자습서 전체에서 샘플 잉크 앱을 사용 하 여 설명 된 개념과 기능을 보여 줍니다.

[GitHub](https://github.com/) 에서이 Visual Studio 샘플 및 소스 코드 다운로드 [-appsample-시작-잉크 샘플](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink):

1. 녹색 **복제 또는 다운로드** 단추를 선택 합니다.  
![리포지토리 복제](images/ink/ink-clone.png)
2. GitHub 계정이 있는 경우 **Visual Studio에서 열기** 를 선택 하 여 로컬 컴퓨터에 리포지토리를 복제할 수 있습니다. 
3. GitHub 계정이 없거나 프로젝트의 로컬 복사본을 원하는 경우 **ZIP 다운로드** 를 선택 합니다. 최신 업데이트를 다운로드 하려면 정기적으로 다시 확인 해야 합니다.

> [!IMPORTANT]
> 샘플에서 대부분의 코드는 주석 처리 되어 있습니다. 각 단계를 진행할 때 코드의 다양 한 섹션에 대 한 주석 처리를 제거 하 라는 메시지가 표시 됩니다. Visual Studio에서 코드 줄을 강조 표시 하 고 CTRL + K, CTRL + U를 차례로 누릅니다.

## <a name="components-of-the-windows-ink-platform"></a>Windows Ink 플랫폼의 구성 요소

이러한 개체는 Windows 앱에 대 한 대부분의 잉크를 제공 합니다.

| 구성 요소 | Description |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) | 기본적으로 펜의 모든 입력을 받아 잉크 스트로크 또는 지우기 스트로크로 표시 하는 XAML UI 플랫폼 컨트롤입니다. |
| [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) | [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤([**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) 속성을 통해 노출)과 함께 인스턴스화되는 코드 숨김 개체입니다. 이 개체는 추가 사용자 지정 및 개인 설정을 위한 포괄적인 Api 집합과 함께 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)에서 노출 하는 모든 기본 잉크 기능을 제공 합니다. |
| [**InkToolbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) | 연결 된 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)의 잉크 관련 기능을 활성화 하는 사용자 지정 가능 하 고 확장 가능한 단추 컬렉션을 포함 하는 XAML UI 플랫폼 컨트롤입니다. |
| [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)<br/>여기서는이 기능에 대해 다루지 않습니다. 자세한 내용은 [복합 잉크 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)을 참조 하세요. | 기본 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤이 아니라 유니버설 Windows 앱의 지정 된 Direct2D 장치 컨텍스트로 잉크 스트로크를 렌더링할 수 있습니다. |

## <a name="step-1-run-the-sample"></a>1 단계: 샘플 실행

RadialController 샘플 앱을 다운로드 한 후 실행 되는지 확인 합니다.
1. Visual Studio에서 샘플 프로젝트를 엽니다.
2. **솔루션 플랫폼** 드롭다운을 ARM이 아닌 선택 항목으로 설정 합니다.
3. F5 키를 눌러 컴파일, 배포 및 실행 합니다.  

   > [!NOTE]
   > 또는 **디버그**  >  **디버깅 시작** 메뉴 항목을 선택 하거나 여기에 표시 된 **로컬 컴퓨터** 실행 단추를 선택할 수 있습니다.
   > ![Visual Studio 빌드 프로젝트 단추](images/ink/ink-vsrun-small.png)

앱 창이 열리고 몇 초 동안 시작 화면이 표시 되 면이 초기 화면이 표시 됩니다.

![빈 앱](images/ink/ink-app-step1-empty-small.png)

이제이 자습서의 나머지 부분에서 사용할 기본 Windows 앱이 있습니다. 다음 단계에서는 잉크 기능을 추가 합니다.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>2 단계: InkCanvas를 사용 하 여 기본 잉크를 지원할 수 있습니다.

응용 프로그램의 초기 형태에서 앱이 앱과 상호 작용 하는 표준 포인터 장치로 사용 될 수 있지만 펜을 사용 하 여 아무것도 그릴 수 없다는 것을 알고 있을 것입니다. 

이 단계에서는 약간의 단점을 해결 해 보겠습니다.

기본 잉크 기능을 추가 하려면 앱의 해당 페이지에 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤을 추가 하면 됩니다.

> [!NOTE]
> InkCanvas의 기본 [**높이**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) 및 [**너비**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width) 속성은 자식 요소의 크기를 자동으로 조정 하는 요소의 자식인 경우를 제외 하 고는 0입니다. 

### <a name="in-the-sample"></a>이 샘플의 내용은 다음과 같습니다.
1. MainPage.xaml.cs 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드를 찾습니다 ("//2 단계: InkCanvas를 사용 하 여 기본 잉크를 지원 합니다.").
3. 다음 줄의 주석 처리를 제거 합니다. 이러한 참조는 이후 단계에서 사용 되는 기능에 필요 합니다.  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. MainPage .xaml 파일을 엽니다.
5. 이 단계의 제목으로 표시 된 코드를 찾습니다 (" \< !--2 단계: 기본 잉크를 사용 하 여 InkCanvas-->").
6. 다음 줄의 주석 처리를 제거 합니다.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

간단하죠. 

이제 앱을 다시 실행합니다. 계속 해 서 자유롭게 이동 하거나 이름을 작성 하거나 (미러를 보유 하거나 매우 좋은 메모리가 있는 경우) 자체 세로를 그립니다.

![기본 잉크](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>3 단계: 터치 및 마우스로 잉크 입력 지원

기본적으로 잉크는 펜 입력에 대해서만 지원 됩니다. 손가락, 마우스 또는 터치 패드를 사용 하 여 쓰거나 그리면 실망 됩니다.

이 찡그린 얼굴 보내기 반대로 전환 하려면 두 번째 코드 줄을 추가 해야 합니다. 이번에는 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)를 선언한 XAML 파일에 대 한 코드 숨김이 있습니다. 

이 단계에서는 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)의 입력, 처리 및 잉크 입력의 렌더링 (표준 및 수정 됨)에 대 한 세부적인 관리를 제공 하는 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) 개체를 소개 합니다.

> [!NOTE]
> 표준 잉크 입력 (펜 팁 또는 지우개 팁/단추)은 펜 배럴 단추, 오른쪽 마우스 단추 또는 유사한 메커니즘과 같은 보조 하드웨어 affordance 수정 되지 않습니다. 

마우스 및 터치 잉크를 사용 하도록 설정 하려면 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) 의 [**inputdevicetypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) 속성을 원하는 [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) 값의 조합으로 설정 합니다.

### <a name="in-the-sample"></a>이 샘플의 내용은 다음과 같습니다.
1. MainPage.xaml.cs 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드를 찾습니다 ("//3 단계: 터치 및 마우스로 잉크 지원 지원").
3. 다음 줄의 주석 처리를 제거 합니다.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

앱을 다시 실행 하면 모든 손가락 그리기-컴퓨터 화면 꿈을 실현이 참 임을 알 수 있습니다.

> [!NOTE]
> 입력 장치 유형을 지정 하는 경우이 속성을 설정 하면 기본 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 설정을 재정의 하기 때문에 각 특정 입력 유형 (펜 포함)에 대 한 지원을 나타내야 합니다.

## <a name="step-4-add-an-ink-toolbar"></a>4 단계: 잉크 도구 모음 추가

[**Inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 는 잉크 관련 기능을 활성화 하는 데 사용할 수 있는 사용자 지정 가능 하 고 확장 가능한 단추 컬렉션을 제공 하는 UWP 플랫폼 컨트롤입니다. 

기본적으로 [**Inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 에는 사용자가 펜, 연필, 형광펜 또는 지우개 중에서 빠르게 선택할 수 있는 기본 단추 집합이 포함 되어 있습니다 .이 단추를 스텐실 (눈금자 또는 protractor)과 함께 사용할 수 있습니다. 펜, 연필 및 형광펜 단추는 각각 잉크 색 및 스트로크 크기를 선택 하기 위한 플라이 아웃을 제공 합니다.

잉크 앱에 기본 [**Inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 를 추가 하려면 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 와 동일한 페이지에 놓고 두 컨트롤을 연결 하면 됩니다.

### <a name="in-the-sample"></a>샘플의
1. MainPage .xaml 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드 (" \< !--4 단계: 잉크 도구 모음 추가-->")를 찾습니다.
3. 다음 줄의 주석 처리를 제거 합니다.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> UI와 코드를 최대한 간결 하 고 간단 하 게 유지 하기 위해 기본 그리드 레이아웃을 사용 하 고 표 형태 행의 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 뒤에 [**inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 를 선언 합니다. [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)이전에 선언 하는 경우에는 [**inktoolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 가 캔버스 아래에 먼저 렌더링 되 고 사용자가 액세스할 수 없게 됩니다.  

이제 앱을 다시 실행 하 여 [**Inktoolbar 모음**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 을 확인 하 고 일부 도구를 사용해 보세요.

![잉크 작업 영역을 Sketchpad 하는 InkToolbar](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>챌린지: 사용자 지정 단추 추가
<table class="wdg-noborder">
<tr>
<td>

![잉크 작업 영역에 있는 Sketchpad의 InkToolbar](images/challenge-icon.png)

</td>
<td>

다음은 Windows Ink 작업 영역의 Sketchpad에서 사용자 지정 **[Inktoolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)** 의 예제입니다.

![잉크 작업 영역에 있는 Sketchpad의 InkToolbar](images/ink/ink-inktoolbar-sketchpad-small.png)

[Inktoolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)를 사용자 지정 하는 방법에 대 한 자세한 내용은 [Windows 앱 잉크 앱에 Inktoolbar 모음 추가](ink-toolbar.md)를 참조 하세요.

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>5 단계: 필기 인식 지원

이제 앱에서 쓰고 그릴 수 있으므로 이러한 낙서에 유용한 작업을 수행해 보겠습니다.

이 단계에서는 Windows Ink의 필기 인식 기능을 사용 하 여 작성 한 내용을 해독 하려고 시도 합니다.

> [!NOTE]
> **펜 & Windows Ink** 설정에서 필기 인식을 향상 시킬 수 있습니다.
> 1. 시작 메뉴를 열고 **설정**을 선택 합니다.
> 2. 설정 화면에서 **장치**  >  **펜 & Windows Ink**를 선택 합니다.
> ![잉크 작업 영역에 있는 Sketchpad의 InkToolbar](images/ink/ink-settings-small.png)
> 3. 필기 **개인 설정** 대화 상자를 열려면 **가져오기를 선택 하 여 필기를 확인** 합니다.
> ![잉크 작업 영역에 있는 Sketchpad의 InkToolbar](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>이 샘플의 내용은 다음과 같습니다.
1. MainPage .xaml 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드를 찾습니다 (" \< !--5 단계: 필기 인식 지원-->").
3. 다음 줄의 주석 처리를 제거 합니다.  

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
5. 이 단계의 제목으로 표시 된 코드를 찾습니다 ("5 단계: 필기 인식 지원").
6. 다음 줄의 주석 처리를 제거 합니다.  

- 이 단계에 필요한 전역 변수입니다.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- 인식 처리를 수행 하는 **텍스트 인식** 단추에 대 한 처리기입니다.

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

7. 앱을 다시 실행 하 고, 내용을 쓴 다음, **텍스트 인식** 단추를 클릭 합니다.
8. 인식 결과는 단추 옆에 표시 됩니다.

### <a name="challenge-1-international-recognition"></a>챌린지 1: 국제 인식
<table class="wdg-noborder">
<tr>
<td>

![잉크 작업 영역에 있는 Sketchpad의 InkToolbar](images/challenge-icon.png)

</td>
<td>

Windows 잉크는 Windows에서 지 원하는 대부분의 언어에 대 한 텍스트 인식을 지원 합니다. 각 언어 팩에는 언어 팩과 함께 설치할 수 있는 필기 인식 엔진이 포함 되어 있습니다.

설치 된 필기 인식 엔진을 쿼리하여 특정 언어를 대상으로 지정 합니다.

국가별 필기 인식에 대 한 자세한 내용은 [Windows 잉크 스트로크를 텍스트로 인식](convert-ink-to-text.md)을 참조 하세요.

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>챌린지 2: 동적 인식
<table class="wdg-noborder">
<tr>
<td>

![잉크 작업 영역에 있는 Sketchpad의 InkToolbar](images/challenge-icon.png)

</td>
<td>

이 자습서에서는 인식을 시작 하기 위해 단추를 눌러야 합니다. 기본 타이밍 함수를 사용 하 여 동적 인식을 수행할 수도 있습니다.

동적 인식에 대 한 자세한 내용은 [Windows 잉크 스트로크를 텍스트로 인식](convert-ink-to-text.md)을 참조 하세요.

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>6 단계: 도형 인식

이제 필기 메모를 좀 더 읽기 쉽게 변환할 수 있습니다. 그러나 이러한 흔들리는에 대 한 정보는 아침 Flowcharters 익명 회의의 caffeinated doodles?

잉크 분석을 사용 하 여 앱은 다음을 비롯 한 일련의 핵심 셰이프를 인식할 수도 있습니다.

- Circle
- Diamond
- 그리기
- 타원
- EquilateralTriangle
- 육각형
- IsoscelesTriangle
- 평행 사변형
- 오각형
- 사변형
- 사각형
- RightTriangle
- Square
- 사다리꼴
- Triangle

이 단계에서는 Windows Ink의 모양 인식 기능을 사용 하 여 doodles를 정리 합니다.

이 예제에서는 잉크 스트로크를 다시 그리도록 시도 하지 않습니다 (가능한 경우). 대신 원본 잉크에서 파생 된 동등한 타원 또는 Polygon 개체를 그리는 InkCanvas 아래에 표준 캔버스를 추가 합니다. 그런 다음 해당 잉크 스트로크를 삭제 합니다.

### <a name="in-the-sample"></a>이 샘플의 내용은 다음과 같습니다.
1. MainPage .xaml 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드 찾기 (" \< !--6 단계: 셰이프 인식-->")
3. 이 줄의 주석 처리를 제거 합니다.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. MainPage.xaml.cs 파일을 엽니다.
5. 이 단계의 제목으로 표시 된 코드 찾기 ("//6 단계: 인식 셰이프")
6. 다음 줄의 주석 처리를 제거 합니다.  

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

7. 앱을 실행 하 고 일부 셰이프를 그린 다음 **인식 셰이프** 단추를 클릭 합니다.

디지털 napkin의 기초적인 순서도 예제는 다음과 같습니다.

![원본 잉크 순서도](images/ink/ink-app-step6-shapereco1-small.png)

셰이프 인식 후의 순서도는 다음과 같습니다.

![원본 잉크 순서도](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>7 단계: 잉크 저장 및 로드

Doodling 완료 되 고 표시 되는 것을 알고 싶으세요? 나중에 몇 가지를 조정 하는 것이 좋습니다. 잉크 스트로크를 ISF (Serialize 된 형식) 파일에 저장 하 고, 해당 항목을 편집 하기 위해 로드할 수 있습니다. 

ISF 파일은 잉크 스트로크 속성 및 동작을 설명 하는 추가 메타 데이터를 포함 하는 기본 GIF 이미지입니다. 잉크를 사용 하도록 설정 되지 않은 앱은 추가 메타 데이터를 무시 하 고 여전히 기본 GIF 이미지 (알파 채널 배경 투명도 포함)를 로드할 수 있습니다.

이 단계에서는 잉크 도구 모음 옆에 있는 **저장** 및 **로드** 단추를 연결 합니다.

### <a name="in-the-sample"></a>이 샘플의 내용은 다음과 같습니다.
1. MainPage .xaml 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드를 찾습니다 (" \< !--7 단계: 잉크 저장 및 로드 중-->").
3. 다음 줄의 주석 처리를 제거 합니다. 

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
5. 이 단계의 제목으로 표시 된 코드 ("//7 단계: 잉크 저장 및 로드")를 찾습니다.
6. 다음 줄의 주석 처리를 제거 합니다.  

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

7. 앱을 실행 하 고 항목을 그립니다.
8. **저장** 단추를 선택 하 고 드로잉을 저장 합니다.
9. 잉크를 지우거 나 앱을 다시 시작 합니다.
10. **로드** 단추를 선택 하 고 방금 저장 한 잉크 파일을 엽니다.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>챌린지: 클립보드를 사용 하 여 잉크 스트로크를 복사 하 고 붙여 넣습니다. 
<table class="wdg-noborder">
<tr>
<td>

![잉크 작업 영역에 있는 Sketchpad의 InkToolbar](images/challenge-icon.png)

</td>

<td>

Windows ink는 잉크 스트로크를 복사 하 여 클립보드에 붙여 넣는 기능도 지원 합니다.

잉크에 클립보드를 사용 하는 방법에 대 한 자세한 내용은 [Windows 잉크 스트로크 데이터 저장 및 검색](save-and-load-ink.md)을 참조 하세요.

</td>
</tr>
</table>

## <a name="summary"></a>요약

축 하 합니다 **. Windows 앱 자습서에서 입력: 지원 잉크** 를 완료 했습니다. Windows 앱에서 잉크를 지 원하는 데 필요한 기본 코드와 Windows Ink 플랫폼에서 지원 되는 다양 한 사용자 환경을 제공 하는 방법을 살펴보았습니다.

## <a name="related-articles"></a>관련된 문서

* [Windows 앱의 펜 조작 및 Windows Ink](pen-and-stylus-interactions.md)

### <a name="samples"></a>샘플

* [잉크 분석 샘플 (기본) (c #)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [잉크 필기 인식 샘플 (c #)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [ISF (Ink Serialize 된 형식) 파일에서 잉크 스트로크를 저장 하 고 로드 합니다.](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [클립보드에서 잉크 스트로크 저장 및 로드](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [잉크 도구 모음 위치 및 방향 샘플 (기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [잉크 도구 모음 위치 및 방향 샘플 (동적)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [단순 잉크 샘플 (c #/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [복합 잉크 샘플 (c + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ink 샘플 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [자습서 시작: Windows 앱에서 잉크 지원](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [서적 샘플 강조](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [제품군 정보 샘플](https://github.com/Microsoft/Windows-appsample-familynotes)
