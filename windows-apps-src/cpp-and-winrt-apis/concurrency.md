---
author: stevewhims
description: 이 항목에서는 C++/WinRT를 통해 Windows 런타임 비동기 개체를 생성하고 사용하는 방법에 대해서 설명합니다.
title: C++/WinRT로 동시성 및 비동기 작업
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 동시성, 비동기, 비동기식, 비동기성
ms.localizationpriority: medium
ms.openlocfilehash: 85071fb28cb87c991e2f5ba7f64b681c6850c819
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/10/2018
ms.locfileid: "3824797"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)로 동시성 및 비동기 작업
> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

이 항목에서는 C++/WinRT를 통해 Windows 런타임 비동기 개체를 생성하고 사용하는 방법에 대해서 설명합니다.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>비동기 작업 및 Windows 런타임 "비동기" 함수
완료하는 데 50밀리초 이상 걸릴 가능성이 높은 Windows 런타임 API는 비동기 함수(이름이 "Async"로 끝나는 함수)로 구현됩니다. 비동기 함수의 구현체는 다른 스레드에서 작업을 시작하고 비동기 작업을 나타내는 개체를 즉시 반환합니다. 비동기 작업을 마치면 반환된 개체에 작업의 결과 값이 포함됩니다. **Windows::Foundation** Windows 런타임 네임스페이스에는 4가지 형식의 비동기 작업 개체가 포함됩니다.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

각 비동기 작업 형식은 **winrt::Windows::Foundation** C++/WinRT 네임스페이스에서 해당하는 형식으로 프로젝션됩니다. C++/WinRT에는 내부의 await 어댑터 구조체도 포함됩니다. 직접 사용하지는 않지만 이 구조체 덕분에 ``co_await` 문을 작성하여 비동기 작업 형식 중 하나를 반환하는 함수의 결과를 협조적으로 기다릴 수 있습니다. 또한 이러한 형식을 반환하는 사용자 고유의 코루틴을 작성하는 것도 가능합니다.

비동기 Windows 함수의 예로는 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)가 있습니다. 이 비동기 함수는 비동기 작업 개체로 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 형식을 반환합니다. C++/WinRT를 사용해 이러한 API를 호출하는 몇 가지, 즉 차단 및 비차단 방법에 대해서 알아보겠습니다.

## <a name="block-the-calling-thread"></a>호출 스레드 차단
아래 코드 예제는 **RetrieveFeedAsync**에서 비동기 작업 개체를 수신한 후 해당 개체에 대해 **get**을 호출하여 비동기 작업 결과가 나올 때까지 호출 스레드를 차단합니다.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
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
> **Get** 함수가 존재 C + + /winrt 프로젝션 모든 C + 내에서 함수를 호출할 수 있도록 **winrt::Windows::Foundation::IAsyncAction**입력 + WinRT 프로젝트입니다. **가져오기** 는 실제 Windows 런타임 형식의 **IAsyncAction**응용 프로그램 이진 인터페이스 (ABI) 표면의 일부가 되지 않으므로 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 인터페이스의 구성원으로 나열 된 함수를 찾지 않습니다.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

    auto processOp = ProcessFeedAsync();
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

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

    auto feedOp = RetrieveBlogFeedAsync();
    // do other work.
    PrintFeed(feedOp.get());
}
```

위의 예제에서는 **RetrieveBlogFeedAsync**가 진행률과 반환 값을 모두 갖는 **IAsyncOperationWithProgress**를 반환합니다. **RetrieveBlogFeedAsync**가 작업을 하며 피드를 가져오는 동안 다른 작업을 할 수도 있습니다. 그런 다음 차단할 비동기 작업 개체에 대해 **get**을 호출하여 완료될 때까지 기다린 후 작업 결과를 가져옵니다.

Windows 런타임 형식을 비동기 방식으로 반환하는 경우에는 [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) 또는 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 반환해야 합니다. 당사자 또는 제3자 런타임 클래스 또는 Windows 런타임 함수에서, 또는 함수로 전달할 수 있는 모든 형식(예를 들어, `int` 또는 **winrt::hstring**)를 한정합니다. Windows 런타임 이외의 형식으로 이러한 비동기 작업 형식 중 하나를 사용하려고 할 때 발생하는 "*must be WinRT type*" 오류 메시지는 컴파일러를 사용해 해결할 수 있습니다.

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

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
#include <ppltasks.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
    {
        Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
        SyndicationClient syndicationClient;
        SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
        return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
    });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp = RetrieveFirstTitleAsync();
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
코루틴에서 컴퓨팅 바인딩된 작업을 수행하기 전에 호출자가 차단되지 않게 하기 위해 호출자에 실행을 반환해야 합니다(즉, 일시 중단 지점 도입). 일부 기타 작업을 `co-await`하여 아직 이를 수행하지 않고 있는 경우 **winrt::resume_background** 함수를 `co-await`할 수 있습니다. 컨트롤이 호출자에 반환되며 즉시 스레드 풀 스레드에서 실행이 다시 시작합니다.

구현에 사용되는 스레드 풀은 낮은 수준의 [Windows 스레드 풀](https://msdn.microsoft.com/library/windows/desktop/ms686766)이므로 이상적으로 효율적입니다.

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

**TextBlock**을 이를 만든 스레드, 즉 UI 스레드에서 업데이트해야 하기 때문에 위의 코드는 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread) 예외를 throw합니다. 한 가지 방법은 원래 코루틴이 호출된 스레드 컨텍스트를 캡처하는 것입니다. **winrt::apartment_context** 개체를 인스턴스화한 다음 `co_await`합니다.

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

> [!NOTE]
> **다음 코드 예시는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

스레드 호출에 대해 확실하지 않는 경우를 다루는 UI를 업데이트하는 일반적인 솔루션을 위해 [Windows 10 SDK Preview 빌드 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) 이상을 설치할 수 있습니다. 그런 다음 **winrt::resume_foreground** 함수를 `co-await`하여 특정 전경 스레드로 전환합니다. 아래의 코드 예제에서 **TextBlock**(해당 [**발송자**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 속성에 액세스하여)과 연관된 발송자 개체를 전달하여 전경 스레드를 지정합니다. **winrt::resume_foreground**의 구현이 코루틴에서 이후에 오는 작업을 실행하는 해당 발송자 개체에서 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)를 호출합니다.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="important-apis"></a>중요 API
* [concurrency:: task 클래스](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 인터페이스](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Syndicationclient:: Retrievefeedasync 메서드](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 클래스](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
* [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)