---
title: 2018년 8월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2018년 8월 Windows 10 개발자 설명서에 새로운 기능, 비디오, 샘플 및 개발자 지침이 추가되었습니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 8월
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb6900b4d5ad529ee2e94f09439dda6abcaebac8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174397"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>2018년 8월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 8월에 제공된 기능 개요, 개발자 지침 및 비디오는 다음과 같습니다.

Windows 10에 [도구 및 SDK를 설치](https://developer.microsoft.com/windows/downloads#_blank)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 살펴볼 수 있습니다.

## <a name="features"></a>기능

### <a name="design"></a>디자인

다음 기능이 Windows Insider Preview 빌드에 추가되었으며, [Windows Insider](https://insider.windows.com/) 프로그램을 통해 사용할 수 있습니다.

* [Windows UI Library](/uwp/toolkits/winui/)는 UWP 앱용 컨트롤 및 기타 사용자 인터페이스 요소를 제공하는 NuGet 패키지 세트입니다. 또한 이러한 패키지는 이전 버전의 Windows 10과도 호환되므로 사용자에게 최신 OS 버전이 없는 경우에도 앱이 작동합니다.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) 및 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)은 앱의 사용자 인터페이스를 향상시키는 특수 기능을 갖춘 단추 컨트롤을 제공합니다.

![전경색을 선택할 수 있는 분할 단추](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView는 이제 앱의 탐색 옵션 수가 적고 앱의 콘텐츠에 더 많은 공간이 필요한 경우를 위해 [위쪽 탐색 모음](../design/controls-and-patterns/navigationview.md)을 지원합니다.

* TreeView는 [데이터 바인딩, 항목 템플릿 및 끌어서 놓기](../design/controls-and-patterns/tree-view.md)를 지원하도록 향상되었습니다.

### <a name="package-support-framework"></a>패키지 지원 프레임워크

패키지 지원 프레임워크는 소스 코드에 액세스할 수 없을 때 win32 애플리케이션에 수정 사항을 적용하여 MSIX 컨테이너에서 실행하는 데 유용한 오픈 소스 키트입니다.

자세히 알아보려면 [패키지 지원 프레임워크를 사용하여 MSIX 패키지에 런타임 수정 적용](/windows/msix/psf/package-support-framework)을 참조하세요.

## <a name="developer-guidance"></a>개발자 지침

### <a name="web-api-extensions"></a>웹 API 확장

브라우저 간 웹 개발용 [레거시 Microsoft API 확장](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) 목록이 Mozilla Developer Network 설명서에 추가되었습니다. 이러한 API 확장은 Internet Explorer 또는 Microsoft Edge 고유의 확장이며, MDN 웹 문서의 호환성 및 브라우저 지원에 대한 기존 정보를 보완합니다. 레거시 Microsoft [CSS 확장](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) 및 [JavaScript 확장](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)도 사용할 수 있으며, [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)에서 표시되는 MDN의 다양한 웹 API 정보를 직접 확인할 수 있습니다.

### <a name="cwinrt-code-examples"></a>C++/WinRT 코드 예제

기존 C++/CX 코드 예제와 함께 문서의 항목에 250개의 [C++/WinRT](../cpp-and-winrt-apis/index.md) 코드 목록이 추가되었습니다.

### <a name="project-rome"></a>프로젝트 로마

[프로젝트 로마 문서](/windows/project-rome/) 사이트가 기능 우선 방식으로 재구성되었습니다. 따라서 개발자가 원하는 것을 더 쉽게 찾을 수 있고 여러 플랫폼에서 원하는 기능을 구현할 수 있습니다.

## <a name="videos"></a>동영상

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 플러그 인

Unity용 Xbox Live 플러그 인에는 Xbox Live 서명, 통계, 친구 목록, 클라우드 스토리지 및 순위표를 제목에 추가하기 위한 지원이 포함되어 있습니다. 이 [비디오를 시청](https://youtu.be/fVQZ-YgwNpY)하여 자세히 알아본 다음, [GitHub 패키지를 다운로드](https://aka.ms/UnityPlugin)하여 시작하세요.

### <a name="one-dev-question"></a>One Dev Question

Microsoft 개발자는 One Dev Question 동영상 시리즈에서 오랫동안 Windows 개발, 팀 문화 및 이력에 대한 일련의 질문을 다루어 왔습니다. 최근에 답변한 질문은 다음과 같습니다.

Raymond Chen:

* [비디오 드라이버를 다시 시작할 시기를 커널에서 어떻게 알 수 있나요?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Windows의 Burgermaster 개체의 배경 정보는 무엇인가요?](https://youtu.be/0TDSbyAIvX0)