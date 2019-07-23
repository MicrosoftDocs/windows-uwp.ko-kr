---
Description: 최신 XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, 데스크톱 애플리케이션에 다른 최신 구성 요소를 통합합니다.
title: Windows용 데스크톱 앱 현대화
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 47cb292b1f5aeed9473cac0f27074f449dc67032
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414104"
---
# <a name="modernize-your-desktop-apps"></a>데스크톱 앱 현대화

Windows 10 및 UWP(유니버설 Windows 플랫폼)는 데스크톱 앱에서 최신 환경을 제공하는 데 사용할 수 있는 많은 기능을 제공합니다. 이러한 기능 대부분은 다른 플랫폼용 애플리케이션을 다시 작성하지 않고도 자신의 업무 속도에 맞춰 데스크톱 앱에 채택할 수 있는 모듈식 구성 요소로 사용할 수 있습니다. 채택할 Windows 10 및 UWP 부분을 선택하여 기존 데스크톱 앱을 향상시킬 수 있습니다.

이 문서는 현재 데스크톱 앱에서 사용할 수 있는 Windows 10 및 UWP 기능을 설명합니다. 이 문서에 설명된 많은 기능을 사용하도록 기존 앱을 현대화하는 방법을 보여주는 자습서는 [WPF 앱 현대화](modernize-wpf-tutorial.md) 자습서를 참조하세요.

