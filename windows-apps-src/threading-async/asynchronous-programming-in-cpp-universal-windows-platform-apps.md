---
author: TylerMSFT
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: "이 문서에서는 ppltasks.h의 concurrency 네임스페이스에 정의된 task 클래스를 사용하여 Visual C++ 구성 요소 확장(C++/CX)의 비동기 메서드를 이용하는 권장 방법에 대해 설명합니다."
title: "C++의 비동기 프로그래밍"
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: 560b51d5bb67f5f2611311cb78f59d189d4ea440

---

# C++의 비동기 프로그래밍

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 문서에서는 ppltasks.h의 `concurrency` 네임스페이스에 정의된 `task` 클래스를 사용하여 Visual C++ 구성 요소 확장(C++/CX)의 비동기 메서드를 이용하는 권장 방법에 대해 설명합니다.

## UWP(유니버설 Windows 플랫폼) 비동기 형식

UWP(유니버설 Windows 플랫폼)는 비동기 메서드를 호출하기 위한 잘 정의된 모델의 역할을 하며 그러한 메서드를 사용하는 데 필요한 유형을 제공합니다. UWP 비동기 모델에 익숙하지 않은 경우 이 문서에 앞서 [비동기 프로그래밍][AsyncProgramming]을 읽어 보세요.

C++에서 직접 비동기 UWP API를 실행할 수도 있지만 [concurrency****][concurrencyNamespace] 네임스페이스에 포함되어 있고 `<ppltasks.h>`에 정의된 [**task class**][task-class] 및 관련 형식과 함수를 사용하는 것이 좋습니다. **concurrency::task** 클래스의 유형은 일반용이지만 **/ZW** 컴파일러 스위치(UWP(유니버설 Windows 플랫폼) 앱 및 구성 요소에 필요)가 사용될 경우에는 task 클래스가 UWP 비동기 유형을 캡슐화하여 다음 작업을 수행하기가 쉬워집니다.

-   여러 비동기 작업과 동기 작업을 함께 연결

-   작업 체인의 예외 처리

-   작업 체인에서 취소 수행

-   해당 스레드 컨텍스트 또는 아파트에서 개별 작업 실행

이 문서는 UWP 비동기 API와 함께 **task** 클래스를 사용하는 방법에 대한 기본 지침을 제공합니다. **task** 및 관련 메서드([**create\_task**][createTask] 포함)에 대한 전체 설명서는 [작업 병렬 처리(동시성 런타임)][taskParallelism]을 참조하세요. JavaScript 또는 기타 UWP 호환 언어에서 사용할 비동기 공용 메서드를 만드는 방법에 대한 자세한 내용은 [C++에서 Windows 런타임 앱에 대한 비동기 작업 만들기][createAsyncCpp]를 참조하세요.

## 작업을 통해 비동기 작업 사용

다음 예제에서는 task 클래스를 통해 [**IAsyncOperation**][IAsyncOperation] 인터페이스를 반환하고 그 작업이 값을 생성하는 **async** 메서드를 사용하는 방법을 보여 줍니다. 다음은 기본 단계입니다.

1.  `create_task` 메서드를 호출하여 **IAsyncOperation^** 개체에 전달합니다.

2.  작업에서 멤버 함수 [**task::then**][taskThen]을 호출하고, 비동기 작업이 완료되면 호출될 람다를 제공합니다.


``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices ) 
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

