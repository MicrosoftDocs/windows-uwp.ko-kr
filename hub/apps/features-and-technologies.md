---
Description: 이 섹션에서는 주요 Windows 기능이 여러 다른 앱 플랫폼에서 지원되는 방법을 알아보고 코드에서 이러한 기능을 사용해봅니다.
title: 기능 및 기술
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 6bae7fdef4e9cdf05dfa6f56160f1021033123e6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77705778"
---
# <a name="features-and-technologies-for-windows-apps"></a>Windows 앱의 기능 및 기술

빌드하는 앱의 유형 또는 대상으로 하는 디바이스에 관계 없이 Windows는 중요한 앱 시나리오에 대한 주요 구성 요소에 해당하는 다양한 기능을 지원합니다. 이러한 기능 중 일부는 UWP(유니버설 Windows 플랫폼), Win32(Windows API) 및 기타 앱 플랫폼에 다른 방식으로 노출됩니다. 다음 문서에서는 다양한 앱 플랫폼에서 특정 Windows 기능을 지원하는 방법 및 코드에서 기능을 사용하는 방법을 이해하는 데 유용합니다.

이 문서에는 UWP, Win32(Windows API), WPF 및 Windows Forms 앱 플랫폼에서 중요한 Windows 기능 및 기술에 액세스하는 방법에 대한 자세한 내용을 확인할 수 있는 맞춤형 문서 목록이 나와 있습니다. 각 플랫폼의 개발 기능에 대한 자세한 내용은 다음 리소스를 참조하세요.

* [UWP 설명서](/windows/uwp/index)
* [Win32(Windows API) 설명서](/windows/desktop/index)
* [WPF 설명서](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows Forms 설명서](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>주요 Windows 기능 및 기술

다음 섹션에서는 최신 기능을 제공하고 고객에게 매력적인 환경을 제공하는 데 사용할 수 있는 몇 가지 중요한 Windows 기능 및 기술을 강조합니다.

### <a name="windows-ink"></a>Windows Ink

![Surface 펜](images/hero-small.png)  

펜 디바이스와 더불어 Windows Ink 플랫폼을 사용하면 자연스럽게 디지털 필기 메모, 드로잉, 주석을 만들 수 있습니다. 플랫폼에서는 디지타이저 입력을 잉크 데이터로 캡처, 잉크 데이터 생성, 잉크 데이터 관리, 출력 디바이스에서 잉크 데이터를 스트로크로 렌더링 및 잉크를 필기 인식을 통해 텍스트로 변환 등을 지원합니다.

Windows 앱에서 Windows Ink를 사용하는 다양한 방법에 대한 자세한 내용은 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)를 참조하세요.

### <a name="speech-interactions"></a>음성 조작

![SGRS 문법 파일을 기반으로 한 제약 조건의 초기 인식 화면](images/speech-listening-initial.png)

![SGRS 문법 파일을 기반으로 한 제약 조건의 최종 인식 화면](images/speech-listening-complete.png)

Windows에서 다양한 방식으로 음성 인식과 텍스트 음성 변환(TTS 또는 음성 합성이라고도 함)을 앱의 사용자 환경에 직접 통합할 수 있습니다. 음성을 통해 사람들은 효과적이고 즐겁게 앱을 조작할 수 있으며, 음성으로 키보드, 마우스, 터치 및 제스처를 보완하거나 심지어 대체할 수 있습니다.

Windows 앱에서 음성 상호 작용을 사용하는 다양한 방법에 대한 자세한 내용은 [Windows 10의 음성, 목소리 및 대화](speech.md)를 참조하세요.

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

Windows 애플리케이션을 개선하는 데 사용할 수 있는 여러 AI 솔루션을 제공합니다. Windows Machine Learning을 통해 학습된 기계 학습 모델을 앱에 통합하고 디바이스에서 로컬로 실행할 수 있습니다. Windows Vision Skill을 통해 미리 빌드된 라이브러리를 사용하여 일반적인 이미지 프로세싱 작업을 수행하거나 고유한 사용자 지정 솔루션을 만들 수 있습니다. DirectML은 하드웨어의 이점을 활용할 수 있도록 저수준의 DirectX-style API를 제공합니다.

