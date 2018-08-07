---
author: jwmsft
Description: Learn how Fluent motion uses timing and easing functions.
title: 타이밍 및 감속 - UWP 앱의 애니메이션
label: Timing and easing
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 412ba7e36c2bb36562ceee13bb1e204ff402a882
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843895"
---
# <a name="timing-and-easing"></a>타이밍 및 감속

움직임은 실제 세계를 기반으로 하지만 속도와 성능에 대한 기대와 함께 제공되는 디지털 미디어이기도 합니다. 

## <a name="how-fluent-motion-uses-time"></a>Fluent 움직임이 시간을 사용하는 방법

타이밍은 UI 내에서 개체가 출입하거나 이동하는 움직임이 자연스럽게 느껴지도록 만드는 중요한 요소입니다.

1. 보기에 들어오는 개체 또는 장면은 빠르지만 어느 정도 시간이 걸립니다. 장면의 계층적 빌드를 위해 이러한 애니메이션은 일반적으로 나가는 동작보다 깁니다.
1. 보기를 나가는 개체 또는 장면은 매우 빠릅니다. 사용자는 UI가 어디로 이동했는지 파악할 수 있어야 합니다. 하지만 UI를 해제한 후 시야에서 벗어나야 합니다.
1. 장면을 거슬러 변환하는 개체는 이동하는 거리에 적절한 기간이 있어야 합니다.

## <a name="timing-in-fluent-motion"></a>Fluent 움직임의 타이밍

Fluent의 움직임 타이밍은 사용자가 순간으로 인식하는 최대 시간인 500ms(또는 이분의 일초)를 기준선으로 사용합니다.

![영웅 이미지](images/time.gif)

### <a name="150ms-exit"></a>**150ms**(나감)

:::row::: :::column::: 기존 장면을 나가거나 종료하는 개체 또는 페이지에 대한 사용.
원활한 애니메이션을 위해 타이밍이 프레임 속도에 영향을 주지 않는 기존 UI의 매우 빠른 방향에 대한 피드백을 허용합니다.
:::column-end::: :::column::: ![150ms 동작](images/150msAlt.gif) :::column-end::: :::row-end:::

### <a name="300ms-enter"></a>**300ms**(들어옴)

:::row::: :::column::: 기존 장면으로 들어가거나 이를 여는 개체 또는 페이지에 대한 사용.
적절한 시간 동안 장면에 들어오는 콘텐츠를 볼 수 있도록 합니다.
:::column-end::: :::column::: ![300ms 동작](images/300ms.gif) :::column-end::: :::row-end:::

### <a name="500ms-move"></a>**≤500ms**(이동)

:::row::: :::column::: 단일 장면 또는 여러 개의 장면 간에 변환하는 개체를 위한 사용. :::column-end::: :::column::: ![500ms 동작](images/500ms.gif) :::column-end::: :::row-end:::

## <a name="easing-in-fluent-motion"></a>Fluent 움직임의 감속

감속은 개체가 이동할 때 개체의 속도를 조작하는 방법입니다. 모든 Fluent 움직임 환경을 연결합니다. 최고 속도일 때 시스템에서 사용되는 감속을 통해 시스템 전체에서 개체의 실제 느낌을 통합할 수 있습니다. 실제 세계를 흉내내는 하나의 방법이며 움직이는 개체가 그 환경에 속한 것 같은 느낌을 줍니다.

![영웅 이미지](images/easing.gif)

## <a name="apply-easing-to-motion"></a>움직임에 감속 적용

이러한 감속을 통해 더욱 자연스러운 느낌을 줄 수 있으며 Fluent 움직임에 사용하는 기준선입니다.

코드 예제는 권장된 감속 값을 스토리보드 애니메이션(XAML) 또는 구성 애니메이션(C#)에 적용하는 방법을 보여 줍니다.

### <a name="accelerate-exit"></a>**가속**(종료)

:::row::: :::column::: 기존 장면을 종료하는 UI 또는 개체에 대한 사용.

        Objects become powered and gain momentum until they reach escape velocity.
        The resulting feel is that the object is trying its hardest to get out of the user's way and make room for new content to come in.
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**감속**(들어옴)

:::row::: :::column::: 기존 장면으로 들어가거나 이동 또는 뻗어나가는 UI 또는 개체에 대한 사용.

        Once on-scene, the object is met with extreme friction, which slows the object to rest.
        The resulting feel is that the object traveled from a long distance away and entered at an extreme velocity, or is quickly returning to a rest state.

        Even if it's preceded by a moment of unresponsiveness, the velocity of the incoming object has the effect of feeling fast and responsive.
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**표준 감속**(이동)

:::row::: :::column::: 시스템 내 애니메이션이 적용된 매개 변수 변경을 완화하기 위한 기준.
간단한 위치 변경 등 화면의 상태별로 변하는 개체에 대한 표준 감속을 사용합니다. 또한, 증가하는 개체와 같이 화면 내에 모핑하는 개체에 사용합니다.

        The resulting feel is that objects changing state from A to B are overcoming, and taken over by, natural forces.
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>관련 문서

- [움직임 개요](index.md)
- [방향 및 무게](directionality-and-gravity.md)