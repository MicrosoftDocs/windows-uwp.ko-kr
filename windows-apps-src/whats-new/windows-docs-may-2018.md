---
author: QuinnRadich
title: 5 월 2018의 Windows 문서에서 새로운-UWP 앱 개발 (영문)
description: 새로운 기능, 비디오, 및 개발자 지침 5 월 2018에 대 한 Windows 10 개발자 설명서 및 Microsoft 빌드 전화 회의에 추가 되었습니다.
keywords: 새로운 기능, 업데이트 하 고, 기능, 개발자 지침, Windows, 월, 10 빌드
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2822955"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>5 월 2018 Windows 개발자 문서에 새로운 소식

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 지침, 비디오 및 예제 (영문) 사용할 수 있는 [Microsoft 빌드 2018](https://www.microsoft.com/build) 개발자 회의와 일치 하는 월의 해당 월의 합니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="motion-in-fluent-design"></a>Fluent 디자인의 동작

Fluent 시스템 디자인의에서 동작 사용자 진화, 타이밍, 부드럽게, 방향, 및 gravity의 기본 사항을 기반으로 구축 합니다. 앱을 통해 사용자는 절차를 안내 하는 데 도움이 됩니다 고 자연 스러운을 반영 하 여 자신의 디지털 경험으로 연결 하 이러한 기본 사항을 적용 합니다. 이 문서에 자세히 설명 합니다.

* [The 모션 개요 (영문)는](../design/motion/index.md) 이러한 기본 사항을 반영 하도록 업데이트 되었습니다.
* [연습에서 모션](../design/motion/motion-in-practice.md) 앱 내에서 이러한 기본 사항을 적용 하는 방법의 예제를 제공 합니다.
* [Directionality 및 gravity](../design/motion/directionality-and-gravity.md) solidifies 앱의 사용자의 생각해 모델입니다.
* [타이밍 및 간편한](../design/motion/timing-and-easing.md) 현실감이 앱의 동작을 추가합니다.

![동작의 동작](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent 디자인 업데이트

시각적 업데이트 및 변경 된 관리자에 게 다음 Fluent 디자인 페이지:

* [맞춤, 여백 안쪽 여백](../design/layout/alignment-margin-padding.md)
* [색상](../design/style/color.md)
* [명령 기본 사항](../design/basics/commanding-basics.md)
* [Windows 응용 프로그램에 대 한 fluent 디자인](../design/fluent-design-system/index.md)
* [응용 프로그램 디자인 소개](../design/basics/design-and-ui-intro.md)
* [탐색 기본 사항](../design/basics/navigation-basics.md)
* [반응형 디자인 기술](../design/layout/responsive-design.md)
* [화면 크기 및 중단점](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [스타일 개요 (영문)](../design/style/index.md)
* [쓰기 스타일](../design/style/writing-style.md)

또한 자신의 콘텐츠 영역에 있는 모든 새 정보를 사용 하 여 다음 페이지를 다시 작성 했을 때 했습니다.

* [아이콘](../design/style/icons.md) 에 이제 아이콘을 사용 하 고 클릭할 수 있도록 하는 것에 대 한 실제적인 권장 사항을 제공 합니다.
* [입력 체계](../design/style/typography.md) 업데이트 된 지침 및 그림 사용 하 여 단일 위치에 있는 모든 항목에 두면 비슷한 문서에서 정보를 통합 합니다.

![색 색상표 이미지](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Visual Studio에서 앱 설치 관리자 파일

이제 Visual Studio 2017, 업데이트 15.7와 app 설치 관리자 파일을 만들 수 있습니다. 앱을 자동 업데이트를 [App 설치 관리자 파일을 만들려면 Visual Studio를 사용 하는 방법을 설명](../packaging/create-appinstallerfile-vs.md) 하 고 사용 하도록 설정 합니다. 문제를 실행 하는 경우 일반 문제 및 솔루션을 보려면 [앱 설치 관리자 파일와 설치 문제를 해결](../packaging/troubleshoot-appinstaller-issues.md) 를 참조 합니다.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Windows Forms 및 WPF 응용 프로그램에 대 한 웹 보기 컨트롤에 지

이전에 사용할 수 있는 UWP 응용 프로그램에 웹 보기 컨트롤을 사용 하 여 데스크톱 응용 프로그램에 웹 콘텐츠를 표시 합니다. 이 컨트롤 렌더링 엔진 취약점 렌더링에 서식이 HTML는 보기를 포함 하는 원격 웹 서버, 동적으로 생성 된 코드 또는 콘텐츠 파일에서 콘텐츠 Microsoft 지를 사용 합니다. 컨트롤을 찾은 후 웹 보기의 최신 릴리스에서 [Windows 커뮤니티 Toolkit.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

웹 보기 Windows 커뮤니티 Toolkit의 이후 릴리스에서 동일 하 게 다른 컨트롤에 대 한 정보를 제공 합니다. 자세한 내용은 참조 [WPF 및 Windows Forms 응용 프로그램에서 컨트롤을 호스트 UWP.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>게이즈의 입력 사항 및 상호작용

[눈의 위치와 움직임을 기반으로 사용자의 응시, 관심 및 현재 상태를 추적합니다.](../design/input/gaze-interactions.md) 이 강력한 새로운 방식으로 사용 하 고 UWP 앱와 상호작용 하는 보조 기술으로 특히 유용 합니다. 또한 게이즈 입력 게임 (대상 취득을 포함 하 고 추적)와 일반 입력된 장치 (키보드, 마우스, 터치) 사용할 수 없는 다른 대화형 시나리오에 대 한 중요 한 기회를 제공 합니다.

### <a name="msix-packaging-format"></a>MSIX 패키징 형식

Microsoft 빌드 2018 회의에서 발표, MSIX은 Win32, Windows Forms, WPF 및 UWP를 포함 하는 모든 Windows 응용 프로그램에 적용 되는 새로운 containerization 패키지 형식입니다. 이 새로운 형식 UWP에서 유용한 기능을 상속합니다.

* 강력한 설치 및 업데이트 합니다. 
* 유연한 기능 시스템과 보안 모델을 관리합니다.
* 많은 사용자 지정 배포 모델, 엔터프라이즈 관리 및 Microsoft 저장소에 대 한 지원 합니다.

이러한 패키지를 만드는 도구는 Visual Studio 및 Windows sdk (영문)의 향후 릴리스에서 사용할 수 있습니다.

MSIX 패키징 형식은 파트너가 도구 및 솔루션 MSIX 에코 시스템을 지원 하도록 하기 위한 보다 쉽게 하는 오픈 소스 형식이입니다. MSIX 패키징 형식에 대 한 자세한 내용을 보려면 [MSIX sdk (영문)을](https://github.com/Microsoft/msix-packaging)참조 하십시오. 

![MSIX 패키징 이미지](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>실행 가능한 코드가 있는 옵션 패키지

이제 앱의 선택적 패키지에는 실행 C# 코드 포함 될 수 있습니다. [Visual Studio를 사용 하 여 주 응용 프로그램 패키지를 지원 하기 위해 선택적 추가 기능 패키지를 구성 하는 방법에 알아봅니다.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>페이지 전환

[페이지를 전환](../design/motion/page-transitions.md) 응용 프로그램의 페이지 사이 사용자를 이동 합니다. 사용자가 탐색 계층 구조를 이해 하 고 페이지 간의 관계에 대 한 의견을 제공 이용 하 여.

### <a name="project-rome"></a>프로젝트 로마

프로젝트 로마 팀을 자신의 iOS 및 Android Sdk, 사용자 작업의 새로운 기능을 추가 하 고 다른 Sdk 전체에서 일관성 있는 프로그래밍 환경을 제공 하는 코드의 대부분이 리팩터링 전체적으로 개선 했습니다. [모든 새 API 참조 및 사용 방법 문서](https://docs.microsoft.com/windows/project-rome/) 들어갈 수 라이브 빌드 2018 개발자 회의 중입니다.

### <a name="sets"></a>집합

집합 기능은 Windows 내부 인 시험판 빌드에서 사용할 수 있습니다. 집합 기능을 사용할 때 하면 응용 프로그램에서 자체 탭 제목 표시줄에 있는 각 app 다른 응용 프로그램으로 공유할 수 있는 창으로 그려집니다. 설정 하는 UI의 최상의 환경을 제공 하기 위해 앱을 최적화 하는 방법에 대 한 지침을는 [집합에 대 한 디자인](../design/shell/design-for-sets.md) 합니다.

## <a name="developer-guidance"></a>개발자 지침

### <a name="get-started"></a>시작

이 Get revitalized 했을 때는 새 학습 추적을 사용 하 여 콘텐츠를 시작 합니다. 이러한 새 항목을 수행 해야할 몇가지 일반적인 작업에 대 한 정보가 새 Windows 10 개발자에 게 제공 하는 목표입니다. 자습서 모르는 및 핸드 연습을 제공 하지 되지만 대신 기존 설명서 존재 하는 위치 및 사용 하는 방법을 나타내도록 합니다. 개선 된 [코딩 시작](../get-started/create-uwp-apps.md) 체크아웃 페이지 또는 각 개별 학습 추적을 탐색 하십시오.

* [양식 작성](../get-started/construct-form-learning-track.md)
* [목록에서 고객 표시](../get-started/display-customers-in-list-learning-track.md)
* [설정 저장 및 로드](../get-started/settings-learning-track.md)
* [파일 작업](../get-started/fileio-learning-track.md)

![시작된 이미지 가져오기](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>광고 성과 보고서

개발자 센터 대시보드에서 [광고 성능 보고서](../publish/advertising-performance-report.md) 에 이제 가시성 메트릭을 제공합니다. 또한 하므로 광고의 가시성을 최적화 하기 위한 권장 사항을 제공 하기 위해 [최적화 ad 단위 가시성](../monetize/optimize-ad-unit-viewability.md) 문서를 추가 했습니다.

### <a name="targeted-push-notifications"></a>대상이 지정 된 푸시 알림

개발자 센터 대시보드에서 [알림](../publish/send-push-notifications-to-your-apps-customers.md) 페이지는 이제 그래프와 세계 지도 보기에 모든 알림에 대 한 추가 분석 데이터를 제공합니다.

## <a name="videos"></a>비디오

### <a name="cwinrt"></a>C++/WinRT

C + + / WinRT 제작 및 Windows 런타임 Api 사용 (영문)의 새로운 방법이 있습니다. 단독 헤더 파일에서 구현 하는 및 현대 app 기능에 대 한 고급 액세스를 제공 하도록 설계 되었습니다. 작동 방식, 다음 [개발자 문서를 읽기](../cpp-and-winrt-apis/index.md) 추가 정보에 대 한 자세한 [비디오를 시청](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) 합니다.

### <a name="multi-instance-uwp-apps"></a>다중 인스턴스 UWP 앱

이제 Windows을 사용 하면 자체 별도 프로세스에서 각 UWP 앱의 여러 인스턴스를 실행할 수 있습니다. [비디오를 시청](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) 하는 방법에 대 한 자세한 지침에 대 한이 기능을 다음 [개발자 문서를 읽기](../launch-resume/multi-instance-uwp.md) 를 지 원하는 새로운 응용 프로그램을 만드는 방법 및이 기능을 사용 하는 이유에 대해 알아봅니다.

## <a name="samples"></a>샘플

### <a name="customer-database-tutorial"></a>고객 데이터베이스 자습서 (영문)

이 자습서의 고객 목록을 관리 하기 위한 기본 UWP 응용 프로그램을 만들고 개념 및 엔터프라이즈 개발에 유용한 방법을 소개 하 고 있습니다. UI 요소를 구현 하 고 로컬 SQLite 데이터베이스에 대 한 작업 추가 (영문)을 통해 안내해 하 고 추가로 이동 하려는 경우 원격 나머지 데이터베이스에 연결 하는 것에 대 한 넓게 지침을 제공 합니다. [여기에 자습서를 체크아웃](../enterprise/customer-database-tutorial.md)