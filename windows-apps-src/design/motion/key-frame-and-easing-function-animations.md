---
title: 키 프레임 애니메이션 및 감속/가속 함수 애니메이션
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: 선형 키 프레임 애니메이션, KeySpline 값을 사용하는 키 프레임 애니메이션 또는 감속/가속 함수는 거의 동일한 시나리오에 사용되는 세 가지 다른 기술입니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 696a3f0f065c209bec28f774224da6e4c8d93275
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635548"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>키 프레임 애니메이션 및 감속/가속 함수 애니메이션



선형 키 프레임 애니메이션, **KeySpline** 값을 사용하는 키 프레임 애니메이션 또는 감속/가속 함수는 거의 동일한 시나리오에 사용되는 세 가지 다른 기술로, 약간 더 복잡하며 시작 상태에서 종료 상태까지 비선형 애니메이션 동작을 사용하는 스토리보드 애니메이션을 만듭니다.

## <a name="prerequisites"></a>필수 구성 요소

[스토리보드 애니메이션](storyboarded-animations.md) 항목을 읽어야 합니다. 이 항목은 [스토리보드 애니메이션](storyboarded-animations.md)에서 설명한 애니메이션 개념을 기반으로 하며 해당 개념을 다시 반복하지 않습니다. 예를 들어 [스토리보드 애니메이션](storyboarded-animations.md)에서는 애니메이션을 대상으로 지정하는 방법, 스토리보드를 리소스로 정의하는 방법, [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 속성 값([**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration), [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior) 등)에 대해 설명합니다.

## <a name="animating-using-key-frame-animations"></a>키 프레임 애니메이션 사용

키 프레임 애니메이션은 애니메이션 타임라인을 따라 특정 지점에서 도달하는 둘 이상의 대상 값을 허용합니다. 즉, 각 키 프레임에서 다른 중간 값을 지정할 수 있으며 마지막에 도달한 키 프레임이 최종 애니메이션 값입니다. 애니메이션 효과를 주는 여러 값을 지정함으로써 더 복잡한 애니메이션을 만들 수 있습니다. 또한 키 프레임 애니메이션은 다른 보간 논리를 사용하며 각각은 애니메이션 형식에 따라 다른 **KeyFrame** 하위 클래스로 구현됩니다. 특히 각 키 프레임 애니메이션 형식에는 해당 키 프레임을 지정하는 **KeyFrame** 클래스의 **Discrete**, **Linear**, **Spline** 및 **Easing** 변형이 있습니다. 예를 들어 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)을 대상으로 하고 키 프레임을 사용하는 애니메이션을 지정하려면 [**DiscreteDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243130), [**LinearDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210316), [**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446) 및 [**EasingDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210269)으로 키 프레임을 선언할 수 있습니다. 단일 **KeyFrames** 컬렉션 내에서 이러한 형식의 일부 및 전체를 사용하여 새 키 프레임에 도달할 때마다 보간을 변경할 수 있습니다.

보간 동작의 경우 각 키 프레임은 **KeyTime** 시간에 도달할 때까지 보간을 제어합니다. 해당 시간에 해당 **Value**에도 도달합니다. 키 프레임이 더 있을 경우 해당 값은 시퀀스에서 다음 키 프레임에 대한 시작 값이 됩니다.

애니메이션이 시작될 때 **KeyTime**이 "0:0:0"인 키 프레임이 없으면 시작 값은 속성의 애니메이션 효과를 주지 않은 모든 값이 됩니다. 이는 **From**이 없을 경우 **From**/**To**/**By** 애니메이션이 작동하는 방식과 유사합니다.

키 프레임 애니메이션 기간은 해당 키 프레임에 설정된 가장 높은 **KeyTime** 값과 암시적으로 동일합니다. 원하는 경우 명시적 [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration)을 설정할 수 있지만 고유한 키 프레임의 **KeyTime**보다 짧지 않도록 주의해야 합니다. 그렇지 않으면 애니메이션 일부가 잘릴 수 있습니다.

[  **Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) 외에 모든 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 기반 속성을 키 프레임 애니메이션에서 설정할 수 있으며, 키 프레임 애니메이션 클래스도 **Timeline**에서 파생되므로 **From**/**To**/**By** 애니메이션에서도 설정할 수 있습니다. 옵션은 다음과 같습니다.

