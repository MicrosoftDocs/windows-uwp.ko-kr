---
description: 독창성과 창의성을 보여주는 일관적이고 유용한 앱을 만드는 팁입니다.
title: 스타일과 일관성 사이의 적절한 균형 유지(Windows 앱 디자인)
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecfa771d8976b3c9e8cce8c8e9c45a5db5fb9b0a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220276"
---
# <a name="balancing-style-and-consistency"></a>스타일과 일관성 사이의 적절한 균형 유지

 

> 참고: 이 문서는 Windows 10 RS2의 초안입니다. 특징 이름, 용어 및 기능은 변경될 수 있습니다.

제품을 디자인할 때 가장 중요한 것은 고객입니다. 우리는 의도에 부합하는 최고의 디자인을 만들기 위해 노력합니다. 이 문서에서는 일관적인 사용자 환경을 만들기 위한 규칙 준수와 앱을 차별화하는 고유의 기능 및 환경 만들기 사이의 적절한 균형을 유지하는 방법을 살펴보겠습니다. 

 
## <a name="the-importance-of-consistency"></a>일관성의 중요성
일관성이 중요한 이유 앱을 쉽게 사용하려면 일관성이 있어야 합니다. 좋은 디자인의 핵심은 가독성입니다. 앱 전체에서 디자인의 일관성을 유지하면 최종 사용자가 앱 사용 방법을 “다시 배우는” 데 걸리는 시간을 줄일 수 있습니다. 실제 사례를 살펴보자면, 가속 페달은 항상 오른쪽에 있고, 문 손잡이는 항상 시계 반대 방향으로 돌려서 열고, 정지 신호는 항상 빨간색입니다. 

그러나 일관적인 앱이라는 말은 모든 앱의 모양과 동작이 서로 똑같다는 의미가 아닙니다. 다시 실제 사례로 돌아가서, 셀 수 없이 많은 의자 디자인이 있지만 앉는 방법을 다시 배워야 하는 의자는 거의 없습니다. 중요한 요소, 즉 앉는 표면이 일관적이어서 사용자가 사용 방법을 이해하는 데 전혀 어려움이 없기 때문입니다. 

좋은 앱 디자인을 만들 때 겪는 어려움 중 하나는 일관성이 중요한 부분과 독창성이 중요한 부분을 파악하는 것입니다. 

## <a name="the-consistency-spectrum"></a>일관성 스펙트럼
 일관성은 양쪽 끝이 정반대인 스펙트럼으로 생각할 수 있습니다.


![일관성 스펙트럼](images/consistency/consistency-spectrum.png)

다음과 같은 익숙한 환경 요소가 있습니다.
-   정착된 UI 패러다임(마우스 클릭 동락, 요소를 길게 누르기, 비슷한 모양의 아이콘)
-   제품 전반에 적용할 브랜드 요소(입력 체계, 색)

다음과 같은 차별화 요소가 있습니다.
-   제품 고유의 "정신"을 형성하는 요소
-   의도한 폼 팩터에 맞게 환경을 맞춤 구성하는 데 도움이 되는 요소

이 스펙트럼을 가져와 앱의 주요 요소에 적용하여 디자인 모델을 만들어 보겠습니다. 

![디자인 일관성 모델](images/consistency/design-consistency-model.png)

이 모델의 기본 계층은 이미 검증된 일관성 기반을 제공하고 상위 계층은 유연성과 사용자 지정에 집중합니다.  

1. 앱 기본 사항은 모델의 첫 번째 계층인 레이아웃 그리드, [색상표](color.md), [입력 체계 규칙](typography.md) 및 [아이콘 스타일](icons.md)을 구성합니다. 이러한 기본 사항 기능을 일관적으로 적용해야 합니다. 

