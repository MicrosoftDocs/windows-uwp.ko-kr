---
author: serenaz
title: Windows ML 개요
description: Windows Machine Learning에 대해 알아보고 Windows ML을 사용하여 개발하는 방법을 알아봅니다.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, windows machine learning
ms.localizationpriority: medium
ms.openlocfilehash: a2470cff6b5c7f07c720a38d0bff00486e4c6b27
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842873"
---
# <a name="windows-ml-overview"></a>Windows ML 개요

![Windows Machine Learning 그래픽](images/brain.png)

## <a name="what-is-machine-learning"></a>기계 학습이란 무엇인가요?

기계 학습(ML)을 사용하면 컴퓨터가 기존 데이터를 사용하여 예상 결과 및 동작을 예측할 수 있습니다. 이전에 수집한 데이터를 처리하여 ML 알고리즘은 새로운 입력이 표시될 때 올바른 출력을 예측할 수 있는 모델을 빌드합니다. 예를 들어 이메일 메시지(입력)를 스팸 또는 스팸이 아닌 것(출력)으로 평가하도록 모델을 학습시킬 수 있습니다.

모델 빌드 단계를 "학습"이라고 합니다. 기존 데이터로 학습된 경우 모델은 이전에 볼 수 없었던 새로운 데이터를 사용하여 예측을 수행할 수 있습니다. 이를 "추론", "평가" 또는 "채점"이라고 합니다.

학습된 모델은 종종 엄격한 명령어 세트를 따르도록 작성된 프로그램보다 더 나은 결과를 산출합니다. 특히 많은 입력 및 출력 조합이 가능한 복잡한 작업의 경우에는 더욱 그러합니다. 예를 들어 권장 알고리즘은 전자 상거래 및 미디어 스트리밍 사이트에서 수백만 명의 사용자를 위해 맞춤식 권장 사항을 제공합니다. 이는 기계 학습 없이는 거의 불가능합니다. Machine Learning을 사용하는 또다른 분야는 컴퓨터 비전입니다. 이를 사용하면 컴퓨터가 이전에 레이블이 지정된 이미지를 학습한 후 이미지를 분류하고 식별할 수 있습니다.

