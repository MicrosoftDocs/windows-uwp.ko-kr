---
description: C++/WinRT를 사용하여 Windows 런타임 비동기 개체를 만들고 사용하는 방법을 보여 줍니다.
title: C++/WinRT를 통한 동시성 및 비동기 작업
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 동시성, 비동기, 비동기, 비동기성
ms.localizationpriority: medium
ms.openlocfilehash: cbabf38f41ae940f5c92944154638eae7016e043
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660094"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>C++/WinRT를 통한 동시성 및 비동기 작업

이 항목에서는 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 통해 Windows 런타임 비동기 개체를 만들고 사용하는 방법을 보여 줍니다.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>비동기 작업 및 Windows 런타임 “비동기” 함수

완료하는 데 50밀리초 이상 걸릴 가능성이 높은 Windows 런타임 API는 비동기 함수(이름이 “Async”로 끝나는 함수)로 구현됩니다. 비동기 함수 구현은 다른 스레드에서 작업을 시작하고 비동기 작업을 나타내는 개체와 함께 즉시 반환됩니다. 비동기 작업이 완료되면, 반환된 개체에 작업의 결과 값이 포함됩니다. **Windows::Foundation** Windows 런타임 네임스페이스에는 네 가지 유형의 비동기 작업 개체가 포함됩니다.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_),
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

각 비동기 작업 유형은 **winrt::Windows::Foundation** C++/WinRT 네임스페이스의 해당 유형에 프로젝션됩니다. C++/WinRT에는 내부 await 어댑터 구조체도 포함되어 있습니다. 직접 사용하지는 않지만, 이 구조체 덕분에 `co_await` 문을 작성하여 비동기 작업 유형 중 하나를 반환하는 함수의 결과를 협조적으로 기다릴 수 있습니다. 또한 이러한 유형을 반환하는 고유한 코루틴을 작성할 수 있습니다.

비동기 Windows 함수의 예로 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 형식의 비동기 작업 개체를 반환하는 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)가 있습니다. C++/WinRT를 사용하여 이러한 API를 호출하는 몇 가지 방법을 차단 방법과 비차단 방법 순으로 살펴보겠습니다.

## <a name="block-the-calling-thread"></a>호출 스레드 차단

아래 코드 예제는 **RetrieveFeedAsync**에서 비동기 작업 개체를 받은 후 해당 개체에서 **get**을 호출하여 비동기 작업 결과가 제공될 때까지 호출 스레드를 차단합니다.

이 예제를 복사하여 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트의 주 소스 코드 파일에 직접 붙여넣으려는 경우 먼저 프로젝트 속성에서 **미리 컴파일된 헤더 사용 안 함**을 설정합니다.

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

**get**을 호출하면 편리하게 코딩할 수 있으며 어떤 이유로든 코루틴을 사용하지 않으려는 콘솔 앱이나 백그라운드 스레드에 적합합니다. 하지만 동시 또는 비동기가 아니므로 UI 스레드에는 적합하지 않으며, 둘 중 하나에서 사용하려고 하면 최적화되지 않은 빌드에서 어설션이 발생합니다. 따라서 OS 스레드 정체로 인해 다른 유용한 작업을 수행하지 못하는 경우를 방지하려면 다른 기술이 필요합니다.

## <a name="write-a-coroutine"></a>코루틴 작성

C++/WinRT는 C++ 코루틴을 프로그래밍 모델에 통합하여 결과를 협조적으로 기다릴 수 있는 자연스러운 방법을 제공합니다. 코루틴을 작성하여 고유한 Windows 런타임 비동기 작업을 생성할 수 있습니다. 아래 코드 예제에서는 **ProcessFeedAsync**가 코루틴입니다.

> [!NOTE]
> **get** 함수가 C++/WinRT 프로젝션 형식 **winrt::Windows::Foundation::IAsyncAction**에 있으므로 C++/WinRT 프로젝트 내에서 함수를 호출할 수 있습니다. 실제 Windows 런타임 형식 **IAsyncAction**의 ABI(애플리케이션 이진 인터페이스) 표면에 속하지 않으므로 **get** 함수는 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 인터페이스의 멤버로 나열되지 않습니다.

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

