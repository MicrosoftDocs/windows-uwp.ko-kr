---
title: 비동기 C API 호출 패턴
author: aablackm
description: 비동기 C API 패턴 XSAPI C API에 대 한 호출에 대해 알아봅니다.
ms.author: aablackm
ms.date: 06/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 개발자 프로그램
ms.localizationpriority: medium
ms.openlocfilehash: 50d747128dcd85a16c5250997e9431b279203ae0
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4391734"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>XSAPI에 대 한 호출 패턴 평면 C 계층 비동기 호출

**비동기 API** 가 신속 하 게 반환 하지만 **비동기 작업** 만들기를 시작 하는 API 및 결과 작업이 완료 되 면 반환 됩니다.

일반적으로 게임에 거의 제어 스레드를 통해 **비동기 작업** 을 실행 하 고 스레드 **완료 콜백에서**사용 하는 경우 결과 반환 합니다.  일부 게임은 힙의 섹션에는 스레드 동기화 하는 단일 스레드 접촉만 되도록 설계 되었습니다. **완료 콜백에서** 스레드에서 호출 되지 않습니다 **비동기 작업** 의 결과 사용 하 여 공유 상태를 업데이트 하 고 게임 컨트롤, 스레드 동기화를 해야 합니다.

XSAPI C API **XblSocialGetSocialRelationshipsAsync()**, **XblProfileGetUserProfileAsync()** 및 **같은 호출 **비동기 API** 를 만들 때 개발자 직접 스레드 제어를 제공 하는 새로운 비동기 C API를 노출 합니다. XblAchievementsGetAchievementsForTitleIdAsync()**.

**XblProfileGetUserProfileAsync** API 호출 하는 기본 예제는 다음과 같습니다.

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

이 호출 패턴을 이해 하려면 **AsyncBlock** 및 **AsyncQueue**를 사용 하는 방법을 이해 해야 합니다.

* **AsyncBlock** 모든 **비동기 작업** 을 **완료 콜백의**관련 정보를 전달 합니다.

* **AsyncQueue** AsyncBlock의 **모든 완료 콜백이**호출 하는 스레드 및 **비동기 작업** 을 실행 한 스레드를 확인할 수 있습니다 수 있습니다.

## <a name="the-asyncblock"></a>**AsyncBlock**

**AsyncBlock** 에 대해 자세히 살펴보겠습니다. 다음과 같이 정의 된 구조체입니다.

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

**AsyncBlock** 포함 되어 있습니다.

* *콜백* -는 선택적 콜백 함수는 비동기 작업이 완료 된 후에 호출 됩니다.  콜백의 지정 하지 않으면 **AsyncBlock** **GetAsyncStatus** 를 완료 한 다음 결과 얻습니다을 기다릴 수 있습니다.
* *상황에 맞* 는-를 사용 하면 데이터 콜백 함수를 전달할 수 있습니다.
* *큐* -핸들 되는 async_queue_handle_t **AsyncQueue**를 지정 합니다. 설정 되지 않은 경우 기본 큐 사용 됩니다.

각 비동기 API 호출에 대 한 힙에서 새 AsyncBlock 만들어야 합니다.  AsyncBlock AsyncBlock의 모든 완료 콜백이 호출 되 고 삭제 한 다음 될 때까지 live 해야 합니다.

> [!IMPORTANT]
> **비동기 작업이** 완료 될 때까지 **AsyncBlock** 는 메모리에 남아 있어야 합니다. 동적으로 할당 AsyncBlock의 **완료 콜백에서**내에서 삭제할 수 있습니다.

### <a name="waiting-for-asynchronous-task"></a>**비동기 작업** 을 기다리는

완료 되는 **비동기 작업** 을 알 수 다양 한 다른 방법:

* AsyncBlock의 **모든 완료 콜백이** 호출 되어
* 완료 될 때까지 기다릴 true로 **GetAsyncStatus** 를 호출 합니다.
* **AsyncBlock** 에 waitEvent를 설정 하 고 신호 이벤트 대기

**하지만 GetAsyncStatus** 및 waitEvent **비동기 작업이** 완료 후 간주 됩니다는 AsyncBlock의 **완료 콜백에서** 실행 AsyncBlock의 **완료 콜백에서** 선택 사항입니다.

**비동기 작업이** 완료 되 면 결과 얻을 수 있습니다.

### <a name="getting-the-result-of-the-asynchronous-task"></a>**비동기 작업** 의 결과 가져오기

결과 얻기 위해 대부분의 **비동기 API** 함수 Function\의 해당 \[Name 있는] 결과 함수는 비동기 호출의 결과 받을 수 있습니다. 이 예제 코드 **XblProfileGetUserProfileAsync** 해당 **XblProfileGetUserProfileResult** 함수를 포함 합니다. 이 함수는 함수의 결과 반환 하 고 그에 따라 역할을 사용할 수 있습니다.  자세한 내용은 각 **비동기 API** 함수의 documention 검색 결과에 참조 하세요.

## <a name="the-asyncqueue"></a>**AsyncQueue**

**AsyncQueue** AsyncBlock의 **모든 완료 콜백이**호출 하는 스레드 및 **비동기 작업** 을 실행 한 스레드를 확인할 수 있습니다 수 있습니다.

