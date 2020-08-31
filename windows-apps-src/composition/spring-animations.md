---
title: 스프링 애니메이션
description: NaturalMotionAnimation Api를 사용 하 여 앱에서 스프링 동작 환경을 만드는 방법에 대해 알아봅니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: ecfb6fc001fbf42f70d40ee16abc45aa221c0a75
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053873"
---
# <a name="spring-animations"></a>스프링 애니메이션

이 문서에서는 스프링 NaturalMotionAnimations를 사용 하는 방법을 보여 줍니다.

## <a name="prerequisites"></a>필수 구성 요소

여기서는 다음 문서에서 설명 하는 개념에 대해 잘 알고 있다고 가정 합니다.

- [자연 스러운 동작 애니메이션](natural-animations.md)

## <a name="why-springs"></a>스프링은 무엇 인가요?

스프링은 몇 가지 지점에서 모든 경험을 경험 하는 일반적인 동작 환경입니다. slinky 장난감부터 스프링 연결 블록을 사용 하는 물리학 경험에 이르기까지 다양 합니다. 스프링의 진동 동작은 종종 incites 하 고 lighthearted 감정적 응답을 관찰 하는 사람 으로부터 응답 합니다. 결과적으로, 스프링의 동작은 기존 입방 형 3 차원 보다 최종 사용자에 게 "pop" 하는 livelier 동작 환경을 만들려는 사용자를 위해 응용 프로그램 UI를 효과적으로 변환 합니다. 이러한 경우 스프링 동작은 livelier 동작 환경을 만들 뿐만 아니라 새로운 또는 현재 애니메이션 효과를 주는 데 도움이 될 수도 있습니다. 응용 프로그램 브랜딩 또는 동작 언어에 따라 진동 범위가는 더 분명 하 게 표시 되 고 표시 되지만 다른 경우에는 더 미묘한 경우도 있습니다.

