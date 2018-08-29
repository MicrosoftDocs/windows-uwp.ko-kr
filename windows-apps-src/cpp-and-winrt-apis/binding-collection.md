---
author: stevewhims
description: XAML 항목에 효과적으로 바인딩되는 컬렉션은 *관찰 가능한* 컬렉션으로 알려져 있습니다. 이번 항목에서는 관찰 가능한 컬렉션을 구현하여 사용하는 방법과 XAML 항목에 바인딩하는 방법에 대해서 설명합니다.
title: XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 바인딩, 컬렉션
ms.localizationpriority: medium
ms.openlocfilehash: 9ba935b1a5316c2d7af9c7681705595efea7ca08
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2918353"
---
# <a name="xaml-items-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-collection"></a>XAML 항목 컨트롤, [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 컬렉션 바인딩
> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

XAML 항목에 효과적으로 바인딩되는 컬렉션은 *관찰 가능한* 컬렉션으로 알려져 있습니다. 이 아이디어는 *관찰자 패턴*이라고 알려진 소프트웨어 디자인 패턴에 바탕을 두고 있습니다. 이번 항목에서는 C++/WinRT에서 관찰 가능한 컬렉션을 구현하는 방법과 XAML 항목을 이 속성에 바인딩하는 방법에 대해서 설명합니다.

이번 연습은 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)에서 생성된 프로젝트에 바탕을 두고 있으며, 또한 해당 항목에서 설명한 개념에 추가하여 진행됩니다.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="what-does-observable-mean-for-a-collection"></a>컬렉션을 얘기할 때 *관찰 가능하다는 것*은 무슨 뜻입니까?
컬렉션을 나타내는 런타임 클래스에서 이벤트 추가 또는 삭제 시 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 이벤트가 발생하도록 선택하는 경우 이 런타임 클래스는 관찰 가능한 컬렉션이 됩니다. XAML 항목 컨트롤은 업데이트된 컬렉션을 가져와 현재 요소를 표시하도록 스스로 업데이트함으로써 이러한 이벤트에 바인딩하여 처리할 수 있습니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 설치 및 사용에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

## <a name="implement-singlethreadedobservablevectorlttgt"></a>**single_threaded_observable_vector&lt;T&gt;** 구현
관찰 가능한 벡터 템플릿을 [**IObservableVector&lt;T&gt;**](/uwp/api/windows.foundation.collections.iobservablevector_t_)의 유용한 범용 구현체로 사용하는 것은 바람직합니다. 아래는 이름이 **single_threaded_observable_vector\<T\>** 인 클래스를 나열한 것입니다.

> [!NOTE]
> [Windows 10 SDK Preview 빌드 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)설치한 경우 나중에 사용할 수 있습니다 직접 **winrt::single_threaded_observable_vector\ < 하며 >** factory 함수 아래의 코드 목록 대신 (살펴보겠습니다 정확한 코드 나중 이 항목에서). 하지 않는 이미 해당 버전의 SDK 하는 경우 다음 됩니다 쉽게 되 면 코드 목록을 버전 **winrt** 기능을 사용 하 여 전환할.

```cppwinrt
// single_threaded_observable_vector.h
#pragma once

namespace winrt::Bookstore::implementation
{
    using namespace Windows::Foundation::Collections;

    template <typename T>
    struct single_threaded_observable_vector : implements<single_threaded_observable_vector<T>,
        IObservableVector<T>,
        IVector<T>,
        IVectorView<T>,
        IIterable<T>>
    {
        event_token VectorChanged(VectorChangedEventHandler<T> const& handler)
        {
            return m_changed.add(handler);
        }

        void VectorChanged(event_token const cookie)
        {
            m_changed.remove(cookie);
        }

        T GetAt(uint32_t const index) const
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            return m_values[index];
        }

        uint32_t Size() const noexcept
        {
            return static_cast<uint32_t>(m_values.size());
        }

        IVectorView<T> GetView()
        {
            return *this;
        }

        bool IndexOf(T const& value, uint32_t& index) const noexcept
        {
            index = static_cast<uint32_t>(std::find(m_values.begin(), m_values.end(), value) - m_values.begin());
            return index < m_values.size();
        }

        void SetAt(uint32_t const index, T const& value)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values[index] = value;
            m_changed(*this, make<args>(CollectionChange::ItemChanged, index));
        }

        void InsertAt(uint32_t const index, T const& value)
        {
            if (index > m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.insert(m_values.begin() + index, value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, index));
        }

        void RemoveAt(uint32_t const index)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.erase(m_values.begin() + index);
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, index));
        }

        void Append(T const& value)
        {
            ++m_version;
            m_values.push_back(value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
        }

        void RemoveAtEnd()
        {
            if (m_values.empty())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.pop_back();
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, Size()));
        }

        void Clear() noexcept
        {
            ++m_version;
            m_values.clear();
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        uint32_t GetMany(uint32_t const startIndex, array_view<T> values) const
        {
            if (startIndex >= m_values.size())
            {
                return 0;
            }

            uint32_t actual = static_cast<uint32_t>(m_values.size() - startIndex);

            if (actual > values.size())
            {
                actual = values.size();
            }

            std::copy_n(m_values.begin() + startIndex, actual, values.begin());
            return actual;
        }

        void ReplaceAll(array_view<T const> value)
        {
            ++m_version;
            m_values.assign(value.begin(), value.end());
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        IIterator<T> First()
        {
            return make<iterator>(this);
        }

    private:

        std::vector<T> m_values;
        event<VectorChangedEventHandler<T>> m_changed;
        uint32_t m_version{};

        struct args : implements<args, IVectorChangedEventArgs>
        {
            args(CollectionChange const change, uint32_t const index) :
                m_change(change),
                m_index(index)
            {
            }

            CollectionChange CollectionChange() const
            {
                return m_change;
            }

            uint32_t Index() const
            {
                return m_index;
            }

        private:

            Windows::Foundation::Collections::CollectionChange const m_change{};
            uint32_t const m_index{};
        };

        struct iterator : implements<iterator, IIterator<T>>
        {
            explicit iterator(single_threaded_observable_vector<T>* owner) noexcept :
            m_version(owner->m_version),
                m_current(owner->m_values.begin()),
                m_end(owner->m_values.end())
            {
                m_owner.copy_from(owner);
            }

            void abi_enter() const
            {
                if (m_version != m_owner->m_version)
                {
                    throw hresult_changed_state();
                }
            }

            T Current() const
            {
                if (m_current == m_end)
                {
                    throw hresult_out_of_bounds();
                }

                return*m_current;
            }

            bool HasCurrent() const noexcept
            {
                return m_current != m_end;
            }

            bool MoveNext() noexcept
            {
                if (m_current != m_end)
                {
                    ++m_current;
                }

                return HasCurrent();
            }

            uint32_t GetMany(array_view<T> values)
            {
                uint32_t actual = static_cast<uint32_t>(std::distance(m_current, m_end));

                if (actual > values.size())
                {
                    actual = values.size();
                }

                std::copy_n(m_current, actual, values.begin());
                std::advance(m_current, actual);
                return actual;
            }

        private:

            com_ptr<single_threaded_observable_vector<T>> m_owner;
            uint32_t const m_version;
            typename std::vector<T>::const_iterator m_current;
            typename std::vector<T>::const_iterator const m_end;
        };
    };
}
```

