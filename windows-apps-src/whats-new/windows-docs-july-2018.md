---
title: 에서 새로운 기능 Windows Docs에서 2018 년 7 월-UWP 앱 개발
description: 새로운 기능, 비디오, 샘플 및 개발자 지침 2018 년 7 월에 대 한 Windows 10 개발자 설명서에 추가한 합니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 가이드, Windows 10, 7 월
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22a3a9614a4488791a36f81a3d4dedac572111b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596838"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>2018 년 7 월에에서 새로운 Windows 개발자 문서

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 가이드, 비디오 및 샘플 7 월에 제공 되었습니다.

Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="progressive-web-apps-on-windows"></a>Windows에서 점진적 웹 앱

[점진적 웹 앱 (Pwa)](https://developer.microsoft.com/windows/pwa) 은 간단히 수 있는 웹 앱 [점진적으로 향상 된](https://wikipedia.org/wiki/Progressive_enhancement) 플랫폼 및 브라우저 엔진에서 시작-homescreen 설치, 오프 라인을 지원 하기 위해 네이티브 앱과 유사한 기능을 사용 하 여 지원 및 푸시 알림. Microsoft Edge (EdgeHTML) 엔진을 사용 하 여 Windows 10에서 Pwa 즐길 실행 장점을 [UWP 앱으로 브라우저 창을 독립적으로 합니다.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![실행 중인 Pwa의 이미지](images/progressive-web-apps.jpg)

PWA 지침을 확인해 보세요.

* [PWA 간단한 웹 앱 빌드](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [Windows 런타임으로 PWA 향상](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [Microsoft Store PWA 게시](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>Windows 메모장

Windows 10 Insider Preview 빌드 17713를 사용할 수 있습니다 [메모장 많은 새로운 기능으로 업데이트 되었습니다](https://aka.ms/ant-man)합니다. 확대/축소, 찾기/바꾸기, 순환 및 Unix/Linux (LF) 및 Mac (CR)에 해당 하는 줄의 끝에 대 한 지원을 사용할 수 있게 됩니다 [Windows 참가자](https://insider.windows.com/)합니다. 

## <a name="developer-guidance"></a>개발자 지침

### <a name="design-landing-page"></a>디자인 방문 페이지

체크 아웃 합니다 [방문 페이지 디자인 업데이트](https://developer.microsoft.com/windows/apps/design) UWP에서 쉬운 개요에 대 한 영역 및 Fluent 디자인의 최신 추가 기능에 대 한 정보를 디자인 합니다.

### <a name="design-toolkits"></a>디자인 도구 키트

새로운 기능을 사용 하 여 Adobe XD 및 Adobe Illustrator 도구 키트 업데이트 되었습니다. 이러한 디자인 도구 키트는 UWP 앱을 디자인 하기 위한 컨트롤 및 레이아웃 템플릿을 제공 합니다. [여기를 확인 합니다.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

여러 새 항목을 추가 했습니다 합니다 [WebVR 설명서](https://docs.microsoft.com/microsoft-edge/webvr/
):

* [WebVR 란?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) WebVR 란, 왜 사용 해야, 및에 대 한 개발을 시작 하는 방법을 설명 합니다.

* [점진적 웹 앱에서 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): WebVR 점진적 웹 앱 (PWA)를 추가 하는 방법에 알아봅니다.

* [WebView의 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Windows 10 응용 프로그램에서 WebView 컨트롤 WebVR 추가 하는 방법에 알아봅니다.

* [WebVR 데모](https://docs.microsoft.com/microsoft-edge/webvr/demos): Microsoft Edge 및 Windows Mixed Reality 몰입 형 헤드셋을 사용 하 여 몇 가지 WebVR 데모를 확인 합니다.

또한 일부 업데이트가 기존 페이지를 만들었습니다.

* 이제 목차 4 개의 버킷으로 최상위 잘 구성 되어 있습니다. **기본 사항**, **개발**합니다 **리소스**, 및 **데모**합니다.

* [WebVR Developer's Guide (방문 페이지)](https://docs.microsoft.com/microsoft-edge/webvr/): 새로 고친된 모양과 느낌을 더 큰 이미지와 아이콘 및 데모를 사용 하 여 합니다.

* [Microsoft Edge를 사용 하 여 WebVR를 사용 하 여](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge): Windows에 대 한 정보를 포함 하도록 업데이트 되었습니다 10 2018 년 4 월 업데이트 합니다.

## <a name="videos"></a>비디오

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>개발자를 위한 시작 합니다. 만들고 Windows 10에서 양식을 사용자 지정

우리의 [시작 문서](../get-started/index.md) Windows에 대 한 개발자가 이제 실습 환경이 제공 기본 앱 개발 작업을 사용 하 여 합니다. 이 비디오 이러한 항목 중 하나를 통해 안내 하 고 앱에서 폼 UI를 만드는 방법의 기초를 소개 합니다. [비디오를 시청](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) 작업을 수행한 다음의 코드를 보려면 [항목 직접 확인 합니다.](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>프로젝트 개인 정보 채팅을 사용 하 여 봇을 향상합니다

프로젝트 개인 정보 채팅 채팅 봇을를 사용자 지정할 수 있는 가상 사용자를 추가할 수 있습니다. Microsoft Bot Framework SDK와 통합 하 여 고객 상호 작용 하는 좀 더 대화형 방법에 대 한 작은 강연 기능을 추가할 수 있습니다. [비디오를 시청](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) 다음을 구현 하는 방법을 알아보려면 [대화형 데모를 사용해](https://aka.ms/PersonalityChat) 실습 환경에 대 한 합니다.

### <a name="one-dev-question"></a>개발 질문 중 하나

개발 질문 중 하나는 비디오 시리즈에서는 해온 Microsoft 개발자에 게 일련의 Windows 개발, 팀 문화권 및 기록 하는 방법에 대 한 질문을 다룹니다. 최신 질문 대답할 것에 다음과 같습니다.

Raymond Chen:

* [Microsoft로 적용 않았습니다 하는 이유는 있습니다?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [기본 오디오 장치를 변경 하는 개발자가 왜 하도록 하지?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [여러 UWP 함수 비동기 이유는 무엇 인가요?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>샘플

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + /cli WinRT

Photo Editor 샘플 앱을 사용한 개발 소개 합니다 [C + + /cli WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 언어 프로젝션 합니다. 앱에서 사진을 검색할 수는 **사진을** 라이브러리와 연결 된 사진이 효과 사용 하 여 이미지를 선택한 다음 편집 합니다. [복제 또는 예제를 다운로드 합니다.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![작업에서 샘플의 예제](images/photo-editor-banner.png)
