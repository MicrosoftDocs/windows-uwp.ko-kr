---
Description: 새 데스크톱 앱을 만들려는 경우 첫 번째 결정은 Win32 및 COM API 또는 .NET을 사용할지 여부입니다.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 앱 플랫폼 선택
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316997"
---
# <a name="choose-your-app-platform"></a>앱 플랫폼 선택

Windows Pc에 대 한 새 데스크톱 응용 프로그램을 만들려는 경우 가장 먼저 결정할 사항은 사용할 응용 프로그램 플랫폼입니다. Windows는 각각 서로 다른 네 가지 주요 응용 프로그램 플랫폼을 제공 합니다.

* [UWP(유니버설 Windows 플랫폼)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [(Win32](#win32)

이러한 모든 응용 프로그램 플랫폼을 사용 하 여 클래식 Windows 데스크톱에서 실행 되 고 해당 환경의 특정 기능을 최대한 활용 하는 Word, Excel, Photoshop 등의 데스크톱 앱을 만들 수 있습니다. 그러나 이러한 플랫폼 중 일부는 일부 특성을 공유 하 고 특정 유형의 응용 프로그램에 더 적합 합니다.

* **UWP, WPF 및 Windows Forms**. 이러한 플랫폼은 특히 개발자 생산성, 정교 하 고 사용자 지정 가능한 UI, 응용 프로그램 보안 분야에서 많은 이점을 제공 하는 관리 되는 런타임 환경 (UWP 용 Windows 런타임 및 Windows Forms 및 WPF 용 .NET)을 제공 합니다. 이러한 프레임 워크는 UI를 신속 하 게 만들기 위한 비주얼 디자이너 및 UI 태그를 지원 하기 때문에 lob (기간 업무) 응용 프로그램에 특히 적합 합니다.

* **Win32 API**. Win32 API (Windows API 라고도 함)는 Windows 및 하드웨어에 직접 액세스 해야 하는C++ 네이티브 C/Windows 응용 프로그램의 원본 플랫폼입니다. .NET 및 WinRT와 같은 관리 되는 런타임 환경에 의존 하지 않고 최고 수준의 개발 환경을 제공 합니다. 이렇게 하면 가장 높은 수준의 성능을 필요로 하는 응용 프로그램에 대 한 플랫폼을 선택 하 고 시스템 하드웨어에 직접 액세스할 수 Win32 API.

이 문서에서는 이러한 플랫폼에 대해 자세히 설명 하 고 응용 프로그램에 가장 적합 한 플랫폼을 결정 하는 데 도움을 줍니다. 

> [!NOTE]
> 선택한 앱 플랫폼에 관계 없이 UWP (유니버설 Windows 플랫폼)의 여러 기능을 사용 하 여 Windows 10의 앱에서 최신 환경을 제공할 수 있습니다. 예를 들어, WPF, Windows Forms 또는 Win32 API를 사용 하 여 데스크톱 앱을 빌드하는 경우에도 UWP에 처음 도입 된 많은 기능 (예: MSIX 패키지 배포 및 UWP XAML 컨트롤)을 계속 사용할 수 있습니다. 자세한 내용은 [데스크톱 앱 현대화](modernize/index.md)을 참조 하세요.

## <a name="uwp"></a>UWP

UWP는 Windows 10 응용 프로그램 및 게임을 위한 최첨단 플랫폼입니다. XAML 태그를 사용 하 여 코드 (비즈니스 논리)에서 UX (프레젠테이션)를 구분 하는, 사용자 지정이 가능한 매우 강력한 플랫폼입니다. UWP는 정교한 UI, 스타일 사용자 지정 및 그래픽을 많이 사용 하는 시나리오를 필요로 하는 데스크톱 응용 프로그램에 적합 합니다. 또한 UWP는 기본 UX 환경을 위한 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/) 에 대 한 기본 제공 지원을 제공 하며 [WinRT (Windows 런타임) api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)에 대 한 액세스를 제공 합니다. 흐름를 채택 하 여 UWP는 잉크, 터치, 게임 패드, 키보드 및 마우스와 같은 일반적인 입력 방법을 자동으로 지원 합니다.

UWP를 사용 하 여 Windows Pc 용 데스크톱 응용 프로그램을 만들 수 있을 뿐만 아니라, UWP도 Xbox, HoloLens 및 Surface Hub 응용 프로그램에 대해 지원 되는 유일한 플랫폼입니다. UWP는 최신의 첨단 응용 프로그램 플랫폼입니다.

UWP에 대 한 자세한 내용은 [Windows 10 앱 시작](/windows/uwp/get-started/)을 참조 하세요.

## <a name="wpf"></a>WPF

WPF는 전체 .NET Framework에 액세스할 수 있는 관리 되는 Windows 응용 프로그램에 대해 설정 된 플랫폼이 며 XAML 태그를 사용 하 여 코드에서 UX를 분리 합니다. 이 플랫폼은 정교한 UI, 스타일 사용자 지정 및 그래픽을 많이 사용 하는 시나리오를 필요로 하는 데스크톱 응용 프로그램용으로 설계 되었습니다. WPF 개발 기술은 UWP 개발 기술과 비슷하며 WPF에서 UWP 앱으로 마이그레이션하는 것은 Windows Forms에서 마이그레이션하는 것 보다 더 쉽습니다.

WPF에 대 한 자세한 내용은 [시작 하기 (wpf)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)를 참조 하세요.

## <a name="windows-forms"></a>Windows Forms

Windows Forms는 경량 UI 모델을 포함 하는 관리 되는 Windows 응용 프로그램을 위한 원본 플랫폼과 전체 .NET Framework에 대 한 액세스입니다. 이를 통해 개발자는 플랫폼에 대 한 새로운 개발자를 위한 응용 프로그램 빌드를 신속 하 게 시작할 수 뛰어나지만. 이는 visual 및 비시각적 끌어서 놓기 컨트롤의 기본 제공 컬렉션을 포함 하는 폼 기반의 빠른 응용 프로그램 개발 플랫폼입니다. Windows Forms는 XAML을 사용 하지 않으므로 나중에 응용 프로그램을 UWP로 확장 하도록 결정 하면 UI를 완전히 다시 작성 합니다.

Windows Forms에 대 한 자세한 내용은 [Windows Forms 시작](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)하기를 참조 하세요.

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>플랫폼 비교: UWP, WPF 및 Windows Forms

다음 표에서는 Windows Forms, WPF 및 UWP의 다양 한 특성에 대해 자세히 설명 합니다.

| 기능 또는 시나리오  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **지원 되는 버전**      |  Windows 10   |  Windows 7 이상 |  Windows 7 이상  |
| **언어들**      |   C\#, C++/winrt, C++/cx, VB, JavaScript   |  C\#, C++/cli (에 대 한 C++관리 되는\#확장), F, VB |  C\#, C++/cli (에 대 한 C++관리 되는\#확장), F, VB   |
| **UI 런타임** |    네이티브 (C++/Winrt 및 C++/cx) 및 관리 (.NET 네이티브)  |  관리 (.NET Framework 및 .NET Core 3) |   관리 (.NET Framework 및 .NET Core 3)   |
| **오픈 소스** | [예 (Windows UI 라이브러리에만 해당)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [예 (.NET Core에만 해당)](https://github.com/dotnet/wpf) | [예 (.NET Core에만 해당)](https://github.com/dotnet/winforms)  |
| **XAML 지원** |   예   |  예  |   아니요   |
| **강도가**  |  <ul><li>UI에 대 한 XAML 태그</li><li>풍부 하 고 사용자 지정 가능한 UX</li><li>기존 코드 베이스는 .NET Standard 호환 됩니다.</li><li>높은 DPI 지원</li><li>Windows 장치 (터치, 펜, 게임 패드, 마우스 및 키보드 포함)에서 여러 입력 유형을 지원 합니다.</li><li>Xbox, HoloLens, IoT 또는 Surface Hub에 대 한 지원</li><li>네이티브 지원C++</li><li>최적화 된 배터리 수명</li><li>최신 접근성 지원 (예: 화면 판독기)</li><li>서식 있는 텍스트 데이터 기능 (예: 기본 제공 맞춤법 검사)</li><li>잉크 지원</li><li>응용 프로그램 컨테이너를 통한 보안 실행 (예: 신뢰할 수 없는 콘텐츠가 샌드 박싱 됨)</li></ul>  |  <ul><li>UI에 대 한 XAML 태그</li><li>풍부 하 고 사용자 지정 가능한 UX</li><li>Microsoft 및 파트너의 많은 컨트롤 컬렉션</li><li>조밀한 UI</li><li>Windows 7에 대 한 지원</li><li>입력 유효성 검사에 대 한 플랫폼 지원</li></ul> | <ul><li>신속한 응용 프로그램 개발</li><li>UI를 빌드하기 위한 WYSIWYG 편집기</li><li>Microsoft 및 파트너의 많은 컨트롤 컬렉션</li><li>조밀한 UI</li><li>Windows 7에 대 한 지원</li><li>키보드 및 마우스 입력</li></ul>          |
| **지원이 제한 된 시나리오** |  <ul><li>조밀한 UI (조밀한 UI를 만들려면 사용자 지정 스타일 필요)<sup>1</sup></li><li>다중 창 지원<sup>1</sup></li><li>입력 유효성 검사에 대 한 플랫폼 지원<sup>1</sup></li><li>Windows 7은 지원 되지 않습니다.</li><li>일부 UWP Api에는 Windows 10의 특정 최소 버전이 필요 합니다.</li><li>전체 플랫폼 지원 및 셸 통합 (예: UWP는 현재 시스템 트레이 통합 또는 모든 장치에 대 한 전체 액세스를 지원 하지 않음)</li><li>디스크의 모든 파일에 대 한 직접 액세스</li><li>ADO.NET</li><li>Non-.NET Standard 또는 비 Windows 앱 인증 키트 규격 Api를 사용 하는 기존 코드 기반 클래스 라이브러리</li><li>로컬 네트워크 루프백 지원 (즉, 앱이 대상 장치에서 루프백 예외를 만들지 않고 localhost와 통신 해야 하는 경우)</li><li>집약적 파일 i/o</li></ul>     |  <ul><li>높은 DPI 지원<sup>2</sup></li><li>터치 입력<sup>2</sup></li></ul>  |  <ul><li>높은 DPI 지원<sup>2</sup></li><li>터치 입력<sup>2</sup></li><li>사용자 지정 가능한 UI</li><li>풍부한 그래픽 및 사용자 환경 (예: 터치 및 애니메이션)</li><li>보기 및 데이터 모델의 다양 한 추상화</li></ul>    |   |

<sup>1</sup> Windows 10의 향후 릴리스에서이 시나리오를 해결 하는 기능을 공개적으로 발표 했습니다.

<sup>2</sup> 플랫폼이이 시나리오에 대 한 최고 수준의 API를 지원 하지 않더라도 개발자는 해결 방법으로이 시나리오를 지원할 수 있습니다.

## <a name="win32"></a>Win32

에서 Win32 API C++ 를 사용 하 여 WinRT, .net 등의 관리 되는 런타임 환경에서 사용할 수 있는 것 보다는 관리 되지 않는 코드로 대상 플랫폼을 더 세밀 하 게 제어 하 여 가장 높은 수준의 성능과 효율성을 달성할 수 있습니다. 그러나 응용 프로그램의 실행에 대 한 이러한 제어 수준을 연습 하는 것은 적절 한 권한이 필요 하며 런타임 성능에 대 한 개발 생산성을 높이는 데 더 많은 주의가 필요 합니다.

다음은 고성능 응용 프로그램을 빌드할 수 있도록 Win32 API C++ 및에서 제공 하는 기능에 대 한 몇 가지 주요 사항입니다.

-   리소스 할당에 대 한 엄격한 제어, 개체 수명, 데이터 레이아웃, 맞춤, 바이트 압축 등을 비롯 한 하드웨어 수준 최적화
-   내장 함수를 통해 SSE 및 AVX와 같은 성능 지향 명령 집합에 액세스 합니다.
-   템플릿을 사용한 효율적이 고 형식이 안전한 일반 프로그래밍.
-   효율적이 고 안전 하며 컨테이너 및 알고리즘.
-   DirectX, 특히 Direct3D 및 DirectCompute에서 (UWP는 DirectX interop도 제공 합니다.)
-   C++해제.

자세한 내용은 Win32 API 및 [데스크톱 앱 기술을](/windows/desktop/desktop-app-technologies) [사용 하는 데스크톱 Windows 앱 시작](/windows/desktop/desktop-programming) 을 참조 하세요.

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 및 C++ 기존 데스크톱 응용 프로그램

에서 C++데스크톱 응용 프로그램을 작성 하는 경우 UI에 대해 WIN32 또는 MFC를 선택 하거나 비 Windows 플랫폼도 지 원하는 타사 응용 프로그램 프레임 워크의 호스트를 선택할 수 있습니다.

-   **(Win32** 이는 창 고 창, 그리기 및 UI 컨트롤과 같은 UI 기능을 포함 하 되이에 제한 되지 않는 Windows 플랫폼의 핸들 기반 C 언어 API입니다. 이는 핸들을 기반으로 하는 하위 수준의 C 언어 API 이므로 최근에 UI를 많이 사용 하는 앱을 만드는 경우에는 자주 사용 되지 않습니다. 그러나 Windows 플랫폼과 상호 작용 하는 데 필요한 기본 Api를 제공 하 고, 간단한 UI 요구 사항이 있거나 Windows UI를 가능한 한 빨리 포함 하려는 앱에 적합 합니다.
-   **MFC (MFC 라이브러리):** 1992부터 Windows 개발자를 제공 하는 venerable 응용 프로그램 프레임 워크 및 UI 라이브러리입니다. 이는 핸들 기반 C++ C 언어 Win32 API에 비해 씬 래퍼입니다. 또한 미리 정의 된 많은 창, 공용 컨트롤 및 기타 windows 개체에 대 한 개체 지향 인터페이스를 제공 합니다. .NET 에코 시스템의 많은 최신 UI 프레임 워크는 MFC를 편리 하 게 초과할 대부분 C++ 의 개발자가 Windows 데스크톱용 응용 프로그램을 만드는 데 적합 한 기본 ui 프레임 워크입니다.
-   **타사 응용 프로그램 프레임 워크:** 는 C++ 다양 한 플랫폼에서 실행 될 수 있고 Windows 또는 .net 런타임과는 관련이 없기 때문에 타사에서 다양 한 사용자 인터페이스를 사용 하 여 플랫폼 간 C++ 응용 프로그램을 쉽게 개발할 수 있도록에 대 한 새로운 응용 프로그램 및 UI 프레임 워크를 개발 했습니다. 이러한 프레임 워크 중 일부는 고유한 모양 & 느낌을 제공 하지만 wxWidgets 또는 Qt와 같은 다른 사용자는 플랫폼의 네이티브 컨트롤 집합을 사용 하거나 에뮬레이트합니다. 이러한 라이브러리를 사용 하 여 Windows에서 실행 되는 응용 프로그램 버전 또는 OSX 또는 Linux와 같은 다른 플랫폼 간에 거의 모든 응용 프로그램의 소스 코드를 공유할 수 있습니다.

## <a name="other-app-platforms"></a>기타 앱 플랫폼

### <a name="progressive-web-apps-pwas"></a>프로그레시브 Web Apps (PWAs)

PWAs는 개발자가 웹 사이트 코드를 패키지 하 여 Windows 10 Pc의 응용 프로그램 처럼 설치 하 고 실행할 수 있게 합니다. 자세한 내용은 [프로그레시브 Web Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)를 참조 하세요.

### <a name="xamarin"></a>Xamarin

Xamarin을 사용 하 여 iOS 및 Android에서 실행할 수 있는 Windows 10 용 플랫폼 간 응용 프로그램을 빌드합니다. 자세한 내용은 [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)을 참조 하세요.
