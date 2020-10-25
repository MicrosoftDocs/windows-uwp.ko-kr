---
description: 이벤트를 발생시키는 런타임 클래스가 포함된 Windows 런타임 구성 요소를 작성하는 방법을 보여 줍니다. 또한 구성 요소를 사용하고 이벤트를 처리하는 앱도 보여 줍니다.
title: C++/WinRT의 이벤트 작성
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 작성, 이벤트
ms.localizationpriority: medium
ms.openlocfilehash: 66691b1cd75a27e683261c12b7a3056c53160079
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297623"
---
# <a name="author-events-in-cwinrt"></a>C++/WinRT의 이벤트 작성

이 토픽은 Windows 런타임 구성 요소 및 사용 중인 애플리케이션을 기반으로 하며, [C++/WinRT를 사용한 Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) 토픽에서는 빌드 방법을 보여줍니다.

다음은 이 토픽에서 추가하는 새 기능입니다.
- 온도가 0도 미만으로 내려가면 이벤트를 발생시키도록 온도계 런타임 클래스를 업데이트합니다.
- 해당 이벤트를 처리하도록 온도계 런타임 클래스를 사용하는 코어 앱을 업데이트합니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="create-thermometerwrc-and-thermometercoreapp"></a>**ThermometerWRC** 및 **ThermometerCoreApp** 만들기

코드를 빌드하고 실행할 수 있도록 이 토픽에서 설명하는 업데이트를 따라 하려면 가장 먼저 [C++/WinRT를 사용한 Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) 토픽의 연습을 수행해야 합니다. 이렇게 하면 **ThermometerWRC** Windows 런타임 구성 요소와 이를 사용하는 **ThermometerCoreApp** 코어 앱이 갖추어집니다.

## <a name="update-thermometerwrc-to-raise-an-event"></a>이벤트를 발생시키도록 **ThermometerWRC** 업데이트

아래 목록처럼 보이도록 `Thermometer.idl`을 업데이트합니다. 이와 같은 방법으로 대리자 형식이 [**EventHandler**](/uwp/api/windows.foundation.eventhandler-1)이고 단정밀도 부동 소수점 숫자의 인수를 사용하는 이벤트를 선언할 수 있습니다.

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
        event Windows.Foundation.EventHandler<Single> TemperatureIsBelowFreezing;
    };
}
```

파일을 저장합니다. 현재 상태에서는 프로젝트 빌드가 완료되지 않지만, 지금 빌드하여 업데이트된 버전의 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` 및 `Thermometer.cpp` 스텁 파일을 생성합니다. 이제 이러한 파일 내에서 **TemperatureIsBelowFreezing** 이벤트의 스텁 구현을 확인할 수 있습니다. C++/WinRT에서 IDL로 선언된 이벤트는 오버로드된 함수 세트로 구현됩니다(속성이 오버로드된 get 및 set 함수 세트로 구현되는 것과 유사한 방식). 한 오버로드는 등록할 대리자를 가지며, 토큰을 반환합니다([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). 다른 오버로드는 토큰을 가지며 관련 대리자의 등록을 취소합니다.

이제 `Thermometer.h` 및 `Thermometer.cpp`를 열고, **Thermometer** 런타임 클래스의 구현을 업데이트합니다. `Thermometer.h`에서 오버로드된 두 개의 **TemperatureIsBelowFreezing** 함수 및 이러한 함수의 구현에 사용할 프라이빗 이벤트 데이터 멤버를 추가합니다.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...
        winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler);
        void TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_temperatureIsBelowFreezingEvent;
        ...
    };
}
...
```

위에서 볼 수 있듯이, 이벤트는 특정 대리자 형식(args 형식을 사용하여 자체를 매개 변수화 가능)에 의해 매개 변수화된 [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 구조체 템플릿으로 표시됩니다.

`Thermometer.cpp`에서 오버로드된 두 개의 **TemperatureIsBelowFreezing** 함수를 구현합니다.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_temperatureIsBelowFreezingEvent.add(handler);
    }

    void Thermometer::TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept
    {
        m_temperatureIsBelowFreezingEvent.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f) m_temperatureIsBelowFreezingEvent(*this, m_temperatureFahrenheit);
    }
}
```

