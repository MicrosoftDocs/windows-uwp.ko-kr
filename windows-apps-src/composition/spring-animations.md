---
title: 스프링 애니메이션
description: 자연스러운 스프링 동작 애니메이션을 사용하는 방법에 대해서 알아봅니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 9e00aa383bcce17b7cd6b67514647c2f6137cc32
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8204372"
---
# <a name="spring-animations"></a>스프링 애니메이션

이 문서에서는 스프링 NaturalMotionAnimations 사용 방법을 설명합니다.

## <a name="prerequisites"></a>필수 구성 요소

여기에서는 여러분이 이 문서에서 다룬 개념에 익숙하다고 가정합니다.

- [자연스러운 동작 애니메이션](natural-animations.md)

## <a name="why-springs"></a>왜 스프링인가요?

끈적한 장난감에서부터 스프링이 달린 블록을 이용한 물리학 수업에 이르기까지, 스프링은 우리가 모두가 경험한 적 있는 흔한 동작 경험입니다. 스프링이 진동하는 동작은 흔히 보는 사람들에게 장난스럽고 경쾌한 정서적 반응을 유도합니다. 따라서 기존의 큐빅 베지어보다 더 "튀어 나올" 듯한 생생한 동작 경험을 최종 사용자에게 제공하고자 하는 사람들이 응용 프로그램 UI로 스프링의 동작을 선택하곤 합니다. 이러한 경우 스프링 동작은 더 생동감 있는 동작 경험을 창출할 뿐만 아니라, 애니메이션을 적용한 기존 및 신규 컨텐츠에 주의를 집중시키는 데 도움이 될 수 있습니다. 응용 프로그램 브랜딩이나 동작 언어에 따라 진동이 매우 두드러지고 눈에 띄는 경우가 있고, 매우 미묘한 경우도 있습니다.

