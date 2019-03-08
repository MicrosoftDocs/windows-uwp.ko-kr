---
title: 에서 새로운 기능 Windows Docs 2018 년 5 월-UWP 앱 개발
description: 새로운 기능, 비디오 및 개발자 지침 2018 년 5 월에 대 한 Windows 10 개발자 설명서 및 Microsoft Build 컨퍼런스 추가한 합니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 가이드, Windows 10, 월, 빌드
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598788"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>에서 새로운 기능 Windows 개발자 문서 2018 년 5 월

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 가이드, 비디오 및 샘플 사용할 수 있게 된와 일치 하는 월의 달에는 [Microsoft Build 2018](https://www.microsoft.com/build) 개발자 회의입니다.

Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="motion-in-fluent-design"></a>Fluent 디자인 동작

Fluent Design System 동작의 사용자는 진화 하 고, 타이밍, 감속/가속, 방향이 있으며 및 중력의 기본적인 사항을 기반으로 합니다. 이러한 기본 적용 사용자에 게 앱을 안내 하는 데 도움이 됩니다 하 고 자연 스러운을 반영 하 여 디지털 경험을 사용 하 여 연결 합니다. 이 문서에서 자세히 알아보세요.

* [동작 개요](../design/motion/index.md) 이러한 기본 사항에 맞게 업데이트 되었습니다.
* [연습에서 동작](../design/motion/motion-in-practice.md) 앱 내에서 이러한 기본을 적용 하는 방법의 예제를 제공 합니다.
* [방향 및 중력](../design/motion/directionality-and-gravity.md) solidifies 앱의 사용자 멘 탈 모델입니다.
* [시간 및 감속/가속](../design/motion/timing-and-easing.md) 앱의 동작에 현실성을 추가 합니다.

![작업 동작](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent 디자인 업데이트

시각적 개체 업데이트 및 사소한 변경은 다음 Fluent 디자인 페이지에 적용 했습니다.

* [맞춤, 여백, 안쪽 여백](../design/layout/alignment-margin-padding.md)
* [색](../design/style/color.md)
* [명령 기본 사항](../design/basics/commanding-basics.md)
* [Windows 앱 용 Fluent 디자인](../design/fluent-design-system/index.md)
* [앱 디자인 소개](../design/basics/design-and-ui-intro.md)
* [탐색 기본 사항](../design/basics/navigation-basics.md)
* [반응 형 디자인 기술](../design/layout/responsive-design.md)
* [화면 크기 및 중단점](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [스타일 개요](../design/style/index.md)
* [필기 스타일](../design/style/writing-style.md)

또한 자신의 콘텐츠 영역에 있는 모든 새 정보를 사용 하 여 다음 페이지를 다시 작성 되었습니다 했습니다.

* [아이콘](../design/style/icons.md) 아이콘을 사용 하 고 클릭할 수 있도록 하는 것에 대 한 실용적인 권장 사항을 제공 합니다.
* [입력 체계](../design/style/typography.md) 업데이트 된 지침과 그림을 사용 하 여 한 곳에서 모든 항목을 저장 하는 유사한 문서에서 정보를 통합 합니다.

![색 색상표 이미지](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio에서 앱 설치 관리자 파일

이제 Visual Studio 2017 15.7 업데이트를 사용 하 여 앱 설치 관리자 파일을 만들 수 있습니다. [Visual Studio를 사용 하 여 앱 설치 관리자 파일을 만드는 방법에 알아봅니다](../packaging/create-appinstallerfile-vs.md) 앱 자동 업데이트를 사용 하도록 설정 합니다. 문제를 실행 하는 경우 참조 [앱 설치 관리자 파일을 사용 하 여 설치 문제 해결](../packaging/troubleshoot-appinstaller-issues.md) 일반적인 문제 및 솔루션을 볼 수 있습니다.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Windows Forms 및 WPF 응용 프로그램에 대 한 WebView 컨트롤에 지

UWP 응용 프로그램 에서만 사용할 수 있던 WebView 컨트롤을 사용 하 여 데스크톱 응용 프로그램에서 웹 콘텐츠를 표시 합니다. 이 제어는 렌더링 서식 있는 HTML 뷰를 포함 하는 엔진을 원격 웹 서버, 동적으로 생성 된 코드 또는 콘텐츠 파일의 콘텐츠를 렌더링 하는 Microsoft Edge를 사용 합니다. 최신 버전의에서 WebView 컨트롤을 찾는 [Windows 커뮤니티 도구 키트입니다.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

WebView Windows 커뮤니티 도구 키트의 이후 릴리스에서 같은 다른 컨트롤을 찾습니다. 자세한 내용은 참조 하세요. [호스트 UWP WPF 및 Windows Forms 응용 프로그램에서 제어 합니다.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>응시 입력 및 상호 작용

[사용자의 게이즈, 주의가 및 위치와의 시각의 움직임을 기반으로 현재 상태를 추적 합니다.](../design/input/gaze-interactions.md) 이 강력한 새 방법을 사용 하 여 UWP 앱 상호 작용 하는 보조 기술로 특히 유용 합니다. 응시 입력도 게임 (대상 인수를 포함 하 여 및 추적)와 기존 입력된 장치 (예: 키보드, 마우스, 터치)를 사용할 수 없는 다른 대화형 시나리오에 대 한 멋진 기회를 제공 합니다.

### <a name="msix-packaging-format"></a>MSIX 패키징 형식

Microsoft Build 2018 컨퍼런스에서 발표 MSIX은 Win32, Windows Forms, WPF 및 UWP를 포함 하 여 모든 Windows 응용 프로그램에 적용 되는 새 컨테이너 화 패키지 형식. 이 새로운 형식은 UWP에서 뛰어난 기능을 상속합니다.

* 강력한 설치 및 업데이트 합니다. 
* 유연한 기능 시스템을 사용 하 여 보안 모델을 관리 합니다.
* Microsoft Store, 엔터프라이즈 관리 및 기타 사용자 지정 배포 모델을 지원 합니다.

이러한 패키지를 만들기 위한 도구를 Visual Studio 및 Windows SDK의 향후 릴리스에서 사용할 수 있습니다.

MSIX 패키징 형식은 쉽게 해당 도구 및 솔루션을 사용 하 여 MSIX 에코 시스템을 지원 하기 위해 파트너는 오픈 소스 형식이입니다. MSIX 패키징 형식에 대 한 자세한 내용은 참조 하세요 [MSIX SDK](https://github.com/Microsoft/msix-packaging)합니다. 

![MSIX 패키징 이미지](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>실행 가능한 코드가 있는 옵션 패키지

선택적 패키지를 앱에서 이제 실행 파일을 포함할 수 있습니다 C# 코드입니다. [기본 앱 패키지를 지원 하기 위해 선택적 추가 기능 패키지 구성에 Visual Studio를 사용 하는 방법에 알아봅니다.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>페이지 전환

[페이지 전환](../design/motion/page-transitions.md) 사용자 앱에서 페이지 사이 이동 합니다. 지 사용자가 탐색 계층 구조에서 현재 위치를 이해 하 고 페이지 간 관계에 대 한 피드백을 제공 합니다.

### <a name="project-rome"></a>프로젝트 로마

프로젝트 로마 팀이 iOS 및 Android Sdk, 사용자 활동의 새로운 기능을 추가 하 고 대부분의 다른 Sdk에서 일관 된 프로그래밍 환경을 제공 하도록 코드를 리팩터링에서 철저히 점검 합니다. [모든 새 API 참조 및 방법 문서](https://docs.microsoft.com/windows/project-rome/) Build 2018 개발자 컨퍼런스에서 라이브로 전환 합니다.

### <a name="sets"></a>집합

설정 기능은 Windows Insider preview 빌드에 사용할 수 있습니다. 집합 기능을 사용 하는 경우 앱 자체 탭 제목 표시줄에 있는 각 앱과 다른 앱과 공유할 수 있는 창으로 그려집니다. 

## <a name="developer-guidance"></a>개발자 지침

### <a name="get-started"></a>시작

이 Get revitalized 했습니다에서는 새 학습 트랙을 사용 하 여 콘텐츠를 시작 합니다. 이러한 새 항목 수행 하려는 몇 가지 일반적인 작업에 대 한 정보를 사용 하 여 새 Windows 10 개발자에 게 제공 하려고 합니다. 이러한 자습서가 아닙니다 및 핸드헬드 연습을 제공 하지 않습니다 하지만 대신 기존 설명서가 있는 및 사용 하는 방법을 가리킵니다. 개선 된 확인해 [코딩을 시작](../get-started/create-uwp-apps.md) 페이지 또는 각 개별 학습 트랙 탐색:

* [양식 생성](../get-started/construct-form-learning-track.md)
* [목록에서 고객에 게 표시](../get-started/display-customers-in-list-learning-track.md)
* [저장 및 로드 설정](../get-started/settings-learning-track.md)
* [파일 작업](../get-started/fileio-learning-track.md)

![시작된 이미지 가져오기](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>광고 성과 보고서

합니다 [성능 보고서를 광고](../publish/advertising-performance-report.md) 파트너 센터에서 이제 가시성 메트릭을 제공 합니다. 또한 추가 합니다 [ad 단위를 사용 하 여 가시성을 최적화](../monetize/optimize-ad-unit-viewability.md) 광고의 가시성을 최적화 하기 위한 권장 사항을 제공 하는 문서입니다.

### <a name="targeted-push-notifications"></a>대상 푸시 알림

합니다 [알림을](../publish/send-push-notifications-to-your-apps-customers.md) 파트너 센터의 페이지 그래프와 세계 지도 보기에서 모든 알림의 이제 추가 분석 데이터를 제공 합니다.

## <a name="videos"></a>비디오

### <a name="cwinrt"></a>C++/WinRT

C + + /cli WinRT는 작성 하 고 Windows 런타임 Api를 사용 하는 새로운 방법입니다. 헤더 파일의 유일한 구현 되 고 최신 앱 기능에 대 한 최고 수준의 액세스를 제공 하도록 설계 되었습니다. [비디오를 시청](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) 원리, 다음에 대해 알아보려면 [개발자 문서 읽기](../cpp-and-winrt-apis/index.md) 자세한 정보에 대 한 합니다.

### <a name="multi-instance-uwp-apps"></a>다중 인스턴스 UWP 앱

이제 Windows을 사용 하면 각각 별도 자체 프로세스를 사용 하 여 UWP 앱의 여러 인스턴스를 실행할 수 있습니다. [비디오를 시청](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) 한 후이 기능을 지 원하는 새 앱을 만드는 방법에 알아보려면 [개발자 문서 읽기](../launch-resume/multi-instance-uwp.md) 하는 방법에 대 한 자세한 지침 및이 기능을 사용 하는 이유.

## <a name="samples"></a>샘플

### <a name="customer-database-tutorial"></a>고객 데이터베이스 자습서

이 자습서 고객 목록을 관리 하기 위한 기본 UWP 앱을 만들고 개념 및 엔터프라이즈 개발에 유용한 사례를 소개 합니다. UI 요소를 구현 하 고 로컬 SQLite 데이터베이스에 대해 작업 추가 안내 하 고 진행 하려는 경우 원격 나머지 데이터베이스에 연결 하기 위한 느슨한 지침을 제공 합니다. [여기에서 자습서 확인](../enterprise/customer-database-tutorial.md)