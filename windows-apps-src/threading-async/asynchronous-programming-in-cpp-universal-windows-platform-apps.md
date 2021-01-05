---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: 이 문서에서는 ppltasks.h의 concurrency 네임 스페이스에 정의 된 작업 클래스를 사용 하 여 Visual C++ 구성 요소 확장 (c + +/CX)에서 비동기 메서드를 사용 하는 권장 방법을 설명 합니다.
title: C + +의 비동기 프로그래밍
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, 스레드, 비동기, c + +
ms.localizationpriority: medium
ms.openlocfilehash: 8e7deb785f7945f89cb1eb7266a43685691846fe
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860404"
---
# <a name="asynchronous-programming-in-ccx"></a>C + +/CX의 비동기 프로그래밍
> [!NOTE]
> 이 항목은 C++/CX 애플리케이션 유지에 도움을 주기 위해 작성되었습니다. 하지만 새로운 응용 프로그램에 대해 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 것이 좋습니다. C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 최신의 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 최고 수준의 액세스를 제공하도록 설계되었습니다.

이 문서에서는 `task` `concurrency` ppltasks.h의 네임 스페이스에 정의 된 클래스를 사용 하 여 Visual C++ 구성 요소 확장 (c + +/cx)에서 비동기 메서드를 사용 하는 데 권장 되는 방법을 설명 합니다.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>UWP (유니버설 Windows 플랫폼) 비동기 형식
UWP (유니버설 Windows 플랫폼)는 비동기 메서드를 호출 하기 위한 잘 정의 된 모델을 제공 하 고 이러한 메서드를 사용 하는 데 필요한 형식을 제공 합니다. UWP 비동기 모델에 익숙하지 않은 경우이 문서의 나머지 부분을 읽기 전에 [비동기 프로그래밍][AsyncProgramming] 을 참조 하세요.

C + +에서 직접 비동기 Windows 런타임 Api를 사용할 수 있지만, [**동시성**][concurrencyNamespace] 네임 스페이스에 포함 되 고에 정의 된 [**작업 클래스**][task-class] 및 관련 형식 및 함수를 사용 하는 것이 좋습니다 `<ppltasks.h>` . **Concurrency:: task** 는 범용 형식 이지만 유니버설 WINDOWS 플랫폼 (UWP) 앱 및 구성 요소에 필요한 **/zw** 컴파일러 스위치가 사용 되는 경우 작업 클래스는 다음과 같은 작업을 용이 하 게 하기 위해 uwp 비동기 형식을 캡슐화 합니다.

-   여러 비동기 및 동기 작업을 함께 연결

-   작업 체인의 예외 처리

-   작업 체인에서 취소 수행

-   개별 작업이 적절 한 스레드 컨텍스트 또는 아파트에서 실행 되는지 확인 합니다.

이 문서에서는 UWP 비동기 Api에서 **작업** 클래스를 사용 하는 방법에 대 한 기본 지침을 제공 합니다. **작업 및 작업** 만들기를 비롯 한 관련 메서드에 대 한 [**자세한 \_**][createTask]내용은 [작업 병렬 처리 (동시성 런타임)][taskParallelism]를 참조 하세요. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>작업을 사용 하 여 비동기 작업 사용
다음 예제에서는 작업 클래스를 사용 하 여 [**iasyncoperation<tresult>**][Iasyncoperation<tresult>] 인터페이스를 반환 하 고 해당 작업에서 값을 생성 하는 **비동기** 메서드를 사용 하는 방법을 보여 줍니다. 기본 단계는 다음과 같습니다.

1.  메서드를 호출 `create_task` 하 고 **iasyncoperation<tresult> ^** 개체를 전달 합니다.

2.  작업에서 멤버 함수 [**task:: then**][taskThen] 을 호출 하 고 비동기 작업이 완료 되 면 호출 되는 람다를 제공 합니다.

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

[**Task:: then**][taskThen] 함수에서 만들고 반환 하는 작업을 *연속* 작업 이라고 합니다. 사용자가 제공한 람다에 대 한 입력 인수 (이 경우)는 태스크 작업이 완료 될 때 생성 되는 결과입니다. **Iasyncoperation<tresult>** 인터페이스를 직접 사용 하는 경우 [**Iasyncoperation<tresult>:: GetResults**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) 를 호출 하 여 검색 되는 것과 동일한 값입니다.

