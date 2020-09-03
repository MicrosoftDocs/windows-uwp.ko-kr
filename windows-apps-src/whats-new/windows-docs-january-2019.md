---
title: 2019년 1월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2019년 1월 Windows 10 개발자 설명서에 추가된 새로운 기능, 동영상 및 개발자 참고 자료
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 1월
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f1811e3b99077a3be676c57dc3420e88658f1d73
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174367"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>2019년 1월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 1월에 제공된 기능 개요, 개발자 지침 및 비디오는 다음과 같습니다.

Windows 10에 [도구 및 SDK를 설치](https://developer.microsoft.com/windows/downloads#_blank)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="windows-development-on-microsoft-learn"></a>Microsoft Learn에서 Windows 개발

Microsoft Learn에서는 Microsoft 개발자에게 새 실습 학습 및 교육 기회를 제공합니다. Windows 앱을 개발하는 방법을 알고 싶은 경우 [새 학습 경로](/learn/paths/develop-windows10-apps/)에서 플랫폼, 도구에 대한 자세한 소개와 첫 번째 앱을 작성하는 방법을 확인하세요.

![Windows 개발 학습 경로의 이미지](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Direct3D 12 렌더링 단계](/windows/desktop/direct3d12/direct3d-12-render-passes)는 다른 기술 중에서 TBDR(타일식 지연 렌더링)을 기준으로 하는 렌더러의 성능을 향상시킬 수 있습니다. 이 기술을 사용하면 애플리케이션에서 리소스 렌더링의 순서 지정 요구 사항 및 데이터 종속성을 더 잘 파악할 수 있기 때문에 오프칩 메모리와 주고받는 메모리 트래픽이 감소되어 렌더러가 GPU 효율성을 향상시키는 데 도움이 됩니다.

### <a name="msix-modification-packages"></a>MSIX 수정 패키지

Windows 10 버전 1809은 [MSIX 수정 패키지](/windows/msix/modification-package-1809-update)에 대한 지원이 향상되었습니다. 수정 패키지에 레지스트리 기반 플러그 인 및 관련 사용자 지정을 포함할 수 있으며 MSIX를 통해 배포된 애플리케이션에서 가상 레지스트리를 사용하고 예상대로 실행될 수 있습니다.

![MSIX 수정 패키지 만들기](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF, Windows Forms 및 WinUI의 오픈 소스

이제 GitHub에서 WPF, Windows Forms 및 WinUI UX 프레임워크로도 오픈 소스를 제공하고 있습니다. 자세한 내용과 링크를 보려면 [Windows 앱 빌드 블로그](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)를 참조하세요.

### <a name="progressive-web-apps-for-xbox"></a>Xbox용 점진적 웹앱

[Xbox One용 점진적 웹앱](/microsoft-edge/progressive-web-apps/xbox-considerations)을 사용하면 기존 프레임워크, CDN 및 서버 백 엔드를 계속 사용하면서 웹 애플리케이션을 확장하고 Microsoft Store를 통해 Xbox One 앱으로 사용하도록 제공할 수 있습니다. 대부분의 경우 Windows와 동일한 방식으로 Xbox One용 PWA를 패키지화할 수 있지만 몇 가지 중요한 차이점이 있으며, 이 가이드에서는 차이점을 안내합니다.

### <a name="windows-machine-learning"></a>Windows Machine Learning

[WinML API의 방문 페이지](/windows/ai/api-reference)가 재구성되고, WinML 사용자 지정 연산자 및 네이티브 API에 대한 새로운 설명서가 추가되었습니다.

[PyTorch로 모델 학습](/windows/ai/train-model-pytorch)에는 로컬 또는 클라우드에서 PyTorch 프레임워크를 사용하여 모델을 학습시키는 방법에 대한 지침이 제공됩니다. 그런 다음이 모델을 ONNX 파일로 다운로드하여 WinML 애플리케이션에서 사용할 수 있습니다.

![WinML 그래픽](images/winml-graphic.png)

## <a name="developer-guidance"></a>개발자 지침

### <a name="choose-your-platform"></a>플랫폼 선택

새 데스크톱 애플리케이션을 만드는 데 관심이 있나요? 개선된 [플랫폼 선택](/windows/desktop/choose-your-technology) 페이지에서 UWP, WPF 및 Windows Forms에 대한 자세한 설명과 비교 결과 및 Win32 API에 대한 자세한 내용을 확인하세요.

### <a name="faqs-on-win32-webview"></a>Win32 WebView에 대한 FAQ

[질문과 대답](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)에서는 데스크톱 애플리케이션에서 Microsoft Edge WebView를 사용할 때 일반적으로 발생하는 질문과 대답 뿐 아니라 샘플 및 추가 리소스에 대한 링크를 제공합니다.

### <a name="japanese-era-change"></a>일본 연호 변경

[일본의 시대 변경을 대비한 애플리케이션 준비](../design/globalizing/japanese-era-change.md)에서는 Windows 애플리케이션에서 2019년 5월 1일에 일본 연호 변경 세트가 적용될 준비를 수행하는 방법을 보여줍니다. [또한 이 페이지는 일본어로도 제공됩니다](../design/globalizing/japanese-era-change.md).

## <a name="videos"></a>비디오

### <a name="progressive-web-apps"></a>점진적 웹앱

점진적 웹앱은 다른 브라우저 및 다양한 Windows 10 디바이스에서 네이티브 앱처럼 작동하는 웹 사이트입니다. [비디오를 시청하여](https://youtu.be/ugAewC3308Y) 방법을 알아보고 [관련 문서를 확인하여](https://developer.microsoft.com/windows/pwa) 시작하세요.

### <a name="vs-code-series"></a>VS Code 시리즈

[Visual Studio Code에 대한 새 비디오 시리즈](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)를 시청하여 VSCode란 무엇이며, 어떻게 사용하고, 어떻게 만들어졌는지에 대해 알아보세요.

### <a name="one-dev-question"></a>One Dev Question

Microsoft 개발자는 One Dev Question 동영상 시리즈에서 오랫동안 Windows 개발, 팀 문화 및 이력에 대한 일련의 질문을 다루어 왔습니다. 최근에 답변한 질문은 다음과 같습니다.

Raymond Chen:

* [프로그램과 프로그램(x86)이 있는 이유가 무엇인가요?](https://youtu.be/qRb6otsHG5c)
* [Microsoft에서 첫 인터뷰는 어땠나요?](https://youtu.be/MfzzbNp8kfw)

Larry Osterman:

* [COM은 왜 그렇게 복잡한가요?](https://youtu.be/-gkXAV-StVA)
* [Microsoft에서 첫 인터뷰는 어땠나요?](https://youtu.be/N7o9eJpFYco)