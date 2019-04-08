---
description: 이 가이드에서는 WPF 및 Windows Forms 응용 프로그램에서 직접 Fluent 기반 UWP UI를 만들 수 있습니다.
title: 데스크톱 응용 프로그램의 UWP 컨트롤
ms.date: 01/11/2019
ms.topic: article
keywords: Windows 10, uwp, windows 양식, wpf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf25fea6ca6e8809c12324ae57a42cc712ded2a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619708"
---
# <a name="uwp-controls-in-desktop-applications"></a>데스크톱 응용 프로그램의 UWP 컨트롤

> [!NOTE]
> XAML 제도 개발자 미리 보기로 현재 사용할 수 있습니다. 직접 사용해 고유의 프로토타입 코드에서 이제 하는 것이 좋습니다, 있지만 사용 것 프로덕션 코드에서는 현재 하지 않는 것이 좋습니다. 이러한 Api 및 컨트롤 성숙 하 고 안정화 향후 Windows 릴리스에서 계속 됩니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.
>
> XAML 아일랜드에 대 한 의견이 있는 경우에서 새 문제를 작성 합니다 [WindowsCommunityToolkit 리포지토리](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) 두고 의견 있습니다. 개인적으로 피드백을 제출 하려는 경우에 보낼 수 있습니다를 XamlIslandsFeedback@microsoft.com입니다. Insights 및 시나리오에는 우리에 게 매우 중요 합니다.

이제 Windows 10을 사용 하면 모양, 느낌 및 UWP 컨트롤을 통해 이용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 기존 데스크톱 응용 프로그램의 기능을 향상 시킬 수 있도록 비 UWP 데스크톱 응용 프로그램에서 UWP 컨트롤을 사용할 수 있습니다. 즉, 같은 UWP 기능을 사용할 수 [Windows 잉크](../design/input/pen-and-stylus-interactions.md) 지 원하는 컨트롤 및 합니다 [Fluent Design System](../design/fluent-design-system/index.md) 기존 WPF, Windows Forms 및 c + + Win32 응용 프로그램에서 합니다. 이 개발자 시나리오 라고 *XAML 제도*합니다.

응용 프로그램에서 WPF, Windows Forms 및 Win32 c + + 기술 이나 사용할 프레임 워크에 따라 XAML 제도 사용 하는 여러 방법을 제공 합니다.

## <a name="wrapped-controls"></a>래핑된 컨트롤

WPF 및 Windows Forms 응용 프로그램에서 래핑된 UWP 컨트롤의 선택 영역을 사용할 수는 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)합니다. 이러한 컨트롤 이라고 *래핑된 컨트롤* 인터페이스와 기능의 특정 UWP 컨트롤을 래핑하고 있으므로. WPF 또는 Windows Forms 프로젝트의 디자인 화면으로 직접 이러한 컨트롤을 추가 하 고 디자이너에서 다른 WPF 또는 Windows Forms 컨트롤과 마찬가지로 사용할 수 있습니다.

> [!NOTE]
> 래핑된 컨트롤은 c + + Win32 데스크톱 응용 프로그램에 사용할 수 없습니다. 이러한 유형의 응용 프로그램을 사용 해야 합니다 [호스팅 API는 UWP XAML](#uwp-xaml-hosting-api)합니다.

다음 래핑된 UWP 컨트롤은 WPF 및 Windows Forms 응용 프로그램에서 현재 사용할 수 있습니다. Windows 커뮤니티 도구 키트의 향후 릴리스에 대 한 자세한 래핑된 UWP 컨트롤 계획 되어 있습니다.

| 컨트롤 | 지원 되는 최소 OS | 설명 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, 버전 1803 | 웹 콘텐츠를 표시 하는 Microsoft Edge 렌더링 엔진을 사용 합니다. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 버전을 제공 **WebView** 자세한 OS 버전과 호환 되는 합니다. 이 컨트롤은 Windows 10 버전 1803 이상에서 웹 콘텐츠를 표시 하는 Microsoft Edge 렌더링 엔진 및 이전 버전의 Windows 10, Windows에서 웹 콘텐츠를 표시 하는 Internet Explorer 렌더링 엔진 8.x, 및 Windows 7입니다. |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 버전 1809 (17763 빌드) | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 Windows 잉크 기반 사용자 상호 작용에 대 한 노출 및 관련 도구 모음을 제공 합니다. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 버전 1809 (17763 빌드) | 스트림 하 고 Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 비디오와 같은 미디어 콘텐츠를 렌더링 하는 뷰를 포함 합니다. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 버전 1809 (17763 빌드) | Windows Forms 또는 WPF 데스크톱 응용 프로그램에서 바로 가기 또는 사실적인 지도 표시할 수 있습니다. |

## <a name="host-controls"></a>호스트 컨트롤

적용 가능한 래핑된 컨트롤 이외의 시나리오, WPF 및 Windows Forms 응용 프로그램도 사용할 수는 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 에서 제어를 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)합니다. 이 컨트롤에서 파생 되는 모든 UWP 컨트롤을 호스트할 수 있습니다 [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), 사용자 지정 사용자 컨트롤 뿐 아니라 모든 Windows SDK에서 제공 하는 UWP 컨트롤을 포함 합니다. 이 제어는 Windows 10 Insider Preview SDK 빌드 17709 이상 릴리스를 지원합니다.

