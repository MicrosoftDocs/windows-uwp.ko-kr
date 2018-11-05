---
author: Jwmsft
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: 변환 개요
description: UI에서 요소의 상대 좌표계를 변경하여 Windows 런타임&amp;\#160;API에서 변환을 사용하는 방법을 알아봅니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2cfc7c34363bc05e13e618deccc44fada6dec96f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6025848"
---
# <a name="transforms-overview"></a>변환 개요

UI에서 요소의 상대 좌표계를 변경하여 Windows 런타임 API에서 변형을 사용하는 방법을 알아봅니다. 이 방법을 사용하여 크기 조정, 회전 또는 x-y 공간에서 위치 변형과 같은 개별 XAML 요소의 모양을 조정할 수 있습니다.

## <a name="span-idwhatisatransformspanspan-idwhatisatransformspanspan-idwhatisatransformspanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>변형이란?

*변형*은 한 좌표 공간에서 다른 좌표 공간으로 점을 매핑 또는 변형하는 방법을 정의합니다. UI 요소에 변형을 적용하면 해당 UI 요소가 UI의 일부로 화면에 렌더링되는 방식이 변경됩니다.

변형은 변환, 회전, 크기 조정, 기울이기 등 광범위한 네 가지 분류로 나뉩니다. 그래픽 API를 사용하여 UI 요소의 모양을 변경하려는 경우 일반적으로 한 번에 하나의 작업만 정의하는 변형을 만드는 것이 가장 쉽습니다. 따라서 Windows 런타임은 이러한 각 변형 분류에 대한 개별 클래스를 정의합니다.

-   [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027): [**X**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.x.aspx) 및 [**Y**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.y)의 값을 설정하여 xy 공간에서 요소를 변환합니다.
-   [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940): [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centerx.aspx), [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centery.aspx), [**ScaleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scalex.aspx) 및 [**ScaleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scaleyproperty)의 값을 설정하여 중심점을 기준으로 변형 크기를 조정합니다.
-   [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932): [**Angle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.angle.aspx), [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centerx.aspx) 및 [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centery)의 값을 설정하여 xy 공간에서 회전합니다.
-   [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950): [**AngleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.anglex.aspx), [**AngleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.angley.aspx), [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.centerx.aspx) 및 [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centeryproperty)의 값을 설정하여 xy 공간에서 기울입니다.

이 중에서 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 및 [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940)이 UI 시나리오에 가장 많이 사용됩니다.

변형을 결합할 수 있으며, 이 기능을 지원하는 두 개의 Windows 런타임 클래스([**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105) 및 [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022))가 있습니다. **CompositeTransform**에서는 크기 조정, 기울이기, 회전, 변환 순으로 변형이 적용됩니다. 다른 순서로 변형을 적용하려는 경우 **CompositeTransform** 대신 **TransformGroup**을 사용합니다. 자세한 내용은 [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105)을 참조하세요.

## <a name="span-idtransformsandlayoutspanspan-idtransformsandlayoutspanspan-idtransformsandlayoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>변형 및 레이아웃

XAML 레이아웃에서 변형은 레이아웃 단계가 완료된 후에 적용되므로 사용 가능한 공간 계산 및 다른 레이아웃 결정은 변형이 적용되기 전에 수행되었습니다. 레이아웃이 앞에 오기 때문에 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 셀 또는 레이아웃 중 공간을 할당하는 유사한 레이아웃 컨테이너에 있는 요소를 변형하는 경우 예기치 않은 결과가 발생할 수 있습니다. 부모 컨테이너 내의 공간을 나눌 때 변형 후 치수를 계산하지 않은 영역에 그리려고 하기 때문에 변형된 요소가 잘리거나 감춰진 것처럼 나타날 수 있습니다. 변형 결과를 실험하고 일부 설정을 조정해야 할 수도 있습니다. 예를 들어 적응형 레이아웃과 배율 크기 조정을 사용하는 대신 부모가 충분한 공간을 할당하도록 레이아웃 공간에 대해 고정 픽셀 측정을 선언하거나 **Center** 속성을 변경해야 할 수 있습니다.

