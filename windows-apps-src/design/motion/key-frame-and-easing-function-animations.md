---
title: 키 프레임 애니메이션 및 감속/가속 함수 애니메이션
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: 선형 키 프레임 애니메이션, KeySpline 값을 사용 하는 키 프레임 애니메이션 또는 감속/가속 함수는 거의 동일한 시나리오에 사용 되는 세 가지 방법입니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e68c667fe1e6d3ef61e60095e7fed02400044d07
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172427"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>키 프레임 애니메이션 및 감속/가속 함수 애니메이션



선형 키 프레임 애니메이션, **KeySpline** 값을 사용 하는 키 프레임 애니메이션 또는 감속/가속 함수는 거의 동일한 시나리오에 사용할 수 있는 세 가지 방법입니다. 즉, 좀 더 복잡 한 storyboarded 애니메이션을 만들고 시작 상태에서 끝 상태로 비선형 애니메이션 동작을 사용 합니다.

## <a name="prerequisites"></a>필수 구성 요소

[Storyboarded 애니메이션](storyboarded-animations.md) 항목을 확인 해야 합니다. 이 항목은 [Storyboarded 애니메이션](storyboarded-animations.md) 에 설명 된 애니메이션 개념을 기반으로 하며 다시 이동 하지 않습니다. 예를 들어 [Storyboarded 애니메이션](storyboarded-animations.md) 은 애니메이션, storyboard를 리소스로, [**타임 라인**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) 속성 값 (예: [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration), [**fillbehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior)등)을 대상으로 하는 방법을 설명 합니다.

## <a name="animating-using-key-frame-animations"></a>키 프레임 애니메이션을 사용 하 여 애니메이션 효과 주기

키 프레임 애니메이션은 애니메이션 타임 라인을 따라 지점에 도달 하는 대상 값을 둘 이상 허용 합니다. 즉, 각 키 프레임은 다른 중간 값을 지정할 수 있으며 마지막 키 프레임은 최종 애니메이션 값입니다. 애니메이션 효과를 주는 여러 값을 지정 하 여 보다 복잡 한 애니메이션을 만들 수 있습니다. 키 프레임 애니메이션은 각각 애니메이션 유형별 다른 **KeyFrame** 하위 클래스로 구현 되는 다른 보간 논리도 사용할 수 있습니다. 특히 각 키 프레임 애니메이션 유형에는 키 프레임을 지정 하기 위한 **불연속**, **선형**, **스플라인** 및 **감속/가속** 변형이 있습니다. **KeyFrame** 예를 들어 [**Double**](/dotnet/api/system.double) 을 대상으로 하 고 키 프레임을 사용 하는 애니메이션을 지정 하려면 [**DiscreteDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteDoubleKeyFrame), [**LinearDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.LinearDoubleKeyFrame), [**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame)및 [**EasingDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.EasingDoubleKeyFrame)를 사용 하 여 키 프레임을 선언할 수 있습니다. 단일 **KeyFrames** 컬렉션 내에서 이러한 형식의 일부 및 전체를 사용하여 새 키 프레임에 도달할 때마다 보간을 변경할 수 있습니다.

보간 동작의 경우 각 키 프레임은 **KeyTime** 시간에 도달할 때까지 보간을 제어 합니다. 해당 **값** 은 해당 시간에도 도달 합니다. 보다 많은 키 프레임이 있는 경우 값은 시퀀스의 다음 키 프레임에 대 한 시작 값이 됩니다.

애니메이션의 시작 부분에서 **KeyTime** 이 "0:0:0" 인 키 프레임이 없는 경우 시작 값은 속성의 애니메이션이 적용 되지 않은 값입니다. 이는에서가 **From** / **To** / **By** 없는 경우 **From**의에서로의를 실행 하는 방법과 비슷합니다.

키 프레임 애니메이션의 지속 시간은 암시적으로 해당 키 프레임에 설정 된 가장 큰 **KeyTime** 값과 동일 합니다. 원하는 경우 명시적 [**기간**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) 을 설정할 수 있지만 사용자 고유의 키 프레임에서 **KeyTime** 보다 짧은 것은 아니지만 사용자가 애니메이션의 일부를 잘라 냅니다.

