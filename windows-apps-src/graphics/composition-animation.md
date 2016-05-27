---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 컴퍼지션 애니메이션
description: 키 프레임 및 식 애니메이션을 사용하여 많은 컴퍼지션 개체와 효과 속성에 애니메이션 효과를 줄 수 있으므로 UI 요소의 속성을 시간에 따라 또는 계산을 기준으로 변경할 수 있습니다.
---
# 컴퍼지션 애니메이션

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

키 프레임 및 식 애니메이션을 사용하여 많은 컴퍼지션 개체와 효과 속성에 애니메이션 효과를 줄 수 있으므로 UI 요소의 속성을 시간에 따라 또는 계산을 기준으로 변경할 수 있습니다. 두 가지 유형의 애니메이션이 있습니다. [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830) 클래스에서 나타나는 키 프레임 애니메이션과 [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) 클래스에서 나타나는 식 애니메이션입니다.

-   [애니메이션 가능한 속성](./composition-animation.md#animatable_properties)
-   [키 프레임 애니메이션](./composition-animation.md#keyframe-animations)
    -   [애니메이션 및 키 프레임 만들기](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [키 프레임 속성](./composition-animation.md#keyframe-properties)
    -   [감속/가속 함수](./composition-animation.md#easing-functions)
    -   [키 프레임 애니메이션 시작 및 중지](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [애니메이션 완료 이벤트](./composition-animation.md#animation-completion-events)
-   [식 애니메이션](./composition-animation.md#expression-animations)
    -   [식 만들기](./composition-animation.md#creating-your-expression)
    -   [속성 집합](./composition-animation.md#property-sets)
    -   [식 키 프레임](./composition-animation.md#expression_keyframes)

## 애니메이션 가능한 속성

다음 속성은 키 프레임 또는 식 애니메이션을 사용하여 연결함으로써 애니메이션이 가능합니다.

-   CompositionColorBrush.Color
-   InsetClip.BottomInset
-   InsetClip.LeftInset
-   InsetClip.RightInset
-   InsetClip.TopInset
-   Visual.AnchorPoint
-   Visual.CenterPoint
-   Visual.Offset
-   Visual.Opacity
-   Visual.Orientation
-   Visual.RotationAngle
-   Visual.RotationAxis
-   Visual.Size
-   Visual.TransformMatrix
-   CompositionPropertySet

## 키 프레임 애니메이션

키 프레임 애니메이션은 하나 이상의 키 프레임을 사용하는 시간 기반 애니메이션으로, 애니메이션 값이 시간에 따라 변경되는 방식을 지정합니다. 프레임은 표식을 나타내므로 애니메이션 값이 지정된 시간에 무엇이어야 하는지 정의할 수 있습니다.

### 애니메이션 및 키 프레임 만들기

키 프레임 애니메이션을 생성하려면 애니메이션 효과를 주려는 속성의 구조 형식과 관련된 Compositor 클래스의 생성자 메서드 중 하나를 사용합니다.

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

예를 들어 다음 코드 조각은 Vector3 키 프레임 애니메이션을 만듭니다.

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

각 키 프레임은 KeyFrameAnimation의 InsertKeyFrame 메서드를 호출하고 두 구성 요소를 지정한 다음 선택적으로 세 번째 구성 요소를 지정하여 생성됩니다.

-   시간: 0.0 – 1.0 사이의 키 프레임의 정규화된 진행 상태
-   값: 시간 상태에서 애니메이션 값의 특정 값
-   (옵션) 감속/가속 함수: 이전과 현재 키 프레임 사이의 보간을 설명하는 함수(나중에 설명)

예를 들어 다음 코드 조각은 애니메이션 도중 트리거되는 Vector3KeyFrameAnimation에서 키 프레임을 삽입합니다.

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### 키 프레임 속성

키 프레임 애니메이션 및 개별 키 프레임을 정의했으면 애니메이션의 여러 속성을 정의할 수 있습니다.

-   DelayTime – StartAnimation()이 호출된 후 애니메이션을 시작하기 전의 시간
-   Duration – 애니메이션의 지속 시간
-   IterationBehavior – 애니메이션의 수 또는 무한 반복 동작
-   IterationCount - 키 프레임 애니메이션이 반복되는 유한 횟수
-   KeyFrame Count - 특정 KeyFrameAnimation의 키 프레임 수
-   StopBehavior – StopAnimation이 호출될 때 애니메이션 효과를 주려는 속성 값의 동작 지정

예를 들어 다음 코드 조각은 애니메이션 지속 시간을 5초로 설정합니다.

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### 감속/가속 함수

키 프레임 감속/가속 함수인 CompositionEasingFunction은 이전 키 프레임 값에서 현재 키 프레임 값까지 중간 값이 어떻게 진행되는지 나타냅니다. 키 프레임에 감속/가속 함수를 제공하지 않는 경우 기본 곡선이 사용됩니다.

지원되는 감속/가속 함수는 두 가지 유형이 있습니다.

-   선형([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   입방형 3차원([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

감속/가속 함수를 만들려면 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761) 클래스의 [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) 또는 [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791) 메서드를 사용합니다.

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

참고: 입방형 3차원에 대한 점을 생성하는 유용한 도구가 여기에 있습니다.

키 프레임에 감속/가속 함수를 추가하려면 다음과 같이 애니메이션에 삽입할 때 키 프레임에 대한 세 번째 매개 변수에 삽입하기만 하면 됩니다.

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### 키 프레임 애니메이션 시작 및 중지

애니메이션, 키 프레임 및 속성을 정의했으면 애니메이션 효과를 주려는 속성에 애니메이션을 연결할 준비가 된 것입니다. 속성이 속한 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 개체에서 [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840)을 호출하여 애니메이션 효과를 주려는 속성에 애니메이션을 연결합니다.

일반 구문 및 예제는 다음과 같습니다.

```cs
targetVisual.StartAnimation("Offset", animation);
```

애니메이션을 시작한 후 중지하고 연결을 끊는 기능도 있습니다. 이 기능은 [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841) 메서드를 사용해서 애니메이션을 중지하려는 속성을 지정하여 수행됩니다.

예를 들면 다음과 같습니다.

```cs
targetVisual.StopAnimation("Offset");
```

### 애니메이션 완료 이벤트

키 프레임 애니메이션은 조건적인 식 애니메이션과 달리 정의된 종료가 있습니다. 개발자는 일괄 처리를 사용하여 그룹이나 단일 키 프레임 애니메이션을 완료하는 시기를 결정할 수 있습니다. 일괄 처리는 일괄처리 개체 형식에 따라 만들거나 검색할 수 있으며 애니메이션의 종료 상태를 집계하는 데 사용됩니다. 범위 일괄 처리는 커밋 일괄 처리를 검색하는 동안 만들어지는데 이러한 다른 배치를 사용하는 방법은 나중에 설명합니다.

일괄 처리에서 모든 애니메이션이 완료되면 일괄 처리 완료 이벤트가 발생합니다. 일괄 처리 완료 이벤트가 발생하는 데 걸리는 시간은 일괄 처리에서 가장 길거나 가장 지연된 애니메이션에 따라 다릅니다. 다른 작업을 예약하기 위해 선택한 애니메이션 그룹이 언제 완료되는지 알아야 하는 경우 종료 상태 집계가 유용합니다.

### 범위 일괄 처리

특정 그룹이나 단일 애니메이션을 집계하려면 먼저 [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425)를 호출하여 범위 일괄 처리를 만들어야 합니다.

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

범위 일괄 처리를 만든 후 시작한 모든 애니메이션은 [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) 또는 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)를 사용하여 일괄 처리가 명시적으로 중단되거나 종료될 때까지 집계됩니다.

[
            **Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848)를 호출하면 [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847)이 호출될 때까지 애니메이션 종료 상태를 집계하는 것을 중지합니다. 이를 통해 제공된 일괄 처리에서 콘텐츠를 명시적으로 제외할 수 있습니다.

```cs
myScopedBatch.Suspend();
```

일괄 처리를 완료하려면 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)를 호출해야 합니다. 종료 호출이 없는 경우 일괄 처리는 계속 열려 있게 되어 영원히 개체를 수집합니다.

```cs
myScopedBatch.End();
```

단일 애니메이션이 언제 종료되는지 알고 싶은 경우에는 범위 일괄 처리를 만든 다음 애니메이션을 시작하고 일괄 처리를 종료합니다.

예를 들면 다음과 같습니다.

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

### 커밋 일괄 처리

커밋 일괄 처리는 커밋 주기 동안 암시적으로 만들어지는 일괄처리입니다. 현재 커밋 일괄 처리는 커밋 주기 중 언제라도 [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430)를 호출하여 검색할 수 있습니다. 커밋 주기는 작성자가 이전 업데이트에서 다음 업데이트를 수행하는 사이의 시간으로 정의됩니다. 업데이트는 시스템이 변경 사항을 처리할 준비가 되어 화면에 비트를 그릴 때까지 대기 중에 있습니다. 제목은 커밋이 발생하는 시기를 제어하지 않습니다. 커밋 일괄 처리는 커밋 주기 내의 모든 개체(GetCommitBatch가 호출되기 전과 호출된 후의 개체)를 집계합니다. 커밋 주기당 하나의 커밋 일괄 처리만 있으며 커밋 일괄 처리에서 명시적으로 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)를 호출할 필요가 없습니다.

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### 일괄 처리 상태

일괄 처리가 애니메이션 집계에 열려 있는지 확인하려면 **IsActive** 속성을 사용하면 됩니다. **IsEnded** 속성이 true를 반환하는 경우 특정 해당 배치에 애니메이션을 추가할 수 없습니다. 범위 일괄 처리는 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) 메서드를 호출하여 명시적으로 종료되었을 때 "종료됩니다." 커밋 일괄 처리는 현재 커밋 주기가 종료되었을 때 종료됩니다.

## 식 애니메이션

식 애니메이션은 수학적 식을 사용하는 애니메이션으로, 각 프레임에 대해 애니메이션 값이 계산되어야 하는 방법을 지정합니다. 식 언어 자체는 다른 컴퍼지션 개체의 속성을 참조할 수 있습니다. 키 프레임 애니메이션과 달리, 식은 시간 기반이 아니며 각 프레임을 처리합니다(필요한 경우). 식은 컴퍼지션 엔진에서 처리할 수 있는 보다 복잡한 사용자 환경(예: 고정 헤더 및 시차)을 설명하는 데 유용할 수 있습니다.

### 식 만들기

식을 만들려면 다음과 같이 작성자 개체에서 [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002)을 호출하여 사용하려는 식을 지정합니다.

```cs
var expression = _compositor.CreateExpressionAnimation("INSERT_EXPRESSION_STRING")
```

### 연산자, 우선 순위 및 연결

식 언어는 다음 연산자를 지원하고 C# 언어 사양에서 정의된 대로 연산자 우선 순위 및 연결을 준수합니다.

-   단항(-)
-   곱셈(\* /)
-   가산(- +)

또한 식 언어는 구성 요소 단위의 수학 연산의 개념도 지원합니다. 이러한 멤버-액세스(x.y) 연산자는 "기본" 유형(벡터 및 행렬)에서만 지원됩니다. 예를 들면 다음과 같습니다.

```cs
Offset.x + 5.0
```

### 속성 매개 변수

식 언어 중 강력한 구성 요소 중 하나는 매개 변수를 식의 변수로 사용할 수 있다는 것입니다. 식 문자열은 계산될 때 상수 값으로 바뀌는 매개 변수 또는 다른 개체의 속성 값에 대한 참조인 매개 변수를 포함할 수 있습니다. 예를 들면 다음과 같습니다.

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

이 예제에서 ChildOffset 및 ParentOffset은 두 시각적 개체의 오프셋 속성을 나타내는 매개 변수입니다. 다음과 같이 [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708) 클래스의 **Set\*Parameter** 메서드를 사용하여 매개 변수를 정의합니다.

-   벡터 매개 변수를 만들려면 [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx), [**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx) 또는 [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter)를 사용합니다.
-   행렬 매개 변수를 만들려면 [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx) 또는 [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter)를 사용합니다.
-   스칼라 매개 변수를 만들려면 [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter)를 사용합니다.
-   색 매개 변수를 만들려면 [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter)를 사용합니다.
-   쿼터니언 매개 변수를 만들려면 [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter)를 사용합니다.
-   참조 매개 변수를 만들려면 [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter)를 사용합니다.

위의 식 문자열에서 두 시각적 개체를 정의하려면 두 매개 변수를 만들어야 합니다.

```cs
Expression.SetReferenceParameter("ChildVisual", childVisual);
Expression.SetReferenceParameter("ParentVisual", parentVisual);
```

### 식 도우미 함수

연산자 및 속성 매개 변수에 대한 액세스 권한 외에도 식 언어의 전체 도우미 함수 라이브러리에 대한 액세스 권한도 있습니다. [
            **ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) 클래스의 설명 섹션에서 도우미 함수의 전체 목록을 찾을 수 있으며 여기에는 일부만 나와 있습니다.

-   Max (Scalar value1, Scalar value2)
-   Scale (Vector3 value, Scalar factor)
-   Transform(Vector2 value, Matrix 3x2 matrix)

다음은 Clamp 도우미 함수를 사용하는 더 복잡한 식 문자열 예제입니다.다.

```cs
Clamp((scroller.Offset.y * -1.0) - container.Offset.y, 0, container.Size.y - header.Size.y)
```

### 식 애니메이션 시작 및 중지

식 애니메이션을 시작하고 중지하려면 키 프레임 애니메이션에서처럼 동일한 함수([**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840), [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841))를 사용합니다. 참고: 식 애니메이션은 키 프레임 애니메이션과 달리 **StopAnimation**을 사용하여 중지될 때까지 계속 실행되며 한정된 기간이 없습니다.

### 속성 집합

다른 컴퍼지션 개체의 속성을 참조할 수 있는 것 외에도 여러 애니메이션에서 사용할 수 있는 고유한 속성 값을 만들고 저장하는 기능이 있습니다. 속성 집합은 [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772) 클래스로 표시됩니다. 속성 집합을 사용하면 값을 가진 속성을 만들어서 식에서 나중에 참조하거나 키 프레임 애니메이션의 대상으로 연결할 수도 있습니다.

속성 집합을 만들려면 Compositor 클래스의 [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802) 메서드를 사용합니다. 예를 들면 다음과 같습니다.

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

속성 집합을 만들었으면 [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772)의 **Insert\*** 메서드 중 하나를 사용하여 속성 및 값을 속성 집합에 추가할 수 있습니다. 예를 들면 다음과 같습니다.

```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

식 애니메이션을 만들었으면 참조 매개 변수를 사용하여 식 문자열에서 속성 집합의 속성을 참조할 수 있습니다. 예를 들면 다음과 같습니다.

```cs
var expression = _compositor.CreateExpressionAnimation("sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

### 식 키 프레임

이 문서의 앞부분에서 키 프레임 애니메이션 및 이에 대한 개별적인 키 프레임을 만드는 방법에 대해 설명했습니다. 정규화된 시간 및 값 구성 요소를 사용하여 기존 키 프레임을 만드는 것 외에 식 키 프레임을 정의할 수도 있습니다.

키 프레임의 특정 시간에 명시적 값을 정의하는 대신 식 구문을 사용하여 키 프레임 타임라인의 이 특정 지점에서 값이 계산되는 방법을 설명하는 데 식 구문을 사용할 수 있습니다. 키 프레임 애니메이션의 기본 키 프레임을 사용하여 식 키 프레임을 혼합하고 일치시킬 수 있습니다.

### 식 키 프레임 생성

기존의 키 프레임과 마찬가지로 식 키 프레임은 2개의 구성 요소를 지정하여 생성됩니다.

-   시간: 0.0 – 1.0 사이의 키 프레임의 정규화된 시간 상태
-   값: 값이 계산되는 방법을 설명하는 데 사용하는 식 문자열

예를 들어 다음 코드 조각은 일반 키 프레임과 식 키 프레임의 조합을 사용합니다.

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

### 현재 값 및 시작 값 사용

식 언어에서 애니메이션의 현재 값 및 시작 값을 모두 참조할 수 있습니다. 이러한 값은 "this"와 함께 식 언어로 미리 고정되어 있습니다.

-   this.CurrentValue
-   this.StartingValue

식 키 프레임에서 이러한 값을 사용하는 예제:

```cs
animation.InsertExpressionKeyFrame(0.0f, "this.CurrentValue + delta");
```

### 조건식

기본 수학 식을 지원하는 것 외에 3개로 구성된 연산자를 사용하는 조건식도 정의할 수 있습니다.

```cs
(condition ? first_expression : second_expression)
```

식 자체 내에서 다음 조건 연산자가 조건문에서 지원됩니다.

-   같음(==)
-   같지 않음(!=)
-   보다 작음(<)
-   작거나 같음(<=)
-   보다 큼(>)
-   크거나 같음(>=)

마지막으로 다음 접속사는 조건문의 연산자 또는 함수로 지원됩니다.

-   제외: ! / Not(bool1)
-   및: && / And(bool1, bool2)
-   또는: || / Or(bool1, bool2)

다음 코드 조각은 식에서 조건문을 사용하는 예제를 보여 줍니다.

```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f");
```

 

 






<!--HONumber=May16_HO2-->


