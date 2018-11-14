---
author: stevewhims
description: 이 항목에서는 c + +에 존재 하는 값의 다양 한 범주를 설명 합니다. 여러분 봤 lvalue 및 rvalue, 이지만 다른 유형과 있습니다.
title: 값 범주 및 참조
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 이동, 전달 값 범주, 이동, 완벽 한 전달, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: b600c09c3629ce52590daa42b9046fab3784a78f
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6270772"
---
# <a name="value-categories-and-references-to-them"></a>값 범주 및 참조
이 항목에서는 c + +에 존재 하는 값 (및 값에 대 한 참조)의 다양 한 범주를 설명 합니다. 여러분 봤 *lvalue* 및 *rvalue*하지만이 항목에서는 제공 하는 용어로 부분이 간주 하지 않을 수도 있습니다. 하 고 다른 종류의 값을 너무 않습니다.

이 항목에 설명 된 범주 중 하나에 속하는 값을 생성 하는 c + +에서 모든 식입니다. C + + 언어, facilies, 및 이러한 값 범주 및 참조에 대 한 적절 한 이해를 요구 하는 규칙의 측면 있습니다. 예를 들어, 값을 복사 하 고, 값을 이동 하 고, 다른 함수에 값을 전달 하는 값의 주소를 수행 합니다. 이 항목에서는 모든 심층 분석, 측면 이동 하지 않습니다 하지만 그 중 확실 하 게 이해에 대 한 기본 정보를 제공 합니다.

이 항목의 내용은 id와 movability [Stroustrup 2013]의 두 개의 독립 속성 값 범주 중 Stroustrup의 분석 측면에서 프레임이 합니다.

## <a name="an-lvalue-has-identity"></a>Lvalue id를 가진
무엇을 의미 *id*값에 대 한? 값의 메모리 주소 있는 (또는 수행할 수 있는) 하는 경우 값에 id가 다음을 안전 하 게 사용 합니다. 이렇게 할 수 있는 비교 보다 값의 내용: 비교 하거나 id로 구분할 수 있습니다.

*lvalue* id가 있습니다. "Lvalue"에서 "l"는 "왼쪽" (와 같이 왼쪽-hand-측면에서 할당)의 약어는 기록만 관심 함의 되었습니다. C + +에서는 lvalue 왼쪽 *또는* 오른쪽에서 할당에 나타날 수 있습니다. "Lvalue"에서 "l" 그런 다음 하지 실제로 안내 이해할 뿐만 아니라 용도 정의 합니다. 불러서 lvalue id가 하는 값 임을 이해에 필요 합니다.

Lvalue가 식의 예로: 명명 된 변수 또는 상수입니다. 또는 대 한 참조를 반환 하는 함수입니다. Lvalue *하지* 않은 식의 예로: 임시; 또는 값으로 반환 하는 함수입니다.

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

이제는 true 문 lvalue id를 사용할 때 xvalues를 함 있습니다. 어떤는 *xvalue* 은이 항목 뒷부분의으로 더 이동 합니다. 지금은 방금 glvalue, "lvalue 일반화"에 대 한 라는 값 범주는 주의 합니다. Glvalues 상위 lvalue (라고도 *고전 lvalue*) 및 xvalues 모두 포함 되어 있습니다. 따라서 동안 "lvalue id를 가진" true 이면이 그림에서 보듯 id가 있는 작업의 전체 집합은 glvalues의 집합입니다.

