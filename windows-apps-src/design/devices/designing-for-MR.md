---
Description: Design your app so that it looks good and functions well in Mixed Reality.
title: 혼합 현실을 위한 디자인
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: Mixed Reality, Hololens, 증강 현실, 응시, 음성, 컨트롤러
ms.date: 2/5/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: e6aebac45dc32933f55d917c0b1153cba952d819
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7700583"
---
# <a name="designing-for-mixed-reality"></a>혼합 현실을 위한 디자인

혼합 현실에서 자연스럽게 보이도록 앱을 설계하고, 새로운 입력 방식을 활용합니다.

## <a name="overview"></a>개요

[혼합 현실](https://developer.microsoft.com/windows/mixed-reality/mixed_reality)은 실제 세상과 디지털 세상이 혼합된 결과입니다. 혼합 현실 환경의 스펙트럼은 HoloLens(컴퓨터에서 생성된 콘텐츠를 실제 세상과 혼합하는 장치) 같은 장치부터 시작하여 완전한 몰입형 가상 현실 뷰(Windows Mixed Reality 헤드셋을 통한 뷰)에 이르기까지 매우 광범위합니다. 환경이 어떻게 다른지 나타내는 예는 [혼합 현실 앱의 유형](https://developer.microsoft.com/en-us/windows/mixed-reality/types_of_mixed_reality_apps)을 참조하세요.

이번 주제에서 다루는 몇 가지 지침을 따르면 사용자 경험을 개선할 수는 있지만 기존 UWP 앱은 거의 모두 아무런 변경 없이 2D 앱으로도 혼합 현실 환경에서 실행됩니다.

![혼합 현실 뷰](images/MR-01.png)

HoloLens와 Windows Mixed Reality 헤드셋 모두 UWP 플랫폼 기반 응용 프로그램을 지원하며, 동시에 둘 다 서로 다른 유형의 두 가지 환경을 지원하기도 합니다. 

### <a name="2d-vs-immersive-experience"></a>2D vs 몰입형 환경

몰입형 앱은 사용자에게 보이는 전체 디스플레이를 사용하여 앱에서 생성되는 뷰의 중앙에 사용자를 배치합니다. 예를 들어, 몰입형 게임이라고 하면 사용자가 외계 행성의 지표면에 놓이거나, 혹은 여행 가이드 앱일 경우에는 사용자가 남미 지역의 한 마을에 놓일 수 있습니다. 몰입형 앱을 만들려면 3D 그래픽 또는 캡처된 입체 비디오가 필요합니다. 몰입형 앱은 종종 Unity 같은 타사의 게임 엔진이나 DirectX를 사용해 개발됩니다.

몰입형 앱을 개발하는 경우에는 자세한 내용을 [Windows Mixed Reality 개발자 센터](https://developer.microsoft.com/windows/mixed-reality)에서 확인할 수 있습니다.

2D 앱은 사용자의 뷰 내에서 기존 평면 창으로 실행됩니다. HoloLens의 경우, 이 말은 사용자의 실제 거실이나 사무실에서 한쪽 벽 또는 공간의 한 지점으로 뷰가 고정된다는 것을 의미합니다. Windows Mixed Reality 헤드셋에서는 앱이 [혼합 현실 홈](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home)(종종 *클리프 하우스*라고 불림)의 한쪽 벽면에 고정됩니다.

![혼합 현실에서 실행되는 다수의 앱](images/MR-multiple.png)


이러한 2D 앱들은 전체 뷰를 차지하지 않고 전체 뷰 내에 배치됩니다. 2D 앱은 한 번에 다수가 환경에 존재할 수 있습니다.

이번 주제의 나머지 섹션에서는 2D 환경에서 디자인할 때 고려해야 할 사항에 대해서 살펴보겠습니다.

## <a name="launching-2d-apps"></a>2D 앱 실행

![혼합 현실 시작 메뉴](images/MR-start-options.png)

모든 앱은 시작 메뉴에서 실행되지만 앱 시작 관리자로 사용할 3D 개체를 만드는 것도 가능합니다. 자세한 내용은 [비디오](https://www.youtube.com/watch?v=TxIslHsEXno)를 참조하세요.

## <a name="the-2d-app-input-overview"></a>2D 앱 입력 개요

HoloLens와 혼합 현실 플랫폼 모두 키보드 및 마우스가 지원됩니다. Bluetooth를 통해 키보드와 마우스를 직접 HoloLens와 페어링하는 것도 가능합니다. 혼합 현실 앱은 호스트 컴퓨터에 연결되는 마우스와 키보드를 지원합니다. 둘 다 제어를 세분화해야 하는 상황에서 유용하게 사용될 수 있습니다.

그 밖에 더욱 자연스러운 입력 방법도 지원됩니다. 이러한 방법은 사용자가 실제 키보드가 앞에 놓인 책상에 앉아있지 않거나, 혹은 제어를 세분화해야 할 때 특히 유용할 수 있습니다.

추가되는 하드웨어나 코딩이 없는 경우 2D 앱으로 작업하면 앱에서 응시(사용자가 따라서 쳐다보는 벡터)를 마우스 포인터로 사용합니다. 마치 마우스 포인터가 가상 장면의 한 개체 위에서 머무는 것처럼 구현됩니다.

일반적인 상호 작용에서는 사용자가 앱 컨트롤을 쳐다보고 있으면 컨트롤이 강조하여 표시됩니다. 그런 다음 제스처(HoloLens일 때) 또는 컨트롤러를 사용하거나, 혹은 음성 명령을 통해 작업을 트리거합니다. 사용자가 텍스트 입력 필드를 선택하면 소프트웨어 키보드가 나타납니다. 


![혼합 현실의 팝업 키보드](images/MR-keyboard.png)

이러한 상호 작용은 모두 UWP 플랫폼에서 실행한 결과에 따라 사용자가 따로 코딩할 필요 없이 자동으로 이루어진다는 점이 중요합니다. HoloLens 및 Mixed Reality 헤드셋을 통한 입력은 2D 앱에 대한 터치식 입력으로 표시됩니다. 이 말은 기본적으로 혼합 현실에서도 다수의 UWP 앱을 사용할 수 있다는 것을 의미합니다. 

즉, 몇 가지 작업만 추가하면 환경을 크게 개선할 수 있습니다. 예를 들어, [음성 제어](https://developer.microsoft.com/windows/mixed-reality/voice_design)는 특히 효과적일 수 있습니다. HoloLens 및 혼합 현실 환경은 앱을 실행하거나 앱과 상호 작용할 수 있도록 음성 명령을 지원하기 때문에 음성 지원 추가는 이러한 방식이 자연스럽게 확장된 것으로 볼 수 있습니다. 음성 지원을 UWP 앱에 추가하는 방법에 대한 자세한 내용은 [음성 조작]( https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)을 참조하세요. 


### <a name="selecting-the-right-controller"></a>올바른 컨트롤러 선택

![혼합 현실 모션 컨트롤러](images/MR-controllers.png)

혼합 현실 전용으로 사용할 수 있도록 몇 가지 새로운 입력 방식이 개발되었습니다. 예를 들면 다음과 같습니다.

* [핸드 제스처](https://developer.microsoft.com/windows/mixed-reality/gestures)(HoloLens 전용, 2D 앱을 실행하는 데만 사용됨)
* [게임 패드 지원](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories)(두 환경 모두)
* [Clicker 장치](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories)(HoloLens 전용)
* [모션 컨트롤러](https://developer.microsoft.com/windows/mixed-reality/motion_controllers)(혼합 현실 장치 전용, 위 그림 참조)

위 컨트롤러는 가상 개체와 일어나는 상호 작용이 자연스럽고 정확하게 보일 수 있도록 지원합니다. 일부 상호 작용은 아무런 대가 없이 지원됩니다. 예를 들어 HoloLens 제스처를 선택 하거나에서 예상한 다시, 혹은 코딩할 필요 없이 입력된 응답 생성 모션 컨트롤러의 Windows 키 또는 트리거를 클릭 합니다.

그 밖에 추가로 제공되는 정보나 입력 방식을 활용하려면 코드를 추가해야 하는 경우도 있습니다. 예를 들어, 개체 위치나 버튼 누르기를 고려하여 코드를 작성하는 경우에는 모션 컨트롤러를 사용해 제어를 세분화하여 개체를 조작할 수 있습니다.

> [!NOTE]
> 요약하자면 주요 원칙에 따라 최대한 자연스럽고 원활한 입력 방식을 사용자에게 항상 제공해야 합니다.


## <a name="2d-app-design-considerations-functionality"></a>2D 앱 디자인 시 고려해야 할 사항: 기능성

혼합 현실 플랫폼에서 사용될 가능성이 있는 UWP 앱을 개발할 때는 몇 가지 사항을 염두에 두어야 합니다.

* 끌어서 놓기는 모션 컨트롤러, 게임 패드 또는 제스처를 함께 사용할 경우 유효하지 않을 수도 있습니다. 응용 프로그램이 끌어서 놓기에 크게 의존하는 경우에는 개체의 새로운 위치 이동 여부를 확인하는 대화 상자를 표시하는 등 이러한 작업을 지원할 수 있는 대체 방식을 제공해야 합니다.

* 소리의 변경 방식에 주의하세요. 앱이 소리 효과를 생성하는 경우에는 가상 세계에서 앱이 고정된 위치에 음원이 표시됩니다. 이때 사용자가 앱에서 나오면 소리도 줄어듭니다. 자세한 내용은 [공간 음향](https://developer.microsoft.com/windows/mixed-reality/spatial_sound)을 참조하세요.

* 시야각을 고려하여 어포던스를 제공하세요. 모든 장치가 컴퓨터 모니터만큼 넓은 시야각을 제공하는 것은 아닙니다. 자세한 내용은 [홀로그래픽 프레임](https://developer.microsoft.com/windows/mixed-reality/holographic_frame)을 참조하세요. 또한 사용자가 실행 중인 앱에서 약간 떨어져 있는 경우도 있습니다. 다시 말해서 앱이 실제 세상이든 가상 세상이든 앱이 다른 곳 벽면에 고정되어 표시될 수 있습니다. 이때는 앱이 사용자의 주의를 끌 수 있도록 하거나, 혹은 전체 시야각이 항상 한 눈에 들어오지는 않는다는 사실을 감안해야 합니다. 이를 위해 알림 메시지를 사용할 수도 있지만 사용자의 주의를 끌 수 있는 또 한 가지 방법으로 소리 또는 [음성](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs) 경고를 사용하는 것도 괜찮습니다.

* 2D 앱에는 자동으로 [앱 바](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box)가 표시되어 사용자가 가상 환경에서 위치를 옮기거나 크기를 조정할 수 있습니다. 시야 각은 수직으로 크기를 조정하거나, 혹은 가로/세로 비율을 동일하게 유지하면서 크기를 조정할 수 있습니다.


## <a name="2d-app-design-considerations-uiux"></a>2D 앱 디자인 시 고려해야 할 사항: UI/UX

* [흐름 디자인 시스템](https://docs.microsoft.com/windows/uwp/design/fluent-design-system/)([탐색 보기](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) 등)과 효과([아크릴](https://docs.microsoft.com/windows/uwp/design/style/acrylic) 등)를 구현하는 XAML 컨트롤은 모두 2D 혼합 현실 앱에서 특히 효과가 좋습니다.

* 혼합 현실 장치에서, 혹은 적어도 혼합 현실 시뮬레이터에서 앱의 텍스트와 창 크기를 테스트하세요. 앱의 기본 창 크기는 853 x 480 유효 픽셀입니다. 더욱 큰 글꼴 크기(약 32 포인트 크기 권장됨)를 사용하거나, [Hololens를 위한 기존 유니버설 앱 업데이트](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)를 읽어보세요. 이번 주제에 대해서는 [입력 체계](https://developer.microsoft.com/windows/mixed-reality/typography) 문서에 자세하게 나와 있습니다. Visual Studio에서 작업할 경우에는 57" HoloLens 2D 앱을 위한 XAML 디자인 편집기 설정이 있어서 배율과 차원이 정확한 뷰를 제공합니다. 

![혼합 현실 앱에 표시되는 텍스트는 커야 합니다.](images/MR-text.png)

* [마우스가 응시하는 대로 움직입니다](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). 사용자가 한 개체를 쳐다보면 **터치 호버** 이벤트로 작용하기 때문에 단순히 개체를 쳐다보는 것만으로도 갑자기 팝업 창이 호출되거나 원하지 않는 상호작용이 일어날 수 있습니다. 따라서 앱이 현재 혼합 현실에서 실행 중인지 알아낼 수 있다면 이러한 동작을 변경하는 것이 좋습니다. 아래 **런타임 지원**을 참조하십시오. 

* 사용자가 한 개체를 응시하거나, 혹은 모션 컨트롤러를 사용해 가리킬 경우 **터치 호버** 이벤트가 발생합니다. 이 이벤트는 **PointerPoint**로 구성되며, 여기에서 **PointerType**은 **Touch**이지만 **IsInContact**는 **false**입니다. 일부 형태의 커밋이 발생하면(예를 들어, 게임 패드 A 버튼을 누르거나, 클릭커 장치를 누르거나, 모션 컨트롤러 트리거를 누르거나, 음성 인식 헤드셋을 "선택"하는 경우), **터치 프레스** 이벤트가 발생하고, 이때 **PointerPoint**에서 **IsInContact**는 **true**가 됩니다. 이러한 입력 이벤트에 대한 자세한 내용은 [터치 상호 작용](https://docs.microsoft.com/windows/uwp/design/input/touch-interactions)을 참조하세요.

* 응시는 마우스로 가리키는 것만큼 정확하지 않습니다. 마우스 타겟이나 버튼의 크기가 작을수록 사용자가 불편할 수 있기 때문에 상황에 따라 컨트롤의 크기를 조정하세요. 타겟이나 버튼이 터치용으로 디자인된 경우에는 혼합 현실에서도 유효하지만 일부 버튼은 런타임 시 크기를 늘려야 할 수도 있습니다. [Hololens를 위한 기존 유니버설 앱 업데이트](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)를 참조하세요.

* HoloLens는 검은색을 조명이 없는 것으로 정의합니다. 그 결과 렌더링되지 않으면서 "실제 세상"이 보이게 됩니다. 이로 인해 혼동을 초래할 경우에는 응용 프로그램에 검은색을 사용해서는 안 됩니다. 혼합 현실 헤드셋에서는 검은색이 그대로 검은색입니다.

* HoloLens는 앱에서 색 테마를 지원하지 않기 때문에 최상의 사용자 경험을 제공할 수 있도록 파란색으로 기본 설정됩니다. 색상 선택에 대해 자세히 알고 싶다면 [이 주제](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials)를 참조하세요. 혼합 현실 디자인의 색상 및 재료 사용에 대해서 상세하게 설명되어 있습니다.


## <a name="other-points-to-consider"></a>기타 고려해야 할 사항

* [데스크톱 브리지](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)가 기존(Win32) 데스크톱 앱을 Windows 10과 Microsoft Store로 가져오는 데 효과적이기는 하지만 아직까지는 HoloLens 또는 혼합 현실에서 실행되는 앱을 개발하지 못합니다.




## <a name="runtime-support"></a>런타임 지원

앱이 런타임일 때 혼합 현실 장치에서 실행되는지 확인할 수 있습니다. 이를 통해서 컨트롤의 크기를 조정하거나, 혹은 다른 방식으로 헤드셋에서 사용할 수 있도록 앱을 최적화하세요.

다음은 혼합 현실 장치에서 앱을 사용하는 경우에 한해 XAML TextBlock 컨트롤 내에서 텍스트 크기를 조정할 수 있는 짧은 코드 조각입니다.

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 16;
            }

```





## <a name="related-articles"></a>관련 문서


* [현재 앱이 셸에서 API를 사용하는 데 따른 제약](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [2D 앱 개발](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens: Microsoft HoloLens용 UWP 2D 앱 개발](https://channel9.msdn.com/Events/Build/2016/B854)
* [조건부 XAML](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml)


