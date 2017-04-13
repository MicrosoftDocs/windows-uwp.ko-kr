---
author: mtoepke
title: "DXGI 1.3 스왑 체인으로 대기 시간 단축"
description: "DXGI 1.3을 사용하여 스왑 체인이 새로운 프레임 렌더링을 시작할 적절한 시간을 신호로 보낼 때까지 기다려 효과적인 프레임 지연을 줄입니다."
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 게임, 대기 시간, dxgi, 스왑 체인, directx"
ms.openlocfilehash: 9f2babdac40e3baf27bec9b2e214e9350d1f2539
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>DXGI 1.3 스왑 체인으로 대기 시간 단축


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

DXGI 1.3을 사용하여 스왑 체인이 새로운 프레임 렌더링을 시작할 적절한 시간을 신호로 보낼 때까지 기다려 효과적인 프레임 지연을 줄입니다. 일반적으로 게임에서는 플레이어 입력을 받은 시간부터 게임이 해당 입력에 응답하여 디스플레이를 업데이트할 때까지 대기 시간이 최대한 적어야 합니다. 이 항목에서는 게임에서 효과적인 프레임 지연을 최소화하는 데 사용할 수 있도록 Direct3D 11.2부터 제공되는 기술에 대해 설명합니다.

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>백 버퍼에서 대기할 때 대기 시간을 어떻게 줄입니까?


전환 모델 스왑 체인을 사용하면 게임에서 [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576)를 호출할 때마다 백 버퍼 "전환"이 대기 중입니다. 렌더링 루프에서 Present()를 호출하면 시스템에서 이전 프레임을 제공할 때까지 스레드를 차단하여 실제로 제공되기 전에 새 프레임을 대기할 공간을 만듭니다. 따라서 게임에서 프레임을 그리는 시간과 시스템에서 해당 프레임을 표시할 수 있는 시간 사이에 추가 대기 시간이 발생합니다. 대부분의 경우 시스템이 안정적인 평형 상태에 도달하여 게임이 렌더링하는 시간과 각 프레임을 제공하는 시간 사이에서 거의 모든 추가 프레임을 항상 대기합니다. 시스템이 새 프레임을 허용할 준비가 될 때까지 대기한 다음 현재 데이터에 따라 프레임을 렌더링하고 프레임을 바로 대기하는 것이 좋습니다.

[**DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT**](https://msdn.microsoft.com/library/windows/desktop/bb173076) 플래그를 사용하여 대기 가능 스왑 체인을 만듭니다. 이 방법으로 만든 스왑 체인은 시스템이 실제로 새 프레임을 허용할 준비가 되면 렌더링 루프에 알릴 수 있습니다. 그러면 게임에서 현재 데이터에 따라 렌더링한 다음 결과를 현재 대기열에 바로 넣을 수 있습니다.

## <a name="step-1-create-a-waitable-swap-chain"></a>1단계: 대기 가능 스왑 체인 만들기


[**CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559)를 호출할 때 [**DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT**](https://msdn.microsoft.com/library/windows/desktop/bb173076) 플래그를 지정합니다.

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> **참고**   일부 플래그와 달리 이 플래그는 [**ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577)를 사용하여 추가하거나 제거할 수 없습니다. 이 플래그가 스왑 체인을 만들 때와 다르게 설정되면 DXGI에서 오류 코드를 반환합니다.

 

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

## <a name="step-2-set-the-frame-latency"></a>2단계: 프레임 지연 설정


[**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334)를 호출하는 대신 [**IDXGISwapChain2::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/dn268313) API를 사용하여 프레임 지연을 설정합니다.

기본적으로 대기 가능 스왑 체인의 프레임 지연은 1로 설정되어 대기 시간이 최대한 적을 뿐만 아니라 CPU GPU 병렬 처리도 줄어듭니다. 60FPS를 얻기 위해 향상된 CPU-GPU 병렬 처리가 필요한 경우 즉, CPU 및 GPU가 각각 렌더링 작업을 처리하는 데 프레임당 16.7ms보다 적게 걸리지만 합계가 16.7ms보다 큰 경우 프레임 지연을 2로 설정합니다. 따라서 GPU는 이전 프레임 중에 CPU에서 대기한 작업을 처리할 수 있으며, 동시에 CPU에서 현재 프레임에 대해 독립적으로 렌더링 명령을 제출하도록 할 수 있습니다.

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

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>3단계: 스왑 체인에서 대기 가능 개체 가져오기


[**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://msdn.microsoft.com/library/windows/desktop/dn268309)를 호출하여 대기 핸들을 검색합니다. 대기 핸들은 대기 가능 개체에 대한 포인터입니다. 이 핸들을 렌더링 루프에서 사용하도록 저장합니다.

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>4단계: 각 프레임을 렌더링하기 전에 대기


렌더링 루프는 스왑 체인이 대기 가능 개체를 통해 신호를 보낼 때까지 대기한 후 모든 프레임의 렌더링을 시작해야 합니다. 여기에는 스왑 체인으로 렌더링된 첫 번째 프레임이 포함됩니다. [**WaitForSingleObjectEx**](https://msdn.microsoft.com/library/windows/desktop/ms687036)를 사용하여 2단계에서 검색된 대기 핸들을 제공하고 각 프레임의 시작에 신호를 보냅니다.

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

다음 예제에서는 DirectXLatency 샘플의 WaitForSingleObjectEx 호출을 보여 줍니다.

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

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>스왑 체인이 제공할 때까지 대기하는 동안 게임에서 수행해야 할 작업은 무엇입니까?


게임에 렌더링 루프를 차단하는 작업이 없는 경우 스왑 체인이 제공할 때까지 대기하도록 하면 전원을 절약할 수 있으므로 도움이 되며, 특히 모바일 장치에서는 더욱 중요합니다. 그렇지 않으면 게임에서 스왑 체인이 제공할 때까지 대기하는 동안 다중 스레딩을 사용하여 작업을 수행할 수 없습니다. 다음은 게임에서 완료할 수 있는 몇 가지 작업입니다.

-   네트워크 이벤트 처리
-   AI 업데이트
-   CPU 기반 물리학
-   지원되는 장치에서 지연된 컨텍스트 렌더링
-   자산 로드

Windows의 다중 스레드 프로그래밍에 대한 자세한 내용은 다음 관련 항목을 참조하세요.

## <a name="related-topics"></a>관련 항목


* [DirectXLatency 샘플](http://go.microsoft.com/fwlink/p/?LinkID=317361)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://msdn.microsoft.com/library/windows/desktop/dn268309)
* [**WaitForSingleObjectEx**](https://msdn.microsoft.com/library/windows/desktop/ms687036)
* [**Windows.System.Threading**](https://msdn.microsoft.com/library/windows/apps/br229642)
* [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)
* [프로세스 및 스레드](https://msdn.microsoft.com/library/windows/desktop/ms684841)
* [동기화](https://msdn.microsoft.com/library/windows/desktop/ms686353)
* [이벤트 개체 사용(Windows)](https://msdn.microsoft.com/library/windows/desktop/ms686915)

 

 




