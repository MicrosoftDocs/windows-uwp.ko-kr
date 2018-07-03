---
author: serenaz
title: Visual Studio에서 Windows ML용 모델을 학습하는 방법
description: 단계별 자습서가 있는 Visual Studio Tools for AI를 사용하여 Windows ML용 모델을 학습하는 방법을 알아봅니다.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Windows machine learning, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 1b54b0665a2483b8a0be710f505e928c852f4dba
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842670"
---
# <a name="how-to-train-a-model-for-windows-ml-in-visual-studio"></a>Visual Studio에서 Windows ML용 모델을 학습하는 방법

이 자습서에서는 딥러닝 및 AI 솔루션을 빌드, 테스트 및 배포하는 개발 확장 기능인 [Visual Studio Tools for AI](http://aka.ms/vstoolsforai)를 사용하여 [시작](get-started.md)의 MNIST 샘플 앱에 대한 모델을 학습합니다.

[Microsoft Cognitive Toolkit(CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit) 프레임워크 및 [MNIST 데이터 집합](http://yann.lecun.com/exdb/mnist/)(손으로 쓴 숫자에 대한 60,000개의 학습 세트 예제와 10,000개의 테스트 세트 예제가 있음)을 사용하여 모델을 학습합니다. 그런 다음 Windows ML에서 사용할 [ONNX(Open Neural Network Exchange)](https://onnx.ai/) 형식을 사용하여 모델을 저장합니다.

## <a name="prerequisites"></a>필수 조건
### <a name="install-visual-studio-tools-for-ai"></a>Visual Studio Tools for AI 설치
시작하려면 [Visual Studio](https://www.visualstudio.com/downloads/)를 다운로드하여 설치해야 합니다. Visual Studio가 열리면 **Visual Studio Tools for AI** 확장을 활성화합니다.

1. Visual Studio에서 메뉴 모음을 클릭하여 "확장 및 업데이트..."를 선택합니다.
2. "온라인" 탭을 클릭하여 "Visual Studio Marketplace 검색"을 선택합니다.
3. "Visual Studio Tools for AI"를 검색합니다. 
3. **다운로드** 단추를 클릭합니다. 
4. 설치 후에 Visual Studio를 다시 시작합니다. 

Visual Studio가 다시 시작되면 확장이 활성화됩니다. 문제가 있는 경우 [Visual Studio 확장 찾기](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions)를 확인해 보세요.

### <a name="download-sample-code"></a>샘플 코드 다운로드
GitHub에서 [AI용 샘플](https://github.com/Microsoft/samples-for-ai) 리포지토리를 다운로드합니다. 이 샘플은 TensorFlow, CNTK, Theano 등에서 딥러닝을 시작하는 방법을 다룹니다.

### <a name="install-cntk"></a>CNTK 설치
[Windows에서 Python용 CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/setup-windows-python?tabs=cntkpy24)를 설치합니다. 아직 설치하지 않은 경우 Python도 설치해야 합니다.

또는 딥러닝 모델 개발을 위한 시스템을 준비하려면 Python, CNTK, TensorFlow, NVIDIA GPU 드라이버(선택적) 등을 설치하기 위한 간단한 설치 관리자에 대한 [개발 환경 준비](https://github.com/Microsoft/samples-for-ai/blob/master/README.md)를 참조하세요.

## <a name="1-open-project"></a>1. 프로젝트 열기

Visual Studio를 시작하고 **파일 > 열기 > 프로젝트/솔루션**을 선택합니다. AI용 샘플 리포지토리에서 **examples\cntk\python** 폴더를 선택하고 **CNTKPythonExamples.sln** 파일을 엽니다.

![솔루션 열기](images/open-solution.png)

## <a name="2-train-the-model"></a>2. 모델 학습

MNIST 프로젝트를 시작 프로젝트로 설정하려면 Python 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

![솔루션 열기](images/mnist-startup.png)

그런 다음 train_mnist_onnx.py 파일을 열고 F5 또는 녹색 실행 단추를 눌러 프로젝트를 **실행**합니다.

## <a name="3-view-the-model-and-add-it-to-your-app"></a>3. 모델 보기 및 앱에 추가

이제 학습된 **mnist.onnx** 모델 파일은 samples-for-ai/examples/cntk/python/MNIST 폴더에 있어야 합니다. 이 학습된 **mnist.onnx** 모델 파일을 사용하여 [시작](get-started.md)의 MNIST 샘플 앱을 빌드할 수 있습니다! 

## <a name="4-learn-more"></a>4. 자세한 내용
[Azure GPU Virtual Machines](https://docs.microsoft.com/en-us/visualstudio/ai/tensorflow-vm) 등을 사용하여 딥러닝 모델의 학습 속도를 높이는 방법을 알아보려면 [Artificial Intelligence at Microsoft](https://www.microsoft.com/ai) 및 [Microsoft Machine Learning 기술](https://docs.microsoft.com/en-us/azure/machine-learning/#More-Microsoft-Machine-Learning-Technologies)을 방문하세요.