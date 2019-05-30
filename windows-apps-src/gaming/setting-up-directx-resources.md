---
title: DirectX 리소스 설정 및 이미지 표시
description: 여기에서는 Direct3D 디바이스, 스왑 체인 및 렌더링 대상 보기를 만드는 방법과 렌더링된 이미지를 디스플레이에 표시하는 방법에 대해 설명합니다.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, directx 리소스, 이미지
ms.localizationpriority: medium
ms.openlocfilehash: 4e0fd2b5f34c17e39def107f72568916ca3ba6fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368007"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>DirectX 리소스 설정 및 이미지 표시



여기에서는 Direct3D 디바이스, 스왑 체인 및 렌더링 대상 보기를 만드는 방법과 렌더링된 이미지를 디스플레이에 표시하는 방법에 대해 설명합니다.

**목표:** DirectX 리소스를 설정 하는 C++ 유니버설 Windows 플랫폼 (UWP) 앱 단색을 표시 하는 합니다.

## <a name="prerequisites"></a>사전 요구 사항


사용자가 C++에 익숙하다고 가정합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

**완료 시간:** 20 분입니다.

## <a name="instructions"></a>지침

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. ComPtr 사용 하 여 Direct3D 인터페이스 변수를 선언합니다.

WRL(Windows 런타임 C++ 템플릿 라이브러리)의 ComPtr [스마트 포인터](https://docs.microsoft.com/cpp/cpp/smart-pointers-modern-cpp) 템플릿으로 Direct3D 인터페이스 변수를 선언하므로 예외가 발생하지 않는 안전한 방식으로 이러한 변수의 수명을 관리할 수 있습니다. 그런 다음 해당 변수를 사용하여 [**ComPtr class**](https://docs.microsoft.com/cpp/windows/comptr-class)와 그 멤버에 액세스할 수 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

선언 하는 경우 [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) ComPtr, 사용할 수 있습니다 다음 ComPtr의 **GetAddressOf** 에 대 한 포인터의 주소를 가져올 방법  **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView)에 전달할 [ **ID3D11DeviceContext::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)합니다. **OMSetRenderTargets**는 렌더링 대상을 [출력 병합 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)에 바인딩하여 렌더링 대상을 출력 대상으로 지정합니다.

샘플 앱이 시작되면 초기화되어 로드된 후 실행 준비가 됩니다.

### <a name="2-creating-the-direct3d-device"></a>2. Direct3D 장치 만들기

Direct3D API를 사용하여 장면을 렌더링하려면 먼저 디스플레이 어댑터를 나타내는 Direct3D 디바이스를 만들어야 합니다. Direct3D 디바이스를 만들기 위해 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 함수를 호출합니다. 배열의 11.1 통해 9.1 수준의 지정 [ **D3D\_기능\_수준** ](https://docs.microsoft.com/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) 값입니다. Direct3D는 배열을 차례로 이동하고 지원되는 가장 높은 기능 수준을 반환합니다. 따라서 사용 가능한 가장 높은 기능 수준을 가져오려면 나열 된 **D3D\_기능\_수준** 가장 낮은 항목을 배열 합니다. 전달 된 [ **D3D11\_만들기\_장치\_BGRA\_지원** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) 플래그를 합니다 *플래그* 만드는 매개 변수를 Direct2D를 사용 하 여 상호 운용 Direct3D 리소스입니다. 또한 전달 디버그 빌드를 사용 하는 경우는 [ **D3D11\_CREATE\_장치\_디버그** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) 플래그입니다. 앱 디버그에 대한 자세한 내용은 [디버그 계층을 사용한 앱 디버그](https://docs.microsoft.com/windows/desktop/direct3d11/using-the-debug-layer-to-test-apps)를 참조하세요.

Direct3D 11 디바이스 및 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)에서 반환되는 디바이스 컨텍스트를 쿼리하여 Direct3D 11.1 디바이스([**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)) 및 디바이스 컨텍스트([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1))를 가져옵니다.

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. 스왑 체인 만들기

