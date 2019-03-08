---
description: 이 항목에서는 c + +에 있는 값의 다양 한 범주를 설명 합니다. Lvalue 및 rvalue에 얘기가 여러분은 밖에도 다른 종류 너무 합니다.
title: 값 범주 및에 대 한 참조
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10 uwp, 표준, c + +, cpp, winrt, 프로젝션, 이동, 전달, 값 범주, 이동 의미 체계, 완벽 한 전달, lvalue를 rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593018"
---
# <a name="value-categories-and-references-to-them"></a>값 범주 및에 대 한 참조
이 항목에서는 c + +에 존재 하는 값 (및 값에 대 한 참조)의 다양 한 범주를 설명 합니다. 여러분 봤 *lvalue* 하 고 *rvalue*,이 항목에서 제공 하는 측면에서 그 중 간주 하지 않을 수도 있지만. 하 고 다른 종류의 값을 너무 않습니다.

이 항목에서 설명 하는 범주 중 하나에 속하는 값을 생성 하는 c + +의 모든 식입니다. facilies, c + + 언어와 값 범주 및 해당 참조를 적절 한 이해를 요구 하는 규칙의 측면이 있습니다. 예를 들어, 값을 복사 하 고, 값을 이동 하 고, 다른 함수에 값을 전달 된 값의 주소는 중입니다. 이 항목에서는 모든 측면을 심층적으로 이동 하지 않습니다 하지만 그 중 확실 하 게 이해에 대 한 기본 정보를 제공 합니다.

이 항목의 정보를 id 및 movability [2013 Stroustrup]의 두 가지 독립적인 속성 값 범주의 Stroustrup의 분석 측면에서 구성 됩니다.

## <a name="an-lvalue-has-identity"></a>Lvalue는 id
무엇을 의미 있는 값에 대 한 *identity*? 있는 경우 (수행할 수 있는) 값의 메모리 주소 및 id를 가진 값을 안전 하 게 사용 합니다. 이렇게 할 수 있는 비교 이상 값의 내용을: 비교 하거나 id로 구분할 수 있습니다.

*lvalue* id입니다. 이제 "lvalue"에서 "l" "left" (예: 왼쪽-hand의 쪽은 할당)의 약어는 에서만 기록 관심 문제입니다. C + +에서는 lvalue 왼쪽에 나타날 수 있습니다 *또는* 할당의 오른쪽에 있습니다. "Lvalue"에서 "l" 그런 다음 실제로 도움이 되지 이해 하거나 용도 정의 합니다. 이라고 하는 lvalue에 id가 있는 값 임을 이해에 필요 합니다.

Lvalue는 식의 예로: 명명 된 변수 또는 상수입니다. 또는 대 한 참조를 반환 하는 함수입니다. 사용 되는 식의 예 *되지* lvalue 포함: 임시; 또는 값으로 반환 하는 함수입니다.

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

이제 lvalue 개별성 문이 true 이지만, 그렇게 xvalues 합니다. 에 자세히 살펴보겠습니다를 *xvalue* 이 항목의 뒷부분에 나오는 됩니다. 이제 방금 glvalue를 "일반화 lvalue"에 대 한 호출 값 범주는 주의 합니다. Glvalues의 상위 집합 둘 다 lvalue 포함 (라고도 *고전 lvalue*) 및 xvalues 합니다. 따라서 while "lvalue에 id가" true 이면이 그림과 같이 id가 있는 항목의 전체 집합은 glvalues, 집합입니다.

