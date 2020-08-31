---
title: 입력 기반 애니메이션
description: InputAnimation API 및 입력 기반 애니메이션을 사용 하 여 앱 UI에서 동적으로 응답 하는 동작을 만드는 방법에 대해 알아봅니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 애니메이션
ms.localizationpriority: medium
ms.openlocfilehash: 0f9abd902e39b645f27b7a0f5d521097ca4aff8a
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054363"
---
# <a name="input-driven-animations"></a>입력 기반 애니메이션

이 문서에서는 InputAnimation API에 대 한 소개를 제공 하 고 UI에서 이러한 형식의 애니메이션을 사용 하는 방법을 권장 합니다.

## <a name="prerequisites"></a>필수 구성 요소

여기서는 다음 문서에서 설명 하는 개념에 대해 잘 알고 있다고 가정 합니다.

- [관계 기반 애니메이션](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>사용자 상호 작용에서 부드러운 동작 제어

흐름 디자인 언어에서 최종 사용자와 앱 간의 상호 작용은 가장 중요 합니다. 앱은 파트를 살펴볼 뿐만 아니라 상호 작용 하는 사용자에 게 자연스럽 게 동적으로 응답 합니다. 즉, 화면에 손가락을 배치 하면 UI가 입력의 변경 수준에 적절 하 게 반응 해야 합니다. 스크롤은 부드러운 모양으로 이동 하 고 화면에서 손가락으로 이동 합니다.

사용자 입력 결과에 동적으로 응답 하 고 유동적으로 응답 하는 UI를 빌드하는 작업은 이제 정상 이지만 사용자가 다양 한 UI 환경과 상호 작용 하는 경우 유용 하 고 자연스럽 게 보입니다. 이렇게 하면 최종 사용자가 더 쉽게 응용 프로그램에 연결 하 여 환경을 기억 하 고 매력적인 수 있습니다.

## <a name="expanding-past-just-touch"></a>이전 터치만 확장

터치는 최종 사용자가 UI 콘텐츠를 조작 하는 데 사용 하는 보다 일반적인 인터페이스 중 하나 이지만 마우스 및 펜과 같은 다양 한 다른 입력 소프트웨어나 사용 합니다. 이러한 경우, 최종 사용자가 사용 하도록 선택한 입력에 관계 없이 UI가 입력에 동적으로 응답 하는 것을 확인 하는 것이 중요 합니다. 입력 기반 동작 환경을 디자인 하는 경우 다른 입력 소프트웨어나을 cognizant 합니다.

## <a name="different-input-driven-motion-experiences"></a>다른 입력 기반 동작 환경

InputAnimation 공간은 동적으로 응답 하는 동작을 만들 수 있는 다양 한 환경을 제공 합니다. Windows UI 애니메이션 시스템의 나머지 부분과 마찬가지로 이러한 입력 기반 애니메이션은 동적 동작 환경에 기여 하는 독립 스레드에서 작동 합니다. 그러나 환경에서 기존 XAML 컨트롤 및 구성 요소를 활용 하는 경우에도 해당 환경의 부분은 UI 스레드를 바인딩합니다.

동적 입력 기반 동작 애니메이션을 빌드할 때 만드는 세 가지 핵심 환경이 있습니다.

1. 기존 ScrollView 환경 개선 – XAML ScrollViewer 위치를 사용 하도록 설정 하 여 동적 애니메이션 환경을 구동 합니다.
1. 포인터 위치 중심 환경 – 적중 테스트 된 UIElement에서 커서의 위치를 활용 하 여 동적 애니메이션 환경을 구동 합니다.
1. InteractionTracker를 사용 하는 사용자 지정 조작 환경 – 스크롤/확대/축소 캔버스와 같은 InteractionTracker을 사용 하 여 완전히 사용자 지정 된 스레드 조작 환경을 만듭니다.

## <a name="enhancing-existing-scrollviewer-experiences"></a>기존 ScrollViewer 환경 향상

보다 동적인 환경을 만드는 일반적인 방법 중 하나는 기존 XAML ScrollViewer 컨트롤을 기반으로 빌드하는 것입니다. 이러한 경우에는 ScrollViewer의 스크롤 위치를 활용 하 여 간단한 스크롤 환경을 보다 동적으로 만드는 추가 UI 구성 요소를 만듭니다. 몇 가지 예로는 스티커/감춤 헤더 및 시차 있습니다.

![시차를 사용 하 여 목록 보기](images/animation/parallax.gif)

![감춤 헤더](images/animation/shy-header.gif)

이러한 유형의 환경을 만들 때는 다음과 같은 일반적인 수식을 사용할 수 있습니다.

1. 애니메이션을 구동 하려는 XAML ScrollViewer ScrollManipulationPropertySet off에 액세스 합니다.
    - ElementCompositionPreview. GetScrollViewerManipulationPropertySet (UIElement 요소) API를 통해 수행
    - **Translation** 이라는 속성을 포함 하는 CompositionPropertySet를 반환 합니다.
1. Translation 속성을 참조 하는 수식이 포함 된 컴퍼지션 ExpressionAnimation을 만듭니다.
1. CompositionObject의 속성에서 애니메이션을 시작 합니다.

이러한 환경을 빌드하는 방법에 대 한 자세한 내용은 [기존 ScrollViewer 환경 향상](scroll-input-animations.md)을 참조 하세요.

## <a name="pointer-position-driven-experiences"></a>포인터 위치 중심 환경

입력과 관련 된 또 다른 일반적인 동적 환경은 마우스 커서와 같은 포인터의 위치를 기반으로 애니메이션을 구동 하는 것입니다. 이러한 상황에서 개발자는 스포트라이트를 만들 수 있는 것과 같은 환경을 만드는 XAML UIElement 내에서 적중 테스트를 수행할 때 커서의 위치를 활용 합니다.

![포인터 스포트라이트 예](images/animation/spotlight-reveal.gif)

![포인터 회전 예제](images/animation/pointer-rotate.gif)

이러한 유형의 환경을 만들 때는 다음과 같은 일반적인 수식을 사용할 수 있습니다.

1. 적중 테스트를 수행할 때 커서 위치를 알고 싶은 XAML UIElement PointerPositionPropertySet에서 액세스 합니다.
    - ElementCompositionPreview. GetPointerPositionPropertySet (UIElement 요소) API를 통해 수행
    - **Position** 이라는 속성을 포함 하는 CompositionPropertySet를 반환 합니다.
1. Position 속성을 참조 하는 수식이 포함 된 CompositionExpressionAnimation를 만듭니다.
1. CompositionObject의 속성에서 애니메이션을 시작 합니다.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>InteractionTracker를 사용 하는 사용자 지정 조작 환경

XAML ScrollViewer를 활용 하는 문제 중 하나는 UI 스레드에 바인딩되어 있다는 것입니다. 결과적으로, UI 스레드가 사용 되 고 있어 매력적인 환경이 발생 하면 스크롤 및 확대/축소 환경이 종종 지연 될 수 있습니다. 또한 ScrollViewer 환경의 여러 측면을 사용자 지정할 수는 없습니다. InteractionTracker는 독립 스레드에서 실행 되는 사용자 지정 조작 환경을 만드는 일련의 구성 요소를 제공 하 여 두 문제를 해결 하기 위해 만들어졌습니다.

![3D 상호 작용 예제](images/animation/interactions-3d.gif)

![애니메이션 예제로 가져오기](images/animation/pull-to-animate.gif)

InteractionTracker를 사용 하 여 환경을 만들 때 다음을 수행 하는 일반적인 수식이 있습니다.

1. InteractionTracker 개체를 만들고 해당 속성을 정의 합니다.
1. InteractionTracker에 대 한 입력을 캡처하여 사용할 CompositionVisual에 대 한 VisualInteractionSources를 만듭니다.
1. InteractionTracker의 Position 속성을 참조 하는 수식이 있는 컴퍼지션 ExpressionAnimation을 만듭니다.
1. InteractionTracker으로 구동 하려는 컴퍼지션 시각적 개체의 속성에서 애니메이션을 시작 합니다.