*디스패치 모드*를 설정 하 여 이러한 작업을 수행 하는 스레드를 제어할 수 있습니다. 세 가지 디스패치 모드는 사용할 수 있습니다.

* *수동* -수동 큐는 자동으로 발송 하지 않습니다.  개발자가 원하는 모든 스레드에서 디스패치합니다 수입니다. 특정 스레드에 비동기 호출의 작업 또는 콜백 측면을 할당 하려면이 사용할 수 있습니다.  이 아래에 자세히 설명 합니다.

* *스레드 풀* -디스패치 스레드 풀을 사용 합니다.  스레드 풀 threadpool 스레드를 사용할 수 있는 되 면 큐에서 다시 실행 호출에 대해 동시에 호출을 호출 합니다.  사용 하 여 easist 이지만 최소한의 스레드 사용 되는 컨트롤을 제공 합니다.

* *고정 스레드* -QueueUserAPC를 사용 하 여 비동기 큐를 만든 스레드 디스패치. 사용자 모드 APC, 대기 중인 경우 스레드 하지 APC 함수를 호출 하 여 경고 상태가 아니면로 리디렉션됩니다. 경고 대기 작업을 수행 하도록 **SleepEx**, **SignalObjectAndWait**, **WaitForSingleObjectEx**, **WaitForMultipleObjectsEx**또는 **MsgWaitForMultipleObjectsEx** 를 사용 하 여 상태가 경고 스레드

새 **AsyncQueue** 만들려면 **CreateAsyncQueue**호출 해야 합니다.

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

여기서 AsyncQueueDispatchMode 앞에 도입 된 세 개의 디스패치 모드를 포함 합니다.

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

**workDispatchMode** **completionDispatchMode** 비동기 작업의 완료를 처리 하는 스레드에 대 한 디스패치 모드를 결정 하는 동안 비동기 작업을 처리 하는 스레드에 대 한 디스패치 모드를 결정 합니다.

큐의 ID를 지정 하 여 동일한 큐 종류는 **AsyncQueue** 만드는 **CreateSharedAsyncQueue** 호출할 수도 있습니다.

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> 이미 있는 경우이 ID 및 디스패치 모드를 사용 하 여 큐, 참조 됩니다.  그렇지 않은 경우 새 큐 생성 됩니다.

**AsyncQueue** 에 간단 하 게 만든 후 작업 및 완료 함수에서 스레딩 제어 하려면 **AsyncBlock** 에 추가 합니다.
**AsyncQueue**사용이 끝나면 일반적으로 게임을 종료할 때 닫을 수 있습니다 **CloseAsyncQueue**사용:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>수동으로 디스패치 하는 **AsyncQueue**

수동 큐 디스패치 모드를 사용 하는 **AsyncQueue** 작업 또는 완료 큐에 대 한 경우 수동으로 디스패치 해야 합니다.
작업 큐와 완료 큐 수동으로 발송 설정 되는 **AsyncQueue** 생성 된 가정해 같이:

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

디스패치 **AsyncQueueDispatchMode_Manual** 할당 된 작업 하기 위해 **DispatchAsyncQueue** 함수를 사용 하 여 디스패치 해야 합니다.

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *큐* -작업에서 디스패치 하는 큐
* *유형* - **AsyncQueueCallbackType** 열거형의 인스턴스
* *timeoutInMs* -밀리초에서 시간 제한에 대 한 uint32_t 합니다.

두 가지가 콜백 **AsyncQueueCallbackType** 열거형으로 정의 합니다.

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

### <a name="when-to-call-dispatchasyncqueue"></a>**DispatchAsyncQueue** 를 호출 하는 경우

큐가 새 항목을 받을 때 확인 하기 위해 이벤트 처리기 코드 작업 또는 완료 발송 될 준비가 되었는지 알 수 있도록 설정 하려면 **AddAsyncQueueCallbackSubmitted** 호출할 수 있습니다.

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted** 다음 매개 변수를 사용 합니다.

* *큐* -비동기 큐에 대 한 콜백을 제출 합니다.
* *상황에 맞* 는-전송 콜백을 전달 되는 데이터에 대 한 포인터.
* *콜백* -새 콜백 큐에 전송 될 때 호출 되는 함수입니다.
* *토큰* -토큰에 해당 콜백을 제거할 **RemoveAsynceCallbackSubmitted** 이후 호출에 사용 됩니다.

예를 들어 다음은 **AddAsyncQueueCallbackSubmitted**호출이입니다.

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

다음과 같이 해당 **AsyncQueueCallbackSubmitted** 콜백을 구현할 수 있습니다.

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

그런 다음 백그라운드 스레드에서 수신 대기할 수 있습니다 **DispatchAsyncQueue**호출에이 세마포의 합니다.

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

이 가장 좋습니다 Win32 세마포의 개체를 사용 하 여 구현 합니다.  구현 하는 경우 대신 Win32 이벤트 개체를 사용 하 여 데 필요한 다음 잊지 코드를 사용 하 여 모든 이벤트와 같은:

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


[소셜 C 샘플 AsyncIntegration.cpp](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp) 에 비동기 통합에 대 한 모범 사례의 예를 볼 수 있습니다.

