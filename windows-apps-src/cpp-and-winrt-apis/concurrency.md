---
description: 이 항목에서는 C++/WinRT를 통해 Windows 런타임 비동기 개체를 생성하고 사용하는 방법에 대해서 설명합니다.
title: C++/WinRT로 동시성 및 비동기 작업
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 동시성, 비동기, 비동기식, 비동기성
ms.localizationpriority: medium
ms.openlocfilehash: 910d7a7ca2aaebac6dd462d7104b26a989cf8814
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721660"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>C++/WinRT로 동시성 및 비동기 작업

이 항목에서는 둘 다 하는 방법을 만들고 사용 하 여 Windows 런타임 비동기 개체 사용을 보여 줍니다 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)합니다.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>비동기 작업 및 Windows 런타임 "비동기" 함수

완료하는 데 50밀리초 이상 걸릴 가능성이 높은 Windows 런타임 API는 비동기 함수(이름이 "Async"로 끝나는 함수)로 구현됩니다. 비동기 함수의 구현체는 다른 스레드에서 작업을 시작하고 비동기 작업을 나타내는 개체를 즉시 반환합니다. 비동기 작업을 마치면 반환된 개체에 작업의 결과 값이 포함됩니다. **Windows::Foundation** Windows 런타임 네임스페이스에는 4가지 형식의 비동기 작업 개체가 포함됩니다.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_), and
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

각 비동기 작업 형식은 **winrt::Windows::Foundation** C++/WinRT 네임스페이스에서 해당하는 형식으로 프로젝션됩니다. C++/WinRT에는 내부의 await 어댑터 구조체도 포함됩니다. 직접 하지만 해당 구조체 덕분 사용 하지 않는 작성할 수 있습니다는 `co_await` 이러한 비동기 작업 형식 중 하나를 반환 하는 모든 함수의 결과 협조적으로 await 문을 합니다. 또한 이러한 형식을 반환하는 사용자 고유의 코루틴을 작성하는 것도 가능합니다.

비동기 Windows 함수의 예로는 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)가 있습니다. 이 비동기 함수는 비동기 작업 개체로 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 형식을 반환합니다. 몇 가지 방법에 살펴보겠습니다&mdash;첫 번째 차단 및 다음 비차단&mdash;사용 하 여 C++/WinRT는 등의 API를 호출 합니다.

## <a name="block-the-calling-thread"></a>호출 스레드 차단

아래 코드 예제는 **RetrieveFeedAsync**에서 비동기 작업 개체를 수신한 후 해당 개체에 대해 **get**을 호출하여 비동기 작업 결과가 나올 때까지 호출 스레드를 차단합니다.

이 예의 기본 소스 코드 파일에 직접 복사-붙여넣기를 하려는 경우는 **Windows 콘솔 응용 프로그램 (C++/WinRT)** 프로젝트 다음 첫 번째 집합 **미리 컴파일된 헤더 사용 안 함** 프로젝트에서 속성입니다.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

**get**을 호출하면 편리하게 코딩할 수 있으며 어떤 이유로 코루틴을 사용하지 않으려는 콘솔 앱이나 백그라운드 스레드에 적합합니다. 하지만 동시 또는 비동기가 아니므로 UI 스레드에는 적합하지 않습니다(또한 이 중 하나에서 사용하려고 하면 최적화되지 않은 빌드에서 어설션이 발생합니다). 따라서 OS 스레드 정체로 인해 다른 유용한 작업까지 못하는 경우를 방지하려면 다른 기법이 필요합니다.

## <a name="write-a-coroutine"></a>코루틴 작성

C++/WinRT는 C++ 코루틴을 프로그래밍 모델에 통합하여 결과를 협조적으로 기다릴 수 있는 자연스러운 방법을 제공합니다. 사용자는 코루틴을 작성하여 고유의 Windows 런타임 비동기 작업을 생성할 수 있습니다. 아래 코드 예제에서는 **ProcessFeedAsync**가 코루틴입니다.

