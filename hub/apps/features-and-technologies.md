---
Description: 이 섹션에서는 다양 한 앱 플랫폼에서 특정 핵심 Windows 기능을 어떻게 지원 하는지 이해 하 고 코드에서 기능을 사용 하는 방법을 이해 하는 데 도움을 줍니다.
title: 기능 및 기술
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 4fb49b5145350360be3b86c73110762cfa30e9db
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339346"
---
# <a name="features-and-technologies-for-windows-apps"></a>Windows 앱에 대 한 기능 및 기술

작성 하는 앱의 유형 또는 대상으로 하는 장치에 관계 없이 Windows는 중요 한 앱 시나리오에 대 한 주요 구성 요소에 해당 하는 다양 한 기능을 지원 합니다. 이러한 기능 중 일부는 유니버설 Windows 플랫폼 (UWP), Win32 (Windows API) 및 기타 앱 플랫폼에 다른 방식으로 노출 됩니다. 다음 문서에서는 다양 한 응용 프로그램 플랫폼에서 특정 Windows 기능을 지 원하는 방법 및 코드에서 기능을 사용 하는 방법을 이해 하는 데 도움이 되는 정보를 제공 합니다.

이 문서에서는 UWP, Win32 (Windows API), WPF 및 Windows Forms 앱 플랫폼에서 중요 한 Windows 기능 및 기술에 액세스 하는 방법에 대 한 자세한 내용을 확인할 수 있는 문서 목록을 제공 합니다. 각 플랫폼의 개발 기능에 대 한 자세한 내용은 다음 리소스를 참조 하세요.