[
              **task::then**
            ]
            [taskThen] 함수에 의해 만들어지고 반환되는 작업을 *연속 작업*이라고 합니다. (이 경우) 사용자가 제공한 람다에 대한 입력 인수는 작업 연산이 완료될 때 생성하는 결과입니다. 이는 **IAsyncOperation** 인터페이스를 직접 사용하고 있다면 [**IAsyncOperation::GetResults**](https://msdn.microsoft.com/library/windows/apps/br206600)를 호출했을 때 검색되는 값과 동일합니다.

[
              **task::then**
            ]
            [taskThen] 메서드는 즉시 반환하며, 비동기 작업이 완료될 때까지 대리자는 실행되지 않습니다. 이 예제에서 비동기 작업이 예외를 발생시키거나, 취소 요청의 결과로서 취소된 상태로 종료될 경우에는 연속 작업이 실행되지 않습니다. 나중에 이전 작업이 취소 또는 실패한 경우에도 실행되는 연속 작업을 작성하는 방법에 대해 살펴보겠습니다.

로컬 스택에 task 변수를 선언하더라도 이 변수는 자신의 수명을 관리하므로 모든 작업이 완료되고 그에 대한 모든 참조가 범위를 벗어날 때까지 삭제되지 않습니다. 이는 작업이 완료되기 전에 메서드가 반환하는 경우에도 마찬가지입니다.

## 작업 체인 만들기

비동기 프로그래밍에서는 *작업 체인*이라고도 하는 작업 시퀀스를 정의하는 것이 일반적입니다. 각 연속 작업은 이전 작업이 완료된 경우에만 실행됩니다. 경우에 따라 이전(또는 *선행*) 작업은 연속 작업이 입력으로 받아들이는 값을 생성하기도 합니다. [
              **task::then**
            ]
            [taskThen] 메서드를 사용하면 직관적이고 간편한 방법으로 작업 체인을 만들 수 있습니다. 이 메서드는 **task<T>**를 반환하며 여기서 **T**는 람다 함수의 반환 형식입니다. 하나의 작업 체인에 여러 연속 작업을 작성할 수 있습니다(예: ). `myTask.then(…).then(…).then(…);`

작업 체인은 연속이 새로운 비동기 작업(operation)을 만들 때 특히 유용합니다. 그러한 작업을 비동기 작업(task)이라고 합니다. 다음 예제에서는 두 개의 연속 작업이 있는 작업 체인을 보여 줍니다. 초기 작업은 기존 파일에 대한 핸들을 얻고, 해당 작업이 완료되면 첫 번째 연속 작업이 새 비동기 작업을 시작하여 파일을 삭제합니다. 이 작업이 완료되면 두 번째 연속 작업이 실행되고 확인 메시지가 표시됩니다.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current::LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

앞의 예제에는 4가지 중요 사항이 있습니다.

-   첫 번째 연속 작업은 [**IAsyncAction^**][IAsyncAction] 개체를 **task<void>**로 변환하고 **task**를 반환합니다.

-   두 번째 연속 작업은 오류 처리를 수행하지 않으며, 따라서 입력으로 **task<void>**가 아닌 **void**를 사용합니다. 이것은 값 기반 연속 작업입니다.

-   두 번째 연속 작업은 [**DeleteAsync**][deleteAsync] 작업이 완료될 때까지 실행되지 않습니다.

-   두 번째 연속 작업은 값 기반이므로 [**DeleteAsync**][deleteAsync] 호출에 의해 시작된 작업이 예외를 발생시킬 경우 아예 실행되지 않습니다.

**참고** 작업 체인을 만드는 것은 **task** 클래스를 사용하여 비동기 작업을 구성하는 한 가지 방법일 뿐입니다. join 또는 choice 연산자  및 를 사용하여 작업을 구성할 수도 있습니다. 자세한 내용은 작업 병렬 처리(동시성 런타임)\]taskParallelism을 참조하세요.

## 람다 함수 반환 형식 및 작업 반환 형식

작업 연속 작업에서 람다 함수의 반환 형식은 **task** 개체로 묶여 있습니다. 람다가 **double**을 반환하면 연속 작업의 작업 유형은 **task<double>**가 됩니다. 그러나 작업 개체는 불필요하게 중첩된 반환 형식을 생성하지 않도록 설계되어 있습니다. 람다가 **IAsyncOperation&lt;SyndicationFeed^&gt;^**를 반환하는 경우 연속 작업은 **task&lt;task&lt;SyndicationFeed^&gt;&gt;** 또는 **task&lt;IAsyncOperation&lt;SyndicationFeed^&gt;^&gt;^**가 아닌 **task&lt;SyndicationFeed^&gt;**를 반환합니다. 이 프로세스는 *비동기 래핑 해제*라고 하며 다음 연속 작업이 호출되기 전에 연속 작업 내부의 비동기 작업이 완료되었는지도 확인합니다.

