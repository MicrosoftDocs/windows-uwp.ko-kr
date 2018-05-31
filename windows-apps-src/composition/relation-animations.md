---
author: jwmsft
title: 관계 기반 애니메이션
description: 다른 개체 속성을 기반으로 동작을 만듭니다.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: ceca1705938df29bd4fbb857fbc78649fec57a94
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2017
ms.locfileid: "1393552"
---
# <a name="relation-based-animations"></a>관계 기반 애니메이션

이 문서에서는 Composition ExpressionAnimations를 사용하여 관계 기반 애니메이션을 만드는 방법을 간략하게 안내합니다.

## <a name="dynamic-relation-based-experiences"></a>동적 관계 기반 경험

앱에서 동작 경험을 빌드할 때 동작이 시간을 기반으로 하지 않고 다른 개체의 속성에 종속되는 경우가 있습니다. KeyFrameAnimations는 이러한 유형의 동작 경험을 쉽게 표현할 수 없습니다. 이러한 특정 인스턴스에서 동작은 더 이상 개별적이고 사전 정의되어야 할 필요가 없습니다. 대신, 다른 개체 속성과의 관계에 따라 동작이 동적으로 적응할 수 있습니다. 예를 들어, 개체의 수평 위치를 기준으로 개체 불투명도에 애니메이션 효과를 줄 수 있습니다. 다른 예로 고정 헤더와 시차 등의 동작 경험을 들 수 있습니다.

이러한 유형의 동작 경험을 통해 개별적이고 독립적이기보다는 연결되어 있는 느낌이 강한 UI를 만들 수 있습니다. 이 UI는 사용자에게 동적 UI 경험이라는 인상을 줍니다.

![궤도를 선회하는 원](images/animation/orbit.gif)