> [!NOTE]
> 자동 이벤트 취소자에 대한 자세한 정보는 [등록된 대리자 취소](handle-events.md#revoke-a-registered-delegate)를 참조하세요. 이벤트에 대한 자동 이벤트 취소자 구현을 무료로 가져올 수 있습니다. 즉, 이벤트 취소자(C++/WinRT 프로젝션을 통해 제공)에 대한 오버로드를 구현할 필요가 없습니다.

다른 오버로드(등록 및 수동 해지 오버로드)는 프로젝션에 반영되지 *않습니다*. 이는 시나리오에 최적으로 구현할 수 있는 유연성을 제공하기 위한 것입니다. 이러한 구현에 나온 것처럼 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 및 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) 호출은 효율적이며 동시성/스레드로부터 안전한 기본값입니다. 하지만 매우 많은 수의 이벤트를 처리하는 경우 각각에 대한 이벤트 필드 대신 일부 밀도가 낮은 구현을 원할 수 있습니다.

또한 온도가 0도 미만으로 내려가면 **TemperatureIsBelowFreezing** 이벤트를 발생시키도록 **AdjustTemperature** 함수의 구현이 업데이트되었음을 위에서 확인할 수 있습니다.

## <a name="update-thermometercoreapp-to-handle-the-event"></a>이벤트를 처리하도록 **ThermometerCoreApp** 업데이트

**ThermometerCoreApp** 프로젝트의 `App.cpp`에서 이벤트 처리기를 등록한 다음, 온도를 0도 미만으로 떨어뜨리도록 코드를 다음과 같이 변경합니다.

`WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto &, float temperatureFahrenheit)
        {
            WINRT_ASSERT(temperatureFahrenheit < 32.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.TemperatureIsBelowFreezing(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

**OnPointerPressed** 메서드의 변경 내용에 유의하세요. 이제 창을 클릭할 때마다 온도계의 온도에서 화씨 1도씩 *뺍니다*. 그리고 이제는 앱에서 온도가 0도 미만으로 내려가면 발생하는 이벤트를 처리합니다. 이벤트가 예상대로 발생하고 있음을 보여 주기 위해 **TemperatureIsBelowFreezing** 이벤트를 처리하는 람다 식 내에 중단점을 배치하고, 앱을 실행하고, 창 내부를 클릭합니다.

## <a name="parameterized-delegates-across-an-abi"></a>ABI에서 매개 변수가 있는 대리자

구성 요소와 해당 소비 애플리케이션 사이 같은 ABI(Application Binary Interface)를 통해 이벤트에 액세스할 수 있어야 하는 경우 이벤트에는 Windows 런타임 대리자 형식이 사용되어야 합니다. 위 예제에서는 [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) Windows 런타임 대리자 형식을 사용합니다. [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler)는 Windows 런타임 대리자 형식의 또 다른 예입니다.

두 대리자 형식의 형식 매개 변수는 ABI를 가로질러야 하므로 형식 매개 변수도 Windows 런타임 형식이어야 합니다. 여기에는 Windows 런타임 클래스, 타사 런타임 클래스, 숫자나 문자열 같은 기본 형식이 포함됩니다. 해당 제약 조건을 잊을 경우 컴파일러가 "*T는 WinRT 형식이어야 합니다.* "라는 오류를 처리하는 데 도움이 될 수 있습니다.

다음은 코드 목록 형식의 예제입니다. 이 항목의 앞부분에서 만든 **ThermometerWRC** 및 **ThermometerCoreApp** 프로젝트를 시작하고, 이러한 목록의 코드와 비슷하게 해당 프로젝트의 코드를 편집합니다.

첫 번째 목록은 **ThermometerWRC** 프로젝트에 대한 것입니다. 아래와 같이 `ThermometerWRC.idl`을 편집한 다음, 앞에서 `Thermometer.h` 및 `.cpp`를 복사한 것처럼 `MyEventArgs.h` 및 `.cpp`를 프로젝트에(`Generated Files` 폴더의) 복사합니다.

```cppwinrt
// ThermometerWRC.idl
namespace ThermometerWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single TemperatureFahrenheit{ get; };
    }

    [default_interface]
    runtimeclass Thermometer
    {
        ...
        event Windows.Foundation.EventHandler<ThermometerWRC.MyEventArgs> TemperatureIsBelowFreezing;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::ThermometerWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float temperatureFahrenheit);
        float TemperatureFahrenheit();

    private:
        float m_temperatureFahrenheit{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::ThermometerWRC::implementation
{
    MyEventArgs::MyEventArgs(float temperatureFahrenheit) : m_temperatureFahrenheit(temperatureFahrenheit)
    {
    }

    float MyEventArgs::TemperatureFahrenheit()
    {
        return m_temperatureFahrenheit;
    }
}

// Thermometer.h
...
struct Thermometer : ThermometerT<Thermometer>
{
...
    winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs>> m_temperatureIsBelowFreezingEvent;
...
}
...

// Thermometer.cpp
#include "MyEventArgs.h"
...
winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler) { ... }
...
void Thermometer::AdjustTemperature(float deltaFahrenheit)
{
    m_temperatureFahrenheit += deltaFahrenheit;

    if (m_temperatureFahrenheit < 32.f)
    {
        auto args = winrt::make_self<winrt::ThermometerWRC::implementation::MyEventArgs>(m_temperatureFahrenheit);
        m_temperatureIsBelowFreezingEvent(*this, *args);
    }
}
...
```

이 목록은 **ThermometerCoreApp** 프로젝트에 대한 것입니다.

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto&, ThermometerWRC::MyEventArgs args)
    {
        float degrees = args.TemperatureFahrenheit();
        WINRT_ASSERT(degrees < 32.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>ABI에서 단순 신호

이벤트를 통해 매개 변수나 인수를 전달할 필요가 없는 경우에는 고유한 단순 Windows 런타임 대리자 형식을 정의할 수 있습니다. 아래 예제에서는 **Thermometer** 런타임 클래스의 더 간단한 버전을 보여 줍니다. **SignalDelegate**라는 대리자 형식을 선언한 다음, 이 대리자 형식을 사용하여 매개 변수가 있는 이벤트 대신 신호 형식 이벤트를 발생시킵니다.

```idl
// ThermometerWRC.idl
namespace ThermometerWRC
{
    delegate void SignalDelegate();