> [!NOTE]
> **가져오기** 함수에 존재 합니다 C++/WinRT 프로젝션 형식을 **winrt::Windows::Foundation::IAsyncAction**내에서 함수를 호출할 수 있도록, C++/WinRT 프로젝트. 구성원으로 나열 된 함수를 찾을 수 없습니다는 [ **IAsyncAction** ](/uwp/api/windows.foundation.iasyncaction) 때문 **가져오기** 응용 프로그램 이진 인터페이스 (ABI) 화면에 속하지 않은 실제 Windows 런타임 형식 **IAsyncAction**합니다.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

코루틴이란 일시 중단했다가 다시 시작할 수 있는 함수를 말합니다. 위의 **ProcessFeedAsync** 코루틴에서는 `co_await` 문에 이르면 코루틴이 비동기 방식으로 **RetrieveFeedAsync** 호출을 시작했다가 바로 스스로를 일시 중단하고 컨트롤을 호출자(위 예제에서 **main**)에게 반환합니다. 그러면 **main**이 피드를 가져와 출력하는 동안 작업을 계속 할 수 있습니다. 이 과정이 모두 끝나면(**RetrieveFeedAsync** 호출 완료 시) **ProcessFeedAsync** 코루틴이 다음 문으로 다시 시작합니다.

코루틴을 다른 코루틴으로 집계할 수 있습니다 또는 **get**을 호출하여 차단한 후 완료될 때까지 기다릴 수 있습니다(결과가 있을 경우 가져옵니다). 그 밖에 Windows 런타임을 지원하는 다른 프로그래밍 언어로 전달할 수도 있습니다.

대리자를 사용하여 비동기 작업에서 완료되었거나 진행 중인 이벤트를 처리하는 것도 가능합니다. 자세한 내용과 코드 예제는 [비동기 작업을 위한 대리자 형식](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)을 참조하세요.

## <a name="asychronously-return-a-windows-runtime-type"></a>Windows 런타임 형식의 비동기 반환

다음 예제에서는 **RetrieveFeedAsync** 호출을 래핑합니다. 그러면 특정 URI일 때 [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed)를 비동기 방식으로 반환하는 **RetrieveBlogFeedAsync** 함수를 제공합니다.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

위의 예제에서는 **RetrieveBlogFeedAsync**가 진행률과 반환 값을 모두 갖는 **IAsyncOperationWithProgress**를 반환합니다. **RetrieveBlogFeedAsync**가 작업을 하며 피드를 가져오는 동안 다른 작업을 할 수도 있습니다. 그런 다음 차단할 비동기 작업 개체에 대해 **get**을 호출하여 완료될 때까지 기다린 후 작업 결과를 가져옵니다.

Windows 런타임 형식을 비동기 방식으로 반환하는 경우에는 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) 또는 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 반환해야 합니다. 당사자 또는 제3자 런타임 클래스 또는 Windows 런타임 함수에서, 또는 함수로 전달할 수 있는 모든 형식(예를 들어, `int` 또는 **winrt::hstring**)를 한정합니다. Windows 런타임 이외의 형식으로 이러한 비동기 작업 형식 중 하나를 사용하려고 할 때 발생하는 "*must be WinRT type*" 오류 메시지는 컴파일러를 사용해 해결할 수 있습니다.

코루틴에 하나 이상의 `co_await` 문이 없는 경우 이를 코루틴으로 한정하기 위해 하나 이상의 `co_return` 또는 한 개의 `co_yield` 문이 있어야 합니다. 사용자 코루틴이 비동기를 도입하지 않고, 따라서 컨텍스트를 차단하거나 전환하지 않고 값을 반환할 수 있는 경우도 있습니다. 값을 캐싱하여 이(호출되는 두 번째 및 후속 작업)를 수행하는 예는 다음과 같습니다.

