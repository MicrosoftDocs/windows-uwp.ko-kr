---
description: Windows 런타임은 참조 계산 시스템입니다. 이러한 시스템에서는 강한 참조와 약한 참조의 중요성과 차이점을 인식해야 합니다.
title: C++/WinRT의 약한 참조
ms.date: 05/16/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 강한, 약한, 참조
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3ad6bb9a98b0fe2a699580001698740e44cea14f
ms.sourcegitcommit: cba3ba9b9a9f96037cfd0e07d05bd4502753c809
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/14/2019
ms.locfileid: "67870316"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>C++/WinRT의 강한 참조 및 약한 참조

Windows 런타임은 참조 계산 시스템입니다. 이러한 시스템에서는 강한 참조, 약한 참조, 강하지도 약하지도 않은 참조(예: 암시적 *this* 포인터)의 중요성 및 차이점을 아는 것이 중요합니다. 이 항목에서 확인할 수 있듯이, 이러한 참조를 올바르게 관리하는 방법을 알고 있는지 여부에 따라 원활하게 실행되는 시스템과 예기치 않게 작동이 중단되는 시스템 같이 완전히 다른 결과가 나타날 수 있습니다. 언어 프로젝션을 심층 지원하는 도우미 함수를 제공함으로써, [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)는 보다 복잡한 시스템을 간편하고 정확하게 빌드할 수 있도록 도와줍니다.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>클래스-멤버 코루틴에서 안전하게 *this* 포인터 액세스

코루틴에 대한 자세한 내용과 코드 예제는 [C++/WinRT로 동시성 및 비동기 작업](/windows/uwp/cpp-and-winrt-apis/concurrency)을 참조하세요.

아래의 코드 목록은 클래스의 멤버 함수인 코루틴의 일반적인 예제를 보여 줍니다. 이 예제를 복사하여 새로운 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트의 지정된 파일에 붙여넣을 수 있습니다.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync**는 작업하는 데 시간이 걸리지만, 결국 `MyClass::m_value` 데이터 멤버의 복사본을 반환합니다. **RetrieveValueAsync**를 호출하면 비동기 개체가 생성되고, 해당 개체에 암시적 *this* 포인터가 있습니다. 이 포인터를 통해 `m_value`에 액세스합니다.

코루틴에서는 컨트롤이 호출자에 반환되는 첫 번째 일시 중단 지점까지 동기 방식으로 실행됩니다. **RetrieveValueAsync**에서 첫 번째 `co_await`는 첫 번째 일시 중단 지점입니다. 코루틴이 다시 시작될 때까지(이 경우 약 5초 정도) `m_value`에 액세스하는 암시적 *this* 포인터가 변경되었을 수 있습니다.

이벤트의 전체 시퀀스는 다음과 같습니다.

1. **main**에서 **MyClass** 인스턴스가 생성됩니다(`myclass_instance`).
2. `async` 개체가 생성되고 *this*를 통해 `myclass_instance`를 가리킵니다.
3. **winrt::Windows::Foundation::IAsyncAction::get** 함수가 첫 번째 일시 중단 지점에 도달하고 몇 초 동안 차단된 후 **RetrieveValueAsync**의 결과를 반환합니다.
4. **RetrieveValueAsync**가 `this->m_value` 값을 반환합니다.

4단계는 *this*가 유효한 상태로 유지되는 동안만 안전합니다.

그러나 비동기 작업이 완료되기 전에 클래스 인스턴스가 삭제되면 어떻게 될까요? 비동기 메서드가 완료되기 전에 클래스 인스턴스가 다양한 방법으로 범위를 벗어날 수 있습니다. 그러나 클래스 인스턴스를 `nullptr`로 설정하여 시뮬레이트할 수 있습니다.

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

클래스 인스턴스를 삭제한 지점 이후에는 다시 직접 참조하지 않는 것 같습니다. 그러나 비동기 개체에는 해당 인스턴스를 가리키는 *this* 포인터가 있으며, 이 포인터를 사용하여 클래스 인스턴스 내에 저장된 값을 복사하려고 합니다. 코루틴은 멤버 함수이며, impunity와 함께 *this* 포인터를 사용할 수 있어야 합니다.

