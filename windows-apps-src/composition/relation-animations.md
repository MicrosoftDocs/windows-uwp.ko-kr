---
title: 관계 기반 애니메이션
description: 모션이 다른 개체의 속성에 따라 달라 지는 경우 ExpressionAnimations를 사용 하 여 관계 기반 애니메이션을 만드는 방법을 알아봅니다.
ms.date: 10/16/2020
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 75adcd2f762fd4314d7b852811760d523ef522aa
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152415"
---
# <a name="relation-based-animations"></a>관계 기반 애니메이션

이 문서에서는 컴퍼지션 ExpressionAnimations을 사용 하 여 관계 기반 애니메이션을 만드는 방법에 대 한 간략 한 개요를 제공 합니다.

## <a name="dynamic-relation-based-experiences"></a>동적 관계 기반 환경

앱에서 동작 환경을 빌드하는 경우 동작이 시간 기반이 아닌 다른 개체의 속성에 따라 달라 지는 경우가 있습니다. Key프레임 애니메이션은 이러한 유형의 동작 환경을 매우 쉽게 표현할 수 없습니다. 이러한 특정 인스턴스에서 더 이상 동작을 불연속 하 고 미리 정의할 필요가 없습니다. 대신, 동작은 다른 개체 속성과의 관계에 따라 동적으로 조정할 수 있습니다. 예를 들어 가로 위치를 기준으로 개체의 불투명도에 애니메이션 효과를 적용할 수 있습니다. 다른 예로는 고정 헤더 및 시차 같은 동작 환경이 있습니다.

이러한 종류의 동작 환경에서는 단일 및 독립에 상관 없이 더 많은 연결 된 것으로 판단 되는 UI를 만들 수 있습니다. 사용자에 게는 동적 UI 환경을 제공 합니다.

![Orbiting 원](images/animation/orbit.gif)

![시차를 사용 하 여 목록 보기](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>ExpressionAnimations 사용

관계 기반 동작 환경을 만들려면 ExpressionAnimation 유형을 사용 합니다. ExpressionAnimations (또는 short의 경우)는 수학적 관계를 표현할 수 있도록 하는 새로운 유형의 애니메이션입니다 .이는 시스템에서 모든 프레임에 애니메이션 효과를 주는 데 사용 되는 관계입니다. 또 다른 방법으로, 식은 프레임당 애니메이션 속성의 원하는 값을 정의 하는 수학적 방정식입니다. 식은 다음과 같은 다양 한 시나리오에서 사용할 수 있는 매우 다양 한 구성 요소입니다.

- 상대적 크기, 오프셋 애니메이션입니다.
- 고정 헤더, 시차 ScrollViewer. ( [기존 ScrollViewer 환경 향상](scroll-input-animations.md)을 참조 하세요.)
- InertiaModifiers 및 InteractionTracker를 사용 하 여 요소를 맞춥니다. ( [관성 한정자를 사용 하 여 맞추기 요소 만들기](inertia-modifiers.md)를 참조 하세요.)

ExpressionAnimations 함께 작업 하는 경우 앞으로 설명할 수 있는 몇 가지 사항이 있습니다.

- 종료 하지 않음 – Key프레임 애니메이션 형제와 달리 식에는 한정 된 기간이 없습니다. 식은 수학적 관계 이므로 지속적으로 "실행" 되는 애니메이션입니다. 을 선택 하는 경우 이러한 애니메이션을 중지 하는 옵션이 있습니다.
- 항상 실행 되는 것은 아니지만 실행 되는 애니메이션은 항상 실행 되는 애니메이션에서 항상 문제가 됩니다. 하지만 걱정 하지 않아도 되는 경우에는 입력 또는 매개 변수가 변경 된 경우에만 식이 다시 계산 됩니다.
- 올바른 개체 형식으로 확인 – 식이 수학적 관계 이므로 식을 정의 하는 수식이 애니메이션 대상으로 지정 되는 동일한 형식의 속성으로 확인 되는 것이 중요 합니다. 예를 들어 오프셋에 애니메이션을 적용 하는 경우 식은 Vector3 형식으로 확인 되어야 합니다.

### <a name="components-of-an-expression"></a>식의 구성 요소

식의 수학적 관계를 작성할 때 다음과 같은 몇 가지 핵심 구성 요소가 있습니다.

- Parameters – 상수 값 또는 다른 컴퍼지션 개체에 대 한 참조를 나타내는 값입니다.
- 수치 연산자 – 매개 변수를 결합 하 여 방정식을 형성 하는 일반적인 수치 연산자 더하기 (+), 빼기 (-), 곱하기 (*), 나누기 (/) 또한 보다 큼 (>), 같음 (= =), 삼항 연산자 (조건) 등의 조건부 연산자도 포함 되어 있습니다. ifTrue: ifFalse) 등
- 수치 연산 함수 – system.string을 기반으로 하는 수치 연산 함수/바로 가기입니다. 지원 되는 함수의 전체 목록은 [Expressionanimation](/uwp/api/Windows.UI.Composition.ExpressionAnimation)을 참조 하세요.

식은 ExpressionAnimation 시스템 내 에서만 의미가 있는 특수 구와 키워드 집합을 지원 합니다. 이러한 목록은 [Expressionanimation](/uwp/api/Windows.UI.Composition.ExpressionAnimation) 설명서의 전체 수학 함수 목록과 함께 나열 됩니다.

