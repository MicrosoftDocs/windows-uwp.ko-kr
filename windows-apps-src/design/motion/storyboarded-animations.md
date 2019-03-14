---
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: 스토리보드 애니메이션
description: 스토리보드 애니메이션은 시각적 측면의 애니메이션만 의미하는 것이 아닙니다.
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 212ef252e7d123ebf457a6584f77addb04fdfb2c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627078"
---
# <a name="storyboarded-animations"></a>스토리보드 애니메이션

스토리보드 애니메이션은 시각적 측면의 애니메이션만 의미하는 것이 아닙니다. 스토리보드 애니메이션은 종속성 속성의 값을 시간의 함수로 변경하는 방법입니다. 애니메이션 라이브러리에 없는 스토리보드 애니메이션이 필요할 수 있는 주요 이유 중 하나는 컨트롤 템플릿이나 페이지 정의의 일부로 컨트롤의 시각적 상태를 정의한다는 점입니다.

## <a name="differences-with-silverlight-and-wpf"></a>Silverlight와 WPF의 차이점

Microsoft Silverlight 또는 WPF(Windows Presentation Foundation)에 대해 잘 아는 경우 이 섹션을 읽어 보세요. 그렇지 않으면 건너뛸 수 있습니다.

일반적으로 Windows 런타임 앱에서 스토리보드 애니메이션을 만드는 과정은 Silverlight 또는 WPF와 비슷합니다. 그러나 중요한 차이점도 많습니다.

-   스토리보드 애니메이션은 UI에 시각적으로 애니메이션 효과를 주는 유일한 방법이 아니며 앱 개발자가 선택할 수 있는 가장 간단한 방법도 아닙니다. 스토리보드 애니메이션을 사용하는 대신 테마 애니메이션 및 전환 애니메이션을 사용하는 것이 더 좋은 디자인 사례인 경우가 종종 있습니다. 이러한 애니메이션에서는 복잡한 애니메이션 속성 대상 지정을 자세히 검토하지 않고도 권장 UI 애니메이션을 신속하게 만들 수 있습니다. 자세한 내용은 [애니메이션 개요](xaml-animation.md)를 참조하세요.
-   Windows 런타임에서 많은 XAML 컨트롤에는 테마 애니메이션과 전환 애니메이션이 기본 제공 동작의 일부로 포함되어 있습니다. 대부분의 경우 WPF 및 Silverlight 컨트롤에는 기본 애니메이션 동작이 없었습니다.
-   사용자가 만드는 일부 사용자 지정 애니메이션의 경우 해당 애니메이션으로 인해 UI 성능이 저하된다고 애니메이션 시스템에서 확인될 경우 기본적으로 Windows 런타임 앱에서 실행할 수 없습니다. 성능에 영향을 미칠 수 있다고 시스템에서 확인하는 애니메이션을 *종속 애니메이션*이라고 합니다. 활성 사용자 입력 및 기타 업데이트가 런타임 변경을 UI에 적용하는 위치인 UI 스레드에 애니메이션의 클로킹이 직접 작동하기 때문에 이 애니메이션은 종속적입니다. UI 스레드에서 시스템 리소스를 많이 소비하는 종속 애니메이션을 사용하면 특정 상황에서 앱이 응답하지 않는 것으로 나타날 수 있습니다. 애니메이션에서 레이아웃 변경을 일으키거나 그 밖에 성능이 UI 스레드에 영향을 미칠 가능성이 있는 경우 명시적으로 애니메이션을 사용하도록 설정하여 실행되는지 확인해야 합니다. 특정 애니메이션 클래스에서 **EnableDependentAnimation** 속성이 이 용도로 사용됩니다. 자세한 내용은 [종속 애니메이션과 독립 애니메이션](./storyboarded-animations.md#dependent-and-independent-animations)을 참조하세요.
-   사용자 지정 감속/가속 함수는 Windows 런타임에서 현재 지원되지 않습니다.

## <a name="defining-storyboarded-animations"></a>스토리보드 애니메이션 정의

스토리보드 애니메이션은 종속성 속성의 값을 시간의 함수로 변경하는 방법입니다. 애니메이션 효과를 주려는 속성이 항상 앱의 UI에 직접 영향을 주는 속성은 아닙니다. 그러나 XAML은 앱의 UI 정의에 대한 것이므로 일반적으로 UI 관련 속성에 애니메이션 효과를 줍니다. 예를 들어 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932)의 각도 또는 단추 배경의 색상 값에 애니메이션 효과를 줄 수 있습니다.

스토리보드 애니메이션을 정의하는 주요 이유 중 하나는 컨트롤 작성자이거나 컨트롤의 템플릿을 다시 만들려는 경우와 시각적 상태를 정의하는 경우입니다. 자세한 내용은 [시각적 상태에 대한 스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)을 참조하세요.

시각적 상태를 정의하는지 앱에 대한 사용자 지정 애니메이션을 정의하는지에 관계없이 이 항목에서 설명하는 스토리보드 애니메이션에 대한 개념 및 API는 두 경우에 대부분 적용됩니다.

