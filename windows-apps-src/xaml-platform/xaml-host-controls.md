---
author: mcleanbyron
description: 이 가이드에서는 WPF 및 Windows Forms 응용 프로그램에서 직접 Fluent 기반 UWP UI를 만들 수 있습니다.
title: 데스크톱 응용 프로그램의 UWP 컨트롤
ms.author: mcleans
ms.date: 09/21/2018
ms.topic: article
keywords: Windows 10, uwp, windows 양식, wpf
ms.localizationpriority: medium
ms.openlocfilehash: a521016849a1ae9b26464e4948cde093e359bf7d
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6857826"
---
# <a name="uwp-controls-in-desktop-applications"></a>데스크톱 응용 프로그램의 UWP 컨트롤

> [!NOTE]
> Api 및이 문서에서 설명 하는 컨트롤은 현재 개발자 미리 보기를 사용할 수 있습니다. 하지만 직접 사용해 프로토타입 작성 하는 코드에서 이제 새, 사용 하는 이러한 프로덕션 코드에서이 시간에 하지 않는 것이 좋습니다. 이러한 Api 및 컨트롤 성숙 안정화 나중에 Windows 릴리스를 계속 합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

이제 Windows 10을 사용 하면 모양, 느낌 및만 UWP 컨트롤을 통해 사용할 수 있는 최신 Windows 10 UI 기능으로 기존 데스크톱 응용 프로그램의 기능을 향상 시킬 수 있도록 UWP 비 데스크톱 응용 프로그램에서 UWP 컨트롤을 사용할 수 있습니다. 즉, [Windows Ink](../design/input/pen-and-stylus-interactions.md) 기존 WPF, Windows Forms, 및 c + + Win32 응용 프로그램의 [흐름 디자인 시스템](../design/fluent-design-system/index.md) 을 지 원하는 컨트롤 등의 UWP 기능을 사용할 수 있습니다. 이 개발자 시나리오는 *XAML 제도*라고도 합니다.

WPF, Windows Forms, 및 c + + Win32 응용 프로그램을 사용 하는 프레임 워크 또는 기술을 따라에서 XAML 제도 사용 하 여 여러 가지 방법으로 제공 합니다.

## <a name="wrapped-controls"></a>래핑된 컨트롤

WPF 및 Windows Forms 응용 프로그램 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)에서 다양 한 래핑된 UWP 컨트롤을 사용할 수 있습니다. 우리 이러한 컨트롤에 *래핑된 컨트롤으로* 때문에 참조 인터페이스 및 특정 UWP 컨트롤의 기능을 래핑합니다. Windows Forms 또는 WPF 프로젝트의 디자인 화면에 직접 이러한 컨트롤을 추가 하 고에 디자이너에서 다른 Windows Forms 또는 WPF 컨트롤 처럼 사용할 수 있습니다.

