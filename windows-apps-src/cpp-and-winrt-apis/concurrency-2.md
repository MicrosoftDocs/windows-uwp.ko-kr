---
description: C++/WinRT로 동시성, 비동기 작업을 모두 수행하는 고급 시나리오.
title: C++/WinRT를 통한 고급 동시성 및 비동기
ms.date: 07/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 동시성, 비동기, 비동기, 비동기성
ms.localizationpriority: medium
ms.openlocfilehash: 1170b8e1291afd166f210feb291b644d1c7ed546
ms.sourcegitcommit: e5a154c7b6c1b236943738febdb17a4815853de5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71164818"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>C++/WinRT를 통한 고급 동시성 및 비동기

이 항목에서는 C++/WinRT에서 동시성 및 비동기를 사용하는 고급 시나리오에 대해 설명합니다.

이 주제에 대한 소개는 먼저 [동시성 및 비동기 작업](concurrency.md)을 참조하세요.

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
using namespace std::chrono_literals;
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

## <a name="a-deeper-dive-into-winrtresume_foreground"></a>**winrt::resume_foreground**에 대한 심층 분석

[C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20)부터 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 함수는 디스패처 스레드에서 호출되더라도 일시 중단됩니다(이전 버전에서는 아직 디스패처 스레드에 없는 경우에만 일시 중단되므로 일부 시나리오에서 교착 상태가 발생할 수 있음).

현재 동작은 스택 해제 및 큐에 다시 넣기를 수행할 수 있음을 의미하며, 특히 낮은 시스템 코드 수준의 시스템 안정성에 매우 중요합니다. 위의 [스레드 선호도를 고려한 프로그래밍](#programming-with-thread-affinity-in-mind) 섹션의 마지막 코드 목록은 백그라운드 스레드에서 복잡한 계산을 수행한 다음, UI(사용자 인터페이스)를 업데이트하기 위해 적절한 UI 스레드로 전환하는 것을 보여 줍니다.

**winrt::resume_foreground**가 내부적으로 표시되는 방법은 다음과 같습니다.

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

현재와 이전의 이 동작은 Win32 애플리케이션 개발에서 [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew)와 [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) 사이의 차이와 비슷합니다. **PostMessage**는 작업을 큐에 넣은 다음, 작업이 완료될 때까지 기다리지 않고 스택을 해제합니다. 스택 해제는 필수적일 수 있습니다.

**winrt::resume_foreground** 함수도 처음에는 Windows 10 이전에 도입된 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher)([**CoreWindow**](/uwp/api/windows.ui.core.corewindow)에 연결됨)만 지원했습니다. 그 이후 더 유연하고 효율적인 [**DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue) 디스패처를 도입했습니다. **DispatcherQueue**는 사용자 고유의 용도에 맞게 만들 수 있습니다. 다음과 같은 간단한 콘솔 애플리케이션을 살펴보겠습니다.

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

위의 예제에서는 큐(컨트롤러 내에 포함되어 있음)를 프라이빗 스레드에 만든 다음, 해당 컨트롤러를 코루틴에 전달합니다. 코루틴은 큐를 사용하여 프라이빗 스레드에서 대기(일시 중단 및 다시 시작)할 수 있습니다. **DispatcherQueue**의 또 다른 일반적인 용도는 큐를 기존 데스크톱 또는 Win32 앱의 현재 UI 스레드에 만드는 것입니다.

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

여기서는 Win32 스타일의 [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller) 함수를 호출하여 컨트롤러를 만든 다음, 결과 큐 컨트롤러의 소유권을 WinRT 개체로 호출자에 이전함으로써 Win32 함수를 호출하여 C++/WinRT 프로젝트에 통합하는 방법을 보여 줍니다. 또한 이를 통해 기존 Petzold 스타일의 Win32 데스크톱 애플리케이션에서 효율적이고 원활한 큐를 지원할 수 있습니다.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

위의 간단한 **main** 함수는 창을 만드는 것으로 시작합니다. 이 경우 창 클래스를 등록하고 [**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww)를 호출하여 최상위 데스크톱 창을 만드는 것으로 생각할 수 있습니다. 다음으로, **CreateDispatcherQueueController** 함수를 호출하여 큐 컨트롤러를 만든 후에 이 컨트롤러에서 소유한 디스패처 큐를 사용하여 일부 코루틴을 호출합니다. 그런 다음, 일반적인 메시지 펌프가 입력되어 이 스레드에서 코루틴을 자연스럽게 다시 시작합니다. 이렇게 하면 애플리케이션 내의 비동기 또는 메시지 기반 워크플로에 대한 세련된 코루틴 세계로 돌아갈 수 있습니다.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

**winrt::resume_foreground**에 대한 호출은 항상 *큐*에 대기한 다음, 스택을 해제합니다. 필요에 따라 재시작 우선 순위를 설정할 수도 있습니다.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

또는 기본 큐 순서를 사용합니다.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

또는 이 경우 큐 종료를 검색하여 정상적으로 처리합니다.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

`co_await` 식에서 `true`를 반환하며, 이는 디스패처 스레드에서 다시 시작됨을 나타냅니다. 즉 해당 큐가 성공적으로 완료되었습니다. 반대로, 큐 컨트롤러가 종료되고 큐 요청을 더 이상 처리하지 않으므로 실행이 호출 스레드에 남아 있음을 나타내기 위해 `false`를 반환합니다.

따라서 C++/WinRT를 코루틴과 결합하는 경우, 특히 오래된 Petzold 스타일의 데스크톱 애플리케이션 개발을 수행할 때 다양한 기능을 손쉽게 이용할 수 있습니다.

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