애니메이션 효과를 주려면 스토리보드 애니메이션에서 대상으로 지정하려는 속성이 *종속성 속성*이어야 합니다. 종속성 속성은 Windows 런타임 XAML 구현의 주요 기능입니다. 가장 일반적인 UI 요소의 쓰기 가능한 속성이 일반적으로 종속성 속성으로 구현되므로 애니메이션 효과를 주거나, 데이터 바인딩된 값을 적용하거나 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849)을 적용하고 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817)로 속성 대상을 지정할 수 있습니다. 종속성 속성 작동 방식에 대한 자세한 내용은 [종속성 속성 개요](https://msdn.microsoft.com/library/windows/apps/Mt185583)를 참조하세요.

대부분의 경우 스토리보드 애니메이션은 XAML을 작성하여 정의합니다. Microsoft Visual Studio와 같은 도구를 사용하는 경우 XAML이 자동으로 생성됩니다. 코드를 사용하여 스토리보드 애니메이션을 정의할 수도 있지만 덜 일반적입니다.

간간한 예제를 살펴보겠습니다. 이 XAML 예제에서는 특정 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 개체에 대한 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 속성에 애니메이션 효과를 줍니다.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>

  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

### <a name="identifying-the-object-to-animate"></a>애니메이션 효과를 줄 개체 식별

이전의 예제에서는 스토리보드가 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)의 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 속성에 애니메이션 효과를 줍니다. 개체 자체에 대한 애니메이션을 선언하지 않습니다. 대신 스토리보드의 애니메이션 정의 내에서 이 작업을 수행합니다. 일반적으로 스토리보드는 애니메이션 효과를 줄 개체의 XAML UI 정의에 바로 근접해 있지 않는 XAML에 정의되어 있습니다. 대신 XAML 리소스로 설정됩니다.

애니메이션을 대상에 연결하려면 식별하는 프로그래밍 이름으로 대상을 참조합니다. 항상 XAML UI 정의의 [x:Name 특성](https://msdn.microsoft.com/library/windows/apps/Mt204788)을 적용하여 애니메이션 효과를 줄 개체의 이름을 지정해야 합니다. 그런 다음 애니메이션 정의 내에서 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/Hh759823)을 설정하여 애니메이션 효과를 줄 개체를 대상으로 지정합니다. **Storyboard.TargetName** 값에 대해 대상 개체의 이름 문자열을 사용하며, 이전에 및 다른 위치에서 x:Name 특성을 사용하여 설정한 문자열입니다.

### <a name="targeting-the-dependency-property-to-animate"></a>애니메이션 효과를 줄 종속성 속성을 대상으로 지정

애니메이션에서 [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824)에 대한 값을 설정합니다. 이 값은 대상 개체에서 애니메이션 효과를 줄 특정 속성을 결정합니다.

경우에 따라 대상 개체의 직접 속성이 아닌 속성을 대상으로 지정해야 하지만 그러면 개체-속성 관계에서 더 깊게 중첩됩니다. 애니메이션 효과를 줄 수 있는 속성 형식([**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870), [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723))을 참조할 때까지 영향을 주는 일련의 개체 및 속성 값으로 드릴다운하려면 이렇게 해야 하는 경우가 종종 있습니다. 이 개념을 *간접 대상*이라고 하고 이 방식으로 속성을 대상 지정하는 구문을 *속성 경로*라고 합니다.

예를 들면 다음과 같습니다. 스토리보드 애니메이션에 대한 일반적인 시나리오 한 가지는 앱 UI 또는 컨트롤 일부의 색상을 변경하여 컨트롤을 특정 상태로 나타내는 것입니다. 예를 들어 빨간색에서 녹색으로 바뀌도록 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)의 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665)에 애니메이션 효과를 주려고 합니다. [  **ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066)이 관련된다고 예상할 것이며 그 예상이 맞습니다. 그러나 개체의 색상에 영향을 주는 UI 요소의 속성이 모두 실제로는 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 형식이 아닙니다. 대신 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 형식입니다. 따라서 실제로 애니메이션의 대상으로 지정해야 하는 항목은 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 클래스의 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 속성으로, 이러한 색상 관련 UI 속성에 일반적으로 사용되는 **Brush** 파생 형식입니다. 또한 다음은 애니메이션의 속성 대상 지정을 위한 속성 경로 구성 측면에서 어떻게 표시되는지를 보여 줍니다.

```xaml
<Storyboard x:Name="myStoryboard">
  <ColorAnimation
    Storyboard.TargetName="tb1"
    Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
    From="Red" To="Green"/>
</Storyboard>
```

다음은 이 구문을 해당 요소 측면에서 고려하는 방법입니다.

- 각 () 괄호 집합은 속성 이름을 묶습니다.
- 속성 이름 내에는 점이 있고 해당 점은 형식 이름과 속성 이름으로 구분하여 식별하려는 속성이 모호하지 않도록 합니다.
- 괄호 외부에 있는 가운데의 점은 단계입니다. 이 점은 구문으로 해석되며 첫 번째 속성(개체)의 값을 가져오고 개체 모델을 단계별로 이동하고 첫 번째 속성 값의 특정 하위 속성을 대상으로 지정한다는 의미입니다.

다음은 간접 속성 대상 지정 및 사용할 구문과 유사한 몇 가지 속성 경로 문자열을 사용할 수 있는 애니메이션 대상 지정 시나리오 목록입니다.

