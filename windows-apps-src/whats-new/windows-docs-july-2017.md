---
title: 2017년 7월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2017년 7월 Windows 10 개발자 설명서에 추가된 새로운 기능, 샘플 및 개발자 지침
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1875fecbdfa6b97cdc30413c8afa6cc58d3251ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595038"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>2017년 7월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침 및 코드 샘플이 Windows 개발자를 위한 새 정보와 업데이트된 정보를 포함하여 최근에 추가되었습니다.

Windows 10에 [도구 및 SDK를 설치](https://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/your-first-app.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="fluent-design"></a>Fluent 디자인

SDK Preview 빌드의 [Windows 참가자](https://insider.windows.com/)에서 사용할 수 있는 이러한 새로운 효과는 깊이, 관점 및 동작을 사용하여 사용자가 중요한 UI 요소에 집중할 수 있도록 합니다.

[아크릴 소재](../design/style/acrylic.md)는 투명한 텍스처를 만드는 브러시 유형입니다. 

![밝은 테마의 아크릴](../design/style/images/Acrylic_DarkTheme_Base.png)

[시차 효과](../design/motion/parallax.md)는 앱에 3차원 깊이 및 관점을 추가합니다.

![목록 및 배경 이미지를 사용한 시차의 예](../design/style/images/_Parallax_v2.gif)

[표시](../design/style/reveal.md)는 앱의 중요한 요소를 강조 표시합니다. 

![Visual 표시](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>UI 컨트롤

SDK Preview 빌드의 [Windows 참가자](https://insider.windows.com/)에서 사용할 수 있는 이러한 새 컨트롤은 멋진 UI를 신속하고 손쉽게 빌드할 수 있도록 합니다.

[색 선택 컨트롤](../design/controls-and-patterns/color-picker.md)은 사용자를 색상을 검색해서 선택할 수 있도록 해줍니다.  

![기본 색 선택](../design/controls-and-patterns/images/color-picker-default.png)

[탐색 보기 컨트롤](../design/controls-and-patterns/navigationview.md)는 앱에 최상위 탐색을 손쉽게 추가할 수 있게 해줍니다.

![NavigationView 섹션](../design/controls-and-patterns/images/navview_sections.png)

[사람 그림 컨트롤](../design/controls-and-patterns/person-picture.md)은 사람에 대한 아바타 이미지를 표시합니다.

![인물 사진 컨트롤](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

[등급 컨트롤](../design/controls-and-patterns/rating.md)은 사용자가 콘텐츠 및 서비스에 대한 만족도를 반영하는 등급을 손쉽게 확인 및 설정할 수 있도록 해줍니다.

![등급 컨트롤의 예](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>디자인 도구 키트

Sketch 및 Adobe XD의 추가로 [UWP 앱에 대한 디자인 도구 키트 및 리소스](../design/downloads/index.md)가 확장되었습니다. 이전의 도구 키트도 업데이트되고 개선되어, UWP 앱을 위한 더 견고한 컨트롤과 레이아웃 템플릿을 제공합니다.

### <a name="dashboard-monetization-and-store-services"></a>대시보드, 수익 창출 및 스토어 서비스

다음과 같은 새로운 기능을 사용할 수 있습니다.

* Microsoft Advertising SDK를 사용하여 이제 앱에서 [기본 광고](../monetize/native-ads.md)를 표시할 수 있습니다. 기본 광고는 제목, 이미지, 설명, 행동 촉구 텍스트 등의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다. 현재 기본 광고는 파일럿 프로그램에 참여하고 있는 개발자만 사용할 수 있지만, 조만간 모든 개발자가 이 기능을 사용할 수 있도록 지원할 계획입니다.

* [Microsoft Store 분석 API](../monetize/access-analytics-data-using-windows-store-services.md)에서 이제 [앱 오류에 대한 CAB 파일을 다운로드](../monetize/download-the-cab-file-for-an-error-in-your-app.md)하는 데 사용할 수 있는 메서드를 제공합니다.

* [대상 제품](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)은 특정 고객층을 대상으로 맞춤 설정된 매력적인 콘텐츠를 게시하는 방식으로 참여도와 고객 유지, 수익 창출을 향상시킵니다. 

* 이제 앱의 스토어 목록에 [비디오 예고편](../publish/app-screenshots-and-images.md#trailers)을 포함시킬 수 있습니다.

* 새 가격 책정 및 가용성 옵션을 사용하면 [가격 변경을 예약](../publish/set-and-schedule-app-pricing.md)하고 [정확한 출시 날짜를 설정](..//publish/configure-precise-release-scheduling.md)할 수 있습니다.

* [스토어 목록을 가져오고 내보내](../publish/import-and-export-store-listings.md) 빠르게 업데이트할 수 있습니다. 특히 여러 언어로 목록이 있는 경우 더 빠릅니다.

### <a name="my-people"></a>내 피플

SDK Preview 빌드의 [Windows 참가자](https://insider.windows.com/)에 곧 추가될 내 피플 기능을 통해 응용 프로그램에서 작업 표시줄로 연락처를 직접 고정할 수 있습니다. [응용 프로그램 내 사용자 지원을 추가 하는 방법을 알아봅니다.](../contacts-and-calendar/my-people-support.md)

![내 피플 연락처 패널](images/my-people.png)

[내 피플 공유](../contacts-and-calendar/my-people-sharing.md)를 사용하면 사용자가 작업 표시줄에서 바로 응용 프로그램을 통해 파일을 공유할 수 있습니다.

![내 피플 공유](images/my-people-sharing.png)

[내 피플 알림](../contacts-and-calendar/my-people-support.md)은 사용자가 자신의 고정된 연락처로 보낼 수 있는 새로운 종류의 알림입니다.

![내 피플 알림](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>작업 표시줄에 고정

SDK Preview 빌드의 [Windows 참가자](https://insider.windows.com/)에서 사용할 수 있는 새 TaskbarManager 클래스를 사용하여 사용자에게 [작업 표시줄에 앱을 고정](../design/shell/pin-to-taskbar.md)하도록 요청할 수 있습니다.

## <a name="developer-guidance"></a>개발자 지침

### <a name="media-playback"></a>미디어 재생

기본 미디어 재생 문서에 새 섹션인 [MediaPlayer를 사용하여 오디오 및 비디오 재생](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)이 추가되었습니다. [MediaPlayer를 사용하여 구형 비디오 재생](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) 섹션은 지원되는 형식의 시야를 조정하고 방향을 볼 수 있는 기능을 포함하여 구형으로 인코딩된 비디오를 재생하는 방법을 보여 줍니다. [프레임 서버 모드에서 MediaPlayer 사용](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) 섹션은 [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer)에서 재생되는 프레임을 Direct3D 표면에 복사하는 방법을 보여 줍니다. 이를 통해 픽셀 셰이더를 사용한 실시간 효과 적용 시나리오가 가능합니다. 예제 코드는 Win2D를 사용하여 재생되는 비디오에 흐림 효과를 빠르게 구현하는 방법을 보여 줍니다.

### <a name="media-capture"></a>미디어 캡처

[MediaFrameReader를 사용하여 미디어 프레임 처리](../audio-video-camera/process-media-frames-with-mediaframereader.md) 문서가 업데이트되어 새로운 [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 클래스를 사용하여 여러 미디어 원본에서 시간 연관 프레임을 가져오는 방법을 설명합니다. 이는 깊이 카메라 및 색상 카메라와 같은 여러 원본의 프레임을 처리해야 하는 경우에 유용하며 사용자는 각 원본의 프레임이 서로 가깝게 캡처되는지 확인해야 합니다. 자세한 내용은 [MultiSourceMediaFrameReader를 사용하여 여러 원본에서 시간 연관 프레임 가져오기](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources)를 참고하세요.

### <a name="scoped-search"></a>범위가 지정된 검색

docs.microsoft.com의 [UWP 개념](../get-started/universal-application-platform-guide.md) 및 [API 참조](https://docs.microsoft.com/en-us/uwp/api/) 문서에 "UWP" 범위가 추가되었습니다. 이 범위를 비활성화하지 않으면 이 영역 내에서 수행한 검색이 UWP 문서만 반환합니다.

![범위가 지정된 검색](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>Windows 10 S용 Windows 앱 테스트

Windows S를 실행하는 디바이스에서 Windows 앱이 제대로 작동하는지 테스트합니다. [이 새로운 가이드](../porting/desktop-to-uwp-test-windows-s.md)에서 방법을 알아보세요. 

## <a name="samples"></a>샘플

### <a name="annotated-audio-app-sample"></a>주석이 추가된 오디오 앱 샘플

[오디오, 잉크 및 OneDrive 데이터 로밍 시나리오를 보여주는 작은 앱 샘플입니다](https://github.com/Microsoft/Windows-appsample-annotated-audio). 이 샘플은 잉크 주석의 동기화된 캡처와 동시에 오디오를 녹음하여, 노트를 캡처했을 때 무엇을 논의했는지 나중에 기억할 수 있도록 합니다.

![주석이 추가된 오디오 샘플의 스크린샷](images/Playback.png)  

### <a name="shopping-app-sample"></a>쇼핑 앱 샘플

[사용자가 이모지를 구입할 수 있는 기본 쇼핑 환경을 나타내는 작은 앱 샘플입니다](https://github.com/Microsoft/Windows-appsample-shopping). 이 앱은 [결제 요청 API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)를 사용하여 체크아웃 환경을 구현합니다.

![쇼핑 앱 샘플의 스크린샷](images/shoppingcart.png)  

## <a name="videos"></a>비디오

### <a name="accessibility"></a>접근성

앱에 접근성 기능을 추가하여 더 많은 사용자가 사용할 수 있도록 합니다. [동영상을 시청](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility)하고 [접근성용 앱 개발](https://developer.microsoft.com/en-us/windows/accessible-apps)에 대해 자세히 알아보세요.

### <a name="payments-request-api"></a>결제 요청 API

결제 요청 API는 고객과 판매자가 보다 원활하게 온라인 체크아웃 프로세스를 완료할 수 있도록 도와줍니다. [동영상을 시청](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API)하고 [결제 요청 설명서](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API)를 살펴보세요.

### <a name="windows-10-iot-core"></a>Windows 10 IoT Core K

Windows 10 IoT Core 및 유니버설 Windows 플랫폼을 사용하여 이 애완동물 인식 출입구와 같이 시각과 구성 요소 연결을 활용한 프로젝트의 프로토타입을 빠르게 제작하고 빌드할 수 있습니다. [동영상을 시청](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core)하고 [Windows 10 IoT Core 시작](https://developer.microsoft.com/en-us/windows/iot) 방법을 알아보세요.