```cppwinrt
winrt::hstring m_cache;

IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Windows가 아닌 런타임 형식의 비동기 반환

Windows 런타임 *이외의* 형식을 비동기 방식으로 반환하는 경우에는 병렬 패턴 라이브러리(PPL)인 [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)를 반환해야 합니다. **concurrency::task**를 권장하는 이유는 **std::future**보다 성능이 뛰어나고 향후 호환성도 우수하기 때문입니다.

> [!TIP]
> `<pplawait.h>`를 포함하는 경우 **concurrency::task**를 코루틴 형식으로 사용할 수 있습니다.

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>매개 변수-전달

동기화 함수를 위해 기본적으로 `const&` 매개 변수를 사용해야 합니다. 복사본(참조 계산, 즉 연동된 증가 및 감소 수반) 오버헤드를 방지할 수 있습니다.

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

하지만 코루틴에 참조 매개 변수를 전달하는 경우 문제가 발생할 수 있습니다.

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

코루틴에서 실행은 컨트롤이 호출자에 반환되는 첫 번째 일시 중단 지점까지 동기화됩니다. 코루틴이 다시 시작할 때까지 참조 매개 변수가 참조하는 원본 값에 아무 일도 발생하지 않을 수 있습니다. 코루틴의 관점에서 참조 매개 변수에 제어되지 않은 수명이 있습니다. 따라서 위 예제에서는 `co_await`까지 *값*에 액세스하는 것이 안전합니다. 이에 따라 함수가 일시 중단될 위험이 있으며 다시 시작된 후 *값*을 사용하려고 하는 경우 안전하게 값**DoOtherWorkAsync**에 *값*을 전달할 수 없습니다. 일시 중지했다가 다시 시작한 후 매개 변수를 안전하게 사용할 수 있도록 하려면 코루틴이 값으로 캡처하고 수명 문제를 방지할 수 있도록 기본적으로 값으로 전달을 사용해야 합니다. 이렇게 하는 것이 안전함이 확실하기 때문에 이 지침을 벗어날 수 있는 경우는 드물 것입니다.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value);
```

(값을 이동하기 원하지 않는 경우) const 값으로 전달하는 것이 좋은지 여부는 논쟁의 여지가 있습니다. 복사본을 만드는 원본 값에는 영향을 미치지 않지만 의도를 보다 명확하게 만들며, 실수로 복사본을 수정하는 경우 도움이 됩니다.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

또한 표준 벡터를 비동기 호출 수신자에 전달하는 방법을 다루는 [표준 배열 및 벡터](std-cpp-data-types.md#standard-arrays-and-vectors)를 참조하세요.

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Windows 스레드 풀에서 오프로딩 작업

코 루틴은 다른 함수는 호출자가 반환 될 때까지 함수 실행이 차단 됩니다. 반환할 코 루틴에 대 한 첫 번째 기회는 첫 번째 `co_await`, `co_return`, 또는 `co_yield`합니다.

이 수행 하기 전에 실행 호출자에 게 반환 해야 하는 코 루틴에서 컴퓨트 바운드 작업 (즉, 일시 중단 지점을 도입) 호출자가 차단 되지 않도록 합니다. 는 아직 수행 하는 경우 `co_await`진행형이 일부 다른 작업을 수 있습니다 `co_await` 는 [ **winrt::resume_background** ](/uwp/cpp-ref-for-winrt/resume-background) 함수입니다. 컨트롤이 호출자에 반환되며 즉시 스레드 풀 스레드에서 실행이 다시 시작합니다.

구현에 사용되는 스레드 풀은 낮은 수준의 [Windows 스레드 풀](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api)이므로 이상적으로 효율적입니다.

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>스레드 선호도를 염두에 두고 프로그래밍

이 시나리오는 이전 시나리오에 확장됩니다. 스레드 풀에 일부 작업을 오프로드했으면 UI(사용자 인터페이스)에 진행 상태를 표시해야 합니다.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

**TextBlock**을 이를 만든 스레드, 즉 UI 스레드에서 업데이트해야 하기 때문에 위의 코드는 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) 예외를 throw합니다. 한 가지 방법은 원래 코루틴이 호출된 스레드 컨텍스트를 캡처하는 것입니다. 이렇게 하려면 인스턴스화할를 [ **winrt::apartment_context** ](/uwp/cpp-ref-for-winrt/apartment-context) 개체, 작업을 백그라운드에서 차례로 `co_await` 는 **apartment_context** 호출으로 다시 전환 하려면 컨텍스트입니다.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

