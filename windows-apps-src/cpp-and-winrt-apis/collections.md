---
author: stevewhims
description: C + + /cli WinRT 함수 및 다양 한 구현 및/또는 컬렉션을 전달 하 려 할 때 시간과 노력을 저장 하는 기본 클래스를 제공 합니다.
title: 컬렉션을 사용 하 여 C + + /cli WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 창 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 컬렉션
ms.localizationpriority: medium
ms.openlocfilehash: 1ef6fbfab45197c868296186363c168a6c443247
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3930789"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>컬렉션을 [C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

내부적으로 Windows 런타임 컬렉션에 복잡 한 가동 부품이 많이 있습니다. 하지만 함수 및 C + 기본 클래스는 컬렉션 개체는 Windows 런타임 함수에 전달 하거나 고유의 컬렉션의 속성 및 컬렉션 형식을 구현할 수 경우 + WinRT 지원 해. 이러한 기능은 복잡 한, 손으로 밖으로 걸리고 시간과 노력에 많은 오버 헤드를 저장.

> [!IMPORTANT]
> 이 항목에 설명 된 기능은 이상이 [Windows 10 SDK 미리 보기 빌드 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK), 설치한 경우 사용할 수 있습니다.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) 임의 액세스 컬렉션의 요소에 의해 구현 된 Windows 런타임 인터페이스입니다. **IVector** 를 직접 구현 하는 경우 [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)및 [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)를 구현 해야 합니다. *필요한* 사용자 지정 컬렉션을 입력 하는 경우에 많은 작업입니다. 하지만 데이터는 **std:: vector** (또는 **std:: map**, 또는 **std::unordered_map**는) 있고 Windows 런타임 API에 전달 하는 작업을 수행 하려면 모든 다음을 가능 하면 그러한 수준의 작업을 수행 하지. 하 고 있기 때문에 ** 가능한 대신 C + + /cli WinRT를 사용 하면 효율적이 고 적은 노력으로 컬렉션을 만들 수 있습니다.

## <a name="helper-functions-for-collections"></a>컬렉션에 대 한 도우미 함수

### <a name="general-purpose-collection-empty"></a>빈 범용 컬렉션

새 범용 컬렉션을 구현 하는 형식의 개체를 검색 하려면 [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) 함수 템플릿을 호출할 수 있습니다. [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)으로 해당 개체를 반환 하며 반환 되는 개체, 함수 및 속성 호출을 통해 인터페이스 됩니다.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    init_apartment();

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

위의 코드 예제에서 볼 수 컬렉션을 만든 후 요소를 추가, 반복, 하 수 API 로부터 받은 모든 Windows 런타임 컬렉션 개체와 마찬가지로 일반적으로 개체를 처리 합니다. 컬렉션 변경 불가능 한 뷰를 해야 할 경우와 같이 [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)호출할 수 있습니다. 위의 패턴&mdash;만들기 및 컬렉션 사용의&mdash;를 API에서 데이터를 가져오거나 데이터를 전달 하는 간단한 시나리오에 적합 합니다. **IVector**또는 **IVectorView**를 전달할 수 있습니다, [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_) 는 예상 되는 곳입니다.

### <a name="general-purpose-collection-primed-from-data"></a>데이터에서 전기 충전 완료 하는 범용 컬렉션

위의 코드 예제에서 볼 수 있듯이 **추가** 에 대 한 호출의 오버 헤드를 방지할 수 있습니다. 원본 데이터의 경우 이미 또는 Windows 런타임 컬렉션 개체를 만들기 전에 채울 수도 있습니다. 그 방법은 다음과 같습니다.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

**Winrt::single_threaded_vector**데이터를 포함 하는 임시 개체를 전달할 수와 마찬가지로 `coll1`, 위. **Std:: vector** (있습니다 없습니다 수 액세스 다시 가정)를 이동할 수 있습니다 또는 함수입니다. 두 경우 모두 *rvalue* 는 함수에 전달 합니다. 효율적 이어야 하 고 데이터를 복사 하지 않기 위해 컴파일러가 있습니다. *Rvalue*에 대해 자세히 알고 싶으면 [범주 값 및 참조를](cpp-value-categories.md)참조 하십시오.

XAML 항목 컨트롤 컬렉션에 연결 하려면 다음을 수행할 수 있습니다. 하지만 [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 제대로 설정 하려면 할 경우 **IVector** 의 **IInspectable** (또는 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)와 같은 상호 운용성 형식) 형식의 값을 설정할 수 있습니다. 바인딩의 경우 적합 한 형식의 컬렉션을 생성 하 고 요소를 추가 하는 코드 예제는 다음과 같습니다.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