코드를 이렇게 변경하면, 클래스 인스턴스가 삭제되고 *this*가 더 이상 유효하지 않으므로 4단계에서 문제가 발생합니다. 비동기 개체가 클래스 인스턴스 내의 변수에 액세스하는 즉시 작동이 중단되거나 전혀 정의되지 않은 작업을 수행합니다.

해결 방법은 비동기 작업(코루틴) 자체에 클래스 인스턴스에 대한 강한 참조를 제공하는 것입니다. 현재 작성된 코드에서는 클래스 인스턴스에 대한 원시 *this* 포인터가 코루틴에 실질적으로 포함되지만, 이것만으로는 클래스 인스턴스를 활성 상태로 유지하는 데 충분하지 않습니다.

클래스 인스턴스를 활성 상태로 유지하려면 **RetrieveValueAsync** 구현을 아래와 같이 변경합니다.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

C++/WinRT 클래스는 직간접적으로 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 템플릿에서 파생됩니다. 이런 이유로, C++/WinRT 개체는 해당 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) protected 멤버 함수를 호출하여 *this* 포인터에 대한 강한 참조를 검색할 수 있습니다. 위의 코드 예제에서는 실제로 `strong_this` 변수를 사용할 필요가 없습니다. **get_strong**을 호출하기만 하면 C++/WinRT 개체의 참조 개수가 증가하고 암시적 *this* 포인터가 유효한 상태로 유지됩니다.

> [!IMPORTANT]
> **get_strong**은 **winrt::implements** 구조체 템플릿의 멤버 함수이므로, C++/WinRT 클래스와 같이 **winrt::implements**에서 직간접적으로 파생된 클래스에서만 호출할 수 있습니다. **winrt::implements**에서 파생하는 방법에 대한 자세한 내용과 예제는 [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)을 참조하세요.

이전에 4단계에서 발생했던 문제는 이 방법으로 해결됩니다. 클래스 인스턴스에 대한 다른 모든 참조가 사라지더라도 코루틴은 종속성을 안정적으로 사용할 수 있도록 예방 조치를 수행했습니다.

강한 참조가 적절하지 않은 경우, 대신 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)를 호출하여 *this*에 대한 약한 참조를 검색할 수 있습니다. *this*에 액세스하기 전에 강한 참조를 검색할 수 있는지 확인하기만 하면 됩니다. **get_weak**도 **winrt::implements** 구조체 템플릿의 멤버 함수입니다.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

위의 예제에서는 강한 참조가 없을 경우 약한 참조를 통해 클래스 인스턴스가 삭제되지 않도록 유지하지 않습니다. 그러나 멤버 변수에 액세스하기 전에 강한 참조를 얻을 수 있는지 여부를 확인하는 방법을 제공합니다.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>이벤트 처리 대리자를 사용하여 안전하게 *this* 포인터 액세스

### <a name="the-scenario"></a>시나리오

이벤트 처리에 대한 일반적인 내용은 [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)를 참조하세요.

이전 섹션에서는 코루틴과 동시성 영역의 잠재적 수명 문제를 강조해서 설명했습니다. 그러나 개체의 멤버 함수를 사용하거나 개체의 멤버 함수에 있는 람다 함수 내에서 이벤트를 처리하는 경우, 이벤트 수신자(이벤트를 처리하는 개체)와 이벤트 소스(이벤트가 발생하는 개체)의 상대 수명을 고려해야 합니다. 몇 가지 코드 예제를 살펴보겠습니다.

아래의 코드 목록에서는 먼저 추가된 대리자를 통해 처리되는 일반 이벤트를 발생시키는 간단한 **EventSource** 클래스를 정의합니다. 이 예제 이벤트는 [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) 대리자 형식을 사용하지만, 여기서 설명하는 문제와 해결 방법은 모든 대리자 형식에 적용됩니다.

그런 다음, **EventRecipient** 클래스가 **EventSource::Event** 이벤트 처리기를 람다 함수 형식으로 제공합니다.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

