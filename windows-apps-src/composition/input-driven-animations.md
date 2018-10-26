---
author: jwmsft
title: 입력 기반 애니메이션
description: 앱 UI에서 입력 기반 애니메이션을 사용하는 방법에 대해서 알아봅니다.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 04eabb4c70143a08f5b850e6444f7f3d21a9dd4a
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5566915"
---
# <a name="input-driven-animations"></a>입력 기반 애니메이션

이 문서에서는 InputAnimation API에 대해 소개하고 UI에 이러한 유형의 애니메이션을 사용하는 바람직한 방법을 안내합니다.

## <a name="prerequisites"></a>필수 구성 요소

여기에서는 여러분이 이 문서에서 다룬 개념에 익숙하다고 가정합니다.

- [관계 기반 애니메이션](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>사용자 상호 작용에 기반한 매끄러운 동작

Fluent 디자인 언어에서는 최종 사용자와 앱 간의 상호 작용이 가장 중요합니다. 앱은 부분을 살펴야 할뿐만 아니라 상호 작용하는 사용자에게 자연스럽고 능동적으로 반응해야 합니다. 즉 손가락을 화면에 댔을 때 달라지는 입력 강도에 따라 UI가 적절히 반응해야 합니다. 스크롤 동작이 매끄럽게 느껴지고 화면을 가로지르는 손가락에 감응해야 합니다.

사용자 입력에 동적으로 유연하게 반응하는 사용자 인터페이스는 사용자의 참여도를 높입니다. 이제는 사용자가 다양한 UI 경험과 상호 작용하는 동안 동작이 보기에 좋아야 할 뿐만 아니라, 느낌이 좋고 자연스러워야 합니다. 이를 통해 최종 사용자는 응용 프로그램과 보다 쉽게 연결하여, 더 인상적이고 즐거운 경험을 쌓을 수 있습니다.

## <a name="expanding-past-just-touch"></a>단순한 터치 이상으로 확장

터치는 최종 사용자가 UI 콘텐츠를 조작하는 데 사용하는 가장 일반적인 인터페이스 중 하나이지만, 마우스나 펜처럼 다양한 다른 입력 방법도 함께 사용됩니다. 이러한 경우, 최종 사용자가 어떤 입력 방식을 선택하든 UI가 입력에 동적으로 응답한다고 느껴야 합니다. 입력 기반 동작 경험을 설계할 때는 다양한 입력 방식을 알고 있어야 합니다.

## <a name="different-input-driven-motion-experiences"></a>다양한 입력 기반 동작 경험

InputAnimation 공간은 동적으로 응답하는 동작을 만들 수 있는 몇 가지 환경을 제공합니다. 다른 Windows UI Animation 시스템과 마찬가지로, 이러한 입력 기반 애니메이션은 독립적인 스레드에서 작동하므로 동적 동작 경험에 도움이 됩니다. 그러나 기존 XAML 컨트롤과 구성 요소를 사용자 경험에 활용하는 일부 환경에서는 이러한 경험 중 일부가 UI 스레드에 바인딩됩니다.

동적 입력 기반 동작 애니메이션을 빌드할 때 만드는 세 가지 핵심 경험이 있습니다.

1. 기존 ScrollView 경험 향상 - XAML ScrollViewer의 위치를 통해 동적 애니메이션 경험을 유도할 수 있습니다.
1. 포인터 위치 기반 경험 - 적중 횟수 테스트를 마친 UIElement에서 커서의 위치를 활용하여 동적 애니메이션 경험을 유도합니다.
1. InteractionTracker를 사용한 사용자 지정 조작 경험 - InteractionTracker를 사용하여 완전히 사용자 지정된 오프 스레드 조작 환경(예: 스크롤 동작 및 캔버스 확대/축소)을 만듭니다.

## <a name="enhancing-existing-scrollviewer-experiences"></a>기존 ScrollViewer 경험 향상

보다 동적인 경험을 만드는 일반적인 방법 중 하나는 기존 XAML ScrollViewer 컨트롤을 기반으로 빌드하는 것입니다. 이러한 상황에서는 ScrollViewer의 스크롤 위치를 활용하여 단순한 스크롤링 환경을 보다 동적으로 만드는 추가 UI 구성 요소를 만들 수 있습니다. 고정/Shy 헤더 및 시차가 여기에 포함됩니다.

![시차가 포함된 목록 보기](images/animation/parallax.gif)

![shy 헤더](images/animation/shy-header.gif)

이러한 유형의 경험을 만들 때 따라야 하는 일반적인 공식이 있습니다.

1. 애니메이션을 구동할 XAML ScrollViewer에서 ScrollManipulationPropertySet에 액세스합니다.
    - ElementCompositionPreview.GetScrollViewerManipulationPropertySet(UIElement 요소) API를 통해 완료
    - **Translation**이라는 속성이 포함된 CompositionPropertySet 반환
1. Translation 속성을 참조하는 수식을 사용하여 Composition ExpressionAnimation을 만듭니다.
1. CompositionObject의 속성을 기반으로 애니메이션을 시작합니다.

이러한 경험 빌드에 대한 자세한 내용은 [기존 ScrollViewer 경험 향상](scroll-input-animations.md)을 참조하세요.

## <a name="pointer-position-driven-experiences"></a>포인터 위치 기반 경험

입력을 이용하는 또 한 가지 일반적인 동적 경험은 마우스 커서 같은 포인터의 위치를 기반으로 애니메이션을 구동하는 것입니다. 이러한 상황에서 개발자는 Spotlight Reveal 같은 경험을 만들 수 있게 해주는 XAML UIElement 내에서 적중 횟수 테스트를 통과한 커서 위치를 활용합니다.

![포인터 추천 예제](images/animation/spotlight-reveal.gif)

![포인터 회전 예제](images/animation/pointer-rotate.gif)

이러한 유형의 경험을 만들 때 따라야 하는 일반적인 공식이 있습니다.

1. 적중 테스트 당시의 커서 위치를 알고자 하는 XAML UIElement에서 PointerPositionPropertySet에 액세스합니다.
    - ElementCompositionPreview.GetPointerPositionPropertySet(UIElement 요소) API를 통해 완료
    - **Position**이라는 속성이 포함된 CompositionPropertySet 반환
1. Position 속성을 참조하는 수식을 사용하여 CompositionExpressionAnimation을 만듭니다.
1. CompositionObject의 속성을 기반으로 애니메이션을 시작합니다.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>InteractionTracker를 사용한 사용자 지정 조작 경험

XAML ScrollViewer 사용에 따른 문제점 중 하나는 UI 스레드에 바인딩된다는 것입니다. 따라서 UI 스레드를 사용 중일 때는 스크롤링 및 확대/축소 경험이 지연되고 지터가 발생하여 유쾌하지 않은 경험을 하게 될 수 있습니다. 또한 ScrollViewer 경험의 여러 가지 측면을 사용자 지정할 수 없습니다. InteractionTracker는 독립적인 스레드에서 실행되는 사용자 지정 조작 경험을 만들 수 있는 구성 요소 집합을 제공함으로써, 이 두 가지 문제를 모두 해결하도록 만들어졌습니다.

![3D 상호 작용 예제](images/animation/interactions-3d.gif)

![당겨서 애니메이션 예제](images/animation/pull-to-animate.gif)

InteractionTracker로 경험을 만들 때 따라야 하는 일반적인 공식이 있습니다.

1. InteractionTracker 개체를 만들고 속성을 정의합니다.
1. InteractionTracker가 사용할 입력을 캡처해야 하는 CompositionVisual에 대한 VisualInteractionSources를 만듭니다.
1. InteractionTracker의 속성을 참조하는 수식을 사용하여 Composition ExpressionAnimation을 만듭니다.
1. InteractionTracker에서 구동하고자 하는 Composition Visual의 속성을 기반으로 애니메이션을 시작합니다.
