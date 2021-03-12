---
title: WinUI 3 프로젝트 리유니언 0.5 미리 보기(2021년 3월 출시 예정)
description: WinUI 3 프로젝트 리유니언 0.5 미리 보기의 개요를 설명합니다.
ms.date: 03/08/2021
ms.topic: article
ms.openlocfilehash: 8e5dea7ee18fe305cd9550ab42d30c561b9d803e
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629268"
---
# <a name="windows-ui-library-3---project-reunion-05-preview-march-2021"></a>Windows UI 라이브러리 3 - 프로젝트 리유니언 0.5 미리 보기(2021년 3월 출시 예정)

WinUI(Windows UI) 라이브러리 3은 최신 Windows 앱을 빌드하는 데 사용되는 네이티브 사용자 환경(UX) 플랫폼입니다. 데스크톱/Win32 및 UWP 앱과 함께 작동하며, WinUI 기반 사용자 인터페이스를 사용하여 앱 빌드를 시작하는 데 도움이 되는 Visual Studio 프로젝트 템플릿, 그리고 WinUI 라이브러리가 포함된 NuGet 패키지가 들어 있습니다.

**WinUI 3 - 프로젝트 리유니언 0.5 미리 보기** 는 프로젝트 리유니언 패키지의 일부로 제공되는 최초의 WinUI 3 릴리스입니다. 이번 변화와 함께, 이 미리 보기 릴리스에는 중요한 버그 수정, 향상된 안정성 및 몇 가지 일반적인 개선 사항이 포함되어 있습니다( **[WinUI 3 - 프로젝트 리유니언 0.5 미리 보기에 도입된 기능](#major-changes-introduced-in-this-release)** 참조).

> [!Important]
> WinUI 3 미리 보기의 목적은 개발자 커뮤니티에서 초기 평가를 받고 피드백을 수집하는 것입니다. 프로덕션 앱에는 사용하면 **안 됩니다**.
>
> 3월 말부터 프로젝트 리유니언 0.5를 제공할 수 있을 것으로 예상되며, 안정적으로 지원되는 첫 번째 WinUI 3 버전이 포함될 것입니다.
>
> [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)를 사용하여 피드백을 제공하고 제안 사항과 문제를 기록해 주세요.

## <a name="install-winui-3---project-reunion-05-preview"></a>WinUI 3 - 프로젝트 리유니언 0.5 미리 보기 설치

이 새 버전의 WinUI 3은 프로젝트 리유니언 0.5 미리 보기의 일부로 제공됩니다. 이 버전을 설치하려면 [프로젝트 리유니언 0.5 미리 보기 설치 지침](../../project-reunion/index.md#set-up-your-development-environment)을 참조하세요. 

WinUI 3의 이전 미리 보기 버전과 달리, WinUI VSIX 패키지 대신 프로젝트 리유니언 VSIX 패키지를 다운로드해야 합니다. 하지만 이 VSIX에는 이전 버전에서 사용할 수 있었던 것과 동일한 [WinUI 프로젝트 템플릿](#create-winui-projects)이 포함되어 있습니다. 설치를 완료한 후에는 WinUI 3 앱 개발 환경이 변경되지 않습니다.

> [!NOTE]
> [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-branch)의 WinUI 3 미리 보기 버전을 복제하고 빌드할 수도 있습니다.

> [!NOTE]
> 라이브 시각적 트리, 핫 다시 로드, 라이브 속성 탐색기와 같은 WinUI 3 도구를 사용하려면 [여기 지침](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)에 설명된 대로 Visual Studio 미리 보기 기능과 함께 WinUI 3 도구를 사용하도록 설정해야 합니다.

### <a name="webview2"></a>WebView2
이 WinUI 3 미리 보기에서 WebView2를 사용하고 싶은데 WebView2 런타임이 아직 설치되지 않은 경우 [이 페이지](https://developer.microsoft.com/microsoft-edge/webview2/)에서 제공하는 Evergreen Bootstrapper 또는 Evergreen Standalone Installer를 다운로드하세요. 

### <a name="windows-community-toolkit"></a>Windows 커뮤니티 도구 키트

Windows 커뮤니티 도구 키트를 사용하는 경우 [최신 버전을 다운로드](https://aka.ms/wct-winui3)합니다.

## <a name="create-winui-projects"></a>WinUI 프로젝트 만들기

프로젝트 리유니언 0.5 미리 보기 VSIX 패키지가 설치되면 Visual Studio에서 WinUI 프로젝트 템플릿 중 하나를 사용하여 새 프로젝트를 만들 수 있습니다. **새 프로젝트 만들기** 대화 상자에서 WinUI 프로젝트 템플릿에 액세스하려면 언어를 **C++** 또는 **C#** 으로, 플랫폼을 **Windows** 로, 프로젝트 형식을 **WinUI** 로 필터링합니다. 또는 *WinUI* 를 검색하여 사용 가능한 C# 또는 C++ 템플릿 중 하나를 선택할 수 있습니다.

![WinUI 프로젝트 템플릿](images/winui-projects-csharp.png)

WinUI 프로젝트 템플릿을 시작하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

- [데스크톱 앱용 WinUI 3 시작](get-started-winui3-for-desktop.md)
- [UWP 앱용 WinUI 3 시작](get-started-winui3-for-uwp.md)

[제한 사항 및 알려진 문제](#limitations-and-known-issues) 외에도 WinUI 프로젝트를 사용하여 앱을 빌드하는 것은 XAML 및 WinUI 2.x를 사용하여 UWP 앱을 빌드하는 것과 비슷합니다. 따라서 UWP 앱 및 Windows SDK의 **Windows.UI** WinRT 네임스페이스에 대한 대부분의 [지침 설명서](/windows/uwp/design/)를 적용할 수 있습니다.

WinUI 3 API 참조 설명서는 [WinUI 3 API 참조](/windows/winui/api)에서 찾을 수 있습니다.

WinUI 3 미리 보기 4를 사용하여 프로젝트를 만든 경우 프로젝트 리유니언 0.5 미리 보기를 사용하도록 프로젝트를 업그레이드할 수 있습니다. 자세한 지침은 [WinUI GitHub 리포지토리](https://aka.ms/winui3/upgrade-instructions)를 참조하세요.

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

핫 다시 로드, 라이브 시각적 트리 및 라이브 속성 탐색기와 같은 WinUI 3에 추가된 최신 도구 기능을 활용하려면 최신 WinUI 3 미리 보기와 함께 최신 미리 보기 버전의 Visual Studio를 사용하고 [여기 지침](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)에 설명된 대로 Visual Studio 미리 보기 기능의 WinUI 도구를 사용할 수 있도록 해야 합니다. 아래 표에서는 WinUI 3 - 프로젝트 리유니언 0.5 미리 보기와 이후 버전의 호환성을 보여줍니다.

| VS 버전  | WinUI 3 - 프로젝트 리유니언 0.5 미리 보기  |
|---|---|
| 16.8 RTM  | 아니요   |
| 16.9 Preview  | 예  | 
| 16.9 RTM  | 아니요   |
| 16.10 Preview  | 예   |

## <a name="major-changes-introduced-in-this-release"></a>이 릴리스의 주요 변경 내용

- WinUI 3은 이제 프로젝트 리유니언 패키지의 일부로 제공되며, 앞으로 출시될 지원 릴리스 역시 같은 방식으로 제공됩니다.

- 이제 앱 내 아크릴이 지원됩니다.

- 피벗 컨트롤은 더 이상 지원되지 않으며, WinUI 3에서는 더 이상 사용되지 않습니다. 앱 내 탐색 시나리오에는 [NavigationView 컨트롤](/windows/uwp/design/controls-and-patterns/navigationview)을 사용하는 것이 좋습니다.

- WinUI 3 및 프로젝트 리유니언은 하위 버전으로 Windows 10 버전 1809만 지원하며, 빌드 17763 이상이 필요합니다.

- 미리 보기 기능은 현재 실험적 기능으로 표시됩니다. 
  - 미리 보기 기능은 계속해서 WinUI 3 미리 보기에 포함되어 제공되지만, 다음 WinUI 3 지원 릴리스에는 포함되지 않습니다. 
  - 미리 보기 기능에는 WinUI 2.6 미리 보기에 포함된 실험적 API도 포함됩니다.
  - 미리 보기 기능을 사용하는 앱을 빌드할 때 앱에서 경고를 throw합니다. 


## <a name="list-of-bugs-fixed-in-winui-3---project-reunion-05-preview"></a>WinUI 3 - 프로젝트 리유니언 0.5 미리 보기에서 수정된 버그 목록

다음은 Preview 3 이후 팀에서 수정한 사용자에게 표시되는 버그 목록입니다. 또한 안정화 관련 및 테스트 개선을 위해 많은 작업이 진행되어 왔습니다.

- 수정이 필요한 WinUI 3 오류 메시지: "'Windows.metadata'를 확인할 수 없습니다.  Windows 소프트웨어 개발 키트를 설치하세요. Windows SDK는 Visual Studio와 함께 설치됩니다."
- Windows 기본 테마를 선택하면 앱을 다시 시작하기 전에는 앱이 Windows의 테마 변경에 응답하지 않음
- XamlDirect.CreateInstance 호출에서 예외 발생
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3509)를 제출해 주신 @BorzillaR에게 감사드립니다.
- ProgressBar가 일시 중지됨 옵션과 오류 옵션에서 차이가 없음
- StandardUICommand 목록 보기 항목 플라이아웃이 잘못된 위치에 표시됨
- 터치를 사용하여 ListView 항목의 순서를 변경하려고 할 때 데스크톱 XamlControlsGallery에서 충돌 발생
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3694)를 제출해 주신 @j0shuams에게 감사드립니다.
- RichTextBlock을 누르고 있으면 플라이아웃이 잘못된 위치에 배치됨
- 왼쪽/오른쪽 화살표를 눌러도 포커스가 RadioButtons를 통해 이동하지 않음
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3385)를 제출해 주신 @vmadurga에게 감사드립니다.
- 사용자가 DatePicker에서 다음/이전 월/년/일을 선택하기 위해 아래쪽/위쪽 화살표 키를 눌러도 내레이터가 응답하지 않음

- NavigationView 빠른 해제가 WinUI 3에서 작동하지 않음


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>이전의 WinUI 3 Preview에 도입된 새로운 특징 및 기능

다음 기능은 WinUI 3 미리 보기 1-4에서 도입되었으며 WinUI 3 - 프로젝트 리유니언 0.5 미리 보기에서도 계속 지원됩니다.

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
- MRT 핵심 지원
  - 이를 통해 시작 시 앱이 더 빠르고 가벼워지며, 리소스 조회가 더 빨라집니다.
- ARM64 지원
- 앱 내부 및 외부에서 끌어서 놓기
- RenderTargetBitmap(현재 XAML 콘텐츠만 - SwapChainPanel 콘텐츠 없음)
- 사용자 지정 커서 지원
- 오프 스레드 입력
- 향상된 도구/개발자 환경 기능:
  - 라이브 시각적 트리, 핫 다시 로드, 라이브 속성 탐색기 및 이와 비슷한 도구
  - WinUI 3 용 Intellisense
- 오픈 소스 마이그레이션에 필요한 향상된 기능
- 사용자 지정 제목 표시줄 기능: 개발자가 데스크톱 앱에서 사용자 지정 제목 표시줄을 만들 수 있는 새로운 [Window.ExtendsContentIntoTitleBar](/windows/winui/api/microsoft.ui.xaml.window.extendscontentintotitlebar) 및 [Window.SetTitleBar](/windows/winui/api/microsoft.ui.xaml.window.settitlebar) API
- VirtualSurfaceImageSource 지원

WinUI 3 및 WinUI 로드맵의 이점에 대한 자세한 내용은 GitHub의 [Windows UI 라이브러리 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)을 참조하세요.

### <a name="provide-feedback-and-suggestions"></a>피드백 및 제안 제공

[WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)에 대한 피드백을 환영합니다.

### <a name="whats-coming-next"></a>다음 단계

특정 기능이 WinUI 3에 도입되는 시점을 확인하려면 자세한 [기능 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)을 살펴보세요. 

## <a name="limitations-and-known-issues"></a>제한 사항 및 알려진 문제

WinUI 3 - 프로젝트 리유니언 0.5 미리 보기는 미리 보기일 뿐입니다. 특히 데스크톱 앱과 관련된 시나리오는 새로운 시나리오입니다. 버그, 제한 사항 및 기타 문제가 발생할 수도 있습니다.

다음은 WinUI 3 - 프로젝트 리유니언 0.5 미리 보기의 알려진 문제입니다. 아래에 나열되지 않은 문제가 발견되면 [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)를 통해 기존 문제에 참여하거나 새 문제를 제출하여 알려주세요.

### <a name="platform-and-os-support"></a>플랫폼 및 OS 지원

WinUI 3 - 프로젝트 리유니언 0.5 미리 보기는 Windows 10 2018년 10월 업데이트(버전 1809 - 빌드 17763) 이상을 실행하는 PC와 호환됩니다.

### <a name="developer-tools"></a>개발자 도구

- C# 및 C++/WinRT 앱만 지원됩니다.
- 데스크톱 앱은 .NET 5 및 C# 9를 지원하며 MSIX 앱에 패키지되어야 합니다.
- UWP 앱은 .NET 네이티브 및 C# 7.3을 지원합니다.
- 개발자 도구와 Intellisense가 Visual Studio에서 제대로 작동하지 않을 수 있습니다.
- XAML 디자이너 지원 기능이 없음
- 새 C++/CX 앱은 지원되지 않지만, 기존 앱은 계속 작동합니다(가능한 한 빨리 C++/WinRT로 이동).
- 데스크톱 앱에서 여러 창 지원을 진행하고 있지만 아직 불완전하고 안정적이지 않습니다.
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
- CoreWindow, ApplicationView, CoreApplicationView, CoreDispatcher 및 해당 종속성은 데스크톱 앱에서 지원되지 않습니다(아래 참조).

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>데스크톱 앱의 CoreWindow, ApplicationView, CoreApplicationView 및 CoreDispatcher

미리 보기 4 버전과 이후 표준 버전의 새 기능, [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow), [ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView), [CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
[CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) 및 해당 종속성은 데스크톱 앱에서 사용할 수 없습니다. 예를 들어 [Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 속성은 항상 null이지만, Window.DispatcherQueue 속성을 대안으로 사용할 수 있습니다.

이러한 API는 UWP 앱에서만 작동합니다. 과거 미리 보기에서는 데스크톱 앱에서도 부분적으로 작동했지만 미리 보기 4에서는 완전히 사용하지 않도록 설정되었습니다. 이러한 API는 스레드당 창이 하나만 있고 WinUI3의 기능 중 하나가 여러 개를 사용할 수 있는 UWP 사례를 위해 설계되었습니다.

이러한 API의 존재 여부에 내부적으로 의존하는 API가 있으므로 결과적으로 데스크톱 앱에서 지원되지 않습니다. 이러한 API에는 일반적으로 정적 `GetForCurrentView` 메서드가 있습니다. 예를 들어 [UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)가 있습니다.

영향을 받는 API와 이러한 API의 해결 방법 및 대안에 대한 자세한 내용은 [데스크톱 앱의 WinRT API 변경 내용](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md)을 참조하세요.

### <a name="known-issues"></a>알려진 문제

- Windows 10 버전 1809에서 UWP 앱이 시작되지 않는 문제가 있습니다. 다음 미리 보기 릴리스에서 이 버그를 수정하기 위해 담당 팀에서 열심히 작업 중입니다.

- Alt+F4 키를 눌러도 데스크톱 앱 창이 닫히지 않습니다.

- [UISettings.ColorValuesChanged 이벤트](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) 및 [AccessibilitySettings.HighContrastChanged 이벤트](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged)는 더 이상 데스크톱 앱에서 지원되지 않습니다. Windows 테마의 변경 사항을 감지하는 데 사용하는 경우 문제가 발생할 수 있습니다. 

- 이 릴리스에는 사용할 경우 빌드 경고를 발생시키는 몇 가지 실험적 API가 포함되어 있습니다. 팀에서 이를 철저히 테스트하지 않아 알 수 없는 문제가 있을 수 있습니다. 문제가 발생하면 리포지토리에서 [버그를 신고](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=)하세요. 

- 이전에는 CompositionCapabilities 인스턴스를 가져오려면 [CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)를 호출했습니다. 그러나 이 호출에서 반환된 기능은 보기에 종속되지 *않았습니다*. 이 문제를 해결하고 반영하기 위해 이번 릴리스에서 GetForCurrentView() static을 삭제했으므로 이제 [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) 객체를 직접 만들 수 있습니다.

- C#UWP 앱의 경우:

  WinUI 3 프레임워크는 C++(C++/WinRT 사용) 또는 C#에서 사용할 수 있는 WinRT 구성 요소 세트입니다. C#을 사용하는 경우 앱 모델에 따라 두 가지 버전의 .NET이 있습니다. 즉, UWP 앱에서 WinUI 3을 사용하는 경우 .NET 네이티브를 사용하고, 데스크톱 앱에서 사용하는 경우 .NET 5(및 C#/WinRT)를 사용합니다.

  UWP에서 C#을 WinUI 3 앱에 사용하는 경우 WinUI 3 데스크톱 앱 또는 C# WinUI 2 앱의 C#과 비교하면 API 네임스페이스에 대한 몇 가지 차이점이 있습니다. 즉, 일부 형식이 `System` 네임스페이스가 아니라 `Microsoft` 네임스페이스에 있습니다. 예를 들어 `INotifyPropertyChanged` 인터페이스는 `System.ComponentModel` 네임스페이스에 있지 않고 `Microsoft.UI.Xaml.Data` 네임스페이스에 있습니다. 

  적용 대상:
    - `INotifyPropertyChanged` (및 관련 유형)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 네임스페이스 버전은 여전히 있지만 WinUI 3에서 사용할 수 없습니다. 즉, `ObservableCollection`이 WinUI 3 C# UWP 앱에서 있는 그대로 작동하지 않습니다. 해결 방법은 [XAML Controls Gallery 샘플](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)의 [CollectionsInterop 샘플](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)을 참조하세요.

## <a name="xaml-controls-gallery-winui-3-preview-branch"></a>XAML Controls Gallery(WinUI 3 미리 보기 분기)

WinUI 3 - 프로젝트 리유니언 0.5 미리 보기에서 제공하는 모든 컨트롤과 기능이 들어 있는 샘플 앱은 [XAML Controls Gallery의 WinUI 3 미리 보기 분기](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)를 참조하세요.

![WinUI 3 미리 보기 XAML Controls Gallery 앱](images/WinUI3XamlControlsGallery.png)<br/>
*WinUI 3 미리 보기 XAML Controls Gallery 앱의 예*

샘플을 다운로드하려면 다음 명령을 사용하여 **winui3preview** 분기를 복제합니다.

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

복제 후, 로컬 Git 환경에서 **winui3preview** 분기로 전환해야 합니다. 

```
git checkout winui3preview
```
