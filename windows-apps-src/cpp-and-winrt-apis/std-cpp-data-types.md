---
author: stevewhims
description: C++/WinRT에서는 표준 C++ 데이터 형식을 사용하여 Windows 런타임 API를 호출할 수 있습니다.
title: 표준 C++ 데이터 형식 및 C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 데이터, 형식
ms.localizationpriority: medium
ms.openlocfilehash: 0b004b5dfbeaa1d123398db780582382857eec25
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4085426"
---
# <a name="standard-c-data-types-and-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>표준 C++ 데이터 형식 및 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
C++/WinRT에서는 일부 C++ 표준 라이브러리 데이터 형식이 포함된 표준 C++ 데이터 형식을 사용해 Windows 런타임 API를 호출할 수 있습니다. Api에 표준 문자열을 전달할 수 있습니다 (참조 [문자열 처리 C + + WinRT](strings.md)), 목록 및 표준 컨테이너 의미가 같은 컬렉션 것으로 예상 되는 Api에 이니셜라이저를 전달할 수 있습니다.

## <a name="standard-initializer-lists"></a>표준 이니셜라이저 목록
이니셜라이저 목록(**std::initializer_list**)은 C++ 표준 라이브러리 구문을 말합니다. 이니셜라이저 목록은 일부 Windows 런타임 생성자와 메서드를 호출할 때 사용할 수 있습니다. 예를 들어, 목록 하나로 [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes)를 호출할 수 있습니다.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to an array_view before being passed to WriteBytes.
}
```

이 작업에는 두 가지 메서드가 있습니다. 첫 번째인 **DataWriter::WriteBytes** 메서드는 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 형식의 매개 변수를 갖습니다.

```cppwinrt
void WriteBytes(array_view<uint8_t const> value) const
```

 여기에서 **array_view**는 사용자 지정 C++/WinRT 형식으로서 인접하여 연속되는 값을 나타냅니다(연속되는 값은 C++/WinRT 기본 라이브러리인 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`에 정의되어 있음).

두 번째인 **array_view**에는 이니셜라이저 목록 생성자가 있습니다.

```cppwinrt
template <typename T> array_view(std::initializer_list<T> value) noexcept
```

대부분 경우 프로그래밍할 때 **array_view**의 인식 여부를 선택할 수 있습니다. 인식하지 *않도록* 선택하면 C++ 표준 라이브러리에 해당하는 형식이 표시되더라도 어떤 코드도 변경할 필요 없습니다.

이니셜라이저 목록은 컬렉션 매개 변수가 필요한 Windows 런타임 API에게 전달할 수 있습니다. **StorageItemContentProperties::RetrievePropertiesAsync**를 예로 들어보겠습니다.

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

여기에서는 두 가지 요인이 작용합니다. 첫째, 호출 수신자가 이니셜라이저 목록에서 **std::vector**를 생성합니다(이 수신자는 비동기식이기 때문에 해당 개체를 반드시 가지고 있어야 합니다). 둘째, C++/WinRT가 투명하게(그리고 복사본을 사용하지 않고) **std::vector**를 Windows 런타임 컬렉션 매개 변수로 바인딩합니다.

## <a name="standard-arrays-and-vectors"></a>표준 배열 및 벡터
**array_view** 역시 **std::vector** 및 **std::array**에서 변환 생성자를 갖습니다.

```cppwinrt
template <typename C, size_type N> array_view(std::array<C, N>& value) noexcept
template <typename C> array_view(std::vector<C>& vectorValue) noexcept
```

따라서 **std::vector**를 사용하여 **DataWriter::WriteBytes**를 호출할 수 있습니다.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to an array_view before being passed to WriteBytes.
```

또는 **std::array**를 사용하는 것도 가능합니다.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to an array_view before being passed to WriteBytes.
```

C++/WinRT는 **std::vector**를 Windows 런타임 컬렉션 매개 변수로 바인딩합니다. 따라서 **std::vector&lt;winrt::hstring&gt;** 을 전달할 수 있으며, 이후 **winrt::hstring**의 Windows 런타임 컬렉션으로 적절하게 변환됩니다. 비동기 호출 수신자가 경우 염두에 추가 정도로. 해당 하는 구현 세부 정보로 인해 복사본 또는 벡터의 이동 제공 해야 하므로 rvalue를 제공 해야 합니다. 아래의 코드 예제에서 비동기 호출 수신자가 수락 매개 변수 형식 개체에 벡터 소유권 이동 (에 액세스 하지 않도록 주의 하 고 `vecH` 다시 이동한 후). Rvalue에 대해 자세히 알고 싶다면 [값 범주 및 참조를](cpp-value-categories.md)참조 하세요.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

하지만 Windows 런타임 컬렉션이 필요한 경우에는 **std::vector&lt;std::wstring&gt;** 을 전달할 수 없습니다. 이는 **std::wstring**의 Windows 런타임 컬렉션으로 적절하게 변환된 경우에는 C++ 언어가 컬렉션의 형식 매개 변수를 강제 변환하지 못하기 때문입니다. 결과적으로, 다음 코드 예제는 컴파일되지 않습니다 (하며 솔루션은 전달 하는 **std:: vector&lt;winrt:: hstring&gt; ** 위에 표시 된 대로 대신).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>원시 배열 및 포인터 범위
앞으로 동등한 형식이 C++ 표준 라이브러리에 존재할 수 있다는 점을 감안하면 원할 때나 필요할 때 **array_view**를 사용해 직접 작업할 수 있습니다.

**array_view** 범위와 원시 배열에서 변환 생성자가 **T&ast; ** (포인터부터 요소 형식).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view 함수와 연산자
**array_view**일 때는 다양한 생성자, 연산자, 함수 및 반복기가 구현됩니다. **array_view**는 범위이기 때문에 범위 기반 `for`와 함께, 혹은 **std::for_each**와 함께 사용할 수 있습니다.

더 많은 예제와 정보를 보려면 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) API 참조 항목을 참조하세요.

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt; ** 표준 반복 구문
[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) 형식의 컬렉션을 반환 하는 Windows 런타임 API의 예는 [**IVector&lt;T&gt; **](/uwp/api/windows.foundation.collections.ivector_t_) (프로젝션에 C + + /winrt **winrt::Windows::Foundation::Collections::IVector&lt;T&gt; ** ). 표준 반복 구문으로이 이와 같은 사용 범위 기반 `for`.

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>비동기 Windows 런타임 Api를 사용 하 여 c + + 코
비동기 Windows 런타임 Api를 호출할 때 [병렬 패턴 라이브러리 (PPL)를](/cpp/parallel/concrt/parallel-patterns-library-ppl) 사용 하 여 계속 수 있습니다. 그러나 대부분의 경우 c + + 코 비동기 개체와 상호 작용을 효율적이 고 더 쉽게 코딩 된 방법을 제공 합니다. 자세한 내용은 및 코드 예제를 참조 하세요 [동시성 및 비동기 작업을 사용 하 여 C + + WinRT](concurrency.md).

## <a name="important-apis"></a>중요 API
* [IVector&lt;T&gt; 인터페이스](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view 구조체 템플릿](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 문자열 처리](strings.md)
