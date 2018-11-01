---
author: stevewhims
description: XAML 항목에 효과적으로 바인딩되는 컬렉션은 *관찰 가능한* 컬렉션으로 알려져 있습니다. 이번 항목에서는 관찰 가능한 컬렉션을 구현하여 사용하는 방법과 XAML 항목에 바인딩하는 방법에 대해서 설명합니다.
title: XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 바인딩, 컬렉션
ms.localizationpriority: medium
ms.openlocfilehash: f9d9d6a2aafe1b81ff331bac92662b111eb48956
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5875145"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩

XAML 항목에 효과적으로 바인딩되는 컬렉션은 *관찰 가능한* 컬렉션으로 알려져 있습니다. 이 아이디어는 *관찰자 패턴*이라고 알려진 소프트웨어 디자인 패턴에 바탕을 두고 있습니다. 이 항목에서 관찰 가능한 컬렉션을 구현 하는 방법을 보여 줍니다 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), XAML에 바인딩하는 방법을 항목을 제어 하 고 있습니다.

이번 연습은 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)에서 생성된 프로젝트에 바탕을 두고 있으며, 또한 해당 항목에서 설명한 개념에 추가하여 진행됩니다.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="what-does-observable-mean-for-a-collection"></a>컬렉션을 얘기할 때 *관찰 가능하다는 것*은 무슨 뜻입니까?
컬렉션을 나타내는 런타임 클래스에서 이벤트 추가 또는 삭제 시 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 이벤트가 발생하도록 선택하는 경우 이 런타임 클래스는 관찰 가능한 컬렉션이 됩니다. XAML 항목 컨트롤은 업데이트된 컬렉션을 가져와 현재 요소를 표시하도록 스스로 업데이트함으로써 이러한 이벤트에 바인딩하여 처리할 수 있습니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 설치 및 사용에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>**BookSkus** 컬렉션을 **BookstoreViewModel**에 추가

[XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)에서는 **BookSku** 형식의 속성을 기본 보기 모델에 추가하였습니다. 이 단계에서는 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 공장 함수 템플릿 동일한 보기 모델에서 **BookSku** 의 관찰 가능한 컬렉션을 구현 하는 데 사용 됩니다.

> [!NOTE]
> Windows SDK 버전 10.0.17763.0 (Windows 10, 버전 1809)를 설치 하지 않은 또는 나중에 다음 참조 [는 Windows SDK의 이전 버전을 사용 하는 경우](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) **winrt::single_ 대신 사용할 수 있는 관찰 가능한 벡터 템플릿의 목록에 대 한 경우 threaded_observable_vector**.

새로운 속성을 `BookstoreViewModel.idl`로 선언합니다.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> 위의 MIDL 3.0 목록에 **BookSkus** 속성의 형식이 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) note 합니다. 이 항목의 다음 섹션에서는 **BookSkus**에 [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) 항목 소스를 바인딩 됩니다. 목록 상자 항목 컨트롤, 이며 [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 올바르게 설정 하려면 설정할 필요가 **IObservableVector** 형식의 값에 (또는 **IVector**) **IInspectable**또는 상호 운용성 형식과 같은 [** IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

저장 후 빌드합니다. `BookstoreViewModel.h`와 `BookstoreViewModel.cpp`의 접근자 스텁을 `Generated Files` 폴더에 복사하여 구현합니다.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

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
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
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