- 애니메이션 효과 적용 합니다 [ **X** ](https://msdn.microsoft.com/library/windows/apps/BR243029) 값을 [ **TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)에 적용 될 때를 [ **RenderTransform** ](https://msdn.microsoft.com/library/windows/apps/BR208980): `(UIElement.RenderTransform).(TranslateTransform.X)`
- 애니메이션 적용을 [ **색** ](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 내에서 [ **GradientStop** ](https://msdn.microsoft.com/library/windows/apps/BR210078) 의 [ **LinearGradientBrush** ](https://msdn.microsoft.com/library/windows/apps/BR210108)처럼에 적용 된 [ **채우기**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill): `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
- 애니메이션 효과 적용 합니다 [ **X** ](https://msdn.microsoft.com/library/windows/apps/BR243029) 값을 [ **TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), 1에서 4 개의 변환를 [  **TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022)처럼에 적용 된 [ **RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980):`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

이러한 예제 중 일부에서 숫자 주위에 대괄호를 사용하는 것을 확인할 수 있습니다. 이는 인덱서이며, 앞에 오는 속성 이름에는 컬렉션이 값으로 포함되며 해당 컬렉션에서 0부터 시작하는 색인으로 식별되는 항목이 필요함을 나타냅니다.

XAML 연결 속성에도 애니메이션 효과를 줄 수 있습니다. 항상 전체 연결 속성 이름을 괄호로 묶습니다(예: `(Canvas.Left)`). 자세한 내용은 [XAML 연결 속성에 애니메이션 효과 주기](./storyboarded-animations.md#animating-xaml-attached-properties)를 참조하세요.

애니메이션 효과를 줄 속성의 간접 대상에 속성 경로를 사용하는 방법에 대한 자세한 내용은 [속성 경로 구문](https://msdn.microsoft.com/library/windows/apps/Mt185586) 또는 [**Storyboard.TargetProperty 연결된 속성**](https://msdn.microsoft.com/library/windows/apps/Hh759824)을 참조하세요.

### <a name="animation-types"></a>애니메이션 형식

Windows 런타임 애니메이션 시스템에는 스토리보드 애니메이션에서 적용할 수 있는 세 가지 특정 형식이 있습니다.

-   [**이중**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)를 사용 하 여 애니메이션을 적용할 수 있습니다 [ **DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)
-   [**지점**](https://msdn.microsoft.com/library/windows/apps/BR225870)를 사용 하 여 애니메이션을 적용할 수 있습니다 [ **PointAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210346)
-   [**색**](https://msdn.microsoft.com/library/windows/apps/Hh673723)를 사용 하 여 애니메이션을 적용할 수 있습니다 [ **ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066)

개체 참조 값에 대해 일반화된 [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) 애니메이션 형식도 있으며, 뒷부분에서 설명합니다.

### <a name="specifying-the-animated-values"></a>애니메이션 효과를 준 값 지정

지금까지는 애니메이션 효과를 줄 개체 및 속성을 대상으로 지정하는 방법을 살펴보았으며, 애니메이션이 실행될 때 속성 값에 대해 수행하는 작업에 대해서는 아직 설명하지 않았습니다.

지금까지 설명한 애니메이션 형식은 경우에 따라 **From**/**To**/**By** 애니메이션이라고도 합니다. 즉, 애니메이션이 애니메이션 정의에서 제공되는 이러한 입력 중 하나 이상을 사용하여 시간에 따라 속성의 값을 변경한다는 의미입니다.

-   값은 **From** 값에서 시작합니다. **From** 값을 지정하지 않으면 시작 값은 애니메이션이 실행되기 전에 애니메이션 효과를 준 속성에 있던 모든 값이 됩니다. 이 값은 기본값, 스타일 또는 템플릿의 값 또는 XAML UI 정의나 앱 코드에서 특별히 적용한 값이 될 수 있습니다.
-   애니메이션이 끝날 때의 값은 **To** 값입니다.
-   또는 시작 값을 기준으로 종료 값을 지정하려면 **By** 속성을 설정합니다. **To** 속성 대신 이 속성을 설정하는 것입니다.
-   **To** 값 또는 **By** 값을 지정하지 않으면 종료 값은 애니메이션이 실행되기 전에 애니메이션 효과를 준 속성에 있던 모든 값이 됩니다. 이 경우 **From** 값을 사용하는 것이 좋습니다. 그렇지 않으면 애니메이션에서 값을 전혀 변경하지 않아 시작 값과 종료 값이 같아지기 때문입니다.
-   일반적으로 애니메이션에는 **From**, **By** 또는 **To** 중 하나 이상이 있지만 세 개 모두 없을 수 있습니다.

이전 XAML 예제로 다시 돌아가 **From** 및 **To** 값과 **Duration**을 다시 살펴보겠습니다. 이 예제에서는 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 속성에 애니메이션 효과를 주며 **Opacity**의 속성 형식은 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)입니다. 따라서 여기서 사용하는 애니메이션은 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)입니다.

`From="1.0" To="0.0"` 애니메이션을 실행 하면 지정 된 [ **불투명도** ](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 속성 값이 1에서 시작 하 고 0으로 애니메이션 효과 줍니다. 즉, **Opacity** 속성에 대한 이러한 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 값의 의미와 관련하여 이 애니메이션은 개체가 불투명하게 시작된 다음 투명하게 페이드되도록 합니다.

```xaml
...
<Storyboard x:Name="myStoryboard">
  <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
</Storyboard>
...
```

`Duration="0:0:1"` 애니메이션 지속, 즉, 사각형 페이드 속도 지정 합니다. [  **Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 속성은 *시*:*분*:*초* 형식으로 지정합니다. 이 예제의 기간은 1초입니다.

[  **Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 값 및 XAML 구문에 대한 자세한 내용은 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)을 참조하세요.

> [!NOTE]
> 앞에서 살펴본 예제에서 애니메이션 효과를 줄 개체의 시작 상태에 항상 1인 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)가 있다는 확신이 있는 경우 기본값이나 명시적 설정을 통해 **From** 값을 생략할 수 있으며 애니메이션에서 암시적 시작 값을 사용해도 결과는 동일해집니다.

### <a name="fromtoby-are-nullable"></a>nullable인 From/To/By

앞에서 **From**, **To** 또는 **By**를 생략하여 애니메이션 효과를 주지 않은 현재 값을 누락된 값을 대체하는 값으로 사용할 수 있다고 설명했습니다. 애니메이션의 **From**, **To** 또는 **By** 속성이 추측할 수 있는 형식이 아닙니다. 예를 들어 [**DoubleAnimation.To**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) 속성의 형식이 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)이 아닙니다. 대신 **Double**에 대한 [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)입니다. 또한 기본값도 0이 아닌 **null**입니다. 해당 **null** 값은 애니메이션 시스템에서 **From**, **To** 또는 **By** 속성에 대한 값을 특별히 설정하지 않았음을 구분하는 방법입니다. Visual c + + 구성 요소 확장 (C + + /cli CX) 없는 **Nullable** 입력을 사용 하 여 [ **IReference** ](https://msdn.microsoft.com/library/windows/apps/BR225864) 대신 합니다.

### <a name="other-properties-of-an-animation"></a>애니메이션의 다른 속성

이 섹션에서 설명하는 다음 속성은 대부분의 애니메이션에 적합한 기본값이 있다는 점에서 모두 선택 사항입니다.

### <a name="autoreverse"></a>**AutoReverse**

애니메이션에서 [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) 또는 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211)를 지정하지 않으면 해당 애니메이션은 한 번 실행되며 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)으로 지정한 시간 동안 실행됩니다.

[  **AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) 속성은 타임라인이 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)의 끝에 도달한 후 반대로 재생되는지 여부를 지정합니다. **true**로 설정하면 애니메이션이 선언된 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)의 끝에 도달한 후 반대로 실행되어 해당 값을 종료 값(**To**)에서 시작 값(**From**)으로 다시 변경합니다. 즉, 애니메이션이 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 시간의 두 배인 기간 동안 효율적으로 실행된다는 의미입니다.

### <a name="repeatbehavior"></a>**RepeatBehavior**

[  **RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 속성은 타임라인이 재생되는 횟수 또는 타임라인이 반복되어야 하는 더 큰 기간을 지정합니다. 기본적으로 타임라인에는 "1x"의 반복 횟수가 있으며 해당 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 동안 한 번 재생되고 반복되지 않는다는 의미입니다.

애니메이션이 여러 번 반복하여 재생되도록 할 수 있습니다. 예를 들어, 값이 "3x"이면 애니메이션이 세 번 재생됩니다. 또는 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211)에 대해 다른 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)을 지정할 수 있습니다. 효율성을 높이려면 해당 **Duration**이 애니메이션 자체의 **Duration**보다 길어야 합니다. 예를 들어 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)이 "0:0:2"인 애니메이션에 대해 **RepeatBehavior**를 "0:0:10"으로 지정하면 해당 애니메이션은 5번 반복됩니다. 이러한 값이 똑같이 나누어지지 않으면 **RepeatBehavior** 시간에 도달할 때 애니메이션이 잘려 중간까지만 실행될 수 있습니다. 마지막으로 특수한 값 "Forever"를 지정하여 애니메이션이 의도적으로 중지될 때까지 무한 실행되도록 할 수 있습니다.

[  **RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411) 값 및 XAML 구문에 대한 자세한 내용은 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411)를 참조하세요.

