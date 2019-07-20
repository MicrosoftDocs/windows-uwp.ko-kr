---
description: C++/WinRT는 컬렉션을 구현하거나 전달하려는 경우 많은 시간과 노력을 절약할 수 있는 함수와 기본 클래스를 제공합니다.
title: C++/WinRT로 작성된 컬렉션
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 컬렉션
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4f1b15ec377b030a467dded634abe3fdde717896
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270151"
---
# <a name="collections-with-cwinrt"></a>C++/WinRT로 작성된 컬렉션

내부적으로 Windows 런타임 컬렉션에는 많은 복잡한 이동 부분이 있습니다. 그러나 Windows 런타임 함수에 컬렉션 개체를 전달하거나 자체 컬렉션 속성과 컬렉션 형식을 구현하려는 경우에는 지원을 위한 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)에 함수 및 기본 클래스가 있습니다. 이 기능을 사용하면 복잡성을 줄일 수 있고 시간 및 노력의 오버헤드가 절약됩니다.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)는 요소의 임의 액세스 컬렉션을 통해 구현된 Windows 런타임 인터페이스입니다. **IVector**를 직접 구현하려면 [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_) 및 [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)도 구현해야 합니다. 사용자 지정 컬렉션 형식이 ‘필요’한 경우에도 수행할 작업이 많습니다.  하지만 **std::vector**, **std::map** 또는 **std::unordered_map**에 데이터가 있고 이 데이터를 Windows 런타임 API에만 전달하려는 경우에는 가능하면 해당 수준의 작업을 피하려고 합니다. 또한 C++/WinRT를 사용하면 최소한의 노력으로 효율적으로 컬렉션을 만들 수 있으므로 해당 작업을 ‘피할 수 있습니다’. 

[XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩](binding-collection.md)을 참조하세요.

## <a name="helper-functions-for-collections"></a>컬렉션용 도우미 함수

### <a name="general-purpose-collection-empty"></a>범용 컬렉션, 비어 있음

이 섹션에서는 처음에 비어 있는 컬렉션을 만든 ‘후’ 채우는 시나리오를 다룹니다. 

범용 컬렉션을 구현하는 형식의 새 개체를 검색하려면 [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) 함수 템플릿을 호출하면 됩니다. 개체는 [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)로 반환되며, 이 개체는 반환된 개체의 함수 및 속성을 호출하는 데 사용되는 인터페이스입니다.

다음 코드 예제를 복사하여 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트의 주 원본 코드 파일에 직접 붙여넣으려는 경우 먼저 프로젝트 속성에서 **미리 컴파일된 헤더 사용 안 함**을 설정합니다.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

위의 코드 예제에서 살펴본 대로, 컬렉션을 만든 후에 요소를 추가하고 반복하며, 일반적으로 API에서 수신한 Windows 런타임 컬렉션 개체를 처리하는 것처럼 개체를 처리할 수 있습니다. 컬렉션에 대한 변경할 수 없는 보기가 필요한 경우 표시된 대로 [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)를 호출할 수 있습니다. 위에 표시된 컬렉션 만들기 및 사용 패턴은 데이터를 API에 전달하거나 API에서 데이터를 가져오는 간단한 시나리오에 적합합니다. [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)이 필요한 경우 항상 **IVector** 또는 **IVectorView**를 전달할 수 있습니다.

위의 코드 예제에서 **winrt::init_apartment** 호출은 기본적으로 다중 스레드 아파트에서 Windows 런타임의 스레드를 초기화합니다. 이 호출에서 COM도 초기화합니다.

### <a name="general-purpose-collection-primed-from-data"></a>범용 컬렉션, 데이터에서 초기화됨

이 섹션에서는 컬렉션을 만들고 동시에 채우는 시나리오를 다룹니다.

이전 코드 예제에 있는 **Append** 호출의 오버헤드를 피할 수 있습니다. 원본 데이터가 이미 있거나 Windows 런타임 컬렉션 개체를 만들기 전에 원본 데이터를 채우는 방법을 선호할 수도 있습니다. 방법은 다음과 같습니다.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

위의 `coll1`을 사용하는 것처럼 데이터를 포함하는 임시 개체를 **winrt::single_threaded_vector**에 전달할 수 있습니다. 또는 **std::vector**(다시 액세스하지 않는다고 가정)를 함수로 이동할 수 있습니다. 두 경우에 모두 *rvalue*를 함수로 전달합니다. 이렇게 하면 컴파일러가 효율적으로 작동하고 데이터를 복사하지 않아도 됩니다. *rvalue*에 대해 자세히 알아보려면 [값 범주 및 해당 참조](cpp-value-categories.md)를 참조하세요.

