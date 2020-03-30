---
description: 이 항목에서는 [C#](/visualstudio/get-started/csharp) 프로젝트의 소스 코드를 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)의 해당하는 소스 코드로 포팅하는 데 관련된 기술 세부 정보에 대해 설명합니다.
title: C#에서 C++/WinRT로 이동
ms.date: 07/15/2019
ms.topic: article
keywords: Windows 10, UWP, 표준, C++, cpp, WinRT, 프로젝션, 이식, 마이그레이션, C#
ms.localizationpriority: medium
ms.openlocfilehash: f7cd35dbf211b14dfb886fc9ba4305cd7ce56e5e
ms.sourcegitcommit: f288bcc108f9850671662c7b76c55c8313e88b42
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290050"
---
# <a name="move-to-cwinrt-from-c"></a>C#에서 C++/WinRT로 이동

이 항목에서는 [C#](/visualstudio/get-started/csharp) 프로젝트의 소스 코드를 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)의 해당하는 소스 코드로 포팅하는 데 관련된 기술 세부 정보에 대해 설명합니다.

## <a name="register-an-event-handler"></a>이벤트 처리기 등록

XAML 태그에서 이벤트 처리기를 등록할 수 있습니다.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

C#에서 **OpenButton_Click** 메서드는 private일 수 있으며, XAML은 여전히 *OpenButton*에서 발생한 [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트에 연결할 수 있습니다.

C++/WinRT에서 **OpenButton_Click** 메서드는 [구현 형식](/windows/uwp/cpp-and-winrt-apis/author-apis)에서 public이어야 합니다.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

또는 등록 클래스를 구현 클래스의 friend로 만들고 **OpenButton_Click**을 private으로 만들 수 있습니다.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="making-a-class-available-to-the-binding-markup-extension"></a>{Binding} 태그 확장에서 사용할 수 있는 클래스 만들기

{Binding} 태그 확장을 사용하여 데이터를 데이터 형식에 바인딩하려면 [{Binding}을 사용하여 선언된 Binding 개체](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding)를 참조하세요.

## <a name="making-a-data-source-available-to-xaml-markup"></a>XAML 태그에서 데이터 원본을 사용할 수 있도록 만들기

C++/WinRT 버전 2.0.190530.8 이상에서 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)는 **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** 및 **IObservableVector\<IInspectable\>** 을 모두 지원하는 관찰 가능한 벡터를 만듭니다.

다음과 같이 **Midl 파일(.idl)** 을 작성할 수 있습니다([런타임 클래스를 Midl 파일(.idl)로 팩터링](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)도 참조).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

그리고 다음과 같이 구현합니다.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

자세한 내용은 [XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩](/windows/uwp/cpp-and-winrt-apis/binding-collection)을 참조하세요.

## <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>XAML 태그에서 데이터 원본을 사용할 수 있도록 만들기(C++/WinRT 2.0.190530.8 이전)

XAML 데이터 바인딩을 수행하려면 항목 원본에서 **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** 뿐만 아니라 다음 인터페이스 조합 중 하나도 구현해야 합니다.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** 및 **INotifyCollectionChanged**
- **IBindableVector** 및 **IBindableObservableVector**
- **IBindableVector** 자체(변경에 응답하지 않음)
- **IVector\<IInspectable\>**
- **IBindableIterable**(요소를 반복하여 프라이빗 컬렉션에 저장)

**IVector\<T\>** 와 같은 제네릭 인터페이스는 런타임에 검색할 수 없습니다. 각 **IVector\<T\>** 에는 **T**의 함수인 다른 IID(인터페이스 식별자)가 있습니다. 모든 개발자는 임의로 **T** 집합을 확장할 수 있으므로 XAML 바인딩 코드는 쿼리할 전체 집합을 알 수 없습니다. **IEnumerable\<T\>** 를 구현하는 모든 CLR 개체에서 **IEnumerable**을 자동으로 구현하므로 이 제한은 C#에서 문제가 되지 않습니다. ABI 수준에서는 **IObservableVector\<T\>** 를 구현하는 모든 개체에서 **IObservableVector\<IInspectable\>** 을 자동으로 구현한다는 것을 의미합니다.

C++/WinRT는 이러한 보장을 제공하지 않습니다. C++/WinRT 런타임 클래스에서 **IObservableVector\<T\>** 를 구현하는 경우 **IObservableVector\<IInspectable\>** 의 구현도 어떻게든 제공된다고 가정할 수 없습니다.

따라서 이전 예제를 조사해야 하는 방법은 다음과 같습니다.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

그리고 구현은 다음과 같습니다.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

*m_bookSkus*의 개체에 액세스해야 하는 경우 해당 개체를 **Bookstore::BookSku**로 다시 QI해야 합니다.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

## <a name="tostring"></a>ToString()

C# 형식은 [Object.ToString](/dotnet/api/system.object.tostring) 메서드를 제공합니다.

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/WinRT는 이 기능을 직접 제공하지 않지만 대체 기능으로 전환할 수 있습니다.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

또한 C++/WinRT는 제한된 수의 형식에 대해 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)도 지원합니다. 문자열화하려는 추가 형식에 대한 오버로드를 추가해야 합니다.

