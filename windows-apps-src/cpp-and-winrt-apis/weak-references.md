---
author: stevewhims
description: Windows 런타임에서 참조 계산 시스템입니다. 시스템의 중요성 및, 간의 차이점에 대 한 알 수 이기도 하 고 강력한 및 약한 참조 합니다.
title: C++/WinRT의 약한 참조
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 강력한, 약한, 참조
ms.localizationpriority: medium
ms.openlocfilehash: 414a73c8df31e4547b8bd154945a8e9960529320
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5445590"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>강력 하 고 약한 참조를 C + + WinRT

Windows 런타임에서 참조 계산 시스템입니다. 이며 이러한 시스템의 중요성 및, 간의 차이점에 대 한 강력한 알 수 및 약한 참조 (하 고 중요 암시적 *이* 포인터와 같은 둘 다를 수 있는 참조). 이 항목에서 알 수 있듯이 이러한 참조를 올바르게 관리 하는 방법을 알고 예기치 않게 중지 하는 것을 원활 하 게 실행 하는 신뢰할 수 있는 시스템 간의 차이 의미할 수 있습니다. 언어 프로젝션에 대 한 심층 지원이 도우미 함수를 제공 하 여 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 간단 하 게 하 고 정확 하 게 더 복잡 한 시스템을 구축의 작업에 중간 충족 하는 있습니다.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>안전 하 게 클래스 멤버 코 루틴에서 *이* 포인터에 액세스

아래 코드 목록을 클래스의 멤버 함수는 코 루틴의 일반적인 예를 보여 줍니다.

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

잠시 동안 **MyClass::RetrieveValueAsync** 작동 하 고 다음의 복사본을 반환 결국은 `MyClass::m_value` 데이터 멤버입니다. 비동기 개체를 만들 수로 인해 **RetrieveValueAsync** 를 호출 하 고 해당 개체에 대 한 암시적 *이* 포인터 (결국에는 `m_value` 에 액세스).

이벤트의 전체 시퀀스는 다음과 같습니다.

1. **MyClass** 의 인스턴스를 만들 **주**에서 (`myclass_instance`).
2. `async` 개체가 생성 됩니다 (통해 해당 *이*) 가리키는 `myclass_instance`합니다.
3. **Winrt::Windows::Foundation::IAsyncAction::get** 함수 몇 초 동안 차단 하 고 **RetrieveValueAsync**의 결과 반환 합니다.
4. 값을 반환 하는 **RetrieveValueAsync** `this->m_value`.

4 단계는 *이* 유효한으로 안전 합니다.

그러나 비동기 작업이 완료 되기 전에 클래스 인스턴스를 소멸 되는 어떻게 될까요? 다양 한 방법으로 비동기 메서드가 완료 되기 전에 범위를 벗어나면 클래스 인스턴스를 이동할 수 있습니다. 하지만 클래스 인스턴스를 설정 하 여 시뮬레이션할 수 있습니다 `nullptr`.

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

클래스 인스턴스를 없애 버리면 지점, 것 처럼 보이고 하지 직접 참조를 다시 합니다. 하지만 물론 비동기 개체를 *이* 포인터 있으며 클래스 인스턴스 내에 저장 된 값을 복사 하는 사용 하려고 합니다. 코 루틴 멤버 함수 이며 수 impunity를 사용 하 여 *이* 포인터를 사용할 수 있다고 예상 합니다.

코드에이 변경으로에서는 컴퓨터 클래스 인스턴스를 되었기 때문에 *이* 더 이상 유효 4 단계에서 문제를 실행 합니다. 비동기 개체 변수 클래스 인스턴스 내에 액세스 하려고 시도 하면 즉시 것은 크래시 (또는 완전히 정의 되지 않은 작업을 수행).

해결 방법은 비동기 작업을 제공 하는&mdash;코 루틴이&mdash;클래스 인스턴스를 자체 강한 참조 합니다. 현재 상태 코 루틴이 효과적으로 원시 *이* 포인터를 보유 클래스 인스턴스; 하지만 아니므로 클래스 인스턴스를 활성 상태로 유지 하는 데 충분 합니다.

클래스 인스턴스를 유지 하려면 **RetrieveValueAsync** 구현의을 아래와 같이 변경 합니다.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

때문에 C + + /winrt 개체 직접 또는 간접적으로 파생 [**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements) 템플릿을 C + + /winrt 개체 수를 *이* 포인터에 대 한 강력한 참조를 검색 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) 보호 된 멤버 함수를 호출 하는. 실제로 사용할 필요가 없습니다는 합니다 `strong_this` 변수입니다. **get_strong** 호출에 참조 개수를 증가 하 고 유효한 암시적 *이* 포인터를 유지 합니다.

이전에 했습니다 4 단계로 가져온 경우 문제를 해결 합니다. 클래스 인스턴스를 다른 모든 참조 사라집니다 하는 경우에 코 루틴이 보장 하는 해당 종속성은 안정적의 예방 조치를 수행 했습니다.

