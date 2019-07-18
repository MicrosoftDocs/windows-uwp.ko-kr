---
description: ABI(애플리케이션 이진 인터페이스)와 C++/WinRT 개체 간에 변환하는 방법을 보여 줍니다.
title: C++/WinRT와 ABI 사이의 Interop
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, 상호 운용성, ABI
ms.localizationpriority: medium
ms.openlocfilehash: d1def649772f94a03d5a1f352dcec1d32c7b0868
ms.sourcegitcommit: 5d71c97b6129a4267fd8334ba2bfe9ac736394cd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800573"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>C++/WinRT와 ABI 사이의 Interop

이번 항목에서는 SDK ABI(Application Binary Interface)와 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 개체를 서로 변환하는 방법에 대해서 설명합니다. 여기에서 설명하는 방법은 Windows 런타임을 통한 두 가지 프로그래밍 방법을 사용하는 코드 사이의 Interop에 사용하거나, 코드를 ABI에서 C++/WinRT로 점차 마이그레이션하는 데 사용할 수도 있습니다.

일반적으로 C++/WinRT는 ABI 형식을 **void\*** 로 표시하므로 플랫폼 헤더 파일을 포함하지 않아도 됩니다.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>Windows 런타임 ABI란 무엇이며, ABI 형식이란 무엇인가요?
Windows 런타임 클래스(런타임 클래스)는 추상화입니다. 이 추상화는 여러 프로그래밍 언어가 개체와 상호 작용할 수 있게 하는 이진 인터페이스(Application Binary Interface, ABI)를 정의합니다. 프로그래밍 언어에 관계없이 Windows 런타임 개체와의 클라이언트 코드 상호 작용은 개체의 ABI에 대한 호출로 번역되는 클라이언트 언어 구문으로 최저 수준에서 수행됩니다.

“%WindowsSdkDir%Include\10.0.17134.0\winrt” 폴더(필요한 경우 SDK 버전 번호를 사용자 요건에 맞게 조정)에 있는 Windows SDK 헤더가 Windows 런타임 ABI 헤더 파일입니다. 이 파일은 MIDL 컴파일러에서 생성되었습니다. 다음은 이 헤더 중 하나를 추가하는 예제입니다.

```cpp
#include <windows.foundation.h>
```

다음은 특정 SDK 헤더에서 찾을 수 있는 ABI 형식 중 하나를 간단히 표현한 예제입니다. **ABI** 네임스페이스, **Windows::Foundation**, 그리고 모든 다른 Windows 네임스페이스는 **ABI** 네임스페이스 내 SDK 헤더에 의해 선언됩니다.

```cpp
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

**IUriRuntimeClass**는 COM 인터페이스입니다. 하지만 **IUriRuntimeClass**가 **IInspectable**을 기반으로 한다는 점에서 Windows 런타임 인터페이스에 더욱 가깝습니다. 예외 발생보다는 오히려 **HRESULT** 반환 형식에 주의하세요. 또한 **HSTRING** 핸들 같은 아티팩트의 사용도 주의할 필요가 있습니다(작업을 마치면 핸들을 다시 `nullptr`로 설정하는 것이 좋음). 이렇게 하면 Windows 런타임이 애플리케이션 이진 수준, 즉 COM 프로그래밍 수준에서 어떻게 보이는지 알 수 있습니다.

Windows 런타임은 COM(구성 요소 개체 모델) API를 기반으로 합니다. 따라서 Windows 런타임에는 COM API를 통하거나, ‘언어 프로젝션’을 통해 액세스할 수 있습니다.  프로젝션은 COM 세부 정보를 숨기며, 지정된 언어에 더욱 자연스러운 프로그래밍 환경을 제공합니다.

예를 들어, “%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt” 폴더(다시 말하지만 필요한 경우 사용자 요건에 맞게 SDK 버전 번호를 조정) 안을 보면 C++/WinRT 언어 프로젝션 헤더를 찾을 수 있습니다. Windows 네임스페이스마다 ABI 헤더가 하나씩 있는 것처럼 각 Windows 네임스페이스에도 헤더가 있습니다. 다음은 C++/WinRT 헤더 중 하나를 추가하는 예제입니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

다음은 위의 헤더에서 시작하여 방금 본 ABI 형식에 상응하는 C++/WinRT를 간략하게 표현한 예제입니다.

```cppwinrt
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

