---
title: 스왑 체인 확장 및 오버레이
description: 모바일 디바이스에서 보다 신속한 렌더링을 위해 크기 조정된 스왑 체인을 만들고 오버레이 스왑 체인(사용 가능한 경우)을 사용하여 시각적 품질을 향상시키는 방법을 알아봅니다.
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 스왑 체인 크기 조정, 오버레이, directx
ms.localizationpriority: medium
ms.openlocfilehash: 12aede6c4af61c4b86d1f1090a2ec3d0e5ecce68
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476723"
---
# <a name="swap-chain-scaling-and-overlays"></a>스왑 체인 크기 조정 및 오버레이



모바일 디바이스에서 보다 신속한 렌더링을 위해 크기 조정된 스왑 체인을 만들고 오버레이 스왑 체인(사용 가능한 경우)을 사용하여 시각적 품질을 향상시키는 방법을 알아봅니다.

## <a name="swap-chains-in-directx-112"></a>DirectX 11.2의 스왑 체인


Direct3D 11.2에서는 기본이 아닌 축소된 해상도에서 확대되는 스왑 체인으로 UWP(유니버설 Windows 플랫폼) 앱을 만들어 채우기 속도를 향상시킬 수 있습니다. Direct3D 11.2에는 하드웨이 오버레이로 렌더링하기 위한 API도 포함되어 있어 다른 스왑 체인의 UI를 기본 해상도로 제공할 수 있습니다. 따라서 게임에서 높은 프레임 속도를 유지하면서 전체 기본 해상도로 UI를 그릴 수 있으므로 모바일 장치 및 높은 DPI 디스플레이(예: 3840 x 2160)를 최대한 활용할 수 있습니다. 이 문서에서는 겹치는 스왑 체인을 사용하는 방법에 대해 설명 합니다.

또한 Direct3D 11.2에서는 전환 모델 스왑 체인으로 대기 시간을 단축하는 새로운 기능을 제공합니다. [DXGI 1.3 스왑 체인으로 대기 시간 단축](reduce-latency-with-dxgi-1-3-swap-chains.md)을 참조하세요.

## <a name="use-swap-chain-scaling"></a>스왑 체인 크기 조정 사용


게임이 하위 수준 하드웨어 또는 절전에 최적화된 하드웨어에서 실행되는 경우 디스플레이에서 기본적으로 가능한 것보다 낮은 해상도에서 실시간 게임 콘텐츠를 렌더링하는 것이 좋을 수 있습니다. 이렇게 하려면 게임 콘텐츠 렌더링에 사용되는 스왑 체인이 기본 해상도보다 작거나 스왑 체인의 하위 영역을 사용해야 합니다.

1.  먼저 전체 기본 해상도로 스왑 체인을 만듭니다.

    ```cpp
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

    swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
    swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
    swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
    swapChainDesc.Stereo = false;
    swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
    swapChainDesc.SampleDesc.Quality = 0;
    swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
    swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
    swapChainDesc.Flags = 0;
    swapChainDesc.Scaling = DXGI_SCALING_STRETCH;

    // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
    ComPtr<IDXGIDevice3> dxgiDevice;
    DX::ThrowIfFailed(
        m_d3dDevice.As(&dxgiDevice)
        );

    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );

    ComPtr<IDXGIFactory2> dxgiFactory;
    DX::ThrowIfFailed(
        dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
        );

    ComPtr<IDXGISwapChain1> swapChain;
    DX::ThrowIfFailed(
        dxgiFactory->CreateSwapChainForCoreWindow(
            m_d3dDevice.Get(),
            reinterpret_cast<IUnknown*>(m_window.Get()),
            &swapChainDesc,
            nullptr,
            &swapChain
            )
        );

    DX::ThrowIfFailed(
        swapChain.As(&m_swapChain)
        );
    ```

2.  그런 다음 원본 크기를 축소된 해상도로 설정하여 확대할 스왑 체인의 하위 영역을 선택합니다.

    DX 포그라운드 스왑 체인 샘플에서는 비율에 따라 축소된 크기를 계산합니다.

    ```cpp
    m_d3dRenderSizePercentage = percentage;

    UINT renderWidth = static_cast<UINT>(m_d3dRenderTargetSize.Width * percentage + 0.5f);
    UINT renderHeight = static_cast<UINT>(m_d3dRenderTargetSize.Height * percentage + 0.5f);

    // Change the region of the swap chain that will be presented to the screen.
    DX::ThrowIfFailed(
        m_swapChain->SetSourceSize(
            renderWidth,
            renderHeight
            )
        );
    ```

