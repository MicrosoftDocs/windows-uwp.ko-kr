---
description: XAML 항목에 효과적으로 바인딩할 수 있는 컬렉션은 *식별할 수 있는*(observable) 컬렉션이라고 합니다. 이 항목에서는 식별할 수 있는 컬렉션을 구현하고 사용하는 방법과 XAML 항목에 바인딩하는 방법을 보여 줍니다.
title: XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 바인딩, 컬렉션
ms.localizationpriority: medium
ms.openlocfilehash: 388e8ebb062dbbb33ffb269f2adcced34a7e577c
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493648"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩

XAML 항목에 효과적으로 바인딩할 수 있는 컬렉션은 *식별할 수 있는*(observable) 컬렉션이라고 합니다. 이 아이디어는 ‘관찰자 패턴’이라고 알려진 소프트웨어 디자인 패턴에 바탕을 두고 있습니다.  이 항목에서는 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)에서 관찰 가능한 컬렉션을 구현하는 방법과 XAML 항목 컨트롤을 이 속성에 바인딩하는 방법을 보여줍니다(배경 정보는 [데이터 바인딩](/windows/uwp/data-binding) 참조).

이 항목의 과정을 따르려면 먼저 [XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md)에 설명된 프로젝트를 만드는 것이 좋습니다. 이 항목에서는 해당 프로젝트에 더 많은 코드를 추가하며 이 코드는 이 항목에서 설명하는 개념에 추가됩니다.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="what-does-observable-mean-for-a-collection"></a>컬렉션을 얘기할 때 ‘관찰 가능하다는 것’은 무슨 뜻인가요? 

컬렉션을 나타내는 런타임 클래스에서 요소 추가 또는 제거 시 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 이벤트가 발생하도록 선택하는 경우, 이 런타임 클래스는 관찰 가능한 컬렉션이 됩니다. XAML 항목 컨트롤은 업데이트된 컬렉션을 가져와 현재 요소를 표시하도록 스스로 업데이트함으로써 이러한 이벤트에 바인딩하여 처리할 수 있습니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>**BookSkus** 컬렉션을 **BookstoreViewModel**에 추가

[XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md)에서는 **BookSku** 형식의 속성을 기본 보기 모델에 추가했습니다. 이 단계에서는 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 팩터리 함수 템플릿을 사용하여 동일한 보기 모델에서 **BookSku**의 관찰 가능한 컬렉션을 구현하도록 도와줍니다.

`BookstoreViewModel.idl`에서 새 속성을 선언합니다.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
}
...
```

> [!NOTE]
> 위의 MIDL 3.0 목록에서 **BookSkus** 속성의 형식은 **BookSku**의 [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_)입니다. 이 항목의 다음 섹션에서는 [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox)의 항목 원본을 **BookSkus**에 바인딩합니다. 목록 상자는 항목 컨트롤이며, [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 제대로 설정하려면 **IObservableVector** 또는 **IVector** 형식 값으로 설정하거나 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)와 같은 상호 운용성 형식 값으로 설정해야 합니다.

> [!WARNING]
> 이 항목에 표시된 코드는 C++/WinRT 버전 2.0.190530.8 이상에 적용됩니다. 이전 버전을 사용하는 경우 표시된 코드에 몇 가지 사소한 변경을 해야 합니다. 위의 MIDL 3.0 목록에서 **BookSkus** 속성을 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_)로 변경합니다. 그 다음, 구현에서도 **IInspectable**(**BookSku** 대신)을 사용합니다.

저장 후 빌드합니다. `\Bookstore\Bookstore\Generated Files\sources` 폴더의 `BookstoreViewModel.h` 및 `BookstoreViewModel.cpp`에서 접근자 스텁을 복사합니다. 자세한 내용은 이전 항목인 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)을 참조하세요. 다음과 같이 접근자 스텁을 구현합니다.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>ListBox를 **BookSkus** 속성에 바인딩

`MainPage.xaml`을 엽니다. 여기에는 기본 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. 다음 태그를 **Button**과 동일한 **StackPanel** 내부에 추가합니다.

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

`MainPage.cpp`에서 하나의 코드 줄을 **Click** 이벤트 처리기에 추가하여 책을 컬렉션에 추가합니다.

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

이제 프로젝트를 빌드하고 실행합니다. 단추를 클릭하여 **Click** 이벤트 처리기를 실행합니다. 앞에서 **Append** 구현이 UI가 컬렉션의 변경 사실을 알 수 있도록 이벤트를 발생시키고, 그러면 **ListBox**가 컬렉션을 다시 쿼리하여 **Items** 값을 업데이트하는 것을 확인했습니다. 이와 마찬가지로 책 중 하나의 제목이 변경되면 변경된 해당 제목이 단추와 목록 상자 모두에 반영됩니다.

## <a name="important-apis"></a>중요 API

* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>관련 항목

* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT를 통한 API 작성](author-apis.md)
