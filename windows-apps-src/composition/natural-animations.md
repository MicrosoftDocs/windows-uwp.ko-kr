---
author: jwmsft
title: 자연스러운 동작 애니메이션
description: 자연스러운 동작 애니메이션과 앱 UI에서 애니메이션을 사용하는 방법에 대해서 알아봅니다.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 537e722917f00d590428dd2b5ee2d24e023e52b6
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5666459"
---
# <a name="natural-motion-animations"></a>자연스러운 동작 애니메이션

이 문서에서는 NaturalMotionAnimation 공간에 대한 간략한 개요와 이러한 유형의 애니메이션을 UI에 개념적으로 사용하는 방법에 대해 설명합니다.

## <a name="making-motion-feel-familiar-and-natural"></a>익숙하고 자연스러운 동작 만들기

멋진 앱은 사용자의 주의를 끌고 유지하는 경험을 창출하고, 사용자에게 작업 순서를 친절하게 안내하는 앱입니다. 동작은 사용자 인터페이스와 사용자 경험을 구별하는 중요한 차별화 요소로, 사용자와 상호 작용하는 응용 프로그램 간의 연결을 이끌어냅니다. 연결 성능이 우수할수록 최종 사용자의 참여도와 만족도가 높아집니다.

동작을 통해 이러한 연결을 빌드하는 데 기여하는 한 가지 방법은 사용자에게 친숙한 모양과 느낌의 경험을 창출하는 것입니다. 사용자는 실제 경험을 바탕으로 동작이 이루어질 것이라는 무의식적 기대를 품고 있습니다. 우리는 물체가 어떻게 바닥을 가로질러 미끄러지고, 테이블에서 떨어지고, 서로 부딪혀 튕겨 나가고, 스프링 끝에서 진동하는지 관찰합니다. 실제 물리학 법칙을 기반으로 이러한 기대에 부합하는 동작은 더 자연스러워 보입니다. 이렇게 하면 동작이 더 자연스러워지고 반응성도 우수해지지만, 무엇보다 중요한 것은 더욱 매력적이고 즐거운 경험을 선사할 수 있다는 점입니다.

![애니메이션 없이 크기 조정 동작](images/animation/scale-no-animation.gif)
![큐빅 베지어로 크기 조정 동작](images/animation/scale-cubic-bezier.gif)
![스프링 애니메이션으로 크기 조정 동작](images/animation/scale-spring.gif)

그 결과 앱 사용자의 참여도와 보유율이 높아집니다.

## <a name="balancing-control-and-dynamism"></a>제어와 동적 특성의 균형 달성

기존 UI에서 동작을 설명하는 일반적인 방법은 [KeyFrameAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.keyframeanimation)입니다. KeyFrames는 디자이너와 개발자가 시작, 끝 및 보간을 정의할 수 있는 최고의 제어 기능을 제공합니다. KeyFrame 애니메이션은 많은 경우에 매우 유용하지만, 그다지 동적이지 않습니다. 동작에 적응성이 없으며 어떤 조건에서도 동일하게 보입니다.

스펙트럼의 반대편 끝에는 게임 및 물리학 엔진에서 흔히 볼 수 있는 시뮬레이션이 있습니다. 이러한 경험은 흔히 사용자가 상호 작용하는 가장 현실적이고 역동적인 것으로, 일상에서 경험하는 분위기와 무작위적인 느낌을 자아냅니다. 이 경우 동작이 보다 생생하고 역동적으로 구현되더라도 디자이너와 개발자의 제어 역량이 떨어지므로 기존 UI에 통합하기가 더 어렵습니다.

![제어 스펙트럼 다이어그램](images/animation/natural-motion-diagram.png)

[NaturalMotionAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.naturalmotionanimation)은 시작/종료와 같은 애니메이션의 중요한 요소에 대한 제어 균형을 유지하면서도 동작이 자연스럽고 동적인 모양과 느낌을 유지하도록 하여, 이러한 구분으로 인한 간극을 메웁니다.

> [!NOTE]
> NaturalMotionAnimations는 KeyFrame Animations를 대신하는 것이 아닙니다. Fluent 디자인 언어에서도 KeyFrames 사용이 권장되는 영역이 있습니다. NaturalMotionAnimations는 동작이 필요하지만 KeyFrame Animations만으로는 충분히 동적이지 않은 영역에 사용하기 위한 것입니다.

## <a name="using-naturalmotionanimations"></a>NaturalMotionAnimations 사용

가을 크리에이터스 업데이트부터 새로운 동작 경험 **스프링 애니메이션**에 액세스할 수 있습니다. 스프링에 대해 자세히 알아보려면 [스프링 애니메이션](spring-animations.md)을 참조하세요.

이 동작 유형은 새로운 NaturalMotionAnimation을 사용하여 구현됩니다. 이 새로운 유형의 애니메이션을 이용해 개발자는 제어와 동적 특성의 균형을 유지하면서 UI에 더욱 친숙하고 자연스러운 동작을 구현할 수 있습니다. 다음과 같은 기능이 포함됩니다.

- 시작 및 종료 값을 정의합니다.
- InitialVelocity를 정의하고 InteractionTracker로 입력에 연결합니다.
- 동작별 속성(예: 스프링의 DampingRatio)을 정의합니다.

시작하기 위한 일반 공식:

1. **Create** 메서드 중 하나를 사용해 Compositor 없이 NaturalMotionAnimation을 만듭니다.
1. 애니메이션의 속성을 정의합니다.
1. NaturalMotionAnimation을 CompositionObject의 StartAnimation 호출에 대한 매개 변수로 전달합니다.
    - 또는 InteractionTracker InertiaModifier의 동작 속성을 설정합니다.

스프링 NaturalMotionAnimation을 사용해 시각적 개체 "스프링"을 새로운 X 오프셋 위치에서 사용할 수 있도록 만드는 예:

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```
