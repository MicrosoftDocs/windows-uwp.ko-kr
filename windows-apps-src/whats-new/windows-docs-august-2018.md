---
title: 에서 새로운 기능 Windows Docs에서 2018 년 8 월-UWP 앱 개발
description: 새로운 기능, 비디오, 샘플 및 개발자 지침 2018 년 8 월에 대 한 Windows 10 개발자 설명서에 추가한 합니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 가이드, Windows 10 년 8 월
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616488"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>2018 년 8 월에에서 새로운 Windows 개발자 문서

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 지침 및 비디오에서 8 월에 제공 되었습니다.

Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="design"></a>디자인

다음 기능은에 추가한는 Windows Insider Preview 빌드를 통해 사용할 수는 [Windows Insider](https://insider.windows.com/) 프로그램입니다.

* 합니다 [Windows UI 라이브러리](https://aka.ms/winui-docs) 는 UWP 앱에 대 한 컨트롤 및 기타 사용자 interfact 요소를 제공 하는 NuGet 패키지의 집합입니다. 이러한 패키지는 시험판 버전의 Windows 10과 호환도 앱 사용자가 최신 OS 버전을가지고 있지 하는 경우에 작동 합니다.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button), 및 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 앱의 사용자 인터페이스를 향상 시키기 위해 특별 한 기능을 사용 하 여 단추 컨트롤을 제공 합니다.

![전경색을 선택 하기 위한 분할 단추](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 지원 [탐색 상위](../design/controls-and-patterns/navigationview.md), 경우 앱에 더 작은 수의 탐색 옵션 및 앱의 콘텐츠에 대 한 더 많은 공간이 필요 합니다.

* TreeView를 지원 하도록 향상 되었습니다 [데이터 바인딩, 항목 템플릿, 끌어서 놓습니다.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>패키지 지원 프레임 워크

패키지 지원 프레임 워크 MSIX 컨테이너에서 실행할 수 있도록 소스 코드에 액세스할 수 없을 때 수정 win32 응용 프로그램에 적용 하는 데 도움이 되는 오픈 소스 키트입니다.

자세한 내용은 참조 하세요 [적용 런타임 패키지 지원 프레임 워크를 사용 하 여 MSIX 패키지에 수정](../porting/package-support-framework.md)합니다.

## <a name="developer-guidance"></a>개발자 지침

### <a name="web-api-extensions"></a>웹 API 확장

목록을 [레거시 Microsoft API 확장](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) 교차 브라우저 웹 개발에 대 한 Mozilla Developer Network 설명서에 추가 되었습니다. 이러한 API는 Internet Explorer 또는 Microsoft Edge에 고유한 확장과 MDN 웹 문서에서 호환성 및 브라우저 지원에 대 한 기존 정보를 보완 합니다. 레거시 Microsoft [CSS 확장](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) 하 고 [JavaScript 확장](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) 도 사용할 수 하 고 다양 한 웹 API 정보에서 MDN에 직접 표시를 찾을 수 [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + /cli WinRT 코드 예제

250 추가 했습니다 [C + + WinRT](../cpp-and-winrt-apis/index.md) 코드 목록 항목에서 문서를 함께 제공 되는 기존 C + + /cli CX 코드 예제.

### <a name="project-rome"></a>프로젝트 로마

합니다 [프로젝트 로마 docs](https://docs.microsoft.com/windows/project-rome/) 사이트 기능 중심으로 재구성 되었습니다에 있습니다. 이 쉽게 개발자가 여러 플랫폼에서 원하는 기능을 구현 하 고, 원하는 내용을 찾을 수 있습니다.

## <a name="videos"></a>비디오

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 플러그 인

Unity에 대 한 Xbox Live 플러그 인을 제목에 서명 Xbox Live, 통계, 친구 목록, 클라우드 저장소 및 순위표를 추가 하는 것에 대 한 지원을 포함 합니다. [비디오를 시청](https://youtu.be/fVQZ-YgwNpY) 다음 자세히 알아보려면 [GitHub 패키지 다운로드](https://aka.ms/UnityPlugin) 시작 하려면.

### <a name="one-dev-question"></a>개발 질문 중 하나

개발 질문 중 하나는 비디오 시리즈에서는 해온 Microsoft 개발자에 게 일련의 Windows 개발, 팀 문화권 및 기록 하는 방법에 대 한 질문을 다룹니다. 최신 질문 대답할 것에 다음과 같습니다.

Raymond Chen:

* [커널은 어떻게 확인 합니까 비디오 드라이버를 다시 시작 하는 경우?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Windows에서 Burgermaster 개체 비하인드 란?](https://youtu.be/0TDSbyAIvX0)