여기에서 사용하는 인터페이스는 최신 표준 C++입니다. 이 인터페이스는 **HRESULT**를 사용하지 않습니다(필요한 경우 C++/WinRT에서 예외 발생). 접근자 함수는 범위 끝에서 정리되는 단순 문자열 개체를 반환합니다.

이 항목은 ABI(Application Binary Interface) 계층에서 사용할 수 있는 코드와 interop하거나 코드를 이식하려는 경우를 다룹니다.

## <a name="converting-to-and-from-abi-types-in-code"></a>코드의 ABI 형식 변환
안전성과 단순성, 그리고 양방향 변환 시 편의성을 고려하여 간단하게 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) 및 [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)를 사용할 수 있습니다. 다음은 C++/WinRT 프로젝션 및 ABI 간의 잠재적인 네임스페이스 충돌을 다루기 위해 다른 격리 영역에 대한 네임스페이스 별칭을 사용하는 방법을 보여 주는 코드 예제입니다(**콘솔 앱** 프로젝트 템플릿 기반).

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

**as** 함수의 구현이 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))를 호출합니다. [**AddRef**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)만 호출하는 하위 수준의 변환을 원한다면 도우미 함수로 [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi)와 [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi)를 사용할 수 있습니다. 다음 코드 예제에서는 이 하위 수준 변환을 위의 코드 예제에 추가합니다.

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uri, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uri, ptr.get());
    ptr = nullptr;
}
```

다음은 비슷한 하위 수준의 변환 예제이지만 원시 포인터를 ABI 인터페이스 형식(Windows SDK 헤더에서 정의한 형식)에 사용한다는 점에서 다릅니다.

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

주소만 복사하는 최하위 수준의 변환일 때는 도우미 함수로 [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) 및 [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi)를 사용할 수 있습니다.

`WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uri)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>convert_from_abi 함수
이 도우미 함수는 오버헤드를 최소화하면서 원시 ABI 인터페이스 포인터를 상응하는 C++/WinRT 개체로 변환합니다.

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

이 함수는 단순하게 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))를 호출하여 요청된 C++/WinRT 형식의 기본 인터페이스에 대해 쿼리를 실행합니다.

앞에서 본 것처럼 C++/WinRT 개체를 상응하는 ABI 인터페이스 포인터로 변환할 때는 도우미 함수가 필요하지 않습니다. 단순히 [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)(또는 [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) 멤버 함수를 사용하여 요청된 인터페이스에 대해 쿼리를 실행하면 됩니다. **as** 및 **try_as** 함수는 요청된 ABI 형식을 래핑하는 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 개체를 반환합니다.

## <a name="code-example-using-convertfromabi"></a>convert_from_abi를 사용하는 코드 예제
다음은 이 도우미 함수가 실제로 사용되는 코드 예제입니다.

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr };

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="interoperating-with-abi-com-interface-pointers"></a>ABI COM 인터페이스 포인터와 상호 운용

아래 도우미 함수 템플릿은 지정된 형식의 ABI COM 인터페이스 포인터를 해당 C++/WinRT 프로젝션된 스마트 포인터 형식으로 복사하는 방법을 보여 줍니다.

```cppwinrt
template<typename To, typename From>
To to_winrt(From* ptr)
{
    To result{ nullptr };
    winrt::check_hresult(ptr->QueryInterface(winrt::guid_of<To>(), winrt::put_abi(result)));
    return result;
}
...
ID2D1Factory1* com_ptr{ ... };
auto cppwinrt_ptr {to_winrt<winrt::com_ptr<ID2D1Factory1>>(com_ptr)};
```

