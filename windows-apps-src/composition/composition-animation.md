---
author: jwmsft
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 컴퍼지션 애니메이션
description: 키 프레임 및 식 애니메이션을 사용하여 많은 컴퍼지션 개체와 효과 속성에 애니메이션 효과를 줄 수 있으므로 UI 요소의 속성을 시간에 따라 또는 계산을 기준으로 변경할 수 있습니다.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 38f9d0daf230007d1d32a7d2187d54baa90986e5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5988088"
---
# <a name="composition-animations"></a>컴퍼지션 애니메이션

Windows.UI.Composition API를 사용하면 통합된 API 계층에서 작성자 개체를 만들고, 변환하고, 조작할 수 있으며 작성자 개체에 애니메이션 효과를 줄 수 있습니다. 컴퍼지션 애니메이션은 응용 프로그램 UI에서 애니메이션을 실행하는 강력하고 효율적인 방법을 제공합니다. 컴퍼지션 애니메이션은 UI 스레드와 독립적인 60FPS에서 애니메이션이 실행되도록 하고 시간뿐만 아니라 입력 및 기타 속성을 사용하여 멋진 애니메이션 실행 환경을 구축하기 위한 유연성을 제공하도록 처음부터 디자인되었습니다.

## <a name="motion-in-windows"></a>Windows의 동작

동영상 같은 동작 디자인이라고 볼 수 있습니다. 매끄러운 전환은 스토리에 대한 집중도를 높이고 생동감 있는 경험을 선사합니다. 영화 사용자 작업에서 다음 주요 느낌을 디자인에 해당 느낌을 초대할 수 있습니다 했습니다. 동작은 종종 사용자 인터페이스와 사용자 환경 간에 차별화 요소입니다.

Windows UI 플랫폼의 기본 빌딩 블록을 CompositionAnimations 응용 프로그램의 UI에서 동작 경험을 만들 수는 강력 하 고 효율적인 방법을 제공 합니다. 애니메이션 엔진 처음부터 등록 하도록 만들어졌습니다 활동에 UI 스레드와 독립적인 60fps 실행 되는지 확인 합니다. 이러한 애니메이션은 시간, 입력 및 다른 속성에 따라 혁신적인 동작 경험을 구축 하는 유연성을 제공 하기 위해 설계 되었습니다.

### <a name="examples-of-motion"></a>동작의 예

다음은 앱 내에서 이루어지는 동작의 예입니다.

여기에 나오는 앱은 항목 이미지가 "계속해서" 다음 페이지의 머리글이 되도록 연결된 애니메이션을 사용하여 항목 이미지에 애니메이션 효과를 줍니다. 이 효과는 전환 시 사용자 컨텍스트를 유지하는 데 도움이 됩니다.

![연결 된 애니메이션의 예](images/animation/connected-animation-example.gif)

여기서는 깊이감, 원근감 및 운동성을 주기 위해 UI가 스크롤하거나 이동할 때 시각적 시차 효과를 통해 여러 개체를 서로 다른 속도로 움직입니다.

![목록 및 배경 이미지를 사용한 시차의 예](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>CompositionAnimations를 사용 하 여 동작을 만들 수

UI에서 동작을 생성 하기 위해 개발자 애니메이션 (여기에 링크 스토리 보드를), XAML 또는 시각적 계층에 액세스할 수 있습니다. 시각적 계층에 애니메이션 개발자에 게는 일련의 혜택을 제공합니다.

- 기존 UI 스레드 바인딩된 애니메이션, Windows UI 플랫폼에서 애니메이션 하는 대신 성능 – 60fps 매끄러운 동작 경험을 사용 하도록 설정 하는 독립적인 스레드에서 작동 합니다.
- 템플릿 모델 – Windows UI 계층의 애니메이션은 템플릿입니다, 의미 여러 개체에서 단일 애니메이션을 사용할 수 있으며 속성을 조정할 또는 이전을 방해 한다는 걱정 없이 매개 변수를 사용 합니다.
- 사용자 지정 – Windows UI 계층 뿐만 아니라 간편 하 게 아름 다운 UI를 만들기 위해 하지만 애니메이션 형식, 전체 범위를 사용 하 여 새롭고 놀라운 만들 수의 사용자 지정 그라데이션으로 경험

Windows UI 계층에서 경험을 만드는 개발자를 다양 한 디자인을 가져오는 애니메이션 개념에 액세스할을 수 있습니다. 속성에 애니메이션을 적용 하거나 구성 요소 (있는 경우) 모든 CompositionObject의 subchannel 이러한 개념 중 하나를 사용할 수 있습니다.

> [!NOTE]
> CompositionObject의 모든 속성은 애니메이션을 적용 합니다. 속성에 애니메이션을 적용 되는지 확인 하려면 개별 CompositionObject의 설명서를 참조 하세요.

> [!NOTE]
> 용어 _하위 채널_ 속성 형식의 구성 요소를 가리킵니다. 예를 들어, X, 또는 XY Vector3 Offset 속성의 subchannel 합니다.

| 애니메이션 개념 | 설명 |
| ----------------- | ----------- |
| [KeyFrameAnimations로 시간 기반 동작](time-animations.md)  | KeyFrameAnimations 직접 전체 시간 동안 동작 경험을 제어 하는 데 사용 됩니다. 동작의 시작, 종료, 사이 보간 및 기존 키프레임 방식에서 기간을 설명 하는 개발자. |
| [ExpressionAnimations 사용 하 여 상대 동작](relation-animations.md)  | ExpressionAnimations는 다른 개체의 속성을 기준으로 개체의 속성의 동작 해야 구동 하는 방법을 설명 하는 데 사용 됩니다. 개발자 참조 기반 관계를 정의 하는 수학적 수식을 정의 합니다. |
| ImplicitAnimations | 이러한 애니메이션 트리거 기반 되며 핵심 앱 논리에서 개별적으로 정의 됩니다. ImplicitAnimations는 애니메이션 직접 속성 변경에 대 한 응답으로 발생 방법과 시기에 대해 설명 하는 데 사용 됩니다. |
| [입력 애니메이션을 사용 하 여 입력 기반 동작](input-driven-animations.md)  | 입력된 기반 애니메이션에는 터치 또는 다른 입력된 형식을 통해 조작 기반 동작을 설명 하는 개발자를 사용 하는 시나리오의 집합을 포함 합니다. 이러한 애니메이션 활성 사용자 입력 또는 제스처에 따라 달라 집니다. |
| [NaturalMotionAnimations 사용 하 여 물리학 기반 동작](natural-animations.md)  | NaturalMotionAnimations는 실제 경험 강제로 제어 동작 친숙 하 고 자연 스러운 동작을 설명 하는 데 사용 됩니다. 시간을 정의 하는 대신 개발자 정의 (예: 스프링의 비율 감쇠) 동작의 특성 |