### <a name="creating-expressions-with-expressionbuilder"></a>ExpressionBuilder를 사용 하 여 식 만들기

UWP 앱에서 식을 작성 하는 두 가지 옵션은 다음과 같습니다.

1. 공식 공용 API를 통해 수식을 문자열로 작성 합니다.
1. [Windows 커뮤니티 도구 키트](/windows/communitytoolkit/animations/expressions)에 포함 된 expressionbuilder 도구를 통해 형식이 안전한 개체 모델에서 수식을 작성 합니다.

이 문서의 편의를 위해 ExpressionBuilder를 사용 하 여 식을 정의 합니다.

### <a name="parameters"></a>매개 변수

매개 변수는 식의 핵심을 구성 합니다. 다음과 같은 두 가지 유형의 매개 변수가 있습니다.

- 상수: 형식화 된 system.string 변수를 나타내는 매개 변수입니다. 이러한 매개 변수는 애니메이션이 시작 될 때 한 번 할당 된 값을 가져옵니다.
- 참조: CompositionObjects에 대 한 참조를 나타내는 매개 변수입니다 .이 매개 변수는 애니메이션이 시작 된 후 해당 값을 지속적으로 가져옵니다.

일반적으로 참조는 식의 출력이 동적으로 변경 될 수 있는 방법에 대 한 주요 측면입니다. 이러한 참조가 변경 되 면 식의 출력이 결과로 변경 됩니다. 문자열을 사용 하 여 식을 만들거나 템플릿 시나리오에서 사용 하는 경우 (여러 CompositionObjects를 대상으로 하는 식을 사용 하 여) 매개 변수의 값을 설정 하 고 값을 설정 해야 합니다. 자세한 내용은 예제 섹션을 참조하세요.

### <a name="working-with-keyframeanimations"></a>Key프레임 애니메이션 사용

식은 Key프레임 애니메이션에도 사용할 수 있습니다. 이러한 경우 식을 사용 하 여 특정 시점의 키 프레임 값을 정의 하려고 합니다. 이러한 형식 키프레임은 ExpressionKeyFrames 이라고 합니다.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

그러나 Expressionanimations과 달리 Expressionanimations Key프레임 애니메이션을 시작할 때 한 번만 평가 됩니다. ExpressionAnimation은 문자열 (Expressionanimation를 사용 하는 경우 Expressionanimation) 대신 키 프레임의 값으로 전달 하지 않습니다.

## <a name="example"></a>예제

이제 식 사용 예를 살펴보겠습니다. 특히 Windows UI 샘플 갤러리의 PropertySet 샘플을 살펴보겠습니다. 파란색 구슬의 궤도 동작 동작을 관리 하는 식을 살펴보겠습니다.

![Orbiting 원](images/animation/orbit.gif)

총 환경을 위한 세 가지 구성 요소가 있습니다.

1. 빨간색 구슬의 Y 오프셋에 애니메이션을 적용 하는 Key프레임 애니메이션입니다.
1. 다른 Key프레임 애니메이션에 의해 애니메이션이 적용 되는 궤도를 구동 하는 데 도움이 되는 **회전** 속성을 사용 하는 PropertySet입니다.
1. 빨강 구슬 오프셋 및 회전 속성을 참조 하는 파란색 구슬의 오프셋을 구동 하 여 완벽 한 궤도를 유지 하는 ExpressionAnimation입니다.

#3에 정의 된 ExpressionAnimation에 초점을 맞춘 것입니다. 또한 ExpressionBuilder 클래스를 사용 하 여이 식을 생성할 것입니다. 문자열을 통해이 환경을 빌드하는 데 사용 되는 코드 복사본이 끝에 나열 됩니다.

이 수식에서는 PropertySet에서 참조 해야 하는 두 가지 속성이 있습니다. 하나는 centerpoint 오프셋이 고 다른 하나는 회전입니다.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

다음으로 실제 orbiting 회전에 대해 계정을 지정 하는 Vector3 구성 요소를 정의 해야 합니다.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` 는 ExpressionFunctions를 정의 하는 간단한 "using" 표기법입니다.
>
> `using EF = Microsoft.Toolkit.Uwp.UI.Animations.Expressions.ExpressionFunctions;`

마지막으로, 이러한 구성 요소를 함께 결합 하 고 빨강 해골의 위치를 참조 하 여 수학적 관계를 정의 합니다.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

가상의 상황에서이 동일한 식을 사용 하 고 두 개의 다른 시각적 개체를 사용 하는 경우 두 개의 orbiting 원 집합을 의미 합니다. CompositionAnimations를 사용 하면 애니메이션을 다시 사용 하 고 여러 CompositionObjects를 대상으로 지정할 수 있습니다. 추가 궤도 케이스에이 식을 사용할 때 변경 해야 하는 유일한 사항은 시각적 개체에 대 한 참조입니다. 이 템플릿을 호출 합니다.

이 경우 이전에 작성 한 식을 수정 합니다. CompositionObject에 대 한 참조를 "가져오는" 대신 이름을 사용 하 여 참조를 만든 다음 다른 값을 할당 합니다.

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

공용 API를 통해 문자열을 사용 하 여 식을 정의한 경우 코드는 다음과 같습니다.

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
