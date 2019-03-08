---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: 이 문서에서는 Visual c + + 구성 요소 확장의 비동기 메서드를 사용 하는 권장된 방법에 설명 합니다 (C + + /cli CX) ppltasks.h의 concurrency 네임 스페이스에 정의 된 작업 클래스를 사용 하 여 합니다.
title: C++의 비동기 프로그래밍
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, 스레드, 비동기, C++
ms.localizationpriority: medium
ms.openlocfilehash: beab78415ab36fc7bc0659af1b3466b2c3601d88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662838"
---
# <a name="asynchronous-programming-in-ccx"></a>C++/CX의 비동기 프로그래밍
> [!NOTE]
> 이 항목은 C++/CX 응용 프로그램 유지에 도움을 주기 위해 작성되었습니다. 하지만 새로운 응용 프로그램에 대해 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 것이 좋습니다. C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일 기반 라이브러리로 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다.

이 문서에서는 Visual c + + 구성 요소 확장의 비동기 메서드를 사용 하는 권장된 방법에 설명 합니다 (C + + CX)를 사용 하 여는 `task` 에 정의 된 클래스는 `concurrency` ppltasks.h의 네임 스페이스입니다.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>UWP(유니버설 Windows 플랫폼) 비동기 형식
UWP(유니버설 Windows 플랫폼)는 비동기 메서드를 호출하기 위한 잘 정의된 모델의 역할을 하며 그러한 메서드를 사용하는 데 필요한 유형을 제공합니다. UWP 비동기 모델에 익숙하지 않을 경우 이 문서의 나머지 부분을 읽기 전에 [비동기 프로그래밍][AsyncProgramming]을 읽으세요.

C++에서 직접 비동기 UWP API를 실행할 수도 있지만 [**concurrency**][concurrencyNamespace] 네임스페이스에 포함되어 있고 `<ppltasks.h>`에 정의된 [**task 클래스**][task-class] 및 관련 형식과 함수를 사용하는 것이 좋습니다. **concurrency::task** 클래스의 유형은 일반용이지만 **/ZW** 컴파일러 스위치(UWP(유니버설 Windows 플랫폼) 앱 및 구성 요소에 필요)가 사용될 경우에는 task 클래스가 UWP 비동기 유형을 캡슐화하여 다음 작업을 수행하기가 쉬워집니다.

-   여러 비동기 작업과 동기 작업을 함께 연결

-   작업 체인의 예외 처리

-   작업 체인에서 취소 수행

-   해당 스레드 컨텍스트 또는 아파트에서 개별 작업 실행

이 문서는 UWP 비동기 API와 함께 **task** 클래스를 사용하는 방법에 대한 기본 지침을 제공합니다. 자세한 설명서에 대 한 완료 **작업** 및 해당 관련된 메서드를 포함 하 여 [ **만들기\_태스크**][createTask], 를참조하세요.[ 작업 병렬 처리 (동시성 런타임)][taskParallelism]합니다. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>작업을 통해 비동기 작업 사용
다음 예제에서는 task 클래스를 통해 [**IAsyncOperation**][IAsyncOperation] 인터페이스를 반환하고 해당 작업이 값을 생성하는 **async** 메서드를 사용하는 방법을 보여 줍니다. 다음은 기본 단계입니다.

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