[**Task:: then**][taskThen] 메서드는 즉시 반환 되며 해당 대리자는 비동기 작업이 성공적으로 완료 될 때까지 실행 되지 않습니다. 이 예제에서는 비동기 작업으로 인해 예외가 throw 되거나 취소 요청의 결과로 취소 된 상태에서 종료 되는 경우 연속 작업이 실행 되지 않습니다. 나중에 이전 태스크가 취소 되거나 실패 한 경우에도 실행 되는 연속 작업을 작성 하는 방법을 설명 합니다.

로컬 스택에 작업 변수를 선언 하는 경우에는 작업을 완료 하기 전에 메서드가 반환 되는 경우에도 해당 작업이 완료 될 때까지 삭제 되지 않고 해당 작업에 대 한 모든 참조가 범위를 벗어날 때까지 해당 수명이 관리 됩니다.

## <a name="creating-a-chain-of-tasks"></a>작업 체인 만들기
비동기 프로그래밍에서는 각 연속 작업이 완료 될 때만 실행 되는 작업 순서 ( *작업 체인* 이 라고도 함)를 정의 하는 것이 일반적입니다. 경우에 따라 이전 (또는 *선행* 작업) 작업은 연속에서 입력으로 허용 하는 값을 생성 합니다. [**Task:: then**][taskThen] 메서드를 사용 하 여 직관적이 고 간단한 방법으로 작업 체인을 만들 수 있습니다. 메서드는 **T** 가 람다 함수의 반환 형식인 **작업 <T>** 을 반환 합니다. 작업 체인에 여러 연속을 구성할 수 있습니다. `myTask.then(…).then(…).then(…);`

작업 체인은 연속 작업에서 새 비동기 작업을 만들 때 특히 유용 합니다. 이러한 작업을 비동기 작업 이라고 합니다. 다음 예에서는 두 개의 연속 작업이 있는 작업 체인을 보여 줍니다. 초기 태스크는 기존 파일에 대 한 핸들을 가져오고, 작업이 완료 되 면 첫 번째 연속 작업이 새 비동기 작업을 시작 하 여 파일을 삭제 합니다. 작업이 완료 되 면 두 번째 연속 작업이 실행 되 고 확인 메시지를 출력 합니다.

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

앞의 예제에서는 네 가지 중요 한 사항을 보여 줍니다.

-   첫 번째 연속은 [**Iasyncaction ^**][IAsyncAction] 개체를 **<void> 작업** 으로 변환 하 고 **작업** 을 반환 합니다.

-   두 번째 연속 작업은 오류 처리를 수행 하지 않으므로 **작업 <void>** 을 입력으로 사용 하지 않고 **void** 를 사용 합니다. 값 기반 연속입니다.

-   [**DeleteAsync**][deleteAsync] 작업이 완료 될 때까지 두 번째 연속 작업이 실행 되지 않습니다.

-   두 번째 연속은 값을 기반으로 하기 때문에 [**DeleteAsync**][deleteAsync] 에 대 한 호출에 의해 시작 된 작업이 예외를 throw 하는 경우 두 번째 연속 작업이 전혀 실행 되지 않습니다.

**참고**  작업 체인을 만드는 **작업은 작업** 클래스를 사용 하 여 비동기 작업을 작성 하는 방법 중 하나일 뿐입니다. Join 및 choice 연산자와을 사용 하 여 작업을 작성할 수도 있습니다 **&&** **||** . 자세한 내용은 [작업 병렬 처리 (동시성 런타임)][taskParallelism]를 참조 하세요.

## <a name="lambda-function-return-types-and-task-return-types"></a>람다 함수 반환 형식 및 작업 반환 형식
작업 연속에서 람다 함수의 반환 형식은 **작업** 개체에 래핑됩니다. 람다가 **double** 을 반환 하는 경우 연속 작업의 형식은 **task <double>** 입니다. 그러나 작업 개체는 불필요 하 게 중첩 된 반환 형식을 생성 하지 않도록 디자인 되었습니다. 람다가 **iasyncoperation<tresult><SyndicationFeed ^>^** 을 반환 하는 경우 연속 작업은 작업<작업<**SyndicationFeed ^>>** 또는 **작업<iasyncoperation<tresult><SyndicationFeed ^>^>^**>아니라 작업 **<SyndicationFeed ^** 반환 합니다. 이 프로세스를 *비동기 래핑 해제* 하 고 다음 연속이 호출 되기 전에 연속 작업 내의 비동기 작업이 완료 되도록 합니다.

