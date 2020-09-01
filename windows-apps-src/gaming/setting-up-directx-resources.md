---
title: DirectX 리소스를 설정 하 고 이미지를 표시 합니다.
description: 여기서는 Direct3D 장치, 스왑 체인 및 렌더링 대상 뷰를 만드는 방법 및 렌더링 된 이미지를 디스플레이에 표시 하는 방법을 보여 줍니다.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 리소스, 이미지
ms.localizationpriority: medium
ms.openlocfilehash: cced3b5cb6ad9c3e1ffe077887c5f23ce95745bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159197"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>DirectX 리소스를 설정 하 고 이미지를 표시 합니다.



여기서는 Direct3D 장치, 스왑 체인 및 렌더링 대상 뷰를 만드는 방법 및 렌더링 된 이미지를 디스플레이에 표시 하는 방법을 보여 줍니다.

**목표:** UWP (c + + 유니버설 Windows 플랫폼) 앱에서 DirectX 리소스를 설정 하 고 단색을 표시 합니다.

## <a name="prerequisites"></a>필수 조건


C + +에 대해 잘 알고 있다고 가정 합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

**완료 시간:** 20 분

## <a name="instructions"></a>Instructions

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. ComPtr를 사용 하 여 Direct3D 인터페이스 변수 선언

Windows 런타임 c + + 템플릿 라이브러리 (WRL)의 ComPtr [스마트 포인터](/cpp/cpp/smart-pointers-modern-cpp) 템플릿을 사용 하 여 Direct3D 인터페이스 변수를 선언 하므로 이러한 변수의 수명은 예외를 안전 하 게 관리할 수 있습니다. 그런 다음 이러한 변수를 사용 하 여 [**ComPtr 클래스**](/cpp/windows/comptr-class) 및 해당 멤버에 액세스할 수 있습니다. 예를 들면 다음과 같습니다.

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

ComPtr를 사용 하 여 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 를 선언 하는 경우 ComPtr의 **getaddressof** 메서드를 사용 하 여 **ID3D11RenderTargetView** \* \* [**ID3D11RenderTargetView:: ID3D11DeviceContext**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)에 전달할 ID3D11RenderTargetView (OMSetRenderTargets)에 대 한 포인터의 주소를 가져올 수 있습니다. **OMSetRenderTargets** 렌더링 대상을 출력 [-병합기 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) 에 바인딩하여 렌더링 대상을 출력 대상으로 지정 합니다.

샘플 앱이 시작 되 면 초기화 되 고 로드 된 다음 실행할 준비가 됩니다.

### <a name="2-creating-the-direct3d-device"></a>2. Direct3D 장치 만들기

Direct3D API를 사용 하 여 장면을 렌더링 하려면 먼저 디스플레이 어댑터를 나타내는 Direct3D 장치를 만들어야 합니다. Direct3D 장치를 만들려면 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 함수를 호출 합니다. [**D3D \_ 기능 \_ 수준**](/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) 값 배열에서 9.1 ~ 11.1 수준을 지정 합니다. Direct3D는 배열을 순서 대로 탐색 하 고 가장 높은 지원 기능 수준을 반환 합니다. 따라서 사용 가능한 가장 높은 기능 수준을 얻기 위해 **D3D \_ 기능 \_ 수준** 배열 항목을 최고에서 최저로 나열 합니다. [**D3D11 \_ CREATE \_ DEVICE \_ Bgra \_ SUPPORT**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) 플래그를 *Flags* 매개 변수에 전달 하 여 Direct3D 리소스가 Direct2D와 상호 운용할 수 있도록 합니다. 또한 디버그 빌드를 사용 하는 경우 [**D3D11 \_ CREATE \_ DEVICE \_ debug**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) 플래그를 전달 합니다. 앱 디버깅에 대 한 자세한 내용은 [디버그 계층을 사용 하 여 앱 디버그](/windows/desktop/direct3d11/using-the-debug-layer-to-test-apps)를 참조 하세요.

[**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)에서 반환 되는 direct3d 11 장치 및 장치 컨텍스트를 쿼리하여 direct3d 11.1 장치 ([**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)) 및 장치 컨텍스트 ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1))를 가져옵니다.

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

그런 다음 장치에서 렌더링 하 고 표시 하는 데 사용 하는 스왑 체인을 만듭니다. 스왑 체인을 설명 하기 위해 [**DXGI \_ swap \_ chain \_ DESC1**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) 구조체를 선언 하 고 초기화 합니다. 그런 다음 스왑 체인을 대칭 이동 모델로 설정 합니다. 즉, 스와핑 **효과 멤버에서** **지정** 된 순차 값을 [** \_ \_ \_ 대칭 \_ **](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) 이동 하 여 dxgi [** \_ 형식 \_ B8G8R8A8 \_ unorm**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)으로 설정 합니다. 대칭 이동 모델이 여러 샘플 앤티 앨리어싱 (MSAA)를 지원 하지 않기 때문에 **SampleDesc** 멤버가 지정 하는 [**dxgi \_ sample \_ **](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) desc 구조의 **Count** 멤버를 **0으로 \_ \_ ** **설정 합니다.** **BufferCount** 멤버를 2로 설정 하면 스왑 체인이 프런트 버퍼를 사용 하 여 표시 장치에 표시 하 고 렌더링 대상으로 사용 되는 백 버퍼를 사용할 수 있습니다.

