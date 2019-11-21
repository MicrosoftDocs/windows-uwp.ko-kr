---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: 시각적 계층
description: Windows.UI.Composition API는 프레임워크 계층(XAML)과 그래픽 계층(DirectX) 간의 컴퍼지션 계층에 대한 액세스를 제공합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ac41d461982a39a939e460b7a81b144e5a08fdb3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255521"
---
# <a name="visual-layer"></a>시각적 계층

시각적 계층은 고성능, 그래픽 유지 모드 API, 효과 및 애니메이션을 제공하며 Windows 장치 전체에서 모든 UI의 기반이 됩니다. You define your UI in a declarative manner and the Visual layer relies on graphics hardware acceleration to ensure your content, effects and animations are rendered in a smooth, glitch-free manner independent of the app's UI thread.

눈에 띄는 주요 사항은 다음과 같습니다.

* 친숙한 WinRT API
* 보다 동적인 UI와 상호 작용에 대한 설계
* 디자인 도구에 맞게 조정된 개념
* 갑작스런 성능 절벽 없는 선형 확장성

Windows UWP 앱은 이미 UI 프레임 워크 중 하나를 통해 시각적 계층을 사용하고 있습니다. 사용자 지정 렌더링, 효과 및 애니메이션을 위해 매우 적은 노력으로도 시각적 계층을 바로 활용할 수 있습니다.

![UI 프레임워크 계층화: 프레임워크 계층(Windows.UI.XAML)은 그래픽 계층(DirectX)에 빌드된 시각적 계층(Windows.UI.Composition)에 빌드됩니다.](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>시각적 계층에는 무엇이 있나요?

시각적 계층의 주요 기능은 다음과 같습니다.

1. **콘텐츠**: 그린 사용자 지정 콘텐츠를 경량 합성
1. **효과**: 효과를 애니메이션화, 연결, 사용자 지정할 수 있는 실시간 UI 효과 시스템
1. **애니메이션**: UI 스레드에서 관계 없이 실행되는 표현력이 뛰어나며, 프레임 워크를 알 수 없는 애니메이션

### <a name="content"></a>콘텐츠

시각을 사용하여 애니메이션 및 효과가 사용할 수 있도록 콘텐츠는 호스팅 및 변환됩니다. 클래스 계층의 기반에는 작성자에서 시각 상태에 대한 앱 프로세스의 가벼운 스레드 민첩 프록시인 [**시각**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual) 클래스가 있습니다. Sub-classes of Visual include  [**ContainerVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) to allow for children to create trees of visuals and [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) that contains content and can be painted with either solid colors, custom drawn content or visual effects. 함께 이러한 시각 유형은 2D UI에 대한 시각적 트리 구조 및 대부분의 XAML FrameworkElements를 차지합니다.

자세한 내용은 [컴퍼지션 시각](composition-visual-tree.md) 개요를 참조하세요.

### <a name="effects"></a>효과

시각적 계층의 효과 시스템으로 시각 또는 시각 트리에 체인 필터 및 투명 효과를 적용할 수 있습니다. 이는 UI 효과 시스템이며 이미지 및 미디어 효과와 혼동해서는 안됩니다. 효과는 애니메이션 시스템과 함께 작동하며 사용자에게 UI 스레드와 독립적으로 렌더링되는 효과 속성의 원활하고 동적인 애니메이션을 제공합니다. 시각적 계층의 효과는 대화형 맞춤 환경을 만드는 데 결합하고 애니메이션화할 수 있는 창의적인 구성 요소를 제공합니다.

애니메이션 효과 체인뿐만 아니라 시각적 계층은 애니메이션 조명에 응답하여 물질 속성을 모사하도록 하는 조명 모델 또한 지원합니다. 시각 효과는 그림자를 비출 수도 있습니다. 조명 및 그림자를 결합하여 깊이 있고 실제적인 인식 만들 수 있습니다.

자세한 내용은 [컴퍼지션 효과](composition-effects.md) 개요를 참조하세요.

### <a name="animations"></a>애니메이션

시각적 계층의 애니메이션 시스템으로 시작을 이동하고 효과를 애니메이션화하고 전화, 클립 및 기타 속성을 유발할 수 있습니다.  It is a framework agnostic system that has been designed from the ground up with performance in mind.  It runs independently from the UI thread to ensure smoothness and scalability.  While it lets you use familiar KeyFrame animations to drive property changes over time, it also lets you set up mathematical relationships between different properties, including user input, letting you directly craft seamless choreographed experiences.

자세한 내용은 [컴퍼지션 애니메이션](composition-animation.md) 개요를 참조하세요.

### <a name="working-with-your-xaml-uwp-app"></a>XAML UWP 앱 사용

XAML 프레임워크에서 생성한 시각을 가져와 [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting)에서 [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) 클래스를 사용하여 시각적 FrameworkElement를 백업할 수 있습니다. 프레임워크에서 사용자를 위해 만든 시각 효과는 사용자 지정에 제한이 있을 수 있습니다. 이는 프레임워크가 오프셋, 변환 및 수명을 관리하기 때문입니다. 하지만 직접 시각 효과를 만들고 ElementCompositionPreview를 통해 또는 시각 트리 구조의 어딘가에 있는 기존 ContainerVisual에 추가하여 기존 XAML 요소에 이를 연결할 수 있습니다.

자세한 내용은 [XAML로 시각 계층 사용](using-the-visual-layer-with-xaml.md) 개요를 참조하세요.

### <a name="working-with-your-desktop-app"></a>Working with your desktop app

You can use the Visual layer to enhance the look, feel, and functionality of your WPF, Windows Forms, and C++ Win32 desktop apps. You can migrate islands of content to use the Visual layer and keep the rest of your UI in its existing framework. This means you can make significant updates and enhancements to your application UI without needing to make extensive changes to your existing code base.

자세한 내용은 [시각적 계층을 사용하여 데스크톱 앱 현대화](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [**Full reference documentation for the API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* [WindowsUIDevLabs GitHub](https://github.com/microsoft/WindowsCompositionSamples)의 고급 UI 및 Composition 샘플
* [Windows.UI.Composition Sample Gallery](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui Twitter feed ](https://twitter.com/windowsui)
* 이 API에 대한 Kenny Kerr의 MSDN 문서 [그래픽 및 애니메이션 - Windows Composition(Windows 10)](https://msdn.microsoft.com/magazine/mt590968)을 참조하세요.