[  **task::then**][taskThen] 함수에 의해 만들어지고 반환되는 작업을 *연속 작업*이라고 합니다. (이 경우) 사용자가 제공한 람다에 대한 입력 인수는 작업 연산이 완료될 때 생성하는 결과입니다. 이는 **IAsyncOperation** 인터페이스를 직접 사용하고 있다면 [**IAsyncOperation::GetResults**](https://msdn.microsoft.com/library/windows/apps/br206600)를 호출했을 때 검색되는 값과 동일합니다.

[  **task::then**][taskThen] 메서드는 즉시 반환하며, 비동기 작업이 완료될 때까지 대리자는 실행되지 않습니다. 이 예제에서 비동기 작업이 예외를 발생시키거나, 취소 요청의 결과로서 취소된 상태로 종료될 경우에는 연속 작업이 실행되지 않습니다. 나중에 이전 작업이 취소 또는 실패한 경우에도 실행되는 연속 작업을 작성하는 방법에 대해 살펴보겠습니다.

로컬 스택에 task 변수를 선언하더라도 이 변수는 자신의 수명을 관리하므로 모든 작업이 완료되고 그에 대한 모든 참조가 범위를 벗어날 때까지 삭제되지 않습니다. 이는 작업이 완료되기 전에 메서드가 반환하는 경우에도 마찬가지입니다.

## <a name="creating-a-chain-of-tasks"></a>작업 체인 만들기
비동기 프로그래밍에서는 *작업 체인*이라고도 하는 작업 시퀀스를 정의하는 것이 일반적입니다. 각 연속 작업은 이전 작업이 완료된 경우에만 실행됩니다. 경우에 따라 이전(또는 *선행*) 작업은 연속 작업이 입력으로 받아들이는 값을 생성하기도 합니다. [  **task::then**][taskThen] 메서드를 사용하면 직관적이고 간편한 방법으로 작업 체인을 만들 수 있습니다. 이 메서드는 **task<T>** 를 반환하며 여기에서 **T**는 람다 함수의 반환 형식입니다. 여러 연속 작업 체인을 작성할 수 있습니다. `myTask.then(…).then(…).then(…);`

작업 체인은 연속이 새로운 비동기 작업(operation)을 만들 때 특히 유용합니다. 그러한 작업을 비동기 작업(task)이라고 합니다. 다음 예제에서는 두 개의 연속 작업이 있는 작업 체인을 보여 줍니다. 초기 작업은 기존 파일에 대한 핸들을 얻고, 해당 작업이 완료되면 첫 번째 연속 작업이 새 비동기 작업을 시작하여 파일을 삭제합니다. 이 작업이 완료되면 두 번째 연속 작업이 실행되고 확인 메시지가 표시됩니다.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

앞의 예제에는 4가지 중요 사항이 있습니다.

-   첫 번째 연속 작업은 [**IAsyncAction^**][IAsyncAction] 개체를 **task<void>** 로 변환하고 **task**를 반환합니다.

-   두 번째 연속 작업은 오류 처리를 수행하지 않으며, 따라서 입력으로 **task<void>** 가 아닌 **void**를 사용합니다. 이것은 값 기반 연속 작업입니다.

-   두 번째 연속 작업은 [**DeleteAsync**][deleteAsync] 작업이 완료될 때까지 실행되지 않습니다.

-   두 번째 연속 작업은 값 기반이므로 [**DeleteAsync**][deleteAsync] 호출에 의해 시작된 작업이 예외를 발생시킬 경우 아예 실행되지 않습니다.

**참고**  은 작업 체인을 만들어 사용 하는 방법 중 하나는 **태스크** 비동기 작업을 작성 하는 클래스입니다. join 또는 choice 연산자 **&&** 및 **||** 를 사용하여 작업을 구성할 수도 있습니다. 자세한 내용은 [작업 병렬 처리(동시성 런타임)][taskParallelism]를 참조하세요.

## <a name="lambda-function-return-types-and-task-return-types"></a>람다 함수 반환 형식 및 작업 반환 형식
작업 연속 작업에서 람다 함수의 반환 형식은 **task** 개체로 묶여 있습니다. 람다가 **double**을 반환하면 연속 작업의 작업 유형은 **task<double>** 가 됩니다. 그러나 작업 개체는 불필요하게 중첩된 반환 형식을 생성하지 않도록 설계되어 있습니다. 람다가 **IAsyncOperation&lt;SyndicationFeed^&gt;^** 를 반환하는 경우 연속 작업은 **task&lt;task&lt;SyndicationFeed^&gt;&gt;** 또는 **task&lt;IAsyncOperation&lt;SyndicationFeed^&gt;^&gt;^** 가 아닌 **task&lt;SyndicationFeed^&gt;** 를 반환합니다. 이 프로세스는 *비동기 래핑 해제*라고 하며 다음 연속 작업이 호출되기 전에 연속 작업 내부의 비동기 작업이 완료되었는지도 확인합니다.

앞의 예제에서 람다가 [**IAsyncInfo**][IAsyncInfo] 개체를 반환했음에도 작업이 **task<void>** 를 반환함을 확인할 수 있습니다. 다음 표에는 람다 함수와 바깥쪽 작업 간에 발생하는 유형 변환에 대한 요약이 나와 있습니다.

| | |
|--------------------------------------------------------|---------------------|
| 람다 반환 형식                                     | `.then` 반환 형식 |
| TResult                                                | task<TResult> |
| IAsyncOperation<TResult>^                        | task<TResult> |
| IAsyncOperationWithProgress&lt;TResult, TProgress&gt;^ | task<TResult> |
|IAsyncAction^                                           | task<void>    |
| IAsyncActionWithProgress<TProgress>^             |task<void>     |
| task<TResult>                                    |task<TResult>  |


## <a name="canceling-tasks"></a>작업 취소
사용자에게 비동기 작업을 취소할 수 있는 옵션을 제공하는 것이 좋습니다. 경우에 따라서는 작업 체인의 외부에서 프로그래밍 방식으로 작업을 취소해야 할 수 있습니다. 하지만 각 \* **비동기** 반환 형식에는 [ **취소** ] [ IAsyncInfoCancel] 에서 상속 하는 메서드 [  **IAsyncInfo**][IAsyncInfo], 외부 메서드를 노출 하기에 비효율적인 것입니다. 작업 체인에서 취소를 지원 하는 기본 방법은 사용 하는 것을 [ **취소\_토큰\_소스** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh749985.aspx) 만들려면를 [ **취소 \_토큰**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749975.aspx), 다음 토큰 초기 작업의 생성자에 전달 하는 것입니다. 비동기 작업 취소 토큰을 사용 하 여 만들어지는 경우 및 [ **취소\_토큰\_source::cancel** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh750076.aspx) 가 호출 작업이 자동으로 호출  **취소** 에 **IAsync\***  작업 및 전달 해당 연속 체인 취소를 요청 합니다. 다음 의사 코드에서는 기본 접근 방법을 보여 줍니다.

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

작업을 취소할 때를 [ **태스크\_취소** ] [ taskCanceled] 예외가 작업 체인 아래로 전파 됩니다. 단지 값 기반 연속 작업이 실행되지 않을 뿐이지만 [**task::get**][taskGet]이 호출될 경우 작업 기반 연속 작업에서 예외가 발생합니다. 오류 처리 연속을 사용 하는 경우 catch 하 고 있는지 확인 합니다 **태스크\_취소** 예외 명시적으로 합니다. [  **Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755825.aspx)에서 파생되지 않음).