앞 예제에서 람다가 [**IAsyncInfo**][IAsyncInfo] 개체를 반환했음에도 불구하고 작업이 **task<void>**를 반환하는 것에 주목하세요. 다음 표에는 람다 함수와 바깥쪽 작업 간에 발생하는 유형 변환에 대한 요약이 나와 있습니다.

| | |
|--------------------------------------------------------|---------------------|
| 람다 반환 형식                                     | `.then` 반환 형식 |
| TResult                                                | 작업<TResult> |
| IAsyncOperation<TResult>^                        | 작업<TResult> |
| IAsyncOperationWithProgress&lt;TResult, TProgress&gt;^ | 작업<TResult> |
|IAsyncAction^                                           | 작업<void>    |
| IAsyncActionWithProgress<TProgress>^             |작업<void>     |
| 작업<TResult>                                    |작업<TResult>  |


## 작업 취소

사용자에게 비동기 작업을 취소할 수 있는 옵션을 제공하는 것이 좋습니다. 경우에 따라서는 작업 체인의 외부에서 프로그래밍 방식으로 작업을 취소해야 할 수 있습니다. 각 \***Async** 반환 형식에 [**IAsyncInfo**][IAsyncInfo]에서 상속하는 [**Cancel**][IAsyncInfoCancel] 메서드가 있더라도 이를 메서드 외부에 표시하는 것은 권장되지 않습니다. 작업 체인에서 취소를 지원하는 좋은 방법은 [**cancellation\_token\_source**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh749985.aspx)를 사용하여 [**cancellation\_token**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh749975.aspx)을 만든 다음 이 토큰을 초기 작업의 생성자로 전달하는 것입니다. 비동기 작업이 취소 토큰을 사용하여 만들어졌고 [**cancellation_token_source::cancel**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750076.aspx)이 호출되는 경우 작업은 자동으로 **IAsync\*** 작업에서 **Cancel**을 호출하고 취소 요청을 연속 작업 체인에 전달합니다. 다음 의사 코드에서는 기본 접근 방법을 보여 줍니다.

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName), 
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

작업이 취소되면 [**task\_canceled**][taskCanceled] 예외가 작업 체인으로 하향 전파됩니다. 단지 값 기반 연속 작업이 실행되지 않을 뿐이지만 [**task::get**][taskGet]이 호출될 경우 작업 기반 연속 작업에서 예외가 발생합니다. 오류 처리 연속 작업이 있으면 **task\_canceled** 예외를 명시적으로 catch하는지 확인하세요(이 예외는 [
              **Platform::Exception**
            ](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh755825.aspx)에서 파생되지 않음).

취소는 공동 작업입니다. 연속 작업에 단지 UWP 메서드를 호출하는 것 이상으로 장기 실행되는 일부 작업이 있을 경우 사용자 스스로 취소 토큰을 정기적으로 확인하고 취소된 토큰이 있으면 실행을 중지해야 합니다. 연속 작업에 할당된 모든 리소스를 정리한 후 [**cancel\_current\_task**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh749945.aspx)를 호출하여 해당 작업을 취소하고 값 기반의 후속 연속 작업에 취소를 하향 전파합니다. [
              **FileSavePicker**
            ](https://msdn.microsoft.com/library/windows/apps/BR207871) 작업의 결과를 나타내는 작업 체인을 만들 수 있는 다른 예를 살펴봅시다. 사용자가 **취소** 단추를 선택하면 [**IAsyncInfo::Cancel**][IAsyncInfoCancel] 메서드가 호출되지 않습니다. 작업은 계속되는 대신 **nullptr**을 반환합니다. 연속 작업은 입력 매개 변수를 테스트하고 입력이 **nullptr**이면 **cancel\_current\_task**를 호출합니다.

자세한 내용은 [PPL에서의 취소](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dd984117.aspx)를 참조하세요.

## 작업 체인의 오류 처리

선행 항목이 취소되었거나 예외가 발생한 경우에도 연속 작업을 실행하려면 선행 작업의 람다가 [**IAsyncAction^**][IAsyncAction]을 반환하는 경우의 람다 함수에 대한 입력을 **task<TResult>** 또는 **task<void>**로 지정하여 작업 기반 연속 작업으로 만듭니다.

작업 체인에서 오류 및 취소를 처리하기 위해 모든 연속 작업을 작업 기반으로 지정하거나, `try…catch` 블록 내에서 발생할 수 있는 각 작업을 묶을 필요는 없습니다. 대신 체인의 끝에 작업 기반 연속 작업을 추가하여 거기서 모든 오류를 처리할 수 있습니다. 예외([**task\_canceled**][taskCanceled] 예외 포함)는 작업 체인으로 하향 전파되고 값 기반 연속 작업을 무시하므로 오류 처리 작업 기반 연속 작업에서 이를 처리할 수 있습니다. 오류 처리 작업 기반 연속 작업을 사용하도록 앞의 예제를 다시 작성할 수 있습니다.

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t) 
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

