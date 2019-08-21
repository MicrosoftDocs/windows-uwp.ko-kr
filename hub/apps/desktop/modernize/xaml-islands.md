---
description: 이 가이드에서는 WPF 및 Windows Forms 응용 프로그램에서 직접 Fluent 기반 UWP UI를 만들 수 있습니다.
title: 데스크톱 앱의 UWP 컨트롤
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bd49417d110759dc9fec4ff4c9003e842bf1d7bb
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643355"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>데스크톱 앱에서 UWP XAML 컨트롤 호스트 (XAML 제도)

Windows 10 버전 1903부터 *XAML 아일랜드*라는 기능을 사용 하 여 uwp 이외의 데스크톱 응용 프로그램에서 uwp 컨트롤을 호스트할 수 있습니다. 이 기능을 사용 하면 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 C++ 기존 WPF, Windows Forms 및 Win32 응용 프로그램의 모양, 느낌 및 기능을 향상 시킬 수 있습니다. 즉, 기존 WPF, Windows Forms 및 C++ Win32 응용 프로그램에서 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/index) 을 지 원하는 [WINDOWS 잉크](/windows/uwp/design/input/pen-and-stylus-interactions) 및 컨트롤과 같은 UWP 기능을 사용할 수 있습니다.

다음을 포함 하 여 [Windows. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생 된 모든 UWP 컨트롤을 호스트할 수 있습니다.

* Windows SDK 또는 WinUI 라이브러리에서 제공 하는 모든 자사 UWP 컨트롤입니다.
* 사용자 지정 UWP 컨트롤 (예: 함께 작동 하는 여러 UWP 컨트롤로 구성 된 사용자 정의 컨트롤) 응용 프로그램을 사용 하 여 컴파일할 수 있도록 사용자 지정 컨트롤에 대 한 소스 코드가 있어야 합니다.

기본적으로 XAML 아일랜드는 *UWP xaml 호스팅 API*를 사용 하 여 만듭니다. 이 API는 Windows 10 버전 1903 SDK에 도입 된 몇 가지 Windows 런타임 클래스 및 COM 인터페이스로 구성 됩니다. 또한 내부적으로 UWP XAML 호스팅 API를 사용 하 고 WPF 및 Windows Forms 앱에 보다 편리한 개발 환경을 제공 하는 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) 의 XAML 아일랜드 .net 컨트롤 집합을 제공 합니다.

XAML 아일랜드를 사용 하는 방법은 응용 프로그램 형식 및 호스팅할 UWP 컨트롤의 형식에 따라 달라 집니다.

> [!NOTE]
> XAML 아일랜드에 대 한 피드백이 있는 경우 [Microsoft Toolkit 리포지토리의](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) 새 문제를 만들고 의견을 남겨 두세요. 사용자 의견을 개인적으로 제출 하는 것을 선호 하는 경우 XamlIslandsFeedback@microsoft.com로 보낼 수 있습니다. 귀하의 통찰력 및 시나리오는 매우 중요 합니다.

## <a name="wpf-and-windows-forms-applications"></a>WPF 및 Windows Forms 응용 프로그램

WPF 및 Windows Forms 응용 프로그램은 Windows 커뮤니티 도구 키트에서 제공 되는 XAML 아일랜드 .NET 컨트롤을 사용 하는 것이 좋습니다. 이러한 컨트롤은 해당 UWP 컨트롤의 속성, 메서드 및 이벤트를 모방 하거나 액세스를 제공 하는 개체 모델을 제공 합니다. 또한 키보드 탐색 및 레이아웃 변경과 같은 동작을 처리 합니다.