XAML 항목 컨트롤을 컬렉션에 바인딩할 수 있습니다. 하지만 [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 제대로 설정하려면 **IInspectable**의 **IVector** 형식 값으로 설정하거나 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)와 같은 상호 운용성 형식 값으로 설정해야 합니다.

바인딩에 적합한 형식의 컬렉션을 생성하고 요소를 추가하는 코드 예제는 다음과 같습니다. [XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩](binding-collection.md)에서 이 코드 예제의 컨텍스트를 확인할 수 있습니다.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

데이터에서 Windows 런타임 컬렉션을 만들고 복사 작업 없이도 API로 전달할 수 있는 보기를 가져올 수 있습니다.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

위의 예제에서 만든 컬렉션은 XAML 항목 컨트롤에 바인딩’할 수 있지만’ 컬렉션을 관찰할 수는 없습니다. 

### <a name="observable-collection"></a>Observable 컬렉션

‘관찰 가능한’ 컬렉션을 구현하는 형식의 새 개체를 검색하려면 요소 형식에 상관없이 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 함수 템플릿을 호출합니다.  그러나 XAML 항목 컨트롤에 바인딩하는 데 적합한 관찰 가능한 컬렉션을 만들려면 **IInspectable**을 요소 형식으로 사용합니다.

개체는 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)로 반환되며, 이 개체는 반환된 개체의 함수 및 속성을 호출하는 데 사용되는 인터페이스이거나 바인딩된 컨트롤입니다.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

UI(사용자 인터페이스) 컨트롤을 관찰 가능한 컬렉션에 바인딩하는 방법에 대한 자세한 내용과 코드 예제를 보려면 [XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩](binding-collection.md)을 참조하세요.

### <a name="associative-collection-map"></a>연결 컬렉션(맵)

앞에서 살펴본 두 가지 함수의 경우 연결 컬렉션 버전이 있습니다.

- [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) 함수 템플릿은 관찰 불가능한 연결 컬렉션을 [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)으로 반환합니다.
- [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) 함수 템플릿은 관찰 가능한 연결 컬렉션을 [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_)으로 반환합니다.

필요한 경우 **std::map** 또는 **std::unordered_map** 형식의 *rvalue*를 함수에 전달하여 데이터로 이 컬렉션을 초기화할 수 있습니다.

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>단일 스레드

이 함수의 이름에 있는 “단일 스레드”는 함수가 동시성을 제공하지 않음을 나타냅니다. 즉, 이 함수는 스레드로부터 안전하지 않습니다. 이 함수에서 반환된 개체는 모두 민첩하므로 스레드 설명은 아파트와 관련이 없습니다([C++/WinRT의 Agile 개체](agile-objects.md) 참조). 개체가 단일 스레드일 뿐입니다. 또한 ABI(Application Binary Interface)를 통해 어떻게든 데이터만 전달하려는 경우 완전히 적합합니다.

## <a name="base-classes-for-collections"></a>컬렉션용 기본 클래스

완벽한 유연성을 제공하기 위해 고유한 사용자 지정 컬렉션을 구현하는 경우 어려운 방법을 피하려고 할 것입니다. 예를 들어 사용자 지정 벡터 보기가 ‘C++/WinRT 기본 클래스의 지원 없이’ 표시되는 경우입니다. 

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

대신, [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) 구조체 템플릿에서 사용자 지정 벡터 보기를 훨씬 더 쉽게 파생시킬 수 있으며, 데이터를 포함하는 컨테이너를 공개하려면 **get_container** 함수를 구현하면 됩니다.

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

**get_container**에서 반환된 컨테이너는 **:::: vector_view_base**가 예상하는 **시작** 및 **종료** 인터페이스를 제공해야 합니다. 위의 예제와 같이 **std::vector**가 이 인터페이스를 제공합니다. 그러나 고유한 사용자 지정 컨테이너를 포함하여 동일한 계약을 충족하는 모든 컨테이너를 반환할 수 있습니다.

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

이 기본 클래스는 C++/WinRT에서 사용자 지정 컬렉션 구현을 지원하기 위해 제공합니다.

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

위의 코드 예제를 참조하세요.

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>중요 API
* [ItemsControl.ItemsSource 속성](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector 인터페이스](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector 인터페이스](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base 구조체 템플릿](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base 구조체 템플릿](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base 구조체 템플릿](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base 구조체 템플릿](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map 함수 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map 함수 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector 함수 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector 함수 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base 구조체 템플릿](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base 구조체 템플릿](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>관련 항목
* [값 범주 및 해당 참조](cpp-value-categories.md)
* [XAML 항목 컨트롤(C++/WinRT 컬렉션에 바인딩)](binding-collection.md)
