---
title: 비동기 C API 호출 패턴
description: 비동기 C API 패턴 XSAPI C API에 대 한 호출에 대해 알아봅니다.
ms.date: 06/10/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 개발자 프로그램
ms.localizationpriority: medium
ms.openlocfilehash: edc6248a363b844d94c8fa03ab7ce071cc941908
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608008"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>XSAPI에 대 한 호출 패턴 플랫 C 계층 비동기 호출

**비동기 API** 은 신속 하 게 반환 하지만 시작 하는 API는 **비동기 작업** 작업이 완료 되 면 결과가 반환 됩니다.

게임 제어 스레드를 통해 실행 거의 일반적으로 **비동기 작업** 스레드를 사용할 때 결과 반환 합니다를 **완료 콜백이**합니다.  일부 게임 힙의 섹션 스레드 동기화에 대 한 필요를 방지 하려면 단일 스레드에서 연결 되도록 설계 되었습니다. 경우는 **완료 콜백이** 게임 스레드에서 호출 되지 않은 컨트롤에 결과 사용 하 여 공유 상태를 업데이트는 **비동기 작업** 스레드 동기화 해야 합니다.

XSAPI C API를 노출 시 개발자에 게 직접 스레드 컨트롤을 제공 하는 새로운 비동기 C API는 **비동기 API** 와 같은 호출 **XblSocialGetSocialRelationshipsAsync()**,  **XblProfileGetUserProfileAsync()** 하 고 **XblAchievementsGetAchievementsForTitleIdAsync()** 합니다.

다음은 기본 예제를 호출 합니다 **XblProfileGetUserProfileAsync** API.

```cpp
AsyncBlock* asyncBlock = new AsyncBlock {};
asyncBlock->queue = asyncQueue;
asyncBlock->context = customDataForCallback;
asyncBlock->callback = [](AsyncBlock* asyncBlock)
{
    XblUserProfile profile;
    if( SUCCEEDED( XblProfileGetUserProfileResult(asyncBlock, &profile) ) )
    {
        printf("Profile retrieved successfully\r\n");
    }
    delete asyncBlock;
};
XblProfileGetUserProfileAsync(asyncBlock, xboxLiveContext, xuid);
```

이 호출 패턴을 이해 하려면 사용 하는 방법을 이해 해야 합니다 **AsyncBlock** 하며 **AsyncQueue**합니다.

* **AsyncBlock** 모든 관련 된 정보를 전달 합니다 **비동기 작업** 하 고 **완료 콜백**.

* **AsyncQueue** 스레드 실행을 확인할 수 있습니다 합니다 **비동기 작업** 스레드 AsyncBlock의 호출 **완료 콜백**합니다.

## <a name="the-asyncblock"></a>**AsyncBlock**

에 대해 살펴보겠습니다 합니다 **AsyncBlock** 자세히 설명에서 합니다. 것이 구조체는 다음과 같이 정의 합니다.

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

합니다 **AsyncBlock** 포함 되어 있습니다.

* *콜백* -비동기 작업이 완료 된 후 호출 되는 선택적 콜백 함수입니다.  콜백을 지정 하지 않으면를 기다릴 수 있습니다 합니다 **AsyncBlock** 로 완료 하 **GetAsyncStatus** 다음 결과 가져옵니다.
* *상황에 맞는* -콜백 함수에 데이터를 전달할 수 있습니다.
* *큐* -핸들 인 async_queue_handle_t 지정 하는 **AsyncQueue**합니다. 이 설정 되어 있지 않으면 기본 큐 사용 됩니다.

힙의 각 비동기 API 호출에 대 한 새 AsyncBlock 만들어야 합니다.  AsyncBlock AsyncBlock의 완료 콜백이 호출 되 고 삭제 한 후 될 때까지 있어야 합니다.

> [!IMPORTANT]
> **AsyncBlock** 때까지 메모리에 남아 있어야 합니다 **비동기 작업** 완료 합니다. 동적으로 할당 하는 경우 AsyncBlock의 내에서 삭제할 수 있습니다 **완료 콜백이**합니다.

### <a name="waiting-for-asynchronous-task"></a>대기 **비동기 작업**

알 수는 **비동기 작업** 다양 한 다른 방법으로 완료 됩니다.

* AsyncBlock **완료 콜백이** 라고
* 호출 **GetAsyncStatus** 완료 될 때까지 대기 하는 true입니다.
* WaitEvent를 설정 합니다 **AsyncBlock** 이벤트 신호를 받을 때까지 대기 하 고