**마이그레이션 참고 사항:** WPF(Windows Presentation Foundation)에는 레이아웃 단계 전에 변환을 적용하는 **LayoutTransform** 속성이 있었습니다. 그러나 Windows 런타임 XAML은 **LayoutTransform** 속성을 지원하지 않습니다. Microsoft Silverlight에도 이 속성이 없었습니다.

## <a name="span-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>UI 요소에 변형 적용

개체에 변형을 적용하는 경우 일반적으로 [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 속성을 설정하기 위해 적용합니다. 이 속성을 설정해도 개체가 픽셀 단위로 변경되지는 않습니다. 이 속성은 실제로 해당 개체가 있는 로컬 좌표 공간 내에서 변형을 적용합니다. 렌더링 논리 및 작업(사후 레이아웃)은 결합된 좌표 공간을 렌더링하여 개체의 모양 및 잠재적으로 레이아웃 위치([**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)이 적용된 경우)가 변경된 것처럼 보이게 합니다.

기본적으로 각 렌더링 변형은 대상 개체의 로컬 좌표계 원점, 즉 (0, 0)을 중심으로 합니다. 유일한 예외는 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)으로, 변환 효과가 중심 위치에 관계없이 동일하기 때문에 설정할 중심 속성이 없습니다. 그러나 다른 변형에는 각각 **CenterX** 및 **CenterY** 값을 설정하는 속성이 있습니다.

[**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)과 함께 변형을 사용하는 경우 변형 동작에 영향을 주는 다른 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 속성인 [**RenderTransformOrigin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransformorigin.aspx)이 있습니다. **RenderTransformOrigin**에서 선언하는 사항은 요소의 기본(0, 0) 지점 또는 해당 요소의 상대 좌표 공간 내 다른 원점에 전체 변형을 적용할지 여부입니다. 일반적인 요소의 경우 (0, 0)은 왼쪽 맨 위에 변형을 적용합니다. 원하는 효과에 따라 변형의 **CenterX** 및 **CenterY** 값을 조정하는 대신 **RenderTransformOrigin**을 변경할 수도 있습니다. **RenderTransformOrigin** 및 **CenterX** / **CenterY** 값을 모두 적용하는 경우 특히 값에 애니메이션 효과를 적용하면 결과가 혼란스러울 수 있습니다.

적중 횟수 테스트를 위해 변형이 적용된 개체는 x-y 공간의 시각적 모양과 일치하는 예상 방식으로 계속 입력에 응답합니다. 예를 들어 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)을 사용하여 UI에서 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 수평으로 400픽셀만큼 이동한 경우 **Rectangle**이 시각적으로 표시되는 지점을 사용자가 누르면 **Rectangle**이 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) 이벤트에 응답합니다. 사용자가 변환 전에 **Rectangle**이 있던 영역을 누르는 경우 false 이벤트가 표시되지 않습니다. 적중 횟수 테스트에 영향을 주는 모든 Z-인덱스 고려 사항의 경우 변형을 적용해도 차이가 없습니다. x-y 공간의 한 지점에 대한 입력 이벤트를 처리하는 요소를 제어하는 Z-인덱스는 컨테이너에서 선언된 자식 순서를 사용하여 평가됩니다. 해당 순서는 일반적으로 XAML에서 요소를 선언한 순서와 같습니다. 하지만 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267) 개체의 자식 요소는 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) 연결된 속성을 자식 요소에 적용하여 순서를 조정할 수 있습니다.

## <a name="span-idothertransformpropertiesspanspan-idothertransformpropertiesspanspan-idothertransformpropertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>기타 변형 속성

