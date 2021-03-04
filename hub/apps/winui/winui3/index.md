---
title: WinUI 3 Preview 4(2021년 2월)
description: WinUI 3 Preview 4 릴리스에 대한 개요입니다.
ms.date: 02/09/2021
ms.topic: article
ms.openlocfilehash: a6c74ac64e3384b5a1f5cdc466b4faf441f14445
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824087"
---
# <a name="windows-ui-library-3-preview-4-february-2021"></a>Windows UI 라이브러리 3 Preview 4(2021년 2월)

WinUI(Windows UI) 라이브러리 3은 Windows 데스크톱 및 UWP 앱 모두에 대한 네이티브 UX(사용자 환경) 프레임워크입니다.

**WinUI 3 Preview 4** 는 중요한 버그 수정 및 기타 일반적인 개선 사항이 포함된 안정성 미리 보기 릴리스입니다( **[Preview 4에 도입된 기능](#capabilities-introduced-in-preview-4)** 참조).

> [!Important]
> 이 WinUI 3 Preview 릴리스는 개발자 커뮤니티에서 초기에 평가하고 피드백을 수집하기 위한 것입니다. 프로덕션 앱에는 사용하면 **안 됩니다**.
>
> WinUI 3의 미리 보기 릴리스는 2021년까지 계속 제공되며, 2021년 3월에 첫 번째 공식 지원 릴리스를 출시할 것입니다.
>
> [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)를 사용하여 피드백을 제공하고 제안 사항과 문제를 기록해 주세요.

## <a name="install-winui-3-preview-4"></a>WinUI 3 Preview 4 설치

WinUI 3 Preview 4에는 WinUI 기반 사용자 인터페이스를 사용하여 앱 빌드를 시작하는 데 도움이 되는 Visual Studio 프로젝트 템플릿 및 WinUI 라이브러리가 포함된 NuGet 패키지가 포함되어 있습니다. WinUI 3 Preview 4를 설치하려면 다음 단계를 수행합니다.

> [!NOTE]
> [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-4-branch)의 WinUI 3 Preview 4 버전을 복제하고 빌드할 수도 있습니다.

1. Windows 10, 버전 1803(빌드 17134) 이상이 개발 컴퓨터에 설치되어 있는지 확인합니다.

2. [Visual Studio 2019 버전 16.9 Preview](https://visualstudio.microsoft.com/vs/preview/)를 설치합니다. 워크로드(예: .NET 5)에 필요한 모든 업데이트를 받으려면 **최신 미리 보기** 를 다운로드합니다.

    Visual Studio를 설치할 때 다음 워크로드를 포함해야 합니다.
    - 유니버설 Windows 플랫폼 개발

    .NET 앱을 빌드하려면 다음 워크로드도 포함해야 합니다.
    - .NET 데스크톱 개발(필요한 최신 버전의 .NET 5도 설치됨)

    C++ 앱을 빌드하려면 다음 워크로드도 포함해야 합니다.
    - C++를 사용한 데스크톱 개발
    - 유니버설 Windows 플랫폼 워크로드용 선택적 *C++(v142) 유니버설 Windows 플랫폼 도구* 구성 요소(오른쪽 창에 있는 "유니버설 Windows 플랫폼 개발" 섹션의 "설치 세부 정보" 참조)

3. **nuget.org** 에 사용하도록 설정된 NuGet 패키지 원본이 시스템에 있는지 확인합니다. 자세한 내용은 [일반적인 NuGet 구성](/nuget/consume-packages/configuring-nuget-behavior)을 참조하십시오.

4. [WinUI 3 Preview 4 VSIX 패키지](https://aka.ms/winui3/preview4-download)를 다운로드하여 설치합니다. 그러면 WinUI 3 프로젝트 템플릿 및 WinUI 3 라이브러리가 포함된 NuGet 패키지가 모두 Visual Studio 2019에 추가됩니다.

    VSIX 패키지를 Visual Studio에 추가하는 방법에 대한 지침은 [Visual Studio 확장 찾기 및 사용](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)을 참조하세요.

5. 라이브 시각적 트리, 핫 다시 로드, 라이브 속성 탐색기와 같은 WinUI 3 도구를 사용하려면 [여기 지침](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)에 설명된 대로 Visual Studio 미리 보기 기능과 함께 WinUI 3 도구를 사용하도록 설정해야 합니다.

#### <a name="webview2"></a>WebView2
WebView2를 WinUI 3 Preview 4와 함께 사용하려면 [이 페이지](https://developer.microsoft.com/microsoft-edge/webview2/)에 있는 Evergreen Bootstrapper 또는 Evergreen Standalone Installer를 다운로드하세요. 

#### <a name="windows-community-toolkit"></a>Windows 커뮤니티 도구 키트

Windows 커뮤니티 도구 키트를 사용하는 경우 [최신 버전을 다운로드](https://aka.ms/wct-winui3)합니다.

## <a name="create-winui-projects"></a>WinUI 프로젝트 만들기

WinUI 3 Preview 4 VSIX 패키지가 설치되면 Visual Studio에서 WinUI 프로젝트 템플릿 중 하나를 사용하여 새 프로젝트를 만들 준비가 되었습니다. **새 프로젝트 만들기** 대화 상자에서 WinUI 프로젝트 템플릿에 액세스하려면 언어를 **C++** 또는 **C#** 으로, 플랫폼을 **Windows** 로, 프로젝트 형식을 **WinUI** 로 필터링합니다. 또는 *WinUI* 를 검색하여 사용 가능한 C# 또는 C++ 템플릿 중 하나를 선택할 수 있습니다.

![WinUI 프로젝트 템플릿](images/winui-projects-csharp.png)

WinUI 프로젝트 템플릿을 시작하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

- [데스크톱 앱용 WinUI 3 시작](get-started-winui3-for-desktop.md)
- [UWP 앱용 WinUI 3 시작](get-started-winui3-for-uwp.md)

[제한 사항 및 알려진 문제](#limitations-and-known-issues) 외에도 WinUI 프로젝트를 사용하여 앱을 빌드하는 것은 XAML 및 WinUI 2.x를 사용하여 UWP 앱을 빌드하는 것과 비슷합니다. 따라서 UWP 앱 및 Windows SDK의 **Windows.UI** WinRT 네임스페이스에 대한 대부분의 [지침 설명서](/windows/uwp/design/)를 적용할 수 있습니다.

이 릴리스에 대한 API 참조 문서가 곧 제공될 예정입니다. 링크는 사용할 수 있게 되면 여기에 제공됩니다. 그 동안에는 [Preview 3용 WinUI 3 API 참조 문서](/windows/winui/api/)를 자유롭게 살펴보세요.

WinUI 3 Preview 3을 사용하여 프로젝트를 만든 경우 Preview 4를 사용하도록 프로젝트를 업그레이드할 수 있습니다. 자세한 지침은 [WinUI GitHub 리포지토리](https://aka.ms/winui3/upgrade-instructions)를 참조하세요.

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

핫 다시 로드, 라이브 시각적 트리 및 라이브 속성 탐색기와 같은 WinUI 3 Preview 4에 추가된 최신 도구 기능을 활용하려면 최신 WinUI 3 미리 보기와 함께 최신 미리 보기 버전의 Visual Studio를 사용하고 [여기 지침](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)에 설명된 대로 Visual Studio 미리 보기 기능의 WinUI 도구를 사용할 수 있도록 해야 합니다. 아래 표에서는 WinUI 3 Preview 4 및 이후 버전과의 호환성을 보여줍니다.

| VS 버전  | WinUI 3 Preview 4  |
|---|---|
| 16.8 RTM  | 아니요   |
| 16.9 Preview  | 예  | 
| 16.9 RTM  | 아니요   |
| 16.10 Preview  | 예   |

## <a name="capabilities-introduced-in-preview-4"></a>Preview 4에 도입된 기능

- WinUI 2.5와 패리티(InfoBar 컨트롤, ProgressRing 및 NavigationView의 새로운 기능, 버그 수정 포함)
- 사용자 지정 제목 표시줄 기능: 개발자가 데스크톱 앱에서 사용자 지정 제목 표시줄을 만들 수 있는 새로운 Window.ExtendsContentIntoTitleBar 및 Window.SetTitleBar API.
- VirtualSurfaceImageSource 지원

## <a name="list-of-bugs-fixed-in-preview-4"></a>Preview 4에서 수정된 버그 목록

다음은 Preview 3 이후 팀에서 수정한 사용자에게 표시되는 버그 목록입니다. 또한 안정화 관련 및 테스트 개선을 위해 많은 작업이 진행되어 왔습니다.

- 이 릴리스는 다음 버그를 해결한 새 버전의 CS/WinRT 및 Windows SDK에서 수행되었습니다.
  - {Binding}을 사용하여 URI 속성에 바인딩할 때 충돌 발생
  - C#/WinRT Marshal 함수가 .NET 5와 올바르게 상호 운용되지 않음

- Windows 참가자 빌드에서 실행할 때 WinUI 3 충돌 발생
  - GitHub에서 이 버그를 보고해 주신 여러 커뮤니티 기여자에게 감사드립니다. 
- WebView2는 호스트 앱의 언어/로캘을 CoreWebView2Environment에 적용하지 않음
- Windows Community Toolkit DataGrid 컨트롤이 시작 시/스크롤 막대가 표시될 때 앱 충돌 발생
  - GitHub에서 이 버그를 보고해 주신 여러 커뮤니티 기여자에게 감사드립니다.
- 디스플레이 모드가 변경되면 페이지 렌더링이 잘못된 상태가 됨
- CalendarView에서 Language ComboBox를 사용할 때 충돌 발생
- WinUI 3 데스크톱: WebView2에서 탭할 수 없음
- WinUI 3 데스크톱: 파생된 TreeViewNodes가 있는 TreeView가 충돌 발생 
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2699)를 제출해 주신 @eleanorleffler에게 감사드립니다.
- WinUI 3 데스크톱: ContentDialog 내에서 TextBox에 텍스트를 입력할 수 없음 
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2704)를 제출해 주신 @eleanorleffler에게 감사드립니다.
- WinUI 3 데스크톱: ALT 및 F6이 작동하지 않음
- 이전에 제거된 SwapChainPanel이 새 SwapChain 위에 렌더링됨
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2942)를 제출해 주신 @dotMorten에게 감사드립니다.
- WinUI 3 데스크톱: 트랙 패드로 스크롤할 수 없음
- 동일한 스레드의 여러 창에서 NavigationView 컨트롤을 사용할 때 충돌 발생
- 접근성 문제: WinUI 데스크톱 앱 시작 시 포커스 rect 표시
- DataGrid에서 스크롤하는 동안 액세스 위반
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2946)를 제출해 주신 @TroelsL에게 감사드립니다.
- WinUI 3 데스크톱: 탭 순환이 작동하지 않음 
- GridView의 끌어서 놓기가 WinUI Xaml Islands를 사용하는 데스크톱 애플리케이션에서 실패함
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3871)를 제출해 주신 @smk2007에게 감사드립니다.
- 접근성 문제: WinUI 3 데스크톱에서 PageUp/PageDown 키로 스크롤할 수 없음
- WebView2의 뷰포트 크기가 잘못됨
- MenuFlyout을 연 후 클릭 시 WebView2 충돌 발생
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3729)를 제출해 주신 @sudongg에게 감사드립니다.
- WinUI 3 데스크톱: DropDownButton 또는 SplitButton의 플라이아웃을 가져오려고 하면 앱 충돌 발생
- WebView2: 마우스 오른쪽 단추를 두 번 클릭하면 충돌 발생
- ToggleSplitButton을 클릭하면 애플리케이션이 충돌
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3641)를 제출해 주신 @lhak에게 감사드립니다.
- WinUI 3 데스크톱: 작업 표시줄에 빈 DesktopWindowXamlSource 창 표시
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3698)를 제출해 주신 @bridgesquared에게 감사드립니다.
- WinUI 3 데스크톱: DataGrid가 표시되지 않음
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2703)를 제출해 주신 @eleanorleffler에게 감사드립니다.
- WinUI 3 데스크톱: 파일을 그리드에 놓을 수 없음 
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2715)를 제출해 주신 @eleanorleffler에게 감사드립니다.
- WinUI 3 데스크톱: WinUI 3 Preview 2에서 ItemsRepeater 충돌 발생 
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3007)를 제출해 주신 @hshristov에게 감사드립니다.
- 바인딩을 업데이트할 때 AccessViolationException이 throw됨
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3680)를 제출해 주신 @WamWooWam에게 감사드립니다.
- WinUI 3 데스크톱: scroll NavigationView에서 앱 충돌 발생
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3598)를 제출해 주신 @Berkunath에게 감사드립니다.
- ItemsSource 컬렉션에서 항목을 동적으로 추가하거나 제거하는 동안 ItemsControl이 업데이트되지 않습니다. 
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3517)를 제출해 주신 @VigneshRameshh에게 감사드립니다.
- C++ 적합성 모드를 사용하는 경우 App.xaml.g.h에서 오류 C2760 컴파일 
  - [GitHub에서 이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/3716)를 제출해 주신 @boostafazoo에게 감사드립니다.


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>이전의 WinUI 3 Preview에 도입된 새로운 특징 및 기능