Windows 앱에서 AI를 통합하는 다양한 방법에 대한 자세한 내용은 [Windows AI](https://docs.microsoft.com/windows/ai/)를 참조하세요.

## <a name="features-and-technologies-by-platform"></a>플랫폼별 기능 및 기술

다음 섹션에는 주요 앱 플랫폼에서 주요 Windows 기능 및 기술을 통합하는 방법에 대한 자세한 내용을 제공하는 유용한 링크가 나와 있습니다. UWP, Win32(Windows API), WPF 및 Windows Forms.

### <a name="user-interface-and-accessibility"></a>사용자 인터페이스 및 접근성

|  UWP  |  Win32(Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [디자인](/windows/uwp/design/basics/)<br/><br/>[레이아웃](/windows/uwp/design/layout/)<br/><br/>[컨트롤](/windows/uwp/design/controls-and-patterns/)<br/><br/>[입력](/windows/uwp/design/input/)<br/><br/>[Tile](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[시각적 계층](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 플랫폼](/windows/uwp/xaml-platform/)<br/><br/>[실행, 다시 시작 및 백그라운드 작업](/windows/uwp/launch-resume/)<br/><br/>[Windows 접근성](/windows/uwp/design/accessibility/accessibility)<br/><br/>[음성 조작](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)<br/><br/> |  [데스크톱 사용자 인터페이스](/windows/desktop/windows-application-ui-development)<br/><br/>[데스크톱 환경 및 셸](/windows/desktop/user-interface)<br/><br/>[Windows 컨트롤](/windows/desktop/controls/window-controls)<br/><br/>[데스크톱 앱의 UWP 컨트롤(XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[데스크톱 앱의 UWP 시각적 계층](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows 및 메시지](/windows/desktop/winmsg/windowing)<br/><br/>[메뉴 및 다른 리소스](/windows/desktop/menurc/resources)<br/><br/>[높은 DPI](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[접근성](/windows/desktop/accessibility)<br/><br/>[Microsoft Speech Platform - SDK(소프트웨어 개발 키트) (버전 11)](https://www.microsoft.com/download/details.aspx?id=27226)<br/><br/>[Microsoft Speech SDK, 버전 5.1](https://www.microsoft.com/download/details.aspx?id=10121)<br/><br/>  |  [WPF의 Windows](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[탐색 개요](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[WPF의 XAML](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[컨트롤](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[시각적 계층 프로그래밍](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[입력](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[접근성](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>[.NET Framework용 System.Speech 프로그래밍 가이드](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/>  | [Windows Form 만들기](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[컨트롤](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[대화 상자](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[사용자 입력](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows Forms 접근성](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/>[.NET Framework용 System.Speech 프로그래밍 가이드](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/> |

### <a name="audio-video-and-graphics"></a>오디오 비디오 및 그래픽

|  UWP  |  Win32(Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [오디오 비디오 및 카메라](/windows/uwp/audio-video-camera/)<br/><br/>[미디어 재생](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[시각적 계층](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 플랫폼](/windows/uwp/xaml-platform/) |  [오디오 및 동영상](/windows/desktop/audio-and-video)<br/><br/>[그래픽 및 게임](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [그래픽](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[멀티미디어](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [그래픽 및 그리기](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer 클래스](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>데이터 액세스 및 앱 리소스

|  UWP  |  Win32(Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [데이터 액세스](/windows/uwp/data-access/)<br/><br/>[데이터 바인딩](/windows/uwp/data-binding/)<br/><br/>[파일, 폴더 및 라이브러리](/windows/uwp/files/)<br/><br/>[앱 리소스](/windows/uwp/app-resources/) |  [데이터 액세스 및 스토리지](/windows/desktop/data-access-and-storage)<br/><br/>[로컬 파일 시스템](/windows/desktop/fileio/file-systems)<br/><br/>[리소스 개요](/windows/desktop/menurc/resources-overviews)</li>  |  [데이터 및 모델링](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[데이터 바인딩](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[.NET 앱의 리소스](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[애플리케이션 리소스, 콘텐츠 및 데이터 파일](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [데이터 및 모델링](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[데이터 바인딩](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[.NET 앱의 리소스](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[애플리케이션 설정](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>디바이스, 문서 및 인쇄

|  UWP  |  Win32(Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [디바이스 기능 사용](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[디바이스 열거](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[센서](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[인쇄 및 스캔](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [센서 API](/windows/desktop/sensorsapi/portal)<br/><br/>[인쇄](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP API](/desktop/upnp/universal-plug-and-play-start-page) |  [인쇄 및 인쇄 시스템 관리](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [인쇄 지원](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>시스템, 네트워크 및 전원

|  UWP  |  Win32(Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [디바이스 열거](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[배터리 정보 가져오기](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[스레딩 및 비동기 프로그래밍](/windows/uwp/threading-async/)<br/><br/>[네트워킹 및 웹 서비스](/windows/uwp/networking/) | [시스템 서비스](/windows/desktop/system-services)<br/><br/>[메모리 관리](/windows/desktop/memory/memory-management)<br/><br/>[전원 관리](/windows/desktop/power/power-management-portal)<br/><br/>[프로세스 및 스레드](/windows/desktop/procthread/processes-and-threads)<br/><br/>[네트워킹 및 인터넷](/windows/desktop/networking)<br/><br/>[Windows 시스템 정보](/windows/desktop/sysinfo/windows-system-information) |  [스레딩 모델](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[.NET Framework의 네트워크 프로그래밍](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [시스템 정보](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[전원 관리](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[.NET Framework의 네트워크 프로그래밍](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Windows Forms의 네트워킹](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="debugging-and-performance"></a>디버깅 및 성능

|  UWP  |  Win32(Windows API) |  WPF 및 Windows Forms  |
|-------|----------------------|-------|-----------------|
| [디버깅, 테스트 및 성능](/windows/uwp/debug-test-perf)<br/><br/>[UWP 앱 배포 및 디버깅](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)<br/><br/>[Windows 앱 인증 키트](/windows/uwp/debug-test-perf/windows-app-certification-kit)<br/><br/>[성능](/windows/uwp/debug-test-perf/performance-and-xaml-ui)| [디버깅 및 오류 처리](https://docs.microsoft.com/windows/win32/debugging-and-error-handling)<br/><br/>[Windows용 디버깅 도구](https://docs.microsoft.com/windows-hardware/drivers/debugger/)<br/><br/>[ETW(Windows용 이벤트 추적)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal)<br/><br/>[.NET TraceProcessing API](/windows/apps/trace-processing/)<br/><br/>[TraceLogging](https://docs.microsoft.com/windows/win32/tracelogging/trace-logging-portal)<br/><br/>[성능 카운터](https://docs.microsoft.com/windows/win32/perfctrs/performance-counters-portal) |  [디버깅, 추적 및 프로파일링](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/)<br/><br/>[애플리케이션 추적 및 계측](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications)<br/><br/>[관리 디버깅 도우미를 사용하여 오류 진단](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants)<br/><br/>[런타임 프로파일링](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/runtime-profiling)<br/><br/>[성능 카운터](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/performance-counters)<br/><br/>[Windows Forms용 ClickOnce 배포](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

### <a name="packaging-and-deployment"></a>패키지 및 배포

|  UWP  |  Win32(Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [앱 패키징](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[앱 패키지 매니페스트 스키마](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Windows 데스크톱 앱 패키지(MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[애플리케이션 설치 및 서비스](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Windows 데스크톱 앱 패키지(MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[.NET Framework 및 애플리케이션 배포](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[WPF 애플리케이션 배포](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Windows 데스크톱 앱 패키지(MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[.NET Framework 및 애플리케이션 배포](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Windows Forms용 ClickOnce 배포](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
