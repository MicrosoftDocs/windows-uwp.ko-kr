---
title: WinRT API 오류 처리
description: WinRT Api를 사용 하 여 Xbox Live 서비스 호출을 수행할 때 오류를 처리 하는 방법을 알아봅니다.
ms.assetid: b4f04798-df91-430e-90f0-83b5a48b9ab2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox, 오류 처리
ms.localizationpriority: medium
ms.openlocfilehash: e72dfa0b6f98284c240cf6af2dde02439d694b48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598638"
---
# <a name="winrt-api-error-handling"></a>WinRT API 오류 처리

WinRT api에서 호출할 수에서 C + + /CX에서는 C#, 또는 Javascript를 try/catch 블록에서 오류를 처리 하는 데 사용 해야 하므로 예외를 사용 하 여 오류를 보고 합니다.

비동기 호출에 대 한 두 개의 try/catch 블록이 필요 같이:

```cpp
    try
    {
        auto asyncOp = xboxLiveContext->TitleStorageService->UploadBlobAsync(
            blobMetadata,
            blobBuffer,
            TitleStorageETagMatchCondition::NotUsed,
            DEFAULT_UPLOAD_BLOCK_SIZE
            );

        create_task(asyncOp)
        .then([this, ui]( task<TitleStorageBlobMetadata^> t )
        {
            try
            {
                TitleStorageBlobMetadata^ blobMetadata = t.get();

                ui->Log(L"UploadBlobAsync succeeded");
                PrintBlobMetadata(ui, blobMetadata);

                SaveNewETag(blobMetadata->ETag);
            }
            catch (Platform::Exception^ ex)
            {
                // This could happen if there's a network error or the service returns an error
                ui->Log("The async task of UploadBlobAsync failed with", ex->ToString());
            }
        });
    }
    catch (Platform::Exception^ ex)
    {
        // This could happen if there's invalid args sent to the API
        ui->Log("The API call to UploadBlobAsync failed with", ex->ToString());
    }
```

XSAPI 비동기 메서드는 메서드가 호출 될 때 동기적으로 실행 되는 일부 코드를 권한이 있습니다.  동기 코드 입력된 인수 유효성 검사 및 비동기 작업 또는 작업 시작과 같은 작동 합니다.  따라서도 비동기 메서드를 호출 하지만 (즉, 프로그래머 오류, 네트워크 오류가 아닌) 일반적인 시나리오에 대 한이 발생 해서는 안 됩니다-예외가 발생할 수 있습니다.  수이 가능성 및 프로그램을 인식 방어적 입력 유효성 검사 및 개발 하는 동안 코드를 철저히 테스트 합니다.  자체 호출 주위 try/catch를 배치 하거나 호출 스택의 상위 수준에서 하나를 저장, 중요 한 점은 하나 있어야 합니다.

## <a name="help-my-game-crashes-when-i-call-xyz-xbox-service-api"></a>도움말! 게임 내 충돌 XYZ Xbox 서비스 API를 호출 하는 경우

따라서 게임에서 충돌 하 고 Xbox 서비스 API 호출을 수행 하지만 호출 스택을 표시?

```cpp
msvcr110.dll!Concurrency::details::_ReportUnobservedException() Line 2455    C++
Social110Release.exe!Concurrency::details::_ExceptionHolder::~_ExceptionHolder() Line 915    C++
Social110Release.exe!Concurrency::details::_Task_impl_base::~_Task_impl_base() Line 1488    C++
Social110Release.exe!Concurrency::details::_Task_impl<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::`scalar deleting destructor'(unsigned int)    C++
Social110Release.exe!Concurrency::task<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::_ContinuationTaskHandle<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64,void,<lambda_8571b6148830c0805feee6ba9e76a692>,std::integral_constant<bool,1>,Concurrency::details::_TypeSelectorNoAsync>::`scalar deleting destructor'(unsigned int)    C++
msvcr110.dll!Concurrency::details::_TaskCollection::_NotifyCompletedChoreAndFree(Concurrency::details::_UnrealizedChore * pChore) Line 1171    C++
msvcr110.dll!Concurrency::details::_UnrealizedChore::_UnstructuredChoreWrapper(Concurrency::details::_UnrealizedChore * pChore) Line 373    C++
msvcr110.dll!Concurrency::details::InternalContextBase::Dispatch(Concurrency::DispatchState * pDispatchState) Line 1716    C++
msvcr110.dll!Concurrency::details::FreeThreadProxy::Dispatch() Line 203    C++
msvcr110.dll!Concurrency::details::ThreadProxy::ThreadProxyMain(void * lpParameter) Line 174    C++
ntdll.dll!RtlUserThreadStart(long (void *) * StartAddress, void * Argument) Line 822    C++
```