패턴은 이벤트 수신자에게 *this* 포인터에 대한 종속성을 가진 람다 이벤트 처리기가 있다는 것입니다. 이벤트 수신자가 이벤트 소스보다 오래 활성화되는 경우 항상, 이러한 종속성보다 오래 활성화됩니다. 이러한 일반적인 경우에는 패턴이 효과적입니다. UI 페이지가 해당 페이지에 있는 컨트롤에서 발생한 이벤트를 처리하는 경우와 같이 이러한 경우 중 일부는 쉽게 이해할 수 있습니다. 페이지가 단추보다 오래 활성화되므로 처리기도 단추보다 오래 활성화됩니다. 수신자가 소스를 데이터 멤버 등으로 소유하고 있거나, 수신자와 소스가 형제이고 다른 개체가 직접 소유하고 있는 경우에도 동일하게 적용됩니다.

처리기가 종속되는 *this*보다 오래 활성화되지 않는 경우가 있다고 확신할 때, 강하거나 약한 수명을 고려하지 않고 *this*를 정상적으로 캡처할 수 있습니다.

그러나 *this*가 처리기(비동기 작업에서 발생한 완료/진행률 이벤트의 처리기 포함)에서 사용되는 기간보다 오래 활성화되지 않는 경우도 있으며, 이러한 경우를 처리하는 방법을 알아 두어야 합니다.

- 이벤트 원본에서 해당 이벤트를 *동기적으로* 발생시키면 처리기를 취소하고 이벤트를 더 이상 받지 않을 것임을 확신할 수 있습니다. 그러나 비동기 이벤트의 경우 해지 후에도(특히 소멸자 내에서 해지하는 경우) 진행 중인 이벤트에서 소멸을 시작한 후 개체에 도달할 수 있습니다. 소멸하기 전에 구독을 취소할 장소를 찾으면 문제가 완화될 수 있지만 강력한 해결 방법을 계속 읽어야 합니다.
- 코루틴을 작성하여 비동기 메서드를 구현하면 처리할 수 있습니다.
- 드물긴 하지만 특정 XAML UI 프레임워크 개체(예: [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel))를 사용하는 경우 이벤트 소스에서 등록을 취소하지 않고 수신자를 종료하면 처리할 수 있습니다.

### <a name="the-issue"></a>문제

다음 버전의 해당 **main** 함수는 이벤트 소스에서 이벤트가 계속 발생하는 동안 이벤트 수신자가 범위를 벗어나서 삭제될 경우 어떤 결과가 나타나는지를 시뮬레이트합니다.

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

이벤트 수신자는 삭제되지만, 이벤트 수신자 내의 람다 이벤트 처리기가 **Event** 이벤트를 계속 구독합니다. 이벤트가 발생하면 람다가 해당 지점에 유효하지 않은 *this* 포인터를 역참조하려고 합니다. 따라서 포인터를 사용하려는 처리기(또는 코루틴의 연속) 코드에서 액세스 위반이 발생합니다.

> [!IMPORTANT]
> 이러한 상황이 발생할 경우 *this* 개체의 수명과 캡처된 *this* 개체가 캡처보다 오래 활성화되지는 여부를 고려해야 합니다. 오래 활성화되지 않는 경우 아래와 같이 강한 참조나 약한 참조로 캡처합니다.
>
> 또는 시나리오에 적합하고 스레딩 고려 사항에 따라 가능한 경우 수신자가 이벤트 작업을 완료한 후 또는 수신자의 소멸자에서 처리기를 철회하는 옵션도 있습니다. [등록된 대리자 철회](handle-events.md#revoke-a-registered-delegate)를 참조하세요.

다음과 같은 방법으로 처리기를 등록합니다.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

람다가 참조로 모든 지역 변수를 자동으로 캡처합니다. 따라서 이 예제에서는 다음과 같이 작성해도 의미가 같습니다.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

두 경우 모두, 원시 *this* 포인터를 캡처합니다. 이렇게 하면 참조 계산에 영향을 미치지 않으므로 현재 개체가 삭제되지 않도록 방지할 수 없습니다.

### <a name="the-solution"></a>해결 방법

해결 방법은 강한 참조(또는 나중에 확인하겠지만 더 적절한 경우 약한 참조)를 캡처하는 것입니다. 강한 참조는 참조 개수를 ‘증가’시키고 현재 개체를 활성 상태로 ‘유지’합니다.   캡처 변수(이 예제에서 `strong_this` *이*(가) 호출됨)만 선언하고 이 포인터에 대한 강력한 참조 를 검색하는 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)에 대한 호출로 초기화합니다.

