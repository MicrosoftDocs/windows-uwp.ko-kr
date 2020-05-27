---
title: WinUI 3.0 Preview 1(2020년 5월)
description: WinUI 3.0 Preview의 개요를 설명합니다.
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: 3aac14807f8455eb9a9294c40ddc76ddfa224659
ms.sourcegitcommit: 7e8c7f89212c88dcc0274c69d2c3365194c0954a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83688481"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Windows UI 라이브러리 3.0 Preview 1(2020년 5월)

Windows UI 라이브러리(WinUI) 3.0은 WinUI를 Win32에서 UWP에 이르는 모든 유형의 Windows 앱용 전체 UX 프레임워크로 변환하는 주요 업데이트입니다.

> [!Important]
> 이 WinUI 3.0 Preview 릴리스의 목적은 개발자 커뮤니티에서 초기 평가를 받고 피드백을 수집하는 것입니다. 프로덕션 앱에는 사용하면 **안 됩니다**.
>
> **[Preview 1 제한 사항 및 알려진 문제](#preview-1-limitations-and-known-issues)를 참조하세요**.
## <a name="new-features-in-winui-30-preview-1"></a>WinUI 3.0 Preview 1의 새로운 기능

- Win32 앱용 [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)를 포함하여 WinUI를 사용하여 데스크톱 앱을 만드는 기능
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView 업데이트](/windows/uwp/design/controls-and-patterns/tab-view)
- 어두운 테마 업데이트
- [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2)의 향상된 기능 및 업데이트
  - 높은 DPI 지원
  - 창 크기 조정 및 이동 지원
  - 최신 버전의 Edge를 대상으로 업데이트됨
  - 더 이상 WebView2별 Nuget 패키지를 참조할 필요 없음
- SwapChainPanel
- 오픈 소스 마이그레이션에 필요한 향상된 기능

WinUI 3.0 및 WinUI 로드맵의 이점에 대한 자세한 내용은 GitHub의 [Windows UI 라이브러리 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)을 참조하세요.

### <a name="provide-feedback-and-suggestions"></a>피드백 및 제안 제공

[WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)에 대한 피드백을 환영합니다.

## <a name="try-winui-30-preview-1"></a>WinUI 3.0 Preview 1 체험