코루틴은 일시 중단했다가 다시 시작할 수 있는 함수입니다. 위의 **ProcessFeedAsync** 코루틴에서는 `co_await` 문에 도달할 때 코루틴이 비동기 방식으로 **RetrieveFeedAsync** 호출을 시작한 후 즉시 일시 중단되고 컨트롤을 호출자(위 예제에서는 **main**)에 반환합니다. 그러면 피드를 검색하고 출력하는 동안 **main**이 작업을 계속할 수 있습니다. 작업이 완료되면(**RetrieveFeedAsync** 호출 완료 시) **ProcessFeedAsync** 코루틴이 다음 문에서 다시 시작됩니다.

코루틴을 다른 코루틴에 집계할 수 있습니다. 또는 **get**을 호출하여 차단하고 완료될 때까지 기다린 다음, 결과가 있을 경우 가져올 수 있습니다. 또는 Windows 런타임을 지원하는 다른 프로그래밍 언어에 전달할 수 있습니다.

대리자를 사용하여 비동기 작업의 완료 및/또는 진행률 이벤트를 처리할 수도 있습니다. 자세한 내용과 코드 예제는 [비동기 작업을 위한 대리자 형식](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)을 참조하세요.

## <a name="asychronously-return-a-windows-runtime-type"></a>Windows 런타임 형식의 비동기 반환

다음 예제에서는 특정 URI에 대해 **RetrieveFeedAsync** 호출을 래핑하여 [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed)를 비동기 방식으로 반환하는 **RetrieveBlogFeedAsync** 함수를 제공합니다.

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

위의 예제에서 **RetrieveBlogFeedAsync**는 진행률과 반환 값이 둘 다 있는 **IAsyncOperationWithProgress**를 반환합니다. **RetrieveBlogFeedAsync**가 작업을 수행하고 피드를 검색하는 동안 다른 작업을 수행할 수 있습니다. 해당 비동기 작업 개체에서 **get**을 호출하여 차단하고 완료될 때까지 기다린 다음, 작업 결과를 가져옵니다.

Windows 런타임 형식을 비동기 방식으로 반환하는 경우 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) 또는 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 반환해야 합니다. 자사 또는 타사 런타임 클래스는 Windows 런타임 함수로 전달하거나 전달받을 수 있는 형식(예: `int` 또는 **winrt::hstring**)을 한정합니다. Windows 런타임이 아닌 형식에 이러한 비동기 작업 유형 중 하나를 사용하려고 하면 컴파일러에서 “WinRT 형식이어야 합니다.” 오류를 생성하여 도와줍니다. 

코루틴에 `co_await` 문이 없는 경우, 코루틴이 되려면 `co_return` 또는 `co_yield` 문이 하나 이상 있어야 합니다. 코루틴이 비동기성을 도입하지 않아 컨텍스트를 차단하거나 전환하지 않고 값을 반환할 수 있는 경우도 있습니다. 다음은 값을 캐시하여 두 번째 이상 호출 시 해당 작업을 수행하는 예제입니다.

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

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Windows 런타임이 아닌 형식의 비동기 반환

Windows 런타임 형식이 ‘아닌’ 형식을 비동기 방식으로 반환하는 경우 PPL(병렬 패턴 라이브러리) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)를 반환해야 합니다.  **std::future**보다 성능이 뛰어나고 향후 호환성도 우수한 **concurrency::task**를 사용하는 것이 좋습니다.

> [!TIP]
> `<pplawait.h>`를 포함하면, **concurrency::task**를 코루틴 형식으로 사용할 수 있습니다.

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

## <a name="parameter-passing"></a>매개 변수 전달

동기 함수의 경우 기본적으로 `const&` 매개 변수를 사용해야 합니다. 그러면 참조 계산을 포함하며 연동된 증가 및 감소를 의미하는 복사본 오버헤드를 방지할 수 있습니다.

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

