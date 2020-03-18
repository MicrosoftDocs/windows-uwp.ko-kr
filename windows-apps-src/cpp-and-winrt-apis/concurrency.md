---
description: C++/WinRT를 사용하여 Windows 런타임 비동기 개체를 만들고 사용하는 방법을 보여 줍니다.
title: C++/WinRT를 통한 동시성 및 비동기 작업
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 동시성, 비동기, 비동기, 비동기성
ms.localizationpriority: medium
ms.openlocfilehash: 949f8c407e0a49c87cbb45c01117a7e2e1525010
ms.sourcegitcommit: 5f22e596443ff4645ebf68626d8a4d275d8a865f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083181"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>C++/WinRT를 통한 동시성 및 비동기 작업

> [!IMPORTANT]
> 이 항목에서는 *코루틴* 및 `co_await`의 개념을 소개하며, UI 및 비 UI 애플리케이션 모두에서 사용하는 것이 좋습니다.  간단히 하기 위해 이 소개 항목의 코드 예제에서는 대부분 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트를 보여 줍니다. 이 항목의 뒷부분에 나오는 코드 예제에서는 코루틴을 사용하지만, 편의상 콘솔 애플리케이션 예제에서 종료 직전에 차단 **get** 함수 호출도 사용하므로 출력 인쇄를 마치기 전에 애플리케이션이 종료되지 않습니다. UI 스레드에서는 이 작업(차단 **get** 함수 호출)을 수행하지 않습니다. 대신 `co_await` 문을 사용합니다. UI 애플리케이션에서 사용하는 기술은 [고급 동시성 및 비동기](concurrency-2.md) 항목에서 설명하고 있습니다.

이 소개 항목에서는 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 통해 Windows 런타임 비동기 개체를 만들고 사용할 수 있는 몇 가지 방법을 보여 줍니다. 이 항목을 읽은 후, 특히 UI 애플리케이션에서 사용하는 기술에 대해서는 [고급 동시성 및 비동기](concurrency-2.md)도 참조하세요.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>비동기 작업 및 Windows 런타임 “비동기” 함수

완료하는 데 50밀리초 이상 걸릴 가능성이 높은 Windows 런타임 API는 비동기 함수(이름이 “Async”로 끝나는 함수)로 구현됩니다. 비동기 함수의 구현은 다른 스레드에서 작업을 시작하고, 비동기 작업을 나타내는 개체와 함께 즉시 반환합니다. 비동기 작업이 완료되면, 반환된 개체에 작업의 결과 값이 포함됩니다. **Windows::Foundation** Windows 런타임 네임스페이스에는 네 가지 유형의 비동기 작업 개체가 포함됩니다.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_),
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

각 비동기 작업 유형은 **winrt::Windows::Foundation** C++/WinRT 네임스페이스의 해당 유형에 프로젝션됩니다. C++/WinRT에는 내부 await 어댑터 구조체도 포함되어 있습니다. 직접 사용하지는 않지만, 해당 구조체 덕분에 `co_await` 문을 작성하여 이러한 비동기 작업 유형 중 하나를 반환하는 함수의 결과를 협조적으로 기다릴 수 있습니다. 또한 이러한 유형을 반환하는 고유한 코루틴을 작성할 수 있습니다.

비동기 Windows 함수의 예로 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 형식의 비동기 작업 개체를 반환하는 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)가 있습니다.

C++/WinRT를 사용하여 이러한 API를 호출하는 몇 가지 방법을 차단 방법과 비차단 방법 순으로 살펴보겠습니다. 기본적인 아이디어를 설명하기 위해 다음 몇 가지 코드 예제에서는 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트를 사용합니다. UI 애플리케이션에 더 적합한 기술은 [고급 동시성 및 비동기](concurrency-2.md)에서 설명하고 있습니다.

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

위의 코드 예제에서 볼 수 있듯이 **main**을 종료하기 직전에 차단 **get** 함수 호출을 계속 사용합니다. 그러나 이는 출력 인쇄를 마치기 전에 애플리케이션이 종료되지 않도록 하기 위한 것입니다.

## <a name="asynchronously-return-a-windows-runtime-type"></a>Windows 런타임 형식을 비동기식으로 반환

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

Windows 런타임 형식을 비동기 방식으로 반환하는 경우 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) 또는 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 반환해야 합니다. 자사 또는 타사 런타임 클래스는 Windows 런타임 함수로 전달하거나 전달받을 수 있는 형식(예: `int` 또는 **winrt::hstring**)을 한정합니다. Windows 런타임이 아닌 형식에 이러한 비동기 작업 유형 중 하나를 사용하려고 하면 컴파일러에서 "*WinRT 형식이어야 합니다.* " 오류를 생성하여 도와줍니다.

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

## <a name="asynchronously-return-a-non-windows-runtime-type"></a>Windows 런타임이 아닌 형식을 비동기식으로 반환

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

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

코루틴에서 실행은 첫 번째 일시 중단 지점까지 동기화됩니다. 이 경우 컨트롤이 호출자에 반환되고 호출하는 프레임이 범위를 벗어납니다. 코루틴이 다시 시작될 때까지 참조 매개 변수가 참조하는 소스 값이 변경되었을 수 있습니다. 코루틴의 관점에서 참조 매개 변수의 수명은 제어되지 않습니다. 따라서 위 예제에서 `co_await`까지는 ‘값’에 액세스해도 안전하지만 이후에는 안전하지 않습니다.  호출자에 의해 *값*이 소멸되는 이벤트에서 그 이후 코루틴 내의 해당 값에 액세스하려고 하면 메모리가 손상됩니다. 함수가 일시 중단되었다가 다시 시작된 후 ‘값’을 사용하려고 시도할 위험이 있는 경우 **DoOtherWorkAsync**에 ‘값’을 안전하게 전달할 수도 없습니다.  

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

## <a name="important-apis"></a>중요 API
* [concurrency::task 클래스](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 인터페이스](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 메서드](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 클래스](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>관련 항목
* [한층 향상한 동시성, 비동기 프로그래밍 기능](concurrency-2.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
* [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)