> [!NOTE]
> Windows 10으로 데스크톱 앱을 마이그레이션하기 위한 지원이 필요한가요? [Desktop App Assure](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure) 서비스는 앱을 Windows 10으로 이식하려는 개발자에게 직접적인 무료 지원을 제공합니다. 이 프로그램은 모든 ISV 및 적격 엔터프라이즈에서 사용할 수 있습니다. 자격 및 프로그램 자체에 대한 자세한 내용은 [https://aka.ms/DesktopAppAssure](https://aka.ms/DesktopAppAssure)를 참조하세요. 당장 시작하려면 [요청을 제출](https://aka.ms/DesktopAppAssureRequest)하세요.

## <a name="msix-packages"></a>MSIX 패키지

MSIX는 UWP, WPF, Windows Forms 및 Win32 앱을 비롯한 모든 Windows 앱에 유니버설 패키징 환경을 제공하는 최신 Windows 앱 패키지 형식입니다. MSIX는 MSI, .appx, App-V 및 ClickOnce 설치 기술의 장점을 결합하며 안전하고, 안정적이며, 신뢰할 수 있도록 빌드되었습니다.

MSIX 패키지에 데스크톱 Windows 앱을 패키지하면 강력한 설치 및 업데이트 환경, 유연한 기능 시스템을 포함하는 관리형 보안 모듈, Microsoft Store, 엔터프라이즈 관리 및 여러 사용자 지정 모델에 대한 지원 기능에 액세스할 수 있습니다.

자세한 내용은 MSIX 설명서의 [데스크톱 애플리케이션 패키지](/windows/msix/desktop/desktop-to-uwp-root)를 참조하세요.

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3는 .NET Core의 차기 주 릴리스입니다. 이 예정된 릴리스의 주요 기능은 Windows Forms 및 WPF 앱을 포함하는 Windows 데스크톱 앱에 대한 지원입니다. .NET Core 3에서 새 및 기존 Windows 데스크톱 앱을 실행하고 .NET Core에서 제공하는 모든 이점을 누릴 수 있습니다. [XAML Islands](xaml-islands.md)에 호스트되는 UWP 컨트롤을 .NET Core 3를 대상으로 하는 Windows Forms 및 WPF 앱에서도 사용할 수 있습니다.

자세한 내용은 다음 문서를 참조하세요.

* [.NET Core 3.0 Preview 1 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [.NET Core 3.0 Preview 2 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [.NET Core 3.0 Preview 2 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [.NET Core 3.0 Preview 4 공지](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [.NET Core 3.0의 새로운 기능](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).

## <a name="uwp-apis"></a>UWP API

WPF, Windows Forms 또는 C++ Win32 데스크톱 앱에서 직접 여러 UWP API를 호출하여 Windows 10 사용자에게 유용한 최신 환경을 통합할 수 있습니다. 예를 들어, UWP API를 호출하여 데스크톱 앱에 알림 메시지를 추가할 수 있습니다.

자세한 내용은 [데스크톱 앱에서 UWP API 사용](desktop-to-uwp-enhance.md)을 참조하세요.

## <a name="host-uwp-controls-xaml-islands"></a>호스트 UWP 컨트롤(XAML Islands)

Windows 10, 버전 1903부터 창 핸들(HWND)에 연결된 WPF, Windows Forms 또는 C++ Win32 앱의 모든 UI 요소에 직접 [UWP XAML 컨트롤](/windows/uwp/design/controls-and-patterns/controls-by-function)을 추가할 수 있습니다. 즉, [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) 및 [Fluent Design System](/windows/uwp/design/fluent-design-system/index)을 지원하는 컨트롤을 비롯한 최신 UWP 기능을 데스크톱 앱의 창 및 기타 디스플레이 화면에 완전히 통합할 수 있습니다. 이 개발자 시나리오를 *XAML Islands*라고도 지칭합니다.

자세한 내용은 [데스크톱 앱의 UWP 컨트롤](xaml-islands.md)을 참조하세요.

## <a name="use-the-visual-layer-in-desktop-apps"></a>데스크톱 앱에서 시각적 계층 사용

이제 비 UWP 데스크톱 앱의 UWP API를 사용하여 WPF, Windows Forms 및 C++ Win32 앱의 모양, 느낌 및 기능을 향상시키고, UWP를 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 활용할 수 있습니다. 이러한 기능은 XAML Islands를 사용하여 호스트할 수 있는 기본 제공 UWP 컨트롤을 능가하는 사용자 지정 환경을 만들어야 할 때 유용합니다.

자세한 내용은 [시각적 계층을 사용하여 데스크톱 앱 현대화](visual-layer-in-desktop-apps.md)를 참조하세요.

## <a name="additional-features-available-to-packaged-apps"></a>패키지된 앱에서 사용할 수 있는 추가 기능

일부 최신 Windows 10 환경은 [MSIX 패키지](/windows/msix/desktop/desktop-to-uwp-root)에 패키지된 데스크톱 앱에서만 사용할 수 있습니다. MSIX 패키지에 데스크톱 앱을 패키지하는 경우, 패키지된 앱에서 패키지 ID, 패키지 확장 및 UWP 구성 요소가 필요한 UWP API를 사용할 수 있습니다.

자세한 내용은 [패키지 ID가 필요한 기능](modernize-packaged-apps.md)을 참조하세요.

<a id="desktop-uwp-controls"/>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>데스크톱 앱용으로 최적화된 UWP 컨트롤

데스크톱 디바이스 제품군만을 대상으로 하는 UWP 앱을 빌드하거나 WPF, Windows Forms 또는 C++ Win32 데스크톱 앱에서 UWP 컨트롤을 사용하려는 경우 [Fluent Design System](/windows/uwp/design/fluent-design-system/index)을 사용하여 데스크톱 최적화 환경을 제공하도록 디자인된 다음의 새롭거나 업데이트된 UWP 컨트롤을 사용하세요. 이러한 컨트롤은 Windows 10, 버전 1809(2018년 10월 업데이트 또는 버전 10.0.17763)에 도입되었습니다.

| 컨트롤 |  설명 |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | 더 많은 조직이나 그룹화가 필요할 수 있으며 **CommandBar**에서 허용되는 앱용 명령 세트를 제공하는 빠르고 간단한 방법을 제공합니다. |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | 더 많은 옵션을 포함하는 연결된 플라이아웃이 있는 시각적 표시기 형태의 갈매기형 펼침 단추를 표시합니다.  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | 별도로 호출할 수 있는 두 부분으로 이루어진 단추를 제공합니다. 한 부분은 표준 단추처럼 동작하며 즉각적인 작업을 호출합니다. 다른 부분은 사용자가 선택할 수 있는 추가 옵션을 포함하는 플라이아웃을 호출합니다.|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | 별도로 호출할 수 있는 두 부분으로 이루어진 단추를 제공합니다. 한 부분은 끄거나 켤 수 있는 토글 단추처럼 동작합니다. 다른 부분은 사용자가 선택할 수 있는 추가 옵션을 포함하는 플라이아웃을 호출합니다. |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  UI 캔버스에 항목 컨텍스트로 일반적인 사용자 작업을 표시할 수 있습니다. |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | 컨트롤에 나열되지 않는 값을 입력할 수 있도록 편집 가능한 콤보 상자를 제공합니다.  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | 데이터 바인딩, 항목 템플릿 및 끌어서 놓기를 사용하도록 트리 보기를 구성할 수 있습니다.  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   행과 열에 데이터 컬렉션을 표시할 수 있는 유연한 방법을 제공합니다. 이 컨트롤은 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)에서 사용할 수 있습니다.  |

## <a name="windows-ui-library"></a>Windows UI Library

Windows UI Library는 UWP 앱용 새 컨트롤 및 기타 사용자 인터페이스 요소를 제공하는 NuGet 패키지 세트입니다. Windows UI Library API는 이전 버전의 Windows 10에서 작동하므로 사용자가 최신 버전의 Windows 10을 실행하지 않더라도 앱이 작동할 수 있습니다. 따라서 새 컨트롤을 채택하여 Windows UI Library에서 릴리스하면서 버전 확인 또는 조건부 XAML을 포함할지에 대해 걱정하지 않아도 됩니다.

[Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요.

## <a name="other-technologies-for-modern-desktop-apps"></a>최신 데스크톱 앱을 위한 기타 기술

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph는 수백만 사용자 데이터와 상호 작용하는 조직 및 소비자를 위한 앱을 빌드하는 데 사용할 수 있는 API 컬렉션입니다. Microsoft Graph는 데이터에 액세스하기 위한 REST API 및 클라이언트 라이브러리를 다음에 제공합니다.
* Azure Active Directory
* Office 365 서비스: SharePoint, OneDrive, Outlook/Exchange, Microsoft Teams, OneNote, Planner, Excel
* Enterprise Mobility + Security 서비스: Identity Manager, Intune, Advanced Threat Analytics, Advanced Threat Protection.
* Windows 10 서비스: 작업 및 디바이스

자세한 내용은 [Microsoft Graph 설명서](https://developer.microsoft.com/graph/docs/concepts/overview)를 참조하세요.

### <a name="adaptive-cards"></a>적응형 카드

적응형 카드는 공통적이며 일관된 방식으로 디바이스 및 플랫폼 간에 카드 기반 UI 콘텐츠를 교환하는 데 사용할 수 있는 개방형 플랫폼 간 프레임워크입니다.

자세한 내용은 [적응형 카드 설명서](https://docs.microsoft.com/adaptive-cards/)를 참조하세요.