3.  스왑 체인의 하위 영역에 맞게 뷰포트를 만듭니다.

    ```cpp
    // In Direct3D, change the Viewport to match the region of the swap
    // chain that will now be presented from.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        static_cast<float>(renderWidth),
        static_cast<float>(renderHeight)
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

4.  Direct2D를 사용하는 경우 회전 변형을 조정하여 원본 영역을 보완해야 합니다.

## <a name="create-a-hardware-overlay-swap-chain-for-ui-elements"></a>UI 요소에 대한 하드웨어 오버레이 스왑 체인 만들기


스왑 체인 크기 조정을 사용하는 경우 UI 크기도 축소되어 흐리게 표시되므로 사용하기 어려울 수 있다는 단점이 내재되어 있습니다. 하드웨어가 오버레이 스왑 체인을 지원하는 장치에서 이 문제는 실시간 게임 콘텐츠와 구분되는 UI를 스왑 체인에서 기본 해상도로 렌더링하면 완전히 해결됩니다. 이 기술은 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 스왑 체인에만 적용되며 XAML interop에서는 사용할 수 없습니다.

다음 단계를 사용하여 하드웨어 오버레이 기능을 사용하는 포그라운드 스왑 체인을 만듭니다. 이러한 단계는 위에서 설명한 실시간 게임 콘텐츠에 대한 스왑 체인을 먼저 만든 후에 수행합니다.

1.  먼저 DXGI 어댑터가 오버레이를 지원하는지 여부를 확인합니다. 스왑 체인에서 DXGI 출력 어댑터를 가져옵니다.

    ```cpp
    ComPtr<IDXGIAdapter> outputDxgiAdapter;
    DX::ThrowIfFailed(
        dxgiFactory->EnumAdapters(0, &outputDxgiAdapter)
        );

    ComPtr<IDXGIOutput> dxgiOutput;
    DX::ThrowIfFailed(
        outputDxgiAdapter->EnumOutputs(0, &dxgiOutput)
        );

    ComPtr<IDXGIOutput2> dxgiOutput2;
    DX::ThrowIfFailed(
        dxgiOutput.As(&dxgiOutput2)
        );
    ```

    DXGI 어댑터는 출력 어댑터가 [**SupportsOverlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411)에 대해 True를 반환하는 경우 오버레이를 지원합니다.

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **참고**  DXGI 어댑터가 오버레이 지 원하는 경우 다음 단계로 넘어갑니다. 디바이스가 오버레이를 지원하지 않는 경우에는 여러 스왑 체인을 사용하는 렌더링이 효율적이지 않습니다. 대신 실시간 게임 콘텐츠와 동일한 스왑 체인에서 축소된 해상도로 UI를 렌더링합니다.

     

2.  [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559)로 포그라운드 스왑 체인을 만듭니다. 다음 옵션은 *pDesc* 매개 변수에 제공된 [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)에서 설정해야 합니다.

    -   [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) 스왑 체인 플래그를 지정하여 포그라운드 스왑 체인을 나타냅니다.
    -   [**DXGI\_ALPHA\_MODE\_PREMULTIPLIED**](https://msdn.microsoft.com/library/windows/desktop/hh404496) 알파 모드 플래그를 사용합니다. 포그라운드 스왑 체인은 항상 프리멀티플라이됩니다.
    -   [**DXGI\_SCALING\_NONE**](https://msdn.microsoft.com/library/windows/desktop/hh404526) 플래그를 설정합니다. 포그라운드 스왑 체인은 항상 기본 해상도에서 실행됩니다.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **참고**  스왑 체인 크기를 조정할 때마다 [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) 를 다시 설정 합니다.

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  스왑 체인을 두 개 사용하고 있는 경우 최대 프레임 지연을 2로 늘려 DXGI 어댑터가 두 스왑 체인을 동일한 VSync 간격 내에서 동시에 제공할 시간을 제공합니다.

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

4.  포그라운드 스왑 체인은 항상 프리멀티플라이된 알파를 사용합니다. 각 픽셀의 색 값은 프레임을 표시하기 전에 이미 알파 값이 곱해져 있어야 합니다. 예를 들어 50% 알파의 100% 흰색 BGRA 픽셀은 (0.5, 0.5, 0.5, 0.5)로 설정됩니다.

    알파 미리 곱하기 단계는 [**D3D11\_RENDER\_TARGET\_BLEND\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476200) 구조의 **SrcBlend** 필드가 **D3D11\_SRC\_ALPHA**로 설정된 앱 혼합 상태([**ID3D11BlendState**](https://msdn.microsoft.com/library/windows/desktop/ff476349) 참조)를 적용하여 출력 병합 단계에서 수행할 수 있습니다. 프리멀티플라이된 알파 값이 있는 자산을 사용할 수도 있습니다.

    알파 미리 곱하기 단계를 수행하지 않으면 포그라운드 스왑 체인의 색이 예상보다 밝아집니다.

5.  포그라운드 스왑 체인을 만들었는지 여부에 따라 UI 요소의 Direct2D 그리기 화면을 포그라운드 스왑 체인에 연결할 수 있습니다.

    렌더링 대상 보기 만들기:

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

    Direct2D 그리기 화면 만들기:

    ```cpp
    if (m_foregroundSwapChain)
    {
        // Create a Direct2D target bitmap for the foreground swap chain.
        ComPtr<IDXGISurface2> dxgiForegroundSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiForegroundSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiForegroundSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }
    else
    {
        // Create a Direct2D target bitmap for the swap chain.
        ComPtr<IDXGISurface2> dxgiSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
    ```

    ```cpp
    // Create a render target view of the swap chain's back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );
    ```

6.  실시간 게임 콘텐츠에 사용된 크기 조정된 스왑 체인과 함께 포그라운드 스왑 체인을 제공합니다. 두 스왑 체인에 대해 프레임 지연을 2로 설정했으므로 DXGI가 동일한 VSync 간격 내에서 두 스왑 체인을 제공할 수 있습니다.

    ```cpp
    // Present the contents of the swap chain to the screen.
    void DX::DeviceResources::Present()
    {
        // The first argument instructs DXGI to block until VSync, putting the application
        // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
        // frames that will never be displayed to the screen.
        HRESULT hr = m_swapChain->Present(1, 0);

        if (SUCCEEDED(hr) && m_foregroundSwapChain)
        {
            m_foregroundSwapChain->Present(1, 0);
        }

        // Discard the contents of the render targets.
        // This is a valid operation only when the existing contents will be entirely
        // overwritten. If dirty or scroll rects are used, this call should be removed.
        m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());
        if (m_foregroundSwapChain)
        {
            m_d3dContext->DiscardView(m_d3dForegroundRenderTargetView.Get());
        }

        // Discard the contents of the depth stencil.
        m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

        // If the device was removed either by a disconnection or a driver upgrade, we
        // must recreate all device resources.
        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            HandleDeviceLost();
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    ```

 

 