### <a name="fillbehaviorstop"></a>**FillBehavior="Stop"**

기본적으로 애니메이션이 종료되면 애니메이션은 기간이 초과된 후에도 속성 값을 최종 **To** 또는 수정된 **By** 값으로 유지합니다. 그러나 [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) 속성의 값을 [**FillBehavior.Stop**](https://msdn.microsoft.com/library/windows/apps/BR210306)으로 설정하면 애니메이션 효과를 준 값이 애니메이션이 적용되기 이전에 있던 모든 값이나 보다 정확히 말해 종속성 속성 시스템에 의해 결정된 현재 유효한 값으로 되돌아갑니다. 이 차이점에 대한 자세한 내용은 [종속성 속성 개요](https://msdn.microsoft.com/library/windows/apps/Mt185583)를 참조하세요.

### <a name="begintime"></a>**BeginTime**

기본적으로 애니메이션의 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204)은 "0:0:0"이므로 포함하고 있는 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)가 실행되는 즉시 시작됩니다. **Storyboard**에 둘 이상의 애니메이션이 포함된 경우 다른 애니메이션 및 초기 애니메이션의 시작 시간에 시차를 두거나 의도적으로 지연을 짧게 만들려는 경우 이 값을 변경할 수 있습니다.

### <a name="speedratio"></a>**SpeedRatio**

