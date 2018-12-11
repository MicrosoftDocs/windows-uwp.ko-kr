---
title: 2018 년 8 월 Windows 문서의 새로운-UWP 앱 개발
description: 새로운 기능, 동영상, 샘플 및 개발자 지침 2018 년 8 월에 대 한 Windows 10 개발자 설명서에 추가한 합니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 월
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: da8bc3b441a1b619e086934f277cb14be6bcc37a
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883659"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>2018 년 8 월 Windows 개발자 문서의 새로운

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침 및 동영상 내용이 월 8 월에에서 사용할 수 있습니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="design"></a>디자인

다음과 같은 기능이 추가 되었습니다 windows Insider Preview 빌드를 [Windows 참가자](https://insider.windows.com/) 프로그램을 통해 사용할 수 있습니다.

* [Windows UI 라이브러리](https://aka.ms/winui-docs) 는 UWP 앱에 대 한 컨트롤 및 기타 사용자 interfact 요소를 제공 하는 NuGet 패키지의 집합입니다. 이러한 패키지는 Windows 10의 이전 버전과 호환도 앱이 사용자에 게 최신 OS 버전 없는 경우에 작동 합니다.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [분할 단추](../design/controls-and-patterns/buttons.md#create-a-split-button)및 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 앱의 사용자 인터페이스를 향상 하기 위해 특수 기능을 사용 하 여 단추 컨트롤을 제공 합니다.

![전경 색을 선택 하기 위한 분할 단추](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView는 이제 앱에 더 작은 다양 한 탐색 옵션 및 앱의 콘텐츠에 대 한 더 많은 공간을 필요로 하는 경우 [위쪽 탐색](../design/controls-and-patterns/navigationview.md)지원 합니다.

* TreeView를 지원 하도록 향상 되었습니다 [데이터 바인딩을 항목 템플릿을, 끌어서 놓기.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>지원 프레임 워크 패키지

MSIX 컨테이너에서 실행 될 수 있도록 소스 코드에 액세스할 수 없는 경우 수정 하면 win32 응용 프로그램에 적용 하는 데 도움이 되는 오픈 소스 키트 있는 패키지 지원 프레임 워크가입니다.

자세한 내용을 보려면 [패키지 지원 프레임 워크를 사용 하 여 MSIX 패키지로 적용 런타임 수정 사항을](../porting/package-support-framework.md)참조 하세요.

## <a name="developer-guidance"></a>개발자 지침

### <a name="web-api-extensions"></a>웹 API 확장

Mozilla Developer Network 설명서 브라우저 간 웹 개발을 위한 [레거시 Microsoft API 확장](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) 목록 추가 되었습니다. 이러한 API 확장 Internet Explorer 또는 Microsoft Edge를 고유 하 고 호환성 및 broswer 지원 MDN 웹 문서에 대 한 기존 정보를 보충 합니다. 레거시 Microsoft [확장 CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) 및 [JavaScript 확장](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) 도 사용할 수 있으며, 풍부한 웹 MDN에서 API 정보에 직접 표시를 찾을 수 있습니다 [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + /winrt 코드 예제

250 추가한 [C + + WinRT](../cpp-and-winrt-apis/index.md) 코드와 함께 제공 된 기존 C + 우리의 문서에는 항목에 대 한 목록을 + CX 코드 예제입니다.

### <a name="project-rome"></a>Project Rome

[프로젝트 "로마" 문서](https://docs.microsoft.com/windows/project-rome/) 사이트 기능 중심 접근 방식으로 재구성 되었습니다 했습니다. 이 해야 쉽게 개발자가 여러 플랫폼에서 원하는 기능을 구현 하 고, 원하는 내용을 찾을 수 있습니다.

## <a name="videos"></a>비디오

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 플러그 인

Unity에 Xbox Live 플러그인 타이틀에 Xbox Live 서명, 통계, 친구 목록, 클라우드 저장소 및 순위표 추가 대 한 지원이 포함 되어 있습니다. [동영상을 시청](https://youtu.be/fVQZ-YgwNpY) , 자세한 다음을 시작 하려면 [GitHub 패키지를 다운로드](https://aka.ms/UnityPlugin) 합니다.

### <a name="one-dev-question"></a>개발자 질문

개발자 질문 비디오 시리즈 오랜 기간 사용해 온 Microsoft 개발자는 일련의 Windows 개발 팀 문화 및 기록에 대 한 질문을 설명합니다. 최신 질문을 검토 하는 다음과 같습니다.

Raymond Chen:

* [어떻게 커널 알 비디오 드라이버를 다시 시작 하는 경우?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Windows의 Burgermaster 개체 스토리 란?](https://youtu.be/0TDSbyAIvX0)