* [UWP 설명서](/windows/uwp/index)
* [Win32 (Windows API) 설명서](/windows/desktop/index)
* [WPF 설명서](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows Forms 설명서](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>주요 Windows 기능 및 기술

다음 섹션에서는 최신 기능을 제공 하 고 고객에 게 매력적인 환경을 제공 하는 데 사용할 수 있는 몇 가지 중요 한 Windows 기능 및 기술을 강조 합니다.

### <a name="windows-ink"></a>Windows Ink

![Surface 펜](images/hero-small.png)  

펜 장치와 더불어 Windows Ink 플랫폼을 사용하면 자연스럽게 디지털 필기 메모, 드로잉, 주석을 만들 수 있습니다. 플랫폼에서는 디지타이저 입력을 잉크 데이터로 캡처, 잉크 데이터 생성, 잉크 데이터 관리, 출력 장치에서 잉크 데이터를 스트로크로 렌더링 및 잉크를 필기 인식을 통해 텍스트로 변환 등을 지원합니다.

Windows 앱에서 Windows Ink를 사용 하는 다양 한 방법에 대 한 자세한 내용은 [Windows ink](/windows/uwp/design/input/pen-and-stylus-interactions)를 참조 하십시오.

### <a name="speech-interactions"></a>음성 조작

![SGRS 문법 파일을 기반으로 한 제약 조건의 초기 인식 화면](images/speech-listening-initial.png)

![SGRS 문법 파일을 기반으로 한 제약 조건의 최종 인식 화면](images/speech-listening-complete.png)

Windows는 음성 인식 및 텍스트 음성 변환 (TTS 또는 음성 합성이 라고도 함)을 앱의 사용자 환경에 직접 통합 하는 여러 가지 방법을 제공 합니다. 음성은 사용자가 앱과 상호 작용 하거나, 키보드, 마우스, 터치 및 제스처를 대체할 수 있는 강력 하 고 즐겁게 보완 수 있습니다.

Windows 앱에서 음성 상호 작용을 사용 하는 다양 한 방법에 대 한 자세한 내용은 [음성 상호 작용](/windows/uwp/design/input/speech-interactions)을 참조 하세요.

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

Windows 응용 프로그램을 개선 하는 데 사용할 수 있는 여러 AI 솔루션을 제공 합니다. Windows Machine Learning를 사용 하 여 숙련 된 기계 학습 모델을 앱에 통합 하 고 장치에서 로컬로 실행할 수 있습니다. Windows 비전 기술을 사용 하면 미리 빌드된 라이브러리를 사용 하 여 일반적인 이미지 처리 작업을 수행 하거나 고유한 사용자 지정 솔루션을 만들 수 있습니다. DirectML은 하드웨어를 최대한 활용 하는 데 사용할 수 있는 낮은 수준의 DirectX 스타일 Api를 제공 합니다.

Windows 앱에서 AI를 통합 하는 다양 한 방법에 대 한 자세한 내용은 [WINDOWS ai](https://docs.microsoft.com/windows/ai/)를 참조 하세요.

## <a name="features-and-technologies-by-platform"></a>플랫폼별 기능 및 기술

다음 섹션에서는 주요 앱 플랫폼의 핵심 Windows 기능 및 기술과 통합 하는 방법에 대해 자세히 알아볼 수 있는 유용한 링크를 제공 합니다. UWP, Win32 (Windows API), WPF 및 Windows Forms입니다.

### <a name="user-interface-and-accessibility"></a>사용자 인터페이스 및 내게 필요한 옵션

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [디자인](/windows/uwp/design/basics/)<br/><br/>[레이아웃](/windows/uwp/design/layout/)<br/><br/>[컨트롤](/windows/uwp/design/controls-and-patterns/)<br/><br/>[입력](/windows/uwp/design/input/)<br/><br/>[타일](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[시각적 계층](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 플랫폼](/windows/uwp/xaml-platform/)<br/><br/>[실행, 다시 시작 및 백그라운드 작업](/windows/uwp/launch-resume/)<br/><br/>[Windows 내게 필요한 옵션](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [데스크톱 사용자 인터페이스](/windows/desktop/windows-application-ui-development)<br/><br/>[데스크톱 환경 및 셸](/windows/desktop/user-interface)<br/><br/>[Windows 컨트롤](/windows/desktop/controls/window-controls)<br/><br/>[데스크톱 앱의 UWP 컨트롤(XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[데스크톱 응용 프로그램의 UWP 시각적 계층](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows 및 메시지](/windows/desktop/winmsg/windowing)<br/><br/>[메뉴 및 기타 리소스](/windows/desktop/menurc/resources)<br/><br/>[높은 DPI](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[액세스 가능성](/windows/desktop/accessibility)<br/><br/>  |  [WPF의 창](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[탐색 개요](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[WPF의 XAML](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[컨트롤](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[시각적 계층 프로그래밍](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[입력](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[액세스 가능성](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Windows Form 만들기](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[컨트롤](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[대화 상자](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[사용자 입력](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows Forms 내게 필요한 옵션](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>오디오, 비디오 및 그래픽

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [오디오 비디오 및 카메라](/windows/uwp/audio-video-camera/)<br/><br/>[미디어 재생](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[시각적 계층](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 플랫폼](/windows/uwp/xaml-platform/) |  [오디오 및 동영상](/windows/desktop/audio-and-video)<br/><br/>[그래픽 및 게임](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI +](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Graphics](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [그래픽 및 그리기](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer 클래스](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>데이터 액세스 및 앱 리소스

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [데이터 액세스](/windows/uwp/data-access/)<br/><br/>[데이터 바인딩](/windows/uwp/data-binding/)<br/><br/>[파일, 폴더 및 라이브러리](/windows/uwp/files/)<br/><br/>[앱 리소스](/windows/uwp/app-resources/) |  [데이터 액세스 및 저장소](/windows/desktop/data-access-and-storage)<br/><br/>[로컬 파일 시스템](/windows/desktop/fileio/file-systems)<br/><br/>[리소스 개요](/windows/desktop/menurc/resources-overviews)</li>  |  [데이터 및 모델링](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[데이터 바인딩](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[.NET 앱의 리소스](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[응용 프로그램 리소스, 콘텐츠 및 데이터 파일](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [데이터 및 모델링](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[데이터 바인딩](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[.NET 앱의 리소스](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[응용 프로그램 설정](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>장치, 문서 및 인쇄

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [디바이스 기능 사용](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[디바이스 열거](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[센서](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[인쇄 및 스캔](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [센서 API](/windows/desktop/sensorsapi/portal)<br/><br/>[인쇄](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP Api](/desktop/upnp/universal-plug-and-play-start-page) |  [인쇄 및 인쇄 시스템 관리](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [인쇄 지원](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>시스템, 네트워크 및 전원

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [디바이스 열거](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[배터리 정보 가져오기](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[스레딩 및 비동기 프로그래밍](/windows/uwp/threading-async/)<br/><br/>[네트워킹 및 웹 서비스](/windows/uwp/networking/) | [시스템 서비스](/windows/desktop/system-services)<br/><br/>[메모리 관리](/windows/desktop/memory/memory-management)<br/><br/>[전원 관리](/windows/desktop/power/power-management-portal)<br/><br/>[프로세스 및 스레드](/windows/desktop/procthread/processes-and-threads)<br/><br/>[네트워킹 및 인터넷](/windows/desktop/networking)<br/><br/>[Windows 시스템 정보](/windows/desktop/sysinfo/windows-system-information) |  [스레딩 모델](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[.NET Framework의 네트워크 프로그래밍](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [시스템 정보](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[전원 관리](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[.NET Framework의 네트워크 프로그래밍](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Windows Forms 네트워킹](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>패키징 및 배포

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [앱 패키징](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[앱 패키지 매니페스트 스키마](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Windows 데스크톱 앱 패키지 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[응용 프로그램 설치 및 서비스](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Windows 데스크톱 앱 패키지 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[.NET Framework 및 응용 프로그램 배포](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[WPF 응용 프로그램 배포](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Windows 데스크톱 앱 패키지 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[.NET Framework 및 응용 프로그램 배포](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Windows Forms에 대 한 ClickOnce 배포](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
