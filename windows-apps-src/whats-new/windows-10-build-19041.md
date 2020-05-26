---
title: Windows 10 빌드 19041의 새로운 기능
description: Windows 10 빌드 18362 및 새로운 개발자 도구는 Windows 10이 지원되는 도구, 기능 및 환경을 제공합니다.
keywords: 새로운 기능, 새 기능, Windows, Windows 10, 업데이트, 업데이트, 기능, 신규, 최신, 개발자, 19041, 5월
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bb7630afd6cc69497494a2e86e6c5e3544acefec
ms.sourcegitcommit: f806d5f3b0c1e046c903d3388092c0e059d21858
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790985"
---
# <a name="whats-new-for-developers-in-windows-10-build-19041"></a>Windows 10 빌드 19041의 개발자용 새로운 기능

Windows 10 빌드 19041(SDK 버전 2004라고도 함)은 Visual Studio 2019와 관련 도구 및 기능과 함께 뛰어난 Windows 앱을 만드는 데 필요한 모든 것을 제공합니다. Windows 10에 [도구 및 SDK를 설치](https://developer.microsoft.com/windows/downloads#_blank)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 살펴볼 수 있습니다.

다음은 이 릴리스에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. Windows SDK에 추가된 새 네임스페이스의 전체 목록은 [Windows 10 빌드 19041 API 변경 사항](windows-10-build-19041-api-diff.md)을 참조하세요. Windows 10의 주요 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](https://developer.microsoft.com/windows/windows-10-for-developers)을 참조하세요.

## <a name="windows-10-apps"></a>Windows 10 앱

기능 | 설명
:------ | :------
Bluetooth 오디오 재생 | [원격 Bluetooth 연결 디바이스에서 오디오 재생 사용](../audio-video-camera/enable-remote-audio-playback.md)에서는 [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection)을 사용하여 원격 Bluetooth 연결 디바이스가 로컬 컴퓨터에서 오디오를 재생할 수 있도록 하는 방법을 보여 줍니다. 이를 통해 Bluetooth 스피커처럼 작동하도록 PC를 구성하고 사용자가 휴대폰에서 오디오를 들을 수 있습니다.
C# 앱 포팅 | C# 애플리케이션을 C++/WinRT로 포팅하는 프로세스를 문서화했습니다. [C#에서 C++/WinRT로 클립보드 샘플 포팅](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp)은 상황에 따라 다를 수 있으며, 특정 실제 포팅 환경을 기반으로 합니다. 해당 부록 항목 [C#에서 C++/WinRT로 이동](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp)에는 포팅에 관련된 기술 세부 정보 및 단계가 백과사전식으로 설명되어 있습니다. 
C++/WinRT | [최신 개선 사항/추가 롤업](/windows/uwp/cpp-and-winrt-apis/news#rollup-of-recent-improvementsadditions-as-of-march-2020)에서 빌드 시간 및 런타임 성능 향상(Visual C++ 컴파일러 팀과 협력하여 실현)과 관련된 C++/WinRT 업데이트를 참조하세요. </br> C++/WinRT의 경우 다음 항목에 대한 정보를 추가했습니다. [C++/CX에서 포팅](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#boxing-and-unboxing), [C#에서 포팅](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp#boxing-and-unboxing), [간단한 C++/WinRT Windows UI 라이브러리 예제](/windows/uwp/cpp-and-winrt-apis/simple-winui-example), [동시성](/windows/uwp/cpp-and-winrt-apis/concurrency), [get_unknown()](/uwp/cpp-ref-for-winrt/get-unknown), [C++/WinRT에서 XAML 사용자 지정(템플릿 적용) 제어](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl).
DirectX | Windows의 여러 이전 버전(Creators Update에서 Windows 10 버전 1903까지)에 대한 DirectX 관련 "새로운 기능" 항목을 최신 상태로 업데이트했습니다. [DirectWrite의 새로운 기능](/windows/win32/directwrite/what-s-new-in-directwrite-for-windows-8-consumer-preview), [DXGI 1.6 향상된 기능](/windows/win32/direct3ddxgi/dxgi-1-6-improvements) 및 [Direct3D 12의 새로운 기능](/windows/win32/direct3d12/new-releases).
DirectXMath | 두 개의 행렬 구조와 해당 멤버 함수 및 free 함수를 포함하는 21개의 새 DirectXMath 항목을 게시했습니다. 한 예로 [XMFLOAT3X4 구조](/windows/win32/api/directxmath/ns-directxmath-xmfloat3x4)가 있습니다.
Direct3D | [High Dynamic Range 디스플레이 및 고급 색으로 DirectX 사용](/windows/win32/direct3darticles/high-dynamic-range)에서 Windows high-dynamic-rnge 앱에 대한 모범 사례 목록을 제공합니다. </br> 새 [ID3D11On12Device2](/windows/win32/api/d3d11on12/nn-d3d11on12-id3d11on12device2) 인터페이스 및 해당 메서드를 사용하면 Direct3D 11 API를 통해 만든 리소스를 가져와 Direct3D 12에서 사용할 수 있습니다.
Direct3D 12 | *계산 전용* 디바이스에서 사용할 수 있도록 [Direct3D 12 Core 1.0 기능 수준](/windows/win32/direct3d12/core-feature-levels)이 추가되었습니다. </br> [ID3D12Debug3 인터페이스](/windows/win32/api/d3d12sdklayers/nn-d3d12sdklayers-id3d12debug3)에 대한 새 항목이 추가되었습니다.
직접 ML | WinML이 빌드되어 있는 하위 수준 하드웨어 가속 API인 DirectML에 18개의 연산자가 추가되었습니다. 한 예로 [DML_ACTIVATION_SHRINK_OPERATOR_DESC 구조](/windows/win32/api/directml/ns-directml-dml_activation_shrink_operator_desc)가 있습니다.
오류 보고 | RoFailFastWithErrorContextInternal2 함수가 Win32에 추가되어 추가 오류 컨텍스트가 포함될 수 있는 예외를 발생시킵니다.
Machine Learning | Windows Machine Learning에서 [이제 ONNX 버전 1.4 및 opset 9를 지원](/windows/ai/windows-ml/release-notes)합니다. </br>  [CloseModelOnSessionCreation](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions.closemodelonsessioncreation?view=winrt-19041) API를 사용하면 학습 모델이 더 이상 필요하지 않을 때 자동으로 닫아 메모리를 절약할 수 있습니다.
Wi-Fi | 여러 가지 새로운 기본 WiFi 함수 및 구조가 추가되었습니다(예: [WlanDeviceServiceCommand 함수](/windows/win32/api/wlanapi/nf-wlanapi-wlandeviceservicecommand)).
Wi-Fi 핫스팟 2 | [웹 사이트를 통해 Wi-Fi 프로필 프로비저닝](/windows/win32/nativewifi/prov-wifi-profile-via-website)에서 Wi-Fi 핫스팟 2에 대한 새로운 기능을 설명합니다.
Windows Holographic interop | [`windows.graphics.holographic.interop.h`](/windows/win32/api/windows.graphics.holographic.interop) 헤더가 17개의 Win32 API와 함께 추가되었습니다. API는 Win32와 Windows 런타임 간의 상호 운용을 위한 것입니다. API는 Windows 10 빌드 18362에 추가되었으며, 헤더는 빌드 19041에 새로 추가되었습니다.
Windows 소켓 | Windows 소켓 2 SPI 콘텐츠에 대한 기능이 향상되었습니다. 많은 항목이 향상되고 확대되었으며 그중 하나의 예로 [LPWSPEVENTSELECT 콜백 함수](/windows/win32/api/ws2spi/nc-ws2spi-lpwspeventselect) 항목이 있습니다.
XAML 아일랜드 - 기본 사항 | XAML 아일랜드를 사용하여 데스크톱 Windows 앱에서 UWP XAMl 컨트롤을 호스트합니다. [WPF 앱에서 표준 UWP 컨트롤 호스트](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands) 방법 및 [C++ Win32 앱에서 표준 UWP 컨트롤 호스트](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands-cpp) 방법을 알아봅니다.
XAML 아일랜드 - 사용자 지정 컨트롤 | [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) 및 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 패키지를 사용하여 .NET 및 C++ Win32 앱에서 사용자 지정 UWP XAML 컨트롤을 보다 쉽게 호스트할 수 있습니다. </br> 단계별 연습은 [WPF 앱에서 사용자 지정 UWP 컨트롤 호스트](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands) 및 [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands-cpp)를 참조하세요. </br> 마지막으로 좀 더 복잡한 C++ Win32 시나리오에 대한 지침은 [XAML 아일랜드에 대한 고급 시나리오](/windows/apps/desktop/modernize/advanced-scenarios-xaml-islands-cpp)를 참조하세요.

## <a name="build-with-windows"></a>Windows를 사용하여 빌드

기능 | 설명
:------ | :------
Windows 개발 환경 | [Windows 개발 환경](/windows/dev-environment/) 문서에서는 다양한 플랫폼에서 개발할 수 있도록 Windows 사용 리소스를 제공하여 개발 목표를 달성할 수 있습니다.
Windows의 Python | [Windows의 Python](/windows/python/) 섹션에서는 Python 언어를 처음 접하는 개발자뿐만 아니라 Windows에서 사용 가능한 다른 도구를 사용하여 Python 개발을 최적화하고자 하는 개발자를 위한 정보를 제공합니다. [웹 개발](/windows/python/web-frameworks) 및 [데이터베이스 상호 작용](/windows/python/databases)에 대한 Python 환경을 설정하는 방법에 대해 알아봅니다.
Windows의 NodeJS | [Node.js 개발 환경에 대한 권장 설정](/windows/nodejs/setup-on-wsl2)에서 Linux 서버에 배포하는 고급 개발자를 위한 자세한 지침을 제공합니다. [인기 Node.js 웹 프레임워크](/windows/nodejs/web-frameworks), [데이터베이스 상호 작용](/windows/nodejs/databases) 및 [Docker 컨테이너](/windows/nodejs/containers)에 대한 설치 지침도 사용할 수 있습니다.
Mac-Windows | [개발 환경 변경 가이드](/windows/dev-environment/mac-to-windows)는 Mac에서 Windows로 개발 플랫폼을 전환하는 사용자를 대상으로 하며, 상응하는 바로 가기 및 개발 유틸리티에 대한 매핑을 제공합니다.
Windows 터미널 | 명령 프롬프트, PowerShell 및 WSL(Linux용 Windows 하위 시스템)과 같은 명령줄 도구 및 셸 사용자를 위한 [최신 터미널 애플리케이션](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)입니다. 주요 기능에는 여러 탭, 창, 유니코드 및 UTF-8 문자 지원, GPU 가속 텍스트 렌더링 엔진, 사용자 고유의 테마를 만들고 텍스트, 색, 배경 및 바로 가기 키 바인딩을 사용자 지정하는 기능이 있습니다.
WSL 2 | 이제 [새로운 버전의 WSL(Linux용 Windows 하위 시스템)](/windows/wsl/wsl2-about)을 사용할 수 있습니다. WSL 2 기능은 Windows에서 실제 Linux 커널을 실행하는 아키텍처를 다시 구성하여 파일 시스템 성능 및 전체 시스템 호출 호환성을 향상시킵니다. 이 새 아키텍처는 Linux 이진 파일이 Windows 및 컴퓨터의 하드웨어와 상호 작용하는 방식을 변경하되, WSL 이전 버전과 동일한 사용자 환경을 제공합니다. 각 개별 Linux 배포는 WSL1 또는 WSL2 배포판으로 실행될 수 있으며(병렬 실행 가능), 언제든지 변경할 수 있습니다. </br> 시작하려면 [WSL 2를 설치](https://docs.microsoft.com/windows/wsl/wsl2-install)합니다. </br> [WSL 1과 WSL 2 간의 사용자 환경 변경 내용](https://docs.microsoft.com/windows/wsl/wsl2-ux-changes)에 대한 자세한 내용을 살펴봅니다. </br> [WSL 2에 대한 질문과 대답](https://docs.microsoft.com/windows/wsl/wsl2-faq)을 확인하세요.

## <a name="msix-packaging-and-deployment"></a>MSIX, 패키징 및 배포

기능 | 설명
:------ | :------
MSIX | Windows 10 SDK의 마지막 릴리스 이후 [MSIX 패키지 형식](/windows/msix/overview)에 대한 중요한 업데이트가 있습니다. 
서비스를 사용하여 패키징 | MSIX 및 MSIX 패키징 도구가 [이제 서비스를 포함하는 앱 패키지를 지원](/windows/msix/packaging-tool/convert-an-installer-with-services)합니다.
MSIX 패키지의 스크립트 | [PSF(패키지 지원 프레임워크)를 사용하여 MSIX 앱 패키지의 스크립트를 실행](/windows/msix/psf/run-scripts-with-package-support-framework)할 수 있습니다. 이를 통해 IT 전문가는 애플리케이션이 MSIX를 사용하여 패키징된 후 사용자 환경에 맞게 해당 애플리케이션을 동적으로 사용자 지정할 수 있습니다.
패키지 무결성 적용 | 이제 패키지 매니페스트에서 [uap10:PackageIntegrity 요소](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity)를 사용하여 MSIX 패키지의 콘텐츠에 대한 패키지 무결성을 적용할 수 있습니다. MSIX 패키징 도구를 통해 MSIX 패키지를 만들 때 패키지 무결성을 적용할 수도 있습니다.
스파스 패키지 | *스파스 패키지*를 앱과 함께 빌드하고 등록하여 [MSIX 패키지에 패키징되지 않은 데스크톱 앱에 패키지 ID를 부여](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)할 수 있습니다. 이러한 기능이 제공되므로 아직 배포를 위한 MSIX 패키지를 도입할 수 없는 데스크톱 앱에서도 패키지 ID가 필요한 Windows 10 확장 기능을 사용할 수 있습니다.
호스트되는 앱 | 이제 [호스트되는 앱을 만들 수 있습니다](/windows/uwp/launch-resume/hosted-apps). 호스트되는 앱은 부모 호스트 앱과 동일한 실행 파일 및 정의를 공유하지만 시스템에서 별도의 앱처럼 보이며 동작합니다. 호스트되는 앱은 독립 실행형 Windows 10 앱처럼 동작하는 구성 요소(예: 실행 파일 또는 스크립트 파일)를 원하는 경우에는 유용하지만, 구성 요소를 실행하려면 호스트 프로세스가 필요합니다. 호스트되는 앱에는 자체 시작 타일, ID 및 Windows 10 기능(예: 백그라운드 작업, 알림, 타일 및 공유 대상)과의 긴밀한 통합이 있을 수 있습니다.

## <a name="windows-ui-library-winui"></a>Windows UI 라이브러리(WinUI)

기능 | 설명
:------ | :------
WinUI 2.4 | [WinUI 2.4](/uwp/toolkits/winui/release-notes/winui-2.4)는 Windows UI 라이브러리의 첫 번째 공식 릴리스입니다. 모든 WinUI 버전은 Windows 앱에 대한 광범위한 공식 UI 컨트롤을 제공하며 Windows SDK에 관계없이 NuGet 패키지로 제공되므로 이전 버전의 Windows 10에서 작동합니다. WinUI를 설치하려면 [다음 지침을 따르세요](/uwp/toolkits/winui).
RadialGradientBrush | WinUI 2.4의 새로운 기능인 [RadialGradientBrush](../design/style/brushes.md#radial-gradient-brushes)는 Center, RadiusX 및 RadiusY 속성으로 정의된 타원 안에 그려집니다. 그라데이션의 색은 타원의 중심에서 시작하고 반지름에서 끝납니다.
ProgressRing | WinUI 2.4의 새로운 기능인 [ProgressRing 컨트롤](../design/controls-and-patterns/progress-controls.md)은 ProgressRing이 사라질 때까지 사용자가 차단되는 모달 상호 작용에 사용됩니다. 작업이 완료될 때까지 앱과의 대부분의 상호 작용을 일시 중단해야 하는 경우 이 컨트롤을 사용합니다.
TabView | [TabView 컨트롤](../design/controls-and-patterns/tab-view.md)에 대한 업데이트는 탭을 렌더링하는 방법에 대한 더 많은 제어 기능을 제공합니다. 선택하지 않은 탭의 너비를 설정하고 화면 공간을 절약하기 위해 아이콘만 표시할 수 있으며, 사용자가 탭을 마우스로 가리킬 때까지 선택되지 않은 탭에서 닫기 단추를 숨길 수도 있습니다.
TextBox 컨트롤 | 어두운 테마를 사용하는 경우 이제 TextBox 패밀리 컨트롤의 배경색은 텍스트 삽입 시 기본적으로 짙은 색으로 유지됩니다. 영향을 받는 컨트롤은 [TextBox](/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock), [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox), [Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox) 및 [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)입니다.
NavigationView | [NavigationView 컨트롤](/uwp/api/microsoft.ui.xaml.controls.navigationview)은 이제 계층적 탐색을 지원하고 Left, Top 및 LeftCompact 표시 모드를 포함합니다. 계층적 NavigationView는 페이지 범주를 표시하거나, 관련 하위 페이지가 있는 페이지를 식별하거나, 많은 다른 페이지에 연결된 허브 스타일 페이지가 있는 앱 내에서 사용하는 데 유용합니다.
Windows UI 갤러리 | 각 WinUI 기능의 예는 XAML 컨트롤 갤러리에서 사용할 수 있습니다. [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)에서 다운로드하거나 [Github에서 소스 코드를 확인](https://github.com/Microsoft/Xaml-Controls-Gallery)하세요.
이전 버전 | Windows 10 SDK의 이전 주요 릴리스 이후에 [WinUI 2.3](/uwp/toolkits/winui/release-notes/winui-2.3) 및 [WinUI 2.2](/uwp/toolkits/winui/release-notes/winui-2.2)도 출시되어 Windows 개발자에게 새 UI 기능을 추가로 제공합니다.

## <a name="samples"></a>샘플

다음 샘플 앱이 Windows 10 빌드 19041을 대상으로 업데이트되었습니다.

* [원격 세션(퀴즈 게임)](https://github.com/microsoft/Windows-appsample-remote-system-sessions)
* [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
* [RSS 수집기](https://github.com/Microsoft/Windows-appsample-rssreader)
* [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze)
* [사진 편집기](https://github.com/Microsoft/Windows-appsample-photo-editor)
* [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
* [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [색조 라이트 컨트롤러](https://github.com/Microsoft/Windows-appsample-huelightcontroller)
* [사진 랩](https://github.com/Microsoft/Windows-appsample-photo-lab)
* [가족 메모](https://github.com/Microsoft/Windows-appsample-familynotes)

## <a name="videos"></a>동영상

### <a name="windows-terminal-the-secret-to-command-line-happiness"></a>Windows 터미널: 편리한 명령줄 사용

워크플로에 맞게 Windows 터미널을 사용자 지정하는 방법에 대해 알아보고 해당 기능의 데모를 확인합니다. [동영상을 확인](https://www.youtube.com/watch?v=2dsnwlnNBzs)하고 자세한 내용은 [문서](https://github.com/microsoft/terminal#terminal--console-overview)를 참조하세요.

### <a name="wsl2-code-faster-on-the-windows-subsystem-for-linux"></a>WSL2: Linux용 Windows 하위 시스템에서 더 빠르게 코딩 가능

WSL2, Linux용 Windows 하위 시스템의 새 버전 및 성능 향상을 위해 변경된 사항에 대해 자세히 알아봅니다. [동영상을 확인](https://www.youtube.com/watch?v=MrZolfGm8Zk)하고 자세한 내용은 [문서](/windows/wsl/wsl2-about)를 참조하세요.

### <a name="msix-package-desktop-apps-for-windows-10-replace-outdated-installers"></a>MSIX: Windows 10용 데스크톱 앱 패키징 이전 버전의 설치 관리자를 바꿉니다.

Visual Studio를 사용하여 기존 코드를 패키징하는 방법 및 앱을 배포하는 방법을 비롯하여 Windows 앱을 설치하기 위한 패키지 형식인 MSIX에 대해 알아봅니다. [동영상을 확인](https://www.youtube.com/watch?v=yhOnClQrvBk)하고 자세한 내용은 [문서](/windows/msix/)를 참조하세요.
