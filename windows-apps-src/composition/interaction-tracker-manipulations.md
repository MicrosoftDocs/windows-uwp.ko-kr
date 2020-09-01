---
title: InteractionTracker를 사용한 사용자 지정 조작
description: InteractionTracker Api를 사용 하 여 사용자 지정 조작 환경을 만듭니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 8a4b682f009a4ac1350ceee3b8c23fe5e772150d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163597"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>InteractionTracker를 사용 하는 사용자 지정 조작 환경

이 문서에서는 InteractionTracker를 사용 하 여 사용자 지정 조작 환경을 만드는 방법을 보여 줍니다.

## <a name="prerequisites"></a>필수 구성 요소

여기서는 다음 문서에서 설명 하는 개념에 대해 잘 알고 있다고 가정 합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>사용자 지정 조작 환경을 만드는 이유

대부분의 경우 미리 작성 된 조작 컨트롤을 활용 하는 것은 UI 환경을 만드는 데 충분 합니다. 그러나 공용 컨트롤과 구분 하려면 어떻게 해야 하나요? 입력으로 구동 되는 특정 환경을 만들거나 기존 조작 동작이 충분 하지 않은 UI가 있는 경우 어떻게 하나요? 여기서는 사용자 지정 환경을 만드는 방법을 제공 합니다. 이를 통해 앱 개발자와 디자이너가 더 창의적인 기능을 제공할 수 있습니다 .이를 통해 브랜드와 사용자 지정 디자인 언어를 exemplify 하는 데 더 적합 합니다. 처음부터 조작 환경을 완전히 사용자 지정 하기 위한 적절 한 구성 요소 집합에 대 한 액세스 권한이 제공 됩니다. 즉, 동작이 화면에서 손가락을 사용 하 여 응답 해야 하는 방법과 맞춤 지점과 입력 연결에 대 한 액세스를 제공 합니다.

다음은 사용자 지정 조작 환경을 만드는 몇 가지 일반적인 예입니다.

- 사용자 지정 살짝 밀기, 삭제/해제 동작 추가
- 입력 기반 효과 (패닝 시 콘텐츠가 흐리게 발생)
- 조정 된 조작 동작 (사용자 지정 ListView, ScrollViewer 등)이 있는 사용자 지정 컨트롤

![살짝 scroller 예제](images/animation/swipe-scroller.gif)