WinUI 3 Preview 1-3에서 도입되었고 WinUI 3 Preview 4에서도 계속 지원되는 특징과 기능은 다음과 같습니다.

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

WinUI 3 및 WinUI 로드맵의 이점에 대한 자세한 내용은 GitHub의 [Windows UI 라이브러리 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)을 참조하세요.

### <a name="provide-feedback-and-suggestions"></a>피드백 및 제안 제공

[WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)에 대한 피드백을 환영합니다.

### <a name="whats-coming-next"></a>다음 단계

특정 기능이 WinUI 3에 도입되는 시점을 확인하려면 자세한 [기능 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)을 살펴보세요. 

## <a name="limitations-and-known-issues"></a>제한 사항 및 알려진 문제

Preview 4 릴리스는 미리 보기일 뿐입니다. 특히 데스크톱 앱과 관련된 시나리오는 새로운 시나리오입니다. 버그, 제한 사항 및 기타 문제가 발생할 수도 있습니다.

WinUI 3 Preview 4에서 알려진 문제 중 일부는 다음 항목과 같습니다. 아래에 나열되지 않은 문제가 발견되면 [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)를 통해 기존 문제에 참여하거나 새 문제를 제출하여 알려주세요.

### <a name="platform-and-os-support"></a>플랫폼 및 OS 지원

