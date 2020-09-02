---
Description: 최신 XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, 데스크톱 애플리케이션에 다른 최신 구성 요소를 통합합니다.
title: Windows용 데스크톱 앱 현대화
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: d2ae73cc32fd4e3717fe40b8a6ec8c3397b40619
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161537"
---
# <a name="modernize-your-desktop-apps"></a>데스크톱 앱 현대화

Windows 10 및 UWP(유니버설 Windows 플랫폼)는 데스크톱 앱에서 최신 환경을 제공하는 데 사용할 수 있는 많은 기능을 제공합니다. 이러한 기능 대부분은 다른 플랫폼용 애플리케이션을 다시 작성하지 않고도 자신의 업무 속도에 맞춰 데스크톱 앱에 채택할 수 있는 모듈식 구성 요소로 사용할 수 있습니다. 채택할 Windows 10 및 UWP 부분을 선택하여 기존 데스크톱 앱을 향상시킬 수 있습니다.

이 문서는 현재 데스크톱 앱에서 사용할 수 있는 Windows 10 및 UWP 기능을 설명합니다. 이 문서에 설명된 많은 기능을 사용하도록 기존 앱을 현대화하는 방법을 보여주는 자습서는 [WPF 앱 현대화](modernize-wpf-tutorial.md) 자습서를 참조하세요.

