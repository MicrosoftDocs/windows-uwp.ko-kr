---
title: 스왑 체인 확장 및 오버레이
description: 모바일 장치에서 더 빠르게 렌더링 하기 위해 크기가 조정 된 스왑 체인을 만들고 오버레이 스왑 체인 (사용 가능한 경우)을 사용 하 여 시각적 품질을 높이는 방법에 대해 알아봅니다.
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 스왑 체인 크기 조정, 오버레이, directx
ms.localizationpriority: medium
ms.openlocfilehash: ade5812999a3fe085a7c2091363857d7eefa1870
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165167"
---
# <a name="swap-chain-scaling-and-overlays"></a>스왑 체인 확장 및 오버레이



모바일 장치에서 더 빠르게 렌더링 하기 위해 크기가 조정 된 스왑 체인을 만들고 오버레이 스왑 체인 (사용 가능한 경우)을 사용 하 여 시각적 품질을 높이는 방법에 대해 알아봅니다.

## <a name="swap-chains-in-directx-112"></a>DirectX 11.2의 스왑 체인


Direct3D 11.2에서는 기본이 아닌 (감소) 해상도에서 확장 된 스왑 체인을 사용 하 여 UWP (유니버설 Windows 플랫폼) 앱을 만들 수 있으므로 빠른 채우기 속도를 사용할 수 있습니다. Direct3D 11.2에는 네이티브 해상도에서 다른 스왑 체인에 UI를 제공할 수 있도록 하드웨어 오버레이를 사용 하 여 렌더링 하기 위한 Api도 포함 되어 있습니다. 이를 통해 게임을 통해 높은 수준의 프레임을 유지 하면서 전체 기본 해상도로 UI를 그릴 수 있으므로 모바일 장치 및 높은 DPI 디스플레이 (예: 3840 x 2160)를 최대한 활용할 수 있습니다. 이 문서에서는 겹치는 스왑 체인을 사용 하는 방법을 설명 합니다.

Direct3D 11.2에는 대칭 전환 모델 스왑 체인의 대기 시간을 줄이는 새로운 기능도 도입 되었습니다. [DXGI 1.3 스왑 체인으로 대기 시간 감소를](reduce-latency-with-dxgi-1-3-swap-chains.md)참조 하세요.

## <a name="use-swap-chain-scaling"></a>스왑 체인 배율 사용


게임이 하위 하드웨어에서 실행 되는 경우 또는 전원 절약을 위해 최적화 된 하드웨어에서 실행 되는 경우에는 디스플레이가 기본적으로 사용할 수 있는 것 보다 낮은 해상도로 실시간 게임 콘텐츠를 렌더링 하는 것이 유용할 수 있습니다. 이렇게 하려면 게임 콘텐츠를 렌더링 하는 데 사용 되는 스왑 체인이 네이티브 해상도 보다 작거나이 swapchain present의 하위 영역이 사용 되어야 합니다.

1.  먼저 전체 네이티브 해상도에서 스왑 체인을 만듭니다.

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

2.  그런 다음 원본 크기를 축소 된 해상도로 설정 하 여 확장할 스왑 체인의 하위 영역을 선택 합니다.

    DX 전경 스왑 체인 샘플은 백분율을 기준으로 축소 된 크기를 계산 합니다.

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

3.  스왑 체인의 하위 영역과 일치 하는 뷰포트를 만듭니다.

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

4.  Direct2D를 사용 하는 경우 원본 영역에 맞게 보정 하려면 회전 변환을 조정 해야 합니다.

## <a name="create-a-hardware-overlay-swap-chain-for-ui-elements"></a>UI 요소에 대 한 하드웨어 오버레이 스왑 체인 만들기


스왑 체인 크기 조정을 사용 하는 경우 UI도 축소 되어 희미 하 고 사용 하기가 어려울 수 있다는 점에서 내재 된 단점이 있습니다. 오버레이 스왑 체인에 대 한 하드웨어 지원이 있는 장치에서이 문제는 실시간 게임 콘텐츠와는 별개의 교환 체인에서 기본 해상도로 UI를 렌더링 하 여 완전히 완화 됩니다. 이 기법은 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 스왑 체인에만 적용 되며 XAML interop에서 사용할 수 없습니다.