> [!NOTE]
> 래핑된 컨트롤은 c + + Win32 데스크톱 응용 프로그램에 사용할 수 없습니다. 이러한 유형의 응용 프로그램 [UWP XAML 호스팅 API를](#uwp-xaml-hosting-api)사용 해야 합니다.

다음 래핑된 UWP 컨트롤은 WPF 및 Windows Forms 응용 프로그램에 대 한 현재 사용할 수 있습니다. Windows 커뮤니티 도구 키트의 향후 릴리스에 대 한 자세한 래핑된 UWP 컨트롤 될 예정입니다.

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, 버전 1803 | 웹 콘텐츠를 표시 하는 Microsoft Edge 렌더링 엔진을 사용 합니다. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 자세한 OS 버전과 호환 되는 **웹 보기** 의 버전을 제공 합니다. 이 컨트롤은 Windows 10 버전 1803 이상에서 웹 콘텐츠를 표시 하는 Microsoft Edge 렌더링 엔진 및 이전 버전의 Windows 10, Windows에서 웹 콘텐츠를 표시 하는 Internet Explorer 렌더링 엔진을 사용 8.x 및 Windows 7입니다. |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 참가자 미리 보기 SDK 빌드 17709 | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 Windows Ink 기반 사용자 상호 작용에 대 한 표면 및 관련 도구 모음을 제공 합니다. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 참가자 미리 보기 SDK 빌드 17709 | 스트림 및 Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 비디오와 같은 미디어 콘텐츠를 렌더링 하는 보기를 포함 합니다. |

## <a name="host-controls"></a>호스트 컨트롤

사용 가능한 래핑된 컨트롤 덮인 것 이외의 시나리오의 경우 WPF 및 Windows Forms 응용 프로그램에서 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용할 수도 있습니다. 이 컨트롤에서 [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), 사용자 지정 사용자 컨트롤 뿐만 아니라 Windows SDK에서 제공 하는 모든 UWP 컨트롤을 포함 하 여 파생 된 모든 UWP 컨트롤을 호스트할 수 있습니다. 이 컨트롤은 Windows 10 참가자 미리 보기 SDK 빌드 17709 이상 릴리스를 지원합니다.

> [!NOTE]
> 호스트 컨트롤은 c + + Win32 데스크톱 응용 프로그램에 사용할 수 없습니다. 이러한 유형의 응용 프로그램 [UWP XAML 호스팅 API를](#uwp-xaml-hosting-api)사용 해야 합니다.

## <a name="uwp-xaml-hosting-api"></a>UWP XAML 호스팅 API

C + + Win32 응용 프로그램이 있는 경우 연결 된 창 핸들 (HWND) 응용 프로그램의 모든 UI 요소에 [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 에서 파생 되는 모든 UWP 컨트롤을 호스팅할 *UWP XAML 호스팅 API를* 사용할 수 있습니다. 이 API는 Windows 10 참가자 미리 보기 SDK 빌드 17709에서에서 도입 되었습니다. 이 API를 사용 하는 방법에 대 한 자세한 내용은 [호스팅 API는 데스크톱 응용 프로그램에서 XAML을 사용 하 여](using-the-xaml-hosting-api.md)참조 하세요.

> [!NOTE]
> C + + Win32 데스크톱 응용 프로그램 호스트 UWP 컨트롤을 호스팅 API UWP XAML을 사용 해야 합니다. 래핑된 컨트롤과 호스트는 이러한 유형의 응용 프로그램에 대 한 사용할 수 없습니다. WPF 및 Windows Forms 응용 프로그램에 대 한 권장 래핑된 컨트롤 및 호스트 컨트롤에 UWP XAML 대신 Windows 커뮤니티 도구 키트 사용 호스팅 API입니다. 이러한 컨트롤 호스팅 API 내부적으로 UWP XAML을 사용 하 고 간단한 개발 환경을 제공 합니다. 그러나 선택 하는 경우 WPF 및 Windows Forms 응용 프로그램에서 직접 호스팅 API UWP XAML을 사용할 수 있습니다.

## <a name="architecture-overview"></a>아키텍처 개요

여기에서 이러한 컨트롤이 아키텍처 측면에서 구성되는 방법을 빠르게 확인할 수 있습니다. 이 다이어그램에 사용되는 이름은 변경될 수 있습니다.  

![호스트 컨트롤 아키텍처](images/host-controls.png)

이 다이어그램 맨 아래에 표시되는 API는 Windows SDK와 함께 제공됩니다. 디자이너에 추가할 컨트롤은 Windows 커뮤니티 도구 키트에 NuGet 패키지로 제공됩니다.

이러한 새 컨트롤은 제한 사항이 있으므로 사용하기 전에 잠시 지원되지 않는 사항과 임시 방편으로만 사용할 수 있는 기능을 살펴보세요.

## <a name="limitations"></a>제한 사항

### <a name="whats-supported"></a>지원되는 사항

대부분의 경우 아래 목록에서 명시적으로 언급되지 않으면 모든 사항이 지원됩니다.

### <a name="whats-supported-only-with-workarounds"></a>임시 방편으로만 지원되는 사항

:heavy_check_mark: 다중 창 내의 여러 받은 편지함 컨트롤을 호스팅. 자체 스레드에 각 창을 배치해야 합니다.

:heavy_check_mark: 호스트된 컨트롤로 ``x:Bind`` 사용. .NET 표준 라이브러리에서 데이터 모델을 선언해야 합니다.

:heavy_check_mark: C# 기반 타사 컨트롤. 타사 컨트롤에 대한 소스 코드가 있는 경우 이에 대해 컴파일할 수 있습니다.

### <a name="whats-not-yet-supported"></a>아직 지원되지 않는 사항

:no_entry_sign: 응용 프로그램 및 호스트된 컨트롤에서 원활하게 작동하는 접근성 도구.

:no_entry_sign: Windows 앱 패키지를 포함하지 않는 응용 프로그램에 추가하는 컨트롤의 지역화된 콘텐츠.

:no_entry_sign: Windows 앱 패키지를 포함하지 않는 응용 프로그램 내의 XAML에서 만든 자산 참조.

:no_entry_sign: DPI 및 규모의 변경에 제대로 응답하는 컨트롤.

:no_entry_sign: 사용자 지정 사용자 컨트롤(스레드에서, 스레드 밖에서 또는 프로세서 밖에서)에 **웹 보기** 컨트롤 추가.

:no_entry_sign: [강조 표시](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent 효과.

:no_entry_sign: 입력 컨트롤에 대한 수동 입력, @Places 및 @People.

:no_entry_sign: 바로 가기를 키 할당.

:no_entry_sign: C++ 기반 타사 컨트롤.

:no_entry_sign: 사용자 지정 사용자 컨트롤 호스팅.

바탕 화면에 Fluent를 가져오는 환경을 개선하기 위해 이 목록의 항목은 계속해서 변경될 수 있습니다.  
