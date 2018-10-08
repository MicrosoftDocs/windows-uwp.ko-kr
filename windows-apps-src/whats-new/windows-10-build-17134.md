---
author: QuinnRadich
title: 개발자, 도구 및 기능을 위한 Windows 10의 새로운 기능
description: Windows10 빌드 17134 및 새로운 개발자 도구는 유니버설 Windows 플랫폼이 지원되는 도구, 기능 및 환경을 제공합니다.
keywords: 새로운 기능, 새로운 기능, 업데이트, 업데이트, 기능, 신규, Windows10, 최신, 개발자, 17134
ms.author: quradic
ms.date: 4/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e2f12190c405ad611cf5b884b82c4a430aa5264f
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415959"
---
# <a name="whats-new-in-windows-10-for-developers-build-17134"></a>개발자를 위한 Windows 10 빌드 17134의 새로운 기능

Visual Studio 2017 및 업데이트된 SDK와 함께 사용되는 Windows 10 빌드 17134(4월 업데이트 또는 버전 1803이라고도 함)는 놀라운 유니버설 Windows 플랫폼 앱을 만드는 도구, 기능 및 환경을 제공합니다. Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

다음은 이 릴리스에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. Windows SDK에 추가된 새 네임스페이스의 전체 목록은 [Windows10 빌드 17134 API 변경 내용](windows-10-build-17134-api-diff.md)을 참조하세요. Windows 10의 주요 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](http://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요. 그리고 이전에 있었던 그리고 앞으로 있을 Windows 플랫폼 기능 추가에 대한 개략적인 내용은 [Windows 개발자 플랫폼 기능](https://developer.microsoft.com/windows/platform/features)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

특징 | 설명
 :------ | :------
적응형 및 대화형 알림 메시지 | 적응형 및 대화형 알림으로 앱을 개선합니다. [업데이트된 알림 메시지에 대한 지침](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)으로 시작하고, 이미지 크기 제한, 진행률 표시줄, 그리고 입력 옵션 추가에 대한 새로운 정보를 살펴보세요.<br><br>[ExpirationTime](https://docs.microsoft.com/uwp/api/windows.ui.notifications.scheduledtoastnotification.expirationtime#Windows_UI_Notifications_ScheduledToastNotification_ExpirationTime)이 예정된 알림 메시지에서 지원됩니다.
콘텐츠 링크 | 새 [링크 콘텐츠](../design/controls-and-patterns/content-links.md) 컨트롤은 텍스트 컨트롤에 다양한 데이터를 포함하는 방법을 제공하여 사용자가 앱을 종료하지 않고도 사람이나 장소에 대한 정보를 찾고 더욱 활용할 수 있도록 합니다.
디자인 샘플 | [디자인 도구 키트 및 샘플](../design/downloads/index.md) 페이지에 BuildCast 샘플이 추가되었습니다. BuildCast는 유니버설 Windows 플랫폼의 다른 기능과 Fluent 디자인 시스템을 소개하기 위해 고안된 종합적인 샘플입니다.
포함된 필기 | 펜 입력 기능이 [텍스트 컨트롤](../design/controls-and-patterns/text-controls.md)에 추가되어 사용자가 Windows Ink로 텍스트 상자에 직접 쓸 수 있습니다. 사용자가 글을 쓸 때 텍스트는 자연스럽게 필기하는 느낌을 유지하는 스크립트로 변환됩니다.
Fluent 디자인 업데이트 | 새로운 정보 및 지침으로 많은 Fluent 디자인 페이지가 업데이트되었습니다. </br> * [Fluent 디자인 개요](../design/fluent-design-system/index.md)가 최신 Fluent 기능과 부합하도록 업데이트되었습니다. </br> * [강조 표시](../design/style/reveal.md)에 테마 및 사용자 지정 컨트롤에 대한 새로운 지침이 있습니다. </br> * [탐색 기록 및 뒤로 탐색](../design/basics/navigation-history-and-backwards-navigation.md)이 자세한 예시, 장치 최적화에 대한 지침 및 지침 사용자 지정 동작에 대한 지침으로 개선되었습니다.
포커스 탐색 | 새 [포커스 탐색](../design/input/focus-navigation.md) 항목에서는 키보드, 게임 패드 또는 원격 제어와 같이 포인팅되지 않는 입력 형식에 의존하는 사용자를 위해 UWP 응용 프로그램을 최적화하는 방법을 설명합니다. 또한 [프로그래밍 방식의 포커스 탐색](../design/input/focus-navigation-programmatic.md)에서는 이러한 환경을 개선하는 데 사용할 수 있는 API에 대해 설명합니다.
바로 가기 키 | [키보드 가속기](../design/input/keyboard-accelerators.md)에 대한 지침이 새로운 유용성 정보로 업데이트되었습니다. 키보드 가속기에 툴팁을 추가하고 컨트롤에 레이블을 추가하여 검색 가능성을 개선하거나 새 API로 기본 키보드 가속기 동작을 재정의합니다.
페이지 레이아웃 | [XAML 페이지 레이아웃](../design/layout/layouts-with-xaml.md) 문서가 유연한 레이아웃 및 시각적 상태에 대한 새로운 정보로 업데이트되었습니다. 이러한 기능을 사용하며 귀하의 앱의 여러 요소의 위치를 지정하는 방법과 사용 가능한 시각적 공간에 맞추는 방법에 대해 보다 더 잘 제어할 수 있습니다.
당겨서 새로 고침 | [당겨서 새로 고침](../design/controls-and-patterns/pull-to-refresh.md) 컨트롤을 사용하면 데이터 목록을 아래로 당겨서 더 많은 데이터를 검색할 수 있습니다. 터치 스크린이 있는 디바이스에서 널리 사용됩니다.
탐색 보기 | [탐색 보기](../design/controls-and-patterns/navigationview.md) 컨트롤은 사용자 앱의 최상위 수준 탐색에 대한 축소 가능한 탐색 메뉴를 제공합니다. 이 컨트롤은 탐색 창(햄버거 메뉴) 패턴을 구현하며 여러 창 크기에 맞춰 창의 디스플레이 모드를 자동으로 조정합니다.
포커스 표시 | 새 [포커스 표시](../design/style/reveal-focus.md) 효과는 Xbox One 및 텔레비전 화면 등의 환경에 조명을 제공합니다. 사용자가 게임 패드 또는 키보드 포커스를 이동하면 버튼과 같이 포커스 맞출 수 있는 요소의 테두리를 애니메이션화합니다.
소리 | XAML은 이제 **SpatialAudioMode** 속성으로 3D 오디오를 지원합니다. 구성 방법에 대한 정보는 [소리](../design/style/sound.md)를 참조하세요.
타일 | [추적형 타일 알림](../design/shell/tiles-and-notifications/chaseable-tile-notifications.md)이 JavaScript 기반 UWP 앱에서 지원됩니다.<br><br>보조 타일 및 배지 알림은 [현재 데스크톱 브리지 앱에서 지원](../design/shell/tiles-and-notifications/secondary-tiles-desktop-pinning.md#send-tile-notifications)됩니다.
트리 보기 | [TreeView](../design/controls-and-patterns/tree-view.md) 컨트롤은 중첩된 항목이 포함된 노드를 확장 및 축소하는 계층적 목록을 만듭니다. 트리 보기 컨트롤을 사용하여 UI에서 폴더 구조 또는 중첩된 관계를 설명할 수 있습니다.
쓰기 스타일 | [쓰기 스타일 지침](../design/style/writing-style.md)에 음성 및 톤과 이를 변환하는 내용이 업그레이드 및 확장되었습니다. 이 새로운 정보는 앱에서 효과적인 텍스트를 만들기 위한 원칙을 제공하며 오류 메시지 또는 대화 상자 등의 컨트롤 쓰기에 대한 모범 사례를 제안합니다.

## <a name="gaming"></a>게임
특징 | 설명
 :------ | :------
게임 개발 시작 | Windows10 게임 개발에 흥미가 있으신가요? 새로운 [게임 개발 시작](../gaming/getting-started.md) 페이지는 앱과 게임을 스스로 설정 및 등록하고 제출하기 위해 수행해야 할 작업에 대한 전체 개요를 제공 합니다.
그래픽 어댑터 | 그래픽 어댑터 기본 설정 및 제거와 관련된 다음 DXGI API가 추가되었습니다. </br> * [IDXGIFactory6](https://msdn.microsoft.com/library/windows/desktop/mt814823) 인터페이스는 지정된 GPU 기본 설정에 따라 그래픽 어댑터를 열거하는 단일 메서드를 사용합니다. </br> * [DXGIDeclareAdapterRemovalSupport](https://msdn.microsoft.com/library/windows/desktop/mt814821) 함수는 프로세스에서 제거되는 모든 그래픽 디바이스를 복원할 수 있음을 나타냅니다. </br> * [DXGI_GPU_PREFERENCE](https://msdn.microsoft.com/library/windows/desktop/mt814822) 열거는 실행할 앱에 대한 GPU의 기본 설정을 설명합니다.

## <a name="develop-windows-apps"></a>Windows 앱 개발

특징 | 설명
 :------ | :------
적응형 카드 | [적응형 카드](https://docs.microsoft.com/adaptive-cards/)는 개발자가 일반적이고 일관된 방법으로 UI 콘텐츠를 교환할 수 있는 개방형 카드 교환 형식입니다. 적응형 카드는 해당 콘텐츠를 호스트 응용 프로그램의 모양과 느낌으로 자동으로 렌더링될 수 있는 JSON 개체로 설명합니다.
앱 리소스 그룹 | [AppResourceGroupInfo](https://docs.microsoft.com/uwp/api/windows.system.appresourcegroupinfo) 클래스에 앱을 일시 중단, 활성(다시 시작), 종료된 상태로 변환하는 데 사용할 수 있는 새로운 메서드가 있습니다.
다양한 파일 시스템 액세스 | **broadFileSystemAccess** 기능을 사용하면 앱이 파일 선택기 스타일 프롬프트 없이 현재 앱을 실행하는 사용자와 같은 파일 시스템에 액세스할 수 있습니다. 자세한 내용은 [파일 액세스 권한](../files/file-access-permissions.md) 및 [앱 접근 권한 값 선언](../packaging/app-capability-declarations.md)의 **broadFileSystemAccess** 항목을 참조하세요.
C++/WinRT | [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)는 WinRT(Windows Runtime) API를 위한 완전한 표준의 최신 C++17 언어 프로젝션입니다. 이는 헤더 파일에서만 구현되고 최신 Windows API에 대한 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT에서 표준 준수 C++17 컴파일러를 사용하여 WinRT API를 작성하고 이용할 수 있습니다. C++ 응용 프로그램(Win32 - UWP)에서는 C++/WinRT를 사용하여 코드를 표준화하고 최신의 정리된 상태로 유지하며, 응용 프로그램을 가볍고 빠르게 만듭니다.
콘솔 UWP 앱 | 이제 DOS 또는 PowerShell 콘솔 창 등의 콘솔 창에서 실행되는 C++ /WinRT 또는 /CX UWP 콘솔 앱을 작성할 수 있습니다. 콘솔 앱은 입출력에 콘솔 창을 사용합니다. 이제 UWP 콘솔 앱이 Microsoft Store에 게시될 수 있으며 앱 목록에 항목과 시작 메뉴에 고정할 수 있는 기본 타일이 있습니다. 자세한 내용은 [유니버설 Windows 플랫폼 콘솔 앱 만들기](../launch-resume/console-uwp.md)를 참조하세요.
확장된 앱 매니페스트 접근 권한 값 | 다양한 파일 시스템 액세스, 서비스 지점 장치에 바코드 스캐너 사용, UWP 콘솔 앱 정의 등의 몇 가지 기능이 앱 패키지 매니페스트 스키마에 추가되었습니다. 자세한 내용은 [Windows 10의 앱 매니페스트 변경 사항](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10)을 참조하세요.
접근성 기술(AT)에 대해 지원되는 이정표 및 머리글 | 주요 건물 및 머리글은 화면 읽기 프로그램 등의 보조 기술의 사용자에 대한 효율적인 탐색에 도움이 되는 사용자 인터페이스의 섹션을 정의합니다. 자세한 내용은 [주요 건물 및 머리글](../design/accessibility/landmarks-and-headings.md)을 참조하세요.
기계 학습 | Windows 기계 학습은 미리 숙련된 기계 학습 모델을 Windows 10 디바이스에서 평가하는 앱을 빌드할 수 있습니다. 플랫폼에 대한 자세한 내용은 [Windows 기계 학습](https://docs.microsoft.com/windows/ai/)을 참조하세요. </br> [MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.preview) 네임스페이스에는 앱이 기계 학습 모델을 로드하고, 데이터를 입력으로 바인딩하고, 결과를 계산하도록 하는 클래스가 포함되어 있습니다.
지도 컨트롤 | [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 클래스에 특정 지역(예: 시/도)의 언어에 따라 지도 컨트롤에서 콘텐츠를 표시하는 데 사용할 수 있는 **지역**이라는 새 속성이 있습니다.
지도 요소 | [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 클래스에 사용자가 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement)와 상호 작용할 수 있는지 여부를 지정하는 데 사용할 수 있는 **IsEnabled**라는 새 속성이 있습니다.
지도 위치 정보 | [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo) 클래스에는 주소 및 표시 이름을 사용하여 [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo)를 만드는 데 사용할 수 있는 새로운 메서드 **CreateFromAddress**가 포함됩니다.
지도 서비스 | [MapRouteDrivingOptions](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutedrivingoptions) 클래스에는 지정된 날짜 및 시간에 일반적인 교통 조건으로 경로를 계산하는 데 사용할 수 있는 **DepartureTime**이라는 새 속성이 포함됩니다.
다중 인스턴스 UWP 앱 | UWP 앱은 여러 인스턴스를 지원할 수 있습니다. 다중 인스턴스 UWP 앱의 인스턴스를 실행 중이며 후속 정품 인증을 요청하는 경우 플랫폼은 기존 인스턴스를 활성화하지 않습니다. 대신 다른 프로세스에서 실행 중인 인스턴스를 새로 만듭니다. 자세한 내용은 [다중 인스턴스 유니버설 Windows 앱 만들기](../launch-resume/multi-instance-uwp.md)를 참조하세요.
패키지 리소스 인덱싱 API 및 사용자 지정 빌드 시스템 | [패키지 리소스 인덱싱(PRI) API](https://docs.microsoft.com/windows/uwp/app-resources/pri-apis-custom-build-systems)를 사용하여 UWP 앱 리소스에 대한 사용자 지정 빌드 시스템을 개발할 수 있습니다. 빌드 시스템은 UWP 앱에 필요한 복잡도 수준에 상관없이 PRI 파일을 만들고 버전화하고 덤프할 수 있습니다. 현재 MakePri.exe 명령줄 도구를 사용하는 사용자 지정 빌드 시스템이 있는 경우 PRI API를 대신 호출하도록 전환하는 것이 좋습니다. 이렇게 하면 성능과 제어가 향상됩니다.
PlayReady | Microsoft PlayReady는 무단 사용으로부터 디지털 콘텐츠를 보호하기 위한 일련의 기술입니다. PlayReady는 모든 종류의 디바이스 및 앱 및 모든 운영 체제에서 실행됩니다. [앱에 PlayReady를 포함하는 방법을 알아봅니다.](https://docs.microsoft.com/playready/)
개인 대상 | 지정한 사용자만 앱의 Store 목록을 볼 수 있도록 하고 싶은 경우에는 새 **개인 대상** 옵션을 사용합니다. 지정한 그룹(들)의 구성원이 아닌 사용자는 앱을 검색하거나 사용할 수 없게 됩니다. 이 옵션은 다른 사람이 앱을 다운로드할 수 없거나 Store 목록을 볼 수 없도록 한 상태에서 테스터에게 앱을 배포할 수 있기 때문에 베타 테스트에 유용합니다. 자세한 내용은 [표시 여부 옵션 선택](../publish/choose-visibility-options.md)을 참조하세요.
점진적 웹앱 | Microsoft Edge 및 UWP 웹앱에서 [점진적 웹앱(PWA)](https://docs.microsoft.com/microsoft-edge/progressive-web-apps)을 지원합니다! </br> * [표준 기반 웹 기술](https://developer.mozilla.org/Apps/Progressive) 및 [기능 검색](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)을 사용하면 푸시 알림, 오프라인 지원 및 OS 통합이 포함된 네이티브 앱 환경을 제공하도록 웹앱을 개선할 수 있으며 아직 PWA 기술을 지원하지 않는 브라우저와 플랫폼에서 뛰어난 기준 웹앱 환경을 제공합니다. </br> * 앱에 [매니페스트 파일을 추가](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)하면 전체 UWP 장치 패밀리(보안 [Windows 10 S 모드 장치](https://www.microsoft.com/windows/windows-10-s) 포함)에서 앱을 설치하고 [Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)에서 배포할 수 있습니다. </br> PWA는 *호스트된 웹앱*이 자연스럽게 진화된 형태이지만 오프라인 시나리오를 위한 표준 기반 지원에서 *서비스 작업자*, *캐시* 및 *API 푸시*의 도움을 받습니다.
화면 캡처 | [Windows.Graphics.Capture 네임스페이스](https://docs.microsoft.com/uwp/api/windows.graphics.capture)는 디스플레이 또는 응용 프로그램 창에서 프레임을 획득하고 비디오 스트림 또는 스냅숏을 만들어 공동 작업 및 대화형 환경을 빌드하기 위해 API를 제공합니다. 자세한 내용은 [화면 캡처](../audio-video-camera/screen-capture.md)를 참고하십시오.
시스템 트리거 | [CustomSystemEventTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.customsystemeventtrigger)를 사용하여 OS가 필요한 시스템 트리거를 제공하지 않는 경우 사용자가 시스템 트리거를 지정할 수 있습니다. 이러한 경우는 하드웨어 드라이버 및 UWP 앱이 둘 다 타사 제품인 경우, 하드웨어 드라이버가 앱이 처리하는 사용자 지정 이벤트를 발생하게 해야 할 때입니다. 예를 들어, 오디오 잭이 연결되어 있을 때 사용자에게 알려야 하는 오디오 카드가 있습니다.
사용자 작업 | 새 [UserActivity 설명서](../launch-resume/useractivities.md)는 여러 장치에서 사용자가 앱에서 수행하고 있었던 작업을 재개하도록 돕는 방법에 대해 설명합니다.</br>**UserActivitySessionHistoryItem** 클래스에 사용자의 최근 활동을 검색하는 새로운 메서드가 있습니다. 자세한 내용은 [GetRecentUserActivitiesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel.getrecentuseractivitiesasync) 및 해당 오버 로드를 참조하세요.
Windows Mixed Reality API | 확장 중인 Windows Mixed Reality 플랫폼을 지원하기 위해 신규 API가 [Windows.Graphic.Holographic](https://docs.microsoft.com/uwp/api/Windows.Graphics.Holographic) 및 [Windows.UI.Input.Spatial](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Spatial) 네임스페이스에 추가되었습니다.
Windows Mixed Reality 문서 | Windows Mixed Reality 개발자 지침이 [docs.microsoft.com에서 호스팅됩니다.](https://docs.microsoft.com/windows/mixed-reality/) 마찬가지로 이러한 UWP 문서에서 이제 GitHub 문제가 포함 된 피드백 하거나 사용자의 기여 끌어오기 요청을 통해 제출 합니다.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

특징 | 설명
 :------ | :------
Microsoft Store에서 패키지 업데이트 다운로드 및 설치 | [Microsoft Store에서 패키지 업데이트 다운로드 및 설치](../packaging/self-install-package-updates.md)를 업데이트했습니다. 업데이트된 내용에는 사용자에게 알림 UI를 표시하지 않는 상태로 패키지 업데이트를 다운로드 및 설치하고 선택적 패키지를 제거하며 앱의 다운로드 및 설치 큐에서 패키지 정보를 가져오는 방법에 대한 예제와 새로운 지침이 있습니다.
특정 시장의 현지 통화에 자유 형식의 가격 입력 | 특정 시장에 대한 앱의 기본 가격을 무시한 경우 더 이상 표준 기준 가격 중 하나를 선택하는 것으로 제한되지 않습니다. 이제 시장의 현지 통화로 자유 형식의 가격을 입력하도록 선택할 수 있습니다. 자세한 내용은 [앱 가격 설정 및 예약](../publish/set-and-schedule-app-pricing.md)을 참조하세요. **이 기능은 모든 Windows 개발자에게 제공되며 업데이트된 SDK가 필요하지 않습니다.**
저장소 컨텍스트 | [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스가 새로운 메서드를 선택할 수 있도록 업데이트되었습니다. 이러한 메서드는 앱에 대한 패키지 업데이트 및 추가 기능의 다운로드 및 설치를 관리합니다.
이제 모든 개발자가 구독 추가를 사용할 수 있습니다. | 구독 추가를 만들고 게시하여 자동화된 반복 청구 기간으로 귀하의 앱 및 게임에서 디지털 제품(앱 기능 또는 디지털 콘텐츠)을 판매할 수 있습니다. 자세한 내용은 [앱에 구독 추가 기능을 사용하도록 설정](../monetize/enable-subscription-add-ons-for-your-app.md)을 참조하세요. **이 기능은 모든 Windows 개발자에게 제공되며 업데이트된 SDK가 필요하지 않습니다.**

## <a name="videos"></a>Videos

다음 동영상은 Fall Creators Update 이후 게시되었으며 개발자를 위한 Windows 10의 새로운 기능과 향상된 기능을 소개합니다.

### <a name="accessibility-tools-for-windows-developers"></a>Windows 개발자용 접근성 도구

Windows 10 SDK에는 앱의 성능을 테스트하고 그 접근성을 향상시킬 수 있는 여러 가지 도구가 포함됩니다. Inspect 및 AccEvent 도구는 모두 사용할 수 있는 앱을 확인하는 데 도움이 됩니다. [동영상](https://www.youtube.com/watch?v=ce0hKQfY9B8&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF&t=0s&index=1)을 시청하여 이 도구에 대해 자세히 알아본 다음 [접근성 테스트에 대한 자세한 내용](../design/accessibility/accessibility-testing.md)을 읽어보세요.

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>Windows Mixed Reality용 3D 앱 시작 관리자 만들기

3D 시작 관리자는 사용자가 그들의 Mixed Reality 홈 환경에서 앱의 진정한 현실감 표현을 배치할 수 있는 독특한 방법을 제공합니다. [동영상을 시청](https://www.youtube.com/watch?v=TxIslHsEXno)하여 3D 모델을 준비하고 이를 앱에 대한 시작 관리자로 지정하는 방법을 알아보고 자세한 내용은 [개발자 문서를 읽거나](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers) [디자인 지침을 확인](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance)하세요.

### <a name="creating-a-uwp-console-app"></a>UWP 콘솔 앱 만들기

PowerShell 또는 DOS 콘솔 창에서 실행되는 UWP 앱을 만들 수 있습니다. [동영상](https://www.youtube.com/watch?v=bwvfrguY20s&t=0s&index=1&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF)을 시청하여 UWP 앱을 만드는 방법을 알아보고 [관련 문서](../launch-resume/console-uwp.md)에서 자세한 내용을 확인해보세요. 

### <a name="how-to-use-windows-ml-in-your-app"></a>앱에서 Windows ML을 사용하는 방법

Windows 기계 학습은 미리 숙련된 기계 학습 모델을 Windows 10 장치에서 평가하는 앱을 빌드할 수 있습니다. [동영상](https://www.youtube.com/watch?v=8MCDSlm326U&index=2&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF)을 시청하여 빠른 연습을 실행해보고 [관련 문서](https://docs.microsoft.com/windows/uwp/machine-learning/)에서 전체 스토리를 읽어보세요.

### <a name="motion-controller-tracking"></a>모션 컨트롤러 추적

모션 컨트롤러는 Windows Mixed Reality에서 사용자의 손을 나타냅니다. [동영상을 시청](https://www.youtube.com/watch?v=rkDpRllbLII)하여 Mixed Reality 헤드셋의 보기 필드 안과 밖에서 모션 컨트롤러가 어떻게 작동하는지 알아보고 [여기에서 컨트롤러 추적에 대해 자세한 내용을 읽어보십시오.](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)

### <a name="package-a-net-app-in-visual-studio"></a>Visual Studio에서 .NET 앱 패키징

그 어느 때보다도 쉽게 데스크톱 앱을 유니버설 Windows 플랫폼으로 가져올 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=fJkbYPyd08w)하여 배포를 위해 .NET 앱을 패키징하는 방법을 알아보고 자세한 내용은 [이 페이지를 확인](../porting/desktop-to-uwp-packaging-dot-net.md)하세요.

### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램

Xbox Live 크리에이터스 프로그램을 사용하면 개발자가 Xbox One 및 Windows 10에 UWP 게임을 빠르게 게시할 수 있습니다. [동영상](https://www.youtube.com/watch?v=zpFfHHBkVq4)을 시청하여 프로그램에 대해 알아보고 [이 페이지](https://www.xbox.com/developers/creators-program)에서 시작 방법을 확인하세요.

### <a name="one-dev-question---why-was-docments-and-settings-renamed-users"></a>개발자 질문 - 문서 및 설정을 왜 사용자로 이름을 바꾸었나요?

문서 및 설정 디렉토리의 이름을 바꾼 이유는 무엇인가요? [Raymond Chen이 해당 이름이 어디에서 유래되었는지와 변경된 이유에 대해 설명합니다](https://www.youtube.com/watch?v=4vDHQewVmM8&index=1&list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7). Windows 및 해당 기록에 대한 자세한 개발 내용은 [Raymond의 블로그](https://blogs.msdn.microsoft.com/oldnewthing/)를 확인해보세요.


## <a name="samples"></a>샘플

### <a name="coloring-book"></a>색칠하기 책

[색칠하기 책 샘플](https://github.com/Microsoft/Windows-appsample-coloringbook)에서는 사용자 지정 잉크 드라이 API를 사용하여 잉크 렌더링 성능을 개선하는 등 고급 잉크 시나리오를 통합하는 주요 업데이트가 수행되었습니다. 또한 전체 채우기 및 아트워크에서 정의한 영역의 선 내부를 색칠하는 기능이 지원됩니다. 

### <a name="photo-lab"></a>사진 랩

[사진 랩 샘플](https://github.com/Microsoft/Windows-appsample-photo-lab)은 파일이 여러 개 있을 때 성능을 향상시키기 위해 데이터 가상화를 사용하여 그림 라이브러리의 이미지를 로드하도록 업데이트되었습니다. 또한 샘플의 이미지 편집 페이지에서는 [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 클래스를 사용하여 효과를 적용합니다.