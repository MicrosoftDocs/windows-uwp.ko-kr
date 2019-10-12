---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 컴퍼지션 애니메이션
description: 키 프레임 및 식 애니메이션을 사용하여 많은 컴퍼지션 개체와 효과 속성에 애니메이션 효과를 줄 수 있으므로 UI 요소의 속성을 시간에 따라 또는 계산을 기준으로 변경할 수 있습니다.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5991b5f9f3c63a25a7476cc35621488c6cb60b2c
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281827"
---
# <a name="composition-animations"></a>컴퍼지션 애니메이션

Windows.UI.Composition API를 사용하면 통합된 API 계층에서 작성자 개체를 만들고, 변환하고, 조작할 수 있으며 작성자 개체에 애니메이션 효과를 줄 수 있습니다. 컴퍼지션 애니메이션은 응용 프로그램 UI에서 애니메이션을 실행하는 강력하고 효율적인 방법을 제공합니다. 컴퍼지션 애니메이션은 UI 스레드와 독립적인 60FPS에서 애니메이션이 실행되도록 하고 시간뿐만 아니라 입력 및 기타 속성을 사용하여 멋진 애니메이션 실행 환경을 구축하기 위한 유연성을 제공하도록 처음부터 디자인되었습니다.

## <a name="motion-in-windows"></a>Windows의 동작

동영상 같은 동작 디자인이라고 볼 수 있습니다. 매끄러운 전환은 스토리에 대한 집중도를 높이고 생동감 있는 경험을 선사합니다. 이런 느낌을 디자인에 구현하여, 영화를 보듯 편안하게 여러 작업을 이어갈 수 있도록 했습니다. 동작은 종종 사용자 인터페이스와 사용자 환경 간의 차별화 요소입니다.

Windows UI 플랫폼의 기본 빌딩 블록으로 CompositionAnimations는 응용 프로그램의 UI에서 동작 환경을 만드는 강력 하 고 효율적인 방법을 제공 합니다. 애니메이션 엔진은 UI 스레드와 무관 하 게 동작이 60 FPS에서 실행 되도록 하기 위해 처음부터 설계 되었습니다. 이러한 애니메이션은 시간, 입력 및 기타 속성에 따라 혁신적인 동작 환경을 만들 수 있는 유연성을 제공 하도록 설계 되었습니다.

### <a name="examples-of-motion"></a>동작의 예

다음은 앱 내에서 이루어지는 동작의 예입니다.

여기에 나오는 앱은 항목 이미지가 "계속해서" 다음 페이지의 머리글이 되도록 연결된 애니메이션을 사용하여 항목 이미지에 애니메이션 효과를 줍니다. 이 효과는 전환 시 사용자 컨텍스트를 유지하는 데 도움이 됩니다.

![연결 된 애니메이션의 예](images/animation/connected-animation-example.gif)

여기서는 깊이감, 원근감 및 운동성을 주기 위해 UI가 스크롤하거나 이동할 때 시각적 시차 효과를 통해 여러 개체를 서로 다른 속도로 움직입니다.

![목록 및 배경 이미지를 사용한 시차의 예](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>CompositionAnimations를 사용 하 여 동작 만들기

UI에서 동작을 생성 하기 위해 개발자는 XAML 또는 시각적 계층 중 하나에서 애니메이션에 액세스할 수 있습니다. 시각적 계층의 애니메이션은 개발자에 게 일련의 이점을 제공 합니다.

- 성능 – 기존 UI 스레드 바인딩 애니메이션 대신 Windows UI 플랫폼의 애니메이션이 60 FPS의 독립 스레드에서 작동 하 여 부드러운 동작 환경을 가능 하 게 합니다.
- 템플릿 모델 – Windows UI 계층의 애니메이션은 템플릿입니다. 즉, 여러 개체에 대해 단일 애니메이션을 사용 하 고 이전 사용 방해 하지 않는지 걱정 하지 않고 속성 또는 매개 변수를 조정할 수 있습니다.
- 사용자 지정 – Windows UI 계층을 사용 하면 쉽게 멋진 UI를 만들 수 있을 뿐 아니라, 전체 범위의 애니메이션 형식을 사용 하 여 사용자 지정 그라데이션으로 새롭고 놀라운 환경을 만들 수 있습니다.

Windows UI 계층에서 환경을 만드는 개발자는 다양 한 애니메이션 개념에 액세스 하 여 디자인을 수명으로 가져올 수 있습니다. 이러한 개념 중 하나를 사용 하 여 속성 또는 하위 채널 구성 요소 (해당 하는 경우)에 애니메이션 효과를 CompositionObject 수 있습니다.

> [!NOTE]
> CompositionObject의 모든 속성이 애니메이션 효과 인 것은 아닙니다. 속성이 애니메이션 효과 인지 여부를 확인 하려면 개별 CompositionObject 설명서를 참조 하세요.

> [!NOTE]
> 하위 _채널_ 이라는 용어는 속성의 구성 요소 형태를 나타냅니다. 예를 들어 Vector3 Offset 속성의 X 또는 XY 하위 채널이 있습니다.

| 애니메이션 개념 | 설명 |
| ----------------- | ----------- |
| [Key프레임 애니메이션을 사용 하는 시간 기반 동작](time-animations.md)  | Key프레임 애니메이션은 일정 기간 동안 전체 동작 환경을 직접 제어 하는 데 사용 됩니다. 기존 keyframed 방식으로 동작의 시작, 종료, 보간 범위를 설명 하는 개발자입니다. |
| [ExpressionAnimations 있는 상대적 동작](relation-animations.md)  | ExpressionAnimations은 한 개체의 속성 동작이 다른 개체의 속성을 기준으로 어떻게 구동 되는지 설명 하는 데 사용 됩니다. 개발자는 참조 기반 관계를 정의 하는 수학 방정식을 정의 합니다. |
| ImplicitAnimations | 이러한 애니메이션은 트리거 기반 이며 핵심 앱 논리와 별도로 정의 됩니다. ImplicitAnimations는 애니메이션이 직접 속성 변경에 대 한 응답으로 발생 하는 방법 및 시기를 설명 하는 데 사용 됩니다. |
| [입력 애니메이션이 있는 입력 기반 동작](input-driven-animations.md)  | 입력 애니메이션은 개발자가 터치 또는 다른 입력 소프트웨어나을 통해 조작 기반 동작을 설명할 수 있는 일련의 시나리오를 포함 합니다. 이러한 애니메이션은 활성 사용자 입력 또는 제스처에 따라 결정 됩니다. |
| [NaturalMotionAnimations를 사용 하는 물리학 기반 동작](natural-animations.md)  | NaturalMotionAnimations는 실제 힘 중심 동작을 기반으로 하는 자연 스러운 동작 환경을 설명 하는 데 사용 됩니다. 개발자는 시간을 정의 하는 대신 동작의 특징 (예: 스프링의 댐핑 ratio)을 정의 합니다. |