WinUI 3 Preview 4는 Windows 10 2018년 4월 업데이트(버전 1803 - 빌드 17134) 이상을 실행하는 PC와 호환됩니다.

### <a name="developer-tools"></a>개발자 도구

- C# 및 C++/WinRT 앱만 지원됩니다.
- 데스크톱 앱은 .NET 5 및 C# 9를 지원하며 MSIX 앱에 패키지되어야 합니다.
- UWP 앱은 .NET 네이티브 및 C# 7.3을 지원합니다.
- 개발자 도구와 Intellisense가 Visual Studio 버전 16.8에서 제대로 작동하지 않을 수 있습니다.
- XAML 디자이너 지원 기능이 없음
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
- CoreWindow, ApplicationView, CoreApplicationView, CoreDispatcher 및 해당 종속성은 데스크톱 앱에서 지원되지 않습니다(아래 참조).

#### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>데스크톱 앱의 CoreWindow, ApplicationView, CoreApplicationView 및 CoreDispatcher

New in Preview4, [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow), [ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView), [CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
[CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) 및 해당 종속성은 데스크톱 앱에서 사용할 수 없습니다.

예를 들어 [Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 속성은 항상 null이지만, Window.DispatcherQueue 속성을 대안으로 사용할 수 있습니다.

이러한 API는 UWP 앱에서만 작동합니다.
과거 미리 보기에서는 데스크톱 앱에서도 부분적으로 작동했지만 Preview4에서는 완전히 사용하지 않도록 설정되었습니다.
이러한 API는 스레드당 창이 하나만 있고 WinUI3의 기능 중 하나가 여러 개를 사용할 수 있는 UWP 사례를 위해 설계되었습니다.

이러한 API의 존재 여부에 내부적으로 의존하는 API가 있으므로 결과적으로 데스크톱 앱에서 지원되지 않습니다. 이러한 API에는 일반적으로 정적 `GetForCurrentView` 메서드가 있습니다. 예를 들어 [UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)가 있습니다.


### <a name="known-issues"></a>알려진 문제

- Alt+F4는 데스크톱 앱 창을 닫지 않습니다.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)의 변경으로 인해, 다음 WinRT API가 더 이상 **데스크톱** 앱에서 예상대로 작동하지 않을 수 있습니다.
  - [`ApplicationView`](/uwp/api/windows.ui.viewmanagement.applicationview) 및 관련된 모든 API가 더 이상 작동하지 않습니다.
  - [`CoreApplicationView`](/uwp/api/windows.applicationmodel.core.coreapplicationview) 및 관련된 모든 API가 더 이상 작동하지 않습니다.
  - 모든 `GetForCurrentView` API가 지원되지 않을 수 있습니다(예: [`CoreInputView.GetForCurrentView`](/uwp/api/Windows.UI.ViewManagement.Core.CoreInputView.GetForCurrentView)).
  - [`CoreWindow.GetForCurrentThread`](/uwp/api/Windows.UI.Core.CoreWindow.GetForCurrentThread)가 이제 null을 반환합니다.

  WinUI 3 데스크톱 앱에서 WinRT API를 사용하는 방법에 대한 자세한 내용은 [데스크톱 앱에서 사용할 수 있는 Windows 런타임 API](../../desktop/modernize/desktop-to-uwp-supported-api.md
)를 참조하세요.

- [UISettings.ColorValuesChanged 이벤트](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) 및 [AccessibilitySettings.HighContrastChanged 이벤트](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged)는 더 이상 데스크톱 앱에서 지원되지 않습니다. Windows 테마의 변경 사항을 감지하는 데 사용하는 경우 문제가 발생할 수 있습니다. 

- 이 릴리스에는 몇 가지 실험적 API가 포함되어 있습니다. 팀에서 이를 철저히 테스트하지 않아 알 수 없는 문제가 있을 수 있습니다. 문제가 발생하면 리포지토리에서 [버그를 신고](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=)하세요. 

- 이전에는 CompositionCapabilities 인스턴스를 가져오려면 [CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)를 호출했습니다. 그러나 이 호출에서 반환된 기능은 보기에 종속되지 *않았습니다*. 이 문제를 해결하고 반영하기 위해 이번 릴리스에서 GetForCurrentView() static을 삭제했으므로 이제 [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) 객체를 직접 만들 수 있습니다.

- C#UWP 앱의 경우:

  WinUI 3 프레임워크는 C++(C++/WinRT 사용) 또는 C#에서 사용할 수 있는 WinRT 구성 요소 세트입니다. C#을 사용하는 경우 앱 모델에 따라 두 가지 버전의 .NET이 있습니다. 즉, UWP 앱에서 WinUI 3을 사용하는 경우 .NET 네이티브를 사용하고, 데스크톱 앱에서 사용하는 경우 .NET 5(및 C#/WinRT)를 사용합니다.

  UWP에서 C#을 WinUI 3 앱에 사용하는 경우 WinUI 3 데스크톱 앱 또는 C# WinUI 2 앱의 C#과 비교하면 API 네임스페이스에 대한 몇 가지 차이점이 있습니다. 즉, 일부 형식이 `System` 네임스페이스가 아니라 `Microsoft` 네임스페이스에 있습니다. 예를 들어 `INotifyPropertyChanged` 인터페이스는 `System.ComponentModel` 네임스페이스에 있지 않고 `Microsoft.UI.Xaml.Data` 네임스페이스에 있습니다. 

  적용 대상:
    - `INotifyPropertyChanged` (및 관련 유형)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 네임스페이스 버전은 여전히 있지만 WinUI 3에서 사용할 수 없습니다. 즉, `ObservableCollection`이 WinUI 3 C# UWP 앱에서 있는 그대로 작동하지 않습니다. 해결 방법은 [XAML Controls Gallery 샘플](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)의 [CollectionsInterop 샘플](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)을 참조하세요.

## <a name="xaml-controls-gallery-winui-3-preview-4-branch"></a>XAML Controls Gallery(WinUI 3 Preview 4 분기)

모든 WinUI 3 Preview 4 컨트롤 및 기능이 포함된 앱 샘플은 [XAML Controls Gallery의 WinUI 3 Preview 4 분기](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)를 참조하세요.

![WinUI 3 Preview 4 XAML Controls Gallery 앱](images/WinUI3XamlControlsGallery.PNG)<br/>
*WinUI 3 Preview 4 XAML Controls Gallery 앱의 예*

샘플을 다운로드하려면 다음 명령을 사용하여 **winui3preview** 분기를 복제합니다.

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

복제 후, 로컬 Git 환경에서 **winui3preview** 분기로 전환해야 합니다. 

```
git checkout winui3preview
```