WPF 및 Windows Forms 응용 프로그램에는 *래핑된 컨트롤과* *호스트 컨트롤*의 두 가지 XAML 아일랜드 컨트롤이 있습니다. Windows 10 버전 1903부터 이러한 컨트롤은 [개발자 미리 보기로 제공](#feature-roadmap)됩니다.

### <a name="wrapped-controls"></a>래핑된 컨트롤

WPF 및 Windows Forms 응용 프로그램에서는 특정 UWP 컨트롤의 인터페이스 및 기능을 래핑하는 XAML 아일랜드 컨트롤을 선택할 수 있습니다. 이러한 컨트롤을 WPF 또는 Windows Forms 프로젝트의 디자인 화면에 직접 추가 하 고 디자이너의 다른 WPF 또는 Windows Forms 컨트롤과 같은 방식으로 사용할 수 있습니다.

다음 래핑된 UWP 컨트롤은 현재 Windows 커뮤니티 도구 키트에서 사용할 수 있습니다. 

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 Windows Ink 기반 사용자 조작을 위한 화면 및 관련 도구 모음을 제공 합니다. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 비디오와 같은 미디어 콘텐츠를 스트리밍하 고 렌더링 하는 뷰를 포함 합니다. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 기호화 된 지도 또는 실사 지도를 표시할 수 있습니다. |

래핑된 UWP 컨트롤을 사용 하는 방법을 보여 주는 연습은 [WPF 앱에서 표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands.md)을 참조 하세요.

### <a name="host-controls"></a>호스트 컨트롤

사용 가능한 래핑된 컨트롤이 포함 하는 시나리오의 경우 WPF 및 Windows Forms 응용 프로그램은 Windows 커뮤니티 도구 키트에서 제공 되는 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용할 수도 있습니다.

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, 버전 1903 | 는 Windows SDK에서 제공 하는 모든 자사 UWP 컨트롤과 사용자 지정 컨트롤을 포함 하 여 [Windows. .xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생 되는 모든 uwp 컨트롤을 호스팅할 수 있습니다. |

**Windowsxamlhost** 컨트롤을 사용 하는 방법을 보여 주는 연습은 [wpf 앱에서 표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands.md) 및 [XAML 아일랜드를 사용 하 여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스팅](host-custom-control-with-xaml-islands.md)을 참조 하세요.

> [!NOTE]
> **Windowsxamlhost** 컨트롤을 사용 하 여 사용자 지정 UWP 컨트롤을 호스트 하는 것은 WPF 및 .net Core 3을 대상으로 하는 앱 Windows Forms 에서만 지원 됩니다. Windows SDK에서 제공 하는 자사 UWP 컨트롤을 호스트 하는 것은 .NET Framework 또는 .NET Core 3을 대상으로 하는 앱에서 지원 됩니다.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>XAML 아일랜드 .NET 컨트롤을 사용 하도록 프로젝트 구성

XAML 아일랜드 .NET 컨트롤에는 Windows 10, 버전 1903 이상 버전이 필요 합니다. 이러한 컨트롤을 사용 하려면 아래에 나열 된 NuGet 패키지 중 하나를 설치 합니다. 이러한 패키지는 XAML 아일랜드 래핑된 컨트롤과 호스트 컨트롤을 사용 하는 데 필요한 모든 것을 제공 하며, 이러한 패키지에는 필요한 다른 관련 NuGet 패키지도 포함 되어 있습니다.

| 컨트롤의 형식 | NuGet 패키지  | 관련 문서 |
|-----------------|----------------|---------------------|
| [래핑된 컨트롤](#wrapped-controls) | 버전 6.0.0-다음 패키지의 preview7 이상: <ul><li>WPF: [Microsoft Toolkit. 컨트롤](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft Toolkit. 컨트롤](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [WPF 앱에서 표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands.md)  |
| [호스트 컨트롤](#host-controls) | 버전 6.0.0-다음 패키지의 preview7 이상: <ul><li>WPF: [Microsoft Toolkit. Wpf. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft Toolkit. Xaml.](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [WPF 앱에서 표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands.md)<br/>[WPF 앱에서 사용자 지정 UWP 컨트롤 호스팅](host-custom-control-with-xaml-islands.md)  |

다음 사항에 유의 하세요.

* 호스트 컨트롤 패키지는 래핑된 컨트롤 패키지에도 포함 됩니다. 두 컨트롤 집합을 모두 사용 하려면 래핑된 컨트롤 패키지를 설치할 수 있습니다.

* 사용자 지정 UWP 컨트롤을 호스트 하는 경우 WPF 또는 Windows Forms 프로젝트는 .NET Core 3을 대상으로 해야 합니다. 사용자 지정 UWP 컨트롤을 호스트 하는 것은 .NET Framework를 대상으로 하는 앱에서 지원 되지 않습니다. 또한 사용자 지정 컨트롤을 참조 하려면 몇 가지 추가 단계를 수행 해야 합니다. 자세한 내용은 [XAML 아일랜드를 사용 하 여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스팅](host-custom-control-with-xaml-islands.md)을 참조 하세요.

* 이러한 지침의 이전 버전에서는 WPF 또는 Windows Forms `maxversiontested` 프로젝트의 응용 프로그램 매니페스트에 요소를 추가 했습니다. 위에 나열 된 NuGet 패키지의 최신 미리 보기 버전을 사용 하는 동안에는이 요소를 매니페스트에 더 이상 추가할 필요가 없습니다.

### <a name="architecture-of-xaml-island-net-controls"></a>XAML 아일랜드 .NET 컨트롤의 아키텍처

다음은 UWP XAML 호스팅 API를 기반으로 다양 한 유형의 XAML 아일랜드 컨트롤을 구조적으로 구성 하는 방법을 간략히 보여 줍니다.

![호스트 컨트롤 아키텍처](images/xaml-islands/host-controls.png)

이 다이어그램 맨 아래에 표시되는 API는 Windows SDK와 함께 제공됩니다. 래핑된 컨트롤과 호스트 컨트롤은 Windows 커뮤니티 도구 키트의 NuGet 패키지를 통해 사용할 수 있습니다.

### <a name="web-view-controls"></a>웹 보기 컨트롤

Windows 커뮤니티 도구 키트는 WPF 및 Windows Forms 응용 프로그램에서 웹 콘텐츠를 호스팅하기 위한 다음과 같은 .NET 컨트롤도 제공 합니다. 이러한 컨트롤은 종종 XAML 아일랜드 컨트롤과 유사한 데스크톱 앱 현대화 시나리오에서 사용 되 고 XAML 아일랜드 컨트롤과 동일한 [Microsoft Toolkit 리포지토리의](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 리포지토리에 유지 됩니다.

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [웹 보기](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, 버전 1803 | Microsoft Edge 렌더링 엔진을 사용 하 여 웹 콘텐츠를 표시 합니다. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 더 많은 OS 버전과 호환 되는 **웹 보기** 버전을 제공 합니다. 이 컨트롤은 Microsoft Edge 렌더링 엔진을 사용 하 여 Windows 10 버전 1803 이상에 웹 콘텐츠를 표시 하 고 Internet Explorer 렌더링 엔진을 사용 하 여 이전 버전의 Windows 10, Windows 8.x 및 Windows 7에 웹 콘텐츠를 표시 합니다. |

이러한 컨트롤을 사용 하려면 다음 NuGet 패키지 중 하나를 설치 합니다.

* WPF: [Microsoft Toolkit. 컨트롤이 나 웹 보기](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft. Toolkit. 컨트롤이 나 웹 보기](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Win32 응용 프로그램

XAML 아일랜드 .NET 컨트롤은 Win32 응용 프로그램에서 C++ 지원 되지 않습니다. 이러한 응용 프로그램은 Windows 10 SDK (버전 1903 이상)에서 제공 하는 *UWP XAML 호스팅 API* 를 대신 사용 해야 합니다.

UWP XAML 호스팅 API는 C++ Win32 응용 프로그램에서 [Windows .xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생 되는 UWP 컨트롤을 호스트 하는 데 사용할 수 있는 몇 가지 Windows 런타임 클래스 및 COM 인터페이스로 구성 됩니다. 연결 된 창 핸들 (HWND)이 있는 응용 프로그램의 모든 UI 요소에서 UWP 컨트롤을 호스트할 수 있습니다. 필수 구성 요소를 포함 하 여이 API에 대 한 자세한 내용은 [ C++ WIN32 앱에서 UWP XAML 호스팅 API 사용](using-the-xaml-hosting-api.md)을 참조 하세요.

> [!NOTE]
> Windows Community Toolkit의 래핑된 컨트롤 및 호스트 컨트롤은 내부적으로 UWP XAML 호스팅 API를 사용 하 고, 사용자가 UWP XAML 호스팅 API를 직접 사용 하는 경우 키보드 탐색을 포함 하 여 직접 처리 해야 하는 모든 동작을 구현 합니다. 및 레이아웃이 변경 되었습니다. WPF 및 Windows Forms 응용 프로그램의 경우 API 사용에 대 한 많은 구현 세부 정보를 추상화 하므로 UWP XAML 호스팅 API 대신 이러한 컨트롤을 직접 사용 하는 것이 좋습니다.

## <a name="feature-roadmap"></a>기능 로드맵

Windows 10 버전 1903 버전부터, Windows Community Toolkit의 XAML 아일랜드 .NET 컨트롤은 버전 1.0의 컨트롤을 사용할 수 있을 때까지 개발자 미리 보기 상태입니다.

* .NET Framework 4.6.2 이상 버전에 대 한 컨트롤의 버전 1.0은 [toolkit의 6.0 릴리스에서](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)출시 될 예정입니다.
* .NET Core 3 용 컨트롤의 버전 1.0은 toolkit의 이후 릴리스에 사용할 예정입니다.
* .NET Framework 및 .NET Core 3에 대해 이러한 컨트롤의 버전 1.0 버전에 대 한 최신 미리 보기를 시도 하려면 [UWP 커뮤니티 도구 키트](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) 갤러리에서 버전 6.0.0-preview7 (또는 이후 버전) NuGet 패키지를 참조 하세요.

자세한 내용은 참조 하세요. [이 블로그 게시물](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)합니다.

## <a name="additional-resources"></a>추가 자료

XAML 아일랜드 사용에 대 한 자세한 배경 정보 및 자습서는 다음 문서 및 리소스를 참조 하세요.

* [현대화 a WPF 앱 자습서](modernize-wpf-tutorial.md): 이 자습서에서는 Windows 커뮤니티 도구 키트에서 래핑된 컨트롤과 호스트 컨트롤을 사용 하 여 UWP 컨트롤을 기존 WPF lob (기간 업무) 응용 프로그램에 추가 하는 방법에 대 한 단계별 지침을 제공 합니다. 이 자습서에는 WPF 응용 프로그램에 대 한 전체 코드 및 프로세스의 각 단계에 대 한 자세한 지침이 포함 되어 있습니다.
* [XAML 아일랜드 v1-업데이트 및 로드맵](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): 이 블로그 게시물에서는 XAML 아일랜드에 대 한 일반적인 질문에 대해 설명 하 고 자세한 개발 로드맵을 제공 합니다.
