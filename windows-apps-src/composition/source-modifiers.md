---
title: 소스 한정자를 사용 하 여 끌어오기-새로 고침
description: InteractionTracker의 Sourcemodifier 대 기능을 사용 하 여 사용자 지정 끌어오기-새로 고침 컨트롤을 만드는 방법에 대해 알아봅니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: b20b4b22d1de2252864287b97bedc4a1fc176602
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053963"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>소스 한정자를 사용 하 여 끌어오기-새로 고침

이 문서에서는 InteractionTracker의 Sourcemodifier 대 기능을 사용 하는 방법에 대해 자세히 알아보고 사용자 지정 끌어오기-새로 고침 컨트롤을 만들어 사용을 보여 줍니다.

## <a name="prerequisites"></a>필수 구성 요소

여기서는 다음 문서에서 설명 하는 개념에 대해 잘 알고 있다고 가정 합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [InteractionTracker를 사용 하는 사용자 지정 조작 환경](interaction-tracker-manipulations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>Sourcemodifier 대 무엇 이며 유용한 이유는 무엇 인가요?

[InertiaModifiers](inertia-modifiers.md)와 마찬가지로 SourceModifiers는 InteractionTracker의 동작에 대 한 미세 제어를 제공 합니다. 하지만 InteractionTracker가 관성로 전환 된 후에 동작을 정의 하는 InertiaModifiers와는 달리, SourceModifiers는 계속 상호 작용 중인 상태에서 동작을 정의 합니다. 이러한 경우 기존 "손가락에 고정"과는 다른 환경을 원합니다.

이에 대 한 일반적인 예는 끌어오기-새로 고침 환경입니다. 사용자가 목록에서 콘텐츠를 새로 고치는 경우 목록에서 손가락의 속도와 동일한 속도로 계획 특정 거리 후에 중지 되는 경우에는 동작이 급격 하 고 기계적으로 판단 됩니다. 사용자가 목록과 적극적으로 상호 작용 하는 동안 저항 느낌을 도입 하는 것이 더 자연 스러운 환경입니다. 이 작은 nuance는 전체 최종 사용자가 목록과 상호 작용 하는 데 도움이 됩니다. 예제 섹션에서는이를 빌드하는 방법에 대해 자세히 설명 합니다.

원본 한정자에는 두 가지 유형이 있습니다.

- DeltaPosition – 터치 팬 상호 작용 중에 현재 프레임 위치와 손가락의 이전 프레임 위치 사이의 델타입니다. 이 소스 한정자를 사용 하면 추가 처리를 위해 보내기 전에 상호 작용의 델타 위치를 수정할 수 있습니다. 이 매개 변수는 Vector3 형식 매개 변수 이며 개발자는 InteractionTracker에 전달 하기 전에 위치의 X 또는 Y 또는 Z 특성을 수정 하도록 선택할 수 있습니다.
- DeltaScale-현재 프레임 비율과 터치 확대/축소 상호 작용 중에 적용 된 이전 프레임 눈금 사이의 델타입니다. 이 소스 한정자를 사용 하 여 상호 작용의 확대/축소 수준을 수정할 수 있습니다. 개발자가 InteractionTracker에 전달 하기 전에 수정할 수 있는 float 형식 특성입니다.

InteractionTracker가 상호 작용 중인 상태 이면 해당 클래스에 할당 된 각 소스 한정자를 계산 하 고 적용 되는지 확인 합니다. 즉, 여러 소스 한정자를 만들어 InteractionTracker에 할당할 수 있습니다. 하지만 각를 정의할 때는 다음을 수행 해야 합니다.

1. 조건 정의-이 특정 소스 한정자를 적용 해야 하는 경우 조건문을 정의 하는 식입니다.
1. DeltaPosition/DeltaScale – 위의 정의 된 조건이 충족 될 때 DeltaPosition 또는 DeltaScale를 변경 하는 소스 한정자 식을 정의 합니다.

## <a name="example"></a>예제

이제 소스 한정자를 사용 하 여 기존 XAML ListView 컨트롤을 사용 하 여 사용자 지정 끌어오기-새로 고침 환경을 만드는 방법을 살펴보겠습니다. 이 환경을 빌드하기 위해 XAML ListView 위에 누적 되는 "새로 고침 패널"로 캔버스를 사용 하 게 됩니다.

최종 사용자 환경에서는 사용자가 목록을 적극적으로 이동 하 고 위치를 특정 지점으로 이동 하 고 나 서 이동을 중지 하는 것 처럼 "저항" 효과를 만들려고 합니다.

![끌어오기-새로 고침을 사용 하는 목록](images/animation/city-list.gif)

이 환경에 대 한 작업 코드는 [GitHub의 창 UI 개발자 리포지토리](https://github.com/microsoft/WindowsCompositionSamples)에서 찾을 수 있습니다. 다음은 해당 환경을 빌드하는 단계별 연습입니다.
XAML 태그 코드에서 다음을 수행 합니다.

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

ListView ()는 `ThumbnailList` 이미 스크롤되는 XAML 컨트롤이 기 때문에 `ContentPanel` 최상위 항목에 도달 하 여 더 이상 스크롤할 수 없으면 부모 항목 ()에 연결 해야 합니다. (ContentPanel은 소스 한정자를 적용 하는 위치입니다.) 이를 수행 하려면 ListView 태그에서 ScrollViewer을 **true** 로 설정 해야 합니다. VisualInteractionSource에 대 한 연결 모드를 **항상**로 설정 해야 합니다.

_HandledEventsToo_ 매개 변수를 **true**로 설정 하 여 Pointer보도 sedevent 처리기를 설정 해야 합니다. 이 옵션을 사용 하지 않으면, ListView 컨트롤이 해당 이벤트를 처리 된 것으로 표시 하 고 시각적 체인으로 전송 되지 않으므로 Pointer보도 Sedevent가 ContentPanel에 연결 되지 않습니다.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

이제 InteractionTracker와 연결할 준비가 되었습니다. InteractionTracker, VisualInteractionSource 및 InteractionTracker의 위치를 활용 하는 식을 설정 하 여 시작 합니다.

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

이 설정을 사용 하는 경우 새로 고침 패널은 뷰포트의 시작 위치에서 제외 되 고 사용자에 게 표시 되는 모든 사용자가 listView로 표시 됩니다 .이 이벤트는 시스템에서 InteractionTracker를 사용 하 여 조작 환경을 구동 하도록 요청 하는 경우에 발생 합니다.

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
> 처리 된 이벤트 체인이 필요 하지 않은 경우 특성 ()을 사용 하 여 XAML 태그를 통해 Pointer보도 Sedevent 처리기를 직접 추가할 수 있습니다 `PointerPressed="Window_PointerPressed"` .

다음 단계는 소스 한정자를 설정 하는 것입니다. 이 동작을 얻으려면 2 개의 소스 한정자를 사용 합니다. _저항_ 및 _중지_.

- 저항 – RefreshPanel의 높이에 도달할 때까지 DeltaPosition를 속도의 절반으로 이동 합니다.

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

- 중지 – 전체 RefreshPanel이 화면에 있는 후 이동을 중지 합니다.

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

이 다이어그램은 SourceModifiers 설정의 시각화를 제공 합니다.

![패닝 다이어그램](images/animation/source-modifiers-diagram.png)

이제 SourceModifiers을 사용 하 여 ListView를 이동 하 고 최상위 항목에 도달할 때 새로 고침 패널을 다시 시작 하는 것을 알 수 있습니다.

전체 샘플에서 키 프레임 애니메이션은 RefreshPanel 캔버스에서 상호 작용 하는 동안 아이콘을 회전 하는 데 사용 됩니다. 모든 콘텐츠를 해당 위치에 사용 하거나 InteractionTracker의 위치를 활용 하 여 해당 애니메이션을 따로 구동 할 수 있습니다.
