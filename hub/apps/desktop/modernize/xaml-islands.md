---
description: 이 가이드에서는 WPF 및 Windows Forms 응용 프로그램에서 직접 Fluent 기반 UWP UI를 만들 수 있습니다.
title: 데스크톱 앱에서 UWP 컨트롤
ms.date: 04/16/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fd4f92e543b224e617e1990e5761d534c87ce6fb
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984933"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>데스크톱 앱 (XAML 제도)에서 호스트 UWP XAML 컨트롤

Windows 10 버전 1903부터 UWP 컨트롤 이라는 기능을 사용 하 여 비 UWP 데스크톱 응용 프로그램에서 호스트할 수 있습니다 *XAML 제도*합니다. 이 기능을 사용 하면 모양, 느낌 및 UWP 컨트롤을 통해 이용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 기존 데스크톱 응용 프로그램의 기능을 향상 시킬 수 있습니다. 즉, 같은 UWP 기능을 사용할 수 [Windows 잉크](/windows/uwp/design/input/pen-and-stylus-interactions) 지 원하는 컨트롤 및 합니다 [Fluent Design System](/windows/uwp/design/fluent-design-system/index) 에 기존 wpf에서 Windows Forms, 및 C++ Win32 응용 프로그램입니다.

Windows Forms, wpf에서 XAML 제도 사용 하는 여러 방법을 제공 하 고 C++ 기술 이나 사용할 프레임 워크에 따라 Win32 응용 프로그램입니다.

> [!NOTE]
> XAML 아일랜드에 대 한 의견이 있는 경우에서 새 문제를 작성 합니다 [Microsoft.Toolkit.Win32 리포지토리](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) 두고 의견 있습니다. 개인적으로 피드백을 제출 하려는 경우에 보낼 수 있습니다를 XamlIslandsFeedback@microsoft.com입니다. Insights 및 시나리오에는 우리에 게 매우 중요 합니다.

## <a name="how-do-xaml-islands-work"></a>XAML 제도 어떻게 작동 하나요?

Windows 10, 버전 1903, 시작에서는 두 가지 방법으로 Windows Forms, wpf에서 XAML 제도 사용 하 고 C++ Win32 응용 프로그램:

* 여러 Windows 런타임 클래스 및 응용 프로그램에서 파생 되는 모든 UWP 컨트롤을 호스트 하는 데 사용할 수 있는 COM 인터페이스를 제공 하는 Windows SDK [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)합니다. 이러한 클래스 및 인터페이스 라고 통칭를 *호스팅 API는 UWP XAML*, 하며 연결 된 창 핸들 (HWND)을 응용 프로그램에서 UI 요소에서 UWP 컨트롤을 호스트 하 수 있도록 합니다. 이 API에 대 한 자세한 내용은 참조 하세요. [API를 호스트 하는 XAML을 사용 하 여](using-the-xaml-hosting-api.md)입니다.

* 합니다 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) 또한 WPF 및 Windows Forms에 대 한 추가 XAML 섬 컨트롤을 제공 합니다. 이러한 컨트롤 호스팅 API에 내부적으로 UWP XAML을 사용 하 고 그렇지 않은 경우 호스팅 API를 직접 키보드 탐색 및 레이아웃 변경 내용을 포함 하 여 UWP XAML을 사용 하는 경우를 직접 처리 해야 하는 동작을 모두를 구현 합니다. WPF 및 Windows Forms 응용 프로그램에 대 한 좋습니다 이러한 컨트롤을 사용 하 여 UWP XAML 대신 다양 API를 사용 하 여 구현 세부 정보를 추상화 하기 때문에 API를 직접 호스팅. Windows 10 버전 1903 기준으로 이러한 컨트롤은 [개발자 미리 보기로 제공](#feature-roadmap)합니다.

> [!NOTE]
> C++Win32 데스크톱 응용 프로그램 호스트 UWP 컨트롤에 호스팅 API는 UWP XAML을 사용 해야 합니다. Windows 커뮤니티 도구 키트의 XAML 섬 컨트롤을 사용할 수 없는 C++ Win32 데스크톱 응용 프로그램입니다.

WPF 및 Windows Forms 응용 프로그램에 대 한 Windows 커뮤니티 도구 키트에서 제공 하는 XAML 섬 컨트롤의 두 가지가: *래핑된 컨트롤* 하 고 *컨트롤을 호스팅할*합니다.

### <a name="wrapped-controls"></a>래핑된 컨트롤

WPF 및 Windows Forms 응용 프로그램에서 래핑된 UWP 컨트롤의 선택 영역을 사용할 수는 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)합니다. 라고 *래핑된 컨트롤* 인터페이스와 특정 UWP 컨트롤의 기능 때문에 해당 합니다. WPF 또는 Windows Forms 프로젝트의 디자인 화면으로 직접 이러한 컨트롤을 추가 하 고 디자이너에서 다른 WPF 또는 Windows Forms 컨트롤과 마찬가지로 사용할 수 있습니다.

