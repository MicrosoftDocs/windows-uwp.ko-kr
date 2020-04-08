---
description: 이 가이드에서는 WPF 및 Windows Forms 애플리케이션에서 직접 Fluent 기반 UWP UI를 만드는 방법을 설명합니다.
title: 데스크톱 앱의 UWP 컨트롤
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 0f596047cfdd01fcfca568ea1c63b1e2cc14c272
ms.sourcegitcommit: 1670eec29b4360ec37cde2910b76078429273cb0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80329503"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)

Windows 10 버전 1903부터 *XAML Island*라는 기능을 사용하여 UWP 이외의 데스크톱 애플리케이션에서 UWP 컨트롤을 호스트할 수 있습니다. 이 기능을 사용하면 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 통해 기존 WPF, Windows Forms 및 C++ Win32 애플리케이션의 모양, 느낌 및 기능을 향상시킬 수 있습니다. 즉, [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)와 같은 UWP 기능과 [Fluent Design 시스템](/windows/uwp/design/fluent-design-system/index)을 지원하는 컨트롤을 기존 WPF, Windows Forms 및 C++ Win32 애플리케이션에서 사용할 수 있습니다.

다음을 포함하여 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생되는 모든 UWP 컨트롤을 호스트할 수 있습니다.

* Windows SDK에서 제공하는 모든 자사 UWP 컨트롤
* 사용자 지정 UWP 컨트롤(예: 함께 작동하는 여러 UWP 컨트롤로 구성된 사용자 컨트롤) 애플리케이션을 사용하여 컴파일할 수 있도록 사용자 지정 컨트롤에 대한 소스 코드가 있어야 합니다.

기본적으로 XAML Island는 *UWP XAML 호스팅 API*를 사용하여 생성됩니다. 이 API는 Windows 10, 버전 1903 SDK에 도입된 몇 가지 Windows 런타임 클래스 및 COM 인터페이스로 구성됩니다. 또한 내부적으로 UWP XAML 호스팅 API를 사용하고 WPF 및 Windows Forms 앱을 보다 편리하게 개발할 수 있는 환경을 제공하는 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)에 XAML Island .NET 컨트롤 세트가 제공됩니다.

애플리케이션 유형 및 호스트할 UWP 컨트롤의 유형에 따라 XAML Island를 사용하는 방법이 달라집니다.

> [!NOTE]
> XAML Island에 대한 피드백이 있는 경우 [Microsoft.Toolkit.Win32 리포지토리](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)에서 새 이슈를 만들고 의견을 남겨 주세요. 피드백을 개인적으로 제출하는 것을 선호하는 경우 XamlIslandsFeedback@microsoft.com으로 보낼 수 있습니다. 귀하의 인사이트와 시나리오는 Microsoft에 매우 중요합니다.

## <a name="requirements"></a>요구 사항

XAML Islands의 런타임 요구 사항은 다음과 같습니다.