작업 기반 연속 작업에서 멤버 함수 [**task::get**][taskGet]을 호출하여 작업 결과를 얻습니다. 작업이 결과를 생성하지 않는 [**IAsyncAction**][IAsyncAction]이었다고 하더라도 **task::get**을 호출해야 합니다. **task::get**도 작업으로 전송된 예외를 가져오기 때문입니다. 입력 작업이 예외를 저장하는 경우 **task::get** 호출 시에 해당 예외가 발생합니다. **task::get**을 호출하지 않거나, 체인의 끝에 작업 기반 연속 작업을 사용하지 않거나, 발생한 예외 유형을 catch하지 않으면 작업에 대한 모든 참조가 삭제되었을 때 **unobserved\_task\_exception**이 발생합니다.

처리할 수 있는 예외만 catch해야 합니다. 앱에 복구할 수 없는 오류가 발생하면 알 수 없는 상태에서 앱이 계속 실행되도록 하는 것보다 손상된 상태로 두는 것이 낫습니다. 또한 일반적으로 **unobserved\_task\_exception** 자체는 catch하려고 하지 마세요. 이 예외는 주로 진단용으로 사용됩니다. **unobserved\_task\_exception**이 발생하면 대개 코드에 버그가 있다는 뜻입니다. 주요 원인은 처리해야 할 예외 또는 코드에서 다른 오류로 인해 발생한 복구할 수 없는 예외 때문입니다.

## 스레드 컨텍스트 관리

UWP 앱의 UI는 STA(단일 스레드 아파트)에서 실행됩니다. 람다가 [**IAsyncAction**][IAsyncAction] 또는 [**IAsyncOperation**][IAsyncOperation]을 반환하는 작업은 아파트를 인식합니다. 사용자가 달리 지정하지 않는 한, 작업이 STA에서 만들어지면 작업의 모든 연속 작업 또한 기본적으로 STA에서 실행됩니다. 이는 전체 작업 체인이 부모 작업에서 아파트 인식을 상속한다는 의미입니다. 따라서 STA에서만 액세스할 수 있는 UI 컨트롤과의 조작 방식이 간소화됩니다.

UWP 앱을 예로 들면, XAML 페이지를 나타내는 클래스의 멤버 함수에서 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 개체를 사용하지 않고 [**task::then**][taskThen] 메서드 내에서 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 컨트롤을 채울 수 있습니다.

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed) 
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

[
              **IAsyncAction**
            ]
            [IAsyncAction] 또는 [**IAsyncOperation**][IAsyncOperation]을 반환하지 않는 작업은 아파트 인식 작업이 아니며, 기본적으로 해당 작업의 연속 작업은 사용 가능한 첫 백그라운드 스레드에서 실행됩니다.

[
              **task\_continuation\_context**
            ](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh749968.aspx)를 가져오는 [**task::then**][taskThen]의 오버로드를 사용하면 두 유형의 작업 모두에 대해 기본 스레드 컨텍스트를 무시할 수 있습니다. 예를 들어, 경우에 따라서는 백그라운드 스레드에서 아파트 인식 작업의 연속 작업을 예약하는 것이 좋을 수 있습니다. 이 경우 [**task\_continuation\_context::use\_arbitrary**][useArbitrary]를 전달하여 다중 스레드 아파트에서 사용 가능한 다음 스레드에서 작업을 예약할 수 있습니다. 이렇게 하면 해당 작업을 UI 스레드에서 발생하는 다른 작업과 동기화할 필요가 없으므로 연속 작업의 성능이 향상될 수 있습니다.

