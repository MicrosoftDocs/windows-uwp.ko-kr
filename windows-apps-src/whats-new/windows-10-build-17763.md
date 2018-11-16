---
author: QuinnRadich
title: 개발자, 도구 및 기능을 위한 Windows 10의 새로운 기능
description: Windows 10 빌드 17763 및 새로운 개발자 도구는 도구, 기능 및 유니버설 Windows 플랫폼이 지원 되는 환경을 제공 합니다.
keywords: 새로운 기능, 새로운 기능, 업데이트, 업데이트, 기능, 신규, Windows 10, 최신, 개발자, 17763
ms.author: quradic
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1503b6816a1ebd687ddd320c550c4476a4c5a038
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995051"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>빌드 17763 개발자를 위한 Windows 10의 새로운 란

Windows 10 빌드 17763 (10 월 2018 라고도 업데이트 또는 버전 1809), Visual Studio 2017 및 업데이트 된 SDK와 함께, 도구, 기능 및 놀라운 유니버설 Windows 플랫폼 앱 환경을 제공 합니다. Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

다음은 이 릴리스에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. Windows SDK에 추가 된 새 네임 스페이스의 전체 목록, [Windows 10 빌드 17763 API 변경 내용](windows-10-build-17763-api-diff.md)을 참조 하세요. Windows 10의 주요 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](http://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요. 그리고 이전에 있었던 그리고 앞으로 있을 Windows 플랫폼 기능 추가에 대한 개략적인 내용은 [Windows 개발자 플랫폼 기능](https://developer.microsoft.com/windows/platform/features)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

특징 | 설명
 :------ | :------
앱 아이콘 및 로고 | [앱 아이콘 및 로고 페이지](../design/style/app-icons-and-logos.md) , 재작성 및 이제 최신 Visual Studio 아이콘 도구를 표시 하 Microsoft Store에서 앱 목록에 이미지 추가 정보를 제공 합니다.
디자인 방문 페이지 | UWP 디자인 영역 및 Fluent 디자인을 최신 추가 기능에 대 한 정보에 요약 개요를 [방문 페이지 디자인을 업데이트](https://developer.microsoft.com/windows/apps/design) 합니다.
Fluent 디자인 컨트롤 | Fluent 디자인 시스템 및 앱의 apparence 향상 하기 위해 다음과 같은 새로운 UI 컨트롤을 추가한: </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) UI 캔버스에 항목의 컨텍스트에서 일반적인 사용자 작업을 표시할 수 있습니다. </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [분할 단추](../design/controls-and-patterns/buttons.md#create-a-split-button)및 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 앱의 사용자 인터페이스를 향상 하기 위해 특수 기능을 사용 하 여 단추 컨트롤을 제공 합니다. </br> * [메뉴 모음](../design/controls-and-patterns/menus.md) 가로 행에 일련의 여러 최상위 수준에 맞는 메뉴를 보여 줍니다. </br> * [NavigationView](../design/controls-and-patterns/navigationview.md) 는 이제 앱에 더 작은 다양 한 탐색 옵션 및 콘텐츠에 대 한 더 많은 공간이 필요 하는 경우 위쪽 탐색을 지원 합니다. </br> * [TreeView](../design/controls-and-patterns/tree-view.md) 에서 항목 템플릿, 데이터 바인딩을 지원 하 고 끌어서 놓기 향상 되었습니다.
Fluent 디자인 업데이트 | 다음 Fluent 디자인 페이지가 시각적 업데이트 및 사소 수행 되었습니다. </br> * [맞춤, 여백, 안쪽 여백](../design/layout/alignment-margin-padding.md) </br> * [색](../design/style/color.md) </br> * [Windows 앱 용 fluent 디자인](../design/fluent-design-system/index.md) </br> * [앱 디자인 소개](../design/basics/design-and-ui-intro.md) </br> * [탐색 기본 사항](../design/basics/navigation-basics.md) </br> * [반응 형 디자인 기술](../design/layout/responsive-design.md) </br> * [화면 크기 및 중단점](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [스타일 개요](../design/style/index.md) </br> * [쓰기 스타일](../design/style/writing-style.md) </br> 또한 해당 콘텐츠 영역에 대 한 모든 새 정보를 사용 하 여 다음 페이지 작성해 했습니다. </br> * [아이콘](../design/style/icons.md) 에 이제 아이콘을 사용 하 여 및 클릭할 수 있으므로 실용적인 권장 사항을 제공 합니다. </br> * [입력 체계](../design/style/typography.md) 업데이트 된 지침 및 그림을 사용 하 여 한곳에서 최선을 다하고 유사한 문서에서 정보를 통합 합니다.
응시 입력 및 조작 | [응시 상호 작용](../design/input/gaze-interactions.md) 사용자의 응시, 관심 및 위치와 눈의 움직임을 기반으로 현재 상태를 추적 하도록 앱을 허용 합니다. 이 기능은 보조 기술로 사용할 수 있으며 게임 및 기존 입력된 장치 사용할 수 없는 기타 대화형 시나리오에 대 한 기회를 제공 합니다.
필기 보기 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md) TextBox와 RichEditBox에 대 한 새 잉크 입력된 표면입니다. 사용자가 자신의 펜 입력 표면에 컨트롤을 확장으로 텍스트 컨트롤을 탭 수 있습니다. 이 지침을 관리 하 고 응용 프로그램에서 HandwritingView 사용자 지정 하는 방법을 설명 합니다.
흐름 디자인의 동작 | 타이밍, 감속/가속, 방향 및 무게의 기본 사항에 빌드된 Fluent 디자인 시스템의 동작을 사용 하는 개선 됩니다. 이러한 기본 사항 적용 사용자에 게 앱을 안내 하는 데 도움이 됩니다 하 고 자연 스러운을 반영 하 여 디지털 환경으로 연결 합니다. 이 문서에서 자세히 알아보세요. </br> * [The 움직임 개요](../design/motion/index.md) 이러한 기본 사항을 반영 하도록 업데이트 되었습니다. </br> * 이러한 기본 사항 앱 내에서 적용 하는 방법의 예제를 제공 하는 [연습에서 동작](../design/motion/motion-in-practice.md) 합니다. 또한 XAML 요소의 속성이 변경 되 면 이전 및 새 값 사이 쉽게 보간을 허용 된 암시적 애니메이션에 대 한 정보를 포함 합니다. </br> * [방향 및 무게](../design/motion/directionality-and-gravity.md) 대 앱 사용자의 멘 탈 모델을 구체화 합니다. </br> * [타이밍 및 감속/가속](../design/motion/timing-and-easing.md) 앱에서 동작에 현실감을 추가합니다. </br> * [XAML 속성 애니메이션](../design/motion/xaml-property-animations.md) 에 직접 기본 컴퍼지션 시각적 상호 작용 하지 않고도 XAML 요소의 속성을 애니메이션할 수 있습니다.
페이지 전환 | [페이지 전환](../design/motion/page-transitions.md) 은 앱 페이지 사이 사용자를 탐색 합니다. 사용자가 탐색 계층 구조에 속해 이해 하 고 페이지 간의 관계에 대 한 피드백 제공 데 도움이 됩니다.
텍스트 크기 조정 | [텍스트 크기 조정 지침](../design/input/text-scaling.md) 새로운 사용자가 운영 체제 및 개별 응용 프로그램 전체 상대 글꼴 크기를 변경 하는 기능을 제공 하는 새 텍스트 크기 조정 동작을 수용 하기 위해 응용 프로그램을 업데이트 하는 방법을 설명 합니다. 돋보기 앱 (함 일반적으로 방금 화면 영역 내에서 모든 확대 및 유용성 문제가 자체)를 사용 하 여, 디스플레이 해상도 변경 하거나 (디스플레이 및 일반적인 가시 기반으로 모든 항목의 크기를 조정 된 DPI 배율에 의존 하는 대신 거리), 사용자는 100% (기본값)에서 사이의 텍스트만 크기를 조정 하는 설정을 빠르게 액세스할 수 225%입니다.
도구 키트 | 새 기능으로는 [Adobe XD 및 Adobe Illustrator 도구 키트](../design/downloads/index.md) 업데이트 되었습니다. 이러한 디자인 도구 키트는 UWP 앱 디자인을 위한 레이아웃 템플릿을 제공 합니다.
UI 명령 | [UWP 명령 인프라](../design/basics/commanding-basics.md) 에 대 한 업데이트 (동작, 레이블, 아이콘, 바로 가기 키, 선택 키 및 설명) 명령 개체의 더 나은 캡슐화를 포함 하 고 붙여넣기, 종료, 등의 표준 잘라내기, 복사를 포함 한 일반적인 명령 집합., 이러한 속성을 수동으로 설정 하는 않아도 됩니다. </br> 새 [XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스 deveining 명령 동작 호출 될 때 작업을 수행 하는 대화형 UI 요소에 대 한 기본 클래스를 제공 합니다. [StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand), 미리 정의 된 속성을 사용 하 여 표준 플랫폼 명령 집합을 노출 하는 부모 클래스입니다. 
Windows UI 라이브러리 | [Windows UI 라이브러리](https://aka.ms/winui-docs) 는 UWP 앱에 대 한 컨트롤 및 기타 사용자 인터페이스 요소를 제공 하는 NuGet 패키지의 집합입니다. 사용자가 최신 OS 버전 없는 경우에 앱이 작동 하므로 이러한 패키지를 이전 버전의 Windows 10과 호환 됩니다. </br> Windows UI 라이브러리의 새로운 기능에 대 한 자세한 내용은 참조 [NuGet 패키지에 포함 된 API 네임 스페이스의이 목록은.](https://docs.microsoft.com/uwp/api/overview/winui/)

## <a name="develop-windows-apps"></a>Windows 앱 개발

특징 | 설명
 :------ | :------
바코드 스캐너 | [바코드 스캐너](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner) 설명서, 재구성 및 자세한 정보와 코드 조각을 사용 하 여 향상 된 합니다. 새 항목 추가 했습니다 [접수 바코드 데이터를 이해 하 고](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data)는 바코드 스캐너에서 데이터를 사용 하는 방법에 설명 합니다.
C++/WinRT | [C + + WinRT](https://aka.ms/cppwinrt) 많은 새로운 기능, 변경 및이 릴리스에 대 한 수정 사항이 포함 되어 있습니다. 새로운 기능 및 하도록 고유한 [컬렉션 속성 및 컬렉션 형식과](/windows/uwp/cpp-and-winrt-apis/collections); 구현에서 지 원하는 기본 클래스 [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 태그 확장을 사용 하 여 C + 이제 있습니다 + WinRT 런타임 클래스 (코드 예제, [데이터 바인딩 개요](/windows/uwp/data-binding/data-binding-quickstart)참조). 새로운 기능과이 릴리스에서 변경 된 모든 항목의 전체 설명, 참조 [새로운 C + + WinRT](../cpp-and-winrt-apis/news.md)합니다.</br></br>다른 새로운 C + + WinRT 내용: [XAML 사용자 지정 컨트롤](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl); [' 저자 ' COM 구성 요소](/windows/uwp/cpp-and-winrt-apis/author-coclasses)입니다. [값 범주](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories)입니다. 고 [강력 하 고 약한 참조](../cpp-and-winrt-apis/weak-references.md)합니다.
C + + WinRT 코드 예제 | 추가한 250 C + + WinRT 코드 목록 항목에서는 설명서, 함께 기존 C + + CX 코드 예제입니다.
영향을 주는 지침 | UWP 설명서에 대 한 [영향을 주는 지침](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md) 업데이트 했습니다. 이 새로운 지침 우리의 문서에 기여를 외부에 대 한 기대와 워크플로 설명합니다.
DirectX 그래픽 Infastructure (DXGI) | 새 Windows 10에서 표시 하는 경우 모범 사례에 대 한 문서 제공 하 고 누락 된 DXGI Api에 대 한 설명서에 추가 되었습니다. </br> * [최상의 성능을 위해 DXGI 대칭 이동 모델을 사용 하 여](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model): 성능 및 최신 버전의 Windows에 대 한 프레젠테이션 스택의 효율성을 극대화 하는 방법에 지침을 제공 합니다. </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 메서드](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport): 하드웨어 확장을 지원 하는지 응용 프로그램에 알립니다. </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 열거형](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags): 지원 되는 수준을 하드웨어 컴퍼지션의에 대해 설명 합니다.
시작 | Windows 10에 익숙하지 않은 개발자 수 다음과 같은 일반적인 작업을 수행 하는 방법에 정보 및 지침을 제공 하는 새 항목을 사용 하 여 [시작](../get-started/index.md) 콘텐츠 revitalized 되었습니다가: </br> * [양식 작성](../get-started/construct-form-learning-track.md) </br> * [목록에서 고객 표시](../get-started/display-customers-in-list-learning-track.md) </br> * [설정 저장 및 로드](../get-started/settings-learning-track.md) </br> * [파일 작업](../get-started/fileio-learning-track.md)
지도 스타일 시트 편집기 | 대화형 응용 프로그램에 추가 하는 지도의 모양을 사용자 지정 하려면 새 [지도 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab) 응용 프로그램을 사용 합니다.
Microsoft 알아봅니다. | [Microsoft 학습 사이트](https://www.microsoft.com/learning/default.aspx) 는 새 Microsoft 개발자에 게 새로운 실습 교육 및 교육 기회를 제공합니다. 현재 Microsoft 학습 교육 및 Microsoft 365, Microsoft Azure, Office 365 및 Windows Server에 대 한 인증을 제공합니다.
메모장 | [메모장 업데이트 된](http://aka.ms/ant-man)추가 확대/축소, 찾기/바꾸기, 랩어라운드 및 Unix/Linux (LF) 및 Mac (CR) 줄의 끝에 대 한 지원 합니다.
프로젝트 로마 | [프로젝트 "로마"은](https://docs.microsoft.com/windows/project-rome/) 이제 지원 되는 플랫폼 및 Sdk에서 일관 된 프로그래밍 환경을 제공합니다. </br>  프로젝트 "로마"를 사용 하 여 앱에 대 한 사람 중심, 플랫폼 간 알림 플랫폼을 제공 하는 새 [Microsoft Graph 알림](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview) .
화면 캡처 | 새 [URI 체계](../launch-resume/launch-screen-snipping.md) 사용 앱을 프로그래밍 방식으로 열고, 새 캡처 또는 주석에 대 한 특정 이미지를 사용 하 여 캡처 및 스케치 앱을 실행할 수 있습니다.
데스크톱 응용 프로그램의 UWP 컨트롤 | 이제 Windows 10을 사용 하면 WPF, Windows Forms, 및 c + + Win32 데스크톱 응용 프로그램의 UWP 컨트롤을 사용할 수 있습니다. 즉, 모양, 느낌 및 Windows Ink Fluent 디자인 시스템을 지 원하는 컨트롤 등 UWP 컨트롤을 통해만 사용할 수 있는 최신 Windows 10 UI 기능으로 기존 데스크톱 응용 프로그램의 기능을 개선할 수 있습니다. 이 기능은 *XAML 제도*이라고 합니다. </br> XAML 제도 사용 하는 응용 프로그램 플랫폼에 따라 응용 프로그램에서 사용 하 여 여러 가지 방법으로 제공 합니다. WPF 및 Windows Forms 응용 프로그램에서 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) 디자이너 지향 개발 환경을 제공 하는 컨트롤 집합을 사용할 수 있습니다. C + + Win32 응용 프로그램 [Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) 네임 스페이스에 *UWP XAML 호스팅 API를* 사용 해야 합니다. 자세한 내용은 [데스크톱 응용 프로그램의 UWP 컨트롤](../xaml-platform/xaml-host-controls.md)을 참조 하세요. </br> **참고:** Api 및 XAML 제도 사용할 수 있는 컨트롤은 현재 개발자 미리 보기를 사용할 수 있습니다. 하지만 직접 사용해 프로토타입 작성 하는 코드에서 이제 새, 사용 하는 이러한 프로덕션 코드에서이 시간에 하지 않는 것이 좋습니다.
Windows 기계 학습 | [Windows 컴퓨터 학습](https://docs.microsoft.com/windows/ai/) 가 이제 공식적으로 시작, 최첨단 기계 학습 모델에 대 한 빠른 평가 지원과 같은 기능을 제공 합니다. 응용 프로그램에 통합 하려는 개발자를 지원 하기 위해 신규 및 업데이트에 대 한 몇 가지 리소스를 사용 하 여 새 설명서 사이트를 만들었습니다. </br> * [자습서: Windows 컴퓨터 학습 데스크톱 응용 프로그램 (c + +)을 만들고](https://docs.microsoft.com/windows/ai/get-started-desktop):이 자습서에서는 데스크톱에 대 한 간단한 Windows ML 응용 프로그램을 빌드하는 방법을 보여 줍니다. </br> * [자습서: Windows 컴퓨터 학습 UWP 응용 프로그램 (C#)을 만들고](https://docs.microsoft.com/windows/ai/get-started-uwp):이 단계별 자습서에서는 Windows ML을 사용 하 여 첫 번째 UWP 응용 프로그램을 만듭니다. </br> * [Windows.AI.MachineLearning Namespace](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning): API 참조의 Windows 10 SDK 최신 릴리스에 대 한 업데이트 되었습니다와 개발자 모두 Win32 및 UWP 응용 프로그램에 대 한이 API를 이제 사용할 수 있습니다.
Windows Mixed Reality | 개발자가 수 요청을 하드웨어로 보호 된 백 버퍼 텍스처 디스플레이 하드웨어에서 지 원하는 경우 이제 PlayReady 같은 소스에서 하드웨어로 보호 된 콘텐츠를 사용 하 여 응용 프로그램을 허용 합니다. 하드웨어 보호 지원 및 설정을 [통해 쿼드 계층에 대 한 [Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)새 속성을 사용 하 여 기본 계층을 사용할 수 있습니다. Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)합니다.

## <a name="iot-core"></a>IoT Core

기능 | 설명
 :------ | :------
AssignedAccessSettings | [AssignedAccessSettings 클래스](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings) 는 다양 한 방법에 대 한 호출을 사용 하 고 사용자의 액세스 하는 속성은 특정 장치에 대 한 액세스 설정을 할당 합니다.
기본 앱 개요 | [Windows 10 IoT Core 기본 앱](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp) 은 새로운 기능 및 수동 입력, 날씨 같은 기능을 사용 하 여 업데이트 및 오디오 이었습니다.
대시보드 | 이제 [Windows 10 Iot Core 대시보드](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup) Dragonboard 410 C 또는 NXP 장치에 사용자 지정 FFUs 플래시를 사용 하는 개발자를 허용 합니다.
화상 키보드 | 동일한 터치 키보드 구성 요소를 사용 하 여 windows 데스크톱 버전으로 이제 [IoT 장치에 대 한 화상 키보드](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard) 합니다. 이렇게 하면 받아쓰기 모드, IME 지원 및 입력된 범위는 등의 기능입니다.
로그인 대화 상자에 대 한 제목 표시줄 | 이제 Windows 10 IoT Core [시스템 대화 상자에 대 한 제목 표시줄](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)을 구성 하는 옵션을 제공 합니다.
터치를 통해 절전 모드가 해제 | [Wake on 터치는](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch) 사용자가 해당 화면을 터치 하는 경우 신속 하 게 전환 하는 동안 사용에 없는 동안 끄려면 디바이스의 화면을 수 있습니다. 
Windows.System.Update | 새 [Windows.System.Update 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.system.update) 에 대화형 시스템 업데이트 제어할 수 있습니다. 이 네임 스페이스는 Windows 10 IoT Core 수만 있습니다.

## <a name="web-development"></a>웹 개발

기능 | 설명
 :------ | :------
EdgeHTML 18 | Windows 10 년 10 월 2018 [EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide), Microsoft Edge 브라우저와 UWP 앱에 대 한 JavaScript 엔진에 대 한 최신 업데이트를 사용 하 여 배를 업데이트합니다. 웹 인증 API, 새로운 WebView 컨트롤 기능 등에 대 한 현대화 및 확장 된 지원을 제공 하는 EdgeHTML 18! 도구가 있는 EdgeHTML 18 Edge 개발자 도구 및 Edge 개발자 도구 프로토콜에 자동 업데이트, 새로운 WebDriver 기능 및 향상 된 기능 제공합니다. 모든 세부 정보에 대 한 [EdgeHTML 18에 새로 란](https://docs.microsoft.com/microsoft-edge/dev-guide) 무엇이 고 [최신 Windows 10에서 개발자 도구 업데이트 (EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new) 확인 합니다.
점진적 웹앱 | Windows 10 JavaScript 앱 ( *WWAHost.exe* 프로세스에서 실행 되는 웹 앱) 이제 지원 보기는 활성화 되기 전에 시작 되는 선택적 [응용 프로그램별 백그라운드 스크립트](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps) 및 프로세스의 기간에 대 한 실행 합니다. 이 모니터링 및 수정 탐색, 탐색 간에 상태를 추적, 탐색 오류를 모니터링 하 고 수 보기는 활성화 코드를 실행 합니다. 로 지정 하는 경우는 [`StartPage`](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application) [앱 매니페스트](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)프로그램에 각 앱의 보기 (windows)에 노출 되는 스크립트의 새 인스턴스를 [`WebUIView`](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview) 클래스를 일반 (Win32) [WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)으로 동일한 이벤트, 속성 및 메서드를 제공 합니다.
웹 API 확장 | Mozilla 개발자 네트워크 설명서 브라우저 간 웹 개발을 위한 [레거시 Microsoft API 확장](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) 목록 추가 되었습니다. 이러한 API 확장 Internet Explorer 또는 Microsoft Edge에 고유한 및 호환성 및 broswer 지원 MDN 웹 문서에 대 한 기존 정보를 보충 합니다. 레거시 Microsoft [확장 CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) 및 [JavaScript 확장](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) 도 사용할 수 있으며, 풍부한 웹 MDN 정보 API에 직접 표시를 찾을 수 [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)
WebVR | 우리 [WebVR 개발자 가이드](https://docs.microsoft.com/microsoft-edge/webvr/), 홈 페이지의 전체 디자인 등 재구성 목차의 주요 업데이트를 했습니다. 또한 여러 새 항목을 포함 하 여 쓴: </br> * [WebVR 란 무엇 인가요?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) WebVR 란, 이유를 사용할지 및 개발을 시작 하는 방법을 설명 합니다. </br> * [점진적 웹 앱에 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): WebVR 점진적 웹 앱 (PWA)를 추가 하는 방법을 알아봅니다. </br> * [WebView에 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Windows 10 응용 프로그램에서 WebView 컨트롤에 WebVR 추가 하는 방법을 알아봅니다. </br> * [WebVR 데모](https://docs.microsoft.com/microsoft-edge/webvr/demos): Microsoft Edge 및 Windows Mixed Reality 몰입 형 헤드셋을 사용 하 여 몇 가지 WebVR 데모를 확인 합니다.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

특징 | 설명
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview) 는 모든 Windows 앱에 최신 패키징 환경을 제공 하는 새 Windows 앱 패키지 형식입니다. 오픈 소스 msix 최신 배포 기능을 사용 하는 동안 기존 패키지의 기능을 유지 합니다.
MSIX 패키징 도구 | 새 [MSIX 패키징 도구](https://docs.microsoft.com/windows/msix/mpt-overview))는 소스 코드에 액세스할 수 없는 경우에 MSIX 형식에서 기존 데스크톱 응용 프로그램을 다시 패키징 수 있습니다. 대화형 UI를 통해 또는 명령줄에서 실행할 수 있습니다.
MSIX에 대 한 데스크톱 앱 변환기 지원 | [Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) 를 사용 하 여 MSIX 패키지를 사용 하 여 출력 하는 `-MakeMSIX` 매개 변수입니다.
MSIX MakeAppx.exe 도구 지원 | UWP 앱 또는 기존 데스크톱 응용 프로그램에 대 한 MSIX 패키지를 만드는 MakeAppx.exe 도구를 사용할 수 있습니다. 이 도구는 Windows 10 SDK에 포함되어 있으며 명령 프롬프트 또는 스크립트 파일에서 사용할 수 있습니다. </br> UWP 앱 [만들기 MakeAppx.exe 도구로 앱 패키지를](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)참조 하세요. </br> 데스크톱 응용 프로그램에 대 한 [데스크톱 응용 프로그램을 수동으로 패키징](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)참조 하세요.
지원 프레임 워크 패키지 | [지원 프레임 워크 패키지](https://docs.microsoft.com/windows/msix/package-support-framework-overview) MSIX 컨테이너에서 실행 될 수 있도록 소스 코드에 액세스할 수 없는 경우 수정 기존 데스크톱 응용 프로그램에 적용 하는 데 도움이 되는 오픈 소스 키트입니다.
Store 분석 API | 이제 [Microsoft Store 분석 API](../monetize/access-analytics-data-using-windows-store-services.md) 에 다음과 같은 새로운 메서드가 포함 됩니다. </br> * [UWP 앱에 대 한 정보 데이터 가져오기](../monetize/get-insights-data-for-your-app.md) </br> * [데스크톱 응용 프로그램에 대 한 정보 데이터 가져오기](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [데스크톱 응용 프로그램에 대 한 업그레이드 차단 가져오기](../monetize/get-desktop-block-data.md) </br> * [데스크톱 응용 프로그램에 대 한 업그레이드 차단 세부 정보 가져오기](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>Videos

다음 동영상은 Fall Creators Update 이후 게시되었으며 개발자를 위한 Windows 10의 새로운 기능과 향상된 기능을 소개합니다.

### <a name="cwinrt"></a>C++/WinRT

C + + /winrt는 Windows 런타임 api를 작성 하는 새로운 방법입니다. 헤더 파일 에서만 구현 하 고 최신 앱 기능에 최고 수준의 액세스를 제공 하도록 설계 했습니다. 작동 방식, 그런 다음 [개발자 문서를 읽거나](../cpp-and-winrt-apis/index.md) 자세한 정보에 대 한 자세한 [비디오를 시청 하세요](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) .

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>개발자를 위한 시작: 만들기 및 Windows 10에서 양식을 사용자 지정

이제 Windows 개발자를 위한 [시작 문서의](../get-started/index.md) 기본 앱 개발 작업을 사용 하 여 실습을 제공 합니다. 이 비디오는 이러한 항목 중 하나를 통해 안내 하 고 앱에서 UI 양식 만들기의 기본 사항을 다룹니다. [동영상을 시청](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) 중인 다음 코드를 보려면 [항목으로 확인 합니다.](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>프로젝트 퍼스 낼 리 티 채팅을 사용 하 여 사용자 물어보세요 향상

프로젝트 퍼스 낼 리 티 채팅 채팅 bot를 사용자 지정할 수 있는 가상 사용자를 추가할 수 있습니다. Microsoft 물어보세요 프레임 워크 SDK를 통합 하 여 고객을 상호 작용 하는 더 대화식 방법에 대 한 작은 작용 기능을 추가할 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) [대화형 데모를 사용해](http://aka.ms/PersonalityChat) 실제 경험에 대 한 다음을 구현 하는 방법을 알아봅니다.

### <a name="multi-instance-uwp-apps"></a>다중 인스턴스 UWP 앱

이제 Windows을 사용 하면 자체 별도 프로세스에서 각 UWP 앱의 여러 인스턴스를 실행할 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) 하는 방법에 대 한 자세한 지침에 대 한이 기능을 다음 [개발자 문서를 읽거나](../launch-resume/multi-instance-uwp.md) 를 지 원하는 새 앱을 만드는 방법을 알아보고이 기능을 사용 하는 이유입니다.

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 플러그 인

Unity에 Xbox Live 플러그인 타이틀에 Xbox Live 서명, 통계, 친구 목록, 클라우드 저장소 및 순위표 추가 대 한 지원이 포함 되어 있습니다. [동영상을 시청](https://youtu.be/fVQZ-YgwNpY) , 자세한 내용을 보려면 다음 시작 하려면 [GitHub 패키지를 다운로드](https://aka.ms/UnityPlugin) 합니다.

### <a name="one-dev-question"></a>개발자 질문

개발자 질문 하나 비디오 시리즈 오랜 기간 사용해 온 Microsoft 개발자는 일련의 Windows 개발 팀 문화 및 기록에 대 한 질문을 설명합니다.

* [Windows 개발 및 기록에 Raymond Chen](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Windows 개발 및 기록에 Larry Osterman](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [점진적 웹 앱에서 Aaron Gustafson](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Webhint 도구에 Chris Heilmann](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>샘플

### <a name="customer-orders-database"></a>고객 주문 데이터베이스

[고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) [DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid), [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)및 [확장](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)같은 새 컨트롤을 사용 하도록 업데이트 되었습니다.

### <a name="customer-database-tutorial"></a>고객 데이터베이스 자습서

[고객 데이터베이스 자습서](../enterprise/customer-database-tutorial.md) 의 고객 목록을 관리 하기 위한 기본 UWP 앱을 만들고 개념 및 엔터프라이즈 개발에 유용한 방법을 소개 합니다. UI 요소를 구현 하 고 추가 로컬 SQLite 데이터베이스에 대 한 작업 과정을 단계별로 안내 하 고 더 나아가 하려는 경우 원격 REST 데이터베이스에 연결 하기 위한 느슨한 지침을 제공 합니다.

### <a name="photo-editor-cwinrt"></a>사진 편집기 C + + WinRT

[사진 편집기 샘플 앱](https://github.com/Microsoft/Windows-appsample-photo-editor) 을 사용 하 여 개발을 보여 주는 합니다 [C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 언어 프로젝션입니다. 앱을 사용 하면 **사진** 라이브러리에서 사진을 검색 한 다음 관련된 사진 효과 사용 하 여 선택 된 이미지를 편집 수 있습니다.

### <a name="windows-machine-learning"></a>Windows 기계 학습

[Windows-기계 학습](https://github.com/Microsoft/Windows-Machine-Learning) 저장소 최신 Windows 10 SDK와 함께 작동 하도록 업데이트 되었습니다 및 C#, c + + 및 JavaScript로 작성 된 샘플을 포함 합니다.

### <a name="xaml-hosting-api"></a>XAML 호스팅 API

[XAML 호스팅 API 샘플](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) UWP XAML 호스팅 API (XAML 제도 라고도 함)를 사용 하 여 다양 한 시나리오를 강조 하는 Win32 데스크톱 앱입니다. 프로젝트는 갤러리 스타일 프레젠테이션에서 Windows Ink, 미디어 플레이어 및 탐색 보기 컨트롤을 통합합니다. 일반 외부 컨트롤 사용에 대해서도 설명 XAML 및 기본 XAML 데이터 바인딩 메시지/네이티브 Windows 이벤트를 처리 합니다.