XAML 제도 구현 하기 위한 다음 래핑된 UWP 컨트롤은 WPF 및 Windows Forms 응용 프로그램에서 현재 사용할 수 있습니다.

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 Windows 잉크 기반 사용자 상호 작용에 대 한 노출 및 관련 도구 모음을 제공 합니다. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 버전 1903 | 스트림 하 고 Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 비디오와 같은 미디어 콘텐츠를 렌더링 하는 뷰를 포함 합니다. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 바로 가기 또는 사실적인 지도 표시할 수 있습니다. |

XAML 아일랜드에 대 한 래핑된 컨트롤 외에도 Windows 커뮤니티 도구 키트 웹 콘텐츠를 호스트 하기 위한 다음과 같은 컨트롤을 제공 합니다.

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [웹 보기](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, 버전 1803 | 웹 콘텐츠를 표시 하는 Microsoft Edge 렌더링 엔진을 사용 합니다. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 버전을 제공 **WebView** 자세한 OS 버전과 호환 되는 합니다. 이 컨트롤은 Windows 10 버전 1803 이상에서 웹 콘텐츠를 표시 하는 Microsoft Edge 렌더링 엔진 및 이전 버전의 Windows 10, Windows에서 웹 콘텐츠를 표시 하는 Internet Explorer 렌더링 엔진 8.x, 및 Windows 7입니다. |

### <a name="host-controls"></a>호스트 컨트롤

적용 가능한 래핑된 컨트롤 이외의 시나리오, WPF 및 Windows Forms 응용 프로그램도 사용할 수는 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 에서 제어를 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)합니다. 이 컨트롤에서 파생 되는 모든 UWP 컨트롤을 호스트할 수 있습니다 [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), 사용자 지정 사용자 컨트롤 뿐 아니라 모든 Windows SDK에서 제공 하는 UWP 컨트롤을 포함 합니다. 이 제어는 Windows 10 Insider Preview SDK 빌드 17709 이상 릴리스를 지원합니다.

### <a name="architecture-overview"></a>아키텍처 개요

여기에서 이러한 컨트롤이 아키텍처 측면에서 구성되는 방법을 빠르게 확인할 수 있습니다.

![호스트 컨트롤 아키텍처](images/xaml-islands/host-controls.png)

이 다이어그램 맨 아래에 표시되는 API는 Windows SDK와 함께 제공됩니다. 래핑된 컨트롤 및 호스트 컨트롤 Windows 커뮤니티 도구 키트에서 Nuget 패키지를 통해 사용할 수 있습니다.

## <a name="feature-roadmap"></a>기능 로드맵

Windows 10 버전 1903, 버전부터 래핑된 컨트롤과 Windows 커뮤니티 도구 키트에서 호스트 컨트롤은 아직 개발자 미리 보기 컨트롤의 버전 1.0 릴리스에 제공 될 때까지입니다.

* 버전 1.0의.NET Framework 4.6.2에 대 한 컨트롤 및 나중에 출시 될 예정인 합니다 [도구 키트의 6.0 릴리스에](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)합니다.
* 버전 1.0의.NET Core 3에 대 한 컨트롤 도구 키트의 후속 릴리스에서 계획 되어 있습니다.
* .NET Framework 및.NET Core 3에 대 한 이러한 컨트롤의 버전 1.0 릴리스의 최신 미리 보기를 시도 하려는 경우 참조를 **6.0.0-preview3** NuGet 패키지를 [UWP 커뮤니티 도구 키트](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) 갤러리입니다.