> [!NOTE]
> 호스트 컨트롤은 c + + Win32 데스크톱 응용 프로그램에 사용할 수 없습니다. 이러한 유형의 응용 프로그램을 사용 해야 합니다 [호스팅 API는 UWP XAML](#uwp-xaml-hosting-api)합니다.

## <a name="uwp-xaml-hosting-api"></a>UWP XAML 호스팅 API

C + + Win32 응용 프로그램에 있는 경우 사용할 수 있습니다는 *호스팅 API는 UWP XAML* 에서 파생 되는 모든 UWP 컨트롤을 호스팅할 [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 모든 UI 요소에 사용자 응용 프로그램에는 연결 된 창 핸들 (HWND)입니다. 이 API는 Windows 10 Insider Preview SDK 빌드 17709에서에서 도입 되었습니다. 이 API를 사용 하는 방법에 대 한 자세한 내용은 참조 하십시오 [데스크톱 응용 프로그램에서 API를 호스트 하는 XAML을 사용 하 여](using-the-xaml-hosting-api.md)입니다.

> [!NOTE]
> C + + Win32 데스크톱 응용 프로그램 호스트 UWP 컨트롤에 호스팅 API는 UWP XAML을 사용 해야 합니다. 래핑된 컨트롤 및 호스트 컨트롤 이러한 유형의 응용 프로그램에 대해 사용할 수 없는 경우 WPF 및 Windows Forms 응용 프로그램에 대 한 것이 좋습니다 래핑된 컨트롤 및 호스트 컨트롤 UWP XAML 대신 Windows 커뮤니티 도구 키트의 사용 API를 호스트 합니다. 이러한 컨트롤 호스팅 API에 내부적으로 UWP XAML을 사용 하 고 간단한 개발 환경을 제공 합니다. 그러나 선택 하면 WPF 및 Windows Forms 응용 프로그램에서 직접 API를 호스트 하는 UWP XAML을 사용할 수 있습니다.

## <a name="architecture-overview"></a>아키텍처 개요

여기에서 이러한 컨트롤이 아키텍처 측면에서 구성되는 방법을 빠르게 확인할 수 있습니다. 이 다이어그램에 사용되는 이름은 변경될 수 있습니다.  

![호스트 컨트롤 아키텍처](images/host-controls.png)

이 다이어그램 맨 아래에 표시되는 API는 Windows SDK와 함께 제공됩니다. 디자이너에 추가할 컨트롤은 Windows 커뮤니티 도구 키트에 NuGet 패키지로 제공됩니다.

이러한 새 컨트롤은 제한 사항이 있으므로 사용하기 전에 잠시 지원되지 않는 사항과 임시 방편으로만 사용할 수 있는 기능을 살펴보세요.

## <a name="limitations"></a>제한 사항

### <a name="whats-supported"></a>지원되는 사항

대부분의 경우 아래 목록에서 명시적으로 언급되지 않으면 모든 사항이 지원됩니다.

### <a name="whats-supported-only-with-workarounds"></a>임시 방편으로만 지원되는 사항

:heavy_check_mark: 여러 windows 내에서 여러 수신함 컨트롤 호스팅. 자체 스레드에 각 창을 배치해야 합니다.

:heavy_check_mark: 사용 하 여 ``x:Bind`` 호스트 컨트롤입니다. .NET 표준 라이브러리에서 데이터 모델을 선언해야 합니다.

:heavy_check_mark: C#-타사 컨트롤을 기반으로 합니다. 타사 컨트롤에 대한 소스 코드가 있는 경우 이에 대해 컴파일할 수 있습니다.

### <a name="whats-not-yet-supported"></a>아직 지원되지 않는 사항

: no_entry_sign: 응용 프로그램에서 원활 하 게 작동 하 고 컨트롤을 호스트 하는 내게 필요한 옵션 도구입니다.

: no_entry_sign: Windows 앱 패키지를 포함 하지 않는 응용 프로그램에 추가한 컨트롤의 지역화 된 콘텐츠입니다.

: no_entry_sign: Windows 앱 패키지를 포함 하지 않는 응용 프로그램 내에서 XAML에서 만든 자산 참조 합니다.

: no_entry_sign: 컨트롤을 제대로 DPI 배율에 대 한 변경 내용에 응답 합니다.

: no_entry_sign: 추가 된 **WebView** 컨트롤을 사용자 지정 사용자 컨트롤 (스레드에, 스레드가 해제 또는 프로시저에서).

: no_entry_sign: 합니다 [표시 강조](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent 적용 합니다.

: no_entry_sign: 인라인 잉크 @Places, 및 @People 입력된 컨트롤에 대 한 합니다.

: no_entry_sign: 액셀러레이터 키를 할당 합니다.

: no_entry_sign: C + + 기반 타사 컨트롤입니다.

: no_entry_sign: 사용자 지정 사용자 컨트롤을 호스트 합니다.

바탕 화면에 Fluent를 가져오는 환경을 개선하기 위해 이 목록의 항목은 계속해서 변경될 수 있습니다.  