위의 코루틴이 **TextBlock**을 만든 UI 스레드에서 호출되는 한 이 기술은 작동합니다. 앱에서 이것이 확실한 경우는 많습니다.

수를 모르는 호출 스레드에 대 한 사례에 적용 되는 UI를 업데이트 하는 보다 일반적인 솔루션에 대 한 `co_await` 는 [ **winrt::resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) 특정 전환할 함수 포그라운드 스레드입니다. 아래의 코드 예제에서 **TextBlock**(해당 [**발송자**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 속성에 액세스하여)과 연관된 발송자 개체를 전달하여 전경 스레드를 지정합니다. **winrt::resume_foreground**의 구현이 코루틴에서 이후에 오는 작업을 실행하는 해당 발송자 개체에서 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)를 호출합니다.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>실행 컨텍스트, 재개 및 코 루틴에서 전환

코 루틴에서 일시 중단 시점, 이후에 원래 실행 스레드 수 사라질 대체로 및 모든 스레드에서 다시 발생할 수 있습니다 (즉, 모든 스레드에서 호출할 수 있습니다 합니다 **Completed** 메서드는 비동기 작업에 대 한).

경우에 있습니다 `co_await` 유형은 Windows 런타임 비동기 작업 중 하나 (**IAsyncXxx**), 한 다음 C++WinRT 시점 호출 컨텍스트를 캡처합니다/있습니다 `co_await`. 및 연속 작업을 다시 시작 하면 해당 컨텍스트에 따라 계속 하는 것이 되도록 합니다. C++/ WinRT 호출 컨텍스트에서 이미 든 및 전환 그렇지 않은 경우이 확인 하 여 수행 합니다. 전에 단일 스레드 아파트 (STA) 스레드에서 경우 `co_await`, 다음 진행할 수 있습니다 것과 동일한 나중; 하기 전에 다중 스레드 아파트 (MTA) 스레드에서 경우 `co_await`를 진행할 수 있습니다 하나 나중에 다음입니다.

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

이 동작에 의존할 수 있습니다 이유 때문입니다 C++/WinRT에 해당 Windows 런타임 비동기 작업 형식에 맞게 코드를 제공 합니다 C++ 코 루틴 언어 지원 (이러한 코드의 대기 어댑터 라고 함). 남은 대기 가능 형식에 C++WinRT는 단순히 스레드 풀 래퍼 및/또는 도우미; 따라서 스레드 풀에서 완료 합니다.