취소는 공동 작업입니다. 연속 작업에 단지 UWP 메서드를 호출하는 것 이상으로 장기 실행되는 일부 작업이 있을 경우 사용자 스스로 취소 토큰을 정기적으로 확인하고 취소된 토큰이 있으면 실행을 중지해야 합니다. 연속 작업에 할당 된 모든 리소스를 정리 하기 호출 [ **취소\_현재\_태스크** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh749945.aspx) 해당 작업을 취소 하 여 취소 된 아래로 전파 그 뒤에 나오는 값 기반 연속 작업입니다. [  **FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/BR207871) 작업의 결과를 나타내는 작업 체인을 만들 수 있는 다른 예를 살펴봅시다. 사용자가 **취소** 단추를 선택하면 [**IAsyncInfo::Cancel**][IAsyncInfoCancel] 메서드가 호출되지 않습니다. 작업은 계속되는 대신 **nullptr**을 반환합니다. 연속 입력된 매개 변수 및 호출 테스트할 수 있습니다 **취소\_현재\_태스크** 입력 이면 **nullptr**합니다.

자세한 내용은 [PPL에서의 취소](https://msdn.microsoft.com/library/windows/apps/xaml/dd984117.aspx)를 참조하세요.

## <a name="handling-errors-in-a-task-chain"></a>작업 체인의 오류 처리
선행 항목이 취소되었거나 예외가 발생한 경우에도 연속 작업을 실행하려면 선행 작업의 람다가 [**IAsyncAction^**][IAsyncAction]를 반환하는 경우의 람다 함수에 대한 입력을 **task<TResult>** 또는 **task<void>** 로 지정하여 작업 기반 연속 작업으로 만듭니다.

작업 체인에서 오류 및 취소를 처리하기 위해 모든 연속 작업을 작업 기반으로 지정하거나, `try…catch` 블록 내에서 발생할 수 있는 각 작업을 묶을 필요는 없습니다. 대신 체인의 끝에 작업 기반 연속 작업을 추가하여 거기서 모든 오류를 처리할 수 있습니다. 예외-여기에 [ **태스크\_취소** ] [ taskCanceled] 예외-작업 체인 아래로 전파 되며 모든 값 기반 연속을 무시 되도록 오류 처리 작업 기반 연속에서 처리할 수 있습니다. 오류 처리 작업 기반 연속 작업을 사용하도록 앞의 예제를 다시 작성할 수 있습니다.

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

작업 기반 연속 작업에서 멤버 함수 [**task::get**][taskGet]을 호출하여 작업 결과를 얻습니다. 작업이 결과를 생성하지 않는 [**IAsyncAction**][IAsyncAction]이었다고 하더라도 **task::get**을 호출해야 합니다. **task::get**도 작업으로 전송된 예외를 가져오기 때문입니다. 입력 작업이 예외를 저장하는 경우 **task::get** 호출 시에 해당 예외가 발생합니다. 호출 하지 않아도 **task:: get**, 또는 체인의 끝에 작업 기반 연속을 사용 하지 않거나, throw 된 예외 형식을 catch 하지 마십시오는 **관찰 되지 않은\_태스크\_예외** 작업에 대 한 모든 참조가 삭제 된 경우 throw 됩니다.

처리할 수 있는 예외만 catch해야 합니다. 앱에 복구할 수 없는 오류가 발생하면 알 수 없는 상태에서 앱이 계속 실행되도록 하는 것보다 손상된 상태로 두는 것이 낫습니다. 또한 일반적으로 하려고 하지 마세요 catch 합니다 **관찰 되지 않은\_태스크\_예외** 자체입니다. 이 예외는 주로 진단용으로 사용됩니다. 때 **관찰 되지 않은\_태스크\_예외** 가 발생 했다는 것을 나타냅니다 코드의 버그입니다. 주요 원인은 처리해야 할 예외 또는 코드에서 다른 오류로 인해 발생한 복구할 수 없는 예외 때문입니다.

## <a name="managing-the-thread-context"></a>스레드 컨텍스트 관리
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

[  **IAsyncAction**][IAsyncAction] 또는 [**IAsyncOperation**][IAsyncOperation]을 반환하지 않는 작업은 아파트 인식 작업이 아니며, 기본적으로 해당 작업의 연속 작업은 사용 가능한 첫 백그라운드 스레드에서 실행됩니다.

오버 로드를 사용 하 여 두 종류의 작업에 대 한 기본 스레드 컨텍스트를 재정의할 수 있습니다 [ **task:: then** ] [ taskThen] 사용 하는 한 [ **작업 \_연속\_상황에 맞는**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749968.aspx)합니다. 예를 들어, 경우에 따라서는 백그라운드 스레드에서 아파트 인식 작업의 연속 작업을 예약하는 것이 좋을 수 있습니다. 이러한 경우에 전달할 수 있습니다 [ **태스크\_연속\_context::use\_임의의** ] [ useArbitrary] 에서 작업의 작업을 예약 하는 다중 스레드 아파트를에서 다음 사용 가능한 스레드를 지원 합니다. 이렇게 하면 해당 작업을 UI 스레드에서 발생하는 다른 작업과 동기화할 필요가 없으므로 연속 작업의 성능이 향상될 수 있습니다.

다음 예제에서는 지정 하는 데 유용 때 합니다 [ **태스크\_연속\_context::use\_임의의** ] [ useArbitrary] 옵션을 보여 줍니다 기본 연속 컨텍스트는 스레드로부터 안전한 컬렉션에 대해 동시 작업을 동기화 하는 데 유용한 방법입니다. 이 코드에서는 RSS 피드 및 각 URL에 대해 URL 목록 전체를 반복하고, 피드 데이터를 검색하기 위해 비동기 작업을 시작합니다. 피드가 검색되는 순서는 제어할 수 없지만 이는 그다지 중요하지 않습니다. 각 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR210642) 작업이 완료되면 첫 번째 연속 작업이 [**SyndicationFeed^**](https://msdn.microsoft.com/library/windows/apps/BR243485) 개체를 받은 다음 이를 사용하여 앱에서 정의된 `FeedData^` 개체를 초기화합니다. 이러한 각 작업은 다른에서 서로 독립적 이기 때문에 수 있습니다 잠재적으로 작업 속도를 지정 하 여 합니다 **태스크\_연속\_context::use\_임의의** 연속 컨텍스트 . 그러나 각 `FeedData` 개체가 초기화된 후에는 스레드로부터 안전한 컬렉션이 아닌 [**Vector**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx)에 추가해야 합니다. 따라서 연속 작업을 생성 하 고 지정할 [ **태스크\_연속\_context::use\_현재** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) 에 대 한 모든 호출 되도록 [ **Append** ](https://msdn.microsoft.com/library/windows/apps/BR206632) 동일한 Application Single-Threaded 아파트 (ASTA) 컨텍스트에서 발생 합니다. 때문에 [ **태스크\_연속\_context::use\_기본값** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) 명시적으로 지정할 필요가 없습니다 되지만 이렇게 여기의 sake에 대 한 기본 컨텍스트는 쉽게 구별할 수 있도록 합니다.

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

## <a name="handing-progress-updates"></a>진행 상황 업데이트 처리
[  **IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) 또는 [**IAsyncActionWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)를 지원하는 메서드는 작업이 완료될 때까지 진행 상황을 정기적으로 업데이트합니다. 진행 상황 보고는 작업 및 연속 작업의 개념과는 별개입니다. 개체의 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br206594) 속성에 대한 대리자를 제공하면 되며, 대리자 사용의 전형적인 예는 UI에서 진행률 표시줄을 업데이트하는 것입니다.

## <a name="related-topics"></a>관련 항목
* [C + 비동기 작업 만들기 + UWP 앱 용 CX](https://msdn.microsoft.com/library/hh750082)
* [Visual c + + 언어 참조](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)
* [비동기 프로그래밍][AsyncProgramming]
* [작업 병렬 처리(동시성 런타임)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "Namespace 동시성"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "Task 클래스"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "작업 병렬 처리"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
