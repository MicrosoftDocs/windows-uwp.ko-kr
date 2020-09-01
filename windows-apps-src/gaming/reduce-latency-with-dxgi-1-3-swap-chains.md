---
title: DXGI 1.3 스왑 체인으로 대기 시간 단축
description: 새 프레임 렌더링을 시작할 때 스왑 체인이 적절 한 시간에 신호를 보낼 때까지 대기 하 여 유효한 프레임 대기 시간을 줄이려면 DXGI 1.3를 사용 합니다.
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 대기 시간, dxgi, 스왑 체인, directx
ms.localizationpriority: medium
ms.openlocfilehash: 41d11865daadacf8ff90971836cab7cd941c4182
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163047"
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>DXGI 1.3 스왑 체인으로 대기 시간 단축

새 프레임 렌더링을 시작할 때 스왑 체인이 적절 한 시간에 신호를 보낼 때까지 대기 하 여 유효한 프레임 대기 시간을 줄이려면 DXGI 1.3를 사용 합니다. 게임은 게임에서 디스플레이를 업데이트 하 여 해당 입력에 응답 하는 경우 일반적으로 플레이어 입력을 받은 시간부터 가장 낮은 대기 시간을 제공 해야 합니다. 이 항목에서는 게임에서 효과적인 프레임 대기 시간을 최소화 하는 데 사용할 수 있는 Direct3D 11.2부터 사용할 수 있는 기술을 설명 합니다.

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>백 버퍼를 대기 하는 방법은 대기 시간을 줄이는 방법

대칭 이동 모델 교환 체인을 사용 하 여 게임에서 [**Idxgiswapchain::P 재전송**](/windows/win32/api/dxgi/nf-dxgi-idxgiswapchain-present)을 호출할 때마다 백 버퍼 "대칭 이동"이 대기 됩니다. 렌더링 루프가 Present ()를 호출 하면 시스템은 이전 프레임을 제시 하기 전까지 스레드를 차단 하 여 실제로 표시 되기 전에 새 프레임을 큐에 대기 시킬 수 있습니다. 이렇게 하면 게임에서 프레임을 그리는 시간과 시스템에서 프레임을 표시 하는 데 걸리는 시간 사이에 추가 대기 시간이 발생 합니다. 대부분의 경우,이 시스템은 게임에서 렌더링 되는 시간과 각 프레임을 표시 하는 시간 사이에 항상 전체 추가 프레임을 대기 하는 안정 된 상태에 도달 합니다. 시스템이 새 프레임을 수락할 준비가 될 때까지 기다린 다음 현재 데이터를 기반으로 프레임을 렌더링 하 고 프레임을 즉시 큐에 대기 하는 것이 좋습니다.

[**DXGI \_ 스왑 \_ 체인 \_ 플래그 \_ 프레임 \_ 대기 시간 \_ 대기 가능 \_ 개체**](/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) 플래그를 사용 하 여 대기 가능 스왑 체인을 만듭니다. 이러한 방식으로 만들어진 스왑 체인은 시스템이 실제로 새 프레임을 받아들일 준비가 되었을 때 렌더링 루프에 알릴 수 있습니다. 이렇게 하면 현재 데이터를 기반으로 게임을 렌더링 한 다음 결과를 현재 큐에 바로 배치할 수 있습니다.

## <a name="step-1-create-a-waitable-swap-chain"></a>1단계. 대기 가능 스왑 체인 만들기

[**CreateSwapChainForCoreWindow**](/windows/win32/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)를 호출할 때 [**DXGI \_ 스왑 \_ 체인 \_ 플래그 \_ 프레임 \_ 대기 시간 \_ 대기 가능 \_ 개체**](/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) 플래그를 지정 합니다.

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> [!NOTE]
> 일부 플래그와는 달리이 플래그는 [**ResizeBuffers**](/windows/win32/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)을 사용 하 여 추가 하거나 제거할 수 없습니다. 이 플래그가 스왑 체인을 만들 때와 다르게 설정 된 경우 DXGI는 오류 코드를 반환 합니다.

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## <a name="step-2-set-the-frame-latency"></a>2단계. 프레임 대기 시간 설정

