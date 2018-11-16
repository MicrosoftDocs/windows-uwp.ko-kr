---
author: stevewhims
description: 이 항목은 C++/WinRT로 프로그래밍하는 경우 오류를 처리하기 위한 전략을 소개합니다.
title: C++/WinRT를 통한 오류 처리
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 오류, 처리, 예외
ms.localizationpriority: medium
ms.openlocfilehash: 15432202e61322191e27e89920f7791878177c8b
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6978518"
---
# <a name="error-handling-with-cwinrt"></a>C++/WinRT를 통한 오류 처리

이 항목에서는 설명 프로그래밍 하는 경우 오류를 처리 하기 위한 전략 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)합니다. 자세한 정보 및 배경은 [오류 및 예외 처리(최신 C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)를 참조하세요.

## <a name="avoid-catching-and-throwing-exceptions"></a>예외 catch 및 throw 방지
[예외로부터 안전 코드](/cpp/cpp/how-to-design-for-exception-safety)를 계속 쓰는 것이 좋지만 가능한 한 예외 catch 및 throw를 방지하는 것이 좋습니다. 예외에 대한 처리기가 없는 경우 Windows는 문제의 위치를 추적하는 데 도움이 되는 오류 보고서(크래시 미니덤프 포함)를 자동으로 생성합니다.

catch하려는 예외를 throw하지 마십시오. 또한 예상된 오류에 대해 예외를 사용하지 마십시오. *예기치 않게 런타임 오류가 발생할 때만* 예외를 throw하고 다른 모든 경우 직접, 오류의 원인과 가까이에서 오류/결과 코드로 처리합니다. 그러면 예외*가* throw 되는 경우 그 원인이 코드의 버그이거나 또는 시스템의 예외적인 오류임을 알 수 있습니다.

Windows 레지스트리에 액세스하는 시나리오를 고려하세요. 앱이 레지스트리에서 값을 읽는 데 실패하면 이를 수행할 수 있으며 적절하게 처리해야 합니다. 예외를 throw하지 마십시오. 아마도 값을 읽을 수 없는 이유를 나타내는 `bool` 또는 `enum`을 반환합니다. 반면에 레지스트리에 값을 *쓰는* 것에 실패하면 응용 프로그램에서 처리할 수 있는 보다 큰 문제가 있다는 것을 나타낼 가능성이 있습니다. 응용 프로그램을 계속하지 않으려는 경우, 오류 보고서를 일으키는 예외는 응용 프로그램이 어떤 손해도 일으키지 않도록 하는 가장 빠른 방법입니다.

다른 예의 경우 [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_)에 대한 호출의 미리 보기 이미지를 검색한 다음 해당 미리 보기를 [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_)에 전달하는 것을 고려하세요. 해당 일련의 호출으로 인해 **SetSourceAsync**에 `nullptr`을 전달할 수 있는 경우(이미지 파일을 읽을 수 없습니다. 아마도 파일 확장명으로 인해 이미지 데이터를 포함하는 것으로 보이지만 실제로 포함하지 않을 수 있습니다) 잘못된 포인터 예외가 발생합니다. 코드에서 이러한 경우를 발견하면 이를 예외로 catch 및 처리하는 대신 **GetThumbnailAsync**에서 반환한 `nullptr`을 확인합니다.

예외 발생은 오류 코드를 사용하는 것보다 속도가 느릴 수 있습니다. 심각한 오류가 발생했을 때 예외를 throw하기만 하는 경우 모든 것이 잘 작동하면 성능 비용을 지불하지 않습니다.

하지만 흔하지 않지만 적절한 소멸자가 예외를 throw한 이벤트에 호출됨을 확인하는 런타임 오버헤드가 발생할 가능성이 있습니다. 이 확인 비용은 예외가 실제로 throw되었는지 여부에 따라 발생합니다. 따라서 컴파일러가 잠재적으로 예외를 throw할 수 있는 함수에 대해 좋은 생각이 있는지 확인해야 합니다. 컴파일러는 특정 함수(`noexcept` 사양)에서 예외가 없을 것이며, 생성하는 코드를 최적화할 수 있음을 증명할 수 있습니다.

## <a name="catching-exceptions"></a>예외 catch
[Windows 런타임 ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 계층에서 발생하는 오류 조건은 HRESULT 값의 형식으로 반환됩니다. 하지만 코드에서 HRESULT를 처리할 필요는 없습니다. 사용 측면에서 API를 위해 생성된 C++/WinRT 프로젝션 코드는 ABI 계층에서 오류 HRESULT 코드를 검색하고 검색하여 처리할 수 있는 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 예외로 코드를 변환합니다.

예를 들어, 응용 프로그램이 해당 컬렉션을 반복하는 동안 사용자가 사진 라이브러리의 이미지를 삭제하면 프로젝션이 예외를 throw합니다. 이러한 경우 해당 예외를 catch 및 처리해야 합니다. 이 경우를 보여주는 코드 예는 다음과 같습니다.

```cppwinrt
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
            HRESULT hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

`co_await`된 함수를 호출할 때 코루틴에서 이와 동일한 패턴을 사용합니다. HRESULT에서 예외로의 변환에 대한 다른 예는 구성 요소 API가 E_OUTOFMEMORY를 반환하면 **std::bad_alloc**이 반환된다는 것입니다.

## <a name="throwing-exceptions"></a>예외 발생
주어진 함수에 대한 호출이 실패하는 경우 응용 프로그램이 복구할 수 없음(더 이상 예상대로 작동하기 위해 이에 의존할 수 없음)을 결정하는 경우가 있습니다. 아래의 예제는 [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) 값을 [**CreateEvent**](https://msdn.microsoft.com/library/windows/desktop/ms682396)에서 반환한 HANDLE의 래퍼로 사용합니다. 그런 다음 핸들을(여기에서 `bool` 값을 만들어) [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 함수 템플릿에 전달합니다. **winrt::check_bool**은 `bool`, 또는 `false`(오류 조건) 또는 `true`(성공 조건)로 변환할 수 있는 값으로 작동합니다.

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

[**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool)에 전달한 값이 false인 경우 다음 순서의 작업이 발생합니다.

- **winrt::check_bool**은 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 함수를 호출합니다.
- **winrt:: throw_last_error** 는 [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) 를 호출 하는 스레드의 마지막 오류 코드 값을 검색을 호출 하 고 [**winrt:: throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) 함수를 호출 합니다.
- **winrt::throw_hresult**는 오류 코드를 나타내는 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 개체(또는 표준 개체)를 사용하여 예외를 throw합니다.

Windows API가 다양한 반환 값 형식을 사용하여 런타임 오류를 보고하기 때문에 **winrt::check_bool** 외에도 값을 확인하고 예외를 throw하기 위해 유용한 몇 가지 다른 도우미 함수가 있습니다.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). HRESULT 코드가 오류를 나타내는지 여부를 확인합니다. 그러한 경우 **winrt::throw_hresult**를 호출합니다.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). 코드가 오류를 나타내는지 여부를 확인합니다. 그러한 경우 **winrt::throw_hresult**를 호출합니다.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). 포인터가 null인지 여부를 확인합니다. 그러한 경우 **winrt::throw_last_error**를 호출합니다.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). 코드가 오류를 나타내는지 여부를 확인합니다. 그러한 경우 **winrt::throw_hresult**를 호출합니다.

일반적인 반환 코드 형식에 대해 이러한 도우미 함수를 사용하거나 모든 오류 조건에 응답하여 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 또는 [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)를 호출할 수 있습니다. 

## <a name="throwing-exceptions-when-authoring-an-api"></a>API를 작성할 때 예외 throw
[Windows 런타임 ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 경계를 넘어가는 예외에 대해 유효하지 않기 때문에 구현에서 발생하는 오류 조건이 HRESULT 오류 코드의 형태로 ABI 계층에서 반환됩니다. C++/WinRT를 사용하여 API를 작성하는 경우 구현에서 HRESULT에 *throw한* 모든 예외를 변환할 수 있도록 코드가 생성됩니다. [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 함수는 이러한 패턴에서 생성된 코드에서 사용됩니다.

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

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)는 **std::exception**, [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 및 그 파생 형식에서 파생된 예외를 처리합니다. API 소비자가 풍부한 오류 정보를 받을 수 있도록 구현에서 **winrt::hresult_error** 또는 파생 형식을 사용하는 것이 좋습니다. **std::exception**(E_FAIL에 매핑됨)은 표준 템플릿 라이브러리를 사용하여 발생하는 예외에서 지원됩니다.

## <a name="assertions"></a>어설션
응용 프로그램에서 내부 가정에 대해 어설션이 있습니다. 가능한 경우 컴파일 시간 유효성 검사에 대해 **static_assert**를 선호합니다. 런타임 조건의 경우 부울 식으로 WINRT_ASSERT를 사용합니다.

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT는 릴리스 빌드, 디버그 빌드에서 떨어져 컴파일됩니다. 어설션이 있는 코드의 줄에 있는 디버거에서 응용 프로그램을 중지합니다.

소멸자에서 예외를 사용하지 마십시오. 따라서 최소한 디버그 빌드에서 WINRT_VERIFY(부울 식으로) 및 WINRT_VERIFY_(예상된 결과 및 부울 식으로)로 소멸자에서 함수를 호출한 결과를 어설션합니다.

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