![](images/animation/offset-spring.gif)
 ![ 입방 형 3 차원 애니메이션이 있는 스프링 애니메이션 동작을 사용한 동작](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>UI에서 스프링 사용

앞서 언급 했 듯이, 스프링은 앱에 통합 하는 데 유용한 동작으로, 매우 친숙 하 고 플레이 된 UI 환경을 소개 합니다. UI에서 일반적으로 사용 되는 스프링은 다음과 같습니다.

| 스프링 사용 설명 | 시각적 예제 |
| ------------------------ | -------------- |
| 동작 환경을 "pop"로 설정 하 고 livelier를 확인 합니다. (배율에 애니메이션 적용) | ![스프링 애니메이션을 사용 하 여 동작 크기 조정](images/animation/scale-spring.gif) |
| 동작 환경을 미세 하 게 energetic (오프셋에 애니메이션 적용) | ![스프링 애니메이션을 사용 하 여 동작 오프셋](images/animation/offset-spring.gif) |

이러한 각 경우에 "도약 to"를 사용 하 여 스프링 동작을 트리거할 수 있으며, 새 값을 사용 하거나 초기 속도를 사용 하 여 현재 값 주위에서 진동 할 수 있습니다.

![스프링 애니메이션 진동 범위가](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>스프링 동작 정의

NaturalMotionAnimation Api를 사용 하 여 스프링 경험을 만듭니다. 특히 Compositor에서 Create * 메서드를 사용 하 여 SpringNaturalMotionAnimation를 만듭니다. 그런 다음 동작의 다음 속성을 정의할 수 있습니다.

- DampingRatio – 애니메이션에 사용 되는 스프링 동작의 댐핑 수준을 나타냅니다.

| 댐핑 비율 값 | Description |
| ------------------- | ----------- |
| DampingRatio = 0 | Undamped – 스프링은 오랜 시간 동안 oscillate 됩니다. |
| 0 < DampingRatio < 1 | Underdamped – 스프링은 약간에서 크게 oscillate. |
| DampingRatio = 1 | Criticallydamped – 스프링은 진동 범위가를 수행 하지 않습니다. |
| DampingRation > 1 | Overdamped – 스프링은 갑작스러운 감속을 사용 하 고 진동 범위가 없이 대상에 빠르게 도달 합니다. |

- 기간 – 스프링에서 단일 진동 범위가를 수행 하는 데 소요 되는 시간입니다.
- 최종/시작 값 – 스프링 동작의 시작 및 끝 위치를 정의 합니다 (정의 되지 않은 경우 시작 값 및/또는 최종 값이 현재 값이 됨).
- 초기 속도 – 동작에 대 한 프로그래밍 방식의 초기 속도입니다.

또한 Key프레임 애니메이션과 동일한 동작의 속성 집합을 정의할 수 있습니다.

- DelayTime/Delay 동작
- StopBehavior

오프셋 및 배율/크기에 애니메이션을 적용 하는 일반적인 경우에는 Windows 디자인 팀에서 다양 한 유형의 스프링에 대해 DampingRatio 및 기간에 대해 다음과 같은 값을 권장 합니다.

| 속성 | 일반 스프링 | 감소 되는지 스프링 | Less 감소 되는지 스프링 |
| -------- | ------------- | --------------- | -------------------- |
| Offset | 댐핑 비율 = 0.8 <br/> 기간 = 50 밀리초 | 댐핑 비율 = 0.85 <br/> 기간 = 50 밀리초 | 댐핑 비율 = 0.65 <br/> 기간 = 60 밀리초 |
| 크기 조정/크기 | 댐핑 비율 = 0.7 <br/> 기간 = 50 밀리초 | 댐핑 비율 = 0.8 <br/> 기간 = 50 밀리초 | 댐핑 비율 = 0.6 <br/> 기간 = 60 밀리초 |

속성을 정의한 후에는 스프링 NaturalMotionAnimation을 CompositionObject의 StartAnimation 메서드 또는 InteractionTracker InertiaModifier의 동작 속성에 전달할 수 있습니다.

## <a name="example"></a>예제

이 예제에서는 사용자가 확장 단추를 클릭할 때 탐색 창이 튕기는, 진동 범위가 동작으로 애니메이션 효과를 주는 탐색 및 캔버스 UI 환경을 만듭니다.

![클릭에 대 한 스프링 애니메이션](images/animation/spring-animation-on-click.gif)

탐색 창이 나타날 때 클릭 한 이벤트 내에서 스프링 애니메이션을 정의 하 여 시작 합니다. 그런 다음 InitialValueExpression 기능을 사용 하 여 값을 정의 하는 식을 사용 하 여 애니메이션의 속성을 정의 합니다. 또한 창이 열려 있는지 여부를 추적 하 고 준비가 되 면 애니메이션을 시작 합니다.

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
 _springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue + 250";
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue - 250";
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

이제이 동작을 입력에 연결 하려면 어떻게 해야 하나요? 따라서 최종 사용자가 swipes 하는 경우에는 스프링 동작으로 창이 나옵니다. 더 중요 한 것은 사용자가 더 swipes 하는 경우 동작이 최종 사용자의 속도에 따라 조정 되는 것입니다.

![살짝 밀기의 스프링 애니메이션](images/animation/spring-animation-on-swipe.gif)

이렇게 하려면 동일한 스프링 애니메이션을 사용 하 고 InteractionTracker를 사용 하 여 InertiaModifier에 전달할 수 있습니다. InputAnimations InteractionTracker에 대 한 자세한 내용은 [InteractionTracker를 사용 하 여 사용자 지정 조작 환경](interaction-tracker-manipulations.md)을 참조 하세요. 이 코드 예제에서는 InteractionTracker 및 VisualInteractionSource를 이미 설정 했다고 가정 합니다. NaturalMotionAnimation (이 경우에는 스프링)에서 InertiaModifiers를 만드는 데 중점을 둡니다.

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

이제 UI에 프로그래밍 방식 및 입력 기반 스프링 애니메이션이 모두 있습니다.

요약 하자면, 앱에서 스프링 애니메이션을 사용 하는 단계는 다음과 같습니다.

1. Compositor SpringAnimation를 만드세요.
1. 기본값이 아닌 값을 원하는 경우 SpringAnimation의 속성을 정의 합니다.
    - DampingRatio
    - 기간
    - 최종 값
    - 초기 값
    - 초기 개발 속도
1. 대상에 할당 합니다.
    - CompositionObject 속성에 애니메이션을 적용 하는 경우 StartAnimation에 SpringAnimation를 매개 변수로 전달 합니다.
    - With input을 사용 하려면 InertiaModifier의 NaturalMotion 속성을 SpringAnimation로 설정 합니다.

