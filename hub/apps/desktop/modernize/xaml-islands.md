---
description: 이 가이드에서는 WPF 및 Windows Forms 응용 프로그램에서 직접 Fluent 기반 UWP UI를 만들 수 있습니다.
title: 데스크톱 앱의 UWP 컨트롤
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 79dcd6069a5746e04565db660e6f5c03988a94a4
ms.sourcegitcommit: 2062d06567ef087ad73507a03ecc726a7d848361
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303560"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>데스크톱 앱에서 UWP XAML 컨트롤 호스트 (XAML 제도)

Windows 10 버전 1903부터 *XAML 아일랜드*라는 기능을 사용 하 여 uwp 이외의 데스크톱 응용 프로그램에서 uwp 컨트롤을 호스트할 수 있습니다. 이 기능을 사용 하면 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 기존 데스크톱 응용 프로그램의 모양, 느낌 및 기능을 향상 시킬 수 있습니다. 즉, 기존 WPF, Windows Forms 및 C++ Win32 응용 프로그램에서 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/index) 을 지 원하는 [WINDOWS 잉크](/windows/uwp/design/input/pen-and-stylus-interactions) 및 컨트롤과 같은 UWP 기능을 사용할 수 있습니다.

사용 중인 기술 또는 프레임 워크에 따라 WPF, Windows Forms 및 C++ Win32 응용 프로그램에서 XAML 아일랜드를 사용 하는 여러 가지 방법을 제공 합니다. 

> [!NOTE]
> XAML 아일랜드에 대 한 피드백이 있는 경우 [Microsoft Toolkit 리포지토리의](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) 새 문제를 만들고 의견을 남겨 두세요. 사용자 의견을 개인적으로 제출 하는 것을 선호 하는 경우 XamlIslandsFeedback@microsoft.com로 보낼 수 있습니다. 귀하의 통찰력 및 시나리오는 매우 중요 합니다.

## <a name="how-do-xaml-islands-work"></a>XAML 아일랜드는 어떻게 작동 하나요?

Windows 10 버전 1903부터 WPF, Windows Forms 및 C++ Win32 응용 프로그램에서 XAML 아일랜드를 사용 하는 두 가지 방법을 제공 합니다.

* Windows SDK는 응용 프로그램에서 [**Windows .xaml**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생 되는 모든 UWP 컨트롤을 호스트 하는 데 사용할 수 있는 몇 가지 Windows 런타임 클래스 및 COM 인터페이스를 제공 합니다. 이러한 클래스와 인터페이스를 통칭 하 여 *UWP XAML 호스팅 API*라고 하며,이를 통해 연결 된 창 핸들 (HWND)이 있는 응용 프로그램의 모든 UI 요소에서 UWP 컨트롤을 호스트할 수 있습니다. 이 API에 대 한 자세한 내용은 [XAML 호스팅 API 사용](using-the-xaml-hosting-api.md)을 참조 하세요.

* 또한 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) 는 WPF 및 Windows Forms에 대 한 추가 XAML 아일랜드 컨트롤을 제공 합니다. 이러한 컨트롤은 내부적으로 UWP XAML 호스팅 API를 사용 하며, 키보드 탐색 및 레이아웃 변경을 포함 하 여 UWP XAML 호스팅 API를 직접 사용 하는 경우 직접 처리 해야 하는 모든 동작을 구현 합니다. WPF 및 Windows Forms 응용 프로그램의 경우 API 사용에 대 한 많은 구현 세부 정보를 추상화 하므로 UWP XAML 호스팅 API 대신 이러한 컨트롤을 직접 사용 하는 것이 좋습니다. Windows 10 버전 1903부터 이러한 컨트롤은 [개발자 미리 보기로 제공](#feature-roadmap)됩니다.

> [!NOTE]
> C++Win32 데스크톱 응용 프로그램은 uwp XAML 호스팅 API를 사용 하 여 UWP 컨트롤을 호스트 해야 합니다. Windows Community Toolkit의 XAML 아일랜드 컨트롤은 Win32 데스크톱 응용 프로그램에 C++ 사용할 수 없습니다.

WPF 및 Windows Forms 응용 프로그램에 대 한 Windows 커뮤니티 도구 키트에서 제공 하는 두 가지 형식의 XAML 아일랜드 컨트롤은 *래핑된 컨트롤과* *호스트 컨트롤*입니다. 

### <a name="wrapped-controls"></a>래핑된 컨트롤

WPF 및 Windows Forms 응용 프로그램은 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)에서 래핑된 UWP 컨트롤을 선택할 수 있습니다. 이러한 컨트롤은 특정 UWP 컨트롤의 인터페이스 및 기능을 래핑하는 *래핑된 컨트롤* 이라고 합니다. 이러한 컨트롤을 WPF 또는 Windows Forms 프로젝트의 디자인 화면에 직접 추가 하 고 디자이너의 다른 WPF 또는 Windows Forms 컨트롤과 같은 방식으로 사용할 수 있습니다.