| Language | 정수 문자열화 | 열거형 문자열화 |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

열거형을 문자열화하는 경우 **winrt::to_hstring**의 구현을 제공해야 합니다.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

이러한 문자열화는 데이터 바인딩에서 암시적으로 사용되는 경우가 많습니다.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

이러한 바인딩은 bound 속성의 **winrt::to_hstring**을 수행합니다. 두 번째 예제(**StatusEnum**)의 경우 **winrt::to_hstring**에 대한 사용자 고유의 오버로드를 제공해야 합니다. 그렇지 않으면 컴파일러 오류가 발생합니다.

## <a name="string-building"></a>문자열 작성

문자열 작성의 경우 C#에는 기본 제공 [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 형식이 있습니다.

| | C# | C++/WinRT |
|-|-|-|
| Builder 클래스 | `StringBuilder builder;` | `std::wstringstream stream;` |
| 문자열 추가, null 유지 | `builder.Append(s);` | `stream << std::wstring_view{ s };` |
| 결과 추출 | `s = builder.ToString();` | `ws = stream.str();` |

## <a name="boxing-and-unboxing"></a>boxing 및 unboxing

C#은 자동으로 스칼라를 개체에 boxing합니다. C++/WinRT에서는 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 함수를 명시적으로 호출해야 합니다. 두 언어에서 모두 명시적으로 unboxing해야 합니다. [C++/WinRT를 사용하여 boxing 및 unboxing](/windows/uwp/cpp-and-winrt-apis/boxing)을 참조하세요.

다음 표에서는 이러한 정의를 사용합니다.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| 작업 | C# | C++/WinRT|
|-|-|-|
| boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX 및 C#에서는 값 형식에 대한 null 포인터를 unboxing하려고 하면 예외가 발생합니다. C++/WinRT는 이를 프로그래밍 오류로 간주하여 작동이 중단됩니다. C++/WinRT에서 개체가 예상한 형식이 아닌 경우를 처리하려면 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 함수를 사용합니다.

| 시나리오 | C# | C++/WinRT|
|-|-|-|
| 알려진 정수 unboxing |`i = (int)o;` | `i = unbox_value<int>(o);` |
| o가 null인 경우 | `System.NullReferenceException` | 작동 중단 |
| o가 boxing된 정수가 아닌 경우 | `System.InvalidCastException` | 작동 중단 |
| 정수 unboxing, null인 경우 대체 사용, 다른 항목이 있는 경우 작동 중단 | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 가능한 경우 정수 unboxing, 다른 항목이 있는 경우 대체 사용 | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>문자열 boxing 및 unboxing

문자열은 어떤 방식에서는 값 형식이고, 다른 방식에서는 참조 형식입니다. C# 및 C++/WinRT는 문자열을 다르게 처리합니다.

[**HSTRING**](/windows/win32/winrt/hstring) ABI 형식은 참조 횟수가 계산되는 문자열에 대한 포인터입니다. 그러나 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)에서 파생되지 않으므로 기술적으로는 *개체*가 아닙니다. 또한 null **HSTRING**은 빈 문자열을 나타냅니다. **IInspectable**에서 파생되지 않은 것에 대한 boxing은 [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_) 안에 래핑하여 수행되며, Windows 런타임에서 표준 구현을 [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) 개체 형식으로 제공합니다(사용자 지정 형식은 [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)으로 보고됨).

