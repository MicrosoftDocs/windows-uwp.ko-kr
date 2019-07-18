---
title: 개발자, 도구 및 기능을 위한 Windows 10의 새로운 기능
description: Windows 10 빌드 17763 및 새로운 개발자 도구는 유니버설 Windows 플랫폼에서 지원되는 도구, 기능 및 환경을 제공합니다.
keywords: 새로운 기능, 새로운 기능, 업데이트, 업데이트, 기능, 새 항목, Windows 10, 최신, 개발자, 17763
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d393ee92be8768861da0fb0783372c8bafc6f815
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67821117"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>개발자용 Windows 10 빌드 17763의 새로운 기능

Visual Studio 2019 및 업데이트된 SDK와 함께 Windows 10 빌드 17763(2018년 10월 업데이트 또는 버전 1809라고도 함)은 놀라운 유니버설 Windows 플랫폼 앱을 만드는 도구, 기능 및 환경을 제공합니다. Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

이는 Windows 개발자가 관심을 갖는 이 릴리스의 새로운 기능, 향상된 기능 및 지침의 모음입니다. Windows SDK에 추가된 새로운 네임스페이스의 전체 목록은 [Windows 10 빌드 17763 API 변경 내용](windows-10-build-17763-api-diff.md)을 참조하세요. Windows 10의 중요한 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](https://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요. 그리고 이전에 있었던 그리고 앞으로 있을 Windows 플랫폼 기능 추가에 대한 개략적인 내용은 [Windows 개발자 플랫폼 기능](https://developer.microsoft.com/windows/platform/features)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

기능 | 설명
 :------ | :------
앱 아이콘 및 로고 | [앱 아이콘 및 로고 페이지](../design/style/app-icons-and-logos.md)가 다시 작성되었으며, 이제 최신 Visual Studio 아이콘 도구를 표시하고, Microsoft Store의 앱 목록에 이미지를 추가하는 방법에 대한 정보를 제공합니다.
디자인 방문 페이지 | [업데이트된 디자인 방문 페이지](https://developer.microsoft.com/windows/apps/design)에는 UWP 디자인 영역에 대한 한 눈에 보기 개요와 Fluent 디자인의 최신 기능에 대한 정보가 있습니다.
Fluent 디자인 컨트롤 | Fluent 디자인 시스템 및 앱의 모양을 향상시키기 위해 추가된 새 UI 컨트롤은 다음과 같습니다. </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md)을 사용하면 UI 캔버스에 있는 항목의 컨텍스트에서 일반적인 사용자 작업을 표시할 수 있습니다. </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) 및 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)은 앱의 사용자 인터페이스를 향상시키는 특수 기능을 갖춘 단추 컨트롤을 제공합니다. </br> * [MenuBar](../design/controls-and-patterns/menus.md)는 수평 행에 여러 개의 최상위 메뉴 세트를 표시합니다. </br> * [NavigationView](../design/controls-and-patterns/navigationview.md)는 이제 앱의 탐색 옵션 수가 적고 콘텐츠에 더 많은 공간이 필요한 경우를 위해 위쪽 탐색 모음을 지원합니다. </br> * [TreeView](../design/controls-and-patterns/tree-view.md)는 데이터 바인딩, 항목 템플릿 및 끌어서 놓기를 지원하도록 향상되었습니다.
Fluent 디자인 업데이트 | 시각적으로 업데이트되고 약간 변경된 Fluent 디자인 페이지는 다음과 같습니다. </br> * [맞춤, 안쪽 여백, 여백](../design/layout/alignment-margin-padding.md) </br> * [색](../design/style/color.md) </br> * [Windows 앱용 Fluent 업데이트](../design/fluent-design-system/index.md) </br> * [앱 디자인 소개](../design/basics/design-and-ui-intro.md) </br> * [탐색 기본 사항](../design/basics/navigation-basics.md) </br> * [반응형 디자인 기술](../design/layout/responsive-design.md) </br> * [화면 크기 및 중단점](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [스타일 개요](../design/style/index.md) </br> * [쓰기 스타일](../design/style/writing-style.md) </br> 콘텐츠 영역에 대한 새 정보를 모두 추가하여 다시 작성된 페이지는 다음과 같습니다. </br> * [아이콘](../design/style/icons.md)은 이제 아이콘을 사용하고 클릭할 수 있도록 하는 실질적인 추천 사항을 제공합니다. </br> * [입력 체계](../design/style/typography.md)는 비슷한 문서의 정보를 통합하여 모든 것을 업데이트된 지침 및 그림과 함께 한 곳에 배치합니다.
응시 입력 및 상호 작용 | [응시 상호 작용](../design/input/gaze-interactions.md)을 사용하면 앱에서 눈의 위치와 움직임에 따라 사용자의 시선, 주의 및 존재를 추적할 수 있습니다. 이 기능은 보조 기술로 사용할 수 있으며, 기존의 입력 디바이스를 사용할 수 없는 게임 및 기타 대화형 시나리오에 대한 기회를 제공합니다.
필기 보기 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md)는 TextBox 및 RichEditBox의 새 수동 입력 표면입니다. 사용자는 펜으로 텍스트 컨트롤을 탭하여 컨트롤을 쓰기 표면으로 확장할 수 있습니다. 이 지침에서는 애플리케이션에서 HandwritingView를 관리하고 사용자 지정하는 방법을 설명합니다.
Fluent 디자인의 움직임 | Fluent 디자인 시스템에서 움직임을 사용하는 것은 타이밍, 감속/가속, 방향 및 무게의 기본 사항을 기반으로 하여 진화하고 있습니다. 이러한 기본 사항을 적용하면 앱을 통해 사용자를 안내하고 자연계를 반영하여 디지털 환경과 연결할 수 있습니다. 다음 문서에서 자세히 알아보세요. </br> * [움직임 개요](../design/motion/index.md)는 이러한 기본 사항을 반영하도록 업데이트되었습니다. </br> * [실제 움직임](../design/motion/motion-in-practice.md)은 앱 내에서 이러한 기본 사항을 적용하는 방법의 예를 보여 줍니다. 또한 암시적 애니메이션에 대한 정보가 포함되어 있어 XAML 요소의 속성이 변경되는 경우 이전 값과 새 값 사이를 쉽게 보간할 수 있습니다. </br> * [방향 및 무게](../design/motion/directionality-and-gravity.md)는 앱의 사용자 정신적 모델을 강화합니다. </br> * [타이밍 및 감속](../design/motion/timing-and-easing.md)은 앱의 움직임에 현실성을 추가합니다. </br> * [XAML 속성 애니메이션](../design/motion/xaml-property-animations.md)을 사용하면 기본 합성 Visual 개체와 상호 작용하지 않고도 XAML 요소의 속성 애니메이션을 직접 적용할 수 있습니다.
페이지 전환 | [페이지 전환](../design/motion/page-transitions.md)은 사용자를 앱의 페이지 간에 이동시킵니다. 사용자가 탐색 계층 구조상의 위치를 파악하고 페이지 간의 관계에 대한 피드백을 제공하는 데 도움이 됩니다.
텍스트 크기 조정 | 새 [텍스트 크기 조정 지침](../design/input/text-scaling.md)에서는 사용자가 OS와 개별 애플리케이션 모두에서 상대 글꼴 크기를 변경할 수 있는 기능을 제공하는 새 텍스트 크기 조정 동작을 수용하도록 애플리케이션을 업데이트하는 방법을 설명합니다. 일반적으로 화면 영역 내의 모든 항목을 확대하고 자체의 유용성 문제를 제기하는 돋보기 앱을 사용하거나, 디스플레이 해상도를 변경하거나, 디스플레이 및 일반 시청 거리에 따라 모든 항목의 크기를 조정하는 DPI 배율을 사용하는 대신, 사용자가 설정에 빠르게 액세스하여 텍스트 크기만 100%(기본 크기)에서 최대 225%까지 조정할 수 있습니다.
도구 키트 | [Adobe XD 및 Adobe Illustrator 도구 키트](../design/downloads/index.md)는 새로운 기능으로 업데이트되었습니다. 이러한 디자인 도구 키트는 UWP 앱 디자인을 위한 레이아웃 템플릿을 제공합니다.
UI 명령 | [UWP 명령 인프라](../design/basics/commanding-basics.md)에 대한 업데이트에는 명령 개체(행동, 레이블, 아이콘, 키보드 액셀러레이터 키, 액세스 키 및 설명)와 일반적인 표준 명령 세트(잘라내기, 복사, 붙여넣기, 종료 등)의 더 효율적인 캡슐화가 포함되어 있으므로 이러한 속성을 수동으로 설정할 필요가 없습니다. </br> 새 [XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스는 호출 시 작업을 수행하는 대화형 UI 요소의 명령 동작을 정의하는 기본 클래스를 제공합니다. 이 클래스는 미리 정의된 속성을 사용하여 표준 플랫폼 명령 세트를 공개하는 [StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand)의 부모 클래스입니다. 
Windows UI Library | [Windows UI Library](https://aka.ms/winui-docs)는 UWP 앱용 컨트롤 및 기타 사용자 인터페이스 요소를 제공하는 NuGet 패키지 세트입니다. 또한 이러한 패키지는 이전 버전의 Windows 10과도 호환되므로 사용자에게 최신 OS 버전이 없는 경우에도 앱이 작동합니다. </br> Windows UI 라이브러리의 새로운 기능에 대한 자세한 내용은 [NuGet 패키지에 포함된 API 네임스페이스 목록](https://docs.microsoft.com/uwp/api/overview/winui/)을 참조하세요.

## <a name="develop-windows-apps"></a>Windows 앱 개발

기능 | 설명
 :------ | :------
바코드 스캐너 | [바코드 스캐너](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner) 설명서가 다시 구성되었으며 더 자세한 정보와 코드 조각으로 향상되었습니다. 또한 바코드 스캐너에서 데이터를 가져오고 사용하는 방법을 설명하는 새 [바코드 데이터 획득 및 이해](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data) 항목이 추가되었습니다.
C++/WinRT | [C++/WinRT](https://aka.ms/cppwinrt)에는 이 릴리스에 대한 많은 새로운 기능, 변경 내용 및 수정 프로그램이 포함되었습니다. 사용자 고유의 [컬렉션 속성 및 컬렉션 형식](/windows/uwp/cpp-and-winrt-apis/collections)을 구현할 때 새 함수 및 기본 클래스가 지원되며, 이제 C++/WinRT 런타임 클래스에서 [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 태그 확장을 사용할 수 있습니다(코드 예제는 [데이터 바인딩 개요](/windows/uwp/data-binding/data-binding-quickstart) 참조). 이 릴리스에서 새로 추가되거나 변경된 모든 내용에 대한 자세한 설명은 [C++/WinRT의 새로운 기능](../cpp-and-winrt-apis/news.md)을 참조하세요.</br></br>다른 새 C++/WinRT 콘텐츠로 [XAML 사용자 지정 컨트롤](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl), [작성자 COM 구성 요소](/windows/uwp/cpp-and-winrt-apis/author-coclasses), [값 범주](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories), [강력한 참조 및 약한 참조](../cpp-and-winrt-apis/weak-references.md)가 포함되었습니다.
C++/WinRT 코드 예제 | 기존 C++/CX 코드 예제와 함께 설명서의 항목에 250개의 C++/WinRT 코드 목록이 추가되었습니다.
영향을 주는 지침 | UWP 설명서에 대한 [영향을 주는 지침](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)이 업데이트되었습니다. 이 새 지침은 Microsoft 문서에 대한 외부 기여와 관련된 작업 흐름과 기대치를 명확히 합니다.
DXGI(DirectX 그래픽 인프라) | 누락된 DXGI API에 대한 새 설명서가 추가되었으며, Windows 10에서 발표할 때 모범 사례에 대한 문서가 제공되었습니다. </br> * [최상의 성능을 위해 DXGI 대칭 이동 모델 사용](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model): 최신 버전의 Windows에서 프레젠테이션 스택의 성능과 효율성을 극대화하는 방법에 대한 지침을 제공합니다. </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 메서드](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport): 애플리케이션에 하드웨어 확대가 지원됨을 알립니다. </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 열거형](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags): 지원되는 하드웨어 구성 수준을 설명합니다.
시작 | [시작](../get-started/index.md) 콘텐츠가 새 항목으로 활성화되어 Windows 10을 처음 사용하는 개발자가 다음과 같은 일반적인 작업을 수행하는 방법에 대한 정보와 지침을 제공합니다. </br> * [양식 작성](../get-started/construct-form-learning-track.md) </br> * [목록에 고객 표시](../get-started/display-customers-in-list-learning-track.md) </br> * [설정 저장 및 로드](../get-started/settings-learning-track.md) </br> * [파일 작업](../get-started/fileio-learning-track.md)
맵 스타일시트 편집기 | 새 [맵 스타일시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab) 애플리케이션을 사용하여 애플리케이션에 추가하는 맵의 모양을 대화형으로 사용자 지정합니다.
Microsoft Learn | 새 [Microsoft Learn 사이트](https://www.microsoft.com/learning/default.aspx)는 Microsoft 개발자에게 새 실습 학습 및 교육 기회를 제공합니다. Microsoft Learn은 현재 Microsoft 365, Microsoft Azure, Office 365 및 Windows Server에 대한 학습 및 인증을 제공합니다.
Windows 메모장 | [메모장이 업데이트](https://aka.ms/ant-man)되어 확대/축소, 줄 바꿈 찾기/바꾸기, Unix/Linux(LF) 및 Mac(CR) 줄 끝 지원이 추가되었습니다.
프로젝트 로마 | [프로젝트 로마](https://docs.microsoft.com/windows/project-rome/)는 이제 지원되는 플랫폼과 SDK에서 일관된 프로그래밍 환경을 제공합니다. </br>  새 [Microsoft Graph 알림](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview)에서 프로젝트 로마를 사용하여 앱을 위한 사용자 중심의 플랫폼 간 알림 플랫폼을 제공합니다.
화면 캡처 | 새 [URI 체계](../launch-resume/launch-screen-snipping.md)를 사용하면 앱에서 프로그래밍 방식으로 새 캡처를 열거나 주석에 대한 특정 이미지를 사용하여 캡처 및 스케치 앱을 시작할 수 있습니다.
데스크톱 애플리케이션의 UWP 컨트롤 | 이제 Windows 10을 사용 하면 WPF, Windows Forms 및 C++ Win32 데스크톱 애플리케이션에서 UWP 컨트롤을 사용할 수 있습니다. 즉, Windows Ink 및 Fluent 디자인 시스템을 지원하는 컨트롤과 같은 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 통해 기존 데스크톱 애플리케이션의 모양, 느낌 및 기능을 향상시킬 수 있습니다. 이 기능을 *XAML 제도*라고 합니다. </br> 사용하는 애플리케이션 플랫폼에 따라 애플리케이션에서 XAML 제도를 사용할 수 있는 몇 가지 방법을 제공합니다. WPF 및 Windows Forms 애플리케이션은 디자이너 중심 개발 환경을 제공하는 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)의 컨트롤 세트를 사용할 수 있습니다. C++ Win32 애플리케이션은 [Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) 네임스페이스의 *UWP XAML 호스팅 API*를 사용해야 합니다. 자세한 내용은 [데스크톱 애플리케이션의 UWP 컨트롤](../xaml-platform/xaml-host-controls.md)을 참조하세요. </br> **참고:** XAML 제도를 사용하도록 설정하는 API 및 컨트롤은 현재 개발자 미리 보기로 사용할 수 있습니다. 사용자 고유의 프로토타입 코드에서 사용해 보도록 추천하지만, 프로덕션 코드에는 지금 사용하지 않는 것이 좋습니다.
Windows Machine Learning | [Windows Machine Learning](https://docs.microsoft.com/windows/ai/)은 이제 공식적으로 시작되어 최첨단 기계 학습 모델에 대한 더 빠른 평가 및 지원과 같은 기능을 제공합니다. 애플리케이션과 통합하려는 개발자를 지원하기 위해 다음과 같은 몇 가지 새롭고 업데이트된 리소스를 사용하여 새 설명서 사이트를 만들었습니다. </br> * [자습서: Windows Machine Learning 데스크톱 애플리케이션 만들기(C++)](https://docs.microsoft.com/windows/ai/get-started-desktop): 이 자습서에서는 간단한 데스크톱용 Windows ML 애플리케이션을 빌드하는 방법을 보여 줍니다. </br> * [자습서: Windows Machine Learning UWP 애플리케이션 만들기(C#)](https://docs.microsoft.com/windows/ai/get-started-uwp): 이 단계별 자습서에서는 Windows ML을 사용하여 첫 번째 UWP 애플리케이션을 만듭니다. </br> * [Windows.AI.MachineLearning 네임스페이스](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning): API 참조가 Windows 10 SDK의 최신 릴리스에 맞게 업데이트되어 개발자는 이제 이 API를 Win32 및 UWP 애플리케이션 모두에 사용할 수 있습니다.
Windows Mixed Reality | 개발자는 이제 디스플레이 하드웨어에서 지원되는 경우 하드웨어 보호 백 버퍼 텍스처를 요청할 수 있으므로 애플리케이션에서 PlayReady와 같은 원본에서 하드웨어로 보호된 콘텐츠를 사용할 수 있습니다. 하드웨어 보호 지원 및 설정은 기본 계층에서 [Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)의 새 속성을 통해 사용하고, Quad 계층에서는 [Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)를 통해 사용할 수 있습니다.

## <a name="iot-core"></a>IoT Core

기능 | 설명
 :------ | :------
AssignedAccessSettings | [AssignedAccessSettings 클래스](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings)를 사용하면 다양한 메서드와 속성을 호출하여 사용자가 특정 디바이스에 대해 할당한 액세스 설정에 액세스할 수 있습니다.
기본 앱 개요 | [Windows 10 IoT Core 기본 앱](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp)이 날씨, 수동 입력 및 오디오와 같은 새로운 기능과 성능으로 업데이트되었습니다.
대시보드 | [Windows 10 Iot Core 대시보드](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup)를 사용하면 Dragonboard 410C 또는 NXP를 사용하는 개발자가 사용자 지정 FFU를 디바이스에 플래시할 수 있습니다.
화상 키보드 | [IoT 디바이스용 화상 키보드](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard)는 이제 Windows의 데스크톱 버전과 동일한 터치 키보드 구성 요소를 사용합니다. 이렇게 하면 받아쓰기 모드, IME 지원 및 전체 입력 범위 세트와 같은 기능을 사용할 수 있습니다.
로그인 대화 상자의 제목 표시줄 | Windows 10 IoT Core는 이제 [시스템 대화 상자의 제목 표시줄](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)을 구성하는 옵션을 제공합니다.
Wake on Touch | [Wake on Touch](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch)는 사용하지 않는 동안 디바이스의 화면을 끄고, 사용자가 화면을 터치하면 빠르게 켜는 기능을 사용하도록 설정합니다.
Windows.System.Update | 새 [Windows.System.Update 네임스페이스](https://docs.microsoft.com/uwp/api/windows.system.update)를 사용하면 시스템 업데이트를 대화형으로 제어할 수 있습니다. 이 네임스페이스는 Windows 10 IoT Core에서만 사용할 수 있습니다.

## <a name="web-development"></a>웹 개발

기능 | 설명
 :------ | :------
EdgeHTML 18 | Windows 10 2018년 10월 업데이트에는 Microsoft Edge 브라우저 및 UWP 앱용 JavaScript 엔진에 대한 최신 업데이트인 [EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)이 제공됩니다. EdgeHTML 18은 웹 인증 API, 새 WebView 컨트롤 기능 등으로 확장된 최신 지원을 제공합니다! 도구 측면에서 EdgeHTML 18은 새 WebDriver 기능, 자동 업데이트, 향상된 Edge DevTools 및 Edge DevTools 프로토콜 기능을 제공합니다. 자세한 내용은 [EdgeHTML 18의 새로운 기능](https://docs.microsoft.com/microsoft-edge/dev-guide) 및 [최신 Windows 10 업데이트의 DevTools(EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new)를 확인하세요.
점진적 웹앱 | Windows 10 JavaScript 앱(*WWAHost.exe* 프로세스에서 실행되는 웹앱)은 이제 보기가 활성화되기 전에 시작하여 프로세스 기간 동안 실행되는 선택적인 [애플리케이션별 백그라운드 스크립트](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps)를 지원합니다. 이렇게 하면 보기를 활성화하기 전에 탐색을 모니터링 및 수정하고, 탐색 전체의 상태를 추적하고, 탐색 오류를 모니터링하고, 코드를 실행할 수 있습니다. [앱 매니페스트](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)에서 [`StartPage`](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application)로 지정되면 앱의 각 보기(창)가 새 [`WebUIView`](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview) 클래스의 인스턴스로 스크립트에 공개되어 일반(Win32) [WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)와 동일한 이벤트, 속성 및 메서드를 제공합니다.
웹 API 확장 | 브라우저 간 웹 개발용 [레거시 Microsoft API 확장](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) 목록이 Mozilla Developer Network 설명서에 추가되었습니다. 이러한 API 확장은 Internet Explorer 또는 Microsoft Edge 고유의 확장이며, MDN 웹 문서의 호환성 및 브라우저 지원에 대한 기존 정보를 보완합니다. 레거시 Microsoft [CSS 확장](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) 및 [JavaScript 확장](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)도 사용할 수 있으며, [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)에서 표시되는 MDN의 다양한 웹 API 정보를 직접 확인할 수 있습니다.
WebVR | 홈 페이지를 완전히 다시 디자인하고 목차를 다시 구성하는 기능을 포함하여 [WebVR 개발자 가이드](https://docs.microsoft.com/microsoft-edge/webvr/)에 대한 주요 업데이트가 수행되었습니다. 또한 다음과 같은 몇 가지 새 항목이 작성되었습니다. </br> * [WebVR이란?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) WebVR의 기본 사항, 사용해야 하는 이유 및 개발을 시작하는 방법을 설명합니다. </br> * [점진적 웹앱의 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): PWA(점진적 웹앱)에 WebVR을 추가하는 방법을 알아봅니다. </br> * [WebView의 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Windows 10 애플리케이션에서 WebVR을 WebView 컨트롤에 추가하는 방법을 알아봅니다. </br> * [WebVR 데모](https://docs.microsoft.com/microsoft-edge/webvr/demos): Microsoft Edge 및 Windows Mixed Reality 몰입형 헤드셋을 사용하여 일부 WebVR 데모를 확인합니다.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

기능 | 설명
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview)는 모든 Windows 앱에 최신 패키징 환경을 제공하는 새 Windows 앱 패키징 형식입니다. 오픈 소스 MSIX 형식은 기존의 패키징 기능을 유지하는 동시에 최신 배포 기능을 구현합니다.
MSIX Packaging Tool | 새 [MSIX 패키징 도구](https://docs.microsoft.com/windows/msix/mpt-overview)를 사용하면 소스 코드에 액세스할 수 없는 경우에도 기존 데스크톱 애플리케이션을 MSIX 형식으로 다시 패키지할 수 있습니다. 명령줄 또는 대화형 UI를 통해 실행할 수 있습니다.
MSIX에 Desktop App Converter 지원 | [Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)를 사용하여 `-MakeMSIX` 매개 변수를 통해 MSIX 패키지를 출력할 수 있습니다.
MSIX에 MakeAppx.exe 도구 지원 | MakeAppx.exe 도구를 사용하여 UWP 앱 또는 기존 데스크톱 애플리케이션용 MSIX 패키지를 만들 수 있습니다. 이 도구는 Windows 10 SDK에 포함되어 있으며 명령 프롬프트 또는 스크립트 파일에서 사용할 수 있습니다. </br> UWP 앱의 경우 [MakeAppx.exe 도구를 사용하여 앱 패키지 만들기](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)를 참조하세요. </br> 데스크톱 애플리케이션의 경우 [수동 데스크톱 애플리케이션 패키징](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)을 참조하세요.
패키지 지원 프레임워크 | [패키지 지원 프레임워크](https://docs.microsoft.com/windows/msix/package-support-framework-overview)는 소스 코드에 액세스할 수 없는 경우 기존 데스크톱 애플리케이션에 수정 프로그램을 적용하여 MSIX 컨테이너에서 실행할 수 있도록 지원하는 오픈 소스 키트입니다.
Store 분석 API | [Microsoft Store 분석 API](../monetize/access-analytics-data-using-windows-store-services.md)에는 이제 다음과 같은 새 메서드가 포함되어 있습니다. </br> * [UWP 앱에 대한 인사이트 데이터 가져오기](../monetize/get-insights-data-for-your-app.md) </br> * [데스크톱 애플리케이션에 대한 인사이트 데이터 가져오기](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [데스크톱 애플리케이션에 대한 업그레이드 차단 가져오기](../monetize/get-desktop-block-data.md) </br> * [데스크톱 애플리케이션에 대한 업그레이드 차단 세부 정보 가져오기](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>비디오

다음 비디오는 Fall Creators Update 이후 게시되었으며, 개발자를 위한 Windows 10의 새로운 기능과 향상된 기능을 강조하고 있습니다.

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT는 Windows 런타임 API를 작성하고 사용하는 새로운 방법입니다. 이는 헤더 파일에서만 구현되고, 최신 앱 기능에 대한 최고 수준의 액세스를 제공하도록 설계되었습니다. 이 [비디오를 시청](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)하여 작동 방식을 알아본 다음, [개발자 문서](../cpp-and-winrt-apis/index.md)에서 자세한 내용을 참조하세요.

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>개발자를 위한 시작: Windows 10에서 양식 만들기 및 사용자 지정

이제 Windows 개발자를 위한 [시작 문서](../get-started/index.md)에서 기본 앱 개발 작업용 실습 환경을 제공합니다. 이 비디오에서는 이러한 항목 중 하나를 안내하고, 앱에서 양식 UI를 만드는 방법에 대한 기본 사항을 설명합니다. 이 [비디오를 시청](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)하여 작동하는 코드를 살펴본 다음, [해당 항목을 직접 확인해 보세요.](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Project Personality Chat을 사용하여 봇 향상

Project Personality Chat을 사용하면 사용자 지정 가능한 가상 사용자를 채팅 봇에 추가할 수 있습니다. Microsoft Bot Framework SDK와 통합하면 고객과 상호 작용하는 더 향상된 대화형 방법에 맞게 작은 대화 기능을 추가할 수 있습니다. 이 [비디오를 시청](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)하여 구현하는 방법을 알아본 다음, 실습 환경용 [대화형 데모를 사용해 보세요](https://aka.ms/PersonalityChat).

### <a name="multi-instance-uwp-apps"></a>다중 인스턴스 UWP 앱

Windows에서는 이제 UWP 앱의 여러 인스턴스를 각각 별도의 자체 프로세스로 실행할 수 있습니다. 이 [비디오를 시청](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)하여 이 기능을 지원하는 새 앱을 만드는 방법을 알아본 다음, [개발자 문서](../launch-resume/multi-instance-uwp.md)에서 이 기능을 사용하는 방법과 이유에 대한 자세한 지침을 참조하세요.

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 플러그 인

Unity용 Xbox Live 플러그 인에는 Xbox Live 서명, 통계, 친구 목록, 클라우드 스토리지 및 순위표를 제목에 추가하기 위한 지원이 포함되어 있습니다. 이 [비디오를 시청](https://youtu.be/fVQZ-YgwNpY)하여 자세히 알아본 다음, [GitHub 패키지를 다운로드](https://aka.ms/UnityPlugin)하여 시작하세요.

### <a name="one-dev-question"></a>One Dev Question

Microsoft 개발자는 One Dev Question 동영상 시리즈에서 오랫동안 Windows 개발, 팀 문화 및 이력에 대한 일련의 질문을 다루어 왔습니다.

* [Raymond Chen, Windows 개발 및 역사](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Ostman, Windows 개발 및 역사](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson, 점진적 웹앱](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann, 웹 힌트 도구](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>샘플

### <a name="customer-orders-database"></a>고객 주문 데이터베이스

[고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)이 [DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid), [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [Expander](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)와 같은 새 컨트롤을 사용하도록 업데이트되었습니다.

### <a name="customer-database-tutorial"></a>고객 데이터베이스 자습서

[고객 데이터베이스 자습서](../enterprise/customer-database-tutorial.md)는 고객 목록을 관리하기 위한 기본 UWP 앱을 만들고, 엔터프라이즈 개발에 유용한 개념과 사례를 소개합니다. UI 요소를 구현하고 로컬 SQLite 데이터베이스에 대한 작업을 추가하는 방법을 안내하고, 추가적으로 원하는 경우 원격 REST 데이터베이스에 연결하기 위한 대략적인 지침을 제공합니다.

### <a name="photo-editor-cwinrt"></a>Photo Editor C++/WinRT

[Photo Editor 샘플 앱](https://github.com/Microsoft/Windows-appsample-photo-editor)은 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 언어 프로젝션을 사용한 개발을 보여 줍니다. 이 앱을 사용하면 **사진** 라이브러리에서 사진을 검색한 다음, 관련 사진 효과를 사용하여 선택한 이미지를 편집할 수 있습니다.

### <a name="windows-machine-learning"></a>Windows Machine Learning

[Windows-Machine-Learning](https://github.com/Microsoft/Windows-Machine-Learning) 리포지토리가 최신 Windows 10 SDK에서 작동하도록 업데이트되었으며, C#, C++ 및 JavaScript로 작성된 샘플이 포함되어 있습니다.

### <a name="xaml-hosting-api"></a>XAML 호스팅 API

[XAML 호스팅 API 샘플](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)은 UWP XAML 호스팅 API(XAML 제도라고도 함)를 사용하는 다양한 시나리오를 강조하는 Win32 데스크톱 앱입니다. 이 프로젝트는 Windows Ink, Media Player 및 탐색 보기 컨트롤을 갤러리 스타일 프레젠테이션에 통합합니다. 샘플에서는 일반 컨트롤 사용 외에도 XAML 및 네이티브 Windows 이벤트/메시지 처리 및 기본 XAML 데이터 바인딩도 보여 줍니다.
