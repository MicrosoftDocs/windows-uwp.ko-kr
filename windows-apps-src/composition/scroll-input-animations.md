---
title: 기존 ScrollViewer 환경 향상
description: XAML ScrollViewer 및 ExpressionAnimations을 사용 하 여 동적 입력 기반 동작 환경을 만드는 방법에 대해 알아봅니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 438f108a07349da6515443e64bd4494529b8e6a0
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152408"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>기존 ScrollViewer 환경 향상

이 문서에서는 XAML ScrollViewer 및 ExpressionAnimations을 사용 하 여 동적 입력 기반 동작 환경을 만드는 방법을 설명 합니다.

## <a name="prerequisites"></a>사전 요구 사항

여기서는 다음 문서에서 설명 하는 개념에 대해 잘 알고 있다고 가정 합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>ScrollViewer을 기반으로 빌드하는 이유는 무엇 인가요?

일반적으로 기존 XAML ScrollViewer를 사용 하 여 앱 콘텐츠에 대해 스크롤 가능 하 고 확대/확대/확대/확대 화면을 만듭니다. 흐름 디자인 언어가 도입 됨에 따라, 화면 스크롤 또는 확대/축소 작업을 사용 하 여 다른 동작 환경을 구동 하는 방법을 집중적으로 살펴보겠습니다. 예를 들어 스크롤을 사용 하 여 배경의 흐림 효과 애니메이션을 구동 하거나 "고정 헤더"의 위치를 구동 합니다.

이러한 시나리오에서는 스크롤 및 확대/축소와 같은 동작 또는 조작 환경을 활용 하 여 응용 프로그램의 다른 부분을 동적으로 만듭니다. 이를 통해 앱을 더 긴밀 하 게 사용할 수 있으므로 최종 사용자의 눈에 더 많은 환경을 기억 하 게 됩니다. 앱 UI를 더 기억 하기 위해 최종 사용자가 앱을 더 자주 사용 하 고 더 오랫동안 사용 합니다.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>ScrollViewer을 기반으로 하 여 무엇을 빌드할 수 있나요?

ScrollViewer의 위치를 활용 하 여 다양 한 동적 환경을 만들 수 있습니다.

- 시차 – ScrollViewer의 위치를 사용 하 여 배경색과 전경 콘텐츠를 스크롤 위치로 이동 합니다.
- StickyHeaders – ScrollViewer 위치를 사용 하 여 헤더를 위치에 애니메이션 효과를 적용 하 고 "스틱" 합니다.
- Input-Driven 효과 – 흐림 효과와 같은 컴퍼지션 효과에 애니메이션 효과를 주려면 Scrollviewer의 위치를 사용 합니다.

일반적으로 ExpressionAnimation을 사용 하 여 ScrollViewer의 위치를 참조 하 여 스크롤 크기를 기준으로 동적으로 변경 되는 애니메이션을 만들 수 있습니다.

![시차를 사용 하 여 목록 보기](images/animation/parallax.gif)

![감춤 헤더](images/animation/shy-header.gif)

## <a name="using-scrollviewermanipulationpropertyset"></a>ScrollViewerManipulationPropertySet 사용

XAML ScrollViewer를 사용 하 여 이러한 동적 환경을 만들려면 애니메이션에서 스크롤 위치를 참조할 수 있어야 합니다. 이 작업은 ScrollViewerManipulationPropertySet 이라는 XAML ScrollViewer의 CompositionPropertySet off에 액세스 하 여 수행 됩니다.
ScrollViewerManipulationPropertySet에는 ScrollViewer의 스크롤 위치에 대 한 액세스를 제공 하는 Translation 이라는 단일 Vector3 속성이 포함 되어 있습니다. 그러면 ExpressionAnimation의 다른 CompositionPropertySet 마찬가지로이를 참조할 수 있습니다.

시작 하는 일반적인 단계:

1. ElementCompositionPreview을 통해 ScrollViewerManipulationPropertySet에 액세스 합니다.
    - `ElementCompositionPreview.GetScrollViewerManipulationPropertySet(ScrollViewer scroller)`
1. PropertySet에서 Translation 속성을 참조 하는 ExpressionAnimation을 만듭니다.
    - 참조 매개 변수를 설정 하는 것을 잊지 마세요.
1. ExpressionAnimation을 사용 하 여 CompositionObject의 속성을 대상으로 합니다.

> [!NOTE]
> GetScrollViewerManipulationPropertySet 메서드에서 반환 된 PropertySet을 클래스 변수에 할당 하는 것이 좋습니다. 이렇게 하면 속성 집합이 가비지 수집에 의해 정리 되지 않으므로에서 참조 되는 ExpressionAnimation에 영향을 주지 않습니다. ExpressionAnimations 수식에 사용 된 개체에 대 한 강력한 참조를 유지 하지 않습니다.

## <a name="example"></a>예제

위에 표시 된 시차 샘플을 함께 사용 하는 방법을 살펴보겠습니다. 참조를 위해 앱의 모든 소스 코드는 [GitHub의 창 UI 개발자 리포지토리](https://github.com/microsoft/WindowsCompositionSamples)에서 찾을 수 있습니다.

첫 번째 작업은 ScrollViewerManipulationPropertySet에 대 한 참조를 가져오는 것입니다.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

다음 단계는 ScrollViewer의 스크롤 위치를 활용 하는 수식을 정의 하는 ExpressionAnimation을 만드는 것입니다.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> 또한 ExpressionBuilder 도우미 클래스를 활용 하 여 문자열 없이도이 동일한 식을 생성할 수 있습니다.

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

마지막으로,이 ExpressionAnimation을 사용 하 고 시차 하려는 시각적 개체를 대상으로 합니다. 이 경우 목록의 각 항목에 대 한 이미지입니다.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```
