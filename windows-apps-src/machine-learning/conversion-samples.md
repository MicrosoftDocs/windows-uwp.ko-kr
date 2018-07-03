---
author: wschin
title: 기존 ML을 ONNX로 변환
description: 코드 샘플은 WinMLTools를 사용하여 scikit-learn 및 Core ML의 기존 모델을 ONNX 형식으로 변환합니다.
ms.author: sezhen
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, windows machine learning, WinML, WinMLTools, ONNX, ONNXMLTools, scikit-learn, Core ML
ms.localizationpriority: medium
ms.openlocfilehash: 7b2e9c8b661ccd2b0358882992da6c4f160b49f0
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843003"
---
# <a name="convert-existing-ml-models-to-onnx"></a>기존 ML을 ONNX로 변환

[WinMLTools](https://pypi.org/project/winmltools/)를 사용하여 다른 프레임워크에서 학습된 모델을 ONNX 형식으로 변환할 수 있습니다. 여기서는 WinMLTools 패키지를 설치하는 방법과 Python 코드를 통해 scikit-learn 및 Core ML의 기존 모델을 ONNX로 변환하는 방법에 대해 설명합니다.

## <a name="install-winmltools"></a>WinMLTools 설치

WinMLTools는 Python 버전 2.7 및 3.6을 지원하는 Python 패키지(winmltools)입니다. 데이터 과학 프로젝트에서 작업하는 경우 Anaconda와 같은 Scientific Python 배포를 설치하는 것이 좋습니다.

WinMLTools는 [표준 Python 패키지 설치 프로세스](https://packaging.python.org/installing/)를 따릅니다. 사용자 Python 환경에서 다음을 실행합니다.

```
pip install winmltools
```

WinMLTools에는 다음 종속 항목이 있습니다.

- numpy v1.10.0+
- onnxmltools 0.1.0.0+
- protobuf v.3.1.0+

종속 패키지를 업데이트하려면 ‘-U’ 인수와 함께 pip 명령을 실행합니다.

```
pip install -U winmltools
```

onnxmltools 종속 항목에 대한 자세한 내용은 GitHub의 [onnxmltools](https://github.com/onnx/onnxmltools)를 따르세요.

WinMLTools 사용 방법에 대한 추가 세부 정보는 도움말 기능이 있는 패키지 특정 설명서에서 찾을 수 있습니다.

```
help(winmltools)
```

> [!NOTE]
> Visual Studio Tools for AI 확장으로 Visual Studio IDE 내에서 WinMLTools를 사용하여 더 친숙하고 클릭이 용이한 환경에서 모델을 ONNX 형식으로 변환할 수 있습니다. 자세한 정보는 [VS Tools for AI](https://github.com/Microsoft/vs-tools-for-ai/)를 참조하세요.

## <a name="scikit-learn-models"></a>Scikit-learn 모델

다음 코드 스니펫은 scikit-learn에서 선형 지원 벡터 머신을 학습하고 모델을 ONNX로 변환합니다.

~~~python
# First, we create a toy data set.
# The training matrix X contains three examples, with two features each.
# The label vector, y, stores the labels of all examples.
X = [[0.5, 1.], [-1., -1.5], [0., -2.]]
y = [1, -1, -1]

# Then, we create a linear classifier and train it.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()
linear_svc.fit(X, y)

# To convert scikit-learn models, we need to specify the input feature's name and type for our converter. 
# The following line means we have a 2-D float feature vector, and its name is "input" in ONNX.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
linear_svc_onnx = convert_sklearn(linear_svc, name='LinearSVC', input_features=[('input', 'float', 2)])    

# Now, we save the ONNX model into binary format.
from winmltools.utils import save_model
save_model(linear_svc_onnx, 'linear_svc.onnx')

# If you'd like to load an ONNX binary file, our tool can also help.
from winmltools.utils import load_model
linear_svc_onnx = load_model('linear_svc.onnx')

# To see the produced ONNX model, we can print its contents or save it in text format.
print(linear_svc_onnx)
from winmltools.utils import save_text
save_text(linear_svc_onnx, 'linear_svc.txt')

# The conversion of linear regression is very similar. See the example below.
from sklearn.svm import LinearSVR
linear_svr = LinearSVR()
linear_svr.fit(X, y)
linear_svr_onnx = convert_sklearn(linear_svr, name='LinearSVR', input_features=[('input', 'float', 2)])   
~~~

사용자는 `LinearSVC`를 `RandomForestClassifier`와 같은 다른 scikit-learn 모델로 대체할 수 있습니다. [자동 코드 생성기](overview.md#automatic-interface-code-generation)는 `name` 매개 변수를 사용하여 클래스 이름과 변수를 생성합니다. `name`을 제공하지 않으면 GUID가 생성되며, 이는 C++/C#과 같은 언어에 대한 변수 명명 규칙을 준수하지 않습니다. 

## <a name="scikit-learn-pipelines"></a>Scikit-learn 파이프라인

다음으로 scikit-learn 파이프라인을 ONNX로 변환할 수 있는 방법을 보여 줍니다.

~~~python
# First, we create a toy data set.
# Notice that the first example's last feature value, 300, is large.
X = [[0.5, 1., 300.], [-1., -1.5, -4.], [0., -2., -1.]]
y = [1, -1, -1]

# Then, we declare a linear classifier.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()

# One common trick to improve a linear model's performance is to normalize the input data.
from sklearn.preprocessing import Normalizer
normalizer = Normalizer()

# Here, we compose our scikit-learn pipeline. 
# First, we apply our normalization. 
# Then we feed the normalized data into the linear model.
from sklearn.pipeline import make_pipeline
pipeline = make_pipeline(normalizer, linear_svc)
pipeline.fit(X, y)

# Now, we convert the scikit-learn pipeline into ONNX format. 
# Compared to the previous example, notice that the specified feature dimension becomes 3.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
pipeline_onnx = convert_sklearn(linear_svc, name='NormalizerLinearSVC', input_features=[('input', 'float', 3)])   

# We can print the fresh ONNX model.
print(pipeline_onnx)

# We can also save the ONNX model into binary file for later use.
from winmltools.utils import save_model
save_model(pipeline_onnx, 'pipeline.onnx')

# Our conversion framework provides limited support of heterogeneous feature values.
# We cannot have numerical types and string type in one feature vector. 
# Let's assume that the first two features are floats and the last feature is integer (encoded a categorical attribute).
X_heter = [[0.5, 1., 300], [-1., -1.5, 400], [0., -2., 100]]

# One popular way to represent categorical is one-hot encoding.
from sklearn.preprocessing import OneHotEncoder
one_hot_encoder = OneHotEncoder(categorical_features=[2])

# Let's initialize a classifier. 
# It will be right after the one-hot encoder in our pipeline.
linear_svc = LinearSVC()

# Then, we form a two-stage pipeline.
another_pipeline = make_pipeline(one_hot_encoder, linear_svc)
another_pipeline.fit(X_heter, y)

# Now, we convert, save, and load the converted model. 
# For heterogeneous feature vectors, we need to seperately specify their types for all homogeneous segments.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
another_pipeline_onnx = convert_sklearn(another_pipeline, name='OneHotLinearSVC', input_features=[('input', 'float', 2), ('another_input', 'int64', 1)])
save_model(another_pipeline_onnx, 'another_pipeline.onnx')
from winmltools.utils import load_model
loaded_onnx_model = load_model('another_pipeline.onnx')

# Of course, we can print the ONNX model to see if anything went wrong.
print(another_pipeline_onnx)
~~~

## <a name="core-ml-models"></a>Core ML 모델

여기서는 예제 Core ML 모델의 경로를 *example.mlmodel*이라고 가정합니다.

~~~python
from coremltools.models.utils import load_spec
# Load model file
model_coreml = load_spec('example.mlmodel')
from winmltools import convert_coreml
# Convert it!
# The automatic code generator (mlgen) uses the name parameter to generate class names.
model_onnx = convert_coreml(model_coreml, name='ExampleModel')   
~~~

`model_onnx`는 ONNX [ModelProto](https://github.com/onnx/onnxmltools/blob/0f453c3f375c1ae928b83a4c7909c82c013a5bff/onnxmltools/proto/onnx-ml.proto#L176) 개체입니다. 이 모델을 두 가지 다른 형식으로 저장할 수 있습니다.

~~~python
from winmltools.utils import save_model
# Save the produced ONNX model in binary format
save_model(model_onnx, 'example.onnx')
# Save the produced ONNX model in text format
from winmltools.utils import save_text
save_text(model_onnx, 'example.txt')
~~~

**참고**: CoreMLTools는 Apple에서 제공하는 Python 패키지이며 Windows에서는 사용할 수 없습니다. Windows에 패키지를 설치해야 하는 경우에는 리포지토리에서 패키지를 직접 설치합니다.

```
pip install git+https://github.com/apple/coremltools
```

## <a name="core-ml-models-with-image-inputs-or-outputs"></a>이미지 입력 또는 출력이 있는 Core ML 모델

ONNX에 이미지 유형이 부족하므로 Core ML 이미지 모델(즉, 이미지를 입력 또는 출력으로 사용하는 모델)을 변환하려면 일부 사전 처리 및 사후 처리 단계가 필요합니다.

사전 처리의 목적은 입력 이미지가 ONNX 텐서(tensor)로 올바르게 형식화되어 있는지 확인하는 것입니다. *X*를 Core ML에서 셰이프 [C, H, W]가 있는 이미지 입력이라고 가정합니다. ONNX에서 변수 *X*는 같은 셰이프가 있는 부동 소수점 텐서(tensor)이고 *X*[0, :, :]/*X*[1, :, :]/*X*[2, :, :]는 이미지의 빨강/초록/파랑 채널을 저장합니다. Core ML에 있는 회색 스케일 이미지의 경우 채널이 하나밖에 없으므로 ONNX에서 형식은 [1, H, W]-텐서입니다.

원본 Core ML 모델의 출력이 이미지인 경우 ONNX의 부동 소수점 출력 텐서를 다시 이미지로 수동으로 변환합니다. 두 가지 기본 단계가 있습니다. 첫 번째 단계는 255보다 큰 값을 255로 자르고 모든 음수 값을 0으로 변경하는 것입니다. 두 번째 단계는 모든 픽셀 값을 정수로 반올림하는 것입니다(0.5를 추가한 다음 소수점 이하를 잘라냄). 출력 채널 순서(예: RGB 또는 BGR)는 Core ML 모델에 표시됩니다. 적절한 이미지 출력을 생성하려면 원하는 형식을 복구하기 위해 순서를 뒤바꾸거나 섞어야 할 수 있습니다.

여기서는 ONNX와 Core ML 형식의 차이점을 설명하기 위한 구체적인 변환 예로 [GitHub](https://github.com/likedan/Awesome-CoreML-Models)에서 다운로드한 Core ML 모델, FNS-Candy를 고려합니다. 모든 후속 명령은 Python 환경에서 실행됩니다.

먼저 Core ML 모델을 로드하고 해당 입력과 출력 형식을 확인합니다.

~~~python
from coremltools.models.utils import load_spec
model_coreml = load_spec('FNS-Candy.mlmodel')
model_coreml.description # Print the content of Core ML model description
~~~

화면 출력:

~~~
...
input {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
output {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
~~~

이 경우 입력과 출력은 모두 720x720 BGR 이미지입니다. 다음 단계는 Core ML 모델을 WinMLTools로 변환하는 것입니다.

~~~python
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from onnxmltools import convert_coreml
model_onnx = convert_coreml(model_coreml, name='FNSCandy')    
~~~

ONNX에서 모델 입력 및 출력 형식을 보는 다른 방법으로 다음 명령을 사용할 수 있습니다.

~~~python
model_onnx.graph.input # Print out the ONNX input tensor's information
~~~

화면 출력:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

ONNX에 생성된 입력(*X*로 표시됨)은 4-D 텐서입니다. 마지막 3축은 각각 C-, H- 및 W-축입니다. 첫 번째 차원은 "없음"이며, 이는 이 ONNX 모델을 여러 이미지에 적용할 수 있음을 의미합니다. 2개의 이미지를 배치로 처리하기 위해 이 모델을 적용하는 경우 첫 번째 이미지는 *X*[0, :, :, :]에 해당하고 두 번째 이미지는 *X*[1, :, :, :]에 해당합니다. 첫 번째 이미지의 파랑/초록/빨강 채널은 *X*[0, 0, :, :]/*X*[0, 1, :, :]/*X*[0, 2, :, :]이고 두 번째 이미지와 유사합니다.

~~~python
model_onnx.graph.output # Print out the ONNX output tensor's information
~~~

화면 출력:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

보시다시피 생성된 형식은 원본 모델 입력 유형과 동일합니다. 그러나 이 경우 픽셀 값이 부동 소수점 수가 아닌 정수이므로 이미지가 아닙니다. 이를 다시 이미지로 변환하려면 255보다 큰 값을 255로 자르고 음수 값을 0으로 변경한 후 모든 소수를 정수로 반올림합니다.