```cppwinrt
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

경우 있습니다 `co_await` 다른 형식&mdash;내 에서도 C++/WinRT 코 루틴 구현&mdash;다른 라이브러리는 어댑터를 제공 하 고 다시 시작 및 컨텍스트를 기준으로 해당 어댑터 수행할 작업을 이해 해야 합니다.

최소한으로 컨텍스트 스위치를 유지 하려면이 항목에서 이미 살펴본 기술 중 일부를 사용할 수 있습니다. 이렇게 하는 몇 가지 예시를 확인해 보겠습니다. 다음 의사 코드 예제에서 이미지를 로드 하는 Windows 런타임 API를 호출 하 고, 해당 이미지를 처리 하는 데 백그라운드 스레드를 삭제 하 고, 다음 UI에 이미지를 표시 하려면 UI 스레드를 반환 하는 이벤트 처리기의 개요를 살펴보겠습니다.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

이 시나리오의 경우는 약간의 호출 하는 주변 ineffiency **StorageFile::OpenAsync**합니다. 배경으로 필요한 컨텍스트 스위치는 (있도록 처리기는 호출자에 게 실행을 반환할 수 있습니다) 스레드를 다시 시작 후에 C++WinRT UI 스레드 컨텍스트를 복원 하 는/입니다. 그러나 UI를 업데이트 하려고 합니다. 우리 될 때까지 UI 스레드에서 하 필요 없는 예제의 경우. 더 많은 Windows 런타임 Api 호출 *하기 전에* 호출 **winrt::resume_background**, 발생 하도록 둔 것 보다 불필요 한 백-및-명시 컨텍스트 스위치입니다. 솔루션은 호출 되지 않습니다 *모든* 그 전에 Windows 런타임 Api입니다. 모든 후 이동 합니다 **winrt::resume_background**합니다.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

직접 작성할 수 있습니다 다음 보다 고급 작업을 수행 하려는 경우 어댑터를 대기 합니다. 예를 들어, 하려는 경우는 `co_await` 에서 비동기 작업을 완료 하는 동일한 스레드에서 다시 시작 하려면 (따라서가 컨텍스트 스위치 없이)를 작성 하 여 시작할 수 있습니다 다음 await 어댑터 아래 표시 된 것과 비슷합니다.

> [!NOTE]
> 아래 코드 예제는 교육용 으로만; 제공 시작 하는 것이 이해 하는 방법을 어댑터 작업을 기다립니다. 개발 하 고 자체 테스트를 하는 것이 좋습니다 고유한 코드 베이스에서이 기술을 사용 하려는 경우 어댑터 struct(s) await 합니다. 예를 들어, 작성할 수 있습니다 **complete_on_any**를 **complete_on_current**, 및 **complete_on(dispatcher)** 합니다. 또한 쉽게 템플릿을 사용 하는 것이 좋습니다 합니다 **IAsyncXxx** 형식 템플릿 매개 변수로 합니다.

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

사용 하는 방법을 이해 하는 **no_switch** 어댑터 await 때 알아야 할 먼저 됩니다는 C++ 컴파일러는 `co_await` 식 호출 된 함수를 찾습니다 **await_ready**, **await_suspend**, 및 **await_resume**합니다. C++/WinRT 라이브러리는 기본적으로 다음과 같은 적절 한 동작을 받을 수 있도록 이러한 기능을 제공 합니다.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

사용 하는 **no_switch** 어댑터 await, 변경 하는 형식의 `co_await` 식 **IAsyncXxx** 에 **no_switch**, 같이 합니다.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

그런 다음 세 가지를 확인 하지 않고 **await_xxx** 일치 하는 함수 **IAsyncXxx**, C++ 컴파일러가 일치 하는 함수를 찾는 **no_switch**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>비동기 작업을 취소 콜백을 취소

비동기 프로그래밍에 대 한 Windows 런타임 기능을 사용 하면 진행 중인 비동기 작업 또는 작업을 취소할 수 있습니다. 호출 하는 예로 [ **StorageFolder::GetFilesAsync** ](/uwp/api/windows.storage.storagefolder.getfilesasync) 파일의 잠재적으로 큰 컬렉션을 검색 하는 데이터 멤버에는 결과 비동기 작업 개체를 저장 합니다. 사용자가 작업을 취소 하는 옵션입니다.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

취소는 구현 측면에 대 한 간단한 예제를 사용 하 여 시작 해 보겠습니다.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

위의 예제를 실행할 경우 보면 **ImplicitCancellationAsync** 3 초 후 자동으로 종료 시간 결과적으로 되 고의 취소에 대 한 초당 하나의 메시지를 인쇄 합니다. 이 때문에 발생 한 `co_await` 식 코 루틴 취소 되었는지 여부를 확인 합니다. 가 있는 경우 다음이 단락 (short-circuit)입니다. 및 되어 있지 않으면 다음 일시 중단 정상적으로 합니다.

취소는 코 루틴은 일시 중단 된 동안에 물론 발생할 수 있습니다. 코 루틴을 다시 시작 되는 경우에 다른 도달 또는 `co_await`, 취소에 대 한 확인 됩니다. 문제는 잠재적으로 너무 정교 하지 않은-세분화 된 대기 시간 취소에 응답 중 하나입니다.

따라서 다른 옵션은 프로그램 코 루틴 내에서 취소에 대 한 명시적으로 폴링할입니다. 아래 목록에 코드를 사용 하 여 위의 예제를 업데이트 합니다. 이 새 예에서 **ExplicitCancellationAsync** 반환 하는 개체를 검색 합니다 [ **winrt::get_cancellation_token** ](/uwp/cpp-ref-for-winrt/get-cancellation-token) 함수를 사용 하 여 주기적으로 코 루틴 취소 되었는지 여부를 확인 합니다. 코 루틴; 무한 반복으로 취소 되지 않은 취소 되 면 루프 및 함수가 정상적으로 종료 합니다. 이전 예제와 이지만 여기서 종료 명시적으로 발생 하는 대로 및 제어 결과 같습니다.

```cppwinrt
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

