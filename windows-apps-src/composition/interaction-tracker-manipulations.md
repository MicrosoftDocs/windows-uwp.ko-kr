---
author: jwmsft
title: InteractionTracker를 사용한 사용자 지정 조작
description: InteractionTracker API를 사용하여 사용자 지정 조작 경험을 만듭니다.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 0a991d692b4ba4c7a221932218a7d25e48fe16ca
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5882004"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>InteractionTracker를 사용한 사용자 지정 조작 경험

이 문서에서는 InteractionTracker를 사용하여 사용자 지정 조작 경험을 만드는 방법을 설명합니다.

## <a name="prerequisites"></a>필수 구성 요소

여기에서는 여러분이 이 문서에서 다룬 개념에 익숙하다고 가정합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>사용자 지정 조작 경험을 만드는 이유는 무엇일까요?

대부분의 경우, 이미 빌드된 조작 컨트롤만으로도 UI 경험을 충분히 만들 수 있습니다. 그러나 일반적인 컨트롤과 차별화하고 싶을 경우 어떻게 해야 할까요? 입력에 기반한 특수한 경험이나 기존의 조작 동작으로는 충분하지 않은 UI를 만들고 싶다면 어떨까요? 이런 경우에 사용자 지정 경험을 만들면 좋습니다. 그러면 앱 개발자와 디자이너가 더욱 폭넓게 창의성을 발휘하여 브랜딩 및 사용자 지정 디자인 언어를 보다 충실하게 구현한 생동감 있는 경험을 구축할 수 있습니다. 처음부터 적절한 구성 요소 집합에 액세스하여 다양한 조작 경험(화면에 손가락을 댔다 뗄 때 동작이 반응하는 방식, 끌기 지점 및 입력 체인 등)을 완전히 사용자 지정할 수 있습니다.

아래 예는 사용자 지정 조작 경험을 만드는 것이 유용한 경우입니다.

- 사용자 지정 살짝 밀기, 삭제/해제 동작 추가
- 입력 기반 효과(이동 시 콘텐츠를 흐리게 표시)
- 맞춤형 조작 동작이 있는 사용자 지정 컨트롤(사용자 지정 ListView, ScrollViewer 등)

![살짝 밀기 스크롤러 예제](images/animation/swipe-scroller.gif)