-   [**Brush.Transform**](https://msdn.microsoft.com/library/windows/apps/BR228082), [**Brush.RelativeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228080): [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)에서 **Brush**가 적용된 영역 내의 좌표 공간을 사용하여 전경, 배경 등의 시각적 속성을 설정하는 방법에 영향을 줍니다. 이러한 변형은 일반적으로 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)를 사용하여 단색을 설정하는 대부분의 일반 브러시와 관련이 없지만, [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 또는 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108)를 사용하여 영역을 그릴 때 유용할 수 있습니다.
-   [**Geometry.Transform**](https://msdn.microsoft.com/library/windows/apps/BR210066): 이 속성을 사용하여 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/BR243356) 속성 값에 해당 형상을 사용하기 전에 형상에 변형을 적용할 수 있습니다.

## <a name="span-idanimatingatransformspanspan-idanimatingatransformspanspan-idanimatingatransformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>변형에 애니메이션 효과 적용

[**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)개체에 애니메이션 효과를 줄 수 있습니다. **Transform**에 애니메이션 효과를 주려면 애니메이션 효과를 줄 속성에 호환되는 형식의 애니메이션을 적용합니다. 모든 변형 속성은 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 형식이므로, 일반적으로 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 또는 [**DoubleAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) 개체를 사용하여 애니메이션을 정의합니다. [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 값에 사용되는 변형에 영향을 주는 애니메이션은 기간이 0이 아닌 경우에도 종속 애니메이션으로 간주되지 않습니다. 종속 애니메이션에 대한 자세한 내용은 [스토리보드 애니메이션](/windows/uwp/design/motion/storyboarded-animations)을 참조하세요.

[**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)을 적용하는 대신 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706)의 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 및 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)에 애니메이션 효과를 주는 경우와 같이 속성에 애니메이션 효과를 주어 순 시각적 모양 측면에서 변형과 유사한 효과를 생성하는 경우 이러한 애니메이션은 거의 대부분 종속 애니메이션으로 처리됩니다. 애니메이션을 사용하도록 설정해야 하며, 특히 개체에 애니메이션 효과를 주는 동안 사용자 조작을 지원하려는 경우 해당 애니메이션으로 인해 성능 문제가 발생할 수 있습니다. 이런 이유로 애니메이션이 종속 애니메이션으로 처리되는 다른 속성에 애니메이션 효과를 주는 대신 변형을 사용하고 변형에 애니메이션 효과를 주는 것이 좋습니다.

변형 대상을 지정하려면 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 값으로 기존 [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)이 있어야 합니다. 일반적으로 초기 XAML에 적절한 변형 형식의 요소를 배치하며, 해당 변형의 속성을 설정하지 않는 경우도 있습니다.

일반적으로 간접 대상 지정 기술을 사용하여 변형 속성에 애니메이션을 적용합니다. 간접 대상 지정 구문에 대한 자세한 내용은 [스토리보드 애니메이션](/windows/uwp/design/motion/storyboarded-animations) 및 [속성 경로 구문](https://msdn.microsoft.com/library/windows/apps/Mt185586)을 참조하세요.

컨트롤의 기본 스타일은 때때로 시각적 상태 동작의 일부로 변형 애니메이션을 정의합니다. 예를 들어 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538)의 시각적 상태는 애니메이션 효과를 준 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 값을 사용하여 링의 점을 "회전"합니다.

변형에 애니메이션 효과를 주는 방법의 간단한 예제는 다음과 같습니다. 이 경우 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932)의 [**Angle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rotatetransform.angle.aspx)에 애니메이션 효과를 주어 시각적 중심을 기준으로 제자리에서 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 회전합니다. 이 예제에서는 **RotateTransform**에 이름을 지정하기 때문에 간접 애니메이션 대상 지정이 필요하지 않지만, 변형 이름을 지정하지 않고 변형이 적용되는 대상의 이름을 지정하여 `(UIElement.RenderTransform).(RotateTransform.Angle)` 등의 간접 대상 지정을 사용할 수도 있습니다.

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <a name="span-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>런타임 시 참조의 좌표 프레임 고려

[**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)에는 두 UI 요소의 참조 좌표 프레임을 상호 연결하는 [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)을 생성할 수 있는 [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.transformtovisual.aspx)이라는 메서드가 있습니다. 루트 시각적 모양을 첫 번째 매개 변수로 전달하는 경우 이 메서드를 사용하여 요소를 앱의 기본 참조 좌표 프레임과 비교할 수 있습니다. 이 기능은 다른 요소에서 입력 이벤트를 캡처한 경우 또는 실제로 레이아웃 단계를 요청하지 않고 레이아웃 동작을 예측하려는 경우에 유용할 수 있습니다.

