---
title: 화면 방향 지원(DirectX 및 C++)
description: 여기에서는 Windows 10 장치의 그래픽 하드웨어가 효율적이 고 효과적으로 사용 되도록 UWP DirectX 앱에서 화면 회전을 처리 하기 위한 모범 사례에 대해 설명 합니다.
ms.assetid: f23818a6-e372-735d-912b-89cabeddb6d4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 화면 방향, directx
ms.localizationpriority: medium
ms.openlocfilehash: bb5ed4d942484ebded50216ad84f8d1346545527
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217686"
---
# <a name="supporting-screen-orientation-directx-and-c"></a>화면 방향 지원(DirectX 및 C++)



UWP (유니버설 Windows 플랫폼) 앱은 [**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 이벤트를 처리할 때 여러 화면 방향을 지원할 수 있습니다. 여기에서는 Windows 10 장치의 그래픽 하드웨어가 효율적이 고 효과적으로 사용 되도록 UWP DirectX 앱에서 화면 회전을 처리 하기 위한 모범 사례에 대해 설명 합니다.

시작 하기 전에 그래픽 하드웨어는 장치 방향에 관계 없이 항상 동일한 방식으로 픽셀 데이터를 출력 한다는 점에 주의 해야 합니다. Windows 10 장치는 현재 표시 방향 (일종의 센서 또는 소프트웨어 설정/해제)을 결정 하 고 사용자가 표시 설정을 변경할 수 있도록 허용 합니다. 따라서 Windows 10 자체는 장치의 방향에 따라 "수직"이 되도록 이미지의 회전을 처리 합니다. 기본적으로 앱은 창 크기와 같이 항목의 방향이 변경 되었다는 알림을 받습니다. 이 경우 Windows 10은 최종 표시를 위해 이미지를 즉시 회전 합니다. 4 가지 특정 화면 방향 (뒷부분에서 설명)의 경우 Windows 10에서는 추가 그래픽 리소스와 계산을 사용 하 여 최종 이미지를 표시 합니다.

UWP DirectX 앱의 경우 [**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation) 개체는 앱이 쿼리할 수 있는 기본 표시 방향 데이터를 제공 합니다. 기본 방향은 *가로*이며 디스플레이의 픽셀 너비가 높이 보다 큽니다. 대체 방향은 *세로*방향으로 표시 됩니다. 여기서 표시는 한 방향에서 90도 회전 하 고 너비는 높이 보다 낮습니다.

Windows 10은 4 가지 특정 표시 방향 모드를 정의 합니다.

-   가로-Windows 10의 기본 표시 방향이 며 회전의 기준 또는 id 각도 (0도)로 간주 됩니다.
-   세로-디스플레이가 90도 시계 방향으로 90도 회전 되었습니다 (또는 시계 방향 270도).
-   가로, 대칭 이동 — 디스플레이가 180도 회전 되었습니다 (거꾸로 설정 됨).
-   세로, 대칭 이동 — 디스플레이가 시계 방향 270 (또는 시계 방향 90도)으로 회전 되었습니다.

디스플레이가 한 방향에서 다른 방향으로 회전 하는 경우 Windows 10은 내부적으로 회전 작업을 수행 하 여 그려진 이미지를 새 방향으로 맞추고, 사용자는 화면에서 수직 이미지를 볼 수 있습니다.

또한 Windows 10은 한 방향에서 다른 방향으로 이동할 때 원활한 사용자 환경을 만드는 자동 전환 애니메이션을 표시 합니다. 디스플레이 방향이 이동 하면 사용자는 표시 된 화면 이미지의 고정 된 확대/축소 및 회전 애니메이션으로 이러한 변화를 확인 합니다. 시간은 Windows 10에서 새 방향으로 레이아웃을 위해 앱에 할당 됩니다.

전반적으로, 화면 방향 변경을 처리 하는 일반적인 프로세스는 다음과 같습니다.

1.  창 범위 값과 디스플레이 방향 데이터의 조합을 사용 하 여 스왑 체인을 장치의 기본 표시 방향에 맞춰 정렬 합니다.
2.  [**IDXGISwapChain1:: SetRotation**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation)을 사용 하 여 스왑 체인의 방향으로 Windows 10에 알립니다.
3.  장치의 사용자 방향에 맞춘 이미지를 생성 하도록 렌더링 코드를 변경 합니다.

## <a name="resizing-the-swap-chain-and-pre-rotating-its-contents"></a>스왑 체인의 크기를 조정 하 고 해당 콘텐츠를 미리 회전


기본 디스플레이 크기 조정을 수행 하 고 UWP DirectX 앱에서 해당 내용을 미리 회전 하려면 다음 단계를 구현 합니다.

1.  [**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 이벤트를 처리 합니다.
2.  스왑 체인을 창의 새 크기로 조정 합니다.
3.  [**IDXGISwapChain1:: SetRotation**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) 을 호출 하 여 스왑 체인의 방향을 설정 합니다.
4.  렌더링 대상 및 기타 픽셀 데이터 버퍼와 같은 창 크기 종속 리소스를 다시 만듭니다.

이제이 단계를 좀 더 자세히 살펴보겠습니다.

첫 번째 단계는 [**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 이벤트에 대 한 처리기를 등록 하는 것입니다. 이 이벤트는 화면 방향이 변경 될 때마다 (예: 디스플레이를 회전 하는 경우) 앱에서 발생 합니다.

[**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 이벤트를 처리 하려면 필요한 [**Setwindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) 메서드에서 **DisplayInformation:: OrientationChanged** 에 대 한 처리기를 연결 합니다 .이 메서드는 뷰 공급자가 구현 해야 하는 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 인터페이스의 메서드 중 하나입니다.

이 코드 예제에서 [**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 에 대 한 이벤트 처리기는 **OnOrientationChanged**이라는 메서드입니다. **DisplayInformation:: OrientationChanged** 가 발생 하면이 메서드는 **SetCurrentOrientation** 라는 메서드를 호출한 다음 **CreateWindowSizeDependentResources**를 호출 합니다.

```cpp
void App::SetWindow(CoreWindow^ window)
{
  // ... Other UI event handlers assigned here ...
  
    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);

  // ...
}
}
```

```cpp
void App::OnOrientationChanged(DisplayInformation^ sender, Object^ args)
{
    m_deviceResources->SetCurrentOrientation(sender->CurrentOrientation);
    m_main->CreateWindowSizeDependentResources();
}

// This method is called in the event handler for the OrientationChanged event.
void DX::DeviceResources::SetCurrentOrientation(DisplayOrientations currentOrientation)
{
    if (m_currentOrientation != currentOrientation)
    {
        m_currentOrientation = currentOrientation;
        CreateWindowSizeDependentResources();
    }
}
```

다음으로, 새 화면 방향에 대 한 스왑 체인의 크기를 조정 하 고 렌더링을 수행할 때 그래픽 파이프라인의 내용을 회전 하도록 준비 합니다. 이 예제에서 **DirectXBase:: CreateWindowSizeDependentResources** 는 IDXGISwapChain:: ResizeBuffers 호출을 처리 하 고, 3D 및 2d 회전 행렬을 설정 하 고, setrotation을 호출 하 고, 리소스를 다시 만드는 것을 처리 하는 메서드입니다.

```cpp
void DX::DeviceResources::CreateWindowSizeDependentResources() 
{
    // Clear the previous window size specific context.
    ID3D11RenderTargetView* nullViews[] = {nullptr};
    m_d3dContext->OMSetRenderTargets(ARRAYSIZE(nullViews), nullViews, nullptr);
    m_d3dRenderTargetView = nullptr;
    m_d2dContext->SetTarget(nullptr);
    m_d2dTargetBitmap = nullptr;
    m_d3dDepthStencilView = nullptr;
    m_d3dContext->Flush();

    // Calculate the necessary render target size in pixels.
    m_outputSize.Width = DX::ConvertDipsToPixels(m_logicalSize.Width, m_dpi);
    m_outputSize.Height = DX::ConvertDipsToPixels(m_logicalSize.Height, m_dpi);
    
    // Prevent zero size DirectX content from being created.
    m_outputSize.Width = max(m_outputSize.Width, 1);
    m_outputSize.Height = max(m_outputSize.Height, 1);

    // The width and height of the swap chain must be based on the window's
    // natively-oriented width and height. If the window is not in the native
    // orientation, the dimensions must be reversed.
    DXGI_MODE_ROTATION displayRotation = ComputeDisplayRotation();

    bool swapDimensions = displayRotation == DXGI_MODE_ROTATION_ROTATE90 || displayRotation == DXGI_MODE_ROTATION_ROTATE270;
    m_d3dRenderTargetSize.Width = swapDimensions ? m_outputSize.Height : m_outputSize.Width;
    m_d3dRenderTargetSize.Height = swapDimensions ? m_outputSize.Width : m_outputSize.Height;

    if (m_swapChain != nullptr)
    {
        // If the swap chain already exists, resize it.
        HRESULT hr = m_swapChain->ResizeBuffers(
            2, // Double-buffered swap chain.
            lround(m_d3dRenderTargetSize.Width),
            lround(m_d3dRenderTargetSize.Height),
            DXGI_FORMAT_B8G8R8A8_UNORM,
            0
            );

        if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
        {
            // If the device was removed for any reason, a new device and swap chain will need to be created.
            HandleDeviceLost();

            // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
            // and correctly set up the new device.
            return;
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    else
    {
        // Otherwise, create a new one using the same adapter as the existing Direct3D device.
        DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

        swapChainDesc.Width = lround(m_d3dRenderTargetSize.Width); // Match the size of the window.
        swapChainDesc.Height = lround(m_d3dRenderTargetSize.Height);
        swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
        swapChainDesc.Stereo = false;
        swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
        swapChainDesc.SampleDesc.Quality = 0;
        swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
        swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
        swapChainDesc.Flags = 0;    
        swapChainDesc.Scaling = DXGI_SCALING_NONE;
        swapChainDesc.AlphaMode = DXGI_ALPHA_MODE_IGNORE;

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

        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForCoreWindow(
                m_d3dDevice.Get(),
                reinterpret_cast<IUnknown*>(m_window.Get()),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }

    // Set the proper orientation for the swap chain, and generate 2D and
    // 3D matrix transformations for rendering to the rotated swap chain.
    // Note the rotation angle for the 2D and 3D transforms are different.
    // This is due to the difference in coordinate spaces.  Additionally,
    // the 3D matrix is specified explicitly to avoid rounding errors.

    switch (displayRotation)
    {
    case DXGI_MODE_ROTATION_IDENTITY:
        m_orientationTransform2D = Matrix3x2F::Identity();
        m_orientationTransform3D = ScreenRotation::Rotation0;
        break;

    case DXGI_MODE_ROTATION_ROTATE90:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(90.0f) *
            Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
        m_orientationTransform3D = ScreenRotation::Rotation270;
        break;

    case DXGI_MODE_ROTATION_ROTATE180:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(180.0f) *
            Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
        m_orientationTransform3D = ScreenRotation::Rotation180;
        break;

    case DXGI_MODE_ROTATION_ROTATE270:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(270.0f) *
            Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
        m_orientationTransform3D = ScreenRotation::Rotation90;
        break;

    default:
        throw ref new FailureException();
    }


    //SDM: only instance of SetRotation
    DX::ThrowIfFailed(
        m_swapChain->SetRotation(displayRotation)
        );

    // Create a render target view of the swap chain back buffer.
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

    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT, 
        lround(m_d3dRenderTargetSize.Width),
        lround(m_d3dRenderTargetSize.Height),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &depthStencilDesc,
            nullptr,
            &depthStencil
            )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2D);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
            depthStencil.Get(),
            &depthStencilViewDesc,
            &m_d3dDepthStencilView
            )
        );
    
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);

    // Create a Direct2D target bitmap associated with the
    // swap chain back buffer and set it as the current target.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    ComPtr<IDXGISurface2> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

    // Grayscale text anti-aliasing is recommended for all UWP apps.
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

}

```

다음에이 메서드를 호출할 때 창의 현재 높이 및 너비 값을 저장 한 후 디스플레이 범위의 DIP (장치 독립적 픽셀) 값을 픽셀로 변환 합니다. 이 샘플에서는이 코드를 실행 하는 간단한 함수인 **ConvertDipsToPixels**를 호출 합니다.

` floor((dips * dpi / 96.0f) + 0.5f);`

0.5 f를 추가 하 여 가장 가까운 정수 값으로 반올림 합니다.

[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 좌표는 항상 dip로 정의 됩니다. Windows 10 및 이전 *버전의 windows*에서는 DIP가 1 인치/인치의 96 정의 되 고 OS 정의에 맞춰 정렬 됩니다. 디스플레이 방향이 세로 모드로 회전 하면 앱에서 **CoreWindow**의 너비와 높이를 대칭 이동 하 고 렌더링 대상 크기 (범위)가 적절 하 게 변경 되어야 합니다. Direct3D's 좌표는 항상 실제 픽셀 이기 때문에 이러한 값을 Direct3D에 전달 하 여 스왑 체인을 설정 하기 전에 **CoreWindow**의 DIP 값에서 정수 픽셀 값으로 변환 해야 합니다.

단순히 스왑 체인의 크기를 조정 하는 경우 보다 더 많은 작업을 수행 하는 것이 좋습니다. 실제로 표시 하기 전에 이미지의 Direct2D 및 Direct3D 구성 요소를 회전 하 고, 결과를 새 방향으로 렌더링 했다는 것을 스왑 체인에 지시 하는 것입니다. 다음은이 프로세스에 대 한 자세한 내용은 **DX::D eviceresources:: CreateWindowSizeDependentResources**에 대 한 코드 예제에 나와 있습니다.

-   표시의 새 방향을 결정 합니다. 디스플레이가 가로에서 세로 방향으로 대칭 이동 하거나 그 반대로 전환 된 경우 표시 범위에 대 한 높이 및 너비 값을 DIP 값에서 픽셀로 변경 합니다.

-   그런 다음 스왑 체인이 만들어졌는지 확인 합니다. 아직 만들지 않은 경우 [**IDXGIFactory2:: CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)를 호출 하 여 만듭니다. 그렇지 않으면 [**Idxgiswapchain: ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)를 호출 하 여 기존 스왑 체인의 버퍼 크기를 새 표시 차원으로 조정 합니다. 회전 이벤트에 대해 스왑 체인의 크기를 조정할 필요는 없지만 렌더링 파이프라인에 의해 이미 회전 된 콘텐츠를 출력 하는 중입니다. 즉, 크기 조정 해야 하는 맞추기 및 채우기 이벤트와 같은 다른 크기 변경 이벤트가 있습니다.

-   그런 다음, 스왑 체인으로 렌더링 될 때 그래픽 파이프라인의 픽셀 또는 꼭 짓 점 (각각)에 적용 되도록 적절 한 2 차원 또는 3 차원 행렬 변환을 설정 합니다. 4 가지 회전 매트릭스가 있습니다.

    -   가로 (DXGI \_ 모드 \_ 회전 \_ id)
    -   세로 (DXGI \_ 모드 \_ 회전 \_ ROTATE270)
    -   가로, 대칭 이동 (DXGI \_ 모드 \_ 회전 \_ ROTATE180)
    -   세로, 대칭 이동 (DXGI \_ 모드 \_ 회전 \_ ROTATE90)

    표시 방향을 결정 하기 위해 Windows 10에서 제공 하는 데이터 (예: [**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged)의 결과)를 기반으로 올바른 행렬이 선택 되어 있으며, 화면 방향에 맞게 효과적으로 회전 하 여 장면의 각 픽셀 (Direct2D) 또는 꼭 짓 점 (Direct3D)의 좌표를 곱합니다. (Direct2D에서는 화면 원점이 왼쪽 위 모퉁이로 정의 되어 있지만 Direct3D에서는 원점이 창의 논리적 가운데로 정의 됩니다.)

> **참고**    회전에 사용 되는 2 차원 변환과이를 정의 하는 방법에 대 한 자세한 내용은 [화면 회전을 위한 행렬 정의 (2 차원)](#appendix-a-applying-matrices-for-screen-rotation-2-d)를 참조 하세요. 회전에 사용 되는 3 차원 변환에 대 한 자세한 내용은 [화면 회전을 위한 행렬 정의 (3 차원)](#appendix-b-applying-matrices-for-screen-rotation-3-d)를 참조 하세요.

 

이제 다음은 중요 한 비트입니다. [**IDXGISwapChain1:: SetRotation**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) 을 호출 하 고 다음과 같이 업데이트 된 회전 매트릭스를 제공 합니다.

`m_swapChain->SetRotation(rotation);`

또한 새 프로젝션을 계산할 때 render 메서드에서 가져올 수 있는 선택한 회전 행렬을 저장 합니다. 최종 3 차원 프로젝션을 렌더링 하거나 최종 2 차원 레이아웃을 합성 하는 경우이 매트릭스를 사용 합니다. 자동으로 적용 되지 않습니다.

그런 다음 회전 된 3 차원 보기에 대 한 새 렌더링 대상과 뷰의 새 깊이 스텐실 버퍼를 만듭니다. [**ID3D11DeviceContext: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports)를 호출 하 여 회전 된 장면에 대해 3 차원 렌더링 뷰포트를 설정 합니다.

마지막으로, 2 차원 이미지를 회전 하거나 레이아웃 하는 경우 [**ID2D1DeviceContext:: CreateBitmapFromDxgiSurface**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-createbitmapfromdxgisurface(idxgisurface_constd2d1_bitmap_properties1__id2d1bitmap1)) 를 사용 하 여 크기 조정 된 스왑 체인에 대해 쓰기 가능한 비트맵으로 2 차원 렌더링 대상을 만들고 업데이트 된 방향에 대 한 새 레이아웃을 합성 합니다. 렌더링 대상에 필요한 모든 속성을 설정 합니다 (예: 코드 예제에 표시 된 대로 앤티앨리어싱 모드).

이제 스왑 체인을 표시 합니다.

## <a name="reduce-the-rotation-delay-by-using-corewindowresizemanager"></a>CoreWindowResizeManager를 사용 하 여 회전 지연 줄이기


기본적으로 Windows 10은 응용 프로그램 모델 또는 언어에 관계 없이 모든 앱에 대 한 짧고 눈에 띄는 시간을 제공 하 여 이미지의 회전을 완료 합니다. 그러나 앱이 여기에 설명 된 기술 중 하나를 사용 하 여 회전 계산을 수행 하는 경우이 기간이 종료 되기 전에 수행 될 수 있습니다. 해당 시간을 다시 가져오고 회전 애니메이션을 완료 하 시겠습니까? 여기서 [**CoreWindowResizeManager**](/uwp/api/Windows.UI.Core.CoreWindowResizeManager) 가 제공 됩니다.

[**CoreWindowResizeManager**](/uwp/api/Windows.UI.Core.CoreWindowResizeManager)를 사용 하는 방법은 다음과 같습니다. [**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 이벤트가 발생 하는 경우 이벤트에 대 한 처리기 내에서 [**CoreWindowResizeManager:: GetForCurrentView**](/previous-versions/hh404170(v=vs.85)) 를 호출 하 여 **CoreWindowResizeManager** 의 인스턴스를 가져오고, 새 방향에 대 한 레이아웃이 완료 되 고 표시 되 면 [**NotifyLayoutCompleted**](/uwp/api/windows.ui.core.corewindowresizemanager.notifylayoutcompleted) 를 호출 하 여 Windows에서 회전 애니메이션을 완료 하 고 앱 화면을 표시할 수 있음을 알려 줍니다.

[**DisplayInformation:: OrientationChanged**](/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 에 대 한 이벤트 처리기의 코드는 다음과 같습니다.

```cpp
CoreWindowResizeManager^ resizeManager = Windows::UI::Core::CoreWindowResizeManager::GetForCurrentView();

// ... build the layout for the new display orientation ...

resizeManager->NotifyLayoutCompleted();
```

사용자가 디스플레이 방향을 회전 하면 Windows 10은 사용자에 대 한 피드백으로 앱과 독립적인 애니메이션을 보여 줍니다. 이러한 애니메이션에는 다음 순서 대로 세 가지 부분이 있습니다.

-   Windows 10은 원래 이미지를 축소 합니다.
-   Windows 10에는 새 레이아웃을 다시 작성 하는 데 걸리는 시간에 대 한 이미지가 포함 되어 있습니다. 앱이 모두 필요 하지 않을 수 있으므로이 기간을 단축 하려는 시간입니다.
-   레이아웃 창이 만료 되거나 레이아웃 완성 알림이 수신 되 면 Windows에서 이미지를 회전 한 다음 교차 페이드를 새 방향으로 확대/축소 합니다.

세 번째 글머리 기호에서 제안 하는 것 처럼 앱이 [**NotifyLayoutCompleted**](/uwp/api/windows.ui.core.corewindowresizemanager.notifylayoutcompleted)를 호출 하면 Windows 10이 제한 시간 창을 중지 하 고, 회전 애니메이션을 완료 하 고, 앱에 제어를 반환 합니다. 그러면 이제 새 표시 방향으로 표시 됩니다. 전반적인 효과는 이제 앱이 더 약간의 유체를 내 고 응답성이 높고 더 효율적으로 작동 하는 것입니다.

## <a name="appendix-a-applying-matrices-for-screen-rotation-2-d"></a>부록 A: 화면 회전을 위한 행렬 적용 (2 차원)


[스왑 체인의 크기를 조정 하 고 해당 콘텐츠를 미리 회전](#resizing-the-swap-chain-and-pre-rotating-its-contents) 하는 예제 코드의 경우 (및 [DXGI swap chain Rotation 샘플](https://github.com/microsoft/VCSamples/tree/master/VC2012Samples/Windows%208%20samples/C%2B%2B/Windows%208%20app%20samples/DXGI%20swap%20chain%20rotation%20sample%20(Windows%208))) Direct2D 출력 및 Direct3D 출력에 대 한 별도의 회전 매트릭스가 있음을 알 수 있습니다. 먼저 2 차원 매트릭스를 살펴보겠습니다.

Direct2D 콘텐츠와 Direct3D 콘텐츠에 동일한 회전 행렬을 적용할 수 없는 이유에는 두 가지가 있습니다.

-   하나는 서로 다른 데카르트 좌표 모델을 사용 합니다. Direct2D는 오른쪽 규칙을 사용 합니다. 여기서 y 좌표는 원점에서 위쪽으로 이동 하는 양수 값으로 증가 합니다. 그러나 Direct3D는 왼쪽 규칙을 사용 합니다. 여기서 y 좌표는 원점에서 오른쪽 양수 값으로 늘어납니다. 그러면 화면 좌표에 대 한 원점이 Direct2D의 왼쪽 위에 있고 화면 (프로젝션 평면)의 원점은 Direct3D의 왼쪽 아래에 있습니다. 자세한 내용은 [3 차원 좌표계](/previous-versions/windows/desktop/bb324490(v=vs.85)) 를 참조 하세요.

    ![direct3d 좌표계입니다.](images/direct3d-origin.png)![direct2d 좌표계입니다.](images/direct2d-origin.png)

-   3 차원 회전 매트릭스는 반올림 오류를 방지 하기 위해 명시적으로 지정 해야 합니다.

스왑 체인은 원점이 왼쪽 아래에 있는 것으로 가정 하므로, 직각 Direct2D 좌표계를 스왑 체인에서 사용 되는 왼손의 좌표계와 맞추려면 회전을 수행 해야 합니다. 특히 회전 행렬을 회전 된 좌표계 원점에 대 한 변환 행렬에 곱하여 이미지를 새 왼쪽 방향으로 변경 하 고 이미지를 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)의 좌표 공간에서 스왑 체인의 좌표 공간으로 변환 합니다. Direct2D 렌더링 대상이 스왑 체인에 연결 된 경우에도 앱은이 변환을 일관 되 게 적용 해야 합니다. 그러나 앱이 스왑 체인과 직접 연결 되지 않은 중간 표면으로 그리면이 좌표 공간 변환을 적용 하지 마십시오.

가능한 네 가지 회전에서 올바른 행렬을 선택 하는 코드는 다음과 같이 표시 될 수 있습니다 (새 좌표계 원본으로의 변환에 유의).

```cpp
   
// Set the proper orientation for the swap chain, and generate 2D and
// 3D matrix transformations for rendering to the rotated swap chain.
// Note the rotation angle for the 2D and 3D transforms are different.
// This is due to the difference in coordinate spaces.  Additionally,
// the 3D matrix is specified explicitly to avoid rounding errors.

switch (displayRotation)
{
case DXGI_MODE_ROTATION_IDENTITY:
    m_orientationTransform2D = Matrix3x2F::Identity();
    m_orientationTransform3D = ScreenRotation::Rotation0;
    break;

case DXGI_MODE_ROTATION_ROTATE90:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(90.0f) *
        Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
    m_orientationTransform3D = ScreenRotation::Rotation270;
    break;

case DXGI_MODE_ROTATION_ROTATE180:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(180.0f) *
        Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
    m_orientationTransform3D = ScreenRotation::Rotation180;
    break;

case DXGI_MODE_ROTATION_ROTATE270:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(270.0f) *
        Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
    m_orientationTransform3D = ScreenRotation::Rotation90;
    break;

default:
    throw ref new FailureException();
}
    
```

2 차원 이미지에 대 한 올바른 회전 행렬 및 원본을 가져온 후에는 [**ID2D1DeviceContext:: begindraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) 및 [**ID2D1DeviceContext:: enddraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-enddraw)호출 사이에 [**ID2D1DeviceContext:: settransform**](/windows/desktop/Direct2D/id2d1rendertarget-settransform) 에 대 한 호출을 사용 하 여 설정 합니다.

**경고**    Direct2D에는 변환 스택이 없습니다. 앱이 해당 그리기 코드의 일부로 [**ID2D1DeviceContext:: SetTransform**](/windows/desktop/Direct2D/id2d1rendertarget-settransform) 을 사용 하는 경우에는이 매트릭스를 적용 한 다른 변환에도 적용 해야 합니다.

 

```cpp
    ID2D1DeviceContext* context = m_deviceResources->GetD2DDeviceContext();
    Windows::Foundation::Size logicalSize = m_deviceResources->GetLogicalSize();

    context->SaveDrawingState(m_stateBlock.Get());
    context->BeginDraw();

    // Position on the bottom right corner.
    D2D1::Matrix3x2F screenTranslation = D2D1::Matrix3x2F::Translation(
        logicalSize.Width - m_textMetrics.layoutWidth,
        logicalSize.Height - m_textMetrics.height
        );

    context->SetTransform(screenTranslation * m_deviceResources->GetOrientationTransform2D());

    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_TRAILING)
        );

    context->DrawTextLayout(
        D2D1::Point2F(0.f, 0.f),
        m_textLayout.Get(),
        m_whiteBrush.Get()
        );

    // Ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = context->EndDraw();
```

다음에 스왑 체인을 제공할 때 2 차원 이미지가 새 표시 방향과 일치 하도록 회전 됩니다.

## <a name="appendix-b-applying-matrices-for-screen-rotation-3-d"></a>부록 B: 화면 회전을 위한 행렬 적용 (3 차원)


[스왑 체인의 크기를 조정 하 고 해당 내용을 미리 회전 하](#resizing-the-swap-chain-and-pre-rotating-its-contents) 는 예제 코드에서는 가능한 각 [DXGI swap chain rotation sample](https://github.com/microsoft/VCSamples/tree/master/VC2012Samples/Windows%208%20samples/C%2B%2B/Windows%208%20app%20samples/DXGI%20swap%20chain%20rotation%20sample%20(Windows%208))화면 방향에 대 한 특정 변형 행렬을 정의 했습니다. 이제 3 차원 장면 회전을 위한 행렬을 살펴보겠습니다. 이전과 마찬가지로 가능한 4 방향 각각에 대해 행렬 집합을 만듭니다. 반올림 오류 및 사소한 시각적 아티팩트를 방지 하려면 코드에서 매트릭스를 명시적으로 선언 합니다.

다음과 같이 이러한 3 차원 회전 매트릭스를 설정 합니다. 다음 코드 예제에 표시 된 매트릭스는 카메라의 3 차원 장면 공간에서 점을 정의 하는 꼭 짓 점의 0, 90, 180 및 270 각도 회전에 대 한 표준 회전 매트릭스입니다. 장면의 \[ 2 차원 프로젝션을 계산 하면 장면의 각 꼭 짓 점 x, y, z \] 좌표 값을이 회전 행렬에 곱합니다.

```cpp
   
// 0-degree Z-rotation
static const XMFLOAT4X4 Rotation0( 
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 90-degree Z-rotation
static const XMFLOAT4X4 Rotation90(
    0.0f, 1.0f, 0.0f, 0.0f,
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 180-degree Z-rotation
static const XMFLOAT4X4 Rotation180(
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, -1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 270-degree Z-rotation
static const XMFLOAT4X4 Rotation270( 
    0.0f, -1.0f, 0.0f, 0.0f,
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );            
    }
```

다음과 같이 [**IDXGISwapChain1:: SetRotation**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation)을 호출 하 여 스왑 체인에서 회전 유형을 설정 합니다.

`   m_swapChain->SetRotation(rotation);`

이제 render 메서드에서 다음과 유사한 일부 코드를 구현 합니다.

``` syntax
struct ConstantBuffer // This struct is provided for illustration.
{
    // Other constant buffer matrices and data are defined here.

    float4x4 projection; // Current matrix for projection
} ;
ConstantBuffer  m_constantBufferData;          // Constant buffer resource data

// ...

// Rotate the projection matrix as it will be used to render to the rotated swap chain.
m_constantBufferData.projection = mul(m_constantBufferData.projection, m_rotationTransform3D);
```

이제 렌더링 메서드를 호출 하면 현재 회전 행렬 ( **m \_ 에서 orientationtransform3d**클래스 변수에서 지정 됨)을 현재 프로젝션 매트릭스와 곱하고 해당 작업의 결과를 렌더러의 새 프로젝션 행렬로 할당 합니다. 업데이트 된 표시 방향의 장면을 볼 수 있도록 스왑 체인을 표시 합니다.

 

 