[**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) **From** [**Timeline**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) / **To** / **By** 키 프레임 애니메이션 클래스도 **타임 라인**에서 파생 되기 때문에, 기간 외에도에서로의를 사용 하는 것과 같이 키 프레임 애니메이션에 대 한 모든 타임 라인 기반 속성을 설정할 수 있습니다. 해당 경고는 다음과 같습니다.

-   [**System.windows.media.animation.timeline.autoreverse**](/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse): 마지막 키 프레임에 도달 하면 프레임은 끝부터 반대 순서로 반복 됩니다. 이렇게 하면 애니메이션의 명백한 기간이 두 배가 됩니다.
-   [**Begintime**](/uwp/api/windows.ui.xaml.media.animation.timeline.begintime): 애니메이션의 시작을 지연 시킵니다. 프레임의 **KeyTime** 값에 대 한 타임 라인은 **begintime** 에 도달할 때까지 계산을 시작 하지 않으므로 프레임을 잘라낼 위험이 없습니다.
-   [**Fillbehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior): 마지막 키 프레임에 도달할 때 발생 하는 작업을 제어 합니다. **Fillbehavior** 는 중간 키 프레임에는 영향을 주지 않습니다.
-   [**RepeatBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty):
    -   이를 **영구적**으로 설정 하면 키 프레임 및 해당 타임 라인이 무한히 반복 됩니다.
    -   반복 횟수로 설정 된 경우 타임 라인은 여러 번 반복 됩니다.
    -   [**기간**](/uwp/api/Windows.UI.Xaml.Duration)으로 설정 된 경우 타임 라인은 해당 시간에 도달할 때까지 반복 됩니다. 이는 타임 라인의 암시적 지속 시간에 대 한 정수 요소가 아닌 경우 키 프레임 시퀀스를 통해 애니메이션 부분을 자를 수 있습니다.
-   [**System.windows.media.animation.timeline.speedratio**](/uwp/api/windows.ui.xaml.media.animation.timeline.speedratioproperty) (일반적으로 사용 되지 않음)

### <a name="linear-key-frames"></a>선형 키 프레임

선형 키 프레임은 프레임의 **KeyTime** 에 도달할 때까지 값의 간단한 선형 보간을 발생 합니다. 이 보간 동작은 **From** / **To** / **By** [Storyboarded 애니메이션](storyboarded-animations.md) 항목에서 설명 하는 것 보다 더 간단한와 가장 비슷합니다.

키 프레임 애니메이션을 사용 하 여 선형 키 프레임을 사용 하 여 사각형의 렌더링 높이를 조정 하는 방법은 다음과 같습니다. 이 예제에서는 사각형의 높이가 처음 4 초 동안 약간 및 선형으로 확장 된 다음 사각형의 시작 높이가 두 배가 될 때까지 마지막 1 초 동안 빠르게 크기를 조정 하는 애니메이션을 실행 합니다.

```xml
<StackPanel>
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames
              Storyboard.TargetName="myRectangle"
              Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <LinearDoubleKeyFrame Value="1" KeyTime="0:0:0"/>
                <LinearDoubleKeyFrame Value="1.2" KeyTime="0:0:4"/>
                <LinearDoubleKeyFrame Value="2" KeyTime="0:0:5"/>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
    </StackPanel.Resources>
</StackPanel>
```

### <a name="discrete-key-frames"></a>불연속 키 프레임

불연속 키 프레임은 모든 보간을 사용 하지 않습니다. **KeyTime** 에 도달 하면 새 **값** 만 적용 됩니다. 애니메이션을 적용 하는 UI 속성에 따라이는 종종 "점프"에 표시 되는 애니메이션을 생성 합니다. 이것은 사용자가 원하는 미적 동작 이어야 합니다. 선언 하는 키 프레임의 수를 늘려서 명백한 점프를 최소화할 수 있지만 부드러운 애니메이션이 목표 이면 대신 선형 또는 스플라인 키 프레임을 사용 하는 것이 더 좋을 수 있습니다.

> [!NOTE]
> 불연속 키 프레임은 [**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point)및 [**Color**](/uwp/api/Windows.UI.Color)형식이 아닌 값에 [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame)를 적용 하는 유일한 방법입니다. 이에 대해서는이 항목의 뒷부분에서 자세히 설명 합니다.

### <a name="spline-key-frames"></a>스플라인 키 프레임

