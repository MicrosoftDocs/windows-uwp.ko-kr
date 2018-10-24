---
author: QuinnRadich
title: 2018 년 7 월 Windows 문서의 새로운-UWP 앱 개발
description: 새로운 기능, 동영상, 샘플 및 개발자 지침 2018 년 7 월에 대 한 Windows 10 개발자 설명서에 추가한 합니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10 7 월
ms.author: quradic
ms.date: 7/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f41d25fd6757e5d3f80d00de341168de4f34e946
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5470473"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>2018 년 7 월 Windows 개발자 문서의 새로운

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침, 동영상 및 샘플 내용이 7 월에 사용할 수 있습니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="progressive-web-apps-on-windows"></a>Windows의 점진적 웹 앱

[점진적 웹 앱 (Pwa)은](https://developer.microsoft.com/windows/pwa) 간단 하 게 지원 플랫폼 및 homescreen에서 시작 설치, 오프 라인 지원 및 푸시 등의 브라우저 엔진에서 네이티브 앱과 유사한 기능을 사용 하 여 [점진적으로 향상](https://wikipedia.org/wiki/Progressive_enhancement) 된 웹 앱 알림입니다. Microsoft Edge (EdgeHTML) 엔진을 사용 하 여 Windows 10 Pwa 즐길 추가적으로 running [UWP 앱으로 브라우저 창을 독립적으로.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![중인 Pwa의 이미지](images/progressive-web-apps.jpg)

PWA 가이드를 살펴보세요.

* [간단한 웹 앱을 PWA로 빌드](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [Windows 런타임에서 PWA 향상](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [PWA에 Microsoft Store에 게시](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>메모장

Windows 10 참가자 미리 보기 빌드 17713, [다양 한 새로운 기능을 사용 하 여 메모장이 업데이트](http://aka.ms/ant-man)에서 사용할 수 있습니다. 확대/축소, 찾기/바꾸기, 랩어라운드 및 Unix/Linux (LF) 및 Mac (CR) 줄의 끝에 대 한 지원을 [Windows 참가자](https://insider.windows.com/)에 게 제공 됩니다. 

## <a name="developer-guidance"></a>개발자 지침

### <a name="design-landing-page"></a>디자인 방문 페이지

확인 된 [방문 페이지 디자인 업데이트](https://developer.microsoft.com/windows/apps/design) UWP 디자인 영역과 흐름 디자인에 대 한 최신 추가 기능에 대 한 정보에 요약 개요 하세요.

### <a name="design-toolkits"></a>디자인 도구 키트

Adobe XD 및 Adobe Illustrator 도구 키트는 새 기능으로 업데이트 되었습니다. 이러한 디자인 도구 키트는 UWP 앱 디자인을 위한 레이아웃 템플릿을 제공 합니다. [여기서 체크 합니다.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

몇 가지 새로운 항목이 [WebVR 설명서](https://docs.microsoft.com/microsoft-edge/webvr/
)에 추가 했습니다.

* [WebVR 란 무엇 인가요?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) WebVR 란, 이유를 사용할지 및 개발을 시작 하는 방법을 설명 합니다.

* [점진적 웹 앱에 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): WebVR 점진적 웹 앱 (PWA)를 추가 하는 방법을 알아봅니다.

* [WebView에 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Windows 10 응용 프로그램에서 WebView 컨트롤에 WebVR 추가 하는 방법을 알아봅니다.

* [WebVR 데모](https://docs.microsoft.com/microsoft-edge/webvr/demos): Microsoft Edge 및 Windows Mixed Reality 몰입 형 헤드셋을 사용 하 여 몇 가지 WebVR 데모를 확인 합니다.

또한 일부 업데이트 기존 페이지를 변경한 내용:

* 이제 목차는 네 가지 최상위 버킷으로 보다 잘 정렬: **기본 사항**, **개발**, **리소스**및 **데모**합니다.

* [WebVR 개발자 가이드 (방문 페이지)](https://docs.microsoft.com/microsoft-edge/webvr/): 새로 고친된 모양과 느낌을 더 큰 이미지 및 아이콘 및 새 데모를 사용 하 여 합니다.

* [Microsoft Edge로 WebVR 사용 하 여](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge): Windows에 대 한 정보를 포함 하도록 업데이트 된 10 2018 년 4 월 업데이트 합니다.

## <a name="videos"></a>비디오

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>개발자를 위한 시작: 만들기 및 Windows 10에서 양식을 사용자 지정

Windows 개발자를 위한 [시작 문서](../get-started/index.md) 에 이제 실제 경험 기본 앱 개발 작업을 제공 합니다. 이 비디오는 이러한 항목 중 하나를 통해 안내 하 고 앱에서 UI 양식 만들기의 기본 사항을 다룹니다. [동영상을 시청](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) 중인 다음 코드를 보려면 [항목으로 확인 합니다.](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>프로젝트 퍼스 낼 리 티 채팅을 사용 하 여 사용자 물어보세요 향상

프로젝트 퍼스 낼 리 티 채팅 채팅 bot를 사용자 지정할 수 있는 가상 사용자를 추가할 수 있습니다. Microsoft 물어보세요 프레임 워크 SDK를 통합 하 여 고객을 상호 작용 하는 더 대화식 방법에 대 한 작은 작용 기능을 추가할 수 있습니다. [동영상을 시청](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) , 다음 실제 경험에 대 한 [대화형 데모를 사용해](http://aka.ms/PersonalityChat) 구현 하는 방법을 알아봅니다.

### <a name="one-dev-question"></a>개발자 질문

개발자 질문 하나 동영상 시리즈 오랜 기간 사용해 온 Microsoft 개발자는 일련의 Windows 개발 팀 문화 및 기록 하는 방법에 대 한 질문을 다룹니다. 최신 질문을 검토 하는 다음과 같습니다.

Raymond Chen:

* [Microsoft는 왜 적용 했습니까?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [기본 오디오 장치를 변경 하는 개발자가 하도록 하지는 이유는 우리?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [많은 UWP 기능 비동기 이유는 무엇입니까?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>샘플

### <a name="photo-editor-cwinrt"></a>사진 편집기 C + + WinRT

사진 편집기 샘플 응용 프로그램 사용 하 여 개발을 보여 주는 합니다 [C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 언어 프로젝션입니다. 앱을 사용 하면 **사진** 라이브러리에서 사진을 검색 한 다음 관련된 사진 효과 사용 하 여 선택 된 이미지를 편집 수 있습니다. [복제 또는 다음 샘플을 다운로드 합니다.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![실행 중인 샘플의 예](images/photo-editor-banner.png)