![시차가 포함된 목록 보기](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>ExpressionAnimations 사용

관계 기반 동작 경험을 만들려면 ExpressionAnimation 형식을 사용합니다. ExpressionAnimations(또는 줄여서 Expressions)는 시스템이 수학적 관계, 즉 매 프레임마다 애니메이션 속성의 값을 계산하는 데 사용하는 관계를 사용할 수 있는 새로운 유형의 애니메이션입니다. 달리 말해, Expressions는 단순히 프레임별 애니메이션 속성의 원하는 값을 정의하는 수학적 식입니다. Expressions는 다음과 같은 여러 가지 시나리오에서 매우 다양한 용도로 사용할 수 있는 구성 요소입니다.

- 상대적 크기, 오프셋 애니메이션.
- 고정 헤더, ScrollViewer가 있는 시차. ([기존 ScrollViewer 경험 향상](scroll-input-animations.md)을 참조하세요.)
- InertiaModifiers 및 InteractionTracker가 있는 끌기 지점. ([관성 한정자로 끌기 지점 만들기](inertia-modifiers.md)를 참조하세요.)

ExpressionAnimations를 사용하여 작업하기 전에 알아두면 좋은 몇 가지 유의사항이 있습니다.

- 기간 무제한 - 비슷한 유형인 KeyFrameAnimation과 달리 Expressions에는 정해진 기간이 없습니다. Expressions는 수학적 관계이기 때문에 상시 "실행 중인" 애니메이션입니다. 원한다면 이러한 애니메이션을 중지할 수 있습니다.
- 실행 중이지만 항상 평가 중인 것은 아닙니다. 상시 실행되는 애니메이션의 경우 항상 성능이 관건입니다. 하지만 걱정할 필요는 없습니다. 시스템은 매우 지능적이기 때문에 Expression은 입력값 또는 매개 변수가 변경되었는지만 다시 평가할 것입니다.
- 올바른 개체 유형으로 해석 - Expressions는 수학적 관계이므로 Expression을 정의하는 수식은 애니메이션 대상과 동일한 유형의 속성으로 해석되어야 합니다. 예를 들어, Offset에 애니메이션을 적용하면 Expression은 Vector3 유형으로 해석됩니다.

### <a name="components-of-an-expression"></a>Expression의 구성 요소

Expression의 수학적 관계를 구축할 때 몇 가지 핵심 구성 요소가 있습니다.

- 매개 변수 - 상수 값 또는 다른 Composition 개체에 대한 참조를 나타내는 값입니다.
- 수학 연산자 - 여러 매개 변수를 결합해 수식을 구성하는 더하기(+), 빼기(-), 곱하기(*), 나누기(/) 등의 일반적인 수학 연산자입니다. 보다 큼(>), 같음(==), 3항 연산자(조건 ? ifTrue : ifFalse), 등의 조건부 연산자도 포함합니다.
- 수학 함수 – System.Numerics 기반의 수학 함수/바로 가기입니다. 지원되는 함수 전체 목록을 보려면 [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation)을 참조하세요.

Expressions는 또한 ExpressionAnimation 시스템 내에서만 고유한 의미를 갖는 특수 구문인 키워드 집합을 지원합니다. 이 목록은 [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation) 문서에 수록되어 있습니다(수학 함수 전체 목록과 함께).

### <a name="creating-expressions-with-expressionbuilder"></a>ExpressionBuilder로 Expressions 만들기

UWP 앱에서 Expressions를 빌드하는 방법은 두 가지입니다.

1. 하나는 공용 API를 통해 문자열로 수식을 작성하는 것입니다.
1. 오픈 소스 ExpressionBuilder 도구를 통해 형식이 안전한 개체 모델에서 수식을 작성하는 것입니다. [Github 원본 및 설명서](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/ExpressionBuilder)를 참조하세요.

이 문서에서는 ExpressionBuilder를 사용하여 Expressions를 정의하겠습니다.

### <a name="parameters"></a>매개 변수

매개 변수는 Expression의 핵심을 구성합니다. 다음 두 종류의 매개 변수가 있습니다.

- 상수: 형식화된 System.Numeric 변수를 나타내는 매개 변수입니다. 이러한 매개 변수는 애니메이션이 시작될 때 한 번 값을 할당 받습니다.
- 참조: CompositionObject에 대한 참조를 나타내는 매개 변수입니다. 이러한 매개 변수는 애니메이션이 시작된 후에도 지속적으로 값을 업데이트합니다.

일반적으로 참조는 Expression의 출력이 어떻게 동적으로 변할 수 있는지 결정하는 주요 요소입니다. 이러한 참조가 변경되면 Expression의 출력도 변경됩니다. 문자열을 사용하여 Expression을 만들거나 템플릿 작업 시나리오에 사용하는 경우(여러 CompositionObjects를 대상으로 삼기 위해 Expression 사용) 매개 변수의 이름을 지정하고 값을 설정해야 합니다. 자세한 내용은 예제 섹션을 참조하세요.

### <a name="working-with-keyframeanimations"></a>KeyFrameAnimations 작업

Expressions는 KeyFrameAnimations와 함께 사용할 수도 있습니다. 이 경우, Expression을 사용하여 한 번에 KeyFrame의 값을 정의할 수 있습니다. 이러한 유형의 KeyFrames를 ExpressionKeyFrames라고 합니다.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

그러나 ExpressionAnimations와 달리 ExpressionKeyFrames는 KeyFrameAnimation이 시작될 때 한 번만 평가됩니다. KeyFrame의 값으로 ExpressionAnimation이 아닌 문자열(또는 ExpressionBuilder를 사용하는 경우 ExpressionNode)을 전달해야 합니다.

## <a name="example"></a>예

이제 Expressions, 특히 Windows UI 샘플 갤러리의 PropertySet 샘플을 사용하는 예제를 살펴보겠습니다. 파란색 공의 궤도 동작을 관리하는 Expression을 살펴보겠습니다.

![궤도를 선회하는 원](images/animation/orbit.gif)

전체 경험에 다음과 같은 세 개의 구성 요소가 작동하고 있습니다.

1. KeyFrameAnimation: 빨간색 공의 Y 오프셋에 애니메이션을 적용합니다.
1. **Rotation** 속성이 있는 PropertySet: 궤도를 구동하며, 다른 KeyFrameAnimation에서 애니메이션을 적용합니다.
1. ExpressionAnimation: 완벽한 궤도를 유지하기 위해 빨간 공 오프셋과 Rotation 속성을 참조하는 파란 공의 오프셋을 구동합니다.

이 문서에서는 #3에서 정의한 ExpressionAnimation에 집중합니다. 또한 ExpressionBuilder 클래스를 사용하여 이 Expression을 구성하겠습니다. 문자열을 통해 이 경험을 빌드하는 데 사용된 코드의 사본은 뒷부분에 수록되어 있습니다.

이 수식에서는 PropertySet에서 참조해야 하는 두 가지 속성이 있습니다. 하나는 중심점 오프셋이고 다른 하나는 회전입니다.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

다음으로, 실제 궤도 회전을 설명하는 Vector3 구성 요소를 정의해야 합니다.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` ExpressionBuilder.ExpressionFunctions를 정의하기 위한 단축형 "using" 표기법입니다.

마지막으로, 이러한 구성 요소를 결합하고 빨간색 공의 위치를 참조하여 수학적 관계를 정의합니다.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

만약 이 Expression을 그대로 사용하는 동시에 다른 시각 개체 두 개, 즉 궤도를 선회하는 원 집합 2개를 사용하고 싶다면 어떻게 해야 할까요? CompositionAnimations를 사용하면 애니메이션을 다시 사용하고 여러 CompositionObject를 대상으로 지정할 수 있습니다. 추가 궤도 사례에 이 Expression을 사용할 때 변경해야 하는 유일한 것은 시각 개체에 대한 참조입니다. 이를 템플릿 작업이라고 합니다.

이 경우 예전에 빌드한 Expression을 수정합니다. CompositionObject에 대한 참조를 "가져오는" 대신, 이름을 가진 참조를 만든 다음 다른 값을 할당합니다.

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

공개 API를 통해 문자열로 Expression을 정의한 경우 코드는 다음과 같습니다.

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