첫 번째 인수(*sender*)는 사용하지 않으므로 명명되지 않은 그대로입니다. 따라서 이 인수는 참조로 두어도 안전합니다. 하지만 *args*는 값으로 전달됩니다. 위의 [매개 변수 전달](concurrency.md#parameter-passing) 섹션을 참조하세요.

## <a name="awaiting-a-kernel-handle"></a>커널 핸들 대기

C++/WinRT는 커널 이벤트에서 신호를 받을 때까지 일시 중단하는 데 사용할 수 있는 **resume_on_signal** 클래스를 제공합니다. `co_await resume_on_signal(h)`이 반환될 때까지 핸들이 계속 유효하게 유지되는지 확인해야 합니다. 이 첫 번째 예제와 같이 **resume_on_signal**이 시작되기도 전에 핸들이 손실되었을 수 있으므로 **resume_on_signal** 자체는 이 작업을 수행할 수 없습니다.

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

들어오는 **HANDLE**은 함수가 반환될 때까지 유효하며, 이 함수(코루틴)는 첫 번째 일시 중단 지점(이 경우 첫 번째 `co_await`)에서 반환됩니다. **DoWorkAsync**를 기다리는 동안 컨트롤이 호출자에 반환되고, 호출하는 프레임이 범위를 벗어났으며, 코루틴이 다시 시작될 때 핸들이 유효한지 여부를 더 이상 알 수 없습니다.

기술적으로 코루틴은 매개 변수를 값으로 받고 있습니다(위의 [매개 변수 전달](concurrency.md#parameter-passing) 참조). 그러나 이 경우 해당 지침의 *정신*(단지 문자만이 아닌)을 따르기 위해 한 단계 더 나아가야 합니다. 핸들과 함께 강한 참조(즉, 소유권)를 전달해야 합니다. 다음과 같이 하세요.

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* not valid here.
}
```

값을 기준으로 [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle)을 전달하면 소유권 의미 체계가 제공되므로 커널 핸들이 코루틴의 수명 동안 유효하게 유지됩니다.

이 코루틴을 호출하는 방법은 다음과 같습니다.

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

## <a name="asynchronous-timeouts-made-easy"></a>간편한 비동기 시간 제한

C++/WinRT는 C++ 코루틴에 많이 투자됩니다. 동시성 코드 작성에 미치는 영향력은 다양합니다. 이 섹션에서는 비동기에 대한 세부 정보가 중요하지 않은 경우에 대해 설명하며, 원하는 것은 결과뿐입니다. 이러한 이유로 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) Windows 런타임 비동기 작업 인터페이스에 대한 C++/WinRT의 구현에는 **std::function**에서 제공하는 것과 비슷한 **get** 함수가 있습니다.

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

비동기 개체가 완료되는 동안 **get** 함수는 무기한 차단됩니다. 비동기 개체는 수명이 매우 짧으므로 필요한 경우가 많습니다.

그러나 이것만으로 충분하지 않은 경우가 있으며, 시간이 좀 경과되면 대기를 중단해야 합니다. 이 코드는 언제든지 Windows 런타임에서 제공하는 구성 요소를 통해 작성할 수 있었습니다. 그러나 이제 C++/WinRT를 사용하면 **wait_for** 함수를 제공하여 훨씬 쉽게 처리할 수 있습니다. 또한 **IAsyncAction**에서도 구현되며, **std::function**에서 제공하는 것과 비슷합니다.

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

> [!NOTE]
> **wait_for** 함수는 인터페이스에서 **std::chrono::duration**을 사용하지만, **std::chrono::duration**이 제공하는 범위보다 작은 범위로 제한됩니다(약 49.7일).

다음 예제의 **wait_for**는 약 5초 동안 기다린 후에 완료를 확인합니다. 비교가 양호하면 비동기 개체가 성공적으로 완료되었음을 알 수 있습니다. 일부 결과를 기다리는 경우 **get** 함수를 호출하여 결과를 검색하기만 하면 됩니다.

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

비동기 개체가 그때까지 완료되었으므로 **get**은 더 이상 기다리지 않고 결과를 즉시 반환합니다. 여기서 볼 수 있듯이 **wait_for**는 비동기 개체의 상태를 반환합니다. 따라서 이 상태는 다음과 같이 더 세분화된 제어에 사용할 수 있습니다.

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- **AsyncStatus::Completed**는 비동기 개체가 성공적으로 완료되었음을 의미하며, **get** 함수를 호출하여 결과를 검색할 수 있습니다.
- **AsyncStatus::Canceled**는 비동기 개체가 취소되었음을 의미합니다. 취소는 일반적으로 호출자가 요청하므로 이 상태를 처리하는 경우는 거의 없습니다. 일반적으로 취소된 비동기 개체는 간단히 삭제됩니다.
- **AsyncStatus::Error**는 비동기 개체가 어떤 방법으로든 실패했음을 의미합니다. 원하는 경우 예외를 다시 throw하기 위해 **get**을 수행할 수 있습니다.
- **AsyncStatus::Started**는 비동기 개체가 아직도 실행되고 있음을 의미합니다. Windows 런타임 비동기 패턴은 여러 대기와 대기자를 허용하지 않습니다. 즉 루프에서 **wait_for**를 호출할 수 없습니다. 대기 시간이 효과적으로 초과되면 몇 가지 선택 사항이 있습니다. 개체를 중단하거나, **get**을 호출하기 전에 해당 상태를 폴링하여 결과를 검색할 수 있습니다. 그러나 이 시점에서 개체를 삭제하는 것이 가장 좋습니다.

## <a name="important-apis"></a>중요 API
* [IAsyncAction 인터페이스](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 인터페이스](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 메서드](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>관련 항목
* [동시성 및 비동기 작업](concurrency.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)