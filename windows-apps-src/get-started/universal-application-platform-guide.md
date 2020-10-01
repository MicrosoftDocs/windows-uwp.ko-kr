---
title: UWP(유니버설 Windows 플랫폼) 앱이란?
description: 이 가이드에서는 Windows 10을 실행하는 다양한 디바이스에서 실행할 수 있는 UWP(유니버설 Windows 플랫폼) 앱에 대해 알아봅니다.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 09/15/2020
ms.topic: article
ms.custom: contperfq1
keywords: windows 10, uwp, 유니버설
ms.localizationpriority: medium
ms.openlocfilehash: 416df29fcb6ac007375ff9cf2a8f22d80a12b73e
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219826"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>UWP(유니버설 Windows 플랫폼) 앱이란?

UWP는 Windows용 클라이언트 애플리케이션을 만드는 여러 가지 방법 중 하나입니다. UWP 앱은 WinRT API를 사용하여 인터넷에 연결된 디바이스에 이상적인 강력한 UI와 고급 비동기 기능을 제공합니다.

UWP 앱 만들기를 시작하는 데 필요한 도구를 다운로드하려면 [설정](get-set-up.md)을 참조하여 [첫 번째 앱을 작성합니다](your-first-app.md).


## <a name="where-does-uwp-fit-in-the-microsoft-development-story"></a>UWP는 Microsoft 개발 스토리의 어디에 적합한가요?

UWP는 Windows 10 디바이스에서 실행되는 앱을 만드는 한 가지 선택 항목으로, 다른 플랫폼과 결합할 수 있습니다. UWP 앱은 Win32 API와 .NET 클래스를 사용할 수 있습니다([UWP 앱의 API 집합](/previous-versions/mt186421(v=vs.85)), [UWP 앱의 Dll](/previous-versions/mt186422(v=vs.85)), [UWP 앱용 .NET](/dotnet/api/index?view=dotnet-uwp-10.0) 참조).