데이터를 Windows 런타임 컬렉션을 만들 수 있으며 보기에 아무 것도 복사 하지 않고 모든 API에 전달할 준비.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

위의 예제에서 컬렉션 생성 *수* 바인딩할 XAML 항목 컨트롤입니다. 그러나 컬렉션에는 관찰 되지 않습니다.

### <a name="observable-collection"></a>눈에 띄는 컬렉션

*관찰 가능* 컬렉션을 구현 하는 형식의 새 개체를 검색 하려면 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 함수 템플릿이 요소 형식을 사용 하 여 호출 합니다. 하지만 XAML 항목 컨트롤에 바인딩하는 데 적합 한 관찰 가능한 컬렉션을 하려면 **IInspectable** 의 요소 형식으로 사용 합니다.

개체는 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)로 반환 됩니다 및 인터페이스를 통해 사용자 (또는 연결 된 컨트롤) 속성을 호출할 반환 된 개체의 함수입니다.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

자세한 정보 및 코드 예제에 대 한 바인딩 사용자에 대 한 인터페이스 (UI) 컨트롤을 관찰 가능한 컬렉션을 참조 하십시오. [XAML 항목 제어; 바인딩할 C + + /cli WinRT 컬렉션](binding-collection.md).

### <a name="associative-collection-map"></a>연결 모음 (맵)

두 가지 기능을 살펴보았습니다 연관 컬렉션 버전이 있습니다.

- [**Winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) 함수 템플릿을 [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)으로 관찰이 아닌 결합형 컬렉션을 반환 합니다.
- [**Winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) 함수 템플릿에는 [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_)으로 눈에 띄는 연관 컬렉션을 반환 합니다.

**Std:: map** 형식 또는 **std::unordered_map**는 *rvalue* 를 함수에 전달 하 여 데이터를 사용 하 여 이러한 컬렉션을 초기화할 필요에 따라 있습니다.

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

모든 동시성을 제공 하지 표시 하는 "단일 스레드"에서이 함수의 이름은&mdash;즉, 달라도 스레드로부터 안전 합니다. 이러한 함수에서 반환 되는 개체는 모두 민첩 한 스레드를 언급 하는 것은 아파트를 상관이 없습니다 (참조 [개체에서 C + + /cli WinRT](agile-objects.md)). 단일 스레드 개체는 단지입니다. 하 고 전적으로 응용 프로그램 이진 인터페이스 (ABI)에서 둘 중 한 가지 방법은 데이터를 전달 하려는 경우에 적합 합니다.

## <a name="base-classes-for-collections"></a>컬렉션에 대 한 기본 클래스

완벽 한 유연성을 위한 사용자 지정 컬렉션을 구현 하려는 경우에 복잡 한 과정을 수행 하지 하 싶을 것입니다. 예를 들어, 이것은 사용자 지정 벡터 보기는 같이 표시 *C + 도움 없이 + WinRT의 기본 클래스*.

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

대신, 훨씬 쉽습니다 [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) 구조체 템플릿에서 사용자 지정 벡터 보기를 추출 하 여 데이터를 보유 하는 컨테이너를 제공 하는 **get_container** 함수를 구현 합니다.

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

**Get_container** 에서 반환 된 컨테이너 **begin** 및 **end** 인터페이스는 **winrt::vector_view_base** 를 제공 해야 것으로 예상 됩니다. 위 예제에서와 같이 하는 **std:: vector** 를 제공 합니다. 하지만 직접 사용자 지정 컨테이너를 포함 하 여 동일한 계약을 fulfils 하는 컨테이너를 반환할 수 있습니다.

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

이것은 기본 클래스는 C + + 사용자 지정 컬렉션을 구현 하기 위해 WinRT를 제공 합니다.

### [<a name="winrtvectorviewbase"></a>winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

위의 코드 예제를 참조 하십시오.

### [<a name="winrtvectorbase"></a>winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### [<a name="winrtobservablevectorbase"></a>winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### [<a name="winrtmapviewbase"></a>winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### [<a name="winrtmapbase"></a>winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### [<a name="winrtobservablemapbase"></a>winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [winrt::map_base 구조체를 템플릿](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base 구조체를 템플릿](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base 구조체를 템플릿](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base 구조체를 템플릿](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base 구조체를 템플릿](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base 구조체를 템플릿](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>관련 항목
* [범주 값 및 참조](cpp-value-categories.md)
* [XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩](binding-collection.md)
