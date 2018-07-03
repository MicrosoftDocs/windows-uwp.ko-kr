---
author: serenaz
title: Windows ML을 사용하여 앱에 모델 통합
description: 로드, 바인딩 및 평가 패턴을 따라 앱에 모델을 통합합니다.
ms.author: sezhen
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, winml, Windows machine learning
ms.localizationpriority: medium
ms.openlocfilehash: 8631c07b45a8642a1de700e424d3ca4f147456fc
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842580"
---
# <a name="integrate-a-model-into-your-app-with-windows-ml"></a>Windows ML을 사용하여 앱에 모델 통합

Windows ML의 [자동 코드 생성](overview.md#automatic-interface-code-generation)은 사용자가 모델과 쉽게 상호 작용할 수 있는 [Windows ML API](/uwp/api/windows.ai.machinelearning.preview)를 호출하는 인터페이스를 만듭니다. 인터페이스에 생성된 래퍼 클래스를 사용하여 로드, 바인딩 및 평가 패턴으로 ML 모델을 앱에 통합합니다.

![로드, 바인딩, 평가](images/load-bind-evaluate.png)

이 문서에서는 [시작](get-started.md)의 MNIST 모델을 사용하여 앱에서 모델을 로드, 바인딩 및 평가하는 방법에 대해 설명합니다.

## <a name="add-the-model"></a>모델 추가

먼저 ONNX 모델을 Visual Studio 프로젝트의 자산에 추가해야 합니다. [Visual Studio(버전 15.7 - 미리 보기 1)](https://www.visualstudio.com/vs/preview/)를 사용하여 UWP 앱을 빌드하는 경우 Visual Studio는 자동으로 래퍼 클래스를 새 파일로 생성합니다. 다른 워크플로에서는 [mlgen](overview.md#automatic-interface-code-generation) 도구를 사용하여 래퍼 클래스를 생성해야 합니다.

다음은 MNIST 모델에 대한 Windows ML 생성 래퍼 클래스입니다. 이 문서의 나머지 부분에서는 앱에서 이러한 클래스를 사용하는 방법에 대해 설명합니다.

```csharp
public sealed class MNISTModelInput
{
    public VideoFrame Input3 { get; set; }
}

public sealed class MNISTModelOutput
{
    public IList<float> Plus214_Output_0 { get; set; }
    public MNISTModelOutput()
    {
        this.Plus214_Output_0 = new List<float>();
    }
}

public sealed class MNISTModel
{
    private LearningModelPreview learningModel;
    public static async Task<MNISTModel> CreateMNISTModel(StorageFile file)
    {
        LearningModelPreview learningModel = await LearningModelPreview.LoadModelFromStorageFileAsync(file);
        MNISTModel model = new MNISTModel();
        model.learningModel = learningModel;
        return model;
    }
    public async Task<MNISTModelOutput> EvaluateAsync(MNISTModelInput input) {
        MNISTModelOutput output = new MNISTModelOutput();
        LearningModelBindingPreview binding = new LearningModelBindingPreview(learningModel);
        binding.Bind("Input3", input.Input3);
        binding.Bind("Plus214_Output_0", output.Plus214_Output_0);
        LearningModelEvaluationResultPreview evalResult = await learningModel.EvaluateAsync(binding, string.Empty);
        return output;
    }
}
```

**참고**: 응용 프로그램을 컴파일할 때 모델이 빌드되도록 하려면 `.onnx` 파일을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **빌드 작업**에 **콘텐츠**를 선택합니다.

## <a name="load"></a>로드

래퍼 클래스를 생성하면 앱에서 모델을 로드합니다.

MNISTModel 클래스는 MNIST 모델을 나타내며, 모델을 로드하기 위해 ONNX 파일에서 매개 변수로 전달되는 CreateMNISTModel 메서드를 호출합니다.

```csharp
// Load the model
StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
MNISTModel model = MNISTModel.CreateMNISTModel(modelFile);
```

## <a name="bind"></a>바인딩

생성된 코드에 입력 및 출력 래퍼 클래스도 포함됩니다. 입력 클래스는 모델의 예상 입력을 나타내며 출력 클래스는 모델의 예상 출력을 나타냅니다.

모델의 입력 개체를 초기화하려면 응용 프로그램 데이터에 전달되는 입력 클래스 생성자를 호출하고 입력 데이터가 모델에서 예상하는 입력 유형과 일치하는지 확인합니다. MNISTModelInput 클래스는 VideoFrame을 예상하므로 도우미 메서드를 사용하여 입력을 위해 VideoFrame을 가져옵니다.

```csharp
//Bind the input data to the model
MNISTModelInputs ModelInput = new MNISTModelInputs();
ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
```

출력 개체는 *Null* 값으로 초기화되며 다음 평가 단계 이후에 모델의 결과로 업데이트됩니다.

## <a name="evaluate"></a>평가

입력이 초기화되면 모델의 EvaluateAsync 메서드를 호출하여 입력 데이터에서 모델을 평가합니다. EvaluateAsync는 입력과 출력을 모델 개체에 바인딩하고 입력에서 모델을 평가합니다.

```csharp
// Evaluate the model
MNISTModelOuput ModelOutput = model.EvaluateAsync(ModelInput);
```

평가 후, 출력에는 보고 분석할 수 있는 모델의 결과가 포함됩니다. MNIST 모델은 확률 목록을 출력하므로 목록을 반복하여 확률이 가장 높은 숫자를 찾아서 표시합니다.

```csharp
 //Iterate through output to determine highest probability digit
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
```

## <a name="using-the-windows-ml-apis-directly"></a>Windows ML API 직접 사용

Windows ML의 자동 코드 생성기는 모델과 상호 작용할 수 있는 편리한 인터페이스를 제공하지만 반드시 래퍼 클래스를 사용할 필요는 없습니다. 대신 앱에서 Windows ML API를 직접 호출할 수 있습니다.
직접 호출하도록 선택하는 경우에도 모델을 로드하고 입력 및 출력을 바인딩하고 평가하는 동일한 패턴을 따릅니다.

API 사용에 대한 자세한 내용은 [Windows ML API 참조](/uwp/api/windows.ai.machinelearning.preview)를 참조하세요.