코루틴에서는 컨트롤이 호출자에 반환되는 첫 번째 일시 중단 지점까지 동기 방식으로 실행됩니다. 코루틴이 다시 시작될 때까지 참조 매개 변수가 참조하는 소스 값이 변경되었을 수 있습니다. 코루틴의 관점에서 참조 매개 변수의 수명은 제어되지 않습니다. 따라서 위 예제에서 `co_await`까지는 ‘값’에 액세스해도 안전하지만 이후에는 안전하지 않습니다.  호출자에 의해 *값*이 소멸되는 이벤트에서 그 이후 코루틴 내의 해당 값에 액세스하려고 하면 메모리가 손상됩니다. 함수가 일시 중단되었다가 다시 시작된 후 ‘값’을 사용하려고 시도할 위험이 있는 경우 **DoOtherWorkAsync**에 ‘값’을 안전하게 전달할 수도 없습니다.  

일시 중단했다가 다시 시작한 후 매개 변수를 안전하게 사용하려면 코루틴이 기본적으로 값으로 전달을 사용하여 값으로 캡처함으로써 수명 문제를 방지해야 합니다. 이 지침을 따르지 않아도 안전하다고 확신할 수 있는 경우는 흔치 않습니다.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

값으로 전달하기 위해서는 저비용으로 인수를 이동 또는 복사할 수 있어야 하며, 이는 일반적으로 스마트 포인터에서 흔한 경우입니다.

값을 이동하려는 경우가 아니면, const 값으로 전달하는 것이 좋다는 주장도 가능합니다. 복사본을 만드는 소스 값에는 영향을 미치지 않지만 의도를 보다 명확하게 하며, 실수로 복사본을 수정하는 경우 도움이 됩니다.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

표준 벡터를 비동기 호출 수신자에 전달하는 방법을 설명하는 [표준 배열 및 벡터](std-cpp-data-types.md#standard-arrays-and-vectors)도 참조하세요.

코루틴의 서명은 변경할 수 없지만 구현은 변경할 수 있는 경우에는 첫 번째 `co_await` 전에 로컬 복사본을 만들 수 있습니다.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

`Param` 복사에 비용이 많이 들면 첫 번째 `co_await` 전에 필요한 구성 요소를 추출합니다.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>클래스-멤버 코루틴에서 안전하게 *this* 포인터 액세스

[C++/WinRT의 강한 참조 및 약한 참조](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)를 참조하세요.

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Windows 스레드 풀에 작업 오프로딩

코루틴은 함수가 실행을 반환할 때까지 호출자가 차단된다는 점에서 다른 함수와 같은 함수입니다. 또한 코루틴의 첫 번째 반환 기회는 첫 번째 `co_await`, `co_return` 또는 `co_yield`입니다.

따라서 코루틴에서 컴퓨팅 바인딩된 작업을 수행하기 전에 호출자에 실행을 반환(즉, 일시 중단 지점 도입)하여 호출자가 차단되지 않도록 해야 합니다. 다른 일부 작업을 `co_await`하여 이미 수행하지 않은 경우, [**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background) 함수를 `co_await`할 수 있습니다. 그러면 컨트롤이 호출자에 반환되고, 스레드 풀 스레드에서 즉시 실행이 다시 시작됩니다.

구현에서 사용되는 스레드 풀은 하위 수준의 [Windows 스레드 풀](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api)이므로 매우 효율적입니다.

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

## <a name="programming-with-thread-affinity-in-mind"></a>스레드 선호도를 고려한 프로그래밍

이 시나리오는 이전 시나리오를 확장합니다. 스레드 풀에 일부 작업을 오프로드한 후 UI(사용자 인터페이스)에 진행 상황을 표시하려고 합니다.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

**TextBlock**은 만든 스레드인 UI 스레드에서 업데이트해야 하므로, 위의 코드에서는 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) 예외가 throw됩니다. 한 가지 해결 방법은 코루틴이 원래 호출된 스레드 컨텍스트를 캡처하는 것입니다. 이렇게 하려면 [**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context) 개체를 인스턴스화하고 백그라운드 작업을 수행한 다음, **apartment_context**를 `co_await`하여 호출 컨텍스트로 다시 전환합니다.

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