[  **Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)에 둘 이상의 애니메이션이 있는 경우 **Storyboard**를 기준으로 여러 애니메이션의 시간 속도를 변경할 수 있습니다. 애니메이션이 실행되는 동안 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 시간이 경과되는 방식을 궁극적으로 제어하는 것은 부모 **Storyboard**입니다. 이 속성이 자주 사용되지는 않습니다. 자세한 내용은 [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/BR243213)를 참조하세요.

## <a name="defining-more-than-one-animation-in-a-storyboard"></a>**Storyboard**에서 둘 이상의 애니메이션 정의

[  **Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)의 콘텐츠는 둘 이상의 애니메이션 정의가 될 수 있습니다. 관련 애니메이션을 동일한 대상 개체의 두 속성에 적용하려는 경우 둘 이상의 애니메이션을 사용할 수 있습니다. 예를 들어 UI 요소의 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)로 사용된 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)의 [**TranslateX**](https://msdn.microsoft.com/library/windows/apps/BR228122) 및 [**TranslateY**](https://msdn.microsoft.com/library/windows/apps/BR228124) 속성을 모두 변경할 수 있습니다. 그러면 요소가 대각선으로 변환됩니다. 이 작업을 수행하려면 두 개의 다른 애니메이션이 필요하지만 이러한 두 애니메이션을 항상 함께 실행하려고 하므로 애니메이션이 동일한 **Storyboard**의 일부가 되도록 할 수 있습니다.

애니메이션이 동일한 형식이거나 동일한 개체를 대상으로 지정할 필요는 없습니다. 다른 기간을 사용할 수 있으며 속성 값을 공유하지 않아도 됩니다.

부모 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)가 실행되면 포함된 각 애니메이션도 실행됩니다.

[  **Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 클래스에는 실제로 애니메이션 형식과 동일한 애니메이션 속성이 많이 있는데, 둘 다 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 기본 클래스를 공유하기 때문입니다. 따라서 **Storyboard**에는 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 또는 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204)이 있을 수 있습니다. 포함된 모든 애니메이션이 해당 동작을 갖도록 하려는 경우가 아니라면 일반적으로 **Storyboard**에서 이러한 값을 설정하지 않습니다. 일반적인 규칙으로 **Storyboard**에서 설정된 모든 **Timeline** 속성은 모든 자식 애니메이션에 적용됩니다. 설정 해제되면 **Storyboard**에는 포함된 애니메이션의 가장 긴 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 값에서 계산되는 암시적 기간이 포함됩니다. 자식 애니메이션보다 짧은 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)이 **Storyboard**에서 명시적으로 설정되면 해당 애니메이션이 잘리게 되며 일반적으로 바람직하지 않습니다.

스토리보드에는 동일한 개체에서 동일한 속성을 대상으로 지정하고 애니메이션 효과를 주려는 두 개의 애니메이션이 포함될 수 없습니다. 그럴 경우 스토리보드가 실행될 때 런타임 오류가 발생합니다. 이 제한 사항은 의도적으로 다르게 설정한 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) 값 및 기간으로 인해 애니메이션 시간이 겹치지 않는 경우에도 적용됩니다. 실제로 보다 복잡한 애니메이션 타임라인을 단일 스토리보드에서 동일한 속성에 적용하려면 키 프레임 애니메이션을 사용합니다. [키 프레임 및 감속/가속 함수 애니메이션](key-frame-and-easing-function-animations.md)을 참조하세요.

애니메이션 시스템은 해당 입력이 여러 스토리보드에서 제공되는 경우 둘 이상의 애니메이션을 하나의 속성 값에 적용할 수 있습니다. 동시에 실행되는 스토리보드에 의도적으로 이 동작을 사용하는 것은 일반적이지 않습니다. 그러나 컨트롤 속성에 적용하는 앱 정의 애니메이션에서는 이전에 컨트롤의 시각적 상태 모델 일부로 실행된 애니메이션의 **HoldEnd** 값을 수정할 수 있습니다.

## <a name="defining-a-storyboard-as-a-resource"></a>스토리보드를 리소스로 정의

[  **Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)는 애니메이션 개체를 담는 컨테이너입니다. 일반적으로 **Storyboard**는 페이지 수준 [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) 또는 [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/BR242338)에서 애니메이션 효과를 주려는 개체에서 사용할 수 있는 리소스로 정의합니다.

다음 예제에서는 이전 예제 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)가 페이지 수준 [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) 정의에 포함되는 방식을 보여 줍니다. 이때 **Storyboard**는 루트 [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503)의 키 입력 리소스입니다. [x:Name 특성](https://msdn.microsoft.com/library/windows/apps/Mt204788)을 확인하세요. 이 특성은 **Storyboard**에 대해 변수 이름을 정의하는 방법이므로 코드 및 XAML의 다른 요소에서 나중에 **Storyboard**를 참조할 수 있습니다.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

page.xaml이나 app.xaml 같은 XAML 파일의 XAML 루트에서 리소스를 정의하는 것은 XAML에서 키 입력 리소스를 구성하는 방법에 일반적인 사례입니다. 리소스를 별도의 파일로 인수화하여 앱이나 페이지에 병합할 수도 있습니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/Mt187273)를 확인하세요.