-   [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.autoreverse): 끝에서 프레임을 역순으로 반복 되 고 마지막 키 프레임에 도달 합니다. 그러면 애니메이션 기간이 명확히 두 배가 됩니다.
-   [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.begintime): 애니메이션의 시작을 연기 합니다. 프레임의 **KeyTime** 값에 대한 타임라인이 **BeginTime**에 도달한 다음에야 카운팅을 시작하므로 프레임이 잘릴 위험이 없습니다.
-   [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior): 마지막 키 프레임에 도달한 경우 어떻게 되는지 제어 합니다. **FillBehavior**는 중간 키 프레임에 영향을 주지 않습니다.
-   [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty):
    -   **Forever**로 설정하면 키 프레임과 타임라인이 무한 반복됩니다.
    -   반복 횟수로 설정하면 타임라인이 해당 횟수만큼 반복됩니다.
    -   [  **Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)으로 설정하면 해당 시간에 도달할 때까지 타임라인이 반복됩니다. 따라서 타임라인의 암시적 기간 중 정수 요소가 아니면 키 프레임 시퀀스 중간에 애니메이션이 잘릴 수 있습니다.
-   [**SpeedRatio** ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.speedratioproperty) (주로 사용 됨)

### <a name="linear-key-frames"></a>선형 키 프레임

선형 키 프레임에서는 프레임의 **KeyTime**에 도달할 때까지 값의 간단한 선형 보간이 수행됩니다. 이 보간 동작은 [스토리보드 애니메이션](storyboarded-animations.md) 항목에서 설명하는 더 간단한 **From**/**To**/**By** 애니메이션과 가장 유사합니다.

다음은 키 프레임 애니메이션을 사용하여 직사각형의 렌더 높이를 조정하는 방법으로 선형 키 프레임을 사용합니다. 이 예제에서는 처음 4초 동안 직사각형의 높이가 선형으로 약간 증가한 다음 마지막 몇 초 동안에는 직사각형이 시작 높이의 두 배가 될 때까지 신속하게 크기가 조정되는 애니메이션을 실행합니다.

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

불연속 키 프레임에서는 보간을 전혀 사용하지 않습니다. **KeyTime**에 도달하면 새 **Value**가 단순히 적용됩니다. 애니메이션 효과를 줄 UI 속성에 따라 "이동"하는 것처럼 보이는 애니메이션이 생성되는 경우가 종종 있습니다. 이 동작은 사용자가 실제로 원하는 미적 동작인지 확인해야 합니다. 선언하는 키 프레임 수를 늘려 명확한 이동을 최소화할 수 있지만 매끄러운 애니메이션이 목표인 경우 선형 또는 스플라인 키 프레임을 대신 사용하는 것이 더 좋을 수 있습니다.

> [!NOTE]
> 불연속 키 프레임은 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 및 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 형식이 아닌 값에 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132)으로 애니메이션 효과를 주는 유일한 방법입니다. 이 항목의 뒷부분에서 이 방법에 대해 좀 더 자세히 설명합니다.

### <a name="spline-key-frames"></a>스플라인 키 프레임

스플라인 키 프레임은 **KeySpline** 속성의 값에 따라 값 사이의 가변 전환을 만듭니다. 이 속성은 베지어 곡선의 첫 번째 및 두 번째 제어 지점을 지정하며, 이는 애니메이션의 가속을 나타냅니다. 기본적으로 [**KeySpline**](https://msdn.microsoft.com/library/windows/apps/BR210307)은 함수-시간 그래프가 해당 베지어 곡선의 모양이 되는 시간에 따른 함수 관계를 정의합니다. 일반적으로 **KeySpline** 값은 네 개의 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 값이 공백 또는 쉼표로 구분되는 XAML 속기 특성 문자열에 지정합니다. 이러한 값은 베지어 곡선의 두 제어점에 대한 "X,Y" 쌍입니다. "X"는 시간이고 "Y"는 값에 대한 함수 한정자입니다. 각 값은 항상 0에서 1(포함) 사이여야 합니다. **KeySpline**에 대한 제어점을 수정하지 않으면 0,0부터 1,1까지의 직선은 선형 보간의 시간에 따른 함수 표현입니다. 제어점은 해당 곡선의 모양을 변경하여 스플라인 애니메이션의 시간에 따른 함수 동작을 변경합니다. 이 동작은 그래프로 시각적으로 표시하는 것이 가장 좋을 수 있습니다. 브라우저에서 [Silverlight 키 스플라인 시각화 도우미 샘플](https://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample)을 실행하여 제어점이 곡선을 수정하는 방법 및 해당 곡선을 **KeySpline** 값으로 사용할 때 샘플 애니메이션이 실행되는 방법을 확인할 수 있습니다.

다음 예제에서는 애니메이션에 적용된 세 가지 다른 키 프레임을 보여 주며, 마지막 키 프레임은 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 값([**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446))에 대한 키 스플라인 애니메이션이 됩니다. **KeySpline**에 대해 적용된 "0.6,0.0 0.9,0.00" 문자열을 확인하세요. 이 예제에서는 애니메이션이 처음에는 느리게 실행되는 것처럼 보이지만 **KeyTime**에 도달하기 바로 전에는 해당 값에 빠르게 도달하는 곡선이 생성됩니다.

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

### <a name="easing-key-frames"></a>감속/가속 키 프레임

감속/가속 키 프레임은 보간이 적용되는 키 프레임이며 보간의 시간에 따른 함수가 여러 가지 미리 정의된 수학 공식에 의해 제어됩니다. 실제로 일부 감속/가속 함수 형식과 동일한 결과를 스플라인 키 프레임에서도 많이 생성할 수 있지만 스플라인에서 재현할 수 없는 [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049)와 같은 몇 가지 감속/가속 함수도 있습니다.

감속/가속 키 프레임에 감속/가속 함수를 적용하려면 해당 키 프레임에 대한 XAML에서 **EasingFunction** 속성을 속성 요소로 설정합니다. 값의 경우 감속/가속 함수 형식 중 하나에 대해 개체 요소를 지정합니다.

다음 예제에서는 [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126)를 적용한 다음 [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057)를 연속 키 프레임으로 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)에 적용하여 튀는 효과를 냅니다.

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

