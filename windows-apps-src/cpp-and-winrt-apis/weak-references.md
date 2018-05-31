---
author: stevewhims
description: C++/WinRT 약한 참조 지원은 개체가 IWeakReferenceSource에 대해 쿼리를 실행하는 경우에만 비용이 발생한다는 점에서 대가성입니다.
title: C++/WinRT의 약한 참조
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 약한, 참조
ms.localizationpriority: medium
ms.openlocfilehash: 63ffad19c0ae8a52737ae13a54e5657df875d0b5
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832607"
---
# <a name="weak-references-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)의 약한 참조
순환 참조와 약한 참조의 필요성을 배제하는 방식으로 사용자 고유의 C++/WinRT API를 설계할 수 있어야 합니다. 하지만 기본적인 XAML 기반 UI frameworkL의 구현과 관련하여 프레임워크의 이전 설계로 인해 C++/WinRT의 약한 참조 메커니즘은 순환 참조를 처리해야 합니다. 이론적으로는 약한 참조에 대한 XAML 고유의 조건이 없기는 하지만 XAML 외부에서는 약한 참조를 사용해야 할 가능성이 낮습니다.

어떤 유형을 선언하든 약한 참조의 필요 여부 또는 시기는 C++/WinRT에게 확연히 드러나지 않습니다. 따라서 C++/WinRT는 구조 템플릿 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 약한 참조 지원을 자동으로 제공합니다. 그 결과 사용자 고유의 C++/WinRT 유형이 이 구조체 템플릿에서 직/간접적으로 파생됩니다. 또한 개체가 실제로 [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609)에 대해 쿼리를 실행하는 경우에만 비용이 발생한다는 점에서 대가성입니다. 또한 명시적으로 [해당 지원을 사용하지 않도록](#opting-out-of-weak-reference-support) 옵트 아웃을 선택할 수도 있습니다.

## <a name="code-examples"></a>코드 예제
[**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 구조체 템플릿은 클래스 인스턴스로 약한 참조를 가져올 수 있는 한 가지 옵션입니다.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```
그 밖에 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 도우미 함수를 사용하는 방법도 있습니다.

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

약한 참조를 만들어도 개체 자체의 참조 수에는 영향을 끼치지 않습니다. 그 이유는 제어 블록만 할당되기 뿐입니다. 이 제어 블록은 약한 참조 의미 체계의 구현을 관리합니다. 그런 다음 약한 참조가 강한 참조로 승격되도록 시도할 수 있으며, 시도가 성공할 경우 승격된 강한 참조를 사용할 수 있습니다.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

그 밖에 일부 강한 참조가 계속해서 존재하는 경우에 한해 [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) 호출이 참조 수를 일정량씩 늘려 강한 참조를 호출자에게 반환합니다.

## <a name="a-weak-reference-to-the-this-pointer"></a>*이* 포인터에 대한 약한 참조
C++/WinRT 개체는 직간접적으로 구조체 템플릿 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 파생됩니다. 보호된 멤버 함수인 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)가 약한 참조를 C++/WinRT 개체의 *이* 포인터로 반환합니다. [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)이 강한 참조를 가져옵니다.

## <a name="opting-out-of-weak-reference-support"></a>약한 참조를 사용하지 않도록 옵트아웃
약한 참조 지원은 자동입니다. 하지만 [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) 마커 구조체를 템플릿 인수로 기본 클래스에 전달하여 명시적으로 지원을 사용하지 않도록 옵트아웃을 선택할 수 있습니다.

**winrt::implements**에서 직접 파생되는 경우

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

런타임 클래스를 작성하는 경우

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

variadic 매개 변수 팩에서 마커 구조체가 표시되는 경우는 중요하지 않습니다. 약한 참조의 옵트아웃을 요청하는 경우에는 컴파일러가 "*This is only for weak ref support*"를 통해 옵트아웃할 수 있도록 지원합니다.

## <a name="important-apis"></a>중요 API
* [implements::get_weak 함수](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::make_weak 함수 템플릿](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 마커 구조체](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 구조체 템플릿](/uwp/cpp-ref-for-winrt/weak-ref)