[**IDXGIDevice1:: SetMaximumFrameLatency**](/windows/win32/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency)를 호출 하는 대신 [**IDXGISwapChain2:: SetMaximumFrameLatency**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-setmaximumframelatency) API를 사용 하 여 프레임 대기 시간을 설정 합니다.

기본적으로 대기 가능 스왑 체인의 프레임 대기 시간은 1로 설정 되며,이로 인해 대기 시간이 최소화 되지만 CPU GPU 병렬 처리도 줄어듭니다. 60 FPS를 얻기 위해 CPU-GPU 병렬 처리를 늘려야 하는 경우 즉, CPU 및 GPU에서 프레임 처리 렌더링 작업을 16.7 밀리초 미만으로 소비 하지만 결합 된 합계가 16.7 밀리초를 초과 하는 경우 프레임 대기 시간을 2로 설정 합니다. 이를 통해 GPU는 cpu가 현재 프레임에 대해 독립적으로 렌더링 명령을 전송 하도록 허용 하는 동시에, 이전 프레임에서 CPU에 의해 대기 중인 작업을 처리할 수 있습니다.

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>3단계. 스왑 체인에서 대기 가능 개체를 가져옵니다.

[**IDXGISwapChain2:: GetFrameLatencyWaitableObject**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject) 를 호출 하 여 대기 핸들을 검색 합니다. 대기 핸들은 대기 가능 개체에 대 한 포인터입니다. 렌더링 루프에서 사용 하기 위해이 핸들을 저장 합니다.

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>4단계. 각 프레임을 렌더링 하기 전에 대기

렌더링 루프는 모든 프레임 렌더링을 시작 하기 전에 대기 가능 개체를 통해 스왑 체인이 신호를 보낼 때까지 기다려야 합니다. 여기에는 스왑 체인으로 렌더링 된 첫 번째 프레임이 포함 됩니다. [**WaitForSingleObjectEx**](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobjectex)를 사용 하 여 2 단계에서 검색 된 대기 핸들을 제공 하 여 각 프레임의 시작을 신호로 알립니다.

다음 예제에서는 DirectXLatency 샘플의 렌더링 루프를 보여 줍니다.

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

다음 예제에서는 DirectXLatency 샘플에서 WaitForSingleObjectEx 호출을 보여 줍니다.

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>스왑 체인이 표시 될 때까지 대기 하는 동안 게임을 수행 해야 하는 것은 무엇 인가요?

게임에 렌더링 루프에서 차단 되는 작업이 없는 경우 스왑 체인이 표시 될 때까지 대기 하는 것은 전원을 절약 하 고 모바일 장치에서 특히 중요 한 것 이기 때문에 유용할 수 있습니다. 그렇지 않으면 교체가 체인이 표시 될 때까지 대기 하는 동안 다중 스레딩을 사용 하 여 작업을 수행할 수 있습니다. 게임에서 완료할 수 있는 몇 가지 작업은 다음과 같습니다.

-   네트워크 이벤트 처리
-   AI 업데이트
-   CPU 기반 물리
-   지연 된 컨텍스트 렌더링 (지원 되는 장치)
-   자산 로드

Windows의 다중 스레드 프로그래밍에 대 한 자세한 내용은 다음 관련 항목을 참조 하십시오.

## <a name="related-topics"></a>관련 항목

* [DirectX 대기 시간 샘플 응용 프로그램](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/DirectX%20latency%20sample)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject)
* [**WaitForSingleObjectEx**](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [**Windows.System.Threading**](/uwp/api/Windows.System.Threading)
* [C + +의 비동기 프로그래밍](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)
* [프로세스 및 스레드](/windows/win32/procthread/processes-and-threads)
* [동기화](/windows/win32/sync/synchronization)
* [이벤트 개체 사용 (Windows)](/windows/win32/sync/using-event-objects)