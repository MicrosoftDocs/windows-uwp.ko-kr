---
title: 2019 년 1 월 Windows 문서의 새로운 내용-UWP 앱 개발
description: 2019 년 1 월에 대 한 Windows 10 개발자 설명서에 추가 된 새로운 기능, 동영상 및 개발자 지침
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10 년 1 월
ms.date: 1/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cc5323ba12fa72fb5350e62f74206ea72fe96497
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9046133"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>1 월 2019의 새로운 Windows 개발자 문서의 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침 및 동영상 내용이 1 월에 사용할 수 있습니다.

Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="windows-development-on-microsoft-learn"></a>Microsoft 학습에 Windows 개발

Microsoft 학습 Microsoft 개발자에 게 새로운 실습 학습 및 교육 기회를 제공합니다. Windows 앱을 개발 하는 방법을 알고 싶은 경우, 철저 한 소개 플랫폼와 도구를 처음 몇 가지 앱을 작성 하는 방법에 대 한 [새 우리의 학습 경로](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) 확인 합니다.

![경로 학습 Windows 개발의 이미지](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Direct3D 12 렌더링 패스가](/windows/desktop/direct3d12/direct3d-12-render-passes) 다른 기술 중 에서도 특히 타일 기반 지연 렌더링 (TBDR)에 기초 하는 경우 렌더러의 성능을 향상 시킬 수 있습니다. 기술을 렌더러 응용 프로그램 리소스 렌더링 순서 지정 요구 사항 및 데이터 종속성을 식별 하는 데 활성화 및 칩 오프 메모리에서 메모리 트래픽을 감소 하 여 GPU 효율성을 높일 수 있습니다.

### <a name="msix-modification-packages"></a>MSIX 수정 패키지

[MSIX 수정 패키지](https://docs.microsoft.com/windows/msix/modification-package-1809-update)에 대 한 Windows 10 버전 1809 향상 지원 합니다. 수정 패키지 레지스트리 기반 플러그 인 및 관련 된 사용자 지정을 포함할 수 있으며 MSIX 가상 레지스트리를 사용 하 고 예상 대로 실행을 통해 배포 된 응용 프로그램을 사용 하도록 설정 됩니다.

![MSIX 수정 패키지 만들기](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF, Windows Forms, 및 WinUI 오픈 소스

WPF, Windows Forms, 및 WinUI UX 프레임 워크 GitHub의 오픈 소스 직원만 사용할 수 있습니다. 자세한 정보와 링크에 대 한 [빌드 Windows 앱 블로그](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)를 참조 하세요.

### <a name="progressive-web-apps-for-xbox"></a>Xbox에 대 한 점진적 웹 앱

[Xbox One에 대 한 점진적 웹 앱](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)함께 웹 응용 프로그램을 확장 하 고 사용할 수 있도록 Xbox One 앱으로 Microsoft Store를 통해 기존 프레임 워크, CDN 및 서버 백 엔드를 사용 하 여 여전히 하면서 수 있습니다. 하지만 Xbox One에 대 한에 PWA를 패키지 Windows에 대 한 동일한 방식으로 대부분의 경우,이 가이드를 안내 하는 몇 가지 주요 차이점이 있습니다.

### <a name="windows-machine-learning"></a>Windows 기계 학습

[방문 페이지 WinML Api에 대 한](https://docs.microsoft.com/windows/ai/api-reference)재구성 내용이 및 WinML 사용자 지정 연산자 및 네이티브 Api에 대 한 새 문서를 추가 합니다.

로컬 또는 클라우드에서 PyTorch 프레임 워크를 사용 하 여 모델을 학습 하는 방법에 지침을 제공 [PyTorch 사용 하 여 모델을 학습](https://docs.microsoft.com/windows/ai/train-model-pytorch) 합니다. 그런 다음이 모델을 ONNX 파일을 다운로드 하 고 WinML 응용 프로그램에서 사용할 수 있습니다.

![WinML 그래픽](images/winml-graphic.png)

## <a name="developer-guidance"></a>개발자 지침

### <a name="choose-your-platform"></a>플랫폼 선택

새 데스크톱 응용 프로그램을 만드는에 관심이 있으십니까? 자세한 설명은, UWP, WPF 및 Windows Forms 플랫폼 및 Win32 API에 대 한 자세한 내용은 비교에 대 한 개선 된 [플랫폼 선택](https://docs.microsoft.com/windows/desktop/choose-your-technology) 페이지를 확인 합니다.

### <a name="faqs-on-win32-webview"></a>Win32 WebView에 Faq

[질문과 대답](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) 우리의 샘플 및 추가 리소스에 대 한 링크를 비롯 하 여 데스크톱 응용 프로그램에 Microsoft Edge 웹 보기를 사용 하는 경우 일반적인 질문과 대답을 제공 합니다.

### <a name="japanese-era-change"></a>일본어 종말 변경

[일본어 연대 응용 프로그램 준비 변경](../design/globalizing/japanese-era-change.md) 일본어 연대 변경 내용을 설정에 대 한 응용 프로그램은 준비 Windows 2019 년 5 월 1 일에 배치 되도록 하는 방법을 보여줍니다. [이 페이지는 또한 일본어에서 사용할 수 있습니다](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>비디오

### <a name="progressive-web-apps"></a>점진적 웹앱

점진적 웹 앱은 여러 브라우저와 다양 한 Windows 10 장치에서 네이티브 앱 처럼 작동 하는 웹 사이트입니다. 더 자세한 [동영상을 시청](https://youtu.be/ugAewC3308Y) 한 다음 초보자를 위한 [문서를 확인](https://aka.ms/Windows-PWA) 합니다.

### <a name="vs-code-series"></a>VS 코드 시리즈

VSCode 란를 사용 하는 방법 및 만들어진 방법에 대 한 정보는 [Visual Studio Code에서 새 비디오 시리즈](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) 를 확인 합니다.

### <a name="one-dev-question"></a>개발자 질문

개발자 질문 동영상 시리즈 오랜 기간 사용해 온 Microsoft 개발자는 일련의 Windows 개발 팀 문화 및 기록에 대 한 질문을 설명합니다. 와 같은 최신 질문에 대답 하는 다음과 같습니다.

Raymond Chen:

* [왜 Program Files 및 Program Files (x86) 있습니까?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [COM 이유는 복잡 한 하므로?](https://youtu.be/-gkXAV-StVA )
* [Microsoft에 대 한 첫 번째 인터뷰 같은 하 던?](https://youtu.be/qRb6otsHG5c)
