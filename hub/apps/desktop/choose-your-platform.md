---
Description: 새 데스크톱 앱을 만들려는 경우 첫 번째 결정 Win32 및 COM API 또는.NET을 사용할 것인지 여부입니다.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 앱 플랫폼 선택
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2a4de1a43e60250e7efc2faf70f3c49e8253beb3
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141795"
---
# <a name="choose-your-app-platform"></a>앱 플랫폼 선택

첫 번째 결정 Windows Pc에 대 한 새 데스크톱 응용 프로그램을 만들려는 경우 사용 하는 응용 프로그램 플랫폼입니다. Windows는 서로 다른 기능을 사용 하 여 각 네 가지 기본 응용 프로그램 플랫폼을 제공합니다.

* [UWP(유니버설 Windows 플랫폼)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

이러한 응용 프로그램 플랫폼의 모든 Photoshop, Word 및 Excel 등의 클래식 Windows 데스크톱 및 수행을 완전히 활용 해당 환경의 특정 기능에서에서 실행 되는 데스크톱 앱을 만들 수 있습니다. 그러나 일부 특성을 공유 이러한 플랫폼 일부 및 특정 유형의 응용 프로그램에 더 적합 합니다.

* **UWP, WPF 및 Windows Forms**합니다. 이러한 플랫폼의 개발자 생산성, 정교 하 고 사용자 지정 가능한 UI 및 응용 프로그램 보안 영역에서 특히 다양 한 혜택을 사용 하 여 관리 되는 런타임 환경 (UWP 및.NET에 대 한 Windows Forms 및 WPF에 대 한 Windows 런타임)을 제공합니다. 되기 때문에 이러한 프레임 워크 신속 하 게 UI를 만들기 위한 비주얼 디자이너 및 UI 태그 지원, 기간 업무 응용 프로그램에 특히 적합 합니다.

* **Win32 API**합니다. Win32 API (Windows API 라고도 함)는 네이티브 C에 대 한 원래 플랫폼 /C++ Windows 및 하드웨어에 직접 액세스 해야 하는 Windows 응용 프로그램입니다. .NET 등 WinRT는 관리 되는 런타임 환경에 따라 없이 최고급 개발 환경을 제공합니다. 이 통해 Win32 API는 선택한 가장 높은 수준의 성능 및 시스템 하드웨어에 대 한 직접 액세스 해야 하는 응용 프로그램에 대 한 플랫폼이 됩니다.

이 문서는 자세히 이러한 플랫폼에 설명 하 고 응용 프로그램에 가장 적합 한 결정 하도록 도와줍니다. 

> [!NOTE]
> 선택 하는 앱 플랫폼에 관계 없이 Windows 10에서 앱의 최신 환경을 전달할 유니버설 Windows 플랫폼 (UWP)의 많은 기능을 사용할 수 있습니다. 예를 들어, WPF, Windows Forms 또는 Win32 API를 사용 하 여 데스크톱 앱의 기반을 하는 경우에 MSIX 패키지 배포 및 UWP XAML 컨트롤과 같은 UWP를 사용 하 여 처음 도입 된 많은 기능을 계속 사용할 수 있습니다. 자세한 내용은 [데스크톱 앱을 현대화](modernize/index.md)합니다.

## <a name="uwp"></a>UWP

UWP는 Windows 10 응용 프로그램 및 게임에 대 한 최신 플랫폼입니다. XAML 태그를 사용 하 여 UX (presentation) (비즈니스 논리) 코드에서 분리 하는 고도로 사용자 지정 가능한 플랫폼 이며 UWP는 복잡 한 UI, 스타일 사용자 지정 및 그래픽 집약적인 시나리오를 필요로 하는 데스크톱 응용 프로그램에 적합 합니다. UWP에 대 한 기본 제공 지원에는 [Fluent Design System](/windows/uwp/design/fluent-design-system/) 기본 UX 환경 및에 대 한 액세스를 제공 합니다 [Windows Runtime (WinRT) Api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Fluent을 채택 하 여 UWP 잉크, 터치, 게임 패드, 키보드 및 마우스와 같은 일반적인 입력된 메서드를 자동으로 지원 합니다.

UWP Windows Pc 용 데스크톱 응용 프로그램을 만드는 데 사용할 수 있는 할 뿐 아니라 UWP Xbox, HoloLens, Surface Hub 응용 프로그램에 대 한 지원 되는 유일한 플랫폼 이기도 합니다. UWP는 최신 첨단 응용 프로그램 플랫폼입니다.

UWP에 대 한 자세한 내용은 참조 하세요. [Windows 10 앱 시작](/windows/uwp/get-started/)합니다.

## <a name="wpf"></a>WPF

WPF는 관리 되는 Windows 응용 프로그램 액세스를 사용 하 여 전체.NET Framework를 위한 견고한 플랫폼도를 사용 하 여 XAML 태그 코드에서 UX를 구분 합니다. 이 플랫폼은 정교한 UI, 스타일 사용자 지정 및 그래픽 집약적인 시나리오를 필요로 하는 데스크톱 응용 프로그램에 대 한 설계 되었습니다. WPF 개발 기술을 Windows Forms에서 마이그레이션 보다 더 쉽게 WPF에서 UWP 앱으로 마이그레이션을 이므로 UWP 개발 기술을 비슷합니다.

WPF에 대 한 자세한 내용은 참조 하십시오 [(WPF) 시작](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)합니다.

## <a name="windows-forms"></a>Windows Forms

Windows Forms는 관리 되는 Windows 응용 프로그램 액세스와 간단한 UI 모델을 전체.NET Framework에 대 한 원본 플랫폼입니다. 개발자가 빠르게 개발자 플랫폼에 대해서도 응용 프로그램을 구축할 수 있도록 뛰어나지만 하는 것입니다. 이것은 시각적 및 비시각적 끌어서 놓기 컨트롤의 많은 기본 제공 컬렉션을 사용 하 여 폼 기반 신속한 응용 프로그램 개발 플랫폼입니다. 완료의 다시 쓰기를 UI에서는 uwp 응용 프로그램을 확장 하고자 나중에 Windows Forms XAML을 사용 하지 않으므로 합니다.

Windows Forms에 대 한 자세한 내용은 참조 하세요. [Windows Forms 시작](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)합니다.

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>플랫폼 비교: UWP, WPF 및 Windows Forms

다음 표에서 자세히 Windows Forms, WPF 및 UWP의 다양 한 특성을 비교 합니다.

| 기능 또는 시나리오  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **지원 되는 버전**      |  Windows 10   |  Windows 7 이상 |  Windows 7 이상  |
| **언어들**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (관리 되는 확장에 대 한 C++), F\#, VB |  C\#, C++/CLI (관리 되는 확장에 대 한 C++), F\#, VB   |
| **UI 런타임** |    네이티브 (C++/WinRT 및 C++/CX) 및 관리 (.NET 네이티브)  |  매니지드(.NET Framework)<br/><br/>.NET Core 3에 대 한 지원이 곧 제공 됩니다.  |   매니지드(.NET Framework)<br/><br/>.NET Core 3에 대 한 지원이 곧 제공 됩니다.    |
| **오픈 소스** | [예 (Windows UI 라이브러리에만 해당)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [예 (.NET Core에만 해당)](https://github.com/dotnet/wpf) | [예 (.NET Core에만 해당)](https://github.com/dotnet/winforms)  |
| **XAML 지원** |   예   |  예  |   아니요   |
| **장점**  |  <ul><li>UI에 대 한 XAML 태그</li><li>풍부 하 고 사용자 지정할 수 있는 UX</li><li>기존 코드 베이스에는.NET 표준 준수</li><li>높은 DPI 지원</li><li>터치, 펜, gamepad, 마우스 및 키보드 등 Windows 장치에서 여러 입력된 형식에 대 한 지원</li><li>Xbox, HoloLens, IoT 또는 Surface Hub에 대 한 지원</li><li>네이티브에 대 한 지원C++</li><li>배터리 최적화</li><li>최신 내게 필요한 옵션 지원 (예: 화면 판독기)</li><li>서식 있는 텍스트 데이터 기능 (예: 기본 제공 맞춤법 검사)</li><li>잉크 입력 기능</li><li>응용 프로그램 컨테이너를 통해 실행 보안 (예를 들어, 신뢰할 수 없는 콘텐츠를 샌드박스)</li></ul>  |  <ul><li>UI에 대 한 XAML 태그</li><li>풍부 하 고 사용자 지정할 수 있는 UX</li><li>컨트롤에서 Microsoft 및 파트너의 대규모 컬렉션</li><li>조밀한 UI</li><li>Windows 7에 대 한 지원</li><li>입력된 유효성 검사에 대 한 플랫폼 지원</li></ul> | <ul><li>신속한 응용 프로그램 개발</li><li>UI를 빌드하기 위한 wysiwyg (위지윅) 편집기</li><li>컨트롤에서 Microsoft 및 파트너의 대규모 컬렉션</li><li>조밀한 UI</li><li>Windows 7에 대 한 지원</li><li>키보드 및 마우스 입력</li></ul>          |
| **제한 된 지원을 제공 하는 시나리오** |  <ul><li>조밀한 UI (사용자 지정 스타일을 필요 조밀한 UI 만들기)<sup>1</sup></li><li>여러 창 지원<sup>1</sup></li><li>입력된 유효성 검사에 대 한 플랫폼 지원<sup>1</sup></li><li>Windows 7 지원 되지 않습니다.</li><li>일부 UWP Api에는 Windows 10의 특정 최소 버전 필요</li><li>완전 한 플랫폼 지원 및 셸 통합 (예를 들어 UWP 현재 지원 하지 않습니다 시스템 트레이에서 통합 또는 모든 장치에 대 한 전체 액세스)</li><li>디스크의 모든 파일에 대 한 직접 액세스</li><li>ADO.NET</li><li>비-.NET Standard를 사용 하는 기존 코드 기반 클래스 라이브러리 또는 비-Windows 앱 인증 키트 규격 Api</li><li>로컬 네트워크 루프백 지원 (즉, 대상 장치에서 루프백 예외를 만들지 않고 localhost를 사용 하 여 통신 해야 하는 앱)</li><li>I/O 집약적인 파일</li></ul>     |  <ul><li>높은 DPI 지원<sup>2</sup></li><li>터치식 입력<sup>2</sup></li></ul>  |  <ul><li>높은 DPI 지원<sup>2</sup></li><li>터치식 입력<sup>2</sup></li><li>UI 사용자 지정 가능</li><li>풍부한 그래픽 및 사용자 환경 (예: 터치 및 애니메이션)</li><li>뷰 및 데이터 모델의 다양 한 추상화</li></ul>    |   |

<sup>1</sup> 공개적으로 Windows 10의 이후 릴리스에서이 시나리오를 해결 하는 기능 발표 했습니다.

<sup>2</sup> 플랫폼에는이 시나리오에 대 한 최고 수준의 API 지원 하지 않습니다, 하지만 개발자 대안을 사용 하 여이 시나리오를 지원할 수 있습니다.

## <a name="win32"></a>Win32

Win32 API를 사용 하 여 C++ 같은 WinRT 및.NET 관리 되는 런타임 환경에서 가능한 것 보다 더 많은 제어 비관리 코드를 사용 하 여 대상 플랫폼의 수행 하 여 최고 수준의 성능과 효율성을 얻을 수 있도록 합니다. 그러나 이러한 응용 프로그램의 실행에 대 한 제어 수준을 실행 주의 크고 바로, 주의가 필요 하며 런타임 성능에 대 한 개발 생산성 trades 합니다.

같습니다. Win32 API의 몇 가지를 요약 하 고 C++ 고성능 응용 프로그램을 빌드할 수 있도록 제공 합니다.

-   리소스 할당, 개체 수명, 데이터 레이아웃, 맞춤, 바이트 압축을 통해 엄격한 제어를 포함 하 여 하드웨어 수준 최적화 및 기타 정보
-   SSE 및 avx 명령와 같은 내장 함수를 통해 성능 지향 명령에 대 한 액세스를 설정합니다.
-   템플릿을 사용 하 여 효율적이 고 형식이 안전한 제네릭 프로그래밍 합니다.
-   효율적이 고 안전한 컨테이너 및 알고리즘
-   DirectX, 특정 Direct3D에 DirectCompute (UWP DirectX interop를 제공 하는 참고).
-   C++AMP 합니다.

자세한 내용은 [Win32 API를 사용 하는 데스크톱 Windows 앱을 사용 하 여 시작](/windows/desktop/desktop-programming) 하 고 [데스크톱 응용 프로그램 기술](/windows/desktop/desktop-app-technologies)합니다.

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 및 C++ 기존 데스크톱 응용 프로그램에 대 한

데스크톱 응용 프로그램을 작성 하는 경우 C++, UI 또는 Windows 이외의 플랫폼을 지 원하는 타사 응용 프로그램 프레임 워크의 호스트에 대 한 Win32 또는 MFC를 선택할 수 있습니다.

-   **Win32:** 이 컨트롤의 창, 그리기, UI 등 UI 기능에 제한 되지 않고 포함 하 여 Windows 플랫폼의 핸들 기반, C 언어 API입니다. 핸들에 기반 하는 하위 수준, C 언어 API 이기 때문에 최신, UI를 많이 사용 앱을 만들기 위한 자주 선택 하는 것입니다. 그러나 Windows 플랫폼과 상호 작용 하는 데 필요한 기본 Api를 제공 하 고 있는 간단한 UI 요구 사항 또는 원하는 게임 예를 들어 방해가 최대한 유지 Windows UI는 앱에 적합 한 선택입니다.
-   **MFC (Microsoft Foundation Class Library):** 부터 응용 프로그램 프레임 워크와 1992 Windows 개발자가 제공 하는 UI 라이브러리입니다. 씬 C++ 래퍼는 핸들 기반, C 언어 Win32 API를 통해 다양 한 미리 정의 된 windows, 공용 컨트롤 및 기타 Windows 개체에 대 한 개체 지향 인터페이스를 제공 합니다. 최신 UI 프레임 워크는.NET 에코 시스템에서 많은 편의에서 MFC를 초과할 이지만 여전히 많은 선택한 네이티브 UI 프레임 워크 C++ Windows 데스크톱 응용 프로그램을 작성 하는 개발자.
-   **타사 응용 프로그램 프레임 워크:** 때문에 C++ 다양 한 플랫폼에서 실행할 수 있으며 연결 되지 않은 Windows 또는.NET 런타임 제 3 자가 개발한 새 응용 프로그램 및 UI 프레임 워크에 대 한 C++ 쉬운 풍부한 사용자 인터페이스를 사용 하 여 플랫폼 간 응용 프로그램의 개발을 위해. 이러한 프레임 워크의 일부 제공 wxWidgets Qt 등 다른 사용 하거나 플랫폼의 네이티브 컨트롤 집합을 에뮬레이션 하는 동안 고유한 모양과 느낌을 합니다. 이러한 라이브러리를 사용 하 여 거의 모든 Windows 또는 OSX 또는 Linux 등의 다른 플랫폼에서 실행 되는 응용 프로그램의 버전 간에 응용 프로그램의 소스 코드를 공유할 가능성이 있습니다.

## <a name="other-app-platforms"></a>다른 앱 플랫폼

### <a name="progressive-web-apps-pwas"></a>점진적 웹 앱 (Pwa)

개발자가 패키지를 설치 및 Windows 10 Pc에는 응용 프로그램과 같이 실행할 수 있도록 해당 웹 사이트 코드를 사용 하는 Pwa 있습니다. 자세한 내용은 [점진적 웹 앱](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)합니다.

### <a name="xamarin"></a>Xamarin

IOS에서 실행할 수도 있는 Windows 10 및 Android 용 플랫폼 간 응용 프로그램을 빌드하려면 Xamarin을 사용 합니다. 자세한 내용은 [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)합니다.
