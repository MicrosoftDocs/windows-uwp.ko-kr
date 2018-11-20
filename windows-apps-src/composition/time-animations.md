---
author: jwmsft
title: 시간 애니메이션
description: KeyFrame Animation 클래스를 사용하여 시간이 지남에 따라 UI를 변경합니다.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: bf6d3f16c7b240ca370c01a787fef09862f35863
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7301986"
---
# <a name="time-based-animations"></a>시간 기반 애니메이션

구성 요소가 도입되거나 전체 사용자 경험이 변경되면 최종 사용자는 시간을 두고 적응하거나 즉시 이를 받아들입니다. Windows 플랫폼에서 전자가 기본 후자 보다-사용자 경험이 급작스럽게 변경 되는 혼동 하 고 느끼고 수 없기 때문에 최종 사용자에 게 불편 합니다. 그러면 최종 사용자들은 사용 환경이 불안하고 부자연스럽다고 인식합니다.

대신, 시간을 두고 UI를 변경하여 최종 사용자에게 안내를 제공하거나 경험 변경 내용을 알릴 수 있습니다. Windows 플랫폼에서는 KeyFrameAnimations라고도 하는 시간 기반 애니메이션을 사용하여 이 작업을 수행합니다. KeyFrameAnimations를 사용하면 시간을 두고 UI를 변경하고, 시작 방법 및 시작 시간, 종료 상태에 도달하는 방법을 비롯하여 애니메이션의 각 요소를 제어할 수 있습니다. 예를 들어 300밀리초 동안 새로운 위치에 애니메이션으로 개체를 표현하면 즉시 "이동"되도록 하는 것보다 즐거운 경험이 됩니다. 즉시 변경하는 대신 애니메이션을 사용하면 결과적으로 더 즐겁고 매력적인 경험을 창출할 수 있습니다.

## <a name="types-of-time-based-animations"></a>시간 기반 애니메이션 유형

Windows에서 멋진 사용자 환경을 구축하는 데 사용할 수 있는 시간 기반 애니메이션에는 두 가지 범주가 있습니다.

**명시적 애니메이션** – 이름에서 알 수 있듯이 명시적으로 애니메이션을 시작하여 업데이트합니다.
**암시적 애니메이션** – 이 애니메이션은 조건이 충족될 때 사용자를 대신하여 시스템에 의해 시작됩니다.

이 문서에서는 KeyFrameAnimations를 사용하여 _명시적_ 시간 기반 애니메이션을 만들고 활용하는 방법에 대해 설명합니다.

명시적이든 암시적이든, 시간 기반 애니메이션은 애니메이션을 적용할 수 있는 CompositionObjects의 속성에 따라 다양한 유형으로 분류할 수 있습니다.

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>KeyFrameAnimations로 시간 기반 애니메이션 만들기

KeyFrameAnimations로 명시적인 시간 기반 애니메이션을 만드는 방법을 설명하기 전에 몇 가지 개념을 살펴보겠습니다.

- KeyFrames - 애니메이션 효과를 내는 데 사용되는 개별 "스냅샷"입니다.
  - 키-값 쌍으로 정의됩니다. 키는 애니메이션 수명에서 이 "스냅샷"이 발생하는 위치인 0과 1 사이에서 얼마나 진행되었는지 나타냅니다. 다른 매개 변수는 이 시점의 속성 값을 나타냅니다.
- KeyFrameAnimation 속성 - UI 요구 사항을 충족시키기 위해 적용할 수 있는 사용자 지정 옵션입니다.
  - DelayTime – StartAnimation이 호출된 후 애니메이션을 시작하기 전의 시간입니다.
  - Duration – 애니메이션의 지속 시간.
  - IterationBehavior – 애니메이션의 수 또는 무한 반복 동작.
  - IterationCount - 키 프레임 애니메이션이 반복되는 유한 횟수.
  - KeyFrame Count - 특정 키 프레임 애니메이션에서 읽은 키 프레임 수.
  - StopBehavior – StopAnimation이 호출될 때 애니메이션 효과를 주려는 속성 값의 동작 지정.
  - Direction – 재생에 대한 애니메이션 방향을 지정합니다.
- Animation Group - 동시에 여러 애니메이션을 시작합니다.
  - 동시에 여러 속성에 애니메이션을 적용하려는 경우에 주로 사용됩니다.

자세한 내용은 [CompositionAnimationGroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup)을 참조하세요.

이러한 개념을 염두에 두고 KeyFrameAnimation을 구성하는 일반적인 공식을 살펴보겠습니다.

1. CompositionObject와 애니메이션을 적용해야 하는 해당 속성을 확인합니다.
1. 애니메이션을 적용하려는 속성 유형과 일치하는 작성자의 KeyFrameAnimation Type 템플릿을 만듭니다.
1. 애니메이션 템플릿을 사용하여 KeyFrames를 추가하고 애니메이션의 속성을 정의하기 시작합니다.
    - 하나 이상의 KeyFrame이 필요합니다(100% 또는 1f 키 프레임).
    - 기간도 정의하는 것이 좋습니다.
1. 이 애니메이션을 실행할 준비가 되면 애니메이션을 적용할 속성을 대상으로 CompositionObject에서 StartAnimation(...)을 호출합니다. 특히 다음 사항에 주의하세요.
    - `Visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `Visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. 실행 중인 애니메이션이 있는 경우, 다음 API를 사용 애니메이션 또는 애니메이션 그룹을 중지할 수 있습니다.
    - `Visual.StopAnimation("targetProperty");`
    - `Visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

이 공식을 실제로 사용하는 예를 살펴보겠습니다.

## <a name="example"></a>예

이 예에서 <0,0,0>에서 <20,20,20>까지 시각 요소 오프셋에 1초 이상 애니메이션 효과를 주고자 합니다. 또한 이러한 위치 사이에서 시각적 애니메이션을 10회 보고 싶습니다.

![키 프레임 애니메이션](images/animation/animated-rectangle.gif)

먼저 애니메이션을 적용할 CompositionObject 및 속성을 식별하는 것으로 시작합니다. 이 경우 빨간색 사각형은 `redSquare`라는 컴퍼지션 시각으로 표시됩니다. 이 개체에서 애니메이션을 시작합니다.

그런 다음 오프셋 속성에 애니메이션을 적용하려고 하므로 Vector3KeyFrameAnimation(오프셋은 Vector3 유형)을 만들어야 합니다. 또한 KeyFrameAnimation에 해당하는 KeyFrames를 정의합니다.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

그런 다음 KeyFrameAnimation의 속성을 정의하여, 두 위치(현재 위치와 <200,0,0>) 사이에서 애니메이션을 10회 반복하는 동작과 함께 재생 시간을 설명합니다.

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

마지막으로, 애니메이션을 실행하려면 CompositionObject의 속성을 기반으로 시작해야 합니다.

```csharp
redVisual.StartAnimation("Offset.X", animation);
```

전체 코드는 다음과 같습니다.

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redSquare)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    visual.StartAnimation("Offset.X", animation);
} 
```