> [!IMPORTANT]
> **get_strong**은 **winrt::implements** 구조체 템플릿의 멤버 함수이므로, C++/WinRT 클래스와 같이 **winrt::implements**에서 직간접적으로 파생된 클래스에서만 호출할 수 있습니다. **winrt::implements**에서 파생하는 방법에 대한 자세한 내용과 예제는 [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)을 참조하세요.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

현재 개체의 자동 캡처를 생략하고, 암시적 *this* 대신 캡처 변수를 통해 데이터 멤버에 액세스할 수도 있습니다.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

강한 참조가 적절하지 않은 경우, 대신 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)를 호출하여 *this*에 대한 약한 참조를 검색할 수 있습니다. 약한 참조는 현재 개체를 활성 상태로 유지하지 *않습니다*. 따라서 멤버에 액세스하기 전에 먼저 약한 참조에서 여전히 강한 참조를 검색할 수 있는지 확인하기만 하면 됩니다.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

원시 포인터를 캡처하는 경우 가리키는 개체를 활성 상태로 유지해야 합니다.

### <a name="if-you-use-a-member-function-as-a-delegate"></a>멤버 함수를 대리자로 사용하는 경우

람다 함수뿐만 아니라 이러한 원칙은 멤버 함수를 대리자로 사용하는 경우에도 적용됩니다. 구문이 다르므로 몇 가지 코드를 살펴보겠습니다. 먼저 원시 *this* 포인터를 사용하는, 잠재적으로 안전하지 않은 멤버 함수 이벤트 처리기는 다음과 같습니다.

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

개체 및 해당 멤버 함수를 참조하는 기존의 표준 방법입니다. 이 처리기를 안전하게 만들기 위해 Windows SDK 버전 10.0.17763.0(Windows 10, 버전 1809)에서는 처리기가 등록된 지점에 강한 참조나 약한 참조를 설정할 수 있습니다. 이 지점에서는 이벤트 수신자 개체가 활성 상태로 알려져 있습니다.

강한 참조의 경우 원시 *this* 포인터 대신 [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)을 호출하면 됩니다. C++/WinRT에서 결과 대리자에 현재 개체에 대한 강한 참조가 포함되도록 합니다.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

강한 참조를 캡처한다는 것은 처리기가 등록 취소되고 처리 중인 모든 콜백이 반환된 후에만 개체가 소멸될 수 있음을 의미합니다. 하지만 이 보장은 이벤트가 발생한 시점에만 유효합니다. 이벤트 처리기가 비동기인 경우 첫 번째 일시 중단 지점 앞에 클래스 인스턴스에 대한 강한 참조를 코루틴에 제공해야 합니다(자세한 내용과 코드는 이 문서의 앞부분에 있는 [클래스-멤버 코루틴에서 안전하게 *this* 포인터 액세스](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) 섹션 참조). 그러나 이렇게 하면 이벤트 원본과 개체 간의 순환 참조가 만들어지므로 이벤트를 해지하여 이를 명시적으로 중단해야 합니다.

약한 참조의 경우 [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)를 호출합니다. C++/WinRT에서 결과 대리자에 약한 참조가 포함되도록 합니다. 최후의 순간에 백그라운드에서 대리자가 약한 참조를 강한 참조로 확인하려고 하며, 성공하는 경우에만 멤버 함수를 호출합니다.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

