---
author: stevewhims
description: 이 항목에서는 c + +에 존재 하는 값의 다양 한 종류에 설명 합니다. 여러분가 듣지 lvalue 및 rvalue, 하지만 다른 종류 너무.
title: 값 범주 및 자신에 대 한 참조
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 이동, 착신 전환, 값 범주, 이동 의미 체계, 완벽 한 전달, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867852"
---
# <a name="value-categories-and-references-to-them"></a>값 범주 및 자신에 대 한 참조
이 항목에서는 c + +에 존재 하는 값 (및 값에 대 한 참조)의 다양 한 종류에 설명 합니다. 여러분가 듣지 *lvalue* 및 *rvalue*하지만이 항목에 게 제공 하는 용어에 쿼리하고 간주 하지 않을 수도 있습니다. 하 고 다른 종류의 값을 너무 않습니다.

모든 식 c + +에서이 항목에서 설명 하는 범주 중 하나에 속하는 값이 생성 됩니다. C + + 언어, facilies, 및 이러한 값 범주와 자신에 대 한 참조의 적절 한 이해를 제안 하는 규칙의 여러 측면 있습니다. 예, 값을 복사 하 고, 값을 이동 하 고, 다른 함수에 로그온 할 값을 전달 하는 값의 주소를 수행 합니다. 이 항목에 깊이에서 이러한 측면의 모든 이동 하지 않습니다 수 있지만 쿼리하고 확실 하 게 이해에 대 한 기본 정보를 제공 합니다.

이 항목의 정보는 id 및 movability [Stroustrup, 2013]의 두 독립 속성 값 범주의 Stroustrup의 분석 측면에서 둘러싸인 합니다.

## <a name="an-lvalue-has-identity"></a>Lvalue id를 가진
무엇을 의미 *id*가 하는 값에 대 한? 또는 있는 경우 (취할 수) 값의 메모리 주소 값은 identity 뒤에 안전 하 게를 사용 하 고 있습니다. 이러한 방법으로 수행할 수 있는 작업 비교 수보다 많은 단위 값의 내용을: 비교 하거나 id로 구별할 수 있습니다.

*lvalue* id를 가진 합니다. "Lvalue"에서 "l" (와 같이 왼쪽-hand-측면이 이루는 배정이) "left"의 약어 인지만 기록 이자의 문제 되었습니다. C + +에서는 lvalue의 왼쪽 *또는* 배정의 오른쪽에 나타날 수 있습니다. "Lvalue"에서 "l" 다음 하지 실제로 도움이 이해 및 이러한 정의할 수 있습니다. Lvalue 호출 기능 id를 가진 값을 인지 이해에 해야 합니다.

Lvalue가 하는 식의 예로: 명명 된 변수 또는 상수입니다. 또는 대 한 참조를 반환 하는 함수입니다. Lvalue *하지* 않은 식의 예로: 임시; 또는 값으로 반환 하는 함수입니다.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

이제 lvalue id가 하는 true 문을 이지만, xvalues 하므로 수행 합니다. 어떤는 *xvalue* 는이 항목의 뒷부분에 나오는으로 더 이동 됩니다. 이제 방금는 glvalue, "lvalue 일반화"에 대 한 호출 값 범주 유의 합니다. Glvalues의 상위 집합 lvalue (로 알려져 *클래식 lvalue*) 및 xvalues 모두 포함합니다. 따라서 동안 참이 "lvalue id를 가진", id가이 문서에서 전체 집합에는이 그림에서와 같이 집합 glvalues,입니다.

