---
description: Windows 런타임은 참조 계산 시스템입니다. 이러한 시스템에서는 강한 참조와 약한 참조의 중요성과 차이점을 인식해야 합니다.
title: C++/WinRT의 약한 참조
ms.date: 05/16/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 강력 하 고, 약한 참조
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c9fb112c6f83fa7bd9a3612916efd2527d821c29
ms.sourcegitcommit: 6c7e1aa3bd396a1ad714e8b77c0800759dc2d8e1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65821077"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>강력한 / 취약 한 참조에서 C++/WinRT

Windows 런타임에서 참조 횟수가 계산 시스템입니다. 에 이러한 시스템의 성과 구분 하는 방법에 대 한 알 수는 것이 반드시 강력한 / 취약 한 참조 (및 참조를 모두 같은 암시적 *이* 포인터). 이 항목에서 살펴보겠지만 이러한 참조를 올바르게 관리 하는 방법을 알면 원활 하 게 실행 하는 신뢰할 수 있는 시스템 및 지거나 충돌 하는 것의 차이 의미할 수 있습니다. 언어 프로젝션에 대 한 심층 지원이 있는 도우미 함수를 제공 하 여 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 간단 하 고 올바르게 더 복잡 한 시스템을 구축 하는 작업의 중간 사항을 충족 합니다.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>안전 하 게 액세스 하는 *이* 클래스 멤버 코 루틴에서 포인터

아래 코드 목록에 클래스의 멤버 함수는 코 루틴의 일반적인 예제를 보여 줍니다. 복사-붙여 넣을 수 있습니다이 예제에서는 지정된 된 파일에 새 **Windows 콘솔 응용 프로그램 (C++/WinRT)** 프로젝트입니다.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/coroutine.h>
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

**MyClass::RetrieveValueAsync** 일부 시간 작업 하는 데 소비 하 고 결국의 복사본을 반환 합니다는 `MyClass::m_value` 데이터 멤버입니다. 호출 **RetrieveValueAsync** 이 인해 생성 되는 비동기 개체가 및 해당 개체에는 암시적 *이* 포인터 (결국는 `m_value` 액세스).

이벤트의 전체 시퀀스는 다음과 같습니다.

1. **주**의 인스턴스이고 **MyClass** 만들어집니다 (`myclass_instance`).
2. 합니다 `async` 가리키는 개체가 생성 됩니다 (통해 해당 *이*)를 `myclass_instance`입니다.
3. 합니다 **winrt::Windows::Foundation::IAsyncAction::get** 함수를 몇 초 동안 차단 하 고의 결과 반환 합니다 **RetrieveValueAsync**합니다.
4. **RetrieveValueAsync** 의 값을 반환 `this->m_value`합니다.

4 단계는 안전한 하기만 *이* 유효 합니다.

하지만 비동기 작업이 완료 되기 전에 클래스 인스턴스는 제거 하는 경우에 어떻게? 다양 한 방법으로 클래스 인스턴스 수 범위를 벗어나는 비동기 메서드가 완료 되기 전에 있습니다. 이 클래스 인스턴스를 설정 하 여 시뮬레이션할 수 있습니다 하지만 `nullptr`합니다.

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

클래스 인스턴스를 파괴 우리는 여기서 지점, 이후에 것 같습니다. 우리가 직접 참조 하지를 다시 합니다. 하지만 물론 비동기 개체를 *이* 및 클래스 인스턴스 내에서 저장 된 값을 복사를 사용 하 여 시도에 대 한 포인터입니다. 코 루틴, 멤버 함수 이며 사용할 수 있게 되기를 예상 해당 *이* impunity 포인터입니다.

코드에 대 한이 변경, 실행 4 단계에서 문제가 클래스 인스턴스를 소멸 하기 때문에 및 *이* 더 이상 유효 합니다. 비동기 개체를 클래스 인스턴스 내에서 변수에 액세스 하려고 하는 즉시는 충돌 (또는 완전히 정의 되지 않은 작업을 수행).

비동기 작업을 제공 하는 솔루션&mdash;는 코 루틴&mdash;클래스 인스턴스에 대 한 자체 강력한 참조 합니다. 현재 기록 된 코 루틴 일 보유는 원시 *이* 클래스 인스턴스에 대 한 포인터로 사용할 수 있지만 클래스 인스턴스를 유지 하는 데 충분 합니다.