2. 두 번째 계층의 경우 UWP는 효율성과 유연성 간의 적절한 균형을 유지하는 [공통 컨트롤](../controls-and-patterns/index.md) 핵심 세트를 제공합니다. 또한 Microsoft에서는 동일한 단어를 사용하여 사람들에게 앱 환경을 설명하고 안내할 수 있도록 일관적인 음성과 톤을 설정하는 데 도움이 되는 지침을 작성해 놓았습니다. 그리고 다양한 크기의 디바이스와 입력에 맞게 디자인 규모를 확장할 수 있도록 앱에서 디자인에 사용할 수 있는 패턴 세트를 만들어 두었습니다. 
3. 계층 3에서는 여러 디바이스와 컨텍스트에 맞게 탐색을 맞춤 구성합니다. 예를 들어 휴대폰에서 터치로 탐색하는 방법은 32인치 모니터에서 키보드와 마우스로 탐색하는 방법 또는 84인치 화면에서 HoloLens 제스처와 100포인트 터치로 탐색하는 방법과 다를 것입니다.
4. 계층 4에서는 브랜드 퍼스낼리티를 정의합니다. 브랜드를 강화하고 경쟁업체와 차별화하는 특징적인 디자인 요소는 무엇일까요? 여기서도 마찬가지로 여러 최종 사용자에 맞게 앱을 맞춤 구성합니다. 게이머, 정보 산업 근로자, 초중고 학생 또는 교육자를 위한 앱인가요? 다양한 고객의 고유한 요구 사항은 무엇이며 이들에게 적합한 디자인을 만들 수 있을까요? 똑같은 디자인을 만들지 말고, 다양한 고객에게 더 많은 가치를 제공하는 방법을 계속 고민해야 합니다.  


## <a name="design-principles"></a>디자인 원칙
이 모델을 효과적으로 사용하려면 적절하게 절충하는 데 도움이 되는 디자인 원칙이 필요합니다. 다음은 Microsoft에서 현재 사용 중인 디자인 일관성 원칙입니다.

**모양이 같으면 동작도 같게 만들기**
-   대부분의 사용자는 텍스트 상자 또는 햄버거 컨트롤을 보았을 때 이 컨트롤이 모든 디바이스에서 똑같은 방식으로 작동할 것이라 기대합니다. 이미 정착된 동작을 벗어나야 하는 합리적인 이유가 있는 경우 모양을 다르게 하여 사용자 기대도 다르게 설정해야 합니다.

**한 요소가 기존 요소 또는 규칙과 매우 비슷하게 보이는 경우 똑같이 만드는 방안을 고려**
-   "새 문서" 아이콘이 필요합니다. 사용자가 이미 알고 있는 기존 아이콘을 사용할 수 있는데, 굳이 별 차이 없는 새 아이콘을 디자인할 이유가 있을까요?

**일관성보다는 사용 편의성이 우선**
-   일관성보다는 사용 편의성이 더 중요합니다. 경우에 따라 사용 편의성을 향상하기 위해 새로운 컨트롤 또는 동작을 개발해야 합니다. 휴대폰을 한 손으로 사용할 경우 그에 따른 고유의 어려움이 있습니다. 80인치 화면을 사용할 때도 마찬가지입니다. 사용자에게 전문가가 된 듯한 느낌을 주어야 좋은 디자인입니다. 

**참여가 중요**
-   디자인이 지루하면 안 됩니다. 모든 것이 평평하고 같은 색에 사각형밖에 없다면 과연 고객이 그 앱을 선택해서 사용하고 싶을까요? 즐겁게 만들어야 합니다. 가독성을 해치지 않으면서도 놀라움을 주는 새로운 요소를 도입하세요. 

**동작의 발전**
-   까다로운 부분으로, 산업이 발전하면 새로운 규칙이 정착됩니다. 현재 동작이 서서히 사라질 수 있으며, 일관적인 동작에 새로운 표준을 채택해야 할 수도 있습니다. 손가락을 모아 확대/축소 동작을 보세요. 과거에는 +/- UI를 사용하여 확대/축소하는 동작이 일반적인 기대였지만 현대식 UI에서는 손가락을 모아 확대/축소가 일반적입니다. 새로운 환경 패러다임을 살펴보고 개선해야 합니다. 