Microsoft 개발 스토리는 지속적으로 발전하고 있으며 [WinUI](/windows/apps/winui/), [MSIX](/windows/msix/) 및 [Project Reunion](https://github.com/microsoft/ProjectReunion)과 같은 이니셔티브와 함께 UWP는 클라이언트 앱을 만들 수 있는 강력한 도구입니다.


## <a name="features-of-a-uwp-app"></a>UWP 앱의 기능

UWP 앱은 다음과 같습니다.

- 보안: UWP 앱은 액세스할 디바이스 리소스와 데이터를 선언합니다. 사용자는 이러한 액세스에 권한을 부여해야 합니다.
- Windows 10을 실행하는 모든 디바이스에서 일반적인 API를 사용할 수 있습니다.
- 디바이스 특정 기능을 사용하고 다른 디바이스 화면 크기, 해상도 및 DPI에 맞춰 UI를 조정할 수 있습니다.
- Windows 10에서 실행되는 모든 디바이스(또는 사용자가 지정하는 항목만)의 Microsoft Store에서 사용할 수 있습니다. Microsoft Store는 앱에서 소득을 올릴 수 있는 다양한 방법을 제공합니다.
- 시스템에 위험이 되거나 "시스템 rot"이 발생할 위험 없이 설치 및 제거를 수행할 수 있습니다.
- 참여: 라이브 타일, 푸시 알림, Windows Timeline과 Cortana의 "중단한 위치부터 다시 시작" 기능과 상호 작용하는 사용자 활동을 활용하여 사용자의 참여를 이끌어냅니다.
- C#, C++, Visual Basic 및 Javascript에서 프로그래밍이 가능합니다. UI에서 WinUI, XAML, HTML 또는 DirectX를 사용합니다.

이러한 내용을 좀 더 자세히 살펴보겠습니다.

### <a name="secure"></a>보안

UWP 앱은 마이크, 위치, 웹캠, USB 디바이스, 파일 및 등에 대한 액세스와 같이 필요한 디바이스의 기능을 매니페스트에 선언합니다. 사용자가 이러한 액세스를 승인하고 인증해야만 앱에 이 기능에 대한 사용 권한이 부여됩니다.

### <a name="a-common-api-surface-across-all-devices"></a>모든 디바이스에서 공통되는 API 화면

Windows 10에는 Windows 10을 실행하는 모든 디바이스에 공통 앱 플랫폼을 제공하는 UWP(유니버설 Windows 플랫폼)가 도입되었습니다. UWP 핵심 API는 모든 Windows 디바이스에서 같습니다. 앱이 핵심 API만 사용하는 경우에는 데스크톱 PC, Xbox 또는 혼합 현실 헤드셋 등, 다양한 대상의 Windows 10 디바이스에서 실행됩니다.

C++ /WinRT 또는 C++ /CX로 작성된 UWP 앱은 UWP의 일부인 Win32 API에 액세스할 수 있습니다. 이러한 Win32 API는 모든 Windows 10 디바이스에서 구현됩니다.

### <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>확장 SDK는 특정 디바이스 유형의 고유한 기능을 노출

유니버설 API를 대상으로 하는 경우, Windows 10을 실행하는 모든 디바이스에서 앱을 실행할 수 있습니다. 그러나 UWP 앱에서 디바이스 고유의 API를 활용하도록 하려는 경우에는 그렇게 할 수도 있습니다.

확장 SDK는 서로 다른 디바이스에 특화된 API를 호출할 수 있도록 해줍니다. 예를 들어, UWP 앱이 IoT 디바이스를 대상으로 하는 경우에는 IoT 디바이스 고유의 기능을 대상으로 삼도록 IoT 확장 SDK를 프로젝트에 추가할 수 있습니다. 확장 SDK를 추가하는 자세한 방법은 [확장 SDK를 사용한 프로그래밍](/uwp/extension-sdks/device-families-overvieww#extension-sdks)의 **확장 SDK** 섹션을 참조하세요.

특정 유형의 디바이스에서만 실행되고 Microsoft Store에서의 배포는 해당 유형의 디바이스로만 제한되도록 앱을 작성할 수 있습니다. 또는 실행 시 API가 있는지 조건부 테스트를 실시하고 이에 따라 동작을 조정할 수 있습니다. 자세한 내용은 [확장 SDK를 사용한 프로그래밍](/uwp/extension-sdks/device-families-overview#writing-code)의 **코드 작성** 섹션을 참조하세요.<br>

다음 동영상은 디바이스 제품군과 적응형 코딩에 대해 간략하게 설명합니다.
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

### <a name="adaptive-controls-and-input"></a>적응형 컨트롤 및 입력

UI 요소는 레이아웃과 배율을 조정하여 앱이 실행 중인 화면의 크기와 DPI에 응답합니다. UWP 앱은 키보드, 마우스, 터치, 펜 및 Xbox One 컨트롤러 같이 여러 유형의 입력에서 원활하게 작동합니다. 특정 화면 크기나 디바이스로 UI를 추가적으로 사용자 지정해야 하는 경우에는 앱이 실행되는 다양한 디바이스 및 폼 팩터에 맞게 UI를 디자인할 수 있는 데 새 레이아웃 패널 및 도구를 사용할 수 있습니다.

![Windows 기반 디바이스](images/1894834-hig-device-primer-01-500.png)

Windows에서는 다음과 같은 기능으로 UI를 여러 디바이스에 맞게 조정하도록 도와줍니다.

- 유니버설 컨트롤 및 레이아웃 패널을 사용하면 디바이스의 화면 해상도에 맞게 UI를 최적화할 수 있습니다. 예를 들어, 단추나 슬라이드 같은 컨트롤은 디바이스의 화면 크기와 DPI 밀도에 자동으로 맞춰집니다. 레이아웃 패널에서는 화면 크기에 따라 콘텐츠 레이아웃을 편리하게 조정할 수 있습니다. 적응형 크기 조정은 디바이스 간의 해상도 및 DPI 차이에 맞게 조정됩니다.
- 일반적인 입력 처리는 터치, 펜, 마우스, 키보드 또는 Microsoft Xbox 컨트롤러와 같은 컨트롤러를 통해 입력을 받을 수 있도록 해줍니다.
- 다양한 화면 해상도에 맞게 조정이 가능하도록 UI를 디자인할 수 있는 유용한 도구입니다.

앱 UI의 일부 측면은 디바이스 간에 자동으로 적응합니다. 그러나 앱의 사용자 환경은 앱이 실행되는 디바이스에 따라 수동으로 적응해야 할 수도 있습니다. 예를 들어 사진 앱은 소형 핸드헬드 디바이스에서 실행될 때 한손으로 사용하기에 적합하도록 UI를 조정할 수 있습니다. 사진 앱을 데스크톱 컴퓨터에서 실행할 때는 추가 화면 공간을 활용하도록 UI를 조정해야 합니다.

### <a name="theres-one-store-for-all-devices"></a>모든 디바이스를 위한 하나의 Store

통합 앱 Store에서는 PC, 태블릿, Xbox, HoloLens, Surface Hub, IoT(사물 인터넷) 같은 Windows 10 디바이스에서 앱을 사용할 수 있습니다. 앱을 Store에 제출하고 모든 유형의 디바이스에서, 또는 선택한 디바이스에서 이를 사용하도록 할 수 있습니다. 한 곳에서 모든 Windows 디바이스용 앱을 제출하고 관리합니다. UWP 기능으로 현대화하고 Microsoft Store에서 판매하고 싶은 C++ 데스크톱 앱이 있나요? 그것도 가능합니다.

사용자에 대한 이해를 높이고 앱을 개선하는 데 중요한 도구인 상세 원격 분석용 [Application Insights](https://azure.microsoft.com/services/application-insights/)에 UWP 앱이 연결됩니다.

UWP 앱은 [MSIX](/windows/msix/)와 함께 패키지되고 Microsoft Store를 통해 또는 다른 방법으로 배포될 수 있습니다. MSIX를 사용하면 배포 방법에 관계없이 앱을 업데이트할 수 있습니다. [코드에서 스토어에 게시되지 않은 앱 패키지 업데이트](/windows/msix/non-store-developer-updates)를 참조하세요.

### <a name="monetize-your-app"></a>앱으로 수익 창출

앱으로 수익을 창출하는 방법을 선택할 수 있습니다. 앱으로 수익을 창출할 수 있는 다양한 방법이 있습니다. 예를 들어 자신에게 가장 적합한 앱을 선택하기만 하면 됩니다.

- 가장 간단한 옵션은 유료 다운로드입니다. 가격만 정하세요.
- 평가판은 사용자가 앱을 구입하기 전에 시험 삼아 사용하여 기존의 "프리미엄(Freemium)" 옵션보다 앱 검색률과 구매전환율을 더욱 쉽게 높일 수 있게 해줍니다.
- 사용자에게 인센티브를 제공하기 위한 판매 가격입니다.
- 앱에서 바로 구매.


### <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>사용자의 재방문율을 높이도록 사용자에게 관련성 높은 실시간 정보를 제공

UWP 앱에서 사용자의 참여도를 높이는 방법은 여러 가지가 있습니다.

- 라이브 타일과 잠금 화면 타일은 앱에서 맥락적으로 연관성이 있고 시의적절한 정보를 한눈에 보여줍니다.
- 실시간 경고를 통해 사용자의 주목을 불러일으키는 푸시 알림입니다.
- 사용자 활동 덕분에 사용자가 디바이스 전반의 걸쳐 앱을 떠나는 지점을 선택할 수 있습니다.
- 알림 센터에는 앱에서 제공된 알림이 정리되어 있습니다.
- 백그라운드 실행 및 트리거는 사용자가 필요할 때만 앱을 실행합니다.
- 사용자가 전 세계와 상호 작용하는 데 도움이 되도록 앱에서 음성 및 Bluetooth LE 디바이스를 사용할 수 있습니다.
- 앱에 음성 명령 기능을 추가하려면 Cortana를 연결합니다.

##  <a name="use-a-language-you-already-know"></a>이미 알고 있는 언어 사용

UWP 앱은 운영 체제에서 제공되는 네이티브 API인 Windows 런타임을 사용합니다. 이 API는 C++에서 구현되고 C#, Visual Basic, C++ 및 JavaScript에서 지원됩니다. UWP 앱을 작성하기 위한 몇 가지 옵션은 다음과 같습니다.

- XAML UI 및 C#, VB 또는 C++
- DirectX UI 및 C++
- JavaScript 및 HTML
- WinUI
-

## <a name="links-to-help-you-get-going"></a>실행에 도움이 되는 링크

### <a name="get-set-up"></a>설정

[시작하기](get-set-up.md)를 확인하여 앱 생성을 시작하는 데 필요한 도구를 다운로드한 다음, [첫 번째 앱을 작성합니다](your-first-app.md).

### <a name="design-your-app"></a>앱 디자인

Microsoft의 디자인 시스템을 Fluent라고 합니다. Fluent Design System은 혁신적인 UWP 기능이 결집된 구현체로서 모든 유형의 Windows 기반 디바이스에서 높은 성능을 발휘할 수 있는 앱을 개발하기 위한 모범 사례가 여기에 통합되어 있습니다. Fluent 환경은 태블릿부터 노트북, PC, TV 및 가상 현실 디바이스에 이르기까지 모든 디바이스에 적응하여 자연스러운 경험을 제공합니다. Fluent Design에 대한 내용은 [UWP 앱을 위한 Fluent Design System](/windows/uwp/design/fluent-design-system)을 참조하세요.

좋은 [디자인](http://design.windows.com/)이란 모양과 기능 외에도 사용자가 앱과 상호 작용하는 방식을 고려한 것입니다. 사용자 환경은 앱 사용의 즐거움을 결정하는 데 중요한 역할을 하므로 이 단계를 간과해서는 안 됩니다. [디자인 기본 사항](https://developer.microsoft.com/windows/apps/design)에서는 유니버설 Windows 앱 디자인을 소개합니다. 사용자를 즐겁게 해주는 UWP 앱을 디자인하는 방법은 [디자이너용 UWP(유니버설 Windows 플랫폼) 앱 소개](../design/basics/design-and-ui-intro.md)를 참조하세요. 코딩을 시작하기 전에 [디바이스 입문](../design/devices/index.md)을 참조하면 대상으로 지정하려는 다양한 폼 팩터에서 앱을 사용하는 조작 환경을 고려하는 데 도움이 됩니다.

다양한 디바이스에서의 조작 외에도 여러 디바이스의 이점을 수용하도록 [앱을 계획](./plan-your-app.md)해야 합니다. 예:

- [UWP 앱의 탐색 디자인 기본 사항](../design/basics/navigation-basics.md)에 따라 모바일, 작은 화면 및 큰 화면 디바이스를 수용하도록 워크플로를 디자인합니다. 다양한 화면 크기와 해상도에 맞게 [사용자 인터페이스를 레이아웃](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)합니다.

- 여러 입력 형식을 수용할 방법을 고려합니다. [Cortana](/cortana/skills/), [음성](../design/input/speech-interactions.md), [터치 조작](../design/input/touch-interactions.md), [터치 키보드](../design/input/keyboard-interactions.md) 등을 사용하여 앱을 조작하는 방법은 [조작에 대한 지침](../design/layout/index.md)을 참조하세요.  기존 조작 환경은 [텍스트 및 텍스트 입력에 대한 지침](../design/controls-and-patterns/text-controls.md)을 참조하세요.

### <a name="add-services"></a>서비스 추가

- 디바이스 간에 동기화하려면 [클라우드 서비스](https://azure.microsoft.com/documentation/services/cloud-services)를 사용합니다.
- 앱 환경에서 지원하기 위해 [웹 서비스에 연결](/previous-versions/windows/apps/hh761504(v=win.10))하는 방법을 알아보세요.
- [푸시 알림](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md) 및 [앱에서 바로 구매](../monetize/enable-in-app-product-purchases.md)를 계획에 포함합니다. 이러한 기능은 디바이스 간에 작동해야 합니다.

### <a name="submit-your-app-to-the-store"></a>스토어에 앱 제출

[파트너 센터](https://partner.microsoft.com/dashboard)에서는 모든 Windows 디바이스용 앱을 한곳에서 관리하고 제출할 수 있습니다. Microsoft Store에서 게시할 앱을 제출하는 방법을 알아보려면 [Windows 앱 및 게임 게시](../publish/index.md)를 참조하세요.

새 기능 덕분에 프로세스는 간소화되고 더 세부적으로 제어할 수 있습니다. 자세한 [분석 보고서](../publish/analytics.md), 결합된 [지급 세부 정보](../publish/payout-summary.md), [앱을 홍보하고 고객 참여를 유도](../publish/attract-customers-and-promote-your-apps.md)하는 방법 등을 알아볼 수 있습니다.

자세한 소개 자료를 보려면 [Windows 10 디바이스용 Windows 앱 빌드 소개](/archive/msdn-magazine/2015/may/windows-10-an-introduction-to-building-windows-apps-for-windows-10-devices)를 참조하세요.

### <a name="more-advanced-topics"></a>고급 항목

- 앱의 사용자 활동이 Windows 타임라인이나 Cortana의 "중단한 위치부터 다시 시작" 기능에 표시되도록 [사용자 활동](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97)을 사용하는 방법을 알아보세요.
- [UWP 앱을 위한 타일, 배지 및 알림](../design/shell/tiles-and-notifications/index.md)을 사용하는 방법을 알아보세요.
- UWP 앱에 사용할 수 있는 Win32 API의 전체 목록은 [UWP 앱의 API 집합](/previous-versions/mt186421(v=vs.85)) 및 [UWP 앱의 Dll](/previous-versions/mt186422(v=vs.85))을 참조하세요.
- .NET UWP 앱에 대한 개요는 [.NET의 유니버설 Windows 앱](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/)을 참조하세요.
- UWP 앱에서 사용할 수 있는 .NET 종류의 목록을 보려면 [UWP 앱용 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)을 참조하세요.
- [.NET 네이티브로 앱 컴파일](/dotnet/framework/net-native/)
- Windows 10 사용자를 위한 최신 환경을 기존 데스크톱 앱에 추가하고 [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하여 이를 Microsoft Store에 배포하는 방법을 알아보세요.

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>유니버설 Windows 플랫폼과 Windows 런타임 API를 연결하는 방법
UWP(유니버설 Windows 플랫폼) 앱을 빌드하는 경우 용어 “UWP(유니버설 Windows 플랫폼)”와 "WinRT(Windows 런타임)"를 어느 정도는 동의어로 처리할 수 있으므로 많은 이점과 편리함을 얻을 수 있습니다. 하지만 기술적 측면을 바라보면 이러한 개념 간에 차이점을 *알 수 있게 됩니다*. 이 내용에 대해 궁금한 점이 있으면 마지막 섹션이 도움이 될 것입니다.

Windows 런타임 및 WinRT API는 Windows API가 진화한 결과입니다. 원래 Windows는 일반적인 C 스타일 Win32 API를 통해 프로그래밍되었습니다. 여기에 COM API(대표적인 예로 [DirectX](/windows/desktop/directx)가 있음)가 추가된 것입니다. Windows Forms, WPF, .NET 및 관리형 언어는 Windows 앱을 작성하는 고유한 방법과 고유한 API 기술 버전을 탄생시켰습니다. Windows 런타임은 내부적으로 볼 때 COM의 다음 단계에 해당합니다. 실제 ABI(애플리케이션 이진 인터페이스) 계층에서는 COM의 해당 루트가 표시됩니다. 하지만 Windows 런타임은 유용한 여러 다른 프로그래밍 언어에서 호출할 수 있도록 디자인되었습니다. 또한 해당하는 각 언어에 매우 자연스러운 방식으로 호출할 수 있습니다. 결과적으로, Windows 런타임에는 언어 프로젝션으로 알려져 있는 방식을 통해 액세스할 수 있습니다. C#, Visual Basic, 표준 C++, JavaScript 등으로의 Windows 런타임 언어 프로젝션이 있습니다. 또한 적절하게 패키징할 경우([데스크톱 브리지](/windows/msix/desktop/source-code-overview) 참조), 다양한 애플리케이션 모델 중 하나에서 빌드한 앱에서 WinRT API를 호출할 수 있습니다. Win32, .NET, WinForms 및 WPF

물론 UWP 앱에서도 WinRT API를 호출할 수 있습니다. UWP는 Windows 런타임을 기준으로 빌드된 애플리케이션 모델입니다. 기술적으로 UWP 애플리케이션 모델은 세부적인 내용은 공개되지 않을 수 있지만 사용자가 선택한 프로그래밍 언어에 따라 [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication)을 기준으로 빌드됩니다. 이 항목에서 설명하는 것처럼 가치 제안의 관점에서 볼 때, UWP는 사용자가 선택할 경우 Microsoft Store에 게시되고 다양한 디바이스 폼 팩터 중 하나에서 실행될 수 있는 단일 바이너리를 작성하게 됩니다. UWP 앱의 디바이스 연결은 앱을 호출 기능으로 제한하거나 조건부로 호출하는 Windows 런타임 API 하위 세트에 따라 달라집니다.

다행히 이 섹션에서는 Windows 런타임 API를 기준으로 하는 기술과 유니버설 Windows 플랫폼의 메커니즘 및 비즈니스 가치 간 차이점을 잘 설명하고 있습니다.