구현을 변경 클래스 인스턴스를 활성 상태로 유지할지 **RetrieveValueAsync** 아래 표시 된 것입니다.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

C++에서 파생 직접 또는 간접적으로 WinRT 클래스/합니다 [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) 템플릿. 따라서는 C++WinRT 개체를 호출할 수/해당 [ **implements.get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 보호 된 멤버 함수에 대 한 강력한 참조를 검색 하는 해당 *이* 대 한 포인터입니다. 실제로 사용 하지 않아도 있다는 점에 주의 합니다 `strong_this` 위의 코드 예제에서 변수; 호출 **get_strong** 증가 C++/WinRT 개체의 참조 횟수 및 해당 암시적 유지 *이* 포인터가 잘못 되었습니다.

> [!IMPORTANT]
> 때문에 **get_strong** 는의 멤버 함수는 **winrt::implements** 구조체 템플릿을 호출할 수 있습니다에서 직접 또는 간접적으로 파생 된 클래스 에서만에서 **winrt::implements**, 예를 C++/WinRT 클래스입니다. 파생 하는 방법에 대 한 자세한 내용은 **winrt::implements**, 및 예제를 참조 하십시오 [사용 하 여 만든 Api C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)합니다.

이 사용 했던 이전에 가져온 4 단계를 수행 하는 경우 문제를 해결 합니다. 클래스 인스턴스를 다른 모든 참조 사라지는 경우에는 코 루틴 종속은 안정적인 보장 예방 조치를 수행 했습니다.

강력한 참조를 적절 하 게 없는 경우 대신 호출할 수 있습니다 [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 에 대 한 약한 참조를 검색할 *이*합니다. 에 액세스 하기 전에 강력한 참조를 검색할 수 있도록 방금 확인 *이*합니다. 마찬가지로 **get_weak** 는의 멤버 함수는 **winrt::implements** 구조체 템플릿.

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

위의 예제에서 약한 참조를 강한 참조가 유지 하는 경우 소멸 되는 클래스 인스턴스를 유지 하지는 못합니다. 하지만 멤버 변수를 액세스 하기 전에 강력한 참조를 가져올 수 있는지 여부를 확인 하는 방법을 제공 합니다.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>안전 하 게 액세스 하는 *이* 이벤트 처리 대리자를 사용 하 여 포인터

### <a name="the-scenario"></a>시나리오

이벤트 처리에 대 한 일반 정보를 참조 하세요. [C + 대리자를 사용 하 여 이벤트를 처리 + WinRT](handle-events.md)합니다.

이전 섹션에는 코 루틴 및 동시성 분야의 잠재적인 수명 문제 강조 표시 합니다. 하지만 이벤트 수신자 (이벤트를 처리 하는 개체), 이벤트 소스 (개체의 상대 수명에 대 한 생각을 개체의 멤버 함수 내에서 람다 함수 해야 내에서 또는 개체의 멤버 함수를 사용 하 여 이벤트를 처리 하는 경우 이벤트를 발생 시키는). 코드 예제를 살펴보겠습니다.

첫 번째 아래 코드 목록 정의 간단한 **EventSource** 클래스에 추가 된 모든 대리자에 의해 처리 되는 일반 이벤트를 발생 시킵니다. 사용 하 여이 예제에서는 이벤트가 발생 합니다 [ **Windows::Foundation::EventHandler** ](/uwp/api/windows.foundation.eventhandler) 모든 대리자 형식에 적용 하는 대리자 형식 이지만 문제 및 해결책을 여기입니다.

그런 다음, **EventRecipient** 클래스에 대 한 처리기를 제공 합니다 **EventSource::Event** 람다 함수 형식의 이벤트입니다.

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

패턴은 이벤트 수신자 종속성이 있는 람다 이벤트 처리기에 해당 *이* 포인터입니다. 이벤트 수신자 길도 이벤트 소스, 때마다 해당 종속성을 보다 깁니다. 이러한 경우에 공통 된 패턴을 잘 작동 하며 이러한 경우 중 일부는, 예를 들어 UI 페이지가 페이지의 컨트롤에서 발생한 이벤트를 처리할 때 같은 경우에 분명해집니다. 페이지에 있는 단추 보다 오래 지속&mdash;따라서 처리기도 보다 오래 지속 단추입니다. 이는 수신자가 소스(데이터 멤버 등)를 소유하고 있거나, 혹은 수신자와 소스가 서로 형제여서 다른 개체에 직접 종속되는 경우에도 동일하게 적용됩니다. 처리기의 수명이 종속되는 *this*보다 짧은 경우가 있다고 확신한다면 강하거나 약한 수명을 고려할 필요 없이 *this*를 정상적으로 캡처할 수 있습니다.

하지만 여전히 위치 *이* 보다 수명이 길 처리기 (비동기 작업 및 작업에 의해 발생 하는 완료 및 진행률 이벤트에 대 한 처리기 포함)에서 사용 하지 않습니다 것을 처리 하는 방법을 알고 있어야 합니다.

- 코루틴을 작성하여 비동기 메서드를 구현하는 것도 가능합니다.
- 일부 XAML UI 프레임워크 개체(예: [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel))를 사용하는 것은 드물지만 수신자가 이벤트 소스에서 등록 해제하지 않고 작업을 마쳤다면 가능합니다.

### <a name="the-issue"></a>문제

이 다음 버전의 합니다 **주** 함수 시뮬레이션 이벤트 수신자 소멸 되 면 어떻게 되나요 (아마도 범위를 벗어날) 이벤트 소스는 이벤트를 발생 시키기도 하지만 합니다.

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

이벤트 수신자 제거 하지만 여전히 그 람다 이벤트 처리기가 구독 하는 **이벤트** 이벤트입니다. 람다를 역참조 하려고 해당 이벤트가 발생 하는 경우는 *이* 이때 잘못 된 포인터입니다. 처리기에서 (또는 코 루틴 일 연속) 코드에서 액세스 위반이 발생 하는, 사용 하려고 합니다.

> [!IMPORTANT]
> 수명을 고려해 야 할 경우 이러한 상황이 발생 하면 합니다 *이* 개체 및 여부 캡처된 *이* 개체 길도 캡처. 그렇지 않은 경우 다음 캡처해야이 강력한 또는 약한 참조를 사용 하 여 아래 보여 드립니다.
>
> 또는&mdash;시나리오에 적합 한 경우 스레딩 고려 사항 확인이 가능한&mdash;받는 사람의 소멸자 또는 이벤트와 함께 완료 되 면 처리기를 해지 하는 또 다른 옵션입니다. 참조 [등록 된 대리자를 해지](handle-events.md#revoke-a-registered-delegate)합니다.

처리기를 등록 하는 방법입니다.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

람다는 자동으로 참조 하 여 모든 로컬 변수를 캡처합니다. 따라서 예를 들어 우리가 동등 하 게 작성할 수도 있습니다.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

두 경우 모두 바로 캡처 중인지 원시 *이* 포인터입니다. 및 nothing 때문에 현재 개체에서 제거 되 고 있으므로 참조 횟수에 영향을 주지는 합니다.

### <a name="the-solution"></a>솔루션

솔루션은 강력한 참조를 캡처합니다. 강력한 참조 *않습니다* 고 참조 횟수를 증가 *않습니다* 현재 개체를 유지 합니다. 방금 캡처 변수를 선언 하면 (호출 `strong_this` 이 예에서)에 대 한 호출을 사용 하 여 초기화 [ **implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)에 대 한 강력한 참조를 검색 하는 우리의  *이* 포인터입니다.

> [!IMPORTANT]
> 때문에 **get_strong** 는의 멤버 함수는 **winrt::implements** 구조체 템플릿을 호출할 수 있습니다에서 직접 또는 간접적으로 파생 된 클래스 에서만에서 **winrt::implements**, 예를 C++/WinRT 클래스입니다. 파생 하는 방법에 대 한 자세한 내용은 **winrt::implements**, 및 예제를 참조 하십시오 [사용 하 여 만든 Api C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)합니다.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

도 현재 개체의 자동 캡처를 생략 하 고 암시적 통해 대신 캡처 변수를 통해 데이터 멤버에 액세스할 수 있습니다 *이*합니다.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

강력한 참조를 적절 하 게 없는 경우 대신 호출할 수 있습니다 [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 에 대 한 약한 참조를 검색할 *이*합니다. 만 검색할 수 있도록 여전히 강력한 참조에서 멤버에 액세스 하기 전에 확인 합니다.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>대리자는 멤버 함수를 사용 하는 경우

뿐만 아니라 람다 함수를 이러한 원칙 멤버 함수를 사용 하 여 사용자 대리자에 적용 합니다. 구문은 다른, 일부 코드를 살펴보도록 하겠습니다. 첫째, 안전 하지 않은 멤버 함수 이벤트 처리기를 사용 하는 원시 같습니다 *이* 포인터입니다.

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

이 방법이 표준, 기존 개체 및 해당 멤버 함수를 가리킵니다. 안전 하 게이 수 있습니다&mdash;Windows SDK의 버전 (Windows 10, 버전 1809) 10.0.17763.0&mdash;강력한 또는 처리기 등록 되어 있는 지점에 대 한 약한 참조를 설정 합니다. 이때 이벤트 받는 사람 개체 여전히 활성화 상태인 것으로 알려져 있습니다.

강력한 참조의 경우 호출할 [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 원시 대신 *이* 포인터입니다. C++/ WinRT는 결과 대리자를 현재 개체에 대 한 강력한 참조를 보유 하는 것을 확인 합니다.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

약한 참조를 호출 [ **get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)합니다. C++/ WinRT는 결과로 얻은 대리자가 약한 참조를 저장 하는 것을 확인 합니다. 마지막 단계에 및 백그라운드 대리자 강력한 단일에 대 한 약한 참조를 확인 하려고 시도 및 성공한 경우에 멤버 함수를 호출 합니다.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>사용 하는 약한 참조 예제 **SwapChainPanel::CompositionScaleChanged**

이 코드 예제를 사용 합니다 [ **SwapChainPanel::CompositionScaleChanged** ](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) 약한 참조의 다른 그림을 통해 이벤트입니다. 코드를 받는 사람에 대 한 약한 참조를 캡처하는 람다를 사용 하 여 이벤트 처리기를 등록 합니다.

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

람다 캡처 절에서는 *this*에 대한 약한 참조를 나타내는 임시 변수가 생성됩니다. 람다 함수의 본문에서는 *this*에 대한 강한 참조를 얻을 수 있다면 **OnCompositionScaleChanged** 함수가 호출됩니다. 이러한 방식으로 **OnCompositionScaleChanged** 내부에서는 *this*를 안전하게 사용할 수 있습니다.

## <a name="weak-references-in-cwinrt"></a>C++/WinRT의 약한 참조

위의 사용 되는 약한 참조를 살펴보았습니다. 일반적으로 순환 참조가 손상에 대 한 일입니다. XAML 기반 UI 프레임 워크의 네이티브 구현에 대 한 예를 들어&mdash;프레임 워크의 기록 디자인으로 인해&mdash;약한 참조 메커니즘에 C++WinRT는 순환 참조가 처리 하는 데 필요한 합니다. XAML을 외부 그러나 가능성이 않아도 약한 참조를 사용 하 여 (아무 것도 not에 대 한 XAML 관련 기본적으로). 대신, 더 경우가 수 있어야 사용자 고유의 디자인 C++약한 참조 및 순환 참조가 필요 하지 않도록 하는 방식에서 WinRT Api. 

어떤 유형을 선언하든 약한 참조의 필요 여부 또는 시기는 C++/WinRT에게 확연히 드러나지 않습니다. 따라서 C++/WinRT는 구조 템플릿 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 약한 참조 지원을 자동으로 제공합니다. 그 결과 사용자 고유의 C++/WinRT 유형이 이 구조체 템플릿에서 직/간접적으로 파생됩니다. 또한 개체가 실제로 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource)에 대해 쿼리를 실행하는 경우에만 비용이 발생한다는 점에서 대가성입니다. 또한 명시적으로 [해당 지원을 사용하지 않도록](#opting-out-of-weak-reference-support) 옵트 아웃을 선택할 수도 있습니다.

### <a name="code-examples"></a>코드 예제
[  **winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) 구조체 템플릿은 클래스 인스턴스로 약한 참조를 가져올 수 있는 한 가지 옵션입니다.

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

그 밖에 일부 강한 참조가 계속해서 존재하는 경우에 한해 [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) 호출이 참조 수를 일정량씩 늘려 강한 참조를 호출자에게 반환합니다.

### <a name="opting-out-of-weak-reference-support"></a>약한 참조를 사용하지 않도록 옵트아웃
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
* [implements::get_weak 함수](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak 함수 템플릿](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 표식 구조체](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 구조체 템플릿](/uwp/cpp-ref-for-winrt/weak-ref)
