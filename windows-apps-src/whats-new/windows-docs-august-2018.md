---
author: QuinnRadich
title: 8 월 2018의 Windows 문서에서 새로운-UWP 앱 개발 (영문)
description: 8 월 2018에 대 한 Windows 10 개발자 설명서의 새로운 기능, 비디오, 예제 및 개발자 지침이 추가 되었습니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 년 8 월
ms.author: quradic
ms.date: 8/9/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 06eef0c115675ba9673a81459c91e0f08f6fab71
ms.sourcegitcommit: be5b71a8ec7b686d5f93d56d10cb9a50c3c5bb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/14/2018
ms.locfileid: "2748874"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>8 월 2018 Windows 개발자 문서에 새로운 소식

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 지침 및 비디오 변경이 이루어진 년 8 월의 월에서 제공 합니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="design"></a>디자인

다음과 같은 기능이 추가 되었습니다 Windows에 [Windows 내부 인](https://insider.windows.com/) 프로그램을 통해 사용할 수 있는 내부 인 미리 보기 빌드.

* [Windows UI 라이브러리](https://aka.ms/winui-docs) 는 UWP 앱에 대 한 컨트롤 및 기타 사용자 interfact 요소를 제공 하는 NuGet 패키지의 집합입니다. 이러한 패키지는 Windows 10의 이전 버전과 호환 되도 사용자가 최신 OS 버전 없는 경우에 작동 하는 앱입니다.

* [DropDownButton, 분할 단추, 및 ToggleSplitButton](../design/controls-and-patterns/buttons.md) 앱의 사용자 환경을 향상 시키기 위해 특수 기능을 사용 하 여 단추 컨트롤을 제공 합니다.

* 이제 NavigationView는 [위쪽 탐색 모음에서](../design/controls-and-patterns/navigationview.md) 앱에 탐색 옵션의 더 적은 수의 경우 지원 하 고 앱의 콘텐츠에 대 한 더 많은 공간이 필요 합니다.

* 트리 보기를 지원 하도록 향상 되었습니다 [데이터 바인딩 (영문), 항목 템플릿 및 용 및 드롭.](../design/controls-and-patterns/tree-view.md)

![전경색을 선택 하는 분할 단추](../design/controls-and-patterns/images/split-button-rtb.png)

### <a name="package-support-framework"></a>패키지 지원 프레임 워크

패키지 지원 프레임 워크는 오픈 소스 키트 (영문) MSIX 컨테이너에서 실행할 수 있도록 소스 코드에 액세스할 수 없을 때 픽스 win32 응용 프로그램에 적용 하는데 도움이 되는 경우  

자세한 내용을 보려면 [적용 런타임 MSIX 패키지 패키지 지원 프레임 워크를 사용 하 여 해결](../porting/package-support-framework.md)을 참조 하십시오.

## <a name="developer-guidance"></a>개발자 지침

### <a name="web-api-extensions"></a>API 웹 확장

크로스 브라우저 웹 개발에 대 한 Mozilla 개발자 네트워크 설명서에 [레거시 Microsoft API 확장](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) 목록이 추가 되었습니다. 이러한 API 확장 Internet Explorer 또는 Microsoft 지를 고유 하 고 호환성 및 broswer 지원 MDN 웹 문서에 대 한 기존 정보를 보충 합니다. 레거시 Microsoft [CSS 확장](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) 및 [JavaScript 확장](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) 을 사용할 수 있는 하며 풍부한 웹 MDN에서 API 정보에 직접 표시를 찾을 수 있습니다 [Visual Studio 코드.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + / WinRT 코드 예제

250 추가 했습니다 [C + + / WinRT](../cpp-and-winrt-apis/index.md) 코드와 함께 제공 된 기존 C +,이 문서에는 항목에 대 한 조각 + / CX 코드 예제입니다.

### <a name="project-rome"></a>프로젝트 로마

[프로젝트 로마 문서](https://docs.microsoft.com/windows/project-rome/) 사이트 기능 우선 방식으로 재구성 되었습니다 했습니다. 여러 플랫폼에 걸쳐는 원하는 기능을 구현 하 고 고객이 원하는을 유도 하는 개발자를 위한 것 보다 쉽게 해야이 합니다.

## <a name="videos"></a>비디오

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 플러그인

Unity에 대 한 Xbox Live 플러그인 서명 Xbox Live, 상태, 친구 목록, 클라우드 저장소 및 순위표 하면 제목에 추가 하는 것에 대 한 지원을 포함 합니다. 자세한 내용은, [비디오를 시청](https://youtu.be/fVQZ-YgwNpY) 한 다음 시작 하려면 [GitHub 패키지를 다운로드](https://aka.ms/UnityPlugin) 합니다.

### <a name="one-dev-question"></a>개발자는 질문 중 하나

개발 질문 중 하나는 비디오 시리즈 오랜 기간 사용해 온 Microsoft 개발자에 게는 일련의 Windows 개발, 팀 문화권 및 기록 하는 방법에 대 한 질문을를 다룹니다. 다음은 우리가 했을 때 혔 최신 질문!

Raymond Chen:

* [커널 어떻게 비디오 드라이버를 다시 시작 하는 경우?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Windows에서 Burgermaster 개체 뒤에 텍스트 이란?](https://youtu.be/0TDSbyAIvX0)