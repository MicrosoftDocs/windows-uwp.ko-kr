---
author: serenaz
title: Windows ML 시작
description: 이 단계별 자습서를 이용해 첫 번째 Windows ML 앱을 만들어 보세요.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, windows machine learning, winml, windows ML
ms.localizationpriority: medium
ms.openlocfilehash: e30786f775a66bcf5c8e6dce0b4aab4f1f239be6
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816588"
---
# <a name="get-started-with-windows-ml"></a>Windows ML 시작

이 자습서에서는 학습된 기계 학습 모델을 사용하여 사용자가 그린 숫자를 인식하는 단순한 UWP 앱을 빌드할 것입니다. 이 자습서는 기본적으로 앱에서 Windows ML을 로드하여 사용하는 방법을 중점적으로 설명합니다.

## <a name="prerequisites"></a>필수 구성 요소

- [Windows SDK - 빌드 17110+](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)
- [Visual Studio(버전 15.7 - 미리 보기 1)](https://www.visualstudio.com/vs/preview/) 

    **참고**: Visual Studio 설치 관리자에서 선택적 Windows 10 미리 보기 SDK(10.0.17110.0)를 선택해야 합니다.

## <a name="1-download-the-sample"></a>1. 샘플 다운로드

먼저 GitHub에서 [MNIST_GetStarted 샘플](https://github.com/Microsoft/Windows-Machine-Learning)을 다운로드해야 합니다. 다음과 같은 XAML 컨트롤 및 이벤트가 구현된 템플릿을 제공했습니다.

- [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)에서는 숫자를 그립니다.
- [Buttons](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)를 사용하여 숫자를 해석하고 캔버스를 지웁니다.
- 도우미 루틴에서는 InkCanvas 출력을 [VideoFrame](https://docs.microsoft.com/uwp/api/windows.media.videoframe)으로 변환합니다.

완료된 MNIST 샘플을 GitHub에서 다운로드할 수도 있습니다.

## <a name="2-open-project-in-visual-studio-preview"></a>2. Visual Studio 미리 보기에서 프로젝트 열기

Visual Studio 미리 보기를 시작하고 MNIST 샘플 응용 프로그램을 엽니다. (솔루션이 사용 불가능으로 표시되면 마우스 오른쪽 단추를 클릭하여 "프로젝트 다시 로드"를 선택해야 합니다.)

솔루션 탐색기에서 프로젝트에는 3개의 기본 코드 파일이 있습니다.

- `MainPage.xaml` - InkCanvas, 단추 및 레이블에 대한 UI를 만드는 모든 XAML 코드가 있습니다.
- `MainPage.xaml.cs` - 응용 프로그램 코드가 있는 위치 정보를 포함합니다.
- `Helper.cs` - 이미지 형식을 자르고 변환하는 도우미 루틴이 있습니다.

![프로젝트 파일이 있는 Visual Studio 솔루션 탐색기](images/get-started1.png)

## <a name="3-build-and-run-project"></a>3. 프로젝트 빌드 및 실행

로컬 컴퓨터에서 프로젝트를 실행할 수 있도록 Visual Studio 도구 모음에서 솔루션 플랫폼을 "ARM"에서 "x64"로 변경합니다.

프로젝트를 실행하려면 도구 모음에서 **디버깅 시작** 단추를 클릭하거나 F5를 누릅니다. 응용 프로그램에서는 사용자가 숫자를 쓸 수 있는 InkCanvas, 숫자를 해석하는 "인식" 단추, 해석된 숫자가 텍스트로 표시될 빈 레이블 필드 및 InkCanvas를 지우는 "숫자 지우기" 단추를 표시해야 합니다.

![응용 프로그램 스크린샷](images/get-started2.png)

**참고**: 17110보다 높은 빌드를 다운로드한 경우 프로젝트의 배포 대상 버전을 변경해야 할 수 있습니다. 프로젝트 솔루션에서 **속성**으로 이동합니다. 응용 프로그램 탭에서 OS와 SDK가 일치하도록 대상 버전을 설정합니다.

## <a name="4-download-a-model"></a>4. 모델 다운로드

다음으로 앱에 추가할 기계 학습 모델을 가져오겠습니다. 이 자습서에서는 미리 학습된 **MNIST 모델**을 사용합니다. 이 모델은 [Microsoft Cognitive Toolkit(CNTK)](https://docs.microsoft.com/cognitive-toolkit/)을 사용하여 학습되었으며 [ONNX 형식으로 내보내졌습니다](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb).

GitHub의 MNIST_GetStarted 샘플을 사용하는 경우 MNIST 모델은 이미 자산 폴더에 포함되어 있으며 이를 기존 항목으로 응용 프로그램에 추가해야 합니다. GitHub의 [ONNX Model Zoo](https://github.com/onnx/models)에서 미리 학습된 모델을 다운로드할 수도 있습니다.

모델 학습에 관심이 있는 경우 이 MNIST 모델을 학습하는 데 사용한 [자습서](train-ai-model.md)를 이용할 수 있습니다.

## <a name="5-add-the-model"></a>5. 모델 추가

MNIST 모델을 다운로드한 후 솔루션 탐색기에서 자산 폴더를 마우스 오른쪽 단추로 클릭하고 "**추가** > **기존 항목**"을 선택합니다. 파일 선택기에서 ONNX 모델 위치를 선택한 후 추가를 클릭합니다. 

이제 프로젝트에 다음 두 개의 새 파일이 있어야 합니다.

- `MNIST.onnx` - 학습된 모델입니다.
- `MNIST.cs` - Windows ML 생성 코드입니다.

![새 파일이 있는 솔루션 탐색기](images/get-started3.png)

응용 프로그램을 컴파일할 때 모델이 빌드되도록 하려면 `mnist.onnx` 파일을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **빌드 작업**에 **콘텐츠**를 선택합니다.

이제 `MNIST.cs` 파일에서 새로 생성된 코드를 살펴보겠습니다. 다음 세 개의 클래스가 있습니다.

- **MNISTModel**은 기계 학습 모델 프리젠테이션을 만들고 특정 입력과 출력을 모델에 바인딩하며 모델을 비동기적으로 평가합니다. 
- **MNISTModelInput**은 모델의 예상 입력 유형을 초기화합니다. 이 경우 입력은 VideoFrame을 예상합니다.
- **MNISTModelOutput**은 모델이 출력될 유형을 초기화합니다. 이 경우 출력은 유형이 `<long>`인 "classLabel" 목록 및 다음 유형의 "prediction" 사전이 됩니다. `<long, float>`

이제 이러한 클래스를 사용하여 프로젝트에서 모델을 로드, 바인딩 및 평가합니다.

## <a name="6-load-bind-and-evaluate-the-model"></a>6. 모델 로드, 바인딩 및 평가

Windows ML 응용 프로그램의 경우 이용하려는 패턴은 로드 > 바인딩 > 평가입니다.

- 기계 학습 모델을 로드합니다.
- 입력 및 출력을 모델에 바인딩합니다.
- 모델 및 보기 결과를 평가합니다.

`MNIST.cs`에 생성된 인터페이스 코드를 사용하여 앱에서 모델을 로드, 바인딩 및 평가합니다.

먼저 `MainPage.xaml.cs`에서 모델, 입력 및 출력을 인스턴스화합니다.

```csharp
namespace MNIST_Demo
{
    public sealed partial class MainPage : Page
    {
        private MNISTModel ModelGen = new MNISTModel();
        private MNISTModelInput ModelInput = new MNISTModelInput();
        private MNISTModelOutput ModelOutput = new MNISTModelOutput();
        ...
    }
}
```

그런 다음 LoadModel()에서 모델을 로드합니다.

```csharp
private async void LoadModel()
{
    //Load a machine learning model
    StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
    ModelGen = await MNISTModel.CreateMNISTModel(modelFile);
}
```

다음으로 입력 및 출력을 모델에 바인딩하려고 합니다. 

이 경우 모델의 예상 입력 유형은 VideoFrame입니다. `helper.cs`에 포함된 도우미 기능을 사용하여 InkCanvas의 콘텐츠를 복사하고 이를 VideoFrame 유형으로 변환한 후 모델에 바인딩합니다.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
     //Bind model input with contents from InkCanvas
     ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
}
```

출력의 경우 단순히 지정된 입력으로 EvaluateAsync()를 호출합니다. 모델은 해당 확률을 가진 숫자 목록을 반환하기 때문에 가능성이 가장 높은 숫자를 판별하고 해당 숫자를 표시하기 위해 반환된 목록을 구문 분석해야 합니다.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
    //Bind model input with contents from InkCanvas
    ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
    
    //Evaluate the model
    ModelOutput = await ModelGen.EvaluateAsync(ModelInput);
            
    //Iterate through evaluation output to determine highest probability digit
    float maxProb = 0;
    int maxIndex = 0;
    for (int i = 0; i < 10; i++)
    {
        if (ModelOutput.Plus214_Output_0[i] > maxProb)
        {
            maxIndex = i;
            maxProb = ModelOutput.Plus214_Output_0[i];
        }
    }
    numberLabel.Text = maxIndex.ToString();
}
```

마지막으로 사용자가 다른 숫자를 그릴 수 있도록 InkCanvas를 정리하려고 합니다.

```csharp
private void clearButton_Click(object sender, RoutedEventArgs e)
{
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    numberLabel.Text = "";
}
```

## <a name="7-launch-the-app"></a>7. 앱 실행

앱을 빌드하고 실행하면 InkCanvas에 그린 숫자를 인식할 수 있습니다.

![앱 완료](images/get-started4.png)

## <a name="8-next-steps"></a>8. 다음 단계

이것이 전부입니다. 당신의 첫 번째 Windows ML 앱을 만들었습니다! Windows ML의 사용 방법을 설명하는 더 많은 샘플을 보려면 [샘플 앱](samples.md)을 확인하세요.