> [!NOTE]
> Windows 런타임 XAML은 [x:Key 특성](https://msdn.microsoft.com/library/windows/apps/Mt204787) 또는 [x:Name 특성](https://msdn.microsoft.com/library/windows/apps/Mt204788)을 사용한 리소스 식별을 지원합니다. 결국에는 변수 이름으로 참조하여 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 메서드를 호출하고 애니메이션을 실행할 수 있기 때문에 x:Name 특성 사용이 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)에 더 일반적입니다. [x:Key 특성](https://msdn.microsoft.com/library/windows/apps/Mt204787)을 사용하면 [**Item**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.item) 인덱서와 같은 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 메서드를 사용하여 키 입력 리소스로 검색한 다음 검색된 개체를 **Storyboard**에 캐스트하여 **Storyboard** 메서드를 사용해야 합니다.

### <a name="storyboards-for-visual-states"></a>시각적 상태에 대한 스토리보드

또한 컨트롤의 시각적 모양에 대한 시각적 상태 애니메이션을 선언할 때 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 단위 내에 애니메이션을 배치합니다. 이 경우 정의한**Storyboard** 요소는 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849)에 더욱 깊게 중첩된 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 컨테이너로 이동합니다. **Style**은 키 입력 리소스입니다. **VisualState**에는 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager)가 호출할 수 있는 대상 이름이 있으므로 이 경우 **Storyboard**에 대한 키 또는 이름이 필요하지 않습니다. 컨트롤의 스타일은 페이지 또는 앱 **Resources** 컬렉션에 배치되지 않고 별도의 XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 파일로 인수화되기도 합니다. 자세한 내용은 [시각적 상태에 대한 스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)을 참조하세요.

## <a name="dependent-and-independent-animations"></a>종속 애니메이션과 독립 애니메이션

이제 애니메이션 시스템의 작동 방식에 대해 몇 가지 중요한 사항을 소개해야 합니다. 특히 애니메이션은 Windows 런타임 앱이 화면으로 렌더링되는 방식 및 해당 렌더링에서 처리 스레드를 사용하는 방식을 기본적으로 조작합니다. Windows 런타임 앱에는 항상 기본 UI 스레드가 있으며 이 스레드는 현재 정보를 화면을 업데이트하는 기능을 담당합니다. 또한 Windows 런타임 앱에는 컴퍼지션 스레드가 있으며, 레이아웃이 표시되기 직전에 해당 레이아웃을 사전 계산하는 데 사용됩니다. UI에 애니메이션 효과를 주면 UI 스레드에 대해 많은 작업이 수행될 가능성이 있습니다. 시스템은 각 새로 고침 사이의 매우 짧은 시간 간격을 사용하여 화면의 큰 영역을 다시 그려야 합니다. 이 작업은 애니메이션 효과를 준 속성의 최신 속성 값을 캡처하는 데 필요합니다. 이 작업을 수행할 때 신중하지 않으면 애니메이션에서 UI의 응답 속도를 저하시킬 위험이 있습니다. 그렇지 않으면 동일한 UI 스레드에도 있는 다른 앱 기능의 성능에 영향을 줍니다.

UI 스레드의 속도를 저하시킬 약간의 위험이 있는 것으로 확인된 다양한 애니메이션을 *종속 애니메이션*이라고 합니다. 이 위험에 종속되지 않는 애니메이션은 *독립 애니메이션*입니다. 종속 애니메이션과 독립 애니메이션의 차이점이 앞에서 설명한 대로 애니메이션 형식([**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 등)에 의해서만 결정되지는 않습니다. 대신 애니메이션 효과를 줄 특정 속성 및 컨트롤의 상속과 컴퍼지션과 같은 다른 요소에 의해 결정됩니다. 애니메이션에서 UI를 변경하는 경우에도 애니메이션이 UI 스레드에 최소한의 영향을 주고 대신 컴퍼지션 스레드에 의해 독립 애니메이션으로 처리되는 경우가 있습니다.

다음과 같은 특성이 있는 경우 애니메이션은 독립적입니다.

-   애니메이션의 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)이 0초입니다(경고 참조).
-   애니메이션이 [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)를 대상으로 합니다.
-   이러한 하위 속성 값을 대상으로 하는 애니메이션 [ **UIElement** ](https://msdn.microsoft.com/library/windows/apps/BR208911) 속성: [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx), [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980), [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection.aspx), [**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)
-   애니메이션이 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) 또는 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772)을 대상으로 합니다.
-   애니메이션이 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 값을 대상으로 하고 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)를 사용하여 해당 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)에 애니메이션 효과를 줍니다.
-   애니메이션이 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)입니다.

> [!WARNING]
> 애니메이션을 독립적으로 처리하려면 `Duration="0"`을 명시적으로 설정해야 합니다. 예를 들어 XAML에서 `Duration="0"`을 제거하면 프레임의 [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR243169)이 "0:0:0"이더라도 애니메이션을 종속으로 처리합니다.