**TextBlock**을 만든 UI 스레드에서 위의 코루틴을 호출하기만 하면 이 기술은 성공합니다. 앱에서 기술 성공을 확신할 수 있는 여러 가지 경우가 있습니다.

호출 스레드가 확실하지 않은 경우에 적용되는, UI 업데이트에 대한 보다 일반적인 해결 방법을 위해 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 함수를 `co_await`하여 특정 포그라운드 스레드로 전환할 수 있습니다. 아래 코드 예제에서는 해당 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 속성에 액세스하여 **TextBlock**과 연결된 디스패처 개체를 전달하여 포그라운드 스레드를 지정합니다. **winrt::resume_foreground** 구현이 해당 디스패처 개체에서 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)를 호출하여 코루틴에서 이후에 오는 작업을 실행합니다.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>코루틴의 실행 컨텍스트, 다시 시작 및 전환

대체로 코루틴의 일시 중단 지점 이후에는 원래의 실행 스레드가 사라지고, 임의의 스레드에서 다시 시작될 수 있습니다. 즉, 임의의 스레드에서 비동기 작업에 대해 **Completed** 메서드를 호출할 수 있습니다.

그러나 네 가지 Windows 런타임 비동기 작업 유형(**IAsyncXxx**) 중 하나를 `co_await`하는 경우 C++/WinRT는 `co_await`한 지점에서 호출 컨텍스트를 캡처합니다. 또한 연속이 다시 시작될 때 해당 컨텍스트에 있도록 합니다. 이 작업을 위해 C++/WinRT는 호출 컨텍스트에 이미 있는지 여부를 확인하고, 호출 컨텍스트에 없을 경우 해당 컨텍스트로 전환합니다. `co_await` 이전에 STA(단일 스레드 아파트) 스레드에 있었다면 이후에도 동일한 스레드에 있습니다. `co_await` 이전에 MTA(다중 스레드 아파트) 스레드에 있었다면 이후에도 동일한 스레드에 있습니다.

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

이 동작에 의존할 수 있는 이유는 C++/WinRT에서 이러한 Windows 런타임 비동기 작업 유형을 C++ 코루틴 언어 지원에 맞게 조정하는 코드를 제공하기 때문입니다(이러한 코드 조각을 대기 어댑터라고 함). C++/WinRT에 남아 있는 대기 가능 유형은 단순히 스레드 풀 래퍼 및/또는 도우미이기 때문에 스레드 풀에서 완료됩니다.

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

다른 일부 유형을 `co_await`하는 경우 C++/WinRT 코루틴 구현 내에서 수행하더라도 다른 라이브러리에서 어댑터를 제공하며, 다시 시작 및 컨텍스트 측면에서 해당 어댑터의 기능을 파악해야 합니다.

컨텍스트 전환을 최소한으로 유지하려면, 이 항목에서 이미 확인한 몇 가지 기술을 사용할 수 있습니다. 이 작업을 수행하는 몇 가지 예를 살펴보겠습니다. 다음 의사 코드 예제에서는 Windows 런타임 API를 호출하여 이미지를 로드하고, 백그라운드 스레드에 놓여 해당 이미지를 처리한 다음, UI 스레드로 돌아가 UI에 이미지를 표시하는 이벤트 처리기의 개요를 보여 줍니다.

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

이 시나리오에서 **StorageFile::OpenAsync** 호출은 약간 비효율적입니다. 처리기가 호출자에 실행을 반환할 수 있도록 다시 시작 시 필요한 백그라운드 스레드로의 컨텍스트 전환이 있으며, 다시 시작된 후 C++/WinRT가 UI 스레드 컨텍스트를 복원합니다. 그러나 이 경우에는 UI를 업데이트할 때까지 UI 스레드에 있지 않아도 됩니다. **winrt::resume_background** 호출 ‘전에’ 호출하는 Windows 런타임 API가 많을수록, 불필요한 컨텍스트 전환이 많이 발생합니다.  해결 방법은 그전에 Windows 런타임 API를 호출하지 ‘않는’ 것입니다.  모든 호출을 **winrt::resume_background** 뒤로 이동합니다.

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