    runtimeclass Thermometer
    {
        Thermometer();
        event ThermometerWRC.SignalDelegate SignalTemperatureIsBelowFreezing;
        void AdjustTemperature(Single value);
    };
}
```

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

        winrt::event_token SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler);
        void SignalTemperatureIsBelowFreezing(winrt::event_token const& token);
        void AdjustTemperature(float deltaFahrenheit);

    private:
        winrt::event<ThermometerWRC::SignalDelegate> m_signal;
        float m_temperatureFahrenheit{ 0.f };
    };
}
```

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void Thermometer::SignalTemperatureIsBelowFreezing(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.SignalTemperatureIsBelowFreezing([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.SignalTemperatureIsBelowFreezing(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>프로젝트 내의 매개 변수가 있는 대리자, 단순 신호 및 콜백

Visual Studio 프로젝트 내부에 있는 이벤트가 필요한 경우(이진 파일이 아닌) 해당 이벤트가 Windows 런타임 형식으로 제한되지 않는 경우에도 [**winrt::event**](/uwp/cpp-ref-for-winrt/event)\<Delegate\> 클래스 템플릿을 사용할 수 있습니다. **winrt::delegate**도 비 Windows 런타임 매개 변수를 지원하므로 간단히 실제 Windows 런타임 대리자 형식 대신 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate)를 사용하면 됩니다.

아래 예제에서는 매개 변수(기본적으로 단순 신호)를 사용하지 않는 대리자 서명을 보여 준 후 문자열을 사용하는 대리자 서명을 보여 줍니다.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

구독 대리자를 원하는 만큼 이벤트에 추가하는 방법을 알 수 있습니다. 그러나 이벤트와 연결된 몇 가지 오버헤드가 있습니다. 단일 구독 대리자만 사용하는 간단한 콜백만 필요한 경우에는 자체적으로 [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate)를 사용할 수 있습니다.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

이벤트 및 대리자가 프로젝트 내에서 내부적으로 사용되는 C++/CX 코드베이스에서 이식하는 경우에는 **winrt::delegate**를 통해 C++/WinRT에서 해당 패턴을 복제할 수 있습니다.

## <a name="design-guidelines"></a>디자인 지침

대리자가 아닌 이벤트를 함수 매개 변수로 전달하는 것이 좋습니다. [**winrt::event**](/uwp/cpp-ref-for-winrt/event)의 **add** 함수는 하나의 예외입니다. 이 경우에는 대리자를 전달해야 하기 때문입니다. 대리자는 하나 또는 여러 개의 클라이언트 등록을 지원하는지 여부에 따라 다양한 Windows 런타임 언어에 걸쳐 다른 형식을 사용할 수 있기 때문에 이 지침이 제공됩니다. 여러 구독자 모델이 있는 이벤트는 훨씬 더 예측 가능하고 일관성 있는 옵션을 구성합니다.

이벤트 처리기 대리자의 서명은 두 개의 매개 변수인 ‘발신자’(**IInspectable**) 및 *args*([**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)와 같은 일부 이벤트 인수 형식)로 구성됩니다.

내부 API를 디자인하는 경우에는 이 지침이 반드시 적용되는 것은 아닙니다. 그래도 내부 API는 일반적으로 시간이 지남에 따라 공개됩니다.

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 작성](author-apis.md)
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
* [C++/WinRT를 사용한 Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)