이러한 호출 스택 매우 혼란 스 러 됩니다.  동시성 따라서이 동시성 하는 중...  아 Microsoft::Xbox::Services::Social::XboxUserProfile 이므로, 호출 스택의 원인에 있어야 합니다.  실제로이 잘못 된 Xbox 사용자 id에 대 한 GetUserProfileAsync 호출 하 여 생성 된 호출 스택  이 호출 스택을 생성 하는 샘플 코드는 다음과 같습니다.

```cpp
    auto pAsyncOp = requester->ProfileService->GetUserProfileAsync("abc123"); //passing invalid Xbox User Id;

    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)
    {
        // Oops, I forgot my exception handling code here.
        // If I don't call resultTask.get() and catch any potential exception it may throw,
        // then PPL will report an unobserved exception.  That unobserved exception will cause your
        // app to crash.
    });
```

프로그램 호출 스택 Concurrency::_ReportUnobservedException() 포함 되어 있습니까?  다시 살펴보려면 위의 호출 스택.  이 중요 합니다.  프로그램 호출 스택 Concurrency:: _ReportUnobservedException() 있으면 게임 코드에서 버그가 발견 되었습니다.

PPL에는 다른 작업에 올 수 있습니다 하 여 작업을 만듭니다.  위의 예에서 create_task() GetUserProfileAsync() 호출할 태스크를 빌드하고.then() 다음 태스크를 만듭니다.  이 종종 라고 선행 작업 (첫 번째 항목) 및 연속 작업 (초)입니다.  예제에서는 연속 오류 처리를 누락 되었습니다.  런타임 예외를 throw 하는 작업 및 태스크 또는 해당 연속 중 하나에서 예외 잡히지 않는 경우 앱을 종료 합니다.

연속 작업을 하더라도 실제로 두 가지 종류는 note 합니다.  한 종류, 작업 기반 연속 작업이 이전 작업 입력된 인수 변수로 사용 합니다.  이 작업 항상 실행을 선행 작업이 예외를 throw 하는 경우에 합니다.  선행 작업의 결과 가져오려면.get() 인수에 호출 해야 합니다.  두 번째 값을 기반으로, 이전 태스크의 출력을 받는 직접 – 제외 하 고 실제로 syntactic sugar는 한 가지 것 – 값 기반 연속은 선행 작업에서 예외가 throw 전혀 실행 되지 않습니다.

권장 사항:  충돌을 방지 하려면 서라운드 모든 동시성:: task::get() 또는 concurrency:: task::wait() 호출의 try/catch 블록에서 복구할 수 있는 오류를 처리 하 고 연속 체인 끝에 작업 기반 연속을 사용 합니다.

몇 가지 예를 살펴보겠습니다.

작업 기반 연속 예제

```cpp
    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)   // Task-based continuation
    {
        try
        {
            XboxUserProfile^ result = resultTask.get();

            // success, do something
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
        }
    });
```

값 기반 연속 예제

```cpp
    create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
        // if the task didn't complete successfully, you'd better have a task-based
        // continuation at the end of the continuation chain or the app will crash.
    })
    .then( [this] (task<void> previousTask) // Task-based continuation
    {
        try
        {
            // DO NOT IGNORE THIS THINKING IT'S NOT IMPORTANT.

            // call concurrency::task::get and handle any unobserved exception
            // so the application doesn't crash.
            previousTask.get();

            // success, continue
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
            // By handling this exception, you ensure your application will not
            // crash when calling Xbox Service APIs
        }
    });
```

