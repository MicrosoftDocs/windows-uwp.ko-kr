---
title: Xbox 모범 사례
description: Xbox development 모범 사례에 따라 Xbox One 용 유니버설 Windows 플랫폼 (UWP) 응용 프로그램을 최적화 하는 방법에 대해 알아봅니다.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 01ae58b7422215a0e4f90c5b3f59819d9a24fa36
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157777"
---
# <a name="xbox-best-practices"></a>Xbox 모범 사례

기본적으로 모든 UWP 앱은 사용자의 추가 작업 없이 Xbox One에서 실행 됩니다. 그러나 앱을 즐거움 하 고, 고객을 선택 하 고, Xbox에서 최상의 앱 환경을 사용 하려면 아래 지침을 따라야 합니다.
  > [!NOTE]
  > 시작 하기 전에 [Xbox 및 TV](../design/devices/designing-for-tv.md)디자인에 배치 된 디자인 지침을 살펴보세요.   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>Xbox One에 대 한 최상의 환경을 빌드하려면

### <a name="do-turn-off-mouse-mode"></a>*Do:* 마우스 모드 해제

Xbox 사용자는 자신의 컨트롤러를 선호 합니다. 컨트롤러 입력을 최적화 하려면 [마우스 모드를 사용 하지 않도록 설정](how-to-disable-mouse-mode.md) 하 고 방향 탐색 ( [XY 포커스 탐색 및 상호 작용](../design/input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction)이 라고도 함)을 사용 하도록 설정 합니다. 포커스 트랩 및 액세스할 수 없는 UI를 확인 합니다.

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*Do:* 10 피트 환경에 적합 한 포커스 사각형 그리기

대부분의 Xbox 사용자는 자신의 TV에서 거실에 앉아 있으므로 표준 포커스 사각형은 10 피트 떨어진 곳에서 볼 때 어렵습니다. 입력 포커스가 있는 UI 요소가 항상 사용자에 게 명확 하 게 표시 되도록 하려면 [시각적 표시](../design/input/gamepad-and-remote-interactions.md#focus-visual) 지침을 따릅니다. XAML에서 앱이 Xbox에서 실행 되는 경우이 동작을 무료로 사용할 수 있지만 HTML 앱은 사용자 지정 CSS 스타일을 사용 해야 합니다.

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*Do:* SystemMediaTransportControls 클래스와 통합

Xbox 사용자는 Xbox Media 원격 Cortana (특히 "재생" 및 "일시 중지" 음성 명령)를 사용 하 여 미디어 앱을 제어 하 고 Xbox SmartGlass 효과를 원합니다. 이러한 기능을 무료로 이용 하려면 앱에서 Xbox 미디어 컨트롤에 자동으로 포함 되는 [SystemMediaTransportControls](/uwp/api/windows.media.systemmediatransportcontrols) 클래스를 사용 해야 합니다. 앱에 사용자 지정 미디어 컨트롤이 있는 경우 **SystemMediaTransportControls** 클래스와 통합 하 여 사용자에 게 이러한 기능을 제공 해야 합니다. 배경 음악 앱을 만드는 경우 **SystemMediaTransportControls** 클래스와 통합 하 여 Xbox 멀티태스킹 탭에서 배경 음악 컨트롤이 제대로 작동 하는지 확인 합니다.

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*고려 사항:* 화면 가장자리에 그리기

많은 Tv가 디스플레이의 가장자리를 벗어나 [tv 안전 지역](../design/devices/designing-for-tv.md#tv-safe-area)내에 모든 앱의 중요 한 콘텐츠를 표시 해야 합니다. UWP에서는 *overscan* 를 사용 하 여 TV 안전 영역 내에서 콘텐츠를 유지 하지만이 기본 동작은 앱 주위에 명확한 테두리를 그릴 수 있습니다. 최상의 환경을 제공 하려면 기본 동작을 끄고 [화면 가장자리에 UI를 그리는 방법](turn-off-overscan.md)의 지침을 따르세요.
> [!IMPORTANT]
  > Overscan를 사용 하지 않도록 설정 하는 경우 대화형 요소 및 텍스트가 TV 안전 영역 내에 유지 되는지 확인 해야 합니다. 

### <a name="consider-use-tv-safe-colors"></a>*고려 사항:* TV 안전 색 사용

Tv는 컴퓨터 모니터 뿐만 아니라 극단적인 색의 강도를 처리 하지 않습니다. 사용자가 홀수 줄무늬 효과 나 씻어 아웃 이미지를 볼 수 없도록 앱에서 높은 강도 색을 사용 하지 마십시오. 또한 *tv에서 잘* 보이는 색은 사용자와 매우 다르게 보일 수 있습니다. [색](../design/devices/designing-for-tv.md#colors) 을 읽고 앱을 모든 사용자에 게 표시 하는 방법을 이해 하세요.

### <a name="remember-you-can-disable-scaling"></a>*주의:* 크기 조정을 사용 하지 않도록 설정할 수 있습니다.

UWP 앱은 모든 장치에서 컨트롤 및 글꼴과 같은 UI 요소를 읽을 수 있도록 자동으로 확장 됩니다. XAML을 사용 하는 앱은 200%까지 크기가 조정 되 고, HTML을 사용 하는 앱은 150%로 크기가 조정 됩니다. 앱이 Xbox에서 표시 되는 방식에 대 한 더 많은 제어를 원하는 경우 기본 배율 인수를 사용 하지 않도록 설정 하 여 HDTV의 실제 픽셀 크기 (1920 x 1080)를 사용 하도록 설정 합니다. Xbox에서 잘 보이도록 앱을 조정 하는 방법에 대 한 크기 조정 및 [효과적인 픽셀 및 크기 조정을](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling) [해제 하는 방법을](disable-scaling.md) 살펴보세요.

UWP 앱에 적용 되는 이러한 사례를 확인 하려면이 비디오를 확인 하세요.
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>채널 9

다음은 [Channel 9](https://channel9.msdn.com/) 에 대 한 정보를 활용 하 여 Xbox에서 놀라운 앱을 빌드하는 데 유용한 정보입니다.

- [Building Great Universal Windows Platform (UWP) Apps for Xbox(멋진 Xbox용 UWP(유니버설 Windows 플랫폼) 앱 빌드)](https://channel9.msdn.com/Events/Build/2016/B883)
- [Adapt Your App for Xbox One and TV(Xbox One 및 TV에 맞게 앱 조정)](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP 개발 1: 적응 UI 빌드](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [브라우저를 벗어난 Web Apps: 크로스 플랫폼은 교차 장치를 충족 합니다.](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>Xbox에서 앱 개발

**Xbox의 앱 개발** 이벤트는 xbox에서 앱을 처음 빌드할 때 사용할 수 있는 개발자를 위한 좋은 출발점입니다.

* [기록 된 세션 보기](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [블로그 게시물 읽기](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>참고 항목

- [Xbox One의 UWP](index.md)
- [Xbox 및 TV용 디자인](../design/devices/designing-for-tv.md)
- [Xbox One용 점진적 웹앱](/microsoft-edge/progressive-web-apps/xbox-considerations)