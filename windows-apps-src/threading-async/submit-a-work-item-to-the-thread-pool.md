---
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: 스레드 풀에 작업 항목 제출
description: 스레드 풀에 작업 항목을 제출하여 별도 스레드에서 작업하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스레드, 스레드 풀
ms.localizationpriority: medium
ms.openlocfilehash: 423f0efa9118f581d6e768a815dd2550801aa87e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658018"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>스레드 풀에 작업 항목 제출

\[ Windows 10의 UWP 앱을 업데이트합니다. Windows 8.x 아티클에 대 한 참조를 [보관](https://go.microsoft.com/fwlink/p/?linkid=619132) \]

<b>중요 한 Api</b>

-   [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)

스레드 풀에 작업 항목을 제출하여 별도 스레드에서 작업하는 방법을 알아봅니다. 스레드 풀을 사용하면 오랜 시간이 걸리는 작업을 완료하는 동시에 응답하는 UI를 유지하고 여러 작업을 병렬로 완료할 수 있습니다.

## <a name="create-and-submit-the-work-item"></a>작업 항목 만들기 및 제출

[  **RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593)를 호출하여 작업 항목을 만듭니다. 작업을 수행할 대리자를 제공합니다(람다 또는 대리자 함수를 사용할 수 있음). **RunAsync**는 [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580) 개체를 반환합니다. 다음 단계에서 사용하기 위해 이 개체를 저장합니다.

선택적으로 작업 항목의 우선 순위를 지정하고 다른 작업 항목과 동시에 실행할지 여부를 제어할 수 있도록 세 가지 버전의 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593)를 사용할 수 있습니다.

>[!NOTE]
>사용 하 여 [ **CoreDispatcher.RunAsync** ](https://msdn.microsoft.com/library/windows/apps/Hh750317) UI 스레드에 액세스 하 여 작업 항목에서 진행률을 표시 합니다.

다음 예제에서는 작업 항목을 만들고 작업을 수행할 람다를 제공합니다.

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
std::shared_ptr<unsigned long> nthPrime = make_shared<unsigned long int>(0);

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

[  **RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) 호출 뒤에 이 작업 항목은 스레드 풀에 의해 대기되고 스레드를 사용할 수 있게 되면 실행됩니다. 스레드 풀 작업 항목은 비동기적으로 실행되며 순서에 관계없이 실행될 수 있으므로 작업 항목이 독립적으로 작동하는지 확인합니다.

작업 항목은 [**IAsyncInfo.Status**](https://msdn.microsoft.com/library/windows/apps/BR206593) 속성을 검사하고, 작업 항목이 취소된 경우 종료됩니다.

## <a name="handle-work-item-completion"></a>작업 항목 완료 처리

작업 항목의 [**IAsyncAction.Completed**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.completed.aspx) 속성을 설정하여 완료 처리기를 제공합니다. 작업 항목 완료를 처리할 대리자를 제공합니다(람다 또는 대리자 함수를 사용할 수 있음). 예를 들어 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317)를 사용하여 UI 스레드에 액세스하고 결과를 표시합니다.

다음 예제에서는 1단계에서 제출된 작업 항목의 결과로 UI를 업데이트합니다.

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

완료 처리기는 UI 업데이트를 디스패치하기 전에 작업 항목이 취소되었는지 여부를 확인합니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이 빠른 시작에서 코드를 다운로드 하 여 자세히 알아볼 수 있습니다 합니다 [ThreadPool 작업 샘플 항목을 만드는](https://go.microsoft.com/fwlink/p/?LinkID=328569) 다시는 win에서 소스 코드를 사용 하 여 Windows 8.1 용으로 작성 된\_unap Windows 10 앱.

## <a name="related-topics"></a>관련 항목

* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [스레드 풀 사용 모범 사례](best-practices-for-using-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)
 