스플라인 키 프레임은 **KeySpline** 속성의 값에 따라 값 사이에 변수 전환을 만듭니다. 이 속성은 애니메이션의 가속을 설명 하는 베 지 어 곡선의 첫 번째 및 두 번째 제어점을 지정 합니다. 기본적으로 [**KeySpline**](/uwp/api/Windows.UI.Xaml.Media.Animation.KeySpline) 은 함수 시간 그래프가 해당 베 지 어 곡선의 모양 인 함수-시간 관계를 정의 합니다. 일반적으로 공백 또는 쉼표로 구분 된 네 개의 [**Double**](/dotnet/api/system.double) 값을 포함 하는 XAML 줄임 특성 문자열에 **KeySpline** 값을 지정 합니다. 이러한 값은 베 지 어 곡선의 두 제어점에 대 한 "X, Y" 쌍입니다. "X"는 시간이 고 "Y"는 값에 대 한 함수 한정자입니다. 각 값은 항상 0에서 1 사이 여야 합니다. **KeySpline**에 대 한 제어 지점 수정 없이 0에서 1 사이의 직선은 선형 보간을 위한 시간이 지남에 따라 함수를 표현한 것입니다. 제어점은 해당 곡선의 모양을 변경 하므로 스플라인 애니메이션에 대 한 시간에 따른 함수의 동작입니다. 시각적 개체를 그래프로 표시 하는 것이 좋습니다. 브라우저에서 [Silverlight 키-스플라인 시각화 도우미 샘플](https://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample) 을 실행 하 여 제어점이 곡선을 수정 하는 방법 및 샘플 애니메이션을 **KeySpline** 값으로 사용할 때 실행 하는 방법을 확인할 수 있습니다.

다음 예제에서는 애니메이션에 적용 되는 세 개의 다른 키 프레임을 보여 줍니다. 마지막 하나는 [**Double**](/dotnet/api/system.double) 값 ([**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame))에 대 한 키 스플라인 애니메이션입니다. **KeySpline**에 적용 되는 문자열 "0.6, 0.0 0.9, 0.00"을 확인 합니다. 이렇게 하면 애니메이션이 처음에 느리게 실행 되는 것 처럼 보이는 곡선이 생성 된 다음 **KeyTime** 에 도달 하기 바로 전에 값에 빠르게 도달 합니다.

```xml
<Storyboard x:Name="myStoryboard">
    <!-- Animate the TranslateTransform's X property
        from 0 to 350, then 50,
        then 200 over 10 seconds. -->
    <DoubleAnimationUsingKeyFrames
        Storyboard.TargetName="MyAnimatedTranslateTransform"
        Storyboard.TargetProperty="X"
        Duration="0:0:10" EnableDependentAnimation="True">

        <!-- Using a LinearDoubleKeyFrame, the rectangle moves 
            steadily from its starting position to 500 over 
            the first 3 seconds.  -->
        <LinearDoubleKeyFrame Value="500" KeyTime="0:0:3"/>

        <!-- Using a DiscreteDoubleKeyFrame, the rectangle suddenly 
            appears at 400 after the fourth second of the animation. -->
        <DiscreteDoubleKeyFrame Value="400" KeyTime="0:0:4"/>

        <!-- Using a SplineDoubleKeyFrame, the rectangle moves 
            back to its starting point. The
            animation starts out slowly at first and then speeds up. 
            This KeyFrame ends after the 6th second. -->
        <SplineDoubleKeyFrame KeySpline="0.6,0.0 0.9,0.00" Value="0" KeyTime="0:0:6"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

### <a name="easing-key-frames"></a>키 프레임 감속

감속/가속 키 프레임은 보간이 적용 되는 키 프레임으로, 보간 시간 경과에 따른 함수는 미리 정의 된 여러 수학 수식에 의해 제어 됩니다. 감속/가속 함수 형식 중 일부를 사용할 때와 마찬가지로 스플라인 키 프레임을 사용 하 여 훨씬 동일한 결과를 생성할 수 있지만, [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase)같은 감속/가속 함수를 사용 하 여 다시 생성할 수 없습니다.

감속/가속 함수를 감속/가속 키 프레임에 적용 하려면 XAML에서 해당 키 프레임에 대 한 속성 요소로 **기능** 속성을 설정 합니다. 값에 대해 감속/가속 함수 형식 중 하나에 대 한 개체 요소를 지정 합니다.

이 예제에서는 [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase) 및 [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) 를 연속 키 프레임으로 [**system.windows.media.animation.doubleanimation.to**](/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) 에 적용 하 여 바운스 효과를 만듭니다.

```xml
<Storyboard x:Name="myStoryboard">
    <DoubleAnimationUsingKeyFrames Duration="0:0:10"
        Storyboard.TargetProperty="Height"
        Storyboard.TargetName="myEllipse">

        <!-- This keyframe animates the ellipse up to the crest 
            where it slows down and stops. -->
        <EasingDoubleKeyFrame Value="-300" KeyTime="00:00:02">
            <EasingDoubleKeyFrame.EasingFunction>
                <CubicEase/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>

        <!-- This keyframe animates the ellipse back down and makes
            it bounce. -->
        <EasingDoubleKeyFrame Value="0" KeyTime="00:00:06">
            <EasingDoubleKeyFrame.EasingFunction>
                <BounceEase Bounces="5"/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

이는 단 하나의 감속/가속 함수 예제입니다. 다음 섹션에서 더 자세히 다루겠습니다.

## <a name="easing-functions"></a>감속/가속 함수

감속/가속 함수를 사용하여 사용자 지정 수식을 애니메이션에 적용할 수 있습니다. 수학적 연산은 종종 2 차원 좌표계에서 실제 물리를 시뮬레이션 하는 애니메이션을 생성 하는 데 유용 합니다. 예를 들어 마치 스프링 위에 있는 것처럼 개체가 사실적으로 바운스되거나 동작하도록 하고 싶을 수 있습니다. 키 **프레임을 사용** / **하** / **여 애니메이션으로** 이러한 효과를 발생 시킬 수 있지만 이러한 효과에는 상당한 양의 작업이 소요 되며 애니메이션은 수학적 수식을 사용 하는 것 보다 정확도가 떨어집니다.

감속/가속 함수는 다음과 같은 세 가지 방법으로 애니메이션에 적용할 수 있습니다.

-   이전 섹션에 설명 된 대로 키 프레임 애니메이션에서 감속/가속 키 프레임을 사용 합니다. EasingDoubleKeyFrame [**함수,**](/uwp/api/windows.ui.xaml.media.animation.easingcolorkeyframe.easingfunction) [**EasingDoubleKeyFrame.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction)또는 기타 기능을 사용 하 [**여 기능을**](/uwp/api/windows.ui.xaml.media.animation.easingpointkeyframe.easingfunction)사용 합니다.
-   **From**애니메이션 형식 중 하나에 대 한 **기능** 속성을 설정 합니다 / **To** / **By** . [**Coloranimation.**](/uwp/api/windows.ui.xaml.media.animation.coloranimation.easingfunction)system.windows.media.animation.doubleanimation.to ingfunction, [**DoubleAnimation.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction) 또는 [**pointanimation**](/uwp/api/windows.ui.xaml.media.animation.pointanimation.easingfunction)을 사용 합니다.
-   [**Generatedeasingfunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) 을 [**visualtransition**](/uwp/api/Windows.UI.Xaml.VisualTransition)의 일부로 설정 합니다. 컨트롤의 시각적 상태를 정의 하는 경우에만 적용 됩니다. 자세한 내용은 [**Generatedeasingfunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) 또는 [시각적 상태의 storyboard](/previous-versions/windows/apps/jj819808(v=win.10))를 참조 하세요.

다음은 감속/가속 함수 목록입니다.

-   [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase): 표시 된 경로에서 애니메이션 효과를 시작 하기 전에 애니메이션의 동작을 약간 취소 합니다.
-   [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase): 바운스 효과를 만듭니다.
-   [**CircleEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase): 순환 함수를 사용 하 여 가속 또는 감속 하는 애니메이션을 만듭니다.
-   [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase): f (t) = t3 수식을 사용 하 여 가속 또는 감속 하는 애니메이션을 만듭니다.
-   [**ElasticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ElasticEase): rest에 도달할 때까지 앞뒤로 진동 하는 스프링과 유사한 애니메이션을 만듭니다.
-   [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase): 지 수 수식을 사용 하 여 가속 또는 감속 하는 애니메이션을 만듭니다.
-   [**Powerease**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase): f (t) = tp 수식을 사용 하 여 가속 또는 감속 되는 애니메이션을 만듭니다. 여기서 p는 [**Power**](/uwp/api/windows.ui.xaml.media.animation.powerease.power) 속성과 같습니다.
-   [**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase): 수식 f (t) = t2를 사용 하 여 가속 또는 감속 하는 애니메이션을 만듭니다.
-   [**QuarticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuarticEase): f (t) = t4 수식을 사용 하 여 가속 또는 감속 하는 애니메이션을 만듭니다.
-   빠른 [**계산: f**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuinticEase)(t) = t5 수식을 사용 하 여 가속 또는 감속 하는 애니메이션을 만듭니다.
-   [**Sineease**](/uwp/api/Windows.UI.Xaml.Media.Animation.SineEase): 사인 수식을 사용 하 여 가속 또는 감속 하는 애니메이션을 만듭니다.

감속/가속 함수 중 일부에는 고유한 속성이 있습니다. 예를 들어 [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) 에는 해당 특정 **BounceEase**의 함수 오버 동작을 수정 하는 두 개의 속성 [**바운스**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounces) 와 [**Bounciness**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounciness) 가 있습니다. [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase) 와 같은 다른 감속/가속 함수에는 모든 감속/가속 함수에서 공유 하는 [**EasingMode**](/uwp/api/windows.ui.xaml.media.animation.easingfunctionbase.easingmode) 속성 이외의 속성이 없으며 항상 동일한 함수 오버 동작을 생성 합니다.

이러한 감속/가속 함수 중 일부는 속성을 포함 하는 감속/가속 함수에서 속성을 설정 하는 방법에 따라 약간 다릅니다. 예를 들어 [**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase) 는 2와 같은 [**전원을**](/uwp/api/windows.ui.xaml.media.animation.powerease.power) 사용 하는 [**powerease**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase) 와 정확히 동일 합니다. 및 [**CircleEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase) 는 기본적으로 기본 값인 [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase)입니다.

[**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase) 감속/가속 함수는에서로 설정 된 일반 범위 밖의 값을 **에서** / **To** 또는 키 프레임의 값으로 변경할 수 있으므로 고유 합니다. 이 메서드는 일반적인 **from**에서 behavior로 예상 되는 것과 반대 방향으로 값을 변경 하 여 애니메이션을 시작 하 / 고 시작 **From** 또는 시작 값으로 다시 이동한 다음 일반적인**방법** 으로 애니메이션을 실행 합니다.

이전 예제에서는 키 프레임 애니메이션에 대 한 감속/가속 함수를 선언 하는 방법을 살펴보았습니다. 다음 샘플에서는 감속/가속 함수를 **From에서** / 애니메이션**으로 적용** / **By** 합니다.

```xml
<StackPanel x:Name="LayoutRoot" Background="White">
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation From="30" To="200" Duration="00:00:3" 
                Storyboard.TargetName="myRectangle" 
                Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <DoubleAnimation.EasingFunction>
                    <BounceEase Bounces="2" EasingMode="EaseOut" 
                                Bounciness="2"/>
                </DoubleAnimation.EasingFunction>
            </DoubleAnimation>
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle x:Name="myRectangle" Fill="Blue" Width="200" Height="30"/>
</StackPanel>
```

감속/가속 함수를 **from에서**애니메이션으로 적용 하는 경우 / **To** / **By** 값이 애니메이션 [**기간**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) 동안 **from과에서** 값 사이를 보간 하는 방법에 대 한 함수 **To** -시간 특성을 변경 합니다. 감속/가속 함수를 사용 하지 않으면 선형 보간으로 사용 됩니다.

## <a name="span-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>불연속 개체 값 애니메이션

[**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point)또는 [**Color**](/uwp/api/Windows.UI.Color)형식이 아닌 속성에 애니메이션이 적용 된 값을 적용할 수 있는 유일한 방법은 애니메이션의 한 가지 유형입니다. 키 프레임 애니메이션 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)입니다. 프레임 간에 값을 보간 가능성이 없기 때문에 [**개체**](/dotnet/api/system.object) 값을 사용 하 여 애니메이션 효과를 적용 하는 것은 다릅니다. 프레임의 [**KeyTime**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.keytime) 에 도달 하면 애니메이션 된 값이 키 프레임의 **값**에 지정 된 값으로 즉시 설정 됩니다. 보간이 없으므로 **ObjectAnimationUsingKeyFrames** 키 프레임 컬렉션에서 사용 하는 키 프레임은 하나 뿐입니다. [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame).