하드웨어 오버레이 기능을 사용 하는 전경 스왑 체인을 만들려면 다음 단계를 수행 합니다. 이러한 단계는 위에서 설명한 대로 실시간 게임 콘텐츠에 대 한 스왑 체인을 먼저 만든 후에 수행 됩니다.

1.  먼저 DXGI 어댑터가 오버레이를 지원 하는지 확인 합니다. 스왑 체인에서 DXGI 출력 어댑터를 가져옵니다.

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

    출력 어댑터가 [**SupportsOverlays**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays)에 대해 True를 반환 하는 경우 DXGI 어댑터는 오버레이를 지원 합니다.

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **참고**    DXGI 어댑터가 오버레이를 지 원하는 경우 다음 단계를 계속 합니다. 장치에서 오버레이를 지원 하지 않는 경우 여러 스왑 체인으로 렌더링 하는 것은 효율적이 지 않습니다. 대신 실시간 게임 콘텐츠와 동일한 스왑 체인의 축소 된 해상도로 UI를 렌더링 합니다.

     

2.  [**IDXGIFactory2:: CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)를 사용 하 여 전경 스왑 체인을 만듭니다. *Pdesc* 매개 변수에 제공 된 [**DXGI \_ 스왑 \_ 체인 \_ DESC1**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) 에서 다음 옵션을 설정 해야 합니다.

    -   전경 스왑 체인을 나타내기 위해 [**DXGI \_ 스왑 \_ 체인 \_ 플래그 \_ 전경 \_ 계층**](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) 스왑 체인 플래그를 지정 합니다.
    -   [**DXGI \_ 알파 \_ 모드 \_ 미리**](/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_alpha_mode) 설정 된 알파 모드 플래그를 사용 합니다. 전경 스왑 체인은 항상 미리 곱한 것입니다.
    -   [**DXGI \_ 배율 \_ NONE**](/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_scaling) 플래그를 설정 합니다. 전경 스왑 체인은 항상 네이티브 해상도로 실행 됩니다.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **참고**    스왑 체인의 크기를 조정할 때마다 [**DXGI \_ 스왑 \_ 체인 \_ 플래그 \_ 포그라운드 \_ 계층**](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) 을 다시 설정 합니다.

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  두 스왑 체인이 사용 되는 경우 DXGI 어댑터가 두 스왑 체인을 동시에 제공할 시간 (동일한 VSync 간격 내)을 갖도록 최대 프레임 대기 시간을 2로 늘립니다.

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

4.  전경 스왑 체인은 항상 미리 곱한 알파를 사용 합니다. 각 픽셀의 색 값은 프레임을 표시 하기 전에 이미 알파 값을 곱해 야 합니다. 예를 들어 50% alpha에서 100% 흰색 BGRA 픽셀은 (0.5, 0.5, 0.5, 0.5)로 설정 됩니다.

    알파 사전 곱셈 단계는 [**D3D11 \_ 렌더링 \_ 대상 \_ Blend \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_blend_desc) 구조체의 **srcblend** 필드가 **D3D11 \_ SRC \_ 알파**로 설정 된 앱 blend 상태 ( [**ID3D11BlendState**](/windows/desktop/api/d3d11/nn-d3d11-id3d11blendstate)참조)를 적용 하 여 출력 병합기 단계에서 수행할 수 있습니다. 미리 곱한 알파 값을 포함 하는 자산을 사용할 수도 있습니다.

    알파 사전 곱셈 단계를 수행 하지 않으면 전경 스왑 체인의 색이 예상 보다 더 밝습니다.

5.  전경 스왑 체인이 생성 되었는지 여부에 따라 UI 요소에 대 한 Direct2D drawing 화면을 전경 스왑 체인에 연결 해야 할 수 있습니다.

    렌더링 대상 뷰 만들기:

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

6.  실시간 게임 콘텐츠에 사용 되는 배율이 조정 된 스왑 체인에 전경 스왑 체인을 제공 합니다. 두 스왑 체인에 대해 프레임 대기 시간이 2로 설정 되었으므로 DXGI는 동일한 VSync 간격 내에 둘 다를 제공할 수 있습니다.

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

 

 