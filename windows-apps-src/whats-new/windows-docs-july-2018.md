---
author: QuinnRadich
title: 7 월 2018에서 Windows 문서의 새로운-UWP 앱 개발 (영문)
description: 7 월 2018에 대 한 Windows 10 개발자 설명서의 새로운 기능, 비디오, 예제 및 개발자 지침이 추가 되었습니다.
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows, 10 년 7 월
ms.author: quradic
ms.date: 7/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f41d25fd6757e5d3f80d00de341168de4f34e946
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862935"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>7 월 2018 Windows 개발자 문서에 새로운 소식

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음 기능 개요, 개발자 지침, 비디오 및 예제 (영문) 변경이 이루어진 7 월에 사용할 수 있습니다.

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="progressive-web-apps-on-windows"></a>Windows에서 점진적 웹 앱

[프로그레시브 웹 응용 프로그램 (Pwa)는](https://developer.microsoft.com/windows/pwa) 단순히 지원 플랫폼 및 푸시 homescreen에서 실행 설치, 오프 라인 지원 등의 브라우저 엔진에서 네이티브 app와 같은 기능을 [점진적으로 향상](https://wikipedia.org/wiki/Progressive_enhancement) 된 웹 앱 알림입니다. Microsoft에 지 (EdgeHTML) 엔진과 Windows 10에서 Pwa 바탕으로 여러분을 실행 하는 추가 된 이점은 [UWP 앱으로 브라우저 창에 관계 없이.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![동작에서 Pwa의 이미지](images/progressive-web-apps.jpg)

이 PWA 안내선을 확인해보세요.

* [PWA로 간단한 웹 응용 프로그램 작성](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [Windows 런타임과 함께 PWA 향상](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [Microsoft 저장소에 사용자 PWA를 게시 합니다.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>메모장

Windows 10 내부 인 미리 보기 빌드 17713, [메모장은 여러가지 새로운 기능이로 업데이트 되었습니다](http://aka.ms/ant-man)에서 사용할 수 있습니다. 확대/축소, 찾기/바꾸기, 둘러싸기 형 및 Unix/Linux (LF) 및 Mac (CR) 줄 끝에 대 한 지원을 [Windows 내부](https://insider.windows.com/)를 사용할 수 있습니다. 

## <a name="developer-guidance"></a>개발자 지침

### <a name="design-landing-page"></a>디자인 방문 페이지

[방문 페이지 디자인 업데이트](https://developer.microsoft.com/windows/apps/design) 에서 한 눈 개요 UWP 디자인 영역과 Fluent 디자인에 대 한 최신 추가 대 한 정보를 체크아웃 합니다.

### <a name="design-toolkits"></a>디자인 도구 키트

Adobe XD 및 Adobe Illustrator 도구 키트 새로운 기능으로 업데이트 되었습니다. 이러한 디자인 도구 키트 UWP 앱 디자인 (영문)에 대 한 컨트롤 및 레이아웃 서식 파일을 제공 합니다. [여기서 체크인 합니다.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

[WebVR 설명서](https://docs.microsoft.com/microsoft-edge/webvr/
)에 몇가지 새 항목 추가 했습니다.

* [WebVR 무엇입니까?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) WebVR 란, 이유를 사용할지 및 그에 대 한 개발을 시작 하는 방법에 설명 합니다.

* [프로그레시브 웹 응용 프로그램에서 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): WebVR 점진적 Web App (PWA)를 추가 하는 방법에 알아봅니다.

* [웹 보기에서 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): WebVR Windows 10 응용 프로그램의 웹 보기 컨트롤을 추가 하는 방법에 알아봅니다.

* [WebVR 데모 (영문)](https://docs.microsoft.com/microsoft-edge/webvr/demos): Microsoft 가장자리와 Windows 혼합 현실 몰입 형 헤드셋을 사용 하 여 일부 WebVR 데모를 확인 합니다.

또한 기존 페이지에 일부 업데이트 만들었고:

* 목차 4 개의 고유한 최상위 버킷으로 더 잘 구성 이제 됩니다: **기본 사항**, **개발**, **리소스**및 **데모**입니다.

* [WebVR 개발자 가이드 (방문 페이지)](https://docs.microsoft.com/microsoft-edge/webvr/): 더 큰 이미지 및 아이콘 새 데모 정보와 함께 새로 고쳐질된 모양 및 느낌.

* [Microsoft에 지와 WebVR를 사용 하 여](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge): Windows에 대 한 정보를 포함 하도록 업데이트 된 10 년 4 월 2018를 업데이트 합니다.

## <a name="videos"></a>비디오

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>트랜잭션당에 대 한 시작 하기: 만들기 및 Windows 10에서 양식 사용자 지정

이제 Windows 개발자를 위한이 [문서 시작 하기](../get-started/index.md) 원하는 실습 환경을 기본 앱 개발 작업을 제공 합니다. 이 비디오에서는 이러한 항목 중 하나를 통해 및 응용 프로그램의 UI 양식 만들기 (영문)의 기본 사항을 다룹니다. 작업을 다음의 코드를 참조 하려면 [비디오를 시청](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) [항목을 사용자가 직접 확인.](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>프로젝트 성격 채팅와 대화 봇 향상

프로젝트 성격 채팅 채팅 bot을 사용자 지정할 수 있는 가상 사용자를 추가할 수 있습니다. Microsoft 봇 Framework SDK를 통합 하 여 고객와 상호작용 하는 보다 대화 방법에 대 한 소규모 내용이 기능을 추가할 수 있습니다. 한 후 [대화형 데모를 사용해](http://aka.ms/PersonalityChat) 실습 환경을 구현 하는 방법에 알아봅니다 [동영상](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) 을 합니다.

### <a name="one-dev-question"></a>개발자는 질문 중 하나

개발 질문 중 하나는 비디오 시리즈 오랜 기간 사용해 온 Microsoft 개발자에 게는 일련의 Windows 개발, 팀 문화권 및 기록 하는 방법에 대 한 질문을를 다룹니다. 다음은 우리가 했을 때 혔 최신 질문!

Raymond Chen:

* [Microsoft에는 왜 적용 않은?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [기본 오디오 장치를 변경 하는 개발자에 게 이유 하지?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [너무 많은 UWP 함수 비동기 이유는 무엇입니까?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>샘플

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + / WinRT

Photo Editor 샘플 응용 프로그램을 사용한 개발 보여줍니다는 [C + + / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 언어 프로젝션 합니다. 응용 프로그램을 사용 하면 **그림** 라이브러리에서 사진을 검색 한 다음 편집을 연결 된 사진 효과와 선출된 이미지를 수 있습니다. [복제 또는 여기에 샘플을 다운로드 합니다.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![동작에서 샘플의 예](images/photo-editor-banner.png)
