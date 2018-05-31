---
author: stevewhims
description: 이 항목에서는 C++/WinRT를 통해 Windows 런타임 비동기 개체를 생성하고 사용하는 방법에 대해서 설명합니다.
title: C++/WinRT으로 동시성 및 비동기 작업
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 동시성, 비동기, 비동기식, 비동기성
ms.localizationpriority: medium
ms.openlocfilehash: 3af9125abc3abf41327f5b49e6a05d81e214f89f
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831837"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)으로 동시성 및 비동기 작업
이 항목에서는 C++/WinRT를 통해 Windows 런타임 비동기 개체를 생성하고 사용하는 방법에 대해서 설명합니다.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>비동기 작업 및 Windows 런타임 "비동기" 함수
완료하는 데 50밀리초 이상 걸릴 가능성이 높은 Windows 런타임 API는 비동기 함수(이름이 "Async"로 끝나는 함수)로 구현됩니다. 비동기 함수의 구현체는 다른 스레드에서 작업을 시작하고 비동기 작업을 나타내는 개체를 즉시 반환합니다. 비동기 작업을 마치면 반환된 개체에 작업의 결과 값이 포함됩니다. **Windows::Foundation** Windows 런타임 네임스페이스에는 다음과 같이 4가지 형식의 비동기 작업 개체가 포함됩니다.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

각 비동기 작업 형식은 **winrt::Windows::Foundation** C++/WinRT 네임스페이스에서 해당하는 형식으로 프로젝션됩니다. C++/WinRT에는 내부의 await 어댑터 구조체도 포함됩니다. 직접 사용하지는 않지만 이 구조체 덕분에 **co_await** 문을 작성하여 비동기 작업 형식 중 하나를 반환하는 함수의 결과를 협조적으로 기다릴 수 있습니다. 또한 이러한 형식을 반환하는 사용자 고유의 코루틴을 작성하는 것도 가능합니다.

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

**get**을 호출하면 코딩 작업이 편리하지만 협조적이지는 않습니다. 동시성이나 비동기성도 없기 때문입니다. 따라서 OS 스레드 정체로 인해 다른 유용한 작업까지 못하는 경우를 방지하려면 다른 기법이 필요합니다.

## <a name="write-a-coroutine"></a>코루틴 작성
C++/WinRT는 C++ 코루틴을 프로그래밍 모델에 통합하여 결과를 협조적으로 기다릴 수 있는 자연스러운 방법을 제공합니다. 사용자는 코루틴을 작성하여 고유의 Windows 런타임 비동기 작업을 생성할 수 있습니다. 아래 코드 예제에서는 **ProcessFeedAsync**가 코루틴입니다.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = co_await syndicationClient.RetrieveFeedAsync(rssFeedUri);
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

코루틴이란 일시 중단했다가 다시 시작할 수 있는 함수를 말합니다. 위의 **ProcessFeedAsync** 코루틴에서는 **co_await** 문에 이르면 코루틴이 비동기 방식으로 **RetrieveFeedAsync** 호출을 시작했다가 바로 스스로를 일시 중단하고 컨트롤을 호출자(위 예제에서 **main**)에게 반환합니다. 그러면 **main**이 피드를 가져와 출력하는 동안 작업을 계속 할 수 있습니다. 이 과정이 모두 끝나면(**RetrieveFeedAsync** 호출 완료 시) **ProcessFeedAsync** 코루틴이 다음 문으로 다시 시작합니다.

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

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
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

Windows 런타임 형식을 비동기 방식으로 반환하는 경우에는(자사 형식이든 타사 형식이든 상관없이) [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) 또는 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 반환해야 합니다.

Windows 런타임 이외의 형식으로 이러한 비동기 작업 형식 중 하나를 사용하려고 할 때 발생하는 "*must be WinRT type*" 오류 메시지는 컴파일러를 사용해 해결할 수 있습니다.

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Windows가 아닌 런타임 형식의 비동기 반환
Windows 런타임 *이외의* 형식을 비동기 방식으로 반환하는 경우에는 병렬 패턴 라이브러리(PPL)인 [**task**](https://msdn.microsoft.com/library/hh750113)를 반환해야 합니다. **task**를 권장하는 이유는 **std::future**보다 성능이 뛰어나고 향후 호환성도 우수하기 때문입니다.

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
    // do other work.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="important-apis"></a>중요 API
* [concurrency::task](https://msdn.microsoft.com/library/hh750113)
* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