XAML 아일랜드를 구현 하기 위한 래핑된 UWP 컨트롤은 현재 WPF에 사용할 수 있으며 ( [Microsoft toolkit. controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 패키지 참조) 응용 프로그램을 Windows Forms 있습니다 ( [Microsoft toolkit. 컨트롤](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) 패키지 참조). ).

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 Windows Ink 기반 사용자 조작을 위한 화면 및 관련 도구 모음을 제공 합니다. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 비디오와 같은 미디어 콘텐츠를 스트리밍하 고 렌더링 하는 뷰를 포함 합니다. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 기호화 된 지도 또는 실사 지도를 표시할 수 있습니다. |

Windows 커뮤니티 도구 키트는 XAML 아일랜드에 대해 래핑된 컨트롤 외에도 WPF에서 웹 콘텐츠를 호스트 하기 위한 다음과 같은 컨트롤을 제공 합니다 ( [Microsoft Windows Forms Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView) . x m l. x m l. [Microsoft. Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView) . n a m.

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [웹 보기](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, 버전 1803 | Microsoft Edge 렌더링 엔진을 사용 하 여 웹 콘텐츠를 표시 합니다. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 더 많은 OS 버전과 호환 되는 **웹 보기** 버전을 제공 합니다. 이 컨트롤은 Microsoft Edge 렌더링 엔진을 사용 하 여 Windows 10 버전 1803 이상에 웹 콘텐츠를 표시 하 고 Internet Explorer 렌더링 엔진을 사용 하 여 이전 버전의 Windows 10, Windows 8.x 및 Windows 7에 웹 콘텐츠를 표시 합니다. |

### <a name="host-controls"></a>호스트 컨트롤

사용 가능한 래핑된 컨트롤이 포함 하는 시나리오의 경우 WPF 및 Windows Forms 응용 프로그램은 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)의 [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용할 수도 있습니다. 이 컨트롤은 Windows SDK에서 제공 하는 모든 UWP 컨트롤과 사용자 지정 사용자 정의 컨트롤을 포함 하 여 [**Windows. .xaml**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생 되는 모든 uwp 컨트롤을 호스팅할 수 있습니다. 이 컨트롤은 Windows 10 Insider Preview SDK 빌드 17709 이상 릴리스를 지원 합니다.

이러한 컨트롤은 [Microsoft toolkit. wpf](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (wpf 용) 및 [microsoft. Toolkit. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (Windows Forms) 패키지에서 사용할 수 있습니다. 이러한 패키지는 래핑된 컨트롤을 포함 하는 [Microsoft toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 및 [microsoft toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) . 컨트롤 패키지에 포함 되어 있습니다.

### <a name="architecture-overview"></a>아키텍처 개요

여기에서 이러한 컨트롤이 아키텍처 측면에서 구성되는 방법을 빠르게 확인할 수 있습니다.

![호스트 컨트롤 아키텍처](images/xaml-islands/host-controls.png)

이 다이어그램 맨 아래에 표시되는 API는 Windows SDK와 함께 제공됩니다. 래핑된 컨트롤과 호스트 컨트롤은 Windows 커뮤니티 도구 키트의 Nuget 패키지를 통해 사용할 수 있습니다.

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>XAML 아일랜드를 사용 하도록 프로젝트 구성

XAML 아일랜드에는 Windows 10, 버전 1903 이상이 필요 합니다. 응용 프로그램에서 XAML 아일랜드를 사용 하려면 먼저 프로젝트를 설정 해야 합니다.

### <a name="wpf-and-windows-forms"></a>WPF 및 Windows Forms

* Windows 런타임 Api를 사용 하도록 프로젝트를 수정 합니다. 자세한 내용은 [이 문서](desktop-to-uwp-enhance.md#set-up-your-project)를 참조 하세요.

* 프로젝트에 최신 버전의 WPF 용 [Microsoft toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (wpf 용) 또는 [microsoft](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) Windows Forms NuGet 패키지를 설치 해야 합니다. 버전 6.0.0-preview 6.4 이상 버전의 패키지를 설치 했는지 확인 합니다.

### <a name="cwin32"></a>C++/Win32

* Windows 런타임 Api를 사용 하도록 프로젝트를 수정 합니다. 자세한 내용은 [이 문서](desktop-to-uwp-enhance.md#set-up-your-project)를 참조 하세요.
* 다음 작업 중 하나를 수행합니다.

    **MSIX 패키지에서 응용 프로그램을 패키지**합니다. [Msix 패키지](https://docs.microsoft.com/windows/msix/) 에서 응용 프로그램을 패키징하 면 다양 한 배포 및 런타임 이점이 제공 됩니다.
    1. Windows 10 버전 1903 SDK (또는 이후 버전)를 설치 합니다.
    2. [Windows 응용 프로그램 패키징 프로젝트](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 를 솔루션에 추가 하 고 C++/win32 프로젝트에 대 한 참조를 추가 하 여 msix 패키지에 응용 프로그램을 패키지 합니다.

    **응용 프로그램 매니페스트에서 maxversiontested 값을 설정**합니다. MSIX 패키지에서 응용 프로그램을 패키지 하지 않으려는 경우 XAML 아일랜드를 사용 하려면 먼저 프로젝트에 [응용 프로그램 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 를 추가 하 고 매니페스트에 **maxversiontested** 요소를 추가 하 여 응용 프로그램을 지정 해야 합니다. Windows 10 버전 1903 이상과 호환 됩니다.
    1. 프로젝트에 응용 프로그램 매니페스트가 아직 없는 경우 프로젝트에 새 XML 파일을 추가 하 고 이름을 **app.config**로 표시 합니다.
    2. 응용 프로그램 매니페스트에서 다음 예제에 표시 된 것과 같은 **호환성** 요소와 자식 요소를 포함 합니다. **Maxversiontested** 요소의 **Id** 특성을 대상으로 하는 windows 10의 버전 번호로 바꿉니다 (windows 10, 버전 1903 이상 릴리스).

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

        > [!NOTE]
        > 응용 프로그램 매니페스트에 **maxversiontested** 요소를 추가 하면 프로젝트 `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`에 다음 빌드 경고가 표시 될 수 있습니다. 이 경고는 프로젝트에서 문제가 발생 한 것으로 표시 되지 않으며 무시 해도 됩니다.

## <a name="feature-roadmap"></a>기능 로드맵

Windows 10 버전 1903의 릴리스에서는 컨트롤의 버전 1.0 릴리스를 사용할 수 있을 때까지 Windows Community Toolkit의 래핑된 컨트롤 및 호스트 컨트롤이 아직 개발자 미리 보기 상태입니다.

* .NET Framework 4.6.2 이상 버전에 대 한 컨트롤의 버전 1.0은 [toolkit의 6.0 릴리스에서](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)출시 될 예정입니다.
* .NET Core 3 용 컨트롤의 버전 1.0은 toolkit의 이후 릴리스에 사용할 예정입니다.
* .NET Framework 및 .NET Core 3에 대해 이러한 컨트롤의 버전 1.0 릴리스 최신 미리 보기를 시도 하려면 [UWP 커뮤니티 도구 키트](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) 갤러리에서 **6.0.0-preview 6.4** NuGet 패키지를 참조 하세요.

자세한 내용은 참조 하세요. [이 블로그 게시물](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)합니다.

## <a name="additional-resources"></a>추가 자료

XAML 아일랜드 사용에 대 한 자세한 배경 정보 및 자습서는 다음 문서 및 리소스를 참조 하세요.

* [현대화 a WPF 앱 자습서](modernize-wpf-tutorial.md): 이 자습서에서는 Windows 커뮤니티 도구 키트에서 래핑된 컨트롤과 호스트 컨트롤을 사용 하 여 UWP 컨트롤을 기존 WPF lob (기간 업무) 응용 프로그램에 추가 하는 방법에 대 한 단계별 지침을 제공 합니다. 이 자습서에는 WPF 응용 프로그램에 대 한 전체 코드 및 프로세스의 각 단계에 대 한 자세한 지침이 포함 되어 있습니다.
* [XAML 아일랜드 v1-업데이트 및 로드맵](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): 이 블로그 게시물에서는 XAML 아일랜드에 대 한 일반적인 질문에 대해 설명 하 고 자세한 개발 로드맵을 제공 합니다.
