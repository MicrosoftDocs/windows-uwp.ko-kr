---
description: XAML 항목에 효과적으로 바인딩되는 컬렉션은 *관찰 가능한* 컬렉션으로 알려져 있습니다. 이 항목에서는 관찰 가능한 컬렉션을 구현하여 사용하는 방법과 XAML 항목에 바인딩하는 방법에 대해서 설명합니다.
title: XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 바인딩, 컬렉션
ms.localizationpriority: medium
ms.openlocfilehash: c3551ebcc59ebfe426b0be8d5bd20f7578517a25
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649208"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩

XAML 항목에 효과적으로 바인딩되는 컬렉션은 *관찰 가능한* 컬렉션으로 알려져 있습니다. 이 아이디어는 *관찰자 패턴*이라고 알려진 소프트웨어 디자인 패턴에 바탕을 두고 있습니다. 이 항목의 관찰 가능한 컬렉션을 구현 하는 방법을 보여 줍니다 [C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), 및 XAML을 바인딩하는 방법 항목에 컨트롤입니다.

이번 연습은 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)에서 생성된 프로젝트에 바탕을 두고 있으며, 또한 해당 항목에서 설명한 개념에 추가하여 진행됩니다.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="what-does-observable-mean-for-a-collection"></a>컬렉션을 얘기할 때 *관찰 가능하다는 것*은 무슨 뜻입니까?
컬렉션을 나타내는 런타임 클래스에서 이벤트 추가 또는 삭제 시 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 이벤트가 발생하도록 선택하는 경우 이 런타임 클래스는 관찰 가능한 컬렉션이 됩니다. XAML 항목 컨트롤은 업데이트된 컬렉션을 가져와 현재 요소를 표시하도록 스스로 업데이트함으로써 이러한 이벤트에 바인딩하여 처리할 수 있습니다.

> [!NOTE]
> 설치 및 C +를 사용 하는 방법에 대 한 정보에 대 한 + WinRT VSIX Visual Studio Extension () (지원을 제공 하는 프로젝트 템플릿)를 참조 하세요 [Visual Studio 지원 C + + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다.

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>**BookSkus** 컬렉션을 **BookstoreViewModel**에 추가

[XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)에서는 **BookSku** 형식의 속성을 기본 보기 모델에 추가하였습니다. 이 단계에서는 사용 합니다 [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 팩터리 함수 템플릿을 관찰 가능한 컬렉션을 구현 하는 데 도움이 **BookSku** 에 동일한 뷰 모델입니다.

> [!NOTE]
> Windows SDK (Windows 10, 버전 1809) 10.0.17763.0 버전을 설치 하지 않은 또는 나중에 다음 참조 하는 경우 [Windows SDK의 이전 버전이 있는 경우](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) 대신사용할수있는observable벡터템플릿의목록은**winrt::single_threaded_observable_vector**합니다.

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
> MIDL 3.0를 나열 하는 위의 형식의 합니다 **BookSkus** 속성은 [ **IObservableVector** ](/uwp/api/windows.foundation.collections.ivector_t_) 의 [ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). 이 항목의 다음 섹션에서는 우리가에서는 바인딩 항목 원인을 [ **ListBox** ](/uwp/api/windows.ui.xaml.controls.listbox) 하 **BookSkus**합니다. 목록 상자는 항목 컨트롤에 올바르게 설정 하 고는 [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 형식의 값으로 설정 해야 하는 속성을 **IObservableVector** (또는 **IVector**)의 **IInspectable**, 또는와 같은 상호 운용성 형식의 [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)합니다.

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
* [사용 Api을 사용 하 여 C + + /cli WinRT](consume-apis.md)
* [작성 Api C + + /cli WinRT](author-apis.md)