강한 참조를 적절 한 없으면 [**implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) *이*에 대 한 약한 참조를 검색할 대신 호출할 수 합니다. 방금 *이*액세스 하기 전에 강한 참조를 검색할 수 있는지 확인 합니다.

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

위 예제에서 약한 참조를 강한 참조가 계속 하는 경우 클래스 인스턴스를 유지 하지 않습니다. 하지만 멤버 변수를에 액세스 하기 전에 강한 참조를 가져올 수 있는지 여부를 확인 하는 방법을 제공 합니다.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>이벤트 처리 대리자를 사용 하 여 *이* 포인터를 안전 하 게 액세스

### <a name="the-scenario"></a>시나리오

이벤트 처리에 대 한 일반 정보를 참조 하세요. [C +의 대리자를 사용 하 여 이벤트를 처리 + WinRT](handle-events.md).

이전 섹션 코 및 동시성 영역의 잠재적인 수명 문제를 강조 표시 합니다. 하지만 다음 개체의 멤버 함수 내에 있는 람다 함수 이벤트 수신자 (이벤트를 처리 하는 개체)와 이벤트 소스 (개체의 상대적 수명에 대해서 생각해 야 내에서 또는 개체의 멤버 함수를 사용 하 여 이벤트를 처리 하는 경우 이벤트를 발생). 일부 코드 예제를 살펴보겠습니다.

먼저 아래 코드 목록에 추가 된 모든 대리자에 의해 처리 되는 일반 이벤트를 발생 시키는 간단한 **EventSource** 클래스를 정의 합니다. 이 예제에서는 이벤트 [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) 대리자 형식을 사용 하는 발생 하지만 문제 및 해결 방법 여기 모든 대리자 형식 적용 됩니다.

그런 다음 **EventRecipient** 클래스는 람다 함수의 형태로 **EventSource::Event** 이벤트에 대 한 처리기를 제공합니다.

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

패턴은 이벤트 수신자는 *이* 포인터에 종속성이 있는 람다 이벤트 처리기입니다. 이벤트 수신자는 이벤트 소스 보다 깁니다, 될 때마다 해당 종속성을 보다 깁니다. 한 공통 되는 이러한 경우에서 패턴 잘 작동 합니다. 이러한 경우 중 일부는, 예를 들어 UI 페이지가 페이지의 컨트롤에서 발생한 이벤트를 처리할 때 같은 경우에 분명해집니다. 페이지의 단추를 보다 깁니다.&mdash;또한 보다 깁니다 단추를 따라서 처리기입니다. 이는 수신자가 소스(데이터 멤버 등)를 소유하고 있거나, 혹은 수신자와 소스가 서로 형제여서 다른 개체에 직접 종속되는 경우에도 동일하게 적용됩니다. 처리기의 수명이 종속되는 *this*보다 짧은 경우가 있다고 확신한다면 강하거나 약한 수명을 고려할 필요 없이 *this*를 정상적으로 캡처할 수 있습니다.

하지만 *이* 수명이 사용 (완료 및 비동기 작업에서 발생 하는 진행률 이벤트의 처리기 포함)를 처리기에서 처리 하는 방법을 알고 해야 하는 경우 여전히 있습니다.

- 코루틴을 작성하여 비동기 메서드를 구현하는 것도 가능합니다.
- 일부 XAML UI 프레임워크 개체(예: [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel))를 사용하는 것은 드물지만 수신자가 이벤트 소스에서 등록 해제하지 않고 작업을 마쳤다면 가능합니다.

### <a name="the-issue"></a>문제

이 다음 버전의 **주요** 기능은 이벤트 수신자 소멸 되 면 어떻게 될까요 시뮬레이션 (아마도 범위를 벗어날) 이벤트 소스는 이벤트를 발생 시키는 여전히 동안 합니다.

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

이벤트 수신자 손상 된 하지만 그 안의 람다 이벤트 처리기는 여전히 **이벤트** 이벤트를 구독 합니다. 해당 이벤트가 발생 하는 경우 람다는 해당 지점에서 유효 *이* 포인터 역참조 하려고 합니다. 처리기에서 (또는 코 루틴의) 코드에서 액세스 위반이 발생 하는 등 사용 하려고 합니다.

