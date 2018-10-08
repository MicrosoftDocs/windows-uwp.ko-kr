---
author: stevewhims
description: 이번 항목에서는 C++/CX 개체와 C++/WinRT 개체를 서로 변환하는 데 사용할 수 있는 두 가지 도우미 함수에 대해서 설명합니다.
title: C++/WinRT와 C++/CX 사이의 상호 운용성
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, 상호 운용성, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: b60b0d7c201f172261de1546fc250e40b8cd670f
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4420169"
---
# <a name="interop-between-cwinrt-and-ccx"></a>C++/WinRT와 C++/CX 사이의 상호 운용성
이 항목에서는 간에 변환 하는 사용할 수 있는 두 가지 도우미 함수 [C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 및 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 개체입니다. 두 언어 프로젝션을 사용 하는 코드 사이의 상호 운용성 사용 하거나로 점차 마이그레이션하 코드에서 C + 기능을 사용할 수 있습니다 + /CX C + + WinRT (참조 [이동 C + +에서 C + + CX](move-to-winrt-from-cx.md)).

## <a name="fromcx-and-tocx-functions"></a>from_cx 함수와 to_cx 함수
아래 도우미 함수는 C++/CX 개체를 상응하는 C++/WinRT 개체로 변환합니다. 이 함수는 C++/CX 개체를 기본 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) 인터페이스 포인터로 캐스팅합니다. 그런 다음 해당 포인터에 대한 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)를 호출하여 C++/WinRT 개체의 기본 인터페이스에 대해 쿼리를 실행합니다. **QueryInterface**는 C++/CX safe_cast 확장에 상응하는 Windows 런타임 응용 프로그램 이진 인터페이스(ABI)입니다. 그러면 [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 함수가 C++/WinRT 개체의 기본 **IUnknown** 인터페이스 포인터 주소를 다른 값으로 설정할 수 있도록 가져옵니다.

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

아래 도우미 함수는 C++/WinRT 개체를 상응하는 C++/CX 개체로 변환합니다. [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 함수는 포인터를 C++/WinRT 개체의 기본 **IUnknown** 인터페이스로 가져옵니다. 그런 다음 포인터를 C++/CX 개체로 캐스팅한 후 C++/CX safe_cast 확장을 사용해 요청된 C++/CX 형식에 대해 쿼리를 실행합니다.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="code-example"></a>코드 예제
다음은 두 도우미 함수가 어떻게 사용되는지 나타낸 코드 예제(C++/CX **비어 있는 앱** 프로젝트 템플릿 기반)입니다. 또한 C++/WinRT 프로젝션 및 C++/CX 프로젝션 간의 잠재적인 네임스페이스 충돌을 다루기 위해 다른 격리 영역에 대한 네임스페이스 별칭을 사용하는 방법을 보여 줍니다.

```cppwinrt
// MainPage.xaml.cpp

#include "pch.h"
#include "MainPage.xaml.h"
#include <winrt/Windows.Foundation.h>
#include <sstream>

using namespace InteropExample;

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows::Foundation;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

MainPage::MainPage()
{
    InitializeComponent();

    winrt::init_apartment(winrt::apartment_type::single_threaded);

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);
}
```

## <a name="important-apis"></a>중요 API
* [IUnknown 인터페이스](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [QueryInterface 함수](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [winrt::get_abi 함수](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 함수](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