![Lvalue는 id](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue는 이동할 수 있습니다. lvalue가 아닙니다.
이 밖에도 glvalues 하지 않은 값입니다. 결과적으로 가지는 값 *없습니다* 의 메모리 주소를 가져옵니다 (또는 유효 하는 데 사용할 수 없게). 위의 코드 예제에 이러한 일부 값에 살펴보았습니다. 단점은 처럼 생각 합니다. 하지만 실제로 값의 이점은 즉 할 수 있습니다 *이동* 후 (이 일반적으로 저렴 한) 대신 (이 일반적으로 비용이 많이 드는)를 복사 합니다. 값에서 이동 하는 위치에 더 이상는 의미입니다. 따라서는 위치에 액세스 하려고 하지 않아야 할 사항입니다. 시기에 대 한 자세한 내용은 및 *어떻게* 이동 값이이 항목의 범위를 벗어났습니다. 이 항목에 대 한 바로 알아야 이동 된 값 이라고 하는 *rvalue* (또는 *고전 rvalue*).

"Rvalue"의 "r"은 "오른쪽" (예: 오른쪽-hand의 쪽은 할당)의 약어입니다. 하지만 rvalue를 및 할당 외부 rvalue에 대 한 참조를 사용할 수 있습니다. "Rvalue"의 "r"에서 가장 중점적으로 그러면 됩니다. 부르는 rvalue 이동할 수 있는 값 임을 이해에 필요 합니다.

이 그림에서와 같이 lvalue은 반대로 이동할 수 없습니다. 이동 하는 lvalue는 정의 무시 *lvalue*, 것이 합리적으로 매우에 계속 lvalue를 액세스할 수 있게 되기를 예상 하는 코드에 대 한 예기치 않은 문제가 및 합니다.

![Rvalue는 이동할 수 있습니다. lvalue가 아닙니다.](images/is-movable.png)

Lvalue를 이동할 수 없습니다. 하지만 거기 *는* glvalue (identity 사용 하 여 작업 집합) 이동할 수 있는 유형의&mdash;무엇을 알고 있는 경우 (하지 않도록 주의 해야 이동한 후 액세스 포함)&mdash;는 xvalue입니다. 한 번 더 아래 값 범주의 전체 그림을 그릴 때 보면이 아이디어를 다시 방문할 예정입니다.

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue 참조 및 참조 바인딩 규칙
이 섹션에서는 rvalue에 대 한 참조를 포함 하는 구문은 소개 합니다. 다른 항목 이동 및 전달, 상당한 처리로 이동할 때까지 대기 해야 하지만를 rvalue 참조로 해결 되는 문제는 합니다. Rvalue 참조를 살펴보기 전에 그러나에서는 먼저이 대 한 명확 하 게 `T&` &mdash;가장에서는 한 이전 호출 된 "참조만"입니다. "(비 const) lvalue 참조를" 참조의 사용자 작성할 수 있는 값으로 참조 하는 것입니다.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Lvalue 참조는 lvalue 아니라 rvalue에 바인딩할 수 있습니다.

Const lvalue 참조 한 다음 (`T const&`)는 개체를 참조 하는 참조의 사용자 *없습니다* 쓰기 (예를 들어, 상수).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Const lvalue 참조는 lvalue 또는 rvalue에 바인딩할 수 있습니다.

형식 rvalue에 대 한 참조를 포함 하는 구문은 `T` 으로 기록 됩니다 `T&&`합니다. Rvalue 참조는 이동 가능한 값을 참조&mdash;값을 해당 내용을 것 (예를 들어 임시) 사용한 후에 유지할 필요가 없습니다. 전체 지점 이후에서 이동할 (수정 함으로써) 값에 바인딩된 rvalue 참조 `const` 고 `volatile` 한정자 (cv 한정자)는 rvalue 참조에 적용 되지 않습니다.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Rvalue는 rvalue를 바인딩합니다. 오버 로드 확인을 rvalue 측면에서 말하자면 *선호* 보다는 rvalue 참조를 const lvalue 참조로 바인딩되어야 합니다. 하지만, 말한 것 처럼 rvalue 참조 하기 때문에 값 (즉, 이동 생성자의 매개 변수)를 유지할 필요가 가정 내용을 rvalue 참조가 lvalue에 바인딩할 수 없습니다.

또한 값으로 인수를 필요한 곳에서 복사 생성을 통해 (또는 rvalue를 xvalue 경우 이동 생성을 통해)는 rvalue을 전달할 수 있습니다.

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>glvalue가 id입니다. prvalue가 않습니다.
이 단계에서는 id가 무엇을 알고 있습니다. 및 이동할 수 란 무엇 이며 그렇지 않은 알 수 있습니다. 에서는 아직 아직 하 명명 된 값 집합이 있지만 *하지* id가 있습니다. 집합 이라고 합니다 *prvalue*, 또는 *순수 rvalue*합니다.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Lvalue는 id입니다. prvalue가 않습니다.](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>범주 값의 전체 개요
정보 및 위의 그림은 단일, 큰 그림으로 결합 하 여만 유지 됩니다.

![범주 값의 전체 개요](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
glvalue (일반화 된 lvalue)에 id가 있습니다.

### <a name="lvalue-im"></a>lvalue (있습니까\&\!m)
Lvalue (glvalue의 kind)는 id가 있지만 이동할 수 없습니다. 이들은 전달 하는 관련 참조 또는 const 참조 또는 값으로 복사 하는 저렴 하는 경우 일반적으로 읽기 / 쓰기 값입니다. Lvalue는 rvalue 참조에 바인딩할 수 없습니다.

### <a name="xvalue-im"></a>xvalue (있습니까\&m)
xvalue (glvalue, 유형의 뿐만 아니라 특정 유형의 rvalue)는 id가 있으며 이동할 수 이기도 합니다. 이 복사는 저렴 하기 때문에 이동 하기로 다니고 lvalue를 수 있습니다 하 고 나중에 액세스 하지 않도록 주의 수입니다. Lvalue를 xvalue로 변환 하는 방법을 다음과 같습니다.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

위의 코드 예제에서는 아직 이동 하지 아무 것도 합니다. Lvalue를 이름이 지정 되지 않은 rvalue 참조로 캐스팅 하 여 xvalue는 방금 만들었습니다. Lvalue 이름으로 계속 식별할 수 있습니다. 하지만 이제는 xvalue *가능* 이동 하는 중입니다. 이렇게 하는 원인은 다른 항목에 대 한 대기 해야 등 조사 어떤 이동 합니다. 하지만 "xvalue" 의미 "전문가 전용 으로"에 도움이 되는 경우에 있는 "x"를 생각할 수 있습니다. Lvalue를 xvalue (rvalue의 kind)로 캐스팅을 하 여 값 그러면 rvalue 참조 바인딩될 수 있습니다.

다음의 두 가지 다른 예 xvalues&mdash;명명 되지 않은 rvalue 참조를 반환 하는 함수를 호출 하 고는 xvalue의 멤버에 액세스 합니다.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!있습니까\&m)
(순수 rvalue; rvalue 유형의) prvalue id 없지만 이동할 수 있습니다. 임시 개체는 일반적으로, 값 또는 glvalue 없는 다른 식 평가의 결과 반환 하는 함수 호출의 결과

### <a name="rvalue-m"></a>rvalue (m)
Rvalue는 이동할 수 있습니다. Rvalue *참조* 항상 rvalue (유지할 필요가 가정 내용을 값)를 참조 합니다.

하지만를 rvalue 참조 자체 rvalue? *명명 되지 않은* rvalue 참조 (예: 위의 xvalue 코드 예제에 나와 있는 것)는 xvalue은 예,이 rvalue입니다. 이동 생성자의 것과 같은 rvalue 참조 함수 매개 변수에 바인딩할 선호 합니다. 반대로 아마도 counter-intuitively rvalue 참조 이름을 가진 경우 해당 이름으로 이루어진 식은 lvalue입니다. 따라서 것 *없습니다* 는 rvalue 참조 매개 변수를 바인딩할 수 있습니다. 하지만 이렇게 하도록 쉽습니다&mdash;다시 명명 되지 않은 rvalue 참조 (xvalue)으로 캐스팅만 합니다.

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

### <a name="im"></a>\!i\&\!m
Id가 없는 것을 이동할 수 없는 값의 종류에는 아직 설명 하지는 하나의 조합입니다. 하지만 해당 범주에는 c + + 언어의 유용한 아이디어를 하지 않기 때문에, 무시 해도 했습니다.

## <a name="reference-collapsing-rules"></a>참조 축소 규칙
다른 아웃 (lvalue 참조를 lvalue 참조 또는 rvalue 참조에 대 한 rvalue 참조) 식에서 여러 like 참조 하나를 취소 합니다.

- `A& &` 축소 `A&`합니다.
- `A&& &&` 축소 `A&&`합니다.

식에서 참조와 달리 여러 lvalue 참조를 축소 합니다.

- `A& &&` 축소 `A&`합니다.
- `A&& &` 축소 `A&`합니다.

## <a name="forwarding-references"></a>참조를 전달합니다.
이 최종 섹션에 설명한, 다른 개념을 사용 하 여 rvalue 참조를 대조 한 *참조를 전달*합니다.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` 앞서 설명한 것 처럼 rvalue 참조가 됩니다. Const 및 volatile rvalue 참조에 적용 되지 않습니다.
- `foo` 형식 rvalue만 수락 **는**합니다.
- 이유 rvalue 참조 (같은 `A&&`)가 전달 되는 임시 (또는 다른 rvalue)의 경우에 최적화 된 오버 로드를 작성할 수 있도록 합니다.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` *참조를 전달*합니다. 에 전달 된 항목에 따라 `bar`, 형식 **_Ty** const/비 const volatile/비 volatile 독립적으로 일 수 있습니다.
- `bar` 모든 lvalue 또는 rvalue 형식의 수락 **_Ty**합니다.
- 되도록 전달 참조는 lvalue를 전달 하면 `_Ty& &&`, lvalue 참조 축소는 `_Ty&`합니다.
- 되도록 전달 참조는 rvalue를 전달 하면 `_Ty&& &&`에 대 한 rvalue 참조를 축소 하는 `_Ty&&`합니다.
- 참조를 전달 하는 이유 (같은 `_Ty&&`) 존재 됩니다 *하지* 최적화를 위해 있지만 있습니다 통과 하는 데 및 투명 하 고 효율적으로 전달 하도록 합니다. 또는 경우에 쓰기 (밀접 하 게 연구) 라이브러리 코드에 대 한 참조를 전달 발생할 가능성이&mdash;예를 들어 팩터리 함수를 생성자 인수에 전달 합니다.

## <a name="sources"></a>원본
* \[Stroustrup 2013\] B. Stroustrup: C + + 프로그래밍 언어, Fourth Edition입니다. Addison-Wesley. 2013.
