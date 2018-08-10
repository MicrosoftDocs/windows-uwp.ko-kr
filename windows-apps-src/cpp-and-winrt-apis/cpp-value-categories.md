---
author: stevewhims
description: 이 항목에서는 c + +에 존재 하는 값의 다양 한 종류에 설명 합니다. 여러분가 듣지 lvalue 및 rvalue, 하지만 다른 종류 너무.
title: 값 범주
ms.author: stwhi
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 이동, 착신 전환, 값 범주, 이동 의미 체계, 완벽 한 전달, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: 75176334909e0e6bcf81763b543a19b70a4cba73
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/10/2018
ms.locfileid: "2739104"
---
# <a name="value-categories"></a>값 범주
이 항목에서는 c + +에 존재 하는 값의 다양 한 종류에 설명 합니다. 여러분가 듣지 *lvalue* 및 *rvalue*하지만 다른 종류 너무. 모든 식 c + +에서이 항목에서 설명 하는 범주 중 하나에 속하는 값이 생성 됩니다. C + + 언어, facilies, 및 제안 값 범주에 대 한 적절 한 이해 하는 규칙의 여러 측면 있습니다.

## <a name="an-lvalue-has-identity"></a>Lvalue id를 가진
무엇을 의미 *id*가 하는 값에 대 한? 값의 메모리 주소, 해야 (또는 취할 수 있는) 하는 경우 값 id를 가지입니다. 이러한 방법으로 수행할 수 있는 작업 비교 수보다 많은 단위 값의 내용을: 비교 하거나 id로 구별할 수 있습니다.

*lvalue* id를 가진 합니다. "Lvalue"에서 "l" (와 같이 왼쪽-hand-측면이 이루는 배정이) "left"의 약어 인지만 기록 이자의 문제 되었습니다. C + +에서는 lvalue의 왼쪽 *또는* 배정의 오른쪽에 나타날 수 있습니다. "Lvalue"에서 "l" 다음 하지 실제로 도움이 이해 및 이러한 정의할 수 있습니다. Lvalue 호출 기능 id를 가진 값을 인지 이해에 해야 합니다.

이제 lvalue id가 하는 true 문을 이지만, xvalues 하므로 수행 합니다. 어떤는 *xvalue* 됩니다 (하지만, 그 중 전체 치료 범위 여기에 하지 않음)이이 항목의 뒷부분에 나오는에 약간 더 살펴보겠습니다. 이제 방금는 glvalue, "lvalue 일반화"에 대 한 호출 값 범주 유의 합니다. Glvalues의 상위 집합 lvalue (로 알려져 "클래식 lvalue")와 xvalues 모두 포함합니다. 따라서 동안 참이 "lvalue id를 가진", id가이 문서에서 전체 집합에는이 그림에서와 같이 집합 glvalues,입니다.

![Lvalue id를 가진](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue는 이동할 수 있습니다. lvalue 없습니다
하지만 glvalues 없는 값입니다. 따라서는 메모리 주소를 가져올 *수 없습니다* (또는 유효한가 사용할 수 없게) 값입니다. 소리 같은 단점이 있습니다. 실제로 값의 장점은 like 즉 수 있는 *이동* (이 일반적으로 저렴)는 것 보다 복사 하지만 it (이 일반적으로 비용이 많이 드는). 값을 이동 되도록 사용 되는 현재 위치에서 더 이상 인지 하는 것을 의미 합니다. 따라서 수에 사용 되는 위치에 액세스 하는 동안을 절감할 수 할 사항입니다. 시기 및 *방법* 값이이 항목에 대 한 범위를 벗어났습니다 이전할의 토론 합니다. 이 항목에 대 한 값을 이동할 수는 *rvalue*라고 알고 하기만 합니다.

"Rvalue"에서 "r"는 "오른쪽" (와 같이 오른쪽-hand-측면이 이루는 배정이)의 약어입니다. 하지만 rvalue, 및 할당 외부 rvalue에 대 한 참조를 사용할 수 있습니다. "Rvalue"에서 "r"을 집중적으로 작업을 한다면입니다. 이동할 수 있는 값은 rvalue 호출 기능을 이해 하려면만 해야 합니다.

이 그림에서와 같이 lvalue은 이와 반대로 움직일 수 있는 없습니다. 수행할 수 있습니다 하는 경우 다음 상태인 것 안전 하지 않은 (또는 치명적인) 때문에 lvalue를 이동할 수는 없습니다 계속 나중에 액세스할 수 있습니다. 기억 해당 id가 있습니다.

![Rvalue는 이동할 수 있습니다. lvalue 없습니다](images/is-movable.png)

Lvalue를 이동할 수 없습니다. 하지만 있는지 *는* 일종의 이동할 수 있는 glvalue (id 가진는 문서 집합)&mdash;발표 중 알고 있는 경우&mdash;는 xvalue입니다. 이 아이디어 한번 더 아래 값 범주의 전체 그림에 살펴보기 때 초반 하겠습니다 했습니다.

## <a name="an-lvalue-has-identity-a-prvalue-does-not"></a>Lvalue가 id prvalue 하지 않습니다.
이 단계에서 id를 가진 기능을 확인 합니다. 고가 움직일 수 있는 이란 무엇 이며 어떤 되지 않습니다. 하지만 하지 않은 아직 명명 된 값의 집합 id가 해당 *하지 않습니다* . 해당 집합을 *prvalue*또는 "순수 rvalue" 라고 합니다.

![Lvalue가 id prvalue 하지 않습니다.](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>값 종류의 전체 그림
정보 및 단일, 큰 그림에 대 한 위의 그림을 결합 하 여만 남아 있습니다.

![값 종류의 전체 그림](images/value-categories.png)

- Id를 가진 glvalue (일반화 lvalue).
- Lvalue (glvalue 종류)는 id가 있지만 이동할 수 없습니다. 이들은 전달 하는 주위 참조 (영문) 또는 const 참조 또는 값으로 복사 하는 것은 저렴 하는 경우 일반적으로 읽기 / 쓰기 값입니다.
- Glvalue, 유형의 혼합 환경 시나리오도 rvalue의 종류는 xvalue는 id가 있으며 움직일 수 있는 이기도 합니다. 이 기능이 필요할 복사 하는 것은 비용이 많이 문제 때문에 이동 하기로 했을 때 erstwhile lvalue 수 하 고 나중에 액세스 하지 않도록 주의 수 있습니다. xvalue 및 하나를 이동 하는 이유는 lvalue를 설정 하는 방법은 다른 항목에 대 한 대기 해야 합니다. 하지만으로 생각할 수 "xvalue"에서 "x"의 의미에 도움이 되는 경우 "전문가" 합니다.
- (순수 rvalue, rvalue 유형의) prvalue id를 없는 되지는 않지만 이동할 수 있습니다. 이러한 옵션은 일반적으로 리터럴을 많은 임시 공간, 반환 값&mdash;(프로그램 glvalue 없는 식), 식 평가의 결과 되는 모든 항목 또는 값 함수에서 반환한 들어 있는 개체입니다.
- Rvalue는 이동할 수 있습니다.