대리자에서 멤버 함수를 *호출*하면 C++/WinRT는 처리기가 반환될 때까지 개체를 활성 상태로 유지합니다. 그러나 처리기가 비동기인 경우 일시 중단 지점으로 반환되므로 첫 번째 일시 중단 지점 앞에 클래스 인스턴스에 대한 강한 참조를 코루틴에 제공해야 합니다. 자세한 내용은 이 문서의 앞부분에 있는 [클래스-멤버 코루틴에서 안전하게 *this* 포인터 액세스](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) 섹션을 참조하세요.

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>**SwapChainPanel::CompositionScaleChanged**를 사용하는 약한 참조 예제

이 코드 예제에서는 다른 약한 참조 예를 통해 [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) 이벤트를 사용합니다. 이 코드는 수신자에 대한 약한 참조를 캡처하는 람다 함수를 사용하여 이벤트 처리기를 등록합니다.

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

람다 캡처 절에서는 *this*에 대한 약한 참조를 나타내는 임시 변수가 생성됩니다. 람다의 본문에서는 *this*에 대한 강한 참조를 얻을 수 있는 경우 **OnCompositionScaleChanged** 함수가 호출됩니다. 이러한 방식으로 **OnCompositionScaleChanged** 내부에서 *this*를 안전하게 사용할 수 있습니다.

## <a name="weak-references-in-cwinrt"></a>C++/WinRT의 약한 참조

위에서, 약한 참조를 사용하는 방법을 살펴보았습니다. 일반적으로 약한 참조는 순환 참조를 중단하는 데 적합합니다. 예를 들어 XAML 기반 UI 프레임워크의 기본 구현에서는 프레임워크의 이전 설계 때문에 순환 참조를 처리하기 위해 C++/WinRT의 약한 참조 메커니즘이 필요합니다. 그러나 XAML 외부에서는 약한 참조를 사용할 필요가 없습니다(본질적으로 XAML과 관련된 것은 아님). 대신, 순환 참조와 약한 참조가 필요하지 않는 방식으로 고유한 C++/WinRT API를 설계할 수 있어야 합니다. 

어떤 형식을 선언하든, 약한 참조가 필요한지 여부 또는 필요한 시기는 C++/WinRT에서 즉시 명확하게 드러나지 않습니다. 따라서 C++/WinRT는 고유한 C++/WinRT 형식이 직간접적으로 파생되는 구조체 템플릿 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 약한 참조 지원을 자동으로 제공합니다. 이 지원은 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource)에 대해 개체를 실제로 쿼리하는 경우에만 비용이 발생한다는 점에서 P4P(Pay-for-Play)입니다. 명시적으로 [해당 지원을 옵트아웃](#opting-out-of-weak-reference-support)할 수도 있습니다.

### <a name="code-examples"></a>코드 예제
[**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 구조체 템플릿은 클래스 인스턴스에 대한 약한 참조를 얻는 한 가지 옵션입니다.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

또는 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 도우미 함수를 사용할 수 있습니다.

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

약한 참조를 만드는 경우 개체 자체의 참조 개수에는 영향을 주지 않습니다. 제어 블록이 할당되도록 하는 역할만 합니다. 이 제어 블록은 약한 참조의 의미 체계 구현을 관리합니다. 그런 후에 약한 참조의 수준을 강한 참조로 올리는 시도에 성공하면 강한 참조를 사용할 수 있습니다.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

다른 몇 가지 강한 참조도 있지만, [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) 호출은 참조 개수를 증가시키고 호출자에 대한 강한 참조를 반환합니다.

### <a name="opting-out-of-weak-reference-support"></a>약한 참조 지원 옵트아웃
약한 참조 지원은 자동입니다. 하지만 [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) 마커 구조체를 템플릿 인수로 기본 클래스에 전달하여 명시적으로 해당 지원을 옵트아웃할 수 있습니다.

**winrt::implements**에서 직접 파생하는 경우

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

variadic 매개 변수 팩에서 마커 구조체가 표시되는 위치는 중요하지 않습니다. 옵트아웃된 형식의 약한 참조를 요청하는 경우 컴파일러에서 “약한 참조 지원에만 사용됩니다.” 오류를 도와줍니다. 

## <a name="important-apis"></a>중요 API
* [implements::get_weak 함수](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak 함수 템플릿](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 마커 구조체](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 구조체 템플릿](/uwp/cpp-ref-for-winrt/weak-ref)