![애니메이션 예제로 가져오기](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>InteractionTracker를 사용 하는 이유

InteractionTracker는 10586 SDK 버전의 네임 스페이스에 도입 되었습니다. InteractionTracker 사용:

- **완벽 한 유연성** – 조작 환경의 모든 측면을 사용자 지정 하 고 조정할 수 있습니다. 특히, 또는 입력에 대 한 응답으로 발생 하는 정확한 동작입니다. InteractionTracker를 사용 하 여 사용자 지정 조작 환경을 빌드하는 경우 필요한 모든 노브을 사용할 수 있습니다.
- **성능 저하** – 조작 환경에서 발생 하는 문제 중 하나는 UI 스레드에 따라 달라 지는 것입니다. 이는 UI가 사용 중인 경우 조작 환경에 부정적인 영향을 미칠 수 있습니다. InteractionTracker는 60 FPS의 독립 스레드에서 작동 하는 새 애니메이션 엔진을 활용 하 여 부드러운 동작을 수행 하도록 빌드 되었습니다.

## <a name="overview-interactiontracker"></a>개요: InteractionTracker

사용자 지정 조작 환경을 만들 때 상호 작용 하는 두 가지 기본 구성 요소가 있습니다. 이에 대해서는 먼저 살펴보겠습니다.

- [InteractionTracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) – 속성이 활성 사용자 입력 또는 직접 업데이트 및 애니메이션에 의해 구동 되는 상태 시스템을 유지 관리 하는 핵심 개체입니다. 그런 다음 CompositionAnimation에 연결 하 여 사용자 지정 조작 동작을 만듭니다.
- [VisualInteractionSource](/uwp/api/windows.ui.composition.interactions.visualinteractionsource) – 입력이 InteractionTracker로 전송 되는 조건 및 위치를 정의 하는 보수 개체입니다. 적중 테스트에 사용 되는 CompositionVisual 및 기타 입력 구성 속성을 모두 정의 합니다.

상태 시스템으로 InteractionTracker의 속성은 다음 중 하나에 의해 결정 될 수 있습니다.

- 직접 사용자 상호 작용-최종 사용자가 VisualInteractionSource 적중 테스트 영역 내에서 직접 조작할 수 있습니다.
- 관성-프로그래밍 속도 또는 사용자 제스처에서 InteractionTracker의 속성은 관성 곡선에서 애니메이션 효과를 적용 합니다.
- CustomAnimation – InteractionTracker의 속성을 직접 대상으로 하는 사용자 지정 애니메이션

### <a name="interactiontracker-state-machine"></a>InteractionTracker 상태 시스템

앞에서 설명한 것 처럼 InteractionTracker은 4 가지 상태를 포함 하는 상태 시스템이 며 각각은 다른 네 가지 상태를 전환할 수 있습니다. 이러한 상태 간에 InteractionTracker 전환 하는 방법에 대 한 자세한 내용은 [InteractionTracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) 클래스 설명서를 참조 하세요.

| 시스템 상태 | Description |
|-------|-------------|
| 유휴 상태 | 활성, 주행 입력 또는 애니메이션 없음 |
| 상호 작용 중 | 활성 사용자 입력 검색 됨 |
| 관성 | 활성 입력 또는 프로그래밍 속도의 결과로 생성 되는 활성 동작 |
| CustomAnimation | 사용자 지정 애니메이션의 결과로 생성 되는 활성 동작 |

InteractionTracker의 상태가 변경 되는 각 경우에서 수신 대기할 수 있는 이벤트 또는 콜백이 생성 됩니다. 이러한 이벤트를 수신 하기 위해 [IInteractionTrackerOwner](/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) 인터페이스를 구현 하 고 CreateWithOwner 메서드를 사용 하 여 InteractionTracker 개체를 만들어야 합니다. 다음 다이어그램은 다른 이벤트가 트리거되는 경우에도 간략하게 설명 합니다.

![InteractionTracker 상태 시스템](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>VisualInteractionSource 사용

InteractionTracker가 입력으로 구동 되려면 VisualInteractionSource (VIS)를 연결 해야 합니다. VIS는 CompositionVisual를 사용 하 여 다음을 정의 하는 보수 개체로 생성 됩니다.

1. 입력을 추적할 적중 테스트 영역으로, 좌표 공간 제스처가에서 검색 됩니다.
1. 검색 되 고 라우팅되는 입력의 구성은 다음과 같습니다.
    - 검색 가능한 제스처: 위치 X 및 Y (수평 및 수직 패닝), 크기 조정 (손가락)
    - 관성
    - 레일 & 체인
    - 리디렉션 모드: InteractionTracker에 자동으로 리디렉션되는 입력 데이터

> [!NOTE]
> VisualInteractionSource는 시각적 개체의 적중 테스트 위치 및 좌표 공간을 기준으로 만들어지므로 이동 하거나 위치를 변경 하는 시각적 개체를 사용 하지 않는 것이 좋습니다.

> [!NOTE]
> 여러 적중 테스트 영역이 있는 경우 동일한 InteractionTracker를 사용 하 여 여러 VisualInteractionSource 인스턴스를 사용할 수 있습니다. 그러나 가장 일반적인 경우는 하나의 VIS 사용 하는 것입니다.

또한 VisualInteractionSource는 여러 소프트웨어나 (touch, PTP, Pen)의 입력 데이터를 InteractionTracker로 라우팅하는 경우를 관리 하는 일을 담당 합니다. 이 동작은 ManipulationRedirectionMode 속성에 의해 정의 됩니다. 기본적으로 모든 포인터 입력은 UI 스레드로 전송 되 고 전체 자릿수 터치 패드 입력은 VisualInteractionSource 및 InteractionTracker로 이동 합니다.

따라서 터치 및 펜 (작성자 업데이트)을 통해 VisualInteractionSource 및 InteractionTracker를 통해 조작을 구동 하려면 TryRedirectForManipulation 메서드를 호출 해야 합니다. XAML 앱의 아래 짧은 코드 조각에서 메서드는 가장 많은 UIElement 표에서 터치 누름 이벤트가 발생할 때 호출 됩니다.

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>ExpressionAnimations의 연결

InteractionTracker를 활용 하 여 조작 환경을 구동 하는 경우 주로 Scale 및 Position 속성과 상호 작용 합니다. 다른 CompositionObject 속성과 마찬가지로 이러한 속성은 모두 대상이 될 수 있으며 가장 일반적으로 ExpressionAnimations CompositionAnimation에서 참조할 수 있습니다.

식 내에서 InteractionTracker를 사용 하려면 아래 예제와 같이 추적기의 위치 (또는 크기 조정) 속성을 참조 합니다. 앞에서 설명한 조건으로 인해 InteractionTracker의 속성이 수정 되 면 식의 출력도 변경 됩니다.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> 식에서 InteractionTracker의 위치를 참조할 때 올바른 방향으로 이동 하려면 결과 식의 값을 부정 해야 합니다. 이는 InteractionTracker가 그래프의 원점에서 진행 되 고 있으며 원본 으로부터의 거리와 같은 "실제 세계" 좌표에서 InteractionTracker의 진행을 고려할 수 있기 때문입니다.

## <a name="get-started"></a>시작

InteractionTracker를 사용 하 여 사용자 지정 조작 환경을 만들려면 다음을 수행 합니다.

1. InteractionTracker 또는 InteractionTracker CreateWithOwner를 사용 하 여 InteractionTracker 개체를 만듭니다.
    - CreateWithOwner를 사용 하는 경우 IInteractionTrackerOwner 인터페이스를 구현 해야 합니다.
1. 새로 만든 InteractionTracker의 최소 및 최대 위치를 설정 합니다.
1. CompositionVisual를 사용 하 여 VisualInteractionSource를 만듭니다.
    - 전달 하는 시각적 개체의 크기가 0이 아닌지 확인 합니다. 그렇지 않으면 적중 테스트를 제대로 수행 하지 않습니다.
1. VisualInteractionSource의 속성을 설정 합니다.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - 레일 & 체인
1. InteractionTracker를 사용 하 여 VisualInteractionSource를 InteractionTracker에 추가 합니다.
1. 터치 및 펜 입력이 검색 되 면 TryRedirectForManipulation가 설정 됩니다.
    - XAML의 경우 일반적으로 UIElement의 PointerPressed 이벤트에서 수행 됩니다.
1. InteractionTracker의 위치를 참조 하 고 CompositionObject의 속성을 대상으로 하는 ExpressionAnimation을 만듭니다.

다음은 작업에서 #1 – 5를 표시 하는 간단한 코드 조각입니다.

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSource
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

InteractionTracker의 고급 사용에 대 한 자세한 내용은 다음 문서를 참조 하세요.

- [InertiaModifiers를 사용 하 여 스냅 점에 만들기](inertia-modifiers.md)
- [SourceModifiers를 사용 하 여 끌어오기-새로 고침](source-modifiers.md)