![Lvalue id를 가진](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue는 이동할 수 있습니다. lvalue가 않습니다.
하지만 glvalues 되지 않는 값 있습니다. 따라서 값에 대 한 메모리 주소를 가져올 *수 없습니다* (또는 유효를 사용할 수 없게)는 됩니다. 일부 이러한 값 위의 코드 예제에서 살펴본 합니다. 단점은 처럼 생각 합니다. 하지만 실제로 값의 장점 같은 즉에서 (일반적으로 할인 된)는 *이동* 아니라입니다 (일반적으로 비용이 많이 드는)에서 복사 수 있습니다. 값에서 이동 수 위치에 더 이상 더 않음을 의미 합니다. 따라서 수 위치에 액세스 하려고 피해 야 할 사항입니다. 시기 및 *방법* 값이이 항목에 대 한 범위를 벗어납니다. 이동 설명 합니다. 이 항목에 대 한 바로 *rvalue* (또는 *고전 rvalue*)으로 이동할 수 있는 값이 알려진 있는지 확인 해야 합니다.

"Rvalue"에서 "r" "오른쪽" (와 같이 오른쪽-hand-측면에서 할당)의 약어입니다. 하지만 rvalue, 및 rvalue 할당 외부에 대 한 참조를 사용할 수 있습니다. "Rvalue"에서 "r" 하는 것이 하지 것에 집중 합니다. 불러서 rvalue 이동할 수 있는 값 임을 이해에 필요 합니다.

이 그림에서 보듯 lvalue은 반대로, 이동 없습니다. 이동 lvalue *lvalue*정의 준수는 및 예기치 않은 문제가 계속 해 서는 lvalue 액세스할 수 있도록 합리적으로 매우 예상 하는 코드에 대 한 것입니다.

![Rvalue는 이동할 수 있습니다. lvalue가 않습니다.](images/is-movable.png)

Lvalue를 이동할 수 없습니다. 하지만 없습니다 *는* 일종의 이동할 수 있는 glvalue (작업 id로 설정)&mdash;무엇을 알고 있는 경우 (이동한 후 액세스 하나 포함)&mdash;는 xvalue입니다. 우리 다시 돌아가이 아이디어 한 번 더 아래 값 범주 중 전체 그림을 그릴 때 살펴봅니다.

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue 참조 및 규칙 참조 바인딩
이 섹션에서는 rvalue에 대 한 참조에 대 한 구문을 소개 합니다. 이동 하 고 전달의 실질적인 처리로 돌아갑니다 다른 주제에 대 한 기다려 해야 하지만 rvalue 참조 하 여 해결할 수 있는 문제를 모두. Rvalue 참조 살펴보기 전에 하지만, 먼저 해야이 대 한 명확 하 게 `T&` &mdash;것은 우리가 했습니다 이전의 되었습니다 호출 방금 "참조"입니다. "Lvalue (비 const) 참조"를 참조의 사용자 작성할 수 있는 값을 참조 하는 것입니다.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Lvalue 참조 lvalue 아니라 rvalue 바인딩할 수 있습니다.

다음 lvalue const 대 한 참조가 있습니다 (`T const&`), (예: 상수) 쓸 *수 없는* 참조의 사용자는 개체를 참조 하 합니다.

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Lvalue const 참조 rvalue 또는 lvalue에 바인딩할 수 있습니다.

Rvalue 형식에 대 한 참조에 대 한 구문 `T` 로 작성 `T&&`. Rvalue 참조는 이동할 수 있는 값&mdash;값을 내용이 것 (예를 들어 임시) 사용한 후 유지할 필요가 없습니다. 전체 포인트 이후 이동 하려면 (함으로써 수정) 값에 바인딩된 rvalue 참조 `const` 및 `volatile` 한정자 (cv 한정자) rvalue 참조에 적용 되지 않습니다.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Rvalue 참조 rvalue에 바인딩합니다. 오버 로드 확인을는 rvalue *선호* lvalue const 참조를 보다 rvalue 참조를 바인딩하려면 측면에서 실제로 하지만 참조 하기 때문에, 말한 것 처럼 rvalue 참조 값 (예, 이동 생성자에 대 한 매개 변수)을 유지할 필요가 모드인 내용이 rvalue 참조 lvalue에 바인딩할 수 없습니다.

또한 값으로 인수가 필요한 곳, 복사 생성 (또는 통해는 rvalue는 xvalue 경우 이동 생성) rvalue을 전달할 수 있습니다.

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>glvalue는 id입니다. prvalue가 없습니다.
이 단계에서 id가 무엇을 알고 있습니다. 및 이동할 수 있는 란 무엇 이며 어떻게 되지 않는다고 알고 있습니다. 하지만 하지 않은 것 아직 명명 된 값 집합 id가 해당 *하지 않습니다* . 해당 집합 *prvalue*또는 *순수 rvalue*이라고 합니다.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Lvalue는 id입니다. prvalue가 없습니다.](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>값 범주 대 한 전체 그림
정보 및 단일, 큰 그림으로 위 그림을 결합 하 여만 남아 있습니다.

![값 범주 대 한 전체 그림](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Glvalue (범용된 lvalue) id가 있습니다.

### <a name="lvalue-im"></a>lvalue (i\ & \!m)
Lvalue (glvalue 종류)는 id가 있지만 이동할 수 없습니다. 이들은 전달 하는 주변 참조 또는 const 참조 또는 값으로 복사 하는 저렴 하는 경우 일반적으로 읽기-쓰기 값입니다. Lvalue는 rvalue 참조에 바인딩할 수 없습니다.

### <a name="xvalue-im"></a>xvalue (i\ & m)
(Glvalue, 유형의 뿐만 아니라 rvalue 유형의)는 xvalue는 id가 있으며 이동할 수 있는 이기도 합니다. 듭니다 복사 되기 때문에 이동 하 려 하십니까 erstwhile lvalue은이 하 고 나중에 액세스 하지 않도록 주의 수 있습니다. Lvalue는 xvalue 변환 하는 방법을 다음과 같습니다.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

위의 코드 예제에서는 아직 이동 하지 아무 것도 합니다. 방금 lvalue 명명된 rvalue 참조로 캐스팅 한 xvalue를 만들었습니다. 해당 lvalue 이름별 여전히 식별할 수 있습니다. 하지만 xvalue로 이제는 *가능* 이동 합니다. 이 작업을 수행 하는 이유는 다른 항목에 대 한 기다릴 필요도 다음과 같은, 실제로 어떤 이동 합니다. 하지만 의미가 "전문가 전용 으로" 데 도움이 되는 경우 "xvalue"에서 "x" 생각할 수 있습니다. Lvalue는 xvalue (rvalue 종류)로 캐스팅, 해당 값이 됩니다 rvalue 참조에 바인딩할 수 있습니다.

Xvalues의 다른 두 가지 예는&mdash;명명된 rvalue 참조를 반환 하는 함수를 호출 하 고는 xvalue의 구성원에 액세스 합니다.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
(순수 rvalue, rvalue 유형의) prvalue id 없지만 이동할 수 있습니다. 이러한 옵션은 일반적으로 많은 임시 공간에 함수를 호출의 결과 반환 값 또는 glvalue, 되지 않은 다른 식이 계산의 결과

### <a name="rvalue-m"></a>rvalue (m)
Rvalue는 이동할 수입니다. Rvalue *참조* 는 항상 rvalue (유지할 필요가 것으로 간주 내용을 값)를 참조 합니다.

하지만 rvalue 자체 rvalue 참조는? (예: 위의 xvalue 코드 예제에 나와 있는 것) *명명된* rvalue 참조 되는 xvalue 예, 것 이므로 rvalue 합니다. 생성자가 이동 하는 등의 프로그램 rvalue 참조 함수 매개 변수에 바인딩될을 선호 합니다. 반대로 (및 counter-intuitively 아마도) 이름이 rvalue 참조를 해당 이름으로 이루어진 식은 lvalue 경우. 따라서이 rvalue 참조 매개 변수는 바인딩할 수 *없습니다* . 이렇게 하기 쉽지만 이지만&mdash;다시 명명된 rvalue 참조 (xvalue)로 캐스팅 방금 합니다.

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

### <a name="im"></a>\!i\ 및 \!m
Id가 하지과 오해 이동할 수 있는 값의 종류는 아직 검토 하지 않은 한 조합입니다. 하지만 해당 범주 c + + 언어에서 유용한 아이디어 아니므로, 무시할 수 있습니다.

## <a name="reference-collapsing-rules"></a>참조 축소 규칙
다른 아웃 (lvalue 참조에 대 한 lvalue 참조 또는 rvalue 참조에 대 한 rvalue 참조) 하는 식의 여러 like 참조 하나를 취소 합니다.

- `A& &` 축소에 `A&`.
- `A&& &&` 축소에 `A&&`.

식에서 참조와 달리 여러 lvalue 참조를 축소 합니다.

- `A& &&` 축소에 `A&`.
- `A&& &` 축소에 `A&`.

## <a name="forwarding-references"></a>참조를 전달합니다.
이 마지막 섹션에서는 이미 설명한, *참조를 전달*하는 다른 개념을 사용 하 여 rvalue 참조, 대조 합니다.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 앞에서 설명한 것 처럼 rvalue 참조가 됩니다. Const 및 휘발성 rvalue 참조에 적용 되지 않습니다.
- `foo` **A**형식의 rvalue만 허용합니다.
- 이유 rvalue 참조 (예: `A&&`) 존재는 전달 되 고 있는 임시 (또는 다른 rvalue)의 경우에 최적화 된 오버 로드를 작성할 수 있습니다.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` *참조를 전달*됩니다. 로 전달 따라 `bar`, 형식 **_Ty** const/비 const 휘발성/비 휘발성 코드와 개별적으로 될 수 있습니다.
- `bar` 모든 lvalue 또는 형식 **_Ty**의 rvalue 수락합니다.
- 전달 참조 될 lvalue를 전달 하면 `_Ty& &&`, lvalue 참조를 축소 하는 `_Ty&`.
- 전달 참조 될 rvalue를 전달 하면 `_Ty&& &&`, rvalue 참조를 축소 하는 `_Ty&&`.
- 참조를 전달 하는 이유 (같은 `_Ty&&`) 존재는 *하지* 최적화에 대 한 하지만 항목에 전달 무엇을 할 및 투명 하 게 하 고 효율적으로에 전달할 수 있습니다. (쓰거나 밀접 하 게 연구) 라이브러리 코드 하는 경우에 전달 참조 발생할 수 있는&mdash;예를 들어 생성자 인수를 전달 하는 팩터리 함수입니다.

## <a name="sources"></a>Sources
* \[Stroustrup, 2013\] 만약 Stroustrup: c + + 프로그래밍 언어, 네 번째 버전입니다. Addison Wesley 합니다. 2013 합니다.
