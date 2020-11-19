---
title: WinUI 3 Preview 3(2020년 11월)
description: WinUI 3 Preview 3 릴리스에 대한 개요입니다.
ms.date: 11/17/2020
ms.topic: article
ms.openlocfilehash: d2ff1646c431ef1f79455260a61027d0a84f77ca
ms.sourcegitcommit: f723edbe3dc846c1988d721f6e8078aaec371899
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94701969"
---
# <a name="windows-ui-library-3-preview-3-november-2020"></a>Windows UI 라이브러리 3 Preview 3(2020년 11월)

WinUI(Windows UI) 라이브러리 3은 Windows 데스크톱 및 UWP 앱 모두에 대한 네이티브 UX(사용자 환경) 프레임워크입니다.

**WinUI 3 Preview 3** 은 중요한 버그 수정과 함께 새로운 기능과 향상된 기능을 모두 제공합니다.

**[Preview 3 제한 사항 및 알려진 문제](#preview-3-limitations-and-known-issues)를 참조하세요**.

> [!Important]
> 이 WinUI 3 Preview 릴리스는 개발자 커뮤니티에서 초기에 평가하고 피드백을 수집하기 위한 것입니다. 프로덕션 앱에는 사용하면 **안 됩니다**.
>
> WinUI 3의 미리 보기 릴리스는 2021년까지 계속 제공되며, 이후에 첫 번째 공식 릴리스를 제공할 예정입니다.
>
> [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)를 사용하여 피드백을 제공하고 제안 사항과 문제를 기록해 주세요.

## <a name="install-winui-3-preview-3"></a>WinUI 3 Preview 3 설치

WinUI 3 Preview 3에는 WinUI 기반 사용자 인터페이스를 사용하여 앱 빌드를 시작하는 데 도움이 되는 Visual Studio 프로젝트 템플릿 및 WinUI 라이브러리가 포함된 NuGet 패키지가 포함되어 있습니다. WinUI 3 Preview 3을 설치하려면 다음 단계를 수행합니다.

> [!NOTE]
> [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-3-branch)의 WinUI 3 Preview 3 버전을 복제하고 빌드할 수도 있습니다.

1. Windows 10, 버전 1803(빌드 17134) 이상이 개발 컴퓨터에 설치되어 있는지 확인합니다.

2. [Visual Studio 2019 버전 16.9 Preview](https://visualstudio.microsoft.com/vs/preview/)를 설치합니다.

    Visual Studio를 설치할 때 다음 워크로드를 포함해야 합니다.
    - .NET 데스크톱 개발(.NET 5도 설치됨)
    - 유니버설 Windows 플랫폼 개발

    C++ 앱을 빌드하려면 다음 워크로드도 포함해야 합니다.
    - C++를 사용한 데스크톱 개발
    - 유니버설 Windows 플랫폼 워크로드용 선택적 *C++(v142) 유니버설 Windows 플랫폼 도구* 구성 요소(오른쪽 창에 있는 "유니버설 Windows 플랫폼 개발" 섹션의 "설치 세부 정보" 참조)
3. **nuget.org** 에 사용하도록 설정된 NuGet 패키지 원본이 시스템에 있는지 확인합니다. 자세한 내용은 [일반적인 NuGet 구성](/nuget/consume-packages/configuring-nuget-behavior)을 참조하십시오.

4. [WinUI 3 Preview 3 VSIX 패키지](https://aka.ms/winui3/preview3-download)를 다운로드하여 설치합니다. 그러면 WinUI 3 프로젝트 템플릿 및 WinUI 3 라이브러리가 포함된 NuGet 패키지가 모두 Visual Studio 2019에 추가됩니다.

    VSIX 패키지를 Visual Studio에 추가하는 방법에 대한 지침은 [Visual Studio 확장 찾기 및 사용](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)을 참조하세요.

5. 앱에서 WebView2 컨트롤을 사용하는 경우 [Microsoft Edge Insider Channels](https://www.microsoftedgeinsider.com/en-us/download)에서 Microsoft Edge 브라우저의 Dev 채널 버전을 설치하세요. Microsoft Edge Beta, Microsoft Edge Dev 및 Microsoft Edge WebView2 런타임의 기존 인스턴스를 모두 제거해야 합니다.

6. Windows 커뮤니티 도구 키트를 사용하는 경우 [최신 버전을 다운로드](https://aka.ms/wct-winui3)합니다.

## <a name="create-winui-projects"></a>WinUI 프로젝트 만들기

WinUI 3 Preview 3 VSIX 패키지가 설치되면 Visual Studio에서 WinUI 프로젝트 템플릿 중 하나를 사용하여 새 프로젝트를 만들 준비가 되었습니다. **새 프로젝트 만들기** 대화 상자에서 WinUI 프로젝트 템플릿에 액세스하려면 언어를 **C++** 또는 **C#** 으로, 플랫폼을 **Windows** 로, 프로젝트 형식을 **WinUI** 로 필터링합니다. 또는 *WinUI* 를 검색하여 사용 가능한 C# 또는 C++ 템플릿 중 하나를 선택할 수 있습니다.

![WinUI 프로젝트 템플릿](images/winui-projects-csharp.png)

WinUI 프로젝트 템플릿을 시작하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

- [데스크톱 앱용 WinUI 3 시작](get-started-winui3-for-desktop.md)
- [UWP 앱용 WinUI 3 시작](get-started-winui3-for-uwp.md)

[제한 사항 및 알려진 문제](#preview-3-limitations-and-known-issues) 외에도 WinUI 프로젝트를 사용하여 앱을 빌드하는 것은 XAML 및 WinUI 2.x를 사용하여 UWP 앱을 빌드하는 것과 비슷합니다. 따라서 UWP 앱 및 Windows SDK의 **Windows.UI** WinRT 네임스페이스에 대한 대부분의 [지침 설명서](/windows/uwp/design/)를 적용할 수 있습니다.

이 릴리스에는 WinUI 3으로 이식된 모든 WinRT API에 대해 [WinUI 3 API 참조 설명서](/uwp/api/overview/winui/)도 추가되었습니다.

WinUI 3 Preview 2를 사용하여 프로젝트를 만든 경우 Preview 3을 사용하도록 프로젝트를 업그레이드할 수 있습니다. 자세한 지침은 [WinUI GitHub 리포지토리](https://aka.ms/winui3/upgrade-instructions)를 참조하세요.

### <a name="project-templates-for-winui-3"></a>WinUI 3용 프로젝트 템플릿

앱을 만드는 데 사용할 수 있는 WinUI 프로젝트 템플릿은 다음과 같습니다.

| 템플릿 | Language | 설명 |
|----------|----------|-------------|
| 비어 있는 앱, 패키지됨(데스크톱의 WinUI) | C# 및 C++ | WinUI 기반 사용자 인터페이스를 사용하여 데스크톱 .NET 5(C#) 또는 네이티브 Win32(C++) 앱을 만듭니다. 생성되는 프로젝트에는 UI 빌드를 시작하는 데 사용할 수 있는 WinUI 라이브러리의 **Microsoft.UI.Xaml.Window** 클래스에서 파생되는 기본 창이 포함됩니다. 이 프로젝트 형식에 대한 자세한 내용은 [데스크톱 앱용 WinUI 3 시작](get-started-winui3-for-desktop.md)을 참조하세요.<p></p>이 솔루션에는 앱을 [MSIX 패키지](/windows/msix/overview)로 빌드하도록 구성된 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)도 포함됩니다. 이는 최신 배포 환경, 패키지 확장을 통해 Windows 10 기능과 통합하는 기능 등을 제공합니다.  |
| 빈 앱(UWP의 WinUI)  | C# 및 C++ | WinUI 기반 사용자 인터페이스를 사용하여 UWP 앱을 만듭니다. 생성되는 프로젝트에는 UI 빌드를 시작하는 데 사용할 수 있는 WinUI 라이브러리의 **Microsoft.UI.Xaml.Controls.Page** 클래스에서 파생되는 기본 페이지가 포함됩니다. 이 프로젝트 형식에 대한 자세한 내용은 [UWP 앱용 WinUI 3 시작](get-started-winui3-for-uwp.md)을 참조하세요. |

WinUI 기반 앱에서 로드하고 사용할 수 있는 구성 요소를 빌드하는 데 사용할 수 있는 WinUI 프로젝트 템플릿은 다음과 같습니다.

| 템플릿 | Language | 설명 |
|----------|----------|-------------|
| 클래스 라이브러리(데스크톱의 WinUI) | C#만 | WinUI 기반 사용자 인터페이스를 사용하여 다른 .NET 5 데스크톱 앱에서 사용할 수 있는 C#에서 .NET 5 관리형 클래스 라이브러리(DLL)를 만듭니다.  |
| 클래스 라이브러리(UWP의 WinUI)  | C#만 | WinUI 기반 사용자 인터페이스를 사용하여 다른 UWP 앱에서 사용할 수 있는 C#에서 관리형 클래스 라이브러리(DLL)를 만듭니다. |
| Windows 런타임 구성 요소(WinUI) | C++ | 앱이 작성된 프로그래밍 언어에 관계없이 WinUI 기반 사용자 인터페이스가 있는 모든 UWP 또는 데스크톱 앱에서 사용할 수 있는 C++/WinRT로 작성된 [Windows 런타임 구성 요소](/windows/uwp/winrt-components/)를 만듭니다. |
| Windows 런타임 구성 요소(UWP) | C# | 앱이 작성된 프로그래밍 언어에 관계없이 WinUI 기반 사용자 인터페이스가 있는 모든 UWP 앱에서 사용할 수 있는 C#으로 작성된 [Windows 런타임 구성 요소](/windows/uwp/winrt-components/)를 만듭니다. |

### <a name="item-templates-for-winui-3"></a>WinUI 3용 항목 템플릿

WinUI 프로젝트에서 사용할 수 있는 항목 템플릿은 다음과 같습니다. 이러한 WinUI 항목 템플릿에 액세스하려면 **솔루션 탐색기** 에서 마우스 오른쪽 단추로 프로젝트 노드를 클릭하고, **추가** -> **새 항목** 을 차례로 선택하고, **새 항목 추가** 대화 상자에서 **WinUI** 를 클릭합니다.

![WinUI 항목 템플릿](images/winui-items-csharp.png)

| 템플릿 | Language | 설명 |
|----------|----------|-------------|
| 빈 페이지(WinUI) | C# 및 C++ | WinUI 라이브러리의 **Microsoft.UI.Xaml.Controls.Page** 클래스에서 파생되는 새 페이지를 정의하는 XAML 파일 및 코드 파일을 추가합니다. |
| 빈 창(데스크톱의 WinUI) | C# 및 C++ | WinUI 라이브러리의 **Microsoft.UI.Xaml.Window** 클래스에서 파생되는 새 창을 정의하는 XAML 파일 및 코드 파일을 추가합니다. |
| 사용자 지정 컨트롤(WinUI) | C# 및 C++ | 기본 스타일을 사용하여 템플릿 기반 컨트롤을 만드는 코드 파일을 추가합니다. 템플릿 기반 컨트롤은 WinUI 라이브러리의 **Microsoft.UI.Xaml.Controls.Control** 클래스에서 파생됩니다.<p></p>이 항목 템플릿을 사용하는 방법을 보여주는 연습은 [C++/WinRT를 사용한 UWP 및 WinUI 3 앱용 템플릿 XAML 컨트롤](xaml-templated-controls-cppwinrt-winui-3.md) 및 [C#을 사용한 UWP 및 WinUI 3 앱용 템플릿 XAML 컨트롤](xaml-templated-controls-csharp-winui-3.md)을 참조하세요. 템플릿 기반 컨트롤에 대한 자세한 내용은 [사용자 지정 XAML 컨트롤](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)을 참조하세요. |
| 리소스 사전(WinUI) | C# 및 C++ | XAML 리소스의 빈 키 컬렉션을 추가합니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)를 확인하세요. |
| 리소스 파일(WinUI) | C# 및 C++ | 앱에 대한 문자열 및 조건부 리소스를 저장하는 파일을 추가합니다. 이 항목을 사용하여 앱을 지역화할 수 있습니다. 자세한 내용은 [UI 및 앱 패키지 매니페스트의 문자열 지역화](/windows/uwp/app-resources/localize-strings-ui-manifest)를 참조하세요. |
| 사용자 지정 컨트롤(WinUI) | C# 및 C++ | WinUI 라이브러리의 **Microsoft.UI.Xaml.Controls.UserControl** 클래스에서 파생되는 사용자 지정 컨트롤을 만드는 XAML 파일 및 코드 파일을 추가합니다. 일반적으로 사용자 지정 컨트롤은 관련된 기존 컨트롤을 캡슐화하고 자체 논리를 제공합니다.<p></p>사용자 지정 컨트롤에 대한 자세한 내용은 [사용자 지정 XAML 컨트롤](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)을 참조하세요. |

### <a name="visual-studio-support"></a>Visual Studio 지원

핫 다시 로드, 라이브 시각적 트리 및 라이브 속성 탐색기와 같은 WinUI 3 Preview 3에 추가된 최신 도구 기능을 활용하려면 최신 WinUI 3 미리 보기와 함께 최신 미리 보기 버전의 Visual Studio를 사용해야 합니다. 아래 표에서는 WinUI 3 Preview 3 및 이후 버전과의 호환성을 보여 줍니다.

| VS 버전  | WinUI 3 Preview 3  |
|---|---|
| 16.8 RTM  | 아니요   |
| 16.9 Preview  | 예  | 
| 16.9 RTM  | 아니요   |
| 16.10 Preview  | 예   |


## <a name="new-features-and-capabilities-in-preview-3"></a>Preview 3의 새로운 특징 및 기능

- ARM64 지원
- 앱 내부 및 외부에서 끌어서 놓기
- RenderTargetBitmap(현재 XAML 콘텐츠만 - SwapChainPanel 콘텐츠 없음)
- 향상된 도구/개발자 환경 기능:
  - 라이브 시각적 트리, 핫 다시 로드, 라이브 속성 탐색기 및 이와 비슷한 도구
  - Intellisense가 이제 WinUI 3에서 작동함 
- MRT 핵심 지원
  - 이를 통해 시작 시 앱이 더 빠르고 가벼워지며, 리소스 조회가 더 빨라집니다.
- 사용자 지정 커서 지원
- 오프 스레드 입력
- Preview 2 이후 성능 향상
- 데스크톱 앱의 여러 창 - Preview 2 이후 향상된 기능 지원


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>이전의 WinUI 3 Preview에 도입된 새로운 특징 및 기능

WinUI 3 Preview 1 및 2에서 도입되었고 WinUI 3 Preview 3에서도 계속 지원되는 특징과 기능은 다음과 같습니다.

- Win32 앱용 [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)를 포함하여 WinUI를 사용하여 데스크톱 앱을 만드는 기능
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView 업데이트](/windows/uwp/design/controls-and-patterns/tab-view)
- 어두운 테마 업데이트
- [WebView2](/microsoft-edge/hosting/webview2)의 향상된 기능 및 업데이트
  - 높은 DPI 지원
  - 창 크기 조정 및 이동 지원
  - 최신 버전의 Edge를 대상으로 업데이트됨
  - 더 이상 WebView2별 Nuget 패키지를 참조할 필요 없음
- SwapChainPanel
- 오픈 소스 마이그레이션에 필요한 향상된 기능

WinUI 3 및 WinUI 로드맵의 이점에 대한 자세한 내용은 GitHub의 [Windows UI 라이브러리 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)을 참조하세요.

### <a name="provide-feedback-and-suggestions"></a>피드백 및 제안 제공

[WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)에 대한 피드백을 환영합니다.

### <a name="whats-coming-next"></a>다음 단계

특정 기능이 WinUI 3에 도입되는 시점을 확인하려면 자세한 [기능 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)을 살펴보세요. 

## <a name="preview-3-limitations-and-known-issues"></a>Preview 3 제한 사항 및 알려진 문제

Preview 3 릴리스는 미리 보기일 뿐입니다. 특히 데스크톱 앱과 관련된 시나리오는 새로운 시나리오입니다. 버그, 제한 사항 및 기타 문제가 발생할 수도 있습니다.

WinUI 3 Preview 3에서 알려진 문제 중 일부는 다음 항목과 같습니다. 아래에 나열되지 않은 문제가 발견되면 [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)를 통해 기존 문제에 참여하거나 새 문제를 제출하여 알려주세요.

### <a name="platform-and-os-support"></a>플랫폼 및 OS 지원

WinUI 3 Preview 3은 Windows 10 2018년 4월 업데이트(버전 1803 - 빌드 17134) 이상을 실행하는 PC와 호환됩니다.

### <a name="developer-tools"></a>개발자 도구

- C# 및 C++/WinRT 앱만 지원됩니다.
- 데스크톱 앱은 .NET 5 및 C# 9를 지원하며 MSIX 앱에 패키지되어야 합니다.
- UWP 앱은 .NET 네이티브 및 C# 7.3을 지원합니다.
- 개발자 도구와 Intellisense가 Visual Studio 버전 16.8에서 제대로 작동하지 않을 수 있습니다.
- 새 C++/CX 앱은 지원되지 않지만, 기존 앱은 계속 작동합니다(가능한 한 빨리 C++/WinRT로 이동).
- 데스크톱 앱에서 여러 창 지원을 진행하고 있지만 아직 완전하고 안정적이지 않습니다.
  - 여러 창 동작으로 인한 새로운 문제 또는 재발을 발견하면 리포지토리에서 버그를 제출해 주세요.
- 패키지되지 않은 데스크톱 배포는 지원되지 않습니다.
- F5 키를 사용하여 데스크톱 앱을 실행하는 경우 패키징 프로젝트를 실행하고 있는지 확인합니다. 앱 프로젝트에서 F5 키를 누르면 WinUI 3에서 아직 지원하지 않는 패키지되지 않은 앱이 실행됩니다.

### <a name="missing-platform-features"></a>플랫폼 기능 누락

- Xbox 지원
- HoloLens 지원
- 창 팝업
  - 구체적으로 `ShouldConstrainToRootBounds` 속성은 속성 값에 관계없이 항상 `true`로 설정된 것처럼 작동합니다.
- 수동 입력 지원
- 아크릴
- MediaElement 및 MediaPlayerElement
- MapControl
- SwapChainPanel 및 비 XAML 콘텐츠에 대한 RenderTargetBitmap
- SwapChainPanel은 투명성을 지원하지 않습니다.
- 글로벌 표시는 솔리드 브러시인 폴백 동작을 사용합니다.
- XAML Islands는 이 릴리스에서 지원되지 않습니다.
- 타사 에코시스템 라이브러리는 완전하게 작동하지 않습니다.
- IME가 작동하지 않습니다.

### <a name="known-issues"></a>알려진 문제

- Alt+F4는 데스크톱 앱 창을 닫지 않습니다.

-   앱에서 WebView2를 사용하지만 이를 렌더링하거나 로드하지 않는 경우 호환되지 않는 버전의 Edge 브라우저가 실행되고 있을 수 있습니다. [Microsoft Edge의 Dev 채널을 다운로드](https://www.microsoftedgeinsider.com/en-us/download)하고, Microsoft Edge Beta, Microsoft Edge Dev 및 Microsoft Edge WebView2 런타임의 기존 인스턴스를 모두 제거해야 합니다.

- Marshal 함수는 C#/WinRT와 올바르게 상호 운용되지 않으므로 .NET 5 WinUI 앱에서 사용하면 안 됩니다. 자세한 내용은 [이 페이지](https://github.com/microsoft/CsWinRT/blob/master/docs/interop.md)를 참조하세요.

- 예를 들어 Xaml 태그에서 `{Binding}`을 사용하는 것과 같이 URI 속성을 설정하여 작동이 중단되는 경우 `{x:Bind}`를 사용하거나 미리 보기 버전의 C#/WinRT를 사용하여 문제를 해결할 수 있습니다. 이렇게 하려면 다음 줄을 .csproj 파일에 추가합니다.

  ```xml
  <ItemGroup>
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        RuntimeFrameworkVersion="10.0.18362.11-preview" />
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        TargetingPackVersion="10.0.18362.11-preview" />
  </ItemGroup>
  ```

- C#UWP 앱의 경우:

  WinUI 3 프레임워크는 C++(C++/WinRT 사용) 또는 C#에서 사용할 수 있는 WinRT 구성 요소 세트입니다. C#을 사용하는 경우 앱 모델에 따라 두 가지 버전의 .NET이 있습니다. 즉, UWP 앱에서 WinUI 3을 사용하는 경우 .NET 네이티브를 사용하고, 데스크톱 앱에서 사용하는 경우 .NET 5(및 C#/WinRT)를 사용합니다.

  UWP에서 C#을 WinUI 3 앱에 사용하는 경우 WinUI 3 데스크톱 앱 또는 C# WinUI 2 앱의 C#과 비교하면 API 네임스페이스에 대한 몇 가지 차이점이 있습니다. 즉, 일부 형식이 `System` 네임스페이스가 아니라 `Microsoft` 네임스페이스에 있습니다. 예를 들어 `INotifyPropertyChanged` 인터페이스는 `System.ComponentModel` 네임스페이스에 있지 않고 `Microsoft.UI.Xaml.Data` 네임스페이스에 있습니다. 

  적용 대상:
    - `INotifyPropertyChanged` (및 관련 유형)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 네임스페이스 버전은 여전히 있지만 WinUI 3에서 사용할 수 없습니다. 즉, `ObservableCollection`이 WinUI 3 C# UWP 앱에서 있는 그대로 작동하지 않습니다. 해결 방법은 [XAML Controls Gallery 샘플](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)의 [CollectionsInterop 샘플](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)을 참조하세요.

## <a name="xaml-controls-gallery-winui-3-preview-3-branch"></a>XAML Controls Gallery(WinUI 3 Preview 3 분기)

모든 WinUI 3 Preview 3 컨트롤 및 기능이 포함된 앱 샘플은 [XAML Controls Gallery의 WinUI 3 Preview 3 분기](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)를 참조하세요.

![WinUI 3 Preview 3 XAML Controls Gallery 앱](images/WinUI3XamlControlsGalleryP3.PNG)<br/>
*WinUI 3 Preview 3 XAML Controls Gallery 앱의 예*

샘플을 다운로드하려면 다음 명령을 사용하여 **winui3preview** 분기를 복제합니다.

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

복제 후, 로컬 Git 환경에서 **winui3preview** 분기로 전환해야 합니다. 

```
git checkout winui3preview
```