이는 단 하나의 감속/가속 함수 예제입니다. 다음 섹션에서 더 자세히 살펴보겠습니다.

## <a name="easing-functions"></a>감속/가속 함수

감속/가속 함수를 사용하면 애니메이션에 사용자 지정 수학 공식을 적용할 수 있습니다. 수학 연산이 실제 물리학을 2-D 좌표계에서 시뮬레이트하는 애니메이션을 생성하는 데 유용한 경우가 자주 있습니다. 예를 들어 개체가 사실적으로 튀게, 즉 스프링 위에 있는 것처럼 동작하게 할 수 있습니다. 키 프레임 애니메이션이나 심지어 **From**/**To**/**By** 애니메이션을 사용해서도 이와 비슷한 효과를 낼 수 있으나 상당한 작업이 필요하고 수학 공식을 사용하는 것보다 정확도가 떨어집니다.

다음과 같은 세 가지 방식으로 애니메이션에 감속/가속 함수를 적용할 수 있습니다.

-   이전 섹션에서 설명한 대로 키 프레임 애니메이션에서 감속/가속 키 프레임을 사용합니다. [  **EasingColorKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210267), [**EasingDoubleKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction.aspx) 또는 [**EasingPointKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210279)을 사용합니다.
-   **From**/**To**/**By** 애니메이션 형식 중 하나에서 **EasingFunction** 속성을 설정합니다. [  **ColorAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR243075), [**DoubleAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) 또는 [**PointAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210354)을 사용합니다.
-   [  **VisualTransition**](https://msdn.microsoft.com/library/windows/apps/BR209034)의 일부로 [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037)을 설정합니다. 이 방법은 컨트롤에 대한 시각적 상태 정의와 관련이 있습니다. 자세한 내용은 [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) 또는 [시각적 상태에 대한 스토리보드](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)를 참조하세요.

다음은 감속/가속 함수의 목록입니다.

-   [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049): 표시 된 경로로 애니메이션을 시작 하기 전에 애니메이션의 이동을 약간 취소 합니다.
-   [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057): 바운스 효과 만듭니다.
-   [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063): 애니메이션 가속 또는 감속 순환 함수를 사용 하 여 만듭니다.
-   [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126): 애니메이션 가속 또는 감속 수식 f(t)를 사용 하 여을 만듭니다 t3 =.
-   [**ElasticEase**](https://msdn.microsoft.com/library/windows/apps/BR210282): 완전히 정지할 때까지 앞뒤로 진동 하는 스프링과 유사한 애니메이션을 만듭니다.
-   [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294): 애니메이션 가속 또는 감속는 지 수 공식을 사용 하 여 만듭니다.
-   [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399): 애니메이션 가속 또는 감속 수식 f(t)를 사용 하 여을 만듭니다 여기서 p는 같음 tp = 합니다 [ **전원** ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power) 속성입니다.
-   [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403): 애니메이션 가속 또는 감속 수식 f(t)를 사용 하 여을 만듭니다 t2 =.
-   [**QuarticEase**](https://msdn.microsoft.com/library/windows/apps/BR210405): 애니메이션 가속 또는 감속 수식 f(t)를 사용 하 여을 만듭니다 t4 =.
-   [**QuinticEase**](https://msdn.microsoft.com/library/windows/apps/BR210407): 애니메이션 가속 또는 감속 수식 f(t)를 사용 하 여을 t5 =.
-   [**다양의 SineEase**](https://msdn.microsoft.com/library/windows/apps/BR210439): 가속 또는 감속 사인 수식을 사용 하는 애니메이션을 만듭니다.

일부 감속/가속 함수에는 고유한 속성이 있습니다. 예를 들어 [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057)에는 특정 **BounceEase**의 시간에 따른 함수 동작을 수정하는 두 가지 [**Bounces**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounces.aspx) 및 [**Bounciness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounciness.aspx) 속성이 있습니다. [  **CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126)와 같은 다른 감속/가속 함수에는 모든 감속/가속 함수가 공유하는 [**EasingMode**](https://msdn.microsoft.com/library/windows/apps/BR210275) 속성 외에 다른 속성이 없으며 항상 동일한 시간에 따른 함수 동작을 생성합니다.

이러한 감속/가속 함수 일부는 속성이 있는 감속/가속 함수에서 속성을 설정하는 방법에 따라 약간 겹칩니다. 예를 들어 [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403)는 [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power)가 2인 [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399)와 정확하게 동일합니다. 또한 [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063)는 기본적으로 기본값 [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294)입니다.

[  **BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049) 감속/가속 함수는 **From**/**To** 또는 키 프레임의 값에 따라 설정되는 일반 범위를 벗어나는 값을 변경할 수 있기 때문에 고유합니다. 이 함수는 값을 변경하여 일반 **From**/**To** 동작에서 예상되는 것과 반대되는 방향으로 애니메이션을 시작하고 **From** 또는 시작 값으로 다시 이동한 다음 정상적으로 애니메이션을 실행합니다.

이전 예제에서는 키 프레임 애니메이션에 대해 감속/가속 함수를 선언하는 방법을 살펴보았습니다. 다음 샘플에서는 감속/가속 함수를 **From**/**To**/**By** 애니메이션에 적용합니다.

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

감속/가속 함수가 **From**/**To**/**By** 애니메이션에 적용되면 애니메이션의 [**Duration에 따라**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration)**From**과 **To** 값 사이에서 값이 보간되는 방법의 시간에 따른 함수 특성을 변경합니다. 감속/가속 함수를 사용하지 않으면 선형 보간이 됩니다.

## <a name="span-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>개별적인 개체 값 애니메이션

한 가지 애니메이션 형식은 애니메이션 효과를 준 값을 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 또는 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 형식이 아닌 속성에 적용할 수 있는 유일한 방법이므로 특별히 검토할 가치가 있습니다. 이는 키 프레임 애니메이션 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)입니다. [  **Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) 값을 사용하여 애니메이션 효과를 주는 작업은 프레임 사이에서 값을 보간할 수 없기 때문에 어렵습니다. 프레임의 [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR210342)에 도달하면 애니메이션 효과를 준 값이 키 프레임의 **Value**에서 지정한 값으로 바로 설정됩니다. 하나의 키 프레임의 사용 방법이 없는 보간에 있기 때문에 합니다 **ObjectAnimationUsingKeyFrames** 키 프레임 컬렉션: [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132)합니다.

설정하려는 개체 값이 특성 구문에서 **Value**를 채우는 문자열로 나타낼 수 없는 경우가 자주 있기 때문에 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132)의 [**Value**](https://msdn.microsoft.com/library/windows/apps/BR210344)는 종종 속성 요소 구문을 사용하여 설정됩니다. [StaticResource](https://msdn.microsoft.com/library/windows/apps/Mt185588)와 같은 참조를 사용하는 경우 특성 구문을 계속 사용할 수 있습니다.

기본 템플릿에서 사용된 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)를 확인할 수 있는 한 위치는 템플릿 속성이 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 리소스를 참조하는 경우입니다. 이러한 리소스는 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 값이 아닌 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 개체이며 시스템 테마 ([**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807))로 정의되는 리소스를 사용합니다. 이러한 리소스는 [**TextBlock.Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665)와 같은 **Brush** 형식 값에 직접 할당할 수 있으며 간접 대상을 사용할 필요가 없습니다. 그러나 **SolidColorBrush**는 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 또는 **Color**가 아니므로 **ObjectAnimationUsingKeyFrames**를 사용하여 리소스를 사용해야 합니다.

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

또한 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)를 사용하여 열거형 값을 사용하는 속성에 애니메이션 효과를 줄 수 있습니다. 다음은 Windows 런타임 기본 템플릿에서 가져온 명명된 스타일의 다른 예입니다. [  **Visibility**](https://msdn.microsoft.com/library/windows/apps/BR209006) 열거형 상수를 사용하는 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 속성을 설정하는 방법을 확인하세요. 이 경우 특성 구문을 사용하여 값을 설정할 수 있습니다. “Collapsed”와 같이 열거형 값으로 속성을 설정하려면 열거형의 비정규화된 상수 이름만 필요합니다.

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

[  **ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) 프레임 집합에 둘 이상의 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132)을 사용할 수 있습니다. 이 방법은 [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760)의 값에 애니메이션 효과를 주어 "슬라이드 쇼" 애니메이션을 만드는 흥미 있는 방법이며, 여러 개체 값이 유용할 수 있는 시나리오를 예로 들 수 있습니다.

 ## <a name="related-topics"></a>관련 항목

* [속성 경로 구문](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [종속성 속성 개요](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [**스토리 보드**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)