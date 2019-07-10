---
description: C++에 존재하는 값의 다양한 범주에 대해 설명합니다. 분명히 lvalues와 rvalues에 대해 들어 보았을 것이지만, 다른 종류도 있습니다.
title: 값 범주 및 해당 참조
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이동, 전달, 값 범주, 이동 의미 체계, 완벽한 전달, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63797084"
---
# <a name="value-categories-and-references-to-them"></a>값 범주 및 해당 참조
이 토픽에서는 C++에 존재하는 다양한 값 범주(및 값 참조)에 대해 설명합니다. 거의 모든 분들이 *lvalue* 및 *rvalue*에 대해 들어보셨겠지만, 이 토픽의 주제와 관련 지어 생각해 본 적은 없을 것입니다. 그리고 그 외에도 다른 값이 더 있습니다.

C++의 모든 식은 이 토픽에서 설명하는 범주 중 하나에 속하는 값을 생성합니다. C++ 언어, 패밀리 및 규칙 중에는 이러한 값 범주와 해당 참조에 대한 적절한 이해가 필요한 경우가 있습니다. 값의 주소를 가져오고, 값을 복사하고, 값을 이동하고, 값을 다른 함수에 전달하는 경우를 예로 들 수 있습니다. 이 토픽에서는 이러한 내용을 심층적으로 다루지는 않을 것이며, 확실한 이해를 돕기 위한 기본 정보를 제공합니다.

이 토픽의 정보는 Stroustrup의 두 가지 독립 속성 ID 및 이동성에 따른 값 범주 분석[2013 Stroustrup]을 기반으로 합니다.

## <a name="an-lvalue-has-identity"></a>ID를 갖고 있는 lvalue
값에 *ID*가 있다는 것은 무슨 의미일까요? 값의 메모리 주소를 갖고 있고(또는 가져올 수 있고) 안전하게 사용할 수 있다면 그 값이 바로 ID입니다. 이와 같은 방식으로 값의 콘텐츠를 비교하는 것보다 많은 것을 할 수 있습니다. 예를 들어 ID로 비교하거나 구분할 수 있습니다.

*lvalue*에는 ID가 있습니다. 이제는 과거의 산물이 된 "lvalue"의 "l"는 "left"의 약어입니다(할당의 왼쪽처럼). C++에서 lvalue는 할당의 왼쪽 *또는* 오른쪽에 나타날 수 있습니다. "lvalue"의 "l"는 lvalue를 이해하거나 정의하는 데 실질적인 도움을 주지 못합니다. lvalue란 ID가 있는 값을 말한다는 것만 이해하면 됩니다.

lvalue인 식의 예로는 명명된 변수나 상수 또는 참조를 반환하는 함수가 있습니다. lvalue가 *아닌* 식의 예로는 임시 항목 또는 값이 반환하는 함수가 있습니다.

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

이제 true 문에서 lvalue가 ID를 갖지만, xvalue도 마찬가지입니다. *xvalue*에 대한 내용은 이 토픽의 뒷부분에서 다루겠습니다. 지금은 "generalized lvalue"를 의미하는 glvalue라는 값 범주가 있다는 사실만 기억하세요. glvalue의 상위 집합에는 lvalue(*클래식 lvalue*라고도 함)와 xvalue가 모두 포함되어 있습니다. 따라서 "lvalue에 ID가 있다"는 말은 true이지만, ID가 있는 부분의 전체 세트는 다음 일러스트레이션처럼 glvalue 세트입니다.

