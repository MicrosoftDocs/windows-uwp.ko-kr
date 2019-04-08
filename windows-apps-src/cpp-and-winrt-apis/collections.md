---
description: C++/WinRT는 컬렉션을 구현하거나 전달하려는 경우 많은 시간과 노력을 절약할 수 있는 함수와 기본 클래스를 제공합니다.
title: C++/WinRT로 작성된 컬렉션
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 컬렉션
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a50ab5f70faa0c8f8b73eada38444bcafd444d8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635438"
---
# <a name="collections-with-cwinrt"></a>C++/WinRT로 작성된 컬렉션

내부적으로 Windows 런타임 컬렉션에 이동 하는 복잡 한 부분이 많이 있습니다. 하지만 함수 및 기본 클래스에는 컬렉션 개체를 Windows 런타임 함수에 전달 하거나 고유한 컬렉션 속성 및 컬렉션 형식 구현를 하려는 경우 [C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 하도록 지원 합니다. 이러한 기능에는 손이에서 복잡성 가져오고 시간과 노력에는 많은 오버 헤드를 저장 합니다.

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_) 요소의 모든 임의 액세스 컬렉션에 의해 구현 된 Windows 런타임 인터페이스입니다. 구현 하는 경우 **IVector** 직접 것도 구현 해야 [ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)하십시오 [ **IVectorView** ](/uwp/api/windows.foundation.collections.ivectorview_t_), 및 [ **IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)합니다. 경우에 있습니다 *필요한* 있는 많은 작업을 사용자 지정 컬렉션 형식입니다. 에 데이터가 있을 경우에 **std:: vector** (또는 **std:: map**, 또는 **std:: unordered_map**) 하려는 모든는 Windows 런타임 api에 전달 하 고 수행 하지 하려는 가능한 경우 작업의 해당 수준입니다. 하 고이 방지할 *는* 가능 하기 때문에 C + + WinRT를 사용 하면 효율적이 고 간편 하 게 컬렉션을 만들 수 있습니다.

도 참조 하세요 [XAML 컨트롤을 항목; 바인딩할 C + + /cli WinRT 컬렉션](binding-collection.md)합니다.

> [!NOTE]
> Windows SDK (Windows 10, 버전 1809) 10.0.17763.0 버전을 설치 하지 않은 경우 나중에 다음 하지 않아도 함수 및이 항목에 설명 된 기본 클래스에 대 한 액세스. 대신 참조 하세요 [Windows SDK의 이전 버전이 있는 경우](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) 대신 사용할 수 있는 observable 벡터 템플릿 목록에 대 한 합니다.

## <a name="helper-functions-for-collections"></a>컬렉션에 대 한 도우미 함수

### <a name="general-purpose-collection-empty"></a>빈 범용 컬렉션

이 섹션에서는 처음에 비어 있는 컬렉션을 만드는 하려는 시나리오를 다룹니다. 입력 한 다음 *후* 생성 합니다.

범용 컬렉션을 구현 하는 형식의 새 개체를 검색 하려면 호출할 수 있습니다 합니다 [ **winrt::single_threaded_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-vector) 함수 템플릿. 개체가으로 반환 되는 [ **IVector**](/uwp/api/windows.foundation.collections.ivector_t_), 반환된 된 개체의 함수 및 속성 호출는 인터페이스입니다.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
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

위의 코드 예제에서 보듯이 컬렉션을 만든 후 요소를 추가, 반복를 API에서 받은 모든 Windows 런타임 컬렉션 개체와 마찬가지로 일반적으로 개체를 처리 합니다. 뷰를 변경할 수 없는 컬렉션에 대해 필요한 경우 호출할 수 있습니다 [ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)표시 된 것 처럼 합니다. 위에 표시 된 패턴&mdash;만들기 및 컬렉션 사용&mdash;API에서 데이터를 가져오거나 데이터를 전달 하려는 간단한 시나리오에 적합 합니다. 전달할 수 있습니다는 **IVector**, 또는 **IVectorView**어디서 나는 [ **IIterable** ](/uwp/api/windows.foundation.collections.iiterable_t_) 예상 됩니다.

에 대 한 호출에 위의 코드 예에서 **winrt::init_apartment** COM을 초기화; 기본적으로 다중 스레드 아파트에 있습니다.

### <a name="general-purpose-collection-primed-from-data"></a>데이터에서 분산이 범용 컬렉션

이 섹션에서는 컬렉션을 만들고 동시에 채울 하려는 시나리오를 다룹니다.

에 대 한 호출의 오버 헤드를 방지할 수 있습니다 **Append** 이전 코드 예제에서입니다. 원본 데이터에 이미 있을 수 있습니다 또는 Windows 런타임 컬렉션 개체를 만들기 전에 원본 데이터를 채울 수도 있습니다. 작업을 수행 하는 방법은 다음과 같습니다.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