보다 높은 수준의 작업을 수행하려는 경우 고유한 await 어댑터를 작성할 수 있습니다. 예를 들어 비동기 작업이 완료되는 스레드에서 `co_await`를 다시 시작하려는 경우(컨텍스트 전환 없음), 먼저 아래와 유사한 await 어댑터를 작성할 수 있습니다.

> [!NOTE]
> 아래 코드 예제는 교육 목적으로만 제공되며, await 어댑터의 작동 방식 이해를 돕기 위한 것입니다. 자체 코드베이스에서 이 기술을 사용하려는 경우 고유한 await 어댑터 구조체를 개발하고 테스트하는 것이 좋습니다. 예를 들어 **complete_on_any**, **complete_on_current** 및 **complete_on(dispatcher)** 을 작성할 수 있습니다. 또한 **IAsyncXxx** 유형을 템플릿 매개 변수로 사용하는 템플릿으로 만드는 것이 좋습니다.

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

**no_switch** await 어댑터를 사용하는 방법을 파악하려면 먼저 C++ 컴파일러에서 `co_await` 식을 발견할 경우 **await_ready**, **await_suspend**, **await_resume** 함수를 찾는다는 것을 알아야 합니다. C++/WinRT 라이브러리는 기본적으로 다음과 같은 적절한 동작을 얻을 수 있도록 이러한 함수를 제공합니다.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

**no_switch** await 어댑터를 사용하려면 다음과 같이 `co_await` 식의 유형을 **IAsyncXxx**에서 **no_switch**로 변경합니다.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

그러면 C++ 컴파일러에서 **IAsyncXxx**와 일치하는 **await_xxx** 함수 3개를 찾는 대신, **no_switch**와 일치하는 함수를 찾습니다.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>비동기 작업 취소 및 취소 콜백

비동기 프로그래밍을 위한 Windows 런타임 기능을 사용하면 진행 중인 비동기 작업을 취소할 수 있습니다. 다음은 [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync)를 호출하여 잠재적으로 큰 파일 컬렉션을 검색하고 결과 비동기 작업 개체를 데이터 멤버에 저장하는 예제입니다. 사용자가 작업을 취소할 수 있습니다.

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

취소 구현 측면의 경우 간단한 예제로 시작하겠습니다.

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

위의 예제를 실행하면 **ImplicitCancellationAsync**가 3초간 초당 1개 메시지를 출력하며, 이후에는 취소 결과로 자동 종료됩니다. `co_await` 식을 발견할 경우 코루틴이 취소 여부를 확인하기 때문에 이 코드는 제대로 실행됩니다. 취소된 경우 단락되고, 취소되지 않은 경우 정상적으로 일시 중단됩니다.

코루틴이 일시 중단된 동안 취소가 발생할 수도 있습니다. 코루틴이 다시 시작되거나 다른 `co_await`를 적중하는 경우에만 취소를 확인합니다. 잠재적 문제 중 하나는 취소 응답 시 너무 성긴 대기 시간입니다.

따라서 또 다른 옵션은 코루틴 내에서 취소를 명시적으로 폴링하는 것입니다. 위의 예제를 아래 목록의 코드로 업데이트합니다. 이 새로운 예제에서 **ExplicitCancellationAsync**는 [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) 함수에서 반환된 개체를 검색하고, 이 개체를 사용하여 코루틴의 취소 여부를 정기적으로 검사합니다. 취소되지 않는 한, 코루틴이 무기한 반복됩니다. 취소되면, 루프와 함수가 정상적으로 종료됩니다. 결과는 이전 예제와 동일하지만, 여기서는 종료가 명시적으로 발생하고 제어됩니다.

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

