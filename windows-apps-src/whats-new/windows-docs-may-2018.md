---
title: 2018년 5월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2018년 5월 Windows 10 개발자 설명서 및 Microsoft Build 컨퍼런스에 추가된 새로운 기능, 동영상 및 개발자 지침
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 5월, 빌드
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9adf5a80595e00a30098044536d1ecfe4fd62279
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820489"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>2018년 5월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 가이드, 동영상 및 샘플은 [Microsoft Build 2018](https://www.microsoft.com/build/) 개발자 컨퍼런스에 맞춰 5월 중에 사용할 수 있게 제공되었습니다.

Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="motion-in-fluent-design"></a>Fluent Design의 동작

Fluent Design System 동작의 기본적인 타이밍, 감속, 방향 및 중력 기능을 토대로 발전하고 있습니다. 이러한 기본 기능을 적용하여 사용자에게 앱을 안내하고, 디지털 환경의 자연스러움을 반영하여 이러한 환경에 사용자를 연결할 수 있습니다. 자세한 내용은 다음 문서를 참조하세요.

* [동작 개요](../design/motion/index.md)는 이러한 기본 기능을 반영하도록 업데이트되었습니다.
* [실제 동작](../design/motion/motion-in-practice.md)은 앱 내에서 이러한 기본 기능을 적용하는 방법의 예를 제공합니다.
* [방향 및 중력](../design/motion/directionality-and-gravity.md)은 앱의 사용자 정신 모델을 강화합니다.
* [타이밍 및 감속](../design/motion/timing-and-easing.md)은 앱의 동작에 현실성을 추가합니다.

