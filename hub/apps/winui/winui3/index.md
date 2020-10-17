---
title: WinUI 3 Preview 2(2020년 7월)
description: WinUI 3 Preview 2 릴리스에 대한 개요입니다.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: ec4a112eceef7053244d676b6070784174291ed1
ms.sourcegitcommit: 8b01b9ab7293dad1259da32d1459fdd454796e12
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92020163"
---
# <a name="windows-ui-library-3-preview-2-july-2020"></a>Windows UI 라이브러리 3 Preview 2(2020년 7월)

WinUI(Windows UI) 라이브러리 3은 Windows 데스크톱 및 UWP 앱 모두에 대한 네이티브 UX(사용자 환경) 프레임워크입니다.

**WinUI 3 Preview 2**는 Preview 1 릴리스의 버그 및 알려진 문제를 해결하는 데 초점을 맞춘 품질 및 안정성 기반 릴리스입니다.

**[Preview 2 제한 사항 및 알려진 문제](#preview-2-limitations-and-known-issues)를 참조하세요**.

> [!Important]
> 이 WinUI 3 Preview 릴리스는 개발자 커뮤니티에서 초기에 평가하고 피드백을 수집하기 위한 것입니다. 프로덕션 앱에는 사용하면 **안 됩니다**.
>
> WinUI 3 Preview 릴리스는 2020년 동안 계속 제공되며, 첫 번째 공식 릴리스 이후 2021년 초까지 사용할 수 있습니다.
>
> [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml)를 사용하여 피드백을 제공하고 제안 사항과 문제를 기록해 주세요.

## <a name="install-winui-3-preview-2"></a>WinUI 3 Preview 2 설치

WinUI 3 Preview 2에는 WinUI 기반 사용자 인터페이스와 WinUI 라이브러리가 포함된 NuGet 패키지를 사용하여 앱 빌드를 시작하는 데 도움이 되는 Visual Studio 프로젝트 템플릿이 포함되어 있습니다. WinUI 3 Preview 2를 설치하려면 다음 단계를 수행합니다.

> [!NOTE]
> [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-2-branch)의 WinUI 3 Preview 2 버전을 복제하고 빌드할 수도 있습니다.

1. Windows 10, 버전 1803(빌드 17134) 이상이 개발 컴퓨터에 설치되어 있는지 확인합니다.

2. [Visual Studio 2019, 버전 16.7.2](https://visualstudio.microsoft.com/vs/) 설치

    Visual Studio를 설치할 때 다음 워크로드를 포함해야 합니다.
    - .NET 데스크톱 개발
    - 유니버설 Windows 플랫폼 개발

    C++ 앱을 빌드하려면 다음 워크로드도 포함해야 합니다.
    - C++를 사용한 데스크톱 개발
    - 유니버설 Windows 플랫폼 워크로드용 선택적 *C++(v142) 유니버설 Windows 플랫폼 도구* 구성 요소(오른쪽 창에 있는 "유니버설 Windows 플랫폼 개발" 섹션의 "설치 세부 정보" 참조)

    Visual Studio를 다운로드한 후에는 프로그램 내에서 .NET 미리 보기를 사용해야 합니다. 
    - 도구 > 옵션 > 미리 보기 기능으로 이동하여 ".NET Core SDK의 미리 보기 사용(다시 시작해야 함)"을 선택합니다. 

3. C#/.NET 5 및 C++/Win32 앱용 데스크톱 WinUI 프로젝트를 만들려는 경우 .NET 5 Preview 5의 x64 및 x86 버전도 모두 설치해야 합니다. **.NET 5 Preview 5는 현재 WinUI 3에 대해 유일하게 지원되는 .NET 5 미리 보기입니다**.

    - [.NET 5 Preview 5용 x64 설치 관리자](https://dotnet.microsoft.com/download/dotnet/thank-you/sdk-5.0.100-preview.5-windows-x64-installer)
    - [.NET 5 Preview 5용 x86 설치 관리자](https://dotnet.microsoft.com/download/dotnet/thank-you/sdk-5.0.100-preview.5-windows-x86-installer)

4. [WinUI 3 Preview 2 VSIX 패키지](https://aka.ms/winui3/previewdownload)를 다운로드하여 설치합니다. 이 VSIX 패키지는 WinUI 3 프로젝트 템플릿 및 WinUI 3 라이브러리가 포함된 NuGet 패키지를 Visual Studio 2019에 추가합니다.

    VSIX 패키지를 Visual Studio에 추가하는 방법에 대한 지침은 [Visual Studio 확장 찾기 및 사용](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)을 참조하세요.


## <a name="create-winui-projects"></a>WinUI 프로젝트 만들기

WinUI 3 Preview 2 VSIX 패키지가 설치되면 Visual Studio에서 WinUI 프로젝트 템플릿 중 하나를 사용하여 새 프로젝트를 만들 수 있습니다. **새 프로젝트 만들기** 대화 상자에서 WinUI 프로젝트 템플릿에 액세스하려면 언어를 **C++** 또는 **C#** 으로, 플랫폼을 **Windows**로, 프로젝트 형식을 **WinUI**로 필터링합니다. 또는 *WinUI*를 검색하여 사용 가능한 C# 또는 C++ 템플릿 중 하나를 선택할 수 있습니다.

![WinUI 프로젝트 템플릿](images/winui-projects-csharp.png)

WinUI 프로젝트 템플릿을 시작하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

- [데스크톱 앱용 WinUI 3 시작](get-started-winui3-for-desktop.md)
- [UWP 앱용 WinUI 3 시작](get-started-winui3-for-uwp.md)

[제한 사항 및 알려진 문제](#preview-2-limitations-and-known-issues) 외에도 WinUI 프로젝트를 사용하여 앱을 빌드하는 것은 XAML 및 WinUI 2.x를 사용하여 UWP 앱을 빌드하는 것과 비슷합니다. 따라서 UWP 앱 및 Windows SDK의 **Windows.UI** WinRT 네임스페이스에 대한 지침 및 설명서 대부분을 적용할 수 있습니다.

WinUI 3 Preview 1을 사용하여 프로젝트를 만든 경우 Preview 2를 사용하도록 프로젝트를 업그레이드할 수 있습니다. [GitHub 리포지토리](https://aka.ms/winui3/upgrade-instructions)에 대한 자세한 지침을 참조하세요.

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
| Windows 런타임 구성 요소(UWP의 WinUI) | C# 및 C++ | 앱이 작성된 프로그래밍 언어에 관계없이 WinUI 기반 사용자 인터페이스를 사용하여 모든 UWP 앱에서 사용할 수 있는 C# 또는 C++/WinRT로 작성된 [Windows 런타임 구성 요소](/windows/uwp/winrt-components/)를 만듭니다. |

### <a name="item-templates-for-winui-3"></a>WinUI 3용 항목 템플릿

WinUI 프로젝트에서 사용할 수 있는 항목 템플릿은 다음과 같습니다. 이러한 WinUI 프로젝트 템플릿에 액세스하려면 **솔루션 탐색기**에서 마우스 오른쪽 단추로 프로젝트 노드를 클릭하고, **추가** -> **새 항목**을 차례로 선택하고, **새 항목 추가** 대화 상자에서 **WinUI**를 클릭합니다.

![WinUI 항목 템플릿](images/winui-items-csharp.png)

| 템플릿 | Language | 설명 |
|----------|----------|-------------|
| 빈 페이지(WinUI) | C# 및 C++ | WinUI 라이브러리의 **Microsoft.UI.Xaml.Controls.Page** 클래스에서 파생되는 새 페이지를 정의하는 XAML 파일 및 코드 파일을 추가합니다. |
| 빈 창(데스크톱의 WinUI) | C# 및 C++ | WinUI 라이브러리의 **Microsoft.UI.Xaml.Window** 클래스에서 파생되는 새 창을 정의하는 XAML 파일 및 코드 파일을 추가합니다. |
| 사용자 지정 컨트롤(WinUI) | C# 및 C++ | 기본 스타일을 사용하여 템플릿 기반 컨트롤을 만드는 코드 파일을 추가합니다. 템플릿 기반 컨트롤은 WinUI 라이브러리의 **Microsoft.UI.Xaml.Controls.Control** 클래스에서 파생됩니다.<p></p>이 항목 템플릿을 사용하는 방법을 보여주는 연습은 [C++/WinRT를 사용한 UWP 및 WinUI 3 앱용 템플릿 XAML 컨트롤](xaml-templated-controls-cppwinrt-winui-3.md) 및 [C#을 사용한 UWP 및 WinUI 3 앱용 템플릿 XAML 컨트롤](xaml-templated-controls-csharp-winui-3.md)을 참조하세요. 템플릿 기반 컨트롤에 대한 자세한 내용은 [사용자 지정 XAML 컨트롤](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)을 참조하세요. |
| 리소스 사전(WinUI) | C# 및 C++ | XAML 리소스의 빈 키 컬렉션을 추가합니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)를 확인하세요. |
| 리소스 파일(WinUI) | C# 및 C++ | 앱에 대한 문자열 및 조건부 리소스를 저장하는 파일을 추가합니다. 이 항목을 사용하여 앱을 지역화할 수 있습니다. 자세한 내용은 [UI 및 앱 패키지 매니페스트의 문자열 지역화](/windows/uwp/app-resources/localize-strings-ui-manifest)를 참조하세요. |
| 사용자 지정 컨트롤(WinUI) | C# 및 C++ | WinUI 라이브러리의 **Microsoft.UI.Xaml.Controls.UserControl** 클래스에서 파생되는 사용자 지정 컨트롤을 만드는 XAML 파일 및 코드 파일을 추가합니다. 일반적으로 사용자 지정 컨트롤은 관련된 기존 컨트롤을 캡슐화하고 자체 논리를 제공합니다.<p></p>사용자 지정 컨트롤에 대한 자세한 내용은 [사용자 지정 XAML 컨트롤](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)을 참조하세요. |

## <a name="bug-fixes-and-other-improvements-in-winui-3-preview-2"></a>WinUI 3 Preview 2의 버그 수정 및 기타 개선 사항

Preview 2의 버그 수정 및 기타 업데이트에 대한 포괄적인 목록입니다. 이 릴리스에서 해결된 가장 중요한 버그 수정의 목록은 [릴리스 알림](https://aka.ms/winui3/preview2-announcement)을 참조하세요.

> [!NOTE]
> WinUI 3 Preview 2는 WinUI 2 라이브러리 버전 2.4.2를 사용합니다. 

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) 및 [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged)는 이제 C# 데스크톱 앱에서 예상대로 작동합니다.
  - 이를 통해 백 엔드에서 업데이트되는 동안 UI에서 업데이트되지 않는 컬렉션 컨트롤과 관련된 몇 가지 다른 문제가 해결되었습니다.
  - *GitHub에서 [비슷한 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2490)를 제출해 주신 @hshristov에게 감사드립니다.*
- Preview 2는 이제 데스크톱 앱용 [.NET 5 Preview 5](/dotnet/api/?view=net-5.0&preserve-view=true)와 호환됩니다.
- WinUI 3에는 이제 [WinUI 2.4](../winui2/release-notes/winui-2.4.md)와 관련된 패리티가 있습니다. 여기에는 [계층적 NavigationView](../winui2/release-notes/winui-2.4.md#hierarchical-navigation) 및 [ProgressRing](../winui2/release-notes/winui-2.4.md#progressring)과 같은 새로운 컨트롤과 기능이 포함되어 있습니다.
- 작동 중단 수정됨: 터치를 통해 [TabView](/windows/uwp/design/controls-and-patterns/tab-view)를 사용합니다.
- [XAML Controls Gallery 샘플](#xaml-controls-gallery-winui-3-preview-2-branch)의 [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview)에서 이제 Left-compact 모드 대신 Left 모드를 사용합니다.
- 작동 중단 수정됨: 입력 유효성 검사 및 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box)에서 너무 빠르게 입력합니다.
  - *GitHub에서 [이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2563)를 제출해 주신 @paulovilla에게 감사드립니다.*
- 작동 중단 수정됨: [TextBox](/windows/uwp/design/controls-and-patterns/text-box) 메뉴가 작동하는 동안 XAML UI와 상호 작용합니다.
- 여러 페이지로 이동하면 [XAML Controls Gallery 샘플](#xaml-controls-gallery-winui-3-preview-2-branch) 제목 텍스트가 더 이상 스크램블되지 않습니다.
- [WebView2](/microsoft-edge/webview2/)에서 터치를 사용하더라도 더 이상 약간의 오프셋이 위치에 제공되지 않습니다.
- WinUIEdit.dll의 클래스가 Windows.UI.Text 네임스페이스에서 Microsoft.UI.Text 네임스페이스로 이동되었습니다.
- 작동 중단 수정됨: 다중 선택 모드(Windows 10 버전 1803)의 [TreeView](/windows/uwp/design/controls-and-patterns/tree-view)에서 항목을 선택합니다.
- Point, Rect 및 Size 멤버는 이제 데스크톱 앱용 API의 C# 프로젝션에서 Double 형식으로 지정됩니다.
  - *GitHub에서 [이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2474)를 제출해 주신 @dotMorten에게 감사드립니다.*
- 작동 중단 수정됨: .rtf 파일에서 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box)를 사용합니다.
- [TabView](/windows/uwp/design/controls-and-patterns/tab-view) 닫기 단추에는 더 이상 빈 도구 설명이 없습니다.
- [Image](/windows/uwp/design/controls-and-patterns/images-imagebrushes) 컨트롤에서 이제 SVG 파일을 올바르게 렌더링합니다.
  - *GitHub에서 [이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2565)를 제출해 주신 @mqudsi에게 감사드립니다.*
- 작동 중단 수정됨: Page 요소를 사용하거나 이 요소로 이동합니다.
- [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview)에서 터치를 사용하여 항목을 선택하면 이제 다른 모든 항목(단일 선택 모드)의 선택이 취소됩니다.
- 작동 중단 수정됨: 특정 크기의 [Slider](/windows/uwp/design/controls-and-patterns/slider) 컨트롤에 설정된 값으로 인해 LayoutSliderException이 더 이상 발생하지 않습니다. 
  - *GitHub에서 [이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/477)를 제출해 주신 @hig-dev에게 감사드립니다.*
- 작동 중단 수정됨: [ColorPicker](/windows/uwp/design/controls-and-patterns/color-picker)를 사용하면 종료 시 작동 중단이 발생했습니다.
- 작동 중단 수정됨: [Pivot](/windows/uwp/design/controls-and-patterns/pivot)을 사용하면 종료 시 작동 중단이 발생했습니다.
- 작동 중단 수정됨: Windows 10 버전 1803에서 리소스가 누락되어 [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) 작동 중단이 발생했습니다.
- 작동 중단 수정됨: [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) 사용자 지정 편집기에서 포커스를 맞춥니다. 
- 작동 중단 수정됨: [SemanticZoom](/windows/uwp/design/controls-and-patterns/semantic-zoom) 
- 바인딩은 이제 Mode=OneWay가 암시적인 태그에서 예상대로 작동합니다.
  - *GitHub에서 [이 문제](https://github.com/microsoft/microsoft-ui-xaml/issues/2630)를 제출해 주신 @tomasfabian에게 감사드립니다.*
- 수정된 애니메이션: [XAML Controls Gallery 샘플](#xaml-controls-gallery-winui-3-preview-2-branch)의 새로운 기능

## <a name="new-features-and-capabilities-introduced-in-winui-3-preview-1"></a>WinUI 3 Preview 1에 도입된 새로운 특징 및 기능

WinUI 3 Preview 1에서 도입되었고 WinUI 3 Preview 2에서 계속 지원되는 특징과 기능은 다음과 같습니다.

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

## <a name="preview-2-limitations-and-known-issues"></a>Preview 2 제한 사항 및 알려진 문제

Preview 2 릴리스는 미리 보기일 뿐입니다. 데스크톱 Win32 앱 관련 시나리오는 특히 새로운 시나리오입니다. 버그, 제한 사항 및 기타 문제가 발생할 수도 있습니다.

WinUI 3 Preview 2에서 알려진 문제 중 일부는 다음과 같습니다. 아래에 없는 문제가 발견되면 [WinUI GitHub 리포지토리](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)에서 기존 문제에 덧붙이거나 새 문제를 제출하여 알려주세요.

### <a name="platform-and-os-support"></a>플랫폼 및 OS 지원

WinUI 3 Preview 2는 Windows 10 2018년 4월 업데이트(버전 1803 - 빌드 17134) 이상을 실행하는 PC와 호환됩니다.

### <a name="developer-tools"></a>개발자 도구

- 현재 C# 및 C++/WinRT 앱만 지원됩니다.
- 데스크톱 앱은 .NET 5 및 C# 8을 지원하며 패키지되어야 합니다.
- UWP 앱은 .NET 네이티브 및 C# 7.3을 지원합니다.
- Intellisense가 불완전합니다.
- 비주얼 디자이너 없음
- 핫 다시 로드 없음
- 라이브 시각적 트리 없음
- VS Code를 사용한 개발은 아직 지원되지 않습니다.
- 새 C++/CX 앱은 지원되지 않지만, 기존 앱은 계속 작동합니다(가능한 한 빨리 C++/WinRT로 이동).
- WinUI 3 콘텐츠는 프로세스당 하나의 창 또는 앱당 하나의 ApplicationView에만 있을 수 있습니다.
- 패키지되지 않은 데스크톱 배포는 지원되지 않습니다.
- ARM64 지원 안 함
- UWP 앱의 C# 사용자 지정 컨트롤: `Themes/Generic.xaml`은 자동으로 생성되지 않습니다. 이 문제는 클래스에서 Themes 폴더를 수동으로 만들고 그 안에 `Generic.xaml`이라는 XAML 파일을 배치하여 해결할 수 있습니다.
- WinUI 사용자 지정 컨트롤이 프로젝트에 추가되면 파일에 "CustomControl.h" 헤더가 없을 수 있습니다. 이 문제는 해당 헤더를 `pch.h` 파일에 수동으로 추가하여 해결할 수 있습니다.
- DataGrid, 다른 Windows 커뮤니티 도구 키트 컨트롤 및 타사 라이브러리 컨트롤이 추가되면 빌드가 실패할 수 있습니다. 이 문제를 해결하려면 이 병합된 사전을 `App.xaml` 파일에 추가합니다.
  ```xaml
  <ResourceDictionary Source="ms-appx:///<library_name>/Themes/Generic.xaml"/>
  ```

### <a name="missing-platform-features"></a>플랫폼 기능 누락

- Xbox 지원
- HoloLens 지원
- 창 팝업
- 수동 입력 지원
- 배경 아크릴
- MediaElement 및 MediaPlayerElement
- RenderTargetBitmap
- MapControl
- SwapChainPanel은 투명성을 지원하지 않습니다.
- 글로벌 표시는 솔리드 브러시인 폴백 동작을 사용합니다.
- XAML Islands는 이 릴리스에서 지원되지 않습니다.
- 타사 에코시스템 라이브러리는 완전하게 작동하지 않습니다.
- IME가 작동하지 않습니다.

### <a name="known-issues"></a>알려진 문제


- C# UWP 앱:

  WinUI 3 프레임워크는 WinRT 구성 요소 세트이며 WinRT에는 .NET에 있는 것과 유사한 유형 및 개체가 있지만 본질적으로 호환되지는 않습니다.  C#/WinRT 프로젝션은 .NET 5에서 .NET과 WinRT 간의 상호 운용을 처리하므로 현재 .NET 5 앱에서 .NET 인터페이스를 자유롭게 사용할 수 있습니다. 
  
  그러나 C#/WinRT는 .NET 네이티브 앱에서 interop를 처리할 수 없으므로 WinUI 3 API는 UWP 앱에서 직접 프로젝션됩니다. 따라서 동일한 .NET 인터페이스를 더 이상 사용할 수 없습니다. **UWP 앱이 더 이상 .NET 네이티브를 사용하지 않는 경우 이 제한은 더 이상 존재하지 않습니다**.

  예를 들어 `INotifyPropertyChanged` API는 데스크톱 앱의 WinUI3에 대한 `System.ComponentModel` 네임스페이스에서 프로젝션되지만 UWP 앱(및 모든 C++ 앱)의 WinUI3에 대한 `Microsoft.UI.Xaml.Data` 네임스페이스에 나타납니다. 
  
  이 문제는
    - `INotifyPropertyChanged` (및 관련 유형)
    - `INotifyCollectionChanged`
    - `ICommand`

> [!Note] 
> `INotifyPropertyChanged` 및 `INotifyCollectionChanged`가 예상대로 작동하지 않으면 `ObservableCollection<T>` 클래스도 영향을 받습니다. `ObservableCollection<T>`의 고유한 버전을 구현하는 예제는 [이 샘플](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)을 참조하세요. 


## <a name="xaml-controls-gallery-winui-3-preview-2-branch"></a>XAML Controls Gallery(WinUI 3 Preview 2 분기)

모든 WinUI 3 Preview 2 컨트롤 및 기능이 포함된 앱 샘플은 [XAML Controls Gallery의 WinUI 3 Preview 2 분기](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)를 참조하세요.

![WinUI 3 Preview 2 XAML Controls Gallery 앱](images/WinUI3XamlControlsGalleryP2.png)<br/>
*WinUI 3 Preview 2 XAML Controls Gallery 앱의 예*

샘플을 다운로드하려면 다음 명령을 사용하여 **winui3preview** 분기를 복제합니다.

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

복제 후, 로컬 Git 환경에서 **winui3preview** 분기로 전환해야 합니다.

```
git checkout winui3preview
```