![Lvalue id를 가진](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue는 이동할 수 있습니다. lvalue 없습니다
하지만 glvalues 없는 값입니다. 따라서는 메모리 주소를 가져올 *수 없습니다* (또는 유효한가 사용할 수 없게) 값입니다. 살펴본 위 코드 예제에서는 이러한 일부 값입니다. 이 점은 단점 처럼 소리합니다. 하지만 팩트 값을 활용에서 (이 일반적으로 저렴) *이동할* 수 있는 즉 like 하지 않고 (이 일반적으로 비용이 많이 드는)에서 복사 합니다. 값에서 이동 되도록 사용 되는 현재 위치에서 더 이상 인지 하는 것을 의미 합니다. 따라서 수에 사용 되는 위치에 액세스 하는 동안을 절감할 수 할 사항입니다. 시기 및 *방법* 값이이 항목에 대 한 범위를 벗어났습니다 이전할의 토론 합니다. 이 항목에 대 한 바로 이동할 수 있는 값을 *rvalue* (또는 *클래식 rvalue*)으로 알 수 있는지 확인 해야 합니다.

"Rvalue"에서 "r"는 "오른쪽" (와 같이 오른쪽-hand-측면이 이루는 배정이)의 약어입니다. 하지만 rvalue, 및 할당 외부 rvalue에 대 한 참조를 사용할 수 있습니다. "Rvalue"에서 "r"을 집중적으로 작업을 한다면입니다. 이동할 수 있는 값은 rvalue 호출 기능을 이해 하려면만 해야 합니다.

이 그림에서와 같이 lvalue은 이와 반대로 움직일 수 있는 없습니다. 이동 하는 lvalue *lvalue*정의 준수 것 하 고 계속는 lvalue가 액세스할 수 있도록 예상 매우 적절 하 게 하는 코드에 대 한 예기치 않은 문제가 수 있습니다.

![Rvalue는 이동할 수 있습니다. lvalue 없습니다](images/is-movable.png)

Lvalue를 이동할 수 없습니다. 하지만 있는지 *는* 일종의 이동할 수 있는 glvalue (id 가진는 문서 집합)&mdash;발표 중 알고 있는 경우 (되는 이동 후에 액세스 하지 않도록 주의 포함)&mdash;는 xvalue 되 고 있습니다. 이 아이디어 한번 더 아래 값 범주의 전체 그림에 살펴보기 때 초반 하겠습니다 했습니다.

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue 참조 및 참조 (영문) 바인딩 규칙
이 섹션에서는 rvalue에 대 한 참조를 포함 하는 구문은 소개 합니다. 이동 하 고, 전달할 많이 치료로 이동 하려면 다른 항목에 대 한 대기 해야 하지만 rvalue 참조 하 여 해결 된 문제는 것입니다. Rvalue 참조 살펴보기 전에 먼저 해야이 대 한 명확 하 게 하려면 `T&` &mdash;것은 우리가 했을 때 이전 된 호출 방금 "참조" 합니다. "Lvalue (비 const) 참조"를 참조의 사용자 쓸 수 있는 값을 참조 하는 것입니다.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Lvalue 참조 lvalue 되지만 rvalue 적용 되지 않음에 바인딩할 수 있습니다.

Lvalue const 대 한 참조 자료가 다음 (`T const&`)를 연결할 *수 없는* 참조 (영문)의 사용자 (예: 상수) 쓰기 개체를 참조 하는 합니다.

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Lvalue const 참조 lvalue 또는 rvalue 바인딩할 수 있습니다.

형식의 rvalue에 대 한 참조를 포함 하는 구문은 `T` 으로 작성 된 `T&&`합니다. 움직일 수 있는 값을 참조 하는 rvalue 참조&mdash;값을 해당 내용을 것 (예: 임시) 사용 된 후에 유지할 필요가 없습니다. 전체 포인트 이후에서 이전할 (함으로써 수정 하 고) 값에 바인딩된 rvalue 참조, `const` 및 `volatile` 한정자 (cv 한정자) rvalue 참조에 적용 되지 않습니다.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Rvalue 참조는 rvalue에 바인딩합니다. 오버 로드 확인는 rvalue *가 선호* 하는 lvalue const 참조에 보다 rvalue 참조에 바인딩할 수 측면에서 실제로 하지만, 말한 것 처럼 rvalue 참조 참조 하기 때문에 값 (예는 이동 생성자에 대 한 매개 변수)를 유지 하지 않아도 가정 내용이 rvalue 참조 lvalue에 바인딩할 수 없습니다.

Rvalue 하 여 값 인수는 예상, 복사 생성 (나를 통해 이동 건설의 rvalue는 xvalue 이면) 위치를 전달할 수도 있습니다.

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>glvalue는 id입니다. prvalue 하지 않습니다.
이 단계에서 id를 가진 기능을 확인 합니다. 고가 움직일 수 있는 이란 무엇 이며 어떤 되지 않습니다. 하지만 하지 않은 아직 명명 된 값의 집합 id가 해당 *하지 않습니다* . 해당 집합을 *prvalue*또는 *순수 rvalue*이라고 합니다.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Lvalue가 id prvalue 하지 않습니다.](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>값 종류의 전체 그림
정보 및 단일, 큰 그림에 대 한 위의 그림을 결합 하 여만 남아 있습니다.

![값 종류의 전체 그림](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Id를 가진 glvalue (일반화 lvalue).

### <a name="lvalue-im"></a>lvalue (i\ & \!m)
Lvalue (glvalue 종류)는 id가 있지만 이동할 수 없습니다. 이들은 전달 하는 주위 참조 (영문) 또는 const 참조 또는 값으로 복사 하는 것은 저렴 하는 경우 일반적으로 읽기 / 쓰기 값입니다. Lvalue는 rvalue 참조에 바인딩할 수 없습니다.

### <a name="xvalue-im"></a>xvalue (i\ & m)
Glvalue, 유형의 혼합 환경 시나리오도 rvalue의 종류는 xvalue는 id가 있으며 움직일 수 있는 이기도 합니다. 이 기능이 필요할 복사 하는 것은 비용이 많이 문제 때문에 이동 하기로 했을 때 erstwhile lvalue 수 하 고 나중에 액세스 하지 않도록 주의 수 있습니다. Lvalue는 xvalue 변환 하는 방법을 다음과 같습니다.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

위의 코드 예제에는 아직 이동 하지 아무것도 합니다. xvalue 명명 되지 않은 rvalue 참조를 lvalue 캐스팅 하 여 방금 만든 했습니다. 여전히 lvalue 이름을 사용 하 여 식별할 수 있습니다. 하지만 xvalue 것 처럼 지금 *가능* 을 이동 합니다. 이러한 작업에 대 한 이유는 다른 항목에 대 한 대기 해야 다음과 같은, 실제로 어떤 이동 합니다. 하지만 "xvalue" 의미 "전문가 전용 으로"에 도움이 되는 경우에 "x" 생각할 수 있습니다. Lvalue는 xvalue (rvalue 종류)으로 캐스팅, 하 여 값은 다음 rvalue 참조에 바인딩된 되 고의 수 됩니다.

다음은 xvalues의 다른 두 예&mdash;명명 되지 않은 rvalue 참조를 반환 하는 함수를 호출 하 고는 xvalue의 멤버에 액세스 합니다.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
(순수 rvalue, rvalue 유형의) prvalue id를 없지만 이동할 수 있습니다. 이러한 옵션은 일반적으로 많은 임시 공간에 값, 또는 glvalue 하지 않은 다른 모든 식 평가의 결과 반환 하는 함수를 호출의 결과

### <a name="rvalue-m"></a>rvalue (m)
Rvalue는 이동할 수 있습니다. Rvalue *참조* 는 항상 rvalue (내용을 포함 하는 것으로 가정 보존할 필요가 없습니다 값)를 참조 합니다.

하지만 자체 rvalue 참조는 rvalue가? (예: 위의 xvalue 코드 예제에 표시 된 것과) *명명 되지 않은* rvalue 참조 되는 xvalue rvalue는 예, 합니다. 이동 생성자 등는 rvalue 참조 함수 매개 변수에 바인딩되어야을 선호 합니다. 이와 반대로 (및 아마도 counter-intuitively), rvalue 참조는 이름이 경우 해당 이름으로 구성 된 식은 lvalue 합니다. 따라서 해당 rvalue 참조 (영문) 매개 변수는 바인딩할 수 *없습니다* . 이렇게 하기 위해 쉽게 이지만&mdash;방금 다시 명명 되지 않은 rvalue 참조 (xvalue)으로 캐스팅 합니다.

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\ (영문) \!m
Identity 없으면 하 고 이동할 수 없습니다는 값의 종류는 아직 설명 하지 않은 하나의 조합입니다. 하지만 해당 범주 c + + 언어의 유용한 아이디어 아니므로 무시할 수는 있습니다.

## <a name="reference-collapsing-rules"></a>규칙 참조 (영문) 축소
다른 아웃 (lvalue 참조에 대 한 lvalue 참조 또는 rvalue 참조에 대 한 rvalue 참조) 식에 대 한 여러 like 참조 하나를 취소 합니다.

- `A& &` 으로 축소 하는 `A&`합니다.
- `A&& &&` 으로 축소 하는 `A&&`합니다.

식에 대 한 참조와는 달리 다중 lvalue 참조도 축소 합니다.

- `A& &&` 으로 축소 하는 `A&`합니다.
- `A&& &` 으로 축소 하는 `A&`합니다.

## <a name="forwarding-references"></a>착신 전환 참조
이 마지막 섹션 했을 때 앞서 설명한, 다른 개념을 *참조 (영문)를 전달*하는 rvalue 참조를 비교해 서 보여줍니다.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 앞서 설명한 것 처럼 rvalue 참조가 됩니다. Const 및 일시적 rvalue 참조에 대 한 적용 되지 않습니다.
- `foo` **A**형식의 rvalue만 허용합니다.
- 이유 rvalue 참조 (예: `A&&`) 존재 전달 되 고 임시 (또는 기타 rvalue)의 대/소문자에 최적화 된 오버 로드를 작성할 수 있도록 합니다.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` *참조 (영문)를 전달*됩니다. 전달 하면 여부에 따라 `bar`, 형식 **_Ty** const/비 const 일시적/비휘발성와 독립적으로 될 수 있습니다.
- `bar` 모든 lvalue 또는 형식 **_Ty**의 rvalue를 허용합니다.
- 사이트로 전달 참조 lvalue를 전달 하면 `_Ty& &&`, lvalue 파일에 대 한 참조를 축소 하는 `_Ty&`합니다.
- 사이트로 전달 참조는 rvalue를 전달 하면 `_Ty&& &&`, rvalue 참조를 축소 하는 `_Ty&&`합니다.
- 참조를 전달 하는 이유 (같은 `_Ty&&`) 존재는 *하지* , 최적화를 위해 하지만 항목에 전달 기능을 수행 하 고 효율적이 고 투명 하 게에 전달 하 합니다. 자신이 (업데이트를 쓰거나 밀접 하 게 연구) 라이브러리 코드 하는 경우에 착신 전환 참조 발생할 가능성이&mdash;공장 함수 생성자 인수를 전달 하는 예입니다.

## <a name="sources"></a>Sources
* \[Stroustrup, 2013\] B. Stroustrup: c + + 프로그래밍 언어, 네번째 Edition입니다. Addison-Wesley 합니다. 2013 합니다.