기계 학습의 가능성과 적용은 무궁무진합니다. 연구 및 솔루션에 대한 자세한 내용을 보려면 [Microsoft의 인공 지능](https://www.microsoft.com/ai) 및 [Microsoft AI 플랫폼](https://azure.microsoft.com/en-us/overview/ai-platform/)을 방문해 보세요. 기계 학습 및 AI 모델을 빌드하려는 경우 [Azure Machine Learning 서비스](https://docs.microsoft.com/azure/machine-learning/preview/overview-what-is-azure-ml)도 확인할 수 있습니다.

## <a name="what-is-windows-ml"></a>Windows ML이란 무엇인가요?

Windows ML은 개발자가 Windows 응용 프로그램 내에서 기계 학습을 사용할 수 있는 Windows 10 장치에서 학습된 기계 학습 모델을 평가하는 플랫폼입니다.

Windows ML의 몇 가지 주요 기능은 다음과 같습니다.

- **하드웨어 가속**
    
    DirectX12 지원 장치에서 Windows ML은 GPU를 사용하여 딥러닝 모델에 대한 평가를 가속화합니다. 또한 CPU 최적화를 사용하여 클래식 ML과 딥러닝 알고리즘 모두에 대한 고성능 평가를 수행할 수 있습니다.

- **로컬 평가판**

    Windows ML은 로컬 하드웨어를 평가하여 연결, 대역폭 및 데이터 개인 정보 보호 문제를 없앨 수 있습니다. 또한 로컬 평가를 통해 대기 시간은 줄이고 성능은 높여 평가 결과를 빨리 얻어낼 수 있습니다.

- **이미지 처리**

    컴퓨터 비전 시나리오의 경우 Windows ML은 프레임 사전 처리를 해결하고 모델 입력을 위한 카메라 파이프라인 설정을 제공하여 이미지, 비디오 및 카메라 데이터에 대한 사용을 단순화하고 최적화합니다.

## <a name="how-to-develop-with-windows-ml"></a>Windows ML을 사용하여 개발하는 방법

> [!VIDEO https://www.youtube.com/embed/8MCDSlm326U]

### <a name="system-requirements"></a>시스템 요구 사항

Windows ML을 사용하는 응용 프로그램을 빌드하려면 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) - 빌드 17110 이상이 필요합니다.

### <a name="onnx-models"></a>ONNX 모델

Windows ML을 사용하려면 [ONNX(Open Neural Network Exchange)](https://onnx.ai) 형식의 미리 학습된 기계 학습 모델이 필요합니다. Windows ML은 개발자가 다른 학습 프레임워크에서 생성된 모델을 사용할 수 있는 v1.0 릴리스의 ONNX 형식을 지원합니다.

공개적으로 사용 가능한 ONNX 모델 목록은 GitHub의 [ONNX 모델](https://github.com/onnx/models)을 참조하세요.

Visual Studio Tools for AI를 사용하여 ONNX 모델을 학습하는 방법을 알아보려면 [모델 교육](train-ai-model.md)을 참조하세요.

### <a name="convert-existing-models-to-onnx"></a>기존 모델을 ONNX로 변환

많은 학습 프레임워크에서는 이미 기본적으로 ONNX 모델을 지원하고 있으며 많은 프레임워크와 라이브러리에 대한 변환기 도구가 있습니다. Caffe 2, PyTorch, CNTK, Chainer 등의 프레임워크에서 내보내는 방법을 알아보려면 GitHub의 [ONNX 자습서](https://github.com/onnx/tutorials)를 참조하세요.

또한 [WinMLTools](https://pypi.org/project/winmltools/)를 사용하여 학습된 기계 학습 모델을 Windows ML에서 허용되는 ONNX 형식으로 변환할 수 있습니다. WinMLTools는 다음 형식에서의 변환을 지원합니다.

- Core ML
- Scikit-Learn
- XGBoost
- LibSVM

WinMLTools를 설치하고 사용하는 방법을 알아보려면 [모델 변환](conversion-samples.md)을 참조하세요.

Visual Studio Tools for AI 확장으로 Visual Studio IDE 내에서 WinMLTools를 사용하여 더 친숙하고 클릭이 용이한 환경에서 모델을 ONNX 형식으로 변환할 수 있습니다. 자세한 정보는 [VS Tools for AI](https://github.com/Microsoft/vs-tools-for-ai/)를 참조하세요.

### <a name="onnx-operators"></a>ONNX 연산자

Windows ML은 CPU에서 100개 이상의 ONNX 연산자를 지원하고 DirectX12 호환 GPU에서 계산을 가속화합니다. 연산자 서명의 전체 목록은 ONNX 연산자 스키마 설명서에서 [ai.onnx](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators.md)(기본값) 및 [ai.onnx.ml](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators-ml.md) 네임스페이스를 참조하세요.

Windows ML은 다음과 같은 차이점은 제외하고 ONNX v1.0 설명서에 정의된 모든 연산자를 지원합니다.

- Windows ML에서 지원하는 "실험적"으로 표시된 연산자:
    - Affine
    - Crop
    - FC
    - Identity
    - ImageScaler
    - MeanVarianceNormalization
    - ParametricSoftplus
    - ScaledTanh
    - ThresholdedRelu
    - Upsample
- MatMul - 2D 매트릭스 곱하기보다 큰 값은 현재 지원되지 않으며 CPU에서만 지원됨
- Cast - CPU에서만 지원됨
- 다음 연산자는 현재 지원되지 않습니다.
    - RandomUniform
    - RandomUniformLike
    - RandomNormal
    - RandomNormalLike

### <a name="automatic-interface-code-generation"></a>자동 인터페이스 코드 생성

ONNX 모델 파일을 사용하여 Windows ML의 코드 생성기는 앱에서 모델과 상호 작용하는 인터페이스를 만듭니다. 생성된 인터페이스에는 모델, 입력 및 출력을 나타내는 래퍼 클래스가 포함됩니다. 생성된 코드는 [Windows ML API](/uwp/api/windows.ai.machinelearning.preview)를 호출하여 사용자 프로젝트에서 모델을 쉽게 로드, 바인딩 및 평가할 수 있습니다. 코드 생성기는 현재 C# 및 C++/CX를 지원합니다.

UWP 개발자를 위해 Windows ML 자동 코드 생성기는 기본적으로 [Visual Studio](https://developer.microsoft.com/windows/downloads)와 통합됩니다. Visual Studio 프로젝트 내에서 ONNX 파일을 기존 항목으로 추가하기만 하면 VS가 새 인터페이스 파일에 Windows ML 래퍼 클래스를 생성합니다.

명령줄 도구 `mlgen.exe`를 사용할 수도 있습니다. 이는 Windows SDK와 함께 제공되어 Windows ML 래퍼 클래스를 생성합니다. 이 도구는 `(SDK_root)\bin\<version>\x64` 또는 `(SDK_root)\bin\<version>\x86`에 있으며, 여기서 SDK_root는 SDK 설치 디렉터리입니다. 이 도구를 실행하려면 다음 명령을 사용합니다.

```
mlgen -i INPUT-FILE -l LANGUAGE -n NAMESPACE [-o OUTPUT-FILE]
```

입력 매개 변수 정의:

- `INPUT-FILE`: ONNX 모델 파일
- `LANGUAGE`: CPPCX 또는 CS
- `NAMESPACE`: 생성된 코드의 네임스페이스
- `OUTPUT-FILE`: 생성된 코드가 기록된 파일 경로. OUTPUT-FILE을 지정하지 않으면 생성된 코드가 표준 출력으로 기록됩니다.

앱에서 생성된 코드를 사용하는 방법을 알아보려면 [모델 통합](integrate-model.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

[시작](get-started.md)의 단계별 자습서를 이용해 첫 번째 Windows ML 앱을 만들어 보세요.