데이터를 포함 하는 임시 개체를 전달할 수 있습니다 **winrt::single_threaded_vector**에서와 마찬가지로 `coll1`위의 합니다. 이동할 수 있습니다는 **std:: vector** (있습니다 없습니다 수 액세스 다시 가정) 함수에 있습니다. 두 경우 모두 전달 하는 *rvalue* 함수로 합니다. 효율성 및 데이터를 복사 하지 않으려면 컴파일러를 수입니다. 자세히 알고 싶다면 *rvalue*를 참조 하십시오 [값 범주 및 해당 참조를](cpp-value-categories.md)입니다.

컬렉션에 XAML 항목 컨트롤을 바인딩하려면 다음을 할 수 있습니다. 하지만 올바르게 설정에 [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 형식의 값으로 설정 해야 하는 속성을 **IVector** 의 **IInspectable** (또는 같은 상호 운용성 형식의 [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). 바인딩에 대 한 적절 한 형식의 컬렉션을 생성 하 고 요소를 추가 하는 코드 예제는 다음과 같습니다.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

데이터에서 Windows 런타임 컬렉션을 만들 수 있으며이 대 한 뷰를 아무것도 복사 하지 않고 모든 API에 전달할 준비 수 있습니다.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

위의 예제에서 컬렉션을 만들겠습니다 *수* 에 바인딩할 수는 XAML 컨트롤을 항목 왔지만 컬렉션 관찰 가능 개체입니다.

### <a name="observable-collection"></a>관찰 가능한 컬렉션

구현 하는 형식의 새 개체를 검색 하는 *관찰 가능 개체* 컬렉션을 호출 합니다 [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 함수 템플릿을 사용 하 여 요소 형식입니다. 사용 하 여 observable 컬렉션 XAML 항목 컨트롤에 바인딩하는 데 적합 하도록 하지만 **IInspectable** 요소 형식입니다.

개체가으로 반환 되는 [ **IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), 인터페이스는 사용자 (또는 바인딩되는 컨트롤) 호출 반환된 된 개체의 함수 및 속성입니다.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

자세한 세부 정보 및 코드 예제에 대 한 바인딩 사용자에 대 한 인터페이스 (UI) 컨트롤을 관찰 가능한 컬렉션을 참조 하세요 [XAML 컨트롤을 항목; 바인딩할 C + + /cli WinRT 컬렉션](binding-collection.md)합니다.

### <a name="associative-collection-map"></a>연결 컬렉션 (map)

연결 컬렉션 버전 살펴보았습니다 두 함수의 가지가 있습니다.

- 합니다 [ **winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map) 으로 관찰이 불가능 한 결합형 컬렉션을 반환 하는 함수 템플릿을 [ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)합니다.
- 합니다 [ **winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) 함수 템플릿은으로 관찰 가능한 연결 컬렉션을 반환 합니다.는 [ **IObservableMap** ](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

함수에 전달 하 여 데이터를 사용 하 여 이러한 컬렉션을 초기화할 필요에 따라 수를 *rvalue* 형식의 **std:: map** 하거나 **std:: unordered_map**합니다.

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

"단일 스레드" 이러한 함수 중 이름에는 모든 동시성을 제공 하지 않는 나타냅니다&mdash;말해 없는 것 이므로 스레드로부터 안전 합니다. 이러한 함수에서 반환 된 개체는 모든 agile 스레드의 멘 션 아파트에 관련 되므로 (참조 [Agile 개체 C + + /cli WinRT](agile-objects.md)). 죠 개체는 단일 스레드입니다. 및 데이터는 한 가지 방법은 또는 다른 응용 프로그램 이진 인터페이스 ABI ()를 통해 전달 하려는 경우 완전히 적합 합니다.

## <a name="base-classes-for-collections"></a>컬렉션에 대 한 기본 클래스

전체 유연성을 위해 사용자 지정 컬렉션을 직접 구현 하려는, 경우에 어려운 방법으로 그렇게 하지 않으려면 원하는 됩니다. 예를 들어,이 사용자 지정 벡터 보기는 모양을 *를 사용 하지 않고 C + + /cli WinRT의 기본 클래스*합니다.

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

대신 것이 훨씬 쉽습니다에서 사용자 지정 벡터 뷰를 파생 하는 [ **winrt::vector_view_base** ](/uwp/cpp-ref-for-winrt/vector-view-base) 구조체 템플릿을 구현할 및 합니다 **get_container** 함수를 데이터를 보유 하는 컨테이너를 노출 합니다.

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

반환 된 컨테이너 **get_container** 제공 해야 합니다는 **시작** 및 **끝** 는 인터페이스 **winrt::vector_view_base** 필요 합니다. 위의 예제 에서처럼 **std:: vector** 를 제공 합니다. 하지만 사용자 고유의 사용자 지정 컨테이너를 포함 하 여 동일한 계약을 수행 하는 모든 컨테이너를 반환할 수 있습니다.

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

이 기본 클래스는 C + + /cli WinRT 사용자 지정 컬렉션을 구현할 수 있도록 제공 합니다.

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

위의 코드 예제를 참조 하세요.

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
