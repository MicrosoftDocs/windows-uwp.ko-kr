---
title: 개발자를 위한 Windows 10의 새로운 기능
description: Windows 10 18362 빌드하고 새로운 개발자 도구는 도구, 기능 및 유니버설 Windows 플랫폼에서 제공 하는 환경을 제공 합니다.
keywords: 새로운 기능, 새로운 기능, 업데이트, 업데이트, 기능, 새로운 Windows 10을 최신 개발자 18362를 수 있습니다.
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 935d7f787d0cc23965c0fd51747b7687adb80a3f
ms.sourcegitcommit: a4fe508e62827a10471e2359e81e82132dc2ac5a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66468317"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>새로운 Windows 10의 개발자에 게 제공 되는 빌드 18362

Visual Studio 2019를 사용 하 여 Windows 10 빌드 18362 (라고도 SDK 버전이 1903)를 조합 하 여 도구, 기능 및 환경을 주목할 만한 Windows 앱을 제공 합니다. Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

다음은 이 릴리스에서 Windows 개발자가 관심을 갖는 신규 및 개선 기능 및 안내 모음입니다. Windows SDK에 추가 하는 새 네임 스페이스의 전체 목록을 참조 하세요. 합니다 [Windows 10 빌드 18362 API 변경 내용](windows-10-build-18362-api-diff.md)합니다. Windows 10의 주요 기능에 대한 자세한 내용은 [Windows 10의 새로운 기능](https://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요.

## <a name="design--ui"></a>디자인 및 UI

기능 | 설명
:------ | :------
AnimatedVisualPlayer | 합니다 [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API을 호스팅하고 앱 애니메이션된 시각적 개체의 재생을 제어 합니다. 컨트롤과 같은 콘텐츠를 표시 합니다.이 API를 사용 [Lottie](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) 시각적 개체를 고유 하 게 응용 프로그램에서 Adobe AfterEffects 애니메이션 렌더링 수 있습니다.
CompactDensity | 사용 하도록 설정 [압축 모드가](../design/style/spacing.md) 앱에서 조밀한 정보가 풍부한 그룹 컨트롤을 사용 하도록 설정 합니다. 많은 양의 콘텐츠를 극대화 하는 페이지에 표시 되는 콘텐츠의 검색에 도움이 또는 사용자 입력 포인터를 사용 하는 경우 탐색 및 상호 작용을 지 원하는 수 있습니다.
반복기 항목 | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md) 컨트롤에 사용자에 게 컬렉션을 표시 하기 위한 사용자 지정 환경을 생명체 수 있습니다. ItemsRepeater 포괄적인 최종 사용자 환경 또는 기본 UI 제공 하지 않습니다. 대신 자신의 고유한 컬렉션 기반 환경을 만들고 사용자 지정 컨트롤을 사용할 수 있는 구성 요소는 것입니다.
교육 팁 | A [팁을 가르치는](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md) 컨텍스트 정보를 제공 하는 반 영구 및 풍부한 콘텐츠가 포함 플라이 아웃 됩니다. 알리는, 상기 시켜, 및 새롭거나 중요 한 기능에 대 한 사용자 교육에 대 한이 컨트롤을 사용할 수 있습니다.
UI 명령 | 사용 하 여 [UWP 앱에서 명령](../design/controls-and-patterns/commanding.md)를 사용 합니다 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 및 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) (ICommand 인터페이스)와 함께 공유 하 고 명령에서 다양 한 관리 클래스 사용 중인 장치 및 입력 형식에 관계 없이 형식 제어 합니다.
Windows UI Library | Windows UI 라이브러리-최신 공식 버전 [WinUI 2.1](https://docs.microsoft.com/uwp/toolkits/winui/release-notes/winui-2.1) – Windows 앱에 대 한 활발 한 새 XAML 컨트롤을 제공 합니다. WinUI 라이브러리 API는 이전 버전의 Windows 10에서 실행되므로 최신 OS를 사용하지 않는 사용자를 지원하기 위해 버전 확인 또는 조건부 XAML을 포함할 필요가 없습니다.
데스크톱 앱의 시각적 계층 | 이제 [데스크톱 응용 프로그램에서 UWP 시각적 계층 Api를 사용 하 여](../composition/visual-layer-in-desktop-apps.md)입니다. 이러한 Api는 그래픽, 효과 및 애니메이션에 대 한 성능 우선 모드 재 학습 API를 제공 하 고 Windows 장치에서 UI에 대 한 기반이 됩니다.
Z-깊이 및 그림자 | 사용 하 여 [Z 깊이 및 섀도](../design/layout/depth-shadow.md) UWP 앱에서 권한 상승을 만들려고 합니다. 이 새 기능을 사용 하면 쉽게 앱의 UI를 검색 하려면 더 중점적으로 사용자에 게 중요 한 것을 전달 합니다.

## <a name="develop-windows-apps"></a>Windows 앱 개발

기능 | 설명
:------ | :------
맬웨어 방지 검사 인터페이스 (AMSI) | 에 대해 알아봅니다 [맬웨어 방지 검사 인터페이스 (AMSI) 어떻게 도움을 주는지 악성 프로그램 방어](https://docs.microsoft.com/windows/desktop/amsi/how-amsi-helps), 다음 체크 아웃 합니다 [샘플 코드](https://docs.microsoft.com/windows/desktop/amsi/dev-audience) 데스크톱 앱에서이 구현 하는 방법을 알아보려면.
C++/WinRT 2.0 | 버전 2.0의 C++/WinRT 놓았습니다. 체크 아웃 [의 새로운 기능 C++/WinRT](../cpp-and-winrt-apis/news.md) 모든 새 변경 및 추가 전체 run-down에 대 한 합니다.
플랫폼 선택 | 새 데스크톱 응용 프로그램을 만드는 데 관심이 있으십니까? 이 개선 된 확인해 [플랫폼을 선택](https://docs.microsoft.com/windows/desktop/choose-your-technology) 자세한 설명 및 플랫폼, UWP, WPF 및 Windows Forms 및 Win32 API에 대 한 자세한 내용은 비교에 대 한 페이지입니다.
대화형 에이전트 | 합니다 [Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent) 네임 스페이스를 사용 하면 Windows 앱에는 Windows 에이전트 활성화 런타임 (AAR) 플랫폼에서 지원 되는 모든 디지털 지원을 추가 합니다.
클라우드 파일 API | 합니다 *클라우드 파일 API* 수 있습니다 [자리 표시자 파일을 지 원하는 클라우드 동기화 엔진을 구축](https://docs.microsoft.com/windows/desktop/cfapi/build-a-cloud-file-sync-engine)합니다.
Direct 3D 12 | [Direct3D 12 렌더링 패스](/windows/desktop/direct3d12/direct3d-12-render-passes) 성능을 개선할 수 여 렌더러의 지연 된 렌더링 타일 기반의 (TBDR)에 기초 하는 경우 다른 기술 중 에서도 특히 합니다. 기술에 렌더러 리소스 렌더링 요구 사항 및 데이터 종속성 순서를 확인 하는 데 응용 프로그램을 사용 하 여 GPU 효율성을 향상 시킬 수 있습니다. 이렇게 하면 칩 해제 메모리에서 메모리 트래픽이 줄어듭니다.
DirectML(Direct Machine Learning) | [DirectML](https://docs.microsoft.com/windows/desktop/direct3d12/dml) 는 machine learning 위한 하드웨어 가속 하위 수준 API. 친숙 한 있기 (네이티브 C++, nano COM) 프로그래밍 인터페이스 및 워크플로에서 DirectX 12의 스타일입니다. Machine learning을 통합할 수 있습니다 게임, 엔진, 미들웨어, 백 엔드, 또는 다른 응용 프로그램에 대 한 추론 작업 합니다. DirectML은 DirectX 12와 호환 되는 모든 하드웨어에서 지원 됩니다.
DirectX HLSL | [HLSL 셰이더 모델 6.4](https://docs.microsoft.com/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) DirectML 사용에 대 한 새 machine learning 내장 함수를 제공 합니다.
드라이버 개발 | 새 오디오, 카메라, 표시, 네트워킹, Windows 드라이버 개발자를 위한 모바일 광대역, 인쇄, 센서, 저장소 및 wifi 기능이 추가 되었습니다. 체크 아웃 [드라이버 개발의 새로운 기능](https://docs.microsoft.com/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest) 자세한 내용은 합니다.
파일 시스템 작업 | 이렇게 [모범 사례 가이드](../files/best-practices-for-writing-to-files.md) 효율적으로 사용 된 Windows.Storage.FileIO 및 파일 시스템 I/O 작업을 수행 하려면 Windows.Storage.PathIO 클래스 도움이 됩니다.
게임 패드 및 리모컨 조작 | 사용 하 여 [gamepad 및 원격 제어 상호 작용](../design/input/gamepad-and-remote-interactions.md) 환경을 사용 가능 하 고 액세스할 수 있는 상호 작용을 만들려고 합니다. 이러한 상호 작용을 사용 하 여 응용 프로그램 수와 직관적이 고 두 피트 떨어진 곳에서 사용 하기 쉬운 10 피트 떨어진 곳에서 그대로입니다.
일본어 연대 변경 | 제공 했습니다 [이러한 지침](../design/globalizing/japanese-era-change.md) 연대 변경 2019 년 5 월 1 일에서 수행 하는 집합 Windows 응용 프로그램의 일본어에 대 한 준비 확인 하는 방법을 보여 줍니다. [또한이 페이지는 일본어에서](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)합니다.
WPF, Windows Forms 및 WinUI의 오픈 소스 | WPF, Windows Forms 및 WinUI UX 프레임 워크를 GitHub에서 오픈 소스 기여에 대 한 사용할 수 있습니다. 자세한 내용과 링크에 대 한 참조를 [빌드 Windows 앱 블로그](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)합니다.
Xbox 용 점진적 웹 앱 | 사용 하 여 [점진적 Web Apps for Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), 웹 응용 프로그램을 확장 하 고 사용할 수 있도록 Xbox One 앱으로 Microsoft Store 통해 여전히 계속 기존 프레임 워크, 백 엔드 서버 및 CDN을 통해 사용할 수 있습니다. 대부분의 경우 Windows의 경우 동일한 방식으로 Xbox One에 대 한 PWA에 패키징할 수 있습니다. 이 가이드는 프로세스를 안내 하 고 주요 차이점을 강조 표시 됩니다.
프로젝트 로마 | 프로젝트 로마 SDK Android 및 iOS에 대 한 출시 되었습니다. 각 플랫폼을 사용 하 여 그래프 알림을 통합 하는 방법에 알아봅니다. [Android](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-android) 하 고 [iOS](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-ios)합니다.
원격 카메라 | DeviceWatcher 클래스를 사용 하 여 [원격 카메라를 연결할](../audio-video-camera/connect-to-remote-cameras.md), Windows 앱에 이러한 카메라에서 프레임을 읽습니다.
데스크톱 응용 프로그램 (XAML 제도)에서 UWP 컨트롤 | Wpf에서 Windows Forms, UWP 컨트롤을 호스트 하는 것에 대 한 Windows SDK의 Api 및 C++ Win32 데스크톱 응용 프로그램은 더 이상 개발자 미리 보기로 제공에서 됩니다. 자세한 내용은 [데스크톱 응용 프로그램에서 UWP 컨트롤](../xaml-platform/xaml-host-controls.md)합니다.
Visual Studio 2019 | 최신 도구와 모든 개발자, 앱 또는 플랫폼에 대 한 서비스를 사용 하 여 visual Studio 2019는 놓았습니다. 체크 아웃 [Visual Studio 2019의 새로운 기능](https://docs.microsoft.com/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019) 시작 하 고 최신 정보를 알아보세요.
Win32 WebView | 우리의 [질문과 대답](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) 샘플 및 추가 리소스에 대 한 링크를 비롯 하 여 데스크톱 응용 프로그램에 Microsoft Edge 웹 보기를 사용 하는 경우 일반적인 질문과 대답을 제공 합니다.
Windows 명령줄 | [새 콘솔 기능](https://devblogs.microsoft.com/commandline/new-experimental-console-features/) 스크롤, 커서 모양 및 커서 색에 대 한 설정 사용 하 여 실험적 터미널 탭이 포함 되어 있습니다. 자세한 내용은 합니다 [Windows 명령줄 도구에 대 한 개발자 블로그](https://devblogs.microsoft.com/commandline/)합니다.
Windows 커뮤니티 도구 키트 | Windows 커뮤니티 도구 키트 v5.1 애니메이션, 원격 장치에서 이미지 자르기 및 내게 필요한 옵션에 대 한 흥미로운 업데이트를 제공 합니다. </br> • 새 [Lottie Windows 라이브러리](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) Windows.UI.Composition Api를 활용 하 여 Windows 10 (1809)에서 고품질 애니메이션 지원을 제공 하 고 사용에 대 한 허용 [Bodymovin](https://aescripts.com/bodymovin/) JSON 파일 또는 Windows 앱에서 재생에 대 한 코드 생성 클래스로 최적화 되어 있습니다. 새로운 [Lottie 뷰어 앱](https://aka.ms/lottieviewer) 애니메이션을 테스트 하 여 Windows 앱에 대 한 최적화 된 코드를 생성 하는 Microsoft Store. </br> • 새 [원격 장치 선택](https://docs.microsoft.com/windows/communitytoolkit/controls/remotedevicepicker) 사용자가 장치를 선택 하도록 허용 (proximally 또는 액세스할 수 있는 클라우드), 해당 장치에서 앱을 시작 하거나 원격 장치에서 앱 서비스와 통신 합니다. </br> • 새 [ImageCropper 컨트롤](https://docs.microsoft.com/windows/communitytoolkit/controls/imagecropper) 선택 프로필 사진 또는 사진 편집 도구를 사용 하 여 자르기 기능을 통합 합니다. </br> • 컨트롤에 대해 내게 필요한 옵션 개선 되었습니다 또한을 [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 WPF 및 WinForms에 대 한 읽을 수 있는 더 많은 기능에 대 한 패키지 업데이트 미리 보기는 [릴리스 정보 ](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0).
Windows Machine Learning | 세 가지 영역으로 분할 Windows AI 문서를 새롭게 디자인 했습니다. Windows Machine Learning (WinML), Windows 비전 기술 및 직접 Machine Learning (DirectML). 새로운 [방문 페이지](https://docs.microsoft.com/windows/ai/) </br> • 합니다 [ *MLGen* 경험](https://docs.microsoft.com/windows/ai/mlgen) Visual Studio에서 변경 됩니다. Windows 10에서 버전이 1903 이상 *mlgen* 가 Windows 10 SDK에 포함 되지 않게 합니다. VS 2017을 사용 하는 경우 대신 다운로드 하 고 해야 Visual Studio 확장을 설치 [Windows Machine Learning 코드 생성기 VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen)합니다. Visual Studio 2019를 사용 하는 경우 설치 해야 합니다 [Windows Machine Learning 코드 생성기](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2) 확장 합니다. </br> • 하면서도 압축 가중치에 대 한 새 지원을 공개 알려드립니다. 개발자가 이제 줄일 수 있습니다는 기계 학습 모델의 디스크 공간 이라는 가중치 압축을 통해 사용할 수 있는 기술을 사용 하 여 합니다 [WinMLTools 변환기](https://docs.microsoft.com/windows/ai/convert-model-winmltools)합니다.
WinRT 참조 통합 | 전체 설명을 추가 했습니다 합니다 [WinRT 형식 시스템](https://docs.microsoft.com/uwp/winrt-cref/winrt-type-system) 하 고 [WinMD 파일](https://docs.microsoft.com/uwp/winrt-cref/winmd-files)WinRT Api의 구조에 대 한 정의 대 한 특정 자세한 노트를 제공 하 합니다.
Linux용 Windows 하위 시스템(WSL) | [WSL 대 한 최근 업데이트](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/) wsl.exe 및 wslconfig.exe 파일 탐색기 및 일부 새 명령을 사용 하 여 Windows에서 Linux 파일에 액세스 하는 기능을 포함 합니다.
Windows Vision Skills | [Windows 비전 기술을](https://docs.microsoft.com/windows/ai/windows-vision-skills) 는 수 있는 Api 집합을 "" 같은 기술을 얼굴 인식을 생성 하 고 다음도 기계 학습 모델을 포함할 필요 없이 다른 앱을 사용할 수 있는 NuGet 패키지로 패키지.

## <a name="publish--monetize-windows-apps"></a>Windows 앱 게시 및 수익 창출

기능 | 설명
 :------ | :------
MSIX | [Windows 10에서 MSIX 지원 1709 및 1803 작성](https://docs.microsoft.com/windows/msix/msix-1709-and-1803-support) Windows 10 버전 1809 이전 버전에서 지원 되는 MSIX 기능에 설명 합니다.
MSIX 패키징 및 배포 | 몇 가지를 도입 했습니다 [수정 패키지와 관련 된 향상 된 기능](https://docs.microsoft.com/windows/msix/modification-package-insider-preview-build-18312) MSIX 패키지에서 사용자 지정 패키지를 쉽게 수행할 수 있도록 합니다. 이러한 향상 된 새 포함 **rescap6:ModificationPackage** 요소는 패키지 매니페스트, 수정 패키지를 사용 하 여 기본 패키지에서 파일을 재정의 하는 기능 및 파일 시스템을 패키지 하는 기능에 따라 플러그 인 MSIX 수정 패키지로 합니다.
MSIX Packaging Tool | • 추가한 [원격 컴퓨터에서 변환을 수행 하는 것에 대 한 지원](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup)합니다. 또한 도입 했습니다 합니다 [MSIX 패키징 도구 참가자 프로그램](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program) 새로운 도구 기능에 대 한 초기 액세스를 제공 합니다. </br> • [MSIX 패키지 지원 1709 이상](https://docs.microsoft.com/windows/msix/packaging-tool/support-on-1709-and-later) MSIX 패키징 도구를 사용 하 여 Windows 10 버전 1709 및 1803에 맞게 패키지를 빌드하는 방법에 대 한 지침을 제공 합니다. </br> • [Hyper-v 빠른 만들기 MSIX 패키징 환경](https://docs.microsoft.com/windows/msix/packaging-tool/quick-create-vm) MSIX 패키징 프로젝트에 대 한 가상 환경을 만드는 방법을 보여 줍니다. </br> • [번들 MSIX 패키지](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages) MSIX 패키징 도구를 사용 하 여 패키지 번들을 만들기 위한 지침을 제공 합니다. </br> • [Windows 10 버전 1809 수정 패키지](https://docs.microsoft.com/windows/msix/modification-package-1809-update) MakeApp.exe 고 MSIX 패키징 도구를 사용 하 여 1809 이상 버전의 Windows 10 버전에 대 한 수정 패키지를 만들기 위한 지침이 포함 되어 있습니다.
MSIX SDK | [MSIX SDK를 사용 하 여 플랫폼 간 사용에 대 한 패키지를 빌드하려면](https://docs.microsoft.com/windows/msix/msix-sdk/sdk-guidance), 패키지를 추출 하려면 대상 플랫폼을 지정 하는 방법을 알아봅니다.

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft 알아보기 Microsoft 개발자에 게 새 실습 학습 및 교육 기회를 제공합니다.

* Windows 앱을 개발 하는 방법을 알고 싶은 경우 체크 아웃 [우리의 새 학습 경로](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) 대 한 철저 한 플랫폼, 도구, 및 첫 번째 몇 가지 앱을 작성 하는 방법을 소개 합니다.

* Windows 앱에 UI 기능을 추가 하는 방법을 알아봅니다 하 시겠습니까? 에 대해 알아봅니다 하는 방법 [UI를 만들](https://docs.microsoft.com/learn/modules/create-ui-for-windows-10-apps/)를 [ui 탐색 및 미디어를 추가](https://docs.microsoft.com/learn/modules/enhance-ui-of-windows-10-app/), 또는 [데이터 바인딩을 구현할](https://docs.microsoft.com/learn/modules/implement-data-binding-in-windows-10-app/)합니다.

* 웹 개발에 관심이 있는 경우 체크 아웃 [Visual Studio Code를 사용 하 여 웹 응용 프로그램 개발](https://docs.microsoft.com/learn/modules/develop-web-apps-with-vs-code/) 하거나 [간단한 웹 사이트 빌드](https://docs.microsoft.com/learn/modules/build-simple-website/)합니다.

* 또는 찾아보기 자유롭게 [알아보기 Microsoft의 모든 Windows 개발자 모듈](https://docs.microsoft.com/learn/browse/?products=windows&resource_type=module)합니다.

## <a name="videos"></a>비디오

### <a name="progressive-web-apps"></a>점진적 웹앱

점진적 웹 앱은 다른 브라우저 및 다양 한 Windows 10 장치에서 네이티브 앱 처럼 작동 하는 웹 사이트입니다. [비디오를 시청](https://youtu.be/ugAewC3308Y) 자세한 내용은 차례로 [문서를 체크 아웃](https://aka.ms/Windows-PWA) 시작 하려면.

### <a name="vs-code-series"></a>VS 코드 시리즈

체크 아웃 우리의 [Visual Studio Code에서 새 비디오 시리즈](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) VSCode 란를 사용 하는 방법 및 생성 방법에 대 한 정보에 대 한 합니다.

### <a name="mixed-reality-services"></a>혼합된 현실 서비스

HoloLens 2 최근에 발표 되었습니다. 이 확인해 [혼합 현실에 대 한 비디오 시리즈](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST) 최신 정보 및 참여 개발을 시작 하는 방법에 대 한 합니다.

### <a name="one-dev-question"></a>개발 질문 중 하나

개발 질문 중 하나는 비디오 시리즈에서는 해온 Microsoft 개발자에 게 일련의 Windows 개발, 팀 문화권 및 기록 하는 방법에 대 한 질문을 다룹니다.

* [Raymond chen이 Windows 개발에 기록](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Windows 개발 기록에 대 한 Larry Osterman](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [점진적 웹 앱에서 Aaron Gustafson](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann webhint 도구](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
