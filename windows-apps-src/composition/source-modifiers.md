---
title: SourceModifier로 당겨서 새로 고침
description: SourceModifiers를 사용하여 사용자 지정 당겨서 새로 고침 컨트롤 만들기
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 834f631cd5c4b8696e75f83f194b95f809b1cf8a
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7837753"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>SourceModifier로 당겨서 새로 고침

이 문서에서는 InteractionTracker의 SourceModifier 기능을 사용하는 방법에 대해 자세히 살펴보고, 사용자 지정 당겨서 새로 고침 컨트롤을 만들어 실제 용례를 시연하겠습니다.

## <a name="prerequisites"></a>필수 구성 요소

여기에서는 여러분이 이 문서에서 다룬 개념에 익숙하다고 가정합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [InteractionTracker를 사용한 사용자 지정 조작 경험](interaction-tracker-manipulations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>SourceModifier란 무엇이고 어떤 면에서 유용한가요?

[InertiaModifiers](inertia-modifiers.md)와 마찬가지로 SourceModifier에서는 InteractionTracker의 동작을 보다 정밀하게 제어할 수 있습니다. 하지만 InteractionTracker가 관성 상태에 진입한 이후 동작을 정의하는 InertiaModifiers와 달리, SourceModifiers는 InteractionTracker가 상호 작용 상태에 있는 동안 동작을 정의합니다. 이러한 경우, "손가락에만 의존하는" 기존 방식과 다른 경험을 원합니다.

전형적인 예는 당겨서 새로 고치는 경험입니다. 사용자가 콘텐츠를 새로 고치기 위해 목록을 당겼을 때 목록이 손가락과 같은 속도로 이동한 후 특정 거리 뒤에 멈추면 동작이 갑작스럽고 기계적으로 느껴질 것입니다. 사용자가 적극적으로 목록과 상호 작용하는 동안 저항감을 유발하면 보다 자연스러운 경험이 창출될 것입니다. 이 미묘한 차이가 목록과 상호 작용하는 전반적인 최종 사용자 경험을 보다 역동적이고 매력적으로 만듭니다. 예제 섹션에서는 이를 빌드하는 방법에 대해 자세히 설명합니다.

2개 유형의 Source Modifier가 있습니다.

- DeltaPosition – 터치 팬으로 상호 작용하는 동안 손가락의 현재 프레임 위치와 이전 프레임 위치 사이의 델타입니다. 이 Source Modifier를 사용하면 추가 처리를 위해 상호 작용의 델타 위치를 보내기 전에 수정할 수 있습니다. 이것은 Vector3 유형의 매개 변수이며 개발자는 InteractionTracker에 전달하기 전에 위치의 X 또는 Y 또는 Z 속성을 수정하기로 선택할 수 있습니다.
- DeltaScale - 터치 확대/축소 상호 작용 중에 적용된 이전 프레임 스케일과 현재 프레임 스케일 사이의 델타입니다. 이 Source Modifier를 통해 상호 작용의 확대/축소 수준을 수정할 수 있습니다. 이는 개발자가 InteractionTracker에 전달하기 전에 수정할 수 있는 float 형식 속성입니다.

InteractionTracker가 상호 작용 중 상태에 있으면 할당된 각각의 Source Modifier를 평가하고 그 중 하나가 적용되는지 판단합니다. 즉, InteractionTracker에 여러 Source Modifier를 생성하고 지정할 수 있습니다. 하지만 각각을 정의할 때 다음을 수행해야 합니다.

1. 조건 정의 – 이 특정 Source Modifier가 적용되어야 하는 시점에 대한 조건문을 정의하는 식입니다.
1. DeltaPosition/DeltaScale 정의 - 위의 정의된 조건이 충족될 때 DeltaPosition 또는 DeltaScale을 변경하는 Source Modifier 식입니다.

## <a name="example"></a>예

이제 Source Modifier를 사용하여 기존 XAML ListView 컨트롤로 사용자 지정 끌어서 새로 고침 경험을 만드는 방법을 살펴보겠습니다. 이 경험을 빌드하기 위해 XAML ListView 위에 누적될 "새로 고침 패널"로 캔버스를 사용하겠습니다.

최종 사용자 경험의 경우, 사용자가 활발하게 목록을 이동(터치를 이용)하다가 위치가 특정 지점을 넘어간 후 이동을 중지할 때 "저항"하는 효과를 만들어야 합니다.

![당겨서 새로 고침이 적용된 목록](images/animation/city-list.gif)

이 경험의 작업 코드는 [GitHub의 Window UI 개발자 랩 리포](https://github.com/Microsoft/WindowsUIDevLabs)에서 확인할 수 있습니다. 아래에 이 경험을 빌드하는 과정을 단계별로 안내합니다.
XAML 태그 코드는 다음과 같이 구성됩니다.

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

ListView(`ThumbnailList`)는 이미 스크롤이 적용된 XAML 컨트롤이므로, 맨 위 항목에 도달해 더 이상 스크롤할 수 없을 때 스크롤 동작이 상위 항목(`ContentPanel`)으로 연결되도록 해야 합니다. (ContentPanel은 Source Modifier를 적용할 위치입니다.) 이렇게 되려면 ListView 태그에서 ScrollViewer.IsVerticalScrollChainingEnabled를 **true**로 설정해야 합니다. 또한 VisualInteractionSource의 연결 모드를 **Always**로 설정해야 합니다.

_handledEventsToo_ 매개 변수를 사용하여 PointerPressedEvent 처리기를 **true**로 설정해야 합니다. 이 옵션을 사용하지 않으면 ListView 컨트롤이 해당 이벤트를 처리된 것으로 표시하므로 PointerPressedEvent가 ContentPanel에 연결되지 않고 상위 비주얼 체인으로 보내지지 않습니다.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

이제 이를 InteractionTracker와 연결할 준비가 되었습니다. InteractionTracker의 위치를 활용하게 될 Expression, InteractionTracker, VisualInteractionSource을 설정하는 것으로 시작합니다.

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

이 설정을 사용하면 새로 고침 패널이 시작 위치의 뷰포트 외부에 있고 사용자가 보는 모든 것이 listView입니다. 이동(panning)이 ContentPanel에 도달하면 PointerPressed 이벤트가 발생하고, InteractionTracker를 사용하여 조작 경험을 유도하도록 시스템에 요청합니다.

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> 처리된 이벤트를 연결할 필요가 없는 경우 PointerPressedEvent 처리기를 추가하는 작업은 속성(`PointerPressed="Window_PointerPressed"`)을 사용하여 XAML 태그를 통해 직접 수행할 수 있습니다.

다음 단계에서는 Source Modifier를 설정합니다. _저항_ 및 _중지_ 동작을 설정하기 위해 두 가지 Source Modifier를 사용하게 됩니다.

- 저항 - RefreshPanel 높이에 도달할 때까지 DeltaPosition.Y를 절반 속도로 이동시킵니다.

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- 중지 – 전체 RefreshPanel이 화면에 나타난 후 이동을 중지합니다.

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

이 다이어그램은 SourceModifiers 설정을 시각화한 것입니다.

![이동 다이어그램](images/animation/source-modifiers-diagram.png)

이제 SourceModifier를 사용하면 ListView를 아래로 이동해 맨 위 항목에 도달할 때, RefreshPanel 높이에 도달할 때까지 새로 고침 패널이 팬의 절반 속도로 아래로 당겨진 후 이동이 중지될 것입니다.

전체 샘플에서 키 프레임 애니메이션은 RefreshPanel 캔버스에서의 상호 작용 중 아이콘을 회전시키는 데 사용됩니다. 원하는 콘텐츠를 대신 사용하거나, InteractionTracker의 위치를 활용하여 해당 애니메이션을 별도로 구동할 수 있습니다.
