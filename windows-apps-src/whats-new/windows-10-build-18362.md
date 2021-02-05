---
title: Windows 10 빌드 18362의 새로운 기능
description: Windows 10 빌드 18362 및 새로운 개발자 도구는 유니버설 Windows 플랫폼이 지원되는 도구, 기능 및 환경을 제공합니다.
keywords: Windows 10, 18362, 1903
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 3db6ce85e81f4478a9776cec6a4e38caee0a2c9c
ms.sourcegitcommit: a7d49538d7ad762d34d41579fdfad2fb4422d667
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061420"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>개발자용 Windows 10 빌드 18362의 새로운 기능

Visual Studio 2019와 Windows 10 빌드 18362(SDK 버전 1903이라고 함)를 함께 사용하면 Windows 앱을 주목할 만하게 만들 수 있는 도구, 기능 및 환경이 제공됩니다. Windows 10에 [도구 및 SDK를 설치](https://developer.microsoft.com/windows/downloads#_blank)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 살펴볼 수 있습니다.

다음은 이 릴리스에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. Windows SDK에 추가된 새 네임스페이스의 전체 목록은 [Windows 10 빌드 18362 API 변경 내용](windows-10-build-18362-api-diff.md)을 참조하세요. Windows 10의 주요 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](https://developer.microsoft.com/windows/windows-10-for-developers)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

기능 | 설명
:------ | :------
AnimatedVisualPlayer | [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API는 앱에서 애니메이트된 시각적 개체의 재생을 호스트하고 제어합니다. 이 API는 [Lottie](/windows/communitytoolkit/animations/lottie) 시각적 개체와 같이 Adobe AfterEffects 애니메이션을 애플리케이션에서 고유하게 렌더링할 수 있는 콘텐츠를 제어 및 표시하는 데 사용합니다.
CompactDensity | 앱에서 [압축 모드](../design/style/spacing.md)를 사용하도록 설정하면 풍부한 정보를 제공하는 조밀한 컨트롤 그룹이 구현될 수 있습니다. 이를 통해 많은 양의 콘텐츠를 검색할 때 페이지에서 시각적 컨텐츠가 극대화되거나 사용자가 포인터 입력을 사용할 때 탐색 및 상호 작용이 용이해집니다.
항목 반복기 | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md) 컨트롤은 사용자에게 컬렉션을 표시하는 사용자 지정 환경을 만들 수 있습니다. ItemsRepeater는 포괄적인 최종 사용자 환경 또는 기본 UI를 제공하지 않습니다. 그보다는 고유한 컬렉션 기반 환경 및 사용자 지정 컨트롤을 만드는 데 사용할 수 있는 문서 블록에 해당합니다.
교육 팁 | [교육 팁](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)은 상황별 정보를 제공하는 풍부한 콘텐츠의 반영구적 플라이아웃입니다. 이 컨트롤은 새롭거나 중요한 정보를 사용자에게 알리고 교육하는 데 사용할 수 있습니다.
UI 명령 | [UWP 앱의 명령](../design/controls-and-patterns/commanding.md)에서는 [XamlUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) 및 [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스를 ICommand 인터페이스와 함께 사용하여 사용하는 디바이스 및 입력 형식에 관계없이 다양한 컨트롤 형식에서 명령을 공유하고 관리할 수 있습니다.
Windows UI Library | Windows UI 라이브러리 - [WinUI 2.1](/uwp/toolkits/winui/release-notes/winui-2.1)의 최신 공신 버전은 Windows 앱을 위한 유용한 새 XAML 컨트롤을 제공합니다. WinUI 라이브러리 API는 이전 버전의 Windows 10에서 실행되므로 최신 OS를 사용하지 않는 사용자를 지원하기 위해 버전 확인 또는 조건부 XAML을 포함할 필요가 없습니다.
데스크톱 앱의 시각적 계층 | 이제 [데스크톱 애플리케이션에서 UWP 시각적 계층 API를 사용](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)할 수 있습니다. 이러한 API는 계층은 고성능, 그래픽 유지 모드 API, 효과 및 애니메이션을 제공하며 Windows 디바이스에서 UI의 기반이 됩니다.
Z-깊이 및 그림자 | [Z-깊이 및 그림자](../design/layout/depth-shadow.md)를 사용하여 UWP 앱에서 고도를 만들 수 있습니다. 이러한 새 기능을 사용하면 앱의 UI를 더 쉽게 검색할 수 있으며, 사용자가 집중해야 하는 중요한 사항을 더 효율적으로 전달할 수 있습니다.

## <a name="develop-windows-apps"></a>Windows 앱 개발

기능 | 설명
:------ | :------
AMSI(맬웨어 방지 프로그램 검사 인터페이스) | [AMSI(맬웨어 방지 프로그램 검사 인터페이스)가 맬웨어로부터 방어하는 데 어떻게 도움을 주는지](/windows/desktop/amsi/how-amsi-helps) 알아보고 [샘플 코드](/windows/desktop/amsi/dev-audience)를 확인하여 데스크톱 앱에서 구현하는 방법을 알아보세요.
C++/WinRT 2.0 | C++/WinRT 버전 2.0이 릴리스되었습니다. [C++/WinRT의 새로운 기능](../cpp-and-winrt-apis/news.md)에서 새로운 변경 내용 및 추가 내용을 전체적으로 확인하세요.
플랫폼 선택 | 새 데스크톱 애플리케이션을 만드는 데 관심이 있나요? 개선된 [플랫폼 선택](/windows/desktop/choose-your-technology) 페이지에서 UWP, WPF 및 Windows Forms에 대한 자세한 설명과 비교 결과 및 Win32 API에 대한 자세한 내용을 확인하세요.
대화형 에이전트 | [Windows.ApplicationModel.ConversationalAgent](/uwp/api/windows.applicationmodel.conversationalagent) 네임스페이스를 사용하여 Windows 플랫폼 AAR(에이전트 활성화 런타임)서 지원하는 모든 디지털 지원을 Windows 앱에 추가할 수 있습니다.
클라우드 파일 API | *클라우드 파일 API* 를 사용하여 [자리 표시자 파일을 지원하는 클라우드 동기화 엔진을 빌드](/windows/desktop/cfapi/build-a-cloud-file-sync-engine)할 수 있습니다.
Direct 3D 12 | [Direct3D 12 렌더링 단계](/windows/desktop/direct3d12/direct3d-12-render-passes)는 다른 기술 중에서 TBDR(타일식 지연 렌더링)을 기준으로 하는 렌더러의 성능을 향상시킬 수 있습니다. 이 기술은 애플리케이션에서 리소스 렌더링의 순서 지정 요구 사항 및 데이터 종속성을 더 잘 파악하도록 하여 렌더러에서 GPU 효율성을 개선할 수 있습니다. 렌더러는 렌더링 단계 기능을 통해 GPU 효율성을 개선할 수 있습니다. 이를 통해 오프칩 메모리와 주고받는 메모리 트래픽이 줄어듭니다.
DirectML(Direct Machine Learning) | [DirectML](/windows/desktop/direct3d12/dml)은 기계 학습을 위한 하위 수준의 하드웨어 가속 API입니다. 이 API에는 DirectX 12 스타일의 친숙한(기본 C++, nano-COM) 프로그래밍 인터페이스 및 워크플로가 있습니다. 기계 학습 추론 워크로드를 게임, 엔진, 미들웨어, 백 엔드 또는 기타 애플리케이션에 연결할 수 있습니다. DirectML은 모든 DirectX 12 호환 가능 하드웨어에서 지원됩니다.
DirectX HLSL | [HLSL 셰이더 모델 6.4](/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12)는 DirectML에서 사용할 수 있는 새로운 기계 학습 내부 기능을 제공합니다.
드라이버 개발 | Windows 드라이버 개발자를 위해 새로운 오디오, 카메라, 디스플레이, 네트워킹, 모바일 광대역, 인쇄, 센서, 스토리지 및 WiFi 기능이 추가되었습니다. [드라이버 개발의 새로운 기능](/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest)에서 자세한 내용을 확인하세요.
파일 시스템 작업 | 이 [모범 사례 가이드](../files/best-practices-for-writing-to-files.md)는 최선의 방식으로 Windows.Storage.FileIO 및 Windows.Storage.PathIO 클래스를 사용하여 파일 시스템 I/O 작업을 수행하는 데 유용할 수 있습니다.
게임 패드 및 리모컨 조작 | [게임 패드 및 리모컨 조작](../design/input/gamepad-and-remote-interactions.md)을 사용하여 유용하고 액세스 가능한 조작 환경을 구축할 수 있습니다. 이러한 조작을 사용하여 애플리케이션에서 3m 떨어진 위치에서도 60cm 정도 떨어진 것처럼 편리하고 쉽게 작업할 수 있습니다.
일본 연호 변경 | Windows 애플리케이션에서 2019년 5월 1일에 일본 연호 변경 세트가 적용될 준비를 수행하는 방법을 보여 주기 위해 [이러한 지침](../design/globalizing/japanese-era-change.md)을 제공했습니다. 이 페이지는 일본어로도 제공됩니다(문서 하단의 언어 컨트롤을 클릭하고 일본어 선택).
WPF, Windows Forms 및 WinUI의 오픈 소스 | 이제 GitHub에서 WPF, Windows Forms 및 WinUI UX 프레임워크로도 오픈 소스를 제공하고 있습니다. 자세한 내용과 링크를 보려면 [Windows 앱 빌드 블로그](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)를 참조하세요.
Xbox용 점진적 웹앱 | [Xbox One용 점진적 웹앱](/microsoft-edge/progressive-web-apps/xbox-considerations)을 사용하면 기존 프레임워크, CDN 및 서버 백 엔드를 계속 사용하면서 웹 애플리케이션을 확장하고 Microsoft Store를 통해 Xbox One 앱으로 사용하도록 제공할 수 있습니다. 대부분의 경우 PWA를 Windows용으로 패키징하는 것과 마찬가지로 Xbox One용으로도 패키징할 수 있습니다. 이 가이드에서는 이 프로세스를 안내하고 주요 차이점을 강조 표시합니다.
프로젝트 로마 | 이제 Android 및 iOS용으로 제공되는 프로젝트 “로마” SDK를 사용할 수 있습니다. Graph 알림을 각 플랫폼 [Android](/windows/project-rome/notifications/how-to-guide-for-android) 및 [iOS](/windows/project-rome/notifications/how-to-guide-for-ios)에 연결하는 방법을 알아보세요.
원격 카메라 | DeviceWatcher 클래스를 사용하여 [원격 카메라에 연결](../audio-video-camera/connect-to-remote-cameras.md)하고 해당 카메라에서 Windows 앱으로 프레임을 읽어올 수 있습니다.
데스크톱 애플리케이션의 UWP 컨트롤(XAML island) | WPF, Windows Forms 및 C++ Win32 데스크톱 애플리케이션에 UWP 컨트롤을 호스트하는 Windows SDK의 API가 더 이상 개발자 미리 보기로 제공되지 않습니다. 자세한 내용은 [데스크톱 애플리케이션의 UWP 컨트롤](/windows/apps/desktop/modernize/xaml-islands)을 참조하세요.
Visual Studio 2019 | 모든 개발자, 앱 또는 플랫폼을 위한 최신 도구 및 서비스를 포함하는 Visual Studio 2019가 릴리스되었습니다. [Visual Studio 2019의 새로운 기능](/visualstudio/ide/whats-new-visual-studio-2019)에서 최신 기능을 알아보고 시작하세요.
Win32 WebView | [질문과 대답](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)에서는 데스크톱 애플리케이션에서 Microsoft Edge WebView를 사용할 때 일반적으로 발생하는 질문과 대답 뿐 아니라 샘플 및 추가 리소스에 대한 링크를 제공합니다.
Windows 명령줄 | [새 콘솔 기능](https://devblogs.microsoft.com/commandline/new-experimental-console-features/)에는 스크롤, 커서 모양 및 커서 색에 대한 설정을 포함하는 실험적 터미널 탭이 있습니다. [개발자용 Windows 명령줄 도구 블로그](https://devblogs.microsoft.com/commandline/)에서 자세한 내용을 확인하세요.
Windows 커뮤니티 도구 키트 | Windows 커뮤니티 도구 키트 v5.1은 애니메이션, 원격 디바이스, 이미지 자르기 및 내게 필요한 옵션에 대한 흥미로운 업데이트를 제공합니다. </br> • 새 [Lottie Windows 라이브러리](/windows/communitytoolkit/animations/lottie)는 Windows.UI.Composition API를 활용하여 Windows 10(1809)에서 고품질 애니메이션 지원을 제공하고 Windows 앱에서 재생하기 위해 [Bodymovin](https://aescripts.com/bodymovin/) JSON 파일 또는 최적화된 코드 생성 클래스를 사용할 수 있도록 지원합니다. Microsoft Store에 제공되는 새로운 [Lottie Viewer 앱](https://www.microsoft.com/p/lottie-viewer/9p7x9k692tmw)을 사용하여 애니메이션을 테스트해보고 Windows 앱용으로 최적화된 코드를 생성해 보세요. </br> • 새 [원격 디바이스 선택](/windows/communitytoolkit/controls/remotedevicepicker) 기능을 사용하여 디바이스를 선택하거나(근접 방식 또는 클라우드를 통해 액세스), 해당 디바이스에서 앱을 시작하거나, 원격 디바이스에서 앱 서비스와 통신할 수 있습니다. </br> • 새 [ImageCropper 컨트롤](/windows/communitytoolkit/controls/imagecropper)은 프로필 사진을 선택하거나 사진 편집 도구를 사용하기 위해 자르기 기능을 통합합니다. </br> • 또한 컨트롤의 접근성이 개선되고, WPF 및 WinForms에 대한 [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 미리 보기 패키지 업데이트가 제공되고, [릴리스 정보](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0)에서 읽을 수 있는 추가 기능도 제공되었습니다.
Windows Machine Learning | Windows AI 문서를 새롭게 디자인하여 WinML(Windows Machine Learning), Windows Vision Skills 및 DirectML(Direct Machine Learning)의 세 영역으로 구분했습니다. 새 [방문 페이지](/windows/ai/)를 확인해 보세요. </br> • [*MLGen* 환경](/windows/ai/mlgen)이 Visual Studio에서 변경될 예정입니다. Windows 10, 버전 1903 이상에서 *mlgen* 은 더 이상 Windows 10 SDK에 포함되지 않습니다. VS 2017을 사용하는 경우 대신, Visual Studio 확장, [Windows Machine Learning 코드 생성기 VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen)을 다운로드하여 설치해야 합니다. Visual Studio 2019를 사용하는 경우 [Windows Machine Learning 코드 생성기](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2) 확장을 설치해야 합니다. </br> •  가중치 압축에 대한 새로운 지원을 공개합니다. 개발자는 이제 [WinMLTools 변환기](/windows/ai/convert-model-winmltools)를 통해 사용할 수 있는 가중치 압축이라는 기술을 사용하여 ML 모델의 디스크 공간을 줄일 수 있습니다.
WinRT 통합 참조 | WinRT API 구조 정의에 대한 구체적인 심층 정보를 제공하기 위해 [WinRT 형식 시스템](/uwp/winrt-cref/winrt-type-system) 및 [WinMD 파일](/uwp/winrt-cref/winmd-files)의 전체 설명을 추가했습니다.
WSL(Linux용 Windows 하위 시스템) | [WSL의 최신 업데이트](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/)에는 파일 탐색기를 사용하여 Windows에서 Linux 파일에 액세스하는 기능과 wsl.exe 및 wslconfig.exe를 위한 일부 새 명령이 들어 있습니다.
Windows Vision Skills | [Windows Vision Skills](/windows/ai/windows-vision-skills)는 얼굴 인식과 같은 “기술"을 만든 후 다른 앱에서 사용할 수 있는 NuGet 패키지로 패키지하여 기계 학습 모델을 포함할 필요가 없도록 하는 API 세트입니다.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

기능 | 설명
 :------ | :------
MSIX | [Windows 10 빌드 1709 및 1803의 MSIX 지원](/windows/msix/msix-1709-and-1803-support)에서는 Windows 10, 1809 이전 버전에서 지원되는 MSIX 기능에 대해 설명합니다.
MSIX 패키징 및 배포 | MSIX 패키지에서 사용자 지정 내용을 보다 쉽게 패키지할 수 있도록 하기 위해 [수정 패키지와 관련된 몇 가지 개선된 기능](/windows/msix/modification-package-insider-preview-build-18312)을 도입했습니다. 이러한 개선된 기능에는 패키지 매니페스트의 새로운 **rescap6:ModificationPackage** 요소, 주 패키지의 파일을 수정 패키지로 재정의하는 기능, 파일 시스템 기반 플러그 인을 MSIX 수정 패키지로 패키지하는 기능이 포함됩니다.
MSIX Packaging Tool | • [원격 컴퓨터에서 변환을 수행하기 위한 지원](/windows/msix/packaging-tool/remote-conversion-setup)을 추가했습니다. 또한 새 도구 기능을 미리 맛볼 수 있는 [MSIX Packaging Tool 참가자 프로그램](/windows/msix/packaging-tool/insider-program)도 도입했습니다. </br> • [1709 이상에서 MSIX 패키지 지원](/windows/msix/packaging-tool/support-on-1709-and-later)은 MSIX Packaging Tool을 사용하여 Windows 10, 버전 1709 및 1803에 맞게 패키지를 빌드하는 방법에 대한 지침을 제공합니다. </br> • [Hyper-V 빨리 만들기의 MSIX 패키징 환경](/windows/msix/packaging-tool/quick-create-vm)은 MSIX 패키징 프로젝트를 위한 가상 환경을 만드는 방법을 보여 줍니다. </br> • [MSIX 패키지 번들](/windows/msix/packaging-tool/bundle-msix-packages)은 MSIX Packaging Tool을 사용하여 패키지 번들을 만들기 위한 지침을 제공합니다. </br> • [Windows 10 버전 1809의 수정 패키지](/windows/msix/modification-package-1809-update)에는 MSIX Packaging Tool 및 MakeApp.exe를 사용하여 Windows 10, 1809 이상 버전을 위한 수정 패키지를 만들기 위한 지침이 포함되어 있습니다.
MSIX SDK | [MSIX SDK를 사용하여 플랫폼 간에 사용할 패키지를 빌드](/windows/msix/msix-sdk/sdk-guidance)하고 패키지를 추출할 대상 플랫폼을 지정하는 방법을 알아봅니다.

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft Learn에서는 Microsoft 개발자에게 새 실습 학습 및 교육 기회를 제공합니다.

* Windows 앱을 개발하는 방법을 알고 싶은 경우 [새 학습 경로](/learn/paths/develop-windows10-apps/)에서 플랫폼, 도구에 대한 자세한 소개와 첫 번째 앱을 작성하는 방법을 확인하세요.

* Windows 앱에 UI 기능을 추가하는 방법을 알고 싶나요? [UI를 만들거나](/learn/modules/create-ui-for-windows-10-apps/), [UI에 탐색 기능 및 미디어를 추가하거나](/learn/modules/enhance-ui-of-windows-10-app/), [데이터 바인딩을 구현](/learn/modules/implement-data-binding-in-windows-10-app/)하는 방법을 알아보세요.

* 웹 개발에 관심이 있는 경우 [Visual Studio Code를 사용하여 웹 애플리케이션 개발](/learn/modules/develop-web-apps-with-vs-code/) 또는 [간단한 웹 사이트 빌드](/learn/modules/build-simple-website/)를 참조하세요.

* [Microsoft Learn의 모든 Windows 개발자 모듈](/learn/browse/?products=windows&resource_type=module)을 자유롭게 찾아볼 수도 있습니다.

## <a name="videos"></a>동영상

### <a name="progressive-web-apps"></a>프로그레시브 웹앱

점진적 웹앱은 다른 브라우저 및 다양한 Windows 10 디바이스에서 네이티브 앱처럼 작동하는 웹 사이트입니다. [비디오를 시청하여](https://youtu.be/ugAewC3308Y) 방법을 알아보고 [관련 문서를 확인하여](https://developer.microsoft.com/windows/pwa) 시작하세요.

### <a name="vs-code-series"></a>VS Code 시리즈

[Visual Studio Code에 대한 새 비디오 시리즈](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)를 시청하여 VSCode란 무엇이며, 어떻게 사용하고, 어떻게 만들어졌는지에 대해 알아보세요.

### <a name="mixed-reality-services"></a>Mixed Reality 서비스

HoloLens 2가 최근에 발표되었습니다. 이 [Mixed Reality에 대한 비디오 시리즈](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST)를 시청하고 필요한 기능을 숙지하고 개발을 시작하는 방법을 알아보세요.

### <a name="one-dev-question"></a>One Dev Question

Microsoft 개발자는 One Dev Question 동영상 시리즈에서 오랫동안 Windows 개발, 팀 문화 및 이력에 대한 일련의 질문을 다루어 왔습니다.

* [Raymond Chen, Windows 개발 및 역사](https://www.youtube.com/watch?v=teV0gjCacug&list=PLlrxD0HtieHge3_8Dm48C0Ns61I6bHThc)

* [Larry Ostman, Windows 개발 및 역사](https://www.youtube.com/watch?v=_34CokLwodE&list=PLlrxD0HtieHhDTjMijDOd0BSJcpSFabfE)

* [Chris Heilmann, VS Code](https://www.youtube.com/watch?v=cYRn5ONWAqo&list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)

* [Aaron Gustafson, 점진적 웹앱](https://www.youtube.com/watch?v=ks3CYvPBO2k&ab_channel=MicrosoftDeveloper)