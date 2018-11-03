---
author: jwmsft
title: 기존 ScrollViewer 경험 향상
description: XAML ScrollViewer 및 ExpressionAnimations를 사용하여 동적 입력 기반의 동작 경험을 만드는 방법에 대해서 알아봅니다.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: a078d096a9cffe26e9b342250726dd75cdf48817
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993451"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>기존 ScrollViewer 경험 향상

이 문서에서는 XAML ScrollViewer 및 ExpressionAnimations를 사용하여 동적 입력 기반의 동작 경험을 만드는 방법에 대해 설명합니다.

## <a name="prerequisites"></a>필수 구성 요소

여기에서는 여러분이 이 문서에서 다룬 개념에 익숙하다고 가정합니다.

- [입력 기반 애니메이션](input-driven-animations.md)
- [관계 기반 애니메이션](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>왜 ScrollViewer를 기반으로 빌드할까요?

일반적으로 기존의 XAML ScrollViewer는 스크롤 및 확대/축소가 가능한 앱 콘텐츠 화면을 만드는 데 사용됩니다. Fluent 디자인 언어가 도입되면서, 이제는 화면을 스크롤하거나 확대/축소하는 동작을 사용하여 다른 동작 경험을 구동하는 방법에도 초점을 맞추어야 합니다. 예를 들어 스크롤 사용으로 배경의 흐림 애니메이션을 구동하거나 "고정 헤더"의 위치를 유도하는 것입니다.

이 시나리오에서는 스크롤링 및 확대/축소 등의 동작 또는 조작 경험을 활용하여 앱의 다른 부분을 보다 동적으로 만들고 있습니다. 이로 인해 앱의 일관성이 더욱 개선되어 최종 사용자가 보기에 더욱 인상적인 경험을 창출할 수 있습니다. 앱 UI를 더욱 인상적으로 만들면 최종 사용자는 앱을 더 자주, 더 오랜 시간에 걸쳐 사용하게 됩니다.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>ScrollViewer를 기반으로 무엇을 빌드할 수 있을까요?

ScrollViewer의 위치를 활용하여 다양한 동적 경험을 빌드할 수 있습니다.

- 시차 - 스크롤 위치에 상대적인 속도로 백그라운드 또는 포그라운드 콘텐츠를 이동하려면 ScrollViewer의 위치를 사용합니다.
- 고정 헤더 – ScrollViewer의 위치를 사용하여 헤더를 애니메이션으로 표현하고 한 위치에 "고정"
- 입력 기반 효과 - Scrollviewer의 위치를 사용하여 흐림 등의 컴퍼지션 효과를 애니메이션으로 만듭니다.

일반적으로 ExpressionAnimation을 사용하여 ScrollViewer의 위치를 참조하면 스크롤 양에 따라 변하는 애니메이션을 만들 수 있습니다.

![시차가 포함된 목록 보기](images/animation/parallax.gif)

![shy 헤더](images/animation/shy-header.gif)

## <a name="using-scrollmanipulationpropertyset"></a>ScrollManipulationPropertySet 사용

XAML ScrollViewer를 사용하여 이러한 동적 경험을 만들려면 애니메이션에서 스크롤 위치를 참조할 수 있어야 합니다. 이 작업은 ScrollManipulationPropertySet이라는 XAML ScrollViewer의 CompositionPropertySet에 액세스하여 수행됩니다.
ScrollManipulationPropertySet에는 ScrollViewer의 스크롤 위치에 대한 액세스를 제공하는, Translation이라는 단일 Vector3 속성이 들어 있습니다. 따라서 ExpressionAnimation의 다른 CompositionPropertySet처럼 이 속성을 참조할 수 있습니다.

시작하기 위한 일반 절차:

1. ElementCompositionPreview를 통해 ScrollManipulationPropertySet에 액세스합니다.
    - `ElementCompositionPreview.GetScrollManipulationPropertySet(ScrollViewer scroller)`
1. PropertySet의 Translation 속성을 참조하는 ExpressionAnimation을 만듭니다.
    - 반드시 참조 매개 변수를 설정해야 합니다!
1. ExpressionAnimation으로 CompositionObject의 속성을 대상으로 지정합니다.

> [!NOTE]
> GetScrollManipulationPropertySet 메서드에서 반환된 PropertySet을 클래스 변수에 할당하는 것이 좋습니다. 이렇게 하면 가비지 수집에 의해 속성 집합이 정리되지 않으므로 참조되는 ExpressionAnimation에 아무런 영향을 미치지 않습니다. ExpressionAnimations는 수식에 사용된 개체에 대한 강력한 참조를 유지하지 않습니다.

## <a name="example"></a>예

위의 시차 샘플을 어떻게 조합하는지 살펴보겠습니다. 참고로, 앱의 모든 소스 코드는 [GitHub의 Window UI 개발자 랩 리포](https://github.com/Microsoft/WindowsUIDevLabs)에서 확인할 수 있습니다.

가장 먼저 할 일은 ScrollManipulationPropertySet에 대한 참조를 얻는 것입니다.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

그 다음 단계는 ScrollViewer의 스크롤 위치를 사용하는 수식을 정의하는 ExpressionAnimation을 만드는 것입니다.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> 또한 ExpressionBuilder 도우미 클래스를 활용하여 문자열이 필요 없는 동일한 Expression을 구성할 수 있습니다.

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

마지막으로, 이 ExpressionAnimation을 가져오고, 시차를 적용할 시각 개체를 대상으로 삼습니다. 이 경우에는 목록의 각 항목에 대한 이미지가 대상입니다.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```