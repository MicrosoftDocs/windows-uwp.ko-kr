---
title: 에서 새로운 기능 Windows Docs에서 2019 년 1 월-UWP 앱 개발
description: 새로운 기능, 비디오 및 개발자 지침에 추가한 2019 년 1 월에 대 한 Windows 10 개발자 설명서
keywords: 새로운 기능, 업데이트, 기능, 개발자 가이드, Windows 10 월
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636578"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>에서 새로운 기능 Windows 개발자 문서에 2019 년 1 월

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 지침 및 비디오에서 1 월에 제공 되었습니다.

Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="windows-development-on-microsoft-learn"></a>Microsoft 학습에 대 한 Windows 개발

Microsoft 알아보기 Microsoft 개발자에 게 새 실습 학습 및 교육 기회를 제공합니다. Windows 앱을 개발 하는 방법을 알고 싶은 경우 체크 아웃 [우리의 새 학습 경로](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) 대 한 철저 한 플랫폼, 도구, 및 첫 번째 몇 가지 앱을 작성 하는 방법을 소개 합니다.

![Windows 개발 학습 과정의 이미지](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Direct3D 12 렌더링 패스](/windows/desktop/direct3d12/direct3d-12-render-passes) 성능을 개선할 수 여 렌더러의 지연 된 렌더링 타일 기반의 (TBDR)에 기초 하는 경우 다른 기술 중 에서도 특히 합니다. 기술은 리소스 렌더링 순서 지정 요구 사항 및 데이터 종속성을 더 잘 식별 하는 응용 프로그램을 활성화 하 고 있으므로 메모리 들어오고 나가는 트래픽을 칩 외부 메모리를 줄여 GPU 효율성을 개선 하 여 렌더러를 수 있습니다.

### <a name="msix-modification-packages"></a>MSIX 수정 패키지

Windows 10 버전 1809 지원 개선된 [MSIX 수정 패키지](https://docs.microsoft.com/windows/msix/modification-package-1809-update)합니다. 수정 패키지 레지스트리 기반 플러그 인 및 관련 된 사용자 지정 등이 있습니다 MSIX 가상 레지스트리를 사용 하 고 예상 대로 실행을 통해 배포 된 응용 프로그램을 사용 하도록 설정 됩니다.

![MSIX 수정 패키지 만들기](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF, Windows Forms 및 WinUI의 오픈 소스

WPF, Windows Forms 및 WinUI UX 프레임 워크를 GitHub에서 오픈 소스 기여에 대 한 사용할 수 있습니다. 자세한 내용과 링크에 대 한 참조를 [빌드 Windows 앱 블로그](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)합니다.

### <a name="progressive-web-apps-for-xbox"></a>Xbox 용 점진적 웹 앱

사용 하 여 [점진적 Web Apps for Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), 웹 응용 프로그램을 확장 하 고 사용할 수 있도록 Xbox One 앱으로 Microsoft Store 통해 여전히 계속 기존 프레임 워크, 백 엔드 서버 및 CDN을 통해 사용할 수 있습니다. 그러나 몇 가지 주요 차이점이이 가이드에서는 안내, Xbox One 용에 PWA를 패키지 Windows의 경우 동일한 방식으로 대부분의 경우.

### <a name="windows-machine-learning"></a>Windows Machine learning

에서는 재구성 했습니다 [WinML Api에 대 한 방문 페이지](https://docs.microsoft.com/windows/ai/api-reference), WinML 사용자 지정 연산자 및 네이티브 Api에 관한 새 설명서를 추가 합니다.

[PyTorch 사용 하 여 모델을 학습](https://docs.microsoft.com/windows/ai/train-model-pytorch) 로컬로 또는 클라우드에서 PyTorch 프레임 워크를 사용 하 여 모델을 학습 하는 방법에 대 한 지침을 제공 합니다. 그런 다음이 모델을 ONNX 파일로 다운로드 하 고 WinML 응용 프로그램에서 사용할 수 있습니다.

![WinML 그래픽](images/winml-graphic.png)

## <a name="developer-guidance"></a>개발자 지침

### <a name="choose-your-platform"></a>플랫폼 선택

새 데스크톱 응용 프로그램을 만드는 데 관심이 있으십니까? 이 개선 된 확인해 [플랫폼을 선택](https://docs.microsoft.com/windows/desktop/choose-your-technology) 자세한 설명 및 플랫폼, UWP, WPF 및 Windows Forms 및 Win32 API에 대 한 자세한 내용은 비교에 대 한 페이지입니다.

### <a name="faqs-on-win32-webview"></a>Win32 WebView에 대 한 Faq

우리의 [질문과 대답](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) 샘플 및 추가 리소스 링크 수 있을 뿐만 아니라 데스크톱 응용 프로그램에서 Microsoft Edge 웹 보기를 사용 하는 경우 일반적인 질문과 대답을 제공 합니다.

### <a name="japanese-era-change"></a>일본어 연대 변경

[일본어 연대 변경에 대 한 응용 프로그램 준비](../design/globalizing/japanese-era-change.md) 응용 프로그램은 일본어 연대 변경 집합을 준비 하 여 Windows 2019 년 5 월 1 일에 배치 되도록 하는 방법을 보여 줍니다. [또한이 페이지는 일본어에서](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)합니다.

## <a name="videos"></a>비디오

### <a name="progressive-web-apps"></a>점진적 웹앱

점진적 웹 앱은 다른 브라우저 및 다양 한 Windows 10 장치에서 네이티브 앱 처럼 작동 하는 웹 사이트입니다. [비디오를 시청](https://youtu.be/ugAewC3308Y) 자세한 내용은 차례로 [문서를 체크 아웃](https://aka.ms/Windows-PWA) 시작 하려면.

### <a name="vs-code-series"></a>VS 코드 시리즈

체크 아웃 우리의 [Visual Studio Code에서 새 비디오 시리즈](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) VSCode 란를 사용 하는 방법 및 생성 방법에 대 한 정보에 대 한 합니다.

### <a name="one-dev-question"></a>개발 질문 중 하나

개발 질문 중 하나는 비디오 시리즈에서는 해온 Microsoft 개발자에 게 일련의 Windows 개발, 팀 문화권 및 기록 하는 방법에 대 한 질문을 다룹니다. 최신 질문 대답할 것에 다음과 같습니다.

Raymond Chen:

* [프로그램 파일 및 프로그램 파일 (x86) 있는 이유는?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [이유는 COM 복잡?](https://youtu.be/-gkXAV-StVA )
* [Microsoft를 사용 하 여 첫 번째 인터뷰와 같은 무엇 이었습니까?](https://youtu.be/qRb6otsHG5c)
