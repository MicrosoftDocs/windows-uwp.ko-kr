---
title: 2018년 7월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2018년 7월 Windows 10 개발자 설명서에 새로운 기능, 비디오, 샘플 및 개발자 지침이 추가되었습니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 7월
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 375cd903a0711a1493930567f557992c0bba46b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174347"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>2018년 7월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 지속적으로 업데이트되고 있습니다. 7월에 제공된 기능 개요, 개발자 지침, 비디오 및 샘플은 다음과 같습니다.

Windows 10에 [도구 및 SDK를 설치](https://developer.microsoft.com/windows/downloads#_blank)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 살펴볼 수 있습니다.

## <a name="features"></a>기능

### <a name="progressive-web-apps-on-windows"></a>Windows의 점진적 웹앱

[PWA(점진적 웹앱)](https://developer.microsoft.com/windows/pwa)는 플랫폼 및 브라우저 엔진을 지원하는 네이티브 앱과 비슷한 기능(예: 홈 화면 시작 설치, 오프라인 지원 및 푸시 알림)으로 [점진적으로 향상되는](https://www.wikipedia.org/wiki/Progressive_enhancement) 간단한 웹앱입니다. Microsoft Edge(EdgeHTML) 엔진이 있는 Windows 10에서 PWA에는 [브라우저 창과 독립적으로 UWP 앱으로 실행](/microsoft-edge/progressive-web-apps/windows-features)될 수 있다는 장점이 추가되었습니다.

![실행 중인 PWA의 이미지](images/progressive-web-apps.jpg)

다음 PWA 지침을 확인해 보세요.

* [간단한 웹앱을 PWA로 빌드](/microsoft-edge/progressive-web-apps/get-started)
* [Windows 런타임으로 PWA 향상](/microsoft-edge/progressive-web-apps/windows-features)
* [Microsoft Store에 PWA 게시](/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>메모장

Windows 10 Insider Preview 빌드 17713에서 사용할 수 있는 [메모장이 많은 새로운 기능으로 업데이트되었습니다](https://blogs.windows.com/windowsexperience/2018/07/11/announcing-windows-10-insider-preview-build-17713/). 이제 확대/축소, 줄 바꿈 찾기/바꾸기, Unix/Linux(LF) 및 Mac(CR) 줄 끝 지원은 [Windows 참가자](https://insider.windows.com/)에서 사용할 수 있습니다. 

## <a name="developer-guidance"></a>개발자 지침

### <a name="design-landing-page"></a>디자인 방문 페이지

[업데이트된 디자인 방문 페이지](https://developer.microsoft.com/windows/apps/design)에서 UWP 디자인 영역에 대한 한 눈에 보기 개요와 Fluent 디자인의 최신 기능에 대한 정보를 확인해 보세요.

### <a name="design-toolkits"></a>디자인 도구 키트

Adobe XD 및 Adobe Illustrator 도구 키트는 새로운 기능으로 업데이트되었습니다. 이러한 디자인 도구 키트는 UWP 앱 디자인을 위한 레이아웃 템플릿을 제공합니다. [여기서 확인해 보세요.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

[WebVR 설명서](/microsoft-edge/webvr/)에는 다음 몇 가지 새로운 항목이 추가되었습니다.

* [WebVR이란?](/microsoft-edge/webvr/what-is-webvr) WebVR의 기본 사항, 사용해야 하는 이유 및 개발을 시작하는 방법을 설명합니다.

* [점진적 웹앱의 WebVR](/microsoft-edge/webvr/webvr-in-pwas): PWA(점진적 웹앱)에 WebVR을 추가하는 방법을 알아봅니다.

* [WebView의 WebVR](/microsoft-edge/webvr/webvr-in-webview): Windows 10 애플리케이션에서 WebVR을 WebView 컨트롤에 추가하는 방법을 알아봅니다.

* [WebVR 데모](/microsoft-edge/webvr/demos): Microsoft Edge 및 Windows Mixed Reality 몰입형 헤드셋을 사용하여 일부 WebVR 데모를 확인합니다.

또한 기존 페이지가 일부 업데이트되었습니다.

* 목차는 이제 **기본 사항**, **개발**, **리소스** 및 **데모**라는 별도의 네 가지 최상위 버킷으로 더 효율적으로 구성되었습니다.

* [WebVR 개발자 가이드(방문 페이지)](/microsoft-edge/webvr/): 더 큰 이미지와 아이콘, 새 데모를 사용하여 모양과 느낌이 새롭게 되었습니다.

* [Microsoft Edge에서 WebVR 사용](/microsoft-edge/webvr/webvr-with-edge): Windows 10 2018년 4월 업데이트에 대한 정보를 포함하도록 업데이트되었습니다.

## <a name="videos"></a>동영상

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>개발자를 위한 시작: Windows 10에서 양식 만들기 및 사용자 지정

이제 Windows 개발자를 위한 [시작 문서](../get-started/index.md)에서 기본 앱 개발 작업용 실습 환경을 제공합니다. 이 비디오에서는 이러한 항목 중 하나를 안내하고, 앱에서 양식 UI를 만드는 방법에 대한 기본 사항을 설명합니다. 이 [비디오를 시청](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)하여 작동하는 코드를 살펴본 다음, [해당 항목을 직접 확인해 보세요.](../get-started/construct-form-learning-track.md)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Project Personality Chat을 사용하여 봇 향상

Project Personality Chat을 사용하면 사용자 지정 가능한 가상 사용자를 채팅 봇에 추가할 수 있습니다. Microsoft Bot Framework SDK와 통합하면 고객과 상호 작용하는 더 향상된 대화형 방법에 맞게 작은 대화 기능을 추가할 수 있습니다. 이 [비디오를 시청](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)하여 구현하는 방법을 알아본 다음, 실습 환경용 [대화형 데모를 사용해 보세요](https://www.microsoft.com/research/project/personality-chat/).

### <a name="one-dev-question"></a>One Dev Question

Microsoft 개발자는 One Dev Question 동영상 시리즈에서 오랫동안 Windows 개발, 팀 문화 및 이력에 대한 일련의 질문을 다루어 왔습니다. 최근에 답변한 질문은 다음과 같습니다.

Raymond Chen:

* [Microsoft에 지원한 이유는 무엇인가요?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [개발자가 기본 오디오 디바이스를 변경할 수 없는 무엇인가요?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [많은 UWP 함수가 비동기인 이유는 무엇인가요?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>샘플

### <a name="photo-editor-cwinrt"></a>Photo Editor C++/WinRT

Photo Editor 샘플 앱은 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 언어 프로젝션을 사용한 개발을 보여 줍니다. 이 앱을 사용하면 **사진** 라이브러리에서 사진을 검색한 다음, 관련 사진 효과를 사용하여 선택한 이미지를 편집할 수 있습니다. [여기서 샘플을 복제하거나 다운로드하세요.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![실행 중인 샘플의 예](images/photo-editor-banner.png)