다음 도우미 함수 템플릿은 [Windows 구현 라이브러리(WIL)](https://github.com/Microsoft/wil)의 스마트 포인터 형식에서 복사한다는 점을 제외하면 동일합니다.

```cppwinrt
template<typename To, typename From, typename ErrorPolicy>
To to_winrt(wil::com_ptr_t<From, ErrorPolicy> const& ptr)
{
    To result{ nullptr };
    if constexpr (std::is_same_v<typename ErrorPolicy::result, void>)
    {
        ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result));
    }
    else
    {
        winrt::check_result(ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result)));
    }
    return result;
}
```

[C++/WinRT를 통한 COM 구성 요소 사용](/windows/uwp/cpp-and-winrt-apis/consume-com)을 참조하세요.

### <a name="unsafe-interop-with-abi-com-interface-pointers"></a>ABI COM 인터페이스 포인터와의 안전하지 않은 상호 운용

다음 표에서는 지정된 형식의 ABI COM 인터페이스 포인터와 해당 C++/WinRT 프로젝션된 스마트 포인터 형식 사이의 안전하지 않은 변환을 보여 줍니다(다른 작업도 포함). 표에 나온 코드의 경우 다음 선언을 가정합니다.

```cppwinrt
winrt::Sample s;
ISample* p;

void GetSample(_Out_ ISample** pp);
```

또한 **ISample**은 **Sample**에 대한 기본 인터페이스라고 가정합니다.

이 코드를 사용하여 컴파일 시간에 해당 항목을 어설션할 수 있습니다.

```cppwinrt
static_assert(std::is_same_v<winrt::default_interface<winrt::Sample>, winrt::ISample>);
```

| 작업 | 작업 방법 | 참고 |
|-|-|-|
| **winrt::Sample**에서 **ISample\*** 추출 | `p = reinterpret_cast<ISample*>(get_abi(s));` | *s*는 여전히 개체를 소유합니다. |
| **winrt::Sample**에서 **ISample\*** 분리 | `p = reinterpret_cast<ISample*>(detach_abi(s));` | *s*는 더 이상 개체를 소유하지 않습니다. |
| **ISample\*** 을 새로운 **winrt::Sample**로 전송 | `winrt::Sample s{ p, winrt::take_ownership_from_abi };` | *s*가 개체의 소유권을 갖습니다. |
| **ISample\*** 을 **winrt::Sample**로 설정 | `*put_abi(s) = p;` | *s*가 개체의 소유권을 갖습니다. *s*가 이전에 소유한 모든 개체가 손실됩니다(디버그 시 어설션됨). |
| **ISample\*** 을 **winrt::Sample**에서 수신 | `GetSample(reinterpret_cast<ISample**>(put_abi(s)));` | *s*가 개체의 소유권을 갖습니다. *s*가 이전에 소유한 모든 개체가 손실됩니다(디버그 시 어설션됨). |
| **ISample\*** 을 **winrt::Sample**에서 교체 | `attach_abi(s, p);` | *s*가 개체의 소유권을 갖습니다. *s*가 이전에 소유한 개체가 해제됩니다. |
| **ISample\*** 을 **winrt::Sample**로 복사 | `copy_from_abi(s, p);` | *s*가 개체에 대한 새 참조를 만듭니다. *s*가 이전에 소유한 개체가 해제됩니다. |
| **winrt::Sample**을 **ISample\*** 로 복사 | `copy_to_abi(s, reinterpret_cast<void*&>(p));` | *p*가 개체의 복사본을 수신합니다. *p*가 이전에 소유한 모든 개체가 해제됩니다. |

## <a name="interoperating-with-the-abis-guid-struct"></a>ABI의 GUID 구조체와 상호 운용

[**GUID**](/previous-versions/aa373931(v%3Dvs.80))는 **winrt::guid**로 프로젝션됩니다. 구현하는 API의 경우 GUID 매개 변수에 **winrt::guid**를 사용해야 합니다. 그렇지 않은 경우 C++/WinRT 헤더를 포함하기 전에 `unknwn.h`(<windows.h> 및 다른 많은 헤더 파일에서 암시적으로 포함)를 포함하면 **winrt::guid** 및 **GUID** 사이에 자동 변환이 수행됩니다.

그렇게 하지 않으면 둘 사이에서 `reinterpret_cast`를 하드코딩할 수 있습니다. 아래의 표에서는 다음 선언을 가정합니다.

```cppwinrt
winrt::guid winrtguid;
GUID abiguid;
```

| 변환 | `#include <unknwn.h>` 사용 | `#include <unknwn.h>` 사용 안 함 |
|-|-|-|
| **winrt::guid**에서 **GUID**로 | `abiguid = winrtguid;` | `abiguid = reinterpret_cast<GUID&>(winrtguid);` |
| **GUID**에서 **winrt::guid**로 | `winrtguid = abiguid;` | `winrtguid = reinterpret_cast<winrt::guid&>(abiguid);` |

## <a name="interoperating-with-the-abis-hstring"></a>ABI의 HSTRING과 상호 운용

다음 표에서는 **winrt::hstring**과 [**HSTRING**](/windows/win32/winrt/hstring) 사이의 변환 및 기타 작업을 보여 줍니다. 표에 나온 코드의 경우 다음 선언을 가정합니다.

```cppwinrt
winrt::hstring s;
HSTRING h;

void GetString(_Out_ HSTRING* value);
```

| 작업 | 작업 방법 | 참고 |
|-|-|-|
| **hstring**에서 **HSTRING** 추출 | `h = static_cast<HSTRING>(get_abi(s));` | *s*는 여전히 문자열을 소유합니다. |
| **hstring**에서 **HSTRING** 분리 | `h = reinterpret_cast<HSTRING>(detach_abi(s));` | *s*는 더 이상 문자열을 소유하지 않습니다. |
| **HSTRING**을 **hstring**으로 설정 | `*put_abi(s) = h;` | *s*가 문자열의 소유권을 갖습니다. *s*가 이전에 소유한 모든 문자열이 손실됩니다(디버그 시 어설션됨). |
| **HSTRING**을 **hstring**으로 수신 | `GetString(reinterpret_cast<HSTRING*>(put_abi(s)));` | *s*가 문자열의 소유권을 갖습니다. *s*가 이전에 소유한 모든 문자열이 손실됩니다(디버그 시 어설션됨). |
| **HSTRING**을 **hstring**에서 교체 | `attach_abi(s, h);` | *s*가 문자열의 소유권을 갖습니다. *s*가 이전에 소유한 문자열이 해제됩니다. |
| **HSTRING**을 **hstring**에 복사 | `copy_from_abi(s, h);` | *s*가 문자열의 개인 복사본을 만듭니다. *s*가 이전에 소유한 문자열이 해제됩니다. |
| **hstring**을 **HSTRING**에 복사 | `copy_to_abi(s, reinterpret_cast<void*&>(h));` | *h*가 문자열의 복사본을 수신합니다. *h*가 이전에 소유한 모든 문자열이 손실됩니다. |

## <a name="important-apis"></a>중요 API
* [AddRef 함수](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [QueryInterface 함수](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::attach_abi 함수](/uwp/cpp-ref-for-winrt/attach-abi)
* [winrt::com_ptr 구조체 템플릿](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::copy_from_abi 함수](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [winrt::copy_to_abi 함수](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [winrt::detach_abi 함수](/uwp/cpp-ref-for-winrt/detach-abi)
* [winrt::get_abi 함수](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::Windows::Foundation::IUnknown::as 멤버 함수](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 멤버 함수](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