C#은 Windows 런타임 문자열을 참조 형식으로 나타내는 반면, C++/WinRT는 문자열을 값 형식으로 프로젝션합니다. 즉, boxing된 null 문자열은 해당 문자열을 가져온 방식에 따라 다르게 표현될 수 있습니다.

| 동작 | C# | C++/WinRT|
|-|-|-|
| 선언 | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| 문자열 형식 범주 | 참조 형식 | 값 유형 |
| null **HSTRING**에서 프로젝션하는 형식 | `""` | `hstring{}` |
| null 및 `""`가 동일한가요? | 아니요 | 예 |
| null의 유효성 | `s = null;`<br>`s.Length`에서 NullReferenceException 발생 | `s = hstring{};`<br>`s.size() == 0`(유효) |
| 개체에 null 문자열을 할당하는 경우 | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| 개체에 `""`을(를) 할당하는 경우 | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

기본 boxing 및 unboxing.

| 작업 | C# | C++/WinRT|
|-|-|-|
| 문자열 boxing | `o = s;`<br>빈 문자열은 null이 아닌 개체가 됩니다. | `o = box_value(s);`<br>빈 문자열은 null이 아닌 개체가 됩니다. |
| 알려진 문자열 unboxing | `s = (string)o;`<br>Null 개체는 null 문자열이 됩니다.<br>문자열이 아닌 경우 InvalidCastException이 발생합니다. | `s = unbox_value<hstring>(o);`<br>Null 개체가 충돌합니다.<br>문자열이 아닌 경우 충돌이 발생합니다. |
| 가능한 문자열 Unbox | `s = o as string;`<br>Null 개체 또는 비문자열은 null 문자열이 됩니다.<br><br>또는<br><br>`s = o as string ?? fallback;`<br>Null 또는 비문자열이 대체됩니다.<br>빈 문자열이 유지됩니다. | `s = unbox_value_or<hstring>(o, fallback);`<br>Null 또는 비문자열이 대체됩니다.<br>빈 문자열이 유지됩니다. |

## <a name="derived-classes"></a>파생 클래스

런타임 클래스에서 파생하려면 기본 클래스를 *구성*할 수 있어야 합니다. C#에서는 클래스를 구성할 수 있게 하는 특별한 단계를 수행할 필요가 없지만, C++/WinRT는 이를 수행합니다. [unsealed 키워드](/uwp/midl-3/intro#base-classes)를 사용하여 클래스를 기본 클래스로 사용할 수 있도록 지정합니다.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

구현 헤더 클래스에서 먼저 기본 클래스 헤더 파일을 포함해야 파생 클래스에 대해 자동 생성된 헤더를 포함할 수 있습니다. 그렇지 않으면 "이 형식을 식으로 잘못 사용했습니다"와 같은 오류가 발생합니다.

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="consuming-objects-from-xaml-markup"></a>XAML 태그에서 개체 사용

C# 프로젝트에서는 XAML 태그에서 프라이빗 멤버 및 명명된 요소를 사용할 수 있습니다. 그러나 C++/WinRT에서 XAML [ **{x:Bind} 태그 확장**](/windows/uwp/xaml-platform/x-bind-markup-extension)을 통해 사용되는 모든 엔터티는 IDL에 공개적으로 노출되어야 합니다.

또한 부울에 바인딩하면 C#에서 `true` 또는 `false`가 표시되지만, C++/WinRT에서는 **Windows.Foundation.IReference`1\<Boolean\>** 이 표시됩니다.

자세한 내용과 코드 예제는 [태그에서 개체 사용](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup)을 참조하세요.

## <a name="important-apis"></a>중요 API
* [winrt::single_threaded_observable_vector 함수 템플릿](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C# 자습서](/visualstudio/get-started/csharp)
* [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [데이터 바인딩 심층 분석](/windows/uwp/data-binding/data-binding-in-depth)
* [XAML 항목 컨트롤(C++/WinRT 컬렉션에 바인딩)](/windows/uwp/cpp-and-winrt-apis/binding-collection)