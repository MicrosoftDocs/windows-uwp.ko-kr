---
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: 스레드 풀에 작업 항목 제출
description: 스레드 풀에 작업 항목을 전송 하 여 별도의 스레드에서 작업을 수행 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스레드, 스레드 풀
ms.localizationpriority: medium
ms.openlocfilehash: 3576f907e4ab601013d22fe9ae7697e0ec523ce4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155207"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>스레드 풀에 작업 항목 제출

\[ Windows 10의 UWP 앱 용으로 업데이트 되었습니다. Windows 8.x 문서는 [보관 파일](/previous-versions/windows/apps/mt244353(v=win.10)) 을 참조 하세요. \]

<b>중요 API</b>

-   [**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync)
-   [**IAsyncAction**](/uwp/api/Windows.Foundation.IAsyncAction)

스레드 풀에 작업 항목을 전송 하 여 별도의 스레드에서 작업을 수행 하는 방법에 대해 알아봅니다. 이를 사용 하 여 상당한 시간을 사용 하는 작업을 완료 하는 동안 응답성이 뛰어난 UI를 유지 관리 하 고이를 사용 하 여 여러 작업을 병렬로 완료할 수 있습니다.

## <a name="create-and-submit-the-work-item"></a>작업 항목 만들기 및 제출

[**Runasync**](/uwp/api/windows.system.threading.threadpool.runasync)를 호출 하 여 작업 항목을 만듭니다. 작업을 수행 하는 대리자를 제공 합니다. 람다 또는 대리자 함수를 사용할 수 있습니다. **Runasync** 는 [**iasyncaction**](/uwp/api/Windows.Foundation.IAsyncAction) 개체를 반환 합니다. 다음 단계에서 사용 하기 위해이 개체를 저장 합니다.

작업 항목의 우선 순위를 선택적으로 지정 하 고 다른 작업 항목과 동시에 실행 되는지 여부를 제어할 수 있도록 세 가지 버전의 [**Runasync**](/uwp/api/windows.system.threading.threadpool.runasync) 를 사용할 수 있습니다.

>[!NOTE]
>[**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) 를 사용 하 여 UI 스레드에 액세스 하 고 작업 항목에서 진행률을 표시 합니다.

다음 예제에서는 작업 항목을 만들고 작업을 수행 하기 위한 람다를 제공 합니다.

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = std::make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new Windows::System::Threading::WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

[**Runasync**](/uwp/api/windows.system.threading.threadpool.runasync)를 호출 하 고 나면 작업 항목이 스레드 풀에 의해 큐에 대기 되 고 스레드를 사용할 수 있게 되 면 실행 됩니다. 스레드 풀 작업 항목은 비동기적으로 실행 되며 순서에 관계 없이 작업 항목이 독립적으로 작동 하는지 확인 합니다.

작업 항목은 [**IAsyncInfo**](/uwp/api/windows.foundation.iasyncinfo.status) 속성을 확인 하 고 작업 항목이 취소 되 면 종료 됩니다.

## <a name="handle-work-item-completion"></a>작업 항목 완료 처리

작업 항목의 [**Iasyncaction. Completed**](/uwp/api/windows.foundation.iasyncaction.completed) 속성을 설정 하 여 완료 처리기를 제공 합니다. 대리자 (람다 또는 대리자 함수를 사용 하 여)를 제공 하 여 작업 항목 완료를 처리 합니다. 예를 들어 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) 를 사용 하 여 UI 스레드에 액세스 하 고 결과를 표시 합니다.

다음 예에서는 1 단계에서 제출한 작업 항목의 결과로 UI를 업데이트 합니다.

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

완료 처리기는 UI 업데이트를 발송 하기 전에 작업 항목이 취소 되었는지 여부를 확인 합니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

Windows 8.1 용으로 작성 된 [ThreadPool 작업 항목 만들기 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample) 에서이 빠른 시작의 코드를 다운로드 하 고 win \_ unap Windows 10 앱에서 소스 코드를 다시 사용 하 여 자세한 내용을 알아볼 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [스레드 풀을 사용하기 위한 모범 사례](best-practices-for-using-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)
 