---
author: QuinnRadich
title: 개발자, 도구 및 기능을 위한 Windows 10의 새로운 기능
description: Windows10 빌드 17110 및 새로운 개발자 도구는 유니버설 Windows 플랫폼이 지원되는 도구, 기능 및 환경을 제공합니다.
keywords: 새로운 기능, 새로운 기능, 업데이트, 업데이트, 기능, 신규, Windows10, 최신, 개발자, 17110, 미리 보기
ms.author: quradic
ms.date: 3/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5d416ad13c2e689c5265164c0269244a387a6c7f
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2018
---
# <a name="whats-new-in-windows-10-for-developers-sdk-preview-build-17110"></a>개발자용 Windows 10 SDK Preview 빌드 17110의 새로운 기능

Visual Studio 2017 및 업데이트된 SDK와 함께 Windows 10 SDK Preview 빌드 17110은 놀라운 유니버설 Windows 플랫폼 앱을 만드는 도구, 기능 및 환경을 제공합니다. Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

다음은 이 SDK Preview에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. 지금은 이러한 기능은 [Windows 참가자 프로그램](https://insider.windows.com/en-us/)의 회원만 액세스할 수 있으며 다음 주요 Windows 10 업데이트에서 공개될 예정입니다. Windows SDK에 추가된 새로운 네임스페이스의 전체 목록을 보려면 [Windows 10 빌드 17110 API의 변경 내용](windows-10-build-17110-api-diff.md)을 참조하세요. Windows 10의 주요 기능에 자세한 내용은 [Windows 10의 새로운 기능](http://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요. 그리고 이전에 있었던 그리고 앞으로 있을 Windows 플랫폼 기능 추가에 대한 개략적인 내용은 [Windows 개발자 플랫폼 기능](https://developer.microsoft.com/windows/platform/features)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

특징 | 설명
 :------ | :------
적응형 및 대화형 알림 메시지 | 적응형 및 대화형 알림으로 앱을 개선합니다. [업데이트된 알림 메시지에 대한 지침](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)으로 시작하고, 이미지 크기 제한, 진행률 표시줄, 그리고 입력 옵션 추가에 대한 새로운 정보를 살펴보세요.
콘텐츠 링크 | 새 [링크 콘텐츠](../design/controls-and-patterns/content-links.md) 컨트롤은 텍스트 컨트롤에 다양한 데이터를 포함하는 방법을 제공하여 사용자가 앱을 종료하지 않고도 사람이나 장소에 대한 정보를 찾고 더욱 활용할 수 있도록 합니다.
디자인 샘플 | [디자인 도구 키트 및 샘플](../design/downloads/index.md) 페이지에 BuildCast 샘플이 추가되었습니다. BuildCast는 유니버설 Windows 플랫폼의 다른 기능과 Fluent 디자인 시스템을 소개하기 위해 고안된 종합적인 샘플입니다.
포함된 필기 | 펜 입력 기능이 [텍스트 컨트롤](../design/controls-and-patterns/text-controls.md)에 추가되어 사용자가 Windows Ink로 텍스트 상자에 직접 쓸 수 있습니다. 사용자가 글을 쓸 때 텍스트는 자연스럽게 필기하는 느낌을 유지하는 스크립트로 변환됩니다.
Fluent 디자인 업데이트 | 새로운 정보 및 지침으로 많은 Fluent 디자인 페이지가 업데이트되었습니다. </br> * [Fluent 디자인 개요](../design/fluent-design-system/index.md)가 최신 Fluent 기능과 부합하도록 업데이트되었습니다. </br> * [강조 표시](../design/style/reveal.md)에 테마 및 사용자 지정 컨트롤에 대한 새로운 지침이 있습니다. </br> * [탐색 기록 및 뒤로 탐색](../design/basics/navigation-history-and-backwards-navigation.md)이 자세한 예시, 디바이스 최적화에 대한 지침 및 지침 사용자 지정 동작에 대한 지침으로 개선되었습니다.
페이지 레이아웃 | [XAML 페이지 레이아웃](../design/layout/layouts-with-xaml.md) 문서가 유연한 레이아웃 및 시각적 상태에 대한 새로운 정보로 업데이트되었습니다. 이러한 기능을 사용하며 귀하의 앱의 여러 요소의 위치를 지정하는 방법과 사용 가능한 시각적 공간에 맞추는 방법에 대해 보다 더 잘 제어할 수 있습니다.
당겨서 새로 고침 | [당겨서 새로 고침](../design/controls-and-patterns/pull-to-refresh.md) 컨트롤을 사용하면 데이터 목록을 아래로 당겨서 더 많은 데이터를 검색할 수 있습니다. 터치 스크린이 있는 디바이스에서 널리 사용됩니다.
탐색 보기 | [탐색 보기](../design/controls-and-patterns/navigationview.md) 컨트롤은 사용자 앱의 최상위 수준 탐색에 대한 축소 가능한 탐색 메뉴를 제공합니다. 이 컨트롤은 탐색 창(햄버거 메뉴) 패턴을 구현하며 여러 창 크기에 맞춰 창의 디스플레이 모드를 자동으로 조정합니다.
포커스 표시 | 새 [포커스 표시](../design/style/reveal-focus.md) 효과는 Xbox One 및 텔레비전 화면 등의 환경에 조명을 제공합니다. 사용자가 게임 패드 또는 키보드 포커스를 이동하면 버튼과 같이 포커스 맞출 수 있는 요소의 테두리를 애니메이션화합니다.
소리 | XAML은 이제 **SpatialAudioMode** 속성으로 3D 오디오를 지원합니다. 구성 방법에 대한 정보는 [소리](../design/style/sound.md)를 참조하세요.
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
콘솔 UWP 앱 | 이제 DOS 또는 PowerShell 콘솔 창 등의 콘솔 창에서 실행되는 C++ /WinRT 또는 /CX UWP 콘솔 앱을 작성할 수 있습니다. 콘솔 앱은 입출력에 콘솔 창을 사용합니다. 이제 UWP 콘솔 앱이 Microsoft Store에 게시될 수 있으며 앱 목록에 항목과 시작 메뉴에 고정할 수 있는 기본 타일이 있습니다. 자세한 내용은 [유니버설 Windows 플랫폼 콘솔 앱 만들기](../launch-resume/console-uwp.md)를 참조하세요.
액세스할 수 있는 기술(AT)에 대해 지원되는 주요 건물 및 머리글 | 주요 건물 및 머리글은 화면 읽기 프로그램 등의 보조 기술의 사용자에 대한 효율적인 탐색에 도움이 되는 사용자 인터페이스의 섹션을 정의합니다. 자세한 내용은 [주요 건물 및 머리글](../design/accessibility/landmarks-and-headings.md)을 참조하세요.
기계 학습 | Windows 기계 학습은 미리 숙련된 기계 학습 모델을 Windows 10 디바이스에서 평가하는 앱을 빌드할 수 있습니다. 플랫폼에 대한 자세한 내용은 [Windows 기계 학습](../machine-learning/index.md)을 참조하세요. </br> [MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.preview) 네임스페이스에는 앱이 기계 학습 모델을 로드하고, 데이터를 입력으로 바인딩하고, 결과를 계산하도록 하는 클래스가 포함되어 있습니다.
지도 컨트롤 | [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 클래스에 특정 지역(예: 시/도)의 언어에 따라 지도 컨트롤에서 콘텐츠를 표시하는 데 사용할 수 있는 **지역**이라는 새 속성이 있습니다.
지도 요소 | [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 클래스에 사용자가 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement)와 상호 작용할 수 있는지 여부를 지정하는 데 사용할 수 있는 **IsEnabled**라는 새 속성이 있습니다.
지도 위치 정보 | [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo) 클래스에는 주소 및 표시 이름을 사용하여 [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo)를 만드는 데 사용할 수 있는 새로운 메서드 **CreateFromAddress**가 포함됩니다.
지도 서비스 | [MapRouteDrivingOptions](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutedrivingoptions) 클래스에는 지정된 날짜 및 시간에 일반적인 교통 조건으로 경로를 계산하는 데 사용할 수 있는 **DepartureTime**이라는 새 속성이 포함됩니다.
다중 인스턴스 UWP 앱 | UWP 앱은 여러 인스턴스를 지원할 수 있습니다. 다중 인스턴스 UWP 앱의 인스턴스를 실행 중이며 후속 정품 인증을 요청하는 경우 플랫폼은 기존 인스턴스를 활성화하지 않습니다. 대신 다른 프로세스에서 실행 중인 인스턴스를 새로 만듭니다. 자세한 내용은 [다중 인스턴스 유니버설 Windows 앱 만들기](../launch-resume/multi-instance-uwp.md)를 참조하세요.
PlayReady | Microsoft PlayReady는 무단 사용으로부터 디지털 콘텐츠를 보호하기 위한 일련의 기술입니다. PlayReady는 모든 종류의 디바이스 및 앱 및 모든 운영 체제에서 실행됩니다. [앱에 PlayReady를 포함하는 방법을 알아봅니다.](https://docs.microsoft.com/playready/)
화면 캡처 | [Windows.Graphics.Capture 네임스페이스](https://docs.microsoft.com/uwp/api/windows.graphics.capture)는 디스플레이 또는 응용 프로그램 창에서 프레임을 획득하고 비디오 스트림 또는 스냅숏을 만들어 공동 작업 및 대화형 환경을 빌드하기 위해 API를 제공합니다. 자세한 내용은 [화면 캡처](../audio-video-camera/screen-capture.md)를 참고하십시오.
시스템 트리거 | [CustomSystemEventTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.customsystemeventtrigger)를 사용하여 OS가 필요한 시스템 트리거를 제공하지 않는 경우 사용자가 시스템 트리거를 지정할 수 있습니다. 이러한 경우는 하드웨어 드라이버 및 UWP 앱이 둘 다 타사 제품인 경우, 하드웨어 드라이버가 앱이 처리하는 사용자 지정 이벤트를 발생하게 해야 할 때입니다. 예를 들어, 오디오 잭이 연결되어 있을 때 사용자에게 알려야 하는 오디오 카드가 있습니다.
사용자 작업 | **UserActivitySessionHistoryItem** 클래스에 사용자의 최근 활동을 검색하는 새로운 메서드가 있습니다. 자세한 내용은 [GetRecentUserActivitiesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel#Windows_ApplicationModel_UserActivities_UserActivityChannel_GetRecentUserActivitiesAsync_System_Int32_) 및 해당 오버 로드를 참조하세요.
Windows Mixed Reality | 확장 중인 Windows Mixed Reality 플랫폼을 지원하기 위해 신규 API가 [Windows.Graphic.Holographic](https://docs.microsoft.com/uwp/api/Windows.Graphics.Holographic) 및 [Windows.UI.Input.Spatial](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Spatial) 네임스페이스에 추가되었습니다.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

특징 | 설명
 :------ | :------
특정 시장의 현지 통화에 자유 형식의 가격 입력 | 특정 시장에 대한 앱의 기본 가격을 무시한 경우 더 이상 표준 기준 가격 중 하나를 선택하는 것으로 제한되지 않습니다. 이제 시장의 현지 통화로 자유 형식의 가격을 입력하도록 선택할 수 있습니다. 자세한 내용은 [앱 가격 설정 및 예약](../publish/set-and-schedule-app-pricing.md)을 참조하세요. **이 기능은 모든 Windows 개발자에게 제공되며 업데이트된 SDK가 필요하지 않습니다.**
저장소 컨텍스트 | [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 클래스가 새로운 메서드를 선택할 수 있도록 업데이트되었습니다. 이러한 메서드는 앱에 대한 패키지 업데이트 및 추가 기능의 다운로드 및 설치를 관리합니다.
이제 모든 개발자가 구독 추가를 사용할 수 있습니다. | 구독 추가를 만들고 게시하여 자동화된 반복 청구 기간으로 귀하의 앱 및 게임에서 디지털 제품(앱 기능 또는 디지털 콘텐츠)을 판매할 수 있습니다. 자세한 내용은 [앱에 구독 추가 기능을 사용하도록 설정](../monetize/enable-subscription-add-ons-for-your-app.md)을 참조하세요. **이 기능은 모든 Windows 개발자에게 제공되며 업데이트된 SDK가 필요하지 않습니다.**

## <a name="videos"></a>Videos

다음 동영상은 Fall Creators Update 이후 게시되었으며 개발자를 위한 Windows 10의 새로운 기능과 향상된 기능을 소개합니다.

### <a name="package-a-net-app-in-visual-studio"></a>Visual Studio에서 .NET 앱 패키징

그 어느 때보다도 쉽게 데스크톱 앱을 유니버설 Windows 플랫폼으로 가져올 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=fJkbYPyd08w)하여 배포를 위해 .NET 앱을 패키징하는 방법을 알아보고 자세한 내용은 [이 페이지를 확인](../porting/desktop-to-uwp-packaging-dot-net.md)하세요.

### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램

Xbox Live 크리에이터스 프로그램을 사용하면 개발자는 UWP 게임을 신속하게 Xbox One 및 Windows 10에 게시할 수 있습니다. [이 동영상을 시청](https://www.youtube.com/watch?v=zpFfHHBkVq4)하여 프로그램에 대해 학습한 다음 [이 페이지를 확인](https://www.xbox.com/developers/creators-program)하여 시작합니다.

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>Windows Mixed Reality용 3D 앱 시작 관리자 만들기

3D 시작 관리자는 사용자가 그들의 Mixed Reality 홈 환경에서 앱의 진정한 현실감 표현을 배치할 수 있는 독특한 방법을 제공합니다. [동영상을 시청](https://www.youtube.com/watch?v=TxIslHsEXno)하여 3D 모델을 준비하고 이를 앱에 대한 시작 관리자로 지정하는 방법을 알아보고 자세한 내용은 [개발자 문서를 읽거나](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers) [디자인 지침을 확인](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance)하세요.

### <a name="motion-controller-tracking"></a>모션 컨트롤러 추적

모션 컨트롤러는 Windows Mixed Reality에서 사용자의 손을 나타냅니다. [동영상을 시청](https://www.youtube.com/watch?v=rkDpRllbLII)하여 Mixed Reality 헤드셋의 보기 필드 안과 밖에서 모션 컨트롤러가 어떻게 작동하는지 알아보고 [여기에서 컨트롤러 추적에 대해 자세한 내용을 읽어보십시오.](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)

### <a name="accessibility-tools-for-windows-developers"></a>Windows 개발자용 접근성 도구

Windows 10 SDK에는 앱의 성능을 테스트하고 그 접근성을 향상시킬 수 있는 여러 가지 도구가 포함됩니다. Inspect 및 AccEvent 도구는 모두 사용할 수 있는 앱을 확인하는 데 도움이 됩니다. [동영상을 시청](https://www.youtube.com/watch?v=ce0hKQfY9B8&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF&t=0s&index=1)하여 이 도구에 대해 자세히 알아본 다음 [테스트 접근성에 대한 자세한 내용을 읽어보십시오](../design/accessibility/accessibility-testing.md).