대기할 **winrt::get_cancellation_token** 대 한 지식이 있는 취소 토큰을 검색 합니다 **IAsyncAction** 코 루틴을 사용자 대신 생성 하는 합니다. 취소 상태를 쿼리하려면 해당 토큰에서 함수 호출 연산자를 사용할 수 있습니다&mdash;기본적으로 취소에 대 한 폴링. 컴퓨트 바운드 작업을 수행 하거나 많은 컬렉션을 반복 하 고 있으면 적절 한 기술 됩니다.

### <a name="register-a-cancellation-callback"></a>취소 콜백을 등록합니다

Windows 런타임의 취소는 다른 비동기 개체에 자동으로 흐르지 않습니다. 하지만&mdash;10.0.17763.0 (Windows 10, 버전 1809) 버전의 Windows SDK에 도입 된&mdash;취소 콜백을 등록할 수 있습니다. 이 선점형 후크는 취소를 전파할 수 및 기존 동시성 라이브러리와 통합할 수 있도록 합니다.

이 다음 코드 예제의 **NestedCoroutineAsync** 작업을 수행 되지만에 특별 한 취소 논리가 없습니다. **CancellationPropagatorAsync** 중첩된 코 루틴 일;에서 래퍼는 래퍼 선제적 취소를 전달 합니다.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([=]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync** 레지스터 람다 함수에 대 한 자체 취소 콜백은 다음 기다립니다 (일시 중단) 중첩 된 작업이 완료 될 때까지 합니다. 경우 때나 **CancellationPropagatorAsync** 는 중첩 된 코 루틴 취소가 전파 취소 합니다. 취소에 대해 폴링할 필요가 없습니다. 나는 취소 인해 무기한으로 차단 합니다. 이 메커니즘은 C + 아무 것도 인식 하는 코 루틴 또는 동시성 라이브러리와 interop을 사용할 수 있을 만큼 유연 + WinRT 합니다.

## <a name="reporting-progress"></a>진행률 보고

사용자 코 루틴 중 하나를 반환 하는 경우 [ **IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_), 또는 [ **IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 검색할 수 있습니다는 반환 된 개체를 [ **winrt::get_progress_token** ](/uwp/cpp-ref-for-winrt/get-progress-token) 함수 및 진행률을 보고 하는 진행률 처리기로 다시 사용 합니다. 코드 예제는 다음과 같습니다.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> 둘 이상의 구현 올바르지 *완료 처리기* 비동기 작업 또는 작업에 대 한 합니다. 해당 완료 이벤트에 대 한 단일 대리자를 할 수 있습니다 또는 수 `co_await` 것입니다. 둘 다에 있는 경우 두 번째 실패 합니다. 중 하나는 다음 두 종류의 완료 처리기 중 하나는 적절 한; 둘 다 동일한 비동기 개체입니다.

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

완료 처리기에 대 한 자세한 내용은 참조 하세요. [대리자 비동기 작업 및 작업에 대 한 형식](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)합니다.

## <a name="fire-and-forget"></a>실행 후 제거

경우에 따라 다른 작업을 동시에 수행할 수 있는 작업 및 해당 작업이 완료 될 때까지 기다리는 필요가 없습니다 (다른 어떠한 작업도에 따라 다름), 또는 값을 반환 하도록 합니다. 이런 경우 작업을 실행 하 고 그는 잊어 수 있습니다. 해당 반환 형식이 코 루틴을 작성 하 여 수행할 수 있습니다 [ **winrt::fire_and_forget** ](/uwp/cpp-ref-for-winrt/fire-and-forget) (Windows 런타임 비동기 작업 형식 중 하나는 대신 또는 **concurrency:: task**).

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

## <a name="important-apis"></a>중요 API
* [concurrency:: task 클래스](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 인터페이스](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 메서드](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 클래스](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>관련 항목
* [C + 대리자를 사용 하 여 이벤트를 처리 + WinRT](handle-events.md)
* [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)
