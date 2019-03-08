---
title: 개발자, 도구 및 기능을 위한 Windows 10의 새로운 기능
description: Windows 10 17763 빌드하고 새로운 개발자 도구는 도구, 기능 및 유니버설 Windows 플랫폼에서 제공 하는 환경을 제공 합니다.
keywords: 새로운 기능, 새로운 기능, 업데이트, 업데이트, 기능, 새로운 Windows 10을 최신 개발자 17763
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2b172844e75d9af3d0112e03f155708af3ca6bed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637748"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>새로운 Windows 10의 개발자에 게 제공 되는 빌드 17763

Windows 10 빌드 17763 (10 월 2018 라고도 업데이트나 1809 버전), Visual Studio 2017 및 업데이트 된 SDK와 함께, 도구, 기능 및 환경을 주목할 만한 유니버설 Windows 플랫폼 앱 만들기를 제공 합니다. Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

다음은 이 릴리스에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. Windows SDK에 추가 하는 새 네임 스페이스의 전체 목록을 참조 하세요. 합니다 [Windows 10 빌드 17763 API 변경 내용](windows-10-build-17763-api-diff.md)합니다. Windows 10의 주요 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](https://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요. 그리고 이전에 있었던 그리고 앞으로 있을 Windows 플랫폼 기능 추가에 대한 개략적인 내용은 [Windows 개발자 플랫폼 기능](https://developer.microsoft.com/windows/platform/features)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

기능 | 설명
 :------ | :------
앱 아이콘 및 로고 | 합니다 [앱 아이콘 및 로고 페이지](../design/style/app-icons-and-logos.md) 가 다시 작성 되었으며, 이제 최신 Visual Studio 아이콘 도구를 보여 줍니다. 및 Microsoft Store 앱의 목록에 이미지 추가 정보를 제공 합니다.
디자인 방문 페이지 | [방문 페이지 디자인 업데이트](https://developer.microsoft.com/windows/apps/design) UWP 디자인 영역 및 Fluent 디자인의 최신 추가 기능에 대 한 정보는 일목요연 개요에 있습니다.
Fluent 디자인 컨트롤 | Fluent Design System 및 앱의 apparence 강화 하려면 다음과 같은 새 UI 컨트롤에 추가한: </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) UI 캔버스에 있는 항목의 컨텍스트에서 일반적인 사용자 작업을 표시할 수 있습니다. </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button), 및 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 앱의 사용자 인터페이스를 향상 시키기 위해 특별 한 기능을 사용 하 여 단추 컨트롤을 제공 합니다. </br> * [메뉴 모음](../design/controls-and-patterns/menus.md) 가로 행에 여러 개의 최상위 메뉴 집합을 표시 합니다. </br> * [NavigationView](../design/controls-and-patterns/navigationview.md) 이제 앱에 더 작은 수의 탐색 옵션 및 콘텐츠에 대 한 더 많은 공간이 필요 하는 경우 위쪽 탐색 모음을 지원 합니다. </br> * [TreeView](../design/controls-and-patterns/tree-view.md) 데이터 바인딩, 항목: 템플릿, 지원 및 끌어서 놓기 하도록 향상 되었습니다.
Fluent 디자인 업데이트 | 시각적 개체 업데이트 및 사소한 변경은 다음 Fluent 디자인 페이지에 적용 했습니다. </br> * [맞춤, 여백, 안쪽 여백](../design/layout/alignment-margin-padding.md) </br> * [색](../design/style/color.md) </br> * [Windows 앱 용 Fluent 디자인](../design/fluent-design-system/index.md) </br> * [앱 디자인 소개](../design/basics/design-and-ui-intro.md) </br> * [탐색의 기본 사항](../design/basics/navigation-basics.md) </br> * [반응 형 디자인 기술](../design/layout/responsive-design.md) </br> * [화면 크기 및 중단점](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [스타일 개요](../design/style/index.md) </br> * [필기 스타일](../design/style/writing-style.md) </br> 또한 자신의 콘텐츠 영역에 있는 모든 새 정보를 사용 하 여 다음 페이지를 다시 작성 되었습니다 했습니다. </br> * [아이콘](../design/style/icons.md) 아이콘을 사용 하 고 클릭할 수 있도록 하는 것에 대 한 실용적인 권장 사항을 제공 합니다. </br> * [입력 체계](../design/style/typography.md) 업데이트 된 지침과 그림을 사용 하 여 한 곳에서 모든 항목을 저장 하는 유사한 문서에서 정보를 통합 합니다.
응시 입력 및 상호 작용 | [상호 작용 gaze](../design/input/gaze-interactions.md) 앱이 사용자의 게이즈, 주의가 및 위치와의 시각의 움직임을 기반으로 현재 상태를 추적할 수 있도록 합니다. 이 기능은 보조 기술에서 사용할 수 있습니다 하 고 게임 및 기존 입력된 장치를 사용할 수 없는 기타 대화형 시나리오에 대 한 기회를 제공 합니다.
필기 보기 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md) TextBox와 RichEditBox 새 잉크 입력된 표면입니다. 사용자 쓰기 화면으로 컨트롤을 확장 하는 펜을 사용 하 여 텍스트 컨트롤을 탭 수 있습니다. 이 설명서에는 관리 및 응용 프로그램에서 HandwritingView 사용자 지정 하는 방법을 설명 합니다.
Fluent 디자인 동작 | Fluent Design System의 동작을 사용 하 여 진화 타이밍, 감속/가속, 방향이 있으며 및 중력의 기본적인 사항을 기반으로 합니다. 이러한 기본 적용 사용자에 게 앱을 안내 하는 데 도움이 됩니다 하 고 자연 스러운을 반영 하 여 디지털 경험을 사용 하 여 연결 합니다. 이 문서에서 자세히 알아보세요. </br> * [동작 개요](../design/motion/index.md) 이러한 기본 사항에 맞게 업데이트 되었습니다. </br> * [연습에서 동작](../design/motion/motion-in-practice.md) 앱 내에서 이러한 기본을 적용 하는 방법의 예제를 제공 합니다. 또한 XAML 요소의 속성이 변경 될 때 이전 및 새 값 간에 쉽게 보간 허용 하는 암시적 애니메이션에 대 한 정보를 포함 합니다. </br> * [방향 및 중력](../design/motion/directionality-and-gravity.md) solidifies 앱의 사용자 멘 탈 모델입니다. </br> * [시간 및 감속/가속](../design/motion/timing-and-easing.md) 앱의 동작에 현실성을 추가 합니다. </br> * [XAML 속성 애니메이션](../design/motion/xaml-property-animations.md) 직접 기본 컴퍼지션 Visual 상호 작용할 필요 없이 XAML 요소의 속성 애니메이션을 적용할 수 있습니다.
페이지 전환 | [페이지 전환](../design/motion/page-transitions.md) 사용자 앱에서 페이지 사이 이동 합니다. 지 사용자가 탐색 계층 구조에서 현재 위치를 이해 하 고 페이지 간 관계에 대 한 피드백을 제공 합니다.
텍스트 크기 조정 | 새 [크기 조정 지침 텍스트](../design/input/text-scaling.md) 을 사용자가 OS 및 개별 응용 프로그램 모두에서 상대 글꼴 크기를 변경 하는 기능을 제공 하는 동작을 확장 하 고 새 텍스트를 수용 하기 위해 응용 프로그램을 업데이트 하는 방법에 설명 합니다. 돋보기 (하는 앱을 일반적으로 방금 화면 영역 안의 모든 항목을 확대 하 고 자체 사용 편의성 문제를 소개)를 사용 하 여, 디스플레이 해상도 변경 하거나 (하는 표시 및 일반적인 보기에 따라 모든 크기를 조정 DPI 배율에 의존 하는 대신 거리)를 사용자에만 텍스트를 100% (기본값)에서 사이의 크기 조정 설정에 빠르게 액세스할 수 최대 225%입니다.
도구 키트 | 합니다 [Adobe XD 및 Adobe Illustrator 도구 키트](../design/downloads/index.md) 새로운 기능으로 업데이트 되었습니다. 이러한 디자인 도구 키트는 UWP 앱을 디자인 하기 위한 컨트롤 및 레이아웃 템플릿을 제공 합니다.
UI 명령 | 업데이트를 [UWP 명령 인프라](../design/basics/commanding-basics.md) (동작, 레이블, 아이콘, 키보드 액셀러레이터 키, 액세스 키 및 설명) 명령 개체의 캡슐화를 효율적으로 및 표준 잘라내기, 복사를 포함 하 여 일반적인 명령 집합을 포함 합니다. 붙여넣기, 종료 등, 이러한 속성을 수동으로 설정할 필요가 없습니다. </br> 새 [XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand) 호출 될 때 작업을 수행 하는 대화형 UI 요소의 명령 동작을 정의 하기 위한 기본 클래스를 제공 합니다. 이 클래스는 부모 클래스에 대 한 [StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand), 미리 정의 된 속성을 사용 하 여 표준 플랫폼 명령 집합을 노출 합니다. 
Windows UI 라이브러리 | 합니다 [Windows UI 라이브러리](https://aka.ms/winui-docs) 는 UWP 앱에 대 한 컨트롤 및 기타 사용자 인터페이스 요소를 제공 하는 NuGet 패키지의 집합입니다. 앱 사용자가 최신 OS 버전을가지고 있지 하는 경우에 작동 하므로 이러한 패키지를 Windows 10의 이전 버전과 호환 됩니다. </br> Windows UI 라이브러리의 새로운 기능에 대 한 자세한 내용은 참조 하세요. [이 NuGet 패키지에 포함 된 API 네임 스페이스이 목록입니다.](https://docs.microsoft.com/uwp/api/overview/winui/)

## <a name="develop-windows-apps"></a>Windows 앱 개발

기능 | 설명
 :------ | :------
바코드 스캐너 | 합니다 [바코드 스캐너](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner) 설명서 다시 구성 및 더 많은 세부 정보 및 코드 조각을 사용 하 여 향상 되었습니다. 새 항목을 추가 했습니다 [얻기 바코드 데이터 및 이해](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data)를 가져오고 바코드 스캐너의 데이터로 작업 하는 방법을 설명 합니다.
C++/WinRT | [C + + /cli WinRT](https://aka.ms/cppwinrt) 많은 새로운 기능, 변경 및이 릴리스에 대 한 수정 프로그램이 포함 되어 있습니다. 새 함수 및 기본 클래스가 자체적으로 구현에서 지원할 [컬렉션의 속성 및 컬렉션 형식](/windows/uwp/cpp-and-winrt-apis/collections); 이제 사용할 수 있습니다 합니다 [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 태그 확장을 사용 하 여 C + + WinRT 런타임 클래스 (코드 예제를 보려면 [데이터 바인딩 개요](/windows/uwp/data-binding/data-binding-quickstart)). 새로운 기능과이 릴리스에서 변경 된 모든 항목에 대 한 자세한 내용은 참조 하세요 [새로운 C + + /cli WinRT](../cpp-and-winrt-apis/news.md)합니다.</br></br>다른 새 C + + /cli WinRT 콘텐츠에 포함 되어 있습니다. [XAML 사용자 지정 컨트롤](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl); [작성자 COM 구성 요소](/windows/uwp/cpp-and-winrt-apis/author-coclasses); [범주 값](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories); 하 고 [강력 하 고 약한 참조](../cpp-and-winrt-apis/weak-references.md)합니다.
C + + /cli WinRT 코드 예제 | 추가한 250 C + + /cli WinRT 코드 샘플의 설명서를 함께 제공 되는 기존 C + + CX 코드 예제입니다.
영향을 주는 지침 | 업데이트 했습니다 [영향을 주는 지침](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md) UWP 설명서에 대 한 합니다. 이 새 지침에는 워크플로 및 문서에 대 한 외부 기여에 대 한 기대치를 명확히 구분합니다.
DirectX 그래픽 인프라 (DXGI) | 새로운 설명서 DXGI Api 누락에 대 한 추가 되 고 Windows 10에서 제공 하는 경우 모범 사례에 대 한 문서를 제공 했습니다. </br> * [최상의 성능을 위해 DXGI 플립 모델을 사용 하 여](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model): 성능 및 최신 버전의 Windows presentation stack 효율성을 최대화 하는 방법에 지침을 제공 합니다. </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 메서드](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport): 하드웨어 확장은 지원 되는 응용 프로그램에 알립니다. </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 열거형](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags): 지원 되는 수준을 하드웨어 컴퍼지션에 대해 설명 합니다.
시작 | 우리의 [시작](../get-started/index.md) 콘텐츠 개발자가 Windows 10에 다음과 같은 일반적인 작업을 수행할 수 있습니다 하는 방법에 정보 및 지침을 제공 하는 새 항목과 revitalized 되었습니다 했습니다. </br> * [양식 생성](../get-started/construct-form-learning-track.md) </br> * [목록에서 고객에 게 표시](../get-started/display-customers-in-list-learning-track.md) </br> * [저장 및 로드 설정](../get-started/settings-learning-track.md) </br> * [파일 작업](../get-started/fileio-learning-track.md)
맵 스타일 시트 편집기 | 새 [맵 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab) 대화형으로 응용 프로그램에 추가한 맵 표시를 사용자 지정 응용 프로그램입니다.
Microsoft Learn | 새 [사이트 Microsoft 학습](https://www.microsoft.com/learning/default.aspx) Microsoft 개발자에 게 새 실습 학습 및 교육 기회를 제공 합니다. 현재 Microsoft 학습 교육 및 Microsoft 365, Microsoft Azure, Office 365 및 Windows Server에 대 한 인증을 제공합니다.
Windows 메모장 | [메모장 업데이트 되었습니다](https://aka.ms/ant-man)추가, 확대/축소, 찾기/바꾸기, 순환 및 Unix/Linux (LF) 및 Mac (CR)에 해당 하는 줄의 끝에 대 한 지원.
프로젝트 로마 | [로마 프로젝트](https://docs.microsoft.com/windows/project-rome/) 이제 지원 되는 플랫폼 및 Sdk에서 일관 된 프로그래밍 환경을 제공 합니다. </br>  새 [Microsoft Graph 알림을](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview) 프로젝트 로마를 사용 하 여 앱에 대 한 사용자 중심의 플랫폼 간 알림 플랫폼을 제공 합니다.
화면 캡처 | 새 [URI 체계](../launch-resume/launch-screen-snipping.md) 새 캡처를 프로그래밍 방식으로 열고 앱을 사용 하거나 주석에 대 한 이미지를 사용 하 여 캡처 & 스케치 앱을 시작 합니다.
데스크톱 응용 프로그램에서 UWP 컨트롤 | 이제 Windows 10을 사용 하면 WPF, Windows Forms 및 c + + Win32 데스크톱 응용 프로그램에서 UWP 컨트롤을 사용할 수 있습니다. 즉, 모양, 느낌 및 Windows 잉크 Fluent Design System을 지 원하는 컨트롤 등의 UWP 컨트롤을 통해 이용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 기존 데스크톱 응용 프로그램의 기능을 개선할 수 있습니다. 이 기능은 이라고 *XAML 제도*합니다. </br> 사용 하는 응용 프로그램 플랫폼에 따라 응용 프로그램에서 XAML 제도 사용 하는 여러 방법을 제공 합니다. WPF 및 Windows Forms 응용 프로그램에서 컨트롤의 집합을 사용할 수는 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) 디자이너 중심 개발 환경을 제공 하는 합니다. C + + Win32 응용 프로그램을 사용 해야 합니다는 *호스팅 API는 UWP XAML* 에 [Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) 네임 스페이스입니다. 자세한 내용은 [데스크톱 응용 프로그램에서 UWP 컨트롤](../xaml-platform/xaml-host-controls.md)합니다. </br> **참고:** Api 및 XAML 제도 사용 하도록 설정 하는 컨트롤 개발자 미리 보기로 현재 사용할 수 있습니다. 직접 사용해 고유의 프로토타입 코드에서 이제 하는 것이 좋습니다, 있지만 사용 것 프로덕션 코드에서는 현재 하지 않는 것이 좋습니다.
Windows Machine Learning | [Windows Machine Learning](https://docs.microsoft.com/windows/ai/) 가 이제 공식적으로 시작, 최첨단 기계 학습 모델에 대 한 보다 빠른 평가가 및 지원과 같은 기능을 제공 합니다. 응용 프로그램에 통합 하려는 개발자를 지원 하기 위해 몇 가지 새로운 기능과 업데이트 된 리소스를 사용 하 여 새 설명서 사이트를 만들었습니다. </br> * [자습서: Windows Machine Learning 데스크톱 응용 프로그램 (c + +)을 만들고](https://docs.microsoft.com/windows/ai/get-started-desktop): 이 자습서에는 데스크톱에 대 한 간단한 Windows 기계 학습 응용 프로그램을 빌드하는 방법을 보여 줍니다. </br> * [자습서: Windows Machine Learning UWP 응용 프로그램을 만듭니다 (C#)](https://docs.microsoft.com/windows/ai/get-started-uwp): 이 단계별 자습서에서는 Windows ML을 사용 하 여 첫 번째 UWP 응용 프로그램을 만듭니다. </br> * [Windows.AI.MachineLearning Namespace](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning): Windows 10 SDK의 최신 릴리스에 대 한 API 참조가 업데이트 되었습니다 및 Win32와 UWP 응용 프로그램에 대 한 개발자가이 API를 이제 사용할 수 있습니다.
Windows Mixed Reality | 개발자 수 이제 요청 된 하드웨어가 보호 따라서 질감 표시 하드웨어에서 지 원하는 경우 하드웨어로 보호 된 콘텐츠를 사용 하 여 PlayReady와 같은 원본에서 응용 프로그램을 허용 합니다. 하드웨어 보호 지원 및 설정 수는 기본 계층의 새 속성을 사용 하 여 [Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera), 및 쿼드 레이어를 통해 [ Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)합니다.

## <a name="iot-core"></a>IoT Core

기능 | 설명
 :------ | :------
AssignedAccessSettings | 합니다 [AssignedAccessSettings 클래스](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings) 수 있도록 다양 한 방법에 대 한 호출 하 고 사용자의 액세스 속성은 특정 장치에 대 한 액세스 설정을 할당 합니다.
기본 앱 개요 | 합니다 [Windows 10 IoT Core 기본 앱](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp) 새로운 기능 및 날씨, 잉크 입력, 오디오 등의 기능을 사용 하 여 업데이트 되었습니다.
대시보드 | 합니다 [Windows 10 Iot Core 대시보드](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup) 이제 개발자가 자신의 장치에 사용자 지정 FFUs를 플래시 하려면 Dragonboard 410 C 또는 NXP을 사용 합니다.
화상 키보드 | 합니다 [IoT 장치에 대 한 화상 키보드](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard) 이제 동일한 터치 키보드 구성 요소를 사용 하 여 Windows 데스크톱 버전으로 합니다. 이 받아쓰기 모드, IME 지원 및 다양 한 입력된 범위 같은 기능을 사용 합니다.
로그인 대화 상자에 대 한 제목 표시줄 | Windows 10 IoT Core는 이제 구성 하는 옵션 제공 [시스템에 대 한 막대 대화 상자 제목](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)합니다.
터치에서 절전 모드 해제 | [터치에서 절전 모드 해제](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch) 기능을 해제 하는 동안 사용 중이 아님, 사용자가 해당 화면을 터치 하면 신속 하 게 설정 하는 동안 장치의 화면을 사용 하도록 설정 합니다.
Windows.System.Update | 새 [Windows.System.Update 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.system.update) 시스템 업데이트의 대화형 제어할 수 있습니다. 이 네임 스페이스는 Windows 10 IoT Core 사용할 수만 있습니다.

## <a name="web-development"></a>웹 개발

기능 | 설명
 :------ | :------
EdgeHTML 18 | Windows 10 년 10 월 2018 업데이트와 함께 제공 됩니다 [EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)가장 최신의 Microsoft Edge 브라우저에서 UWP 앱 용 JavaScript 엔진을 업데이트 합니다. EdgeHTML 18에는 웹 인증 API, 새로운 WebView 컨트롤 기능 등에 대 한 최신 및 확장 된 지원을 제공! 도구 우변 EdgeHTML 18 Edge DevTools 및 Edge DevTools 프로토콜에 새로운 WebDriver 기능 및 자동 업데이트 및 향상 된 기능 제공합니다. 체크 아웃 [EdgeHTML 18의 새로운 기능](https://docs.microsoft.com/microsoft-edge/dev-guide) 하 고 [DevTools 최신 Windows 10 업데이트 (EdgeHTML 18)의](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new) 세부 정보입니다.
점진적 웹앱 | Windows 10 JavaScript 앱 (웹 앱에서 실행 되는 *WWAHost.exe* 프로세스)는 이제 선택적인 지원 [백그라운드 응용 프로그램별 스크립트](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps) 모든 뷰는 활성화를 시작 및 기간 동안 실행 되는 과정입니다. 이 사용 하 여 모니터링 및 탐색을 수정, 탐색 간에 상태를 추적, 탐색 오류를 모니터링 하 고 수 뷰는 활성화 코드를 실행 합니다. 로 지정 된 경우는 [ `StartPage` ](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application) 에서 프로그램 [앱 매니페스트](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest), 새 인스턴스의 변수로 스크립트에 노출 되는 각 앱의 보기 (windows) [ `WebUIView` ](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview) (Win32) 일반적으로 동일한 이벤트, 속성 및 메서드를 제공 하는 클래스, [WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)합니다.
웹 API 확장 | 목록을 [레거시 Microsoft API 확장](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) 교차 브라우저 웹 개발에 대 한 Mozilla Developer Network 설명서에 추가 되었습니다. 이러한 API는 Internet Explorer 또는 Microsoft Edge에 고유한 확장과 MDN 웹 문서에서 호환성 및 브라우저 지원에 대 한 기존 정보를 보완 합니다. 레거시 Microsoft [CSS 확장](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) 하 고 [JavaScript 확장](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) 도 사용할 수 하 고 다양 한 웹 API 정보에서 MDN에 직접 표시를 찾을 수 [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)
WebVR | 에 대 한 주요 업데이트를 만들었습니다 합니다 [WebVR Developer's Guide](https://docs.microsoft.com/microsoft-edge/webvr/), 홈 페이지의 전체 다시 디자인 하는 등의 목차를 재구성 합니다. 또한 여러 새 항목을 포함 하 여 작성: </br> * [WebVR 란?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) WebVR 란, 왜 사용 해야, 및에 대 한 개발을 시작 하는 방법을 설명 합니다. </br> * [점진적 웹 앱에서 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): WebVR 점진적 웹 앱 (PWA)를 추가 하는 방법에 알아봅니다. </br> * [WebView의 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Windows 10 응용 프로그램에서 WebView 컨트롤 WebVR 추가 하는 방법에 알아봅니다. </br> * [WebVR 데모](https://docs.microsoft.com/microsoft-edge/webvr/demos): Microsoft Edge 및 Windows Mixed Reality 몰입 형 헤드셋을 사용 하 여 몇 가지 WebVR 데모를 확인 합니다.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

기능 | 설명
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview) 은 모든 Windows 앱에 최신 패키징 환경을 제공 하는 새로운 Windows 앱 패키지 형식입니다. 오픈 소스 MSIX 형식은 기존의 패키징 기능을 유지하는 동시에 최신 배포 기능을 구현합니다.
MSIX 패키징 도구 | 새 [MSIX 패키징 도구](https://docs.microsoft.com/windows/msix/mpt-overview)) 해당 소스 코드에 액세스할 수 없는 경우에 MSIX 형식의 기존 데스크톱 응용 프로그램을 다시 패키지 수 있습니다. 대화형 UI 또는 명령줄에서 실행할 수 있습니다.
MSIX Desktop App Converter 지원 | 사용할 수는 [Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) MSIX 패키지를 사용 하 여 출력에 `-MakeMSIX` 매개 변수입니다.
MSIX MakeAppx.exe 도구 지원 | MakeAppx.exe 도구를 사용 하 여 UWP 앱 또는 기존 데스크톱 응용 프로그램에 대 한 MSIX 패키지를 만들 수 있습니다. 이 도구는 Windows 10 SDK에 포함되어 있으며 명령 프롬프트 또는 스크립트 파일에서 사용할 수 있습니다. </br> UWP 앱에 대 한 참조 [MakeAppx.exe 도구를 사용 하 여 앱 패키지 만들기](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)합니다. </br> 데스크톱 응용 프로그램을 참조 하세요 [데스크톱 응용 프로그램을 수동으로 패키지](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)합니다.
패키지 지원 프레임 워크 | 합니다 [패키지 지원 프레임 워크](https://docs.microsoft.com/windows/msix/package-support-framework-overview) 가 하는 데 도움이 되는 오픈 소스 키트 수정 내용을 적용 하 여 기존 데스크톱 응용 프로그램 소스 코드에 액세스할 수 없을 때 MSIX 컨테이너에서 실행할 수 있도록 합니다.
저장소 분석 API | 합니다 [Microsoft Store 분석 API](../monetize/access-analytics-data-using-windows-store-services.md) 이제 다음의 새 메서드가 포함 되어 있습니다. </br> * [UWP 앱에 대 한 insights 데이터를 가져오기](../monetize/get-insights-data-for-your-app.md) </br> * [데스크톱 응용 프로그램에 대 한 insights 데이터를 가져오기](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [데스크톱 응용 프로그램에 대 한 업그레이드 요소 가져오기](../monetize/get-desktop-block-data.md) </br> * [데스크톱 응용 프로그램에 대 한 업그레이드 블록 세부 정보 가져오기](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>비디오

다음 동영상은 Fall Creators Update 이후 게시되었으며 개발자를 위한 Windows 10의 새로운 기능과 향상된 기능을 소개합니다.

### <a name="cwinrt"></a>C++/WinRT

C + + /cli WinRT는 작성 하 고 Windows 런타임 Api를 사용 하는 새로운 방법입니다. 헤더 파일 에서만 구현 되 고 최신 앱 기능에 대 한 최고 수준의 액세스를 제공 하도록 설계 되었습니다. [비디오를 시청](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) 원리, 다음에 대해 알아보려면 [개발자 문서 읽기](../cpp-and-winrt-apis/index.md) 자세한 정보에 대 한 합니다.

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>개발자를 위한 시작 합니다. 만들고 Windows 10에서 양식을 사용자 지정

우리의 [시작 문서](../get-started/index.md) Windows에 대 한 개발자가 이제 실습 환경이 제공 기본 앱 개발 작업을 사용 하 여 합니다. 이 비디오 이러한 항목 중 하나를 통해 안내 하 고 앱에서 폼 UI를 만드는 방법의 기초를 소개 합니다. [비디오를 시청](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) 작업을 수행한 다음의 코드를 보려면 [항목 직접 확인 합니다.](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>프로젝트 개인 정보 채팅을 사용 하 여 봇을 향상합니다

프로젝트 개인 정보 채팅 채팅 봇을를 사용자 지정할 수 있는 가상 사용자를 추가할 수 있습니다. Microsoft Bot Framework SDK와 통합 하 여 고객 상호 작용 하는 좀 더 대화형 방법에 대 한 작은 강연 기능을 추가할 수 있습니다. [비디오를 시청](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) 다음을 구현 하는 방법을 알아보려면 [대화형 데모를 사용해](https://aka.ms/PersonalityChat) 실습 환경에 대 한 합니다.

### <a name="multi-instance-uwp-apps"></a>다중 인스턴스 UWP 앱

이제 Windows을 사용 하면 각각 별도 자체 프로세스를 사용 하 여 UWP 앱의 여러 인스턴스를 실행할 수 있습니다. [비디오를 시청](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) 한 후이 기능을 지 원하는 새 앱을 만드는 방법에 알아보려면 [개발자 문서 읽기](../launch-resume/multi-instance-uwp.md) 하는 방법에 대 한 자세한 지침 및이 기능을 사용 하는 이유.

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 플러그 인

Unity에 대 한 Xbox Live 플러그 인을 제목에 서명 Xbox Live, 통계, 친구 목록, 클라우드 저장소 및 순위표를 추가 하는 것에 대 한 지원을 포함 합니다. [비디오를 시청](https://youtu.be/fVQZ-YgwNpY) 다음 자세히 알아보려면 [GitHub 패키지 다운로드](https://aka.ms/UnityPlugin) 시작 하려면.

### <a name="one-dev-question"></a>개발 질문 중 하나

개발 질문 중 하나는 비디오 시리즈에서는 해온 Microsoft 개발자에 게 일련의 Windows 개발, 팀 문화권 및 기록 하는 방법에 대 한 질문을 다룹니다.

* [Raymond chen이 Windows 개발에 기록](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Windows 개발 기록에 대 한 Larry Osterman](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [점진적 웹 앱에서 Aaron Gustafson](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann webhint 도구](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>샘플

### <a name="customer-orders-database"></a>고객 주문 데이터베이스

합니다 [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 와 같은 새 컨트롤을 사용 하도록 업데이트 되었습니다 [DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid)를 [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), 및 [확장기](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)합니다.

### <a name="customer-database-tutorial"></a>고객 데이터베이스 자습서

합니다 [고객 데이터베이스 자습서](../enterprise/customer-database-tutorial.md) 고객 목록을 관리 하기 위한 기본 UWP 앱을 만들고 개념 및 엔터프라이즈 개발에 유용한 사례를 소개 합니다. UI 요소를 구현 하 고 로컬 SQLite 데이터베이스에 대해 작업 추가 안내 하 고 진행 하려는 경우 원격 나머지 데이터베이스에 연결 하기 위한 느슨한 지침을 제공 합니다.

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + /cli WinRT

[Photo Editor 샘플 앱](https://github.com/Microsoft/Windows-appsample-photo-editor) 를 사용한 개발을 소개 합니다 [C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 언어 프로젝션 합니다. 앱에서 사진을 검색할 수는 **사진을** 라이브러리와 연결 된 사진이 효과 사용 하 여 이미지를 선택한 다음 편집 합니다.

### <a name="windows-machine-learning"></a>Windows Machine Learning

[Windows Machine Learning](https://github.com/Microsoft/Windows-Machine-Learning) 리포지토리의 최신 Windows 10 SDK와 함께 작동 하도록 업데이트 되었습니다 및 작성 하는 샘플이 포함 되어 있습니다 C#, c + + 및 JavaScript입니다.

### <a name="xaml-hosting-api"></a>XAML 호스팅 API

합니다 [XAML 호스팅 API 샘플](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) Win32 데스크톱 앱 (XAML 제도 라고도 함) API를 호스트 하는 UWP XAML을 사용 하 여 다양 한 시나리오를 강조 표시 하는 합니다. 프로젝트는 갤러리 스타일 프레젠테이션에서 Windows 잉크, Media Player 및 탐색 보기 컨트롤을 통합합니다. 일반 외부 컨트롤 사용에 대해서도 설명 XAML 및 네이티브 Windows 이벤트/메시지, 기본 XAML 데이터 바인딩을 처리 합니다.