**winrt::get_cancellation_token**을 기다리면 코루틴에서 자동으로 생성하는 **IAsyncAction** 정보와 함께 취소 토큰이 검색됩니다. 해당 토큰의 함수 호출 연산자를 사용하여 취소 상태를 쿼리할 수 있습니다(근본적으로 취소 폴링). 컴퓨팅 바인딩된 일부 작업을 수행하거나 큰 컬렉션을 반복하는 경우에 적합한 기술입니다.

### <a name="register-a-cancellation-callback"></a>취소 콜백 등록

Windows 런타임의 취소는 다른 비동기 개체로 자동으로 이동하지 않습니다. 그러나 Windows SDK 버전 10.0.17763.0(Windows 10, 버전 1809)부터 취소 콜백을 등록할 수 있습니다. 취소를 전파할 수 있는 선점형 후크인 이 취소 콜백을 사용하면 기존 동시성 라이브러리와 통합할 수 있습니다.

다음 코드 예제에서 **NestedCoroutineAsync**는 작업을 수행하지만, 특별한 취소 논리가 없습니다. **CancellationPropagatorAsync**는 근본적으로 중첩 코루틴의 래퍼입니다. 이 래퍼는 선점 방식으로 취소를 전달합니다.

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

**CancellationPropagatorAsync**는 고유한 취소 콜백에 대해 람다 함수를 등록한 다음, 중첩 작업이 완료될 때까지 기다립니다(일시 중단). **CancellationPropagatorAsync**가 취소되는 경우 취소를 중첩 코루틴으로 전파합니다. 취소를 폴링할 필요가 없으며, 취소가 무기한 차단되지도 않습니다. 이 메커니즘은 C++/WinRT를 전혀 모르는 코루틴 또는 동시성 라이브러리와 상호 운용하는 데 사용할 수 있을 만큼 유연합니다.

## <a name="reporting-progress"></a>진행 상황 보고

코루틴이 [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) 또는 [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 반환하는 경우 [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) 함수에서 반환된 개체를 검색하고, 이 개체를 사용하여 진행률 처리기에 진행 상황을 다시 보고할 수 있습니다. 코드 예제는 다음과 같습니다.

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
> 비동기 작업에 대해 ‘완료 처리기’를 둘 이상 구현하는 것은 올바르지 않습니다.  완료 이벤트에 단일 대리자를 사용하거나 완료 이벤트를 `co_await`할 수 있습니다. 둘 다 사용하면 두 번째는 실패합니다. 다음 두 종류의 완료 처리기 중 하나만 사용해야 하며, 동일한 비동기 개체에 대해 둘 다 사용하면 안 됩니다.

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

완료 처리기에 대한 자세한 내용은 [비동기 작업을 위한 대리자 형식](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)을 참조하세요.

## <a name="fire-and-forget"></a>시작 후 망각형(Fire and Forget)

다른 작업과 동시에 수행할 수 있는 작업이 있고, 해당 작업이 완료되기를 기다리거나(종속된 다른 작업이 없음) 작업에서 값이 반환될 필요가 없는 경우도 있습니다. 이 경우 작업을 시작하고 망각할 수 있습니다. 반환 형식이 Windows 런타임 비동기 작업 유형 중 하나 또는 **concurrency::task**가 아니라 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)인 코루틴을 작성하면 됩니다.

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

**winrt::fire_and_forget**은 비동기 작업을 수행해야 할 때 이벤트 처리기의 반환 형식으로도 유용합니다. 다음은 예제입니다([C++/WinRT의 강한 참조 및 약한 참조](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine) 참조).

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

첫 번째 인수(*sender*)는 사용하지 않으므로 명명되지 않은 그대로입니다. 따라서 이 인수는 참조로 두어도 안전합니다. 하지만 *args*는 값으로 전달됩니다. 위의 [매개 변수 전달](#parameter-passing) 섹션을 참조하세요.

## <a name="important-apis"></a>중요 API
* [concurrency::task 클래스](/cpp/parallel/concrt/reference/task-class)
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
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
* [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)