![ID를 갖고 있는 lvalue](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>rvalue는 이동할 수 있지만, lvalue는 그렇지 않습니다.
하지만 glvalue가 아닌 값도 있습니다. 결과적으로 메모리 주소를 가져올 수 *없는*(또는 메모리 주소가 유효하다고 믿을 수 없는) 값이 있습니다. 위의 코드 예제에서 이러한 값을 목격했습니다. 이는 마치 단점인 것처럼 들립니다. 하지만 사실 이러한 값은 복사하는 대신(일반적으로 많은 비용 발생) *이동*(일반적으로 저렴)할 수 있다는 장점이 있습니다. 값을 이동한다는 것은 값이 더 이상 이전 위치에 있지 않다는 것을 의미합니다. 따라서 이전에 있었던 위치에서 값에 액세스하려고 하면 안 됩니다. 값을 이동하는 시기 및 *방법*은 이 토픽의 범위를 벗어납니다. 이 토픽에서는 이동 가능한 *rvalue*(또는 *클래식 rvalue*)라는 값을 이해하기만 하면 됩니다.

"rvalue"의 "r"은 "오른쪽"의 약어입니다(할당의 오른쪽처럼). 하지만 rvalue 및 rvalue 참조를 할당 외부에 사용할 수도 있습니다. "rvalue"의 "r"은 중요하지 않습니다. 우리가 rvalue라고 부르는 값이 이동 가능하다는 사실만 이해하면 됩니다.

반면 이 일러스트레이션처럼 lvalue는 이동할 수 없습니다. 이동된 lvalue는 *lvalue*의 정의를 위반하므로, 당연히 lvalue에 계속 액세스할 수 있을 것으로 예상되는 코드에서 예기치 않은 문제가 발생합니다.

![rvalue는 이동할 수 있지만, lvalue는 그렇지 않습니다.](images/is-movable.png)

lvalue를 이동할 수 없습니다. 하지만 이동할 수 있는 glvalue(ID가 있는 것들의 세트)라는 값이 *있습니다*. 값을 이동한 후에는 액세스하지 않도록 주의해야 한다는 점을 포함하여 충분한 이해가 있다면 사용할 수 있는 xvalue도 있습니다. 아래에서 값 범주의 전체 그림을 살펴볼 때 이 개념을 다시 한 번 검토하겠습니다.

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue 참조 및 참조 바인딩 규칙
이 섹션에서는 rvalue에 대한 참조의 구문을 소개합니다. 다른 토픽에서 값의 이동과 전달에 대해 자세히 알아보겠지만, rvalue 참조로 해결되는 문제입니다. rvalue 참조를 살펴보기 전에, 우리가 이전에 "참조"라고 부르던 `T&`&mdash;에 대해 정확하게 이해해야 합니다. 이것은 진정한 "lvalue(비 const) 참조"이며, 참조의 사용자가 쓸 수 있는 값을 의미합니다.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

lvalue 참조는 lvalue에 바인딩할 수 있지만 rvalue에는 바인딩할 수 없습니다.

그리고 참조의 사용자가 쓸 수 *없는* 개체(예: 상수)를 의미하는 lvalue const 참조(`T const&`)가 있습니다.

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

lvalue const 참조는 lvalue 또는 rvalue에 바인딩할 수 있습니다.

`T` 형식의 rvalue에 대한 참조 구문은 `T&&`로 작성됩니다. rvalue 참조는 이동 가능한 값, 다시 말해서 사용한 후 콘텐츠를 보존할 필요가 없는 값(예: 임시 항목)을 의미합니다. 전체 지점이 rvalue 참조에 바인딩된 값으로부터 이동되므로(따라서 수정되므로) `const` 및 `volatile` 한정자(cv 한정자라고도 함)가 rvalue 참조를 적용하지 않습니다.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

rvalue 참조는 rvalue에 바인딩됩니다. 사실, 오버로드 확인 때문에 rvalue는 lvalue const 참조보다 rvalue 참조에 바인딩하는 것을 *선호*합니다. 하지만 앞서 언급했듯이 rvalue 참조는 lvalue에 바인딩할 수 없으므로, rvalue 참조는 콘텐츠를 보존할 필요가 없는 것으로 가정하는 값(즉, 이동 생성자의 매개 변수)을 의미합니다.

또한 복사 생성을 통해(또는 rvalue가 xvalue인 경우 이동 생성을 통해) by-value 인수가 필요한 곳에 rvalue를 전달할 수 있습니다.

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>ID가 있는 glvalue, ID가 없는 prvalue
이제 ID가 무엇인지 다들 이해하셨을 것입니다. 이동 가능한 값과 이동할 수 없는 값도 이해하셨을 것입니다. 하지만 ID가 *없는* 값 세트의 이름을 지정하지 않았습니다. 이 세트를 *prvalue* 또는 *순수 rvalue*라고 합니다.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![ID가 있는 lvalue, ID가 없는 prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>값 범주의 전체 그림
위의 정보와 일러스트레이션을 하나의 큰 그림으로 결합하면 다음과 같은 그림이 완성됩니다.

![값 범주의 전체 그림](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue(i)
glvalue(일반화된 lvalue)는 ID가 있습니다.

### <a name="lvalue-im"></a>lvalue(i\&\!m)
lvalue(glvalue의 일종)는 ID가 있지만 이동할 수 없습니다. 참조나 const 참조, 또는 복사하는 것이 저렴한 경우 값을 통해 전달하는 일반적인 읽기-쓰기 값입니다. lvalue는 rvalue 참조에 바인딩할 수 없습니다.

### <a name="xvalue-im"></a>xvalue(i\&m)
xvalue(glvalue의 일종이지만 동시에 rvalue의 일종이기도 함)는 ID가 있고 이동할 수 있습니다. 복사는 비용이 많이 들기 때문에 이동하기로 결정한 이전 lvalue일 수 있으며, 이동 후에 액세스하지 않도록 주의해야 합니다. lvalue를 xvalue로 변환하는 방법은 다음과 같습니다.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

위의 코드 예제에서는 아직 아무 것도 이동하지 않았습니다. 조금 전에 lvalue를 이름이 지정되지 않은 rvalue 참조로 캐스팅하여 xvalue를 만들었습니다. 여전히 lvalue 이름으로 식별할 수 있지만, 이제는 xvalue로써 이동이 *가능*합니다. 이렇게 하는 이유와 이동의 결과는 다른 토픽에서 다루겠습니다. 하지만 도움이 된다면 "xvalue"의 "x"가 "expert-only(전문가 전용)"를 의미한다고 생각하셔도 좋습니다. lvalue를 xvalue(rvalue의 일종)로 캐스팅하면 값을 rvalue 참조에 바인딩할 수 있게 됩니다.

다음은 명명되지 않은 rvalue 참조를 반환하는 함수를 호출한 후 xvalue 멤버에 액세스하는 두 가지 xvalue 예제입니다.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue(\!i\&m)
prvalue(순수 rvalue. rvalue의 일종)는 ID가 없지만 이동할 수 있습니다. 일반적으로 임시 항목, 값에 따라 반환하는 함수를 호출한 결과 또는 glvalue가 아닌 다른 식을 평가한 결과입니다.

### <a name="rvalue-m"></a>rvalue(m)
rvalue는 이동할 수 있습니다. rvalue *참조*는 항상 rvalue(콘텐츠를 보존할 필요가 없는 것으로 가정하는 값)을 참조합니다.

그렇다면 rvalue 참조 자체는 rvalue일까요? *명명되지 않은* rvalue 참조(위의 xvalue 코드 예제에 나온 것처럼)는 xvalue이므로 rvalue입니다. 이동 생성자와 마찬가지로 rvalue 참조 함수 매개 변수에 바인딩하는 것을 선호합니다. 반대로(그리고 반직관적으로) rvalue 참조의 이름이 있는 경우 해당 이름으로 구성된 식은 lvalue입니다. 따라서 rvalue 참조 매개 변수에 바인딩할 수 *없습니다*. 하지만 간단하게 바인딩하는 방법이 있습니다. 명명되지 않은 rvalue 참조(xvalue)에 다시 캐스팅하면 됩니다.

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
ID가 없고 이동할 수 없는 값은 아직 살펴보지 않았습니다. 하지만 이러한 값 범주는 C++ 언어에서 유용하지 않으므로 무시해도 됩니다.

## <a name="reference-collapsing-rules"></a>참조 축소 규칙
한 식의 여러 유사 참조(lvalue 참조에 대한 lvalue 참조 또는 rvalue 참조에 대한 rvalue 참조)는 서로를 취소합니다.

- `A& &`는 `A&`로 축소됩니다.
- `A&& &&`는 `A&&`로 축소됩니다.

한 식의 여러 유사하지 않은 참조는 lvalue 참조로 축소됩니다.

- `A& &&`는 `A&`로 축소됩니다.
- `A&& &`는 `A&`로 축소됩니다.

## <a name="forwarding-references"></a>전달 참조
마지막 섹션에서는 앞에서 살펴본 rvalue 참조를 *전달 참조*라는 다른 개념과 비교해 보겠습니다.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&`는 앞서 살펴본 것처럼 rvalue 참조입니다. const 및 volatile은 rvalue 참조에 적용되지 않습니다.
- `foo`는 **A** 형식의 rvalue만 수락합니다.
- rvalue 참조(예: `A&&`)가 있는 이유는 임시 항목(또는 다른 rvalue) 전달에 최적화된 오버로드를 작성하기 위함입니다.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&`는 *전달 참조*입니다. `bar`에 전달하는 항목에 따라 **_Ty** 형식은 휘발성/비휘발성 여부에 관계 없이 const/비 const일 수 있습니다.
- `bar`는 **_Ty** 형식의 lvalue 또는 rvalue를 모두 수락합니다.
- lvalue를 전달하면 전달 참조는 lvalue 참조 `_Ty&`로 축소되는 `_Ty& &&`가 됩니다.
- lvalue를 전달하면 전달 참조는 lvalue 참조 `_Ty&&`로 축소되는 `_Ty&& &&`가 됩니다.
- 전달 참조(예: `_Ty&&`)가 있는 이유는 최적화가 *아니라*, 전달 참조에 항목을 투명하고 효율적으로 전달하는 것입니다. 라이브러리 코드를 작성(또는 면밀하게 연구)하는 경우에만 전달 참조를 경험할 가능성이 있습니다. 생성자 인수에 전달하는 팩터리 함수를 예로 들 수 있습니다.

## <a name="sources"></a>원본
* \[Stroustrup, 2013년\] B. Stroustrup: C++ 프로그래밍 언어, Fourth Edition. Addison-Wesley. 2013년.