> [!NOTE]
> Windows 10으로 데스크톱 앱을 마이그레이션하기 위한 지원이 필요한가요? [Desktop App Assure](/FastTrack/win-10-desktop-app-assure) 서비스는 앱을 Windows 10으로 이식하려는 개발자에게 직접적인 무료 지원을 제공합니다. 이 프로그램은 모든 ISV 및 적격 엔터프라이즈에서 사용할 수 있습니다. 자격 및 프로그램 자체에 대한 자세한 내용은 [/fasttrack/win-10-app-assure-assistance-offered](/fasttrack/win-10-app-assure-assistance-offered)를 참조하세요. 당장 시작하려면 [요청을 제출](https://fasttrack.microsoft.com/dl/daa)하세요.

## <a name="windows-ui-library"></a>Windows UI Library

Windows UI Library는 Windows 10 앱용 컨트롤 및 기타 사용자 인터페이스 요소를 제공하는 NuGet 패키지 세트입니다. WinUI는 하위 버전의 Windows 10을 대상으로 하는 UWP 앱을 위한 최신 버전 및 업데이트된 버전의 UWP 컨트롤을 제공하는 도구 키트로 시작되었습니다. WinUI는 대상 범위를 늘려 왔으며, 이제는 UWP, .NET 및 Win32에서 Windows 10 앱에 사용되는 최신 기본 UI(사용자 인터페이스) 플랫폼으로 자리잡았습니다.

데스크톱 앱에서 다음과 같은 방법으로 WinUI를 사용할 수 있습니다.

* [XAML Islands](xaml-islands.md)를 사용하도록 기존 WPF, Windows Forms 및 C++/Win32 앱을 업데이트하여 앱에서 WinUI 2.x 컨트롤을 호스트할 수 있습니다.
* [WinUI 3.0 Preview 1](../../winui/winui3/index.md)부터 [전적으로 WinUI 기반 UI를 사용하는 .NET 및 C++/Win32 앱을 만들 수 있습니다](../../winui/winui3/get-started-winui3-for-desktop.md).

[Windows UI(WinUI) 라이브러리](../../winui/index.md)를 참조하세요.

## <a name="msix-packages"></a>MSIX 패키지

MSIX는 UWP, WPF, Windows Forms 및 Win32 앱을 비롯한 모든 Windows 앱에 유니버설 패키징 환경을 제공하는 최신 Windows 앱 패키지 형식입니다. MSIX는 MSI, .appx, App-V 및 ClickOnce 설치 기술의 장점을 결합하여 현대적이고 안정적인 패키징 환경을 제공합니다.

MSIX 패키지에 데스크톱 Windows 앱을 패키지하면 강력한 설치 및 업데이트 환경, 유연한 기능 시스템을 포함하는 관리형 보안 모듈, Microsoft Store, 엔터프라이즈 관리 및 여러 사용자 지정 모델에 대한 지원 기능에 액세스할 수 있습니다.

자세한 내용은 MSIX 설명서의 [데스크톱 애플리케이션 패키지](/windows/msix/desktop/desktop-to-uwp-root)를 참조하세요.

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3은 .NET Core의 최신 주요 릴리스입니다. 이 릴리스의 주요 기능은 Windows Forms 및 WPF 앱을 포함하는 Windows 데스크톱 앱에 대한 지원입니다. .NET Core 3에서 신규 및 기존 Windows 데스크톱 앱을 실행하고 .NET Core에서 제공하는 모든 이점을 누릴 수 있습니다. [XAML Islands](xaml-islands.md)에 호스트되는 UWP 컨트롤을 .NET Core 3를 대상으로 하는 Windows Forms 및 WPF 앱에서도 사용할 수 있습니다.

자세한 내용은 [.NET Core 3.0의 새로운 기능](/dotnet/core/whats-new/dotnet-core-3-0)을 참조하세요.

## <a name="windows-runtime-apis"></a>Windows 런타임 API

WPF, Windows Forms 또는 C++ Win32 데스크톱 앱에서 직접 여러 Windows 런타임 API를 호출하여 Windows 10 사용자에게 유용한 최신 환경을 통합할 수 있습니다. 예를 들어, Windows 런타임 API를 호출하여 데스크톱 앱에 알림 메시지를 추가할 수 있습니다.

자세한 내용은 [데스크톱 앱에서 Windows 런타임 API 사용](desktop-to-uwp-enhance.md)을 참조하세요.

## <a name="host-uwp-controls-xaml-islands"></a>호스트 UWP 컨트롤(XAML Islands)

Windows 10, 버전 1903부터 창 핸들(HWND)에 연결된 WPF, Windows Forms 또는 C++ Win32 앱의 모든 UI 요소에 직접 [UWP XAML 컨트롤](/windows/uwp/design/controls-and-patterns/controls-by-function)을 추가할 수 있습니다. 즉, [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) 및 [Fluent Design System](/windows/uwp/design/fluent-design-system/index)을 지원하는 컨트롤을 비롯한 최신 UWP 기능을 데스크톱 앱의 창 및 기타 디스플레이 화면에 완전히 통합할 수 있습니다. 이 개발자 시나리오를 *XAML Islands*라고도 지칭합니다.

자세한 내용은 [데스크톱 앱의 UWP 컨트롤](xaml-islands.md)을 참조하세요.

## <a name="use-the-visual-layer-in-desktop-apps"></a>데스크톱 앱에서 시각적 계층 사용

이제 비 UWP 데스크톱 앱의 Windows 런타임 API를 사용하여 WPF, Windows Forms 및 C++ Win32 앱의 모양, 느낌 및 기능을 향상시키고, UWP를 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 활용할 수 있습니다. 이러한 기능은 XAML Islands를 사용하여 호스트할 수 있는 기본 제공 UWP 컨트롤을 능가하는 사용자 지정 환경을 만들어야 할 때 유용합니다.

자세한 내용은 [시각적 계층을 사용하여 데스크톱 앱 현대화](visual-layer-in-desktop-apps.md)를 참조하세요.

## <a name="additional-features-available-to-apps-with-package-identity"></a>패키지 ID가 있는 앱에 사용할 수 있는 추가 기능

일부 최신 Windows 10 환경은 [패키지 ID](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)가 있는 데스크톱 앱에서만 사용할 수 있습니다. 이러한 기능에는 특정 Windows 런타임 API, 패키지 확장 및 UWP 구성 요소가 포함됩니다. 자세한 내용은 [패키지 ID가 필요한 기능](modernize-packaged-apps.md)을 참조하세요.

데스크톱 앱에 ID를 부여하는 방법에는 여러 가지가 있습니다.

* [MSIX 패키지](/windows/msix/desktop/desktop-to-uwp-root)에 패키지합니다. MSIX는 모든 Windows 앱, WPF, Windows Forms 및 Win32 앱에 유니버설 패키징 환경을 제공하는 최신 앱 패키지 형식입니다. 강력한 설치 및 업데이트 환경, 유연한 기능 시스템을 갖춘 관리형 보안 모듈, Microsoft Store, 엔터프라이즈 관리 및 여러 사용자 지정 모델에 대한 지원 기능을 제공합니다. 자세한 내용은 MSIX 설명서의 [데스크톱 애플리케이션 패키지](/windows/msix/desktop/desktop-to-uwp-root)를 참조하세요.
* 데스크톱 앱을 배포하기 위해 MSIX 패키지를 채택할 수 없는 경우 Windows 10 버전 2004부터 패키지 매니페스트만 포함된 *스파스 MSIX 패키지*를 만들어 패키지 ID를 부여할 수 있습니다. 자세한 내용은 [패키지되지 않은 데스크톱 앱에 ID 부여](grant-identity-to-nonpackaged-apps.md)를 참조하세요.

<a id="desktop-uwp-controls"></a>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>데스크톱 앱용으로 최적화된 UWP 컨트롤

데스크톱 디바이스 제품군만을 대상으로 하는 UWP 앱을 빌드하거나 WPF, Windows Forms 또는 C++ Win32 데스크톱 앱에서 UWP 컨트롤을 사용하려는 경우 [Fluent Design System](/windows/uwp/design/fluent-design-system/index)을 사용하여 데스크톱 최적화 환경을 제공하도록 디자인된 다음의 새롭거나 업데이트된 UWP 컨트롤을 사용하세요. 이러한 컨트롤은 Windows 10, 버전 1809(2018년 10월 업데이트 또는 버전 10.0.17763)에 도입되었습니다.

| 컨트롤 |  설명 |
|------ |--------------|
| [MenuBar](/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | 더 많은 조직이나 그룹화가 필요할 수 있으며 **CommandBar**에서 허용되는 앱용 명령 세트를 제공하는 빠르고 간단한 방법을 제공합니다. |
| [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | 더 많은 옵션을 포함하는 연결된 플라이아웃이 있는 시각적 표시기 형태의 갈매기형 펼침 단추를 표시합니다.  |
| [SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | 별도로 호출할 수 있는 두 부분으로 이루어진 단추를 제공합니다. 한 부분은 표준 단추처럼 동작하며 즉각적인 작업을 호출합니다. 다른 부분은 사용자가 선택할 수 있는 추가 옵션을 포함하는 플라이아웃을 호출합니다.|
| [ToggleSplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | 별도로 호출할 수 있는 두 부분으로 이루어진 단추를 제공합니다. 한 부분은 끄거나 켤 수 있는 토글 단추처럼 동작합니다. 다른 부분은 사용자가 선택할 수 있는 추가 옵션을 포함하는 플라이아웃을 호출합니다. |
| [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  UI 캔버스에 항목 컨텍스트로 일반적인 사용자 작업을 표시할 수 있습니다. |
| [ComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | 컨트롤에 나열되지 않는 값을 입력할 수 있도록 편집 가능한 콤보 상자를 제공합니다.  |
| [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) | 데이터 바인딩, 항목 템플릿 및 끌어서 놓기를 사용하도록 트리 보기를 구성할 수 있습니다.  |
| [DataGridView](/windows/communitytoolkit/controls/datagrid) |   행과 열에 데이터 컬렉션을 표시할 수 있는 유연한 방법을 제공합니다. 이 컨트롤은 [Windows 커뮤니티 도구 키트](/windows/uwpcommunitytoolkit/)에서 사용할 수 있습니다.  |

## <a name="other-technologies-for-modern-desktop-apps"></a>최신 데스크톱 앱을 위한 기타 기술

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph는 수백만 사용자 데이터와 상호 작용하는 조직 및 소비자를 위한 앱을 빌드하는 데 사용할 수 있는 API 컬렉션입니다. Microsoft Graph는 데이터에 액세스하기 위한 REST API 및 클라이언트 라이브러리를 다음에 제공합니다.
* Azure Active Directory
* Microsoft 365 Office 앱: SharePoint, OneDrive, Outlook/Exchange, Microsoft Teams, OneNote, Planner, Excel
* Enterprise Mobility + Security 서비스: Identity Manager, Intune, Advanced Threat Analytics, Advanced Threat Protection.
* Windows 10 서비스: 작업 및 디바이스

자세한 내용은 [Microsoft Graph 설명서](/graph/overview)를 참조하세요.

### <a name="adaptive-cards"></a>적응형 카드

적응형 카드는 공통적이며 일관된 방식으로 디바이스 및 플랫폼 간에 카드 기반 UI 콘텐츠를 교환하는 데 사용할 수 있는 개방형 플랫폼 간 프레임워크입니다.

자세한 내용은 [적응형 카드 설명서](/adaptive-cards/)를 참조하세요.