포인터 이벤트에서 얻은 이벤트 데이터를 통해 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/BR212141) 메서드에 액세스할 수 있으며, 여기서 *relativeTo* 매개 변수를 지정하여 참조 좌표 프레임을 앱 기본값이 아닌 특정 요소로 변경할 수 있습니다. 이 접근 방식에서는 반환된 [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/BR242038) 개체를 만들 때 변환 변형이 내부적으로 적용되고 x-y 좌표 데이터가 변형됩니다.

## <a name="span-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>수학적으로 변형 설명

변형은 변형 행렬 측면에서 설명할 수 있습니다. 3x3 행렬은 2D x-y 평면의 변형을 설명하는 데 사용됩니다. 행렬 유사 변환을 곱하면 회전, 기울이기 등 원하는 개수의 선형 변형 후 변환을 구성할 수 있습니다. 행렬 유사 변환의 최종 열이 (0, 0, 1)과 같으므로 수학적 설명에서 처음 두 열의 멤버만 지정해야 합니다.

변형의 수학적 설명은 수학적 배경 지식이 있거나 행렬을 사용하여 좌표 공간 변형을 설명하는 그래픽 프로그래밍 기술에 익숙한 경우 유용할 수 있습니다. 3x3 행렬 측면에서 변형을 직접 표현할 수 있는 [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) 파생 클래스인 [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137)이 있습니다. **MatrixTransform**에는[**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847), [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853), [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851), [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849), [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) 및 [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816)의 6개 속성이 있는 구조체를 포함하는 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.matrixtransform.matrix.aspx) 속성이 있습니다. 각 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) 속성은 **Double** 값을 사용하며 행렬 유사 변환의 6개 관련 값(1열, 2열)에 해당합니다.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847)         | [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851)         | 0   |
| [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853)         | [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849)         | 0   |
| [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) | [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816) | 1   |

 

[**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940), [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 또는 [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950) 개체를 사용하여 설명할 수 있는 변형은 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) 값을 가진 [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137)으로 똑같이 설명할 수 있습니다. 그러나 일반적으로 **TranslateTransform** 및 기타 변형만 사용하는데, **Matrix**에서 벡터 구성 요소를 설정하는 것보다 이러한 변형 클래스의 속성을 개념화하는 것이 더 쉽기 때문입니다. 또한 변형의 개별 속성에 애니메이션 효과를 주는 것이 더 쉬운 반면, **Matrix**는 실제로 구조체이고 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356)가 아니므로 애니메이션 효과를 준 개별 값을 지원할 수 없습니다.

변형 작업을 적용할 수 있는 일부 XAML 디자인 도구는 결과를 [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137)으로 직렬화합니다. 이 경우 XAML에서 직접 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) 값을 조작하는 대신 동일한 디자인 도구를 다시 사용하여 변형 효과를 변경하고 XAML을 다시 직렬화하는 것이 좋습니다.

## <a name="span-id3-dtransformsspanspan-id3-dtransformsspanspan-id3-dtransformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>3D 변형

Windows 10에서 XAML에는 UI로 3D 효과를 만드는 데 사용할 수 있는 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx) 속성이 새로 도입되었습니다. 이렇게 하려면 [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.perspectivetransform3d.aspx)를 사용하여 공유 3D 원근 또는 "카메라"를 장면에 추가한 다음 [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105)을 사용하듯이 [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.compositetransform3d.aspx)를 사용하여 3D 공간에서 요소를 변환합니다. 3D 변형을 구현하는 방법에 대한 자세한 내용은 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)를 참조하세요.

 단일 개체에만 적용되는 더 간단한 3D 효과의 경우 [**UIElement.Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection) 속성을 사용할 수 있습니다. [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/br210192)을 이 속성의 값으로 사용하는 것은 고정된 원근 변경과 하나 이상의 3D 변형을 요소에 적용하는 것과 동일합니다. 이 변형 유형은 [XAML UI에 대한 3D 원근감 효과](3-d-perspective-effects.md)에서 자세히 설명합니다.

## <a name="span-idrelatedtopicsspanrelated-topics"></a><span id="related_topics"></span>관련 항목

* [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)
* [XAML UI에 대한 3D 원근감 효과](3-d-perspective-effects.md)
* [**변형**](https://msdn.microsoft.com/library/windows/apps/BR243006)
 

 