**Append** 함수는 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 이벤트의 발생 방법을 나타냅니다.

```cppwinrt
m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
```

이벤트 인수는 요소가 삽입되었다는 것과 인덱스(여기에서는 마지막 요소)가 무엇인지 가리키는 역할을 합니다. XAML 항목 컨트롤은 이러한 인수를 통해 이벤트에 응답하거나 스스로를 최적의 방법으로 새로 고칠 수 있습니다.

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>**BookSkus** 컬렉션을 **BookstoreViewModel**에 추가
[XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)에서는 **BookSku** 형식의 속성을 기본 보기 모델에 추가하였습니다. 이번 단계에서는 **single_threaded_observable_vector&lt;T&gt;** 를 사용해 동일한 보기 모델에서 **BookSku**의 관찰 가능한 컬렉션을 구현하도록 하겠습니다.

새로운 속성을 `BookstoreViewModel.idl`로 선언합니다.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> 위의 MIDL 3.0 목록에 **BookSkus** 속성의 형식이 [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821)의 [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) note 합니다. 이 항목의 다음 섹션에서 **BookSkus**에 [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) 항목 소스를 바인딩 합니다. 목록 상자 항목 컨트롤, 이며 [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 올바르게 설정 하려면 **IVector** **IInspectable**또는 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)등의 상호 운용성 형식의 형식의 값으로 설정 해야 합니다.

저장 후 빌드합니다. `BookstoreViewModel.h`와 `BookstoreViewModel.cpp`의 접근자 스텁을 `Generated Files` 폴더에 복사하여 구현합니다.

```cppwinrt
// BookstoreViewModel.h
...
#include "single_threaded_observable_vector.h"
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="if-you-have-a-windows-10-sdk-preview-build"></a>Windows 10 SDK Preview 빌드는 것이 있는 경우
[Windows 10 SDK Preview 빌드 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)설치한 경우 또는 나중에이 코드 줄을 다음 바꾸기

```cppwinrt
m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
```

이 있습니다.

```cppwinrt
m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
```

[**Winrt:: make**](https://docs.microsoft.com/en-us/uwp/cpp-ref-for-winrt/make)를 호출 하는 대신 **winrt::single_threaded_observable_vector\ < 하며 >** factory 함수를 호출 하 여 적절 한 컬렉션 개체를 만듭니다.

## <a name="bind-a-listbox-to-the-bookskus-property"></a>ListBox를 **BookSkus** 속성에 바인딩
`MainPage.xaml`을 엽니다. 여기에는 메인 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. 다음 태그를 **Button**과 동일한 **StackPanel**에 추가합니다.

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

`MainPage.cpp`에서 코드 한 줄을 **Click** 이벤트 처리기에 추가하여 책을 컬렉션에 추가합니다.

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

이제 프로젝트를 빌드하고 실행합니다. 버튼을 클릭하여 **Click** 이벤트 처리기를 실행합니다. 앞에서 **Append** 구현체가 UI가 컬렉션의 변경 사실을 알 수 있도록 이벤트를 발생시키고, 그러면 **ListBox**가 컬렉션에 대한 쿼리를 다시 실행하여 **Items** 값을 업데이트하는 것을 확인하였습니다. 이와 마찬가지로 책 중 하나의 타이트리 변경되면 해당 타이틀 변경이 버튼과 목록 상자 모두에 반영됩니다.

## <a name="important-apis"></a>중요 API
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT를 통한 API 작성](author-apis.md)
