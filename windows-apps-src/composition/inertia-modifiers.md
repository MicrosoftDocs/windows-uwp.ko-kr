---
title: 관성 한정자를 사용하여 끌기 지점 만들기
description: InteractionTracker의 InertiaModifier 기능을 사용하여 지정된 지점으로 끄는 동작 경험을 만드는 방법에 대해 알아봅니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: f99ebc4b98c87a4bc6d77fd2c626f481563e50c5
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7986703"
---
# <a name="create-snap-points-with-inertia-modifiers"></a>관성 한정자로 끌기 지점 만들기

이 문서에서는 InteractionTracker의 InertiaModifier 기능을 사용하여 지정된 지점으로 끄는 동작 경험을 만드는 방법에 대해 자세히 살펴 봅니다.

## <a name="prerequisites"></a>필수 구성 요소

여기에서는 여러분이 이 문서에서 다룬 개념에 익숙하다고 가정합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [InteractionTracker를 사용한 사용자 지정 조작 경험](interaction-tracker-manipulations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="what-are-snap-points-and-why-are-they-useful"></a>끌기 지점이란 무엇이고 왜 유용할까요?

사용자 지정 조작 경험을 빌드할 때 스크롤 및 확대/축소 가능한 캔버스 내에 InteractionTracker가 항상 놓이는, 특수한 _배치 지점_을 만들면 유용할 수 있습니다. 이 지점을 흔히 _끌기 지점_이라고 합니다.

다음 예에서 보듯이, 스크롤을 사용하면 UI가 여러 이미지 사이에서 이상한 위치에 놓일 수 있습니다.

![끌기 지점이 없는 경우의 스크롤 동작](images/animation/snap-points-none.gif)

끌기 지점을 추가하면 이미지 사이에서 스크롤 동작을 멈출 경우 지정된 위치로 "고정"됩니다. 끌기 지점을 사용하면 이미지 스크롤 동작이 훨씬 깔끔하고 빠르게 응답하는 환경을 만들 수 있습니다.

![단일 끌기 지점으로 스크롤](images/animation/snap-points-single.gif)

## <a name="interactiontracker-and-inertiamodifiers"></a>InteractionTracker 및 InertiaModifier

InteractionTracker를 사용하여 사용자 지정 조작 경험을 빌드하는 경우, InertiaModifier를 활용하여 끌기 지점 동작 경험을 만들 수 있습니다. InertiaModifier는 기본적으로 InteractionTracker가 관성 상태에 진입할 때 어디에서 또는 어떻게 대상에 도달할 것인지 정의하는 방법 중 하나입니다. InteractionTracker의 X 또는 Y 위치 또는 배율 속성에 영향을 주는 InertiaModifier를 적용할 수 있습니다.

3개 유형의 InertiaModifier가 있습니다.

- InteractionTrackerInertiaRestingValue – 상호 작용이나 프로그래밍된 속도 이후에 최종적으로 놓일 위치를 수정하는 방법입니다. 사전 정의된 동작에 따라 InteractionTracker가 해당 위치로 이동합니다.
- InteractionTrackerInertiaMotion – 상호 작용 또는 프로그래밍된 속도 이후에 InteractionTracker가 수행할 특정 동작을 정의하는 방법입니다. 최종 위치는 이 동작으로부터 파생됩니다.
- InteractionTrackerInertiaNaturalMotion – 상호 작용이나 프로그래밍된 속도 이후에 물리학 기반 애니메이션(NaturalMotionAnimation)을 사용하여 최종적으로 놓이는 위치를 정의하는 방법입니다.

관성 상태에 진입할 때 InteractionTracker가 할당된 각각의 InertiaModifier를 평가하고 그 중 하나가 적용되는지 판단합니다. 즉, InteractionTracker에 여러 개의 InertiaModifier를 만들고 할당할 수 있지만 각각을 정의할 때는 다음을 수행해야 합니다.

1. 조건 정의 – 이 특정 InertiaModifier가 발생해야 하는 시점에 대한 조건문을 정의하는 식입니다. 이 경우 흔히 InteractionTracker의 NaturalRestingPosition(대상에 기본 관성을 지정)을 확인해야 합니다.
1. RestingValue/Motion/NaturalMotion 정의 – 조건이 충족될 때 발생하는 실제 Resting Value Expression, Motion Expression 또는 NaturalMotionAnimation을 정의합니다.

> [!NOTE]
> InertiaModifier의 조건 측면은 InteractionTracker가 관성 상태에 진입할 때 한 번만 평가됩니다. 그러나 InertiaMotion에 한해, 조건이 참인 한정자에 대해 매 프레임마다 동작 식이 평가됩니다.

## <a name="example"></a>예

이제 InertiaModifier를 사용하여 이미지의 스크롤 캔버스를 다시 만들 수 있는 끌기 지점 환경을 만드는 방법을 살펴보겠습니다. 이 예에서 각 조작은 단일 이미지를 통해 이동할 가능성이 있습니다. 이를 흔히 단일 필수 끌기 지점이라고 합니다.

InteractionTracker의 위치를 활용하게 될 Expression, InteractionTracker, VisualInteractionSource을 설정하는 것으로 시작하겠습니다.

```csharp
private void SetupInput()
{
    _tracker = InteractionTracker.Create(_compositor);
    _tracker.MinPosition = new Vector3(0f);
    _tracker.MaxPosition = new Vector3(3000f);

    _source = VisualInteractionSource.Create(_root);
    _source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    _source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
    _tracker.InteractionSources.Add(_source);

    var scrollExp = _compositor.CreateExpressionAnimation("-tracker.Position.Y");
    scrollExp.SetReferenceParameter("tracker", _tracker);
    ElementCompositionPreview.GetElementVisual(scrollPanel).StartAnimation("Offset.Y", scrollExp);
}
```

다음으로, 단일 필수 끌기 지점 동작은 콘텐츠를 위 또는 아래로 움직이기 때문에 스크롤할 수 있는 콘텐츠를 위로 움직이는 동작과 아래로 움직이는 동작의 두 가지 관성 한정자가 필요합니다.

```csharp
// Snap-Point to move the content up
var snapUpModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
// Snap-Point to move the content down
var snapDownModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
```

위로 또는 아래로 끌기 여부는 InteractionTracker가 자연스럽게 끌기 거리에 비례하여 도달하는 위치(끌기 위치 간 거리)에 따라 결정됩니다. 절반 지점을 지나친 경우는 아래로, 그렇지 않으면 위로 끌리게 됩니다. (이 예에서는 끌기 거리가 PropertySet에 저장됨)

```csharp
// Is NaturalRestingPosition less than the halfway point between Snap Points?
snapUpModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y < (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapUpModifier.Condition.SetReferenceParameter("prop", _propSet);
// Is NaturalRestingPosition greater than the halfway point between Snap Points?
snapDownModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y >= (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapDownModifier.Condition.SetReferenceParameter("prop", _propSet);
```

이 다이어그램은 현재 적용된 로직에 대한 시각적 설명을 제공합니다.

![관성 한정자 다이어그램](images/animation/inertia-modifier-diagram.png)

이제 각 InertiaModifier에 대한 Resting 값을 정의해야 합니다. InteractionTracker의 위치가 이전 끌기 위치 또는 다음 끌기 위치로 이동합니다.

```csharp
snapUpModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue - mod(this.StartingValue, prop.snapDistance)");
snapUpModifier.RestingValue.SetReferenceParameter("prop", _propSet);
snapForwardModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue + prop.snapDistance - mod(this.StartingValue, ” + “prop.snapDistance)");
snapForwardModifier.RestingValue.SetReferenceParameter("prop", _propSet);
```

마지막으로 InteractionTracker에 InertiaModifier를 추가합니다. 이제 InteractionTracker가 InertiaState를 입력하면 InertiaModifier의 상태를 확인하여 위치를 수정해야 하는지 확인합니다.

```csharp
var modifiers = new InteractionTrackerInertiaRestingValue[] { 
snapUpModifier, snapDownModifier };
_tracker.ConfigurePositionYInertiaModifiers(modifiers);
```