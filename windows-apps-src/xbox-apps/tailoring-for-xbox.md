---
title: Xbox 모범 사례
description: Xbox용으로 응용 프로그램을 최적화하는 방법
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6e64feb8938be3e7338c87acdf8fd18fb13e525b
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8342121"
---
# <a name="xbox-best-practices"></a>Xbox 모범 사례

기본적으로 모든 UWP 앱은 추가 작업 없이 Xbox One에서 실행됩니다. 그러나 고객이 편리하게 사용할 수 있도록 앱을 꾸미고 Xbox에서 앱 환경의 경쟁력을 유지하려면 아래 사례를 따릅니다.
  > [!NOTE]
  > 시작하기 전에 [Xbox 및 TV용 디자인](../design/devices/designing-for-tv.md)의 디자인 지침을 참조하세요.   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>Xbox One을 위한 최상의 환경을 조성하려면

### <a name="do-turn-off-mouse-mode"></a>*작업:* 마우스 모드 끄기

Xbox 사용자는 컨트롤러를 선호 합니다. 컨트롤러 입력을 위해 최적화하려면 [마우스 모드를 사용하지 않도록 설정](how-to-disable-mouse-mode.md)하고 방향 탐색([X-Y 포커스](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction)라고도 함)을 사용합니다. 포커스 트래핑 및 액세스할 수 없는 UI에 주의 합니다.

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*작업:* 약 10피트 환경에 적합한 포커스 사각형 그리기

대부분의 Xbox 사용자는 거실에서 TV 주위에 앉아 있으므로 표준 포커스 사각형이 10피트를 넘는 일은 거의 없습니다. 입력 포커스가 있는 UI 요소가 항상 사용자에게 명확하게 표시되도록 하려면 [포커스 화면 효과](../design/devices/designing-for-tv.md#focus-visual) 지침을 따릅니다. XAML에서는 Xbox에서 앱이 실행될 때 이 동작을 무료로 사용할 수 있지만 HTML 앱에서는 사용자 지정 CSS 스타일을 사용해야 합니다.

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*작업:* SystemMediaTransportControls 클래스와 통합

Xbox 사용자는 Xbox 미디어 리모컨, Cortana(특히 "실행" 및 "일시 중지" 음성 명령) 및 Xbox SmartGlass를 사용하여 미디어 앱을 제어하려고 합니다. 이 기능을 무료로 사용하려면 앱에서 Xbox 미디어 컨트롤에 자동으로 포함된 [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.media.systemmediatransportcontrols.aspx) 클래스를 사용해야 합니다. 앱에 사용자 지정 미디어 컨트롤이 있는 경우 **SystemMediaTransportControls** 클래스와 통합하여 사용자에게 이러한 기능을 제공해야 합니다. 배경 음악 앱을 만드는 경우 Xbox 멀티태스킹 탭에서 배경 음악 컨트롤이 올바르게 작동되도록 **SystemMediaTransportControls** 클래스와 통합합니다.

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*고려 사항:* 화면 가장자리까지 그리기

디스플레이 가장자리가 잘리는 TV가 많으므로 앱의 모든 중요 콘텐츠가 [TV 안전 영역](../design/devices/designing-for-tv.md#tv-safe-area) 내에 표시되도록 해야 합니다. UWP에서는 *오버스캔*을 사용하여 콘텐츠를 TV 안전 영역 내에 유지하지만 이 기본 동작으로 인해 앱 주위에 명확한 테두리가 그려질 수 있습니다. 최상의 환경을 제공하기 위해서는 기본 동작을 해제하고 [화면 가장자리까지 UI를 그리는 방법](turn-off-overscan.md)의 지침을 따릅니다.
> [!IMPORTANT]
  > 오버스캔을 사용하지 않도록 설정한 경우 대화형 요소와 텍스트가 TV 안전 영역 내에 모두 포함되도록 해야 합니다. 

### <a name="consider-use-tv-safe-colors"></a>*고려 사항:* TV에 적합한 색 사용

TV는 컴퓨터 모니터처럼 극단적인 색 농도를 처리하지 못합니다. 사용자에게 이상한 밴드 효과 또는 흐린 이미지가 표시되지 않도록 앱에 고농도 색을 사용하지 마세요. 또한 TV 간에 차이가 있어 *사용 중*인 TV에서 보이는 색과 사용자에게 표시되는 색이 다를 수 있다는 점을 명심해야 합니다. 모든 앱을 확인 하는 방법을 이해 하기 위해 [색](../design/devices/designing-for-tv.md#colors) 읽기!

### <a name="remember-you-can-disable-scaling"></a>*유의 사항:* 배율을 사용하지 않도록 설정할 수 있음

UWP 앱에서는 컨트롤과 글꼴 등의 UI 요소가 모든 디바이스에서 올바르게 표시되도록 자동으로 배율이 조정됩니다. XAML을 사용하는 앱은 200%, HTML을 사용하는 앱은 150%로 배율이 조정됩니다. Xbox에서의 앱 모양을 더 자세히 제어하려면 기본 배율 요소를 사용하지 말고 HDTV의 실제 픽셀 치수(1920x1080)를 사용합니다. Xbox에 가장 적합하도록 앱을 조정하는 방법은 [크기 조정을 끄는 방법](disable-scaling.md) 및 [유효 픽셀 및 크기 조정](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling)을 참조하세요.

UWP 앱에 적용된 이러한 사례를 간략하게 보려면 이 동영상을 확인하세요.
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>Channel 9

[Channel 9](https://channel9.msdn.com/)에 대한 다음 문서에서도 Xbox에서 멋진 앱을 빌드하는 방법에 대한 유용한 정보를 제공합니다.

- [Building Great Universal Windows Platform (UWP) Apps for Xbox(뛰어난 Xbox용 UWP(유니버설 Windows 플랫폼) 앱 빌드)](https://channel9.msdn.com/Events/Build/2016/B883)
- [Adapt Your App for Xbox One and TV(Xbox One 및 TV에 맞게 앱 조정)](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP Development 1: Building an Adaptive UI(UWP 개발 1: 적응 UI 빌드)](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device(브라우저 이외의 웹앱: 플랫폼 간 및 디바이스 간)](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>Xbox에서 앱 개발

**Xbox에서 앱 개발** 이벤트는 Xbox에서 앱을 작성 하는 데 새 개발자를 위한 멋진 출발점입니다.

* [기록 된 세션](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [블로그 게시물](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>참고 항목

- [Xbox One의 UWP](index.md)
- [Xbox 및 TV용 디자인](../design/devices/designing-for-tv.md)