* Windows 10 버전 1903 이상 릴리스.
* 애플리케이션이 배포용 [MSIX 패키지](https://docs.microsoft.com/windows/msix)에 패키징되지 않는 경우에는 컴퓨터에 [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)이 설치되어 있어야 합니다.

## <a name="wpf-and-windows-forms-applications"></a>WPF 및 Windows Forms 애플리케이션

WPF 및 Windows Forms 애플리케이션은 Windows 커뮤니티 도구 키트에 제공되는 XAML Island .NET 컨트롤을 사용하는 것이 좋습니다. 이러한 컨트롤은 해당 UWP 컨트롤의 속성, 메서드 및 이벤트를 모방하거나 그에 대한 액세스를 제공하는 개체 모델을 제공합니다. 또한 키보드 탐색 및 레이아웃 변경과 같은 동작을 처리합니다.

WPF 및 Windows Forms 애플리케이션용 XAML Island 컨트롤 세트는 두 가지가 있으며, *래핑된 컨트롤*과 *호스트 컨트롤*입니다. 

### <a name="wrapped-controls"></a>래핑된 컨트롤

WPF 및 Windows Forms 애플리케이션은 특정 UWP 컨트롤의 인터페이스 및 기능을 래핑하는 선별된 XAML Island 컨트롤을 사용할 수 있습니다. WPF 또는 Windows Forms 프로젝트의 디자인 화면에 이러한 컨트롤을 바로 추가하고 디자이너에서 다른 WPF 또는 Windows Forms 컨트롤처럼 사용할 수 있습니다.

현재 Windows 커뮤니티 도구 키트에 제공되는 래핑된 UWP 컨트롤은 다음과 같습니다. 

| 컨트롤 | 지원되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 애플리케이션에서 Windows Ink 기반 사용자 조작을 위한 화면 및 관련 도구 모음을 제공합니다. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 애플리케이션에서 비디오와 같은 미디어 콘텐츠를 스트리밍하고 렌더링하는 보기를 포함합니다. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, 버전 1903 | Windows Forms 또는 WPF 데스크톱 애플리케이션에서 기호화된 지도 또는 실사 지도를 표시할 수 있습니다. |

래핑된 UWP 컨트롤을 사용하는 방법을 보여주는 연습은 [WPF 앱에서 표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands.md)를 참조하세요.

### <a name="host-controls"></a>호스트 컨트롤

사용 가능한 래핑된 컨트롤에서 다루지 않는 사용자 지정 컨트롤 및 기타 시나리오의 경우 WPF 및 Windows Forms 애플리케이션은 Windows 커뮤니티 도구 키트에서 제공되는 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용할 수도 있습니다.

| 컨트롤 | 지원되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, 버전 1903 | Windows SDK에서 제공하는 자사 UWP 컨트롤과 사용자 지정 컨트롤을 포함하여 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생되는 모든 UWP 컨트롤을 호스트할 수 있습니다. |

**WindowsXamlHost** 컨트롤을 사용하는 방법을 보여주는 연습은 [WPF 앱에서 표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands.md) 및 [XAML Island를 사용하여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands.md)를 참조하세요.

> [!NOTE]
> **WindowsXamlHost** 컨트롤을 사용한 사용자 지정 UWP 컨트롤 호스트는 .NET Core 3를 대상으로 하는 WPF 및 Windows Forms 앱에서만 지원됩니다. Windows SDK에서 제공하는 자사 UWP 컨트롤 호스트는 .NET Framework 또는 .NET Core 3를 대상으로 하는 앱에서 지원됩니다.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>XAML Island .NET 컨트롤을 사용하도록 프로젝트 구성

XAML Island .NET 컨트롤을 사용하려면 Windows 10, 버전 1903 이상 버전이 필요합니다. 이러한 컨트롤을 사용하려면 아래에 나열된 NuGet 패키지 중 하나를 설치하세요. 이러한 패키지는 XAML Island의 래핑된 컨트롤과 호스트 컨트롤을 사용하는 데 필요한 모든 것을 제공하며, 필요한 다른 관련 NuGet 패키지도 포함하고 있습니다.

| 컨트롤 유형 | NuGet 패키지  | 관련된 문서 |
|-----------------|----------------|---------------------|
| [래핑된 컨트롤](#wrapped-controls) | 다음 패키지의 버전 6.0.0 이상: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [WPF 앱에서 표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands.md)  |
| [호스트 컨트롤](#host-controls) | 다음 패키지의 버전 6.0.0 이상: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [WPF 앱에서 표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands.md)<br/>[WPF 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands.md)  |

다음 사항에 유의하세요.

* 호스트 컨트롤 패키지는 래핑된 컨트롤 패키지에도 포함됩니다. 두 컨트롤 세트를 모두 사용하려면 래핑된 컨트롤 패키지를 설치하면 됩니다.

* 사용자 지정 UWP 컨트롤을 호스트하는 경우 WPF 또는 Windows Forms 프로젝트가 .NET Core 3를 대상으로 해야 합니다. 사용자 지정 UWP 컨트롤 호스트는 .NET Framework를 대상으로 하는 앱에서 지원되지 않습니다. 또한 사용자 지정 컨트롤을 참조하려면 몇 가지 추가 단계를 수행해야 합니다. 자세한 내용은 [XAML Island를 사용하여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands.md)를 참조하세요.

### <a name="web-view-controls"></a>웹 보기 컨트롤

Windows 커뮤니티 도구 키트는 WPF 및 Windows Forms 애플리케이션에서 웹 콘텐츠를 호스트하는 데 필요한 다음과 같은 .NET 컨트롤도 제공합니다. 이러한 컨트롤은 XAML Island 컨트롤과 유사한 데스크톱 앱 현대화 시나리오에서 종종 사용되며, XAML Island 컨트롤과 동일한 [Microsoft.Toolkit.Win32 리포지토리](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)에서 유지 관리됩니다.

| 컨트롤 | 지원되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, 버전 1803 | Microsoft Edge 렌더링 엔진을 사용하여 웹 콘텐츠를 표시합니다. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 더 많은 OS 버전과 호환되는 **WebView** 버전을 제공합니다. 이 컨트롤은 Microsoft Edge 렌더링 엔진을 사용하여 Windows 10 버전 1803 이상에 웹 콘텐츠를 표시하고 Internet Explorer 렌더링 엔진을 사용하여 이전 버전의 Windows 10, Windows 8.x 및 Windows 7에 웹 콘텐츠를 표시합니다. |

이러한 컨트롤을 사용하려면 다음과 같은 NuGet 패키지 중 하나를 설치하세요.

* WPF: [Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++ Win32 애플리케이션

XAML Island .NET 컨트롤은 C++ Win32 애플리케이션에서 지원되지 않습니다. 이러한 애플리케이션은 Windows 10 SDK(버전 1903 이상)에서 제공하는 *UWP XAML 호스팅 API*를 대신 사용해야 합니다.

UWP XAML 호스팅 API는 C++ Win32 애플리케이션이 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생되는 모든 UWP 컨트롤을 호스트하는 데 사용할 수 있는 몇 가지 Windows 런타임 클래스 및 COM 인터페이스로 구성됩니다. 연결된 창 핸들(HWND)이 있는 애플리케이션의 모든 UI 요소에서 UWP 컨트롤을 호스트할 수 있습니다. 이 API에 대한 자세한 내용은 다음 문서를 참조하세요.

* [C++ Win32 앱에서 UWP XAML 호스팅 API 사용](using-the-xaml-hosting-api.md)
* [C++ Win32 앱에서 표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스팅](host-custom-control-with-xaml-islands-cpp.md)

> [!NOTE]
> Windows 커뮤니티 도구 키트의 래핑된 컨트롤 및 호스트 컨트롤은 내부적으로 UWP XAML 호스팅 API를 사용하고, 사용자가 UWP XAML 호스팅 API를 직접 사용한 경우 키보드 탐색 및 레이아웃 변경을 포함하여 직접 처리해야 하는 모든 동작을 구현합니다. WPF 및 Windows Forms 애플리케이션의 경우 API 사용에 대한 많은 구현 세부 정보를 추상화하므로 UWP XAML 호스팅 API를 직접 사용하는 대신 이러한 컨트롤을 사용하는 것이 좋습니다.

## <a name="architecture-of-xaml-islands"></a>XAML 아일랜드의 아키텍처

UWP XAML 호스팅 API를 기반으로 다양한 유형의 XAML Island 컨트롤의 아키텍처를 구성하는 방법을 간략히 살펴보겠습니다.

![호스트 컨트롤 아키텍처](images/xaml-islands/host-controls.png)

이 다이어그램 맨 아래에 표시되는 API는 Windows SDK와 함께 제공됩니다. 래핑된 컨트롤과 호스트 컨트롤은 Windows 커뮤니티 도구 키트의 NuGet 패키지를 통해 사용할 수 있습니다.

## <a name="limitations-and-workarounds"></a>제한 사항 및 해결 방법

다음 섹션에서는 XAML Islands를 사용하는 데스크톱 앱의 특정 UWP 개발 시나리오에 대한 제한 사항 및 해결 방법을 설명합니다. 

### <a name="supported-only-with-workarounds"></a>임시 방편으로만 지원

:heavy_check_mark: XAML Island에서 XAML 콘텐츠 트리의 루트 요소에 액세스하여 루트 요소가 호스팅되는 컨텍스트에 대한 관련 정보를 얻으려면 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) 및 [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)를 사용하지 마세요. 그 대신 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 클래스를 사용하세요. 자세한 내용은 [이 섹션](#window-host-context-for-xaml-islands)을 참조하세요.

:heavy_check_mark: WPF, Windows Forms 또는 C++ Win32 앱에서 [공유 계약](/windows/uwp/app-to-app/share-data)을 지원하려면 앱에서 [IDataTransferManagerInterop](https://docs.microsoft.com/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) 인터페이스를 사용하여 특정 창의 공유 작업을 시작하는 [DataTransferManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) 개체를 가져와야 합니다. WPF 앱에서 이 인터페이스를 사용하는 방법을 보여주는 샘플은 [ShareSource 샘플](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource)을 참조하세요.

:heavy_check_mark: XAML Islands에 호스팅된 컨트롤에는 `x:Bind`를 사용할 수 없습니다. .NET Standard 라이브러리에서 데이터 모델을 선언해야 합니다.

### <a name="not-supported"></a>지원되지 않음

:no_entry_sign: [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 .NET Framework를 대상으로 하는 WPF 및 Windows Forms 앱에 C# 기반 타사 UWP 컨트롤 호스팅. 이 시나리오는 .NET Core 3를 대상으로 하는 앱에서만 지원됩니다.

:no_entry_sign: XAML Islands의 UWP XAML 콘텐츠는 런타임에 Windows 테마가 어두운 색에서 밝은 색으로 또는 그 반대로 변경되어도 응답하지 않습니다. 콘텐츠는 런타임에 고대비 변경에 응답합니다.

:no_entry_sign: 사용자 지정 사용자 컨트롤(스레드에서, 스레드 밖에서 또는 프로세서 밖에서)에 **WebView** 컨트롤 추가.

:no_entry_sign: [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 컨트롤 및 [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) 호스트 컨트롤은 전체 화면 모드를 지원하지 않습니다.

:no_entry_sign: 필기 보기를 사용한 텍스트 입력. 이 기능에 대한 자세한 내용은 [이 문서](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-handwriting-view)를 참조하세요.

:no_entry_sign: `@Places` 및 `@People` 콘텐츠 링크를 사용하는 텍스트 컨트롤. 이 기능에 대한 자세한 내용은 [이 문서](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/content-links)를 참조하세요.

### <a name="window-host-context-for-xaml-islands"></a>XAML 아일랜드에 대한 창 호스트 컨텍스트

데스크톱 앱에서 XAML 아일랜드를 호스팅하는 경우 동일한 스레드에서 여러 개의 XAML 콘텐츠 트리를 동시에 실행할 수 있습니다. XAML 아일랜드에서 XAML 콘텐츠 트리의 루트 요소에 액세스하고 해당 요소가 호스팅되는 컨텍스트에 대한 관련 정보를 가져오려면 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 클래스를 사용합니다. [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) 및 [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) 클래스는 XAML 아일랜드에 대한 올바른 정보를 제공하지 않습니다. [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 및 [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) 개체는 스레드에 있으며 앱에서 액세스 할 수 있지만, 의미 있는 범위 또는 표시 유형을 반환하지 않습니다(항상 표시되지 않으며, 1x1 크기임). 자세한 내용은 [창 작업 호스트](/windows/uwp/design/layout/show-multiple-views#windowing-hosts)를 참조하세요.

예를 들어 XAML 아일랜드에서 호스팅되는 UWP 컨트롤이 포함된 창의 경계 사각형을 가져오려면 컨트롤의 [XamlRoot.Size](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot.size) 속성을 사용합니다. XAML 아일랜드에서 호스팅할 수 있는 모든 UWP 컨트롤은 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)에서 파생되므로 컨트롤의 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.xamlroot) 속성을 사용하여 **XamlRoot** 개체에 액세스할 수 있습니다.

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

[CoreWindows.Bounds](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.bounds) 속성을 사용하여 경계 사각형을 가져오지 않습니다.

```csharp
// This will return incorrect information for a UWP control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

XAML 아일랜드의 컨텍스트에서 사용하지 않아야 하는 일반적인 창 작업 관련 API 및 추천되는 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 대체 항목에 대한 표는 [이 섹션](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts)의 표를 참조하세요.

WPF 앱에서 이 인터페이스를 사용하는 방법을 보여주는 샘플은 [ShareSource 샘플](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource)을 참조하세요.

## <a name="feature-roadmap"></a>기능 로드맵

다음은 XAML Islands 관련 기능의 현재 상황입니다.

* **C++ Win32 앱:** UWP XAML 호스팅 API는 Windows 10, 버전 1903부터 버전 1.0으로 간주됩니다.
* **.NET Framework 4.6.2 이상을 대상으로 하는 관리형 앱:** [버전 6.0.0 NuGet 패키지](#configure-your-project-to-use-the-xaml-island-net-controls)에서 사용할 수 있는 XAML Island 컨트롤은 .NET Framework 4.6.2 이상을 대상으로 하는 앱의 버전 1.0으로 간주됩니다.
* **.NET Core 3.0 이상을 대상으로 하는 관리형 앱:** [버전 6.0.0 NuGet 패키지](#configure-your-project-to-use-the-xaml-island-net-controls)에서 사용할 수 있는 컨트롤은 .NET Core 3.0 이상을 대상으로 하는 앱의 개발자 미리 보기 버전입니다. .NET Core 3.0 이상을 위한 이러한 컨트롤의 버전 1.0 릴리스는 이후 릴리스용으로 계획되어 있습니다.

## <a name="additional-resources"></a>추가 리소스

XAML Island 사용에 대한 자세한 배경 정보 및 자습서는 다음 문서 및 리소스를 참조하세요.

* [WPF 앱 현대화 자습서](modernize-wpf-tutorial.md): 이 자습서에서는 Windows 커뮤니티 도구 키트에서 래핑된 컨트롤과 호스트 컨트롤을 사용하여 UWP 컨트롤을 기존 WPF LOB(기간 업무) 애플리케이션에 추가하는 방법에 대한 단계별 지침을 제공합니다. 이 자습서에는 WPF 애플리케이션의 전체 코드 및 프로세스의 각 단계에 대한 자세한 지침이 포함되어 있습니다.
* [XAML 아일랜드 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples): 이 리포지토리에는 XAML 아일랜드 사용 방법을 보여주는 Windows Forms, WPF 및 C++/Win32 샘플이 포함되어 있습니다.
* [XAML Island v1 - 업데이트 및 로드맵](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): 이 블로그 게시물에서는 XAML Island에 대한 일반적인 질문을 설명하고 자세한 개발 로드맵을 제공합니다.
