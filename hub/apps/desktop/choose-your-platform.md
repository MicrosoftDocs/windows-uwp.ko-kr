---
description: 새 데스크톱 앱을 만들려는 경우 가장 먼저 결정할 사항은 Win32 및 COM API 또는 .NET을 사용할지 여부를 결정하는 것입니다.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 앱 플랫폼 선택
ms.topic: article
ms.date: 11/04/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: windows win32, 데스크톱 개발
ms.openlocfilehash: 46cf8a8e9a57384b85b3156b87697f898ff08ee8
ms.sourcegitcommit: 37f570c7425a3fa953a0c375c19381bf9cf2b6a2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2020
ms.locfileid: "93191904"
---
# <a name="choose-your-app-platform"></a>앱 플랫폼 선택

Windows PC에 대한 새 데스크톱 애플리케이션을 만들려는 경우 가장 먼저 해야할 것은 사용할 애플리케이션 플랫폼을 결정하는 것입니다. Windows는 각각 서로 다른 강점이 있는 네 가지 주요 애플리케이션 플랫폼을 제공합니다.

* [UWP(유니버설 Windows 플랫폼)](#uwp): 이 플랫폼은 Windows 10을 실행하는 모든 디바이스에 대한 CTS(공용 형식 시스템), API 및 애플리케이션 모델을 제공합니다. UWP 애플리케이션은 네이티브이거나 관리형일 수 있습니다.
* [WPF](#wpf) 및 [Windows Forms](#windows-forms): 이러한 .NET 기반 플랫폼은 관리형 애플리케이션을 위한 공용 형식 시스템, API 및 애플리케이션 모델을 제공합니다.
* [Win32](#win32): Windows 및 하드웨어에 직접 액세스해야 하는 기본 C/C++ Windows 애플리케이션을 위한 원본 플랫폼입니다. 따라서 Win32 API는 가장 높은 수준의 성능과 시스템 하드웨어에 대한 직접 액세스가 필요한 애플리케이션에 적합한 플랫폼입니다.

각 플랫폼에는 클래식 Windows 데스크톱에서 실행되고 해당 환경의 기능을 최대한 활용하는 Word, Excel, Photoshop 등의 데스크톱 앱을 만들 수 있는 완전한 UI 프레임워크 및 UI 컨트롤 세트가 포함되어 있습니다. Windows 10에서 각 플랫폼은 [WinUI(Windows UI) 라이브러리](#windows-ui-library)를 사용하여 사용자 인터페이스를 만들 수 있습니다.

이러한 플랫폼 중 일부는 몇 가지 공통적인 특성을 갖고 있으며 특정 유형의 애플리케이션에 더 적합합니다. 예를 들어 UWP 및 .NET은 Visual Studio와 긴밀하게 통합되었습니다. 이는 특히 개발자 생산성, 정교하고 사용자 지정 가능한 UI, 애플리케이션 보안 분야에서 다양한 이점을 제공합니다. 이러한 프레임워크는 UI를 신속하게 만들기 위한 비주얼 디자이너 및 UI 태그를 지원하므로 LOB(기간 업무) 애플리케이션에 특히 적합합니다.

> [!NOTE]
> 어떤 앱 플랫폼을 선택하든, 여러 Windows 10 기능을 사용하여 앱에서 최신 환경을 제공할 수 있습니다. 예를 들어 WPF, Windows Forms 또는 Win32 API를 사용하여 데스크톱 앱을 빌드하더라도 MSIX 패키지 배포를 계속 사용할 수 있습니다. 데스크톱 앱을 현대화하는 모든 방법에 대한 자세한 내용은 [데스크톱 앱 현대화](modernize/index.md)를 참조하세요.

## <a name="uwp"></a>UWP

UWP는 Windows 10 애플리케이션 및 게임을 위한 최신 플랫폼입니다. XAML 태그를 사용하여 코드(비즈니스 논리)에서 UI(프레젠테이션)를 구분하는 사용자 지정의 폭이 넓은 플랫폼입니다. UWP는 정교한 UI, 스타일 사용자 지정 및 그래픽을 많이 사용하는 시나리오가 필요한 데스크톱 애플리케이션에 적합합니다. UWP는 기본 UX 환경을 위한 [Fluent 디자인 시스템](/windows/uwp/design/fluent-design-system/)에 대한 지원을 기본적으로 제공하며, [WinRT(Windows 런타임) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)에 대한 액세스를 제공합니다. Fluent를 채택함으로써 UWP는 잉크, 터치, 게임 패드, 키보드 및 마우스와 같은 일반적인 입력 방법을 자동으로 지원합니다.

UWP를 사용하여 Windows PC용 데스크톱 애플리케이션을 만들 수 있을 뿐만 아니라, UWP는 Xbox, HoloLens 및 Surface Hub 애플리케이션에 대해 유일하게 지원되는 플랫폼이기도 합니다. UWP는 최신의 첨단 애플리케이션 플랫폼입니다.

UWP에 대한 자세한 내용은 다음 문서를 참조하세요.

* [시작](/windows/uwp/get-started/)
* [프로젝트 템플릿](visual-studio-templates.md#uwp-templates)
* [디자인 및 UI](/windows/uwp/design/)
* [기술 및 기능](/windows/uwp/develop/)
* [API 참조](/uwp/)
* [샘플](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

WPF는 .NET Core 또는 전체 .NET Framework에 액세스할 수 있는 관리형 Windows 애플리케이션에 대해 설정된 플랫폼이며 XAML 태그를 사용하여 UI를 코드와 구분합니다. 이 플랫폼은 정교한 UI, 스타일 사용자 지정 및 그래픽을 많이 사용하는 시나리오가 필요한 데스크톱 애플리케이션용으로 고안되었습니다. WPF 개발 기술은 UWP 개발 기술과 비슷하기 때문에 WPF에서 UWP 앱으로 마이그레이션하는 것은 Windows Forms에서 마이그레이션하는 것보다 더 쉽습니다.

WPF에 대한 자세한 내용은 다음 문서를 참조하세요.

* [시작(WPF)](/dotnet/framework/wpf/getting-started/)
* [프로젝트 템플릿](visual-studio-templates.md#net-templates)
* [첫 번째 앱 만들기(.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [첫 번째 앱 만들기(.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [WPF 앱을 .NET Core로 마이그레이션](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [API 참조(.NET)](/dotnet/api/index)
* [샘플](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows Forms

Windows Forms는 경량 UI 모델과 .NET Core 또는 전체 .NET Framework에 대한 액세스 권한을 가진 관리형 Windows 애플리케이션을 위한 원본 플랫폼입니다. 이를 통해 플랫폼을 처음 접하는 개발자들도 신속하게 애플리케이션 빌드를 시작할 수 있습니다. 이는 시각적 및 비시각적 끌어서 놓기 제어의 다양한 기본 제공 컬렉션이 포함된 폼 기반의 신속한 애플리케이션 개발 플랫폼입니다. Windows Forms는 XAML을 사용하지 않으므로 나중에 UWP로 애플리케이션을 확장하기로 결정하게 되면 UI를 완전히 다시 작성해야 합니다.

Windows Forms에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Windows Forms 시작](/dotnet/framework/winforms/getting-started-with-windows-forms)
* [프로젝트 템플릿](visual-studio-templates.md#net-templates)
* [첫 번째 Windows Forms 앱 만들기](/dotnet/framework/winforms/creating-a-new-windows-form)
* [자습서: 사진 뷰어 만들기](/visualstudio/ide/tutorial-1-create-a-picture-viewer?view=vs-2019)
* [API 참조(.NET)](/dotnet/api/index)
* [Windows Forms 앱 강화](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

C++와 함께 Win32 API를 사용하면 WinRT 및 .NET과 같은 관리형 런타임 환경에서 가능한 것보다 비관리형 코드로 대상 플랫폼을 더 많이 제어함으로써 최고 수준의 성능과 효율성을 달성할 수 있습니다. 그러나 애플리케이션 실행에 대한 이러한 수준의 제어 권한을 행사하는 데에는 올바른 결정을 내리기 위해 더 많은 주의와 관심이 필요하며, 런타임 성능을 위해 개발 생산성을 맞바꾸어야 합니다.

다음에는 고성능 애플리케이션을 빌드하는 데 도움이 되도록 Win32 API 및 C++가 제공하는 기능에 대한 몇 가지 주요 항목이 나와 있습니다.

* 리소스 할당, 개체 수명, 데이터 레이아웃, 정렬, 바이트 압축 등에 대한 엄격한 제어를 포함한 하드웨어 수준의 최적화.
* 내장 함수를 통해 SSE 및 AVX와 같은 성능 지향적인 지침 세트에 대한 액세스.
* 템플릿 사용으로 효율적이고 형식이 안전한 제네릭 프로그래밍.
* 효율적이고 안전한 컨테이너 및 알고리즘.
* DirectX, 특히 Direct3D 및 DirectCompute(UWP도 DirectX interop을 제공함).

자세한 내용은 다음 문서를 참조하세요.

* [시작](/windows/win32/desktop-programming/)
* [프로젝트 템플릿](visual-studio-templates.md#cwin32-templates)
* [첫 번째 Win32 및 C++ 앱 만들기](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [기술 및 기능](/windows/win32/desktop-app-technologies)
* [API 참조](/windows/win32/apiindex/windows-api-list/)
* [샘플](https://github.com/Microsoft/Windows-classic-samples)

## <a name="windows-ui-library"></a>Windows UI Library

Windows 10에서 각각의 주요 데스크톱 플랫폼은 [WinUI(Windows UI) 라이브러리](../winui/index.md)를 사용하여 사용자 인터페이스를 만들 수 있습니다. WinUI는 하위 버전의 Windows 10을 대상으로 하는 UWP 앱을 위한 최신 버전 및 업데이트된 버전의 UWP 컨트롤을 제공하는 도구 키트로 시작되었습니다. WinUI는 대상 범위를 늘려 왔으며, 이제는 UWP, .NET 및 Win32에서 Windows 10 앱에 사용되는 최신 기본 UI(사용자 인터페이스) 플랫폼으로 자리잡았습니다.

데스크톱 앱에서 다음과 같은 방법으로 WinUI를 사용할 수 있습니다.

* UWP 앱은 Windows SDK에서 제공하는 UWP 컨트롤 대신 WinUI 컨트롤을 사용할 수 있습니다.
* [XAML Islands](modernize/xaml-islands.md)를 사용하도록 기존 WPF, Windows Forms 및 C++/Win32 앱을 업데이트하여 앱에서 WinUI 2.x 컨트롤을 호스트할 수 있습니다.
* [WinUi 3.0](../winui/winui3/index.md)부터 [전적으로 WinUI 기반 UI를 사용하는 .NET 및 C++/Win32 앱을 만들 수 있습니다](../winui/winui3/get-started-winui3-for-desktop.md).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>플랫폼 비교: UWP, WPF 및 Windows Forms

다음 표에서는 Windows Forms, WPF 및 UWP의 다양한 특성을 상세하게 비교합니다.

| 기능 또는 시나리오  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **지원되는 버전**      |  Windows 10   |  Windows 7 이상 |  Windows 7 이상  |
| **언어**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI(Managed Extensions for C++), F\#, VB |  C\#, C++/CLI(Managed Extensions for C++), F\#, VB   |
| **UI 런타임** |    네이티브(C++/WinRT 및 C++/CX) 및 관리형(.NET 네이티브)  |  관리형(.NET Framework 및 .NET Core 3) |   관리형(.NET Framework 및 .NET Core 3)   |
| **오픈 소스** | [예(Windows UI 라이브러리만 해당)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [예(.NET Core만 해당)](https://github.com/dotnet/wpf) | [예(.NET Core만 해당)](https://github.com/dotnet/winforms)  |
| **XAML 지원** |   예   |  예  |   아니요   |
| **강점**  |  <ul><li>UI용 XAML 태그</li><li>다양하고 사용자 지정이 가능한 UX</li><li>기존 코드 기반이 .NET Standard 준수 상태임</li><li>높은 DPI 지원</li><li>Windows 디바이스(터치, 펜, 게임 패드, 마우스 및 키보드 포함)의 여러 입력 유형 지원</li><li>Xbox, HoloLens, IoT 또는 Surface Hub 지원</li><li>선택적 조밀(컴팩트) UI</li><li>네이티브 C++ 지원</li><li>최적화된 배터리 사용 시간</li><li>최신 접근성 지원(화면 읽기 등)</li><li>서식 있는 텍스트 데이터 기능(기본 제공되는 맞춤법 검사 등)</li><li>수동 입력 지원</li><li>애플리케이션 컨테이너를 통한 보안 실행(예: 신뢰할 수 없는 콘텐츠에 샌드박스 적용)</li></ul>  |  <ul><li>UI용 XAML 태그</li><li>다양하고 사용자 지정이 가능한 UX</li><li>Microsoft 및 파트너의 대규모 제어 컬렉션</li><li>조밀 UI</li><li>Windows 7에 대한 지원</li><li>입력 유효성 검사를 위한 플랫폼 지원</li></ul> | <ul><li>신속한 애플리케이션 개발</li><li>UI 빌드를 위한 WYSIWYG 편집기</li><li>Microsoft 및 파트너의 대규모 제어 컬렉션</li><li>조밀 UI</li><li>Windows 7에 대한 지원</li><li>키보드 및 마우스 입력</li></ul>          |
| **제한적 지원이 포함된 시나리오** |  <ul><li>다중 창 지원<sup>1</sup></li><li>입력 유효성 검사를 위한 플랫폼 지원<sup>1</sup></li><li>Windows 7은 지원되지 않음</li><li>일부 Windows 런타임 API에는 Windows 10의 특정한 최소 버전이 필요함</li><li>전체 플랫폼 지원 및 셸 통합(예: UWP는 현재 시스템 트레이 통합 또는 모든 디바이스에 대한 전체 액세스를 지원하지 않음)</li><li>디스크의 모든 파일에 직접 액세스</li><li>ADO.NET</li><li>.NET Standard 이외 또는 Windows 앱 인증 키트 호환 API 이외 항목을 사용하는 기존 코드 기반 클래스 라이브러리</li><li>로컬 네트워크 루프백 지원(즉, 앱이 대상 디바이스에 루프백 예외를 생성하지 않고 로컬 호스트와 통신해야 하는 경우)</li><li>집약적인 파일 I/O</li></ul>     |  <ul><li>높은 DPI 지원<sup>2</sup></li><li>터치 입력<sup>2</sup></li></ul>  |  <ul><li>높은 DPI 지원<sup>2</sup></li><li>터치 입력<sup>2</sup></li><li>사용자 지정 가능 UI</li><li>다양한 그래픽 및 사용자 환경(터치 및 애니메이션 등)</li><li>보기 및 데이터 모델의 다양한 추상화</li></ul>    |   |

<sup>1</sup> 해당 기능은 공개적으로 발표되었으며, Windows 10의 향후 릴리스에서 이 시나리오를 다룰 예정입니다.

<sup>2</sup> 플랫폼에는 이 시나리오에 대한 고급 API 지원이 부족하지만, 개발자는 해결책을 통해 이 시나리오를 지원할 수 있습니다.

## <a name="other-app-platforms"></a>다른 앱 플랫폼

### <a name="progressive-web-apps-pwas"></a>PWA(프로그레시브 웹앱)

PWA를 통해 개발자는 웹 사이트 코드를 Windows 10 PC의 애플리케이션처럼 설치하고 실행할 수 있도록 패키징할 수 있습니다. 자세한 내용은 [프로그레시브 웹앱](/microsoft-edge/progressive-web-apps/get-started)을 참조하세요.

### <a name="xamarin"></a>Xamarin

Xamarin을 사용하여 iOS 및 Android에서도 실행할 수 있는 Windows 10용 플랫폼 간 애플리케이션을 빌드합니다. 자세한 내용은 [Xamarin](/xamarin/xamarin-forms/get-started/index)을 참조하세요.

### <a name="uno-platform"></a>Uno Platform

Uno Platform은 Windows UWP 기반 코드(C# 및 XAML)를 iOS, Android, macOS, Linux 및 WebAssembly에서 실행하는 데 사용됩니다. [Windows 10 2004(19041)](/windows/uwp/whats-new/windows-10-build-19041)의 UWP에 대한 전체 API 정의와 UWP API의 일부분에 대한 구현(예: [Windows.UI.Xaml](/uwp/api/windows.ui.xaml.documents?view=winrt-19041))을 제공하여 UWP 애플리케이션을 이러한 플랫폼에서 실행할 수 있도록 합니다. 자세한 내용은 [Uno Platform 문서](https://platform.uno/docs/articles/intro.html)를 참조하세요.