![동작 진행](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design 업데이트

Fluent Design 페이지가 다음과 같이 시각적으로 업데이트되고 약간 변경되었습니다.

* [맞춤, 여백, 안쪽 여백](../design/layout/alignment-margin-padding.md)
* [색](../design/style/color.md)
* [명령 기본 사항](../design/basics/commanding-basics.md)
* [Windows용 Fluent Design 앱](../design/fluent-design-system/index.md)
* [앱 디자인 소개](../design/basics/design-and-ui-intro.md)
* [탐색 기본 사항](../design/basics/navigation-basics.md)
* [반응형 디자인 기술](../design/layout/responsive-design.md)
* [화면 크기 및 중단점](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [스타일 개요](../design/style/index.md)
* [쓰기 스타일](../design/style/writing-style.md)

또한 해당 콘텐츠 영역에 있는 모든 새 정보를 사용하여 다음 페이지를 다시 작성했습니다.

* 이제 [아이콘](../design/style/icons.md)은 아이콘을 사용하고 클릭 가능하게 만들기 위한 실제 권장 사항을 제공합니다.
* [입력 체계](../design/style/typography.md)는 비슷한 문서의 정보를 통합하고, 업데이트된 지침과 그림을 사용하여 모든 정보를 한 곳에서 제공합니다.

![색상표 이미지](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio의 앱 설치 관리자 파일

이제 Visual Studio 2017, 업데이트 15.7 이상 버전을 사용하여 앱 설치 관리자 파일을 만들 수 있습니다. [Visual Studio를 사용하여 앱 설치 관리자 파일을 만들고](../packaging/create-appinstallerfile-vs.md) 앱 자동 업데이트를 사용하도록 설정하는 방법을 알아봅니다. 실행 중에 문제가 발생하는 경우 [앱 설치 관리자 파일의 설치 문제 해결](../packaging/troubleshoot-appinstaller-issues.md)을 참조하여 일반적인 문제 및 해결 방법을 확인하세요.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Windows Forms 및 WPF 애플리케이션에 대한 Edge WebView 컨트롤

이전에는 UWP 애플리케이션에서만 사용할 수 있던 WebView 컨트롤을 사용하여 데스크톱 애플리케이션에서 웹 콘텐츠를 표시합니다. 이 컨트롤은 Microsoft Edge 렌더링 엔진을 사용하여 원격 웹 서버, 동적으로 생성된 코드 또는 콘텐츠 파일에서 서식 있는 HTML 콘텐츠를 렌더링하는 뷰를 포함합니다. [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)의 최신 버전에서 WebView 컨트롤을 찾으세요.

Windows 커뮤니티 도구 키트의 이후 릴리스에서 WebView와 같은 기타 컨트롤을 찾으세요. 자세한 내용은 [WPF 및 Windows Forms 애플리케이션에서 UWP 컨트롤 호스트](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)를 참조하세요.

### <a name="gaze-input-and-interactions"></a>응시 입력 및 조작

[눈의 위치와 움직임을 기반으로 사용자의 응시, 관심 및 현재 상태를 추적합니다.](../design/input/gaze-interactions.md) UWP 앱을 사용하고 상호 작용하는 이러한 강력하고 새로운 방법은 보조 기술로 특히 유용합니다. 또한 응시 입력은 게임(대상 획득 및 추적 포함)과 기존 입력 디바이스(예: 키보드, 마우스, 터치)를 사용할 수 없는 기타 대화형 시나리오에 유용한 기회를 제공합니다.

### <a name="msix-packaging-format"></a>MSIX 패키징 형식

Microsoft Build 2018 컨퍼런스에서 발표한 MSIX는 Win32, Windows Forms, WPF 및 UWP를 비롯한 모든 Windows 애플리케이션에 적용되는 새로운 컨테이너화 패키지 형식입니다. 이 새로운 형식은 UWP에서 다음과 같은 유용한 기능을 상속합니다.

* 강력한 설치 및 업데이트 
* 유연한 기능 시스템을 사용한 관리형 보안 모델
* Microsoft Store, 엔터프라이즈 관리 및 여러 사용자 지정 배포 모델 지원

이러한 패키지를 만들기 위한 도구는 향후 Visual Studio 및 Windows SDK 릴리스에서 사용할 수 있습니다.

MSIX 패키징 형식은 파트너가 해당 도구 및 솔루션으로 MSIX 에코 시스템을 보다 쉽게 지원할 수 있도록 하는 오픈 소스 형식입니다. MSIX 패키징 형식에 대한 자세한 내용은 [MSIX SDK](https://github.com/Microsoft/msix-packaging)를 참조하세요. 

![MSIX 패키징 이미지](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>실행 가능한 코드가 있는 옵션 패키지

앱의 선택적 패키지는 이제 실행 C# 코드를 포함할 수 있습니다. [Visual Studio를 사용하여 기본 앱 패키지를 지원하도록 선택적 추가 기능 패키지를 구성하는 방법을 알아봅니다](../packaging/optional-packages-with-executable-code.md).

### <a name="page-transitions"></a>페이지 전환

[페이지 전환](../design/motion/page-transitions.md)은 사용자를 앱의 페이지 간에 이동시킵니다. 이 기능은 탐색 계층 구조에서 사용자의 현재 위치를 이해하도록 하고 페이지 간 관계에 대한 피드백을 제공합니다.

### <a name="project-rome"></a>프로젝트 로마

프로젝트 로마 팀은 iOS 및 Android SDK를 철저히 점검하고, 사용자 활동과 같은 새로운 기능을 추가하고 많은 코드를 리팩터링하여 여러 다른 SDK에서 일관된 프로그래밍 환경을 제공하고 있습니다. [모든 새 API 참조 및 방법 문서](https://docs.microsoft.com/windows/project-rome/)가 Build 2018 개발자 컨퍼런스 동안 라이브로 선보일 예정입니다.

### <a name="sets"></a>설정

설정 기능은 Windows Insider Preview 빌드에서 사용할 수 있습니다. 설정 기능을 사용하면 앱이 다른 탭과 공유할 수 있는 창에 그려지며, 제목 표시줄에 각 앱의 고유한 탭이 표시됩니다. 

## <a name="developer-guidance"></a>개발자 지침

### <a name="get-started"></a>시작

새로운 학습 트랙을 사용하여 시작 콘텐츠를 개선했습니다. 이러한 새 항목은 새 Windows 10 개발자에게 수행하려는 몇 가지 일반적인 작업에 대한 정보를 제공하기 위해 작성되었습니다. 이러한 항목은 자습서도 아니고 간편한 연습 과정을 제공하지도 않으며, 기존 설명서가 있는 위치와 사용 방법을 알려줍니다. 개선된 [코딩 시작](../get-started/create-uwp-apps.md) 페이지를 확인하거나 각 개별 학습 트랙을 살펴보세요.

* [양식 작성](../get-started/construct-form-learning-track.md)
* [목록에 고객 표시](../get-started/display-customers-in-list-learning-track.md)
* [설정 저장 및 로드](../get-started/settings-learning-track.md)
* [파일 작업](../get-started/fileio-learning-track.md)

![시작 이미지](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>광고 성과 보고서

파트너 센터의 [광고 성과 보고서](../publish/advertising-performance-report.md)는 이제 가시성 메트릭을 제공합니다. 또한 광고 가시성을 최적화하기 위한 권장 사항을 제공하기 위해 [광고 단위의 가시성 최적화](../monetize/optimize-ad-unit-viewability.md) 문서를 추가했습니다.

### <a name="targeted-push-notifications"></a>대상 푸시 알림

파트너 센터의 [알림](../publish/send-push-notifications-to-your-apps-customers.md) 페이지는 모든 알림에 대한 추가 분석 데이터를 그래프와 세계 지도 보기로 제공합니다.

## <a name="videos"></a>비디오

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT는 Windows 런타임 API를 작성하고 사용하는 새로운 방법입니다. 이는 헤더 파일에서만 구현되고 최신 앱 기능에 대한 최고 수준의 액세스를 제공하도록 설계되었습니다. [동영상을 시청](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)하여 작동 원리를 파악한 후 [개발자 문서 읽어 보고](../cpp-and-winrt-apis/index.md) 자세한 내용을 확인하세요.

### <a name="multi-instance-uwp-apps"></a>다중 인스턴스 UWP 앱

Windows에서는 이제 UWP 앱의 여러 인스턴스를 각각 별도의 자체 프로세스로 실행할 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)하여 이 기능을 지원하는 새 앱을 만드는 방법을 알아보고 [개발자 문서를 읽어](../launch-resume/multi-instance-uwp.md) 이 기능을 사용하는 방법과 이유에 대한 추가 지침을 확인하세요.

## <a name="samples"></a>샘플

### <a name="customer-database-tutorial"></a>고객 데이터베이스 자습서

이 자습서는 고객 목록을 관리하기 위한 기본 UWP 앱을 만들고 엔터프라이즈 개발에서 유용한 개념 및 사례를 소개합니다. 로컬 SQLite 데이터베이스에 대한 UI 요소를 구현하고 작업을 추가하는 과정을 안내하며, 추가적인 작업을 원할 경우 원격 REST 데이터베이스에 연결하기 위한 대략적인 지침을 제공합니다. [여기에서 자습서를 확인하세요](../enterprise/customer-database-tutorial.md).