자주 설정 하려는 개체 값을 특성 구문에서 **값** 을 채우는 문자열로 표현할 수 없기 때문에 [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) 의 [**값**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) 은 속성 요소 구문을 사용 하 여 설정 되는 경우가 많습니다. [StaticResource](../../xaml-platform/staticresource-markup-extension.md)와 같은 참조를 사용 하는 경우에도 특성 구문을 사용할 수 있습니다.

기본 템플릿에 사용 되는 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 는 템플릿 속성이 [**브러시**](/uwp/api/Windows.UI.Xaml.Media.Brush) 리소스를 참조 하는 경우에 나타납니다. 이러한 리소스는 [**색**](/uwp/api/Windows.UI.Color) 값 뿐만 아니라 시스템 테마 ([**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries))로 정의 된 리소스를 사용 하는 [**system.windows.media.solidcolorbrush>**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 개체입니다. [**TextBlock**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) 과 같은 **브러시**형식 값에 직접 할당 될 수 있으며 간접 대상 지정을 사용할 필요가 없습니다. 그러나 **system.windows.media.solidcolorbrush>** 는 [**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point)또는 **Color**가 아니기 때문에 **ObjectAnimationUsingKeyFrames** 를 사용 하 여 리소스를 사용 해야 합니다.

```xml
<Style x:Key="TextButtonStyle" TargetType="Button">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid Background="Transparent">
                    <TextBlock x:Name="Text"
                        Text="{TemplateBinding Content}"/>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal"/>
                            <VisualState x:Name="PointerOver">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPointerOverForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="Pressed">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPressedForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
...
                       </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

또한 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 를 사용 하 여 열거형 값을 사용 하는 속성에 애니메이션 효과를 적용할 수 있습니다. Windows 런타임 기본 템플릿에서 제공 하는 명명 된 스타일의 또 다른 예는 다음과 같습니다. [**표시**](/uwp/api/Windows.UI.Xaml.Visibility) 유형 열거형 상수를 사용 하는 [**표시 유형**](/uwp/api/windows.ui.xaml.uielement.visibility) 속성을 설정 하는 방법을 확인 합니다. 이 경우 특성 구문을 사용 하 여 값을 설정할 수 있습니다. 열거형 값 (예: "축소")을 사용 하 여 속성을 설정 하기 위해 열거형에서 정규화 되지 않은 상수 이름만 필요 합니다.

```xml
<Style x:Key="BackButtonStyle" TargetType="Button">
    <Setter Property="Template">
      <Setter.Value>
        <ControlTemplate TargetType="Button">
          <Grid x:Name="RootGrid">
            <VisualStateManager.VisualStateGroups>
              <VisualStateGroup x:Name="CommonStates">
              <VisualState x:Name="Normal"/>
...           <VisualState x:Name="Disabled">
                <Storyboard>
                  <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Visibility">
                    <DiscreteObjectKeyFrame Value="Collapsed" KeyTime="0"/>
                  </ObjectAnimationUsingKeyFrames>
                </Storyboard>
              </VisualState>
            </VisualStateGroup>
...
          </VisualStateManager.VisualStateGroups>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
  </Setter>
</Style>
```

[**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 프레임 집합에 둘 이상의 [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) 를 사용할 수 있습니다. 이는 여러 개체 값이 유용할 수 있는 예제 시나리오로 이미지의 값에 애니메이션 효과를 적용 하 여 "슬라이드 쇼" 애니메이션을 만드는 데 흥미로운 방법일 수 있습니다 [**.**](/uwp/api/windows.ui.xaml.controls.image.source)

 ## <a name="related-topics"></a>관련 항목

* [속성 경로 구문](../../xaml-platform/property-path-syntax.md)
* [종속성 속성 개요](../../xaml-platform/dependency-properties-overview.md)
* [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**System.windows.media.animation.storyboard.targetproperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)