이전 예제에서 작업은 람다가 [**IAsyncInfo**][IAsyncInfo] 개체를 반환한 경우에도 **작업 <void>** 을 반환 합니다. 다음 표에는 람다 함수와 바깥쪽 작업 간에 발생하는 유형 변환에 대한 요약이 나와 있습니다.

| 람다 반환 형식 | `.then` 반환 형식 |
| ------------------ | ------------------- |
| TResult                                                | 임무<TResult> |
| Iasyncoperation<tresult><TResult>^                        | 임무<TResult> |
| IAsyncOperationWithProgress<TResult, TProgress>^ | 임무<TResult> |
|IAsyncAction ^                                           | 임무<void>    |
| Iasyncactionwithprogress<tprogress><TProgress>^             |임무<void>     |
| 임무<TResult>                                    |임무<TResult>  |


## <a name="canceling-tasks"></a>작업 취소
비동기 작업을 취소 하는 옵션을 사용자에 게 제공 하는 것이 좋습니다. 작업 체인 외부에서 프로그래밍 방식으로 작업을 취소 해야 하는 경우도 있습니다. 각 \* *_비동기_* 반환 형식에는 [**IAsyncInfo**][IAsyncInfo]에서 상속 하는 [**취소**][IAsyncInfoCancel] 메서드가 있지만 외부 메서드에 노출 하는 것은 좋지 않습니다. 작업 체인에서 취소를 지 원하는 기본 방법은 취소 [**\_ 토큰 \_ 소스**](/cpp/parallel/concrt/reference/cancellation-token-source-class) 를 사용 하 여 [**취소 \_ 토큰**](/cpp/parallel/concrt/reference/cancellation-token-class)을 만든 다음 토큰을 초기 작업의 생성자에 전달 하는 것입니다. 취소 토큰을 사용 하 여 비동기 작업을 만들고 [**취소 \_ 토큰 \_ source:: cancel**](/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017&preserve-view=true) 을 호출 하면 태스크에서 자동으로 **Iasync \** _ 작업에서 **cancel** 을 호출 하 고 취소 요청을 연속 체인으로 전달 합니다. 다음 의사 코드에서는 기본적인 방법을 보여 줍니다.

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

작업이 취소 되 면 [_ *작업 \_ 취소* *][taskCanceled] 된 예외가 작업 체인에서 전파 됩니다. 값 기반 연속은 단순히 실행 되지 않지만 작업 기반 연속은 [**task:: get**][taskGet] 이 호출 될 때 예외를 throw 합니다. 오류 처리 연속이 있으면 **태스크가 \_ 취소** 된 예외를 명시적으로 catch 하는지 확인 합니다. 이 예외는 [**Platform:: exception**](/cpp/cppcx/platform-exception-class)에서 파생 되지 않습니다.

