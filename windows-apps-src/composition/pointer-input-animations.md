---
author: jwmsft
title: 포인터 기반 애니메이션
description: 포인터의 위치를 사용하여 "커서만 따르는" 동적 경험을 창출하는 방법에 대해서 알아봅니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: b69899761e1c4a139fd2b15d6810440df5192487
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6647452"
---
# <a name="pointer-based-animations"></a>포인터 기반 애니메이션

이 문서에서는 포인터의 위치를 사용하여 "커서만 따르는" 동적 경험을 창출하는 방법을 설명합니다.

## <a name="prerequisites"></a>필수 구성 요소

여기에서는 여러분이 이 문서에서 다룬 개념에 익숙하다고 가정합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>포인터 위치 기반 경험을 생성하는 이유

Fluent 디자인 언어에서 터치가 UI와 상호 작용할 수 있는 유일한 방법은 아닙니다. UWP는 다양한 장치 폼 팩터에 걸쳐 있기 때문에 최종 사용자는 마우스, 펜 등의 다른 입력 형식으로 앱과 상호 작용합니다. 이처럼 다른 입력 형식의 위치 데이터를 사용하면 최종 사용자가 앱과 더욱 긴밀하게 연결되었다고 느낄 수 있습니다.

포인터 위치 기반 경험을 통해 화면상에서 포인터 입력 형식의 위치를 활용하여 앱에 대한 추가 동작과 UI 경험을 만들 수 있습니다. 이러한 경험은 흔히 최종 사용자에게 UI의 동작과 구조에 대한 추가 컨텍스트와 피드백을 제공할 수 있습니다. 이 경험은 더 이상 일방적인 스트림이 아니라 양방향 스트림이 되기 시작합니다. 따라서 최종 사용자가 원하는 입력 형식을 이용해 입력하면 앱 UI가 다시 응답할 수 있습니다.

예를 들면 다음과 같습니다.

- 커서를 따르도록 스포트라이트의 위치에 애니메이션 효과를 적용

    ![포인터 추천 예제](images/animation/spotlight-reveal.gif)

- 포인터의 위치에 기반한 이미지의 회전

    ![포인터 회전 예제](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>PointerPositionPropertySet 사용

PointerPositionPropertySet를 사용하여 이러한 경험을 만들 수 있습니다. 이 PropertySet은 UIElement가 포인터의 위치를 유지하는 한편, 적중 횟수 테스트를 통과하도록 만들어집니다. 위치 값은 UIElement의 좌표 공간을 기준으로 합니다(<0,0>의 위치는 UIElement의 왼쪽 위 모서리입니다). 그런 다음 애니메이션에서 이 속성 집합을 활용하여 다른 속성의 동작을 구동할 수 있습니다.

각기 다른 포인터 입력 형식별로 수많은 입력 상태가 있으며 가리키기, 누름, 누르고 이동 등, 위치가 변화하는 공간에서 입력이 이루어질 수 있습니다. PointerPositionPropertySet은 마우스와 펜에 대해 가리키기, 누름, 누르고 이동 상태에서만 포인터의 위치를 유지합니다.

시작하기 위한 일반 절차:

1. 포인터의 위치를 추적하고자 하는 UIElement를 결정합니다.
1. ElementCompositionPreview를 통해 PointerPositionPropertySet에 액세스합니다.
    - ElementCompositionPreview.GetPointerPositionPropertySet 메서드로 UIElement를 전달합니다.
1. PropertySet에 위치 속성을 참조하는 ExpressionAnimation을 만듭니다.
    - 반드시 참조 매개 변수를 설정해야 합니다!
1. ExpressionAnimation으로 CompositionObject의 속성을 대상으로 지정합니다.

> [!NOTE]
> GetPointerPositionPropertySet 메서드에서 반환된 PropertySet을 클래스 변수에 할당하는 것이 좋습니다. 이렇게 하면 가비지 수집에 의해 속성 집합이 정리되지 않으므로 참조되는 ExpressionAnimation에 아무런 영향을 미치지 않습니다. ExpressionAnimations는 수식에 사용된 개체에 대한 강력한 참조를 유지하지 않습니다.

## <a name="example"></a>예

마우스와 펜 입력 형식의 가리키기 위치를 활용하여 이미지를 동적으로 회전시키는 경우의 예를 살펴보겠습니다.

![포인터 회전 예제](images/animation/pointer-rotate.gif)

이 이미지는 UIElement이므로 먼저 PointerPositionPropertySet에 대한 참조를 가져옵니다.

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

이 샘플에는 두 개의 식이 사용되고 있습니다.

- 하나는 포인터가 이미지의 중심으로부터 떨어진 거리를 기준으로 이미지가 회전하는 식입니다. 멀리 떨어질수록 더 많이 회전합니다.
- 하나는 포인터의 위치를 기준으로 회전 축이 변경되는 식입니다. 회전 축이 위치 벡터에 수직하도록 설정하려는 경우를 가정해봅시다.

각각 RotationAngle 속성을 대상으로 하는 식과 RotationAxis 속성을 대상으로 하는 식, 총 두 개의 식을 정의하겠습니다. 다른 PropertySet과 마찬가지로 PointerPositionPropertySet을 참조합니다.

이 예에서는 ExpressionBuilder 클래스를 사용해 식을 구축하고 있습니다.

```csharp
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```