다음 예제는 [**task\_continuation\_context::use\_arbitrary**][useArbitrary] 옵션을 지정하는 것이 언제 유용한지, 그리고 기본 연속 작업 컨텍스트가 스레드로부터 안전하지 않은 컬렉션에서 동시 작업을 동기화하는 데 있어 어떻게 유용한지를 보여 줍니다. 이 코드에서는 RSS 피드 및 각 URL에 대해 URL 목록 전체를 반복하고, 피드 데이터를 검색하기 위해 비동기 작업을 시작합니다. 피드가 검색되는 순서는 제어할 수 없지만 이는 그다지 중요하지 않습니다. 각 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR210642) 작업이 완료되면 첫 번째 연속 작업이 [**SyndicationFeed^**](https://msdn.microsoft.com/library/windows/apps/BR243485) 개체를 받은 다음 이를 사용하여 앱에서 정의된 `FeedData^` 개체를 초기화합니다. 이러한 각각의 작업은 상호 독립적이므로 **task\_continuation\_context::use\_arbitrary** 연속 작업 컨텍스트를 지정하여 잠재적으로 속도를 높일 수 있습니다. 그러나 각 `FeedData` 개체가 초기화된 후에는 스레드로부터 안전한 컬렉션이 아닌 [**Vector**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)에 추가해야 합니다. 따라서 연속 작업을 만들고 [**task\_continuation\_context::use\_current**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750085.aspx)를 지정하여 동일한 ASTA(응용 프로그램 단일 스레드 아파트) 컨텍스트에서 모든 [**Append**](https://msdn.microsoft.com/library/windows/apps/BR206632) 호출이 발생하게 합니다. [
              **task\_continuation\_context::use\_default**
            ](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750085.aspx)는 기본 컨텍스트이므로 명시적으로 지정할 필요가 없지만 여기서는 명확히 하기 위해 지정합니다.

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an 
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append 
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

중첩 작업, 즉 연속 작업 내부에 만들어진 새 작업은 초기 작업의 아파트 인식을 상속하지 않습니다.

## 진행 상황 업데이트 처리

[
              **IAsyncOperationWithProgress**
            ](https://msdn.microsoft.com/library/windows/apps/BR206594) 또는 [**IAsyncActionWithProgress**](https://msdn.microsoft.com/library/windows/apps/BR206580withprogress_1)를 지원하는 메서드는 작업이 완료될 때까지 진행 상황을 정기적으로 업데이트합니다. 진행 상황 보고는 작업 및 연속 작업의 개념과는 별개입니다. 개체의 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br206594) 속성에 대한 대리자를 제공하면 되며, 대리자 사용의 전형적인 예는 UI에서 진행률 표시줄을 업데이트하는 것입니다.

## 관련 항목

* [C++에서 Windows 스토어 앱에 대한 비동기 작업 만들기]
            [createAsyncCpp]
* [Visual C++ 언어 참조](http://msdn.microsoft.com/library/windows/apps/hh699871.aspx)
* [비동기 프로그래밍]
            [AsyncProgramming]
* [작업 병렬 처리(동시성 런타임)]
            [taskParallelism]
* [task 클래스]
            [task-class]
 
<!-- LINKS -->
[AsyncProgramming]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh464924.aspx>
             "AsyncProgramming"
[concurrencyNamespace]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dd492819.aspx>
             "동시성 네임스페이스"
[createTask]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh913025.aspx>
             "CreateTask"
[createAsyncCpp]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750082.aspx>
             "CreateAsync"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199>
             "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/BR206580>
             "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598>
             "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587>
             "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel>
             "IAsyncInfoCancel"
[taskCanceled]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750106.aspx>
             "TaskCancelled"
[task-class]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750113.aspx>
             "Task 클래스"
[taskGet]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750017.aspx>
             "TaskGet"
[taskParallelism]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dd492427.aspx>
             "작업 병렬 처리"
[taskThen]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750044.aspx>
             "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750036.aspx>
             "UseArbitrary"



<!--HONumber=Jun16_HO3-->