![당겨서 애니메이션 예제](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>왜 InteractionTracker를 사용할까요?

InteractionTracker는 Windows.UI.Composition.Interactions 네임스페이스 10586 SDK 버전에 처음 도입되었습니다. InteractionTracker의 기능:

- **탁월한 유연성** – 개발자는 조작 경험의 모든 측면, 특히 입력 도중 또는 입력에 대한 반응으로 발생하는 정확한 동작을 사용자 지정하기를 원합니다. InteractionTracker로 사용자 지정 조작 경험을 빌드하면 필요한 모든 노브를 자유자재로 활용할 수 있습니다.
- **매끄러운 조작 성능** – 조작 경험의 문제점 중 하나는 성능이 UI 스레드에 종속된다는 것입니다. 이로 인해 UI가 사용되는 동안 조작 경험에 부정적인 영향을 줄 수 있습니다. InteractionTracker는 독립적인 스레드에서 60FPS로 작동하는 새로운 애니메이션 엔진을 사용하도록 만들어졌기 때문에 매끄러운 동작을 구현합니다.

## <a name="overview-interactiontracker"></a>개요: InteractionTracker

사용자 지정 조작 경험을 만들 때 개발자가 상호 작용하는 두 가지 기본 구성 요소가 있습니다. 이 두 요소를 먼저 설명하겠습니다.

- [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) – 핵심 개체로서, 활성 사용자 입력 또는 직접 업데이트 및 애니메이션에 의해 속성이 구동되는 상태 시스템을 유지 관리합니다. 그런 다음 CompositionAnimation에 연결되어 사용자 지정 조작 동작을 만듭니다.
- [VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource) – 언제, 어떤 조건 하에서 InteractionTracker에 입력이 전달되는지 정의하는 보완 개체입니다. 적중 횟수 테스트에 사용되는 CompositionVisual과 기타 입력 구성 속성을 모두 정의합니다.

상태 시스템으로서 InteractionTracker의 속성은 다음 중 하나에 의해 구동될 수 있습니다.

- 직접 사용자 상호 작용 - 최종 사용자가 VisualInteractionSource 적중 횟수 테스트 영역 내에서 직접 조작합니다.
- 관성 – 프로그래밍된 속도 또는 사용자 제스처에 의해, InteractionTracker의 속성은 관성 곡선 아래에 애니메이션으로 표현됩니다.
- CustomAnimation – InteractionTracker의 속성을 직접 겨냥한 사용자 지정 애니메이션입니다.

### <a name="interactiontracker-state-machine"></a>InteractionTracker 상태 시스템

앞서 언급 했 듯이 InteractionTracker는 4 단계의 – 다른 fourstates 중 하나로 전환할 수 있으며 각으로 상태 시스템입니다. (InteractionTracker가 이러한 상태 사이에서 전환하는 방식에 대한 자세한 내용은 [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) 클래스 설명서를 참조하세요.)

| 상태 | 설명 |
|-------|-------------|
| 유휴 상태 | 구동해야 하는 활성 애니메이션 없음 |
| 상호 작용 중 | 활성 사용자 입력이 검색됨 |
| 관성 | 활성 입력 또는 프로그래밍된 속도에서 비롯된 활성 동작 |
| CustomAnimation | 사용자 지정 애니메이션에서 비롯된 활성 동작 |

InteractionTracker의 상태가 변화하는 각각의 경우에 수신 대기할 수 있는 이벤트(또는 콜백)가 생성됩니다. 이러한 이벤트를 수신 대기하려면 [IInteractionTrackerOwner](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) 인터페이스를 구현하고 CreateWithOwner 메서드로 해당 InteractionTracker 개체를 만들어야 합니다. 다음 다이어그램은 다양한 이벤트가 트리거되는 경우도 간략하게 보여줍니다.

![InteractionTracker 상태 시스템](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>VisualInteractionSource 사용

InteractionTracker가 입력으로 구동되도록 하려면 VisualInteractionSource(VIS)를 연결해야 합니다. VIS는 다음을 정의하기 위해 CompositionVisual을 사용하여 보완 개체로 생성됩니다.

1. 입력이 추적되고 좌표 공간 제스처가 감지될 적중 횟수 테스트 영역
1. 감지되고 라우팅될 입력 구성의 몇 가지 예는 다음과 같습니다.
    - 감지되는 제스처: 위치 X 및 Y(수평 및 수직 이동), 크기 조정(손가락 모으기)
    - 관성
    - 레일 및 체인
    - 리디렉션 모드: 자동으로 InteractionTracker로 리디렉션되는 입력 데이터

> [!NOTE]
> VisualInteractionSource는 적중 횟수 테스트 위치 및 시각적 개체의 좌표 공간을 기반으로 만들어지므로, 이동하거나 위치가 변경될 시각적 개체는 사용하지 않는 것이 좋습니다.

> [!NOTE]
> 적중 횟수 테스트 영역이 여럿인 경우, 동일한 InteractionTracker로 여러 VisualInteractionSource 인스턴스를 사용할 수 있습니다. 하지만 가장 일반적인 사례는 하나의 VIS만 사용하는 것입니다.

VisualInteractionSource는 다양한 형식(터치, PTP, 펜)의 입력 데이터가 InteractionTracker로 라우팅되는 시점도 관리합니다. 이 동작은 ManipulationRedirectionMode 속성으로 정의됩니다. 기본적으로 모든 포인터 입력은 UI 스레드로 보내지고 정밀 터치 패드 입력은 VisualInteractionSource 및 InteractionTracker로 전달됩니다.

따라서 VisualInteractionSource 및 InteractionTracker를 통해 터치 및 펜(크리에이터스 업데이트)으로 조작을 구동하도록 하려면 VisualInteractionSource.TryRedirectForManipulation 메서드를 호출해야 합니다. 아래 XAML 앱의 짧은 조각에서 맨 위에 있는 UIElement Grid에서 터치를 누르는 이벤트가 발생하면 이 메서드가 호출됩니다.

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>ExpressionAnimations와 병행

InteractionTracker를 활용하여 조작 경험을 구동하는 경우, 주로 Scale 및 Position 속성과 상호 작용합니다. 다른 CompositionObject 속성과 마찬가지로, 이러한 속성은 CompositionAnimation(가장 일반적으로 ExpressionAnimations)에서 대상이 되고 참조될 수 있습니다.

식 내에서 InteractionTracker를 사용하려면 아래 예와 같이 추적기의 Position(또는 Scale) 속성을 참조하세요. 이전에 설명했던 조건 중 하나로 인해 InteractionTracker의 속성이 수정되면 식의 출력도 변경됩니다.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> 식에서 InteractionTracker의 위치를 참조하는 경우, 올바른 방향으로 이동하도록 결과 식의 값을 무효화해야 합니다. 그래프상에서 InteractionTracker가 원점에서부터 벗어나 "실제" 좌표에서 InteractionTracker의 진행(예: 원점으로부터의 거리)에 대해 생각할 수 있기 때문입니다.

## <a name="get-started"></a>시작하기

InteractionTracker를 사용하여 사용자 지정 조작 경험을 만들기 시작하려면:

1. InteractionTracker.Create 또는 InteractionTracker.CreateWithOwner를 사용하여 InteractionTracker 개체를 만듭니다.
    - (CreateWithOwner를 사용하는 경우 IInteractionTrackerOwner 인터페이스를 구현해야 합니다.)
1. 새로 만든 InteractionTracker의 최소 및 최대 위치를 설정합니다.
1. CompositionVisual로 VisualInteractionSource를 만듭니다.
    - 전달하는 시각적 개체 크기는 0이 아니어야 합니다. 그렇지 않으면 적중 횟수 테스트를 통과할 수 없습니다.
1. VisualInteractionSource의 속성을 설정합니다.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - 레일 및 체인
1. InteractionTracker.InteractionSources.Add를 사용해 InteractionTracker에 VisualInteractionSource를 추가합니다.
1. 터치 및 펜 입력이 감지되는 시점에 대한 TryRedirectForManipulation을 설정합니다.
    - XAML의 경우 일반적으로 UIElement의 PointerPressed 이벤트를 기반으로 수행됩니다.
1. InteractionTracker의 위치를 참조하고 CompositionObject의 속성을 대상으로 하는 ExpressionAnimation을 만듭니다.

다음은 #1 – 5 조치를 보여주는 짧은 코드 조각입니다.

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSourc
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

InteractionTracker의 고급 사용법은 다음 문서를 참조하세요.

- [InertiaModifier로 끌기 지점 만들기](inertia-modifiers.md)
- [SourceModifier로 당겨서 새로 고침](source-modifiers.md)