세 번째 솔루션은 – 값 기반 연속을 완전히 사용 하지만.get() 또는.wait() 다른 스레드에서 호출 및 예외를 catch 합니다.  간단한 예는 다음과 같습니다.

```cpp
    auto getProfileTask = create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
    });
    // Note the lack of a task-based continuation with error handling at the end

    // You may call .get() or .wait() on a value-based only chain, but
    // must ensure you surround the call in a try/catch block and handle errors
    try
    {
        getProfileTask.get();     // or getProfileTask.wait();
    }
    catch (Platform::Exception^ ex)
    {
        // concurrency::task::get threw an exception
        // safely handle the error here
        // By handling this exception, you ensure your application will not
        // crash when calling Xbox Service APIs
    }
```

**PPL을 사용 하지 않는 경우** AsyncOperationCompletionHandler 또는 AsyncActionCompletionHandler PPL을 대신 사용 중인 경우 올바르게 수행 하는 경우 응용 프로그램 작동 중단 발생 하는 몇 가지 오류 처리 해야 합니다.  다음은 오류를 처리 하는 방법을 보여 주는 예

```cpp
    try
    {
        // Example is making a service call with an invalid XboxUserId which will result in an error.
        // The completion handler properly detects the error and does not crash the app.
        requester->ProfileService->GetUserProfileAsync("abc123")->Completed
            = ref new AsyncOperationCompletedHandler<XboxUserProfile^>([=] (IAsyncOperation<XboxUserProfile^>^ operation, Windows::Foundation::AsyncStatus status)
        {
            if( status == Windows::Foundation::AsyncStatus::Completed)
            {
                // Always check the AsyncStatus before calling GetResults().
                // If status is not AsyncStatus::Completed, calls to operation->GetResults()
                // may throw an exception.
                // You can also surround this call in a try/catch block for added safety.

                XboxUserProfile^ result = operation->GetResults();

                // success, do something with the result
            }
            else if( status == Windows::Foundation::AsyncStatus::Error )
            {
                // Failed
            }
        });
    }
    catch ( Platform::COMException^ ex )
    {
        // What is this try/catch block for?
        //
        // Xbox Service APIs do have some code that runs synchronously and errors need
        // to be safely handled.  In this example, if “” was passed instead of “abc123”,
        // then an invalid argument exception would be thrown when calling GetUserProfileAsync
    // See the next section for more a more detailed explanation.
        //
        // Note: this catch block will NOT catch exceptions thrown within the completion handler.
    }
```

**GetNextAsync() 및 예외** 페이징 Api를 사용 하 여?  결과의 다음 페이지를 요청 하려면 GetNextAsync() 메서드를 포함 하는 모든 AchievementResult, LeaderboardResult, InventoryItemResult, 및 TitleStorageBlobMetadataResult 개체입니다.  데이터가 더 이상 사용할 수 있으면 특별 한 경우 있습니다 됩니다 GetNextAsync()를 호출할 때 예외를 트리거하는 합니다.  GetNextAsync() 동기 실행 하는 동안이 예외가 throw 됩니다.  이 경우 GetNextAsync 메서드 INET_E_DATA_NOT_AVAILABLE (0x800C0007)를 throw합니다.

권장 사항: 메서드는 try/catch 블록에서 호출 하 고 INET_E_DATA_NOT_AVAILABLE 정상적으로 처리할 GetNextAsync()를 래핑할 확인 합니다.

```cpp
    try
    {
        // AchievementResult^ LastResult

        // Get next page of achievement results
        if(LastResult != nullptr)
        {
            auto getNextPage = LastResult->GetNextAsync(10);

            // create_task( getNextPage ) ...
        }
    }
    catch (Platform::Exception^ ex)
    {
        if (ex->HResult == INET_E_DATA_NOT_AVAILABLE)
        {
                // we hit the end of the achievements
        }
        else
        {
            // failed for unexpected reason
        }
    }
```