```xaml
<Storyboard>
  <DoubleAnimationUsingKeyFrames
    Duration="0"
    Storyboard.TargetName="Button2"
    Storyboard.TargetProperty="Width">
    <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200"/>
  </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

애니메이션이 이러한 조건을 충족하지 않는 경우 종속 애니메이션일 가능성이 있습니다. 기본적으로 애니메이션 시스템에서는 종속 애니메이션이 실행되지 않습니다. 따라서 개발하고 테스트하는 프로세스 중에는 실행되는 애니메이션이 확인되지 않을 수도 있습니다. 이 애니메이션을 계속 사용할 수 있지만 이러한 각 종속 애니메이션을 특별히 사용하도록 설정해야 합니다. 애니메이션을 사용하도록 설정하려면 애니메이션 개체의 **EnableDependentAnimation** 속성을 **true**로 설정합니다. 애니메이션을 나타내는 각 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 하위 클래스에서는 속성이 다르게 구현되지만 모두 `EnableDependentAnimation`이라고 합니다.

앱 개발자가 종속 애니메이션을 사용하도록 하는 요구 사항은 애니메이션 시스템 및 개발 환경의 의식적인 디자인 측면입니다. 개발자가 UI의 응답성과 관련해서 애니메이션이 성능에 미치는 영향을 인식하도록 하려고 합니다. 애니메이션을 잘못 수행하면 전체 규모 앱에서 분리하고 디버그하기가 어렵습니다. 따라서 앱의 UI 환경에 실제로 필요한 종속 애니메이션만 켜는 것이 좋습니다. 많은 주기를 사용하는 장식 애니메이션으로 인해 앱의 성능이 너무 쉽게 손상되지 않도록 하려고 합니다. 애니메이션의 성능 팁에 대한 자세한 내용은 [애니메이션 및 미디어 최적화](https://msdn.microsoft.com/library/windows/apps/Mt204774)를 참조하세요.

또한 앱 개발자는 종속 애니메이션을 항상 사용하지 않도록 설정하는 앱 수준의 설정을 적용하도록 선택할 수 있으며, **EnableDependentAnimation**이 **true**인 애니메이션에서도 마찬가지입니다. [  **Timeline.AllowDependentAnimations**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.allowdependentanimations)을 참조하세요.

> [!TIP]
> 애니메이션 창 Blend for Visual Studio 2017에서에서 visual state 속성에 종속 애니메이션을 적용 하려고 할 때마다를 사용할 경우 경고 디자이너에서 표시 됩니다. 경고는 빌드 출력이 나 오류 목록에 표시 되지 않습니다. XAML을 직접 편집 하는 경우 디자이너에는 경고가 표시 되지 않습니다. 출력 창의 디버그 출력을 디버깅할 때 런타임 시 경고를 표시 애니메이션 독립적 이며 건너뜁니다.


## <a name="starting-and-controlling-an-animation"></a>애니메이션 시작 및 제어

지금까지 살펴본 모든 내용은 실제로 애니메이션이 실행되거나 적용되도록 하지 않습니다. 애니메이션이 시작되고 실행될 때까지 애니메이션이 XAML에서 선언하는 값 변경 내용은 숨어 있으며 아직 수행되지 않습니다. 앱 수명이나 사용자 환경과 관련이 있는 애니메이션을 어떤 방식으로든 명시적으로 시작해야 합니다. 가장 간단한 수준으로 애니메이션의 부모인 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)에서 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 메서드를 호출하여 해당 애니메이션을 시작합니다. 메서드를 XAML에서 직접 호출할 수는 없으므로 애니메이션을 사용하도록 설정하기 위해 수행하는 작업은 무엇이든 코드에서 수행해야 합니다. 해당 작업은 앱의 페이지 또는 구성 요소에 대해 코드 숨김이거나 사용자 지정 컨트롤 클래스를 정의하는 경우에는 컨트롤의 논리일 수 있습니다.

일반적으로 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin)을 호출하고 기간이 완료될 때까지 애니메이션이 실행되도록 하면 됩니다. 그러나 고급 애니메이션 제어 시나리오에 사용되는 다른 API뿐만 아니라 [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx), [**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) 및 [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) 메서드를 사용하여 런타임에 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)를 제어할 수도 있습니다.

무한 반복(`RepeatBehavior="Forever"`)되는 애니메이션을 포함하는 스토리보드에서 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin)을 호출하면 해당 애니메이션을 포함하는 페이지가 언로드되거나 특별히 [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) 또는 [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop)을 호출할 때까지 해당 애니메이션이 실행됩니다.

### <a name="starting-an-animation-from-app-code"></a>앱 코드에서 애니메이션 시작

애니메이션은 자동으로 시작하거나 사용자 작업에 대한 응답으로 시작할 수 있습니다. 자동 시작의 경우 일반적으로 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723)와 같은 개체 수명 이벤트를 사용하여 애니메이션 트리거 역할을 합니다. **Loaded** 이벤트는 해당 시점에 UI를 조작할 수 있게 되므로 이 경우에 적합한 이벤트이며 UI의 다른 부분이 계속 로드되고 있기 때문에 애니메이션이 시작 부분에서 잘리지 않습니다.

이 예제에서는 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed) 이벤트가 직사각형에 연결되어 사용자가 직사각형을 클릭하면 애니메이션이 시작합니다.

```xaml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
```

이벤트 처리기는 **Storyboard**의 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 메서드를 사용하여 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)(애니메이션)를 시작합니다.

```csharp
myStoryboard.Begin();
```

```cppwinrt
myStoryboard().Begin();
```

```cpp
myStoryboard->Begin();
```

```vb
myStoryBoard.Begin()
```

애니메이션이 값 적용을 마친 후 다른 논리를 실행하려는 경우 [**Completed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.completed.aspx) 이벤트를 처리할 수 있습니다. 또한 속성 시스템/애니메이션 조작 문제 해결을 위해 [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/BR242358) 메서드가 유용할 수 있습니다.

> [!TIP]
> 앱 코드에서 애니메이션을 시작하는 앱 시나리오를 코딩할 때마다 애니메이션이나 전환이 UI 시나리오에 대한 애니메이션 라이브러리에 이미 있는지 여부를 다시 검토해야 할 수 있습니다. 라이브러리 애니메이션은 모든 Windows 런타임 앱에서 보다 일관된 UI 환경을 제공하며 사용하기도 더 쉽습니다.

 

### <a name="animations-for-visual-states"></a>시각적 상태에 대한 애니메이션

컨트롤의 시각적 상태를 정의하는 데 사용되는 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)의 실행 동작은 앱에서 스토리보드를 직접 실행하는 방식과 다릅니다. XAML의 시각적 상태 정의에 적용된 대로 **Storyboard**는 포함하는 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007)의 요소이며 전체 상태는 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) API를 사용하여 제어됩니다. 포함된 모든 애니메이션은 포함하는 **VisualState**가 컨트롤에서 사용될 때 해당 애니메이션 값 및 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 속성에 따라 실행됩니다. 자세한 내용은 [시각적 상태에 대한 스토리보드](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)를 참조하세요. 시각적 상태의 경우 명확한 [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209)가 다릅니다. 시각적 상태가 다른 상태로 변경되면 이전 시각적 상태에 의해 적용된 모든 속성 변경과 해당 애니메이션은 새로운 시각적 상태가 속성에 새 애니메이션을 특별히 적용하지 않는 경우에도 취소됩니다.

### <a name="storyboard-and-eventtrigger"></a>**Storyboard** 및 **EventTrigger**

XAML에서 완전히 선언할 수 있는 애니메이션을 시작하는 방법은 한 가지입니다. 그러나 이 기술은 더 이상 광범위하게 사용되지 않습니다. 이 기술은 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) 지원 이전의 WPF 및 이전 버전 Silverlight의 레거시 구문입니다. 이 [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) 구문은 가져오기/호환성을 이유로 Windows 런타임 XAML에서도 작동하지만 [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723) 이벤트를 기반으로 하는 트리거 동작에 대해서만 작동합니다. 다른 이벤트를 트리거하려고 하면 예외가 발생하거나 컴파일되지 않습니다. 자세한 내용은 [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) 또는 [**BeginStoryboard**](https://msdn.microsoft.com/library/windows/apps/BR243053)를 참조하세요.

## <a name="animating-xaml-attached-properties"></a>XAML 연결 속성에 애니메이션 효과 주기

일반적인 시나리오는 아니지만 애니메이션 효과를 준 값을 XAML 연결 속성에 적용할 수 있습니다. 연결 속성의 정의와 작동 방식에 대한 자세한 내용은 [연결 속성 개요](https://msdn.microsoft.com/library/windows/apps/Mt185579)를 참조하세요. 연결된 속성을 대상으로 지정하려면 속성 이름을 괄호로 묶는 [속성 경로 구문](https://msdn.microsoft.com/library/windows/apps/Mt185586)이 필요합니다. 별도의 정수 값을 적용하는 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)를 사용하여 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/Hh759773) 같은 기본 제공된 연결 속성에 애니메이션 효과를 줄 수 있습니다. 그러나 Windows 런타임 XAML 구현의 기존 제한점은 사용자 지정 연결 속성에 애니메이션 효과를 줄 수 없다는 것입니다.

## <a name="more-animation-types-and-next-steps-for-learning-about-animating-your-ui"></a>추가 애니메이션 형식 및 UI에 애니메이션 효과를 주는 방법을 알아보는 다음 단계

지금까지는 두 값 사이에서 애니메이션 효과를 준 다음 애니메이션이 실행되는 동안 필요에 따라 값을 선형으로 보간하는 사용자 지정 애니메이션에 대해 살펴보았습니다. 이러한 애니메이션을 **From**/**To**/**By** 애니메이션이라고 합니다. 그러나 사용자가 시작과 끝 사이에 오는 중간 값을 선언할 수 있도록 하는 다른 애니메이션 형식이 있습니다. 이러한 애니메이션을 *키 프레임 애니메이션*이라고 합니다. **From**/**To**/**By** 애니메이션이나 키 프레임 애니메이션에서 보간 논리를 변경하는 방법도 있습니다. 이 방법에는 감속/가속 함수 적용이 포함됩니다. 이러한 개념에 대한 자세한 내용은 [키 프레임 및 감속/가속 함수 애니메이션](key-frame-and-easing-function-animations.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [속성 경로 구문](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [종속성 속성 개요](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [키 프레임 및 함수 애니메이션 감속/가속](key-frame-and-easing-function-animations.md)
* [시각적 상태에 대 한 애니메이션 스토리 보드를 만들었습니다](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)
* [컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/Mt210948)
* [**스토리 보드**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824)
 

 




