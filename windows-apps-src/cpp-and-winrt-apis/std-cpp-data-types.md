---
description: C++/WinRT를 사용하면 표준 C++ 데이터 형식을 사용하여 Windows 런타임 API를 호출할 수 있습니다.
title: 표준 C++ 데이터 형식 및 C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 데이터, 형식
ms.localizationpriority: medium
ms.openlocfilehash: a87ba48a0853058ba1259e079c586b97af551656
ms.sourcegitcommit: 8b4c1fdfef21925d372287901ab33441068e1a80
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844326"
---
# <a name="standard-c-data-types-and-cwinrt"></a>표준 C++ 데이터 형식 및 C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)에서는 일부 C++ 표준 라이브러리 데이터 형식이 포함된 표준 C++ 데이터 형식을 사용해 Windows 런타임 API를 호출할 수 있습니다. API에 표준 문자열을 전달할 수 있으며([C++/WinRT의 문자열 처리](strings.md) 참조), 의미 체계적으로 동일한 컬렉션을 예상하는 API에 이니셜라이저 목록 및 표준 컨테이너를 전달할 수 있습니다.

[매개 변수를 ABI 경계로 전달](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi)도 참조하세요.

## <a name="standard-initializer-lists"></a>표준 이니셜라이저 목록
이니셜라이저 목록(**std::initializer_list**)은 C++ 표준 라이브러리 구문을 말합니다. 이니셜라이저 목록은 일부 Windows 런타임 생성자와 메서드를 호출할 때 사용할 수 있습니다. 예를 들어 목록 하나로 [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes)를 호출할 수 있습니다.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

이 작업에는 두 가지 메서드가 사용됩니다. 첫 번째인 **DataWriter::WriteBytes** 메서드는 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 형식의 매개 변수를 갖습니다.

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**winrt::array_view**는 사용자 지정 C++/WinRT 형식으로서 인접하여 연속되는 값을 나타냅니다(연속되는 값은 C++/WinRT 기본 라이브러리인 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`에 정의되어 있음).

두 번째인 **winrt::array_view**에는 이니셜라이저 목록 생성자가 있습니다.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

대부분 경우 프로그래밍할 때 **winrt::array_view**의 인식 여부를 선택할 수 있습니다. 인식하지 ‘않도록’ 선택하면 C++ 표준 라이브러리에 해당하는 형식이 표시되더라도 어떤 코드도 변경할 필요 없습니다. 

이니셜라이저 목록은 컬렉션 매개 변수가 필요한 Windows 런타임 API에 전달할 수 있습니다. **StorageItemContentProperties::RetrievePropertiesAsync**를 예로 들어보겠습니다.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

위의 예제처럼 이니셜라이저 목록을 사용해 API를 호출할 수 있습니다.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

여기에서는 두 가지 요인이 작용합니다. 첫째, 호출 수신자가 이니셜라이저 목록에서 **std::vector**를 생성합니다(이 호출 수신자는 비동기적이므로 필수인 해당 개체를 소유할 수 있음). 둘째, C++/WinRT가 투명하게(그리고 복사본을 사용하지 않고) **std::vector**를 Windows 런타임 컬렉션 매개 변수로 바인딩합니다.

## <a name="standard-arrays-and-vectors"></a>표준 배열 및 벡터
[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view)에는 **std::vector** 및 **std::array**의 변환 생성자도 있습니다.

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

따라서 **std::vector**를 사용하여 **DataWriter::WriteBytes**를 대신 호출할 수 있습니다.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

또는 **std::array**를 사용하는 것도 가능합니다.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

C++/WinRT는 **std::vector**를 Windows 런타임 컬렉션 매개 변수로 바인딩합니다. 따라서 **std::vector&lt;winrt::hstring&gt;** 을 전달할 수 있으며, 이후 **winrt::hstring**의 Windows 런타임 컬렉션으로 적절하게 변환됩니다. 호출 수신자가 비동기적인 경우 기억해야 할 추가적인 세부 정보가 있습니다. 이 경우의 구현 세부 정보로 인해 rvalue를 제공해야 하므로 벡터의 복사 또는 이동을 제공해야 합니다. 아래 코드 예제에서는 벡터의 소유권을 비동기 호출 수신자가 허용하는 매개 변수 형식의 개체로 이동합니다. 이동한 후 `vecH`에 다시 액세스하지 않도록 주의하세요. rvalue에 대해 자세히 알아보려면 [값 범주 및 해당 참조](cpp-value-categories.md)를 참조하세요.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

하지만 Windows 런타임 컬렉션이 필요한 경우에는 **std::vector&lt;std::wstring&gt;** 을 전달할 수 없습니다. 이는 **std::wstring**의 Windows 런타임 컬렉션으로 적절하게 변환된 경우에는 C++ 언어가 컬렉션의 형식 매개 변수를 강제 변환하지 못하기 때문입니다. 결과적으로 다음 코드 예제는 컴파일되지 않으며 솔루션은 위에 표시된 것처럼 **std::vector&lt;winrt::hstring&gt;** 을 대신 전달하는 것입니다.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>원시 배열 및 포인터 범위
앞으로 동등한 형식이 C++ 표준 라이브러리에 존재할 수 있다는 점을 감안하면 원할 때나 필요할 때 **winrt::array_view**를 사용해 직접 작업할 수 있습니다.

**winrt::array_view**는 원시 배열에서, 그리고 **T&ast;** 범위(포인터부터 요소 형식까지)에서 변환 생성자를 갖습니다.

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view 함수와 연산자
**winrt::array_view**일 때는 다양한 생성자, 연산자, 함수 및 반복기가 구현됩니다. **winrt::array_view**는 범위이기 때문에 범위 기반 `for`와 함께, 혹은 **std::for_each**와 함께 사용할 수 있습니다.

더 많은 예제와 정보를 보려면 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) API 참조 항목을 참조하세요.

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** 및 표준 반복 구문
[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items)는 **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;** 로 C++/WinRT에 프로젝션된 [**IVector&lt;T&gt;** ](/uwp/api/windows.foundation.collections.ivector_t_) 형식의 컬렉션을 반환하는 Windows 런타임 API의 예입니다. 범위 기반 `for`와 같은 표준 반복 구문에서 이 형식을 사용할 수 있습니다.

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>비동기 Windows 런타임 API를 사용한 C++ 코루틴
비동기 Windows 런타임 API를 호출할 때 [PPL(병렬 패턴 라이브러리)](/cpp/parallel/concrt/parallel-patterns-library-ppl)를 계속 사용할 수 있습니다. 그러나 대부분의 경우 C++ 코루틴은 비동기 개체와 상호 작용하기 위한 효율적이고 더 쉽게 코딩된 관용구를 제공합니다. 자세한 내용과 코드 예제는 [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)을 참조하세요.

## <a name="important-apis"></a>중요 API
* [IVector&lt;T&gt; 인터페이스](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view 구조체 템플릿](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 문자열 처리](strings.md)
