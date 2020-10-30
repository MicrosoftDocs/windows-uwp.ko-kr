---
description: 혼합 현실에서 잘 작동 하도록 앱을 디자인 합니다.
title: Mixed Reality 디자인
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: 혼합 현실, Hololens, 확대 현실, 응시, 음성, 컨트롤러
ms.date: 02/05/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: 225b91b20f35c974fca865cc4e94a96efceda84d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034376"
---
# <a name="designing-for-mixed-reality"></a>Mixed Reality 디자인

혼합 현실에서 잘 보이도록 응용 프로그램을 디자인 하 고 새 입력 방법을 활용 하세요.

## <a name="overview"></a>개요

[혼합 현실](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) 은 실제 세계를 디지털 세계와 혼합 한 결과입니다. 혼합 현실 환경의 스펙트럼에는 HoloLens (예: 컴퓨터에서 생성 한 콘텐츠를 혼합 하는 장치) 및 가상 현실 (Windows Mixed Reality 헤드셋으로 표시 됨)의 완전히 몰입 형 보기와 같은 하나의 극단적인 장치가 포함 되어 있습니다. 환경에 따라 달라 지는 방법에 대 한 예제는 [혼합 현실 앱 유형](https://developer.microsoft.com/windows/mixed-reality/types_of_mixed_reality_apps) 을 참조 하세요.

거의 모든 기존 UWP 앱은이 항목의 지침에 따라 사용자의 경험을 향상 시킬 수 있지만 변경 되지 않은 2D 앱으로 혼합 현실 환경에서 실행 됩니다.

![혼합 현실 뷰](images/MR-01.png)

HoloLens 및 Windows Mixed Reality 헤드셋 모두 UWP 플랫폼에서 실행 되는 응용 프로그램을 지원 하 고 두 가지 유형의 경험을 모두 지원 합니다. 

### <a name="2d-vs-immersive-experience"></a>2D 및 몰입 형 환경

몰입 형 앱은 사용자에 게 표시 되는 전체 디스플레이를 사용 하 여 앱이 만든 보기의 중심에 배치 합니다. 예를 들어 몰입 형 게임은 사용자를 외계인 전 세계의 표면에 놓을 수 있습니다. 또는 둘러보기 가이드 앱이 사용자를 남부 아메리카 마을로 이동할 수도 있습니다. 몰입 형 앱을 만들려면 3D 그래픽 또는 캡처된 stereographic 비디오가 필요 합니다. 모던 앱은 Unity와 같은 타사 게임 엔진과 DirectX를 사용 하 여 개발 되는 경우가 많습니다.

몰입 형 앱을 만드는 경우 자세한 내용은 [Windows Mixed Reality 개발자 센터](https://developer.microsoft.com/mixed-reality) 를 방문 해야 합니다.

2D 앱은 사용자 뷰 내에서 기존 플랫 창으로 실행 됩니다. HoloLens에서이는 벽에 고정 된 보기나 사용자 소유의 실제 실내 또는 사무실의 특정 지점에 고정 된 보기를 의미 합니다. Windows Mixed Reality 헤드셋에서 앱은 [혼합 현실 홈](/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) ( *절벽 집* 이라고 함)의 벽에 고정 되어 있습니다.

![혼합 현실에서 실행 되는 여러 앱](images/MR-multiple.png)


이러한 2D 앱은 전체 뷰를 사용 하지 않습니다. 한 번에 여러 2D 앱이 환경에 존재할 수 있습니다.

이 항목의 나머지 부분에서는 2 차원 환경의 디자인 고려 사항을 설명 합니다.

## <a name="launching-2d-apps"></a>2D 앱 시작

![혼합 현실 시작 메뉴](images/MR-start-options.png)

시작 메뉴에서 모든 앱을 시작 하지만, 응용 프로그램 시작 관리자 역할을 하는 3D 개체를 만들 수도 있습니다. 자세한 내용은 [이 비디오](https://www.youtube.com/watch?v=TxIslHsEXno) 를 참조 하세요.

## <a name="the-2d-app-input-overview"></a>2D 앱 입력 개요

HoloLens 및 Mixed Reality 플랫폼 모두에서 키보드와 마우스를 사용할 수 있습니다. Bluetooth를 통해 HoloLens와 직접 키보드와 마우스를 연결할 수 있습니다. 혼합 현실 앱은 호스트 컴퓨터에 연결 된 마우스 및 키보드를 지원 합니다. 이 둘은 세밀 한 제어가 필요한 경우에 유용할 수 있습니다.

뿐만 아니라 보다 자연 스러운 입력 메서드도 지원 되며,이는 사용자가 앞에 있는 실제 키보드를 사용 하 여 책상에 앉아 있지 않거나 세밀 하 게 제어 해야 할 때 특히 유용할 수 있습니다.

추가 하드웨어 또는 코딩을 사용 하지 않으면 앱은 사용자가 사용 하는 벡터를 사용 하 여 사용자가 찾고 있는 벡터를 사용 합니다. 이는 마우스 포인터가 가상 장면에서 무언가를 가리키는 것 처럼 구현 됩니다.

일반적인 상호 작용에서 사용자는 앱의 컨트롤을 확인 하 여 강조 표시 합니다. 사용자는에서 제스처 (HoloLens) 또는 컨트롤러를 사용 하거나 음성 명령을 제공 하 여 동작을 트리거할 수 있습니다. 사용자가 텍스트 입력 필드를 선택 하면 소프트웨어 키보드가 표시 됩니다. 


![혼합 현실에서 팝업 키보드](images/MR-keyboard.png)

UWP 플랫폼에서 실행 되는 것 처럼 이러한 모든 상호 작용은 사용자의 추가 코딩 없이 자동으로 발생 합니다. HoloLens 및 Mixed Reality 헤드셋의 입력은 2D 앱에 대 한 터치 입력으로 표시 됩니다. 이는 기본적으로 여러 UWP 앱이 실행 되 고 혼합 현실에서 사용할 수 있음을 의미 합니다. 

즉, 몇 가지 추가 작업을 수행 하면 환경이 크게 향상 될 수 있습니다. 예를 들어 [음성 컨트롤이](https://developer.microsoft.com/windows/mixed-reality/voice_design) 특히 효과적일 수 있습니다. HoloLens 및 Mixed Reality 환경 모두 앱을 시작 하 고 상호 작용 하는 음성 명령을 지원 하 고 음성 지원을 포함 하 여이 접근 방식에 대 한 자연 스러운 확장으로 나타납니다. UWP 앱에 음성 지원을 추가 하는 방법에 대 한 자세한 내용은 [음성 상호 작용]( ../input/speech-interactions.md) 을 참조 하세요. 


### <a name="selecting-the-right-controller"></a>올바른 컨트롤러 선택

![혼합 현실 동작 컨트롤러](images/MR-controllers.png)

특히 다음과 같이 혼합 현실에서 사용 하기 위해 몇 가지 novel 입력 메서드를 디자인 했습니다.

* [손 모양 제스처](https://developer.microsoft.com/windows/mixed-reality/gestures) (HoloLens 전용, 하지만 2d 앱을 시작 하는 데만 사용 됨)
* [게임 패드 지원](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (두 환경)
* [Clicker 장치](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (HoloLens 전용)
* [동작 컨트롤러](/windows/mixed-reality/motion-controllers) (혼합 현실 장치만, 위에 표시 됨)

이러한 컨트롤러는 가상 개체와 상호 작용 하는 것이 자연스럽 고 정확 하 게 보입니다. 사용자가 무료로 얻을 수 있는 일부 상호 작용입니다. 예를 들어 HoloLens 선택 제스처 또는 이동 컨트롤러의 Windows 키 또는 트리거를 클릭 하면 사용자에 게 코딩 하지 않고 다시 필요한 입력 응답이 생성 됩니다.

또는 사용 가능한 추가 정보 및 입력을 활용 하는 코드를 추가 하는 것이 좋습니다. 예를 들어 위치 및 단추 누름을 고려 하는 코드를 작성 하는 경우 동작 컨트롤러를 사용 하 여 세밀 한 제어 수준으로 개체를 조작할 수 있습니다.

> [!NOTE]
> 요약 하자면, 보안 주체는 항상 사용자에 게 자연스럽 고 원활한 입력 메서드를 최대한 제공 해야 합니다.


## <a name="2d-app-design-considerations-functionality"></a>2D 앱 디자인 고려 사항: 기능

혼합 현실 플랫폼에서 잠재적으로 사용 될 수 있는 UWP 앱을 만들 때는 몇 가지 사항을 염두에 두어야 합니다.

* 끌어서 놓기가 동작 컨트롤러, gamepads 또는 제스처와 함께 사용 하는 경우 제대로 작동 하지 않을 수 있습니다. 응용 프로그램이 끌어서 놓기를 크게 좌우 하는 경우 개체를 새 위치로 이동할지 여부를 확인 하는 대화 상자를 표시 하는 것과 같이이 작업을 지 원하는 대체 방법을 제공 해야 합니다.

* 사운드가 어떻게 변경 될 수 있는지 알아 두어야 합니다. 앱에서 음향 효과를 생성 하는 경우에는 가상 세계에서 소리의 소스가 앱의 고정 된 위치에 표시 됩니다. 사용자가 앱에서 벗어나면 소리가 감소 합니다. 자세한 내용은 [공간 사운드](/windows/mixed-reality/spatial-sound) 를 참조 하세요.

* 보기의 필드를 고려 하 고 affordances를 제공 합니다. 모든 장치가 컴퓨터 모니터로 보기의 필드를 크게 제공 하는 것은 아닙니다. 자세한 내용은 [Holographic frame](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) 을 참조 하십시오. 또한 사용자가 실행 중인 앱에서 멀리 떨어져 있을 수 있습니다. 즉, 전 세계의 다른 위치에 있는 벽에 고정 된 앱이 표시 될 수 있습니다 (실제 또는 가상). 앱에서 사용자에 게 주의가 필요 하거나 전체 보기가 항상 표시 되지 않는 것을 고려해 야 할 수 있습니다. 알림 메시지를 사용할 수 있지만 사용자의 주의가 가능한 또 다른 방법은 소리 또는 [음성](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs) 경고를 생성 하는 것입니다.

* 사용자가 가상 환경에서 이동 하 고 크기를 조정할 수 있도록 2D 앱에 [앱 표시줄이](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box)  자동으로 부여 됩니다. 뷰를 세로로 크기 조정 하거나 크기를 조정 하 여 동일한 가로 세로 비율을 유지할 수 있습니다.


## <a name="2d-app-design-considerations-uiux"></a>2D 앱 디자인 고려 사항: UI/UX

* [탐색 보기](../controls-and-patterns/navigationview.md)와 같은 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/) 을 구현 하는 XAML 컨트롤 및 [아크릴](../style/acrylic.md) All Work와 같은 효과는 2d 혼합 현실 앱에서 특히 잘 작동 합니다.

* 혼합 현실 장치에서 또는 혼합 현실 시뮬레이터에서 앱 텍스트와 windows 크기를 테스트 합니다. 앱에는 기본 windows 크기가 853x480 유효 픽셀로 포함 됩니다. 더 큰 글꼴 크기 (약 32의 포인트 크기 권장)를 사용 하 고 [Hololens 용 기존 유니버설 앱의 업데이트](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)를 읽습니다. 문서 [입력 체계](https://developer.microsoft.com/windows/mixed-reality/typography) 는이 항목에 대해 자세히 설명 합니다. Visual Studio에서 작업 하는 경우 올바른 배율 및 차원이 포함 된 보기를 제공 하는 57 "HoloLens 2D 앱에 대 한 XAML 디자인 편집기 설정이 있습니다. 

![혼합 현실 앱에 표시 되는 텍스트는 커야 합니다.](images/MR-text.png)

* [사용자의 응시는 마우스](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting)입니다. 사용자가 무언가를 살펴보면 **터치 가리키기** 이벤트로 작동 하므로 단순히 개체를 살펴보면 실수로 팝업 또는 기타 원치 않는 상호 작용을 트리거할 수 있습니다. 앱이 혼합 현실에서 현재 실행 되 고 있는지 검색 하 고이 동작을 변경 해야 할 수 있습니다. 아래의 **런타임 지원** 을 참조 하세요. 

* 사용자가 동작 컨트롤러를 사용 하 여 요소 또는 요소를 gazes **터치 가리키기** 이벤트가 발생 합니다. 이는 **Pointerpoint** 이 **터치** 이지만 **IsInContact** 가 **false** 인 **pointerpoint** 로 구성 됩니다. 특정 형식의 커밋이 발생 하는 경우 (예를 들어, clicker 장치를 누르고, 이동 컨트롤러 트리거를 누르거나, 음성 인식 헤드 "선택"), **IsInContact** 가 **True** 인 **pointerpoint** 를 사용 하 여 **터치 누름** 이 발생 합니다. 이러한 입력 이벤트에 대 한 자세한 내용은 [터치 조작](../input/touch-interactions.md) 을 참조 하세요.

* 마우스로 가리킬 때와는 정확 하지 않습니다. 마우스 대상 또는 단추가 작을수록 사용자에 게 문제가 발생할 수 있으므로 컨트롤의 크기를 적절 하 게 조정 합니다. 터치를 위해 설계 된 경우 혼합 현실에서 작동 하지만 런타임에 일부 단추를 확대 하도록 결정할 수도 있습니다. [Hololens 용 기존 유니버설 앱 업데이트](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)를 참조 하세요.

* HoloLens는 검정을 밝은 색으로 정의 합니다. 단지 렌더링 되지 않으므로 "실제 세계"를 통해 표시 될 수 있습니다. 응용 프로그램에서 혼동을 일으키는 경우 검정색을 사용 하면 안 됩니다. 혼합 현실 헤드셋에서 검은색은 검은색입니다.

* HoloLens는 앱에서 색 테마를 지원 하지 않으며, 사용자를 위한 최상의 환경을 보장 하기 위해 기본적으로 blue로 설정 됩니다. 색을 선택 하는 방법에 대 한 자세한 내용은 혼합 현실 디자인에서 색 및 재질 사용에 대해 설명 하는 [이 항목](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials) 을 참조 하세요.


## <a name="other-points-to-consider"></a>고려할 기타 사항

* [바탕 화면 브리지가](/windows/msix/desktop/source-code-overview) 기존 (Win32) 데스크톱 앱을 Windows 10 및 Microsoft Store로 가져오는 데 도움이 되지만,이 시점에서 HoloLens 또는 혼합 현실에서 실행 되는 앱을 만들 수는 없습니다.




## <a name="runtime-support"></a>런타임 지원

앱이 런타임에 혼합 현실 장치에서 실행 되 고 있는지 확인할 수 있으며, 컨트롤의 크기를 조정할 수 있는 기회를 사용 하거나 다른 방법으로 헤드셋에서 사용할 앱을 최적화할 수 있습니다.

다음은 응용 프로그램이 혼합 된 현실 장치에서 사용 되는 경우에만 XAML TextBlock 컨트롤 내에서 텍스트의 크기를 조정 하는 짧은 코드 부분입니다.

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
                textBlock.FontSize = 14;
            }

```





## <a name="related-articles"></a>관련된 문서


* [셸에서 Api를 사용 하는 앱에 대 한 현재 제한 사항](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [2D 앱 빌드](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens: Microsoft HoloLens 용 UWP 2D 앱 빌드](https://channel9.msdn.com/Events/Build/2016/B854)
* [조건부 XAML](../../debug-test-perf/conditional-xaml.md)