![스프링 애니메이션이 적용된 동작](images/animation/offset-spring.gif)
![큐빅 베지어 애니메이션이 적용된 동작](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>UI에 스프링 사용

앞서 언급했듯이 스프링 동작은 앱에 통합해 매우 친근하면서도 장난기 있는 UI 경험을 창출할 수 있는, 유용한 동작일 수 있습니다. UI에 스프링이 사용된 일반적인 사례:

| 스프링 사용 설명 | 시각적 예 |
| ------------------------ | -------------- |
| "입체적이고" 활기차 보이는 동작 경험을 창출합니다. (애니메이션 배율) | ![스프링 애니메이션으로 크기 조정 동작](images/animation/scale-spring.gif) |
| 미묘하게 동작 경험이 활발하게 느껴지도록 만듦(애니메이션 오프셋) | ![스프링 애니메이션으로 오프셋 동작](images/animation/offset-spring.gif) |

이러한 각각의 경우, 새로운 값이 "튀어 나오고" 그 주위를 진동하거나 초기 속도로 현재 값 주위를 진동함으로써 스프링 동작이 트리거될 수 있습니다.

![스프링 애니메이션 진동](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>스프링 동작 정의

NaturalMotionAnimation API를 사용하여 스프링 경험을 만듭니다. 특히, Compositor에서 Create* 메서드를 사용하여 SpringNaturalMotionAnimation을 생성합니다. 그런 다음 다음과 같은 동작 속성을 정의할 수 있습니다.

- DampingRatio – 애니메이션에 사용된 스프링 동작의 감쇠 수준을 나타냅니다.

| 감쇠 비율 값 | 설명 |
| ------------------- | ----------- |
| DampingRatio = 0 | 불감쇠 – 스프링이 장시간 동안 진동합니다. |
| 0 < DampingRatio < 1 | 미급 감쇠 – 스프링 진동 범위가 다양합니다. |
| DampingRatio = 1 | 임계 감쇠 – 스프링이 진동하지 않습니다. |
| DampingRation > 1 | 감쇠 – 스프링이 진동하지 않고 급격하게 감속해 대상에 빠르게 도달합니다. |

- 소요 시간 – 스프링이 한 번 진동하는 데 소요되는 시간입니다.
- 최종/시작 값 – 스프링 동작의 정의된 시작 및 끝 위치(정의되지 않은 경우 시작 값 및/또는 최종 값이 현재 값).
- 초기 속도 – 동작에 프로그래밍된 초기 속도입니다.

KeyFrameAnimations와 동일한 동작의 속성 집합도 정의할 수 있습니다.

- DelayTime / Delay Behavior
- StopBehavior

오프셋 및 배율/크기에 애니메이션을 적용하는 일반적인 경우, 다양한 유형의 스프링 DampingRatio 및 소요 시간에 대해 Windows 디자인 팀에서는 다음과 같은 값을 권장합니다.

| 속성 | 보통 스프링 | 감쇠된 스프링 | 덜 감쇠된 스프링 |
| -------- | ------------- | --------------- | -------------------- |
| 오프셋 | 감쇠 비율 = 0.8 <br/> 소요 시간 = 50ms | 감쇠 비율 = 0.85 <br/> 소요 시간 = 50ms | 감쇠 비율 = 0.65 <br/> 소요 시간 = 60ms |
| 배율/크기 | 감쇠 비율 = 0.7 <br/> 소요 시간 = 50ms | 감쇠 비율 = 0.8 <br/> 소요 시간 = 50ms | 감쇠 비율 = 0.6 <br/> 소요 시간 = 60ms |

속성을 정의한 후에는 스프링 NaturalMotionAnimation을 CompositionObject의 StartAnimation 메서드나 InteractionTracker InertiaModifier의 동작 속성에 전달할 수 있습니다.

## <a name="example"></a>예

이 예에서는 사용자가 확장 버튼을 클릭하면 탐색 창이 탄력적인 진동 동작의 애니메이션으로 표현되는 탐색 및 캔버스 UI 경험을 만듭니다.

![클릭 시 스프링 애니메이션](images/animation/spring-animation-on-click.gif)

탐색 패널이 표시될 때 클릭한 이벤트 내에서 스프링 애니메이션을 정의하는 것으로 시작합니다. 그런 다음 Expression을 사용해 FinalValue를 정의하는 InitialValueExpression 기능을 사용해 애니메이션의 속성을 정의합니다. 또한 창이 열렸는지 추적하고, 준비가 되면 애니메이션을 시작합니다.

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue + 250”;
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue - 250”;
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

이 동작을 입력에 연결하고 싶으면 어떻게 할까요? 최종 사용자가 스와이프 아웃할 경우 스프링 동작으로 창이 등장하도록 하려면? 무엇보다도, 사용자가 강하거나 빠르게 스와이프할 경우 동작이 최종 사용자의 속도에 따라 조정되어야 합니다.

![스와이프 시 스프링 애니메이션](images/animation/spring-animation-on-swipe.gif)

이를 위해 동일한 스프링 애니메이션을 InteractionTracker가 있는 InertiaModifier로 전달할 수 있습니다. InputAnimations 및 InteractionTracker에 대한 자세한 내용은 [InteractionTracker를 사용한 사용자 지정 조작 경험](interaction-tracker-manipulations.md)을 참조하세요. 이 코드 예제에서는 InteractionTracker 및 VisualInteractionSource를 이미 설정했다고 가정합니다. 여기에서는 NaturalMotionAnimation(이 경우 스프링)에 포함할 InertiaModifiers를 만드는 데 중점을 둘 것입니다.

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

이제 UI에 프로그래밍 방식과 입력 방식의 스프링 애니메이션이 모두 있습니다!

요약하면 앱에서 스프링 애니메이션을 사용하는 단계는 다음과 같습니다.

1. Compositor에서 SpringAnimation을 만듭니다.
1. 기본값이 아닌 값을 원한다면 SpringAnimation의 속성을 정의합니다.
    - DampingRatio
    - 기간
    - 최종 값
    - 초기값
    - 초기 속도
1. 대상에 할당합니다.
    - CompositionObject 속성에 애니메이션을 적용하려면 StartAnimation에 SpringAnimation을 매개 변수로 전달합니다.
    - 입력과 함께 사용하려면 InertiaModifier의 NaturalMotion 속성을 SpringAnimation으로 설정합니다.

