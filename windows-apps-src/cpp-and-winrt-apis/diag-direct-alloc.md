---
description: 필요에 따라 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 도우미 제품군을 사용하는 대신 스택에서 구현 유형의 개체를 만드는 오류를 진단하는 데 도움이 되는 C++/WinRT 2.0 기능에 대해 자세히 설명합니다.
title: 직접 할당 진단
ms.date: 07/19/2019
ms.topic: article
keywords: Windows 10, UWP, 표준, C++, cpp, WinRT, 프로젝션, 직접, 스택, 할당, 프로젝션된, 구현
ms.localizationpriority: medium
ms.openlocfilehash: 7fe8ff6653b8655ee25cd9adc0c11acb22d42a11
ms.sourcegitcommit: 4e74c920f1fef507c5cdf874975003702d37bcbb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372794"
---
# <a name="diagnosing-direct-allocations"></a>직접 할당 진단

[C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)에서 설명한 대로 구현 형식의 개체를 만드는 경우 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 도우미 패밀리를 사용하여 이 작업을 수행해야 합니다. 이 항목에서는 구현 형식의 개체를 스택에 직접 할당하는 실수를 진단하는 데 도움이 되는 C++/WinRT 2.0 기능에 대해 자세히 설명합니다.

이러한 실수는 디버깅하기가 어렵고 시간이 많이 걸리는 알 수 없는 크래시 또는 손상으로 변할 수 있습니다. 따라서 이는 중요한 특징이며, 배경 지식을 이해할 가치가 있습니다.

## <a name="setting-the-scene-with-mystringable"></a>**MyStringable**을 사용하여 장면 설정

먼저 [**IStringable**](/uwp/api/windows.foundation.istringable)의 간단한 구현을 살펴보겠습니다.

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

이제 **IStringable**을 인수로 예상하는 함수를 구현 내에서 호출해야 한다고 가정합니다.

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

문제는 **MyStringable** 형식이 **IStringable**이 *아니라는 것입니다*.

- **MyStringable** 형식은 **IStringable** 인터페이스의 구현입니다.
- **IStringable** 형식은 프로젝션된 형식입니다.

> [!IMPORTANT]
> *구현 형식*과 *프로젝션된 형식*의 구분을 이해해야 중요합니다. 필수 개념 및 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md) 및 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

구현과 프로젝션 사이의 공간을 이해하기가 미묘할 수 있습니다. 그리고 실제로 구현이 프로젝션과 좀 더 비슷하게 느낄 수 있도록 하기 위해 구현은 구현하는 각 프로젝션된 형식에 대한 암시적 변환을 제공합니다. 그렇다고 해서 이렇게 간단히 수행할 수 있는 것은 아닙니다.

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

대신, 변환 연산자가 호출을 해결하기 위한 후보로 사용될 수 있도록 참조를 가져와야 합니다.

```cppwinrt
void Call()
{
    Print(*this);
}
```

이제 작동합니다. 암시적 변환은 구현 형식에서 프로젝션된 형식으로의 매우 효율적인 변환을 제공하며, 많은 시나리오에서 매우 편리합니다. 이 기능이 없으면 많은 구현 형식을 작성하는 것이 매우 번거로울 수 있습니다. [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 함수 템플릿(또는 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self))만 사용하여 구현을 할당하면 모든 것이 제대로 작동합니다.

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>C++/WinRT 1.0의 잠재적 문제

암시적 변환은 여전히 곤란한 문제를 일으킬 수 있습니다. 도움이 되지 않는 다음 도우미 함수를 살펴보겠습니다.

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

심지어 무해한 다음 명령문도 마찬가지입니다.

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

불행히도 이러한 암시적 변환으로 인해 C++/WinRT 1.0으로 *컴파일된* 코드가 있습니다. 매우 심각한 문제는 지원 메모리가 사용 후 삭제 스택에 있는 참조 수 계산 개체를 가리키는 프로젝션된 형식을 반환할 가능성이 있다는 것입니다.

C++/WinRT 1.0으로 컴파일된 다른 항목은 다음과 같습니다.

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

원시 포인터는 위험하고 노동 집약적인 버그의 원본입니다. 필요하지 않은 경우에는 사용하지 마세요. C++/WinRT는 원시 포인터를 사용하지 않고도 모든 작업을 효율적으로 수행하는 방식으로 진행됩니다. C++/WinRT 1.0으로 컴파일된 다른 항목은 다음과 같습니다.

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

