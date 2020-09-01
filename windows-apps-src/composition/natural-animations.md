---
title: 자연 스러운 동작 애니메이션
description: 자연 스러운 동작 애니메이션 및 앱 UI에서 사용 하는 방법에 대해 알아봅니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 02c76991a60205042642f57fed475755db8c8071
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174087"
---
# <a name="natural-motion-animations"></a>자연 스러운 동작 애니메이션

이 문서에서는 NaturalMotionAnimation 공간에 대 한 간략 한 개요와 UI에서 이러한 형식의 애니메이션 사용에 대해 개념적으로 생각 하는 방법을 제공 합니다.

## <a name="making-motion-feel-familiar-and-natural"></a>친숙 하 고 자연 스러운 동작 만들기

뛰어난 앱은 사용자에 게 주의가 필요한 환경을 만들고 사용자에 게 작업을 안내 하는 데 도움을 주는 것입니다. 동작은 사용자의 사용자 인터페이스와 상호 작용 하는 응용 프로그램 간의 연결을 분리 하는 키 차별화 요소입니다. 더 나은 연결, 최종 사용자의 참여 및 만족도가 높아집니다.

이 연결을 만드는 데 도움이 되는 단방향 동작은 사용자에 게 친숙 하 고 친숙 한 환경을 만드는 것입니다. 사용자는 실제 경험을 기반으로 하는 동작을 어떻게 unconscious 기대 합니다. 개체가 바닥에서 어떻게 이동 하는지 확인 하 고, 테이블을 분리 하 고, 서로를 oscillate 하 고, 스프링을 사용 합니다. 실제 목표를 기반으로 하 여 이러한 기대를 활용 하 고 눈에 잘 자연스럽 게 보이도록 하는 동작입니다. 이 동작은 보다 자연스럽 고 대화형으로 진행 되지만, 더 중요 한 것은 전체 환경이 기억 하기 쉽고 매력적인.

![](images/animation/scale-no-animation.gif)
 ![ ](images/animation/scale-cubic-bezier.gif)
 스프링 애니메이션이 포함 된 입방 형 3 차원 배율 동작을 사용 하 여 애니메이션 크기 조정 동작 없이 동작 크기 조정 ![](images/animation/scale-spring.gif)

결과적으로 앱에 대 한 사용자 참여 및 보존이 높아집니다.

## <a name="balancing-control-and-dynamism"></a>균형 조정 제어 및 활력과

기존 UI에서 [Key프레임 애니메이션](/uwp/api/windows.ui.composition.keyframeanimation)은 동작을 설명 하는 널리 사용할 수 있는 방법입니다. 키 프레임은 디자이너와 개발자가 시작, 종료 및 보간을 정의 하는 데 가장 한 제어를 제공 합니다. 이는 대부분의 경우에 매우 유용 하지만, 키 프레임 애니메이션은 매우 동적이 지 않습니다. 동작은 적응이 아니고 모든 조건에서 동일 하 게 보입니다.

스펙트럼의 다른 쪽 끝에는 게임 및 물리 엔진에 종종 표시 되는 시뮬레이션이 있습니다. 이러한 환경은 사용자가 매일 볼 수 있는 ambiance 및 임의성을 만드는 것과 사용자가 상호 작용 하는 가장 많은 수명 및 동적입니다. 이렇게 하면 동작에 더 많은 연결이 가능 하며, 디자이너와 개발자는 제어 수준이 낮으므로 기존 UI에 통합 하기가 더 어려워집니다.

![컨트롤 스펙트럼 다이어그램](images/animation/natural-motion-diagram.png)

이 나누기를 연결 하는 데 도움이 되는 [NaturalMotionAnimation](/uwp/api/windows.ui.composition.naturalmotionanimation)가 있습니다. 시작/종료와 같이 애니메이션의 중요 한 요소에 대 한 컨트롤의 균형을 유지 하 고 있으며 자연스럽 고 동적인 동적 동작을 유지 하는 데 도움이 됩니다.

> [!NOTE]
> NaturalMotionAnimations는 키 프레임 애니메이션을 대체 하기 위한 것이 아닙니다. 흐름 디자인 언어에서 키프레임이 권장 되는 위치가 여전히 있습니다. NaturalMotionAnimations는 동작이 필요한 위치에서 사용 되며, 키 프레임 애니메이션은 동적으로 충분 하지 않습니다.

## <a name="using-naturalmotionanimations"></a>NaturalMotionAnimations 사용

가 중 작성자 업데이트를 시작 하면 새로운 동작 환경 ( **스프링 애니메이션**)에 액세스할 수 있습니다. 스프링의 심층적 연습에 대해서는 [스프링 애니메이션](spring-animations.md) 을 참조 하세요.

이 동작 유형은 새로운 NaturalMotionAnimation – 개발자가 컨트롤 및 활력과의 균형을 유지 하 여 UI에 보다 친숙 하 고 자연 스러운 동작을 빌드할 수 있도록 하는 새로운 애니메이션 유형인 새로운 애니메이션 유형인를 사용 하 여 구현 됩니다. 이러한 기능은 다음과 같은 기능을 제공 합니다.

- 시작 값과 끝 값을 정의 합니다.
- InteractionTracker를 사용 하 여 입력에 대 한 InitialVelocity 및 연결을 정의 합니다.
- 동작 관련 속성 (예: 스프링의 DampingRatio)을 정의 합니다.

시작 하기 위한 일반 수식:

1. **Create** 메서드 중 하나를 사용 하 여 Compositor에서 NaturalMotionAnimation을 만듭니다.
1. 애니메이션의 속성을 정의 합니다.
1. CompositionObject의 StartAnimation 호출에 NaturalMotionAnimation를 매개 변수로 전달 합니다.
    - 또는 InteractionTracker InertiaModifier의 동작 속성으로 설정 합니다.

스프링 NaturalMotionAnimation를 사용 하 여 새 X 오프셋 위치로 시각적 개체 "스프링"을 만드는 기본적인 예제는 다음과 같습니다.

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```