---
Description: 흐름 동작에서 타이밍 및 감속/가속 함수를 사용 하는 방법에 대해 알아봅니다.
title: 타이밍 및 감속
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 098a75da573a977aa393197a61a62b0337f0dc06
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970508"
---
# <a name="timing-and-easing"></a>타이밍 및 감속

동작은 실제 세계를 기준으로 하지만, 속도와 성능의 기대와 함께 제공 되는 디지털 매체 이기도 합니다.

## <a name="examples"></a>예

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 되어 있는 경우 여기를 클릭 하 여 <a href="xamlcontrolsgallery:/item/EasingFunction">앱을 열고 동작의 감속/가속 함수를 참조</a>하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>흐름 동작에서 시간을 사용 하는 방법

타이밍은 UI 내에서 입력, 종료 또는 이동 하는 개체에 대 한 동작을 자연스럽 게 만드는 중요 한 요소입니다.

1. 보기를 시작 하는 개체 또는 장면을 빠르게 탄생 수 있습니다. 이러한 애니메이션은 일반적으로 장면에서 계층적 빌드를 허용 하기 위해 종료 하는 기간 보다 더 깁니다.
1. 뷰를 종료 하는 개체 또는 장면이 매우 빠릅니다. 사용자는 UI가 발생 한 위치를 이해할 수 있어야 합니다. 그러나 UI를 해제 한 후에는이를 해제 해야 합니다.
1. 장면에서 변환 하는 개체는 이동 하는 거리에 적절 한 기간을 가져야 합니다.

## <a name="timing-in-fluent-motion"></a>흐름 동작 타이밍

흐름에서의 동작 타이밍은 사용자가 인스턴트로 인식 하는 최대 시간 이기 때문에 500ms (또는 1-2 초)를 기준으로 사용 합니다.

![영웅 이미지](images/time.gif)

### <a name="150ms-exit"></a>**150ms** (종료)

:::row:::
    :::column:::
장면 또는 닫기를 종료 하는 개체 또는 페이지에 사용 합니다.
에서 부드러운 애니메이션 효과를 얻기 위해 프레임 속도에 방해가 되지 않는 종료 UI에 대 한 빠른 방향 피드백을 허용 합니다.
    :::column-end:::
    :::column:::
        ![150ms 동작](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300ms** (Enter)

:::row:::
    :::column:::
장면에 입력 하거나 열 때 사용 하는 개체 또는 페이지에 사용 합니다.
장면에 들어갈 때 적절 한 시간 동안 콘텐츠를 축 수 있습니다.
    :::column-end:::
    :::column:::
        ![300ms 동작](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤ 500ms** (이동)

:::row:::
    :::column:::
단일 장면 또는 여러 장면에서 변환 되는 개체에 사용 합니다. 
    :::column-end:::
    :::column:::
        ![500ms 동작](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>흐름 동작의 감속

감속/가속은 개체가 이동 하는 속도를 조작 하는 방법입니다. 모든 흐름 동작 환경을 연결 하는 붙이기가 있습니다. 극단적인 경우 시스템에서 사용 하는 감속은 시스템 전체에서 이동 하는 개체의 실제 느낌을 통합 하는 데 도움이 됩니다. 이 방법은 실제 환경에 포함 된 것 처럼 실제 세계를 모방 하 고 개체를 동작 느낌으로 만드는 한 가지 방법입니다.

![영웅 이미지](images/easing.gif)

## <a name="apply-easing-to-motion"></a>동작에 감속/가속 적용

이러한 추가 정보는 보다 자연 스러운 느낌을 얻기 위해 흐름 동작에 사용 하는 기준선입니다.

코드 예제에서는 Storyboard 애니메이션 (XAML) 또는 컴퍼지션 애니메이션 (c #)에 권장 되는 감속/가속 값을 적용 하는 방법을 보여 줍니다.

### <a name="accelerate-exit"></a>**가속** (종료)

:::row:::
    :::column:::
장면을 종료 하는 UI 또는 개체에 사용 합니다.

개체가 구동 되 고 이탈 속도에 도달할 때까지 모멘텀 됩니다.
결과적으로 개체는 사용자의 작업을 쉽게 수행 하 고 새 콘텐츠를 가져올 수 있는 공간을 확보 하는 것입니다.
    :::column-end:::
    :::column:::
        ![가속 가속화](images/accelEase.gif)
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

### <a name="decelerate-enter"></a>**감속** (Enter)

:::row:::
    :::column:::
장면에 입력 하거나 탐색 하거나 생성 하는 UI에 대해를 사용 합니다.

한 번 장면에서 개체가 만족 되 면 개체가 나머지 상태로 느려지게 됩니다.
결과적으로 개체는 긴 거리에서 멀리 떨어진 곳에서 이동 하거나 극단적인 속도로 이동 하거나, rest 상태로 빠르게 돌아갑니다.

응답 하지 않는 순간에 발생 하는 경우에도 들어오는 개체의 속도는 속도가 빠르며 응답성이 뛰어난 효과를 가집니다.
    :::column-end:::
    :::column:::
        ![감속 감속](images/decelEase.gif)
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

### <a name="standard-easing-move"></a>**표준 감속/가속** (이동)

:::row:::
    :::column:::
이는 시스템 내에서 애니메이션 된 매개 변수의 변경에 대 한 기준선 감속입니다.
간단한 위치 변경과 같이 상태에서 화면에 상태로 변경 되는 개체에 대해 표준 감속/가속을 사용 합니다. 또한 증가 하는 개체 처럼 장면의 모핑 개체에도 사용 합니다.

결과적으로 A에서 B로 상태를 변경 하는 개체는 극복 하 고 자연 스러운 힘을 통해 수행 됩니다.
    :::column-end:::
    :::column:::
        ![표준 감속/가속](images/standardEase.gif)
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

## <a name="related-articles"></a>관련된 문서

- [동작 개요](index.md)
- [방향 및 무게](directionality-and-gravity.md)