## <a name="requirements"></a>요구 사항

XAML 제도 Windows 10, 버전, 1903 이상 필요합니다. 응용 프로그램에서 XAML 제도 사용 하려면 먼저 프로젝트를 설정 해야 합니다.

### <a name="step-1-modify-your-project-to-use-windows-runtime-apis"></a>1단계: Windows 런타임 Api를 사용 하도록 프로젝트를 수정 합니다.

자세한 내용은 [이 문서에서는](desktop-to-uwp-enhance.md#set-up-your-project)합니다.

### <a name="step-2-enable-xaml-island-support-in-your-project"></a>2단계: 프로젝트에서 XAML 섬 사용 지원

XAML 섬 지원을 사용 하려면 프로젝트에 다음 변경 중 하나를 확인 합니다. 자세한 내용은 참조 하세요. [이 블로그 게시물](https://techcommunity.microsoft.com/t5/Windows-Dev-AppConsult/Using-XAML-Islands-on-Windows-10-19H1-fixing-the-quot/ba-p/376330#M117)합니다.

#### <a name="option-1-package-your-application-in-an-msix-package"></a>옵션 1: MSIX 패키지의 응용 프로그램 패키지  

Windows 10, 버전 1903 SDK (또는 이후 릴리스)를 설치 합니다. 그런 다음 추가 하 여 MSIX 패키지의 응용 프로그램 패키지를 [Windows 응용 프로그램 패키징 프로젝트](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 솔루션 및 WPF 또는 Windows Forms 프로젝트에 대 한 참조를 추가 합니다.

#### <a name="option-2-set-the-maxversiontested-value-in-your-assembly-manifest"></a>옵션 2: 어셈블리 매니페스트에 maxVersionTested 값을 설정 합니다.

없는 하려는 경우 MSIX 패키지의 응용 프로그램 패키지를 추가할 수 있습니다는 [side-by-side-어셈블리 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 프로젝트에 추가 합니다 **maxVersionTested** 매니페스트를 지정 하는 값에 응용 프로그램은 Windows 10 버전이 1903 이상 호환 됩니다.

1. 프로젝트에서 매니페스트를 프로젝트에 새 XML 파일을 추가 하 고 이름을 어셈블리 아직 없는 경우 **app.manifest**합니다. WPF 또는 Windows Forms 응용 프로그램을 할당할 수도 있는지 확인 합니다 **매니페스트** 속성을 **. app.manifest** 에 **응용 프로그램** 페이지에 [프로젝트 속성](https://docs.microsoft.com/visualstudio/ide/reference/application-page-project-designer-csharp?view=vs-2019#resources)합니다.
2. 어셈블리 매니페스트에 포함 된 **호환성** 요소와 다음 예제와 같이 자식 요소입니다. 대체는 **Id** 특성을 **maxVersionTested** 대상으로 하는 Windows 10의 버전 번호를 사용 하 여 요소 (Windows 10, 버전 1903 또는 이후 버전 이어야 함). 

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
        <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
            <application>
                <!-- Windows 10 -->
                <maxversiontested Id="10.0.18362.0"/>
                <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
            </application>
        </compatibility>
    </assembly>
    ```

## <a name="additional-resources"></a>추가 자료

자세한 배경 정보 및 XAML 제도 사용 하 여에 대 한 자습서를 보려면 다음 문서 및 리소스를 참조 하세요.

* [XAML 제도 랩](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/tree/microsoftlearn)합니다. 이 포괄적인 랩 UWP 컨트롤 기존 WPF 기간 업무 응용 프로그램에 추가할 Windows 커뮤니티 도구 키트에서 래핑된 컨트롤 및 호스트 컨트롤을 사용 하기 위한 단계별 지침을 제공 합니다. 이 환경에는 [전체 WPF 응용 프로그램에 대 한 코드](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/tree/microsoftlearn/Lab) 뿐만 [상세 지침](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/blob/microsoftlearn/Manual/README.md) 프로세스의 각 단계에 대 한 합니다.