하지만 사용 하 여 **GetAsyncStatus** waitEvent, 및를 **비동기 작업** AsyncBlock의 후 전체 비율은 **완료 콜백이** 실행 AsyncBlock의 **완료 콜백이** 선택 사항입니다.

한 번 합니다 **비동기 작업** 는 완료 하면 결과 얻을 수 있습니다.

### <a name="getting-the-result-of-the-asynchronous-task"></a>결과 얻을 수는 **비동기 작업**

대부분 결과 가져오려면 **비동기 API** 함수에는 해당 \[함수 이름을\]비동기 호출의 결과 받기 위한 함수 결과입니다. 예제 코드에서 **XblProfileGetUserProfileAsync** 에 해당 **XblProfileGetUserProfileResult** 함수입니다. 함수의 결과 반환 하 고 적절 하 게 작동 하려면이 함수를 사용할 수 있습니다.  각각의 설명서를 참조 하세요 **비동기 API** 함수에 대 한 전체 내용은 결과 검색 합니다.

## <a name="the-asyncqueue"></a>**AsyncQueue**

**AsyncQueue** 스레드 실행을 확인할 수 있습니다 합니다 **비동기 작업** 스레드 AsyncBlock의 호출 **완료 콜백**합니다.

설정 하 여 이러한 작업을 수행 하는 스레드를 제어할 수 있습니다.는 *디스패치 모드*합니다. 세 가지 디스패치 모드 사용할 수 있습니다.

* *수동* -수동 큐는 자동으로 디스패치되 지 않습니다.  원하는 모든 스레드에서 발송 하는 개발자는 것입니다. 이 특정 스레드에 대 한 비동기 호출의 작업 또는 콜백 쪽에 할당할 수 있습니다.  이 아래에 자세히 설명 합니다.

* *스레드 풀* -스레드 풀을 사용 하 여 디스패치합니다.  스레드 풀 스레드 풀 스레드가 사용 가능 해질 때 큐에서 다시 실행에 대 한 호출을 수행 하는 동시에 호출을 호출 합니다.  사용할 easist 이지만 스레드 사용 되는 컨트롤의 최소 크기를 제공 합니다.

* *스레드 고정* -QueueUserAPC를 사용 하 여 비동기 큐를 만든 스레드에서 디스패치합니다. 사용자 모드 APC를 큐에 대기 하는 경우 스레드는 진행 상태에 있지 않다면 APC 함수를 호출 하려면 전달 됩니다. 스레드를 사용 하 여 경고 상태로 전환 **SleepEx**를 **SignalObjectAndWait**합니다 **waitforsingleobjectex가**, **WaitForMultipleObjectsEx**, 또는 **MsgWaitForMultipleObjectsEx** 경고 대기 작업을 수행 하려면

새로 만들 **AsyncQueue** 를 호출 해야 **CreateAsyncQueue**합니다.

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

여기서 AsyncQueueDispatchMode 앞서 소개한 세 디스패치 모드를 포함 되어 있습니다.

```cpp
typedef enum AsyncQueueDispatchMode
{
    /// <summary>
    /// Async callbacks are invoked manually by DispatchAsyncQueue
    /// </summary>
    AsyncQueueDispatchMode_Manual,

    /// <summary>
    /// Async callbacks are queued to the thread that created the queue
    /// and will be processed when the thread is alertable.
    /// </summary>
    AsyncQueueDispatchMode_FixedThread,

    /// <summary>
    /// Async callbacks are queued to the system thread pool and will
    /// be processed in order by the threadpool.
    /// </summary>
    AsyncQueueDispatchMode_ThreadPool

} AsyncQueueDispatchMode;
```

**workDispatchMode** 비동기 작업을 처리 하는 스레드에 대 한 디스패치 모드를 결정 하는 동안 **completionDispatchMode** 비동기 완료를 처리 하는 스레드에 대 한 디스패치 모드를 결정 합니다. 작업입니다.

호출할 수도 있습니다 **CreateSharedAsyncQueue** 만들려는 **AsyncQueue** 큐의 ID를 지정 하 여 동일한 큐 형식을 사용 하 여 합니다.

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> 이미 있으면이 ID를 만들어 디스패치하지 모드를 사용 하 여 큐에서 참조 됩니다.  그렇지 않으면 새 큐를 만들어집니다.

만든 후에 **AsyncQueue** 를 추가 하기만 합니다 **AsyncBlock** 작업 및 완료 함수에서 스레딩을 제어 하 합니다.
완료 될 때 사용 하는 **AsyncQueue**, 일반적으로 게임 종료 되 면 닫을 수 있습니다 사용 하 여 **CloseAsyncQueue**:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>수동으로 디스패치를 **AsyncQueue**