개발 환경을 구성하고(자세한 지침은 [개발 환경 구성](#configure-your-dev-environment) 참조) 다음 링크에서 WinUI 3.0 Preview 1 VSIX를 설치한 다음, WinUI 3.0 프로젝트 템플릿을 사용해보세요.

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

[Xaml 컨트롤 갤러리](#xaml-controls-gallery-winui-30-preview-1-branch)의 WinUI 3.0 Preview 1 버전을 복제하고 빌드할 수도 있습니다.

### <a name="configure-your-dev-environment"></a>개발 환경 구성

개발용 컴퓨터에 Windows 10 2018년 4월 업데이트(버전 1803 - 빌드 17134) 이상이 설치되어 있는지 확인합니다.

Visual Studio 2019, 버전 16.7 Preview 1을 설치합니다. [Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview)에서 다운로드할 수 있습니다.

Visual Studio Preview를 설치할 때 다음 워크로드를 포함해야 합니다.

- .NET 데스크톱 개발
- 유니버설 Windows 플랫폼 개발

C++ 앱을 빌드하려면 다음 워크로드도 포함해야 합니다.

- C++를 사용한 데스크톱 개발
- 유니버설 Windows 플랫폼 워크로드에 대한 *C++(v142) 유니버설 Windows 플랫폼 도구* 선택적 구성 요소

### <a name="visual-studio-project-templates"></a>Visual Studio 프로젝트 템플릿

WinUI 3.0 Preview 1 및 프로젝트 템플릿에 액세스하려면 **https://aka.ms/winui3/previewdownload** 로 이동합니다.

Visual Studio 확장(`.vsix`)을 다운로드하여 WinUI 프로젝트 템플릿 및 NuGget 패키지를 Visual Studio 2019에 추가합니다.

Visual Studio에 `.vsix`를 추가하는 방법에 대한 지침은 [Visual Studio 확장 찾기 및 사용](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box)을 참조하세요.

`.vsix` 확장을 설치한 후에는 "WinUI"를 검색하고 사용 가능한 C# 또는 C++ 템플릿 중 하나를 선택하여 새 WinUI 3.0 프로젝트를 만들 수 있습니다.

![WinUI 3.0 Visual Studio 템플릿](images/WinUI3Templates.png)<br/>
*WinUI 3.0 Visual Studio 템플릿 예제*

### <a name="visual-studio-project-configuration"></a>Visual Studio 프로젝트 구성

WinUI 3.0 Preview 1 템플릿 중 하나를 사용하여 프로젝트를 만드는 경우 **대상 버전**을 Windows 10, 버전 1903(빌드 18362)으로 설정하고, **최소 버전**을 Windows 10, 버전 1803(빌드 17134)으로 설정합니다.

프로젝트를 만든 후 이 값을 변경하려면 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>WinUI 3.0 Preview 1을 사용하여 데스크톱 Win32 앱 만들기

[데스크톱 앱용 WinUI 3.0 시작](get-started-winui3-for-desktop.md)을 참조하세요.

아래에 설명된 제한 사항 및 알려진 문제 외에도, WinUI 3.0 Preview 1을 사용하여 앱을 빌드하는 것은 Xaml 및 WinUI 2.x를 사용하여 UWP 앱을 빌드하는 것과 매우 유사하므로, UWP 앱 및 `Windows.UI` API에 대한 대부분의 지침과 설명서가 똑같이 적용됩니다.

## <a name="preview-1-limitations-and-known-issues"></a>Preview 1 제한 사항 및 알려진 문제

Preview 1 릴리스는 단지 미리 보기일 뿐입니다. 데스크톱 Win32 앱 관련 시나리오는 특히 새로운 시나리오입니다. 버그, 제한 사항 및 문제를 예상하세요.

다음 항목은 WinUI 3.0 Preview 1의 알려진 문제 중 일부입니다. 아래에 없는 문제가 발견되면 [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)에서 기존 문제에 덧붙이거나 새 문제를 제출하여 알려주세요.

### <a name="platform-and-os-support"></a>플랫폼 및 OS 지원

WinUI 3.0 Preview 1은 Windows 10 2018년 4월 업데이트(버전 1803 - 빌드 17134) 이상 버전을 실행하는 PC와 호환됩니다.

### <a name="developer-tools"></a>개발자 도구

- 데스크톱 앱은 .NET 5와 C# 8을 지원하며 패키지되어야 합니다.
- UWP 앱은 .NET 네이티브 및 C# 7.3을 지원합니다.
- Intellisense가 불완전합니다.
- 비주얼 디자이너 없음
- 핫 다시 로드 없음
- 라이브 시각적 트리 없음
- VS Code를 사용한 개발은 아직 지원되지 않습니다.
- 새 C++/CX 앱은 지원되지 않지만, 기존 앱은 계속 작동합니다(가능한 한 빨리 C++/WinRT로 이동).
- WinUI 3.0 콘텐츠는 프로세스당 하나의 창 또는 앱당 하나의 ApplicationView에만 있을 수 있습니다.
- 패키지되지 않은 데스크톱 배포는 지원되지 않습니다.
- ARM64 지원 안 함

### <a name="missing-platform-features"></a>플랫폼 기능 누락

- Xbox 지원 안 함
- HoloLens 지원 안 함
- 창 팝업 지원 안 함
- 수동 입력 지원 안 함
- 배경 아크릴
- MediaElement 및 MediaPlayerElement
- RenderTargetBitmap
- MapControl
- NavigationView를 사용하여 계층적 탐색
- SwapChainPanel은 투명성을 지원하지 않습니다.
- 글로벌 표시는 솔리드 브러시인 폴백 동작을 사용합니다.
- XAML Islands는 이 릴리스에서 지원되지 않습니다.
- 타사 에코시스템 라이브러리는 완전하게 작동하지 않습니다.
- IME가 작동하지 않습니다.
- Windows.UI.Text 네임스페이스의 메서드를 호출할 수 없습니다.

### <a name="known-issues"></a>알려진 문제

- C# 데스크톱 앱에서 다음을 확인할 수 있습니다.
   - Windows 개체(Xaml 개체 포함)에 대한 약한 참조에는 `System.WeakReference<T>` 대신 `WinRT.WeakReference<T>`를 사용해야 합니다.
   - [Point](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [Rect](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) 및 [Size](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size) 구조체에는 Double이 아닌 Float 형식의 멤버가 있습니다.


## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>Xaml 컨트롤 갤러리(WinUI 3.0 Preview 1 분기)

모든 WinUI 3.0 Preview 1 컨트롤 및 기능을 포함하고 있는 샘플 앱은 [Xaml 컨트롤 갤러리의 WinUI 3.0 Preview 1 분기](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)를 참조하세요.

![WinUI 3.0 Preview 1 Xaml 컨트롤 갤러리 앱](images/WinUI3XamlControlsGallery.png)<br/>
*WinUI 3.0 Preview 1 Xaml 컨트롤 갤러리 앱의 예*

샘플을 다운로드하려면 다음 명령을 사용하여 **winui3preview** 분기를 복제합니다.

> `git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git`

복제 후, 로컬 Git 환경에서 **winui3preview** 분기로 전환해야 합니다.

> `git checkout winui3preview`
