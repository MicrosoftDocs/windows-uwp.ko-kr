---
title: 시간 애니메이션
description: Key프레임 애니메이션 클래스를 사용 하 여 UI 변경을 통해 사용자를 안내 하는 시간 기반 애니메이션을 만드는 방법을 알아봅니다.
ms.date: 12/12/2018
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 8846fc11dc39a3931d8f3278caf13b7aff464bc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160837"
---
# <a name="time-based-animations"></a>시간 기반 애니메이션

또는 전체 사용자 환경에서 구성 요소가 변경 되 면 최종 사용자가 시간이 지남에 따라 또는 즉각적으로 두 가지 방법으로이를 관찰 하는 경우가 많습니다. Windows 플랫폼에서 이전은 발생 한 작업을 수행할 수 없기 때문에 종종 혼동을 주는 최종 사용자 환경을 즉시 변경 하는 후자 사용자 환경 보다 우선적으로 적용 됩니다. 그러면 최종 사용자가 jarring 및 비 자연으로 환경을 인식 합니다.

대신 시간에 따라 UI를 변경 하 여 최종 사용자에 게 안내 하거나 경험에 대 한 변경 내용을 알릴 수 있습니다. Windows 플랫폼에서이 작업은 Key프레임 애니메이션이 라고도 하는 시간 기반 애니메이션을 사용 하 여 수행 됩니다. Key프레임 애니메이션을 사용 하면 시간에 따른 UI를 변경 하 고, 시작 방법 및 시기 및 애니메이션의 종료 상태에 대 한 제어를 포함 하 여 애니메이션의 각 측면을 제어할 수 있습니다. 예를 들어 개체를 새 위치에서 300 밀리초 보다 더 빠르게 적용 하는 것이 해당 위치에서 "teleporting" 하는 것 보다 더 좋습니다. 애니메이션을 즉시 변경 하는 대신 사용 하는 경우에는 더 편리 하 고 매력적인 환경을 제공 합니다.

## <a name="types-of-time-based-animations"></a>시간 기반 애니메이션의 형식

Windows에서 멋진 사용자 환경을 빌드하는 데 사용할 수 있는 시간 기반 애니메이션에는 두 가지 범주가 있습니다.

**명시적 애니메이션** – 이름이 나타내는 것 처럼 애니메이션을 명시적으로 시작 하 여 업데이트를 수행 합니다.
**암시적 애니메이션** – 조건이 충족 될 때 사용자를 대신 하 여 시스템에서이 애니메이션을 시작 합니다.

이 문서에서는 Key프레임 애니메이션을 사용 하 여 _명시적_ 시간 기반 애니메이션을 만들고 사용 하는 방법을 설명 합니다.

명시적 및 암시적 시간 기반 애니메이션 모두에는 애니메이션 효과를 적용할 수 있는 CompositionObjects의 다양 한 속성 유형에 해당 하는 다양 한 형식이 있습니다.

- Colorkey프레임 애니메이션
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>Key프레임 애니메이션을 사용 하 여 시간 기반 애니메이션 만들기

Key프레임 애니메이션을 사용 하 여 명시적 시간 기반 애니메이션을 만드는 방법을 설명 하기 전에 몇 가지 개념을 살펴보겠습니다.

- 키 프레임 – 애니메이션이 애니메이션 효과를 주는 개별 "스냅숏"입니다.
  - 키 & 값 쌍으로 정의 됩니다. 키는 0과 1 사이의 진행률을 나타내며,이 "snapshot"은 애니메이션 수명의 즉, 위치를 나타냅니다. 이 때 다른 매개 변수는 속성 값을 나타냅니다.
- Key프레임 애니메이션 속성 – UI의 요구 사항을 충족 하기 위해 적용할 수 있는 사용자 지정 옵션입니다.
  - DelayTime – StartAnimation이 호출 된 후 애니메이션이 시작 되기 전의 시간입니다.
  - 기간 – 애니메이션의 지속 시간입니다.
  - IterationBehavior – 애니메이션에 대 한 카운트 또는 무한 반복 동작입니다.
  - IterationCount – 키 프레임 애니메이션이 반복 되는 유한 시간 수입니다.
  - 키 프레임 수 – 특정 키 프레임 애니메이션에서의 키 프레임 수를 읽습니다.
  - StopBehavior – Stopbehavior이 호출 될 때 애니메이션 속성 값의 동작을 지정 합니다.
  - Direction – 재생할 애니메이션의 방향을 지정 합니다.
- 애니메이션 그룹 – 여러 애니메이션을 동시에 시작 합니다.
  - 동시에 여러 속성에 애니메이션 효과를 주기 위해 사용 되는 경우가 많습니다.

자세한 내용은 [CompositionAnimationGroup](/uwp/api/windows.ui.composition.compositionanimationgroup)를 참조 하세요.

이러한 개념을 염두에 두면 Key프레임 애니메이션을 생성 하는 일반적인 수식을 살펴보겠습니다.

1. 애니메이션을 적용 해야 하는 CompositionObject 및 해당 속성을 식별 합니다.
1. 애니메이션을 적용할 속성의 형식과 일치 하는 compositor의 Key프레임 애니메이션 형식 템플릿을 만듭니다.
1. 애니메이션 템플릿을 사용 하 여 키 프레임을 추가 하 고 애니메이션의 속성을 정의 합니다.
    - 하나 이상의 키프레임이 필요 합니다 (100% 또는 1f 키 프레임).
    - 또한 기간을 정의 하는 것이 좋습니다.
1. 이 애니메이션을 실행할 준비가 되 면 애니메이션을 적용할 속성을 대상으로 하는 CompositionObject에서 StartAnimation (...)을 호출 합니다. 특히 다음에 대한 내용을 설명합니다.
    - `visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. 애니메이션을 실행 하 고 애니메이션 또는 애니메이션 그룹을 중지 하려는 경우 다음 Api를 사용할 수 있습니다.
    - `visual.StopAnimation("targetProperty");`
    - `visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

작업에서이 수식을 확인 하는 예를 살펴보겠습니다.

## <a name="example"></a>예제

이 예제에서는 <0, 0, 0>를 <200, 0, 0> 1 초 이상으로 표시 하는 것이 좋습니다. 또한 이러한 위치 간의 시각적 효과를 10 번 확인 하려고 합니다.

![키 프레임 애니메이션](images/animation/animated-rectangle.gif)

먼저 애니메이션을 적용할 CompositionObject 및 속성을 식별 합니다. 이 경우 빨간색 사각형은 라는 컴퍼지션 시각적 개체로 표시 됩니다 `redVisual` . 이 개체에서 애니메이션을 시작 합니다.

그런 다음 Offset 속성에 애니메이션 효과를 주기 때문에 Vector3KeyFrameAnimation (Offset은 Vector3 형식)를 만들어야 합니다. 또한 Key프레임 애니메이션에 대 한 해당 키 프레임을 정의 합니다.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

그런 다음 두 위치 (현재 및 <200, 0>) 사이에 애니메이션 효과를 주는 동작을 설명 하는 Key프레임 애니메이션의 속성을 정의 합니다.

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

마지막으로 애니메이션을 실행 하려면 CompositionObject의 속성에서 애니메이션을 시작 해야 합니다.

```csharp
redVisual.StartAnimation("Offset", animation);
```

전체 코드는 다음과 같습니다.

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redVisual)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    redVisual.StartAnimation("Offset", animation);
} 
```