취소는 협조적입니다. 연속 작업에서 UWP 메서드를 호출 하는 것 외에도 장기 실행 작업을 수행 하는 경우 취소 토큰의 상태를 정기적으로 확인 하 고 취소 된 경우 실행을 중지 해야 합니다. 연속 작업에서 할당 된 모든 리소스를 정리한 후에는 [**\_ 현재 \_ 작업 취소**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017&preserve-view=true) 를 호출 하 여 해당 작업을 취소 하 고 그 다음에 나오는 값 기반 연속으로 취소를 전파 합니다. 또 다른 예: [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 작업의 결과를 나타내는 작업 체인을 만들 수 있습니다. 사용자가 **취소** 단추를 선택 하면 [**IAsyncInfo:: cancel**][IAsyncInfoCancel] 메서드가 호출 되지 않습니다. 대신 작업이 성공 하지만 **nullptr** 를 반환 합니다. 연속 작업은 입력 매개 변수를 테스트 하 고 입력이 **nullptr** 인 경우 **\_ 현재 \_ 작업 취소** 를 호출할 수 있습니다.

자세한 내용은 [PPL에서의 취소](/cpp/parallel/concrt/cancellation-in-the-ppl) 를 참조 하세요.

## <a name="handling-errors-in-a-task-chain"></a>작업 체인의 오류 처리
선행 작업이 취소 되거나 예외를 throw 한 경우에도 연속 작업이 실행 되도록 하려면 선행 작업의 람다가 [**Iasyncaction ^**][IAsyncAction]을 반환 하는 경우 해당 람다 함수에 대 한 입력 **을 <TResult> 작업** 또는 **태스크 <void>** 로 지정 하 여 연속 작업을 연속으로 만듭니다.

작업 체인에서 오류 및 취소를 처리 하려면 모든 연속 작업을 수행 하거나 블록 내에서 throw 될 수 있는 모든 작업을 묶을 필요가 없습니다 `try…catch` . 대신 체인의 끝에 작업 기반 연속을 추가 하 고 여기에서 모든 오류를 처리할 수 있습니다. [**작업 \_ 취소**][taskCanceled] 예외를 포함 하는 모든 예외는 작업 체인을 전파 하 고 값 기반 연속을 무시 하므로 오류 처리 작업 기반 연속 작업에서 처리할 수 있습니다. 오류 처리 작업 기반 연속을 사용 하는 이전 예제를 다시 작성할 수 있습니다.

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

작업 기반 연속에서는 멤버 함수 [**task:: get**][taskGet] 을 호출 하 여 작업의 결과를 가져옵니다. 작업 **:: get** 은 작업에 전송 된 모든 예외도 가져오기 때문에 결과를 생성 하지 않는 [**iasyncaction**][IAsyncAction] 인 경우에도 **작업:: get** 을 호출 해야 합니다. 입력 작업에서 예외를 저장 하는 경우 **task:: get** 에 대 한 호출에서 예외가 throw 됩니다. **Task:: get** 을 호출 하지 않거나 체인의 끝에 작업 기반 연속을 사용 하지 않거나 throw 된 예외 형식을 catch 하지 않는 경우, 작업에 대 한 모든 참조가 삭제 되 면 **관찰 되지 않은 \_ 작업 \_ 예외가** throw 됩니다.

처리할 수 있는 예외만 catch 합니다. 앱에 복구할 수 없는 오류가 발생 하는 경우 앱이 중단 될 때까지 지속 되도록 하는 것이 더 좋습니다. 또한 일반적으로 **관찰 되지 않은 \_ 작업 \_ 예외** 자체를 catch 하지 않습니다. 이 예외는 주로 진단 목적으로 사용 됩니다. **관찰 되지 않은 \_ 작업 \_ 예외가** throw 되 면 일반적으로 코드의 버그를 나타냅니다. 주로 처리 해야 하는 예외 또는 코드의 다른 오류로 인해 발생 하는 복구할 수 없는 예외가 원인일 수 있습니다.

## <a name="managing-the-thread-context"></a>스레드 컨텍스트 관리
UWP 앱의 UI는 STA (단일 스레드 아파트)에서 실행 됩니다. 람다가 [**Iasyncaction**][IAsyncAction] 또는 [**iasyncoperation<tresult>**][Iasyncoperation<tresult>] 를 반환 하는 작업은 아파트를 인식 합니다. 태스크가 STA에서 만들어진 경우에는 달리 지정 하지 않는 한 모든 연속 작업은 기본적으로 실행 됩니다. 즉, 전체 작업 체인은 부모 작업에서 아파트 인식을 상속 합니다. 이 동작은 STA 에서만 액세스할 수 있는 UI 컨트롤과의 상호 작용을 간소화 하는 데 도움이 됩니다.

예를 들어 UWP 앱에서 XAML 페이지를 나타내는 모든 클래스의 멤버 함수에서 [**디스패처**](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체를 사용 하지 않고도 [**task:: then**][taskThen] 메서드 내에서 [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 컨트롤을 채울 수 있습니다.

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

태스크가 [**Iasyncaction**][IAsyncAction] 또는 [**iasyncoperation<tresult>**][Iasyncoperation<tresult>]를 반환 하지 않는 경우에는 아파트를 인식 하지 않으며 기본적으로 사용 가능한 첫 번째 백그라운드 스레드에서 연속 작업이 실행 됩니다.

작업 (task)의 오버 로드를 사용 하 여 작업의 각 종류에 대 한 기본 스레드 컨텍스트를 재정의할 수 있습니다. [**그러면**][taskThen] 작업 [**\_ 연속 \_ 컨텍스트**](/cpp/parallel/concrt/reference/task-continuation-context-class)를 사용 합니다. 예를 들어, 경우에 따라 백그라운드 스레드에서 아파트 인식 작업의 연속을 예약 하는 것이 좋을 수 있습니다. 이러한 경우, [**작업 \_ 연속 \_ 컨텍스트:: \_ 임의의**][useArbitrary] 작업을 사용 하 여 다중 스레드 아파트의 다음 사용 가능한 스레드에서 작업을 예약 하는 작업을 수행할 수 있습니다. 이렇게 하면 해당 작업은 UI 스레드에서 발생 하는 다른 작업과 동기화 될 필요가 없기 때문에 연속 작업의 성능을 향상 시킬 수 있습니다.

다음 예제에서는 [**작업 \_ 연속 \_ 컨텍스트:: \_ 임의의 옵션 사용**][useArbitrary] 옵션을 지정 하는 것이 유용한 경우를 보여 줍니다. 또한 스레드로부터 안전 하지 않은 컬렉션에서 동시 작업을 동기화 하는 데 기본 연속 컨텍스트가 어떻게 유용한 지 보여 줍니다. 이 코드에서는 RSS 피드에 대 한 Url 목록을 반복 하 고 각 URL에 대해 피드 데이터를 검색 하는 비동기 작업을 시작 합니다. 피드가 검색 되는 순서를 제어할 수 없으며 걱정 하지 않아도 됩니다. 각 [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync) 작업이 완료 되 면 첫 번째 연속 작업은 [**SyndicationFeed ^**](/uwp/api/Windows.Web.Syndication.SyndicationFeed) 개체를 수락 하 고이 개체를 사용 하 여 앱 정의 개체를 초기화 합니다 `FeedData^` . 이러한 각 작업은 서로 독립적 이므로 **작업 \_ 연속 \_ 컨텍스트:: \_ 임의** 연속 컨텍스트 사용을 지정 하 여 작업 속도를 높일 수 있습니다. 그러나 각 개체가 초기화 된 후에는 `FeedData` 스레드로부터 안전한 컬렉션이 아닌 [**Vector**](/cpp/cppcx/platform-collections-vector-class)에 추가 해야 합니다. 따라서 연속 작업을 만들고 [**작업 \_ 연속 \_ 컨텍스트:: \_ current를 사용**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017&preserve-view=true) 하 여 [**Append**](/uwp/api/windows.foundation.collections.ivector_t_.append) 에 대 한 모든 호출이 동일한 ASTA (Application Single-Threaded 아파트) 컨텍스트에서 발생 하는지 확인 합니다. [**작업 \_ 연속 \_ 컨텍스트:: use \_ default**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017&preserve-view=true) 는 기본 컨텍스트입니다. 명시적으로 지정할 필요는 없으며 명확 하 게 하기 위해 여기에서 수행 합니다.

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

연속 작업 내에서 생성 되는 새 작업 인 중첩 된 작업은 초기 작업의 아파트 인식 기능을 상속 하지 않습니다.

## <a name="handing-progress-updates"></a>진행률 업데이트 처리
[**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 또는 [**Iasyncactionwithprogress**](/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) 를 지 원하는 메서드는 작업이 완료 되기 전에 작업이 진행 되는 동안 주기적으로 진행률 업데이트를 제공 합니다. 진행률 보고는 태스크 및 연속 개념과는 독립적입니다. 개체의 [**Progress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 속성에 대 한 대리자를 제공 하기만 하면 됩니다. 대리자를 일반적으로 사용 하는 것은 UI에서 진행률 표시줄을 업데이트 하는 것입니다.

## <a name="related-topics"></a>관련 항목
* [UWP 앱 용 c + +/CX로 비동기 작업 만들기](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [Visual C++ 언어 참조](/cpp/cppcx/visual-c-language-reference-c-cx)
* [비동기 프로그래밍][AsyncProgramming]
* [작업 병렬 처리 (동시성 런타임)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: ./asynchronous-programming-universal-windows-platform-apps.md "AsyncProgramming"
[concurrencyNamespace]: /cpp/parallel/concrt/reference/concurrency-namespace "Concurrency 네임 스페이스"
[createTask]: /cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task "CreateTask"
[deleteAsync]: /uwp/api/Windows.Storage.StorageFile "DeleteAsync"
[IAsyncAction]: /uwp/api/Windows.Foundation.IAsyncAction "IAsyncAction"
[IAsyncOperation]: /uwp/api/windows.foundation.iasyncoperation-1 "Iasyncoperation<tresult>"
[IAsyncInfo]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfo"
[IAsyncInfoCancel]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfoCancel"
[taskCanceled]: /cpp/parallel/concrt/reference/task-canceled-class "작업이 취소 됨"
[task-class]: /cpp/parallel/concrt/reference/task-class#get "작업 클래스"
[taskGet]: /cpp/parallel/concrt/reference/task-class "TaskGet"
[taskParallelism]: /cpp/parallel/concrt/task-parallelism-concurrency-runtime "작업 병렬 처리"
[taskThen]: /cpp/parallel/concrt/reference/task-class#then "TaskThen"
[useArbitrary]: /cpp/parallel/concrt/reference/task-continuation-context-class "UseArbitrary"