이는 여러 수준에서 나타나는 실수입니다. 동일한 개체에 대해 서로 다른 두 개의 참조 수가 있습니다. Windows 런타임(및 이전의 클래식 COM)은 **std:shared_ptr**과 호환되지 않는 내장 참조 수를 기반으로 합니다. 물론 **std::shared_ptr**은 많은 유효한 애플리케이션을 포함하고 있지만, Windows 런타임(및 클래식 COM) 개체를 공유하는 경우에는 전혀 필요하지 않습니다. 마지막으로 C++/WinRT 1.0으로도 컴파일됩니다.

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

여기에는 의심의 여지가 다시 있습니다. 고유한 소유권이 **MyStringable**의 내장 참조 수에 대한 공유 수명과 반대입니다.

## <a name="the-solution-with-cwinrt-20"></a>C++/WinRT 2.0을 사용하는 솔루션

C++/WinRT 2.0을 사용하면 구현 형식을 직접 할당하려는 이러한 모든 시도로 인해 컴파일러 오류가 발생합니다. 이는 가장 적합한 오류이고, 알 수 없는 런타임 버그보다 훨씬 더 좋습니다.

구현을 수행해야 할 때마다 위와 같이 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 또는 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)를 사용하기만 하면 됩니다. 그리고 이제 이 작업을 수행하는 것을 잊어버리면 **use_make_function_to_create_this_object**라는 추상 함수에 대한 참조를 사용하여 이를 암시하는 컴파일러 오류가 표시됩니다. 정확히 `static_assert`는 아니지만 가깝습니다. 그럼에도 불구하고, 이는 설명된 모든 실수를 검색하는 가장 신뢰할 수 있는 방법입니다.

구현에 몇 가지 사소한 제약 조건을 적용해야 한다는 것을 의미합니다. 직접 할당을 검색하기 위한 재정의가 없는 경우 **winrt::make** 함수 템플릿은 어떻게든 재정의를 사용하여 추상 가상 함수를 충족해야 합니다. 이를 위해 재정의를 제공하는 `final` 클래스를 사용하여 구현에서 파생됩니다. 이 프로세스에 대해 관찰해야 할 몇 가지 사항이 있습니다.

첫째, 가상 함수는 디버그 빌드에만 있습니다. 즉 검색이 최적화된 빌드의 vtable 크기에는 영향을 주지 않습니다.

둘째, **winrt::make**에서 사용하는 파생 클래스가 `final`이므로, 이전에 구현 클래스를 `final`로 표시하지 않도록 선택한 경우에도 최적화 프로그램에서 추론할 수 있는 모든 가상화 해제가 발생한다는 것을 의미합니다. 따라서 이는 향상된 기능입니다. 반대의 경우는 구현이 `final`이 *될 수 없다*는 것입니다. 다시 말하지만, 인스턴스화된 형식은 항상 `final`이므로 결과는 아무 의미가 없습니다.

셋째, 구현에서 가상 함수를 `final`로 표시할 수 없습니다. 물론 C++/WinRT는 구현에 관한 모든 것이 가상화되는 경향이 있는 WRL과 같은 클래식 COM 및 구현과는 매우 다릅니다. C++/WinRT에서 가상 디스패치는 ABI(애플리케이션 이진 인터페이스)(항상 `final`)로 제한되며, 구현 메서드는 컴파일 시간 또는 정적 다형성을 사용합니다. 이렇게 하면 불필요한 런타임 다형성을 방지하고, C++/WinRT 구현에서 가상 함수를 사용할 이유도 거의 없습니다. 이는 매우 좋은 방법이고, 훨씬 더 예측 가능한 인라인으로 이어집니다.

넷째, **winrt::make**는 파생 클래스를 삽입하므로 구현에는 프라이빗 소멸자가 있을 수 없습니다. 모든 것이 가상이고 일반적으로 원시 포인터를 직접 처리했으며 이에 따라 실수로 [**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) 대신 `delete`를 호출하는 것이 쉬웠으므로 프라이빗 소멸자는 클래식 COM 구현에서 인기가 높았습니다. C++/WinRT에서는 원시 포인터를 직접 처리하기가 어렵습니다. 그리고 C++/WinRT에서 잠재적으로 `delete`를 호출할 수 있는 원시 포인터를 가져오도록 *실제로* 노력해야 합니다. 값 의미 체계는 값과 참조를 처리한다는 것을 의미하며 포인터를 사용하는 경우는 거의 없습니다.

따라서 C++/WinRT는 클래식 COM 코드를 작성하는 것이 무엇을 의미하는지에 대한 선입견에 도전합니다. 그리고 WinRT는 클래식 COM이 아니므로 완벽하게 합리적입니다. 클래식 COM은 Windows 런타임의 어셈블리 언어이며, 매일 작성하는 코드가 아니어야 합니다. 대신, C++/WinRT를 통해 최신 C++와 매우 비슷하고 클래식 COM보다 훨씬 덜 복잡한 코드를 작성할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 함수 템플릿](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)