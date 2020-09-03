---
description: C++/WinRT를 사용하여 프로그래밍할 때 오류를 처리하기 위한 전략에 대해 설명합니다.
title: C++/WinRT를 통한 오류 처리
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 오류, 처리, 예외
ms.localizationpriority: medium
ms.openlocfilehash: c721c70f19533053821139429b9bbd8bcd17f68a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170227"
---
# <a name="error-handling-with-cwinrt"></a>C++/WinRT를 통한 오류 처리

이 항목에서는 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)로 프로그래밍하는 경우 오류를 처리하기 위한 전략을 설명합니다. 일반적인 내용과 배경은 [오류 및 예외 처리(최신 C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)를 참조하세요.

## <a name="avoid-catching-and-throwing-exceptions"></a>예외 catch 및 throw 방지
[예외로부터 안전한 코드](/cpp/cpp/how-to-design-for-exception-safety)를 계속 작성하면서, 가능한 한 예외 catch 및 throw를 방지하는 것이 좋습니다. 예외 처리기가 없는 경우, Windows에서 문제가 있는 위치를 추적하는 데 도움이 되는 오류 보고서(크래시 미니덤프 포함)를 자동으로 생성합니다.

catch할 수 있는 예외를 throw하지 마세요. 또한 예상된 실패에 대해 예외를 사용하지 마세요. ‘예기치 않은 런타임 오류가 발생하는 경우에만’ 예외를 throw하고, 다른 모든 경우에는 실패 원인과 가까운 오류/결과 코드로 직접 처리합니다.&mdash; 그러면 예외가 throw되는 경우 원인이 코드의 버그이거나 시스템의 예외적인 오류 상태임을 알 수 있습니다.

Windows 레지스트리에 액세스하는 시나리오를 고려해 보세요. 앱이 레지스트리에서 값을 읽지 못할 경우 예상된 실패여야 하며, 정상적으로 처리해야 합니다. 예외를 throw하지 말고, 값을 읽지 못했다는 것과 읽지 못한 이유를 나타내는 `bool` 또는 `enum` 값을 반환합니다. 반면에 레지스트리에 값을 ‘쓰지’ 못할 경우 애플리케이션에서 쉽게 처리할 수 있는 문제보다 심각한 문제가 있음을 나타낼 가능성이 큽니다. 이러한 경우, 애플리케이션을 계속하지 않는 것이 좋으므로 오류 보고서를 생성하는 예외가 애플리케이션에 의한 손상을 방지하는 가장 빠른 방법입니다.

또 다른 예로, [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) 호출의 미리 보기 이미지를 검색하고 [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_)에 미리 보기를 전달하는 경우를 고려해 보세요. 호출 시퀀스에서 `nullptr`을 **SetSourceAsync**에 전달하는 경우(이미지 파일을 읽을 수 없음, 파일 확장명 때문에 이미지 데이터가 포함된 것 같지만 실제로는 이미지 데이터가 없음), 잘못된 포인터 예외가 throw됩니다. 코드에서 이러한 경우를 발견하면 예외로 catch 및 처리하는 대신 **GetThumbnailAsync**에서 반환되는 `nullptr`을 확인합니다.

대체로 예외 throw가 오류 코드 사용보다 속도가 느립니다. 심각한 오류가 발생할 때만 예외를 throw하면, 모두 정상적으로 작동할 경우 성능 저하를 감수하지 않아도 됩니다.

그러나 예외가 throw될 가능성이 없는 이벤트에서 적절한 소멸자가 호출되도록 하는 런타임 오버헤드로 인해 성능이 저하될 가능성이 더 큽니다. 이 확인 작업의 비용은 예외가 실제로 throw되었는지 여부에 관계없이 발생합니다. 따라서 컴파일러에서 잠재적으로 예외를 throw할 수 있는 함수를 예상할 수 있도록 해야 합니다. 컴파일러가 특정 함수에 예외가 없음을 입증할 수 있는 경우(`noexcept` 사양), 생성하는 코드를 최적화할 수 있습니다.

## <a name="catching-exceptions"></a>예외 catch
[Windows 런타임 ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 계층에서 발생하는 오류 조건은 HRESULT 값의 형식으로 반환됩니다. 하지만 코드에서 HRESULT를 처리할 필요는 없습니다. 사용 측면에서 API를 위해 생성된 C++/WinRT 프로젝션 코드는 ABI 계층에서 오류 HRESULT 코드를 검색하고, 이 코드를 catch하여 처리할 수 있는 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 예외로 변환합니다. HRESULTS를 ‘처리’하려는 경우 **winrt::hresult** 형식을 사용합니다.

예를 들어 애플리케이션이 해당 컬렉션을 반복하는 동안 사용자가 사진 라이브러리에서 이미지를 삭제하면 프로젝션에서 예외가 throw됩니다. 이 경우 해당 예외를 catch 및 처리해야 합니다. 이러한 경우를 보여 주는 코드 예제는 다음과 같습니다.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.code(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

`co_await`된 함수를 호출하는 경우 코루틴에서 이와 동일한 패턴을 사용합니다. HRESULT를 예외로 변환하는 또 다른 예로, 구성 요소 API에서 반환된 E_OUTOFMEMORY로 인해 **std::bad_alloc**가 throw되는 경우가 있습니다.

HRESULT 코드를 살펴볼 때 [**winrt::hresult_error::code**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorcode-function)를 사용하는 것이 좋습니다. 반면 [**winrt::hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 함수는 COM 오류 개체로 변환되어 COM 스레드 로컬 스토리지에 상태를 푸시합니다.

## <a name="throwing-exceptions"></a>예외 발생
지정된 함수 호출이 실패하면 애플리케이션을 복구할 수 없다고 결정하는 경우가 있습니다(더 이상 예상대로 작동할 것으로 신뢰할 수 없음). 아래 코드 예제는 [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) 값을 [**CreateEvent**](/windows/desktop/api/synchapi/nf-synchapi-createeventa)에서 반환된 HANDLE의 래퍼로 사용합니다. 그런 다음, `bool` 값을 만들어 핸들을 [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 함수 템플릿에 전달합니다. **winrt::check_bool**은 `bool`이나 `false`(오류 조건) 또는 `true`(성공 조건)로 변환할 수 있는 모든 값으로 작업합니다.

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

[**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool)에 전달한 값이 false이면 다음 작업 시퀀스가 발생합니다.

- **winrt::check_bool**이 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 함수를 호출합니다.
- **winrt::throw_last_error**가 [**GetLastError**](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror)를 호출하여 호출 스레드의 마지막 오류 코드 값을 검색한 다음, [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) 함수를 호출합니다.
- **winrt::throw_hresult**가 오류 코드를 나타내는 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 개체(또는 표준 개체)를 사용하여 예외를 throw합니다.

Windows API는 다양한 반환 값 형식을 사용하여 런타임 오류를 보고하기 때문에 **winrt::check_bool** 외에도 값을 확인하고 예외를 throw하기 위한 다른 몇 가지 유용한 도우미 함수가 있습니다.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). HRESULT 코드가 오류를 나타내는지 여부를 확인하고, 오류를 나타내는 경우 **winrt::throw_hresult**를 호출합니다.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). 코드가 오류를 나타내는지 여부를 확인하고, 오류를 나타내는 경우 **winrt::throw_hresult**를 호출합니다.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). 포인터가 null인지 여부를 확인하고, null인 경우 **winrt::throw_last_error**를 호출합니다.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). 코드가 오류를 나타내는지 여부를 확인하고, 오류를 나타내는 경우 **winrt::throw_hresult**를 호출합니다.

일반적인 반환 코드 형식에 대해 이러한 도우미 함수를 사용하거나, 모든 오류 조건에 응답하여 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 또는 [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)를 호출할 수 있습니다. 

## <a name="throwing-exceptions-when-authoring-an-api"></a>API를 작성할 때 예외 throw
모든 [Windows 런타임 ABI(애플리케이션 이진 인터페이스)](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 경계(또는 ABI 경계)는 *noexcept*여야 합니다. 즉, 예외가 여기서 발생하면 안 됩니다. API를 작성할 때 ABI 경계는 항상 C++ `noexcept` 키워드를 사용하여 표시해야 합니다. `noexcept`에는 C++의 특정 동작이 있습니다. C++ 예외가 `noexcept` 경계에 도달하면 프로세스에서 **std::terminate**를 사용하여 페일 패스트합니다. 처리되지 않은 예외는 거의 항상 프로세스에서 알 수 없는 상태를 나타내므로 이 동작은 일반적으로 바람직합니다.

예외는 ABI 경계를 넘지 않아야 하므로 구현에서 발생하는 오류 조건은 ABI 계층에서 HRESULT 오류 코드 형식으로 반환됩니다. C++/WinRT를 사용하여 API를 작성하는 경우 구현에서 throw한 모든 예외를 HRESULT로 변환할 수 있도록 코드가 생성됩니다. [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 함수는 생성된 코드에서 다음과 같은 패턴으로 사용됩니다.

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)는 **std::exception**에서 파생된 예외와 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 및 그 파생 형식을 처리합니다. API 소비자가 다양한 오류 정보를 받을 수 있도록 구현에서 **winrt::hresult_error** 또는 파생 형식을 사용하는 것이 좋습니다. **std::exception**(E_FAIL에 매핑됨)은 표준 템플릿 라이브러리를 사용하는 중 예외가 발생하는 경우에 지원됩니다.

### <a name="debuggability-with-noexcept"></a>noexcept를 사용하는 디버그 효율성
위에서 언급한 대로 `noexcept` 경계에 도달하는 C++ 예외는 **std::terminate**를 사용하여 페일 패스트합니다. **std::terminate**는 대부분 또는 모든 오류, 특히 코루틴이 관련된 경우 throw되는 예외 컨텍스트를 손실하는 경우가 많으므로 디버깅에 적합하지 않습니다.

따라서 이 섹션에서는 `noexcept`를 사용하여 적절히 주석 처리한 ABI 메서드에서 `co_await`를 사용하여 비동기 C++/WinRT 프로젝션 코드를 호출하는 경우를 다룹니다. **winrt::fire_and_forget** 내에서 C++/WinRT 프로젝션 코드에 대한 호출을 래핑하는 것이 좋습니다. 이렇게 하면 처리되지 않은 예외가 stowed 예외로 적절히 기록될 수 있는 적절한 위치가 제공되므로 디버그 효율성이 크게 향상됩니다.

```cppwinrt
HRESULT MyWinRTObject::MyABI_Method() noexcept
{
    winrt::com_ptr<Foo> foo{ get_a_foo() };

    [/*no captures*/](winrt::com_ptr<Foo> foo) -> winrt::fire_and_forget
    {
        co_await winrt::resume_background();

        foo->ABICall();

        AnotherMethodWithLotsOfProjectionCalls();
    }(foo);

    return S_OK;
}
```

**winrt::fire_and_forget**에 **winrt::terminate**를 호출하는 기본 제공 `unhandled_exception` 메서드 도우미가 있습니다. 이 도우미는 **RoFailFastWithErrorContext**를 호출합니다. 이렇게 하면 모든 컨텍스트(stowed 예외, 오류 코드, 오류 메시지, 스택 역추적 등)가 라이브 디버깅 또는 사후 덤프를 위해 유지됩니다. 편의상 fire-and-forget 부분을 **winrt::fire_and_forget**을 반환하는 별도의 함수로 팩터링한 다음, 이를 호출할 수 있습니다.

### <a name="synchronous-code"></a>동기 코드
경우에 따라 ABI 메서드(`noexcept`를 사용하여 적절히 주석 처리했음)는 동기 코드만 호출합니다. 즉 비동기 Windows 런타임 메서드를 호출하거나 포그라운드 스레드와 백그라운드 스레드 간에 전환하기 위해 `co_await`를 사용하지 않습니다. 이 경우 fire_and_forget 기술은 계속 작동하지만 효율적이지 않습니다. 대신 다음과 같은 작업을 수행할 수 있습니다.

```cppwinrt
HRESULT abi() noexcept try
{
    // ABI code goes here.
} catch (...) { winrt::terminate(); }
```

### <a name="fail-fast"></a>페일 패스트
이전 섹션의 코드는 여전히 페일 패스트합니다. 이 코드는 작성된 대로 어떤 예외도 처리하지 않습니다. 처리되지 않은 예외가 발생하면 프로그램이 종료됩니다.

그러나 이 형식은 디버그 효율성을 보장하므로 우수합니다. 드문 경우이지만 `try/catch`를 수행하고 특정 예외를 처리하려고 할 수 있습니다. 그러나 이 항목에서 설명한 대로 예외를 예상하는 조건에 대한 흐름 제어 메커니즘으로 사용하지 않는 것이 좋습니다.

처리되지 않은 예외에서 비보호 `noexcept` 컨텍스트를 이스케이프하지 않는 것이 좋습니다. 이 조건에서는 C++ 런타임에서 프로세스를 **std::terminate**하므로 C++/WinRT에서 주의 깊게 기록한 stowed 예외 정보가 손실됩니다.

## <a name="assertions"></a>어설션
애플리케이션 내부 가정의 경우 어설션이 있습니다. 가능한 경우 컴파일 시간 유효성 검사에는 **static_assert**를 사용하는 것이 좋습니다. 런타임 조건의 경우 `WINRT_ASSERT`에 부울 식을 사용합니다. `WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT는 릴리스 빌드에서 컴파일됩니다. 디버그 빌드에서는 디버거를 통해 어설션이 있는 코드 줄에서 애플리케이션을 중지합니다.

소멸자에 예외를 사용하면 안 됩니다. 따라서 최소한 디버그 빌드에서는 WINRT_VERIFY(부울 식 사용) 및 WINRT_VERIFY_(예상 결과 및 부울 식 사용)를 통해 소멸자에서 함수를 호출한 결과를 어설션할 수 있습니다.

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>중요 API
* [winrt::check_bool 함수 템플릿](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [winrt::check_hresult 함수](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::check_nt 함수 템플릿](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [winrt::check_pointer 함수 템플릿](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [winrt::check_win32 함수 템플릿](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [winrt::handle 구조체](/uwp/cpp-ref-for-winrt/handle)
* [winrt::hresult_error 구조체](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::throw_hresult 함수](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [winrt::throw_last_error 함수](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [winrt::to_hresult 함수](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>관련 항목
* [오류 및 예외 처리(최신 C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [방법: 예외 안전을 위한 디자인](/cpp/cpp/how-to-design-for-exception-safety)