이제 디바이스가 렌더링 및 화면 표시에 사용하는 스왑 체인을 만듭니다. 선언 하 고 초기화 된 [ **DXGI\_스왑\_체인\_DESC1** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) 스왑 체인을 설명 하는 구조입니다. 그런 다음 대칭 이동 후 모델로 스왑 체인 설정 (된 스왑 체인, 합니다 [ **DXGI\_스왑\_효과\_대칭 이동\_순차** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) 에 값이 설정 합니다 **SwapEffect** 멤버) 설정 합니다 **형식** 멤버 [ **DXGI\_형식\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format). 설정 합니다 **개수** 의 멤버는 [ **DXGI\_샘플\_DESC** ](https://docs.microsoft.com/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) 구조체입니다는 **SampleDesc** 멤버 1로 지정 하며 **품질** 소속 **DXGI\_샘플\_DESC** 대칭 이동 후 모델 다중 샘플 앤티앨리어싱을 MSAA ()를 지원 하지 않으므로 0입니다. 스왑 체인이 전면 버퍼를 사용하여 렌더링 대상으로 사용되는 백 버퍼 및 디스플레이 디바이스에 제공할 수 있도록 **BufferCount** 구성원을 2로 설정합니다.

Direct3D 11.1 디바이스를 쿼리하여 기본 DXGI 디바이스를 얻습니다. 랩톱 및 태블릿과 같은 배터리 전력을 사용하는 장치에서 중요한 전력 소비 최소화를 위해 DXGI가 쿼리할 수 있는 백 버퍼 프레임의 최대 수가 1인 [**IDXGIDevice1::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) 메서드를 호출합니다. 이것으로 앱은 수직 블랭크 다음에만 렌더링됩니다.

최종적으로 스왑 체인을 만들기 위해, DXGI 장치에서 상위 팩터리를 가져와야 합니다. [  **IDXGIDevice::GetAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter)를 호출하여 디바이스용 어댑터를 가져온 다음 어댑터에서 [**IDXGIObject::GetParent**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiobject-getparent)를 호출하여 상위 팩터리([**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2))를 가져옵니다. 스왑 체인을 만들기 위해, 스왑 체인 설명자와 앱의 핵심 창이 있는 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)를 호출합니다.

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. 렌더링 대상 뷰 만들기

그래픽을 창에 렌더링하기 위해 렌더링 대상 보기를 만들어야 합니다. 렌더링 대상 보기를 만들 때 사용할 스왑 체인의 백 버퍼를 가져오기 위해 [**IDXGISwapChain::GetBuffer**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-getbuffer)를 호출합니다. 백 버퍼를 2D 텍스처([**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d))로 지정합니다. 렌더링 대상 보기를 만들기 위해, 스왑 체인의 백 버퍼가 있는 [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview)를 호출합니다. 보기 포트를 지정 하 여 전체 코어 창에 그릴 지정 해야 합니다 ([**D3D11\_뷰포트**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_viewport)) 스왑 체인 백 버퍼의 전체 크기로 합니다. [  **ID3D11DeviceContext::RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports)에 대한 호출의 보기 포트를 사용하여 보기 포트를 파이프라인의 [래스터라이저 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage)에 바인딩합니다. 래스터라이저 단계는 벡터 정보를 래스터 이미지로 변환합니다. 이 경우에는 단색을 표시하는 것이므로 변환이 필요하지 않습니다.

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. 렌더링된 된 이미지를 표시합니다.

장면을 계속해서 렌더링 및 표시하기 위해 무한 루프를 입력합니다.

이 루프에서는 다음을 호출합니다.

1.  [**ID3D11DeviceContext::OMSetRenderTargets** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 렌더링 대상 출력 대상으로 지정 합니다.
2.  [**ID3D11DeviceContext::ClearRenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) 단색 렌더링 대상의 선택을 취소 합니다.
3.  [**IDXGISwapChain::Present** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 창에 렌더링 된 이미지를 표시 합니다.

이전에 최대 프레임 지연을 1로 설정했으므로, Windows에서는 일반적으로 렌더링 루프의 속도를 화면 새로 고침 빈도(보통 60Hz)로 늦춥니다. Windows에서는 앱이 [**Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present)를 호출하면 앱을 절전 모드로 전환하여 렌더링 루프의 속도를 늦춥니다. Windows에서는 화면을 새로 고칠 때까지 앱을 절전 모드로 전환합니다.

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. 응용 프로그램 창 및 스왑 체인의 버퍼 크기 조정

앱 창의 크기가 바뀌면, 앱은 스왑 체인의 버퍼 크기를 변경하고 렌더링 대상 보기를 다시 만든 다음 크기가 변경된 렌더링된 이미지를 표시해야 합니다. 스왑 체인의 버퍼 크기를 변경하기 위해 [**IDXGISwapChain::ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)를 호출합니다. 이 호출에서 상태로 두면 변경 되지 않고 버퍼의 형식과 버퍼의 수 (합니다 *BufferCount* 두 개의 매개 변수 및 *NewFormat* 매개 변수를 [ **DXGI\_ 서식을\_B8G8R8A8\_UNORM**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)). 스왑 체인의 백 버퍼 크기를, 크기가 변경된 창과 동일하게 만듭니다. 스왑 체인의 버퍼 크기를 변경한 후 새 렌더링 대상을 만들고, 렌더링된 새 이미지를 앱을 초기화했을 때와 유사하게 표시합니다.

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계


Direct3D 디바이스, 스왑 체인 및 렌더링 대상 보기를 만들었고, 렌더링된 이미지를 디스플레이에 표시했습니다.

이제 디스플레이에 삼각형도 그립니다.

[셰이더를 만들고 기본 그리기](creating-shaders-and-drawing-primitives.md)

 

 




