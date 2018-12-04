---
title: 2018 년 5 월 Windows 문서의 새로운-UWP 앱 개발
description: 새로운 기능, 동영상 및 개발자 지침 2018 년 5 월에 대 한 Windows 10 개발자 설명서 및 Microsoft 빌드 회의에 추가한 합니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 월, 빌드
ms.date: 5/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 20bb514a15963befb5b96a1b01a6c057e8f27482
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485020"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>2018 년 5 월 Windows 개발자 문서의 새로운

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침, 동영상 및 샘플 내용이 [Microsoft 빌드 2018](https://www.microsoft.com/build) 개발자 회의와 일치 하는 월 한 달에 사용할 수 있습니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="motion-in-fluent-design"></a>흐름 디자인의 동작

Fluent 디자인 시스템의 동작의 사용자는 개선 타이밍, 감속/가속, 방향 및 무게는 기본 정보를 기반으로 합니다. 사용자에 게 앱을 안내 하는 데 도움이 됩니다 이러한 기본 개념을 적용 하 고 자연 스러운을 반영 하 여 디지털 환경으로 연결 합니다. 이 문서에서 자세히 알아봅니다.

* [The 움직임 개요](../design/motion/index.md) 이러한 기본 사항을 반영 하도록 업데이트 되었습니다.
* 앱 내에서 이러한 기본 개념을 적용 하는 방법의 예제를 제공 하는 [연습에서 동작](../design/motion/motion-in-practice.md) 합니다.
* [방향 및 무게](../design/motion/directionality-and-gravity.md) 대 앱의 사용자의 멘 탈 모델을 구체화 합니다.
* [타이밍 및 감속/가속](../design/motion/timing-and-easing.md) 현실감을 앱의 동작을 추가합니다.

![동작 중인](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent 디자인 업데이트

시각적 업데이트 및 사소 다음 Fluent 디자인 페이지가에 적용 되었습니다.

* [맞춤, 여백, 안쪽 여백](../design/layout/alignment-margin-padding.md)
* [색상](../design/style/color.md)
* [명령 기본 사항](../design/basics/commanding-basics.md)
* [Windows 앱 용 fluent 디자인](../design/fluent-design-system/index.md)
* [앱 디자인 소개](../design/basics/design-and-ui-intro.md)
* [탐색 기본 사항](../design/basics/navigation-basics.md)
* [반응형 디자인 기술](../design/layout/responsive-design.md)
* [화면 크기 및 중단점](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [스타일 개요](../design/style/index.md)
* [쓰기 스타일](../design/style/writing-style.md)

또한 해당 콘텐츠 영역에 대 한 완전히 새로운 정보를 사용 하 여 다음 페이지를 작성해 했습니다.

* [아이콘](../design/style/icons.md) 에 이제 아이콘을 사용 하 고 클릭할 수 있도록에 대 한 유용한 권장 사항을 제공 합니다.
* [입력 체계](../design/style/typography.md) 업데이트 된 지침 및 그림을 사용 하 여 단일 위치에 배치 하는 모든 유사한 문서에서 정보를 통합 합니다.

![색 색상표 이미지](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio에서 앱 설치 관리자 파일

이제 Visual Studio 2017 업데이트 15.7를 사용 하 여 앱 설치 관리자 파일을 만들 수 있습니다. 앱에 자동 업데이트를 사용 및 [앱 설치 관리자 파일을 만들려면 Visual Studio를 사용 하는 방법을 알아봅니다](../packaging/create-appinstallerfile-vs.md) . 문제를 실행 하는 경우 일반적인 문제 및 솔루션을 보려면 [앱 설치 관리자 파일 관련 설치 문제 해결](../packaging/troubleshoot-appinstaller-issues.md) 을 참조 하세요.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>WPF 및 Windows Forms 응용 프로그램에 대 한 WebView 컨트롤에 지

데스크톱 응용 프로그램에 UWP 응용 프로그램에만 사용 가능 이전에 웹 보기 컨트롤을 사용 하 여 웹 콘텐츠를 표시 합니다. 이 컨트롤은 Microsoft Edge 렌더링 엔진 취약점 렌더링에 풍부한 HTML 형식에 대 한 보기를 포함 하는 원격 웹 서버, 동적으로 생성 된 코드 또는 콘텐츠 파일에서 콘텐츠를 사용 합니다. WebView 컨트롤의 최신 버전에서 찾기는 [Windows 커뮤니티 도구 키트.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

WebView Windows 커뮤니티 도구 키트의 이후 릴리스에서 같은 다른 컨트롤을 찾습니다. 자세한 내용은 참조 [WPF 및 Windows Forms 응용 프로그램의 호스트 UWP 컨트롤.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>응시 입력 및 조작

[눈의 위치와 움직임을 기반으로 사용자의 응시, 관심 및 현재 상태를 추적합니다.](../design/input/gaze-interactions.md) 이 강력한 새로운 방법을 사용 하 여 UWP 앱과 상호 작용 하는 보조 기술로 특히 유용 합니다. 응시 입력은 게임 (대상 획득 및 추적) 및 기존 입력된 장치 (키보드, 마우스, 터치) 사용할 수 없는 기타 대화형 시나리오에 매력적인 기회를 제공 합니다.

### <a name="msix-packaging-format"></a>MSIX 패키징 형식

Microsoft 빌드 2018 회의에서 발표, MSIX는 Win32, Windows Forms, WPF 및 UWP를 비롯 한 모든 Windows 응용 프로그램에 적용 되는 새 컨테이너 화 패키지 형식입니다. 이 새로운 형식은 UWP에서 멋진 기능을 상속합니다.

* 강력한 설치 및 업데이트 합니다. 
* 유연한 접근 권한 값은 시스템을 사용 하 여 보안 모델을 관리합니다.
* Microsoft Store, 엔터프라이즈 관리 및 많은 사용자 지정 배포 모델에 대 한 지원 합니다.

이러한 패키지를 만들 수 있는 도구는 Visual Studio 및 Windows SDK의 이후 릴리스에서 사용할 수 있습니다.

Msix 패키징 도구 및 솔루션을 사용 하 여 MSIX 에코 시스템을 지원 하기 위해 파트너에 대 한 쉽게 오픈 소스 형식이입니다. MSIX 패키징 형식에 대 한 자세한 내용을 보려면 [MSIX SDK](https://github.com/Microsoft/msix-packaging)를 참조 하세요. 

![MSIX 패키징 이미지](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>실행 가능한 코드가 있는 옵션 패키지

이제 앱에서 선택적 패키지에는 실행 C# 코드 포함 될 수 있습니다. [Visual Studio를 사용 하 여 메인 앱 패키지를 지원 하기 위해 선택적 추가 기능 패키지를 구성 하는 방법을 알아봅니다.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>페이지 전환

[페이지 전환](../design/motion/page-transitions.md) 은 앱 페이지 사이 사용자를 탐색 합니다. 탐색 계층 구조에 속해 이해 하 고 페이지 간의 관계에 대 한 피드백을 제공 하는 사용자가 도움이 됩니다.

### <a name="project-rome"></a>프로젝트 로마

자신의 iOS 및 Android Sdk에 사용자 활동의 새로운 기능을 추가 하 고 다른 Sdk에서 일관 된 프로그래밍 환경을 제공 하기 위해 해당 코드를 리팩터링 프로젝트 "로마" 팀에 전체적으로 개선 합니다. [모든 새 API 참조 및 사용 방법 문서](https://docs.microsoft.com/windows/project-rome/) 빌드 2018 개발자 회의 중 라이브 갈 합니다.

### <a name="sets"></a>설정

Sets 기능은 Windows Insider preview 빌드에 사용할 수 있습니다. Sets 기능을 사용할 때 앱의 자체 탭 제목 표시줄에 있는 각 앱을 사용 하 여 다른 앱과 공유할 수 있는 창으로 그려집니다. 

## <a name="developer-guidance"></a>개발자 지침

### <a name="get-started"></a>시작

우리의 Get revitalized 한에서는 새 학습 트랙을 사용 하 여 콘텐츠를 시작 합니다. 새 항목에서는 이러한 작업을 수행 하려고 몇 가지 일반적인 작업에 대 한 정보를 사용 하 여 새로운 Windows 10 개발자에 게 제공을 목표로 합니다. 바로 자습서 알지 못하는 및 핸드 연습을 제공 하지 있지만 대신 기존 설명서가 있는 경우 및 사용 하는 방법을 가리킵니다. [코딩 시작](../get-started/create-uwp-apps.md) 혁신적 확인 페이지 또는 각 개별 학습 트랙을 살펴보세요.

* [양식 작성](../get-started/construct-form-learning-track.md)
* [목록에서 고객 표시](../get-started/display-customers-in-list-learning-track.md)
* [설정 저장 및 로드](../get-started/settings-learning-track.md)
* [파일 작업](../get-started/fileio-learning-track.md)

![시작된 이미지 가져오기](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>광고 성과 보고서

이제 파트너 센터에서 [광고 성과 보고서](../publish/advertising-performance-report.md) 가시성 메트릭을 제공합니다. [광고 단위 가시성 최적화](../monetize/optimize-ad-unit-viewability.md) 문서 광고 가시성 최적화에 대 한 권장 사항을 제공 하기 위해 추가 했습니다.

### <a name="targeted-push-notifications"></a>대상된 푸시 알림

이제 파트너 센터에서 [알림](../publish/send-push-notifications-to-your-apps-customers.md) 페이지 그래프 및 세계 지도 보기에서 모든 알림을 대 한 자세한 분석 데이터를 제공합니다.

## <a name="videos"></a>비디오

### <a name="cwinrt"></a>C++/WinRT

C + + /winrt는 Windows 런타임 api를 작성 하는 새로운 방법입니다. 헤더 파일을 위한 유일한 구현 하 고 최신 앱 기능에 최고 수준의 액세스를 제공 하도록 설계 된가 합니다. 작동 방식, 그런 다음 [개발자 문서를 읽거나](../cpp-and-winrt-apis/index.md) 자세한 정보에 대 한 자세한 [비디오를 시청 하세요](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) .

### <a name="multi-instance-uwp-apps"></a>다중 인스턴스 UWP 앱

이제 Windows을 사용 하면 각각 별도 프로세스로 자체를 사용 하 여 UWP 앱의 여러 인스턴스를 실행할 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) 하는 방법에 대 한 자세한 내용은이 기능을 다음 [개발자 문서를 읽거나](../launch-resume/multi-instance-uwp.md) 를 지 원하는 새로운 앱을 만드는 방법을 알아보고이 기능을 사용 하는 이유입니다.

## <a name="samples"></a>샘플

### <a name="customer-database-tutorial"></a>고객 데이터베이스 자습서

이 자습서의 고객 목록을 관리 하는 기본 UWP 앱을 만들고 개념 및 엔터프라이즈 개발에 유용한 방법을 소개 합니다. UI 요소를 구현 하 여 로컬 SQLite 데이터베이스에 대해 작업을 추가 검색 하 고 더 나아가 하려는 경우 원격 나머지 데이터베이스에 연결 하기 위한 느슨한 지침을 제공 합니다. [다음 자습서를 확인](../enterprise/customer-database-tutorial.md)