> [!IMPORTANT]
> 이런 경우 발생 하는 경우 다음 해야 합니다; *이* 개체의 수명을 및 캡처된 *이* 개체는 캡처 보다 긴 지도 생각해봐야 여부를 나타냅니다. 그렇지 않으면 캡처 강한 또는 약한 참조를 사용 하 여 아래 설명 된 대로 합니다.
>
> 또는&mdash;시나리오에 적합 한 스레딩 고려 사항 수 있도록도&mdash;이벤트를 사용 하 여 또는 수신자의 소멸자에서 수신자를 완료 한 후에 처리기를 취소 하는 또 다른 옵션입니다. [등록된 된 대리자 취소](handle-events.md#revoke-a-registered-delegate)를 참조 하세요.

어떻게 처리기를 등록 하려는 것입니다.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

람다가 자동으로 참조 하 여 로컬 변수를 캡처합니다. 따라서이 예제에서는에서는 의미로 작성할 수도 있습니다.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

두 경우 모두 원시 *이* 포인터를 방금 캡처하는 것입니다. 영향을 주지 않습니다 참조 카운트를에 아무 것도 현재 개체를 방해 하므로 합니다.

### <a name="the-solution"></a>솔루션

솔루션에 대 한 강력한 참조를 캡처하는 것입니다. *않습니다지* 강한 참조 증가 하며 참조 개수는 ** 유지 활성 현재 개체입니다. 방금 캡처 변수를 선언 (라는 `strong_this` 이 예제의), [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function), *이* 포인터에 대 한 강력한 참조를 검색 하는 호출 하 여 초기화 합니다.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

도 현재 개체의 자동 캡처를 생략 하 고 암시적 *이*통해 대신 캡처 변수를 통해 데이터 멤버에 액세스할 수 있습니다.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

강한 참조를 적절 한 없으면 [**implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) *이*에 대 한 약한 참조를 검색할 대신 호출할 수 합니다. 방금 검색할 수 있다는 여전히 강한 참조를 멤버에 액세스 하기 전에 확인 합니다.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>멤버 함수를 대리인으로 사용 하는 경우

뿐만 아니라 람다 함수를 대리인으로 멤버 함수를 사용 하 여 이러한 원칙 적용할 수도 있습니다. 구문은 다른, 따라서 일부 코드에서 살펴보겠습니다. 첫째, 원시 *이* 포인터를 사용 하 여 잠재적으로 안전 하지 않은 멤버 함수 이벤트 처리기가 같습니다.

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

이것이 개체의 멤버 함수를 참조할 수 표준, 일반적인 방법입니다. 안전이 확인 하려면&mdash;Windows sdk 버전 10.0.17763.0 (Windows 10, 버전 1809)&mdash;강한 또는 약한 참조 처리기 등록 된 시점을 설정 합니다. 이 시점에서 이벤트 받는 사람 개체를 여전히 활성화 알려져 있습니다.

강력한 참조를 위해 [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) 원시 *이* 포인터를 대신 호출 합니다. C + + WinRT 하면 결과 대리자는 현재 개체에 대 한 강력한 참조를 보유 합니다.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

약한 참조를 위해 [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)를 호출 합니다. C + + WinRT 하면 결과 대리자는 약한 참조를 보유 합니다. 마지막 단계에 및 백그라운드 대리자는 강력한 한 약한 참조를 확인 하려고 하 고 성공만 멤버 함수를 호출 합니다.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>**SwapChainPanel::CompositionScaleChanged** 를 사용 하는 약한 참조 예제

이 코드 예제에서는 다른 그림 약한 참조를 통해 [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) 이벤트를 사용합니다. 코드를 받는 사람에 대 한 약한 참조를 캡처하는 람다 함수를 사용 하 여 이벤트 처리기를 등록 합니다.

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

위의 약한 참조를 사용 하 고 살펴본 합니다. 일반적으로 순환 참조를 중단에 대 한 일입니다. 예를 들어 XAML 기반 UI 프레임 워크의 기본 구현에 대 한&mdash;프레임 워크의 이전 설계로 인해&mdash;약한 참조 메커니즘 C + + /winrt는 순환 참조를 처리 합니다. XAML 외부에서는 하지만 될 가능성이 않아도 약한 참조를 사용 하 여 (not 아무것도 인지에 대 한 XAML 고유의 본질적으로). 대신, 것을 수 있습니다 디자인 사용자 고유의 C + + WinRT Api 순환 참조와 약한 참조의 필요성을 배제 하는 방식에서입니다. 

어떤 유형을 선언하든 약한 참조의 필요 여부 또는 시기는 C++/WinRT에게 확연히 드러나지 않습니다. 따라서 C++/WinRT는 구조 템플릿 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 약한 참조 지원을 자동으로 제공합니다. 그 결과 사용자 고유의 C++/WinRT 유형이 이 구조체 템플릿에서 직/간접적으로 파생됩니다. 또한 개체가 실제로 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource)에 대해 쿼리를 실행하는 경우에만 비용이 발생한다는 점에서 대가성입니다. 또한 명시적으로 [해당 지원을 사용하지 않도록](#opting-out-of-weak-reference-support) 옵트 아웃을 선택할 수도 있습니다.

### <a name="code-examples"></a>코드 예제
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
* [implements::get_weak 함수](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::make_weak 함수 템플릿](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 마커 구조체](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 구조체 템플릿](/uwp/cpp-ref-for-winrt/weak-ref)