Direct3D 11.1 디바이스를 쿼리하여 기본 DXGI 디바이스를 얻습니다. 랩톱 및 태블릿과 같은 배터리 기반 장치에서 수행 해야 하는 전원 소비를 최소화 하기 위해 DXGI에서 큐에 대기 시킬 수 있는 최대 버퍼 프레임 수로 1을 사용 하 여 [**IDXGIDevice1:: SetMaximumFrameLatency**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) 메서드를 호출 합니다. 이렇게 하면 응용 프로그램이 세로 공백 후에만 렌더링 됩니다.

마지막으로 스왑 체인을 만들려면 DXGI 장치에서 부모 팩터리를 가져와야 합니다. [**Idxgid device:: GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) 를 호출 하 여 장치에 대 한 어댑터를 가져온 다음 어댑터에서 [**Idxgid Object:: getadapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgiobject-getparent) 를 호출 하 여 부모 팩터리 ([**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2))를 가져옵니다. 스왑 체인을 만들려면 스왑 체인 설명자와 앱의 코어 창을 사용 하 여 [**IDXGIFactory2:: CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) 를 호출 합니다.

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

창에 그래픽을 렌더링 하려면 렌더링 대상 뷰를 만들어야 합니다. [**Idxgiswapchain:: GetBuffer**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-getbuffer) 를 호출 하 여 렌더링 대상 뷰를 만들 때 사용할 스왑 체인의 백 버퍼를 가져옵니다. 백 버퍼를 2D 질감 ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d))으로 지정 합니다. 렌더링 대상 뷰를 만들려면 스왑 체인의 백 버퍼를 사용 하 여 [**ID3D11Device:: CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) 를 호출 합니다. 뷰 포트 ([**D3D11 \_ 뷰포트**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_viewport))를 스왑 체인 백 버퍼의 전체 크기로 지정 하 여 전체 코어 창에 그리도록 지정 해야 합니다. [**ID3D11DeviceContext:: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) 에 대 한 호출에서 뷰 포트를 사용 하 여 파이프라인의 [래스터 라이저 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) 에 뷰 포트를 바인딩합니다. 래스터 라이저 단계는 벡터 정보를 래스터 이미지로 변환 합니다. 이 경우 단색을 표시 하기 때문에 변환이 필요 하지 않습니다.

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

### <a name="5-presenting-the-rendered-image"></a>5. 렌더링 된 이미지 표시

무한 루프를 입력 하 여 장면을 지속적으로 렌더링 하 고 표시 합니다.

이 루프에서는 다음을 호출 합니다.

1.  [**ID3D11DeviceContext:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 를 지정 하 여 렌더링 대상을 출력 대상으로 지정 합니다.
2.  [**ID3D11DeviceContext:: ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) 를 선택 하 여 단색으로 렌더링 대상을 지웁니다.
3.  [**Idxgiswapchain:**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 렌더링 된 이미지를 창에 표시 하는:P 다시 보내십시오.

이전에는 최대 프레임 대기 시간을 1로 설정 했기 때문에 Windows에서 일반적으로 렌더링 루프를 화면 새로 고침 속도 (일반적으로 60 Hz)로 낮춥니다. 앱 [**을 호출할 때**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present)앱이 절전 모드로 전환 되 면 Windows에서 렌더링 루프가 느려집니다. Windows에서는 화면을 새로 고칠 때까지 앱이 절전 모드로 전환 됩니다.

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

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. 앱 창 및 스왑 체인의 버퍼 크기 조정

앱 창의 크기가 변경 되는 경우 앱은 스왑 체인의 버퍼 크기를 조정 하 고 렌더링 대상 뷰를 다시 만든 다음 크기가 조정 된 렌더링 된 이미지를 표시 해야 합니다. 스왑 체인의 버퍼 크기를 조정 하려면 [**Idxgiswapchain:: ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)를 호출 합니다. 이 호출에서는 버퍼 수와 버퍼의 형식 ( *BufferCount* 매개 변수는 2로, *newformat* 매개 변수는 [** \_ \_ B8G8R8A8 \_ unorm**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format))으로 그대로 둡니다. 스왑 체인의 백 버퍼 크기는 크기가 조정 된 창의 크기와 동일 하 게 설정 됩니다. 스왑 체인의 버퍼 크기를 조정한 후 새 렌더링 대상을 만들고, 앱을 초기화 했을 때와 유사 하 게 새 렌더링 된 이미지를 표시 합니다.

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


Direct3D 장치, 스왑 체인 및 렌더링 대상 뷰를 만들고 렌더링 된 이미지를 화면에 표시 합니다.

이제 디스플레이에 삼각형을 그립니다.

[셰이더 및 그리기 기본 형식 만들기](creating-shaders-and-drawing-primitives.md)

 

 