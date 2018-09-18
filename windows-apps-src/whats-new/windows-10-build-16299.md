---
author: QuinnRadich
title: 개발자, 도구 및 기능을 위한 Windows 10의 새로운 기능
description: Windows10 빌드 16299 및 새로운 개발자 도구는 유니버설 Windows 플랫폼이 지원되는 도구, 기능 및 환경을 제공합니다.
keywords: 새로운 기능, 업데이트, 기능, 신규, Windows 10, 1709, 10월, 최신, 개발자, 16299, Fall Creators
ms.author: quradic
ms.date: 11/02/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: ffe7de94e4a8564b4971fda0b64f6648d9b6088b
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4017187"
---
# <a name="whats-new-in-windows-10-for-developers-build-16299"></a>개발자용 Windows 10 빌드 16299의 새로운 기능

Visual Studio 2017 및 업데이트된 SDK와 함께 Windows 10 빌드 16299(Fall Creators Update 또는 버전 1709라고도 함)는 놀라운 유니버설 Windows 플랫폼 앱을 만드는 도구, 기능 및 환경을 제공합니다. Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

다음은 이 릴리스에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. Windows SDK에 추가된 새 네임스페이스의 전체 목록은 [Windows10 빌드 16299 API 변경 내용](windows-10-build-16299-api-diff.md)을 참조하세요. Windows 10의 주요 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](http://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요. 그리고 이전에 있었던 그리고 앞으로 있을 Windows 플랫폼 기능 추가에 대한 개략적인 내용은 [Windows 개발자 플랫폼 기능](https://developer.microsoft.com/windows/platform/features)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

특징 | 설명
 :------ | :------
조건부 XAML | 이제 [버전 적응 앱](../debug-test-perf/version-adaptive-apps.md)을 만들 수 있는 [조건부 XAML](../debug-test-perf/conditional-xaml.md)을 사용할 수 있습니다. 조건부 XAML을 통해 XAML 태그에서 **ApiInformation.IsApiContractPresent** 메서드를 사용할 수 있습니다. 이를 통해 코드 숨김을 사용하지 않고도 API의 존재 여부를 기반으로 태그에서 속성을 설정하고 개체를 인스턴스화할 수 있습니다.
디자인 도구 키트 | Sketch 및 Adobe XD의 추가로 [UWP 앱에 대한 디자인 도구 키트 및 리소스](../design/downloads/index.md)가 확장되었습니다. 이전의 도구 키트도 업데이트되고 개선되어, UWP 앱을 위한 더 견고한 컨트롤과 레이아웃 템플릿을 제공합니다. 또한 예와 영감을 제공하기 위해 새로운 도구와 샘플이 추가되었습니다.
Fluent 디자인 효과 | 이러한 새로운 효과는 Fluent Design 시스템의 일부이며 깊이, 관점 및 동작을 사용하여 사용자가 중요한 UI 요소에 집중할 수 있도록 합니다. </br>* [아크릴 소재](../design/style/acrylic.md)는 투명한 텍스처를 만드는 브러시 유형입니다. </br>* [시차 효과](../design/motion/parallax.md)는 앱에 3차원 깊이 및 관점을 추가합니다. </br>* [표시](../design/style/reveal.md)는 앱의 중요한 요소를 강조 표시합니다. </br> 자세한 내용은 [Fluent 디자인 개요](https://fluent.microsoft.com/)를 확인하십시오.
키보드 가속기 | [키보드 가속기](../design/input/keyboard-accelerators.md) 또는 바로 가기로 앱의 사용 편의성과 접근성을 높입니다. 사용자가 앱 UI를 탐색하지 않고도 일반적인 작업이나 명령을 호출하는 직관적인 방법을 제공하며 기능에 필요한 범위에 맞게 구성할 수 있습니다.
잉크 입력 | [CoreIncrementalInkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.core.coreincrementalinkstroke) API를 통해 개별 **InkPoint** 개체를 사용하여 점진적으로 렌더링할 수 있는 개별 잉크 스트로크를 만들 수 있습니다. </br></br> [CoreInkPresenterHost](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.core.coreinkpresenterhost) API를 통해 관련된 **InkCanvas** 컨트롤 없이 **InkPresenter** 개체를 호스팅할 수 있습니다.
Radial Controller | [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration) API가 앱의 보기 또는 프로세스에 대한 **RadialController** 메뉴를 포함하는 기능으로 업데이트되었습니다.
Live Tile | [데스크톱 브리지 Win32 앱에서 보조 타일](../design/shell/tiles-and-notifications/secondary-tiles-desktop-pinning.md)을 고정합니다.
알림 메시지 | 단추에 있는 [보류 중인 업데이트](../design/shell/tiles-and-notifications/toast-pending-update.md)를 사용하여 알림 내에서 다단계 상호 작용을 만들 수 있습니다.
UI 컨트롤 | 이러한 새 컨트롤은 멋진 UI를 신속하고 손쉽게 빌드할 수 있도록 합니다. </br>* [색 선택 컨트롤](../design/controls-and-patterns/color-picker.md)은 사용자를 색상을 검색해서 선택할 수 있도록 해줍니다. </br>* [탐색 보기 컨트롤](../design/controls-and-patterns/navigationview.md)는 앱에 최상위 탐색을 손쉽게 추가할 수 있게 해줍니다. </br>* [사람 그림 컨트롤](../design/controls-and-patterns/person-picture.md)은 사람에 대한 아바타 이미지를 표시합니다. </br>* [평점 컨트롤](../design/controls-and-patterns/rating.md)은 사용자가 콘텐츠 및 서비스에 대한 만족도를 반영하는 평점을 손쉽게 확인 및 설정할 수 있도록 해줍니다.
음성 및 톤 | 앱에서 텍스트를 작성하는 방법에 대한 조언을 제공하기 위해 [UWP 앱의 음성과 톤에 대한 지침](../design/style/voice-and-tone.md)을 새로 추가했습니다. 어떤 앱을 제작하든, 이해하기 쉽고 친근하며 필요한 정보를 주는 언어를 사용해야 합니다.

## <a name="gaming"></a>게임

특징 | 설명
 :------ | :------
게임 브로드캐스팅 | **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** 네임스페이스의 신규 API를 통해 앱에서 시스템에서 제공한 게임 브로드캐스트 UI를 시작할 수 있습니다. </br>또한 브로드캐스팅을 시작하거나 중지하는 경우 앱에 알리는 이벤트를 등록할 수 있습니다. **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** 네임스페이스의 신규 API를 통해 오디오 및 비디오를 녹음/녹화하고 게임 플레이의 스크린샷을 캡처할 수 있습니다. </br>또한 시스템이 브로트캐스트에 포함되어 스트림을 캡처하는 메타데이터를 제공하여 앱이 게임 플레이 이벤트와 동기화되는 보기 환경을 제공할 수도 있습니다. 이러한 기능에 대한 자세한 내용은 [게임 브로드캐스트 및 캡처](../gaming/game-broadcast-and-capture.md)를 참조하세요.
게임 채팅 오버레이 | [GameChatOverlay 클래스](https://docs.microsoft.com/uwp/api/windows.gaming.ui.gamechatoverlay)는 기본 게임 채팅 오버레이 인스턴스를 가져오고, 오버레이의 원하는 위치를 설정하고, 이에 메시지를 추가하는 메서드를 제공합니다.
게임 장치 정보 | 다른 콘솔 기능으로 인해 유니버설 Windows 플랫폼(UWP) 게임 개발자들은 하드웨어를 최적으로 사용하는 방법에 대해 런타임 선택을 하기 위해 게임을 실행하는 콘솔의 종류를 확인하는 방법이 필요합니다. **&lt;gamingdeviceinformation.h&gt;** 의 [게임 장치 정보](https://aka.ms/gamingdeviceinfo) API가 이 기능을 제공합니다.
게임 모드 | UWP(유니버설 Windows 플랫폼)을 위한 [게임 모드](https://msdn.microsoft.com/library/windows/desktop/mt808808) API를 사용하여 Windows 10의 게임 모드가 제공하는 장점을 활용하여 가장 최적화된 게임 환경을 만들 수 있습니다. 이러한 API는 **&lt;expandedresources.h&gt;** 헤더에 있습니다.
게임 모니터 | [GameMonitor 클래스](https://docs.microsoft.com/uwp/api/windows.gaming.ui.gamemonitor)를 통해 앱이 디바이스의 게임 모니터 권한 상태를 가져오고 사용자에게 게임 모니터링을 사용하도록 설정하라는 메시지를 표시할 수 있습니다.
TruePlay | [TruePlay](https://aka.ms/trueplay)는 개발자에게 PC 게임 내 부정 행위에 대처하기 위한 새로운 도구 집합을 제공합니다. TruePlay에 등록한 게임은 일반적인 공격의 클래스를 완화하는 보호된 프로세스에서 실행됩니다. 유니버설 Windows 플랫폼(UWP)에 대한 TruePlay API를 통해 Windows 10 PC에서 게임 및 게임 모니터링 시스템 간의 상호 작용을 제한할 수 있습니다. 이러한 API는 **&lt;gamemonitor.h&gt;** 헤더에 있습니다.
Xbox Live | UWP 및 XDK(Xbox 개발자 키트) 게임 모두에 대한 Xbox Live 개발자를 위한 문서를 추가했습니다. </br>* Xbox Live API를 사용하여 게임을 Xbox Live 소셜 게임 네트워크에 연결하는 방법은 [Xbox Live 개발자 가이드](../xbox-live/index.md)를 참조하세요. </br>* [Xbox Live 크리에이터스 프로그램](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 사용하면 모든 UWP 게임 개발자가 PC와 Xbox One에 모두에 Xbox Live 지원 게임을 게시할 수 있습니다. </br>* Xbox Live 개발자가 사용할 수 있는 프로그램 및 기능에 대한 자세한 내용은 [Xbox Live 개발자 프로그램 개요](../xbox-live/developer-program-overview.md)를 참조하세요.

## <a name="develop-windows-apps"></a>Windows 앱 개발

특징 | 설명
 :------ | :------
UWP 앱 정품 인증 | 다음과 같은 새로운 기능을 사용할 수 있습니다. </br>* [StartupTask 클래스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.startuptask)를 사용하여 사용자가 로그인할 때 또는 시스템 시작 시 UWP 앱 시작을 지정할 수 있습니다. </br> * UWP 앱이 [명령줄에서 시작](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)한 경우 식별합니다. </br>* [RequestRestartAsync() 및 RequestRestartForUserAsync()](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication) API를 사용하여 프로그래밍 방식으로 UWP 앱 다시 시작을 요청합니다. </br>* [Windows 설정 앱 시작](../launch-resume/launch-settings-app.md)이 `ms-settings:storagesense`, `ms-settings:cortana-notifications` 등과 같은 새로운 URI 스키마를 반영하도록 업데이트되었습니다.
앱 패키징 | 웹 페이지에서 UWP 앱 패키지를 다운로드할 수 있도록 앱 설치 관리자가 확장되었습니다. 또한 앱 패키지 관련 세트를 이제 앱 설치 관리자로 다운로드할 수 있습니다. 자세히 알아보려면 새로운 [앱 설치 관리자를 사용하여 UWP 앱 설치](../packaging/appinstaller-root.md) 섹션을 참조하세요.
앱 서비스 및 확장 | 사용자가 Microsoft Store에서 설치할 수 있는 패키지를 통해 앱을 확장할 수 있는 UWP(유니버설 Windows 플랫폼) 추가 앱 정보를 작성하고 호스팅하는 것을 돕는 새로운 [앱 확장 만들기 및 사용](../launch-resume/how-to-create-an-extension.md) 가이드가 추가되었습니다. </br></br> 앱을 확장 및 구성 요소화하는 데 사용할 수 있는 Windows 10의 다른 기술을 분류하는 새로운 [서비스, 확장 및 패키지로 앱 확장](../launch-resume/extend-your-app-with-services-extensions-packages.md) 가이드가 추가되었습니다.
백그라운드 작업 | 백그라운드 작업을 활용하는 데 도움이 되는 세 개의 가이드를 추가했습니다.</br></br> * 백그라운드 또는 확장된 실행 제한이 전혀 없이 장치에서 사용할 수 있는 모든 리소스를 사용하려면 [백그라운드에서 무기한 실행](../launch-resume/run-in-the-background-indefinetly.md)을 참조하세요. 이는 엔터프라이즈 UWP 앱 및 Microsoft Store에 전송되지 않는 UWP 앱에 적용됩니다. </br></br> * 앱 내에서 백그라운드 작업을 활성화하려면 [앱 내에서 백그라운드 작업 트리거](../launch-resume/trigger-background-task-from-app.md)를 참조하세요. </br></br>* UWP 앱이 업데이트될 때 실행되는 백그라운드 작업을 만들려면 [UWP 앱 업데이트 시 백그라운드 작업 실행](../launch-resume/run-a-background-task-during-updatetask.md)을 참조하세요.
Cortana | [Cortana 기술 키트](https://docs.microsoft.com/cortana/skills/overview)를 사용하여 Cortana의 자연스러운 기능을 확장하고 사용자의 앱 및 서비스와 상호 작용하는 기술을 추가하고 테스트합니다.
데스크톱 브리지 | Windows 10 에서 데스크톱 응용 프로그램에 최신 환경을 추가하는 데 도움이 되는 세 개의 가이드를 추가했습니다. </br>* 올바른 파일을 찾아 참고하고, Windows 10 사용자를 위한 UWP 환경을 돋보이게 하려면 [Windows 10용 데스크톱 응용 프로그램 향상](../porting/desktop-to-uwp-enhance.md) 가이드를 참조하세요. </br></br>* UWP 앱 컨테이너에서 실행해야 하는 최신 XAML UI 및 다른 UWP 환경을 통합하려면 [최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장](../porting/desktop-to-uwp-extend.md)을 참조하세요. </br></br>* WPF, Windows Forms, UWP, Android 및 iOS 응용 프로그램 간에 코드를 공유하려면 [유니버설 Windows 플랫폼으로 응용 프로그램 마이그레이션](../porting/desktop-to-uwp-migrate.md)을 참조하세요.
데스크톱 브리지 패키지 | Visual Studio에 전체 신뢰 데스크톱 응용 프로그램을 패키징하는 데 필수적이었던 수동 단계를 모두 제거하는 새 [패키지 프로젝트](../porting/desktop-to-uwp-packaging-dot-net.md)가 도입되었습니다. 패키징 프로젝트를 추가하고 데스크톱 프로젝트를 참조한 다음 F5 키를 눌러 앱을 디버깅하면 됩니다. 수동 조정이 필요하지 않습니다. 이 새로운 간소화된 환경은 이전 버전의 Visual Studio에서 사용할 수 있는 환경에 대해 상당히 향상되었습니다.
진단 및 스레딩 | 새로운 진단 API는 실행 중인 앱에 대한 정보를 제공합니다. </br></br>* [AppMemoryReport](https://docs.microsoft.com/uwp/api/Windows.System.AppMemoryReport) 클래스는 앱에서 예상되는 총 커밋 제한, 개인 커밋 사용량 등에 대한 정보를 제공합니다. </br>* [AppDiagnosticInfo](https://docs.microsoft.com/uwp/api/windows.system.appdiagnosticinfo) 클래스는 이제 앱 또는 작업의 실행 상태를 모니터링하고 실행 상태가 변경되는 경우 알림을 제공합니다. </br>* [MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) 클래스에 앱 메모리 사용량 제한을 설정하고 예상되는 앱 메모리 사용량 제한을 보고하는 새로운 방법을 제공합니다. </br></br>작업을 우선 순위대로 정렬하고 이를 [DispatcherQueue](https://docs.microsoft.com/uwp/api/windows.system.dispatcherqueue) 클래스로 다른 스레드에서 실행되도록 할 수 있습니다. 이 기능은 [CreateDispatcherQueueController](https://msdn.microsoft.com/library/windows/desktop/mt826210.aspx) 기능을 통해 Win32에서도 사용할 수 있습니다.
EdgeHTML 16 | Microsoft Edge 및 JS 기반 유니버설 Windows 플랫폼 앱을 지원하는 웹 플랫폼이 EdgeHTML 16으로 업데이트되었으며, 이제 F12 개발자 도구에 대한 주요 개선, CSS 그리드 레이아웃에 대한 지원, 그리고 기타 중요한 기능이 포함되었습니다. </br></br> * [CSS 그리드 레이아웃](https://docs.microsoft.com/microsoft-edge/dev-guide/css/grid-layout)이 이제 Microsoft Edge에서 지원됩니다. 그리드 레이아웃은 부동 또는 스크립트를 사용하여 위치 지정으로 가능한 것보다 많은 레이아웃 가변성을 가능하게 하는 2차원 그리드 기반 레이아웃 시스템을 정의합니다.</br></br> * [Microsoft Edge F12 개발자 도구 문서](https://docs.microsoft.com/microsoft-edge/f12-devtools-guide)가 향상된 안정성 및 성능에 대해 업데이트되었습니다. 또한 개발 환경을 최적화하기 위해 새로운 기능들이 추가되었습니다. </br></br>* Microsoft Edge 내에서만 [WebVR](https://docs.microsoft.com/microsoft-edge/webvr/)에 [동작 컨트롤러](https://docs.microsoft.com/microsoft-edge/webvr/input#controller-buttons) 및 다양한 [Windows Mixed Reality 헤드셋](https://docs.microsoft.com/microsoft-edge/webvr/hardware)에 대한 지원이 추가되었습니다. 초당 프레임 수 90까지 지원하기 위해 WebVR도 최적화되었습니다. </br></br> 변경 사항 및 새로 지원되는 API의 전체 목록은 [Microsoft Edge 개발자 가이드](https://docs.microsoft.com/microsoft-edge/dev-guide)를 참조하세요.
지도 3D 요소 | 3D 개체를 지도에 추가할 수 있습니다. 새 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 클래스를 사용하여[3D 제조 형식(3MF)](http://3mf.io/specification/) 파일에서 3D 개체를 가져올 수 있습니다.
지도 요소 스타일 | [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntry), 및 [MapStyleSheetEntryState](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntryState)의 두 가지 새로운 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 속성을 사용하여 지도 요소의 모양을 사용자 지정할 수 있습니다. </br></br>* [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntry) 속성을 사용하여 지도 요소가 기본 지도의 일부처럼 보이도록 만들 수 있습니다(예: *물*과 같은 지도 스타일 시트의 기존 항목에 요소 스타일을 설정). </br></br>* [MapStyleSheetEntryState](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntryState) 속성을 사용하여 지도 스타일 시트에서 *가리키기* 및 *선택함*과 같은 기본 상태를 활용하여 지도 요소의 모양을 수정하거나 이를 재정의하여 사용자 지정할 수 있습니다.
지도 계층 | [지도 계층](../maps-and-location/display-poi.md#layers)에 관심 요소 포인트를 추가한 다음 해당 계층에 바로 XAML을 바인딩할 수 있습니다. 요소를 계층에 그룹화합니다. 그런 다음 각 계층을 서로 독립적으로 조작할 수 있습니다. 예를 들어 각 계층에 자신의 이벤트 세트가 있으므로 특정 계층에서 이벤트에 응답하고 해당 이벤트 관련 작업을 수행할 수 있습니다.
지도 위치 정보 | UI 요소 또는 사용자가 터치하는 앱의 영역 위, 아래, 또는 측면에 [경량 팝업 창](../maps-and-location/display-maps.md#placecard) 내에서 지도를 표시할 수 있습니다.  사용자가 컨텍스트를 변경하면 이 창은 닫힙니다. 그러면 사용자가 위치에 대한 정보를 보기 위해 다른 앱 또는 브라우저 창으로 전환하지 않아도 됩니다.
지도 서비스 | 둘러보고자 하나요? 새로운 [MapRouteOptimization.Scenic](https://docs.microsoft.com/uwp/api/windows.services.maps.maprouteoptimization) 값을 사용하여 가장 멋진 도로를 포함하여 경로를 최적화하고 [MapRoute.IsScenic](https://docs.microsoft.com/uwp/api/windows.services.maps.maproute)을 사용하여 기존 경로에 멋진 도로 포함되어 있는지 여부를 알아볼 수 있습니다.
미디어 캡처 | [MediaFrameReader를 사용하여 미디어 프레임 처리](../audio-video-camera/process-media-frames-with-mediaframereader.md) 문서가 업데이트되어 새로운 [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 클래스를 사용하여 여러 미디어 원본에서 시간 연관 프레임을 가져오는 방법을 설명합니다. </br></br>[MediaFrameReader를 사용하여 미디어 프레임 처리](../audio-video-camera/process-media-frames-with-mediaframereader.md)가 버퍼링된 프레임 취득에 대한 설명을 포함하도록 업데이트되어 앱이 이전 프레임을 처리하는 동안 취득한 프레임을 누락하지 않고 앱이 취득한 프레임을 순서대로 앱에 제공할 수 있습니다. </br></br>또한 **MediaCapture** 개체가 하나 이상의 미디어 프레임 소스를 포함하는 미디어 프레임 소스 그룹으로 초기화된 경우 XAML 페이지의 **MediaPlayerElement** 컨트롤에 미디어 프레임을 표시할 수 있는 **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** 개체를 만들 수 있습니다.</br></br>자세한 내용은 [MediaFrameReader를 사용하여 미디어 프레임 처리](../audio-video-camera/process-media-frames-with-mediaframereader.md)를 참조하세요.
미디어 재생 | 기본 미디어 재생 문서에 새 섹션인 [MediaPlayer를 사용하여 오디오 및 비디오 재생](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)이 추가되었습니다. </br></br>* [MediaPlayer를 사용하여 구형 비디오 재생](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) 섹션은 지원되는 형식의 시야를 조정하고 방향을 볼 수 있는 기능을 포함하여 구형으로 인코딩된 비디오를 재생하는 방법을 보여 줍니다. </br></br>* [프레임 서버 모드에서 MediaPlayer 사용](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) 섹션은 [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer)에서 재생되는 프레임을 Direct3D 표면에 복사하는 방법을 보여 줍니다. 이를 통해 픽셀 셰이더를 사용한 실시간 효과 적용 시나리오가 가능합니다. 예제 코드는 Win2D를 사용하여 재생되는 비디오에 흐림 효과를 빠르게 구현하는 방법을 보여 줍니다.
내 피플 | 내 피플을 사용하면 사용자가 응용 프로그램에서 바로 작업 표시줄에 연락처를 고정할 수 있습니다. [응용 프로그램에 내 피플 지원을 추가하는 방법을 알아보세요.](../contacts-and-calendar/my-people-support.md) </br></br>* [내 피플 공유](../contacts-and-calendar/my-people-sharing.md)를 사용하면 사용자가 작업 표시줄에서 바로 응용 프로그램을 통해 파일을 공유할 수 있습니다. </br>* [내 피플 알림](../contacts-and-calendar/my-people-support.md)은 사용자가 자신의 고정된 연락처로 보낼 수 있는 새로운 종류의 알림입니다.
.NET Standard 2.0 | 유니버설 Windows 플랫폼은 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)을 완전히 구현합니다. 이 새 버전의 Standard에는 즐겨 찾는 NuGet 패키지 및 타사 라이브러리에 대한 호환성 shim과 함께 여러 .NET API를 대폭 늘렸습니다. </br></br> iOS, Android 등 다른 플랫폼을 대상으로 하려는 경우 또는 데스크톱 응용 프로그램이 있고 UWP 앱을 만들려는 경우 코드를 .NET Standard 2.0 클래스 라이브러리로 이동하고 앱의 각 버전에 해당 코드를 다시 사용할 수 있습니다.
작업 표시줄에 고정 | 새 TaskbarManager 클래스를 사용하여 사용자에게 [작업 표시줄에 앱을 고정](../design/shell/pin-to-taskbar.md)하도록 요청할 수 있습니다.
서비스 지점 | [서비스 지점 디바이스를 시작](../devices-sensors/pos-get-started.md)하는 데 도움이 되는 새 가이드를 추가했습니다. 여기에서는 디바이스 열거, 디바이스 기능 확인, 디바이스 클레임, 디바이스 공유에 대해 다룹니다.
음성 인식 | 이제 [SpeechRecognitionTopicConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) 웹 서비스와 함께 [SpeechRecognitionListConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)를 사용하여 받아쓰는 동안 사용될 것으로 생각되는 도메인별 키워드 세트를 제공하여 받아쓰기 정확도를 향상시킬 수 있습니다.
사용자 작업 | 새 [Windows.ApplicationModel.UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) API를 사용하면 나중에 계속 사용할 수 있고 다른 디바이스에도 사용할 수 있는 사용자 작업을 캡슐화할 수 있습니다.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

이전 버전의 Windows 1703 출시 이후 이 섹션에 기능이 추가되었습니다. 모든 Windows 개발자에게 제공되며 업데이트된 SDK가 필요하지 않습니다.

특징 | 설명
 :------ | :------
계정 관리 | 여러 계정 사용자를 추가하기 위해 [개발자 센터 계정에 Azure AD 테넌트를 연결](../publish/associate-azure-ad-with-dev-center.md)할 때 이제 더 많은 유연성을 제공합니다. 단일 개발자 센터 계정에 여러 Azure AD 테넌트를 연결하거나 하나 이상의 개발자 센터 계정에 단일 Azure AD 테넌트를 연결할 수 있습니다.
광고 | Microsoft Advertising SDK를 사용하여 이제 앱에서 [기본 광고](../monetize/native-ads.md)를 표시할 수 있습니다. 기본 광고는 제목, 이미지, 설명, 행동 촉구 텍스트 등의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다. 현재 기본 광고는 파일럿 프로그램에 참여하고 있는 개발자만 사용할 수 있지만, 조만간 모든 개발자가 이 기능을 사용할 수 있도록 지원할 계획입니다.
가격 책정 및 가용성 |  새 가격 책정 및 가용성 옵션을 사용하면 [가격 변경을 예약](../publish/set-and-schedule-app-pricing.md)하고 [정확한 출시 날짜를 설정](../publish/configure-precise-release-scheduling.md)할 수 있습니다.
Store 분석 API | [Microsoft Store 분석 API](../monetize/access-analytics-data-using-windows-store-services.md)에서 이제 [앱 오류에 대한 CAB 파일을 다운로드](../monetize/download-the-cab-file-for-an-error-in-your-app.md)할 수 있는 방법을 제공합니다.
Store 목록 | Store 목록이 잠재 사용자가 참여할 수 있는 새로운 기능으로 향상되었습니다. </br>* 이제 앱의 Store 목록에 [비디오 예고편](../publish/app-screenshots-and-images.md#trailers)을 포함할 수 있습니다. </br></br>* [Store 목록을 가져오고 내보내](../publish/import-and-export-store-listings.md) 빠르게 업데이트할 수 있습니다. 특히 여러 언어로 목록이 있는 경우 더 빠릅니다.
제출 API | [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)가 이제 앱 제출에 [비디오 예고편](../monetize/manage-app-submissions.md#trailer-object)과 [게임 옵션](../monetize/manage-app-submissions.md#gaming-options-object)을 포함하도록 지원합니다.
대상 제품 | [대상 제품](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)은 특정 고객층을 대상으로 맞춤 설정된 매력적인 콘텐츠를 게시하는 방식으로 참여도와 고객 유지, 수익 창출을 향상시킵니다.

## <a name="samples"></a>샘플

### <a name="lunch-scheduler"></a>Lunch Scheduler

[Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler) 샘플은 친구와 동료와의 점심 약속을 예약합니다. 점심 약속을 만들고, 식당에 친구를 초대하면 앱이 관련된 모든 사람의 점심 약속 관리를 담당합니다. 이 앱은 다음을 강조합니다.

- Facebook, 인증을 위한 Microsoft Graph, 그래프 기반 작업, 친구 검색과 같은 서비스와의 통합을 보여줍니다.
- 음식점 추천에 Yelp 및 Bing 지도를 사용합니다.
- 아크릴, 표시, 연결된 애니메이션 등을 포함하여 UWP 앱에 Fluent 디자인 시스템의 요소를 통합합니다.

### <a name="quiz-game"></a>퀴즈 게임

[퀴즈 게임 앱(원격 시스템 세션 API)](https://github.com/microsoft/Windows-appsample-remote-system-sessions) 샘플은 퀴즈 게임 시나리오의 컨텍스트에서 원격 시스템 세션 API를 사용 하는 방법을 보여줍니다. 호스트가 근접 장치에 질문을 보내면 참가자는 자신의 디바이스에서 질문에 응답합니다.

원격 시스템 세션 API를 통해 기기는 근처에 있는 다른 기기에서 검색 가능한 세션을 호스팅할 수 있습니다. 그러면 해당 기기가 이 세션에 참가하고 다른 호스트와 참가자에게 메시지를 보낼 수 있습니다. 