에 대 한 수동 큐 디스패치 모드를 사용 하는 경우는 **AsyncQueue** 수동으로 발송 해야 작업 또는 완료 큐입니다.
한다고 가정해 보겠습니다를 **AsyncQueue** 작업 큐와 완료 큐를 모두 수동으로 디스패치를 위해 설정 하는 위치를 만든 다음과 같이 합니다.

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

할당 된 작업을 디스패치 하려면 **AsyncQueueDispatchMode_Manual** 사용 하 여 발송 해야 합니다 **DispatchAsyncQueue** 함수입니다.

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *큐* -에서 작업을 디스패치 하는 큐
* *형식* -의 인스턴스를 **AsyncQueueCallbackType** 열거형
* *timeoutInMs* -제한 시간 (밀리초)에 대 한 uint32_t 합니다.

두 가지가 콜백 하 여 정의 된 **AsyncQueueCallbackType** 열거형:

```cpp
typedef enum AsyncQueueCallbackType
{
    /// <summary>
    /// Async work callbacks
    /// </summary>
    AsyncQueueCallbackType_Work,

    /// <summary>
    /// Completion callbacks after work is done
    /// </summary>
    AsyncQueueCallbackType_Completion
} AsyncQueueCallbackType;
```

### <a name="when-to-call-dispatchasyncqueue"></a>호출 하는 경우 **DispatchAsyncQueue**

큐에 받으면 새 확인 하기 위해 항목 호출할 수 있습니다 **AddAsyncQueueCallbackSubmitted** 코드 작업 또는 완료를 디스패치할 수 준비 되었는지 알 수 있도록 이벤트 처리기를 설정 합니다.

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted** 다음 매개 변수를 사용 합니다.

* *큐* -비동기 큐에 대 한 콜백을 제출 합니다.
* *상황에 맞는* -제출 콜백에 전달 되어야 하는 데이터에 대 한 포인터입니다.
* *콜백* -새 콜백 큐에 제출 되 면 호출 되는 함수입니다.
* *토큰* -이후의 호출에서 사용할 토큰 **RemoveAsynceCallbackSubmitted** 콜백을 제거 합니다.

예를 들어 호출 같습니다 **AddAsyncQueueCallbackSubmitted**:

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

해당 **AsyncQueueCallbackSubmitted** 콜백을 다음과 같이 구현할 수 있습니다.

```cpp
void CALLBACK HandleAsyncQueueCallback(
    _In_ void* context,
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type)
{
    switch (type)
    {
    case AsyncQueueCallbackType::AsyncQueueCallbackType_Work:
        ReleaseSemaphore(g_workReadyHandle, 1, nullptr);
        break;
    }
}
```

다음 호출에이 세마포를 대기할 수 백그라운드 스레드에서 **DispatchAsyncQueue**합니다.

```cpp
DWORD WINAPI BackgroundWorkThreadProc(LPVOID lpParam)
{
    HANDLE hEvents[2] =
    {
        g_workReadyHandle.get(),
        g_stopRequestedHandle.get()
    };

    async_queue_handle_t queue = static_cast<async_queue_handle_t>(lpParam);

    bool stop = false;
    while (!stop)
    {
        DWORD dwResult = WaitForMultipleObjectsEx(2, hEvents, false, INFINITE, false);
        switch (dwResult)
        {
        case WAIT_OBJECT_0: 
            // Background work is ready to be dispatched
            DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);
            break;

        case WAIT_OBJECT_0 + 1:
        default:
            stop = true;
            break;
        }
    }

    CloseAsyncQueue(queue);
    return 0;
}
```

것이 가장 좋습니다를 사용 하는 방법은 Win32 세마포 개체를 사용 하 여 구현 합니다.  구현 하는 경우 대신 Win32 이벤트 개체를 사용 하 여 확인 해야 놓치지 코드를 사용 하 여 모든 이벤트와 같은:

```cpp
    case WAIT_OBJECT_0: 
        // Background work is ready to be dispatched
        DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);        
        
        if (!IsAsyncQueueEmpty(queue, AsyncQueueCallbackType_Work))
        {
            // If there's more pending work, then set the event to process them
            SetEvent(g_workReadyHandle.get());
        }
        break;
```


예제에서 비동기 통합에 대 한 모범 사례를 보면 [소셜 C 샘플 AsyncIntegration.cpp](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp)

